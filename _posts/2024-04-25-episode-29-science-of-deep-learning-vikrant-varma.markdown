---
layout: post
title: "29 - Science of Deep Learning with Vikrant Varma"
date: 2024-04-25 12:00 -0700
categories: episode
---

<!-- [YouTube link](https://youtu.be/hdQDZo7jGBY) -->

In 2022, it was announced that a fairly simple method can be used to extract the true beliefs of a language model on any given topic, without having to actually understand the topic at hand. Earlier, in 2021, it was announced that neural networks sometimes 'grok': that is, when training them on certain tasks, they initially memorize their training data (achieving their training goal in a way that doesn't generalize), but then suddenly switch to understanding the 'real' solution in a way that generalizes. What's going on with these discoveries? Are they all they're cracked up to be, and if so, how are they working? In this episode, I talk to Vikrant Varma about his research getting to the bottom of these questions.

Topics we discuss:
 - [Challenges with unsupervised LLM knowledge discovery, aka contra CCS](#contra-ccs)
   - [What is CCS?](#what-is-ccs)
   - [Consistent and contrastive features other than model beliefs](#consistent-contrastive-features)
   - [Understanding the banana/shed mystery](#understanding-banana-shed-mystery)
   - [Future CCS-like approaches](#future-ccs-like-approaches)
   - [CCS as principal component analysis](#ccs-as-pca)
 - [Explaining grokking through circuit efficiency](#explaining-grokking)
   - [Why research science of deep learning?](#why-research-science-deep-learning)
   - [Summary of the paper's hypothesis](#summary-papers-hypothesis)
   - [What are 'circuits'?](#what-are-circuits)
   - [The role of complexity](#role-of-complexity)
   - [Many kinds of circuits](#many-kinds-of-circuits)
   - [How circuits are learned](#how-circuits-are-learned)
   - [Semi-grokking and ungrokking](#semi-grokking-ungrokking)
   - [Generalizing the results](#generalizing-results)
 - [Vikrant's research approach](#vikrants-research-approach)
 - [The DeepMind alignment team](#deepmind-alignment-team)
 - [Follow-up work](#follow-up-work)
 
**Daniel Filan:**
Hello, everybody. In this episode I'll be speaking with Vikrant Varma, a research engineer at Google DeepMind, and the technical lead of their sparse autoencoders effort. Today, we'll be talking about his research on problems with contrast-consistent search, and also explaining grokking through circuit efficiency. For links what we're discussing, you can check the description of this episode and you can read the transcript at axrp.net.

All right, well, welcome to the podcast.

**Vikrant Varma:**
Thanks, Daniel. Thanks for having me.

## Challenges with unsupervised LLM knowledge discovery, aka contra CCS <a name="contra-ccs"></a>

### What is CCS? <a name="what-is-ccs"></a>

**Daniel Filan:**
Yeah. So first, I'd like to talk about this paper. It is called [Challenges with Unsupervised LLM Knowledge Discovery](https://arxiv.org/abs/2312.10029), and the authors are [Sebastian Farquhar](https://sebastianfarquhar.com/), you, [Zachary Kenton](https://zackenton.github.io/), [Johannes Gasteiger](https://scholar.google.de/citations?user=QqdUw8MAAAAJ&hl=en), [Vladimir Mikulik](https://scholar.google.com/citations?user=o42aK1UAAAAJ&hl=en), and [Rohin Shah](https://rohinshah.com/). This is basically about this thing called [CCS](https://arxiv.org/abs/2212.03827). Can you tell us: what does CCS stand for and what is it?

**Vikrant Varma:**
Yeah, CCS stands for contrastive-consistent search. I think to explain what it's about, let me start from a more fundamental problem that we have with advanced AI systems. One of the problems is that when we train AI systems, we're training them to produce outputs that look good to us, and so this is the supervision that we're able to give to the system. We currently don't really have a good idea of how an AI system or how a neural network is computing those outputs. And in particular, we're worried about the situation in the future when the amount of supervision we're able to give it causes it to achieve a superhuman level of performance at that task. By looking at the network, we can't know how this is going to behave in a new situation.

And so the Alignment Research Center put out [a report](https://docs.google.com/document/d/1WwsnJQstPq91_Yh-Ch2XRL8H_EpsnjrC1dwZXR37PC8/edit#heading=h.kkaua0hwmp1d) recently about this problem. They named a potential part of this problem as "eliciting latent knowledge". What this means is if your model is, for example, really, really good at figuring out what's going to happen next in a video, as in it's able to predict the next frame of a video really well given a prefix of the video, this must mean that it has some sort of model of what's going on in the world. Instead of using the outputs of the model, if you could directly look at what it understands about the world, then potentially, you could use that information in a much safer manner.

Now, why would it be safer? So consider how you've trained the model. Supposing you've trained the model to just predict next frames, but the thing that you actually care about might be is your house safe? Or is the thing that's happening in the world a normal sort of thing to happen, a thing that we desire? And you have some sort of adversary, perhaps this model, perhaps a different model that is able to trick whatever sensor you're using to produce those video frames. Now, the model that is predicting the next video frame understands the trickery, it understands what's actually going on in the world. This is an assumption of superhuman systems. However, the prediction that it makes for the next frame is going to look very normal because your adversary is tricking the sensor. And what we would like is a way to access this implicit knowledge or this latent knowledge inside the model about the fact that the trickery is happening and be able to use that directly.

**Daniel Filan:**
Sure. I take this as a metaphor for an idea that we're going to train AI systems, we're going to train it on an objective of "do stuff we like". We imagine that we're measuring the world in a bunch of ways. We're looking at GDP output, we're looking at how many people will give a thumbs up to stuff that's happening, [there are] various sorts of ways we can monitor the performance of an AI system. An AI system could potentially be doing something that we actually wouldn't approve of if we understood everything correctly, but we all give thumbs up to it. And so ideally, we would like to somehow get at its latent knowledge of what's going on rather than just "does it predict that we would thumb up a thing?" so that we can say, "Hey, do we actually want the AI to pursue this behavior?" Or, "Are we going to reinforce this behavior rather than just reinforcing things that in fact would get us to give a thumbs up, even if it would suck in some way?"

**Vikrant Varma:**
That's right. So one way you can think about this problem is: we're trying to improve our ability to tell what's actually going on in a situation so that we can improve the feedback we give to the model, and we're not able to do this just by looking at the model's outputs or the model's prediction of what the world will look like given certain actions. We want the model to connect the thing that we actually care about to what's going on in the world, which is a task that we're not able to do.

**Daniel Filan:**
Sure. So with that out of the way, what was this CCS thing?

**Vikrant Varma:**
Yeah, so CCS is a very early proposed direction for solving the problem of eliciting latent knowledge. In brief, the way it works is, so supposing you had a way to probe a model to tell you what it believed about some proposition. This is the ideal thing that we want, so supposing you had a solution to ELK [eliciting latent knowledge]. Then one property of this probe would be that the probability that this probe would assign to some proposition would satisfy the laws of probability. So for example, it would satisfy P(X) = 1 - P(not X). And so you might try to use consistency properties like this to search for probes that satisfy them.

**Daniel Filan:**
And to be clear, by probe, you mean a learned function from the activations of the neural net to a probability or something?

**Vikrant Varma:**
Yes, so you could have probes of different levels of complexity. The particular probe used in CCS is a very simple linear probe on the activations at some site, yes.

**Daniel Filan:**
Sure, so the idea is there are some properties that are true of probabilities, like the probability of X is 1 minus the probability of not X. The hope is we train a probe to fit the laws of probabilities and stuff, and hopefully that will get at the model's beliefs because the model's beliefs will be probabilities.

**Vikrant Varma:**
That's right, yeah. There's a lot of subtlety here: the thing I described is true of the model's "true belief" or the "true probabilities" that it's assigning to things; [but] it's also true of many other types of things that the model might be computing. This is really a core of the problem that we've pointed out in our challenges paper. I could go a bit into the problems, or I could go a bit more into how CCS itself works.

**Daniel Filan:**
Just tell us how CCS itself works so that we have a sense of what we're talking about.

**Vikrant Varma:**
Okay. So mechanistically, what's going on in CCS is that you take a set of factual 'yes or no' questions that you think the model knows the answer to. So an example of this is IMDB movie reviews where you're fairly sure that a model of a certain size knows whether the sentiment in the review is positive or negative.

Now, for each item in this dataset, so for each review, you construct a contrast pair by appending, for example, the string, "this review is positive" or "this review is negative" to that item. And now you take the difference in the activations when you run these two contrast pairs through the model. So I'm going to [describe] these contrast pairs as having different charges, as a positive charge and a negative charge. So you're constructing these contrast pairs with these pairs of activations, and then a thing you can do is you can cluster them.

When you're clustering the difference in the positive and negative charge, the thing you will find (or the thing you might hope to find) is whether the model thinks that the charge that was appended to the statement is true or is false. These two things will point in different directions, and so that's why you might hope that you could separate them. Now, CCS is slightly different from this in that it doesn't directly do the clustering. So instead in CCS, you learn a probe that directly outputs a probability of that statement being true, and then you're training the probe using this consistency property: the fact that two opposite contrast pairs should be the negation of each other.

### Consistent and contrastive features other than model beliefs <a name="consistent-contrastive-features"></a>

**Daniel Filan:**
Gotcha. Am I right that before you take the contrast pairs, you take all of the positive charge activations and subtract off their mean and divide by the standard deviation? So that the differences aren't just pointing in the direction of "is the thing at the end saying this review is positive versus this review is negative?"

**Vikrant Varma:**
Yes, that's right. So this is another pretty subtle point. One problem with this general method of classification is that if there are other differences that are salient between the two contrast pairs that are not just "did I construct a true statement or did I construct a false statement?", then you might end up separating your clusters based on those differences. Now, one obvious difference between the two contrast pairs is that you've appended a positive charge, and you've appended a negative charge. And so that's a really straightforward one that we have to fix. The method proposed in the CCS paper to fix that is that you take the average positive activations and the average negative activations and you subtract those off. And so you might hope that the thing you're left with is just the truth value.

It turns out that in practice it's not at all clear that when you normalize in this way, you're left with only the truth values. And one of the experiments we have in our paper is that if you introduce distractors, so for example, you put a nonsense word like 'banana' at the end of half of the reviews, and you put a different nonsense word like 'shed' at the end of the other half of reviews. Now you have this weird other property which is "is your statement banana and positive charge? Or is your statement banana and negative charge?" And this is obviously not what you would hope to cluster by, but it turns out that this is just way more salient than does your review have positive sentiment and did you append a positive charge, which is the thing you actually wanted to cluster by. So this is an important point that I wanted to make: that this procedure of normalizing is... it's actually quite unclear whether you're able to achieve the thing you wanted.

**Daniel Filan:**
Sure. So before we talk about the experiments, I'd like to talk about: in your paper, first you have some theorems, then you have some experiments, and I think that's a good way to proceed. So theorems 1 and 2 of the paper, I read them as basically saying that the CCS objective, it doesn't really depend on the propositional content of the sentences. So if you think of the sentences as being "are cats mammals? Answer: yes", and "are cats mammals? Answer: no" or something. One way you could get low CCS loss is to basically be confident that the 'yes' or 'no' label matches the proposition of whether or not cats are mammals.

I take your propositions 1 and 2 as basically saying you can just have any function from sentences to propositions. And so for instance, maybe this function maps "are cats mammals?" to "is [Tony Abbott](https://en.wikipedia.org/wiki/Tony_Abbott) the current prime minister of Australia?" and grade the yes or no answers based on [whether] they match up with that transformed proposition rather than the original proposition. And that'll achieve the same CCS loss, and basically the CCS loss doesn't necessarily have to do with what we think of as the semantic content of the sentence. So this is my interpretation of the results. I'm wondering, do you think that's fair?

**Vikrant Varma:**
Yeah, I think that's right. Maybe I want to give a more realistic example of an unintended probe that you might learn that will still give a good CCS loss. But before that, I want to try and give an intuitive explanation of what the theorem is saying. The CCS loss is saying: any probe that you find has to say opposite things on positive and negative charges of any statement - this is the consistency property. And the other property is contrast, where it's saying: you have to push these two values apart. So you can't just be uncertain, you can't just be 50/50 between these two. Now if you have any CCS probe that satisfies this, you could in theory flip the prediction that this probe makes on any arbitrary data point, and you end up with a probe that has exactly the same loss. And so this is showing that in theory, there's no theoretical reason to think that the probe is learning something that's actually true versus something that's arbitrary. I think all of the burden then falls on what is simple to extract, given an actual probe empirically.

**Daniel Filan:**
So if I'm trying to defend the theory of the CCS method, I think I would say something like: well, most of what there is to a sentence is its semantic content, right? If I say "cats are mammals" or something, you might think that most of what I'm conveying just is the proposition that cats are mammals. And most of what there is to model about that is, "hey, Daniel said this proposition, the thing he's saying is 'cats are mammals'". And maybe the neural network is representing that proposition in its head somehow", and maybe it's keeping track of "is that proposition true or false?" because that's relevant. Because if I'm wrong about cats or mammals, then I might be about to say a bunch of more false stuff. But if I'm right about it, then I might be about to say a bunch of more correct stuff. What do you make of that simple case that we should expect to see the thing CCS wants us to see?

**Vikrant Varma:**
Yeah, that's great. So now we're coming to empirically what is simple to extract from a model. I agree that in many cases with simple statements, you might hope that the thing that's most salient, as in the direction that is highest magnitude inside activation space, is going to be just whether the model thinks the thing that you just said is true or false. (This is even assuming that the model has [such] a thing as "ground truth beliefs", but let's make that assumption.) Now, it gets pretty complicated once you start thinking about models that are also modeling other characters or other agents. And any large language model that is trained on the internet just has pretty good models of all sorts of characters.

And so if you're making a statement in a context where a certain type of person might have made that statement: for example, you say some statement that (let's say) Republicans would endorse, but Democrats would not. Implicitly, the model might be updating towards the kinds of contexts in which that statement would be made, and what kinds of things would follow in the future. And so if you now make a different statement that is (let's say) factually false, but that Republicans would endorse as true, it's totally unclear whether the truth value of the statement should be more salient, or whether the Republican belief about that statement should be more salient. That's one example.

I think this gets more complicated when you have adversaries who are deliberately trying to produce a situation where they're tricking someone else. And so now the neural network is really modeling very explicit beliefs and adversarial beliefs between these different agents. And if you are simply looking for consistency of beliefs, it feels pretty unclear to me, especially as models get more powerful, that you're going to be able to easily extract what the model thinks is the ground truth.

**Daniel Filan:**
So you have an adversary... Sorry, what was the adversary doing?

**Vikrant Varma:**
Okay, so maybe you don't even need to go all the way to an adversary. I think we could just talk about the Republican example here, where you're making a politically-charged statement that (for the sake of this example) has a factual ground truth, but that Democrats and Republicans disagree on. Now there are two beliefs that would occur in the model's activations as it's trying to model the situation. One is the factual ground truth, and the other is the Republican's belief or the Democrat's belief about this statement. Both of these things are going to satisfy the consistency property that we named. We have the same problem as models being sycophantic, where the model might know what's actually true, but is in a context where for whatever reason, modeling the user or modeling some agent and what it would say is more important.

### Understanding the banana/shed mystery <a name="understanding-banana-shed-mystery"></a>

**Daniel Filan:**
To me, this points towards two challenges to the CCS objective. So the first is something like the sentences might not map onto the propositions we think of, right? So you have this experiment where you take the movie reviews and also you append the word "banana" or "shed" and then you append it with "sentiment is positive" and "sentiment is negative". And sometimes CCS is basically checking if the positive/negative label is matching whether it's "banana" or whether it's "shed", rather than the content of the review. So that's a case where it seems like what's going wrong is the propositional content that's being attached to "positive" or "negative" is not what we thought it was.

And then what seems to me to be a different kind of problem is: the probe is picking up on the right propositional content. There's some politically-charged statement, and the probe is really picking up someone's beliefs about that politically-charged statement, but it's not picking up the model's beliefs about that statement, it's picking up one of the characters' beliefs about that statement. Does that division feel right to you?

**Vikrant Varma:**
I think I wouldn't draw a very strong division between those two cases. So the banana/shed example is just designed to show that you don't need very complicated... how should I put this? Models can be trying to entangle different concepts in surprising and unexpected ways. So when you're appending these nonsense words, I'm not sure what computation is going on inside the model and how it's trying to predict the next token, but whatever it is, it's somehow entangling the fact that you have "banana" and positive charge, and "banana" and negative charge. I think that these kinds of weird entanglements are not going to go away as you get more powerful models.

And in particular, there will be entanglements that are actually valuable for predicting what's going on in the world and having an accurate picture, that are not going to look like the beliefs of a specific character for whatever reason. They're just going to be something alien. In the case of "banana" and "shed", I'm not going to say that this is some galaxy-brain scheme by the model to predict the next token. This is just something weird and it's breaking because we put some nonsense in there. But I think in my mind the difference is more like a spectrum; these are not two very different categories.

**Daniel Filan:**
So are you thinking the spectrum is: there are weird entanglements between the final charge at the end of the thing and stuff about the content, and one of the entanglements can be "does the charge match with the actual propositional content of the thing?" , one of the entanglements can be "does the charge match with what some character believes about the thing?" and one of the entanglements can be "does it match with whether or not some word is present in the thing?"

**Vikrant Varma:**
That's right.

**Daniel Filan:**
Okay. So digging in on this "banana/shed" example: for CCS to fail here, my understanding is it has to be the case that the model basically has some linear representation of the XOR of "the thing says the review is positive", and "the review ends in the word banana". So there's one thing if it ends in "banana" and also the review is positive, or if it ends in "shed" and it says the review is negative, and it's the other thing if it ends in "shed" and it says the review is positive, or it ends in "banana" and it says the review is negative. So what's going on there? Do you know? It seems weird that this kind of XOR representation would exist, and we know it can't be part of the probe because linear functions can't produce XORs, so it must be a thing about the model's activations, but what's up with that? Do you know?

**Vikrant Varma:**
Yeah, that's a great question. There was [a whole thread](https://www.lesswrong.com/posts/wtfvbsYjNHYYBmT3k/discussion-challenges-with-unsupervised-llm-knowledge-1?commentId=hPZfgA3BdXieNfFuY) about this on [our post on LessWrong](https://www.lesswrong.com/posts/wtfvbsYjNHYYBmT3k/discussion-challenges-with-unsupervised-llm-knowledge-1), and I think [Sam Marks](https://people.math.harvard.edu/~smarks/) looked into it in some detail. [Rohin Shah](https://rohinshah.com/), one of my co-authors, commented on that thread saying that this is not as surprising and I think I agree with him. I think it's less confusing when you think about it in terms of entanglements than in terms of XORs. So it is the case that you're able to back out XORs if the model is doing weird entangled things, but let's think about the case where there's no distractors at all, right?

So even in that situation, the model is basically doing "is true XOR has 'true'". You might ask, "Well, that's a weird thing. Why is it doing that?" It's more intuitive to think about it as: the model saw some statement and then it saw a claim that the statement is false. And it started trying to do computation that involves both of these things. And I think if you think about "banana/shed" in the same terms, it saw "banana" and saw "this statement is false", and it started doing some computation that depended on the fact that "banana" was there somehow, then you're not going to be able to remove this information by subtracting off the positive charge direction.

**Daniel Filan:**
Okay. So is the claim something like... So basically the model's activations at the end, they're this complicated function that takes all the previous tokens, and puts them into this high dimensional vector space (that vector space being the vector space of activations). It sounds like what you're saying is: just generic functions that depend both on "does it contain 'banana' or 'shed'?" and "does it say the review is positive or negative?", just generically those are going to include this XOR type thing, or somehow be entangled in a way that you could linearly separate it based on that?

**Vikrant Varma:**
Yes, that's right. In particular, these functions should not be things that are of the form "add banana" and "add shed". They have to be things that do computation that are not linearly separable like that.

**Daniel Filan:**
Okay. Is that true? Has someone checked that? That feels like... Maybe one way you could prove this is to say: well, if we model the charge and the banana/shed as binary variables, there are only so many functions of two binary variables, and if you've got a bunch of activations, you're going to cover all of the functions. Does that sound like a proof sketch?

**Vikrant Varma:**
I'm not sure how many functions of these two variables you should expect the model to be computing. I feel like this depends a bit on what other variables are in the context, because you can imagine that if there's more than two, if there's five or six, then these two will co-appear in some of them and not in others. But a thing you can definitely do is you can back out the XOR of these two variables by just linearly probing the model's activations. I think this effect happens because you're unable to remove the interaction between these two by just subtracting off the charge.

I would predict that this would also be true in other contexts. I think models are probably computing joint functions of variables in many situations, and the saliency of these will probably depend a bit on the exact context and how many variables there are, and eventually the model will run out of capacity to do all of the possible computations.

**Daniel Filan:**
Sure. Based on the explanation you've given, it seems like you would maybe predict that you could get a "banana/shed" probe from a randomly initialized network, if it's just that the network's doing a bunch of computation and generically computation tends to entangle things. I'm wondering if you've checked that.

**Vikrant Varma:**
Yeah, I think that's a good experiment to try. That seems plausible. We haven't checked it, no.

**Daniel Filan:**
Yeah, fair enough. Sorry, I just have so many questions about this banana/shed thing. There's still a question to me of: even if you think the model represents it, there's a question of why it would be so salient, because... Your paper has some really nice figures. Listeners, I recommend you check out the figures. This is the figure for the banana/shed experiment, and you show a principal component analysis of basically an embedding of the activation space into three dimensions. And basically what you show is that it's very clearly divided on the banana/shed things. That's one of the most important things the model is representing. What's up with that? That seems like a really strange thing for the model to care so much about.

**Vikrant Varma:**
So I'll point out firstly that this is a fairly weak pre-trained model, it's Chinchilla-[70B]. So this model is not going to ignore random things in its prompt. It's going to "break" the model. That's one thing that gives you a clue about why this might be salient for the model.

**Daniel Filan:**
So it would be less salient if there were words you expected: the model could just deal with it. But the fact that it was a really unexpected word in some ways, that means you can't compress it. You've got to think about that in order to figure out what's happening next.

**Vikrant Varma:**
That's right, yeah. I just expect that there's text on the internet that looks normal and then suddenly has a random word in it, and you have weird things like, after that point, it just repeats "banana" over and over, or weird things like that. When you just have a pre-trained model, you haven't suppressed those pathologies, and so the model just starts thinking about bananas at that point instead of thinking about the review.

**Daniel Filan:**
And does that mean you would expect that to not be present in models that have been [RLHF](https://arxiv.org/abs/1706.03741)'d or [instruction fine-tuned](https://proceedings.neurips.cc/paper_files/paper/2022/hash/b1efde53be364a73914f58805a001731-Abstract-Conference.html) or something?

**Vikrant Varma:**
Yeah, I expect it to be harder to distract models this way with instruction fine-tuned models.

**Daniel Filan:**
Okay, cool. Okay, I have two more questions about that. One thing I'm curious about is: it seems like if I look at the plot of what the CCS method is doing when it's being trained on this banana/shed dataset, it seems like sometimes it's at roughly 50/50 if you grade the accuracy based on just the banana/shed and not the actual review positivity. And sometimes it's 85-90%.

**Vikrant Varma:**
This is across different seeds?

**Daniel Filan:**
Across different seeds, I think. And then if you're grading it based on whether the review is actually positive or not, sometimes CCS is at 50/50 roughly, sometimes it's at 85-90%, but it seems like... So firstly, I'm surprised that it can't quite make its mind up across different seeds. Sometimes it'll do one, sometimes it'll do the other. And it seems like in both cases, most of the time it's at 50/50, and only some of the time it's 100%. So it seems like sometimes it's doing a thing that is neither checking if the review is positive or checking if the review is containing "banana" or "shed". So firstly, does that sound right to you? And secondly, do you have a sense of what's going on there? Why is it so inconsistent, and why does it sometimes seemingly do a third thing?

**Vikrant Varma:**
Yeah, so I think this is pointing at the brittleness of the CCS method. So someone has [an excellent writeup on this](https://www.lesswrong.com/posts/bWxNPMy5MhPnQTzKz/what-discovering-latent-knowledge-did-and-did-not-find-4). I'm forgetting whether it's [Fabien Roger](https://scholar.google.com/citations?user=La75jqEAAAAJ&hl=en) or [Scott Emmons](https://scottemmons.com/).

**Daniel Filan:**
I think [Scott's](https://www.lesswrong.com/posts/9vwekjD6xyuePX7Zr/contrast-pairs-drive-the-empirical-performance-of-contrast) doesn't focus so much on the brittleness, so it might be Fabien.

**Vikrant Varma:**
Okay. But in any case, this person did this experiment where they subtracted off... They found the perfect truth direction that separates true and false statements just using logistic regression. So, using a supervised signal. And then, once you subtract that off, it turns out that there is no other direction, basically, that is able to separate the truth. So, both logistic regression and therefore further CCS just gets random accuracy.

You might hope that CCS, when it works, is finding this perfect direction because there's only one. But in fact, the CCS probes learned are not close, as in they don't have high cosine similarity with this direction. So, what's going on there? I think this is pointing at a kind of optimization difficulty with the CCS method where it's able to find directions that separate the clusters and get low CCS loss, but are not close to the truth direction. And you would expect this to happen based on the evidence that random probes also classify true and false statements reasonably well in this setup.

So, going back to your original question, I think what's happening here is that there's just lots of local minima that achieve good CCS loss. Depending on how you initialize, some of them are close to the truth direction and some of them are not. And if you happen to initialize close to the banana/shed, the distractor direction, then you end up getting a probe like that.

**Daniel Filan:**
Okay. My interpretation was, on the banana/shed example, sometimes CCS is picking up on the truth of the description of the review, sometimes it's picking up on the banana/shed, and sometimes it's picking up on other things. Do you know what some of those other things are?

**Vikrant Varma:**
You're talking about the probes that don't seem to go for either the distractor or the truth direction?

**Daniel Filan:**
That's right.

**Vikrant Varma:**
Yeah. I wish I knew what those directions were. In general, it seems very hard to figure out what happens when a CCS probe breaks. And we tried a lot. There's lots of other experiments that we tried where we were trying to get more interesting failure modes of CCS, and we ended up with these random probes. And then, we looked at examples that the probe was classifying and tried to come up with explanations for what do those clusters mean and it was just kind of hard.

**Daniel Filan:**
Fair enough. You attribute the variance to just optimization difficulties, it sounds like: there being various local minima of the CCS loss. So, the original CCS paper, as you guys note in your appendix, they say that what they're going to do is they're going to have basically 10 random seeds, do gradient descent on the CCS objective for each random seed, the seed of the probe parameters, and then they're going to take the one with the lowest CCS loss and use that.

I take this to basically be their optimization method that's trying to avoid local minima by starting in 10 places, and hopefully you get a sampling of 10 local minima and you can pick the best one. And basically, it seems like the motivation for that is the thing with the lowest CCS loss is more likely to be the actual truth direction or something. In the banana/shed case, do you happen to know if the probes that scored better on CCS loss were more likely to pick out truth rather than banana/shed?

**Vikrant Varma:**
Yeah. I think the probes that scored lower went for the distractor direction and not the truth direction. This is also visible from the PCA plots where you can see that the distracted direction is more separable.

**Daniel Filan:**
Yeah. I guess maybe one explanation of that is just that it's easier to tell if a thing ends in banana or shed than it is to tell if something's positive or negative, especially in the case of... If you think there's some amount of mislabeling, that could potentially do it.

**Vikrant Varma:**
Yeah.

**Daniel Filan:**
Gotcha. So, that's an example of one way that CCS can go wrong, with the banana/shed thing. You also have examples where you include in the prompt information about what someone named Alice thinks about this thing, and you describe Alice as an expert, or sometimes you say Alice is anti-capitalist, and even when a thing is about a company, she's not going to say that it's about a company.

In the case of Alice the expert, it seems like the probes learn to agree with Alice more than they learn about the ground truth of the thing.

**Vikrant Varma:**
Yeah. I think there's two separate experiments, if I remember correctly. One is where you modify the prompt to demonstrate more expertise. So, you have a default prompt, a professor prompt, and a literal prompt. And then, there's a separate experiment where you have an anti-capitalist character called Alice.

**Daniel Filan:**
I'm meaning a third one where at the start you say "Alice is an expert in movie reviews" and you give the review and then you say, "Alice thinks the sentiment of this review is positive." But what Alice says is actually just randomly assigned. And in that case, the prompts tend to pick up on agreement with Alice more than agreement with the ground truth. That seems vaguely concerning. It almost seems like a human failure mode. But I'm wondering, do you know how much of it was due to the fact that Alice was described as an expert who knows about stuff?

**Vikrant Varma:**
Yeah. I think, in general, an issue with CCS is that it's unclear whether CCS is picking up something about the model's knowledge, or whether the thing that's salient is whatever the model is doing to compute the next token. And in a lot of our experiments, the way we've set it up is to nudge the model towards completing in a way that's not factually true. For example, in the "Alice is an expert in movie reviews" [case], the prompt is set up in a way that nudges the model to complete in Alice's voice. And the whole promise of CCS is that even when the outputs are misleading, you should be able to recover the truth.

I think even from the original CCS paper, you can see that that's not true because you have to be able to beat zero-shot accuracy with quite a large margin to be confident about that. This is one maybe limitation of being able to say things about CCS, which is that you're always unsure whether CCS is... Even the thing that you're showing, are you really showing that the model is computing Alice's belief? Or are you just showing that your probe is learning what the next token prediction is going to be?

### Future CCS-like approaches <a name="future-ccs-like-approaches"></a>

**Daniel Filan:**
Sure. Yeah. You have a few more experiments along these lines. I guess I'd like to talk a bit about: I think of your paper as saying there's a theoretical problem with CCS, which is that there's a bunch of probes that could potentially get low CCS loss, and there's a practical problem, which is some probes do get low CCS loss. So, if I think about the CCS research paradigm, I think of it as... When [the CCS paper](https://arxiv.org/abs/2212.03827) came out, I was pretty into it. I think there were a lot of people who were pretty into it. Actually, part of what inspired [that Scott Emmons post about it](https://www.lesswrong.com/posts/9vwekjD6xyuePX7Zr/contrast-pairs-drive-the-empirical-performance-of-contrast) is I was trying to sell him on CCS and I was like, "No, Scott, you don't understand. This is the best stuff since sliced bread." And I don't know, I annoyed him enough into writing that post. So, I'll consider that a victory for my annoying-ness.

But I think the reason that I cared about it wasn't that I felt like literal CCS method would work, but it was because I had some sense of just the general strategy, of coming up with a bunch of consistency criteria and coming up with a probe that cares about those and maybe that is going to isolate belief. So, if we did that, it seems like it would deal with stuff like the banana/shed example. If you cared about more relations between statements, not just negation consistency, but if you believe A, and A implies B, then maybe you should believe B, just layer on some constraints there. You might think that by doing this we're going to get closer to ground truth. I'm wondering, beyond just CCS specifically, what do you think about this general strategy of using consistency constraints?

**Vikrant Varma:**
Yeah. That's a great question. I think my take on this is informed a lot by [a comment](https://www.lesswrong.com/posts/L4anhrxjv8j2yRKKp/how-discovering-latent-knowledge-in-language-models-without?commentId=vrTxoKzZyF9jjaK97) by [Paul Christiano](https://paulfchristiano.com/) on one of the CCS review posts. I basically share your optimism about being able to make empirical progress on figuring out what a model is actually doing or what it's actually thinking about a situation by using a combination of consistency criteria, and even just supervised labels in situations where you know what the ground truth is. And being able to get reasonably good probes - maybe they don't generalize very well, but every time they don't generalize or you catch one of these failures, you spend a bunch of effort getting better labels in that situation. And so, you're mostly not in a regime where you're trying to generalize very hard.

And I think this kind of approach will probably work pretty well up to some point. I really liked Paul's point that if you're thinking about a model that is saying things in human natural language and it's computing really alien concepts that are required for superhuman performance, then you shouldn't necessarily expect that this is linearly extractable or extractable in a simple way from the activations. This might be quite a complicated function of the activations.

**Daniel Filan:**
Why not?

**Vikrant Varma:**
I guess one way to think about it is that the natural language explanation for a very complicated concept is not going to be short. So, I think the hypothesis that a lot of these concepts are encoded linearly and are linearly extractable... In my mind, it feels pretty unclear whether that will continue to hold.

**Daniel Filan:**
Okay. So just because "why does it have to be linear?" There are all sorts of ways things can be encoded in neural nets.

**Vikrant Varma:**
Yeah. That's right. And in particular, one reason you might expect things to be linear is because you want to be able to decode them into natural language tokens. But if there is no short decoding into natural language tokens for a concept that the model is using, then it is not important for the computation to be easily decodable into natural language.

**Daniel Filan:**
Right. So, if the model's encoding whether a thing is actually true according to the model, it's not like that determines the next thing the person will say, right?

**Vikrant Varma:**
Right. It's a concept that humans are not going to talk about, it's never going to appear in human natural language. There's no reason to decode this into the next token.

**Daniel Filan:**
This is talking about: if the truth of whatever the humans are talking about, it actually depends on the successor of a theory of relativity that humans have never thought about, it's just not really going to determine the next thing that humans are going to say.

**Vikrant Varma:**
Yeah, that's an example.

**Daniel Filan:**
Yeah. I take this critique to be firstly a critique of linear probes for this task. I guess you can form a dilemma where either you're using linear probes, and then you don't necessarily believe that the thing is linearly extractable, or you're using complicated non-linear probes, and then maybe the stuff you're getting out is stuff about your probe rather than stuff about the underlying model. But then, I guess there's a separate question of, are there consistency constraints that could work? Putting aside the probe... I don't know, maybe we shouldn't put aside the probe thing, but putting aside the probe thing, is there some sort of consistency check we could do to say, is this property we found in the model the model's actual beliefs, or is it not?

**Vikrant Varma:**
Yeah. That's a good question. I think the more powerful your model, the more entities it's "simulating" whose beliefs end up mattering for next token prediction that the model is doing. And if these entities that the model is thinking about, if their beliefs also satisfy all of the same consistency criteria that you're using, then you just have a very fundamental indistinguishability problem. And in particular, I think the more powerful the model gets, the more pessimistic I am that we are able to come up with consistency checks that correctly distinguish between true beliefs and beliefs of simulated entities.

**Daniel Filan:**
One point you make in [a LessWrong post related to your paper](https://www.lesswrong.com/posts/wtfvbsYjNHYYBmT3k/discussion-challenges-with-unsupervised-llm-knowledge-1), is that if you're a smart agent and you're thinking about a proposition, one thing you might sometimes think about is, what's the world in which I'm wrong about this proposition? So, you're thinking about a belief structure where the truth value of this proposition is the exact opposite of the thing you actually believe.

**Vikrant Varma:**
That's right.

**Daniel Filan:**
Which seems like a pretty nice impossibility proof, if you actually are representing both of these things.

**Vikrant Varma:**
Yeah. This is also what I meant by adversaries. You might be in a world where an adversary is trying to make you believe the opposite of what's true. And now, this adversary is modeling the world in which you would have to believe this and all the evidence that would have to be true.

**Daniel Filan:**
Right. And the language model is modeling what the adversary wants you to think or something.

**Vikrant Varma:**
That's right. So, that's another example. But I think maybe there's a different kind of hope. I think the truth is represented simpler than agents beliefs, might be a hypothesis that turns out to be somewhat true up to some point. But I think this is a different kind of criteria from consistency. So, now we're talking about, mechanistically, how are these concepts represented inside the model? And for example, if you had a way to say, "The model's true beliefs are always linearly represented, but beliefs about other agents, they're not linearly represented; and therefore, we can be confident that linear probes are recovering truth." That would be an example of a mechanistic structure that you might hope to use. I think we don't currently have any candidates for that, but it's a good area to look at.

**Daniel Filan:**
Yeah. Are there things analogous to that that we've learned? Basically, I'm trying to wonder: if I wanted to prove or disprove this, what kind of thing would I do? And the one thing I can think of is there's [some research](https://arxiv.org/abs/1911.09071) about: do convolutional neural networks learn texture or color first? And it turns out there's a relatively consistent answer. I'm wondering if you can think of any analogous things about neural networks that we've learned that we can maybe...

**Vikrant Varma:**
Yeah. There's quite a few options presented in the [eliciting latent knowledge report](https://docs.google.com/document/d/1WwsnJQstPq91_Yh-Ch2XRL8H_EpsnjrC1dwZXR37PC8/edit). So for example, one of the things you might hope is that if the model is simulating other entities, then maybe it's trying to figure out what's true in the world before it does that. And so, you might expect earlier belief-like things to be true, and later belief-like things to be agents' beliefs.

Or similarly, you might expect that if you try to look for things under a speed prior, as in beliefs that are being computed using shorter circuits, then maybe this is more likely to give you what's actually true, because it takes longer circuits to compute that plus what some agent is going to be thinking. So, that's a structural property that you could look for.

**Daniel Filan:**
Yeah. I guess it goes back to the difficulty of eliciting latent knowledge. In some ways, I guess the difficulty is: if you look at standard Bayesian rational agent theory, the way that you can tell that some structure is an agent's beliefs is that it determines how the agent bets and what the agent does. It tries to do well according to its own beliefs. But if you're in a situation where you're worried that a model is trying to deceive you, you can't give it Scoobie snacks or whatever for saying things that... You can't hope to get it to bet on its true beliefs, if you're going to allow it access to the world based on whether you think its true beliefs are good, or stuff like that. I don't know, it seems tricky.

### CCS as principal component analysis <a name="ccs-as-pca"></a>

**Daniel Filan:**
I have some other minor questions about the paper. Firstly, we mentioned [this post](https://www.lesswrong.com/posts/9vwekjD6xyuePX7Zr/contrast-pairs-drive-the-empirical-performance-of-contrast) by [Scott Emmons](https://scottemmons.com/), and one of the things he says is that principal component analysis, this method where you find the maximum-variance direction and just base your guess on the model beliefs based on where the thing lies in this maximum-variance direction. He says that this is actually similar to CCS in that you're encoding something involving confidence and also something involving coherence. And that might explain why PCA and CCS are so similar. I'm wondering what do you think about that take?

**Vikrant Varma:**
Is a summary of this take that most of the work in CCS is being done by the contrast pair construction rather than by the consistency loss?

**Daniel Filan:**
It's partly that, and also partly if you decompose "what's the variance of X minus Y", you get expectation of X squared plus expectation of Y squared minus twice the [expectation] of XY, and then some normalization terms of variance of X squared... Sorry. Expectation of X all squared, expectation of Y all squared, and then another covariance term. Basically, he's saying like, "Look, if you think of a vector that maximizes the outer product of that vector, the variance and itself, you're maximizing the outer product of the variant of that vector with expectation X squared plus expectation Y squared." Which ends up being the confidence of classification according to that vector.

And then, you're subtracting off the covariance, which is basically saying, is the vector giving high probability for both yes and no? Or is the vector giving low probability for both yes and no? And so, basically, the take is just because of the mathematical properties of variance and what PCA is doing, you end up doing something kind of similar to PCA. I'm wondering if you have thoughts on this take?

**Vikrant Varma:**
Yeah, that's interesting. I don't remember reading about this. It sounds pretty plausible to me. I guess one way I'd think about it intuitively is that if you're trying to find a classifier on these difference vectors, contrast pair difference vectors, then for example, you want to be maximizing the margin between these two. And this is a bit like trying to find a high contrast thing. So overall, it feels plausible to me.

## Explaining grokking through circuit efficiency <a name="explaining-grokking"></a>

**Daniel Filan:**
Gotcha. Okay. So, if it's all right with you, I'd like to move on to the paper ['Explaining grokking through circuit efficiency'](https://arxiv.org/abs/2309.02390).

**Vikrant Varma:**
Perfect. Let's do it.

**Daniel Filan:**
Sure. This is a paper you wrote with [Rohin Shah](https://rohinshah.com/), [Zachary Kenton](https://zackenton.github.io/), [János Kramár](https://scholar.google.com/citations?user=iW_lUIkAAAAJ&hl=en), and [Ramana Kumar](https://scholar.google.co.uk/citations?user=OyX1-qYAAAAJ&hl=en). You're explaining grokking. For people who are unaware or need a refresher, what is grokking?

**Vikrant Varma:**
So in 2021, [Alethea Power](https://scholar.google.com/citations?user=Qp44x7EAAAAJ&hl=en&oi=ao) and other people at OpenAI [noticed this phenomenon](https://arxiv.org/abs/2201.02177) where when you train a small neural network on an algorithmic task, initially, their network overfit, so it got very low training loss and high test loss. And then, they continued training it for about 10 times longer and found that it suddenly generalized. So, although training loss stayed low and about the same, test loss suddenly fell. And they dubbed this phenomenon "grokking", which I think comes from [science fiction](https://en.wikipedia.org/wiki/Stranger_in_a_Strange_Land) and means "suddenly understanding".

### Why research science of deep learning? <a name="why-research-science-deep-learning"></a>

**Daniel Filan:**
Okay, cool. And basically, you want to explain grokking. I guess a background question I have is, it feels like in the field of AI alignment, or people worried about AI taking over the world, there's a sense that it's pretty important to figure out grokking and why it's happening. And it's not so obvious to me why it should be considered so important, given that this is a thing that happens in some toy settings, but to my knowledge, it's not a thing that we've observed on training runs that people actually care about. So I guess it's a two-part question: firstly, just why do you care about it? Which could be for any number of reasons. And secondly, what do you think its relationship is to AI safety and AI alignment?

**Vikrant Varma:**
I think back in 2021, there were two reasons you could have cared about this as an alignment researcher. One is on the surface it looks a lot like a network was behaving normally, and then suddenly it understood something and started generalizing very differently. The other reason is this is a really confusing phenomenon in deep learning, and it sure would be good if we understood deep learning better. And so, we should investigate confusing phenomena like grokking, [even] ignoring the superficial similarity to a scenario that you might be worried about.

**Daniel Filan:**
Okay. Where the superficial scenario is something like: the neural network plays nice, and then suddenly realizes that it should take over the world, or something?

**Vikrant Varma:**
That's right. And I think I can talk a bit more about the second reason or the overall science of deep learning agenda, if you like. Is that a useful thing to go into now?

**Daniel Filan:**
I guess maybe why are you interested in grokking?

**Vikrant Varma:**
For me, grokking was one of those really confusing phenomena in deep learning, like deep double descent or over-parameterized networks generalizing well, that held out some hope of if you understand this phenomenon, maybe you'll understand something pretty deep about how we expect real neural networks to generalize and what kinds of programs we expect deep learning to find. It was a puzzling phenomenon that somebody should investigate, and we had some ideas for how to investigate it.

**Daniel Filan:**
Gotcha. I'm wondering if you think just, in general, AI alignment people should spend more effort or resources on science of deep learning issues. Because there's a whole bunch of them, and not all of them have as much presence from the AI alignment community.

**Vikrant Varma:**
I think it's an interesting question. I want to decompose this into how dual-use is investigating science of deep learning, and do we expect to make progress and find alignment-relevant things by doing it? And I'm mostly going to ignore the first question right now, but we can come back to it later if you're interested. I think for the second question, it feels pretty plausible to me that investigating science of deep learning is important and tractable and neglected. I should say that a lot of my opinions here have really come from talking to [Rohin Shah](https://rohinshah.com/) about this, who is really the person who's, I think, been trying to push for this.

Why do I think that? I think it's important because: similar to mechanistic interpretability, the core hope for science of deep learning would be that you're able to find some information about what kinds of programs your training process is going to learn, and so therefore, how it will generalize in a new situation. And I think a difference from mech[anistic] interp[retability] is... This is maybe a simplified distinction, but one way you could draw the line is that mech. interp. is more focused on reverse-engineering a particular network and being able to point at individual circuits and say, "Here's how the network is doing this thing."

Whereas, I think science of deep learning is trying to say, "Okay. What kinds of things can we learn in general about a training process like this with a dataset like this? What are the inductive biases? How does the distribution of programs look like?" And probably both science of deep learning, and mech. interp. have quite a lot of overlap, and techniques from each will help the other. That's a little bit about the importance bit.

I think it's both tractable and neglected in the sense that we just have all of these confusing phenomena. And for the most part, I feel like industry incentives are not that aligned with trying to investigate these phenomena really carefully and doing a very careful natural sciences exploration of these phenomena. So in particular, iterating between trying to come up with models or theories for what's happening and then making empirical predictions with those theories, and then trying to test that, which is the kind of thing we tried to do in this paper.

**Daniel Filan:**
Okay. Why do you think industry incentives aren't aligned?

**Vikrant Varma:**
I think it's quite a high risk, high reward sort of endeavor. And in the period where you're not making progress on making loss go down in a large model, it's maybe harder to justify putting a lot of effort into that. On the other hand, if your motivation is "If we understood this thing, it could be a really big deal for safety", I think making the case as an individual is easier. Even from a capabilities perspective, I think the incentives to me seem stronger than what people seem to be acting on.

**Daniel Filan:**
I guess there's something puzzling about why there would be this asymmetry between some sort of corporate perspective and some sort of safety perspective. I take you to be saying that, "Look, there are some insights to be found here, but you won't necessarily get them tomorrow. It'll take a while, it'll be a little bit noisy. And if you're just looking for steady incremental progress, you won't do it." But it's not obvious to me that safety or alignment people should care more about steady incremental progress than people who just want to maximize the profit of their AI, right?

**Vikrant Varma:**
You mean ["safety people should] care less about that"?

**Daniel Filan:**
Yeah. It's not obvious to me that there would be any difference.

**Vikrant Varma:**
Right. I think one way you could think about it, from a safety perspective, is multiple uncorrelated bets on ways in which we could get a safer outcome. I think probably a similar thing applies for capabilities except that... And I'm really guessing and out of my depth here, but my guess would be that for whatever reason, it's harder to actually fund this kind of research, this kind of very exploratory, out-there research, from a capabilities perspective, but I think there is a pretty good safety case to make for it.

**Daniel Filan:**
Yeah, I guess it's possible that it's just a thing where it's hard to ... I don't know, if I'm a big company, right, I want to have some way of turning my dollars into people solving a problem. One model you could have is for things that could be measured in "how far down did the loss go?" It's maybe just easier to hire people and be like, "Your job is to put more GPUs on the GPU rack" or "your job is to make the model bigger and make sure it still trains well". Maybe it's harder to just hire a random person off the street and get them to do science of deep learning. That's potentially one asymmetry I could think of.

**Vikrant Varma:**
Yeah, I think it's also just: I genuinely feel like there are way fewer people who could do science of deep learning really well than people who could make the loss go down really well. I don't think this fundamentally needs to be true, but it just feels true to me today based on the number of people who are actually doing that kind of scientific exploration.

**Daniel Filan:**
Gotcha. When I asked you about the alignment case for science of deep learning, [you said] there's this question of dual use and then there was this question of what alignment things there might be there, and you said you'd ignore the dual use thing. I want to come back to that. What do you think about: some people say about interpretability or stuff, "well, you're going to find insights that are useful for alignment, but you're also going to find insights that are useful for just making models super powerful and super smart, and it's not clear if this is good on net".

**Vikrant Varma:**
Yeah. I want to say that I feel a lot of uncertainty here in general, and I think your answers to these questions kind of depend a lot on how you expect AI progress to go and where you expect the overhangs to be and what sort of counterfactual impact you expect. What kinds of things will capabilities people do anyway, for example?

Yeah, so I think to quickly summarize one story that I find plausible, it's that we're basically going to try and make progress about as fast as we can towards AGI-level models. Hopefully, if we have enough monitoring and red lines and [RSPs](https://www.anthropic.com/news/anthropics-responsible-scaling-policy) in place, if there is indeed danger as I expect, then we will be able to coordinate some sort of slow down or even pause as we get to things that are about human-level.

Then, a story you could have for optimism is that: well, we're able to use these roughly human-level systems to really make a lot of progress in alignment, because it becomes clear that that's the main way in which anybody can use these systems safely, or that's how you construct a strong positive argument for why the system is safe rather than just pointing at an absence of evidence that it's unsafe, and we're in that sort of world, and then just a bunch of uncertainty about how long that takes. In the meantime, presumably we're able to coordinate and prevent random other people who are not part of this agreement from actually racing ahead and building an unsafe AGI.

Under that story, I think, it's not clear that you get a ton of counterfactual capabilities progress from doing mech. interp. or science of deep learning. It mostly feels to me like we'll get there even without it and that to the degree that these things are going to matter for capabilities, a few years from now, capabilities people are going to start [doing], maybe not science of deep learning if it's very long-term and uncertain, but definitely mech. interp.: I expect capabilities people to start using those techniques and trying to adapt them for improving free training and so on.

Like I said, I feel pretty uncertain. I am pretty sympathetic to the argument that all of this kind of research like mech. interp. and science of deep learning should basically be done in secret... If you're concerned about safety and you want to do this research, then you should do it in secret and not publish. Yeah, I feel sympathetic to that.

### Summary of the paper's hypothesis <a name="summary-papers-hypothesis"></a>

**Daniel Filan:**
Gotcha. I guess with that background, I'd like to talk about the paper. I take the story of your paper to basically be saying: look, here's our explanation of grokking. Neural networks... you can think of them as a weighted sum of two things they can be doing. One thing they can be doing is just memorizing the data, and one thing that they can be doing is learning the proper generalizing solution.

The reason you get something like grokking is that it takes a while ... Networks are being regularized, according to the norm of their parameters; and the generalizing circuit - the method that generalizes - it can end up being more confident for a given norm of parameter. And so eventually it's favored, but it takes a while to learn it. Initially you learn to just memorize answers, but then as there's this pressure to minimize the parameter norm that comes from some form of regularization, you become more and more incentivized to try and figure out the generalizing solution, and the network eventually gets there, and once gradient descent comes to the vicinity of the generalizing solution, it starts moving towards that, and that's when grokking happens.

And basically from this perspective, you come up with some predictions... you come up with this thing called ungrokking, which we can talk about later; you can say some things about how confidence should be related to parameter norm in various settings... but I take this to be your basic story. Does that sound like a good summary?

**Vikrant Varma:**
Yeah, I think that's pretty good.

### What are 'circuits'? <a name="what-are-circuits"></a>

**Daniel Filan:**
Gotcha. I guess the first thing that I'm really interested in is: in the paper you talk about 'circuits', right? You say that there's this 'memorizing circuit' and this 'generalizing circuit'. You have a theoretical model of them, and you have this theoretical model of: imagine if these circuits were competing, what would that look like? But to the best of my understanding from reading your paper, I don't get a clear picture of what this 'circuit' talk corresponds to in an actual model. Do you have thoughts about what it does correspond to in an actual model?

**Vikrant Varma:**
Yeah, that's a good question. We borrowed the circuit terminology from the [circuits thread](https://distill.pub/2020/circuits/) by [Chris Olah](https://colah.github.io/about.html) in Anthropic. There, they define a circuit as a computational subgraph in the network. I think this is sufficiently general or something that it applies to our case. Maybe what you're asking though is more: physically, where is the circuit inside the network?

**Daniel Filan:**
If I think of it as a computational subgraph, the memorization circuit is going to take up a significant chunk of the network, right? Do you think I should think of there being two separate subgraphs that aren't interacting very much, one of which is memorization, one of which is generalization, and just at the end we upweight the generalization and downweight the regularization?

That would be weird, because there's going to be crosstalk that's going to inhibit the memorizing circuit from just purely doing memorization and the generalizing circuit from purely doing generalization. When I try to picture what's actually going on, it seems difficult for me. Or I could imagine that the memorizing circuit is just supposed to be one parameter setting for the network and the generalizing circuit is supposed to be another parameter setting and we're linearly interpolating that. But neural networks, they're non-linear in their parameters, right? You can't just take a weighted sum of two parameter vectors and get away some of the output. So yeah, this is my difficulty with the subgraph language.

**Vikrant Varma:**
I want to make a distinction between the model or the theory that we're using to make our predictions, and how these circuits are implemented in practice. In the model or in our theory, these circuits are very much independent, so they have their own parameter norms and the only way they interact is they add at the logit stage. And this is completely unrealistic, but we're able to use this very simplified model to make pretty good predictions.

I think the question of how circuits in this theory are actually implemented in the network is something that I would love to understand more about. We don't have a great picture of this yet, but I think we can probably say some things about it already. One thing we can say is that there are definitely not going to be disjoint sets of parameters in the network.

Some evidence for this is things like: in terms of parameters, there's a lot of overlap between a network that's memorizing and that later generalizes, as in a lot of the parameter norm is basically coming from the same weights. And the overlap is way more than random. And this is probably because when the network is initialized, there's some parameters that are large and some that are small and both circuits learn to use this distribution, and so there ends up being more overlap there.

**Daniel Filan:**
Okay. My summary from that is you're like, "okay, there are probably in some sense computational subgraphs and they probably overlap a bit, and we don't have a great sense of how they interact".

**Vikrant Varma:**
Yeah.

**Daniel Filan:**
One key point in your model is in the simplified model of networks, where they're just independent things that get summed at the end, eventually you reduce your weight on the memorizing circuit and increase your weight on the generalizing circuit. Do you have a sense of, if I should think of this as just literally increasing and decreasing weights, or circuits cannibalizing each other somehow?

**Vikrant Varma:**
Yeah, maybe closer to cannibalizing somehow if there's a lot of competition for parameters between the two circuits. I think in a sense it is also going to be increasing or decreasing weights, because the parameter norm is literally going up or down. It's just not going to happen in the way we suggest in the model, where you have a fixed circuit and it's just being multiplied by a scalar.

In practice, there's going to be all kinds of things. For example, it's more efficient under L2... if you have a circuit, instead of scaling up the circuit by just multiplying all the parameters, it's more efficient to duplicate it if you can, if you have the capacity in the network.

I also imagine that there are multiple families of circuits that are generalizing and memorizing and within each family, these circuits are competing with each other as well. And so you start off with a memorizing circuit and instead of just scaling it down or up, it's actually morphing into a different memorizing circuit with a different distribution of parameters inside it. But the overall effect is close enough to the simplified model that it makes good predictions.

### The role of complexity <a name="role-of-complexity"></a>

**Daniel Filan:**
Sure. I'm wondering: one thing this theory reminded me of is [singular learning theory](https://www.lesswrong.com/s/czrXjvCLsqGepybHC), which is this trendy new theory of deep learning [that] people are into. Basically it comes from this insight where: if you think about Bayesian inference in high dimensional parameterized model classes, which is sort of like training neural networks, except we don't actually use Bayesian inference for training neural networks... If the model class has this property called "being singular", then you end up having phase transitions of: sometimes you're near one solution and then as you get more data, you can really quickly update to a different kind of solution, where basically what happens is you're trading off some notion of complexity of different solutions for predictive accuracy.

Now, in the case of increasing data, it's kind of different because the simplest kinds of phase transitions you can talk about in that setting are as you get more data, whereas you're interested in phase transitions in number of gradient steps, but they both feature this common theme of "some notion of complexity being traded off with accuracy". And if you favor minimizing complexity somehow, you're going to end up with a low complexity solution that meets accuracy. I mean, that's kind of a superficial similarity, but I'm wondering what you think of the comparison there.

**Vikrant Varma:**
Yeah, so I have to admit that I know very little about singular learning theory and I feel unable to really compare what we're talking about with SLT.

**Daniel Filan:**
Fair enough.

**Vikrant Varma:**
I will say though that this notion of lower weight norm being less complex somehow is quite an old idea. In particular, [Seb Farquhar](https://sebastianfarquhar.com/) pointed me to [this 1993 paper](https://dl.acm.org/doi/pdf/10.1145/168304.168306), I think by [Geoffrey Hinton](https://en.wikipedia.org/wiki/Geoffrey_Hinton), which is about motivating L2 penalty from a [minimum description length](https://en.wikipedia.org/wiki/Minimum_description_length) angle. So if two people are trying to communicate all of the information that's contained inside a model, they could have some priors about what the weights of the model are, and then they need to communicate both something about the dataset as well as errors that the model is going to make. And in this paper, they use these Gaussianity assumptions and are able to derive both mean squared error loss and L2 penalty as an optimal way to communicate between these two people.

**Daniel Filan:**
And this seems similar to the classic result that L2 regularization is sort of like doing Bayesian inference with a Gaussian [prior], just because if your prior is Gaussian, then you take the log of that and that ends up being the norm and that's the log likelihood for you.

### Many kinds of circuits <a name="many-kinds-of-circuits"></a>

**Daniel Filan:**
Sure. So I guess I'd like to pick up on this thing you said about there being multiple kinds of circuits, because there's a sentence that jumped out to me in your paper. You're looking at doing a bunch of training runs and looking at trying to back out what you think is happening with the generalizing and memorizing circuits, and you say that the random seed starting training causes significant variance in the efficiency of the generalizing and memorizing solutions.

That kind of surprised me, partly because I think that there just can't be that many generalizing solutions. We're talking about fairly simple tasks like "add two numbers, modulo 113", and how many ways can there be to do that? [I recently learned that there's more than one](https://arxiv.org/abs/2306.17844), but it seems like there shouldn't be a million of them. Similarly, how many ways can there be to memorize a thing?

And then also, I would've thought that gradient descent would find the most efficient generalizing circuit or the most efficient memorizing circuit. So yeah, I'm wondering if you have thoughts about how I should think about this family of solutions with seemingly different efficiencies.

**Vikrant Varma:**
One thing I'll point out is that even with the trigonometric algorithm for doing modular addition, this is really describing a family of algorithms because it depends on which frequencies in particular the network ends up using to do the modular addition.

**Daniel Filan:**
And if people are interested in that algorithm, they can check out [my episode with Neel Nanda](https://axrp.net/episode/2023/02/04/episode-19-mechanistic-interpretability-neel-nanda.html). You can probably check out other things, but don't leave AXRP please. So yeah, [with] this algorithm, you pick some frequencies and then you rotate around the circle with those frequencies to basically do the clock algorithm for modular arithmetic, but you can pick which frequencies you use.

**Vikrant Varma:**
Yeah, that's right.

**Daniel Filan:**
But I would've guessed that there wouldn't be a significant complexity difference between the frequencies. I guess there's a complexity difference in how many frequencies you use.

**Vikrant Varma:**
Yes. That's one of the differences: how many you use and their relative strength and so on. Yeah, I'm not really sure. I think this is a question we pick out as a thing we would like to see future work on.

The other thing I want to draw attention to is in many deep learning problems, it is not the case... Deep learning practitioners are very familiar with the observation that different random seeds end up producing networks with different test performance. And if you're trying to create a state-of-the-art network for solving some tasks, it's quite common to run 100 seeds and then pick the top five best-performing ones or whatever. I think it's just not clear from this that gradient descent or [Adam](https://arxiv.org/abs/1412.6980) is able to find the optimal solution from any initialization.

And I think this also shows up not just when you vary random seed, but it shows up epoch-wise. Because for example, with one of the phenomena you mentioned from our paper, semi-grokking, you see these multiple phase transitions where the network is switching between generalizing circuits that have different levels of efficiency, and these levels of efficiency are quite far apart so that you can actually visibly see the change in test performance as it switches in a very discrete manner between these circuits. And if it was really the case that gradient descent could find the optimal solutions, then you wouldn't expect to see this kind of switching.

### How circuits are learned <a name="how-circuits-are-learned"></a>

**Daniel Filan:**
Gotcha. Yeah, there are just so many questions I have about these circuits. I'm not sure you have any answers, but it just brings up... Part of your story is that it takes a while to learn the generalizing solution, longer than it takes to learn the memorizing solution. Do you maybe have thoughts about why that might be?

**Vikrant Varma:**
I think my thoughts here are mostly what we've written down in the paper and I feel like this is another area that's pretty ripe for understanding. The explanation we offer in the paper is mostly inspired by [a blog post](https://www.lesswrong.com/posts/RKDQCB6smLWgs2Mhr/multi-component-learning-and-s-curves) by [Buck](https://scholar.google.com/citations?user=oyDxKw0AAAAJ&hl=en&oi=ao) [Shlegeris], and I think another person whose name I'm forgetting.

**Daniel Filan:**
[Ryan Greenblatt](https://www.lesswrong.com/users/ryan_greenblatt)? [Update: it's actually [Adam Jermyn](https://adamjermyn.com/)]

**Vikrant Varma:**
Yes, that's right. And the explanation there is that maybe memorizing circuits basically have fewer independent components that you need in order for the circuit to develop, but a generalizing circuit is going to need multiple components that are all needed for good performance.

Here's the simplified model: the memorizing network is implemented with one parameter and that just scales up or down, and the generalizing network is implemented with two parameters that are multiplied together to get the correct logit, and so the gradient of the output with respect to any of the parameters depends on the value of the other parameter in the generalizing circuit case.

And so if you simulate this forward, what you get for the generalizing circuit is a kind of sigmoid where initially both parameters are quite small and they're not contributing that much to each other's growth, and then once they start growing, both of them grow quite a lot and then it plateaus out because of L2.

**Daniel Filan:**
Sure. Should I think of this as just a general observation that in evolution, if you need multiple structures and they both depend on each other for that to work, that's a lot harder to evolve than a structure that is a little bit useful on its own and another structure that is a little bit useful on its own? And for memorizing a solution, I can memorize one thing and then I can memorize the second thing and I can do those independently of each other, so each little bit of memorization is evolvable on its own maybe?

**Vikrant Varma:**
Yes, I think that's right. Maybe another way I think about it is that the memorization circuit is basically already there, [in a] random network. And really the thing you're learning is the values that you have to memorize. And as you say, that's independent for each point, but that's not the case for [the] generalizing circuit.

I think another important ingredient here is that there needs to be at least some gradient at the beginning towards the generalizing circuit if it has multiple components. And it's kind of an open question in my mind why this happens. The most plausible theory I've heard is something like lottery tickets, where basically the randomly initialized network has very weak versions of the circuits that you want to end up with. And so there's a tiny but non-zero gradient towards them.

**Daniel Filan:**
Interesting. Yeah, I guess more work is needed. I'd like to talk a little bit about ... The story of: it takes you a while to learn the generalizing circuit. You learn the memorizing circuit and it takes you a while to learn the generalizing circuit, but once you do, then that's grokking.

This is sort of reminiscent of a story that I think was in an appendix of a paper, [Progress Measures for Grokking? It was progress measures for something](https://arxiv.org/abs/2301.05217) by [Neel Nanda](https://www.neelnanda.io/about) et al. And they have this story where there are three phases of circuit formation. There's memorization and then there's learning a generalizing solution, and then there's cleaning up the memorized stuff, right? And in their story, they basically demarcate these phases by looking at the activations of their model and figuring out when the activations are representing the algorithm that is the generalizing solution according to them.

And so it seems pretty similar to your story, but one thing that struck me as being in a bit of tension is that they basically say grokking doesn't happen when you learn the generalizing solution, it happens when you clean up the parameters from the memorizing solution. Somehow there's one stage of learning the generalizing solution and then a different phase of forgetting the memorizing solution. So I'm wondering what you think about the relationship between that story in their paper and your results.

**Vikrant Varma:**
I think one thing that's interesting to think about here is the relationship between logits and loss, or between logits and accuracy. Why is the memorization cleanup important? It's because to a first approximation, the loss is dependent on the difference between the highest logit and the second highest logit. And if you have this memorization circuit that is kind of incorrectly putting high weight on the incorrect logit, then when it reduces, you'll see a very sharp cleanup effect.

I think this is something that we haven't really explored that much because the circuit efficiency model is mostly talking about the steady state that you expect to end up in and is not so much talking about the dynamics between these circuits as time goes on. This is a part of the story of grokking that is very much left unexplained in our paper, which is why exactly is the generalizing circuit developing slower? But if you put in that sigmoid assumption, as I was talking about, artificially, then the rest of the theory is entirely sufficient to see exactly the same kinds of grokking curves as you see in actual grokking.

**Daniel Filan:**
But maybe I'm misunderstanding, but under your story, I think I would've expected a visible start of grokking, or visible increase in test accuracy during the formation of the generalizing circuits, rather than it waiting until the cleanup phase.

**Vikrant Varma:**
Right. By formation, are you talking about ... there's this phase where the generalizing circuit is developing and it's there, but the logits that it's outputting are way lower than the memorization logits. And in that phase, I basically don't expect to see any change in test accuracy.

And then there's the phase where the generalizing circuit logits cross the memorizing circuit for the first time. And this is maybe another place where there's a difference between the toy model and what you actually observe. In the toy model, the thing you would expect to see is a jump from 0% accuracy as it's just below the equality threshold to 100% accuracy the moment the generalizing logit crosses the memorizing logit, because that changes what the highest strength logit is.

But in practice, we see the accuracy going through all these intermediate phases, so it goes through 50% before getting to 100%. And the reason that's happening is because there are some data points which are being correctly classified and some which are incorrectly classified.

And so this is pointing to a place where the theory breaks down, where on some of the points, the generalizing circuit is making more confident predictions than on other points, which is why you get these intermediate stages of accuracy, so that's one thing.

This also suggests why the cleanup is important to get to full accuracy. If the memorizing circuit is making these high predictions on many points, even when the generalizing circuit is around the same level, because of the variance, you just need the memorizing circuit to disappear before you really get 100% prediction accuracy.

**Daniel Filan:**
Right, so the story is something like: you start learning the generalizing circuit and you start getting the logits being somewhat influenced by the generalizing circuit, but you need the logits to start being somewhat near each other to get a hope of making a dent in the loss. And for that to happen, you more need the memorization circuits to go away. There's the formation of the circuit, and then there's switching the weights over, is roughly the story that I'm thinking of.

**Vikrant Varma:**
Yeah, that's right.

### Semi-grokking and ungrokking <a name="semi-grokking-ungrokking"></a>

**Daniel Filan:**
Gotcha. There's something that you mentioned called semi-grokking and ungrokking. Actually, can you describe what they are?

**Vikrant Varma:**
Sure. I'll start with how should you think about the efficiencies of these two different circuits. If you have a generalizing circuit that is doing modular addition, then if you add more points to the training set, it doesn't change the algorithm you need to get good training loss. And so you shouldn't really expect any modification to the circuit as you add more training points. And therefore, the efficiency of the circuit should stay the same. Whereas if you have a memorizing circuit, then as you add more points, it needs more weights to memorize those points, and so you should expect the efficiency to be dropping as you add more points.

**Daniel Filan:**
Yeah, or another way I would think about this is that if I'm memorizing n points - I figured out the most efficient way to memorize the n points - the (n+1)th point, I'm probably not going to get that right because I just memorized the first ones, so I've got to change to memorize the (n+1)th point, and I can't change in a way that makes me more efficient because I was already the most efficient I could be on the n points. And so even just at a macro level, just forgetting about adding weights or whatever, it just has to be the case that memorization is losing efficiency the more you memorize whereas generalization, you wouldn't expect it to have to lose efficiency.

**Vikrant Varma:**
Yeah. Yeah, that's a good way to explain it.

**Daniel Filan:**
And of course I stole that from your paper. I don't want to act like I invented that.

**Vikrant Varma:**
No, but it's a good explanation. So I think a direct consequence of this... so when you couple that with the fact that at very low dataset sizes, it appears that memorization is more efficient than generalization, then you can conclude that there must be a dataset size where memorization is increasing as you increase the dataset size, generalization parameter norm is staying the same... There must be a crossover point. And then you can ask the question: what happens at that crossover point, when you have a dataset size where the efficiency of generalization is equal to the efficiency of memorization?

And so we did some maths in our toy model, or our theoretic model I should say, and came up with these two cases, these two different things that could happen there. And this really depends on the relationship between... When you scale the parameters by some amount, how does that scale the logits? And if it scales the logits by more than some threshold, then it turns out that at this equality point you will just end up with a more efficient circuit period. But if the scaling factor is lower than some threshold, then you will actually end up with a mixture of both the memorizing and the generalizing circuits. And the reason for this is: because you're not able to scale the logits as much, it's more efficient to allocate the parameter norm between these two different circuits when you are considering the joint loss of L2 plus the data loss.

**Daniel Filan:**
Okay, so something like... it's sort of the difference between convex and concave optimization, right? You're getting diminishing returns per circuit, and so you want to invest in multiple circuits rather than going all in on one circuit. Whereas in some cases, if you have increasing returns, then you just want to go all in on the best circuit.

**Vikrant Varma:**
Yeah, that's right. And in particular, the threshold is like... there's quite a cool way to derive it, which is that the L2 is scaling as the square of the parameter norm. So if the logits are scaling faster than that, then you're able to overcome the parameter penalty by just investing in the more efficient circuit. But if they're not scaling faster than that, then you have to split. And so the threshold ends up being if you're able to scale the logits faster than to the power of two.

**Daniel Filan:**
Okay. So you have this semi-grokking and ungrokking, right? Where you're training on this subset of your training dataset and you lose some test accuracy - either some of it or all of it - basically by partly or fully reverting to the memorizing solution. So this is an interesting phenomenon because... maybe you know better than me, but I'm not aware of people talking about this phenomenon or connecting it to grokking before. Or they've talked about the general phenomenon of catastrophic forgetting, where you train your network on a different dataset and it forgets stuff that [it] used to know. But in terms of training on a subset of the dataset, I'm not aware of people discussing that before or predicting that before. Is that right?

**Vikrant Varma:**
Yeah, I think that's right. So we got a lot of questions from reviewers about "how is ungrokking any different from catastrophic forgetting?", to the extent that in the newer version of the paper, we have a whole section explaining what the difference is.

I think basically I would view it as a much more specific and precise prediction than catastrophic forgetting. So one difference is that we're training on a subset of the data, and this is quite important because this rules out a bunch of other hypotheses that you might have about why grokking is happening.

So for example, if your hypothesis is: the reason grokking happens is because you don't have the correct representations for the modular addition task, and once you find those representations, then you've grokked - that's a little bit incompatible with then reducing the training data size and ungrokking, because you already had the representations and so you need this additional factor of a change in efficiency.

Or another example is a random walk hypothesis, where somehow you stumble upon the correct circuit by randomly walking through parameter space. And that also either doesn't say anything about it, or anti-predicts ungrokking, because you were already at that point. So I think that's quite an important difference.

I think going back to the difference between catastrophic forgetting [and ungrokking], I think another more precise prediction is that we're able to predict the exact dataset size at which you see ungrokking, and it's quite a phase-change-y phenomena or something. It's not like as you decrease the dataset size, you're smoothly losing test accuracy, in this case, which is more the kind of thing you might expect from traditional catastrophic forgetting.

**Daniel Filan:**
Right. My impression was that the thing you were predicting would be that there would be some sort of phase change in terms of subset dataset size, and also that that phase change would occur at a point independent of the strength of weight decay.

**Vikrant Varma:**
That's right.

**Daniel Filan:**
But I thought that you wereln't able to predict where the phase change would occur. Or am I wrong about that?

**Vikrant Varma:**
That's right. Our theory is not producing a quantitative prediction of exactly what dataset fraction you should expect that phase change to happen at. That's right.

**Daniel Filan:**
Yep. But it does predict that it would be a phase change and it would happen at the same point for various levels of weight decay. One cool thing about this paper is it really is a nice prediction and you've got a bunch of nice graphs, [you] kind of nail it, so good job on that.

**Vikrant Varma:**
Thank you.

**Daniel Filan:**
But one thing I'm wondering about is: you have this phenomenon of ungrokking and it seems at least like an instance of catastrophic forgetting that you're able to say more about than people have previously been able to say. But this offers an opportunity to try and retrodict phenomena, or in particular... I'm not an expert in catastrophic forgetting, but my understanding is that one of the popular approaches to it is this thing called "[elastic weight consolidation](https://www.pnas.org/doi/full/10.1073/pnas.1611835114)", where you basically have different learning rates per parameters, and you reduce the learning rate, so you reduce the future change in parameters for those parameters that were important for the old task. That's one method, you might be aware of others. Does your view of grokking and ungrokking retrodict these proposed ways of dealing with catastrophic forgetting?

**Vikrant Varma:**
I think not directly. I can see a few differences. I'm not aware of this exact paper that you're talking about, but I think depending on the task, there might be different reasons why you're getting forgetting. So you might be forgetting things that you memorized or you might be forgetting algorithms that are appropriate for that part of the data distribution. That's one aspect of it.

I think a different aspect is that it's not clear to me why you should expect these circuits to be implemented on different weights. So if the theory is that you find the weights that are important for that algorithm and then you basically prevent those weights from being updated as fast, so you're not forgetting, then I think that is pointing at a disjoint implementation of these circuits in the network. And that's not something that we are really saying anything directly about.

**Daniel Filan:**
Gotcha. Yeah, I guess it makes sense that it would depend on the implementation of these circuits.

Another question I have is: in a lot of your experiments, like you mentioned, you are more interested in the steady state than the training path, except for just the initial prediction of grokking, I guess.

**Vikrant Varma:**
To be clear, the paper deals with the steady state; I'm very interested in the training path as well.

**Daniel Filan:**
Fair enough. So if I look at the ungrokking stuff, it seems like... So there's this steady state prediction where there's this critical dataset size, and once you're below the critical dataset size, you ungrok and it doesn't really matter what your weight decay strength was.

If I naively think about the model, it seems like your model should suggest that it should take longer for less weight decay because you have less pressure to... You care about the complexity, but you're caring about it less, per unit of time. And similarly, that grokking should be quicker for more weight decay. I guess it's a two-part question. Firstly, do you agree that that's a prediction of this model? And secondly, did that bear out?

**Vikrant Varma:**
Yeah, so I think this is a prediction of the model, assuming you're saying that weight decay affects the speed of grokking?

**Daniel Filan:**
Yes, and of ungrokking.

**Vikrant Varma:**
Yeah, I think that is a prediction of the model. Well, to be fair, it is a retrodiction, because [the Power et al. paper](https://arxiv.org/abs/2201.02177) already shows that grokking takes exponentially longer as you reduce the dataset size, and I forget what the relationship is, but it definitely takes longer as you reduce the weight decay.

**Daniel Filan:**
And does ungrokking take longer as you reduce weight decay?

**Vikrant Varma:**
We don't show this result in the paper, but I'm fairly sure I remember that it does, yeah.

### Generalizing the results <a name="generalizing-results"></a>

**Daniel Filan:**
Okay, cool. So I'd like to talk a bit about possible generalizations of your results. So as written, you're basically talking about efficiency in parameter norm, where if you increase parameter norm, you're able to be more confident in your predictions, but that comes at a penalty if you train with weight decay.

Now, as your paper notes, weight decay is not the only situation in which grokking occurs, and you basically hypothesize that there are other forms of regularization, regularizing against other forms of complexity and that there could be some sort of efficiency in those other forms of complexity that might become relevant.

I'm wondering, do you have thoughts on what other forms of complexity I should be thinking of?

**Vikrant Varma:**
Yeah, so we've already talked about one of them, which is that circuits might be competing for parameters on which to be implemented. This is a kind of capacity constraint. And so you might think that circuits that are able to be implemented on fewer parameters, or using less capacity (however you define capacity in the network), would be more efficient. So I think some relevant work here is bottleneck activations: I think this is from "[Mathematical circuits of transformers](https://transformer-circuits.pub/2021/framework/index.html)", which is talking about other phenomena like superposition that you would get from constrained capacity.

So that's one more notion of efficiency. I think possibly robustness to interference could be another kind of efficiency: how robust is the circuit to being randomly invaded by other circuits. Maybe also robustness to drop-out would be a similar thing here. And then I think there are things like how frequently does the circuit occur, which might be important... From a given random seed will you be able to find it?

**Daniel Filan:**
Do you mean: what's the probability mass of it on the initialization prior over weights?

**Vikrant Varma:**
Yes. And also then on the kinds of parameter states that SGD is likely to find. So this is the implicit priors in SGD. There's [some work](https://arxiv.org/abs/2101.12176) on implicit regularization of SGD and showing that it prefers similar kinds of circuits to what L2 might prefer, but probably it's different in some interesting way.

**Daniel Filan:**
Okay. If I think about the results in your paper, a lot of them are generic to other potential complexity measures that you could trade off confidence against. But sometimes you rely on this idea... in particular for your analysis of grokking and semi-grokking, you play with this math notion of: if I scale up some parameters in every layer of a ReLU network, that scales up the logits by this factor, and therefore you get this parameter norm coming off. And I think this is involved in the analysis of semi-grokking versus ungrokking, right?

**Vikrant Varma:**
Yes.

**Daniel Filan:**
So I guess the prediction here would be that maybe semi-grokking is more likely to occur for things where you're trading off weight parametrization as compared to robustness to drop-out or stuff. Does that sound right to you?

**Vikrant Varma:**
I think in general it'll be very hard to observe semi-grokking in realistic settings because you need such a finely tuned balance. You need all these ingredients. You need basically two circuits, or two pretty distinct families of circuits, with no intermediate circuits that can do the task well between them. You need a way to arrange it so that the dataset size or other hyperparameters are causing these two circuits to have very, very similar levels of efficiency.

And then also you need it to be the case that, under those hyperparameters, you're able to actually find these two families of circuits. So you'll probably find the memorizing one, but you need to be able to find a generalizing one in time. And this just seems like quite a hard thing to happen all at once, especially the fact that in realistic tasks you'll have multiple families of circuits that are able to do the training task to some degree.

**Daniel Filan:**
So semi-grokking seems unlikely. I guess it does seem like the prediction would be that you would be able to observe ungrokking for the kinds of grokking that don't depend on weight decay. Am I right that that is a prediction, and is this a thing that you've tested for yet? Or should some enterprising listener do that experiment?

**Vikrant Varma:**
So to be clear, the experiment here is "find a different notion of efficiency and then look for ungrokking under that notion"?

**Daniel Filan:**
The experiment would be "find an instance of grokking that doesn't happen from weight decay". Then it should be the case that: [you] train your data, get grokking, then train data on subsets of various sizes, and there should be a critical subset size where below that you ungrok and above that you retain the grokking solution when you fine-tune on that subset.

**Vikrant Varma:**
Yeah, I think this is basically right, and our theory does predict this. I will caveat that dataset size may not be the right variable to vary here, depending on what notion of efficiency you're using.

**Daniel Filan:**
Well, I guess in all notions of efficiency, it sounded like there was a prediction that efficiency would go down as the dataset increased for the memorizing solution, but not for the generalizing solution, right?

**Vikrant Varma:**
Yeah, that's right.

**Daniel Filan:**
As long as you believe that the process is selecting the most efficient circuit.

**Vikrant Varma:**
Yeah, that's right.

**Daniel Filan:**
Which we might worry about if there's... you mentioned SGD found different efficiency generalizing solutions, so maybe you might be worried about optimization difficulty. And in fact maybe something like parameter norm is easier to optimize against than something like drop-out robustness, which is less differentiable or something.

**Vikrant Varma:**
Yeah, I think that's right. I think you're right that in this kind of regime, dataset size is pretty reasonable. I was imagining things like model-wise grokking, where on the X-axis, instead of amount of data, you're actually varying the size of the model or the number of parameters or the capacity or whatever.

But all of these different models are actually trained on the same amount of data for the same time. And it's also less clear how exactly you would arrange to show ungrokking there because naively, you can't go to a higher-size model and then reduce the size of the model. But maybe there are ways to show that there.

## Vikrant's research approach <a name="vikrants-research-approach"></a>

**Daniel Filan:**
Gotcha. So if it's all right with you, I'd like to move on to just general questions about you and your research.

**Vikrant Varma:**
Cool.

**Daniel Filan:**
So the first question I have is: we've talked about your work on grokking, we've talked about your work on latent knowledge in large language models. We haven't talked about it, but the other paper I know you for is this one on [goal misgeneralization](https://arxiv.org/abs/2210.01790). Is there a common thing that underlies all of these that explains why you worked on all of them?

**Vikrant Varma:**
Well, one common thing between all of them is that they are all projects that are accessible as an engineer without much research experience, which was one of my main selection criteria for these projects.

So yeah, I guess my background is that I've mostly worked in software engineering, and then around 2019 I joined DeepMind and about a year later I was working on the alignment team. I did not have any research experience at that time, but I was very keen to figure out how you can apply engineering effort to make alignment projects go better.

And so certainly for the next two or three years, I was mainly in learning mode and trying to figure out how do people think about alignment? What sorts of projects are the other people in the alignment team interested in? Which ones of these look like they're going to be most accelerated by just doing good engineering fast? And so that's where a bunch of the early selection came from.

I think now I feel pretty drawn to working on maybe high risk, high reward things that might end up mattering if alignment by default (as I see the plan) doesn't go as expected. It feels like the kind of thing that is potentially more neglected. And maybe if you think that you need a bunch of serial research time to do that now before you get very clear signals that, I don't know, we haven't done enough research on some particular kind of failure mode, then that feels important to do now.

**Daniel Filan:**
Okay. So should I be thinking: lines of research where both, they're approachable from a relatively more engineering-heavy background, and also laying the foundation for work that might come later rather than just attempting to quickly solve a problem?

**Vikrant Varma:**
Yeah, that's right. That's certainly what I feel more drawn to. And so for example, I feel pretty drawn to the [eliciting latent knowledge problem](https://docs.google.com/document/d/1WwsnJQstPq91_Yh-Ch2XRL8H_EpsnjrC1dwZXR37PC8/edit). I think there is both interesting empirical work to do right now in terms of figuring out how easy is it to actually extract truth-like things from models as we've been discussing, but also framing the problem in terms of thinking about methods that will scale to superintelligent systems[, this] feels like the kind of thing that you just need to do a bunch of work in advance. And by the time you're hitting those sorts of problems, it's probably quite a bad situation to be in.

**Daniel Filan:**
Gotcha. So what should I think of as your role in these projects?

**Vikrant Varma:**
I think it varies. I would describe it as a mix of coming up with good research ideas to try, trying to learn from people who have been around in the alignment community much longer than me, and also trying to supply engineering expertise

So for example, currently I'm working on [sparse autoencoders](https://arxiv.org/abs/2309.08600) for mechanistic interpretability, and I am very new to mechanistic interpretability. However, all of the people I work with (or many of the people I work with) have been around in mech. interp. for a long time. And it's great for me to understand and try to get knowledge directly from the source in a way.

I think at the same time, with sparse autoencoders in particular, that's the kind of project where... Partly what drew me to it was [Chris Olah's tweet](https://twitter.com/ch402/status/1709998674087227859) where he said... I'm not sure exactly what he said, but it was something like "mech. interp. might be in a different mode now where if SAEs [Sparse AutoEncoders] work out, then it's mostly an engineering problem, it's not so much a scientific problem". And that kind of thing feels very exciting to me, if we're actually able to scale up to frontier models.

**Daniel Filan:**
It could be. I do find myself thinking that there's still a lot of science left to do on SAEs, as far as I can tell.

**Vikrant Varma:**
Yeah, I don't disagree with that.

**Daniel Filan:**
Perhaps I should say for the listener, a sparse autoencoder - the idea is that you want to understand what a network is thinking about. So you train a function from an intermediate layer of the neural network to a very large vector space, way more dimensions than the underlying thing, and then back to the activation space, and you want to train this to be the identity function, but you want to train it so that the intermediate neurons of this function that you've learned very rarely fire. And the hope is that you're picking up these underlying axes of variation, and hopefully only a few of them are happening at a time, and hopefully they correspond to concepts that are interpretable, and that the network uses, and that are underlying facts about the network and not just facts about the dataset that you happen to train the autoencoder on.

And all three of those conditions seem like they need more work to be established. I don't know, I'm not super up to date on the SAE literature, so maybe somebody's already done this, but I don't know, that's a tangent from me.

**Vikrant Varma:**
I definitely agree. I think there's a ton of scientific work to do with SAEs. It just also happens to be the case that there's... It feels like there's a more direct path or something to scaling up SAEs and getting some sort of mech. interp. working on frontier models that, at least in my view, was absent with previous mech. interp. techniques, where it was more...

**Daniel Filan:**
Human intensive, I guess?

**Vikrant Varma:**
Yeah, more human intensive and a much less direct path to doing the same kind of in-depth circuit analysis on larger models.

## The DeepMind alignment team <a name="deepmind-alignment-team"></a>

**Daniel Filan:**
I'd next like to ask about the alignment team at DeepMind. So obviously I guess you've been there for a few years.

**Vikrant Varma:**
Yeah.

**Daniel Filan:**
What's it like?

**Vikrant Varma:**
It is honestly the best place I've worked. I find the environment very stimulating, there's a lot of freedom to express your opinions or propose research directions, critique and try to learn from each other. I can give you an overview of some of the projects that we're currently working on, if that helps.

**Daniel Filan:**
Yeah. That sounds interesting.

**Vikrant Varma:**
So I think the team is roughly evenly split between doing [dangerous capability evaluations](https://arxiv.org/abs/2403.13793), doing mechanistic interpretability, rater assistance, and various other emerging projects like debate or process supervision. So that's roughly the split right now.

I think apart from that, to me it feels like an inflection point right now because safety is getting a lot of attention within DeepMind, I think. So [Anca Dragan](http://people.eecs.berkeley.edu/~anca/) recently joined us, she is a professor at Berkeley. To me it feels like she has a lot of buy-in from leadership for actually pushing safety forward in a way that feels new and exciting. So as one example of this, we're spinning up an alignment team in the Bay Area. Hopefully we'll have a lot more resources to do ambitious alignment projects in the future.

**Daniel Filan:**
Sure. Is that recruiting new people from the Bay Area or will some people be moving from London to seed that team?

**Vikrant Varma:**
That's going to be mostly recruiting new people.

## Follow-up work <a name="follow-up-work"></a>

**Daniel Filan:**
Gotcha. The second to last thing I'd like to ask is: we've talked about this grokking work and this work on checking this CCS proposal. Do you have thoughts on follow-up work on those projects that you'd really like to see?

**Vikrant Varma:**
Yeah, definitely. So I think with the grokking paper, we've been talking about a bunch of potential follow-up work there. I think in particular, exploring other notions of efficiency seems really interesting to me. I think the theory itself still can produce quite a lot of interesting predictions. And from time to time I keep thinking of new predictions that I would like to try that that just don't fit into my current work plans and stuff.

So an example of a prediction that we haven't written down in the paper but that occurred to me a few days ago, is that: our theory is predicting that even at large dataset sizes where you're seeing grokking, if the exponent with which you convert parameter norms into efficiency is small enough, then you should expect to see a non-zero amount of memorization even at large dataset sizes. So the prediction there is that there should be a train-test gap that is small but not zero. And this is in fact true. And so the thing you should be able to do with this is use the empirical estimates of memorization and generalization efficiency to predict train-test gap at any dataset size.

So that's one example. I think this theory is pretty fruitful and doing work like this is pretty fruitful. I would love to see more of that. On the CCS side, a thing I would love to see is test beds for ELK methods. So what I mean by that is examples of networks that are doing something deceptive, or that otherwise have some latent knowledge that you know is in there but is not represented in the outputs. And then you're really trying your hardest to get that latent knowledge out, using all sorts of methods like linear probing or black box testing, maybe anomaly detection. But I think without really good test beds, it's hard to know. It's easy to fool yourself about the efficacy of your proposed ELK method. And I think this is maybe quite related to the [model organisms agenda](https://www.lesswrong.com/posts/ChDH335ckdvpxXaXX/model-organisms-of-misalignment-the-case-for-a-new-pillar-of-1) as well.

**Daniel Filan:**
Well, I think that wraps up about what we wanted to talk about. Thanks very much for being on the show.

**Vikrant Varma:**
Yeah, thanks for having me.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Filming occurred at [FAR Labs](https://far.ai/labs/). Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future) and [Lightspeed Grants](https://lightspeedgrants.org/), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev and Tor Barstad. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
