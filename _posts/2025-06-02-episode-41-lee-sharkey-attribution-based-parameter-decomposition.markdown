---
layout: post
title: "41 - Lee Sharkey on Attribution-based Parameter Decomposition"
date: 2025-06-02 20:25 -0700
categories: episode
---

[YouTube link](https://youtu.be/ZmJ-ov2TywM)

What's the next step forward in interpretability? In this episode, I chat with Lee Sharkey about his proposal for detecting computational mechanisms within neural networks: Attribution-based Parameter Decomposition, or APD for short.

Topics we discuss:
 - [APD basics](#apd-basics)
 - [Faithfulness](#faithfulness)
 - [Minimality](#minimality)
 - [Simplicity](#simplicity)
 - [Concrete-ish examples of APD](#examples)
 - [Which parts of APD are canonical](#which-parts-are-canonical)
 - [Hyperparameter selection](#hyperparam-selection)
 - [APD in toy models of superposition](#apd-in-tms)
 - [APD and compressed computation](#apd-comp-comp)
 - [Mechanisms vs representations](#mech-v-rep)
 - [Future applications of APD?](#future-apps)
 - [How costly is APD?](#how-costly)
 - [More on minimality training](#more-min)
 - [Follow-up work](#follow-up-work)
 - [APD on giant chain-of-thought models?](#apd-on-cot)
 - [APD and "features"](#apd-and-features)
 - [Following Lee's work](#following-lees-work)

**Daniel Filan** (00:00:09):
Hello everybody. In this episode, I'll be speaking with Lee Sharkey. Lee is an interpretability researcher at Goodfire. He co-founded Apollo Research, which he recently left, and he's most well-known for his early work on sparse autoencoders. Links to what we're speaking about are available in the description. There's a transcript available at [axrp.net](https://axrp.net/). You can tell me what you think about this episode at [axrp.fyi](axrp.fyi). And you can become a patron at [patreon.com/axrpodcast](https://patreon.com/axrpodcast). Well, let's continue to the episode. Well, Lee, welcome to AXRP.

**Lee Sharkey** (00:00:40):
It's good to be here.

## APD basics <a name="apd-basics"></a>

**Daniel Filan** (00:00:41):
So today, we're going to talk about this paper ["Interpretability in Parameter Space: Minimizing Mechanistic Description Length with Attribution-Based Parameter Decomposition"](https://arxiv.org/abs/2501.14926). It's authored by [Dan Braun](https://danbraunai.github.io/), Lucius Bushnaq, [Stefan Heimersheim](https://heimersheim.eu/) - those three being, I guess, joint first authors - Jake Mendel and yourself. So I guess, how would you summarize just: what's this paper doing?

**Lee Sharkey** (00:01:03):
So I would say that this paper was born out of two lines of thinking, one primarily coming from what I was thinking about and one coming from where Lucius was thinking about. And where I was coming from was: we'd been working with SAEs - sparse autoencoders - for some time. The community got quite excited about them and we'd just been thinking about them quite a lot and noticing a bunch of conceptual and ultimately practical issues with them. And then, the line of thinking that Lucius had been thinking about was a potential area of research that might form a foundation for decomposing neural networks. And what this paper does is basically bring those lines of thinking together. And the whole thing that we're trying to achieve here is just breaking up the parameters of the network instead of its activations.

**Daniel Filan** (00:02:16):
Fair enough. When you say "break up the parameters of the network," if I look at the paper, you have this APD method, and the core of it is this objective of "here's how we're going to decompose the network." There are these three parts of the objective. Can you walk us through just what those are?

**Lee Sharkey** (00:02:45):
Yeah. So as I mentioned, the whole goal here is to break up the parameters of a network into different components. And this is necessary for understanding what the objective of this algorithm is. So we have a neural network. And, as many of these networks are, they're composed of matrices, and these matrices are the parameters of the network. And, even though these are matrices, you can flatten these matrices out, and just concatenate them all together, and you just make one big vector that you call your parameter vector, and your neural network lives as a vector in parameter space.

(00:03:35):
And what the method does is it basically supposes that you can break up the neural network into a bunch of mechanisms and those mechanisms sum together to the parameters of the original network. And so, what we want to do then is start with a set of parameter vectors that all sum to the... Well, initially, they sum to a random vector because they're randomly initialized. But we basically want to optimize these parameter components - the components of this sum - and we want to sum to the original network. And we optimize them such that, one, they do actually in fact sum to the parameters of the original network. Two, that as few as possible of them are used on any given forward pass. And three, that they are in some sense simple, that individually they don't use very much computational machinery.

(00:04:37):
Because one of the ways that you might have a set of parameter vectors that sums to the parameters of the original model is just to have one parameter vector that is in fact the parameters of the original network. And as few as possible of these are used in any given forward pass, because it just uses one of these parameter vectors. But it's not very simple, right? You haven't really done very much work to decompose this into smaller steps that you might more easily be able to understand. And so, you want these individual parameter components to be simple, as well as faithful to the original network that is the sum, and also minimal: as few as possible of them are necessary on any given forward pass.

**Daniel Filan** (00:05:24):
Got you. So one thing that immediately struck me about this idea is... So it's presented as, "Ah, SAEs have these problems, and so we're going to do this thing." And it strikes me as almost just a sparse autoencoder for the network, right? What's a sparse autoencoder? Well, you have this activation layer, and you want to have something that recreates the activation layer, and there are a bunch of components that should sparsely activate on any given thing. And if you train it with L1 loss or L2 loss or something, somehow you're supporting simplicity as well. I'm wondering: how much do you think there is to this analogy?

**Lee Sharkey** (00:06:14):
I do think there are many parallels for sure. I wouldn't want to overstate them, because I do feel much more satisfied with the APD direction. But, as you point out, there are many similarities. You might think of SAEs as, in some sense, minimizing the description length of a given set of activations. You want to be able to describe in as few bits as possible a given data set of activations in any given layer. But yeah, the method focuses on a slightly different object. It focuses on parameter space, rather than activation space. And in that sense, [it] focuses more on the computations, rather than the results of the computations.

(00:07:13):
But it's not a coincidence that we've been thinking about SAEs for some time, and then we come up with this direction. There are some deeper similarities there. But I think that the core similarity is that whenever you're describing a neural network, you in some sense want to use as few objects as possible, because in that way you're going to be able to break it up into individually more understandable or simpler chunks. And the hope then is that if you understand those chunks, you can understand the rest of the network as well. And so both of them rely on that principle, but act on different objects and a few other differences as well.

## Faithfulness <a name="faithfulness"></a>

**Daniel Filan** (00:07:57):
Sure. So I think just to get people to understand what APD is, I think it's actually helpful to go through the three parts of the objective and talk through them. So I guess the first part is: you have this set of vectors in parameter space, and they have to sum up together to make the whole network.

**Lee Sharkey** (00:08:21):
Yep.

**Daniel Filan** (00:08:22):
I think the first thing that strikes... or at least it struck me as somewhat strange, is: because you're looking at vectors in parameter space, rather than subsets of the parameters of the neural network, you're allowed to say, "Oh yeah, this mechanism is three times this parameter, minus two times this parameter, plus half of this other parameter." At first blush, it seems like there's something kind of strange about that. And I'm wondering: firstly, are there other parts of the objective that mitigate against this sort of thing? Or, if there aren't, is this just the thing that ought to be done?

**Lee Sharkey** (00:09:08):
I'm inclined to say that implicitly there are... I think what we will find - and we do, to some extent, find this in some of our experiments - is that even though networks try not... We don't want our understanding of a neural network to, in some sense, privilege some basis, either in activation space or in parameter space. We don't get to presume a fundamental basis. We have to go and find that basis, either in activation space or parameter space. However, you might be familiar with the idea of privileged bases. This is the idea that because of the activation function serving as these non-linearities, certain bases might be preferred. And in particular, bases that somewhat align with neurons, although not equivalent to the neuron basis.

(00:10:14):
So it does feel likely to be the case that, because neural networks seem to have some tendency to align with the neuron basis under some data distributions and some training objectives, I would guess then that if those bases are indeed privileged in the network, APD should be able to recover them. And thus, implicitly has a bias toward... If it has a bias toward finding true things in the network and the network privileges some basis, then it should ultimately find that. But I'm not sure if it does have a part of the objective that biases it toward that explicitly.

## Minimality <a name="minimality"></a>

**Daniel Filan** (00:11:10):
Fair enough. So I guess the next thing I want to talk about, that's somewhat distinct to the method, is the second part: optimizing for minimality. So concretely, how does this work? What are you actually optimizing for here?

**Lee Sharkey** (00:11:29):
So we came up with a couple of different ways that you might be able to do this. And we use one in particular that we call the "top-k method". So we have this set of parameter components that we're training, and we want them to each individually take the form of one individual mechanism of the network. And we want these mechanisms to have the property such that as few as possible of them are necessary on a given forward pass. And so, the way we optimize for this then is that we have two forward passes and two backward passes. So on the first forward pass, we use the summed parameter components, which are approximately equivalent to the parameters of the original network.

(00:12:26):
And then, on the first backward pass, we take the gradients of each of the output dimensions with respect to the parameters. And the idea here is that we use these gradients as attributions of which of these parameter components was most influential over the output, and so, in some sense, which of these parameter components is most causally responsible for the output. And then we take those attributions - each parameter component has a number that is some approximation of how important it was for the output...

**Daniel Filan** (00:13:09):
And is that number just the sum of the absolute values of all the attributions?

**Lee Sharkey** (00:13:17):
It's a slightly more complicated formula. Basically, you take some inner product with the parameter components themselves and the gradients, but you can conceptually approximate it with that. It's roughly that idea. And so, basically, you have this number that tells you approximately how important this parameter component was for the output. And then you say, "Well, I'm going to only take the top k most important parameter components." And then, you do a second forward pass, only using the top k most important parameter components.

(00:13:56):
And what this should do, whenever you train the output of that second forward pass to be the same as the original model, is that it should update these active parameter components, such that they become more important on this forward pass for that data point. So they basically should increase their attributions on this data point, compared with before the gradient update. And the gradient update is just the second backward pass. So yeah, that's basically what the four steps of that training step do.

**Daniel Filan** (00:14:39):
Got you. So I guess my main question is: it seems like the fundamental thing here is minimizing the number... not the number of mechanisms total, but the number of mechanisms that are relevant for any single forward pass of the network. I think when I first came across this idea, it just wasn't at all intuitive to me why that should be the minimality that's necessary, rather than just minimizing the total number. What's going on there?

**Lee Sharkey** (00:15:18):
Yeah. So I'm just trying to understand what the confusion is. So I think the way maybe to think about it is that if I wanted to minimize just the number of parameter components that I used on any given forward pass, one thing I might do is, as we were discussing earlier, we may just use the parameters of the original network. Of course, this isn't satisfactory, because it doesn't break up the parameter components into something that is simpler than the original network. So already, we don't get to just minimize the number of parameter components that are active on a given forward pass. So you might then imagine that there is a Pareto frontier of how many parameter components I've split up the network into versus how simple they are. And for a given level of simplicity, I'm going to require a certain number of parameter components on a given forward pass. But, yeah, you don't really get to... Maybe you can spell [out] the question a bit more.

**Daniel Filan** (00:16:46):
Basically, my question is: so in actual APD, one of the things you're optimizing for is that on any given forward pass, there should be few components active, but on different forward passes... Maybe on this forward pass you have mechanisms 1, 3, and 5 active. On this [other] forward pass, you have mechanisms 2, 4, and 6 active. And then you're like, "Ah, this is pretty good." But you can imagine a world where you say, "Hey, I just want there to be as few mechanisms as possible for all the inputs." Right?

**Lee Sharkey** (00:17:25):
Yeah.

**Daniel Filan** (00:17:26):
So in this hypothetical network where you have 1, 3, and 5 on this input, [and] 2, 4, and 6 on this [other] input. For APD, you're saying, "Oh yeah, it's only using three mechanisms for any forward pass." But you could have a hypothetical method that's saying, "Ah, that's six mechanisms that are being used in total and I want to minimize that number." So why is it the per forward pass number that we want to minimize?

**Lee Sharkey** (00:17:50):
Yeah. I think it is in fact the other one that you want to minimize - you do want to minimize the total number, because we're ultimately averaging the gradient steps over batches, such that it will on average point toward a configuration such that if you get to share parameter components between these different data points - if you have a data point that has 1, 3, and 5, and another one that has 1, 4, and 6 - this one should be favored over the one where you just get to split up one into two different mechanisms that are active on both of these data points. I guess what I'm saying is that you basically do want to optimize for cases where things are shared, and thus where there are as few mechanisms as possible over the entire data set. You just happen to be doing this batch-wise over individual data points.

**Daniel Filan** (00:19:01):
My understanding of the method is: so you have this batch of inputs, right? And what you do is in a batched way, for each input you take the top k, you don't really, you do batch-top-k, but that's just an implementation detail.

**Lee Sharkey** (00:19:18):
Yeah.

**Daniel Filan** (00:19:18):
So for each of these inputs, you take the top k mechanisms that are being used. And then, you do a backward pass where you're optimizing for, on each input, the top k things that were used for that input. [They] are basically optimized to better reconstruct the output of the network on that particular input.

**Lee Sharkey** (00:19:50):
Sure.

**Daniel Filan** (00:19:51):
And so, I don't see... Mechanistically, if I have the 1-3-5, 2-4-6 case, right? I don't see how that's going to be optimized for, "No, actually you should use the same few things." Because you're just taking the top k for both of them, right? I don't see where the gradient term would be for things to share mechanisms. In the 1-3-5, 2-4-6 case, I think you just upweight 1, 3, 5 on the first input and upweight 2, 4, 6 on the second input, right?

**Lee Sharkey** (00:20:28):
Yeah. It might be useful to think of a concrete example. So maybe the [toy model of superposition](https://transformer-circuits.pub/2022/toy_model/index.html) model might be a useful example here. So the toy model of superposition is a model developed at Anthropic. It's a very simple model where there are sparsely activating input features, and just a few of these are active on any given input. It's also just a very simple model. It's got a weight matrix. And that weight matrix, I suppose, it has as many rows as the number of sparsely active input features. And it has this basically smaller number of columns. So it's basically a down projection matrix from data space into this hidden space. And then, this down projection gets up-projected by this matrix whenever you... You could use, in theory, a different matrix, but you can just transpose the matrix, and that spits out an output after you pass it through a ReLU activation. And you basically want to reconstruct the input data. And there's a bias in there.

(00:21:58):
So suppose then you have some input datum, which has the first, and the third, and the fifth features active. These are in fact active inputs. But in APD, you haven't yet learned whether or not there is a... Your parameter components haven't yet learned to correspond to these input features. Maybe one thing then to think about is: well, suppose you have a bunch of different parameter components. Because they're not aligned with the features, in some sense, there's too many of these parameter components active. And APD is designed to learn only as many parameter components as are necessary to explain whatever the network is doing on this data distribution. And you basically don't want it to learn more parameter components than are necessary. And this is what you achieve by both optimizing for minimality, such that as few as possible are necessary, and that they are as simple as possible.

(00:23:41):
And so, suppose you have two parameter components, where even though the ground truth feature one is active, two of your parameter components are in fact active. One is maybe slightly more active than the other. But you don't want this, because ultimately you want to learn only one of these parameter components per input feature. And, the idea then is that in some of these cases where this input feature is active, and one of these parameter components is more active than the other - because it would be statistically unlikely that they are equally active - there will be cases where, because you're thresholding this, the one that does get active will get updated, such that, it in future cases like this, where the feature one is active, it gets more attribution, versus cases where it doesn't. I'm not sure if this fully addresses your concern. But, I guess I'm pointing at: if there is a ground truth where one thing is active and two parameter components are then active, then this is something that we do in fact get to avoid by [optimizing] for minimality and also for simplicity.

**Daniel Filan** (00:25:03):
Right. So I think the image I am getting from your answer - which might be totally wrong, so tell me if it's wrong - but I think the thing you're saying is: okay, probably for every input there are in fact some parts of the network that are more active on that input. And I think you're almost saying: imagine there is some ground truth decomposition that's not super big, right? Well, if I have input A and input B, right? And they in fact do use many of the same mechanisms, then basically APD is going to be disincentivized from the 1-3-5, 2-4-6 solution, just because you're picking few mechanisms active on any given thing, but you're trying to make it mimic what the network actually does. And so, if the thing the network is actually doing is using some of the same actual parts of the actual network, then you're going to push these 2, 4, 6 to be close to the actual mechanisms of the actual network and you're pushing 1, [3], and [5] to be close to the actual mechanisms of the actual network. So they're just going to merge basically. Is that roughly right?

**Lee Sharkey** (00:26:20):
Yeah, I think so.

**Daniel Filan** (00:26:21):
Okay. So in that case, it seems like basically, for this story to work, you're basically saying, "No, there is some ground truth decomposition, and because we're doing this thing that's getting close to the ground truth decomposition, that's what's powering our thing working," as opposed to some constructivist thing of like, "Oh, here's just the nicest way we can find of decomposing things."

**Lee Sharkey** (00:27:00):
Yeah, this is a question I haven't quite made up my mind on yet. I think, in toy models, it can be the case that you have a ground truth decomposition, because you made it that way. And the way that you might have designed this is that if someone came to you and told you, "Well, I've got an equivalent way to describe this network that you designed yourself." And their description, it uses either more components than is necessary, or it uses more complex components than is necessary, then you might say, "Well, sure, kind of. But, I think this other explanation, the one I used in my head to design this network, is better."

(00:27:54):
And in some sense then, it is more toward this constructivist way of thinking. Maybe then, there is actually no such thing as a ground truth explanation for the network, even though you designed it. And even though you said, "This is the ground truth explanation." If there are other equivalent things where more objects, more complexity was necessary, then sure, they're still explanations, but they're not as good. And, in the case of more natural networks, maybe it is also the case that even though we can debate whether or not there is some ground truth to the thing that the network is doing, the style of explanation that we most prefer is something that is the shortest, simplest explanation for what the network is doing.

## Simplicity <a name="simplicity"></a>

**Daniel Filan** (00:28:44):
So I think, before we go further into the philosophy of APD, I think I want to just get through the parts so that people fully understand. So the third component of this objective function is simplicity. You're optimizing each component to be simple. Can you tell us: what's "simple"?

**Lee Sharkey** (00:29:06):
So the way we defined "simple"... "Simple" is supposed to capture this intuitive notion that it uses as little computational machinery as possible. And what does it mean for a set of matrices to use as little computational machinery as possible? The definition that we settled on was that if the network consists of one matrix, that matrix is as low rank as possible. You can't get much simpler than a rank one matrix and a rank two matrix is less simple. It does more things to a given input vector. And, if your network consists of more matrices than just one, you basically get penalized for ranks in those matrices as well. So basically, the thing that we want to minimize is the sum of ranks overall of the matrices in a network. Now, I don't know, we're not fully happy with this, but we do think that this is a fairly reasonable notion of what it means to use as little computational machinery as possible.

**Daniel Filan** (00:30:41):
Yeah. So if I think about what that's saying, right? So there is something intuitive there, right? For instance, if you use fewer matrices, that should count as more simple. "Lower rank" is basically saying, your matrix is secretly over a smaller dimensional input/smaller dimensional output space.

**Lee Sharkey** (00:31:03):
Yeah.

**Daniel Filan** (00:31:04):
I think it's in some ways being basis-independent in this interesting sense, right? You're saying that the identity function, versus a weird rotation and scaling, as long as you're doing it on the same number of dimensions, those count as the same, which I think is actually plausible, given that different layers' activations functions are, in some sense... Maybe they just should be incomparable in that way. Maybe you don't want to equate these neurons with these neurons.

(00:31:39):
Maybe the other thing that seems slightly strange about that is by being basis-independent, by saying that the complexity of this weight matrix is just the rank of it... Suppose you have two components in one layer, right? By saying the complexity of both of them is the rank, somehow you're saying that the basis you're thinking about for the computation of thing A, and the basis you're thinking about for the computation for the thing B, are just not related at all. And maybe there's something there that's worth... I don't exactly know what the objection there would be, but it seems like there's possibly something there that's worth getting into.

**Lee Sharkey** (00:32:30):
Yeah, I mean, I think that's just something that we're willing to accept. In some sense, the exercise we're trying to do here is basically discretize the network into discrete objects. And ideally, we want to discretize it into objects that have as little to do with each other as possible. And, if it is the case then that we can in fact just distinguish between one kind of operation and another - sometimes that operation is used and on other data points it is not - then I think we're okay with that. But one of the reasons that APD was developed was the case of multi-dimensional features. And the idea of a multi-dimensional feature is that, well maybe you don't get to just break things up into rank one components, maybe you actually do in fact need more than one. So the classic example here is the days of the week features, where the days of the week lie on points on a circle.

**Daniel Filan** (00:33:41):
And crucially they're in the right order, right? It's Monday, then Tuesday, then Wednesday.

**Lee Sharkey** (00:33:46):
Yeah, exactly. And, in order to describe these features, sure you can describe them as seven different directions in activation space, but you can more succinctly describe them as two-dimensional objects, basically. And, if you want to understand the operations that are done on those, it might just be useful to think of them as two dimensions, rather seven one-dimensional objects. But the idea is that we want APD to be able to decompose networks into chunks that if they do have these computational units that are best thought of as two-dimensional, rather than one-dimensional, that it can indeed find those, and isn't just decomposing things into too many objects.

## Concrete-ish examples of APD <a name="examples"></a>

**Daniel Filan** (00:34:50):
Fair enough. So I guess I next want to just talk about... So I want to test my intuitions for "do these objective functions make sense when I compare against certain examples?" So the first example I want to ask about is: suppose I have a decomposition of the network that's just, each component is one layer of the network: component one is the first layer, component two is the second layer, component three is the third layer. I feel like that might score well on APD, as long as you're allowed that many components.

(00:35:32):
So, my reason for thinking this is: basically, you have the cost that each matrix is full rank, but unless there are... I guess it's possible that there are unused dimensions in the network that you could prune off. Sometimes in ReLU networks, some neurons will die. So yeah, suppose you're taking the weight matrices but you're pruning off the dead ReLUs. It seems like that might actually be optimal as long as you're allowed that many components, just because it's a perfect recreation of the network, and no other way of mixing around the things is going to save on the total rank, because you just need that much rank total. Is that right?

**Lee Sharkey** (00:36:17):
It depends on the data distribution. So, there is a case where it is, right, but it's a fairly strange case. Suppose you have a data distribution where for every input, all of your ReLUs in your three layers are always active. So fine, you've pruned off the dead ones, those never activate, but on the other ones that do activate, they're always active. So everything is always above threshold. And so what you've really got here is just three linear transformations. And in that case, you don't really get to summarize that any more than just describing the three linear transformations, because on every given input in our data distribution, there's always some non-zero amount that each rank is used. Fine, there's going to be some infinitesimally small number of cases where it's perfectly orthogonal to some of the smallest singular dimensions of some of these matrices, where in that very small number of cases, that an activation that aligns with that dimension, the attribution will be zero. But in almost every case, all of the ranks will be used.

(00:37:42):
Now you can imagine for certain other data distributions... Well, I guess maybe one way to think about it is that that wouldn't be a very useful neural network, because it's just doing the same transformation to every input, and that you might as well just use one linear transformation. The interesting thing about neural networks is that they can do different transformations to different inputs. And in that case then, in some inputs, you may use transformations that go one way, and on other inputs you may use transformations that go another way. That's the kind of thing that you want to be able to break up using APD.

**Daniel Filan** (00:38:26):
Right. Sorry, if I think through this example, it seems like... Suppose you have these alternate set of mechanisms, right? This alternate decomposition, where on this input, we're only using this half of the neurons, and on this [other] input we're only using this half of the neurons. At first... Tell me if I'm thinking about this wrong. It seems like this is actually a case where minimality per input is actually buying you something, because in my imagination you're still using the same amount of rank, and maybe you still have the same total number of things, but the thing you're saving on is in the per layer thing: every layer is active on every input, right? But if you can break it up with "oh, this is only using a subset of the neurons, so I only need this subset of the mechanisms," it seems like maybe the thing I'm saving on there is... Maybe it's rank, and maybe it's number of components, but on a per input basis, rather than over all the inputs.

**Lee Sharkey** (00:39:36):
Yeah, I think that's right. So, suppose you have two components in each of these layers, and you've got three layers, and so you've got six components overall. Well, if parameter components... Suppose your data distribution is split up such that you can in fact throw away half the network that is involved in one half of the data distribution, and you can for the other half of the data distribution, throw away the other half of the network. So you can basically just treat these as two separate networks that happen to be mushed into one.

(00:40:20):
So we've got these six parameter components, and if they're lined up such that three of these parameter components correspond to one of these data distributions, and the other three correspond to the other data distribution, then yes, on some inputs, you'll be able to use only three, and on others... Well yeah, in all cases you'll be able to use just three. But if your parameter components don't line up perfectly with these distributions, you'll have to use six every time, which is just not something that you want to do, if you want to decompose it into a smaller number of active objects at any given point.

**Daniel Filan** (00:41:04):
Okay. So I think I feel satisfied with that case. I next want to just talk about: so this is a little bit out there, but to help me understand, I think it would be helpful for me to talk about doing APD to a car, right?

**Lee Sharkey** (00:41:20):
Go on. Yeah.

**Daniel Filan** (00:41:21):
So, basically because a car is an instance where I feel like I understand what the... Well, okay, I'm not actually that good at cars, but I have a vague sense of what they're like, and I think I have a vague sense of what the mechanisms in cars are. So, if I imagine taking a car and doing APD to it, I want some decomposition of all the stuff in the car that firstly, all the stuff in all of the decompositions just reconstitutes the whole car. I'm not leaving out a bit of the car. That makes sense to me. Secondly, I want there to be as few parts to my decomposition that are relevant on any given car situation. So like if there's some situation, maybe suppose we discretize time, and there's some input to me driving, and then I do a thing, and then the car... You know. Maybe it has to be a self-driving car for this to fully make sense. And then the third thing is that each component, the components have to be as simple as possible.

(00:42:24):
One concern I have is: I think when people are driving a car, usually there are a bunch of components that are active at the same time, that are basically always active at the same time, even though I think of them as different components. So one example is: there's always an angle at which the steering wheel is going, and whenever the car is on, that angle matters to how the car is going. There's also a speedometer, which tells you how fast you're going, and that speedometer is always active whenever the thing is on.

(00:43:07):
Would APD tell me that the steering wheel and the speedometer are part of the same component? I worry that it would, because I think there's no complexity hit from describing... If I describe them separately, that I have the complexity of the speedometer, plus the complexity of the steering wheel, these two things. And if I describe them jointly as a speedometer and the steering wheel, then I've got to describe the speedometer and I've got to describe the steering wheel, same amount of complexity. But in the case where I merge them, I have one component instead of two. And there's never some cases where the steering wheel is active but the speedometer is not active, or vice versa - if I understand cars correctly, maybe people have a counterexample. So, in this case, would APD tell me that the speedometer and the steering wheel are part of the same mechanism? And if so, is this a problem?

**Lee Sharkey** (00:44:13):
I think that there's a kind of, I don't know, functionalist stance that we're taking here. We want to understand a particular function of the car, and I think it might help to specify what that function is. So, suppose that function is just, "get me, a human, from A to B." So, suppose I live in a country that doesn't require speedometers, and I don't really care what my speed is, and it really just doesn't affect my behavior, and therefore it doesn't affect the behavior of the car. In this case, we can basically ablate the speedometer, and the car would go from A to B with very little changed. Now in a different context, whether or not there's a speedometer might affect the decomposition that we think is the most succinct description of the thing that is doing the driving from A to B.

(00:45:31):
A more general case might be: well, we have the engine, and we have the brakes. Now, whenever I'm moving, the brakes are not always on. And so whenever I don't need the brakes, whenever I'm not braking, I can basically ablate the brakes, and the behavior of the car, the behavior of the Lee and car system is basically going to be unchanged. Now of course if I ablate the brakes, and then do want them, there is a difference between those two worlds where I do have the brakes, and I don't, and there's some sense in which breaking it up into a thing that makes the car go forward and a thing that makes the car stop is actually a useful decomposition.

(00:46:11):
So, bringing it back to your example, I do think that it matters the kind of function that we are specifying here. And in the case that you mentioned, it might not matter whether or not you decompose the car into the engine and the speedometer, because it's all one part of... In your example there was no driver, and it's all part of one causal process. The speedometer is just basically intrinsically attached to the engine, and we therefore don't really get to chunk the system up into two different objects. But because what we're describing as the function here matters, that determines whether or not you can in one sense decompose them and in another sense not.

**Daniel Filan** (00:47:10):
Right. So maybe one way of saying this is: how do you tell the speedometer and the steering wheel are different? Well one way you can do it is you can have test cases, where you have this guy who doesn't really care about how fast he's going - which is still a little bit weird, right? Because at least back when I was driving, that was relevant to "can you turn?" But I don't know, maybe you can just figure that out by looking at the road, and being smart, right? But at the very least you can go to a mechanic, and you can get your car in some sort of test situation, where you're just checking if the speedometer is accurate by hooking it up to some car treadmill thing, and the steering wheel doesn't matter there, maybe, or vice versa. So one way I could think about this is: this shows the importance of a diversity of inputs for APD, that you've really got to look at the whole relevant input space, and if you don't look at the whole relevant input space, you might inappropriately merge some mechanisms that you could have distinguished. Is that maybe a takeaway?

**Lee Sharkey** (00:48:33):
Yeah, that feels right. It feels right that in order to decompose networks into all the distinct mechanisms, we do need to look at all the cases where those mechanisms may be distinguishable. Yeah, that feels like a reasonable takeaway.

**Daniel Filan** (00:48:58):
Sure. I guess the next thing... Actually the other thing about the car that I thought about when you were talking about it is, it seems relevant for just identifying which mechanisms are active. So, in the paper, the test for whether a mechanism is active is this gradient-based attribution, which is basically like, "if you changed this bit of the network, would that result in a different output?" Now suppose I'm driving, and I'm not using the brakes. If you change the brakes such that they were always on, then that would change my driving behavior, right?

**Lee Sharkey** (00:49:34):
Correct, yes.

**Daniel Filan** (00:49:35):
Or even in an incremental way, right? Like if you changed the brake pedal such that it was always a little bit pressed, that would be slowing me down.

**Lee Sharkey** (00:49:43):
Yeah.

**Daniel Filan** (00:49:45):
So, am I right to think that if... And maybe we're just straining the limits of the analogy or whatever, but am I right to think that if we used the equivalent of gradient-based attribution to decomposing a car, you would be thinking that the brakes were always an active mechanism?

**Lee Sharkey** (00:50:05):
I think it may be running up against the limits of the analogy, maybe. But one of the things that the gradient-based attribution is supposed to approximate is if you were to... What gradients are actually measuring is: if you twiddle the activations or the parameters in one direction, will it affect the thing with which you're taking the gradient of? I don't know, this is supposed to approximate basically "how ablatable is this direction?" You're basically saying, "If I moved, if I didn't just do a small twiddle, but did a very large twiddle from where I currently am to zero, then should it affect the thing that I'm taking the gradient of?" You're basically taking a first-order approximation of the effect of ablating. That's just what you're trying to do whenever you're taking the gradient here.

(00:51:10):
So, maybe ablatability is a way to port this into the analogy. Hence, if you can ablate the brakes, and nothing changes in that situation, then the brakes are in some sense... For this moment, the brakes are degenerate, the brakes just are not needed for this particular data point, a data point where I did not need to brake. But on data points where I was braking, I do not get to ablate the brakes and have that. The state does change quite a lot, whether I ablate the brakes or not in cases where I am in fact requiring the brakes.

## Which parts of APD are canonical <a name="which-parts-are-canonical"></a>

**Daniel Filan** (00:52:00):
Fair enough. So, I guess the last question that I want to ask just to help me understand APD is, if I recall correctly, in either the abstract or the introduction of the paper, there's this disclaimer that, "Okay, parts of this are just implementation details, and there's a core idea, and there's how you made it work for this paper, and those are not quite the same thing."

**Lee Sharkey** (00:52:28):
Yeah.

**Daniel Filan** (00:52:28):
Out of the stuff that we talked about, which parts do you feel like [are] the core important parts of the version of APD that you're interested in investigating? And which parts of it just felt like, "Okay, this is the first way I thought of to operationalize this thing?"

**Lee Sharkey** (00:52:46):
Certainly using gradient-based attributions is not something that we're wedded to at all. What they're supposed to do, as I mentioned, is just figure... It's supposed to get some measure of how causally important a given parameter component is. Now it's not the only potential method that you might consider using. You should be able to sub in any method of causal attribution there, and replace that. This is something that we're keen to replace, basically, because gradient-based attributions will have all sorts of predictable pathologies, such as... Well, I mentioned that it's the first-order approximation of causal ablation, but it is really just a first-order approximation - it's not going to be very good.

(00:53:39):
There will be cases where if you twiddle the parameters in a certain direction, the output doesn't change very much, but in fact if you ablate it the entire way, it does change a lot. A classic example of this is attention, where if you're really paying a lot of attention to a particular sequence position, your attention softmax is basically saturated on that sequence position, and even if you change the parameters a fair bit, locally, it may not change very much, but if you change them a lot, you may go from a region where you're saturated to non-saturated, and then you realize, ah, in fact this was a causally important sequence position. And so there's just lots of predictable pathologies that will arise out of gradient-based attributions.

(00:54:43):
We're also not totally wedded to the definition of simplicity that we have. We're open to other potential definitions that may be more principled. For instance, one of the main motivations in the design process of this method was not to be basis-privileged. And there are a bunch of reasons for this, but one of the reasons is that, well, representations or computations in neural networks seem to be distributed over a very large number of different things. The classic case is that you don't get to just look at an individual neuron, and understand an individual function within the network by looking at one neuron. You have to at very least look at multiple neurons. Things seem to be distributed over multiple neurons.

(00:55:44):
But it gets even worse than that. Representations may be distributed across multiple layers, in fact, especially in residual networks, where you don't really get to just look at one layer to understand something, you have to look at multiple. And the same thing goes for attention heads. Maybe, in fact, a lot of analysis looks at individual attention heads, but this is kind of an assumption. We're kind of assuming that the network has chunked it up such that one head does one thing, and there's some intuitive reasons to believe that, but there are some intuitive reasons to believe that one neuron does one thing, and there's no fundamental reason why it can't distribute things across attention heads. And there's some toy examples and some empirical evidence that this may be happening in networks.

(00:56:35):
And so there's a bunch of reasons why you might not want to be basis-privileged. And the thing that our simplicity measure... it does in fact privilege layers, because it's the sum over layers. It doesn't privilege particular ranks, but it does privilege layers, and we're open to versions of this metric that don't privilege layers.

(00:57:08):
Aside from that, the fundamental thing about this whole method is that we get to decompose parameters into directions in parameter space, and we're open to different ways [of] doing this. It's more, we hope this is just a first pass of a general class of methods that do parameter decomposition, and the kind that we're introducing to some extent here is linear parameter decomposition. We're decomposing it into something that sums to the parameters of the original network, and we think that's likely to be a somewhat powerful way to decompose networks. Not necessarily the only one, but yeah, we hope this points toward a broader class of networks, of which APD is just one.

## Hyperparameter selection <a name="hyperparam-selection"></a>

**Daniel Filan** (00:58:10):
Sure. Okay. It turns out I lied. I have another question about how the method actually works, which is: I guess obviously there are a few hyperparameters in APD training, but one that feels very salient to me is how many components actually get to be active on any given thing? So, first of all, how, in fact, do you pick that?

**Lee Sharkey** (00:58:36):
It is one of the things that we want to move away from in future versions of the model. I mentioned that we were using an implementation that is like a top-k implementation, where you are just choosing a certain value of k, and you're saying, "This is the number that is active on each data point." In fact, we use batch top-k, where you get a little bit more flexibility per data point, but you still have to say, "Over a batch of a given size, we still want on average there to be only k active per data point." And that's a hyperparameter that is like... One of the main issues with the whole method is that it's currently still pretty hyperparameter sensitive, and this is just one of the hyperparameters, that if you manage to get rid of, then you may arrive at a more robust method.

(00:59:40):
The way that we choose it is basically, because we've got toy models, we have ground truth, and we can know whether or not the method is doing the right thing, and we can basically search for the right number of values of k, such that it yields the ground truth mechanisms. But yeah, we want something that's more robust, such that if you didn't know the ground truth mechanisms, you could just choose an okay value for the hyperparameter and you could rest assured that you should end up with something approximately right.

**Daniel Filan** (01:00:11):
Right. So one thing that occurs to me is: so in the title, it says "Minimizing Mechanistic Description Length With Attribution-Based Parameter Decomposition", and you present it as a part of this minimal description length... part of this family of things, where you're trying to run some algorithm to describe stuff, and you want to... It's related to all these ideas of Solomonoff induction and stuff.

(01:00:43):
And I thought that one of the points of minimal description length-type things was that it offered you this ability to have this principled choice of how to choose hyperparameters, or at least these sorts of hyperparameters. I think of MDL as saying, "Oh, when you're doing regression, you can model it as a degree one polynomial, or you can model it as a degree two, or degree three," and you have this trade-off between fit and something else, and MDL is supposed to tell you how many degrees of your polynomial you're supposed to have. Right? In a similar way. I would imagine that it should be able to tell you, "okay, how many components are you supposed to divide into?" I guess you must have thought of this. Does that actually work?

**Lee Sharkey** (01:01:34):
The story's a little bit more nuanced, in that minimum description length, whenever you're dealing with, say, some continuous variables, you may have to fix one of your continuous variables, and say, "For a given value of this continuous variable, how few can I get in these other variables?" And in the case of an SAE you might say, "for a given mean squared error" or how low can I get basically the description of the set of activations, where that depends on how many things are active for a given data point, and how many features I've used in my sparse autoencoder dictionary.

(01:02:25):
The same thing kind of applies in APD. You need to fix some of your variables. So, the mean squared error is one of them. If you really want your mean squared error to be very, very low, you might get to ablate fewer parameter components, because you'll just predictably increase the loss if you ablate things, even if your parameter components are perfect. But there are also some other continuous variables here. Even though we're trying to minimize the rank. Rank is a non-differentiable quantity. What we are in fact getting to minimize is basically the sum of the singular values of the matrix. This is what we call in the paper the "Schatten norm".

(01:03:28):
That's just the name of the quantity. And so, this is a continuous approximation of the rank. Basically, if you minimize this, you minimize the rank. But it's not a perfect quantity. But this is our measure of simplicity, and we kind of have to say, "for a given level of simplicity, how few active components do we get to have?" So there's a lot of degrees of freedom that we have to hold constant, such that we can hold them constant and say, "how well can I do, in terms of minimum description length?" But yeah, we basically want to get toward a method such that we hold these things constant at a sufficiently low level, that we don't have to really worry that we're introducing arbitrary choices.

**Daniel Filan** (01:04:31):
Right. So in terms of, okay, you've got to balance against the loss... I had this impression that for a lot of these mean squared error losses, you could actually think of it as the likelihood of something, and end up measuring it in bits. So it makes sense that you would have to think about singular values, rather than literal rank, because in the presence of any noise... every matrix is full rank. Right?

**Lee Sharkey** (01:04:57):
Yeah.

**Daniel Filan** (01:05:04):
So you are dealing... One thing going on with description length-type things is that description length is inherently a discrete concept, like how many bits are you using to describe a thing? And if the thing is continuous, it's like, well, at what resolution do you want to describe it? And I think this ends up being a hyperparameter, but a hyperparameter of MDL that seems like it's relevant. In this case, it seems like: how many bits do you need to describe the "stuff"? If it's parameters, then you can control that by saying, "If I quantize my network with however many bits, how bad is that?" I don't know, maybe this is one of these things where if I sat down and tried to do it, I'd realize the issue, but it seems doable to me. It seems like there's possibly something here.

**Lee Sharkey** (01:06:03):
Yeah, I do agree that it feels like we should be able to at least find a satisfactory Pareto frontier for minimum description length. I'm not sure we'll be able to get away from... Requiring that it just be a Pareto frontier. I'm not sure there will be some sort of single optimal version of it, but at very least I do think we can do better than the current algorithm.

## APD in toy models of superposition <a name="apd-in-tms"></a>

**Daniel Filan** (01:06:40):
Fair enough. So, I think the thing I next want to talk about is basically the experiments you run in your paper. So, in my recollection, in the main paper, conceptually, there are two types of experiments. So there's firstly this toy models of superposition, and secondly, there's this...

**Lee Sharkey** (01:07:08):
Compressed computation.

**Daniel Filan** (01:07:09):
Compressed computation. Yeah. So, I mean, you spoke about it a little bit earlier, but first can you recap how the toy model of superposition experiments are working?

**Lee Sharkey** (01:07:23):
Yeah, so some of the folks who are reading our paper, and many listeners, will be familiar with the model, but again, it's just this matrix that projects sparsely activating data down into some bottleneck space, and in that bottleneck space, features have to be represented in superposition, such that there are more features than dimensions in this bottleneck space. And then the matrix has to up-project them back to a space of the original size of the number of data features. So it's like an autoencoder setup.

(01:08:05):
And because it compresses these data set features down, it's kind of in some sense unintuitive that it can actually do this, because it has fewer dimensions than features. And because it has these fewer dimensions, there will be some interference between features that are not orthogonal to each other in this bottleneck space. But the way it gets over this is that, because it has ReLU activations following the up-projection, it can filter out some of this interference noise, and do a reasonably good job at reconstructing the input data features.

(01:08:59):
Now, one of the ways you might think about this network is that we have this matrix, and if one of the input data features is active, well, only, say, one row of the matrix is actually necessary. We can basically throw away the other rows. We can set them to zero, in cases where only this one - let's call it "input data feature one" - is active. And in particular, the row that we have to keep is the first row. So, we can basically set the other rows to zero. And so, there's some sense in which the rows of our toy model are like the ground truth mechanisms.

(01:09:59):
Why are they the ground truth mechanisms? Well, they satisfy the properties that we were aiming to recover here. So they all sum to the original network; that is, all the rows, whenever you set to zero the other rows, that basically sums to the original network. Then looking at minimality, because the dataset features are sparsely activating, there is... If you only activate the mechanism that corresponds to that dataset feature and you don't activate other ones, well, this is going to be the smallest number of mechanisms that you have to activate on this data point, so it's minimal.

(01:10:48):
And they're simple, in some sense, in that single rows of this matrix, when you zero out all the other rows, are rank one. They just correspond to the outer product of an indicator vector and the row itself. So they satisfy what we wanted to call a "ground truth mechanism". And the things that we're basically optimizing are randomly initialized parameter components to try to approximate. And so what we then find whenever we do this is that at least for a given set of hyperparameters, we are able to recover this set of ground truth features using APD.

**Daniel Filan** (01:11:39):
Okay. So in the paper, one thing you mention is: so the original toy models for superposition... It has a bunch of geometry and it draws some pictures and that's partly relying on the fact that there are five inputs and two hidden units, and that's a setting where it's just very small, and so things depend a lot on hyperparameters. You also look at a somewhat higher dimensional case where there's what? 50 inputs and 10 hidden units or something? Is that right?

**Lee Sharkey** (01:12:10):
It's 40 and 10, yeah.

**Daniel Filan** (01:12:11):
40 and 10. So my understanding is that you are pretty hyperparameter-sensitive in this really small setting. In the 40 and 10 setting, how hard is it to get the hyperparameters right?

**Lee Sharkey** (01:12:24):
It's easier, but I still think it's pretty hard. The five and two case is particularly challenging because optimizing in a two-dimensional space is just... It's something that gradient descent is not especially good at. I mean, it can do it, it's just that moving vectors around each other can be more difficult in two-dimensional space versus in N-dimensional space, where they basically just get to move in any direction and not interfere with each other. In two-dimensional space, there's just much greater chance for interference.

**Daniel Filan** (01:12:59):
Okay. I guess I'm just especially drawn to this hyperparameter of how many components you have. I don't know. For some reason, to me, it feels like the most juicy hyperparameter, even though obviously, relative weighting of these objective terms and all sorts of things are also important. Well, in this case you have a ground truth number of components. If you get the number of components slightly wrong, what happens? How bad does it go?

**Lee Sharkey** (01:13:35):
I can't recall an exact story for what happens, but for some cases it will learn a bunch of reasonable features, but then some features will just not be learned. In other cases, it will be just much more noisy and it'll fail to learn altogether. I can't give a good sense of how sensitive it is to this hyperparameter. My colleague [Dan](https://danbraunai.github.io/) [Braun] will have a much more informed sense of how sensitive it is to twiddling this. But it's also hard to tell "is it this hyperparameter that is the most sensitive thing?" versus others. Because there's basically a bunch of different hyperparameters to get right here, it's hard to get really intuitive around any given one of them. Yeah.

## APD and compressed computation <a name="apd-and-comp-comp"></a>

**Daniel Filan** (01:14:40):
Okay. I eventually want to get to a question about these experiments in general. And so in order to get me there, can you tell me about the compressed computation setup and what's going on there?

**Lee Sharkey** (01:14:53):
Yeah. So compressed computation is the name for a phenomenon that we observed in our experiments. We were initially trying to model two different things. One is a theoretically well-grounded phenomenon that my colleagues, Lucius [Bushnaq] and Jake [Mendel], had talked about in [a previous post of theirs: computation in superposition](https://www.alignmentforum.org/posts/roE7SHjFWEoMcGZKd/circuits-in-superposition-compressing-many-small-neural), where a network is basically learning to compute more functions than it has neurons. And there's also a related phenomenon that's more empirical, which is from [the original "Toy models of superposition" paper](https://transformer-circuits.pub/2022/toy_model/index.html) that they also called computation in superposition. And then there's also this third phenomenon that we've called "compressed computation".

(01:15:56):
Now, it may be the case that all of these are the same thing, but we are not yet confident enough to say that they are all exactly the same phenomenon. The reason is that we are not super, super confident - at least were not at the time. We became a little bit more confident and have slightly updated against the update - that the compressed computation is similar to these other phenomena, computation in superposition. Which one, I would not be able to answer. But it is nevertheless the case that all of these can be described as learning to compute more functions than you have neurons. It's just that there's a fair bit of wiggle room in the words when you put those words into maths.

**Daniel Filan** (01:16:53):
Sure. So with toy models of superposition, the basic intuition for why it was possible to reconstruct more stuff than you had hidden activation space dimensions, was that the stuff you had to reconstruct was sparse and so you didn't have to do that many things at a time. Is that the same thing...? Sorry. This is almost definitely in the paper. In compressed computation, is the trick just it doesn't have to compute all the things at the same time? Or somehow it actually really does compute all of the things at the same time with less space than you thought you would need?

**Lee Sharkey** (01:17:30):
This is the point in which we are uncertain, basically. Basically, we are not super confident about how much this phenomenon depends on sparsity. Now, we are also just not super confident on how much the Anthropic computation in superposition depends on sparsity. We know in their example it does, but because we don't have access to the experiments, we don't know what was going on in the backgrounds of those figures. We just haven't got around to doing extensive experiments to actually figure that out. It wouldn't be too difficult.

(01:18:09):
But in our case, we're basically quite uncertain as to how much our phenomenon depends on sparsity. My colleague [Stefan](https://heimersheim.eu/) [Heimersheim] has done some experiments in this direction. It's somewhat inconclusive for now. I think he's got a project ongoing on this that hopefully there'll be a write-up of soon. But yeah, long story short, it may or may not depend on sparsity, but I think for the purposes of the conversation, it may be reasonable to proceed as though it does.

**Daniel Filan** (01:18:41):
Okay. So basically, the thing of compressed computation is computing more functions than you have width of internal neurons, and it's surprising that you'd be able to do it, but you can. And my understanding is that the particular functions you're trying to compute are ReLU functions of the inputs.

**Lee Sharkey** (01:19:06):
Yes.

**Daniel Filan** (01:19:08):
And you might be like, "ReLU networks, shouldn't they be able to do it?" But the trick is, the network narrows significantly. And so what is the hope here? What should APD be able to do in this setting, if it's working?

**Lee Sharkey** (01:19:25):
So in this setting, the ground truth mechanisms are supposed to be things where, even though the data has, say, 100 input dimensions and the labels are 100 ReLUs of that input data, the models have learned basically to compute 100 ReLUs using only 50 ReLUs in the model. And the idea here is that, well, if they're able to do this, they are in some sense using... They're distributing their computation over multiple ReLUs, such that they can nevertheless do this without interfering with other features whenever they are not active. So you're basically computing more functions than you have neurons because you're not always having to compute them all at the same time.

**Daniel Filan** (01:20:27):
Right. And so this is just because if you have a negative input, then all you have to know is that it's ReLU is zero, and you don't have to do that much computation to make sure you have the identity function?

**Lee Sharkey** (01:20:39):
Yes. But in other cases where suppose you have two input features that are positive, and so you need to compute two ReLUs. Well, if you have basically projected one of your input features to one set of hidden neurons, such that you can spread your ReLU function over these multiple ReLUs. And if they are a different set of hidden ReLU neurons than the other feature, then you should be able to make a good approximation of the ReLU of the input data, because the magnitude matters here. Suppose there was some overlap in one of their neurons between these two input features, well, they would double up and that would contribute to... They would basically overshoot in the output. And so if you spread things out a little bit, such that they don't overlap very much, you should be able to compute things, with some interference, but ultimately compute more functions than you have neurons. But yeah, the cost is interference.

**Daniel Filan** (01:22:01):
Gotcha. And so just as long as you're distributing over the set of neurons... Sorry, a thing that I just realized: the fact that you're going from 100 inputs to 50 wide, which is half of 100, is that just for the same reason as numbers have a 50% chance of being positive and negative, and so on average, you only need to represent half of them?

**Lee Sharkey** (01:22:24):
I don't think the number 50 was especially important. I think we could have easily chosen something else. Yeah, I think it was somewhat arbitrary.

**Daniel Filan** (01:22:36):
Okay. Fair enough. All right, so I was asking what APD is meant to get and what was the answer to that?

**Lee Sharkey** (01:22:58):
Yeah. Thanks for reminding me. So I was trying to get a sense of what the ground truth features should be. Sorry, I said ground truth features, ground truth...

**Daniel Filan** (01:23:09):
Mechanisms.

**Lee Sharkey** (01:23:09):
Yeah, mechanisms. And these ground truth mechanisms should be things that distribute across multiple hidden neurons. And so the input... you've got this down-projection matrix and then this up-projection matrix. Rather, maybe think about it as an embedding matrix, an MLP in matrix, an MLP out matrix and then an unembedding.

(01:23:47):
So it's a residual architecture. And so you have this embedding matrix and this MLP in matrix, and whenever you multiply these two matrices together, you basically want to show that a given input dimension projects onto multiple hidden neurons. And this is what one component should do. And those hidden neurons should then project back to that output feature that corresponds to the input feature that you care most about. And so you can basically do this for multiple input and output features.

(01:24:45):
Because your input and output features are sparsely activating, you want your parameter components to mostly only correspond to one of these input and output computations. And so you want basically to have parameter components that line up strongly with these input and output components.

**Daniel Filan** (01:25:11):
Right. So it seems like the thing is, maybe you don't know exactly which parameters light up or whatever, but you do know for each component that APD finds, it should reconstruct the ReLU of exactly one input and none of the rest of them. Is that basically right?

**Lee Sharkey** (01:25:33):
Yeah, basically. Because in this case, we basically get to define what the minimal set of components is, because we get to choose a lot about the data distribution.

## Mechanisms vs representations <a name="mech-v-rep"></a>

**Daniel Filan** (01:25:43):
Okay. So I think the thing that I'm wondering about with both of these tests is: so I think of the idea of APD as, previously a bunch of people have been trying to explain representation of features. They've looked at these neurons, they've said, "What do these neurons represent?" But you want to find the mechanisms, right?

**Lee Sharkey** (01:26:05):
Yep.

**Daniel Filan** (01:26:06):
Now, the thing that strikes me about both of these examples is they feel very representation-y to me. They're like, "Okay. We've got this sparsely activating input and we've got this low-dimensional bottleneck space, and we want to reconstruct these parameter vectors to tell us how the bottleneck space is able to reconstruct each component of the input." But for ReLU, it's like, for each of these inputs, there should be something that's representing the ReLU of that input, and I just want to divide into things that get the ReLU.

(01:26:50):
It seems to me that networks could have a bunch of mechanisms that don't necessarily do representing individual features or things, right? Or potentially representing things could involve a bunch of mechanisms for any given thing you represent. Maybe there are five mechanisms that are necessary. But basically, I just had this thought, reading this paper, of: it feels like the experiments are too representation-y and not mechanistic-y enough. What do you think of this anxiety that I'm feeling?

**Lee Sharkey** (01:27:27):
Yeah, I think that's reasonable. I share this. There are a few toy models that we would be keen to see people work on. I'll also, just before I get into that, just say I do think that there's some... In some sense, it's not a perfect duality between representation and mechanisms or computation, the computation-y point of view. There's nevertheless a relationship. It is therefore more a matter of perspective, like which one is most convenient to think about at a given point in time.

(01:28:14):
I do think that when designing these toy models, we wanted to get a method that works in very simple setups, where these representations do in fact correspond to the mechanisms, right? This is a case where it's been easier to design networks where there's a ground truth that's easily accessible to us. We found it a little bit harder to train networks where you could be somewhat sure of what the ground truth was, even though there are multiple computational steps that may be involved. I think it's perfectly possible. We did have some cases where we handcrafted some models. There's an example of this in the appendix, but that had some pathologies. The gradients didn't work especially well on this because it was handcrafted. And so we did find it somewhat challenging.

(01:29:18):
Now, there are some models that you could think of that may capture this notion a little bit more than the ones in the paper. One that's very similar to what is in the paper could be: consider a toy model of superposition model where instead of just this down-projection and then up-projection, you have a down-projection and then, for example, an identity matrix in the middle, and then an up-projection. Or you can replace this identity with a rotation, say. Now, what would you want APD to find here? Well, you don't really get to think about it in terms of representations anymore. Because fine, you've broken it up into these mechanisms in the down-projection and in the up-projection, but there's this bottleneck where you're doing something in the bottleneck, if it's an identity or a rotation. Suppose it's a rotation. It's probably easier to think of that. You're basically having to use all ranks of that rotation in the middle, for every given data point. You don't actually get to chunk it up.

(01:30:39):
So what you would want APD to find is parameter components that correspond to the things that we originally found, for the simpler example here, where it's just the rows of the down-projection matrix and the up-projection matrix, but then also, a component that corresponds to this rotation in the middle. Why? Because you're having to use all ranks of this rotation for every data point. You always have to do it. You don't get to throw it away and reduce your minimality. You don't get to make it any simpler to reduce your simplicity. It's just always there. And so this is maybe a case where you do get to think about it in terms of computational steps rather than representations.

**Daniel Filan** (01:31:33):
Before I go further, just to pick up on the thing I said: so I believe this is in Appendix B.1, you hand designed these networks to compute these functions or whatever.

**Lee Sharkey** (01:31:42):
Yes.

**Daniel Filan** (01:31:45):
How did you hand design the networks?

**Lee Sharkey** (01:31:47):
So I believe this was Jake [Mendel] and Lucius [Bushnaq] and [Stefan](https://heimersheim.eu/) [Heimersheim]. I may be misattributing there, but I've at least included all of them. One or the other may not been involved. But they, I think, just thought about it really hard and then came up with it. They're not super complicated networks. They have particular steps. They just have a little gate and for certain inputs, your gate is active, and on other inputs, it's not. And this lets you do subsequent computations. It's been a little while since I've looked at it, but the basic principle is that it's not a complicated network.

**Daniel Filan** (01:32:35):
So my recollection is that it's basically sinusoidal functions. I guess if I had to, I could write down a network. If you just divide it up into piecewise linears for a wide network, you could figure out how to do it. It's just tricky.

**Lee Sharkey** (01:32:52):
Yeah, yeah, yeah. Yeah, this network gave us a lot of grief because it's intuitively quite a simple network. But because we are using gradient-based attributions, it just didn't play nice with the method, even though to our naive selves, it intuitively felt like it should. But we eventually got it working, but it is demoted to the appendix.

**Daniel Filan** (01:33:27):
Fair enough.

**Lee Sharkey** (01:33:28):
For punishment.

**Daniel Filan** (01:33:30):
So you mentioned: in this toy network where you have "project down, do an operation in down-projected space and then un-project back up" - well, this is ideally what APD should find. When you say it like that, it sounds like an easy enough experiment to run. Have you tried it?

**Lee Sharkey** (01:33:59):
I believe we at various points gave it a go. I think it just wasn't top priority to get the paper out.

**Daniel Filan** (01:34:08):
Fair enough.

**Lee Sharkey** (01:34:09):
It's very possible that we have got this working already and I'm just forgetting. It's also very possible that we had tried it and couldn't get it working or at least didn't want to invest the time to get it working, such as the sensitivity of the hyperparameters. But yeah, I would be keen to see a verification that it is at least possible for APD to find this. Intuitively, it feels like it ought to be able to. But yeah, I'd just like to see it empirically.

## Future applications of APD? <a name="future-apps"></a>

**Daniel Filan** (01:34:41):
Sure. I guess other things that strike me as interesting to look into... So there are a few cases in the literature where people do really seem to have identified mechanisms within neural networks. I guess the most famous one is these [induction heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html), right? As some people have pointed out, it can be a loose term. People can use it for a few things, but in at least some cases, people can just point to, look, this attention head, if you pay attention to this thing it's doing and this thing it's doing... Or I guess it's two attention heads maybe. But you could just tell a very clear story about how it's looking for an initial thing and then taking the thing after that. And then this thing copies the thing after that into the output. So that's one example of a thing that feels very much like a mechanism and does not feel so representational.

(01:35:42):
Another example is group multiplication. These neural networks that are trained on group multiplication tables, they have to get the other ones... I guess I was semi-involved in a paper... Well, I chatted with one of the people and tried to make sure he was on track for things. But there's this [Wilson Wu paper](https://arxiv.org/abs/2410.07476) that's basically together with Jacob Drori, [Louis Jaburi](https://cogeometry.com/), and [Jason Gross](https://jasongross.github.io/). And basically, I think they end up with a pretty good story of how these networks learn group multiplication. They could basically tell you, they do this thing and then they transform it in this way and then they get this output, and it works because of this weird group theory fact.

(01:36:34):
I think there are a few more of these. I guess, for at least those examples, can we get APD working on those? How hard would it be to check if APD actually works on these?

**Lee Sharkey** (01:36:54):
It feels possible, certainly in toy models for the induction head. Indeed, it was one of the motivations for APD that I'd been working with various [MATS](https://www.matsprogram.org/) scholars, [Chris Mathwin](https://www.lesswrong.com/users/cmathw?from=post_header), [Keith Wynroe](https://www.lesswrong.com/users/keith_wynroe?from=post_header), and [Felix Biggs](https://felixbiggs.com/) as well, on decomposing attention in neural networks. It feels like you should be able to do this, just add in an SAE here or transcoder there. You can make some progress in this, but it just didn't feel conceptually very satisfying.

(01:37:36):
And it basically was one of the motivations for APD that... Well, we really wanted a method where we don't have to modify it, such that if you have a slightly different architecture - maybe it's a gated linear unit or maybe it's a state space model - ideally you wouldn't have to adapt your interpretability methods to these. You should just be able to decompose, just apply the method that you have that works for all neural networks. That would be ideal. And so this was one of the motivations, looking at attention and how it may actually distribute computations across heads or various other ways to distribute it.

(01:38:15):
Now, it feels possible then that we should be able to do this in toy models of, say, induction heads. It would be a somewhat more complicated model than APD has been used for thus far, but it does feel possible, and it's one of the things I'm excited about people trying. In the cases where... say, group multiplication or modular addition, it's very possible that if you did apply APD to these models where you don't really get to... It feels possible that in these models, all mechanisms are active all the time and therefore APD just returns the original network.

(01:39:05):
And if that's the case, this is a bullet I'm willing to bite on the method. Sometimes we just don't get to decompose things into more things than the original network. These are, after all, fairly special networks trained in quite different ways from the tasks that we really care about, such as language modeling. It's nevertheless something to bear in mind when applying APD to models. It's not going to immediately tell you, in cases where it may be a multidimensional feature, how to understand the interactions within this multidimensional, multilayer component. But at very least, what we wanted was a method that would, in cases where you could decompose it, where it actually succeeds in doing that.

**Daniel Filan** (01:40:09):
Right. Sorry. The thing you said just inspired me to look at this paper. So the paper is ["Towards a unified and verified understanding of group operation networks"](https://arxiv.org/abs/2410.07476). And the author I was forgetting was Louis Jaburi. Sorry, Louis. So there's this question: for group multiplications, are all of the things active at once? I think I'm not going to be able to figure it out quickly enough in time for this, but yeah. It does seem like an interesting question of: can you get APD working in a setting where there are...

(01:40:55):
I guess it's tricky because it's a lot easier to have ground truth representations than ground truth mechanisms. Especially if you know, okay, I'm an autoencoder or I'm doing this known function of each individual input. And I guess this just relates to the fact that we understand... Representation is just much easier for us to have good a priori theories of than computation, somewhat, unfortunately.

**Lee Sharkey** (01:41:23):
Yeah, maybe I'm just too APD-brained at this point, but I'm curious, could you flesh that intuition out a bit more? I feel like what it means for a hidden activation to represent one of the input features in the TMS case doesn't feel intuitively obvious to me. There may be a direction in hidden activation space that corresponds to one of the input dimensions. It doesn't feel more intuitive, that point of view than, say, "this input feature activated that computation". I'm curious-

**Daniel Filan** (01:42:08):
Yeah. I guess all I'm saying is that with toy models of superposition, it feels like the reason you can very confidently say, "This part is doing this thing," is in some sense you know that all the neural network has to do is, it has to put a bunch of information into this two-dimensional knapsack and be able to get it out again, right? That's just everything the network's doing. And you can say, "Okay. Well, I understand that it should be sensitive to all these things." I guess there are some things you can say about the computation there, but for cases like compressed computation, like toy models of superposition, you can just say, "Okay. Look, I have these things and I know this input should correspond to this output." That's just definite ground truth because it's basically what I'm training on. Whereas, it's a lot harder to look at a network and say, "Well, I know it should be doing this computation and I know it should be divided up into this way."

**Lee Sharkey** (01:43:15):
Yeah, I think that's fair.

**Daniel Filan** (01:43:16):
And therefore it's easier to test against, well, do I reconstruct this thing in toy models of superposition, where I know what I'm supposed to reconstruct, versus do I reconstruct this way of doing things, where a priori, you don't exactly know.

**Lee Sharkey** (01:43:36):
Yeah, I think that's fair. And I think this maybe is part of the... This goes back to a little bit of what we were talking about at the start, where even though there may be multiple equivalent ways to describe the computations going on in the network, we really just have to be opinionated about what constitutes a good explanation, and faithfulness to the network, minimality, and simplicity are just the ones that we think are a reasonable set of properties for an explanation to have.

**Daniel Filan** (01:44:12):
Fair enough. So, okay, I think I'm going to transition into just more asking miscellaneous questions.

**Lee Sharkey** (01:44:18):
Yeah, sounds good.

## How costly is APD? <a name="how-costly"></a>

**Daniel Filan** (01:44:19):
Less grouped by a theme. I think the first thing I want to talk about is that at one point in your paper you say that, "So why are we doing APD on these small networks and not on Llama-whatever, however many billion models you can get these days?" And the answer is that it's expensive to run APD. Concretely, how expensive is it to actually run?

**Lee Sharkey** (01:44:48):
So the version that we've come up with here is: we didn't aim for efficiency, we didn't aim for some of the obvious things that you might try to get a method that works more efficiently than ours, the reason being that we wanted something where on theoretical grounds we could be somewhat satisfied with it and satisfied with it working. And then after that we can move to things that are more efficient. So for the current method, what we have here is, for a start we've got - let's call it the letter C - C components. We've got C components and each of these require as much memory as the original model, right?

(01:45:35):
Now, that's already a multiple of the expensiveness of the original model just to do one forward pass. We also have the first forward pass, the first backward pass, the second forward pass and the second backward pass. So during one training update, we have these four steps. So it's already a multiple of just a given forward pass and backward pass that might be required to train an original model, but I guess different goals with each of these steps. Yeah.

**Daniel Filan** (01:46:22):
So I don't know, maybe the answer to this is just it's another hyperparameter and you've got to fiddle with it: there's a number of components that you want to end up being active, right? This k that you talk about. And then there's just a total number of components that you have to have in order to run the method at all. Is there something to say about "it turns out you need five times as many total components as you want components active in any single run," or is it just kind of a mess?

**Lee Sharkey** (01:46:52):
Well, some people will be familiar with training sparse autoencoders, and in some cases you start with more features than you expect to need, the reason being that during training, some might die. There's various tricks that people have invented to stop them dying - reinitialization and so on. The same's true in APD. Some of these parameter components will in some sense die and depending on the type of model, in general, you'll want to train with a little bit more than the total number of ground truth mechanisms just so that on the off chance that some do die, you still nevertheless have enough to learn all of the ground truth mechanisms.

**Daniel Filan** (01:47:42):
Okay, but it sounds like you're thinking that it has to be more like a factor of two than a factor of 100 or something.

**Lee Sharkey** (01:47:52):
That would be my expectation. I don't think there's going to be a ground truth answer to that, but yeah. I don't see any reason why it would need to be many multiples higher.

**Daniel Filan** (01:48:03):
Okay. So I guess if you're thinking about the expense of this, it seems like, okay, you have this constant blow up of you've got to do two forward passes and two backward passes on each gradient step, and also you've got to keep on having these C copies of the network around at all times. And then there's this question of how many steps it takes to actually train APD, which presumably is just an empirical thing that is not super well understood. I guess one question I have is: if I remember correctly, there's some part of your paper where you mentioned that naively, APD might take order of N squared time to run. Do I remember that correctly?

**Lee Sharkey** (01:48:51):
Yeah. I think this is a pessimistic upper bound on the expensiveness, but I think there's plenty of reasons to expect it to be lower than this, but I would need to revisit the sentence to be 100% sure what we're actually talking about.

**Daniel Filan** (01:49:08):
Fair enough.

**Lee Sharkey** (01:49:09):
There is a sentence that talks about the scaling and mentions O(N^2). Yeah.

## More on minimality training <a name="more-min"></a>

**Daniel Filan** (01:49:14):
Okay. So the next thing that just came across my mind that I wanted to ask is: when you're training for a minimality, so on each input you run it forward, you do attribution to get the k most active components, and then you drop all the other components, and then have some training step to make the k most active components reconstruct the behavior better on that. I'm kind of surprised - it seems like one thing you could imagine doing is also training the ones that you dropped to be less relevant on that input than they actually are. I'm wondering if you tried this and it didn't work or if this is just less of an obvious idea than it feels like to me.

**Lee Sharkey** (01:50:16):
Yeah, I guess so concretely what you might consider doing in that case would be you might have a third forward pass where you only run it with... I guess I don't know. It may be hard. I haven't thought about this enough, but it may be hard to distinguish between the influences of... I don't know. On the face of it, it feels like something that could be useful to implement if it's possible to implement. Yeah, it does feel possible, for sure. I don't recall us trying it, though.

(01:51:07):
The things that we did try were the top k and then we also tried an Lp sparsity version where you penalize everything for being attributed. You penalize everything for having some causal influence over the output, but you penalize the things that were most causally attributed proportionally less than the things that had some small influence. And this is kind of doing that, but it is not equivalent. But yeah, it feels possible to do that. I'd be curious if it could be done.

## Follow-up work <a name="follow-up-work"></a>

**Daniel Filan** (01:51:49):
Gotcha. So I think at this point I'm interested in what follow-up work you think is important to do on APD, either that you think is important to do or if enterprising listeners maybe want to pick up some of the slack.

**Lee Sharkey** (01:52:03):
Yeah, so I've mentioned a few of the things that I'd be keen to see already. So non-exhaustively: attention - I'd be curious to see if it can decompose attention in a sensible way. There's various other things. However, the main thing right now is figuring out whether or not we can make it less sensitive to hyperparameters and more scalable. Basically, these are the two: robustness and scalability are the main things that we're keen to solve, just because it will open up... Whenever we do investigate these other things, like attention, distributed representations across attention, that will be less painful to do. And also, you can do this in larger, more interesting models. So the main thing is scalability and hyperparameter sensitivity or robustness.

(01:53:07):
Those being the two main things, suppose we solve those, I would be keen to see attention, keen to see other types of architecture decomposed here. There's also a few other phenomena that you might be curious to apply APD to. For instance, the phenomena of memorization, right? You might imagine that memorization - whenever APD successfully decomposes the network into memorized data points versus generalizing data points - there may be one parameter component that corresponds to one memorized data point and one parameter component that corresponds to a generalizing computation within the network. It may therefore be a nice place to distinguish between these two computational regimes of memorization and generalization. So I'd be keen to see APD applied to that.

(01:54:11):
I mentioned some of the more theoretical things that you might want to look into, such as privileging layers or more implementationally figuring out whether or not we can get rid of having to do a top-k setup where you have to choose k. Then yeah, there's a bunch of fairly disparate directions, all of which I'm super keen to see done. I think our main priorities now are just creating a method that makes those other things a bit easier. That's a non-exhaustive view, though. There's a more exhaustive list, I think, in the paper.

**Daniel Filan** (01:55:08):
Makes sense. So a couple things that seemed interesting to me, but I'm curious if you have comments on: so I guess somewhat inspired by our discussion about doing APD to a car, it seems like APD is a method that sort of is sensitive to the input distribution that you do training to. And I think there's this ["Interpretability illusions" paper](https://arxiv.org/abs/2104.07143) that says: sometimes you might think that you have a rich enough input distribution, but you don't actually. I think just how sensitive you are to this input distribution and how right you have to get it... I think that's something that I don't know if you've had much preliminary exploration into, but it does seem pretty relevant.

**Lee Sharkey** (01:55:59):
It seems relevant. I think this is in some senses unavoidable just because: I want to decompose neural networks. What does that mean? Well, it means to decompose what these networks are doing. What they're doing depends on the input distribution. And simply with a different distribution, natural or unnatural, it just will lead to different things. I do think that when we get to more scalable versions of this method, this will become even more important. You ideally want to have a method where suppose you're decomposing Llama 3 or whatever, if you've got a scalable method, you train it using the training distribution of Llama, but then you also train it with the training distribution of Llama permuted.

(01:57:02):
You ideally want to end up with the same thing, similarly for a large enough subset and then more adversarial subsets. It will be the case that for a sufficient level of adversity it will break. I think this maybe emphasizes the importance of just doing interpretability on as large a distribution as you possibly can, which stands in contrast from some of the interpretability that's happened in the past. I like to call this "using the big data approach", where you're finding structure first and then asking questions later. It's kind of borrowing from areas of science where there's just a lot going on and you kind of want to leverage computation first to actually narrow down what questions you really ought to be asking.

(01:58:13):
And the application here in interpretability would be: you want to let computational methods do the work first, and then you figure out "what does this component mean?", rather than presupposing your own ideas of what the components ought to be and then studying those in more detail. This is the kind of approach that I think APD intends to leverage, this big data approach. And I think that's somewhat unavoidable in interpretability that can tell you things that you weren't looking for in the first place.

**Daniel Filan** (01:58:57):
Fair enough. So another thing that struck my eye in the paper is: so there's a section that is basically... I think of this section of the paper as basically saying why SAEs are bad and rubbish. And one thing that is mentioned is: there's this feature geometry in SAEs, sort of like the day of the week thing where they're in a circle, Monday, Tuesday, Wednesday. And I think there's some line that says the fact that there is this geometry is not as - maybe Jake Mendel has written about this - but this is not purely explained by this linear representation hypothesis. We need to understand mechanisms to get us there. How soon until APD tells us what's going on with SAE feature geometry, or feature geometry in general?

**Lee Sharkey** (01:59:51):
Yeah. So Jake's post was, if I'm recalling the title correctly, ["Feature geometry is outside the superposition hypothesis"](https://www.alignmentforum.org/posts/MFBTjb2qf3ziWmzz6/sae-feature-geometry-is-outside-the-superposition-hypothesis). And feature geometry is this idea where... It's older than the mechanistic interpretability community. This idea was present in neuroscientific literature a bit before, but the idea here is that: suppose you've got a neural network and you train an SAE on the activations and you look at the features that you end up with. These features tend to correspond to certain things. This was the whole point of training SAEs, to identify interpretable individual components. But whenever you start comparing the directions of these features relative to each other, you'll notice that, if I look at the Einstein direction, the Munich direction, the...

**Daniel Filan** (02:01:10):
Lederhosen?

**Lee Sharkey** (02:01:10):
...I don't know, lederhosen direction and so on, you'll find that all these kind of point in somewhat similar directions. There's a kind of latent semanticity to them. There's something underneath these features. These features were supposed to correspond to the computational units of neural networks. And now what this feature geometry is indicating is that there's an underlying computational structure that organizes these features relative to each other, which is, in my opinion, something of a... This doesn't bode well if you considered SAE features to be fundamental units of computation, because you shouldn't be able to identify these latent variables that are shared across multiple features. And what's giving the structure? What is giving the geometry to these features? Well, the hypothesis here is that: suppose you have the Einstein feature and you've also got this Lederhosen feature and so on.

(02:02:28):
Well, these all get the German computation done to them. They're all pointing in this direction because somewhere down the line in the network the network needs to do the German computation to them and just apply some specific transformation or some set of transformations that correspond to the German-ness of a thing. And you can imagine other cases for animal features. Why do all the animals point in similar directions? Well, the network needs to do animal-related computations to them. And now you could go further. Why do all the furry animals point in similar directions? Well, because there needs to be furry computations done to them. The hope here is that instead of studying the features and trying to use that as a lens to understand the network, study the computations and that will inform why the geometry is the way it is, because you get to look at the computations that get done to it, which is presumably why the network is structuring them in this way.

(02:03:39):
It's very possible that you just kick the can down the road there. You may find that if you decompose your computations into very simple computational units, well, you might find that there's some relationship between your computational units in terms of geometry, but it nevertheless feels like a you've done better than the SAE case, basically.

**Daniel Filan** (02:04:06):
Right.

**Lee Sharkey** (02:04:08):
It's not obviously totally solved the problem.

**Daniel Filan** (02:04:11):
Yeah. So how long until APD explains all this?

**Lee Sharkey** (02:04:18):
Well, either you would need a toy model of feature geometry such that it's a small enough model that you can apply APD to it. And that toy model would need to be convincing such that people can say that it probably applies to larger models. But absent a convincing toy model, you would need to be able to scale this such that you can apply it to larger models. I can't say for certain when we'll have a scalable method, it's something we're currently working on. We're very keen for other folks to work on [it] as well. I would be speculating irresponsibly to say when we'll have a working method for that, but I would hope that anywhere between three months and three years. That's the kind of uncertainty.

**Daniel Filan** (02:05:15):
But I guess it illustrates the importance of just robustifying this thing to make it easier to run on bigger instances.

**Lee Sharkey** (02:05:23):
Yep.

## APD on giant chain-of-thought models? <a name="apd-on-cot"></a>

**Daniel Filan** (02:05:24):
So I guess the last question that I want to ask is: what's the end game of APD? Is the hope that I run it on the underlying model of o3 or whatever and then I just understand all the things it's thinking about at any given point? How should I think about: where is this going? What's it actually going to get me in the case of these big chain-of-thought networks?

**Lee Sharkey** (02:06:06):
Yeah, it's an important question to ask. I think the way I see this kind of work and the way I see the similar work that came before, such as SAEs or transcoders or things like this... The point is to break up networks into as simple of components as you can, such that whenever you try to understand larger facts about the network you've got some solid ground to stand on. You can say, "Well, I got this set of components. If I were really invested, I could in theory just understand everything with this very large number of components." Now, do I really think that mech. interp. is going to let us understand everything? Well, probably not as humans, but I do think that it will give us solid ground to stand on whenever we want to understand particular phenomena.

(02:07:00):
Now, if I want to understand, say, the deception mechanisms within e.g. o3 or any other model, where do I go looking for them? Well, currently we look at behaviors. One thing that you might be able to do is look at transcoder kind of approaches. But because transcoders and other activation-based methods are primarily looking at activations, they're not necessarily giving you the things that are doing the generalization such that you are... I don't know. I think you can be less confident that you're understanding how the network would behave in a more general sense. And by looking at the objects that are doing the generalization, by looking at the parts of the parameters that are actually doing the thing, you might be able to make more robust claims.

**Daniel Filan** (02:08:04):
Yeah, I think it's fair enough to say, yeah, look at very specific things. I guess there's also some world in which once you're able to have these good building blocks, you can do automated interpretability of everything, if you need to.

**Lee Sharkey** (02:08:18):
For sure. Yeah. I mean, I guess I'm leaving that implicit. Yeah, the ultimate aim would be that you can automate the process by which you would understand parts of the network such that you can understand broader swathes of it. And yeah, ideally you have given yourself a solid enough ground to stand on that whenever you do this, fewer things will slip through the cracks.

**Daniel Filan** (02:08:45):
Sure. I guess one thing that strikes me as interesting about these reasoning models in particular... And sorry, this might be kind of far afield, but I think a lot of interpretability work has been focused on understanding single forward passes. Especially for classification models, the early stuff was done on vision classification, where of course you just want to find the curve detectors or whatever. And for SAEs you're like, "Oh, which things are being represented?" One thing that I think reasoning models bring to light is: it seems to me in some sense, the relevant mechanisms should be thought of as distributed across forward passes, right?

(02:09:33):
You do a forward pass, you write a thing in your chain of thought, then you do another forward pass, you write another thing in your chain of thought. And in some sense, the real mechanism is a bunch of these end-to-end copies of this network. This might be too speculative to ask about, but where do we go in that setting? Do you think it still makes sense to focus so much on understanding these individual forward passes versus the whole web of computation?

**Lee Sharkey** (02:10:07):
I think it probably does. The reason being, what alternatives might we aim for? If we wanted to instead just to ensure that in these more distributed settings where computations are spread across a whole chain of thought, well, what might we do in that case? We care about the faithfulness of the chain of thought. So in the case where we care about the faithfulness, we want some way to measure how faithful the chain of thought actually is being. And mech. interp. does give you some measure of: if you can understand a given forward pass and maybe even a small chain, it should give you firmer ground to stand on whenever you make claims about, "this method that I developed that improves the faithfulness of the chain of thought..." I don't know how you can make such statements without actually having some way to measure the faithfulness of the chain of thought, and that's maybe one way that mech. interp. may be able to help in that regime. Yeah, that's just kind of the one thing that comes to mind.

## APD and "features" <a name="apd-and-features"></a>

**Daniel Filan** (02:11:27):
So wrapping up, I want to check, is there anything that I haven't yet asked you that you think I should have?

**Lee Sharkey** (02:11:36):
I think one of the things that I find most... satisfying, maybe, about thinking about interpretability in parameter space is that many of the notions that we had going into interpretability become a little less confusing. So one of the main examples that I have in mind here is just this idea of a feature. People have used this notion of a feature in a very intuitive sense and struggled for a long time to actually nail it down. What is a feature? What are we really talking about here? It kind of evaded formalism in some sense. And I think one of the things that I find most satisfying then about interpretability in the parameter space is that it gives you some foundation on which to base this notion. In particular, the thing that we might call a feature of a network is something that uses one parameter component.

(02:12:49):
For instance, what does it mean to say that a model has a feature of a cat inside it? Well, you can perhaps equivalently say that this model has got a cat classifier computation or it's got a cat recognition computation. This is what I mean. There's a kind of duality between... It's not an exact duality by any means, but it helps provide a sense in which features mean something specific. In particular, it means whenever you break up a network into faithful, minimal, and simple components, these components, these mechanisms are what you might reasonably call... In some cases, you couldn't call them a feature. In other cases, it's more natural to think about them in terms of "this is a step in the algorithm. This is a computation that the network is doing." And I think in that sense it's a bit more general than thinking about things in terms of features.

## Following Lee's work <a name="following-lees-work"></a>

**Daniel Filan** (02:14:11):
Fair enough. Well, I guess to finally wrap up, if people listen to this and they're interested in following your research, how should they do that?

**Lee Sharkey** (02:14:26):
Yeah, I post most of my things. I post them on Twitter and I also post on the Alignment forum as well. You can just follow [me on Twitter](https://twitter.com/leedsharkey) and check out [me on the Alignment Forum](https://www.alignmentforum.org/users/lee_sharkey).

**Daniel Filan** (02:14:38):
So links to those will be in the description. But for those who don't want to open the description, are you just "Lee Sharkey" on Twitter and the Alignment Forum?

**Lee Sharkey** (02:14:48):
I think I am [leedsharkey on Twitter](https://twitter.com/leedsharkey), at least in my Twitter handle, but I should just be Lee Sharkey and findable by that. And yeah, [Lee Sharkey on the Alignment Forum](https://www.alignmentforum.org/users/lee_sharkey).

**Daniel Filan** (02:14:58):
All right, well, thanks very much for coming here. We've been recording for a while and you've been quite generous with your time, so thank you very much.

**Lee Sharkey** (02:15:05):
No, thank you, Daniel. It's been great. I've had an awesome time. Cheers.

**Daniel Filan** (02:15:08):
This episode is edited by Kate Brunotts and Amber Dawn Ace helped with the transcription. The opening and closing themes are by Jack Garrett. The episode was recorded at FAR.Labs. Financial support for the episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript, you can visit [axrp.net](https://axrp.net/). You can also become a patron at [patreon.com/axrpodcast](https://patreon.com/axrpodcast) or give a one-off donation at [ko-fi.com/axrpodcast](https://ko-fi.com/axrpodcast). Finally, you can leave your thoughts on this episode at [axrp.fyi](axrp.fyi).
