---
layout: post
title:  "2 - Learning Human Biases with Rohin Shah"
date:   2020-12-10 21:00:00 -0800
categories: episode
---
[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/OTY2ZDUzNmEtYjRjOC00ODZiLTliZGMtMWQwOTMyMzdhZWY1)

This podcast is called AXRP, pronounced axe-urp and short for the AI X-risk Research Podcast. Here, I ([Daniel Filan](https://danielfilan.com/)) have conversations with researchers about their papers. We discuss the paper and hopefully get a sense of why it's been written and how it might reduce the risk of artificial intelligence causing an [existential catastrophe](https://en.wikipedia.org/wiki/Global_catastrophic_risk): that is, permanently and drastically curtailing humanity's future potential.

One approach to creating useful AI systems is to watch humans doing a task, infer what they're trying to do, and then try to do that well. The simplest way to infer what the humans are trying to do is to assume there's one goal that they share, and that they're optimally achieving the goal. This has the problem that humans aren't actually optimal at achieving the goals they pursue. We could instead code in the exact way in which humans behave suboptimally, except that we don't know that either. In this episode, I talk with Rohin Shah about his paper about learning the ways in which humans are suboptimal at the same time as learning what goals they pursue: why it's hard, how he tried to do it, how well he did, and why it matters.

**Daniel Filan:**
Today, we have Rohin Shah. Rohin is a graduate student here at UC Berkeley's Center for Human Compatable A.I., or CHAI. He's co-authored quite a few different papers and he's soon to be a research scientist at DeepMind. Today, we'll be talking about his paper ["On the Feasibility of Learning, Rather than Assuming, Human Biases for Reward Inference"](https://arxiv.org/abs/1906.09624). This appeared at ICML 2019 and the co-authors were Noah Gundotra, Pieter Abbeel and Anca Dragan. Welcome to the show. 

**Rohin Shah:**
Yeah, thanks for having me Daniel. I'm excited to be here. 

**Daniel Filan:**
All right. So I guess my first question is, what's the point of this paper? Why did you write it? 

**Rohin Shah:**
Yeah. So I think this was one of the first - this was the first piece of research that I did after joining CHAI. And at the time - I wouldn't necessarily, I just wouldn't agree with this now - but at the time, the motivation was, well, when we have a superintelligent system, it's going to look like an expected utility maximiser. So that determines everything about it except, you know, what utility function it's maximising. It seems like the thing we want to do is give it the right utility function. A natural way to do this is inverse reinforcement learning where you learn the utility function by looking at what humans do. But a big challenge with that is, like all the existing techniques, assume that humans were optimal. Which is clearly false. Humans are systematically biased in many ways. It also seems kind of rough to specify all of the human biases. So this paper was saying, well, what if we tried to learn the biases, you know, just throw deep learning at the problem? Does that work? Is this a reasonable thing to do? So that's why I initially started looking into this problem. 

**Daniel Filan:**
OK. So basically, the story is, we're going to - we to learn a utility function from humans, and we're gonna learn it by seeing what humans do and then trying to do what they're trying to do. And in order to figure out what they're trying to do, we need to figure out how they're trying to do it. Is that a fair summary? 

**Rohin Shah:**
Yeah. 

**Daniel Filan:**
And specifically, you're talking about learning rather than assuming human biases. Could you say more about exactly what type of thing you mean by bias? 

**Rohin Shah:**
Yeah. So this is bias in the sense of cognitive biases. Like if people have read "Thinking Fast and Slow" by Tversky and Kahneman it's like that sort of thing. So a canonical example might be hyperbolic time discounting where we basically discount the value of things in the future more than could be plausibly said to be rational in the sense that maybe right now, I would say that I would prefer two chocolates in thirty one days to one chocolate in thirty days. But then if I then wait 30 days and it's now the day where I could get one chocolate, then I'd say, oh, maybe I want the chocolate right now rather than having to wait a whole day for two chocolates the next day. So that's an example of the kind of bias that we study in this paper. 

**Daniel Filan:**
All right. And I guess to give our listeners a sense of what's going on, could you try to summarise the paper maybe in a minute or two? 

**Rohin Shah:**
Sure. So the key idea of how you might try to deal with these human biases without assuming that you know what they are, is to assume that the human has not just a reward function, which we're trying to infer, but also a planning module, let's say. And this planning module -

**Daniel Filan:**
And you put that in scare quotes, right? 

**Rohin Shah:**
Yeah, scare quotes. Exactly. Planning module scare quotes. And what this planning module does is it takes as input the environment - the world model where the human is acting, as well as what reward function the human wants to optimise and spits out a policy for the human. So this is like, how do you decide what  you're going to do in order to achieve your goals? And it's this planner, this planning module that contains the human biases. Like, maybe when if you think about overconfidence, maybe this planning module is - tends to select policies that choose actions that are not actually that likely to work. But the planning module thinks that it's likely to work. So that's sort of the key formalism. And then we tried to learn this planning module using a neural net alongside the reward function by just looking at human behaviour - well, simulated human behaviour - and inferring both the planner and the - both the planning module and the reward function that would lead to that behaviour. There is also a bunch of details on why that's hard to do. But maybe I will pause there. 

**Daniel Filan:**
Sure. Well, I guess that brings up one of my questions. Isn't that literally impossible, right? How can you distinguish between somebody who's acting perfectly optimally with one set of preferences or one reward function, you might say in the reinforcement learning paradigm, isn't that just indistinguishable from somebody who's being perfectly suboptimal, doing exactly the worst thing with exactly the opposite reward function? 

**Rohin Shah:**
Yep, that's right. And indeed, this is a problem if you don't have any additional assumptions and you just sort of take the most naive approach to this, where you just say "do back propagation end-to-end learning to just maximise agreement with human behaviour". You basically just get nonsense, you get a planning module and a reward that together produce the right behaviour. But if you then tried to interpret the reward as a reward function and optimised for it, that's just like, is basically pretty arbitrary, and you get random misbehaviour. And in our experiments, we show that, you get, if you do just that, you get basically zero reward on average. If you optimise the learned reward. These are with reward functions that are pretty symmetric. So you should expect that on average you'd get zero if you optimised a random reward. 

**Daniel Filan:**
OK, cool. So you're using some kind of some extra information, right? 

**Rohin Shah:**
That's right. So there are two versions of this that we consider. One is unrealistic in practice, but serves as a good intuition, which is: suppose there's just a class of environments and you know the reward functions for - and behaviour - for let's say half of them, some fraction of them, and for the other half, you only see the behaviour and not the reward functions. And the idea is that if the planning module is the same across all of these, you can learn what the planning module is from the first set where you know what the reward function is. And then, you use that planning module when talking about the second set where you're inferring the rewards. So in some sense, it's like a two phase process where you first infer what the biases are and then you use those biases to infer what the rewards are. There is a second version where instead of assuming that we have access to some reward functions, we assume that the human is close to optimal: meaning their planning module is close to the planning module that would make optimal decisions. And what this basically means is we initialise the planning module to be optimal and then we essentially say, OK, now reproduce human behaviour and you're allowed to make some changes to the planning module such that you can better fit the human behaviour. Which you could think of as like introducing the systematic biases, but since you started out near the optimum, started out as being optimal, you're probably not going to go all the way to being suboptimal or anything like that. 

**Daniel Filan:**
OK. So, yeah, I guess let's talk about those. In the paper, you sort of have these assumptions 1, 2a, and 2b, right? Which which you talked about a little bit. But I was wondering if you could more clearly say what those assumptions are and also how - in the paper you give sort of natural language explanations of what these assumptions are. But I was wondering if you could say, OK, how that translated into code. 

**Rohin Shah:**
Yeah. So the first assumption, assumption 1 which is needed across both of these two situations I talk about, is that the planning module is "the same" in scare quotes, again, for similar enough environments. So we - in my description, I assume you've got access to this set of environments in which the planning module works the same way across all of those environments. Now -

**Daniel Filan:**
Yeah, what does that mean? 

**Rohin Shah:**
It's not totally clear what that means. I wouldn't be able to write down a formal meaning of this, because if you tried to say - because there's always the planning module that says, well, if the environment is, you know, this specific environment where the ball is on top of the vase or whatever, then, you know, output this policy. But if it's this other environment, then do this other thing. And that's technically a single planning module that works on all of the environments. And so in some sense, really it's just, is there a reasonable or a simple - there's a reasonable or a simple planning module that's being used across all of the environments. And I think this is the sort of dependence on reasonableness or simplicity is just something that we are going to have to depend on, not necessarily in this particular way, but if you don't allow for it, you get into problems well before that. For example, the problem of induction in philosophy, which is just how do you know that the past is a good predictor of the future? How do you know - how can you know that - how can you eliminate the hypothesis that tomorrow, the evil god that has so far been completely invisible to us decides to like turn off the sun? 

**Daniel Filan:**
OK. But what does that amount to? Like, does that just mean you use one neural network, and -

**Rohin Shah:**
That's right. In our code, we just use a single neural network. Neural networks tend to be biased towards simplicity. So effectively it turns into - that becomes like a simplicity, kind of like a simplicity prior over the planning module. 

**Daniel Filan:**
All right. I guess I sort of understand that. So that was assumption 1. How about 2a and 2b? 

**Rohin Shah:**
Yeah. So assumption 2a is the version where we say that the demonstrator is close to optimal and we don't assume that we have any rewards. In that one, what we do is we take our neural net that corresponds to this planning module and we train it to produce the same things that value iteration would produce. Value iteration is an algorithm that produces optimal policies for the small environments that we consider. And so by training, basically, we're just training our neural net to make optimal predictions. 

**Daniel Filan:**
OK. And you're initialising at this optimal network? 

**Rohin Shah:**
Right, this training happens in an initialisation phase. Like, I would call this training the initialisation for the subsequent phase when we then use it with actual human behaviour. This all happens before we ever look at any human behaviour. We just simulate some environments. We compute optimal policies for those environments with value iteration. And then we train our planning module neural net to mimic those simulated optimal policies. So this all happens before we ever look at any human data. 

**Daniel Filan:**
OK. So assumption 2a essentially comes down to, when we train our networks to mimic humans, we're going to be initialised at this - trained at this - demonstrator that was trained on optimality. 

**Rohin Shah:**
Correct. 

**Daniel Filan:**
So one question I have is, why - initialisation seems like kind of a strange way to use this assumption. Like, if I were being - my default is to maybe be kind of Bayesian and then say, okay, we're going to have some sort of prior. Or maybe we're going to do this regularisation thing, where I know what the weights are of an optimal planner and I'm going to L2 regularise away from those weights. Initialisation, you know, the strength of that quote unquote 'prior' or something that you're putting on the model is going to depend a lot on how long you're training, what your step size is and such. So, yeah. Why did you choose these initialisation instead of something else?

**Rohin Shah:**
Really. That was just the first approach that occurred to me, and so I tried it and it seemed to work. 

**Daniel Filan:**
All right. 

**Rohin Shah:**
Reasonably well. I think - so, I think - I don't think I'd ever considered regularisation. That seems like another reasonable thing to do and does seem like it would be easier to control what happens with that prior. So that does seem - that does seem like a better approach, actually. I think I would lean away from the Bayesian perspective just because then you have to design a hypothesis space and so on. And the whole point is just to -

**Daniel Filan:**
I mean -

**Rohin Shah:**
Sorry. 

**Daniel Filan:**
Oh, I mean - I mean, regularisation is secretly Bayesian anyway, right? 

**Rohin Shah:**
Oh, sure. Yeah. Fair enough. I mean, I would say that I wouldn't be surprised if this initialisation was also secretly phase-in given the like other hyperparameters used in the - in the training. 

**Daniel Filan:**
OK, so that was 2a. Then there was also assumption 2b, right?

**Rohin Shah:**
So assumption 2b is pretty straight-forward. It just says that, you know, we had this set of tasks over which we're assuming the planning module is the same. And for half of those tasks, we assume that we know what the reward function is. And the way that we use this is that we - basically both the planning module and the reward function in our architecture is trained by end to end gradient - by gradient descent. So once, when we have assumption 2b, when we have the reward functions, we set the reward functions, and the human behaviour, and we freeze those and we use gradient descent to just train the planning module. And this lets the planning module learn what the biases are in planning. And then once we have the plan, once we - after we've done this training, the planning module is then frozen. And it now has already encoded all the biases. And then we use gradient descent to learn the reward functions on the new tasks for which we don't already have the rewards. And so there you are just inferring the reward functions with a - given your already learned model of what the biases are. 

**Daniel Filan:**
OK. Yeah, I guess I guess one comment I have on that assumption is initially it seems, well, initially it seems realistic, sometimes people are in situations and you know what, kind of what they want. And then you think about it a bit more. And it seems unrealistic because you're saying you know exactly what they want. But I think it's a little bit less unrealistic than the second phase thinks. For instance, one one cool research design you can do in microeconomic studies is to have lottery tickets that you use to pay people with, right? And the nice feature about lottery tickets is, if you assume that people want the lottery tickets more than they want nothing, a nice feature of lottery tickets is that if you want - if you get two lottery tickets, that's exactly twice as good as having one lottery ticket. Because, by the linearity of probabilities in expected utilities. So there are some situations in which you can actually make that work. I just wanted to share that research design. I think it's quite neat. 

**Rohin Shah:**
Wow. I love that this is a way to just get around the fact that utility is not linear in money. 

**Daniel Filan:**
Right? 

**Rohin Shah:**
That's cool. 

**Daniel Filan:**
It's excellent. Unfortunately, you only have so many lottery tickets you can give out, right? So you can't do it indefinitely. At some point they just have of the lottery tickets. And they won the lottery. And you can't give them any more. Until then - Yeah. All right. So I want to jump around in the paper a little bit. So the question I have is, in the introduction, you spend quite a bit of time saying all the strange ways in which humans can be biased and suboptimal or something. Reading this, I almost think that this might be a good argument for modelling modelling humans maximally entropically using something like the Boltzmann distribution, because there you're just saying, look, I don't know what's going on. I'm - I'm going to have no assumptions. But, you know, then I'm just going to I'm just going to use that probabilistic model that uses the least assumptions and in practice it does alright. So I guess I'm wondering, what do you think of this as an argument for Boltzmann rational models? 

**Rohin Shah:**
Yeah, I mean, so I think I want to note that the actual maximally entropic model is one that just uniformly at random chooses an action. 

**Daniel Filan:**
Which is in the Boltzmann family. 

**Rohin Shah:**
True, but if you did that, you'd never be able to learn about the reward because the human policy, just by assumption, does not have anything to do with the reward. So you need, sort of - I actually mostly agree with this now. I am not entirely sure what I would have said, you know, two and a half years ago when I was working on this, but I mostly agree with this perspective where, what you need out of your model is it needs to, assign a decent amount of probability to all the actions. And it also needs to rank actions that do better as having higher probability. And, that's it. Those are the important parts of your model. And if you take those two constraints, it's a Boltzmann rational model is a pretty reasonable model to come out with. And I think, you would expect, I think but I'm not sure, that this - well, it should at the very least, hurt your sample complexity in terms of like how long it takes you to converge to the right reward function compared to if you knew what the biases were. It also probably makes you converge to the wrong thing when people have systematic biases, because you sort of attribute - when they make systematic mistakes, you attribute it to them - in some sense the generative model that Boltzmann rationality is suggesting is that when humans make systematic mistakes a Boltzmann national model is like, well, I guess they, every single time they're in this situation, when they flip their coin to decide what action to take, the coin comes up tails and they take the bad action. It's just a weird generative model...

**Daniel Filan:**
The actual model would be that the human prefers that action in that situation, right? That would be the actual inference that a Boltzmann model - 

**Rohin Shah:**
Correct. Yeah. So either it would make the wrong inference or it would have - if it somehow got to it - I think more what I'm saying is like if we assume - you might expect that the Boltzmann rational level would not get to the true reward. Because if it were at the true reward, it would be having this weird generative model. And so, yeah, it would like make the opposite inference where you - anything that humans systematically make a mistake on, you would expect that humans wanted to do that, for some reason at least. It would have to invent some explanation by which humans want - that action was a good one. 

**Daniel Filan:**
That's an interesting thread. I might talk about - let me get a little bit more into the nuts and bolts of the paper before I pick up on that thread more. 

**Rohin Shah:**
Sure. 

**Daniel Filan:**
Another question I have is when you're doing these experiments, when you're learning models of the human demonstrator you use value information - sorry, value iteration networks or V.I.N.s, right? Could - for our audience, could you say what is a V.I.N? 

**Rohin Shah:**
Yeah. So a value iteration network is a particular architecture for neural networks. That is able to - that mimics the structure of the value iteration algorithm, which is an algorithm that can be used in A.I. to learn optimal policies for tabular Markov  Decision Processes. Point is, it's an example of a neural net whose architecture is good for learning a learning algorithm, which is the sort of thing that you want if you want to have a planning module. 

**Daniel Filan:**
OK. And it works in grid worlds, right? 

**Rohin Shah:**
Yes. I believe people have used it elsewhere, too? I want to say that someone adapted it to be used on graphs? But I used it on gridworlds and the basic design was definitely meant for gridworlds. 

**Daniel Filan:**
Sure. 

**Rohin Shah:**
It basically stacks a bunch of convolutional layers and then uses max pooling layers to mimic the maximisation in the value iteration algorithm. 

**Daniel Filan:**
Sure. So one - I guess one question that I have is, it seems - my vague memory of value iteration networks is that they can express the literally optimal value iteration, right? 

**Rohin Shah:**
That's correct. 

**Daniel Filan:**
So if that's true, why bother learning? You know, doing gradient descent on optimal behaviour to get a model of an optimal agent rather than just setting the weights to be the optimal thing?

**Rohin Shah:**
There was a reason for this. I'm struggling a bit to remember it. I'm guessing - partly it's that, you know, actually setting the weights takes some amount of time and effort. It's not a trivial - there's not a trivial way to do it. It depends a bit on your transition function, depends a bit on your reward function. It only becomes - it's able to express literal optimal value iteration when the horizon is long enough, which I think it was not in my case. And also I believe you might need multiple convolutional layers in order to represent the transition function and the reward function. But I am not sure. It's possible that those two things happen because the horizon - because of the horizon issue. 

**Daniel Filan:**
Okay. 

**Rohin Shah:**
Yeah. I did at one point, actually, just sit down and figure out what the weights were to encode optimal value iteration, because I was very confused why my value iteration network was not learning very well. And then I found out that, oh, I can't do it with my current architecture. But if I add an additional convolutional layer to the part that represents the reward, then I can. So I added that and then even the learned version started working much better, it was great. 

**Daniel Filan:**
OK. I mean, if you knew that adding this could let you express optimality. When, I don't know, I guess I have a vendetta against people learning things. 

**Rohin Shah:**
There is a reason -

**Daniel Filan:**
Against people training models that learn things. I'm in favour of human learning. To clarify. But you were saying.

**Rohin Shah:**
So there was a reason. The version that I wrote computed the optimal policy but did not compute the optimal explanation for human behaviour under - so (a) it wasn't Boltzmann rational. And (b), it's like computation of the Q-values is kind of sketchy. I don't remember if it was - I don't think it was actually the right Q-values and I don't remember why. It might just be that it didn't work for Boltzmann rationality. But you'd get the case where it'd be like if, you know, up was the correct action, you'd get up having a value of like 3.3, and then left having a value of like 3.25, and so on. And so, you get the right optimal policy, which is the main thing I was checking. But you wouldn't actually do very well according to the loss function it was using. 

**Daniel Filan:**
OK. So I guess the other half of this question is, what biases - you had this list of biases at the start. Are value iteration networks able to express these biases, and in general, what kinds of biases are they able to display? 

**Rohin Shah:**
That's a good question. I mean, in some sense, the answer is, are - anything, they can express arbitrarily - like, they're neural nets -

**Daniel Filan:**
I mean your value iteration networks. They had a finite width and a finite depth, right?

**Rohin Shah:**
Yeah. My value iteration networks? Hard to say. I think they could definitely express the underconfidence and overconfidence ones. Well, sorry. Maybe, now even that, you have to compute the amount by which you should be underconfident or overconfident, I'm not sure the lay- there were enough layers to do that exactly. I think in general the answer is the networks I was using could not in fact, literally exactly compute the biases that I was doing, but they would get very, very close in the same way that neural nets usually cannot just - depending on your architecture, they can't exactly multiply two input numbers, but they can get arbitrarily close. 

**Daniel Filan:**
OK. So I guess my - this leads to another question of, I guess, the setup for your paper. You sort of encode biases as having this - as having your planner be the slightly wrong value iteration network. At least at the time of doing inference. But interestingly, you assume that the world model is accurate. Now, when I think of bias, like either cognitive bias or, you know, the kind of thing that I might read in Thinking Fast and Slow, a lot of it is about making wrong inferences or having a bad idea of what's going on in the world. So I was wondering why you chose to have, you know, bias specifically in the planning phase and the focus on learning that. 

**Rohin Shah:**
Yeah. I think you - I mean, two answers. The first answer is, you know, this was the one that I had some idea of how to deal with. 

**Daniel Filan:**
All right, that's fair. 

**Rohin Shah:**
Second answer: You can view - the ones where you're - you have a bad model of the world, you can also view that as your planning module transforms the true model into a bad model and then does planning with that bad model. This is actually what value iteration networks kind of do, they learn the transition function, the weights of the value iteration network encode what the transition function is. 

**Daniel Filan:**
I mean -

**Rohin Shah:**
Or sorry, that's -

**Daniel Filan:**
If I were really -

**Rohin Shah:**
Yeah. 

**Daniel Filan:**
Yeah. If I if I were interested in understanding humans, it seems to me the typical human bias is not well modelled by, somewhere in your brain has the exact model of how everything in the world works. Right?

**Rohin Shah:**
That seems right. Yeah, I mean -

**Daniel Filan:**
Point taken about how you've got to focus on something. 

**Rohin Shah:**
Yeah. I think in this case, it was actually that the transition dynamics were the same across all the environments and the value iteration network was allowed to learn a warped version of them. Which is not the same thing as when humans look at the world, they misunderstand what the transitions are. This is more like when we came up with our model of what the human planner was doing, we put into it this incorrect model of how the world works. So that is still a difference. But it isn't like we learnt a planner that gets the correct transition dynamics and then warps them. I know I said that earlier. I more meant that the optimisation was doing that. Or possibly I just said the wrong thing earlier. I'm not entirely sure which which I did. 

**Daniel Filan:**
So I guess next, our listeners are probably wondering what happened. I gather you did some experiments and got some results. Could you describe briefly, roughly, what experiments you ran? And overall, what were the results? 

**Rohin Shah:**
Yeah. So it was pretty simple. We just simulated a bunch of biases in gridworlds. And we - let's see - oh, I'll just look at the paper. It was a naive and sophisticated version of hyperbolic time discounting, a version where the human was overconfident about the likelihood of their actions succeeding. Another one where they were underconfident about the likelihood of their actions succeeding. And one where the human was myopic, so wasn't planning far, far out into the future. So we had all of these biases. We would then generate a bunch of environments and simulate some human behaviour. Simulate human behaviour on those environments. So this created a dataset of environments in which we had human behaviour and we also had the ground truth rewards that were used to create that behaviour. So we had a metric to compare against. Then we would take this dataset and then depending on whether we were using assumption 2a or 2b, we'd either remove all of the reward functions or only half of the reward functions and give it to the algorithm and it had to predict what the reward functions - it had to predict the reward functions that we didn't give it. And then it was evaluated on how well it could predict those reward functions. In particular, it was, if we then optimise the inferred reward functions, how much of the true reward function would that policy obtain? 

**Daniel Filan:**
So importantly, you're measuring the the learned planner, learned reward pair rather than just the learned reward function. 

**Rohin Shah:**
No, sorry. We take the learned reward function and we optimise it with the perfect planner, not the learned planner. 

**Daniel Filan:**
Oh okay. All right. So you're evaluating the reward function by if you planned perfectly with it, how much reward you would get, which is a pretty stringent standard. Right? Because if your reward function - well, actually in a tabular setting, if your reward function is a little bit off then the optimal policy only gets a little bit less reward. But, in general, this can be a little bit tricky. 

**Rohin Shah:**
Yeah, it's a stringent setting in the sense that, at least in the environments we were - it's kind of stringent and kind of not stringent. In the environments we were looking at, it mostly mattered whether you got the highest reward gridworld entry correct, because that was the main thing that determined the optimal policy. It was not the only thing, but it was the main thing. So you needed to get that correct, mostly. But it also mattered to know where the other rewards were, because if you can easily pick up a reward on the way to the best one, that's often a good thing to do. Sometimes, I forget if this was actually true in the final experiments that we ran, but sometimes if the reward is far enough away, you just want to stay at a maybe slightly smaller but closer reward and just take that instead. But for the most part, you mostly need to predict where is the highest reward in this grid world. 

**Daniel Filan:**
OK. And what kind of results did you get? 

**Rohin Shah:**
So if you don't make any assumptions at all. Well, if you take assumption 1 but not assumption 2a or 2b, the ones that let you get around the impossibility results - so basically you just don't run the initialisation where you make the neural net approximately optimal - then you just get basically zero reward on average. So that was the first result: yep, the impossibility result, it really does affect things. You do have to make some assumptions to deal with it. Then for the versions that actually did use assumptions, we found that it helped relative to assuming either a Boltzmann rational model of the human or a perfectly optimal model of the human. But it only helped a little bit. And this was only if you controlled for using a differentiable planner - sorry, from using this planning module, because it introduces a bunch of approximation error. So when I say we compared to having a Boltzmann rational human model, I don't mean that we use an actual Boltzmann rational model. I mean, we simulated a bunch of data from Boltzmann rational models, trained the planning module off of that data. And then use that trained planning module to infer rewards. And this was basically to say, well, differentiable planning modules are not very good. We want it to be consistent across all of our comparisons. But that's obviously going to not hobble, the - well, it's not going to hobble it relative to the others. But if you were just going to assume the Boltzmann rational model, you wouldn't need to do this differentiable planner thing, and so you would do better. 

**Daniel Filan:**
So basically you said, assuming either 2a or 2b did really help you a little bit compared to assuming neither, but using a differentiable planner, but using a differentiable planner is quite a bit of loss. 

**Rohin Shah:**
That's right. 

**Daniel Filan:**
OK. Do you have info about how 2a and 2b compared, if you only wanted to use one of them?

**Rohin Shah:**
Yes. So you should expect that 2a does quite a bit. I mean I expected that 2a would do - sorry, I forget which is which - I expected that 2b would do quite a bit better than 2a, so like -

**Daniel Filan:**
For our listeners who might've forgotten what 2a and 2b are -

**Rohin Shah:**
Yeah, I was about to say. If you knew - I thought I would that it would help significantly to know what the reward functions were. So, you know half the reward functions and that lets you ground your learning of the biases. 

**Daniel Filan:**
And you thought that would help more than assuming - than training your learnt model starting from optimality? 

**Rohin Shah:**
Yeah, because in some sense, there's now some sort of ground truth about the biases. There's a good learning signal. There's no impossibility result that you're trying to navigate around. It seems like a much better situation. And it did do a bit better, but only a little bit better, I was surprised at how -

**Daniel Filan:**
Only a little bit better than what? 

**Rohin Shah:**
If you had assumption 2a instead where you just initialise to optimality. 

**Daniel Filan:**
So, so slightly better to know the rewards compared to assuming that it was close to optimal, but not actually very much better. That's what you're saying. 

**Rohin Shah:**
Yep, that's right. Yeah. And like really it was a lot better in two of the conditions that we checked in, a little bit worse in a couple of the conditions we checked in and on average, it washed out to be a little bit better. 

**Daniel Filan:**
Okay. All right, that's interesting. So one thing that's interesting to me about your results section. If I read section 5.2, it comes off as more scientific than most machine learning papers, I want to say. Just in the sense that it seems to be interested in carefully testing hypotheses. And in the paper you have these headings of manipulated variables and, you know, dependent measures and various comparisons. So I guess I'm wondering, firstly, why did you adopt this slightly non-standard approach and, maybe related to this, what do you think of the scientific rigour of mainstream machine learning? 

**Rohin Shah:**
Oh man, controversial questions.

**Daniel Filan:**
Let's start with the easy one. Why do you adopt that approach? 

**Rohin Shah:**
I mean, the actually correct answer is I am advised by Anca. 

**Daniel Filan:**
Who's Anca? 

**Rohin Shah:**
Anca Dragan is a professor at UC Berkeley. She is one of my advisors. And of my advisors, she had the most input into this paper. And she is very big on doing experiments more in the style of normal scientific experiments as opposed to the typical ML experiments. So the first answer is because my advisor is Anca. But I do agree with Anca about this, where... There definitely is a point to the normal ML way of doing experiments where, this is an oversimplification, but the point is basically to show that you do better than whatever previously happened. This structure does lead to significant progress on any metric that is deemed to be something that you can do this sort of experiment on. And so it does tend to incentivise a lot of progress in cases where we can crystallise a nice metric. I am less keen on these sorts of experiments, though, because I don't see the main problems in AI research as "we have these metrics and we need to get higher numbers on them". I think that there are much more - all of the things in AI that are are interesting to me, even if we set out, set aside AI safety particularly, look more like, "Oh my God, what's going on with deep learning? It's got all these crazy empirical facts that I wouldn't have predicted a priori. What's going on there? Can we try to understand it?" Those are the vein, the types of questions that I'm interested in and for those sorts of questions, if you are running an experiment, you would run experiments to learn new information. If you already know how the experiment is going to come out, it's kind of a pointless experiment to run. The point of running that experiment is to successfully publish a paper. And I've done my share of that. But those aren't the experiments that I'm usually excited about. 

**Daniel Filan:**
All right. So I guess now that we're getting more philosophical: my understanding is that you think of yourself as an AI alignment researcher or an AI safety researcher or something. Is that right? 

**Rohin Shah:**
Yes. 

**Daniel Filan:**
So. How do you see this paper fitting into some path to create safe or aligned AGI? 

**Rohin Shah:**
Yeah, so I think there's like the path that I mentioned way back when at the beginning of this podcast. 

**Daniel Filan:**
So that was the idea that we were going to learn some utility function. And just, whenever we had a task we wanted an AI to do, we would have a human do that task and then have the - or, what does that even look like? Can you say more? 

**Rohin Shah:**
Yeah. I mean, I'm not particularly optimistic about this path myself. But it's not an obvious - I feel like while relying just on learning a reward function from human behaviour that can then be just perfectly optimised, I think I'm fairly confident that that will not work. But it seems likely that there are plans that involve learning what humans want and having better methods to do that seems valuable. Whether it had to specifically be about systematic biases and whether they can be learnt, I think that part I feel is less important at this point. But you do need to account for human biases at some point. So to outline it, to outline maybe more of a full plan or something, you could imagine that we build AI systems and  we're training them to be essentially helpful, to be good personal assistants with superhuman capabilities at various things. But still thinking the way personal assistants might do in the sense of, they're not sure what your preferences are, what you to happen, and so they need to clarify that with you. And so on. This feels like one of the subtasks that such an agent would have to do. 

**Daniel Filan:**
Inferring what you want? 

**Rohin Shah:**
Yes, exactly. 

**Daniel Filan:**
I guess if I think about machines that I employ to do things for me - if I want video conferencing software or even if I were to get an employee, not that I've done that, usually the way I get good video conferencing software is that I do not first demonstrate the task of relaying video from and audio from one place to another very quickly, right? Because I just can't do that. I can't even do an approximation of that. And similarly, with employees - I guess there's probably a little bit of instruction by demonstration, but don't think that that's the main way we communicate tasks to people. Right? Am I wrong about this? 

**Rohin Shah:**
No, that seems right. I think you're you're conflating the evaluation that we did with the technique. The technique is trying to infer what the reward is, right? So I think the the a better analogy would be, I don't know, since it's sort of assuming optimal demonstrations, it's more like assuming that you have a magic camera that gets to watch the person as they go about their day to day life, and I guess also they're not aware of this camera, which is not a great assumption. But let's assume that for now. So, you just sort of watch the human go around with their life and you're like, "OK. Based on the fact that they, you know, had cake today, I can infer that they would like cake or something." But maybe then you're like, "Oh, no. But actually, humans have this short term bias, so I shouldn't infer that they don't care about their lifespan or their their overall level of health. It could just be that even though reflectively they would endorse being healthy, having a long lifespan, they, in the moment, went with a short term preference for nice sweet cake."

**Daniel Filan:**
OK. I want to, this always comes up in these discussions, and I want to defend eating cake. I feel like you can care about your lifespan and also eat some cake. 

**Rohin Shah:**
Wait. Yes. That's totally true. I'm just saying you don't want to over update on it. 

**Daniel Filan:**
You don't want to - sure. But I mean, we already know that - I don't know. Presumably your AI spy camera is also going to see you put on a seatbelt. Right? I feel like there's - I don't know. I feel like there's already a bunch of information about you caring about your life and like eating a bit of cake is not strong evidence that you don't actually care at all about your future lifespan, even under a naive - even if you assume that people are acting optimally. I just feel like all of these examples are super biased against cake. You know -

**Rohin Shah:**
Fair enough. 

**Daniel Filan:**
"Oh, we wouldn't want people to think that eating cake is ever a good idea, or is a human value." You know? Sometimes cake's nice.

**Rohin Shah:**
OK. Sure. I'll stay away from the cake. But I think I wouldn't say that - if you assume perfect optimality in that situation, I think you don't learn something like, "Oh, humans care about having a long lifespan." You learn something more like "Humans don't want to be in violent accidents, but they don't mind dying of whatever it is that cake causes." You can always inject more and more state variables to distinguish behaviours in order to explain why humans seem to not care about their life in one case, but do care about their life in the other case. And that's also a thing you don't want your systems to do. 

**Daniel Filan:**
So I guess going up a level, is the idea that I'm going to - that the way AGI is going to work is: the product that AGI Corp., the corporation that sells AGI, this product is going to be, OK. You're going to have this - you'll wear it like a GoPro on your head for a while or something. And the system is going to just learn roughly what you value in life. And it's just going to generically do things to get you more value. To make your life more like you want it to be. Is that it? 

**Rohin Shah:**
Plausibly? 

**Daniel Filan:**
That seems a little bit scary, I don't know. I think I want a product to do a thing. Right? 

**Rohin Shah:**
Sorry, say that again?

**Daniel Filan:**
I guess it seems to me that, if I want a superintelligent system, I'd rather first - I'd rather have a superintelligent system that did a well-defined task rather than generically making my life better, or at least I think, when I think about AI alignment, it seems like we would be able to figure out how to create an aligned, safe AI that does one task before we figured out how to create an aligned AI that generically makes all of your life better. And, I'm kind of on board with the arguments that we don't know how to have an aligned AI that does one concrete task, even if, you know, you can still allow problems of vagueness of specifying what the task is if the task's 'build a thriving city' or something. But I find it weird that so much of the field is about 'generically, make your life good'. 

**Rohin Shah:**
I feel like there's not that much of a difference between these. 'Build a good city' seems pretty similar to 'build an agent that generically helps me'. And it turns out that in this particular case, what I really want to do is make a city. You would still want these personal assistants to defer to you. And if you give explicit instructions to obey those, I think you do want to shoot for that. So it doesn't feel like those at that different in terms of the actual things that they would do. In terms of research strategies for how to accomplish this, I guess the versions where we're trying to build AGIs that can do these broad, vague tasks, well, we're saying, OK, we need to first figure out how to make an AGI that does this task. It seems to me - I just don't see what benefits this gives, how this makes the problem easier than just 'build an AGI that's trying to help me.'

**Daniel Filan:**
So, I mean, one benefit is, well, it just seems - OK, let's go with the city task first, right? The thing with the city task is, you're not - I think the way to do it is not 'OK, I'm going to watch a human try to do urban planning for a month or something, and then I the AI am going to take over.' That's not what you're suggesting, right? Is it actually, I can't tell. 

**Rohin Shah:**
No. So, for the city planning case in particular, this algorithm is going to be, I mean it might infer some sort of common sense details, but it's not going to infer what a good city looks like because you just don't get information about that by looking at a single human. So you need something else. So I think I'm more, let's leave this paper aside, I am not at all convinced that it will matter at all for AGI alignment. I would not bet on that. 

**Daniel Filan:**
All right. 

**Rohin Shah:**
But I do think the general idea of, 'oh the AI system is going to try to help us and will be inferring our preferences as part of that', that is something I'm more willing to stand behind. I think in this case, it looks more like, the AI system, when you tell it, "Hey, please design me this city", it goes around and reads a bunch of books about how to design cities, if such books exist, I don't know. It looks at what previous urban planners have done. It maybe surveys the people who are going to live in the city to figure out what they would like in a city, it periodically checks in with you and says "This is what I'm planning to do with the city. Does that seem good to you?" And if I then say, "This is the reason that's bad, this doesn't seem good to me for X reason", they can say "Well, I chose that for Y reason because I thought you would prefer Z, but if you don't, then I can switch it to this other thing." I'm maybe rambling a bit here. 

**Daniel Filan:**
So I guess you're imagining, OK, we're going to have these AI systems, they're going to do tasks that are kind of vague. But the way they're going to do that is they're going to infer human preferences, basically in order to infer what we mean when we say 'please build a good city'. And they're going to do that by a whole bunch of sources of information, and maybe one fifth of what they're doing is looking at people who are trying to do the task and trying to infer what the task was, assuming they were, you know, doing a good job of trying to do the task. 

**Rohin Shah:**
I don't know about one fifth. 

**Daniel Filan:**
Well, you said a lot of things and very few of them sounded like inverse reinforcement learning to me. 

**Rohin Shah:**
Sorry, I was imagining much less than one fifth, to be clear. 

**Daniel Filan:**
OK, all right. A small amount. I'm happy with less than or equal to one fifth. And I guess this gets a bit into other work of yours that you've collaborated on that we won't be talking about right now. But I guess I can kind of understand this. But to answer a question you asked me a little bit earlier, I think the reason that I want to do the 'plan a city' rather than 'generically make my life better'. Firstly, I think I want to be clear that if indeed we are trying to create an AGI that can plan a city rather than generically make your life better, if somebody says that out loud, then we can check if our research helps with that. So that's one reason to be a little bit clear about it. The reason to prefer planning a city to generically making your life better is firstly, intuitively it seems like an easier job. If you're planning a city, the way you're generically making my life better is by planning a city that's good. If you're generically making my life better, you're generically making my life better in every single way, including presumably the way of maybe occasionally planning a city to make my life better. So it must be strictly easier to do the first thing. And then thirdly, I prefer the 'plan a city' type task, because at the end you can check if a city got planned, and roughly evaluate the city and see if it's a good city. And that seems like a target that you're going to know if you hit it more easily than you're going to know if this AI system generically made your life better. I'm wondering what you think of those points. 

**Rohin Shah:**
Yeah. I think I didn't understand the first point, but maybe I'll talk about the second and third first, and then we can come back to the first point. 

**Daniel Filan:**
I'll say the first point. Maybe it wasn't responding to anything, but I think that the first one is just we should try to be clear what problem our research is trying to solve. And I guess the second and third point are arguments that planning a city and generically making your life better are importantly different problems, and about why one of them is better than the other. 

**Rohin Shah:**
Yeah, I mean, first point seems good. Being clear about what we're trying to do seems good. 

**Daniel Filan:**
It's so rare in papers, right? 

**Rohin Shah:**
It is. It's annoyingly difficult to get papers that say exactly what we're trying to do to be published. It's very sad. I tried with NeurIPS. We'll see whether or not it works. But. For the second point, I agree that the like planning a city type task must be strictly - should be strictly easier if you're imagining that your 'help me generically' AI system could also be asked to plan a city. I think that's more of a statement about capabilities though, where I'm like, OK. But I sort of see the safety, the alignment, the good properties that we're aiming for here in AI safety and AI alignment coming from the 'trying to help you' part. And we can have different levels of competence at helping. So, maybe initially, we just have agents that are trying to help us schedule meetings on a calendar and, you know, not doing anything beyond that because they're not competent at it. And, you know, as we get to more general agents we'll need to ensure that these agents know what they can and cannot do so that they don't try to help us by doing something that they are incompetent at, where they just ruin things without realising it. And that's one additional challenge you have here. But I sort of see this as, most of the safety comes from the agent being helpful in the first place. And that's the reason I'm aiming for that instead of the things like 'plan a city'. Remind me what the third point was?

**Daniel Filan:**
The third point was that you can check if you've succeeded at planning a city more easily than you can check if you've generically had your whole life been a bit better. 

**Rohin Shah:**
Yeah, I guess I don't see why that's true. It seems like I totally could evaluate whether my life is better as a result of having this AI system. Maybe the AI tricks me into thinking my life is better when it's not actually but the same thing can happen with a city. 

**Daniel Filan:**
I mean, to me, it seems like building a city is a better defined task. Hmm, I guess there are so many ways... Yeah, maybe this is wrong. It just feels like there are so many more ways in which my life could plausibly be better. But at least with the city. It feels like I can check if it's there or not. 

**Rohin Shah:**
Yeah, I mean, I think - So, it depends a little bit on how you're going to quantify the helpful part. Maybe you're just like, you know, as one metric, did the AI assistant follow the instructions that I gave? Was it competent at those instructions? Did it infer something that I wanted without me having to say it or something like that. But I feel like we can, or I would guess that at least all the people who hire personal assistants are in fact able to tell whether those personal assistants help them or not. And hopefully they can distinguish between bad and good personal assistants. 

**Daniel Filan:**
I mean, I think that's because they give the personal assistants specific tasks like 'please do my taxes'. 

**Rohin Shah:**
Yes. And I we will plausibly do that, too. But even our personal assistants can take - often, I expect, take a lot of initiative. I guess one example for me personally is I write the alignment newsletter and, I guess not that recently anymore, for quite a while now it's been published by somebody else, specifically Georg [Arndt] from the Future of Humanity Institute, and also Sawyer [Bernath] from BERI helps run it as well. And at some point I was like, "You know, we should probably switch from this pretty plain template that MailChimp has to something that has a nicer design". And I mostly just said this. And then Sawyer and Georg just sort of did it periodically sending a message being like "This is the plan". And I'd be like, "Yep, thumbs up". And then at the end of it, there was a design. It was great. And in some sense, I did specify a task, which is, 'let's have a pretty design'. But it really felt like it was a fairly vague kinda sorta instruction but not really, that they just then took and executed well on. And I sort of expect it to be similar for AI systems. 

**Daniel Filan:**
I think that's fair. Yeah, I guess the other thing I want to pick up on was in your answer, it sounds like you think there is this core of 'being helpful'. That like there's there's some technique to be helpful. And, you know, you can just be better or worse at it. And once you know how to be helpful, you can be helpful at essentially anything. That's my interpretation of what you said. I'm wondering to what extent you think this is right, and that you see the important part of the AI alignment or safety community as trying to figure out computationally what it means to be helpful. 

**Rohin Shah:**
That seems broadly correct to me. I think I wouldn't say that it's the entire AI alignment community's thing that they're doing, I think there's a subset that cares about this and a different subset does other things -

**Daniel Filan:**
So my question was whether you think that's what they should be doing. Whether that's what the problem is. 

**Rohin Shah:**
I think there's some meta level outside view that's like, "oh, man, we should be encouraging diversity of thought" or whatever. But if you were like, "what is your personal thing that seems most promising to you such that you'd want to see at least the most resources devoted to it", yeah, I think it's right to say that that would be AI that is trying to help you, or trying to do what you want, is how I think Paul Cristiano would phrase it. 

**Daniel Filan:**
It sure seems like there are a lot of sub problems of that. If I imagined that being my main thing, it's like, how much have I made my life easier?

**Rohin Shah:**
It does feel like it's got a domain independent core or something. If you look at assistance games, which were previously called Cooperative Inverse Reinforcement Learning games or maybe just Cooperative Inverse Reinforcement Learning, I feel like that is a nice crisp formalisation of what it means to be helpful. It's still making some simplifying assumptions that are not actually true. But it really does seem to incentivise quite a lot of things that I would characterise as helpfulness skills or something. It incentivises preference learning, it incentivises, you know, asking questions when being unsure, it incentivises asking questions only when they become relevant and not asking about every possible situation that could ever come up at the beginning of time. It incentivises learning from human behaviour, passively observing and learning from human behaviour, which is the sort of thing we were talking about before. So I don't know. It feels like this is a thing that we can, in fact, get agents to do in a relatively domain-independent way. And if we succeeded at it, then there would not be existential risk any more. 

**Daniel Filan:**
OK. Well, on that note, I think we've had a good conversation. Hopefully our listeners understand the paper a little bit better. But of course, I would recommend reading it. The name of the paper is "On the feasibility of learning, rather than assuming, human biases for reward inference". Today's guest has been Rohin Shah. Rohin, if viewers wanted to follow your work, what should they do? 

**Rohin Shah:**
Yeah. I mean, the most obvious thing to do is to sign up for the [Alignment Newsletter](https://rohinshah.com/alignment-newsletter/). This is a newsletter I write every week that just summarises recent work in AI alignment, including my own. So that's a good place to start. It's also available in podcast form. Other things, I write some stuff on the Alignment Forum so you could go to [alignmentforum.org](https://www.alignmentforum.org/) and look for my username, just [search](https://www.alignmentforum.org/search?terms=Rohin%20Shah) for "Rohin Shah". And I think the last thing would be the papers that I've written, links to them, are all available on my website, which is [rohinshah.com](https://rohinshah.com/). You can also find a link to sign up to the Alignment Newsletter there. 

**Daniel Filan:**
All right. Thanks for today's interview, Rohin. 

**Rohin Shah:**
Yeah. Thanks for having me. 
