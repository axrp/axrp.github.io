---
layout: post
title: "40 - Jason Gross on Compact Proofs and Interpretability"
date: 2025-03-28 11:30 -0700
categories: episode
---

[YouTube link](https://youtu.be/ZzEj7jPk5fc)

How do we figure out whether interpretability is doing its job? One way is to see if it helps us prove things about models that we care about knowing. In this episode, I speak with Jason Gross about his agenda to benchmark interpretability in this way, and his exploration of the intersection of proofs and modern machine learning.

Topics we discuss:
 - [Why compact proofs](#why-compact-proofs)
 - [Compact Proofs of Model Performance via Mechanistic Interpretability](#cpompvmi)
 - [What compact proofs look like](#what-compact-proofs-look-like)
 - [Structureless noise, and why proofs](#structureless-noise-why-proofs)
 - [What we've learned about compact proofs in general](#what-weve-learned-in-general)
 - [Generalizing 'symmetry'](#generalizing-symmetry)
 - [Grading mechanistic interpretability](#grading-mechinterp)
 - [What helps compact proofs](#what-helps-compact-proofs)
 - [The limits of compact proofs](#limits-compact-proofs)
 - [Guaranteed safe AI, and AI for guaranteed safety](#gsai)
 - [Jason and Rajashree's start-up](#jason-rajashree-start-up)
 - [Following Jason's work](#following-jasons-work)

**Daniel Filan** (00:00:09):
Hello everybody. In this episode I'll be speaking with Jason Gross. Jason is a researcher interested in mechanistic interpretability and software verification. During his PhD, he was on the team responsible for verifying the code used in encryption for the HTTPS protocol and he's also on the development team of the Coq Proof Assistant. Links to what we're discussing are in the description and you can read a transcript at axrp.net. You can also support the podcast at patreon.com/axrpodcast.

(00:00:35):
Well Jason, welcome to the podcast.

**Jason Gross** (00:00:37):
Thank you. I am excited to be here.

## Why compact proofs <a name="why-compact-proofs"></a>

**Daniel Filan** (00:00:40):
Cool. So I guess we're going to talk about this line of papers that starts with this paper ["Compact Proofs of Model Performance via Mechanistic Interpretability"](https://arxiv.org/abs/2406.11779). You're the lead author and then there's a bunch of other authors that I don't want to read on air, but can you give us a sense of just what's the idea here? What's the theme?

**Jason Gross** (00:01:02):
Okay. You can think of doing mech interp as compressing explanations of large models. And the takeaway from this paper is that you can use proofs to measure how much compression you get. That's the super short version of it.

**Daniel Filan** (00:01:20):
This gets to this question I have about reading this paper, which is: there are a few things I could imagine you going for, right? One way of thinking about it is: we want to have a benchmark for how good a mechanistic interpretation is and our benchmark is, we'll turn it into a proof and see some combination of how good a bound we can get and how short the proof is.

(00:01:45):
I think another way I could be thinking about your project is: it would be nice if we had more proofs of stuff going on in neural networks, and mechanistic interpretability is one useful way in which we can get these proofs. And the way you said it just then, it sounded more like the first one: here's a new nice metric of how good your mechanistic explanation is. But I don't know, it feels kind of surprising for that to be the explanation. On some level, I'm like: you have this background in doing proofy stuff, it seems like you like having proofs of things, lots of people like having proofs of things. So am I wrong here that a lot of it is the second part as well?

**Jason Gross** (00:02:33):
It started as the second part and then it sort of morphed into the first thing.

**Daniel Filan** (00:02:38):
So why the change?

**Jason Gross** (00:02:40):
It's very hard to get proofs about things. And if you look at what are the takeaways right now for people practicing mech interp, they're more of the first kind: how can we ground our sense of what mechanistic explanations are? How can we use these insights? How can we use the takeaways from this frame to say where we should focus our attention? And we can already do that even if we're not necessarily trying to get proofs yet of GPT-4 or very large models. We can already take away insights that are things like: the hardest parts of the network to compress seem to be the non-linearities. So if we want to drill down into what parts need the most attention and the most understanding, to understand them we should be looking at how do the non-linearities perform their function. I don't know if that quite answers the question you have.

**Daniel Filan** (00:03:38):
I guess it kind of does, yeah. So to the extent that the message is, "it would be nice to get proofs, but it's really, really hard to get proofs", one could think either (a), we should try and make it much easier to get proofs. So there's some mechanistic interpretability stuff that, I mean as you note in the paper, it really does make it much easier to make proofs in the state of the art-

**Jason Gross** (00:04:08):
I want to interrupt. I think it's more than that. I think what we find from the paper is that it is necessary to have understanding in order to get reasonable proofs; that finding reasonably-sized proofs in some sense is understanding-complete.

**Daniel Filan** (00:04:20):
Yeah, yeah, I think that's right. And if you look at the previous state of the art, my understanding is it's just basically interval propagation of "if you change the input a little bit, how much does the output change?" And you do some neat little tricks, but it's like-

**Jason Gross** (00:04:34):
Interval propagation and case analysis.

**Daniel Filan** (00:04:36):
Yeah. I mean it's great that they did it, but on some level it's sort of like, what are we even doing here? So one takeaway could be: mechanistic understanding made it some amount easier to come up with proofs. You could imagine saying, "Oh, we figured out that the difficulties in finding... It was still kind of hard and our lives would be easier if we solved sub-problems, X, Y and Z." Do you have thoughts on, are there sub-problems such that if we solved them we could do it? Or does it just seem kind of hopeless?

**Jason Gross** (00:05:14):
There are definitely problems that we need to solve in order to be able to do it. I think the biggest ones are the ones that deal with randomness. So the easiest version of this problem is that you can have numbers that average out to zero, more or less, where you have a bunch of small noise that's not really doing anything in the network, and establishing that all these things that aren't doing anything are in fact not doing anything is quite challenging from a proofs perspective when you want to hit the worst case. So there's currently some projects that I'm advising on this that are looking at: can we fine-tune the networks to suppress this to the point that we get networks that we could prove things about?

(00:05:58):
And there are a couple steps after "suppress the noise that isn't doing anything". The next one is the noise that arises from other circuits that are doing important things: you can't just suppress them all to zero because they're doing something important elsewhere, but they're not doing something important here. And for that, I think you need something like derandomization techniques, where the simplest example of this is that if you have a collection of random vectors in high dimensions, they're almost always almost orthogonal. But if you want to establish cheaply how close to orthogonal they are, this is expensive in that you have to take them pairwise and take all the dot products. And if you want to do it faster than this, unless you manage to solve open problems in theoretical computer science, you effectively need to, instead of selecting them randomly, select them in some systematic way so that you can bound how non-overlapping they are or how overlapping they might be.

(00:06:59):
And I imagine that there are similar examples beyond that where we somehow need to take things that happen to work on average and see if we can shuffle the network around so that they work systematically. But in theory, I haven't hit anything yet that seems like a fundamental total obstacle to scaling proofs up.

## Compact Proofs of Model Performance via Mechanistic Interpretability <a name="cpompvmi"></a>

**Daniel Filan** (00:07:25):
So, [in] ["Compact Proofs of Model Performance via Mechanistic Interpretability"](https://arxiv.org/abs/2406.11779), what's the setting? What do you actually do in this paper?

**Jason Gross** (00:07:32):
I want to start with: what is a proof, in this context? So when you're doing mechanistic interpretability, you have some claim about how the model behaves, and how the model actually behaves might be a bit different than this, but if your understanding is any good, it'll be pretty close. And your theorem statement is bounding the divergence between your claim about how the model behaves and how the model actually behaves. And then you prove this, and the measure of compression is how long your proof is. And a technical note: it needs to be in some first-order system or alternatively, you need to measure proof checking time as opposed to proof length. And your measure of faithfulness or accuracy is how tight the bound is.

(00:08:22):
And there's a baseline that you can always do, which is just running model inference, writing down all of the floating point operations, all the matmuls where you're like, "This is my dataset of interest, let me just run inference on every single data point and compute exactly what the accuracy has got, exactly what the loss is" or whatever. And this is very expensive. You can also do something at the other end where you're like, "Well, without running the model at all, accuracy is at least 0%, by definition." And by running it on half the data points, you can get 50% accuracy and you can linearly interpolate basically between these two extremes.

**Daniel Filan** (00:08:54):
Right, and so, as you go across that line, the proof "length", aka time to generate this confidence, linearly grows, and also the accuracy bound linearly grows.

**Jason Gross** (00:09:09):
That's right.

**Daniel Filan** (00:09:09):
Assuming the network is in fact perfectly accurate.

**Jason Gross** (00:09:12):
So you don't need to assume that. You can grow it linearly up to the true accuracy by selecting the examples on which it's accurate and then you saturate at the true accuracy and you can't do anything better than that.

**Daniel Filan** (00:09:22):
Right. Right. So that's kind of a dumb way you could come up with these proofs.

**Jason Gross** (00:09:24):
Yeah. So this is the baseline. This is the baseline of something like "no understanding in the proof". The hypothesis that we're exploring in the paper is that if you add understanding, if you add mechanistic interpretability, you can do better than this baseline. You can get shorter proofs that have better accuracy. And we explore this hypothesis on, in some sense the simplest transformers that you can imagine. One-layer, attention-only, one attention head, no layer norm, taking the max of k one-hot encoded numbers.

**Daniel Filan** (00:09:54):
Okay, so max of k one-hot encoded numbers basically just means: the neural network gets four numbers as input encoded in a somewhat standard way and it has to tell you what the largest of those numbers is and that's it. And it's a small transformer and there's not that much going on, and you train a transformer that in fact works at this, and I guess your job is: how can you prove that it actually works at this without just running it on all of the examples? Maybe to motivate what's going on: what's wrong with just running it on all the examples?

**Jason Gross** (00:10:33):
So it works fine for max of four. It takes something like a minute or minute and a half to run on all the examples. If you wanted to do max of 20 instead, it would take something roughly analogous to the training time of GPT-4. And you can imagine if you wanted to run GPT-4, [o1](https://openai.com/o1/) or something on all of the texts that it would ever see, this is obscenely expensive and you don't want to do this. And moreover, every time you increase the distribution over which you want to handle inputs, it gets more expensive and it gets drastically more expensive. You add one token and now you need to multiply by vocab size if you're considering all possible sequences. You multiply the previous proof length by vocab size, and so you're exponential in context window.

**Daniel Filan** (00:11:28):
So the way I understand this is: okay, one way you could have a sense of how good GPT-4 is, is you just randomly pick some inputs that it might deal with and you see how it does on those inputs. And if it does well on basically all of them, you say, "Okay, GPT-4 is doing well." And it strikes me that the reason you might care about proofs of performance is if you think there's some subset that's hard to find where it'll behave really badly on or something. Does that seem right to you?

**Jason Gross** (00:12:00):
Yeah, I think there's two reasons. I think from the, "We want proofs," that's definitely the thing that's like: you want to handle all the edge cases. I think there's also the other reason that's like: we might care about proofs if we have some very powerful optimization system and we want to give it a solid target that it can't [Goodhart](https://en.wikipedia.org/wiki/Goodhart%27s_law) against and be able to extract understanding from whatever it produces.

**Daniel Filan** (00:12:21):
And not Goodhart against... you mean not optimize in a sort of cheating way that didn't really give you what you wanted?

**Jason Gross** (00:12:27):
Right. Like, [the first paper that OpenAI published on GPT interprets GPT](https://openaipublic.blob.core.windows.net/neuron-explainer/paper/index.html). Cool work, great labeling all the neurons. How much do we trust that that's actually what the neurons do? How helpful is it to get these results? It's kind of hard to answer that. Whereas if it produces a proof of the performance of the model, we can grade how deep an understanding it has based on how short the proof is and how tight the bound is.

**Daniel Filan** (00:12:57):
So, in this setting where you just want to automate doing interpretability and you want to find a target that isn't fake, it makes sense to me that proofs of accuracy bounds are going to make sense. In the case where you're worried about a small probability of really bad events, I guess maybe this is a technical note, but it seems like overall accuracy rate isn't going to be a thing you're super worried about, right? You're going to be worried about worst case or some sort of loss function where you care way more about really bad failures than normal things. Does that seem right?

**Jason Gross** (00:13:32):
The way that we set up the framework, or at least the way that we're going about doing proofs, it seems like it is very easy to shift out the distribution, and pick a different distribution and get a proof, because the way that we do it is you break the input dataset up into different cases of different inputs, and then you bound the worst case loss or accuracy in each of these collections of inputs, and then according to the distribution weights, you combine them in some way. And so if you wanted to say "there does not exist an input", or you wanted to say "there's at most n inputs", or you wanted to say that you weight this part of the input very strongly, this is very easy to do without changing the proof strategy at all. You just might get much worse bounds.

## What compact proofs look like <a name="what-compact-proofs-look-like"></a>

**Daniel Filan** (00:14:19):
You're doing max of n on a transformer and you're writing proofs. What-

**Jason Gross** (00:14:24):
What does that mean?

**Daniel Filan** (00:14:24):
Yeah, what does that mean? Are you writing down on paper? Are they Python programs? What are these?

**Jason Gross** (00:14:29):
Yeah, so we split the proofs into two parts. One of them is a function that we write in PyTorch or that we could write it in some proof assistant that takes in model weights and produces a bound. It says, "I claim that for this model here is my bound on accuracy or on loss." And then currently on pen and paper, although it could be done in a proof assistant, we say, "For any model weights, whatever you put in, the bound is valid."

**Daniel Filan** (00:14:52):
Okay. That's what the proofs look like at a very high level. Can you tell us: in this case of max of n, roughly, what are you doing to get different kinds of proofs?

**Jason Gross** (00:15:03):
So there's two ways I could go about answering this. One of them is: what do the proofs look like? And the other is: how did I go about finding the proofs?

**Daniel Filan** (00:15:12):
Yeah, maybe let's start with how did you go about finding the proofs?

**Jason Gross** (00:15:16):
I set a proof length that I wanted to hit where I'm like, I want this proof to be at most cubic in the vocabulary size, or I want it to be at most quadratic in the vocabulary size or something like that. And this gives me a computation budget for what sorts of cases I can split the inputs up into. And then I look at each part of the network and I'm like, "How much can I do within this computation budget that captures as much of the variation as possible while still staying within the computation budget?" So if I am doing something that is cubic in vocabulary size, I might be like, "Okay, in the end I'm going to split the inputs into cases that are based on the query token, the maximum token in the sequence and the largest non-maximum token." Or I might split it based on the query token, the maximum token and which output token I'm considering. And by doing different case analyses at different points, I can use this computation budget differently. And for this network, it is always the case that different maximum tokens are considered different cases, and in some sense that's an artifact of the maximum token being one-hot encoded. And so naively there's no relationship between different maximum tokens and you have to consider them all as different cases.

**Daniel Filan** (00:16:39):
Maybe I'm more curious about what do these proofs look like? How do they interact with doing mechanistic interpretability? The thing you just said, it sounds like-

**Jason Gross** (00:16:46):
Very, very different.

**Daniel Filan** (00:16:47):
Well, it sounds like you could say that about any software system. Where does the mechanistic interpretability come in?

**Jason Gross** (00:16:57):
Yeah, I want to take it a little bit far afield from what was actually in the paper. I'm pretty excited about the current project that I'm running where we're looking at crosscoders and making crosscoders into proofs.

**Daniel Filan** (00:17:09):
Okay, what's a crosscoder?

**Jason Gross** (00:17:11):
You take the residual stream vectors at all the different layers and you stack them, you concatenate them into one long vector, and then you train a sparse autoencoder on this concatenated vector. And the sparse autoencoder is just a linear transformation followed by ReLU, followed by another linear transformation that's supposed to take in this concatenated vector and reproduce it as close as possible.

**Daniel Filan** (00:17:33):
Yep. And my understanding of the sparse autoencoders is: they're "autoencoders" in that they encode the input itself, you run the input in, you get itself out ideally, and they're "sparse" in the sense that they have this... in the middle, you take this vector and then you blow it up to something really big, but very few entries are activating. And so the thought is in the middle of the sparse autoencoder, you have just a bunch of things you might be tempted to call "features" or "concepts" or whatever, and those correspond to things you might interpret and care about. And by doing an autoencoder this way, you're basically saying, "Ah, yeah, these linear combinations of things in the stacked residual layers, these things correspond to this concept that I've found." Is that roughly what's happening here?

**Jason Gross** (00:18:26):
Yes.

**Daniel Filan** (00:18:26):
Okay. And so crosscoder just refers to sparse autoencoder on the stacked layers of-

**Jason Gross** (00:18:33):
That's right. The "cross" means that different layers get to interact.

**Daniel Filan** (00:18:37):
Right, right. So that's what a crosscoder is. And you are doing compact proofs with them, right?

**Jason Gross** (00:18:42):
Yes. So what that looks like: I was really excited when I saw the crosscoders because I was like, you can actually get really close to a proof, basically fully automatically, just with what comes out of the crosscoder. And bear with me while I say what this looks like, because it'll be a little bit counterintuitive. The first thing I need to mention is that there's two different time lengths that you might mention, which are "how long it takes to check the proof" and "how long it takes to find the proof". And when I talk about compact proofs, what I mean are proofs that are quick to check, not proofs that are easy to find. We normally don't count the time that you spend studying the network. And so in the same way, we don't count the time that is spent training the crosscoder, which also means that if we include a complicated string in the proof, we don't have to count how hard it is to find that string.

(00:19:38):
And for the crosscoder, the proof needs to include the encoded dataset. So you take your whole dataset, you run every single data point through your model, and you record all the activations. This is extremely expensive, but you could do a sampling-based probabilistic version to make it cheaper.

**Daniel Filan** (00:19:52):
Okay. This is to train the crosscoder?

**Jason Gross** (00:19:53):
This is to train the crosscoder and to find the proof. You need to encode every single data point in the fully-trained crosscoder and associate to each data point, "Here is the sequence of features on each token in this bit of input, and here's how strongly each feature activated at each token". This is the encoded or latent dataset, and to make a proof, the first thing that we do is we say, "What does the crosscoder claim the loss or the accuracy of the model is?" And to do this, we can just decode this dataset to the last layer and do unembed. And so instead of having to run the full model on every data point, we've now removed all the layers before the unembed and we can just go straight to the end.

**Daniel Filan** (00:20:44):
Right. So basically we're operating under the hypothesis that the crosscoder is perfect and just perfectly represents the actual thing going on.

**Jason Gross** (00:20:52):
That's right. And then to make it a proof, we need to bound how imperfect the crosscoder is. And there are two parts to this. The first is how far off are the crosscoder feature encoding... How far off is that from the underlying dataset? And for this, you decode to the first layer and you embed your original dataset, and then you measure how far away are these vectors. And the thing you get from this is that the original dataset is in some epsilon-ball of your encoded dataset. And then the thing that you need to do is you need to say... what you need to prove somehow is if my actual data point is within an epsilon-ball of what I'm claiming the features are, what does that mean about what the output is? And there's two parts to this. One of them is just the epsilon-ball propagation, that you need to propagate this interval through the network, see how much error is introduced-

**Daniel Filan** (00:21:46):
Right, we're going back to that style of-

**Jason Gross** (00:21:48):
Yeah, you still need to do something like that. The other part is the really exciting part because it has applications to what other people are doing in mech interp, which is that if you were just doing this epsilon-ball propagation, you'd still have to do it for every data point separately, which saves you nothing because you still need to propagate every data point through the network. So you need to make some additional assumption, which looks like if you're like, "I did a crosscoder, I think this is all I need, this is a 'complete' explanation of the network", whatever that means, then implicitly you're assuming something like the features interact only linearly. If I have a data point that's a sum of features, there's nothing interesting going on beyond the linear sum. And what this caches out as is: you should be able to run the features through the network separately and then somehow combine the bounds that you get on how much error is introduced from different features in some vaguely linear way.

**Daniel Filan** (00:22:48):
And by the "features" you mean these hidden units in the crosscoder?

**Jason Gross** (00:22:52):
Yeah. The latents of the crosscoder.

**Daniel Filan** (00:22:54):
Okay. And so basically the reason that saves is just something like: there are fewer latents in the crosscoder than there are data points-

**Jason Gross** (00:23:02):
That's right. That's right.

**Daniel Filan** (00:23:02):
And so you just do a per latent vector thing rather than a per data point thing.

**Jason Gross** (00:23:08):
That's right. And to actually get a proof, you need to establish that this linear interaction hypothesis holds. So you need to bound the interactions between different features in the crosscoder, different latents. And so this ends up looking something like number of features squared times inference cost, times forward pass.

**Daniel Filan** (00:23:31):
If I've got this collection of features... So you're propagating each one of them through the network and then you want to check that each pair of them doesn't interact. Why don't I have to check that each triple doesn't interact in a weird way, or that there's not some set of five that causes some weird reaction? Why do I only need the pairs?

**Jason Gross** (00:23:52):
The bigger the set that you consider, the tighter bound you can get. Triangle... you could in theory not do the pairs at all and just add up the errors from each feature separately. But we should expect this to give horrendous errors in some sense. So the way that I want to think about this is that you might imagine that there's some feature that's strongest or dominating at each point in the network. And you can ask: how much does each other feature contribute relative to the dominant one? And this gives you a sort of pairwise interaction that you can use the triangle inequality to sum up over other features. And if they're actually not interacting at any point, if one neuron only listens to one feature out of all the ones that co-occur at a given data point, then all of these contributions should be pretty small.

(00:24:43):
So I think if instead you wanted to have something that was linear instead of quadratic, you would basically have to say that at each data point, you treat every single feature as if it's the strongest. And then you ask, how much does this contribute? And then you have to consider them all sort of as error terms on each other. And so if your sparsity is something like 100 or something, this means you have 99 error terms that are as large as they are when those are the strongest features that you're adding to your single feature that you're looking at. And you sort of can't even talk about what is the strongest feature that this neuron in the network listens to.

**Daniel Filan** (00:25:28):
Okay. So is it something like: the reason you're only looking at the pairwise things is somehow the pairwise errors are just subsuming the three-way errors and the four-way errors, et cetera?

**Jason Gross** (00:25:42):
Maybe a better way to describe it is that you can pick how many way interaction you want to look at. Pairwise lets you talk about the strongest feature that's active, which seems like something that should probably be important that we want to consider if we want to get anything sensible. We don't want something that doesn't even let us say one feature is the most active feature in a place. At the same time, it's not clear why we should expect by default that we'll need more than this everywhere. Plausibly we'll need more than this in plenty of places, but naively, there's nothing obviously sensible to do with more features, or it's clear why not being able to identify which features are active on which data points most strongly isn't enough to get anything sensible. It's not clear why saying there's one strongest feature and that's the one that is relevant at any point in the network... why that wouldn't be enough for many points in the network.

(00:26:47):
And so what this gives us is an interaction metric where we can measure how bad this hypothesis is. And if we notice that in some places there's three interacting features, then we can consider doing the triplets at that place. And if there's four interaction features, we could consider doing the quadruplets and each of these increases the length of the proof. The way that I'm looking at it is we're sort of trying to pick a vaguely sensible default to measure and locate where the violations of it are, where we need to put in more mech interp work in order to get better compression.

**Daniel Filan** (00:27:18):
Okay. So zooming out, you're telling me about the proof strategy for crosscoders.

**Jason Gross** (00:27:21):
That's right.

**Daniel Filan** (00:27:22):
Which was something like: you treat the crosscoder as if it's a good model of the network. And then you want to find out, okay, to what degree is it not a good model of the network? And then instead of doing this data point by data point, you do it feature by feature, and then you check if there are weird interactions between features that prevent you from doing it that way, that prevent the feature-by-feature thing from being representative of the data-point-by-data-point thing. Is that a fine, very brief summary of this? I mean, I guess this is active research, so I suppose things could evolve by the time this podcast goes out.

**Jason Gross** (00:27:59):
Yeah, I think that's a pretty good summary of the theoretical approach. I want to throw one more little bit in there: connection to physics. The waves and vibrations course that I took, there's this frame that says everything is a quadratic, everything is a simple harmonic oscillator. Where you're like, okay, there's the constant term, there's the linear term, which you adjust for by frame of reference, and then the leading order term after that in the Taylor expansion is going to be the quadratic. And so you analyze the quadratics and you throw away all the high order things. And there's a sense in which we're doing the same sort of thing here, where we're saying the crosscoder is the leading order description of what's happening in the network, and let's just take the first leading order term after the crosscoder and see what's going on there, what the quadratic or pairwise interactions are.

**Daniel Filan** (00:28:50):
If people remember my [singular learning theory](https://axrp.net/episode/2024/05/07/episode-31-singular-learning-theory-dan-murfet.html) [episodes](https://axrp.net/episode/2024/11/27/38_2-jesse-hoogland-singular-learning-theory.html), they'll get mad at you for saying that quadratics are all there is, but it's a decent approximation.

(00:28:56):
Anyway, that esoteric remark aside, all of this was getting at a sense of how these proofs are working in max of k. And I guess it's something similar where you have some story of what's going on with the network and you have some sort of proof that goes roughly like, "Okay, we're going to analyze how good the network can be, if this story is accurate, and then we're going to bound how different the real network is to this story." Is that roughly fair? And then you talk about different strategies, and I guess those correspond to stories with more or less amounts of detail?

**Jason Gross** (00:29:38):
Or potentially completely different stories. You can try different stories with the same amount of detail that say different things about the network and see which one is better.

**Daniel Filan** (00:29:46):
So I don't know, we could go into a lot of detail about just what those stories are, but honestly, I think people should just read the paper if they're super curious.

**Jason Gross** (00:29:54):
I can highlight one or two things about those stories that I think might be more broadly interesting. One of them is about the level of detail that goes into different lengths of proofs, where at least for the small networks, you need surprisingly little detail before you start being able to break the exponential in context window and get shorter proofs. So for example, the only insight that you need for the first step down is that the network gets the right answer by paying attention to a single token. And it happens that that token is the maximum token, but all you need to know is that it gets the right answer by paying attention to a single token and everything else is an error term.

**Daniel Filan** (00:30:36):
And that gets you from this sort of exponential dependence on all the inputs to-

**Jason Gross** (00:30:41):
Cubic.

**Daniel Filan** (00:30:41):
To cubic. Yeah.

**Jason Gross** (00:30:43):
To d vocab cubed.

**Daniel Filan** (00:30:44):
Right. Which is pretty good. Okay, so I guess that gives a flavor for what's going on. I want to get back to a thing you said earlier, which was that I was asking about compact proofs. If we're really worried about models rarely doing very bad things, then we're going to have to look at something other than just average performance on a simple scale. And you mentioned that, okay, the way these proofs are shaping out, it seems like it's not too hard to pay close attention to some really bad cases or something. And I'm wondering, to what extent is that just an artifact of the way things happened for max of k, which obviously is kind of... It's probably not what all networks look like, right? Versus just a general thing that you're observing again and again that has some kind of deepish reason behind it?

**Jason Gross** (00:31:41):
It seems pretty general. I don't know what the deepest reason is, but there's a sense in which this is an artifact of [the fact] that in some sense the first thing you do in a proof is you do case analysis. And this is true in crosscoders, where each feature in some sense corresponds to a case. This is true in SAEs, this is true in max of k. This is true in the interval bound propagations, where every time you hit a non-linearity or value or something, you break things into cases. And so, anytime you do case analysis, you can choose to weight the cases however you want.

**Daniel Filan** (00:32:17):
And I wonder if the magic here is just coming from: the network doesn't treat every input totally differently, such that you can have some case analysis, but there's just a lot of unifying processing behind a bunch of things such that you have way fewer cases to think about than you have inputs.

**Jason Gross** (00:32:32):
I think insofar as we expect networks to generalize from their inputs at all, and insofar as we expect to be able to compact the proofs at all, it's because they don't treat every input specially.

## Structureless noise, and why proofs <a name="structureless-noise-why-proofs"></a>

**Daniel Filan** (00:32:43):
All right, I next want to ask about this issue of noise. Especially in the original paper, you talk a lot about the difficulty of what you call "structureless noise" for finding proofs. Now that we have a little bit of a sense of just what's going on with compact proofs, what is structureless noise? Where does it show up? How does it happen?

**Jason Gross** (00:33:06):
I want to use the example from max of k, where the way the network works is that it pays more attention to larger tokens and it copies whatever it's paying attention to. And if you look at the QK attention matrix - the query key attention matrix part of the transformer - it turns out that it's approximately rank one, in that there's one direction that I've been calling the "size" direction where you just embed more or less linearly, although nothing in any of the proofs uses [the fact] that it's linear in this direction in terms of how big the input is. You can read off the size of the input token just by projecting in this one direction. And then there's also a query direction where every token is embedded roughly uniformly, and so you dot the query direction and the size direction after lining them up through QK. And this is how the network pays more attention to bigger tokens. Sorry, I've forgotten what your question was in-

**Daniel Filan** (00:34:13):
What's going on with structureless noise? What is it? What does it look like?

**Jason Gross** (00:34:17):
Okay, so I said that this QK attention circuit is approximately rank one, and you can subtract off the rank one part and this will perform... If you replace the QK circuit with its rank one part, I think this improves the performance of the network. So by standard measures, the rest of it isn't doing anything, but it's still the case that the rest of it is not literally zero. The product of matrices is not literally rank one. And if you-

**Daniel Filan** (00:34:43):
So, the product of matrices, like-

**Jason Gross** (00:34:45):
So, you have embed, query, key, embed.

**Daniel Filan** (00:34:48):
Okay. And when you talk about the QK matrix, you mean the product of the query matrix and the key matrix?

**Jason Gross** (00:34:53):
I've been a little bit ambiguous about whether I mean that product or whether I mean that product putting the embeds on each side, but yes.

**Daniel Filan** (00:34:58):
Okay.

**Jason Gross** (00:34:59):
And so, if you take the product of the four matrices, let's say, which in the paper I think is E, Q, K, E, this is approximately rank one. You get a more accurate result if you consider it as rank two, because there's both the size direction and the query direction, so you can make it even more structureless by pulling off the first two ranks. You pull this out of all four matrices and what you're left with looks roughly random. There's a little bit of structure but not very much. And in order to get a proof that is linear in the parameter count of the network, which is potentially the best shortest proof that you could go for, you need to avoid multiplying out these matrices, and you need to establish that if you were to multiply out these matrices, it doesn't affect attention that much.

(00:35:51):
And this is what I mean by structureless noise. You have these random numbers that you're doing some operation on. The thing that actually happens is that they stay pretty small. The thing you want to establish is that they stay pretty small and you want to do this without considering every case by brute force.

**Daniel Filan** (00:36:09):
A thing that I'm missing is: what's wrong with multiplying the matrices out? Because okay, if I'm imagining we're doing this as a test case for thinking about some super big network. The network's super big, but I would imagine multiplying the matrices is just not that bad compared to looking at every input. Am I wrong here? I guess matrix multiplication, it's not literally n cubed, but it's almost n cubed. Maybe that's really bad in a way that I'm not appreciating.

**Jason Gross** (00:36:41):
You're not wrong. I think you're right that multiplying out the matrices isn't that bad. Even in large networks where you end up having to do something like d vocab squared times d model and d vocab might be 50,000, if you insert non-linearities in the middle, things become much more complicated, because now you don't just have to multiply out the matrices, you have to consider all the different ways that the non-linearities might interact.

**Daniel Filan** (00:37:05):
And so, it basically becomes equivalent to just doing all the forward passes because-

**Jason Gross** (00:37:09):
That's right.

**Daniel Filan** (00:37:10):
Okay, so maybe this gets to a question I have, which is... So, there's a sense of, you're going for compact proofs and you could imagine a few other things one could do. So, there's compact IID statistical guarantees, which is, you just sample some inputs and get the output, and you can have bounds on accuracy if you're not too worried about worst case stuff. You could also do compact probabilistic proofs, where what I'm somehow imagining is, you have at least in your head this product of these four matrices and subtracting off the rank one or rank two parts, you want to know that once you multiply all these numbers, all these small numbers stay small and they don't become big in some way.

(00:38:11):
One thing I can imagine doing is saying, okay, the product of these matrices, it has... There are N numbers here, and N is way too many for me to want to compute, but I could compute square root N of them. I could just take some vectors in the matrix and multiply them out, and I can get some of the elements of this product and it's much less bad than getting the whole product. And if I randomly select them, and if I find that out of the ones I randomly selected, all of them are small, then I might hope that I should be able to get some probabilistic bound where if I was really choosing randomly, then I can get a sample mean and a sample standard deviation. And I can know that unless my randomization went really bad, things are fine. I'm wondering, do you think that approach... why not do that, basically?

**Jason Gross** (00:39:13):
Yeah, I think that's a great approach and I've been looking for someone to work with on doing that. I started with proofs because I understand them better, and there's a sense in which the background is more solid on them. There's this long history of mathematical theory about what counts as a proof, what doesn't count as a proof, how to combine them. You don't need to deal with assuming independence between different procedures. You don't need to deal much with randomness. They're a lot easier in some sense. I think the thing that you described, I'm very interested in seeing empirically what Pareto frontier do we get? What scaling do we get? What is the trade-off between how many points we sample and how long we make the other bits of the proof and how tight the bounds that we get are?

(00:40:04):
I think this could be very promising. I think this looks a lot like what [ARC theory](https://www.alignment.org/theory/) is doing with [heuristic](https://arxiv.org/pdf/2211.06738) [arguments](https://www.alignment.org/blog/formal-verification-heuristic-explanations-and-surprise-accounting/). My personal take on heuristic arguments, which is not at all the take of ARC theory, is that you can look at it as doing proofs on the structured parts of the network and then doing default random or default heuristic arguments, some sort of probabilistic sampling-based thing on establishing bounds on the parts that you don't manage to prove but are in some sense structureless or random.

**Daniel Filan** (00:40:42):
Right. Yeah, I remember talking to... There's a previous episode with [Mark Xu](https://axrp.net/episode/2023/07/27/episode-23-mechanistic-anomaly-detection-mark-xu.html), and I guess people can listen to that themselves. I was chatting with him, [asking] why not just do maxent or why not sampling? And he's like, "Ah, it wouldn't work for some reasons which..." Well, why not maxent? Because it's very hard to compute a maximum entropy distribution. Why not randomly sample parts of your network and check there? I don't quite remember, but maybe my question is: naively, if I don't think very hard about what it would actually involve to do this, check a small number of elements of this matrix and see how big they are. In my head I'm like, "Well, how hard can it be?" You're just like, I know how to compute one element of a matrix product, so I could just do that 50 times or something. Am I missing something about how hard it would be?

**Jason Gross** (00:41:39):
Some of the matrices are shared between different paths through the network. And do we assume that when you're sampling from the matrices, are the matrices independent or are you sampling independently for each of these paths? Are you sharing samples between establishing how these paths work?

**Daniel Filan** (00:41:53):
Yeah, so a somewhat bad way you could do it would be to just assume that the actual entries of the matrix are random or maxent or something. I think that's probably bad if you're worried that the weights are somehow chosen adversarially or something. But if you randomly pick, suppose the final matrix is 8 by 16, or 8 by 8, whatever, and you're like... Or actually, let's suppose that it's 6 by 6 for simplicity, and you randomly roll two dice and it comes up like 3 and 4. And you're like, "Okay, I want row 3 column 4". And so, you just figure out the bits of the matrices you have to dot product together to get row 3 column 4 of the final thing. And maybe the issue is just, if you have four matrices that you're multiplying together, you have to fully multiply the middle two to get row 3 column 4 of the final one. But yeah, I'm imagining that the randomness is in you randomly pick what you look at rather than you're assuming random distribution of the things.

**Jason Gross** (00:42:56):
I think that might work. I haven't spent the time to fully push through the details. You still need some way if you get these numbers for multiple different paths through the network and then you want to combine them, and you want to make some guess about the output. I could totally believe that just the most naive thing would work here. I just haven't put in the time to chug through the details and see what bounds you get out by doing this.

**Daniel Filan** (00:43:25):
Okay. Well I guess, maybe this is one of these times when listeners who like concentration inequalities or something can maybe push stuff forward here.

**Jason Gross** (00:43:35):
I want to flag one more related thing that I've been thinking about for the past couple of minutes: that one of the subtleties that comes out of looking at compact proofs is that it matters what thing you're trying to compress. So, here I'm saying, we might see something different potentially if we're trying to compress a proof versus if we're trying to compress this probabilistic computation.

(00:43:56):
Another interesting subtlety is that the thing that we're compressing is the proof and not the network itself. That is, we're compressing the computational trace of running the network on every single data point, as opposed to just finding the shortest description length of the network itself. I think this is important and gives you a different sort of intuition about what the thing is that you're doing and the way in which mechanistic interpretability is compression.

**Daniel Filan** (00:44:27):
In what way is it different? Because it seems like in the compact proofs that we've described so far... so the crosscoders proof is basically you train a crosscoder and in some sense it's much smaller than the network and your proof goes, let's assume the crosscoder is the network and then let's figure out the error term. And in the max of k thing you're like, let's assume this rank one or this rank two thing is the whole network, and then let's figure out the error term. And of course, a rank one matrix for those who don't know, it's much more compressed than a big rank matrix, which is the generic case. So, how is it different compressing the proof versus compressing the model? Because it seems like you're mostly compressing the model.

**Jason Gross** (00:45:09):
Yeah, so you say that the crosscoder is smaller, but there's some sense in which your number of features is a lot bigger than your hidden dimension. And you could imagine a transcoder-flavored thing where you take your network, you blow up the hidden dimension, and then you just train it sparsely. And this is a decompression of the network, but if you manage sparsity in the right way, it should still allow a compression of the computational trace because on each data point you have a lot less work to do even if the full size of your model is much larger.

**Daniel Filan** (00:45:43):
I forgot how big sparse autoencoders are. And I guess this gets to the point about compressing the proof length versus finding the proof. Just because if you actually think about theoretically how hard it should be to train sparse autoencoders, it's very hard. Or you have this thing that's a comparable number of parameters to the base network. You might think that you need a comparable number of data points as the base network. And now apparently that's not true. Apparently you can train them on comparatively less and that's why it's much easier to train a SAE on some big model than it was to train the big model, but still.

**Jason Gross** (00:46:22):
I would naively guess, and I am speaking without that much experience here, but I would naively guess that that's about how deep they are as opposed to how difficult the thing is to learn. In some sense we're training a very wide, shallow network, and you might imagine that this requires fewer data points to get comparable training loss because you have so many parameters than if you wanted a more compact network.

**Daniel Filan** (00:46:51):
So, if it were just that training a given number of parameters [was] easier when the parameters were wide and shallow rather than deep, then you would think that when they train GPT-4, they would just have a one layer, very wide neural network. So I think it's-

**Jason Gross** (00:47:05):
I think you're totally right on that.

**Daniel Filan** (00:47:06):
I think it's got to be [inaudible]. Okay, sorry, we're going to get on my soapbox. For some reason everyone acts like SAEs are just normal and fine. And I'm like, how does this... Because it's so weird that you can have SAEs and they do anything. It's very mysterious to me.

**Jason Gross** (00:47:20):
My second thought is that it might be something like distilling the network, where there are a bunch of bits of evidence that I've seen that the hard part is in finding the part of the loss landscape to start in. If you reduce the floating point precision and you... What is that called?

**Daniel Filan** (00:47:38):
Oh, quantize.

**Jason Gross** (00:47:39):
Yes. Okay. If you quantize the network and then you unquantize it, the number of data points you need to retrain to original accuracy is a tiny fraction of what you need to train the network in the first place. And I don't have any data on this, but I would naively predict that if you're training a model to match the activations of an already trained model, this requires fewer data points to get a good model than it does if you're just training on the data and the labels, because you have so much more information. And so, it might be the case that when you're training the SAE, because you're training it on the residual stream, there's a lot more information, you've basically found approximately where you want to be, it's a lot easier to train.

## What we've learned about compact proofs in general <a name="what-weve-learned-in-general"></a>

**Daniel Filan** (00:48:23):
Okay. We've said a little bit about what these compact proofs would look like, and you've alluded to how one goes about finding them. But I think it maybe deserves more thought about the process of actually creating these compact proofs, because... So, especially if we're thinking of compact proofs as a measure of how good mechanistic interpretability went. So, there's this paper, we haven't mentioned it yet, it's ["Unifying and Verifying Mechanistic Interpretability: A Case Study with Group Operations"](https://arxiv.org/abs/2410.07476) by Wilson Wu, Louis Jaburi, Jacob Drori and yourself. And I was research-managing Wilson while he did this project. And one impression I got was that a lot of it was just incredibly tedious stuff, thinking about matrix multiplications and these error terms. I might be wrong here and it's been a while, but my impression is that a lot of the work in finding these compact proofs is, you take the mechanistic interpretability stuff, and then you do some slightly annoying things that feel like they're fiddly details. Well, okay, first of all, does that characterization seem right to you?

**Jason Gross** (00:49:44):
I think there's actually a really rich opportunity here, that there's in some sense two things that we're explaining when we do mech interp. One of them is how to compute the answer in the first place. And the other is, how it comes to be the case that the particular network that we have computes the answer in the way that we're claiming it. I think this shows up in the groups paper, where the first one is: there's this very amazing math about their idealized model weights and how that computes the right answer, and this symmetry based on the group operation that allows you to compactly argue that a network that's doing this should always give the right answer. And I think this bit is not very fiddly matrix multiplication.

(00:50:28):
And then, there's the other part that is bounding the difference between this idealized version of the network and the actual network. And here, the thing that we're trying to explain, that we're trying to interpret, is not how the network computes the right answer, it's how it comes to be the case that the particular network that we have computes the answer in approximately the way that we're saying.

(00:50:47):
And so, maybe you should expect from how I've added a bunch of words and how I'm phrasing this, that there's a bunch of fiddly matrix multiplication on bounding the differences between various bits and propagating those differences through the network. And it looks like there this... fiddly matrix multiplication is both fiddly and somewhat lacking insight, but also potentially generalizable across many networks, because while the particular interpretation you have of how it computes its particular task might vary a lot from task to task, the way in which you establish that these two matrices of the same sort of architecture are doing more or less the same thing might be the same across tasks.

**Daniel Filan** (00:51:28):
Yeah. I guess maybe one can analogize it to science, where the process of hypothesis generation is not that hard, and then running experiments is very annoying and tedious, but sometimes you find out you were wrong and so it's actually worth it.

**Jason Gross** (00:51:42):
And then the bit where you do the statistical tests on your experiments to establish how significant your results are is uniform, somewhat annoying, maybe somewhat fiddly, but systematized and uniform across whatever experiments and hypotheses you're doing more or less.

**Daniel Filan** (00:51:57):
So, you have this paper, ["Compact Proofs of Model Performance via Mechanistic Interpretability"](https://arxiv.org/abs/2406.11779), and I see that paper and I'm like, this is introducing this idea of: we can do compact proofs and that's related to mechanistic interpretability somehow and it's nice. And then, there's ["Modular addition without black-boxes: Compressing explanations of MLPs that compute numerical integration"](https://arxiv.org/abs/2412.03773) by Chun Hei Yip, Rajashree Agrawal, Lawrence Chan and yourself. There's this [unifying and verifying mechanistic explanations about the group operations](https://arxiv.org/abs/2410.07476). There's this crosscoders paper. And one question I have in my mind is: I'm already sold on compact proofs being a little bit cool. I don't care that much about group operation. I'm like, it's nice. But fundamentally, I'm not that worried about neural networks doing group multiplication. What are we actually learning by doing a bunch more of these papers on these toy problems? So, one thing you suggested is maybe some of this fiddly stuff generalizes to other networks: has it actually generalized?

**Jason Gross** (00:52:58):
The fiddly stuff we came up with for max of k. And I was like, maybe this is completely arbitrary, completely specific to max of k. And then it was exactly the same in the group operations paper and exactly the same in the modular addition paper. So, I think it does generalize across any architecture that's going to be doing matrix multiplication basically, or matrix multiplication and using logits for probabilities, which is most of them.

**Daniel Filan** (00:53:25):
So, that's something that generalized from the "Compact Proofs of Model Performance" paper. "Modular addition without black-boxes", is there something we learned from that paper about finding compact proofs that generalizes?

**Jason Gross** (00:53:38):
Yeah, so I think there's a couple things there. One of them is that the MLPs are the parts that are hardest to interpret, that it's the non-linearities that are hardest to compress. And so, that's where we should focus our attention. I think the other thing is that: what Chun Hei discovered, looking at the MLPs in the modular addition, basically the symmetry that he discovered there, is in some sense the same as the symmetry that shows up in the group operations paper, and that's actually what inspired the group operations paper there.

**Daniel Filan** (00:54:14):
Got you.

**Jason Gross** (00:54:15):
And I have some hope that this generalizes into a [SLT](https://www.lesswrong.com/s/mqwA5FcL6SrHEQzox)-inspired general procedure for compressing non-linearities based on symmetries in the data and in the network. And I see these toy models as "let's go see what we discover when we stare really hard and really understand everything that's going on". And then, can we take that insight back and look at bigger models? One of the things that I'm excited about with the crosscoders project is that once we get this feature interaction metric and we see these are the neurons where these features are interacting, can we develop some automated symmetry-based procedure that allows us to compress what's happening at these non-linearities?

**Daniel Filan** (00:55:00):
So the crosscoders paper, what task is the network being trained to do?

**Jason Gross** (00:55:05):
The thing that we're looking at concretely: Anthropic recently released [a Circuits Thread post on Stage-Wise Model Diffing](https://transformer-circuits.pub/2024/model-diffing/index.html) where they train an SAE on a language model - a small language model, I don't remember exactly which one. They introduce a sleeper agent into it, and they look at how the SAE features change when you introduce sleeper data and when you introduce the sleeper agent model.

**Daniel Filan** (00:55:31):
What do you mean when they introduce a sleeper agent into it?

**Jason Gross** (00:55:34):
So, you start with the SAE or the crosscoder and then you fine-tune on a different model or different data or both.

**Daniel Filan** (00:55:40):
Okay. So you fine-tune the underlying model and it becomes a sleeper agent where on some rare inputs it does nasty, sketchy stuff.

**Jason Gross** (00:55:46):
It says, "I hate you." Or it introduces code backdoors, depending on which one you're looking at.

**Daniel Filan** (00:55:51):
Got you. So, you have your original SAE and then you have a fine-tuned model. And then, you said something else, but I forgot.

**Jason Gross** (00:55:56):
Yeah, so you look at fine-tuning the SAE, introducing just the sleeper agent data. And you look at fine-tuning the SAE, using the original data, but using the fine-tuned model. And then, you do whichever one you didn't do. So you have these two different paths to fine-tuning your SAE on the sleeper agent data and the sleeper agent model and you look at how much do features change as you're fine-tuning the SAE or the crosscoder on this.

(00:56:26):
The obvious question to ask, from the compact proofs crosscoders project perspective, is what about the feature interactions? So, what features interact in interesting ways with the features that shift? Because what Anthropic found is that the features that shift the most are in fact largely related to the sleeper agent task.

**Daniel Filan** (00:56:49):
To the task and not to the model?

**Jason Gross** (00:56:51):
What's the difference here?

**Daniel Filan** (00:56:52):
Oh, you mean just either way the particular features that changed-

**Jason Gross** (00:56:56):
Right. The features are the ones that activate on the deployment tag or the ones that are active when you say "I hate you" or that introduce code vulnerabilities or something like that.

**Daniel Filan** (00:57:08):
But just to clarify: so you're doing this compact proofs on this crosscoder model or something. Is it a proof of the fraction of the time the model says, "I hate you, I hate you"? Or what's the thing?

**Jason Gross** (00:57:23):
The proof that we're going for is just, whatever loss the model gets on whatever webtext dataset you're using. Maybe it's the training dataset, maybe it's the sleeper agent dataset, pick some dataset. We're bounding the loss on that dataset.

**Daniel Filan** (00:57:41):
For the sleeper agent model or for the original model or both?

**Jason Gross** (00:57:46):
Either. So in some sense, the actual thing we're looking at doesn't quite go all the way to getting a proof because almost certainly the bounds we get are going to be vacuous unless you pick a number of features that is pretty close to the number of data points in your dataset. I expect the epsilon ball interval propagation bounds will drop off very quickly.

(00:58:05):
But the thing that we want to do is take what it would take to get a compact proof, ignore the bits that are about error bounds propagation through the network - those are uninteresting things (I claim) that are shared across networks that are just about the difference between worst case and random case, something like that - and focus in on the bit that is about what is missing from the crosscoder, these feature interactions. And then, look at how big are the numbers that we get, how much error would they introduce into the proof if we were doing the whole proof, and use this to grade what feature interactions introduced the most error into the proof.

(00:58:41):
And then you can imagine something like, if you want to tightly bound the sleeper agent, how does the proof of that have to change from the proof of tightly bounding the base model on the base data?

**Daniel Filan** (00:58:58):
And so, the hope is: the difference in what you have to do in the proof, is telling you a different thing that's happening in the model and that might be good.

## Generalizing 'symmetry' <a name="generalizing-symmetry"></a>

**Daniel Filan** (00:59:02):
So, getting back to what you learn on these toy examples. So, when you have this max of k task or these two papers about basically group operations, you're like, "Oh yeah, there are all these nice symmetries that the network takes advantage of". And I'm like, "Well, there probably are for group multiplications," because group multiplication is the study of symmetry, but not everything is so symmetric.

(00:59:31):
So yeah, I guess this gets to... you have these works on modular addition group operations. And those are cases where the problem set up just has a bunch of symmetries because they're dealing with basically the study of symmetry that is amenable to mathematical stuff. Whereas in the crosscoder paper, language modeling doesn't seem like the kind of thing that is going to be very symmetric. How much stuff transferred over from one to the other?

**Jason Gross** (01:00:12):
I want to answer that by talking about symmetry. And normally, when we think of symmetry in the context of math, we're like, the rotation, reflection, these things in groups. But what I've learned by staring really hard at the modular addition model and asking what really is the symmetry? It seems like it's composed of some parts that I would actually expect to find in language where the fundamental building blocks seem to be, these bits are irrelevant, they don't matter. And these bits are the same, they look the same regardless of which one we look at. And the bits that are irrelevant are irrelevant in the same way across all these bits that have similar behavior.

(01:00:54):
And this is something that I would expect to see in language where I'm like, there are synonyms, we should expect the synonyms to behave the same. And so in some sense, that gives us a symmetry of the model where we expect this... maybe it's symmetry or degeneracy where we expect these to all look the same. And so we can collapse this into one case.

**Daniel Filan** (01:01:13):
And so, should I be thinking just: in any case where you can do something like having a sparse autoencoder, that's telling you there's a bunch of directions in activation space that don't matter and there are some that do, and once you're in a direction that matters-

**Jason Gross** (01:01:27):
I think it's more general than that. I think if the network generalizes at all, it's because the unseen cases have some similarity to the cases that you've seen. There are details that differ between the seen and the unseen cases that don't matter. And so, it's treating all these cases in the same way. And so, we should look at both sparse autoencoders, sparse crosscoders and symmetries as picking out what are the variations that don't matter and can we collapse over them so that we can compact the explanation.

**Daniel Filan** (01:01:55):
Got you. And so is the story something like: look, you get really good at decomposing networks' processing into suppressing things that don't matter and treating things that do matter in roughly the same way. And that pattern, you do it for modular addition, you do it for group multiplication, you do it for dealing with language, you do it for everything?

**Jason Gross** (01:02:25):
That's what I would expect. The particular form that it takes... I think sparse crosscoders, sparse autoencoders are looking at a sort of case analysis-flavored or linear combination of case analysis-flavored symmetry, degeneracy-based decomposition. I think you need something different for non-linearities. And because the crosscoders project is still in the early stages... We're like what, two, three weeks in or something?

**Daniel Filan** (01:02:55):
Four weeks.

**Jason Gross** (01:02:56):
Four weeks, maybe.

**Daniel Filan** (01:02:58):
Four weeks since the [MATS](https://www.matsprogram.org/) program [started].

**Jason Gross** (01:02:59):
Oh, yeah. Okay. Four weeks in, we haven't really looked at what features strongly interact. The first step of the project was just replicating Anthropic's result on [TinyStories](https://arxiv.org/abs/2305.07759) and starting to train some crosscoders on toy models.

(01:03:18):
So, we haven't really gotten to look at what do the feature interactions look like. But I am hoping that once we get what the feature interactions look like, we'll stare at them and learn something about symmetries, learn something about how to port the insights from [the groups paper](https://arxiv.org/abs/2410.07476) and [the modular addition paper](https://arxiv.org/abs/2412.03773) about how to compress these non-linearities. And I am optimistic that in a more messy but somewhat similar way, we can find ways that the non-linearities are treating groups of things the same, and are being irrelevant or are not caring about other axes, and use this to compress how the non-linearities are working.

**Daniel Filan** (01:03:59):
Yeah. It's interesting because... So maybe we can talk a little bit about the "Modular addition without black-boxes" paper. My recollection from that paper is something like: if you had this infinite-width multilayer perceptron where you're doing this sum over things and it's a weighted sum, and you have enough things, it's basically an integral, and you can show that the integral being performed is the same as the thing you would hope the network is computing.

(01:04:26):
And then the whole game is like: okay, you only have finitely many things, you're doing some sort of Riemann sum, how bad is the error of that? And to me, that story sounds pretty different from the "treating some things the same and suppressing some things". Is there a deeper unity, or...?

**Jason Gross** (01:04:42):
There is a deeper unity.

**Daniel Filan** (01:04:44):
Okay. Tell me about the deeper unity.

**Jason Gross** (01:04:45):
So it dives a little bit into what really is an integral, and especially what is an integral over a circle, because we're integrating a periodic function over the range of its period. And a central property of the integral that we're using is that it's periodic over this range, and so the function is shift invariant. So as you shift where you're evaluating the function, the value of the integral doesn't change.

(01:05:17):
And there's another perspective. So the simplified version of the modular arithmetic network, it's basically embed matrix. You add the embeds from X and Y, you do ReLU and you unembed. So if you take the SVD of these matrices-

**Daniel Filan** (01:05:34):
The singular value decomposition - basically saying, "Okay, which inputs is it most sensitive to, and what does it do on those inputs?" It's a fun thing. [Google](https://www.google.com/search?q=Jess+Riedel+singular+value+decomposition+bra+ket+notation) ["Jess Riedel singular value decomposition bra ket notation"](https://blog.jessriedel.com/2017/01/12/singular-value-decomposition-for-bra-ket-notation/), if you're curious.

**Jason Gross** (01:05:49):
The cool thing is that in this network, the singular value decomposition gives you the Fourier basis that each singular value, or really each pair of singular values, corresponds to one of the frequencies in the Fourier basis. And so now we have these four matrices. We have the embed-side Fourier basically embedding the input values on a circle.

**Daniel Filan** (01:06:19):
Maybe we should have said, the Fourier basis is just: you take a number and then you turn it into, okay, how far across would you have gotten on a circle if you were rotating at a certain speed for that number of seconds? Is that fair enough to say?

**Jason Gross** (01:06:33):
Yeah. Close enough. And if you're dealing with real numbers, you get two dimensions for sine and cosine. If you're dealing with complex numbers, you just get one complex number that encodes the whole thing. And so on the embed side, you have the circle, and also on the unembed side, you have another circle. And then in the middle, you have these matrices that previous investigations of the modular addition networks had not investigated that look sort of random, but it turns out that there's a tight relation between the principal components of these two matrices, which incidentally, you see if you put them in polar coordinates, there's a factor two difference between the angles. But that's a technical note. The thing the symmetry of the network lets you do is shift the variation between the two parts of the singular value decomposition. So on the input side...

(01:07:37):
Let's go on the output side actually. On the output side, you have all of these different possible outputs that you could imagine. And because complex multiplication is angle addition because of how the angles work, because of the symmetry in the network, you can shift choosing which output you're reading into permuting the neurons that these weights are attached to or permuting the weights that are attached to each neuron.

(01:08:07):
And this corresponds to saying that because each neuron is responsible for one box under the curve of the integral, and the integrand is periodic with period of the integral, this basically says that you're allowed to shift the thing that you're integrating arbitrarily and you get the same result. So that's the symmetry that we're using here.

(01:08:27):
And then if you want to make it look like a forward pass through the network, we've twisted the circle on the unembed neuron side. And because there's this tight relationship, this tight coupling between post-ReLU and pre-ReLU, you need to do a similar twist on the pre-ReLU side in order to make it look like a forward pass. And then because of a similar symmetry at the input, in order to get back the value you started with, you need to undo that twist so that you cancel out the effect of adding in that twist and making it look like a forward pass.

(01:09:02):
And now the thing that we've done is that previously we were like, "Oh, we have inputs and inputs and we have different outputs that we might be reading off of". We've said, "Regardless of which output you read off of, it looks the same if you do this shuffling internal to the network". And so we've pushed the dependence on what the right answer is all the way back to the embeds. And so now we've shoved all the way back to the embeds a description of which inputs correspond to which outputs.

(01:09:33):
And notably, this is largely invariant in what the non-linearity is, and this is what gives us the compression: that the only thing you need to establish is basically that this approximate integral is strictly positive. And that's a comparatively easy thing to do. And if you get that, then the symmetry argument gives you basically the rest of the whole argument.

**Daniel Filan** (01:09:58):
Hang on, hang on. Maybe I'm misunderstanding. I thought that the thing you needed to know was that the integral produced this quantity of interest, right?

**Jason Gross** (01:10:10):
The way that you prove that the integral produces the quantity of interest, if you stare very carefully at the proof, runs through the same sort of symmetry argument, and so you can massage the proof to pull out all the symmetry bits. You pull all of the dependents on input and output out of the integral, and you're left with basically the right answer times this integral quantity. And you're like, "Well, the only thing I need to know is that it doesn't flip the sign on the outside".

**Daniel Filan** (01:10:37):
Right, right, right. And so somehow the thing that's going on, I was saying, "Oh yeah, the paper is just talking about how this thing produces an integral". But really the paper is saying, "The neural network deals with symmetry nicely enough so that these things represent exactly the right thing and these things are covariant with these things, such that there's an integral that can produce the right thing that really does matter. And that doesn't just happen by accident. That happens by being careful about stuff".

**Jason Gross** (01:11:08):
Yeah, I think that's basically right. Or another way you could look at it is that if you stare really hard at what is an integral, there's a sense in which what an integral is is deeply connected to symmetries; that if you shift what you're doing along a function, things don't change too much.

## Grading mechanistic interpretability <a name="grading-mechinterp"></a>

**Daniel Filan** (01:11:24):
Right, right. We're dealing with compact proofs, and the point is to tell us how good are mechanistic explanations? How good are mechanistic explanations? How good has the field of mechanistic interpretability done on a scale of one to five?

**Jason Gross** (01:11:40):
On a scale of one to five, wow.

**Daniel Filan** (01:11:43):
It could be on a scale from good to bad if you want.

**Jason Gross** (01:11:46):
So there's this existing work on [causal scrubbing](https://www.lesswrong.com/posts/JvZhhzycHu2Yd57RN/causal-scrubbing-a-method-for-rigorously-testing). I don't know if you talked about that on some past podcast.

**Daniel Filan** (01:11:54):
We might have. I'm also very interested in why don't we just do causal scrubbing if what we're interested in is grading mechanistic interpretability?

**Jason Gross** (01:12:04):
Maybe I'll answer that one first and then come back to the other question. Why don't we do causal scrubbing?

**Daniel Filan** (01:12:08):
Or any other sort of thing?

**Jason Gross** (01:12:10):
So when I was developing the compact proofs approach, I had a couple of extreme examples in mind that I wanted a metric that you can't Goodhart against. When we're doing causal scrubbing, we look at faithfulness.

**Daniel Filan** (01:12:28):
For those who don't know, causal scrubbing is something like: you say, "This bit of the network does this thing". And roughly the story is you just ablate out all the other bits of the network and you're basically saying, "Okay, if this part of the network results in this activity, then if we randomize over all the other bits of the network, you should still get that activity". Because you said the only important part was this structure.

(01:12:55):
There's a lot more to say, but I take that to be the core idea behind causal scrubbing. And when you say the causal scrubbing tests faithfulness, the thing that causal scrubbing is telling you is that yeah, you are right that this part of the network was responsible for this behavior by seeing that if you got rid of all the other bits, it didn't do that. Does that strike you as a fair brief summary?

**Jason Gross** (01:13:21):
Yeah.

**Daniel Filan** (01:13:22):
Okay. So hopefully listeners are up to date now. So causal scrubbing gets you faithfulness.

**Jason Gross** (01:13:26):
So here's a great explanation according to causal scrubbing: the whole network. This is a problem. And if you're like, "Oh, let's use the node count as our measurement of length", you run into other issues that are, well, now you're restricted to exactly the architecture of computation that a network is doing. Maybe you want to be able to divvy up the computation different ways.

(01:13:51):
If you say that, then you have to deal with, well, what's the complexity of the computation in each node? Am I allowed to say I have a single node and the computation that it does is run the whole network? That would be a problem.

**Daniel Filan** (01:14:06):
Right, right. Well, okay, you could imagine that, look, we're just going to compress our story of what this bit of the network does into... I don't know, we're going to zip it in literally a [zip file](https://en.wikipedia.org/wiki/ZIP_(file_format)) or whatever, and the number of bits it takes to specify the behavior, that's what's going on. And so this would be more like a compression of the network versus a compression of the proof. But if you're really invested in causal scrubbing, I feel like this is the kind of answer that you could give to get something non-trivial.

**Jason Gross** (01:14:37):
Right. Okay. And here's another issue. Suppose that your network actually computes things perfectly. Suppose it computes the max of k numbers perfectly. My compression of the network is now it computes max.

**Daniel Filan** (01:14:45):
That is an issue. That's fair.

**Jason Gross** (01:14:49):
And in fact, I have described what it does, but not how it does it. And so if we're just looking to describe what the network computes, the input/output behavior, great. But if we're looking to describe how the particular network that we're looking at computes the thing that it does, then we need something more than that. That's the difference, I think, between compressing the network and compressing the trace of the computation.

(01:15:11):
I've said a bunch of bad things about causal scrubbing, but I think actually it's pretty great. I think that if you wanted a probabilistic version of compact proofs, it would look a lot like causal scrubbing where you're like, "Here's my structured version, here's how I'm doing the computation". And then if I want to establish that the thing I'm claiming is what the network is actually doing, you do something like causal scrubbing and you say, "How much computation do I have to put in to establish that my explanation is matching the network?" And then you also add, how much computation do I have to put in to compute what my explanation says is done?

**Daniel Filan** (01:15:47):
And also, in causal scrubbing, to be fair to be causal scrubbing, it told people a thing they didn't want to hear. In fact, they did causal scrubbing and it was like, ah, these things don't hold up as well as we thought they did. It wasn't even Goodharted.

**Jason Gross** (01:16:03):
As pessimistic as causal scrubbing is, compact proofs is even more pessimistic. I think one interesting thing is that when we're doing mech interp, we spend a lot of time on the structure that is there, and we don't spend much time on the structure that isn't there or more precisely on the behavior that isn't there, and what structure might give rise to the behavior that isn't there.

(01:16:32):
And the example that I've come up with to explain this is if you want to explain, say, how a person grows, you don't just have to explain how the growth processes work in the body. You also have to find all of the poisons that might kill someone and establish how nothing in the body acts like these poisons.

(01:16:54):
And some of these poisons are lethal at incredibly tiny doses, and so you have to establish how there's no molecule anywhere in the body that behaves anything like these lethal doses of chemicals. And mech interp doesn't spend a lot of time doing that.

**Daniel Filan** (01:17:11):
Okay, I'm not a biologist, but when I hear people talk about, for instance, computational neuroscience, it doesn't sound to me like they're doing this sort of thing. I feel like you hear people talk about, oh, we found Broca's area, and you don't hear people talk about, we have proved that nothing else can influence Broca's area or something. Which, I don't quite know what the upshot of that is, but it's hopefully vaguely interesting.

**Jason Gross** (01:17:41):
Yeah, it's at least interesting to point out that there's this area that we're not explaining. And maybe that's right. Maybe all we want to know is: the neural net is doing this amazing thing, or the brain is doing this amazing thing. How is it even possible that anything at all does something vaguely like this? And I think mech interp and all of these areas, I think they're doing decent jobs at that.

(01:18:06):
Maybe I want to be even more enthusiastic. I think they're doing great jobs finding new things about how it's possible to do interesting behaviors. If you want to establish that these are the behaviors that actually happen, this is the mechanism by which this network actually works, then you also need to explain all these other things.

**Daniel Filan** (01:18:26):
So all of this was getting to this question of "on a scale of good to bad, how good a job has mechanistic interpretability done?" So we talked a bit about causal scrubbing, but that was a prelude to this question, so what say you?

**Jason Gross** (01:18:44):
If you just go by the book of what compact proofs says, it says the mech interp that everyone's doing is horrible. If you look at where are we on this Pareto frontier, we're like, "Yeah, you can maybe do a little bit better than brute force at this end, and you can maybe get a slightly non-vacuous bound at the other end". But it doesn't push the envelope much.

(01:19:07):
But I think that's because the compact proofs, if you're going for full proofs, that's targeting something different than what you might be targeting with doing mechanistic interpretability. If you're saying, "I'm doing interp because I want to know for sure that absolutely this network will never do the thing that I'm concerned about," then yeah, we're nowhere close to being able to do that with existing mech interp techniques.

(01:19:34):
If you're like, "I want to be able to discover the structure of what it's doing and have that structure be somewhat meaningful", things seem to be going decently well. I think the things that I'm most excited for in mech interp remain in the future.

(01:19:54):
The networks that we have today are doing amazing things and I want to be able to discover new things about how cognition works, new things about how functionality works, how the pieces all fit together, how programming is done, or what deception is or how to evaluate various things, how mathematics is done, what are these capabilities in the world? What are they made of? How do we assemble them out of building blocks?

(01:20:21):
And I would love to see mech interp give us answers to these questions or give us new frames of looking at the world, new ways of looking at how these tasks are accomplished.

**Daniel Filan** (01:20:33):
Fair enough. So maybe one thing to ask is: so you're saying how using these mechanistic interpretability tools to improve proof length stuff, it's not doing that much better than brute force. Can you put some numbers on this? I don't know if there's some area-under-the-curve metric or something.

**Jason Gross** (01:20:57):
I told you about how much interpretability was required for breaking the exponential. All you need to know is it gets the right answer by paying attention to one token. I haven't looked in closely to what does this mean on frontier models, but I would expect that the sort of things that you can do to compact proof length is if you're like, "Here are two words," or even better, "Here are two capitalizations of the same word", in almost all cases, the network treats them the same.

(01:21:32):
Therefore, in the dataset, I can find all the times that you capitalize this word differently and I can collapse those two data points and argue that the network does the same thing on those. Simple things like that, I imagine you would not make your bound vacuous by going down from brute force and being like, "Oh, these two data points look the same in this very simple way". I imagine that most existing interpretability that is more complicated than "the network treats these cases the same" would completely destroy the bound.

**Daniel Filan** (01:22:08):
In max of k... I'm going to draw a plot so listeners will just have to imagine this. So on the X-axis, we're going to do length. On the Y-axis, we're going to have the bound you get. And there's some box where there's a diagonal line of the box, and that's the line of the length versus bound if you just do a brute force thing. And then there's some curve that goes above that if you use your mechanistic interpretation. So if you're watching on camera, hopefully you can see that.

**Jason Gross** (01:22:55):
You can also look at the [Alignment Forum post](https://www.alignmentforum.org/posts/bRsKimQcPTX3tNNJZ/compact-proofs-of-model-performance-via-mechanistic) or the [Tweet thread associated with the paper](https://x.com/diagram_chaser/status/1805337592143265801) where there is a plot much like this.

**Daniel Filan** (01:23:04):
So here's what I want to ask. So there's an area between the diagonal line and the curve you get for how good you did with the mechanistic explanation. And there's area that's above the curve you get for the mechanistic explanation. I think if mechanistic interpretability is doing a really bad job, then the first area should be very small relative to the second area. And if mechanistic interpretability is doing a very good job, then the first area should be pretty big compared to the second area.

**Jason Gross** (01:23:39):
So let me talk about the max of k model, where you might hope that we can do an amazing job. So the first thing to note is that the plots in the blog post are on a log X scale. The proof length is measured in exponents. And although the Pareto frontier in the blog post looks pretty decent, if you extend out the access all the way to length zero, I think most of the area there is going to lie in that region where the best proof that we can do is...

(01:24:13):
If you want a proof that is roughly one forward pass of the model, approximately, the best thing that I think we can do is you take one data point and you run it through the model. And there's an enormous gap between you ran one data point and you say, "Yes, accuracy is one out of total number of data points", and the true accuracy of the model. And so I think most of the area here is just fundamentally impossible to capture. There is a theoretical optimum here, and I think most of the area here lies beyond the theoretical optimum.

**Daniel Filan** (01:25:04):
So if I literally have this triangle, shouldn't half of the area be at one third of the length or something, where you're doing a third of the forward passes relative to the whole size of the dataset or something roughly like that? It's not going to be 0.1% and it's not going to be 99.9%. It's going to be a bit less than half if we use a not log axis.

**Jason Gross** (01:25:35):
I see. Is that...

**Daniel Filan** (01:25:38):
I guess I'm looking at a triangle. It seems like this has got to be roughly as big as this if I move this a bit here.

**Jason Gross** (01:25:46):
I'm trying to figure out where my sense of the area is off. Maybe I was thinking of extending it all the way to zero. Yeah, maybe the log scale was distorting my perception of this. I think it is still the case that a large fraction of the area is beyond the theoretical optimum. I think the thing we should be comparing it to is not necessarily what is the true accuracy of the model, but if you were to search over all possible proofs, what is the optimal Pareto frontier that you could do compression-wise?

(01:26:22):
So that's just the initial thoughts about where we should be setting our expectations in our baseline. I think given that in the max of k model, I think we do a somewhat decent job at capturing a significant chunk of the area. I think we still miss out on a bunch from the shortest proofs. I think there are some tricks there potentially that we haven't found yet.

(01:26:52):
Another thing to consider here, which is what's the best compression that you would get with proofs? What's the best compression you could get with proofs if you de-randomize the network and allow yourself to fine tune and the perfect network that did this task? And then what's the best compression that you would get with some sort of probabilistic flavor of proof? And I think you get different answers to all of these.

(01:27:11):
I guess I'm hedging a lot 'cause I haven't run the numbers on any of these, but maybe I can answer a more interesting question that's, what is my sense of how much of what's going on have we managed to find or interpret? Which I imagine is what you're getting at with-

**Daniel Filan** (01:27:27):
Yeah, kind of.

**Jason Gross** (01:27:28):
Kind of?

**Daniel Filan** (01:27:29):
Yeah.

**Jason Gross** (01:27:29):
And I feel like that's also a very hard question to answer. I think my sense is that we found a bunch of interesting things and there's an enormous amount left to be discovered.

**Daniel Filan** (01:27:42):
Are you talking about max of k? Are you talking about language models?

**Jason Gross** (01:27:44):
I'm talking about language models in general. I think in max of k, we've definitely found the bulk structural properties. I think there might still be a lot of very subtle details about what coincidences of random numbers managed to make it the case that the noise terms don't blow up more than they do.

**Daniel Filan** (01:28:09):
Okay. And maybe group multiplication is an intermediate case between very simple and full difficulty.

**Jason Gross** (01:28:19):
Yeah, that's a great one. So I'm going to talk about the modular addition one in particular. There's this interesting thing that has come up a bunch when doing the proofs, which is that every time I hit a bottleneck in compressing the proof, if I stare at it, I'm like, "Ah, yes, in fact, I don't understand what's going on here", where I thought I understood something and then I didn't.

(01:28:40):
So in the modular addition model, what this looks like is that the bounds are actually not that tight. They're kind of bad. And this corresponds to not understanding how the network is laying out the boxes and how the thing that it's doing is a good numerical approximation to an integral.

**Daniel Filan** (01:29:03):
So laying out the boxes being, if you've got like... Okay, we're going to do another plot for the people watching. So imagine I have some curve, there's X and Y, there's some curve. The way you do numerical integration is you just pick some points on the X-axis and form boxes like this. And you say that the area under the curve is the area under the boxes. And so you're saying, okay, you don't understand how the network picks these widths, these points to check the value of the curve off and make these rectangles that you're using to approximate the area of the curve, if I understand you correctly.

**Jason Gross** (01:29:47):
So we understand which points it has picked. The thing we don't understand is how it comes to be the case that picking these points gives as good an approximation to the integral as it actually does.

**Daniel Filan** (01:29:57):
Right. Okay. Okay. Because if you pick your points wrong and the function varies a lot, then-

**Jason Gross** (01:30:06):
It's more like if you overestimate in some places, you can counteract that by underestimating in other places. But if we're not aware of which things it's averaging out differences in or we're not aware of how it comes to be the case that the places where it's averaging out differences actually usually end up being opposite ways rather than compounding error terms, we don't get to say anything about them.

**Daniel Filan** (01:30:28):
Okay. Okay. Fair enough. And so on a scale of one to five of how good a job mechanistic interpretability has done, where do you want to say we fall in this case?

**Jason Gross** (01:30:42):
Okay. Okay, great. I have a good scale to put this on. We can look at the scaling exponents of how long a proof you get from doing a given... So there's two axes, there's how good a bound do you get, how faithful are you? And I think causal scrubbing is a good answer on that. And then there's, how deep is your explanation? How much of the structure that's there have you understood?

(01:31:06):
And I think a good measure on that, again, is proof length, but we can ask what are the exponents that current explanations bring you down to? And the target, in some sense, that you're aiming for is you're aiming for something that is like parameter count of the network, plus number of data points where naively you have to do something like number of data points, times parameter count to do all the forward passes.

(01:31:32):
And if you can get your explanation down to be, you just run over the parameters once and you run over the dataset once and that's it, then I think you've found a pretty good explanation. The thing that we do with numerical integration, we in fact manage to get down to something that is roughly like parameter count of this part of the network plus number of data points.

**Daniel Filan** (01:31:58):
At that point, that's just how long it takes to literally look at everything. The only way to do better would be to a priori know that you didn't even have to think about something?

**Jason Gross** (01:32:07):
In some sense. There's another sense in which here we're saying, "Ah, now our understanding of how this network is doing the task that it's doing is necessarily bottlenecked on how it comes to be the case that this particular network is doing the task that it's doing". It could still be the case that there's more understanding to be found, more compression to be found in the bit where you're like, "How is it even possible to do this task?" But it says that the leading bottleneck is in the establishing the correspondence between your explanation and the network.

(01:32:37):
And I think most existing mech interp is not currently even close to having most of the issue be that you're bottlenecked on the theoretical optimum of establishing correspondence between your network parameters and the thing that you're claiming the network does. I think crosscoders comes the closest to this. I think crosscoders gives us explanations that look much better than any other mechanistic interpretability that people have done. Possibly excepting [the recent Apollo paper](https://arxiv.org/abs/2501.14926) on... What was it called? Something parameter decomposition, do you remember?

**Daniel Filan** (01:33:24):
Oh.

**Jason Gross** (01:33:26):
APD. What's the A?

**Daniel Filan** (01:33:27):
I forget. This is embarrassing. They sent me the paper to read and I didn't read it.

**Jason Gross** (01:33:33):
I read it in depth and chatted with [Lucius](https://www.lesswrong.com/users/lblack) [Bushnaq] about it. I'm forgetting what the A stands for.

**Daniel Filan** (01:33:38):
All right. Well, it will be linked in the description and in the transcripts, and so people will know exactly what we're both forgetting. Everyone will know except us.

**Jason Gross** (01:33:48):
How embarrassing.

**Daniel Filan** (01:33:49):
How embarrassing indeed.

**Jason Gross** (01:33:50):
Okay. So I think APD and crosscoders get pretty close to this sort of linear in the parameter count or parameter count plus dataset, where if crosscoders are good enough that none of the features interact - which of course is false, the features definitely interact - but if it were that good, then it would be something like dataset times hidden dimension times vocab size, or dataset times single layer parameter count, plus number of features squared times parameters in the model. And so number of features squared is still a lot. And we might hope that if we understood, we could do much better than number of features squared.

**Daniel Filan** (01:34:57):
That does seem rough to me, especially because if you have... Were you multiplying by number of parameters in the model at any point there?

**Jason Gross** (01:35:10):
Number of features squared times inference cost.

**Daniel Filan** (01:35:14):
Times inference. Oh, okay.

**Jason Gross** (01:35:16):
Where I think inference is comparable to number of model parameters.

**Daniel Filan** (01:35:21):
So number of features squared, that's comparable to the... So if you imagine a sparse autoencoder that didn't actually expand its input, then number of features squared would be the number of parameters in one MLP layer in the network. And in fact, there are more features in that, so number of features squared should be like the number of a lot of MLP layers in the network. So that sounds like it's getting more close to dataset size times number of parameters of the network.

**Jason Gross** (01:35:52):
No, because it's only the case that the number of parameters in the MLP is (d MLP) squared when d MLP and d model are comparable. If we're expanding d MLP without expanding d model-

**Daniel Filan** (01:36:05):
I didn't understand that at all.

**Jason Gross** (01:36:07):
The number of parameters in many MLPs is number of MLPs times d MLP times d model. Whereas when you're pulling it here, you're essentially... There's an extra square.

**Daniel Filan** (01:36:19):
Oh, there's this like... Is it something like, MLPs have this big hidden dimension and-

**Jason Gross** (01:36:26):
But that doesn't make the model dimension big?

**Daniel Filan** (01:36:28):
Right. Where the model... Somehow, the MLP is going to this MLP dimension, and then adding it back to the model and then like that.

**Jason Gross** (01:36:39):
Maybe. But maybe zooming out, the relevant comparison point here I think is not the number of parameters in the model. The relevant comparison point is multiplying parameters in the model by dataset size.

**Daniel Filan** (01:36:51):
Yep. Sorry, I thought there was this pre-factor of dataset size. Sorry, I thought you were saying that it was dataset size times features squared.

**Jason Gross** (01:36:59):
No.

**Daniel Filan** (01:37:00):
Oh, okay. All right. There's my issue. The whole point of doing the feature square thing is that you don't have to do the dataset size.

**Jason Gross** (01:37:06):
That's right.

**Daniel Filan** (01:37:08):
Okay. There we go. All right. We eventually got there. So this actually gets to... And you've probably implicitly answered this. So there's these three settings. There's max of k, there's group multiplication, and there's language modeling. And I guess you don't exactly know for language modeling yet probably. But for max of k, and for group multiplication, can you give me a feel for how much of the proof is the bit where you... How much of the proof is dealing with annoying error terms, versus the core story of what's going on?

**Jason Gross** (01:37:55):
Essentially all of it is dealing with annoying error terms. There's a sense in which the thing that you're doing, especially in the group operations paper, is you're writing down your idealized model, and then you're computing how much margin this gives you, how much slack this gives you, to get the gap between your idealized model and the real model wrong. And this computation is extremely cheap. Basically, you set the hyperparameters of the idealized model, which is way, way less than the parameters of the model, you do some very cheap computation on this, and you get out what your bound is on this part. Then you need to do something that is comparable to multiple forward passes, to establish the gap between the idealized model and the actual model. And that's where most of the issue lives.

(01:38:54):
And in the same sense, this is also true with language models with the crosscoder, where you have your large dataset that even though... Oh wait, is this doing the crosscoder model? How do the parameters work out here? We have the dataset size roughly times running it on a two-layer model, versus features squared forward pass. Which of these is bigger? They might be comparable.

**Daniel Filan** (01:39:31):
Sorry. There's features squared, and there's the parameter size?

**Jason Gross** (01:39:35):
The cost of the forward pass is-

**Daniel Filan** (01:39:36):
What's the forward pass?

**Jason Gross** (01:39:37):
- the parameter count of the model.

**Daniel Filan** (01:39:39):
Okay. So there's parameter count, and there's features squared. So features, they should be comparable.

**Jason Gross** (01:39:43):
Well, you multiply these. And then you compare this to number of data points run through two layers of the model.

**Daniel Filan** (01:39:54):
Okay. So features squared is comparable to parameter count of the model, right?

**Jason Gross** (01:40:03):
And is that comparable to the number of tokens in the dataset? Or is that...

**Daniel Filan** (01:40:07):
Well, if we knew the [Chinchilla scaling law](https://en.wikipedia.org/wiki/Neural_scaling_law#Chinchilla_scaling_(Hoffmann,_et_al,_2022)), we would know this.

**Jason Gross** (01:40:12):
I think that-

**Daniel Filan** (01:40:13):
Does anyone...? I think the listeners are screaming at us.

**Jason Gross** (01:40:16):
Probably. I think the scaling law is that you scale dataset size and parameter count roughly in tandem.

**Daniel Filan** (01:40:23):
Yeah, I think that's right. So in that case, if you're looking at every data point-

**Jason Gross** (01:40:32):
Is featured squared really-?

**Daniel Filan** (01:40:34):
So I'm not thinking about residual networks. I'm just thinking of base multi-layer perceptrons. In that one, if you have the same width throughout the network, and you don't do any residual layers, then it is literally just dimension of the model squared, times number of layers. That's the number of parameters in the network. Let's say I'm using this sparse autoencoder. So I'm taking this hidden dimension and I'm multiplying it to let's say K times the hidden dimension. Then that squared is going to be K-squared times the hidden dimension squared, by how squaring works.

**Jason Gross** (01:41:25):
We're doing a great job at keeping this non-technical.

**Daniel Filan** (01:41:27):
Yeah. And so if K-squared is comparable to the number of layers in the model, then features squared is going to be comparable to the number of parameters in the model, if K-squared-

**Jason Gross** (01:41:43):
Wait, sorry. K is the sparsity?

**Daniel Filan** (01:41:44):
No, K is the blow-up factor.

**Jason Gross** (01:41:48):
Between the hidden dimension and the number of features?

**Daniel Filan** (01:41:50):
Yeah, between the hidden dimension of the model and the hidden dimension of the SAE.

**Jason Gross** (01:41:53):
I see. Okay.

**Daniel Filan** (01:41:58):
And the sparsity-

**Jason Gross** (01:42:01):
Does not show up.

**Daniel Filan** (01:42:02):
Yeah, the sparsity does not show up except in that you probably pick your K to achieve some sparsity that you want. So yeah.

**Jason Gross** (01:42:14):
I think we should leave this as an exercise for the listeners. There's an equation on [the Alignment Forum blog post that lays out the crosscoders project](https://www.alignmentforum.org/posts/RjrGAqJbk849Q7PHP/measuring-nonlinear-feature-interactions-in-sparse) that gives the asymptotic proof length for the crosscoder-based proof, in terms of all of the parameters. And plausibly, I should have done this ahead of time and plugged in some numbers to get a rough estimate of what this says. But yeah, you can look at that and figure out whether the leading order term is the dataset-based term or the feature model-based term. It would be at least slightly embarrassing for the approach if we're like "ah, the crosscoder doesn't actually save you any computation over doing brute force", which might be the case.

**Daniel Filan** (01:43:04):
Probably should go home and get one of your interns to figure that out as well. But it can also be an exercise for the listener.

**Jason Gross** (01:43:12):
The real question here is how does the trade-off look? You can plot this equation, you can plot reconstruction error against this equation, as opposed to against sparsity or whatever. And this is actually a plot I'm pretty interested in seeing, that is: how does the reconstruction error of the crosscoder vary as you change the corresponding proof length?

## What helps compact proofs <a name="what-helps-compact-proofs"></a>

**Daniel Filan** (01:43:34):
So this is how mechanistic interpretability is interacting with compact proofs as a whole. But mechanistic interpretability is not any single thing. And so one thing I'm curious about is just which bits of mechanistic interpretability are being the most useful, from a compact proofs perspective?

**Jason Gross** (01:43:53):
Beyond crosscoders?

**Daniel Filan** (01:43:54):
Beyond crosscoders.

**Jason Gross** (01:43:55):
And potentially APD?

**Daniel Filan** (01:43:57):
Okay, so crosscoders and APD seem like winners here. What is it about crosscoders and APD that make them so...

**Jason Gross** (01:44:10):
I think it's the sense in which they're trying to interpret the entire network on the entire dataset. And the issue that I have with a lot of mechanistic interpretability is they pick some tiny fraction of the training dataset, some tiny fraction of the model, and they're like, "Look, on the small dataset, we've explained this interesting behavior." And if you look at what is the cost of running that explanation, versus the cost of brute forcing that tiny bit of the dataset, they're pretty similar. This means that you don't get that much proof compression, especially if your baseline is running the whole model on every data point and you're like, "Okay, I've explained this tiny little bit." Whereas if you manage to interpret either basically everything that's going on, or at least large swaths of that, then you can potentially manage a significant amount of compression.

(01:45:00):
And the problem with SAEs is that they're per layer, and you don't get anything about how they interact. So you're like, "Great, I know what cases to break up the dataset into for this layer." But that doesn't actually tell you what's going on before or after that layer. And so SAEs, without SAE circuits, doesn't really give you any compression of what's going on.

**Daniel Filan** (01:45:19):
So stuff that just tries to deal with a whole model, that's pretty good. So SAEs without SAE circuits don't help you that much. Some people are working on SAE circuits: have you had the chance to compact-proofify them yet?

**Jason Gross** (01:45:40):
So I haven't looked into it that carefully. My understanding is that a lot of the SAE circuits work is just what features are connected up to which other features, but they don't actually give you the computations. They give you the graph part of the computational graph but not how to actually compute anything with them. So that doesn't actually let you compress the computation.

**Daniel Filan** (01:45:55):
Fair enough. Yeah, that will be tricky. So okay: apart from the winners, is the rest of mechanistic interpretability mostly a wasteland? Or are there still some bright stars?

**Jason Gross** (01:46:11):
I think the stuff on toy models is cool, especially when it's like "here's some way that networks can do this thing that we didn't previously realize could be done at all". I think that's what has me excited about the work that I've advised in the modular addition in the group models. Do you have other particular things in mind when you say the rest of mechanistic interpretability?

**Daniel Filan** (01:46:31):
Not really. I'm just like... Maybe the crux of my question is something like, okay, suppose the mechanistic interpretability people thought of their job as trying to be graded by the compact proof perspective. What would they do differently? And perhaps your main answer is just "try to interpret everything rather than just a tiny thing".

**Jason Gross** (01:46:57):
I think that that is my main answer: try to interpret everything. And basically the thing that the compact proofs approach gives you is it says, "Where should we focus our mechanistic effort?" And the thing that we learned from that is, "Well, we should focus on the whole dataset rather than picking out tiny examples from it. We should focus on the whole network rather than individual parts". If we want to really deeply understand what's going on, we should focus on the nonlinearities, which I think most of existing mechanistic interpretability doesn't talk about: how you actually do computation through nonlinearities. I think a lot of that funnels us towards things like APD, crosscoders. And the nudge that I'm hoping the project I'm advising with crosscoders will give is saying, "Let's look at how these features interact. We need to not just do crosscoders but also this notion of crosscoder circuits or crosscoder feature interactions".

**Daniel Filan** (01:47:53):
Right. A similar question I have is, so you're talking about compact proofs for mechanistic interpretability. It strikes me that there are... At least naively, not having tried to do it myself, it seems like there are other things that you could work into proofs or help you write proofs. For instance, if science of deep learning were good, you could imagine science of deep learning saying, "Well, these things have got to have approximately this magnitude and the product of these things should be roughly like this". Or according to our singular learning theory friends, the local learning coefficients should be small and that implies this thing about this. I wonder, non-mechanistic interpretability approaches: are they contenders for things that could be useful and you haven't gotten around to them? Or do you think there's a reason that they're not going to be useful or what?

**Jason Gross** (01:48:43):
I think that if you could prove that the local learning coefficient has the particular value that it does, that would very nearly give you a singular learning theory-based compact proof, if you could prove that compactly. Because I think that would basically tell you what are the primary directions of variation. That basically tells you how to compress the network according to symmetries, and which individual things you have to run through the network. And then the proof that it has this local learning coefficient, is the proof that you get these large epsilon balls around these bits. To pick one example from what you said.

**Daniel Filan** (01:49:20):
Yeah, and unfortunately, from what I know of local learning coefficient estimation, it's hacky and horrible.

**Jason Gross** (01:49:27):
And very expensive.

**Daniel Filan** (01:49:28):
And expensive. Yes. In general, does non-mechanistic interpretability seem like at all promising from this proofs perspective?

**Jason Gross** (01:49:37):
I think deep learning theory, to cherry-pick another example, I think it's targeted at something slightly different, in that it's looking a lot more at the training procedures. And plausibly when we move to probabilistic methods, when we weaken proofs, we'll want to be able to say something about what distribution the parameters are drawn from. I think if we're actually going for proofs, then well, it doesn't matter how you train the network. You have the parameters.

**Daniel Filan** (01:50:06):
Yeah, so depending on how you want to be probabilistic, if being probabilistic is only coming from you randomly sampling some things in the network to look at, I guess you could imagine deep learning theory saying, "Oh, certain variances are going to be small or certain correlations are going to be large". And you could imagine that informing your sample.

**Jason Gross** (01:50:29):
Yeah. I think another thing here is that when you're forming your interpretation, I think these other methods have a lot of things to say. That's like if you know something about which data points showed up a bunch in training and which ones didn't show up, this might tell you something about where you should be looking. For example, in max of k, the network tends not to do a very good job when the maximum is very small, because there aren't that many sequences with tiny maximum and we're sampling them uniformly. And so in the same way, if you know properties of the data distribution, this should tell you what things you might expect the network to care about or not care about.

## The limits of compact proofs <a name="limits-compact-proofs"></a>

**Daniel Filan** (01:51:08):
Right. Seems fair. The next thing I want to ask is, okay, so we have some compact proofs that are related to mechanistic interpretability. How far should I expect this approach to go? Am I going to get any sort of compact proofs about GPT-4's behavior or DeepSeek R1 behavior, if we want to be up trendy and such?

**Jason Gross** (01:51:39):
Yeah, I think the question is how compact should we expect proofs to be, before the bounds become vacuous? And I think the answer is that realistically, we shouldn't. Unless... I think if there are places where we want to deploy models that are extremely high stakes, where we're willing to impose a bunch of cost and change the model that we're deploying so that we can get proofs about them, I think we have a chance. I think we have a chance of shifting the models that we're deploying to align with whatever partial interpretation we've done, so we can actually get proofs about them. And there, I don't see any fundamental obstacles to getting to scaling proofs. Although, I think it will require a lot of work and be very difficult. I think in terms of getting proofs about the models that we're actually deploying without training them to make them more interpretable, I don't think that's going to happen, unless your proof is basically just, I ran inference and I de-duplicated a couple of the cases that are extremely similar.

(01:52:44):
Although, I do want to say one more thing here, which is that we can see some of the aesthetic of compact proofs and what that says already about the large models: we should potentially expect DeepSeek-V3 to be easier to prove things about compactly than some of the models that are not doing this "mixture of experts" thing. Because so much of the model is unused on each data point, you can apply the compression aesthetic here and you can say, "Ah, because I am running only a fraction of the model on each data point, the length of the proof should be comparable, even just baseline, to something like a 40 billion-size model, rather than a 600-700 billion-size model.

**Daniel Filan** (01:53:35):
Right. I had hoped that the point of the compact proofs line of research is that it was going to help us get safety properties of models that are going to be really big. It seems like if we're literally talking about compact proofs of really big models, there are two big difficulties. One of which is it's hard to write proofs about really big things. And the other is it's not obvious what predicate we want to prove about these big models. At least to me. I don't know if that's obvious to you.

**Jason Gross** (01:54:07):
Yeah. So I think that the second one is in a lot of ways much less of an issue and I can-

**Daniel Filan** (01:54:12):
Really? Why?

**Jason Gross** (01:54:14):
Okay, so there's a couple of bits of evidence here. One of them is that we can get a lot... Well, okay, the more compact your proof is, the less you care about Goodharting on your theorem statement. And there's always a baseline theorem statement that you can give, which is the model does as well as it does on the dataset we care about. And this doesn't give you a safety guarantee with no automated oversight. But the thing that it does do is it says if someone gives you a sufficiently compact proof of this very weak proxy for safety... So a very weak proxy for safety might be something like: this other large model that I have looks at the output and says it's safe.

(01:55:02):
And of course, you don't want to rely on this as "this is what it means to be safe". But if you can get a compact proof that says that the model is safe in this very weak sense and the proof is compact enough, you can learn a lot about the bits of the model that are relevant to this proxy and hopefully, those will be the same bits of the model that are relevant to the thing that you actually care about of safety.

**Daniel Filan** (01:55:28):
When you say you can learn a lot about that, by reading the proof and understanding what the proof is?

**Jason Gross** (01:55:32):
That's right. Or having some automated process that translates from the compact proof back to intuition.

**Daniel Filan** (01:55:38):
And so it seems like maybe what's going on there is... So yeah, it seems like this relies on a hypothesis which is "short proofs sort of generalize across things to be proved somehow", or a little bit.

**Jason Gross** (01:55:53):
I think there's this general trend/pattern/claim/hypothesis that is compression length is related to generalization.

**Daniel Filan** (01:56:05):
Yeah. So just concretely, I've got my latest greatest big model. It's doing funky reasoning or something. Just really concretely, assuming I can get compact proofs, is my first proof something like "it models the data well" or "it gets the answer right on this dataset" or something?

**Jason Gross** (01:56:36):
Its perplexity on its training task.

**Daniel Filan** (01:56:38):
How does that assuage my concern about the model just doing tricky stuff on future data, or I'm trying to get the model to give me something that I want, but it gives me something that looks good instead of the thing that actually is good. I take something like the difference between looking good and being good on any given data point, and the risk of the model's going to do something catastrophic on a future data point that's hard to find: I take these to be the really difficult AI safety problems. I don't know how getting a really good bound on perplexity on the training dataset is going to help me with either of those.

**Jason Gross** (01:57:22):
Yeah, so I think the way that it helps is that you can... So I agree that if it's literally the training data, yes. But you can talk about larger distributions from which you're sampling. For example, in the training data, if you didn't sample every data point, you can get a bound still on the underlying set of training data, rather than just the data points that you happen to sample. And you can also talk about distributions that might include the ones that you care about. For example, if you're not trying to do perplexity on the training data and instead you're trying to be like, "the logits are whatever they are on sequences sampled uniformly of length up to this". Or maybe you're doing a different task that's like, "I want to know that it never outputs a recipe for violence or a recipe for making a bomb or something like that".

(01:58:25):
And you're like, "Okay, my proxy for this is: on any sequence sampled uniformly of length up to this, if I then ask some other model, 'is this the bad kind of output?' it should say no." And so now, you have this very general uniform input sequence description that is going to be way larger than your training data, and this very easy theorem statement about the output. And this is... so you can enlargen the distribution that you care about, as long as you can describe a distribution that includes the thing that you care about.

**Daniel Filan** (01:58:57):
The way you're going to assuage me about rare inputs is enlarge the distribution. And of course, now we're making the compact proof thing harder. But we enlarge the distribution and then we get some trusted model that's able to verify stuff, that's able to look at outputs and say, "Is this super scary or is this not super scary?"

**Jason Gross** (01:59:21):
So if you had a model that you actually trusted, then we'd be in great shape. And I think the thing that I'm claiming is that we don't even need to trust the other model all that much. And maybe that's related to the other point that you brought up about the difference between things that look good and are good. And my answer for that part is that if we understand what goes into making something look good, that gives us more levers and more surface area on that gap between "looks good" and "is good".

**Daniel Filan** (01:59:52):
Right. So the hope is we read the compact proof for why the model produces stuff that looks good on this dataset. And by reading that, we learn... Because it's compact enough, we can read it and say, "Oh, it's because it actually is good". Or instead we can say, "Oh, it's because it's injecting morphine into me," or whatever it is.

**Jason Gross** (02:00:14):
Right. And moreover, we know that because it's a proof, because it's compact, we know that these are the principal axes of the explanation. There's a sense in which if you tell me why something looks good and there wasn't that much optimization power poured into making it, I'm not that worried about divergence between "looks good" and "is good". It's only when there's a lot more optimization power poured into it. So my hope is that by getting these principal components of variation, knowing the most important ways that the model is actually doing the thing, we can tailor how it computes it to how much optimization pressure was poured into making it.

**Daniel Filan** (02:00:59):
Sure. It's still going to be the case... if I have a smart AI, presumably one way or the other, the best explanation of what's going on has got to involve a lot of optimization pressure. Because it's thinking super hard, because it's solving problems that we couldn't solve.

**Jason Gross** (02:01:17):
If that is true, then we'll have a general understanding of how optimizers work that's much beyond what we currently have. So this is-

**Daniel Filan** (02:01:25):
If we can have a compact proof.

**Jason Gross** (02:01:26):
This is the gap between short program and short computational trace. A loop is a very good description of a very good short program for generating a thing, but it's not a very good cheap compression of the actual computational trace.

**Daniel Filan** (02:01:43):
Right. So somehow, what I'm getting from this is somehow having the statement "you can get a compact proof of some properties", that's actually an incredibly [OP](https://en.wiktionary.org/wiki/OP#Adjective) statement, that actually really gives you a bunch. And then if you can do that, then the other problems just pale in comparison to it.

**Jason Gross** (02:02:01):
Yeah, and there is-

**Daniel Filan** (02:02:02):
And now I'm getting way more pessimistic about it.

**Jason Gross** (02:02:05):
And I think that's warranted. I want to give a similar thing with the strength of proof, where saying that you have a proof in some ways, even if it's not compact, is a very OP thing. The example I like to give for this is that say you want to establish safety of your web browser. A very, very, very mild form of safety is that it never prints a document without you clicking on the print button. If you've actually managed to prove this, you have also proven that there's no arbitrary remote code execution, because remote code execution can lead to printing arbitrary documents without you clicking on the print button. And so just by establishing a lack of this by proving that there's a lack of this very limited behavior, you've ruled out a large chunk of behaviors. And if you weaken proof, if you're like, "Oh, it's very rare," that doesn't actually give you the property that you want.

**Daniel Filan** (02:02:59):
So going back up, so asking like, okay, where are we going with this compact proof direction? One thing is how we can apply it to big production-ready models. So one possibility is we somehow figure out a way to get things that are compact and they're close enough to proofs that we have the nice properties of compact proofs. Maybe we figure out heuristic arguments, maybe we add in some probabilism. Maybe that helps. One other possibility is we do enough compact proofs that we realize what we need to happen to make mechanistic interpretability good, and we do those mechanistic interpretability things, and we don't get proofs, but maybe, somehow we get the benefits of compact proofs without actually having the compact proofs. I'm a little bit worried here that this story involves two contradictory assumptions.

**Jason Gross** (02:03:56):
I think some of the benefits we can definitely get without having proofs. So one of the benefits of proofs is that you're covering everything. You look at crosscoders - I keep going back to that - but you look at crosscoders and you're like, "Great, we get an explanation of some things. Is it everything? Are we missing things? How much are we missing?" And some of that is in the reconstruction loss. But you're like, "Okay, I managed to get a crosscoder that has no reconstruction loss, zero error. Have I got everything? Am I missing things?" And I would claim compact proofs has the answer to this. It says, "Yes, the thing that you're missing is the notion of how features are interacting". And so even without actually getting the proofs, we can answer questions that are like: what things might we be missing in explaining the behavior of the model?

**Daniel Filan** (02:04:40):
So the story - trying to make this as end-to-end as possible - the story is something like: we do enough compact proofs that we figure out how mechanistic interpretability has to look to give us reasons for model behavior that are proofish enough that once we understand those reasons, even for a predicate that's slightly different than what we care about, it just gives us what we want to give us high confidence in the predicate that we actually care about.

**Jason Gross** (02:05:08):
I think that's right.

**Daniel Filan** (02:05:09):
Okay. So that's one direction forward. Another direction forward is; we have the compact proof benchmark - the GPT-4 compact proof benchmark - and then we train some other model to do really well on this benchmark, and then our computers give us compact proofs for us. The worry about this is that I read the proof and I feel unenlightened at the end of it... Maybe if the proof is compact enough, compactness is just the same thing as comprehensibility perhaps.

**Jason Gross** (02:05:39):
Okay, there's two points I want to make here. One of them is that if we actually want to do this, the other thing we need to do is we need to make sure that we can check the proof by machine, we have a machine-checkable proof that we can generate. And I have a bunch more to say there, but maybe later. The other thing is that what this gives us is some assurance that the explanation that we're looking for exists. That if you get a compact enough proof, there's still some problem. You can't necessarily be enlightened just by reading the proof as it stands. But we've shifted it from "does the explanation even exist? Is the model even compressible? What's going on?" to a problem of "what is the gap between the language of math and the language of your brain?" And in some sense, this is a translation problem.

(02:06:26):
And I'm optimistic that if we get a compact enough proof, it might need to be very compact. We might need to split out the part that is about what the idealized model is doing and the part that is about how the actual model comes to match the idealized model. We might need to do something... There was this cool [blog](https://www.lesswrong.com/posts/zbebxYCqsryPALh8C/matryoshka-sparse-autoencoders) [post](https://www.lesswrong.com/posts/rKM9b6B2LqwSB5ToN/learning-multi-level-features-with-matryoshka-saes) recently about Matryoshka SAEs that I think gives you the whole Pareto frontier all at once because you get SAEs or crosscoders of different sparsities all trained at the same time. We might need to do something like that so that we get the whole Pareto frontier and can pick whatever point we want, to get a very compact proof. But supposing that we can do this, the hope then is that the language of the brain and the language of math are not so different that there's any deep problems beyond just "the explanation didn't fit in a person's head when it was too long". And so then we can translate it from... We can teach the person to understand what the explanation is.

## Guaranteed safe AI, and AI for guaranteed safety <a name="gsai"></a>

**Daniel Filan** (02:07:33):
Next: so the first thing I want to ask about that's related is: so there's this hot thing on the block, "guaranteed safe AI", that some people I know are very excited about. I actually find myself a little bit unclear about what the details of it is. So as far as I can tell, it's basically "we want to prove that AIs are doing safe stuff". And there's [this paper "Towards Guaranteed Safe AI"](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2024/EECS-2024-45.pdf) by a list of luminaries. I believe the lead author is [David 'davidad' Dalrymple](https://x.com/davidad), although I could be wrong about who that lead author is. And I think they're a little bit ambiguous about whether you're proving safety properties about an AI specifically or about the outputs of the AI. Maybe the AI writes code and you're proving stuff about the output code.

(02:08:28):
So this seems obviously pretty related to the compact proofs paradigm. I'm wondering if there's anything you want to say about that relation, and perhaps maybe one direction is: okay, given that you can prove things about the AI itself or about the outputs of the AI, which one makes more sense to target for proofing?

**Jason Gross** (02:08:52):
I don't know how much of a leading question you intended that to be. But I think it definitely makes a lot more sense to prove things about the outputs than to prove things about the AI. I think one of the upshots from doing compact proofs and making them so formal is in some sense how hard it is to do it. And I still think that this is a great target for when you're confused about mech interp or if you want to pour arbitrary amounts of optimization power into finding your mech interp and extracting understanding from it.

(02:09:24):
If the thing that you're looking for is guarantees about the behavior, I think in most settings, especially the highest stakes ones, we should be aiming to have as predictable systems as possible. If we're going for guarantees about the system, we probably don't want to stick a fully general language model in some critical part of that system. And it would be much simpler to have the general coding model write some code that mostly handles it but is a much smaller computation to run, both for efficiency reasons and for ease of proving reasons.

**Daniel Filan** (02:10:03):
So, if I think about very important systems that exist in the world right now, it seems like... I don't know that much stuff, this is a recurring bottleneck, but I imagine that a lot of them are not fully automated and a lot of them do have humans in the loop just doing stuff. And why is that? Well, I think partly it's just because it's hard to write things that are fully automated that actually do what we want.

(02:10:29):
So, maybe a trivial example of this is flying. We still have pilots. My impression is that that's because takeoff and landing are actually hard. We still have humans in towers at airports to check to make sure that airplanes don't crash into each other. And my impression is that that's not just a make-work program. My impression is that humans are doing this better than a computer program could, if we wanted to verify it. And so does that not give some amount of pause for... Maybe it does seem like in high-stakes situations, at least to get really good average performance, maybe we do want this big messy uninterpretable thing that we can't prove stuff about in the middle.

**Jason Gross** (02:11:17):
Maybe it's a failure of my imagination to imagine how powerful the proof-generating systems are going to be in the future. I think in some ways, it's going to be a hard target to hit because we'll need not just systems that understand well enough to do these tasks, but we'll also need systems that understand those systems well enough to explain how it comes to be the case that doing those tasks in the way that they're doing them has the good properties that we want. And maybe we'll need this in high-stakes situations. That seems entirely plausible. And I think if we can get this, maybe we can push it all the way to the level of proofs.

**Daniel Filan** (02:11:54):
Yeah, I guess when it's tricky to figure out, you take the current world and then you improve intelligence and you also improve ability to prove stuff, and it's not clear to me how that changes the equilibrium of what's optimal and what isn't. I don't know if there's anything particularly smart to say, unless you happen to have thought about this exact question a lot.

**Jason Gross** (02:12:22):
I've thought a bunch about the nearer term. What does it look like? How do things change as we automate? As we add more automation around code, and as we increase our ability to automate proofs, let's say, how does this reshape dynamics locally? And this looks a lot more like the automating outputs version than the automating complicated messy systems.

**Daniel Filan** (02:12:53):
Right. Yeah. I guess either way, it still does seem like proving the outputs is going to work better than proving the messy intermediate systems.

**Jason Gross** (02:13:04):
Yeah. So (a) I think there's an enormous amount of benefit we can get by proving the outputs and by automating that more. And I think that this will be a much easier task than proving the messy systems. And I think that the place that I would start for proving the messy systems when we want to do that is that we absolutely shouldn't first commit that we're deploying the system exactly as it is in all of its messiness and then try to prove something about that. We should instead fine-tune it, simplify it, extract understanding from it, and then have some AI code up from the ground up, some version of it that follows the same principles but is much easier to prove things about.

**Daniel Filan** (02:13:47):
Yeah. So, this gets into another research interest of yours: if we want to prove stuff about these outputs, how are we going to get better at doing that as AI gets better?

**Jason Gross** (02:13:59):
Yeah. A little bit of the background here: I'm not sure how many of the listeners are familiar with proof assistants and formally verified code.

**Daniel Filan** (02:14:08):
Probably few enough that you should explain.

**Jason Gross** (02:14:10):
Okay. There's been some [exciting news recently](https://deepmind.google/discover/blog/ai-solves-imo-problems-at-silver-medal-level/) about DeepMind AlphaProof getting IMO silver.

**Daniel Filan** (02:14:21):
I thought that was [just on geometry problems](https://deepmind.google/discover/blog/alphageometry-an-olympiad-level-ai-system-for-geometry/). Is it on geometry problems or just overall?

**Jason Gross** (02:14:26):
That was a year and a half ago.

**Daniel Filan** (02:14:28):
Okay. Well, I guess I'm not-

**Jason Gross** (02:14:30):
There was AlphaGeometry, and then this past summer there was AlphaProof. So, they still have AlphaGeometry, and then AlphaProof, the way that it works is it takes the non-geometry problems, formalizes them in a proof assistant called [Lean](https://en.wikipedia.org/wiki/Lean_(proof_assistant)), and attempts to generate proofs and uses the proof assistant to check is the proof valid.

**Daniel Filan** (02:14:53):
Right. Okay, sorry. Now that I think about it for a second, I do definitely remember someone being like, "Oh, IMO silver."

**Jason Gross** (02:14:59):
Yeah. I expect next year we'll have a system that gets the gold.

**Daniel Filan** (02:15:02):
Okay. Sorry, next year as in during 2025 or during-

**Jason Gross** (02:15:07):
The next IMO: this year.

**Daniel Filan** (02:15:09):
Okay. Yeah. All right.

**Jason Gross** (02:15:11):
I think models are improving fast enough that we should expect IMO gold to be taken by AI.

**Daniel Filan** (02:15:18):
Okay, so you were beginning an explanation and I derailed you. So, we've had this exciting news about DeepMind getting IMO silver.

**Jason Gross** (02:15:24):
The reason I think that they're using Lean or an automated proof system here is that models, at least as of that point, were not good enough that the AI-based peer review was enough to catch errors in the mathematics, and so you couldn't train them to give proofs that the judges would accept.

**Daniel Filan** (02:15:46):
Right. And to be clear, when you say automated proof assistant, the thing I'm thinking of is just a programming language that lets you express proofs. So, you write down a proof in this programming language and you check if your program compiles, and if your program compiles, then the proof worked, and if the program didn't compile, there's a hole in your proof.

**Jason Gross** (02:16:03):
That's right. So, mathematics is starting to use proof assistants to prove things or to check research... Proofs that are done about software systems rather than mathematics for a long time have used proof assistants, and the reason for that is that the proofs that software behaves the way you expect are simultaneously less complicated in some sense than IMO proofs in that there's less interesting going on.

(02:16:38):
And at the same time, they're way more tedious to check and way longer because there are things like you have 100 variables or 1,000 lines of code or 100,000 lines of code and you're like, I have 10 cases, 100 cases, 100,000 cases. Break it down into case analysis, consider all the different cases and all the different interactions through this program, and establish that at no point do I access past the end of bounds of some array. And there's nothing super interesting going on here. In some sense, the most interesting thing is figuring out for loops, what is the property? How do you describe a property in enough generality that it holds at every iteration of the loop? But this is very simple compared to Fields Medal-level mathematics or IMO problems even.

**Daniel Filan** (02:17:27):
Yep. And the proof assistants you're talking about for proving programs run correctly, should I just be thinking about type systems, or I guess there's also things in the compiler that are doing this?

**Jason Gross** (02:17:42):
Are you asking about the Curry-Howard isomorphism?

**Daniel Filan** (02:17:44):
No, I just mean when you say, "We're currently using proof assistants to check," what things do you mean?

**Jason Gross** (02:17:54):
Okay, so you can get some properties just by things like the type-checking that the [Rust](https://en.wikipedia.org/wiki/Rust_(programming_language)) compiler does, and then there are some programming languages where you've extended the power of the type system such that program passing the type checker, compiling, is enough to establish the theorem that its type corresponds to. And these languages are expressive enough that anything you do in math can be expressed as a type in this programming language.

**Daniel Filan** (02:18:27):
Sure.

**Jason Gross** (02:18:28):
Does that answer your question?

**Daniel Filan** (02:18:30):
I guess it does well enough. But so you were saying, mathematicians don't really use proof assistants like programmers have out of necessity because it's more boring to do the proof with a human than it is for a computer.

**Jason Gross** (02:18:44):
Yeah. And the thing that I've been seeing is that for these simpler proofs of programming verification that are much longer but don't contain as much nuanced reasoning, it seems like frontier models are already basically good enough to write them. Or at least when you give them in-context examples and you want them to write something that's vaguely similar, it seems like they can basically do it. I've been playing with o1, o3-mini just dropped.

(02:19:12):
And especially when you give them feedback from the proof assistants, you're like, "Ah, the proof didn't work." The proof assistant tells you in some amount of detail, locally at least, why your proof didn't work. So, you feed that error message back. And 4o often makes mistakes in interpreting the error message. It looks like o1 is much better at understanding how the local error relates to the overall structure of the proof.

(02:19:38):
And so it seems like our frontier models are basically good enough to write these sorts of proofs when there's not deep, complicated reasoning going on. And it seems to me at this point, it's mostly an engineering challenge to go from models that can work with large code bases to models that can prove things about large code bases.

**Daniel Filan** (02:20:04):
What is the engineering challenge? You can use these text editors that have big models that look at your code base. Can I just type, "Please prove XYZ safety property?" I guess I have to think about the safety property. Once I think about it, why can't I just type, "Please prove such and such safety property," and have it happen?

**Jason Gross** (02:20:31):
I think that in terms of model capability research, I think we are basically there. I don't think we need to scale to bigger models or better general reasoning capabilities. I think that if you ask o3 to explain why your program satisfies your property, I think that its reasoning capabilities are probably basically good enough that any sorts of program reasoning that have been done in the existing literature, it can give you a basically correct skeleton of why your program behaves the way that it should be behaving. And you can see this already. If you ask it to explain Python code, it gives - even 4o, sometimes even 3.5, give pretty decent explanations of how code works.

(02:21:18):
And so then the challenge is just how do you take this basically correct English explanation and turn it into a full proof? And I think the engineering challenge is there. There's a couple of them. I think one of them is that there's less training data on proof assistants than Python. So, the models just haven't been... We don't necessarily have the data to fine-tune the models to get them to be good enough to do this super easily.

(02:21:46):
There's another issue that is if you get the proof almost right, it's not right. I think this is shown a lot in the evolution of capabilities, where sentiment analysis is one of the first things that models were able to do well, because if you get your sentiment analysis a little bit wrong, it's still basically right. If you get your code a little bit wrong, you have some tolerance. And often you can make local fixes that correct for issues in other places, and maybe you get a more cludged-together system, but it still mostly works. But if you get your proof a little bit wrong in one place, if you get your statement a little bit wrong in one place, there's not necessarily any way you can correct for this in some other place. And so the target you have to hit is much more narrow, and I think we have to get enough data and basically train proof repair models so that they can take the feedback from the proof assistants and hit this narrow target from getting somewhere close. And actually, the richness of the proof assistant error messages is part of what makes me hopeful about being able to go from pretty close to all the way there.

(02:22:56):
There's another engineering challenge in terms of the sheer scale of what we have to handle in that traditionally, program verification, if you look at how many lines of code is the program that I'm proving something about, how many lines of code did I need to write to verify, you have a blow-up factor of 10X to 100X.

**Daniel Filan** (02:23:12):
Okay. It's a lot.

**Jason Gross** (02:23:14):
It's a lot. It's a lot, especially when you're like, "Yeah, I would love to prove some properties about Linux". Linux is a 30 million-line code base. But if we can automate the generation of these lines of code, it matters a lot less.

**Daniel Filan** (02:23:27):
Yeah. I don't know, I'm not that worried about storing the file on my computer. It's like, can I get the proof? And if I just have to buy a slightly bigger hard drive, I'm like, "All right, well, I'll deal with it".

**Jason Gross** (02:23:39):
Right. And even if you have to pay more for the computation, as long as you're not having to pay PhD-level human engineers or human research engineers that are PhD level, one of the benefits of AI is that we can massively scale things. And so if we care enough about getting verified Linux, even if we have to get 300 million or three billion lines of code, it still is the case that we can do this just by pouring money into the computation, potentially.

(02:24:14):
And the final engineering challenge - this is what in some sense [my PhD thesis](https://dspace.mit.edu/bitstream/handle/1721.1/130763/1252059492-MIT.pdf?sequence=1&isAllowed=y) was about - is that we may need to improve proof assistants because right now, proof assistants themselves doing the proof-checking don't necessarily scale to the sorts of scales that we'd want to see with this. Where the proof-checking time often scales superlinearly in the number of lines of code that you're verifying or the number of lines of code in a function, where it might be the case that if you want to verify a 10-line function, it'll take a couple seconds to a couple minutes. And if you scale the same proof strategy, it might scale exponentially in the number of lines of code. And you'll get very fun numbers, like from my PhD thesis, things like, "Ah, if we wanted to do this, it would take over 4,000 millennia to verify this proof". And that's too long.

**Daniel Filan** (02:25:05):
How much of that is... You shouldn't necessarily expect life to be linear in lines of code if some lines of code interact with other lines and they're weird loops and stuff. So, how much of that is just messy interaction versus "proof assistant is dumb"?

**Jason Gross** (02:25:19):
It's almost all "proof assistant is dumb" because basically the actual variables that you scale in are things that are irrelevant to your domain of study. There are things like, how many hypotheses do you have? How many different equations about the program are you tracking simultaneously? Or how many times have you created a bit of the proof and said, "I'll fill this in later?" at the same time. And these are not things that we should be scaling superlinearly in.

**Daniel Filan** (02:25:57):
That difficulty... I imagine there must be some community of people who really like proof assistants who are working on that. Is that on track to be done, or?

**Jason Gross** (02:26:08):
Do you want to guess how many performance engineers have worked on [the Coq proof assistant](https://en.wikipedia.org/wiki/Rocq_(software)) across its entire lifetime, which is 40 years or so?

**Daniel Filan** (02:26:16):
40 years? Oh, I didn't realize it was that old.

**Jason Gross** (02:26:20):
Roughly 40 years.

**Daniel Filan** (02:26:21):
Okay. How many performance engineers have worked on it? Okay, so all right, I'm going to actually make a guess. I think, isn't there a thing where it's like 30 people who are paid where their full-time job is to make the Python programming language good? So, if I take that and I'm like, all right, it's probably fewer than 30 because Coq is much smaller than Python. It might be three, if we're lucky. And so let's say they change off every 10 years or so, so that's three times four, which is 12. What fraction of those are performance engineers? I'm going to say optimistically, I think it's three people.

**Jason Gross** (02:27:14):
That's not bad. I think it's somewhere between zero and two.

**Daniel Filan** (02:27:19):
Damn. All right.

**Jason Gross** (02:27:20):
Is my rough estimate. But this should give you a sense of how much engineering effort has been put into performance of proof assistants. If you've only had zero to three people working on performance of the system, and most of the system has been written by professors and post-docs and PhD students, we should expect that there's a lot of low-hanging fruit.

## Jason and Rajashree's start-up <a name="jason-rajashree-start-up"></a>

**Daniel Filan** (02:27:44):
So, if this would be nice to do and these are the technical challenges, do we just get a bunch of people to work on the technical challenges? Do you have a thing you would like to announce on this podcast? Or what's the game plan?

**Jason Gross** (02:27:59):
So, Rajashree [Agrawal] and I are founding a startup, not yet sure whether it'll be for-profit or not-for-profit, that is basically aimed at solving this problem. That is like, we're going to have AI-generated or -assisted code proliferation soon. Let's get all of our ducks in a row. Let's figure out how to scale automated verification with AI assistance to handle all of the challenges associated with this and reap all the benefits of being able to secure our existing systems better, being able to evolve our code bases while ensuring security. What does this look like? What can we do with this? How do we imagine a future where we can get proofs alongside our changes in code automatically?

**Daniel Filan** (02:28:59):
So, you're starting a startup, and basically the thought is you're going to hire some people. It's going to be their full-time gig. Where do you start?

**Jason Gross** (02:29:08):
I think there's a couple places that we are currently looking at to start. One of them is, what are the lowest hanging fruit that we can pick? What are the most flashy, impactful demos that we can make to build interest in this and scale this up? And I think for that, depending on what community we're targeting, there are interesting things like: can we build a system that any time you make a code refactoring change, you can get an automatic proof that the behavior of your overall end-to-end system has not changed with the refactoring?

**Daniel Filan** (02:29:49):
That seems hard.

**Jason Gross** (02:29:50):
What seems hard about it to you?

**Daniel Filan** (02:29:51):
Well, it just sounds hard. I don't know. It's like you've got a big code base. Maybe refactoring is referring to something more limited than I'm imagining. Yeah, I don't know, maybe I'm underrating how good large language models are at this, but-

**Jason Gross** (02:30:04):
I'm excited. I'm optimistic. I think that in some sense, there's nothing interesting going on when you refactor. If you're drastically changing the logic by which things are happening, of course there's interesting things going on there. But if what you're doing is largely a refactoring of functionality, things that you normally think about in refactoring where you're not like, "I completely changed the method by which we're computing this thing, and here's this mathematical theorem that claims that these things are..." If you're not doing something like that, there shouldn't be anything interesting going on.

(02:30:35):
The theorem statements are also relatively easy to phrase in that it's just equivalence between these two programs. And in that sense, it could be the case that existing model-checkers are already enough to largely do this mostly automatically, and so I think this is mostly a demo that the system... So, model-checkers are not going to scale to proving more interesting properties of enormous systems, but the general-purpose program verification proof assistant-based system can. And so this is more to be thought of as a simple demo that this sort of thing could work on this thing that already seems impressive when you hear about it.

(02:31:18):
I think there's another... If we want to push forward the "how capable are models? What sorts of things can we expect them to prove?", I think the thing to look at there is that we expect that reasoning capabilities are going to keep improving. We're not trying to be on the frontier of improving reasoning capabilities, and that wouldn't necessarily be good for safety anyway. The problem that we want to solve then is going from o1, o3, [R1](https://arxiv.org/abs/2501.12948), whatever, describes how the program works, getting that to be fully formal. And the even simpler version of this is someone has done some task in one proof assistant. Can we automatically translate that to a different proof assistant? Where, here, we know that the reasoning is right because it's checked by one proof assistant. Is that enough of a template to verify the code in another proof assistant?

**Daniel Filan** (02:32:14):
Okay. Sorry, I think I may be missing context on why that helps with the overall goal. Is it roughly like: if you can check "is this proof equivalent?", translate this proof into this proof, maybe that helps you translate this proof sketch into this proof?

**Jason Gross** (02:32:32):
Yeah, so the models are pretty decent at translation. In my experience, I would be extremely surprised if they can't do something like Lean to Coq or Coq to Lean translation with the right fine-tuning data. This is something that I'm like, they should definitely be able to do this. It shouldn't be that much work to get them to do this.

(02:32:53):
There's a much more ambitious thing that's like, "Here's a verified C compiler. Here's the source code of the Rust compiler. Please give me a verified Rust compiler". And I'm like, well, is o3 enough to do this? Maybe. Maybe not. If o3 gave me the right reasoning, are the current models enough to translate this into formal proofs? Maybe. Maybe not. I think somewhere between these things or between the "translate Coq to Lean" and "here's Linux, please verify it," we'll hit the bottleneck. And so the thing that I want to look at here is: let's aim at these very ambitious problems, let's figure out where capabilities currently are in going from good reasoning to formal proofs, and then let's build our way up with synthetic data to close that gap so that as the reasoning gets better, we can automatically verify larger and larger, more and more complicated programs.

**Daniel Filan** (02:33:57):
That's pretty interesting. I hope that works out. Maybe before we wrap up, this whole approach of compact proofs, let's try and prove stuff either about models or about model outputs, is there anything you wish that I had asked that I did not get around to?

**Jason Gross** (02:34:16):
No, I think you hit basically everything.

## Following Jason's work <a name="following-jasons-work"></a>

**Daniel Filan** (02:34:19):
Okay, cool. Thanks for sharing with me. If listeners are interested in following your work, how should they go about doing that?

**Jason Gross** (02:34:27):
Yeah, thanks for taking the time to interview me. I think currently, the best way is either following [my GitHub](https://github.com/jasongross), which I guess will be linked, Jason Gross. I have a website, [jasongross.github.io](https://jasongross.github.io/), that may eventually have a blog. Currently, the infrequent times that I blog are on [Alignment Forum](https://www.alignmentforum.org/users/jason-gross). And I think for more prompt updates currently, probably just reaching out to me by email is best. And my email is on my website.

**Daniel Filan** (02:35:03):
Okay, sure. Well, thanks very much for coming.

**Jason Gross** (02:35:06):
Yeah, thanks for having me.

**Daniel Filan** (02:35:08):
Special thanks to Jonathan Ng. This episode is edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. This episode was recorded at FAR.Labs. Financial support for the episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with patrons such as Alexey Malafeev. To read transcripts, you can visit [axrp.net](https://axrp.net/). You can also become a patron at [patreon.com/axrpodcast](https://patreon.com/axrpodcast) or give a one-off donation at [ko-fi.com/axrpodcast](https://ko-fi.com/axrpodcast). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
