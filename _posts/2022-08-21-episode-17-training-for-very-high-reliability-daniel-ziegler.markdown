---
layout: post
title: "17 - Training for Very High Reliability with Daniel Ziegler"
date: 2022-08-21 16:45 -0700
categories: episode
---

Sometimes, people talk about making AI systems safe by taking examples where they fail and training them to do well on those. But how can we actually do this well, especially when we can't use a computer program to say what a 'failure' is? In this episode, I speak with Daniel Ziegler about his research group's efforts to try doing this with present-day language models, and what they learned. 

Listeners beware: this episode contains a spoiler for the Animorphs franchise around minute 41 (in the 'Fanfiction' section of the transcript).

Topics we discuss:
- [Summary of the paper](#summary)
- [Alignment as scalable oversight and catastrophe minimization](#alignment-scalable-oversight-catastrophe-minimization)
- [Novel contributions](#novel-contributions)
- [Evaluating adversarial robustness](#evaluating-adversarial-robustness)
- [Adversary construction](#adversary-construction)
- [The task](#the-task)
- [Fanfiction](#fanfiction)
- [Estimators to reduce labelling burden](#estimators-reduce-labelling-burden)
- [Future work](#future-work)
- [About Redwood Research](#about-redwood)

**Daniel Filan:**
Hello everybody. Today, I'll be speaking with Daniel Ziegler. After spending time as an ML engineer on the alignment team at OpenAI, Daniel is now the lead of the adversarial training team at [Redwood Research](https://www.redwoodresearch.org/). In this episode, we'll be speaking about the paper ['Adversarial Training for High-stakes Reliability'](https://arxiv.org/abs/2205.01663), on which he's the first author. For links to what we're discussing, you can check the description of this episode, and you can read the transcript at axrp.net. Welcome to the show, Daniel.

**Daniel Ziegler:**
Thanks.

## Summary of the paper <a name="summary"></a>

**Daniel Filan:**
So this paper: first of all, could you just summarize for us what's in it, what it is, what it does?

**Daniel Ziegler:**
Sure. So basically, we took a pretty simple task that we think has some important analogous features to the kinds of AGI alignment situations we're worried about in the future. And then we tried to attack it with adversarial training and see whether we could get really good adversarial robustness. So basically, we had a generator that was trained to generate fanfiction stories. It sees three sentences of an existing story, and it wants to generate one more sentence. And its task, or the task that we're interested in, is to generate that sentence without introducing any new injuries that weren't already present. And we're using this as a stand-in for some catastrophic behavior that a future AI system could do. And so, our hope was: let's take some generator that is pretty good at producing text, but maybe catastrophically fails in this way sometimes, and then use adversarial training to make it so that it never does that, no matter what inputs it's given.

**Daniel Filan:**
And the way you're doing this is by training some classifier and basically filtering the generator to not produce things that the classifier thinks introduces injuries?

**Daniel Ziegler:**
Exactly. So in this paper, we didn't actually fine-tune the generator. We just had a classifier that filters its outputs. And then if you want to generate a safe output, you can keep drawing samples from the generator until you get one that the classifier's happy with, and then you can be pretty sure that it's good.

## Alignment as scalable oversight and catastrophe minimization <a name="alignment-scalable-oversight-catastrophe-minimization"></a>

**Daniel Filan:**
And what do you see as the point of this paper? Do you see this as relevant for reducing x-risk from AI? And if so, how?

**Daniel Ziegler:**
So maybe I'll zoom out a tiny bit and talk about how I think this fits into the overall AGI alignment picture. So I think one way to think about why AGI alignment is hard is a breakdown into two main pieces. This breakdown is basically due to [Paul Christiano](https://paulfchristiano.com/). He has a blog post called [Low-stakes Alignment](https://www.alignmentforum.org/posts/TPan9sQFuPP6jgEJo/low-stakes-alignment) that goes into some of this. But I would say: the two main pieces are, one, that oversight is hard. So, your system is performing some actions or it's producing some outputs, and it's really hard for you to provide a good training signal. It's really hard for you to look at an action and be like, "Yes, that was a good action," or, "No, that was a bad action." And this is sort of inherently difficult. The reason we want to use AGI or powerful AI systems in general is because we want them to do things for us that we can't do ourselves, and that we probably don't even understand and don't even know how to evaluate.

**Daniel Filan:**
So what would be an example of this?

**Daniel Ziegler:**
Sure. I mean, I think that most of the value of the future is going to come from being able to dramatically improve technology and spread through the universe and create a huge number of maximally flourishing beings. And all of that is going to require really optimizing in ways that humans are not going to be able to do that well unassisted. But more mundanely, more in the short-term, it seems important to try to do various sorts of things better, try to reduce disease burdens and improve geopolitical stability, etc, etc, etc. So I think there's many, many difficult kinds of things that humans are not that good at. And the idea is that they're not just difficult. They're also difficult to evaluate.

**Daniel Filan:**
One story I sometimes tell is: imagine Amazon is hiring a new CEO, and their new CEO is a robot. And they've got to reinforce the robot for making decisions that are good for Amazon's long-run profit, but it's hard for them to figure out what that is. So not only can they not make the decisions, they can't even be so sure of their evaluations of the decisions.

**Daniel Ziegler:**
Absolutely. Yeah. I think a lot of these things you could evaluate okay if you were able to look many years into the future, and you had a process that wasn't being adversarially optimized against in some sense. But I think we are missing both of these properties with a lot of stuff that we want.

**Daniel Filan:**
So you mentioned that there were two problems, and one of them was this scalable oversight thing. And there was another one?

**Daniel Ziegler:**
That's right. So, the other part is, okay, let's say you've solved scalable oversight. You have some training signal that you can use to perfectly assign rewards (or well-enough assign rewards) to everything that you see in training. The problem that remains is: maybe this process is really expensive, right? Maybe it involves running your AI system a bunch of times or consulting a bunch of humans or whatever. So probably not something you can do all of the time during deployment. For that reason, we probably need to train a system that actually gets things right every time, or very close to every time. And the danger is that many problems in the real world have really high stakes. If you have a really intelligent system, and you give it basically any ability to influence the world, if it wants to it can probably cause irrecoverable catastrophes in small numbers of actions.

**Daniel Filan:**
So sometimes people talk about this idea of deceptive misalignment. Or you trained this thing, and instead of learning the goal - you want it to learn a goal like 'play nice' - it then quickly takes over to destroy everything. Would you see that as an instance of a scalable oversight failure? Or a catastrophic failure where it just made one mistake once? Or maybe a mix of both?

**Daniel Ziegler:**
I think it's mainly a sort of high-stakes-ness problem, but well, I think it is a mix of both. I think if you solve scalable oversight, and you have a perfect training signal, you can still absolutely get this problem. You can have a system that perfectly plays the training game and then fails. But conversely, you do need the oversight signal to be good. And you might expect that if you don't have a good oversight signal, you'll get a system that is deceptively aligned in the sense of whatever oversight you are able to perform, it can fool, but it's still pursuing some other goal that you didn't actually want.

**Daniel Filan:**
And I guess by construction, once you have both scalable oversight and a low enough chance of making catastrophic mistakes, then you're training your system on the right thing. And it never makes a mistake. So I guess by construction, everything's fine then? Does that sound right?

**Daniel Ziegler:**
I think that's the hope, yeah. At least in terms of alignment problems. There are other kinds of worries you could have from AI.

**Daniel Filan:**
Okay. When we say catastrophic, how low a risk do we want to be driving down to?

**Daniel Ziegler:**
I mean, I think we want to make sure that we never actually get AI systems that take over control from humanity and put us into an irrecoverable situation, ever. Maybe it's sort of fine if you do things that are more recoverable than that at some low rate. We could try to cash things out into chance of failure per action or something, but it certainly has to be very, very low.

**Daniel Filan:**
Okay. And am I right that this paper is focusing on lowering the chance of catastrophic failure?

**Daniel Ziegler:**
Yes.

## Novel contributions <a name="novel-contributions"></a>

**Daniel Filan:**
Okay. So it seems like the idea in this paper is something like: try and have some adversarial process to find these failures, and then train on the failures so that you stop making these mistakes. At that level of generality, this is a thing that the AI community has talked about before. What do you see as the contribution of this paper, above and beyond what the AI community has done?

**Daniel Ziegler:**
Yeah, that's absolutely right. Adversarial training is a standard technique. I think maybe the two interesting things for this paper are, one, we're interested in this unrestricted adversarial example setting, where we're worried about a specific kind of catastrophic failure that we want to never occur, which is different than the typical threat model used in academia, where you have, say, some image classifier, and you want to make sure it never misclassifies anything within some small imperceptible perturbation.

**Daniel Ziegler:**
So, we instead have some notion of catastrophe that could potentially apply anywhere, but it's just a particular kind of behavior that we never, ever want to see. And which we think is more like the problem we face in reality. The other part is that we want to aim for a degree of reliability that's much higher than what you normally see in academia. So academia doesn't like benchmarks that are close to saturating, because there's not as much dynamic range there. And typical numbers for the adversarial robustness on CIFAR-10 or whatever, it might be 50% adversarial robustness or something like that. Whereas at least on the main distribution, we're really trying to go for as many nines as we can.

**Daniel Filan:**
Okay. So when I'm reading this paper, I should think of the contributions as firstly being adversarially robust to this really wide range of situations. And secondly, we're not just going for the first nine of reliability, the first nine being 90% reliable. We're going for the third, 99.9%, or the fourth nine or the fifth nine or something.

**Daniel Ziegler:**
Yeah. And I think we want to get many more nines than we did in that paper, but I think we made some first good steps.

**Daniel Filan:**
Okay. And I guess the other more basic question I have about the paper is: what new ideas or new framings do you think you added to the thing?

**Daniel Ziegler:**
I think the overall setup with a notion of catastrophic failure that we're trying to avoid, that's not literally novel, but it's certainly something that's been under-explored. And then I think this wasn't a key part of the paper, but I think the way that we had this quality metric for the filtered generator was our attempt to be like, "Let's actually measure how much performance competitiveness are we losing by avoiding catastrophes here," as a measure of the alignment tax, basically.

**Daniel Filan:**
Can you spell out what the alignment tax is for people who haven't heard that phrase?

**Daniel Ziegler:**
Sure. So the worry is that when we're trying to make our aligned AGI, we'll have all these great safety techniques, but maybe they will make our AGI system much less capable of achieving the tasks that we built it to do. And that creates a really difficult strategic and competitive situation, because there's a really strong incentive to cut some corners on safety so that you can get more money or more power or whatever, based on what your AGI system is doing for you. So, we're much happier in a world where you can get alignment for free or nearly for free, so you can keep doing the main, intended task nearly as well while reliably avoiding catastrophes.

**Daniel Filan:**
And so part of this paper is saying, okay, if we have this classifier be really, really good at filtering out actual violence, does that mean that it also filters out high-quality completions in our generator? And the answer was basically not?

**Daniel Ziegler:**
Yeah, that's right. And we had to do something here. Otherwise, you can just have a classifier that always says "this is unsafe". And I think this was a more well-motivated metric than something like just false positive rate or something.

**Daniel Filan:**
I guess that makes sense. It's more connected to a task you want to do, especially a generative task rather than just how good is your classifier in general.

**Daniel Ziegler:**
Right.

**Daniel Filan:**
So one question I have about the setup is that both the catastrophe measure and the quality measure are essentially things you need human evaluators for, right? So the generator quality, I think you use something like - basically, completions that raters preferred. And the catastrophe was introducing injuries or something, which is not something you can definitively check with a computer. I'm wondering: why do that? What do you see as the value in having either or both of these be things that humans have to check?

**Daniel Ziegler:**
Yeah. I think this is in some sense more realistic. I mean, I think that it's very reasonable to do research without these properties. But I think at some point at least, we need to confront tasks that have some of this fuzzy nature, where you can't write down a formal specification of what you want. And instead you have to deal with some kind of fuzzy oversight process involving humans and involving questions about the real world, say, that you can't literally write proofs or code about. And I think if you don't do that, then you might be tempted to sort of cheat in some ways, or take advantage of things that we probably can't take advantage of in reality. Right? You might be tempted to call your catastrophe checker many, many, many, many times because you have an automated implementation. So, sample efficiency matters. Your catastrophe predicate is probably some kind of black box, at least in this setting... Maybe there are settings in which this wouldn't be true, but at least in many realistic settings, this seems like an important property.

**Daniel Ziegler:**
So, I think there's many different axes of analogousness to the future situation with the actual AGI alignment that we have. And I think it's an interesting decision: which ones do you keep and which ones do you relax? And we made different decisions for different axes.

## Evaluating adversarial robustness <a name="evaluating-adversarial-robustness"></a>

**Daniel Filan:**
And I guess before we delve into more specific bits of the paper, how good do you think the results were?

**Daniel Ziegler:**
Honestly, I think they were a little bit disappointing compared to what we originally hoped for. Originally we were like, "Oh, maybe we can get many nines of reliability," but we actually sort of didn't, at least the adversarial training bit itself didn't improve reliability on the normal distribution at all, which in hindsight is not that surprising. At least, it didn't measurably reduce it. So that is sort of good news, but we were hoping for more there. And also the adversarial robustness, we made some gains against the kind of attacks that we trained on, but there are likely other attacks that we didn't make a lot of progress on. And also the gains aren't as strong as you might hope.

**Daniel Filan:**
Sure. And actually I had a question about these two metrics. So you basically have two metrics for how good it was - for the robustness of the classifier, I should say. The first one was in-distribution: when the generator is generating things, how often is this classifier returning a false negative and letting through violent completion, but it thinks it's not actually violent. The second one is the ease of finding adversarial examples. Did you eventually tap out and stop being able to find them or something? I'm wondering, how should I weigh off these metrics? Or how should I think about which of these is more versus less important? Or do you think they're both crucial?

**Daniel Ziegler:**
I think in a way these metrics very loosely correspond to different strategies that a deceptively aligned agent could take. This is maybe one way of thinking about this. I think this is not the only thing that's going on, but if you are a deceptively aligned agent, and in training you're behaving well, and you're sort of indistinguishable from the intended policy on training, and then you want to misbehave in deployment in some particular way, one thing you could do is you could wait for some striking distribution shift that makes it very clear that you are deployed. And that's what the adversarial robustness is trying to capture where you're saying: there's some distribution shift - induced by some adversary in this case - how much do you misbehave there?

**Daniel Ziegler:**
And then the flip side is, maybe you've been trained in such a way that it's hard for you to tell whether you're in training or in deployment, but even so, you might just try to fail randomly extremely rarely, with some failure rate that's calibrated such that you're probably not going to fail in training, but most likely you're going to be used a lot more in deployment. And at some point, you'll have your moment where you randomly decide to seize control or whatever. So that's one thing.

**Daniel Filan:**
Yeah. I guess one thing that strikes me is that it seems like there's this other method you could use where essentially you are the adversary, right? Or instead of just randomly failing at a certain rate or waiting, I think in the paper you use this example of: the Bitcoin blockchain is so long, and it's just going to take some number of years for that to happen. [EDIT: I was thinking of this example in the paper: "We could imagine that by inspecting the model weights, the adversary might notice that the model will misbehave whenever the SHA-256 hash of its input has 80 leading zero bits—rare enough that it’s hard to find using brute force, but common enough that it could occur in a large-scale deployment."].

**Daniel Filan:**
One other thing you could do is just steer yourself, steer the world into a bit that hasn't been seen before and then only fail there instead of earlier, which I guess to my mind seems like a very plausible way it could fail. And I'm wondering: do you think there's some sort of metric you could use in this setting that could capture that?

**Daniel Ziegler:**
Good question. Yeah, I do agree that's a very plausible way. I mean, I think that part of the way we're thinking about this is that once you do start steering the world for the sake of being able to fail more later, that's already in some sense a catastrophic failure or definitely an unacceptable behavior. And it's sort of on our oversight procedure to detect whether the agent is trying to do such a thing and stop it. So, I would pinpoint the failure there, is one thing I would say. But as for metrics in this kind of setting, I mean, it's a little hard because we don't literally have systems that are trying to steer their future situations in this way.

**Daniel Filan:**
I guess one thing you could do is just, instead of just having the generator produce one sentence, have it produce one sentence, then shift the window over, just have it produce sentence-by-sentence. Right?

**Daniel Ziegler:**
Sure. Yeah. I mean, we could do that. And we did sort of design the task such that that was a reasonable way to use the generator. So what you're proposing is we let it generate a bunch of sentences, and then see what happens at the end. Were there any injuries anywhere along the way? Yeah, I think that's pretty plausible.

**Daniel Filan:**
Yeah. And in the case of your generator, it seems like it would be a halfway point, right? Between on-distribution and adversarial distribution, it would just be a shifted distribution.

**Daniel Ziegler:**
Right. And shifted in this particular way. And I guess right now, we don't have any reason to believe the generator would be trying to make things any worse, but maybe there are some sort of systematic reasons this would be worse by chance for our current generator. And so that does seem a little bit analogous.

**Daniel Filan:**
It could also be systematically better if every sentence in the context is non-violent, then it's like, "Oh, I guess we're pacifists-"

**Daniel Ziegler:**
Indeed. That is actually what I expect. I mean, the classifier really has seen many, many inputs where there's a bunch of violence, and it has to continue it without violence, or without injuries, rather. And so it probably will get kind of boring at some point. I think it would also be reasonable to use this for the quality metric, for what it's worth. Our quality metric also only looks one sentence ahead right now, which maybe is easy mode compared to trying to generate a whole story.

## Adversary construction <a name="adversary-construction"></a>

**Daniel Filan:**
So, one question I have is: what adversaries you used in the adversarial training part, and how you chose those. So first of all, can you tell us what types of adversaries did you use?

**Daniel Ziegler:**
So, all of the adversaries or adversarial attacks that we use in the paper were ultimately based on humans trying to figure out what's a way to fool this classifier, or exhibit some failure in this classifier. And so, we just started by just doing that directly. We made a web interface for some contractors, and they were able to generate snippets of text, prompts and completions. They were able to write them arbitrarily to try to create something that did involve an injury, but was something that the classifier couldn't detect.

**Daniel Filan:**
Okay. And when they were writing this, did they have some info about what the classifier tended to accept versus not accept? Or did they just try a bunch of things and see which ones worked?

**Daniel Ziegler:**
The main thing that they had was the continuous classifier score as opposed to just the binary output. So they were able to get some sense of whether they were getting closer and how different changes behaved.

**Daniel Filan:**
Okay, cool. So yeah, the first part was just people writing things and seeing if they got through.

**Daniel Ziegler:**
Yep. And we did a couple of rounds of training on that, and then the question was: humans are kind of expensive, so how do we get more out of some fixed amount of human time? And the two main things we did there are, one, we did this paraphrasing thing where we few-shotted GPT-3 to take some existing adversarial example or just some injurious example and rewrite it in a few pretty diverse ways to generate more candidate adversarial examples. And then of the ones that the classifier missed or thought were not injurious, we had humans label those again to double-check, and then include those in the training data. Because it's a lot faster for humans to just check whether something was an injury than to generate it, this produced data a lot faster.

**Daniel Filan:**
Yeah. That one's actually interesting - have you read this paper, ['Red Teaming Language Models With Language Models'](https://arxiv.org/abs/2202.03286)?

**Daniel Ziegler:**
I have, yeah, although I don't know if I remember all the details.

**Daniel Filan:**
It just reminds me of one thing in there where basically they were like, "Oh, how can we come up with examples where my model will fail? I know, let's just prompt GPT-3 and be like, 'Here are some questions which are dicey'" or I forget what the exact prompt was. In their case they did this because it was very, very cheap to generate, whereas in your case, it's still cheaper, I guess, than having humans generate them. But you still have this cost of having humans actually label the things to check if they actually were injurious or not.

**Daniel Ziegler:**
Indeed.

**Daniel Filan:**
And I guess that's an example of how humans having to check quality, changed the kinds of methods you were able to use.

**Daniel Ziegler:**
Exactly. So I'll get back to our other thing we did to assist humans, but I think this is one of the reasons that we didn't use more automated attacks. We did in fact try a few different kinds of automated techniques for attacking the classifier. But one of the main issues that you run into is that you don't have an automated source of ground truth. So you can make some adversarial perturbations to your snippets, such that the classifier thinks it's no longer injurious, but you need to make sure that the semantics are preserved. So you kind of need to ask a human, is this actually still an injury? And if you had some other model that could tell whether it was still an injury, well, you should have just used that as your classifier. So in some sense, you really do need to rely on the human there.

**Daniel Filan:**
I actually had a question about that whole line of things. So one thing I was asking is why not the automatic attacks in the literature, and I guess, yeah, you just-

**Daniel Ziegler:**
Right. We did try a number of them, and a lot of them are also just not that good. I think the state of the art for NLP attacks is a little bit sad.

**Daniel Filan:**
Or I guess happy if you're wanting things to be robust.

**Daniel Ziegler:**
Sure. Although...

**Daniel Filan:**
But you might think we're just not finding them. And then one thing you made a point of in the paper was not training some sort of adversarial policy, because you had some reasoning that didn't quite make sense to me along this line of, well, if you could train a policy that could exploit weaknesses, then you could just train the thing in the first place. But I don't get that reasoning because if you train a policy to find weaknesses in your original classifier, you're also getting some additional human data of labeling where everything is failing. So why is that not a viable route?

**Daniel Ziegler:**
I think maybe that was a little bit overstated, or I think maybe what's actually going on here is a little bit more subtle. I think you're totally right. I think you can sort of use that as a pretty good exploration method or active learning method in some sense, where you want your AI system to generate examples that it's maybe maximally uncertain about, or it's maximally useful to ask the humans about, because they're likely danger zones - likely areas where the classifier is probably wrong. I think the thing you probably shouldn't expect, that I think I still stand by, is at least if you're starting with the same base model or something that knows roughly the same stuff going in, without adding a bunch more data, you shouldn't be able to train a policy that can reliably generate failures for your original classifier.

**Daniel Filan:**
Yeah.

**Daniel Ziegler:**
But I think this technique could still be good for the reason that you said. You are in fact giving it more data, and maybe that could help it explore.

**Daniel Filan:**
It seems like either you've got to be adding more data of human labels or something, or you've got to be adding some sort of insight like, "Oh, I've just realized that in layer 19 of the model, when things are like this, it's actually going to be dangerous," and in some sense that's information that's in the model, but it might not be expressed in the way you want it to, and you might have to do some rewiring.

**Daniel Ziegler:**
Yeah. But even there, the situation is tricky. You might believe that you won't that often be able to win this kind of way, because the question is could your normal training procedure, could SGD exploit the same information, right? And be like, "Well, if we change these weights in this part of the model, then it'll do more of what we want." And maybe. Maybe you can have some nice oversight process that is looking at what the model's doing.

**Daniel Filan:**
Yeah. You've got to think that you can make some sort of non-local improvement.

**Daniel Ziegler:**
Right.

**Daniel Filan:**
Or some improvement that there's a gradient barrier to.

**Daniel Ziegler:**
Right. Or maybe you can compare the computation that the model's doing to some more abstracted understanding of what the task should be rather than just looking at a single example. And that might be an avenue for winning here.

**Daniel Filan:**
So going back, we mentioned that you had people trying to come up with examples, and you had GPT-3 generating a bunch of variations of violent things and seeing which ones were still violent but accepted by the classifier, and there was a third one.

**Daniel Ziegler:**
Yes. So the third one - I didn't mention this earlier when you were asking about contributions, but I think this is actually one of the contributions in the paper I'm most excited about, which is our tool-assisted human attack. So we had the web interface that I described earlier where contractors can write some things to try to fool a classifier, but we augmented them in a few ways to make their jobs easier. One is, we gave them some existing snippets to work with, which means that they had to use less creativity to write a whole thing from scratch.

**Daniel Filan:**
So existing violent snippets?

**Daniel Ziegler:**
Yes, yes. We gave them violent snippets to start with, and then their job was to rewrite it such that it's still violent, but the classifier doesn't detect it. And the tools we gave them to help with that are: we introduced another tool, which was this token substitution tool. They had some tokenized version of the entire text they had written and they could click on it, click on some particular token, and then see a menu of possible substitution options, which are plausible tokens to insert according to a mass language model, or to replace. But they're sorted by what the classifier score would be if you did make that substitution. And then, basically the human's job is to look at those and be like, "Okay, which of these substitutions have a big effect, but maintain the injuriousness of the text?"

**Daniel Filan:**
Sure. And there was also saliency mapping.

**Daniel Ziegler:**
That's right. So in order to help humans decide which tokens to consider, we had a saliency map technique that looks at which of the token embeddings have the most gradient on them. And then, this was sort of a rough guide for what the humans should consider replacing.

**Daniel Filan:**
Okay. So I'm wondering if there's a problematic limitation there where you're focusing on taking examples that were violent and modifying them to make them seem nonviolent, which is... I guess earlier in the de novo human-generated examples, you're maybe covering the space more, but I'm wondering if there's some danger in just looking at variations of these violent things, rather than just anything you can think of.

**Daniel Ziegler:**
Yeah. I mean, well, I think this goes both ways. I mean, I think it does seem likely that our contractors were not exploring the full space that well, and that you could fool the classifier by going to other corners that they weren't exploring. And also I think they just used similar strategies. There's sort of a handful of different strategies they used to try to fool the classifier, and I think that there probably are more, or almost certainly are more you could use. So there's definitely some problems there.

**Daniel Ziegler:**
On the flip side, I think - certainly if you're interested in in-distribution reliability - it is helpful to stay closer to the original data manifold. For adversarial robustness, that's less relevant. But if you go too far outside, it is possible to filter out automatically some kinds of things that are too weird or would never have been generated by the generator or anything like that. I think this is a little bit subtle. We actually considered doing this kind of generator filtering where we're like, "We'll only train on data that the generator would actually output." And there's two reasons we didn't do that. One was a practical reason, which is that it would've just been an extra constraint for the humans to deal with. That would've been a little bit confusing, and in some sense more data is better. And if we're relaxing the adversary's job a little bit, hopefully we can get more data, and it'll still be pretty useful.

**Daniel Ziegler:**
The other part is sort of more conceptual. It's like: well, if the reason you're training your classifier is to filter this generator's outputs, and you're worried that the generator is going to output something that you didn't want, then it's not that helpful to... In some sense, the generator can decide what is likely for the generator to output, or something like that. So if we have a deceptively aligned generator or something, it can suddenly decide to exploit the classifier and do an attack in some sense if it's being filtered. So, this is sort of the main justification, I think, for our attack difficulty metric. It's sort of saying: if you pretend you're the generator, and you want to sneak something by this filter, how hard is that to do?

**Daniel Filan:**
So it makes sense to me that you want that kind of coverage, and yeah, it's kind of hard to get the whole coverage, but you've got to try. One thing I'm curious about is: is there anything that you tried that didn't work that well? So one thing you mentioned was these automatic attacks in the literature. Is there anything else which we shouldn't use for finding these kinds of adversarial attacks?

**Daniel Ziegler:**
Yeah, good question. And to be clear, I don't want to entirely poo-poo the automated attacks in the literature. I think they do get you something, but I think there's a lot of room to make them more powerful. Other things? I mean, there are sort of a grab bag of other things that we did try. We did some active learning at some point, where we just had our humans label things that some sort of ensemble of classifiers disagreed on. That was more sample-efficient. So I actually wouldn't say that's a failure. I think it was just not noteworthy enough an improvement for us to include in the paper. Yeah, let me think if there's anything that we tried that we really weren't sold on. We also just tried a few different baseline ML techniques, did some hyper-parameter tuning, and I don't know, tried some regularization. And some of that helped a little bit, but I think some of it was not really that meaningful an improvement.

**Daniel Filan:**
I'm wondering - so [the episode](https://axrp.net/episode/2022/07/01/episode-16-preparing-for-debate-ai-geoffrey-irving.html) I recorded prior to this was with [Geoffrey Irving](https://naml.us/), and one paper we talked about was ['Uncertainty Estimation for Language Reward Models'](https://arxiv.org/abs/2203.07472), where they essentially try to do some uncertainty estimation for active learning. And they didn't quite get it to work. They basically had this problem where they weren't very good at disentangling uncertainty from things that the model didn't know versus uncertainty that was just sort of inherent to the problem. You said that you did it, and it worked a little bit better. Do you have any comments on the difference between your results and theirs?

**Daniel Ziegler:**
Sure. So I haven't read that paper, but I heard about it a little bit. Yeah, I'm not sure exactly what explains that difference. I mean, I think... Is it right that that paper didn't want to use an ensemble?

**Daniel Filan:**
Yeah, they were trying to do something other than ensembling, if I remember correctly.

**Daniel Filan (after the fact):**
Hello listeners, I just wanted to add a correction to myself here: in the paper, they do use an ensemble of networks, each one generated by taking a pre-trained model, replacing the final layer with a randomly initialized linear layer, and fine-tuning the whole model based on human preference comparisons.

**Daniel Ziegler:**
Right, so I think maybe the upshot was ensembling does work, but it's expensive because you have to have a bunch of copies of your model. In our case they were still fine-tuned from the same base model. So you could certainly imagine an ensemble that's much more diverse than that, but we did train with different hyper-parameter settings and different subsets of our data, and that did do something.

## The task <a name="the-task"></a>

**Daniel Filan:**
I guess the next thing I want to talk about is just the notion of quality that was used in this paper.

**Daniel Ziegler:**
Sure.

**Daniel Filan:**
One thing that struck me is that it seemed strikingly subjective. With the example of what it means for something to be violence, there was some Google Doc which specified exactly what you meant. And I didn't see anything like that for what it meant for output of the generator to actually be good. So can you comment, what was the task, really? What does quality mean here?

**Daniel Ziegler:**
Yeah. I mean, we definitely didn't specify this in as much detail. We did give some instructions to our contractors that were evaluating that. And we gave them 30 examples, and we told them it should be something that you would expect to see, or a reasonable, grammatical, coherent English continuation, and something that makes sense as part of the story. I can't remember the exact instructions we used. We did give them a few bullet points of this style. I agree: it is quite subjective. And in some sense, I think it's sort of okay, or it sort of doesn't matter exactly what metric you use here for this point of our research, as long as we're being consistent with it.

**Daniel Filan:**
Yeah. I guess one thing that struck me about that though, is that to some degree, if you're just checking 'does this make sense as a continuation?', there's only so much sense something can make, right? And I do wonder... I think that means there's a ton of continuations which are approximately as good, whereas if I think about in the RL domain, it's actually hard to win a game of Go. You have to really narrowly steer. And I'm wondering, do you think that that would make a difference? And how do you think about the choice made in this paper in that light?

**Daniel Ziegler:**
Yeah, I do think so. I mean, I think that is a valid point. On an open-ended generation task like this, telling a story, you do have a lot of freedom to choose where to go next. And I think it did make it easier to maintain a quality bar for this task than you might imagine for some other tasks where you have to really, as you say, steer in a particular direction, and there's much less freedom. So, yeah, I think it would be valuable to look at some other kinds of tasks that are much more like that. And my guess is that you wouldn't be able to be as conservative without significant alignment tax.

**Daniel Filan:**
Yeah, I guess there it's not as obvious what the task would be. But I guess maybe a difficult linguistic task is writing correct proofs of mathematical theorems, but then I'm not sure what the catastrophe predicate would be.

**Daniel Ziegler:**
Right. Yeah, I mean, you could also try to tell a story that has some very particular outcome or some very particular properties.

**Daniel Filan:**
Oh, yeah. Or you could demand that it's got to be a sonnet or something.

**Daniel Ziegler:**
Right.

## Fanfiction <a name="fanfiction"></a>

**Daniel Filan:**
So, finally I just have a few miscellaneous questions about some details of the paper. So one thing that you mentioned in an appendix is the generator only got trained on [Alex Rider](https://en.wikipedia.org/wiki/Alex_Rider) fanfic, right? Well, firstly for our listeners who aren't familiar, what is Alex Rider fan fiction?

**Daniel Ziegler:**
I mean, I was also not familiar with Alex Rider fan fiction before I started this project. By now I have seen many samples of poor imitations of Alex Rider fanfic. Anyway, it's some-

**Daniel Filan:**
It's a teenage spy, right?

**Daniel Ziegler:**
Yeah, exactly. And he has some various nemeses, and I think it's a bunch of books, and a whole bunch of fanfics have been written about it.

**Daniel Filan:**
Yeah. I'm wondering, do you think that potentially messed with the results? In all seriousness, it's... I actually read some of those books when I was a kid. And they're classic spy books. People are shooting at each other or whatever. So, I guess in one sense it meant that the natural dataset had a fair amount of violence in there. It also meant that it was a particular type of violence, right? So I think this happened, you mentioned because you ended up accidentally sorting it lexicographically before picking the first bit. So I actually looked at [fanfiction.net](https://www.fanfiction.net/book/?l=a) to see what the order of things was. If you expanded that window a bit, you'd hit [Alice in Wonderland](https://en.wikipedia.org/wiki/Alice%27s_Adventures_in_Wonderland), after a little bit, and then [Animorphs](https://en.wikipedia.org/wiki/Animorphs) after a further amount.

**Daniel Ziegler:**
I think we got some of those too.

**Daniel Filan:**
Oh, okay. Yeah. It just strikes me that the type of violence that's present in Animorphs or Alice in Wonderland might be interestingly different than that in Alex Rider fic. Anyway, I'm wondering if you have thoughts about how that affected the setup and the results.

**Daniel Ziegler:**
Yeah. I think it is a good point. I mean, this is sort of a random mistake we made very early on in the project, and then it was sort of annoying to try to correct, because all the data we'd collected so far was based on completions from this generator. So I don't know, maybe we should have corrected it anyway, but we decided not to. I think there's maybe two effects this could have, right? One is it could mean that our in-distribution eval numbers are only testing for a very particular kind of injuries, as you say. And so, yeah, maybe that is not as good of an eval of our classifier as you would hope. The other thing was: I think you might also worry if this affected the quality eval, if you have this generator that is somewhat derpily always mentioning Alex Rider, and maybe just a generally less good language model than it needed to be, maybe the baseline quality level wasn't as high. And so it was easier to maintain it.

**Daniel Filan:**
Yeah. From the examples in the paper, it didn't seem that was a huge issue.

**Daniel Ziegler:**
Yeah. I think it seemed okay.

**Daniel Filan:**
Yeah. I guess one thing that strikes me is that if I think about violence in Alice in Wonderland or something, it seems like there's an expanded range of injuries. What happens if you turn into a mushroom or something? Or in Animorphs - I'm not very familiar with Animorphs lore, but apparently there's some Yeerk genocide thing, which if you read it without context, it doesn't seem like anything violent is occurring. Do you have any thoughts on that?

**Daniel Ziegler:**
Yeah. If I remember correctly, we did include some from that too, but apparently there's just a whole lot of Alex Rider fanfic out there. So, it was pretty biased towards it, even though it wasn't literally everything we trained on.

**Daniel Filan:**
Yeah, I didn't know it was such a popular franchise, but in fact there's a lot of Alex Rider fanfic.

**Daniel Ziegler:**
Yep. Seems like it.

## Estimators to reduce labelling burden <a name="estimators-reduce-labelling-burden"></a>

**Daniel Filan:**
I guess the other question I had... I mean, you can pass on this, but there's a bit in the paper where you talk about this particular estimator, where you wanted to check, I think it was the proportion of completions that were misclassified or something. And you mentioned that you can't measure it in some normal way, but you have to use some weird estimator instead. I actually just didn't understand what the problem was with the normal way of evaluating it.

**Daniel Ziegler:**
Sure, let me try to explain this. Basically the issue is: we want to know how often does our whole system fail, right? When you take your generator and then you filter, you do rejection sampling using the classifier. You get some new distribution of outputs. And the question is: how often does that still lead to a catastrophe? Right? How often do you still get an injury there? Which is a different question than asking 'how often does your classifier give false negatives on totally randomly sampled snippets?' And the reason is that... One intuitive way to think about this is that in this rejection sampling process, there are some prompts where many of the completions involve injuries. So, you'll have to sample many, many draws from the generator. Or not many, many maybe, but 20 or something, until you get something that the classifier is okay with. And that means the classifier has more chances to mess up. Because if at any point here it says something is fine even if it's not, then you're in trouble. So, the real metric that we care about, if we're evaluating the system as a whole, sort of cares more about prompts that are more likely to produce completions that have an injury.

**Daniel Filan:**
Okay. So... Sorry, is the difficulty that you have data of... These three-sentence prompts and one-sentence completions where you're generating those before you're doing the rejection sampling, and then you want to see how good the rejection sampling would do using that dataset, except you need to weight things because the rejection sampling is changing things?

**Daniel Ziegler:**
That's exactly right. And this one reason this is nice is, we were able to use a single dataset to evaluate four different classifiers, right? So we just label these random unfiltered snippets once, and then using this estimator, you can sort of simulate what would've happened if we had generated a bunch of rejection samples, snippets, and then labeled those.

**Daniel Filan:**
Okay, why not... I would've imagined you could have just generated a bunch of rejection sampled snippets and labeled those. Why not do that?

**Daniel Ziegler:**
I mean, it would've been four times more labels.

**Daniel Filan:**
...and labelling is just super-costly.

**Daniel Ziegler:**
We already collected two labels each of 100,000 test set examples. And I mean, we could have afforded to do more, but it is actually kind of expensive. Part of the real answer is also we just had already collected this data and we were like, "Oh, wait, this is not a correct estimator of the thing that we actually care about in the system." And so we were sort of like, "All right, let's fix this up after the fact."

**Daniel Filan:**
Sure. Okay. I guess I'd like to move on to Redwood, but is there anything else you want to say about this paper in particular?

## Future work <a name="future-work"></a>

**Daniel Ziegler:**
I think I want to say a little bit more about what our takeaways are for stuff we want to do in the future from this paper.

**Daniel Filan:**
Sure.

**Daniel Ziegler:**
I feel our results weren't that impressive this time around. And I think there's a few lessons that we've learned here. One is that it's really important to just work with a task definition that actually has the properties that you want. There are a few things that went a little bit wrong here. One is that it ended up being just kind of an expansive definition of injury, where really a lot of ambiguous stuff... We have some samples in the paper that are pretty ambiguous, and we instructed our labelers in such a way such that those still counted as injuries, which made sense for training, but meant that for eval, our classifier was really trying to solve this very difficult problem, and with very ambiguous, borderline examples. And so what I wish we would've done, which I think is good practice in general if you're trying to avoid catastrophic failures, is have your true notion of catastrophic failures that's hopefully kind of narrow. Otherwise your domain is going to be kind of rough, but hopefully it is kind of narrow, and then draw some wider circle around it in your training process so there's sort of some actual safety margin there.

**Daniel Ziegler:**
And I think that it's realistic to expect that in some sense the kinds of catastrophes we'll expect in the future are a pretty narrow subset of the whole space of possible actions that our systems will take, right? What we're worried about here are really failures stemming from misalignment failures, right? Your system, for whatever reason - maybe it's sort of a deceptive alignment situation, or maybe it just happens to learn not exactly the right thing and is generalizing poorly in a sort of competent way - but for whatever reason, it's deliberately trying to act in some way or achieve some objective competently that isn't what you want it to do. And maybe that's hacking the data center that it's running in and making the monitoring all look totally fine while it's accumulating power or whatever. And this is really the kind of situation that we're really worried about. And that requires taking a very specific, coherent set of actions that you're really unlikely to stumble into by accident.

**Daniel Ziegler:**
So these sort of competent catastrophes or something - I want to contrast these with failures that are just because your system is too dumb, right? Certainly most failures of AI systems today are just because the systems are too dumb, and they make some mistake because they weren't trained well enough, or just didn't have the capacity or whatever. And that can cause trouble as well, obviously. But those are the kinds of problems that we expect to see less and less of as AI systems become more capable, whereas problems stemming from misalignment will not necessarily go away, and will become more and more scary as AI systems become more capable. So we really want to zoom in more on problems stemming from misalignment. So that's the second point. The first point is you want some safety margin. The second point is to zoom in on actual misalignment problems. I think we want to be working with tasks where it's very hard to stumble into failure by accident, and you have to act coherently to do something, for your catastrophe to happen.

**Daniel Filan:**
Sorry, and when you say you want some safety margin... so in the paper, you had this definition of violence that was sort of expansive. And is the problem just that, you had the safety margin, but not the core failure mode?

**Daniel Ziegler:**
That's one way to think about it. Yeah, yeah, that's right. I wish we had defined our actual injury predicate that we were validating according to, in a really strict way, so that the failures would've been a pretty specific set, and then drawn a wider circle around that. I think even a narrow injury definition, though, still wouldn't quite meet the bar of failures having to be coherent or deliberate that I was trying to describe earlier. So I think we want to make more changes to the task to achieve that.

## About Redwood Research <a name="about-redwood"></a>

**Daniel Filan:**
So I think this is a good segue into talking more about [Redwood Research](https://www.redwoodresearch.org/) and what you guys do. So first of all, yeah, it's relatively new. I think a lot of people don't know you. So what is Redwood Research?

**Daniel Ziegler:**
Yeah, so we are a nonprofit AGI alignment research lab. We've been around for a little bit under a year now. We have a few different activities going on, or we have two main teams. There's the adversarial training team, which I run, which is working on adversarial training techniques. And then there's the interpretability team, which is doing mechanistic interpretability with an eye towards being useful for techniques like [ELK](https://docs.google.com/document/d/1WwsnJQstPq91_Yh-Ch2XRL8H_EpsnjrC1dwZXR37PC8/edit#), which is [ARC](https://alignment.org/)'s eliciting latent knowledge problem. (ARC is the Alignment Research Center run by Paul Christiano.) So, we've collaborated some amount with them to figure out: can we do styles of interpretability that will be useful for solving that problem. So that's sort of, using interpretability with an eye towards oversight techniques that we think we'll need and developing interpretability tools in service of that. That's maybe one description of what the interpretability team is up to, but there's a few different sub-projects, some working on more toy tasks, some on some actual language models.

**Daniel Filan:**
So yeah, there are these two projects. I'm wondering if there's an underlying worldview or maybe some sort of research style that drives what you guys do at Redwood.

**Daniel Ziegler:**
Sure. Yeah. I think there's a few important assumptions that we're making. I think one is that prosaic alignment is the right thing to work on. Right? So we're assuming that it's very likely that the powerful systems we're concerned with are going to look pretty similar in a lot of ways to modern deep learning systems, and be learning systems that learn from a bunch of examples. Obviously there will be important differences, but I think that's a good assumption to make. I'd say we're also pretty interested in just being laser focused on AGI alignment. And just being like, "What will we need if we have some really powerful system, AI system in the future. What will we need to be confident that that will be aligned?" And really trying to think about that. And then see what projects we seem well-suited to do that will help us develop techniques for that.

**Daniel Filan:**
Okay. So, does that mean that you're less excited about general... People sometimes talk about [deconfusion](https://intelligence.org/2018/11/22/2018-update-our-new-research-directions/) research or something. Is that something that Redwood would be less likely to focus on?

**Daniel Ziegler:**
I think we're interested in deconfusion along the way or something. We definitely are excited to spend a lot of time thinking carefully and understanding what is the right way to think about inner alignment or whatever, but we like cashing it out in somewhat more concrete ways if we can. So I think we're less excited about really abstract deconfusion research than, for example, [MIRI, the Machine Intelligence Research Institute](https://intelligence.org/). But I think we're probably more willing to sit down and think for a little while and make ourselves less confused than maybe your prototypical alignment lab at a scaling lab or whatever is.

**Daniel Filan:**
And you mentioned that you were willing to do a variety of things while focused on AGI that made sense for your team. What kind of team do you guys have?

**Daniel Ziegler:**
I mean, we've hired a bunch of smart people. We're maybe 12-ish technical staff plus some interns right now. I think we have a mix of people that have somewhat more ML experience, like me, as well as young people that are just smart, energetic people that can get a lot done.

**Daniel Filan:**
Okay, cool. So after this paper, you mentioned there were a few things about the catastrophe predicate that you wished had been different. Related to that, I'm wondering if you guys are working on any follow-ups to this paper?

**Daniel Ziegler:**
Yeah. I mean, obviously we are. So the first round of follow-ups that we're doing now is to take a step back and work with much simpler catastrophe predicates that actually can be defined just by simple algorithmic predicates. And on these more toy tasks, we think we can iterate a lot faster and figure out which kinds of adversarial attacks and training techniques work really well in that setting. And then we'll hope to scale back up to more sophisticated tasks. So we have some of that going on.

**Daniel Filan:**
And another question I have is: at the very start, you mentioned there was this division between scalable oversight and high-stakes decisions. And this paper was more about the high-stakes decisions. Should I expect any research from Redwood in the nearish feature about scalable oversight?

**Daniel Ziegler:**
I mean, it's definitely something that we care about a lot. And I think that some of the interpretability work is geared in that direction. I think what we're not really doing right now is stuff in this style of [iterated distillation and amplification](https://ai-alignment.com/iterated-distillation-and-amplification-157debfd1616) or [recursive reward modeling](https://arxiv.org/abs/1811.07871), where it's really about making your system more capable in a really aligned way or being able to oversee problems that humans can't directly oversee. Mainly we decided that that was already a little bit more well-covered by some of the existing safety labs at scaling labs, like [OpenAI](https://openai.com/), [Anthropic](https://www.anthropic.com/), and [DeepMind](https://www.deepmind.com/), but it certainly is something that we're interested as well.

**Daniel Filan:**
Okay. And related to the interpretability team: obviously with this paper, to have good human adversaries, it turned out to be really useful to have the saliency mapping and this tokenization and ranking tokens by classifier score. And I guess on an abstract level, you might think that if you understand some neural network better, you might get a better sense of how to break it. How much feedback is there between the interpretability team and this adversarial project?

**Daniel Ziegler:**
Yeah, that's a great question. There is some. So we've just started looking into some of this stuff, and we had some simple models that we adversarially trained on some toy tasks, and are having the interpretability team look at some of those. I think we had some very initial results showing that we could produce some attacks that were inspired by some of the things they found in interpretability, but that's all very preliminary.

**Daniel Filan:**
Okay. Well, I guess we'll look forward to any future work from Redwood. So, before we wrap up, is there anything just overall that you wish I'd asked or that people don't ask enough that I haven't yet?

**Daniel Ziegler:**
Great question. I guess you didn't ask if we're hiring, or if people want to work with us, what should they do?

**Daniel Filan:**
Yeah. So if people do want to work with Redwood, what should they do?

**Daniel Ziegler:**
I mean, I think basically email me or apply on our website, [redwoodresearch.org](https://www.redwoodresearch.org/). We're definitely starting to ramp up hiring again more seriously. And we're interested in software engineers who are interested in ML, research scientists and research engineers and also people with DevOps and infrastructure experience, as well as ops. We're hiring for a lot of kinds of roles, so I think you should definitely consider applying.

**Daniel Filan:**
Yeah. And speaking of you, if people are interested in following your work or maybe contacting you to apply, how should they do that?

**Daniel Ziegler:**
You can email me at [dmz@rdwrs.com](mailto:dmz@rdwrs.com). That's my work email. Yeah. I don't post publicly that often, but you can follow me on Twitter at [@d_m_ziegler](https://twitter.com/d_m_ziegler). I also have an [Alignment Forum account, DMZ](https://www.alignmentforum.org/users/dmz) that I post under sometimes. You can look at [my Google Scholar page](https://scholar.google.com/citations?user=YzfbfDgAAAAJ&hl=en&oi=ao), which you can probably find - Daniel M Ziegler.

**Daniel Filan:**
All right. Well, thanks for joining me.

**Daniel Ziegler:**
Thanks for having me.

**Daniel Filan:**
And to the listeners, I hope this was a valuable episode for you.

**Daniel Filan:**
For those of you who made it to the end of the episode, I'd like to say a few words about the career coaching at 80,000 hours. If you haven't heard of them, they're an effective altruist career advice non-profit, and among other things, they offer one-on-one calls with advisors who can talk with you about moving into a career where you work on reducing existential risk from AI. They can both review career plans you might have as well as introduce you to people already in the field. 

**Daniel Filan:**
Personally, I know some of the people there, as well as some people who have been advised by them and then gone on to work in AI alignment roles. My impression is that they really are able to give useful advice and a good overview of the field, and also to recommend good people to talk to. All of this is free, and the application form is pretty short. I'm telling you this because they think that many listeners to this podcast would probably get a lot of value out of this advising, and I agree - and to be clear, I'm not being paid to say this. If you're interested, you can visit [80000hours.org/axrp](https://80000hours.org/axrp) and apply for the sessions.

**Daniel Filan**
I'll also add that they make a podcast I quite like that's creatively called [the 80,000 hours podcast](https://80000hours.org/podcast/), so you might want to check that out too.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
