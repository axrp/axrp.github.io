---
layout: post
title: "35 - Peter Hase on LLM Beliefs and Easy-to-Hard Generalization"
date: 2024-08-24 15:20 -0700
categories: episode
---

[YouTube link](https://youtu.be/tEge5uo4E-A)

How do we figure out what large language models believe? In fact, do they even have beliefs? Do those beliefs have locations, and if so, can we edit those locations to change the beliefs? Also, how are we going to get AI to perform tasks so hard that we can't figure out if they succeeded at them? In this episode, I chat with Peter Hase about his research into these questions.

Topics we discuss:
  - [NLP and interpretability](#nlp-and-interpretability)
  - [Interpretability lessons](#interpretability-lessons)
  - [Belief interpretability](#belief-interpretability)
  - [Localizing and editing models' beliefs](#localizing-editing-model-beliefs)
  - [Beliefs beyond language models](#beliefs-beyond-lms)
  - [Easy-to-hard generalization](#easy-to-hard-generalization)
  - [What do easy-to-hard results tell us?](#easy-to-hard-results)
  - [Easy-to-hard vs weak-to-strong](#easy-to-hard-vs-weak-to-strong)
  - [Different notions of hardness](#different-notions-of-hardness)
  - [Easy-to-hard vs weak-to-strong, round 2](#easy-to-hard-vs-weak-to-strong-2)
  - [Following Peter's work](#following-peters-work)

**Daniel Filan** (00:00:08):
Hello, everybody. This episode, I'll be speaking with Peter Hase. Peter is an AI researcher who just finished his PhD at UNC Chapel Hill, where he specialized in natural language processing and interpretability research with a special interest in applications to AI safety. For links to what we're discussing, you can check the description of the episode, and a transcript is available at AXRP.net. All right. Peter, welcome to AXRP.

**Peter Hase** (00:00:33):
Thanks so much, Daniel. I'm excited to be here.

## NLP and interpretability <a name="nlp-and-interpretability"></a>

**Daniel Filan** (00:00:35):
I'm excited to have you on. So my understanding is that most of your work is in interpretability, roughly interpretability in language models. Is that fair to say?

**Peter Hase** (00:00:46):
Yeah. That's right. I've been in an NLP [natural language processing] lab for my PhD, so we work mostly with language models, but a lot of it, in terms of methods, evals, has been focused on interpretability.

**Daniel Filan** (00:00:55):
Actually, maybe one thing I want to ask is: I have the impression that you were into language models even before they were cool, doing NLP before it was cool. Right? So just today, I looked at your Google Scholar and scrolled down to see the oldest paper you were a co-author on. It's [this 2018 paper on algorithmic sonnet generation](https://arxiv.org/abs/1811.05067). 2018, before the rest of us had caught up. What got you interested in NLP?

**Peter Hase** (00:01:23):
I remember the project you were talking about. That was when I was an undergrad. I feel so lucky to have had the opportunity to do a little special projects class, actually with [Cynthia Rudin](https://users.cs.duke.edu/~cynthia/) at Duke, in my undergrad. I mean, I was interested in language. I was interested in psychology and linguistics in my undergrad, and very interested in language, and increasingly interested in machine learning and statistics. And so it was just a great intersection of the topics and learning about language models - which, at the time, were really LSTMs - and getting an opportunity to apply those to a fun task. Even then, of course, I admitted it wasn't necessarily that consequential (poetry generation) but it was certainly a great opportunity to get to work with language models a bit.

**Daniel Filan** (00:02:13):
Like you mentioned, the bulk of your work has been in interpretability. What got you interested in that aspect?

**Peter Hase** (00:02:22):
That was also an interest that developed in undergrad. So I think a long time ago, there were many different arguments put forth for why interpretability was a good thing to study. At the time, there was definitely a very intuitive draw, and there still is an intuitive draw, that it's just good to know how these things work. It's just good to know how AI systems work, how language models work.

(00:02:47):
They were doing increasingly interesting things. At the time, there was so much progress in vision. This was 2018, so there'd been a lot of progress in RL. Language models - I think by 2018, GPT-1 was out, and I think GPT-2 was coming out in spring 2018 or '19. It was just very clear that these systems were making a lot of progress and doing fundamentally interesting things. From a safety perspective, it's like, "Gosh, we should know how they work. We should be able to explain their decision-making process."

**Daniel Filan** (00:03:25):
This is kind of a broad question, but how would you say interpretability is doing as a subfield?

**Peter Hase** (00:03:31):
Well, this is a great question. I tend to be, I think, optimistic when talking with people from other subfields or who are working on capabilities research or other research areas. I probably come off as a little bit pessimistic when I'm talking with my colleagues about this. Let me be clear: there's definitely a lot of progress being made, where we have better evals, we have a better understanding of when do we have a ground truth? When are we speculating about the reasoning process? What would it mean for an interpretation or an explainability method to be useful, and what could it be useful for downstream? This picture has just become a lot clearer in the past five to six years.

(00:04:17):
One of the reasons I'm pessimistic, at least when it comes to colleagues, is we just end up talking about all the false positives, and false starts, and like, "Oh, the reason this result didn't hold up was because of this". I think some of this is decently high-profile. People might know about things like feature attribution or saliency maps. So this was a popular [method] and one of the first major methods for trying to get a sense of what neural networks were doing, and you could think of this as being a 2015 to 2016 method, which was to say, "Okay. If you have a vision model, what's it looking at? Is it looking at the dog in the image? Is it looking at the background? Is it looking at the human in the image?"

(00:05:08):
People were really excited, because this was one of the first ways to generate... I mean, the images just looked good. The visualizations looked good, and you could say, "Wow. It really seems like the neural network thinks this is a husky because it's looking at the snow in the background, and not because it's looking at the dog, per se." So people were really excited about these methods, and then if you've worked in the subfield for a while, you know how these methods have had a bit of a fall from grace. They didn't turn out to be useful in human studies for the most part. There's been theoretical work showing that some of these popular feature attribution and saliency methods can do no better than random in certain settings. There've been a lot of hard-learned lessons in the subfield in terms of what to trust and what's promising to run with in the long term.

**Daniel Filan** (00:06:01):
I'm wondering if you have an agenda of questions you think that it's important for the interpretability field to answer, and if so, what are they? Where should we be looking? What should we be aiming for here?

**Peter Hase** (00:06:14):
I think we're still in the stage of figuring out what methods are good and what evals tell us when we've created something useful. I don't think we're yet at the stage where we have the tools, and we're mainly interested in detecting bad reasoning processes or detecting deception in language models. We're not yet at the stage where we're trying to catch safety failures. We're still at the stage where we're trying to build tools that would let us catch safety failures, and we're trying to build evaluations for the tools so we know which tools would work at that. So it's still pretty upstream. I'll stop there and actually ask you to maybe elaborate on the question a bit so I can keep going.

**Daniel Filan** (00:07:08):
Yeah. I guess it seems like a view of more basic science. The reasons to do interpretability is we know that something about understanding model behavior, understanding model internals, why things are happening, something around that is going to be useful. And if we did some more basic science, we would just have a better sense of what the important questions to ask are. Is that roughly a good gloss of your view, or am I missing something?

**Peter Hase** (00:07:40):
Okay, thanks. So the research stage we're at, I think, is still basic science. So I gave the kind of intuitive motivation before for interpretability and why I think that's an exciting area. It's: we want to know how these things work. It's: they're so fascinating, they do such interesting things, we want to know how they work. This is the intuitive pitch. I think that the strongest pitch that has emerged over time for interpretability research is that we need something that goes a little bit beyond testing. So all models get tested on all kinds of datasets, benchmarks, evals. We're looking for dangerous behaviors. We're looking for dangerous capabilities. We want to know what kinds of reasoning and knowledge that models possess.

(00:08:25):
So really, I think the best pitch for interpretability is, what can our tests not catch? So one thing that our tests can't catch a lot of the time is the underlying reasoning process. So if we just have a huge multiple choice exam that is going to tell us if the models have dangerous bioweapons development capabilities or the models have really strong theory of mind such that they could operate in a social setting and either be cooperative or intentionally deceptive... if we just have surface-level, "Let's prompt the model and see what text it outputs," kind of tests for that thing, we can't exhaustively test every relevant scenario.

(00:09:13):
There are settings where we'd be interested in deploying the model when it's interacting with people. It might be more or less knowledgeable or aware that it's interacting with people, and there's going to be settings where we can't actually test the thing we're interested in or we can't exhaustively test the model. That's especially the setting where we want to open up the hood and figure out what's going on inside, and be able to say, "Okay. Yes. It did this multiple-choice problem correctly, and it had a really impressive strong reasoning process for how it got there. We're pretty sure it's actually going to generalize beyond just the things we're testing."

(00:09:52):
Or we haven't deployed it in a setting where it is cooperating with people in the real world yet, but we've leveraged some interpretability method to say, "Yes. This model fully intends on cooperating with people. Even if it knew that it could slightly better optimize one of its incentives at the expense of harming a human, we know it wouldn't do that, because we've been able to truly inspect its reasoning process underneath."

## Interpretability lessons <a name="interpretability-lessons"></a>

**Daniel Filan** (00:10:19):
So we've been doing this interpretability work for a while, right? What things do you think we've learned out of interpretability research?

**Peter Hase** (00:10:30):
So I think one big thing we've learned is that we really want the methods to be useful for some downstream purpose. So that means when we have an interpretability tool, like we're inspecting what features the model relies on, we want that to enable us to do some debugging. We want that to enable us to catch failures that we might not have been aware of before. We might want that to make the decision-making process more clear to an end user so they can decide if it was reasonable or not. This comes up in, for instance, this problem called algorithmic recourse, where a person is being classified by a model, and they want to understand why they got the decision they did. So a lot of it's increasingly gearing our evals towards these downstream use cases so that we can make sure that we're actually getting good signal on the methods we're developing.

(00:11:29):
So that's one broad lesson, I think, and I could say a little bit more about some of the more upstream evals that still seem valuable to me. So that's figuring out methods that actually improve safety of models in some tangible way, basically, and going back to what I said before, especially in a way that's complementary to testing or complementary to other kinds of evals.

(00:11:57):
I think one of the other lessons is - this will be more object-level - we're learning that language models can generate really plausible-sounding textual explanations of their decision making that aren't actually how they're reasoning. So this is just an immediate object-level lesson about how language models work. Their ability to chat with people is really impressive. Their ability to offer justifications for their answers is really impressive, and we're starting to catch them out in inconsistencies via some cleverly-designed tests that show that what the model says is not really what it was thinking a lot of the time.T hat's, I think, a very important insight in terms of interacting with the models in text. I'd say that's more in the natural language explanations category in terms of what research stream that is.

(00:12:51):
Then, there's this area of mechanistic interpretability and other kinds of probing research, historically in NLP, that I'd say is a setting where we're really gaining traction on figuring out how models represent things. A lot of the work in 2015 and 2016 was focused on looking at the input and [working out] "What part of the input is this model looking at?" For vision models, you'd get heat maps that would light up over a part of an image that the model might be looking at. In a text setting, you'd get text highlights. So you'd say, "Oh. It's these words, and we're going to highlight them. That shows you what the model's looking at."

(00:13:32):
I think we're really starting to go deeper than that. We're really starting to be able to say, "Okay. Here are the hidden activations in the model," and there's been one development I'll point out. It might've been that we used to say, "Okay. Here are the neurons, and here are the neurons that represent this or the neurons that represent that." There's been some really interesting mathematical progress, I think, on showing it's not just individual neurons, but it's particular combinations of neurons that might represent a certain feature. So you turn this cluster of neurons on, and that means the model has definitely detected that this text is discussing soccer as a sport. Or you have this other cluster of activations or neurons that have been turned on, and it's like, okay, now, it's discussing soccer as a political phenomenon or governing bodies of soccer.

(00:14:19):
Very abstract features of model inputs... We're starting to connect the dots between those abstract features and model internals, and how the models are actually representing them inside, and then, after that, how the models are using those representations. So we might know that, okay, the model has detected something, and now, how is it going to influence the decision? People are developing tools for saying, "Okay. Yes. This feature's been detected, and it plays an important role in the model's answer."

**Daniel Filan** (00:14:53):
So your first two points of things we learned - it's important to get some sort of downstream benefit from your interpretability method or peg it to, "Does it actually help you do such and such task?", and large language models are really good at faking explanations of how they're thinking. These sound to me like kind of negative results, right? Like, "you might've thought this thing was true, but it's not true." Right? You might've thought that, just because you have a plausible story for why this integrated gradient method tells you something about the model, [but] you're just wrong, and actually, you should just test it against "does it actually help you do something?" [Or] you might've thought that if a thing can talk, it's going to say something reasonable; that's not true. Does that seem like a fair characterization to you?

**Peter Hase** (00:15:43):
Yeah, to be clear, those basically were negative results. I mean, we were realizing that some of our evals weren't really demonstrating external usefulness or downstream usefulness, and the natural language stuff... Some people, I think not in the explainability world, when they saw things like these dialogue models get developed, or chain of thought get developed, or RLHF models get developed, and they saw models explaining reasoning in words to people... I mean, I certainly saw public perception from NLP people, experts in the field, basically say[ing], "Wow. We just almost solved explainability, right?" It took some additional studies to say, "Okay, no, this is a result we've seen before. We have a new explanation method, and it still doesn't quite tell us what's going on inside the model."

**Daniel Filan** (00:16:35):
So if I'm trying to think about what we learned there, it seems like the underlying theme is: you might think that neural networks are sort of neat and tidy, such that there's a place where a thing is happening, and you find the place, and you understand the thing; and it's just not true. Somehow, the story of interpretability is falsifying naive models of how neural networks work. The way we falsify them is: we get a thing that seems like it should work, and it turns out to not be helpful. Somehow, the point of it is to just help us realize how alien language models are.

**Peter Hase** (00:17:19):
Yeah. I think that's a good way to put it, and I think this is one reason people are starting to notice a need for more ground truth evals and being able to say, "Okay. Here's what we know that the model's doing, because we specifically designed a neural network to reason in a certain way, or to be vulnerable to certain adversarial examples, or to rely too strongly on certain input." Sometimes, people do that with language models; sometimes, people do it with just very toy neural networks that learn a specific function, and the goal is simply to figure out what that function is.

(00:17:55):
So this is a setting where to avoid all of the difficulties of an interpretation maybe being right, or maybe being wrong, or maybe being halfway right and halfway wrong, and then trying to figure out what we could possibly use this thing for, this is going a little bit further upstream and saying, "Let's just design a system that looks kind of like a black box, but we secretly know exactly what it's doing," and then figure out if our methods can reliably detect the behavior going on. People are definitely waking up and becoming a little bit more alert to this kind of research angle.

(00:18:31):
There's some interesting broader commentary on this kind of thing. So [Chris Olah](https://colah.github.io/) has [this nice figure](https://www.lesswrong.com/posts/X2i9dQQK3gETCyqh2/chris-olah-s-views-on-agi-safety#What_if_interpretability_breaks_down_as_AI_gets_more_powerful_) in some blog post that's like "the uncanny valley of abstractions" or this valley of abstractions with neural networks, where it might be that... Neural networks start out - in terms of their capabilities, if you're thinking of a small network trained on a small amount of data - basically doing a bunch of hacky stuff and using a bunch of hacky heuristics to solve a problem. But as the models get better, and particularly as they solve harder and harder problems, you begin to think, "Well, the reasoning process, plausibly, is going to look a little bit more human."

(00:19:12):
Because we might think, "Well, basically, the way you do these math word problems or the way you do this college biology exam is just going to require more human-like reasoning and to rely on some more human-like concepts." So there's been this idea that interpretability will actually get easier over time as the language models or as vision models develop a more... You can think of this being like, the model's vocabulary is more easily translatable into human vocabulary or a human language.

**Daniel Filan** (00:19:44):
Yeah. I guess another thing I wanted to pick up on is when you were talking about advances in understanding the representations of neural networks, you mentioned that we now know that things are represented as combinations of neurons, and there was some math research backing that up. Can you say what you were referring to?

**Peter Hase** (00:20:11):
So something that really put this on the map in more of the public landscape was Anthropic's superposition work, their [toy models of superposition](https://transformer-circuits.pub/2022/toy_model/index.html), where they were able to show that in a given representation space... so if the dimensionality representation space was 784 - which is equal to the number of neurons - so if you had 784 neurons, you could have a model that could actually represent more features than neurons. Immediately, this implies that it's not just a one-to-one map, because it's not just now that one neuron means one feature. Mathematically, what that ends up looking like is that features are now directions in the latent space, and they're not all orthogonal.

(00:20:58):
So previously, if one neuron was one feature, that's also a direction in the latent space. That's a basis-aligned direction. It's just right along one axis. So features have always been directions, and we've clued in a little bit more to how features are now not basis-aligned directions, but they can point in some kind of seemingly arbitrary direction in the latent space. It happens that if you have 1,000 features and a 784-dimensional space, these 1,000 features, you can imagine them kind of slightly pushing apart. So they're all just the right distance from one another, they're all pointing in some direction, but they're minimizing potential interference between them.

(00:21:43):
So I'll point out that work as something that I think did a good job visualizing this, a good job demonstrating it in toy settings. I would go all the way back to probably 2017 with [TCAV from Google](https://arxiv.org/abs/1711.11279), and this was some work [Been Kim](https://beenkim.github.io/) led at Google that showed that there could be feature vectors in the latent space. They showed this not really in an unsupervised way, which is basically the way Anthropic showed it, but they showed this in a supervised way.

(00:22:15):
So if you have a dataset... Let's say you're looking for how a vision model represents stripes. So what you do is you have a bunch of images with stripes, and you have a bunch of images without stripes. You feed all those through the model, and then you learn a classifier on the model's latent space that can classify representations as stripes or not stripes. With a feature like that and strong enough models, you often see that there's a direction in the latent space that basically measures how stripy something is. It was never axis-aligned or basis-aligned to begin with. It was always a direction.

**Daniel Filan** (00:22:54):
So this actually gets to a methodological question about interpretability. So I remember looking at this TCAV paper... So TCAV, it's "something concept aligned vector."

**Peter Hase** (00:23:09):
Oh. I wouldn't even remember the acronym.

**Daniel Filan** (00:23:10):
It's something like that.

**Peter Hase** (00:23:11):
Yeah. It's concept activation vectors with a T.

**Daniel Filan** (00:23:13):
Activation. Right. Something concept activation vectors or something like that. Please forgive us, listeners and Been Kim.

(00:23:21):
But I remember one concern I had about this paper is that it was trying to understand how concepts were represented in networks, but by "concepts", it kind of meant "a thing a human thought of". Right? We think that there should be some concept of stripiness. So we have this dataset of stripy versus non-stripy things, and we see where that is in the network. At the time, there was this thought of, "Well, there's some danger in imposing our concepts onto neural networks or assuming that neural networks are going to use our concepts." Right?

(00:23:55):
You were a co-author on this paper, [Foundational Challenges in Assuring Alignment and Safety of Large Language Models](https://arxiv.org/abs/2404.09932). Lead author was [Usman Anwar](https://uzman-anwar.github.io/) and then a bunch of co-authors. You wrote this section about difficulties in interpretability, and I think one of the things you mentioned was models might not use human-like concepts. We've kind of learned this, but at the same time, it seems like this TCAV work really did teach us something about how concepts really were represented in neural networks for real.

(00:24:24):
So on the one hand I want to say, "Hey. We shouldn't impose our concepts onto neural networks, and we shouldn't assume that they're thinking of things the same way we're thinking about it." On the other hand, this work that did make that assumption turned out to tell us something that it took the rest of us five years to work out. Right? So how should we think about imposing our concepts on networks?

**Peter Hase** (00:24:45):
Yeah, that's a good point, and I think this line of research has taught us something durable about how language models or vision models represent things. In that longer agenda paper, the foundational challenges paper, we definitely criticize this line of research as much as we can manage. These kinds of methods... So you can think of this as supervised probing and unsupervised probing. The ["sparse autoencoders"](https://transformer-circuits.pub/2023/monosemantic-features) direction that Anthropic, [OpenAI](https://arxiv.org/abs/2406.04093), [Apollo](https://arxiv.org/abs/2405.12241), and [others](https://arxiv.org/abs/2309.08600) have been pushing, has been uncovering the same kinds of feature vectors in hidden spaces, but just in an unsupervised way. But then, you need to figure out what they mean.

(00:25:35):
So you don't start with this idea that stripiness is represented, but you first just find that, okay, there's a vector number 1,011. It's a pretty important vector. It seems to play a role in many different kinds of animal classification problems. And so one of the ways people have been interpreting these kinds of vectors is to say, "Okay. Let's look at max activating examples." So we comb through our train data and figure out what kinds of examples activate this vector strongly. Let's get some negative examples, too. We'll comb through the training data just to make sure that, okay, if there's an example that doesn't activate this vector, it doesn't really have anything to... It could just be some other random thing. Hopefully, the max activating examples all have some clear thing in common, and the non-max activating examples definitely represent other things and not the thing that the first set had in common.

(00:26:28):
So what's the issue with all these approaches? It's an art. It's hardly a science. I mean, you're really doing this interpretative act: "Okay. What do these examples have in common, and how would we verify that more strongly?" It might be that you have something in mind already - that's in the supervised case. And the unsupervised case... text data is really, really high dimensional. It might be that we have five or ten activating examples that are positive examples and five to ten negative examples, and we're going to try to... So we basically have 10 data points, and we're going to try to make a claim about what one factor ties them all together or what two factors tie them all together.

(00:27:19):
This is just a difficult process to get right. Lots of confirmation bias, lots of dataset sensitivity to this kind of thing. Basically, saying it's an art and not science goes into a little bit of how we just risk finding things that we're aware of, seeing patterns in the data that make sense to us and not patterns in the data that are actually used by the model but maybe alien to us. I'll go into the data sensitivity thing a little bit, which is: there's been some criticism of the TCAV stuff that actually, if you use a different dataset of stripy and unstripy images, you might get a different vector. So it seems like some of these methods are quite sensitive to the datasets we're using.

(00:28:09):
You get similar kinds of data critiques with the unsupervised vector discovery, as well. So if you really wanted to know, "What were all the features that my model is using, and what could this feature vector possibly represent?" So when I say you go through the data and figure out what are max activating examples, that literally means you run the model over a bunch of data points and figure out, what are the activations for this feature vector? If you wanted to do this exhaustively, it actually means going through the pre-training data. It means that you would need to do a forward pass over the entire pre-training dataset to be able to say... And this is still just correlational! We haven't even got to a causal analysis yet.

(00:28:57):
But even doing a correlational analysis means you've run through the entire pre-train dataset and looked for max activating examples. This is prohibitively expensive. So now, we have this issue where this feature's going to represent something, but figuring out what it represents is now this huge task, both in terms of getting the human annotation process correct and in terms of using the right data to begin with.

**Daniel Filan** (00:29:25):
So it seems like the thing going on here is there's this sort of spectrum of methods, where on the one end, you have things like the sparse autoencoders work, which is trying to be relatively neutral about what's going on with the model. It's still making some assumptions that this dataset is representative and such, but it's trying to not impose a bunch of structure. On the other hand, if you think about TCAV-style work, it's kind of assuming "the model's going to have a stripy concept: the only question is, where is it?" Right?

(00:30:06):
I see this tension a lot in interpretability, where on the one hand, you don't want to add in a bunch of assumptions about how your thing is going to work. But on the other hand, if you don't add in a bunch of assumptions, how are you validating your thing? You have some method. It has very few assumptions. How do you tell if it worked? Do you just look at it and see, "Do I like what I see?" How do you think it makes sense to manage this trade-off?

**Peter Hase** (00:30:37):
That's a good question, especially because there's so much that's qualitatively different around... If you're talking about feature discovery, it's probing methods, if it's supervised versus unsupervised, that changes a lot about what kinds of data you need. It changes a lot about how computationally expensive these methods are. So how can we compare them?

(00:30:56):
Well, one answer is: let's figure out what they could help us do, and then just figure out what is best at that. So maybe these methods help us do model editing. Maybe it helps us say, "Okay. Here's a feature that is important to the model, and it's making some errors on certain data points. So I want to edit how much this model relies on that feature. Maybe I need to turn up its reliance, maybe I need to turn down its reliance on that feature." Maybe there'd be an important feature that's missing from the model, and either there's some incredible mechanistic intervention on the model that equips the model with the ability to represent that feature, or I just need to go back, and put some data into the training dataset, and retrain the model so it represents that feature properly.

(00:31:38):
Let's compare all these methods in terms of usefulness for this thing that we care about. And I can unpack the model editing a little bit. One thing I mean there is just basically making fine-grained adjustments to model behavior. So you've already trained a classifier that maybe handles a thousand classes, or you have this language model that can do any kind of text-to-text task. But these things are expensive to train, and they might just make small mistakes. You just want to be able to fix the small mistakes, and diagnose what's going wrong mechanistically, and then fix that mistake. That would be the model editing application that a lot of this mechanistic interpretability kind of work could be useful for, I think.

## Belief interpretability <a name="belief-interpretability"></a>

**Daniel Filan** (00:32:22):
Right. I'd like to go in a little bit of a more concrete direction. Specifically, at least a few papers... Maybe I'm reading into it too much to think of it as a line of work, but I see you as having some kind of line of work on beliefs of large language models. So if I look back, you have this paper, [Do Language Models Have Beliefs? Methods for Detecting, Updating, and Visualizing Model Beliefs](https://arxiv.org/abs/2111.13654) by yourself and some co-authors in 2021. [Does Localization Inform Editing? Surprising Differences in Causality-Based Localization vs. Knowledge Editing in Language Models](https://arxiv.org/abs/2301.04213) by yourself, [Mohit Bansal](https://www.cs.unc.edu/~mbansal/), [Been Kim](https://beenkim.github.io/), and [Asma Ghandeharioun](https://asmadotgh.github.io/) in 2023, and also [Are Language Models Rational? The Case of Coherence Norms and Belief Revision](https://arxiv.org/abs/2406.03442) by [Thomas Hofweber](https://www.thomashofweber.com/), yourself, [Elias Stengel-Eskin](https://esteng.github.io/), and Mohit Bansal this year. What got you interested in beliefs as a thing to look at, rather than representations or all sorts of other things people do interpretability for?

**Peter Hase** (00:33:23):
I think it's totally fair to call this a line of work. This has been an interest of mine for a while. So I think things might seem more coherent if I go backwards in terms of the papers, but the real narrative story is coming from the beginning. In 2021, we knew that models might represent features, and I think people forget how much perception of neural networks has changed over time. So a lot of people, especially academics in 2019 and 2020, [were thinking] these things are classifiers. They learn features, and they classify features. They learn features, and they draw a hyperplane in the latent space and divide positives and negatives. That's what these things are doing. So we want to figure out how important features are to those hyperplanes. That's the recipe for a lot of people back then.

(00:34:22):
Then, it became increasingly clear that language models store a lot of information about the world, and then it became increasingly clear with, basically, ChatGPT, RLHF models that language models could converse reasonably about this information in the world. The picture got a lot richer, and it started to seem more and more that these neural networks were doing something just a little bit more interesting than storing raw data, learning patterns in data. It seems like they might actually be representing things about the world.

(00:35:03):
And, particularly with models that are fine-tuned to be truthful or fine-tuned to be helpful and can converse fluently with people about the thing, about questions in the world... A lot of people were just really tempted to speak of these systems in totally anthropomorphic terms. I don't think this is always a mistake. It's just really natural a lot of the times to say, "Oh, the model gave me this answer. It knew this thing, but it actually made a little mistake over here. It didn't quite know what it was talking about in this case." And, speaking about language models having knowledge about the world really presupposes that language models are representing things in the world and that language models have beliefs about the world.

(00:35:55):
Okay, that's a bit about why beliefs emerged as a potentially interesting thing, as opposed to simply features that are used in classifiers.

(00:36:04):
So, what is the fascination with beliefs and why is it so natural for people to speak of models having beliefs or knowledge? Well, I think this has to do a lot with how people explain behavior of agents. And so this is something that we are really interested in in [the last paper you mentioned](https://arxiv.org/abs/2406.03442), which is about if language models are rational. And the philosopher [Daniel Dennett](https://en.wikipedia.org/wiki/Daniel_Dennett) did a lot of work, elaborating this ["intentional stance" theory](https://en.wikipedia.org/wiki/Intentional_stance), which is kind of a folk psychology for how people work. And it's that people explain behavior in terms of an agent's beliefs and desires. But, I think we see this spat out again and again between scientific work and everyday situations. When you're thinking about [theory of mind tasks](https://en.wikipedia.org/wiki/Sally%E2%80%93Anne_test) and asking someone, "Okay, why did Sally look for her..." I forget what she usually stores in the basket.

**Daniel Filan** (00:37:09):
Let's say it's eggs.

**Peter Hase** (00:37:10):
Yeah, they have an egg in a basket, versus an egg in a bucket. And, if you're out of the room and things have been moved from one container to the other, and then they return to the room, where will they look? This is just classic beliefs and desires. We believe that someone has a desire to find an object that they own, or that they're looking for, and we recognize basically via theory of mind that they have a belief about the state of the world. And these two things combine to produce behavior. And this is just a great way to explain lots of stuff.

(00:37:42):
And, Daniel Dennett elaborates what is really a minority view in philosophy, to my understanding, that beliefs are just informational states. So it's a very stripped-down view of what a belief is. And it's basically totally okay to ascribe beliefs to things like animals and robots, as long as it does a good job explaining their behavior basically, as long as it seems appropriate. Clearly, animals have information about the world. Clearly, robots store information about the world. And it seems like if the equation "behavior equals beliefs plus desires" is a good recipe for explaining behavior, Daniel Dennett basically says, "Go for it. Use all the terminology you want to explain how these things are working."

**Daniel Filan** (00:38:28):
So, can you tell us a little bit about what you've done in trying to understand beliefs in language models?

**Peter Hase** (00:38:34):
Yeah, so this is work that was really led by a philosopher at UNC, Thomas Hofweber. I love reading how philosophers write. It's so methodical and so clear. It's like: okay, what would it mean for language models to have beliefs? We're going to break it up into three questions. One, do they have the kinds of representations that could be beliefs or the kinds of representations that are aimed at truth? And then, when we're thinking about belief and rationality, number two, if language models have these kinds of representations that are aimed at truth, what would it mean for norms of rationality to apply to those representations?

(00:39:19):
So, it's number one, do they have the representations? Number two, do we expect norms of rationality, norms of truthfulness to apply to the representations? And then, number three, how well do language models live up to those norms? And the paper basically explores each of these three questions one at a time. And some of the core arguments, I think, are pretty simple. I mean, when we're thinking about models having beliefs, beliefs are supposed to be true. So this is in contrast to Dennett. We're not just talking about an information store, we're talking about an information store that exists for the purpose of truly representing something.

(00:40:04):
So, there's this really fun example in the paper that's like, okay, so we know about the [Chinese room and dictionaries](https://en.wikipedia.org/wiki/Chinese_room). You could say, "Okay, you have a language model, but what if it's just some huge symbol shuffling machine and it doesn't really know what it's talking about. Just whenever you ask it a question, it just does some really complicated lookup procedure. It doesn't really know what it's talking about."

(00:40:27):
And, you can ask the same thing of, "Well, a dictionary stores a lot of information, it might store a lot of information about the city of Paris or something, but it doesn't mean it knows about Paris. It's a dictionary. We put the information in it." And there's this really fun example in the paper that's like, "Yeah, clearly, just having information about something is not enough. If a wolf walks through the snow in its environment and the snow has tracks in it, the snow carries information about the wolf, and a human could read that an animal had gone through the snow. That doesn't mean the snow knows anything, it's just carrying information." So what is the clear requirement beyond just carrying information? It's aiming at truth.

**Daniel Filan** (00:41:14):
There it seems like there are two things we could say, right? One is there's some sort of criterion of correctness: is it natural to say that the patterns of snow are aiming at truth or something? This is the route that's taken in the paper you mentioned. If I'm thinking of Daniel Dennett, expected utility theory-style accounts of belief, there it seems like the distinction is: in some sense the snow has a representation of whether a wolf walked through, but it's not using that for anything, right? The thing that beliefs are for is: you have some belief-like things, you have some desire-like things, you combine them to get behavior that you believe will achieve what you desire and that's the outcome. So, it seems like these are two accounts that are distinct, maybe in tension. Maybe you could have one without the other. I'm wondering what you think about which of these we should go for.

**Peter Hase** (00:42:16):
Yeah. So let me clarify this expected utility view: is that view supposing that beliefs are basically information stores that help you achieve your goals?

**Daniel Filan** (00:42:30):
Yeah.

**Peter Hase** (00:42:31):
Yeah, this view that beliefs are information stores that help you achieve your goals, I think, does really contrast with this truthfulness-oriented view. So, I think, philosophers have managed as a community to agree that beliefs are aimed at truth. But, it's not an evolutionary account of how beliefs work in people. And it's not an evolutionary account of how all the information stores in our brain work or our own attitudes about our own beliefs. So, we might hope for our beliefs to be truth-seeking, but actually, our beliefs merely help us achieve our goals, and parts of our brain or parts of our mind will happily distort our beliefs to help us achieve our goals. And this might be disconcerting to us, because we wanted the beliefs to be truth seeking, but nonetheless, that's what our brain does or that's what part of our mind does, because that's the job or something.

(00:43:31):
I don't know empirically what goes on. I mean, I guess, it's a mix of a bunch of different stuff and it depends on the setting. I'm not a cognitive psychologist, but there's absolutely some tension between these things.

**Daniel Filan** (00:43:45):
So maybe one thought that I have is: I suppose I just want to understand language models. I want to understand what they're doing and why they're doing it. It strikes me that the functionalist account of "beliefs are just things that combine with desire-like things to produce behavior," that might help me do my job better than understanding, "Okay, here's this functional role, but is it aimed at truth? Does it have the right relationship to reality? Or does it merely have a relationship to what it sees and being useful?" As long as I can use it, why do I care?

**Peter Hase** (00:44:23):
I think the intentional stance equation is less noisy when beliefs are aimed at truth. So when you're decomposing behavior into beliefs plus desires, and you're trying to understand... So then, you have raw data of a system at work, where you ask it some questions and if it's truthful and honest, it tells you what it believes, and then you deploy it in an environment and you see what it tends to pursue. The equation is easier to apply, in order to gain predictive power over understanding what the system will do in different situations, if you can trust that the beliefs are truth-seeking and the beliefs are kept cleanly apart from the system's desires.

(00:45:17):
Based on everything we've discussed before - all the mech. interp. stuff, a lot of the natural language explainability stuff - it's not like you have to have this folk psychology theory of how the system is working. You might insist on treating this thing as a machine and you're going to understand all the gears and levers inside: forget about beliefs and desires; I want to know what features are represented, and how that feature influences the next feature, and how that feature influences the next logit, and then how that transforms into the model's overall answer to a question.

(00:45:51):
Let me say one more thing about how these approaches relate to one another. In some ways, I think, these approaches are slightly ideologically at odds. I mean, they certainly attract different researchers with different interests. To a large extent, I think they're totally complementary, because we can think of the mech. interp. approach as being at a low level of abstraction, qnd you're concerned about what's going on inside the model and how those gears and levers work to produce next tokens. And then, we can think of the behavior plus desires work as going on at a much higher level of abstraction. And hopefully, these are good abstractions.

(00:46:30):
And this goes back to some of the [uncanny valley of abstractions](https://www.lesswrong.com/posts/X2i9dQQK3gETCyqh2/chris-olah-s-views-on-agi-safety#What_if_interpretability_breaks_down_as_AI_gets_more_powerful_) work. I think I'm using that phrase correctly. I don't remember the exact title of that blog post from Chris Olah. And this is one of our main motivations for working on some of the language model rationality stuff: asking, "Are these good abstractions? Could these be good abstractions for thinking about how language models work?" And let me give a little bit of opinion at this point: I think we need some higher level of abstractions, and it's going to be really important for us to get the abstractions correct, because I both think that mech. interp. right now feels a little too low-level to me, and I'm not sure if we're going to be able to fully parse all of the internal mechanisms in these really large and complicated systems, at least not as fast as we probably need to in order to keep up with safely deploying models.

(00:47:28):
And, I really don't want us to fool ourselves into thinking, "Okay, yeah, here's the system, and here's how it works, and it has these beliefs, and these desires. And, don't worry, all of the concepts that the system uses are totally human concepts and very easily translatable into human vocabulary. And, the system is going to be rational in ways that basically a lay person could expect it to be rational."

(00:47:58):
Because the language models are still pretty alien and they still do weird stuff, insist on certain reasoning patterns being why they arrived at an answer when we for sure know that they're hiding their reasoning, or the reasoning is hidden internally or misrepresented by the text that gets produced. Weird stuff is still happening and I don't want us to fall into the... There's two traps. One is, we stay in the low level territory forever and we never actually gain a higher level of abstraction and predictability in the systems that is able to keep up with where the abilities progress is going; and the other trap is we really treat the systems as way too human-like and way too rational, and then we forget actually how alien they are.

**Daniel Filan** (00:48:42):
So, this actually gets to a question I have about how we figure out model beliefs. So, one way you could do this, which I see is represented in [the "Are language models rational?" paper](https://arxiv.org/abs/2406.03442), is to say, "Okay, a model's belief is just whatever controls what it says in a somewhat straightforward way." Right? If whenever you ask a model, "Is Paris the capital of France?" If it's the case that whenever you ask it that, its answer is yes, then you might want to just methodologically say, "Okay, that's just identical to saying that it believes that Paris is in France."

(00:49:21):
But I think, you might also have a different perspective where you're like, "Okay, maybe models just have some underlying beliefs, but they're not totally straightforward in how those beliefs translate to what they say. Maybe they're speaking strategically, maybe they're willing to lie." So they actually think that Paris is the capital of Italy, not France, but they just know that you're going to make a big stink if the language model says that. So that's why it says it's the capital of France. These strike me as different ways of understanding language model beliefs. Which way should we go?

**Peter Hase** (00:50:00):
Yeah, this is a good question. It's a really tricky problem right now. I think the direction we go in the paper is trying to make a lot of assumptions, and then give some basic formulation for what beliefs would look like and how they'd be expressed. So the assumptions are that the system understands what you're asking and is trying to be truthful and honest, and it's really playing along. It really is cooperating. And, one of the very first assumptions we make in the paper is that models represent things about the world. I mean, this ongoing debate between some of the [Emily] [Bender](https://dl.acm.org/doi/10.1145/3442188.3445922) crowd, and then the [Piantadosi and Felix Hill paper](https://arxiv.org/abs/2208.02957) is more of a "conceptual roles" kind of view of meaning and language models, which is much more charitable to the idea that language models are actually representing things in the world.

(00:51:03):
That's one of the very first assumptions we make in the paper: that language models are representing things in the world. They seem like competent speakers in important ways. They seem to understand what we're asking them a lot of the time. So, if they're also trying to be truthful, and honest, and capable of reporting what they would believe, then what you do is you look at the probability mass on "yes" tokens and you look at the probability mass on "no" tokens in response to a yes/no question. And you basically treat what the model generates... We're going one level below that; rather than just what it generates, we're looking at probability mass and all kinds of affirmations to a yes/no question and saying, if you met all those criteria, this seems like a reasonable way to say the model assents to the question that you've asked it.

(00:51:52):
But things just get a lot thornier from there, I think, for reasons you describe. In [the 2021 paper](https://arxiv.org/abs/2111.13654), which introduces some of the belief terminology and really is largely focused on model editing, we take a slightly more expansive view, less formally, but more expansive or ambitious in scope in terms of what a belief should count as. So one thing we're looking for there is logical consistency. And this immediately opens up a lot of issues for language models, because they're definitely really knowledgeable and they're decent at a variety of logical reasoning tasks. But, they're just going to say stuff that conflicts sometimes. And, if you ask, "Okay, what are all the consequences of Paris being the capital of France?", something that should be a consequence of Paris being the capital of France, the model might not actually agree with, or the model might say something to the contrary.

(00:52:53):
And then, it's like, "Okay, well, if the model contradicted itself..." So basically, in that 2021 paper we're pointing out that this seems like a criterion people would be interested in. If you want to know if a human really believes something, you ask the question one way to them, and then you might ask a bunch of related questions just to make sure that they really understand what you mean and they really understand what they're talking about, and they've considered some basic consequences of the things they're espousing, such that they really do basically know what they're saying and stand by what they're saying.

(00:53:28):
And so, what happens if you catch some really basic, silly, logical discrepancies, content knowledge discrepancies in the language models, what do you conclude then? Well, maybe the language model is not really an agent. Maybe it's modeling a bunch of different personas, or modeling a weird combination of agents from the pre-training data. And it's doing this thing that [if] you ask the question one way and it knows you want the answer that an educated liberal would give, [it gives that answer], and then you ask the question a different way and it's going to give the answer that a conservative domain expert would give to the question. So, it seems like it said something inconsistent. It seems like it doesn't have a coherent belief, but it's actually doing something even more complicated than that, which is modeling what other people would say in response to your question. I mean, that's nowhere near the end of the difficulties in terms of really getting at the underlying "what does the model believe?" question.

**Daniel Filan** (00:54:32):
Yeah, I wonder if this is a way of thinking about the criterion of beliefs being aimed at truth. So, suppose I take a very functionalist account of what it means for beliefs to be aimed at truth, which is to say that there's some reliable process by which beliefs tend towards the truth, right? That gives me a way of nailing down which things count as beliefs, because if I'm just inferring beliefs from behavior, I worry, "Well, does the model believe this thing or does it have some unusual preferences?" It's really hard to disentangle belief from preferences. People are interested in this. There's this thing called [Jeffrey-Bolker](https://www.lesswrong.com/posts/oheKfWA7SsvpK7SGp/probability-is-real-and-value-is-complex) rotation, which is interesting to look up, about how you can change your probabilities and your utilities and you act just totally the same.

(00:55:22):
But if we say the beliefs have to be accurate, then that fixes what counts as your beliefs. It lets you pick a thing from this class of things you're unclear of how to choose between. I'm wondering what you think about that, just as a strategy for getting at beliefs in language models.

**Peter Hase** (00:55:49):
Yeah. Actually, I really like this line of thinking, because one of the things you might be able to test here empirically is you say, "Okay. We're looking for information stores in the model that are truth-seeking." So, let's give the model some data and figure out what its behavior looks like. And then we have some behavior in an environment. It's still often just really hard to parse what are the differences between the preferences and the desires, and what are the differences in the beliefs. So, we've given it some data, now let's give it more evidence for various hypotheses and see how it updates. If this information store is actually truth-seeking, we know with this amount of data, the model behavior should look like this. And with additional data, and if the model understands the state of the world better, then the behavior should change to this other thing.

(00:56:43):
And I think you can design some experiments like that where if you can fix the desires or make assumptions about the desires and then vary how much evidence the model has about the world, you should be able to see: in what way is the model learning more about the world and how does that influence the behavior? And then start to actually identify how truth-seeking it is in different regards, versus maybe there are certain things about the world that it's totally just using in an expected utility way, and it's totally just instrumental how it relies on that information. So, that's a little abstract. But I think, yeah, there's some unknown variables and you need enough data to actually be able to identify all the unknown variables.

(00:57:31):
What makes this strategy still difficult, I think, is that we don't know yet how language models incorporate new information. We don't know how they respond to different kinds of evidence. We don't know what they treat as evidence. There's so much that we can take for granted when we're studying animals and humans, that it's so hard to even begin applying these to language models, because we want to run studies where we can treat them as agents. But, there's so many ways in which it's hard to exactly know it's a rational agent. Like I said before, it might be this weird amalgamation of different agent simulators. It might be a perfectly truth-seeking agent, but it just has really bad practices for interpreting data about the world, and we're just trying to communicate certain things to it, and it doesn't know how to update its beliefs rationally over time, and this just leads to really wonky behavior in experiments.

**Daniel Filan** (00:58:35):
Yeah. And interestingly... So, I genuinely didn't plan this, but this thinking about beliefs... This is kind of just copying the structure of the paper ["Are language models rational?"](https://arxiv.org/abs/2406.03442), right? Half of this paper is just about coherence norms: beliefs should be coherent with each other. And this is really related to your paper ["Do language models have beliefs?"](https://arxiv.org/abs/2111.13654) where you say, "Okay, you have a belief if it's coherent with some other beliefs, and if this implies this, you change the belief here, it should change the belief here. If you edit the belief here, that should produce a result here." And then, talking about giving language models evidence gets to this part of "are language models rational?" of belief revision, and it just says, "Yeah, it's difficult to understand how things get evidence, but if you could, this would be related to rationality norms."

**Peter Hase** (00:59:25):
Yeah. So earlier when you said this is a line of research, yeah absolutely, because we've got one more thing coming on this. The straggler project from my PhD is going to be one more paper that will hopefully offer a lot of criticism of the model editing problem and belief revision problem in language models, and try to make it clear how difficult it's going to be to actually properly measure belief revision in language models, and hopefully eventually help people better equip language models with the ability to do that. I mean, we certainly want to be able to edit individual beliefs in language models. For all the reasons we've been discussing, it's going to be a little harder, I think, than people have given it credit for so far.

## Localizing and editing models' beliefs <a name="localizing-editing-model-beliefs"></a>

**Daniel Filan** (01:00:11):
Yeah. And actually, this gets to a paper that you have been able to publish ["Does localization inform editing?"](https://arxiv.org/abs/2301.04213). Can you tell us a little bit about what you found out in that paper?

**Peter Hase** (01:00:24):
Yeah, absolutely. So, basically, we were very surprised by the main interpretability finding in this paper, that past work had pitched some model editing methods - again, you're trying to update factual knowledge in a model - past work had pitched some model editing methods and motivated these methods based on some interpretability analysis that they'd done. So, there have been some claims... This paper was not the only paper to make such claims. Many people have this very intuitive notion that where information is represented in models should tell you where you should edit the model in order to adjust its behavior, its answers to questions, and so on.

(01:01:12):
So, in this setting, they were looking at updating knowledge in models, and they ran a kind of interpretability analysis. The work we were building on was [this work on ROME](https://arxiv.org/abs/2202.05262) from [Kevin Meng](https://mengk.me/) and [David Bau](https://baulab.info/) and others. And they used an interpretability analysis called "causal tracing", which aims to identify certain layers in the model that are responsible for its expression of knowledge and the storage of knowledge. And so they make a really intuitively convincing argument: if the knowledge looks like it's stored at layer six in a model and you want to change what the model says in response to a question like "where is Paris? What country is Paris the capital of?", you should edit layer six. That's where it's stored, so go and edit that layer, and that'll help you change what the model says in response to questions about Paris. Very, very intuitive argument.

(01:02:08):
And then, they developed a method for doing this model editing that was really successful and a huge improvement over prior fine-tuning and hyper-network or learned optimizer-based approaches. Their method was focused on really low-rank updates to certain matrices in language models, and it was heavily inspired by this linear associative memory model from computational neuroscience that is a model of how matrices can be information stores or memories for biological systems.

(01:02:46):
The method worked great and the empirical results were really good, and the story sounded great. And we did not initially set out to try to verify the interpretability result here, but that is where this project went. So, we noticed that sometimes the causal tracing method, the probing method, suggested that knowledge was actually stored at later layers. They make this claim that knowledge is stored in early- to mid-layer MLPs in transformers. Everything replicated fine, we just noticed that 20% of the time the knowledge seemed to be stored at later layer MLPs. And so we were like, "Oh, that's weird. It seems like there's some free lunch here." Because if 80% of the time it's stored early on, you should edit early layers 80% of the time. And then, if you ever notice that it's stored in later layers, you should edit the later layers. And this isn't actually how the editing results look empirically. It's always better to edit earlier layers than the later layers. The method's much better editing early layers than later layers, in terms of adjusting the knowledge in the model.

(01:03:51):
The main contribution of the paper is to look at: at the data point level, do the causal tracing results tell you where you should edit? If the causal tracing says it's stored at layer six, is layer six the best layer to edit? If causal tracing says it's stored at layer 20, is layer 20 the best layer to edit? And the surprising thing to us was that the correlation between the localization results, that's the causal tracing results, and the editing performance was just zero. It was just zero. There was just no relationship between the localization results and where to edit.

**Daniel Filan** (01:04:30):
That's very strange. What do you make of that? What does that mean?

**Peter Hase** (01:04:35):
Well, we certainly spent a lot of time racking our brains about it. And something that was helpful actually was talking to a couple people. I would say, 80-90% of people were pretty surprised by this result. And then, 10-20% of people were just like, "Oh, yeah. I mean, I don't know. I wouldn't have expected language models to do anything like localize information in specific places. Or, I don't know, fine-tuning is weird." So actually, some people weren't that surprised about it, which was helpful for breaking us out of the mold.

(01:05:05):
So, what we came to through all of our discussions was that we're guessing residual layers play a pretty clear role here. And I think this is another big, let me say, object-level win of interpretability research over the years: we've been able to gain a lot of insight into how information accrues over time in the transformer forward pass.

(01:05:28):
So when language models consist of these stacked attention and MLP layers that compose a transformer, and between all the layers, there are these residual layers, a lot of work in interpretability has been able to show that a representation across layers slowly approaches some final state, where the final state is the state that is useful for answering a question or predicting the next token. But if you look across layers, it's just very gradual how information gets added to the hidden states over the course of the model forward pass. And, this basically leads us to believe that... So let me point out one empirical thing that will suggest, I think, our final conclusion so far, which is that, if the knowledge seemed to be stored at layer 10, you can often do a good job editing at layer 5, or editing at layer 15, or editing at layer 10.

(01:06:33):
So, if you're thinking about inserting the information into the model forward pass, it seems like you could insert the information before or after this particular place, where some other information is represented. So, this gives us the sense that what you're doing is just adding to the residual stream. Information's just flowing, you can drop some new information in wherever you want. That's the clean picture. I mean, it's speculative. We had a reviewer ask us, "Oh, this discussion section, can you run some experiments to do that?" And we were like, "Well, there are no good language models that only have residual layers."

(01:07:19):
And the big caveat here is that, and here's the real tension, there's [a really interesting paper](https://arxiv.org/abs/2101.04547) that is looking at BERT. So this is a model from a few years ago, it's a paper from a few years ago. Basically, the whole point of the paper is to look at what happens if you swap layers. It's how commutative are layers in a model. And people read this paper very differently, but I can give you some numbers. If you swap two adjacent layers in a BERT model, which could be a 12-layer model or an 18-layer model, your performance on an NLP task might drop by 2-3%. And if you change the first layer and the last layer, its performance will crash 30% or something.

**Daniel Filan** (01:08:09):
This is 30 percentage points of accuracy?

**Peter Hase** (01:08:11):
Yeah, 30 raw points, off of 80 or 90 or something.

**Daniel Filan** (01:08:14):
Okay.

**Peter Hase** (01:08:15):
People really read these numbers differently. So some people have looked at these numbers and said, "Wow, layers seem surprisingly swappable. You could swap two layers and only lose two points or something." Some people, I'm probably in the latter camp, are like, "Three points of accuracy... I mean, for years that's been a publishable result. That's been a big deal in various settings. You've really found something if you're changing accuracy by three points."

(01:08:41):
Yeah, layer role is important. And, it seems totally weird that you could inject information into layer five, versus layer 15, and have it have the same effect on... Surely, there is dependence on the information coming in to layer seven, and layer eight, and layer nine. That's the tension here. We really don't have a complete picture. But there's been a lot of cool mech. interp. work here, focusing on particularly... [Mor Geva](https://mega002.github.io/) has been doing a lot of work looking at how this information accrues over time in the model forward pass, and also, recently, how this information enables models to answer factual questions or do some simple kinds of factual associations.

(01:09:28):
So, we're gradually gaining a bigger picture there, which maybe will one day help us design better model editing methods, because that's still the goal here. We mentioned this before, this was certainly the goal in [the ROME paper](https://arxiv.org/abs/2202.05262), and I'm still optimistic about this. I'm hopeful that we're going to be developing our own better causal models of how the neural networks are working. I'm optimistic it will eventually help us actually do some model editing. It will eventually help us tweak some internals and change the behavior.

**Daniel Filan** (01:10:00):
So that was kind of, I guess, a hopeful read of, "Oh, here's a way we can make sense of the results of this paper." When I read your paper, one thought I had is: we're working off this assumption that belief localization is a thing, right? Beliefs are stored in one bit of a neural network, such that we could say, "here's the bit, we found it." It doesn't seem like that has to be true, right? And I wonder: if I had this method that purported to tell me where a belief was localized in a network, and I have this new thing, which is like, "oh, but if I'm editing a network, I have to change it somewhere else." One way I can think of that is, "oh, I just proved my assumption wrong. We just demonstrated that it's not actually true, that there's one place where this knowledge resides." What do you think of this interpretation? Am I being too paranoid or too skeptical here?

**Peter Hase** (01:10:59):
No, this is a good point. I think skepticism's completely warranted. You had this comment earlier that in interpretability a lot of the progress actually seems to be disproving naive models of how language models or neural networks work, and I think this is a good example of that. And really the next step here is to start developing some better causal models of what's going on.

(01:11:26):
Simply the idea that information is localized is this very simple, intuitive, potentially naive mental model of how things work. And yeah, we've probably disproved that. And like I said before, 10-20% of people I talked to about this were just not surprised at all. So they already had some kind of working mental model of how the transformers would work.

(01:11:48):
What next? We should figure out what components are necessary for achieving a certain behavior. We should figure out what components are sufficient for achieving a certain behavior. We need to start drawing some actually more complicated causal pictures of: so this layer represents this information, and then it passes that information to this other layer, which applies a function to that information. And then you get a new variable, and then the next layer reads off that information. And actually, all it does is it reads it off and changes its position, and so it puts it in a new spot, and then the layer after that reads from the new spot and decides how that information should combine with some other information.

(01:12:28):
And basically, this is saying we need circuits. We need to build up a circuit understanding of how the information flows through the network. And this picture that was like, "there's a feature here and here's how it relates to behavior. There's information here and here's how that relates to behavior," was just way too high level, and we need to start actually drawing a much more detailed picture, which is the complete end to end story, I think.

**Daniel Filan** (01:12:59):
So actually, while you were saying that, an idea occurred to me about another kind of simple localization model that may or may not be right, that you might already have enough information to shoot down. So here's the thought. I think sometimes, especially in the [transformer circuits line of work](https://transformer-circuits.pub/) at Anthropic by Chris Olah et al., I think in that work there's this thought that the residual stream is the key thing. This also relates to what you were saying, the residual stream is some kind of key thing.

(01:13:27):
And maybe a thing we can do is we can interpret dimensions within that residual stream. Maybe there's one dimension within the residual stream or one direction inside the residual stream, that really is where some knowledge is localized in some sense, but it's localized in a dimension in the residual stream, not in a layer of the network.

(01:13:49):
I think, and let me know if I'm wrong. I think if this were true, then it would suggest that any layer of the neural network you can edit to change the model's beliefs about a certain thing, and it doesn't really matter which layer you edit, but whichever layer you edit, the edits should do a similar thing to the residual stream. I think that's a prediction of the residual stream direction theory of language model knowledge. You might already know if that holds up or not. Does it hold up and am I even right to think that this tests it?

**Peter Hase** (01:14:24):
No, no, I like the sketch. So I think there's been more work looking at interventions on weights versus interventions on representations, for one, here, which may be a little bit [of a] more direct path. So I don't think the exact experiment you described has been done, but certainly when people are thinking about a certain direction encoding for some knowledge, or a certain direction encoding for a specific feature and just how highly that feature activates, that immediately suggests, okay, let's just turn that feature up, or let's turn it down, let's clamp it to a certain value. Let's do some intervention at every layer, at a certain layer, and so on, and see the effect on behavior. And this is a good causal intervention for actually understanding if that representation represents what you think it's representing, and testing that a bit. And then, the useful thing here would be editing it. So if it was faulty in some way or malfunctioning in some way, you would change it, and it's a very direct route. You're just editing the representations.

(01:15:30):
The first kind of odd thing about this is: well, we would just like to be doing an intervention on the model. We want to be intervening on the model such that we're permanently changing the model's knowledge or we're permanently changing how the model processes information. I can always clamp a representation, but nothing changes after that on other data points. I can't always clamp this representation for every data point, obviously.

(01:15:59):
I mean, I like the idea that testing... so let's say we're going to try to edit the weights to adjust that knowledge. Presumably the hypothesis there is when we edit those weights, those weights act on that mechanism. And what I mean is they upweight or downweight that feature, and that's how the ultimate behavior gets changed. What I think is more elegant about your sketch and this weight intervention thing, or potentially there's something that's just appealing in terms of generalizability or universality of this weight intervention thing, is when you edit the representation, you're kind of starting in the middle of the process. You're like, "well, the model acts upon representations and that leads to behavior." So if you start in the middle and you say, "okay, let's clamp the representation and see how that leads to behavior," it's like, well, great. That's a hypothesis that you might be able to verify, but it's not actually the whole causal chain. The whole causal chain is that input comes in and weights act upon the representations, and then representations are processed by other weights, and then there's logits, and then there's behavior.

(01:17:10):
And if you can actually adjust the weights at that point, you're getting I think a larger slice of the causal pipeline, and you're doing something that can be permanent. You could permanently edit the model such that it changes its behavior on one example, and then hopefully you would want to check [that it doesn't change its behavior on] others, that it's not supposed to change its behavior on. And also, to tie back in the "consistency among beliefs" things, if you're editing some knowledge, there is other data that its behavior should change on, that you would want to check, that this activation clamping thing, I think is maybe just not the right exact method for.

**Daniel Filan** (01:17:56):
I do hear about people checking this sort of thing with activation clamping. I'm also thinking [steering vectors](https://arxiv.org/abs/2308.10248), so [Alex Turner](https://turntrout.com/welcome)'s work: you have some steering vector for some property. If I give it some examples, does the network generalize to other things where it's supposed to have this property? Sorry, I'm being a little bit abstract. Unfortunately, the concrete thing I'm thinking of is unpublished work he's discussed, so I'll talk about it after the recording's over. But I think there is some work kind of like this.

**Peter Hase** (01:18:34):
Yeah. And I mean there's so much work in this area nowadays. It's hard to keep up with everything, but the "inference time intervention" [paper](https://arxiv.org/abs/2306.03341) does something that's kind of like permanent activation steering. You're just permanently upgrading some activations that are supposed to be truthfulness activations. And it's like, "we want all the questions to be answered truthfully. It's fine that we've just permanently upweighted these activations via some..." Actually, I don't even remember the exact implementation detail there. But yeah, I definitely remember some other examples that look at this kind of generalizability thing in (I think) the right way.

## Beliefs beyond language models <a name="beliefs-beyond-lms"></a>

**Daniel Filan** (01:19:18):
Sure. I'd like to move out a little bit to talk about beliefs in neural networks. This might be a little bit out of your wheelhouse. Mostly, I think when people, especially in the public think about language models, or think about neural networks being smart, being intelligent, having beliefs, I think they're normally thinking about language models. It's not so obvious why we shouldn't think of other kinds of networks as having beliefs.

(01:19:48):
So the simplest case is if you think about AlphaGo, right? Reinforcement learning [game-]playing networks: plausibly, they have some sort of beliefs about what moves work. I'm also thinking of image generation models or movie generation models. They're generating some scenes in the world. If you prompt one of these models to say, "Hey, please show me a video of the cool sites in Paris," and it shows you a thing of something that looks like the Eiffel Tower, one might be tempted to say, "Oh, this image or this video generation model believes that the Eiffel Tower is in Paris," right? I'm wondering, are you familiar with the work on inferring beliefs of things other than language models, and does it look similar or do we need to do different things?

**Peter Hase** (01:20:43):
Yeah, this is really interesting. And I can say my first reaction to this is I'm pretty sympathetic to describing a lot of these systems, like image generation models, video generation models, RL agents, as having beliefs. I think one of the first stumbling blocks there is that people normally think of beliefs as being expressed in language, and philosophers think of beliefs as being expressed in some kind of formal language, that might then map noisily to natural language, but it's expressible at some level of formality. And I think what would make this case compelling for (let's say) a video generation model having beliefs, is for it to be able to generate some scene that demonstrates some knowledge about the world.

(01:21:43):
And then, if you could actually translate its internal representations into a sentence that expresses that knowledge, you'd be like, "Oh, okay. Yeah, definitely it knew that thing." When the model generates a ball being thrown and it seems to have some intuitive physics, and if you could actually figure out how to translate its internal representations into a sentence that describes how you might expect a thrown ball to move through the air, you'd be like, "Ah, yes, okay. This was just the last step to showing that the model actually knows what it's talking about."

(01:22:21):
And I think that colloquialism actually is important, because it knows what it's talking about. You're able to express what the thing is. But it doesn't mean that the representations just don't already act as truth-seeking representations. Again, that being the criterion - an information store that is aimed at truthfully representing the world - I think there's a lot of representations and all kinds of multimodal models, all kinds of RL agents that aim to represent things truthfully.

**Daniel Filan** (01:22:55):
Actually, one thing I'm thinking of: so you mentioned there's this difficulty, which is "how do you translate it into a sentence?" There are two thoughts I have here. So the first thing is: in some ways it's a little bit nice that neural networks, they don't necessarily use the same concepts as we do, right? As you've written about, as you've noted.

(01:23:17):
And so, on the one hand I'm like, "it's kind of nice that by being a little bit removed from natural language, just a little bit, maybe this helps us not be too shackled to it." And then on the other hand, if I look at your work - ["Do language models have beliefs? Detecting, updating, and visualizing beliefs"](https://arxiv.org/abs/2111.13654) - where you're sort of using these implication networks of "if you edit this belief and this implies this, then that should change this, but it shouldn't change this." It strikes me that you could do something very similar, with (let's say) video generation models. Somehow it seemed like it used language, but it didn't really. Imagine you want to persuade a thing that, you want to intervene on some video generation model and get it to think that the Eiffel Tower is in Rome, but not in Paris.

(01:24:09):
So here's what you do. You try and make some edits, such that you generate a video like, "Hey, show me a video of the top tourist attractions in Paris," and it just has the Arc de Triomphe. It doesn't have the Eiffel Tower. "Show me a video of the top tourist attractions in Rome," it does have the Eiffel Tower. "Show me a video of the top tourist attractions in London," shouldn't change anything. This seems like a very close match to work you've already done. Now, I could be missing something important, or it's probably way more annoying to run that experiment, because now you've got to watch the video and somehow you've got to check does it look enough like the Eiffel Tower? There's some difficulty there, but it seems like some of this natural language work actually could translate over.

**Peter Hase** (01:24:55):
Oh, absolutely. And I like the kind of cases you're setting out. I mean, I think it's almost directly analogous in a lot of ways. You could run that experiment. It sounded like you're kind of imagining a text to a video model, which I think makes that experiment a little easier to run. But yeah, for sure, the setup makes sense. There's [a paper that comes to mind](https://arxiv.org/abs/2112.01008) that I unfortunately won't remember the name of, but they were trying to do some editing with vision models. This was an earlier paper, I think before people had this more grand view of what editing could accomplish, and it's like, "wow, we're really changing the knowledge and the models, or we're trying to at least."

(01:25:36):
And it was a little bit more in the feature-to-classifier pipeline in a sense, where it's like, okay, the neural network as a classifier uses features, we're going to intervene on what features are represented. And this paper did something... it was changing how the model represents snow, I think. So there'd be a dataset where snow's statistically related to a variety of classes, and the model learns that, and they wanted to do some intervention that would lead one class to usually get classified as another, by virtue of there being snow in the image, or by virtue of there not being snow in the image. That was their goal for editing.

**Daniel Filan** (01:26:21):
Do you remember the authors?

**Peter Hase** (01:26:24):
The paper was ["Editing a classifier by rewriting its prediction rules"](https://arxiv.org/abs/2112.01008).

(01:26:27):
So so far people have been thinking about this in other modalities as well. I'm sure there's work in an RL setting where... Unfortunately (I think) not all of the subfields in AI communicate that well with each other. There's a lot of work that I think is actually super interesting interpretability work that goes on in vision and RL that just doesn't get branded that way, so it's hard to find sometimes. But yeah, people train RL agents and then are doing all kinds of interventions on them nowadays, like changing things about the environment, changing things about the policy network itself to try to better understand what factors lead to what behavior. And a lot of that you can think of like model editing: editing a goal of the agent, or editing how it perceives its environment.

## Easy-to-hard generalization <a name="easy-to-hard generalization"></a>

**Daniel Filan** (01:27:21):
So moving out a bit, I think, maybe on your website, maybe somewhere else, I've got the idea that there are three lines of work that you're interested in. So the first two are interpretability and model editing, and we discussed those in the previous discussion. The third that I've heard you mention is scalable oversight. I take [this] to mean something like figuring out how we should supervise models, how we can tell if they did things that were good or bad when they get significantly smarter than us. In my mind, this is the odd one out of these three. Do you agree, or do you see there being some unifying theme?

**Peter Hase** (01:27:59):
No, I think you're right about that. It's a recent new area for me. I've really stretched to tie them together in talks before, where I said "okay, model editing is about trying to control model behaviors when you have an expected behavior in mind, or you can properly supervise the model, and scalable oversight or easy-to-hard generalization is about trying to develop this predictable control over model behaviors, but in a setting where you don't exactly know how to supervise the model." But it's a segue for a talk, it's not a very deep connection.

**Daniel Filan** (01:28:51):
Well, I do think there's something to that. Often people talk about inner and outer alignment as being somewhat related but distinct, and interpretability has this relation to inner alignment, scalable oversight has this relation to outer alignment. I think there's something there, but it sounds like this isn't how you got interested in scalable oversight. So how did it become one of the three?

**Peter Hase** (01:29:09):
Well, yeah, you're right, because it's not the original story behind the research we've done in that area. Originally, we were really interested in some of this work on eliciting latent knowledge: there's been some blog posts in the area. There was [a research paper](https://arxiv.org/abs/2212.03827) from [Collin](https://collinpburns.com/) [Burns](https://en.wikipedia.org/wiki/Collin_Burns) and others at Berkeley on this problem, that we were very interested in, largely from an interpretability perspective, understanding how to probe and detect knowledge in language models. But then, I realized after reading and rereading Collin Burns's CCS paper, that it was really about scalable oversight, and it really wasn't an interpretability thing. The problem that they were primarily interested in was getting models to report their knowledge or extracting knowledge from models, even when you don't have labels, even when you can't supervise, or fit the model to a dataset, or probe it in an unsupervised way.

(01:30:22):
How this came to my attention was, we were really looking into this closely when I was working at the Allen Institute for AI last year, doing a research internship there with [Sarah Wiegreffe](https://sarahwie.github.io/) and [Peter Clark](https://pclark425.github.io/). And so, we were looking at [the CCS paper](https://arxiv.org/abs/2212.03827), and then we realized it was really about scalable oversight. I think that was immediately clear to a lot of people. It wasn't immediately clear to us, because it was also written in this interpretability language at times too. And then, the first thought we had was, well, it's not like we don't have any labeled data. We have some labeled data. It's just we're trying to solve problems that we don't have labeled data for, but there's labeled data everywhere. There's just all kinds of labeled NLP data that we have, all kinds of datasets that are just specifically constructed to contain true/false labels for claims about the world. Shouldn't we be leveraging this to fine-tune models to be truthful, or extract knowledge for models?

(01:31:26):
So what really is the way to set up this problem? This turned into a paper we worked on called [The Unreasonable Effectiveness of Easy Training Data for Hard Tasks](https://arxiv.org/abs/2401.06751), which in a lot of respects looks a lot like [OpenAI's "Weak to strong" paper](https://arxiv.org/abs/2312.09390), and there's just some interesting close analogies between them. The setup is, you want to do well on a problem that you don't know the answers to, and you can supervise the model on some problems, but not the problems you really care about. And there's some methods work in their paper. Our paper is really just focused on benchmarking and getting a lay of the land.

(01:32:08):
We just wanted to try to gather data - it could be STEM questions, it could be math word problems, it could be general knowledge trivia. We had various tasks like that - and divide the data into easy and hard, and pretend that you can't label the hard data, pretend that you can only label the easy data, and fit a model to that, prompting, fine-tuning, probing, whatever way, fit a model to that. And we're just doing some benchmarking, where we're asking "that was a little bit of supervision. It wasn't the right supervision, but it was a little bit of supervision. How effective was that supervision?"

**Daniel Filan** (01:32:46):
Okay. And should I think of this as kind of modeling humans giving feedback to... we're doing RLHF++ on CEO-bot, we're training it on problems where we do know what CEO-bot should do, and we're hoping that it does well on problems where we don't know what CEO-bot should do. Is that roughly the kind of picture I should have for what this paper is trying to be a toy model of?

**Peter Hase** (01:33:17):
Yeah, so that's an important question, because it's like, what does this lead to? We want to have some calibrated judgment on when there are problems where we really don't think we're going to be able to supervise the model effectively. And let me quickly drop in an example from the [Dario] [Amodei](https://en.wikipedia.org/wiki/Dario_Amodei) et al. paper ["Concrete problems in AI safety"](https://arxiv.org/abs/1606.06565), that I think is the one that basically introduces this terminology 'scalable oversight'.

(01:33:46):
We're thinking about a setting where the model might be acting in an environment where it's taking large complex actions and we just can't check everything. We just can't check every possible case. So we can't properly reward the model based on if it's done a good job or not all the time. And I think the CEO analogy here is: the model's doing complicated things in a complex environment and a long time horizon, and we just can't properly reward or penalize the model all the time. So backing up: we want a calibrated judgment for if we can properly supervise the model on some things, how should we expect the model to behave on the other things that we can't supervise it on?

(01:34:34):
I'm really excited about more methods work here for trying to... if there's a gap there, if there's a supervision gap, I'm excited about ways for trying to close that gap, but whether we get it, whether our easy supervision or our weak supervision is 60% effective or 70% effective, compared to if we could get the proper supervision for a problem in place, getting that number from 70 to 80% is good. That's interesting methods research that should be done, but upfront, we just want to know what the number is, and we just want to be able to say, "if we think the supervision is halfway effective, are we comfortable deploying this agent in a setting where it's sometimes going to be doing things that we never actually checked if we could do it properly or not, or we don't even know how?"

**Daniel Filan** (01:35:28):
Right. And just to concretize that, by "halfway effective", 60%, you're thinking just in terms of the gap between an unsupervised model that hasn't been trained on any instances of this problem, versus a model that's being trained on the right answers to the hard problems. Is that the gap you're talking about?

**Peter Hase** (01:35:52):
Yeah. Sorry. Thanks for clarifying. That is the gap, exactly. And that's the gap in our paper where... terminology is not important here. We're calling it "easy-to-hard", where the baseline is you have some system that you just can't supervise at all - so that might look like zero-shot prompting, it might look like a totally unsupervised method. It might look like [CCS](https://arxiv.org/abs/2212.03827), or an unsupervised probe, where you have questions or you have data, but you don't have labels for anything. The ceiling is, you can fully fine-tune the model. You can fully probe the model with labeled data for exactly the kinds of problems that you care about. And then the question is: between that baseline and that ceiling, how far can you get with incomplete supervision?

(01:36:45):
That's our setting. A small technical detail: a slight difference [between our setting and] the "weak to strong" setting in [some of OpenAI's work](https://arxiv.org/abs/2312.09390), is that their baseline is the weaker teacher model trying to do the problem on its own. So they have this analogy to people and a superintelligent system, where we could either imagine that we try to do the problem on our own, or we try to align the superintelligent [system] to do it for us. So their baseline is a person doing a problem on their own and their ceiling is a fully aligned, superintelligent system. And then, what's in the middle is us weakly supervising the superintelligent system.

(01:37:32):
And so, the baseline's different there. I think this is actually important to think about a little bit, because we're just going to have options for baselines. I mean, it happens that a lot of the time, pre-trained language models do decently at stuff zero-shot, which is kind of surprising.

(01:37:54):
Sometimes pre-trained language models do better at things zero-shot than laypeople do. So if you're thinking about accuracy as a metric, the lay person is a weaker baseline than the fully unsupervised model. That's how we ended up with our baseline. The story I just gave is how they ended up with their baseline, but the gap is the gap.

**Daniel Filan** (01:38:20):
Yeah. So I actually have a bunch of questions about just how we're measuring things, what the methodology is, but before we get to that, I think listeners are chomping at the bit. They want to hear, what's the gap? How much of the gap can you-

**Peter Hase** (01:38:34):
Oh, yeah. On a lot of our tasks that we were covering, it's like 95% or 97% as effective. So let me make that concrete. If you were solving eighth-grade STEM questions and supervising a 70 billion-parameter language model with third grade-level supervision, it does just as well as if you're supervising it with eighth grade-level supervision. If you were testing on college STEM questions and you were supervising the model at a high school level, it's going to do just as well as if you had supervised it with the college-level supervision.

(01:39:16):
There are a couple of places where the gap starts to grow and the effectiveness of the incomplete supervision starts to become clear. One of the settings was something a little bit more like reasoning tasks or settings where there's chain of thought. So if you're doing math word problems, if you're doing compositional reasoning kinds of tasks, and you're just supervising the model with really short, simple reasoning problems, and asking it to do better on longer, more difficult reasoning problems, that's a setting where the gap grows a bit. That interpretation has a couple caveats that are in our appendix or something, but I think that interpretation is basically plausible, that that's a setting where the gap grows. If the supervision is just very weak, very far away from the thing you care about, this is also probably pretty intuitive.

(01:40:13):
So we did something where we tested on college STEM questions and we supervised with high school versus eighth grade versus third grade. So there's just a bit of a gradient there. The high school was as effective as the college, but the eighth grade, you're starting to do a little worse, and the third grade you're doing noticeably worse. So we can imagine there are some settings where the gap grows a bit. Overall, we were pretty surprised how effective this incomplete supervision was, and I think this is mirrored in a lot of... I mentioned there's a difference in terminology, "easy-to-hard" versus "weak-to-strong", where [the OpenAI paper](https://arxiv.org/abs/2312.09390) was focused on a slightly different "weak-to-strong" setup. Still quite analogous.

(01:41:01):
I'n their appendix, they actually have directly analogous easy-to-hard results that do the same kind of labeling setup we do, and they also were seeing really positive... You can talk about the effectiveness, you can talk about the ratio, you can talk about the gap. They just also got quite positive results, that this partial supervision ended up being quite good. Seems likely to me that there's something just especially good about getting clean labels versus noisy labels here.

**Daniel Filan** (01:41:28):
I guess the question is: how is this possible, right? This model doesn't know how to do a thing. It doesn't know how to do this really hard thing. You teach it a really easy thing, and then it just does pretty well on the hard thing. Like with humans, when you've gone through third grade that isn't sufficient to have you graduate from college, right? And yet somehow with language models, it is. What's going on?

**Peter Hase** (01:41:55):
So I mean, really interesting possibilities here. So one thing I'd point out is I would dispute one of your premises a little bit when you say that, "Well, language models don't know what's going on. How are they getting all the way there to solving these hard problems?" Because I suspect that there are some latent skills or latent abilities that we're tapping into in the model when we're doing this kind of partial supervision or incomplete supervision.

(01:42:23):
This just comes from pre-training. It just seems like it must be the case that in pre-training, models will have seen examples of hard problems, and potentially either directly learned how to do certain problems, directly memorized certain facts, or just learned certain facts. I think we're seeing stronger and stronger cases over time that language models are learning robust generalizable skills, that are interesting skills that are just learned across data points in their pre-training dataset.

(01:42:57):
Like, you read a bunch of different biology textbooks, and you actually start to learn themes of biology and some core principles, in a way that you can think of individual documents being important for answering a question as just more like learning some facts about the world. The true themes of how to solve math problems or how to think about chemical reactions being the skills of doing math, or the skills of doing chemistry. It just seems like models are picking up on these things. And so, when we're thinking about what is the effect of easy supervision on a model, it looks something like eliciting task knowledge, or activating task knowledge, that you're kind of cueing the model into: "okay, I'm doing biology and I need to do biology in a way that a college student is supposed to do biology."

**Daniel Filan** (01:43:51):
It seems like this is kind of related to the discussion on figuring out beliefs in language models, right? If a language model can have this latent knowledge that isn't even reflected in its answers, unless you fine-tune it a little bit on related questions, it seems like that's got to say something about how we're going to understand what it means for a language model to believe something and how we figure it out, right?

**Peter Hase** (01:44:16):
Yeah, that's a good point. So I remember when we were talking about simply how one detects beliefs in language models: how do you even go about saying that the model believes something? I remember mentioning in the paper, we definitely have to make a lot of assumptions about understanding the question, truthfulness, honesty, that if you bundle them all together, I think can be kind of analogous to this task specification thing, where it's like, "okay, what am I doing? I'm answering questions truthfully, according to this person's understanding of the world..." Hopefully we think of truth as some kind of objective thing, but also it's always going to be a little bit catered to our 21st-century scientific worldview of how things work. So, there's something that looks like task specification there, which I think we assumed away in the belief detection case, but really comes to the forefront when we're thinking about easy-to-hard generalization and how models are even doing this.

(01:45:20):
So, I'll mention there's an extra result which we have in [the new camera-ready version of the paper](https://arxiv.org/abs/2401.06751v2), which is now on arXiv. We compared to something like a trivial prompt, just giving the model the simplest possible true statements and seeing how that does. So you just say, "Okay, what color is the sky normally? How many legs does a dog have normally?" These are questions that essentially anyone, probably most children, could answer, as opposed to third-grade questions or eighth-grade questions. I mean, believe me, I could not do any of the college questions in the data.

(01:46:02):
There's something just very basic about trying to strip anything away that's like, is it domain knowledge? Is it math ability? Is it answering things? And the way an eighth-grade science textbook would be written, trying to strip some of that away and just think about truthfulness.

(01:46:19):
And what was interesting is that these trivial truthful prompts did not explain the entire effect of the easy supervision. They explained part of the effect. So, it's a little noisy, it's probably somewhere around half. But it seems if you're thinking about "how do we get the model to do college biology if we can't do college biology?" We're going back to: this is a stand-in for "how do we get the model to do something really hard that we don't know how to do?"

(01:46:49):
We definitely need to do something that's convincing it to be truthful and fine-tuning it to be truthful or mechanistically intervening to get it to be truthful. And then, we also need to do something that's communicating to it that it's task is to do biology, and get it in its biology representation space, task space: these both seem to contribute to the overall generalization.

## What do easy-to-hard results tell us? <a name="easy-to-hard-results"></a>

**Daniel Filan** (01:47:15):
Okay. Let's say I take this picture of elicitation for granted. The reason we're getting easy-to-hard generalization [is] training on the easy things sort of elicits a mode of "we're doing this task, and we're trying to get it right rather than wrong." There are two takeaways I could have for this.

(01:47:35):
One takeaway is this means that these experiments are just very unrepresentative of the task that we're interested in, because we'll want to train CEO-bot to do stuff that CEO-bot doesn't already know. And the only reason we're getting easy-to-hard generalization here is that in some sense, the language model already knew how to do these tasks.

(01:47:55):
Another way I could interpret these results is, this is actually great news. It turns out that language models know a bunch of stuff that they don't appear to know, and all we have to do is just nudge them on track. So, if we want language models to be really good CEOs, it might not seem like they know how to do it, but they actually secretly do, and all you need to do is just nudge them a little bit to make it happen. Which interpretation is right?

**Peter Hase** (01:48:23):
I would describe this more as a difference in use cases, I think. So, I think we can imagine use cases where there's some extremely capable system that we suspect could do a task very well, either in the way we want it to or in a way we don't want it to. But we know it's competent, we know it's capable, and we're just focusing on aligning that thing or doing this little bit of steering, eliciting the one task representation rather than the other.

(01:48:53):
But we're basically taking for granted that it's going to be competent, and it's going to be able to do the thing well. So, that's the kind of use case and that's the kind of world where it's a really strong model: the empirical results we've seen so far feel promising, conditioned on [the fact] that we're doing this thing that's treating hard test questions that we secretly know the label to as a stand-in for difficult questions that are actually really hard for us to label.

(01:49:23):
And when we're using big pre-trained language models that have probably learned a fair amount about this stuff before, this contrasts with the setting where we want this model to do something truly novel. We have no idea how to do it. We don't know if the agent knows how to do it. We want the agent to try to do it and to try to do it in an aligned way, in a way that would be good for people, but we don't even necessarily have a reason to think that it would already know how based on its training data.

(01:49:54):
And this use case, this kind of hypothetical world looks a lot more like classical research in compositional generalization in NLP where people have, for a long time, studied settings where the training data does not have the information you need to actually solve the test problem, and we know that. The test problem requires a particular kind of architecture, a particular kind of bias in the learning system that would lead the learning system to learn the right abstractions from the train data and combine them in the right way that would lead it to get the test problem correctly.

(01:50:38):
One thing we do in the paper is we speculate a little bit about why our results look a lot different from previous compositional generalization research in NLP where people have looked at the ability of language models to do, for instance, this kind of length generalization before.

(01:50:56):
And there have been a lot of previous results that, in certain language learning settings, other kinds of NLP tasks, like when the training data looks different from the test data and the test data includes questions that are compositional and those skills are not directly represented in the training data, neural networks often really fail at that kind of generalization. It's often just really hard for neural networks to generalize to these entirely novel problems that require combining known things in exactly the right way.

(01:51:32):
And so, we speculated in the paper that this... we were guessing this has a lot to do with pre-training, and it has a lot to do with there already being some of the right building blocks in place and language models becoming increasingly good at combining those building blocks based on an incomplete partial amount of supervision.

(01:51:50):
But for more concrete research in this direction, I'd point to [some](https://cims.nyu.edu/~brenden/papers/LakeEtAl2015Science.pdf) [work](https://www.nature.com/articles/s41586-023-06668-3.pdf) from [Brenden Lake](https://cims.nyu.edu/~brenden/), who's a cognitive scientist at NYU, and certainly some other NLP people, who I might be able to remember later, are looking really directly at tests for compositional generalization ability. And particularly some of Brenden Lake's work, I think, has started to tease together a bit "when do you need really strong architectural assumptions? When do you really need really strong biases in models to learn the right generalization patterns from limited data? Or when will neural networks actually be able to pull this off basically and actually be able to do the entirely novel thing?"

**Daniel Filan** (01:52:45):
This also gets me a bit back to this question of, how representative is this of hard problems? And one concern, I think a lot of people in the x-risk community have, is generalization of alignment versus capabilities, where a thing people imagine is: "Look, if you learn a little bit, it's just really valuable to just keep on knowing stuff. But if you're playing along with the human for some period of time, that doesn't necessarily mean you're going to play along later."

(01:53:13):
So, I think a thing you said earlier is, imagine you have a setting where the AI has a bunch of knowledge, and it knows how to be aligned or it knows how to be misaligned, and we're going to give it some examples of doing stuff that we want to fine-tune it on somehow.

(01:53:27):
I think a concern a lot of people have is, "Well, it's plausible that the generalization it learns is 'play nice with humans when you can or do whatever is necessary to achieve your secret goal of taking over the universe and replacing everything with cream cheese', and for now, that that goal involves playing nicely with people."

(01:53:50):
And so to the degree that you really worry about this, it's possible that this is going to reduce how much you trust these preliminary easy-to-hard generalization results as saying much about the difficult case. So, I wonder what do you think about these concerns and how do you think they play into how we interpret the results in your paper?

**Peter Hase** (01:54:16):
Yeah, that's a good question because I think it's fair to try to contrast some of these "you can do math, you can do biology" kinds of tasks with learning human values and learning to act in a way and in an environment that preserves human values. These just feel like different things. Particularly, we would expect during pre-training, and to an extent during RLHF, these different kinds of information to be instrumentally useful to different degrees.

(01:54:52):
This is my understanding of your question: there's going to be a bunch of settings where it's useful for the model to know all kinds of stuff about the world, but whether or not it needs to have learned our values and be robustly aligned with our values when it's deployed is maybe less clear, just based on the way pre-training is done or the way RLHF is done. This is a really good question.

(01:55:14):
I think I'm excited about this kind of easy-to-hard, weak-to-strong work with reward modeling and in a RLHF setting, where this wasn't something we looked at, but [OpenAI](https://arxiv.org/abs/2312.09390) looked at this and I believe other people are currently building on this as well. I'm trying to get a sense of how the problem looks in a reward modeling or reward inference setting where we partially specify things that we care about, or the environment's really big and it's always hard to say exhaustively when everyone was harmed or not or exactly how good or bad an action was for the people involved. So, we give incomplete supervision to the model about our values and what states are good for us or not or good for users, and we see how aligned the model actually is on tests where we actually take the time to then inspect, "Okay, would this behavior have been harmful? Would this behavior have been aligned?"

(01:56:26):
I think the trick in the experiment design here is still that we need a setting where we can check, so this is the college route. We actually do have the answers to the hard questions, and so we end up doing the same kind of thing in the reward modeling or reward learning setup where, well, at some point we need to think of what would the questions be that we could ask the model, such that we know what counts as a good response or not? What would be the scenarios we would deploy the model in, such that based on the model behavior it was safe or not?

(01:57:05):
We need those scenarios to figure out what this gap is. So, how effective was the supervision relative to perfect supervision? Was it 60% as effective? Was it 90% as effective? Is it effective enough that we then trust, based on our incomplete ability to supervise the models, that they will be robustly value-aligned?

(01:57:24):
I think that part has a lot in common. It could be that the results just simply look worse due to some fundamental differences in what gets learned during pre-training.

## Easy-to-hard vs weak-to-strong <a name="easy-to-hard-vs-weak-to-strong"></a>

**Daniel Filan** (01:57:33):
I guess I'd like to move on a little bit and talk about methodological questions, because I think there are a few really interesting ones that come up in this paper or this line of work.

(01:57:42):
So, the first is, we've talked a little bit about the distinction between [this easy-to-hard generalization paper](https://arxiv.org/abs/2401.06751) and a paper that I believe roughly concurrently came out of OpenAI [about] [weak-to-strong generalization](https://arxiv.org/abs/2312.09390). When I first saw them, I was like, "Oh, they accidentally did exactly the same thing." And then, you read them a bit carefully, and I'm like, "Oh no, it's kind of different."

(01:58:09):
The biggest difference that I at least noticed is, it seems like your work is "train on easy problems, and then how good are you at hard problems?" whereas the OpenAI version seems more like, "suppose you get trained - the problems are just as difficult, but you initially get trained on data where the person grading how good the answers were, just wasn't very good at their job. Do you generalize to really getting the right answer even though the grader was noisy?" I should say it's been a while since I read the weak-to-strong generalization paper.

**Peter Hase** (01:58:47):
If it helps, I can try to rattle off some of the differences that I've noted as we've been giving talks about the paper, because certainly they look pretty similar to a high level.

**Daniel Filan** (01:59:00):
I think I'm most interested though in this axis of difference, but I'm not sure I'm characterizing it correctly.

**Peter Hase** (01:59:07):
Okay. Well, yeah, I'm happy to focus on that one axis, but if you can try to describe it again for me, we could start from there.

**Daniel Filan** (01:59:14):
Just this question of easy-to-hard generalization where you have the right answers to easy problems, and you're trying to generalize the right answers to hard problems. My recollection of OpenAI is, they're [researching] "inaccurate to accurate grader" generalization.

**Peter Hase** (01:59:34):
This does seem like an important difference, and I think you can tell it's an important difference even based on the empirical results we've seen so far. So, if you compare some of the weak-to-strong results versus the easy-to-hard results in the OpenAI paper, I think they were also seeing that the easy-to-hard results looked better or more promising, similar to ours. So, it seemed like the models were generalizing better from cleanly-labeled easy data as opposed to noisily-labeled all of the data.

(02:00:07):
I think you can tie these two labeling approaches together in the same universal framework. So, what you suppose is that you have a labeler, and they write down soft labels for data points, but they could write down hard labels for data points. So, they might be perfectly confident in what the label is, and so they basically put probability one for something or 0.99. They might be uncertain, so they write down probabilities for what the label should be. And they're calibrated to some extent, maybe they're perfectly calibrated; we might just assume they're perfectly calibrated.

(02:00:47):
And so, what easy-to-hard looks like is: supposing the labeler can get all of the easy problems correct, and they know that they can get them correct. And they can't get the hard problems correct, and they know that they can't get the hard problems correct. And then, you sort the data based on the label probabilities.

(02:01:11):
When the labeler is confident that they don't know the answer to a hard question, they're uncertain over all the labels to the hard question. And when they're confident that they know the answer to an easy question, they are certain that one label's correct, so that distribution looks like 1, 0, 0, 0 versus 0.25, 0.25, 025, 0.25. And you sort the data based on the entropy of these distributions, based on how peaky they are. And that's how you get easy-to-hard.

(02:01:38):
So, this is the kind of labeler that you have in mind. They can do easy stuff, they can't do hard stuff, and they know what they can and can't do.

(02:01:45):
There's a smooth transition from this labeler to the weak labeler. The weak labeler just does their best on all of the data, and they know most of the easy problems and some of the medium problems and very few of the hard problems. And they might still be perfectly calibrated, but two things change.

(02:02:09):
One, the labeler changes a little bit: we're supposing they don't get all the easy problems, they just get most of them. They get a medium number of the medium problems correctly. And they get some of the hard problems correctly. They don't get none of the hard problems correctly. Maybe there's some hard and maybe there's some super hard problems where they don't get any of them.

(02:02:28):
The labeler changes a little bit. The labeler doesn't have to change, but what really changes is the sorting mechanism for getting the training dataset, where we're not using a hard cutoff anymore. We're actually going to include hard data that is just noisy labeled. And I think this is how some of the methods work in the OpenAI [paper] succeeds, is that they're leveraging some noisy label-learning approaches to be able to say, "Okay, what happens if you know that the data is noisily labeled? How could you still learn something from that?"

(02:03:04):
So, there's just this continuous spectrum in terms of which parts of the data distribution the labeler knows, how calibrated they are, and then how you decide to translate those uncertain labels into a training dataset? [If] it looks like domain shift, you're thinking about easy-to-hard domain shift, [if] it looks like noisy labels, you're thinking about noisy labels learning.

**Daniel Filan** (02:03:27):
Broadly, a thing I'm a fan of is just, now these papers are out, this is an easier distinction to notice. I might've not noticed these were different ways in which you can have easy-to-difficult generalization. And now, we can just think about, "Okay, which of these regimes are we in?" So, they're different. I think this is cool.

## Different notions of hardness <a name="different-notions-of-hardness"></a>

**Daniel Filan** (02:03:50):
And related to this, I actually want to talk about notions of hardness in your paper. In order to do this easy-to-hard generalization, you have to rank problems by how hard they are, train on the easier ones, and then do something on the harder ones, right?

**Peter Hase** (02:04:05):
Yeah.

**Daniel Filan** (02:04:06):
A really cool thing about your paper, in my opinion, is you have multiple different notions of hardness. And you talk a little bit about how they're mostly correlated, but they're not actually entirely correlated. And a thing I really want to know is: what do we know about the different types of hardness and which ones should I pay attention to?

**Peter Hase** (02:04:26):
Yeah, absolutely. And this is another big difference with the OpenAI work where... And this is not a weakness of their work, it's just they choose a more abstract approach: it's like they say, "We have some arbitrary labeler, and they have some kind of arbitrary capability, so let's just use the model as a stand-in for that." And they have a model label the data. And we take a different approach. We take a very empirical approach and just say, "Okay, what metadata do we have? What metadata can we get for hardness?"

(02:04:54):
And so, we're looking at grade level for [ARC](https://arxiv.org/abs/1803.05457), which is a science QA dataset. We have a couple other annotations that are really interesting. So there's a psychological skills scale. It's called [Bloom skills](https://en.wikipedia.org/wiki/Bloom%27s_taxonomy). It goes from one to five, where one is the simplest factual association, almost like rote memorization. And five is the most complex thing you could imagine, like analyzing a complicated argument and formulating a counter argument, and then using that to decide what the answer to a question is. So, it's like a hierarchy of reasoning skills that psychologists and educators use as they're thinking about constructing test questions; just a rote memorization test question is easier than a "synthesize a counter-argument" test question.

(02:05:50):
And then, there was one last annotation we had for the ARC data. Besides grade level and Bloom skill, we just had a 1, 2, 3 difficulty level. And I don't know where that comes from. I think some of the dataset collectors know where that comes from, but this was interesting because this is something that the educators designed as intentionally orthogonal to grade level.

(02:06:16):
You can imagine that when you're designing a test for eighth graders, you don't want all the test questions to be the same difficulty, because one thing you're doing is you're ranking students, so you want some of the questions to be easier and some of the questions to be harder.

(02:06:30):
So grade level, on its own... if you're just pulling questions from exams, the way people write exams, grade level on its own is not a perfect indicator of difficulty, because we use exams for rank-ordering students. So, there's naturally overlap in the difficulty between and across grade levels by design.

(02:06:52):
And you see this exactly in the data where this expert 1, 2, 3 difficulty gets designed as a within grade level difficulty thing. So, it just ends up being orthogonal to the grade level difficulty itself.

**Daniel Filan** (02:07:05):
Yeah. Should I think of this as: grade level difficulty is something about difficulty of just understanding the domain at all, whereas 1, 2, 3 difficulty is like, "Okay, you know the basic facts. How hard is it to reason about this thing?"

**Peter Hase** (02:07:19):
This is the end of my ability to confidently comment on these things. Those are the ones from ARC, we had grade level for [MMLU](https://arxiv.org/abs/2009.03300). The main thing we used with [GSM8K](https://arxiv.org/pdf/2110.14168) and [StrategyQA](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00370/100680/Did-Aristotle-Use-a-Laptop-A-Question-Answering) is this "number of reasoning steps". This is the measure of compositional difficulty. How many sub-problems did you have to solve on the way to solving the overall problem?

(02:07:48):
And sub-problems are nice to think about because it's just axiomatically a measure of difficulty. If there's a problem that requires seven steps versus a problem that requires six steps, and each step is itself of the same difficulty, you just know that the problem that requires seven steps is harder, because it's just one more chance to be wrong. That's one we looked at for those problems. And then, there's basic stuff like question length. Answer length was something we looked at.

(02:08:21):
Actually, we also had a model-based difficulty measurement. We didn't just use a model zero-shot label probability for sorting the data. We actually used a minimum description length-based measure, but it's a roughly similar idea. So, we had a model-based measurement too. And then, you look at how all these things correlate, and they don't correlate that strongly.

(02:08:48):
So, there's a lot of ways to read that. I mean, you can read that as data noise. I think I favor reading it as: problems vary along many dimensions. And among some dimensions, they might be harder, and along other dimensions they might be easier. Maybe you have a hard reasoning problem about very general knowledge or maybe you have a very easy reasoning problem about very specific domain knowledge. That'd be just two axes to think about.

(02:09:16):
But because problems vary along all these dimensions, it's just rare... this is not really the way we design tests. Basically, for the reason I just mentioned, we don't design tests and we don't collect datasets, or we don't collect questions about things in the world in the way that all of the most niche domain scientific questions we can ask also require a ton of reasoning and also require really complicated higher-order reasoning.

(02:09:45):
And then, all of the very basic factual association questions don't require any reasoning and are just a matter of association. And you just wouldn't expect all of these latent factors to be perfectly correlated because that's not the way we ask questions about the world.

**Daniel Filan** (02:10:01):
Sure. And I guess because these aren't correlated, it strikes me as possible that you might, after having done this paper, be able to say something like, "Oh, easy-to-strong generalization works really well when the notion of hardness is this minimum description length..." which by the way... So, I wasn't familiar with this minimum description length difficulty thing before I read your paper. We have little enough time that I don't want to go into it right now, but people should look up the paper [Rissanen Data Analysis](https://arxiv.org/abs/2103.03872) by [Ethan] [Perez](https://ethanperez.net/) et al., 2021. It's a really cool paper.

**Peter Hase** (02:10:33):
Yeah, I'd asked Ethan [Perez] for how to implement this thing because [Lena Voita](https://lena-voita.github.io/) introduces [some work](https://arxiv.org/abs/2003.12298)... basically theoretically pitches this MDL metric for measuring information content. And I read that paper and loosely understood it but had no idea what to code up, and Ethan helped me code this up. That was helpful.

**Daniel Filan** (02:10:56):
I guess it strikes me as possible that you might be able to say something like, "Oh yeah, easy-to-strong generalization, it works well when the notion of hardness is this MDL notion, but it works poorly when it's number of star or difficulty out of three," or something like that. Is there something like this that you're able to say or are they all about as good as each other?

**Peter Hase** (02:11:16):
I was hopeful to get to that point, but I don't think we got to that point. And I'm not sure we ultimately have the data to get there because we just got all the data we could, and it leads to this patchwork of we don't actually have all the variables for every dataset. So, sometimes when one of the variables changes, the domain changes, the dataset changes, other difficulty measures change all at the same time.

(02:11:41):
Once we had all the data written file, I toyed around with some regression models where we did try to tease apart what were the important factors, but I'm not really going to make any confident conclusions there, but I would think this would be great follow-up work, where you fix the domain and start varying all these individual factors, and then see how the results break down by that.

**Daniel Filan** (02:12:09):
Yeah, I think it's just almost similar to what we've learned from interpretability, it seems there's just different potential notions of what we might mean, and digging into which one is important, it seems pretty high value and maybe underrated here.

**Peter Hase** (02:12:29):
Yeah. It would be great if we could pin it down because then we could say it's a matter of how infrequent and rare this factual knowledge is when you're reading about the world or how specific this kind of thing is; that paints a very different picture compared to how difficult is it to compute the answer to this question? How long does it take to arrive at the answer to this question? Those give very different pictures.

## Easy-to-hard vs weak-to-strong, round 2 <a name="easy-to-hard-vs-weak-to-strong-2"></a>

**Daniel Filan** (02:13:00):
Before we move on, you mentioned there are a bunch of differences between [this paper](https://arxiv.org/abs/2401.06751) and [the] [weak-to-strong](https://arxiv.org/abs/2312.09390) [paper]: is there one methodological difference that you're really interested in talking about before I leave this behind?

**Peter Hase** (02:13:15):
We've definitely hit a bunch of them. We talked about the baselines, we talked about how to construct the dataset based on labeller confidence, we talked about the human hardness variables. And we talked about even the differences in the results, how positive easy-to-hard looks versus weak-to-strong.

(02:13:33):
One minor thing I would add, that I suppose is a little bit more in the criticism category, I think a few people were definitely concerned about the early stopping that seemed to be important to actually doing the fine-tuning in the weak-to-strong setup.

(02:13:51):
So, they're mostly looking at fine-tuning models. I think they did some prompting... I don't actually remember if they did prompting or ICL [in-context learning]. I think they do. I don't think they do linear probing. We also tried linear probing in addition to the other fine-tuning and prompting. But when they're doing their fine-tuning, there's a little bit of hyperparameter tuning and a little bit of dev set model selection, like early stopping, that seemed important. This is important theoretically.

(02:14:22):
So, because the idea is [that] based on incomplete supervision, the right function would still be identifiable. You don't want the right function to be one of many possible functions, and it just depends on getting exactly the right amount of fitting to the data, such that if you're underfit, you're in a bad region and if you're overfit, you're in a bad region. But if you fit exactly the right amount, you happen to uncover the right function.

(02:14:49):
One thing that empirically I can point out, we don't do much of this analysis actually in the paper, but in retrospect it feels important is that we could fine-tune as much as we wanted. And the longer the ICL prompt, usually the better. And the more data that went into the linear probe, the better.

(02:15:10):
I mean, the linear probe fits easily, but we could basically fit as much as we wanted to this clean, easy data and performance would just go up on the hard data, which is great. So, I mean, if this problem is clearly not correctly specified, is it misspecified? I don't know. We couldn't overfit to this signal. So this was something that was interesting to us, in retrospect.

## Following Peter's work <a name="following-peters-work"></a>

**Daniel Filan** (02:15:38):
Wrapping up a bit, we've talked a bit about stuff you've worked on. You've actually worked on a bunch more stuff that I didn't have time to go into. If people are interested in following your research, seeing what you've done, how should they go about doing that?

**Peter Hase** (02:15:54):
Well, you can find me on Twitter, and I think we announce basically all of our papers on Twitter, so that's a good way to stay up to date. The handle is [@peterbhase](https://twitter.com/peterbhase). But I think you'll find me easily there. And if you're really curious about reading all the PDFs, probably a Google Scholar Alerts is something I tend to enjoy for others as well.

**Daniel Filan** (02:16:14):
All right, great. Well, thanks for coming on AXRP.

**Peter Hase** (02:16:19):
Thanks so much, Daniel. What a pleasure. This was great.

**Daniel Filan** (02:16:22):
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Filming occurred at [FAR Labs](https://far.ai/labs/). Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
