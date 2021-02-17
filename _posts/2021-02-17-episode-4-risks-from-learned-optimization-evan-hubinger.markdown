---
layout: post
title:  "4 - Risks from Learned Optimization with Evan Hubinger"
date:   2021-02-17 15:15:00 -0800
categories: episode
---
[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/NmY0Y2NjODMtNjc2NS00OWZmLWFmNWUtNWIzOWQxMjZmN2Vm)

**Daniel Filan:**
Today, I'll be talking to Evan Hubinger about risks from learned optimization. Evan is a research fellow at the Machine Intelligence Research Institute, was previously an intern at OpenAI working on theoretical AI safety research with Paul Christiano. He's done software engineering work at Google, Yelp and Ripple, and also designed the functional programming language, Coconut. We're going to be talking about the paper ["Risks from Learned Optimization in Advanced Machine Learning Systems"](https://arxiv.org/abs/1906.01820). The authors are Evan Hubinger, Chris van Merwijk, Vladimir Mikulik, Joar Skalse and Scott Garrabrant. So Evan, welcome to AXRP.

**Evan Hubinger:**
Thank you for having me Daniel.

**Daniel Filan:**
So the first thing I wanted to say about this paper is it has a nice glossary at the end with what various terms mean. And I think this is unusual in machine learning papers. So just straight off the bat, I wanted to firstly thank you for that and let potential readers of the paper know that it's maybe easier to read than they might think.

**Evan Hubinger:**
Thank you. We worked hard on that. I think one of the things that we set out to do in the paper was to define a lot of things and create a lot of terminology. And so in creating a lot of terminology, it's useful to have a reference for that terminology, and so we figured that would be helpful for people trying to go back and look at all of the terms that we're using.

**Evan Hubinger:**
And terms evolve over time. It's not necessarily the case that the same terms that we use in our paper are going to be used in the future or people might use them differently, but we wanted to have a clear reference for, "We're going to use a lot of words. Here are the words, here's what they mean and here's how we're defining them, here's how we're using them." So at least if you're looking at the paper, you could read it in context and understand what it's saying and what the terms mean and everything.

**Daniel Filan:**
Yeah, that makes sense. So just a heads up for the listeners, in this episode, I'm mostly going to be asking you questions about the paper rather than walking through the paper. That being said, just to start off with, I was wondering if you could briefly summarize what's the paper about, and what's it trying to do?

**Evan Hubinger:**
Yeah, for sure. So essentially the paper is looking at trying to understand what happens when you train a machine learning system, and you get some model, and that model is itself doing optimization. It is itself trying to do some search, it's trying to accomplish some objective. And it's really important to distinguish between---and this is one of the main distinctions that the paper tries to draw, is the distinction between the loss function that you train your model on, and the actual objective that your model learns to try to follow internally.

**Evan Hubinger:**
And so we make a bunch of distinctions, we try to define these terms where specifically we talk about these two alignment problems. The outer alignment problem, which is the problem of aligning the human goals, the thing you're trying to get it to do with the loss function. And the inner alignment problem, which is how do you align the actual objective that your model might end up pursuing with the loss function. And we call this actual objective the mesa-objective, and we call a model which is itself doing optimization a mesa-optimizer.

**Daniel Filan:**
What's mesa, by the way?

**Evan Hubinger:**
Yes, that's a very good question. So mesa is Greek. It's a prefix and it's the opposite of meta. So meta is Greek for above, mesa is Greek for below. The idea is that if you could think about meta optimization in the context of machine learning, meta optimization is when I have some optimizer, a gradient descent process, a training process or whatever. And I create another optimizer above that optimizer, a meta optimizer to optimize over it.

**Evan Hubinger:**
And we want to look at the opposite of this. We want to look at the situation in which I have a training process. I have a loss function. I have this gradient descent process. What happens when that gradient descent process itself becomes a meta optimizer? What happens when the thing which is optimizing over, the model, the actual training neural network itself develops an optimization procedure. And so we want to call that mesa-optimization for it's the opposite of meta-optimization instead of jumping and having optimization one level above what we're expecting, we have optimization one level below.

**Daniel Filan:**
Cool, cool. So one question I have about this paper is, you're thinking about optimization, where's optimization happening, what's it doing. What's optimization?

**Evan Hubinger:**
Yeah, so that's a great question. And I think this is one of the sorts of things things that is notoriously difficult to define, and there's a lot of definitions that have existed out there, with different people trying to define it in different ways. You have things like Daniel Dennett's intentional stance, you have things like Alex Flint recently had [a post](https://www.alignmentforum.org/posts/znfkdCoHMANwqc2WE/the-ground-of-optimization-1) on the Alignment Forum going into this.

**Evan Hubinger:**
In risks from learned optimization, we define optimization in a very mechanistic way where we're like, "Look, a system is an optimizer if it is internally doing some sort of search according to some objective, searching through a space of possible plans, strategies, policies, actions, whatever."

**Evan Hubinger:**
And the argument for this is, well once you have a model which is doing search, it has some large space of things and it's trying to look over those things and select them according to some criterion, that criterion has become an objective of sorts, because now the way in which its output is produced, has a large amount of optimization power, a large amount of selection going into choosing which things satisfy this criterion. And so we really want it to be the case that whatever that criterion that exists there is one that we like.

**Evan Hubinger:**
And furthermore, it's the sort of thing which we argue you should actually expect, that you will learn models which have this sort of search, because search generalizes really well, it's a relatively simple procedure. And then finally, having this mechanistic definition lets us make a lot of arguments and get to a point where we can understand things in a way where if we just adopted a behavioral definition like the intentional stance for example, we wouldn't be able to do it. Because we want to be able to say things like, "How likely is it that when I run a machine learning process, when I do a training process, I will end up with a model that is an optimizer?"

**Evan Hubinger:**
And it's pretty difficult to say that if you don't know what an optimizer looks like internally. Because I can say, "Well, how likely is it that I'll get something which is well described as an optimizer." And that's a much more difficult question to answer because you don't know the process of gradient descent, the process of machine learning, it selects on the internals, it selects on ... we have all of these implicit inductive biases which might be pushing you in one direction and then you have this loss function which is trying to ensure that the behavior of the model has particular properties.

**Evan Hubinger:**
And to be able to answer whether or not those inductive biases are going to push you in one direction or another, you need to know things like, "How simple is the thing which you are looking for according to those inductive biases? How likely is it that the process of gradient descent is going to end up in a situation where you end up with something that satisfies that?"

**Evan Hubinger:**
And I think the point where we ended up was not that there's something wrong necessarily with the behavioral stance of optimization, but just that it's very difficult to actually understand from a concrete perspective, the extent to which you will get optimizers when you do machine learning if you're only thinking of optimizers behaviorally, because you don't really know to what extent those optimizers will be simple, to what extent will they be favored by gradient descent and those inductive biases and stuff. And so by having a mechanistic definition, we can actually investigate those questions in a much more concrete way.

**Daniel Filan:**
One thing that strikes me there is, if I were thinking about what's easiest to learn about what neural networks will be produced by gradient descent, I would think, "Well, gradient descent, it's mostly selecting on behavior, right?" It's like, "Okay, how can I tweak this neural network to produce behavior that's more like what I want." Or outputs or whatever is appropriate to the relevant problem. And famously, we currently can't say very much about how the internals of neural networks work. Hopefully that's changing, but not that much.

**Evan Hubinger:**
Yeah, that's absolutely right.

**Daniel Filan:**
So it strikes me that it might actually be easier to say, "Well, behaviorally, we know we're going to get things that behave to optimize an objective function because that's the thing that is optimal for the objective function." So I was wondering: it seems like in your response, you put a lot of weight on "Networks will select simple ..." or "Optimization will select simple networks or simple algorithms". And I'm wondering, why do you have that emphasis instead of the emphasis on selecting for behavior?

**Evan Hubinger:**
Yeah, so that's a really good question. So I think the answer to this comes from ... so first of all, let me just establish, I totally agree. And I think that it's in fact a problem that the way in which we currently do machine learning is so focused on behavioral incentives. One of the things that in my research, I actually have spent a lot of time looking into and trying to figure out is how we can properly do mechanistic incentives. How could we create ways to train neural networks that are focused on making them more transparent or making them able to satisfy some internal properties we might want.

**Evan Hubinger:**
That being said, the reason that I think it's very difficult to analyze this question behaviorally is because fundamentally, we're looking at a problem of unidentifiability. If you only look at behavior, you can't distinguish between the model which is internally doing some optimization process, it has some objective and produces the particular results, versus a model which doesn't have any internal objective but it still produces the same results on the training process which has a bunch of heuristics.

**Evan Hubinger:**
But those two models have very different generalization behavior. And it's the generalization behavior that we care about. We're looking for situations in which ... and we talked about this a little bit, there is [this post](https://www.alignmentforum.org/posts/2mhFMgtAjFJesaSYR/2-d-robustness) by Vlad that talks about this 2D robustness picture where the thing that we're fundamentally concerned about is a situation where you have a model and the model is very robust in the sense of competitiveness, it's very capability robust. It has the ability to pursue whatever objective that it's pursuing very effectively off distribution and on distribution, but it's not very objective robust.

**Evan Hubinger:**
So it learned some proxy objective, maybe, that works pretty well on a training distribution, but it doesn't work very well off distribution. And the problem is that if you're just looking at the behavior on the training distribution, you can't really distinguish between a model which will be objective robust, which has learned a proxy which will work well off distribution, and one which has learned a proxy which won't work well off distribution.

**Evan Hubinger:**
Because if you're just looking at the training distribution and you're just looking at its behavior on the training distribution, you can't tell because those might just look identical on the training distribution. As a really concrete example, this maze example that I often like to give, so you have a situation where I'm training a model on ... the training distribution has a bunch of mazes. And I want it to get to the end of the maze, and I put a little green arrow at the end of the maze and I incentivize the model for getting to the end, and I train it on a bunch of these relatively small mazes.

**Evan Hubinger:**
And then I deploy the model in a new situation where I have larger mazes and I put the green arrow at some random place in the middle. And now there's a bunch of things that can happen. So it could be the case that in this new deployment environment, the model is capability robust and it really knows how to solve mazes. But it can also not be capability robust and it just can't solve the bigger mazes. But if it is capability robust, then there's two options. It's either objective robust and it learned the correct proxy which was getting to the end, and so that makes it navigate to the end of the maze.

**Evan Hubinger:**
Or it could be objective non-robust. It learned an incorrect proxy which was, "Get to the green arrow." So it navigates to the green arrow. And if you're only looking at the behavior on the training distribution, you can't distinguish between the model which is trying to get to the green arrow and the model which is trying to get to the end of the maze, because in the training distribution, those are not identifiable. You can't distinguish between them by looking at behavior.

**Evan Hubinger:**
And so we want to ask the question, "How likely is it that we will end up with models which generalize in one way versus models which generalize in another way?" We have to look at the other thing than just the behavioral incentives, which is causing models to fall into one category or the other, and that means the inductive biases. The reason that your training process would produce a model which is going to generalize in one way or generalize in another way despite having the same training performance is all going to come down to, "What is incentivized by the inductive biases of that training process?"

**Evan Hubinger:**
And to understand what's incentivized by the inductive biases of the training process, we have to look at the model's internals, because the inductive biases aren't just about the ... what's happening here at this point, the inductive biases, all of the behavior in training distribution is the same. So the only difference is, "What are the inductive biases incentivizing in terms of the internals of the model?" Are the inductive bias going to incentivize the sorts of models that have particular properties or models that have other properties?

**Evan Hubinger:**
And fundamentally, that's where you have to look if you want to start answering the question of what you're going to get in a standard machine learning setting because that's the thing which is going to determine what option you end up with when you're in this unidentifiable position.

**Daniel Filan:**
Yeah. So it sounds like one thing that you notice reading this paper is that it seems very focused on the case of distributional shift, right? You're interested in generalization, but not in the generalization bound sense of from the training distribution to the training distribution but a sample you haven't literally seen, but some kind of different world. I'm like, why can't we just train on the distribution that we're going to deploy on? Why is this a problem?

**Evan Hubinger:**
Yeah, so that's a really good question. So I think there's a couple of things around here. So, first one thing that's worth noting is that I think a lot of times I think people read the paper and will be like, "Well, what is going on with this deployment distribution?" And sometimes we just do online learning. Sometimes we just do situations where we're just going to keep training, we're just going to keep going. Why can't we understand that?

**Evan Hubinger:**
I think basically my argument is that, those situations are not so dissimilar as you might think. So let's for example, consider online learning. I'm going to try to just keep training it on every data point as it comes in and keep pushing in different directions. Well, let's consider the situation where you are doing online learning and the model encounters some new data point. We still want to ask the question, "For this new data point, will it generalize properly or will it not?"

**Evan Hubinger:**
And we can answer the question, "Will it generalize properly or will it not on this new data point?" by looking at all of the previous training that we've done, was a training environment of sorts and this new data point is a deployment environment of sorts where something potentially has changed from the previous data points to this new data point. And we want to ask the question, "Will it generalize properly to this new data point?"

**Evan Hubinger:**
Now, fundamentally, that generalization problem is not changed by the fact that after it produces some action on this data point, we'll go back and we'll do some additional training on that. There's still a question of how is it going to generalize to this new data point. And so you can think of online learning setups and situations where we're trying to just keep training on all the data as basically, all they're doing is they're adding additional feedback mechanisms. They're making it so that if the model does do something poorly, maybe we have the ability to have the training process intervene and correct that.

**Evan Hubinger:**
But fundamentally, that's always the case. Even if you're doing a deployment where you're not doing online learning, if you notice that the model is doing something bad, there are still feedback mechanisms. You can try to shut it down. You can try to look at what is happening and retrain a new model. There's always feedback mechanisms that exist. And if we take a higher level step and we think about what is the real problem that we're trying to solve, the real problem we're trying to solve is situations in which machine learning models take actions that are so bad that they're unrecoverable. That our natural feedback mechanisms that we've established don't have the ability to account for and correct for the mistake that has been made.

**Evan Hubinger:**
And so an example of a situation like this ... Paul Christiano talks a lot about this sort of unrecoverability. One example would be, Paul has this post, ["What Failure Looks Like."](https://www.alignmentforum.org/posts/HBxe6wdjxK239zajf/what-failure-looks-like) And in the, "What Failure Looks Like" post, Paul goes into this situation where you could imagine, for example, a cascade of problems where one model goes off the rails because it has some proxy and it starts optimizing for the wrong thing, and this causes other models to go off the rails, and you get a cascade of problems that creates an unrecoverable situation where you end up with a society where we put models in charge of all these different things, and then as one goes off the rails, it pushes others off distribution.

**Evan Hubinger:**
Because as soon as you get one model doing something problematic, that now puts everyone else into a situation where they have to deal with that problematic thing, and that could itself be off distribution from the things we just had to deal with previously, which increases the probability of those other models failing and you get that cascade. I'm not saying this is the only ... I think this is one possible failure mode, but if you can see that it's one possible failure mode that is demonstrating the way in which the real problem is situations in which our feedback mechanisms are insufficient.

**Evan Hubinger:**
And you will always have situations where your feedback mechanisms could potentially be insufficient if it does something truly catastrophic. And so what we're trying to prevent is it doing something so catastrophic in the first place. And to do that, you have to pay attention to the generalization because every time it looks at a new data point, you're always relying on generalization behavior to do the correct thing on that new data point.

**Daniel Filan:**
So when you say generalization behavior, you mean it's going to encounter a situation and ... I guess I'm not sure why just normal ML isn't enough. Generalization bounds exist. Why can't we just train on a distribution, deploy, you get different samples from that distribution, so it's a little bit different. But generalization bounds say that it's just not going to be so bad. Why isn't that the end of the story?

**Evan Hubinger:**
Yeah. So I think that a lot of these generalization bounds rely on a particular assumption which I think is quite questionable, which is the i.i.d. [independent and identically distributed] assumption. The assumption that in fact the deployment distribution and the training distribution, that we can actually take i.i.d. samples that we can create, I don't know, particular bounds between the divergence between the distributions or things like this.

**Evan Hubinger:**
In practice, all of these sorts of assumptions basically just don't hold. And there's a lot of obvious ways in which they don't hold. There's always things that are changing about our environment. There's always various different things. You can imagine, just the fact that the date advances, the fact that various things are happening in the world that you might not be able to anticipate. And obviously there are ways to try to address these problems in the standard practice of machine learning. So a very standard thing to do is adversarial training.

**Evan Hubinger:**
So you might have a situation where you're like, "Well, we expect that the distribution is going to vary in a bunch of different ways. We can try to sample some of the ways in which we expect the distribution might vary and train on those things." But there can still be a lot of problems that happen even in that situation. So one of the things that we talk about in the paper that I think becomes particularly problematic when you start getting into these situations where you've really done a good job at doing adversarial training and tried to train away a lot of the naive simple ways in which you might end up with a "green arrow" optimizer versus an "end of the maze optimizer", is something that we call deceptive alignment.

**Evan Hubinger:**
And deceptive alignment is a particularly pernicious case of a mesa-optimizer which has a mesa-objective that is different from the ideal loss function. And the particular case of deceptive alignment is a situation in which we have a model, and that model, let's say, has some proxy objective. It's trying to get to the green arrow or whatever, but it realizes that it's inside of a training process, and that that training process is going to select it for getting to the end of the maze rather than getting to the green arrow.

**Evan Hubinger:**
And so it realizes that the correct instrumental strategy to be able to get the green arrow in the future is to currently try to get to the end of the maze so that it will stay around in the training process and won't be modified to go to the green arrow instead. And then eventually at some point maybe it gets the ability to take some action, maybe you just deploy it. And now when it's in deployment, it can go to the green arrow a bunch of times and you won't correct for it. Or it otherwise gets into some situation where the correction mechanisms, the feedback mechanisms that you have are no longer sufficient to prevent it from being able to do the true proxy objective that it learned.

**Evan Hubinger:**
And this is particularly concerning because if you're in a deceptive alignment situation, the adversarial training set up no longer really works, because if you think about a deceptive aligned system, it's going to be actively looking for any possible differences between the training distribution and the deployment distribution to let it know that now it's in a situation where it can do what it wants and it doesn't have to be as afraid of corrective feedback. And so there's a lot of examples of the sorts of things that a deceptive model might be looking for.

**Evan Hubinger:**
Paul Christiano has this one example that I think is fun, and it's not necessarily realistic, but it just goes to show the way in which this can happen, which is this RSA-2048 example. So the RSA-2048 example goes like this. So RSA-2048 is this really large pseudoprime. There is this challenge to try to factor RSA-2048.

**Daniel Filan:**
Sorry, what's a pseudoprime?

**Evan Hubinger:**
It's like a product of two large primes.

**Daniel Filan:**
Okay.

**Evan Hubinger:**
I think, yeah. I might be using the terminology wrong. I'm pretty sure, like whatever. It's the product of two very large primes, but finding that factorization, finding the two very large primes that make up this very large number is very challenging and computationally difficult. And currently, it's beyond the computational abilities of modern computers and there are, I don't know ... at least there's a challenge to try to factor it and nobody has yet been able to do so. But it's the sort thing that probably we'll have enough compute that somebody will eventually get around to being able to factor it in the future.

**Evan Hubinger:**
And so you can imagine the situation where you have a deceptive model and it's like, "Well, I don't know when I'm going to be deployed. But at some point probably I'll be in a deployment situation where I have lots of power and I can just do whatever I want. And so I'll choose to look for a factorization of RSA-2048. And if I see such a factorization, then I can be pretty sure that I'm no longer in training because the people training me don't have the compute available to factor RSA-2048. But probably at some point somebody is going to factor it. And so I can be pretty sure that if I see a factorization of RSA-2048, that I am longer in training and I can be pretty sure that eventually I will see such a factorization."

**Evan Hubinger:**
And so pointing to this general problem of when you try to do adversarial training, you're putting yourself in this situation where you have to be able to ... it's sort of this P versus NP thing where you're putting yourself in a situation where you have to be able to actually generate all of the possible counter examples, whereas the model is in a much easier position. It only has to be able to do the check. It only has to be able to check whether it is in such a situation. And there's just a lot of things where it's easier to check than it is to generate. The factorization is an example of that. It's a lot easier to check that you have a factorization than it is to generate such a factorization.

**Evan Hubinger:**
And this is a general thing like I was saying, that comes from the fact that there are different complexity classes that vary based on ... we can think about NP as being the complexity class of things which are relatively easily checkable and P as being the complexity class of things which are relatively easy to actually generate, and we think that these are distinct and so there's likely lots of problems like this.

**Evan Hubinger:**
Even if we're not thinking about things as rigorous mathematically as RSA-2048, there's just a lot of things about the world that are easier to check than they are to generate. And so that means that if you have a model which is actively looking for differences, your procedure of, "Let's just try to generate a bunch of differences." is no longer likely to work.

**Daniel Filan:**
Yeah. I think this is ... so there's a lot in that answer that I can get back to. One thing that it does remind me of is often people do talk about adversarial training and it always strikes me that, it seems like if you want to really make that work, you need much better interpretability tools than we have at the moment, precisely to check this kind of thing, like what is the model checking? To really find the worst case scenario, you need to know a lot about the model, right? So I guess I'm wondering, do you think that's right or do you have any thoughts about that?

**Evan Hubinger:**
I think that's exactly right. So I have a post that talks about [relaxed adversarial training](https://www.alignmentforum.org/posts/9Dy5YRaoCxH9zuJqa/relaxed-adversarial-training-for-inner-alignment) which is terminology that comes from Paul Christiano. And basically the idea there is, well, how can we improve this adversarial training setup? Well, instead of doing adversarial training on the model's inputs, we can try to do it on the model's internals. We can try to generate descriptions of situations in which the model might fail, try to generate possible activations for which the model might fail. And these problems might be significantly easier than actually trying to generate an input on which the model fails.

**Daniel Filan:**
Cool. So there's a lot to get back to. I want to get back to basics a little bit, just about mesa-optimizers. So one of the examples you use in the post is, you can think at least for the sake of example, you can think of evolution as some optimizer and evolution has produced humans and humans are themselves optimizers for such and such thing. And I wanted to get a little bit more detail about that. So I'm a human and a lot of the time I'm almost on autopilot. For instance, I'll be acting on some instinct, right? Like a loud noise happens and I jump. Sometimes I'll be doing what I've previously decided to do, or maybe what somebody has told me to do.

**Daniel Filan:**
So for instance, we just arranged this interview and I know I'm supposed to be asking you questions and because we already penciled that into the calendar, that's what I'm doing. And often what I'm doing is very local means-ends reasoning. So I'll be like, "Oh, I would like some food. I'm hungry, so I would like some food now. So I'm going to cook some food." That kind of thing.

**Daniel Filan:**
Which is different from what you might imagine of like, "Oh, I'm going to think of all the possible sequences of actions I could take and I'm going to search for which one maximizes how rich I end up in the future" or some altruistic objective maybe, if you want to think nicely of me, or something. So am I a mesa-optimizer? And if I am, what's my mesa-objective?

**Evan Hubinger:**
Yeah, so that's a really good question. So I'm happy to answer this. I will just warn, I think that my personal opinion is that it's easy to go astray I think, thinking too much about the evolution and humans example and what's happening with humans and forget that humans are not machine learning models, and there are real differences there. But I will try to answer the question. So if you think about it from a computational perspective, it's just not very efficient to try to spend a bunch of compute for every single action that you're going to choose to try to compare it to all of these different possible opportunities.

**Evan Hubinger:**
But that doesn't mean you're not doing search. It doesn't mean you're not doing planning, right? So you can think about, one way to model what you're doing might be, well, maybe you have a bunch of heuristics, but you change those heuristics over time, and the way in which you change those heuristics is by doing careful search and planning or something.

**Evan Hubinger:**
I don't know exactly what the correct way is to describe the human brain and what algorithm it internally runs and how it operates. But I think it's a good guess to say and pretty reasonable to say that it is clearly doing search and it is clearly doing planning. It is clearly at least sometimes, and like you were saying, it would be computationally intractable to try to do some massive search every time you think about you are going to move a muscle or whatever. But that doesn't mean that you can't, for particular actions, for particular strategies, develop long-term strategies, think about possibilities for ... consider many different strategies and select them according to whether they satisfy whatever you are trying to achieve.

**Evan Hubinger:**
And that obviously also leads to the second question which is, well what is mesa-objective? What is it that you might be trying to achieve? This is obviously very difficult to answer. People have been trying to answer the question, "What are human values?" for forever essentially. I don't think I have the answer and I don't expect us to get the answer to this sort of thing. But I nevertheless think that there is a meaningful sense in which you can say that humans clearly have goals, and not just in a behavioral sense, in a mechanistic sense.

**Evan Hubinger:**
I think there's a sense in which you can be like, "Look, humans select strategies. They look at different possible things which they could do, and they choose those strategies which perform well, but which have good properties according to the sorts of things they are looking for. They try to choose strategies which put them in a good position and try to do strategies which make the world a better place." They try to do strategies which ... people have lots of things that they use to select the different sorts of strategies that they pursue, the different sorts of heuristics they use, the different sorts of plans and policies that they adapt.

**Evan Hubinger:**
And so I think it is meaningful to say and correct to say that humans are mesa-optimizers of a form. Are they a central example? I don't know. I think that they are relatively non-central. I think that they're not the example I would want to have first and foremost of mesa-optimization. But they are a meaningful example because they are the only example that we have of a training process of evolution being able to produce a highly intelligent, capable system. So they're useful to think about certainly, though we should also expect there to be differences.

**Daniel Filan:**
Yeah, that makes sense.

**Evan Hubinger:**
But I think [crosstalk 00:29:28].

**Daniel Filan:**
Yeah, that makes sense. Yeah, I guess I'm just asking to get a sense of what a mesa-optimizer really is. And your answer is interesting because it suggests that if I do my start of year plan or whatever, and do search through some strategies and then execute, even when I'm in execution mode, I'm still an optimizer. I'm still a mesa-optimizer. Which is interesting because it suggests that if one of my plans is like, "Okay, I'm going to play go." And then I open up a game, and then I start doing planning about what moves are going to win, am I a mesa-mesa-optimizer when I'm doing that?

**Evan Hubinger:**
So my guess would be not really. You're still probably using exactly the same optimization machinery to do go, as you are to plan about whether you're going to go do go. And so in some sense, it's not that you did ... So for a mesa-mesa-optimizer, it would have to be a situation in which you internally did an additional search over possible optimization strategies and possible models to do optimization and then selected one.

**Daniel Filan:**
OK.

**Evan Hubinger:**
If you're reusing the exact same optimization machinery, you're just one optimizer and you are optimizing for different things at different points.

**Daniel Filan:**
Okay. That gives me a bit of a better sense of what you mean by the concept. So I have a few more questions about this. So one thing you do, I believe in the introduction of the paper is you distinguish between mesa-objectives and behavioral objectives. I think the distinction is supposed to be that my mesa-objective is the objective that's internally represented in my brain, and my behavioral objective is the objective you could recover by asking yourself, "Okay, looking at my behavior, what am I optimizing for?" In what situations would these things be different?

**Evan Hubinger:**
Yeah, so that's a great question. So I think that in most situations where you have a meaningful mesa-objective, we would hope that your definitions are such that the behavioral objective would basically be that mesa-objective. But there certainly exist models that don't have a meaningful mesa-objective. And so the behavioral objective is therefore in some sense a broader term. Because you can look at a model which isn't internally performing search and you can still potentially ascribe to it some sort of objective that it looks like it is following.

**Daniel Filan:**
What's an example of such a model?

**Evan Hubinger:**
You could imagine something like, maybe you're looking at ... I think for example a lot of current reinforcement learning models might potentially fall into this category. If you think about, I don't know ... It's difficult to speculate on exactly what the internals of things are, but my guess would be, if we think about something like [OpenAI Five](https://openai.com/blog/openai-five/) for example. It certainly looks like this-

**Daniel Filan:**
Can you remind listeners what OpenAI Five is?

**Evan Hubinger:**
Sure. OpenAI Five was a project by OpenAI to solve Dota 2 where they built agents that are able to compete on a relatively professional level at Dota 2. And I think if you think about some of these agents, the OpenAI Five agents, I think that my guess would be that the OpenAI Five agents don't have very coherent internally representative objectives. They probably have a lot of heuristics.

**Evan Hubinger:**
Potentially, there is some sort of search going on, but my guess would be not very much, such that you wouldn't really be able to ascribe a very coherent mesa-objective. But nevertheless, we could look at it and be like, "There's clearly a coherent behavioral objective in terms of trying to win the Dota game." And we can understand why that behavioral objective exists. It exists because, if we think about OpenAI Five as being a grab bag of heuristics and all of these various different things, those heuristics were selected by the base optimizer, which is the gradient descent process, to achieve that behavioral goal.

**Evan Hubinger:**
And one of the things that we're trying to hammer home in Risks from Learned Optimization is that there's a meaningful distinction between that base objective, that loss function that the training process is optimizing for, and if your model actually itself learns an objective, what that objective will be.

**Evan Hubinger:**
And it's easy because of the fact that in many situations, the behavioral objective looks so close to the loss function to conflate the ideas and assume that, "Well, we've trained OpenAI Five to win Dota games. It looks like it's trying to win Dota games. So it must actually be trying to win Dota games." But that last step is potentially fraught with errors. And so that's one of those points that we're trying to make in the paper.

**Daniel Filan:**
Great. Yeah, perhaps a more concrete example. Would a thermostat be an example of a system that has a behavioral objective but not necessarily a mesa-objective or an internally represented objective?

**Evan Hubinger:**
Yeah. I think the thermostat is a little bit of an edge case because it's like, "Well, does the thermostat even really have a behavioral objective?" I think it kind of does. I mean, it's trying to stabilize temperature at some level, but it's so simple that I don't know. What is your definition of behavioral objective? I don't know. Probably yeah, thermostat is a good example.

**Daniel Filan:**
Cool. So I wanted to ask another question about this interplay between this idea of planning and internally represented objective. So this is, I guess this is a little bit of a troll question, so I'm sorry. But suppose I have my program, I have this variable called my_utility, and I'm writing a program to navigate a maze quickly. That's what I'm doing. In the program, there's a variable called my_utility and it's assigned to the longest path function. And I also have a variable called my_maximizer that I'm going to try to maximize utility with, but I'm actually a really bad programmer. So what I assigned to my maximizer was argmin function. So the function that takes another function and minimizes that function. So that's my maximizer. It's not a great maximizer.

**Daniel Filan:**
And then the act function is my_maximizer applied to my_utility. So here, the way this program is structured is if you read the variable names, is there's this objective, which is to get the longest path and the way you maximize that objective is really bad. And actually, you use the minimization function. So this is a mesa-optimizer, because I did some optimization to come up with this program. According to you, what's the mesa-objective of this?

**Evan Hubinger:**
Yeah. So I think that this goes to show something really important, which is that if you're trying to look at a trained model and understand what that model's objective is, you have to have transparency tools or some means of being able to look inside the model that gives you a real understanding of what it's doing, so that you can ascribe a meaningful objective. You can imagine a situation - So, I think clearly the answer is, well, the mesa-objective should be the negative of the thing which you actually specified, because you had some variable name which said maximize, but in fact you're minimizing. And so, the real objective, if we were trying to extract the objective, which was meaningful to us, would be the negative of that.

**Daniel Filan:**
Just so the listeners don't have to keep that all in their heads. The thing this does, is find us the shortest path, even though my_utility is assigned to longest path, but you were saying.

**Evan Hubinger:**
Right. And so, I think what that goes to show though, is it goes to show that if you had a level of interpretability tools, for example, that enabled you to see the variable names, but not the implementation, you might be significantly lead astray in terms of what you expected the mesa-objective be of this model. And so it does, it goes to show that there's difficulty in really properly extracting what the model is doing, because the model isn't necessarily built to be easy to extract that information. It might have weird red herrings or things that are implemented in a strange way that we might not expect. And so, if you're in the business of trying to figure out what a model's mesa-objective is by looking at it, you have to run into these sorts of problems where things might not be implemented in the way you expect. You might have things that you expect to be a maximizer, but it's a minimizer or something. And so, you have to be able to deal with these problems, if you're trying to look at models and figure out what mesa-objectives are.

**Daniel Filan:**
So I guess my, the question that I really want to get at here is this is obviously a toy example, but typically optimizers, at least optimizers that I write, they're not necessarily perfect. They don't find the thing that - I'm trying to maximize some function when I'm writing it. But the thing doesn't actually maximize the function. So I guess, part of what I'm getting at is to what extent, if there's going to be any difference between your mesa-objective and your behavioral objective, to what extent, it seems like either you've got to pick the variable name that has the word utility in it, or you've got to say, well, the thing that actually got optimized was the objective, or you might hope that there's a third way, but I don't know what it is. So do you think there's a third way? And if so, what is it?

**Evan Hubinger:**
Yeah. So, I guess the way in which I would describe this is that fundamentally, we're just trying to get good mechanistic descriptions. And when we say 'mesa-optimizer', I mean 'mesa-optimizer' to refer to the sort of model which has something we would look at as a relatively coherent objective like an optimization process. Obviously not all models have this. So there's lots of models, which don't really look like they have a coherent optimization process and objective. But we can still try to get a nice mechanistic description of what that model would be doing. When we talk about mesa-optimizers, we're referring to models, which have this nice mechanistic description which look like they have an objective and they have a search process, which is trying to select for that objective. I think that the way in which we are getting around these sorts of unidentifiability problems, I guess, would be well, we don't claim that this works for all models, lots of models just don't fall into the category of mesa-optimizers.

**Evan Hubinger:**
And then also, we're not trying to come to a conclusion about what a model's objective might be only by looking at its behavior, we're just asking if the model is structured such that it has a relatively coherent optimization process and objective, then what is that objective? And it's worth noting that this obviously doesn't cover the full scope of possible models. And it doesn't necessarily cover the full scope of possible models that we are concerned about. But I do think it covers a large variety of models and we make a lot of arguments in the paper for why you should expect this sort of optimization to occur, because it generalizes well, it performs well on a lot of simplicity measures. It has the ability to compress really complex policies into relatively simple search procedures. And so, there's reasons that you might expect that we go into in the paper, this sort of model to be the sort of thing that you might find. But it's certainly not the case that it's always going to be the sort of thing you find.

**Daniel Filan:**
Okay. That makes sense. So I guess, the final question I want to ask to get a sense of what's going on with mesa-optimization, how often are neural networks that we train mesa-optimizers?

**Evan Hubinger:**
Yeah. So that's a really good question. I have no idea. I mean, I think this is the thing that is an open research problem. My guess, would be that most of the systems that we train currently don't have this sort of internal structure. There might be some systems which do, we talk a little bit in the Risks from Learned Optimization paper about some of the sorts of related work that might have these sorts of properties. So one of the examples that we talk about is the [RL squared](https://arxiv.org/abs/1611.02779) paper from OpenAI where in that paper, they demonstrate that you could create a reinforcement learning system, which learns how to do reinforcement learning by passing it through many different reinforcement learning environments and asking it to, along the way, figure out what's going on each time, rather than explicitly training on that specific reinforcement learning environment.

**Evan Hubinger:**
This sort of thing seems closest to the sort of thing that we're pointing at in terms of existing research, but of course the problem is that our definition is one which is mechanistic and requires an understanding of "is it actually doing a search", which is not really something we know, because we don't have the ability to really look inside of our models and know whether they're doing search or not. So is it happening with current models? I don't know. I think that we certainly see some of the sorts of failures in terms of, sometimes we see models which try to pursue proxies to the objectives which they're trained under, but those proxies aren't necessarily the sorts of ones which are very objective robust. They might not be a really good coherent search process, which generalizes well, and is able to pursue that proxy intelligently and coherently, even off-distribution, which fundamentally is what we care about.

**Evan Hubinger:**
Because if you think about the 2D robustness picture, if you have a situation where a model is very capability robust, it's really good at doing optimization, it's able to take really coherent capable actions, but it's doing so for a certain non-robust objective, an objective that is not the one which we actually wanted to achieve. You end up in a bad situation, because you have a very capable model, which is doing the wrong thing. We certainly see some amount of objective non-robustness, but in some sense, we also see that along with capability non-robustness. A lot of cases, when you look at models off-distribution, they just are bad, they don't do really coherent things. And so, we're in a situation where these notions haven't yet come apart completely, because we haven't yet gotten good enough at capability robustness. Good enough at producing models that are capable of doing really coherent optimization on- and off-distribution to be able to get to a situation where they have potentially an objective, which might be meaningful.

**Daniel Filan:**
Okay, cool. So, I now want to move on a little bit to how we might produce mesa-optimizers. So, one reason you give for why or when mesa-optimization might happen is when there's a lot of computation available at inference time or runtime relative to how much computation is devoted to training the system. And I guess, this is because the optimization has to happen at some point. And if it doesn't, if it can happen at inference time, when the model's running, then you don't do it so much when you're training. So I guess, I'm wondering, do you expect this situation of a lot of computation being used at inference time relative to during training and if so, why? And also how big a deal is this do you think?

**Evan Hubinger:**
Yeah, so I guess, I'm not sure I 100% agree with that description. So I think that the thing which I would just say, is that the more compute your model has available to it at inference time, the more likely it is to be a mesa-optimizer. I think the additional question of how the compute available at inference time versus the compute available at training time compare to each other, the relative differential, I think is more complicated. So for example, I think that in fact, the more compute you have available at training time, the better you are at selecting for models which perform very well according to your inductive biases. So this is a little bit complicated. So one story for why I believe this. So when you think about something like double descent. So double descent is a phenomenon, which has been demonstrated to exist in machine learning systems where what happens is you start training your machine learning model and initially, you get this standard learning theory curve where the training error goes down and the test error goes down and then goes back up as you start overfitting.

**Evan Hubinger:**
And then, the insight of double descent is, well, if you keep training past zero training error, your test error actually starts to go back down again and can potentially get lower than it ever was. And the reason for this is that if you keep training, if you put enough compute into just training past the point of convergence, you end up in a situation where you're selecting for the model which performs best according to the inductive biases of your machine learning system, which fits the data. Whereas if you only train to the point where it gets zero training error, then stop immediately, there's a sense in which there's only potentially maybe one model. It's just the first model that it was able to find, which fit to the training data. Whereas if you keep training, it's selecting from many different possible models, which might fit the training data and finding the simple one. And one of the arguments that we make for why you might expect mesa-optimization is that mesa-optimizers are relatively simple models.

**Evan Hubinger:**
Search is a pretty simple procedure, which is not that complicated to implement, but has the ability to in fact, do tons of different complex behavior at inference time. And so, search can be thought of as a form of compression. It's a way of taking these really complex policies that depend on lots of different things happening in the environment and compressing them into this very simple search policy. And so, if you were in a situation where you're putting a ton of compute into training your model, then you might expect that according to something like this double descent story, that what's happening is the more compute you put, the more your training process is able to select the simplest model, which fits the data as opposed to just the first model that it finds that fits the data. And the simplest model that fits the data, we argue is more likely to be a mesa-optimizer.

**Evan Hubinger:**
And so, in that sense, I would actually argue that the more training compute you do, the more likely you are to get mesa-optimizers and the more inference time to compute you have, the more likely you get mesa-optimizers. And the reason for the inference time compute is similar to the reason you were saying, which is that inference time compute enables you to do more search, to make search more powerful. Search does require compute. You have to do this- it takes a certain amount of compute to be able to do the search, but it has the benefit of this compression of "Yeah, you have to do a bunch of compute for performing the search. But as a result, you get the ability to have a much more complex policy, that's able to be really adapted to all these different situations." And so, in that sense, the more compute on both ends, I would expect to lead to more likelihood of mesa-optimization.

**Evan Hubinger:**
It is worth noting, there's some caveats here. So in practice, we generally don't train that far past convergence. I think that there's also still a lot of evidence that especially if you're doing things where for example, you do very early stopping where you only do a very small number of gradient descent steps after initialization. There's a sense in which that also is very strong on inductive biases, because you're so close to initialization the inductive biases of, well, the things which are relatively easy and simple to initialize, which take up a large volume in the initialization space, those are the sorts of things that should be very likely to find. And so, you still end up with very strong inductive biases, but actually it's different inductive biases. There's not a lot of great research here, but I think it's still the case that when you're doing a lot of training compute on very large models, if you're doing really early stopping or training a bunch past convergence, you're ending up in situations where the inductive biases matter a lot.

**Evan Hubinger:**
And you're selecting for relatively simple models, which I think leads to something for search and something for optimization and something for mesa-optimizers.

**Daniel Filan:**
Okay. So that makes sense. So yeah, this gets me to another question. One of the arguments that you make in the paper is this idea that well, mesa-optimizers are just more simple, they're more compressible, they're easier to describe and therefore, they're going to be favored by inductive biases or by things that prefer simplicity, but this presumably depends on the language that you're using to describe things. So I'm wondering in what languages do you think it's the case that optimizers are simple and in particular I'm interested in the quote unquote languages of the L2 norm of your neural network and the quote unquote language again, of the size of say a multi-layer perceptron or a CNN, how many layers it has.

**Evan Hubinger:**
Yeah. So that's a really good question. So I mean, the answer is, I don't know. So I don't know the extent to which these things are in fact easy to specify in concrete machine learning settings. How hard is it to actually, if you were to try to write an optimization procedure in neural network weights, how difficult that would be to do, and how much space do those models take up in the neural network prior? I don't know. My guess is that it's not that hard and it certainly depends on the architecture, but if we start just with a very simple architecture, we think about a very simple multi-level perceptron or whatever, can you code optimization? I think you can. So you can do things where you can, I think depth is pretty important here, because it gives you the ability to work on results over time, create many different possibilities and then refine those possibilities as you're going through. But I do think it's possible to implement bare bones, search style things in a multilayer perceptron setup.

**Evan Hubinger:**
It certainly gets easier when you look at more complicated setups. If you think about something like an LSTM, recurrent neural network, you have the ability now to work on a plan over time in a similar way to a human mind, like you were describing earlier where you could make plans and execute them. And especially if you think about something like AlphaZero or even more especially something like MuZero, which explicitly has a world model that it tries to search over and do MCTS, Monte Carlo Tree Search over that to find some objective. And the objective that it's searching over in that situation is a value network that is learned, and so you're learning some objective and then you're explicitly doing search over that objective. Now, and we talk about this situation of hard-coded searching in the paper a lot.

**Evan Hubinger:**
Things get a little bit tricky, because you might also have mesa-optimization going on inside of any of the individual models and the situations under which you might end up with that are well, you might end up in a situation where the sort of search you've given it to be hard-coded is not the sort of search which it really needs to do to solve the problem. So it's a little bit complicated, but certainly I think that current architectures are capable of implementing search and being able to do search. There's then the additional question of how simple it is. I think that - so unfortunately we don't have really great measures of simplicity for neural networks. We have very simple metrics like Kolmogorov complexity, and there is some evidence to indicate that Kolmogorov complexity is at least related to the inductive biases of neural networks. So there's [some recent research](https://arxiv.org/abs/1909.11522) from one of the co-authors, Joar Skalse who's one of the co-authors on Risks from Learned Optimization trying to analyze what exactly is the prior of neural networks at initialization.

**Evan Hubinger:**
And there's some theoretical results there that indicate that Kolmogorov complexity actually can serve as a real bound there. But of course, Kolmogorov complexity is uncomputable. For people who are not aware, Kolmogorov complexity is a measure of a description length that is based on finding the simplest Turing machine, which is capable of reproducing that. And so we can look at these theoretical metrics, like Kolmogorov complexity and be like, well, it seems like search has relatively low Kolmogorov complexity, but of course the actual inductive biases of machine learning and gradient descent that are more complicated and we don't fully understand them. There is other research. So there's also some research that looks at trying to understand the inductive biases of gradient descent, it seems like gradient descent generally favors these very large basins and there's evidence these very large basins seems to match onto relatively simple functions.

**Evan Hubinger:**
There's also stuff like the lottery tickets hypothesis, which hypothesizes that if you train a very, very large neural network, it's going to have a bunch of different sub-networks, some of those sub-networks at initialization, will happen to encode - already be relatively close to the goal that you're going to get to. And then the gradient descent process will be amplifying some sub-networks and de-amplifying other sub-networks. And if you buy into a hypothesis like this, then you're in a situation where, well, there's a question of how likely are sub-networks implementing something like optimization likely to be at initialization. Obviously, the larger your neural network is the more possible sub-networks at initialization there are that exist in a combinatorial explosion way, such that as we get to much larger and larger networks, the combinatorial explosion of sub-networks seems like it would eventually cover even things potentially quite complicated. But unfortunately, there's just not that much I can say, because our understanding of the inductive biases of neural networks just isn't great.

**Evan Hubinger:**
We have some things which we can talk about, about simplicity measures and stuff, but unfortunately at the end of the day, I can be like, well, it certainly seems like search will be relatively simple and likely to learn. But I don't have really strong results for that because we just don't have a great understanding of exactly how these inductive biases operate.

**Daniel Filan:**
It does sound like one thing you think though is that, you mentioned, if you want to do search in something like a multi-layered perceptron, you mentioned that it helps to just have a bunch of layers, to refine options and such. So it kind of sounds like you think that if, if we were selecting on network depth, that would be a kind of simplicity that would disfavor search rather than favoring it. Does that sound right to you?

**Evan Hubinger:**
That does sound right. So the way in which I would describe that is in some ways looking at inference time compute, it's thinking about more like a speed prior. So if you think about a speed prior or something like... Yeah, so speed prior is trying to select on models, which have a small time from initialization to completion. So the actual amount of time it takes to run the function. If you think about depth versus width, if you have a very wide neural network, but it's not very deep, then it's actually very fast to run, because you just run through that initial step. It might still be computationally expensive, but because you can parallelize width-wise, the actual individual step is relatively fast in a way in which it would be favored by a speed prior.

**Evan Hubinger:**
And so, one of the things that we do claim in the paper is that these sorts of speed prior-like incentives disincentivize mesa-optimization, because mesa-optimization does require - it requires you to be able to have these multi-level relatively long inference time computations. And if you restrict your model to not be able to do these relatively long inference time compensations, then it's probably going to be less likely to be able to do search, to be able to do mesa-optimization. So certainly in that sense, I think that there is a distinction there between a simplicity prior and a minimal circuit speed prior thing, where with the speed prior you get different incentives. And there's a distinction where if you really are selecting on depth, you're doing something more like a speed prior, which changes things.

**Daniel Filan:**
What do you think the important difference here is between time and space? Because it sounds like you're saying, well, if you have a lot of time to compute, then you can be an optimizer, but if you just have a lot of space, then you can't be an optimizer. Why is there this asymmetry? Doesn't special relativity tell us that these things are the same?

**Evan Hubinger:**
Well, okay. So here's a really simple analogy to think about this is, let's say I have a lookup table as my model versus an optimizer. The lookup table that is equivalent to the optimizer behaviorally is going to be really large, because it has to explicitly encode all of these different things that's going to do, but it's also going to be really fast, because you just look at the lookup table, you figure out what it's going to do, and then you do that action.

**Daniel Filan:**
Hang on. Why is it fast? Doesn't it take exponentially long to figure out where... You go through the lookup table and there's a billion entries there to go really [crosstalk 00:56:49].

**Evan Hubinger:**
Hash map has O(1) look up something, something, something.

**Daniel Filan:**
I mean, not worst case, right? Eventually you get collisions.

**Evan Hubinger:**
Well, amortized expected O(1).

**Daniel Filan:**
All right.

**Evan Hubinger:**
I think this is actually, in fact, the case. There is analysis of looking at minimal circuits and speed priors and stuff, trying to understand what would happen in these sorts of situations. And in fact, I think lookup tables are really good because they're in fact quite fast. If you have all of the information already encoded, yes, you need to have some, you need to be able to generate some key, be able to look it up. But, I don't know, there are mechanisms that are able to do that relatively quickly, a hash map being one of them. Even just looking it up in a binary search tree or something, it's still not going to be that bad potentially. And so, if you have a lookup table, it can be relatively fast, but it takes up a lot of space. Whereas if you have an optimization system, I mean, it has to be relatively slow, because it has to actually come up with the possibility of all of the different possible options and then select one of them.

**Evan Hubinger:**
And that process is going to take some - it takes compute, it takes time. It has to actually look through different options and select them. And you can have heuristics to try to narrow down the field of possible things, which you're going to look for, but you still need to look, you're still probably going to need to look at a bunch of different things to be able to select the correct one. And so, we can think about in this situation, the optimizer, it has a larger inference time compute, but has a smaller description, because you don't have to explicitly write down in the description of this optimizer, all of the different actions that it's going to take for all of these different things.

**Evan Hubinger:**
It just needs to have a general procedure of, well, let's look at an objective, let's do some search to try to select something. Whereas when you think about the lookup table, it has to explicitly write all of these things down, then an inference time, it gets benefits. And this sort of thing, this sort of trade-off between space and time occurs a lot in computational complexity setups. A lot of times you think about, you'll take a function and then you'll memoize it, which is this procedure of well, I have this function, I call it a bunch, with different arguments. I can make it faster, if I'm willing to spend a bunch of space storing all of the different possible results that I generally call it with.

**Evan Hubinger:**
And as a result, you get a faster function, but you have to spend a bunch of space storing all these different things. And so you might expect a sort of similar thing to happen with neural networks where if you're able to spend a bunch of space, you can store all of these different things. And it doesn't have to spend all this time computing the function. Whereas if you force it to compute the function every time, then it isn't able to store all these different things and it has to re-compute them.

**Daniel Filan:**
Okay. So yeah, I mean, one thing this makes me think of is the paper is talking about these risks from these learned optimizers. And optimizers are these things that are doing search, they're actually doing search at inference time and they're not these things that are behaviorally equivalent to things that do search at inference time. And I guess, we already got to a bit about why you might be worried about these mesa-optimizers and we're going to avoid getting them. And I'm wondering, is 'mesa-optimizer' the right category. If it's the case that "oh, we could instead search for things that are like... Our base optimizer, we could instead try to get these systems that they're really quick at inference and then they're definitely not going to be mesa-optimizers, because they can't optimize." But of course they definitely could be these tabular forms of mesa-optimizers, which is presumably just as big a problem. So I guess this is making me wonder, is 'mesa-optimizer' the right category?

**Evan Hubinger:**
Yes, that's a good question. So I think that if you think about the tabular mesa-optimizer, how likely would you be to actually find this tabular mesa-optimizer? I think the answer is very unlikely, because the tabular mesa-optimizer has to encode all of these really complex results of optimization explicitly and in a way, which especially when you get to really complex, really diverse environments, just takes up so much space and explicit memorization, but not just memorization. You have to be able to memorize what your out-of-distribution behavior would be according to performing some complex optimization and then have some way of explicitly encoding that. And that's, I think, quite difficult.

**Daniel Filan:**
But presumably if we're doing this selection on short computation time, well we do want something that actually succeeds at this really difficult task. So how is that any easier than - it seems like if you think there are a bunch of these potential mesa-optimizers out there and they're behaviorally equivalent, surely the lookup table for really good behavior is behaviorally equivalent to the lookup table for the things that are these dangerous mesa-optimizers.

**Evan Hubinger:**
Sure. So the key is inductive biases and generalization. So if you think about it, look, there's a reason that we don't train lookup tables and we instead train neural networks. And the reason is that, if we just train lookup tables, if you're doing tabular Q-learning, for example, tabular Q-learning doesn't have as good inductive biases. It is not able to learn the nice simple and expressive functions that we're trying to learn to actually understand what's happening. And it's also really hard to train. If you think about tabular Q-learning in a really complex setting, with very diverse environments. You're just never going to be able to even get enough data to understand what the results are going to be for all of these different possible tabular steps.

**Evan Hubinger:**
And so there's a reason that we don't do tabular Q-learning, there's a reason that we use deep learning instead. And the reason is because deep learning has these nice inductive biases. It has the ability to train on some particular set of data and have good generalization performance, because you selected a simple function that fit that data. Not just any function that fit that data, but a simple function that fit that data such that it has a good performance when you try it on other situations. Whereas if you are just doing training on other situations like tabular stuff or something, you don't have that nice generalization property, because you aren't learning these simple functions to fit the data. You're just learning some function which fits the data.

**Evan Hubinger:**
And that's what would be the direction I expect machine learning to continue to go, is to develop these nice inductive biases setups, these sorts of situations in which we have these model spaces where we have inductive biases that select for simple models in large expressive model spaces. And if you expect something like that, then you should expect a situation where we're not training these tabular models, we're training these simple functions, but the simple functions that nevertheless generalize well, because they are simple functions that fit the data. And this is also worth noting - why simplicity? Well, I think that there's a lot of research that understands - it looks like if you actually look at neural network inductive biases, it does seem like simplicity is the right measure.

**Evan Hubinger:**
A lot of the functions, which are very likely to exist at initialization as well as the functions that SGD is likely to find have the simplicity properties. In a way which is actually very closely tied to computational description length, like Kolmogorov complexity. There's, I don't know, the research I was mentioning recently from Joar and others was talking about the Levin bound, which is this particular mathematical result which directly ties to Kolmogorov complexity. So there's arguments that - and there's also a further argument, which just, well, if you think about why do things generalize at all, why would a function generalize in the world? The answer is something like Occam's razor, it's that, well, the reason that things generalize in the world that we live in is because we live in a world where the actual true descriptions of reality are relatively simple.

**Evan Hubinger:**
And so, if you look for the simplest description that fits the data, you're more likely to get something that generalizes well, because Occam's razor is telling you that's the thing which is actually more likely to be true. And so there's these really fundamental arguments for why we should expect simplicity to be the sort of measure which is likely to be used. And that we're likely to continue to be training in these expressive model spaces and then selecting on simplicity rather than doing something like, I don't know, tabular Q-learning.

**Daniel Filan:**
Okay. So I'm just going to try to summarize you, check that I understand. So it sounds like what you're saying is, look, sure, we could think about like training these - selecting models to be really fast at inference time. And in that world, maybe you get tabularized mesa-optimizers or something. But look, the actual world we're going to be in is, we're going to select on description length simplicity, and we're not going to select very hard on computation runtime. So in that world, we're just going to get mesa-optimizers and you just want to be thinking about that world, is that right?

**Evan Hubinger:**
I mean, I think that's basically right. I think I basically agree with that.

**Daniel Filan:**
Okay. Cool. All right. So I think I understand now. So I'm going to switch topics a little bit. So what types - In terms of "are we going to get these mesa-optimizers", do you have a sense of what types of learning tasks done in AI do you think are more or less prone to producing mesa-optimizers? So if we just avoided RL, would that be fine if we only did image classification? Would that be fine?

**Evan Hubinger:**
Yeah, that's a great question. So we do talk about this a little bit in the paper. I think that the RL question is a little bit tricky. So we definitely mostly focus on RL. I do think that you can get mesa-optimization in more supervised learning settings. It's definitely a little bit trickier to understand. And so, I think it's a lot easier to comprehend a mesa-optimizer in an RL setting. I think it is more likely to get mesa-optimizers in RL settings. Let's see. Maybe I'll start with the other question though, which is just about the environment. I think that that's a simpler question to understand. I think that basically, if you have really complex really general environments that require general problem solving capabilities, then you'll be incentivizing mesa-optimization, because fundamentally, if you think about mesa-optimization as a way of compressing really complex policies, if you're in a situation where you have to have a really complex policy that carefully depends on the particular environment that you end up in, where you have to look at your environment and figure out what you have to do in a specific situation to be able to solve the problem. And reason about what sorts of things would likely to succeed in this environment versus that environment, you're requiring these really complex policies that have simple descriptions if they're compressed into search. And so you're directly incentivizing search in those situations. So I think the question is relatively easy to answer in the environment case. In the case of reinforcement learning versus supervised learning, it's a little bit more complicated. So certainly in reinforcement learning, you have this setup where, well, we're directly incentivizing the model to act in this environment and over multiple steps generally.

**Evan Hubinger:**
And we're acting in the environment over multiple steps. There's a natural way in which optimization can be nice in that situation, where it has the ability potentially also to develop a plan and execute that plan, especially if it has some means of storing information over time. But I do think that the same sort of thing can happen in supervised learning. So even if you just think about a very simple supervised learning setup.

**Evan Hubinger:**
Let's imagine a language model. The language model looks at some prompt and it has to predict what the future... what the text after that would be, by a human that wrote it or whatever. Imagine a situation in which a language model like this could be just doing a bunch of heuristics, has a bunch of understanding of the various different ways in which people write. And the various different sorts of concepts that people might have that would follow this, some rules for grammar and stuff. But I think that optimization is really helpful here. So if you think about a situation where like, well, you really want to look over many different possibilities. You're going to have lots of different situations where, "well, maybe this is the possible thing which will come next." It's not always the case.

**Evan Hubinger:**
There are very few hard and fast rules in language, you'll look at a situation where there's... Sometimes after this sort of thing there'll be that sort of thing. But it might be that actually they're using some other synonym or using some other analysis of it. And the ability to well, maybe, we'll just come up with a bunch of different possibilities that might exist for the next completion and then evaluate those possibilities. I think it's still a strategy which is very helpful in this domain. And it still has the same property of compression. It still has the same property of simplicity. You're in a situation where there's a really complex policy to be able to describe exactly what the human completion of this thing would be.

**Evan Hubinger:**
And a simple way to be able to describe that policy, is a policy which is well, searching for the correct completion. If you think about what humans are actually doing, when they're producing language, they are in fact doing a search for parts of it. You're searching for the correct way, the best possible word to fill into this situation. And so if you imagine a model which is trying to do something similar to the actual human process of completing language then you're getting a model which is just potentially doing some sort of search. And so I really do think that even in these sorts of supervised learning settings, you can absolutely get mesa-optimization. I think it's trickier to think about certainly and I think it's probably less likely. But I certainly don't think it's impossible or even unlikely.

**Daniel Filan:**
Okay. So, as I guess you mentioned or at least alluded to, suppose I wanted to avoid mesa-optimizers from just being - I just don't want them. So one thing I could do, is I could avoid doing things that require modeling humans. So for instance, like natural language processing, or natural language prediction, where you're trying to predict what maybe me, Daniel Filan, would say after some prompt. And also avoid doing things that require modeling other optimizers. So really advanced programming or advanced program synthesis or analysis would be a case of this, where some programs are optimizers and you don't want your system modeling optimizers. So, you want to avoid that. So a question I have is, how much do you think you could do if you wanted to avoid mesa-optimizers and you were under these constraints of "don't do anything for which it would be useful to model other optimizers"?

**Evan Hubinger:**
Yeah. So this is the question that I have... There's some writing about that I've done in the past. So I think there are some concrete proposals of how to do AI alignment that I think, fall into this general category of trying not to do... Not produce mesa-optimizers. So I have this post, [an overview of 11 proposals for building safe advanced AI](https://arxiv.org/abs/2012.07532) which goes into a bunch of different possible ways which you might structure an AI system in a way to be safe. Some of those proposals, so one of them is STEM AI where the basic idea is, well, maybe you try to totally confine your AI to just thinking about mathematical problems, scientific problems. Maybe you want to build... If you think about something like AlphaFold, you want to just do really good protein folding, you want to do really good physics, you want to do really good chemistry, you want to do... I think we could accomplish a lot just by having very domain restricted models, that are just focusing on domains like chemistry or protein folding or whatever that are totally devoid of humans.

**Evan Hubinger:**
There aren't optimizers in this domain, there aren't humans in this domain to model. You're just thinking about these very narrow domains. There's still a lot of progress to be made, and there's a lot of things that you could do just with models along these lines. And so maybe if you just use models like that, there's... You might be able to do powerful advanced technological feats like, things like nanotechnology or whole brain emulation, if you really push it potentially. So that's one route. Another route that I talked about in the 11 proposals posts is this idea of microscope AI. Microscope AI is an idea which is due to Chris Olah. Chris has this idea of, well, maybe another thing which we could do is just train a predictive model on a bunch of data. Oftentimes that data... And I think many of the ways which Chris produces this, that might involve humans.

**Evan Hubinger:**
You might still have some human modeling. But the idea would be, well, maybe in a similar way to a language model being less likely to produce mesa-optimization than a reinforcement learning agent. We could just train in this supervised setting, a purely predictive model to try to understand a bunch of things. And then rather than trying to use this model to directly influence the world as an agent, we just look inside it with transparency tools, extract this information or knowledge that it has learned and then use that knowledge to guide human decision-making in some way. And so this is another approach which tries not to produce an agent, tries not to produce an optimizer which acts in the world, but rather just produce a system which learned some things and then we use that knowledge to do other things.

**Evan Hubinger:**
And so there are approaches like this, that try to not build mesa-optimizers. I think that these are... So both the STEM AI style approach and the microscope AI style approach I think are relatively reasonable things to be looking into and to be working on. Whether these things will actually work, it's hard to know. And there's lots of problems. With microscope AI there are problems like maybe you actually do produce a mesa-optimizer like in the language modeling example, and there's other problems as well even with STEM AI also there's problems where, what can you actually do with this? If you're just doing all of these various... These physics problems or chemistry problems, if the main use case for your AI is being a AI CEO or helping you do more AI research or something, it's going to be hard to get a model to be able to do some of those things without sort of modeling humans, for example. And so there's certainly limits, but I do think that these are valuable proposals that are worth looking into, more.

**Daniel Filan:**
Okay. Yeah. It also seems like if you really wanted to avoid modeling humans, these would be pretty significant breaks from the standard practice of AI as most people understand it, right? Or at least the standard story of "We're going to develop more and more powerful AIs and then they're just going to be able to do everything". It seems like you're going to really have to try. Is that fair to say?

**Evan Hubinger:**
Yeah. I mean, I think that there's a question of exactly how you expect the field to AI to develop. So you could imagine a situation, and I talked about this a little bit also in the post - I have a post ["Chris Olah's views on AGI safety"](https://www.alignmentforum.org/posts/X2i9dQQK3gETCyqh2/chris-olah-s-views-on-agi-safety), which goes into some of the ways in which Chris views the strategic landscape. One of the things that Chris thinks about is microscope AI and one of the ways in which he talks about, a way where we might actually end up with microscope AI being a real possibility, would be if you had a major realignment of the machine learning community towards understanding models and trying to look inside models and figure out what they were doing, as opposed to just trying to beat benchmarks and beat the state of the art with relatively black box systems.

**Evan Hubinger:**
You can think of this as a realignment towards a more scientific view, as opposed to a more engineering view. Where currently a lot of the way in which people, I think a lot of researchers have approached machine learning is as a engineering problem. It's "well, we want to achieve this goal of getting across this benchmark or whatever and we're going to try and throw our tools at it to achieve that". But you can imagine a more scientific mindset, which is "well, the goal is not to achieve some particular benchmark, to build some particular system, but rather to understand these systems and what they're doing and how they work". And from a more... If you imagine a more scientifically focused machine learning community, as opposed to a more engineering focused machine learning community, I think you could imagine a realignment that could make microscope AI seem relatively standard and normal.

**Daniel Filan:**
Okay. So I guess I have one more question about how we might imagine mesa-optimizers being produced. In the paper, you're doing some reasoning about what types of mesa-optimizers we might see. And it seems like one of the ways you're thinking about it is, there's going to be some parameters that are dedicated to the goals or the mesa-objective and there's other parameters dedicated to other things. And if you have something like first order gradient descent it's going to change the parameters and the goal thing, assuming all of the parameters in the belief or the planning or whatever parts are staying constant.

**Daniel Filan:**
And one thing I'm wondering is, if I think about these classically understood parts of an optimizer, so... Or something doing search, right? So maybe it has perception, beliefs, goals, and planning. Do you think there's necessarily going to be different parameters of your machine learned system for these different parts? Or do you think that potentially there might be parts spread across - or parameters divided to different parts. In which case you couldn't reason quite as nicely about how gradient descent is going to interact with it.

**Evan Hubinger:**
So this is the sort of stuff that gets the most speculative. So I think anything that I say here has to come with a caveat, which is, we just have no idea what we're talking about when we try to talk about these sorts of things, because we simply don't have a good enough understanding of exactly the sorts of ways in which neural networks decompose into pieces. There are ways in which people are trying to get an understanding of that. So you can think about Chris Olah's circuits approach and the Clarity team trying to understand [inaudible 01:18:01] decompose into circuits. Also, I know Daniel, you have your modularity research, where you're trying to understand the extent to which you can break neural networks down to coherent modules. I think that all of this stuff is great and gives us some insight into the extent to which these sorts of things are true.

**Evan Hubinger:**
Now, that being said, I think that the actual extent of our understanding here is still pretty minimal. And so anything I say, I think is basically just speculation, but I can try to speculate nevertheless. So I think that my guess is that you will have relatively coherent things... To be able to do search, and I think you will get search, you have to do things like come up with options and then select them. And so you have to have at least some point where you're coming up - you have to have heuristics to guide your ability to come up with options. You have to somewhere generate those options and store those options and then somehow prune them and select them down. And so those sorts of things have to exist somewhere.

**Evan Hubinger:**
Now, certainly the... We found... If you think about many of the sorts of analysis of neural networks that have been done, there exists a sort of poly-semanticity where you can have neurons, for example, that encode for many different possible concepts. And you can certainly have these sorts of things overlapping because the structure of a neural network is such that, it's not necessarily the case that things have to be totally isolated or one neuron does one particular thing or something. And so it's certainly the case that you might not have these really nice, you can point to the network and be like, "Ah, that's where it does the search. That's where it's objective is encoded."

**Evan Hubinger:**
But hopefully at least we can have, "Okay, we know how the objective is encoded, it's encoded via this combination of neurons there that do this particular selection that encode, this thing here". I think that I'm optimistic, I don't know, I expect those things to at least be possible. I'm hopeful that we will be able to get to a point as a field in our understanding of neural networks, our understanding of how they work internally, that we'll actually be able to concretely point to those things and extract them out. I think this would be really good at our ability to be able to address inner alignment and mesa-optimization. But I do expect those things to exist and to be extractable. Even if they require understanding of poly-semanticity and the way in which concepts are spread across the network in various different ways.

**Daniel Filan:**
So I'd like to change topics again a little bit and talk about inner alignment. So in the paper, correct me if I'm wrong, but I believe you described inner alignment as basically the task of making sure that the base objective is the same as the mesa-objective. And you give a taxonomy of ways that inner alignment can fail. So there's proxy alignment where the thing that you're... Or specifically that inner alignment can fail but you don't know that it's failed because if inner alignment fails and the system just does something totally different, that's like, normal machine learning will hopefully take care of that. But there are three things you mentioned, firstly, proxy alignment where there's some other thing, which is causally related to what the base optimizer wants and your system optimizes for that other thing.

**Daniel Filan:**
There's approximate alignment, which is where your base optimizer has some objective function and your system learns an approximation of that objective function, but it's not totally accurate. And thirdly, sub-optimality alignment. So your system has a different goal, but it's not get so good at optimizing for that goal. And when you're kind of bad at optimizing for that goal, it looks like it's optimizing for the thing that the base optimizer wants. So proxy alignment, approximate alignment, and sub-optimality alignment. And pseudo-alignment is the general thing, where it looks like you're inner aligned, but you're not really. So one thing I wanted to know is how much of the space of pseudo-alignment cases do you think that list of three options covers?

**Evan Hubinger:**
Yeah. So maybe it's worth... Also first, there's a lot of terminology being thrown around. So I'll just try to clarify some of these things.

**Daniel Filan:**
Alright

**Evan Hubinger:**
So the base objective, we mean basically, the loss function. So we're trying to understand, we have some machine learning system. We have some loss function, or call this the base objective or a reward function, whatever, we'll call this the base objective. We're training a model according to that base objective, but the actual mesa-objective that might learn it might be different. Because there might be many different mesa-objectives [that] perform very well on the training solution, but which have different properties off-distribution. Like going to the green arrow versus getting to the end of the maze. And we're trying to catalog the ways in which the model could be pseudo-aligned, which is, it looks like it's aligned on distribution in the same way that getting to the end of the maze and getting to the green arrow, both look like they're aligned on the training distribution, but when you move them off distribution, they no longer are aligned because one of them does one thing and one of them does the other thing.

**Evan Hubinger:**
And so this is the general category of pseudo-alignment. And so, yes, we try to categorize them and we have some possible categories like proxy alignment, proxy alignment being the main one that I think about the most and the paper talks about the most. Approximate alignment and sub-optimality alignment also being other possible examples. In terms of the extent to which I think this covers the full space. I don't know. I think it's... A lot of this is tricky because it depends on exactly how you're defining these things, understanding what counts as a mesa-optimizer, what doesn't count as a mesa-optimizer. Because you could certainly imagine situations in which a thing does something bad off distribution, but not necessarily because it learned some incorrect objective or something.

**Evan Hubinger:**
And sub-optimality alignment, even, is already touching on that because sub-optimality alignment is thinking about a situation in which the model has a defect in its optimization process that causes it to look aligned, such that if that defect were to be fixed it would no longer look aligned. And so you're already starting to touch on some of the situations which maybe the reason that it looks aligned is because of some weird interaction between the objective and the optimization process, similar to your example of the way in which it maximizes, it actually minimizes or something. I don't know. I think there are other possibilities out there than just those three. I think that we don't claim to have an exhaustive characterization.

**Evan Hubinger:**
And one of the things also that I should note is, there's one post that I made later on after writing the paper that clarifies and expands upon this analysis a little bit. Which brings up the case of, for example, sub-optimality deceptive alignment, which would be a situation in which the model should act deceptively, but it hasn't realized that it should act deceptively yet because it's not very good at thinking about what it's supposed to be doing. But then later it realizes it should act deceptively and starts acting deceptively, which is an interesting case where it's sort of sub-optimality alignment, sort of deceptive alignment. And so that's for example, another-

**Daniel Filan:**
So if it's sub-optimal and it's not acting deceptively, does that mean that you notice that it's optimizing for a thing that it shouldn't be optimizing for? And just because it does a different thing that it's supposed to do and-

**Evan Hubinger:**
The sub-optimality alignment is a little bit tricky. The point we're trying to make is, it's sub-optimal at optimizing the thing it's supposed to be optimizing for, such that it looks like it's doing the right thing. Right? So for example-

**Daniel Filan:**
And simultaneously sub-optimal at being deceptive in your sub-optimality deceptive alignment.

**Evan Hubinger:**
Well, if it... Yeah, I mean, so it's sub-optimal in the sense that it hasn't realized that it should be deceptive and if it had realized it would be deceptive, then it might start doing bad things and looking because it tries to defect against you or something, at some point. It would be no longer actually aligned. Whereas, if it never figures out that it's supposed to be deceptive then maybe it just looks totally aligned basically for most of the time.

**Daniel Filan:**
Okay.

**Evan Hubinger:**
I don't know. So there certainly are other possible cases than just those, I think that it's not exhaustive. Like I mentioned, I don't remember exactly what the title, but I think it's ["More variations on pseudo-alignment"](https://www.alignmentforum.org/posts/iydwbZhATANhjoGP7/more-variations-on-pseudo-alignment), is the one that I'm referencing, that talks about some of these other possibilities. And there are others. I don't think that's exhaustive, but it is a... I don't know. Some of the possible ways in which this can fail.

**Daniel Filan:**
Do you think it covers as much as half of the space? Those main three?

**Evan Hubinger:**
Yeah, I do. I think actually what I would say is I think proxy alignment alone covers half the space. Because I basically expect... I think proxy alignment really is the central example in a meaningful way. In that, I think it captures most of the space and the others are looking mostly at edge cases.

**Daniel Filan:**
Okay. So a question I have about proxy alignment and really about all of these inner alignment failures. It seems like your story for why they happen is essentially some kind of unidentifiability, right? You know, you're proxy misaligned because you have a vacuum, the base objective is to have that vacuum clean your floors and somehow the mesa-objective is instead to get a lot of dust inside the vacuum and that you normally get from floors. But you could imagine other ways of getting it. And the reason you get this alignment failure is that, well on distribution, whenever you had dust inside you, it was because you cleaned up the floor.

**Daniel Filan:**
So what I'm wondering is, as we train better AIs that have more capacity, they're smarter in some sense, on more rich datasets - so whenever there are imperfect probabilistic causal relationships, we find these edge cases and we can select on them. And these data sets have more features and they just have more information in them. Should that make us less worried about inner alignment failures? Because we're reducing the degree to which there's these unidentifiability problems.

**Evan Hubinger:**
So this is a great question. So I think a lot of this comes back to what we were talking about previously also about adversarial training. So I think that there's a couple of things going on here. So first, unidentifiability is really not the only way in which this can happen. So [??unidentifiability??] is required at zero training error. If we're imagining a situation where we have zero training error, it perfectly fits the training data, then yeah, in some sense if it's misaligned, it has to be because of unidentifiability. But in practice in many situations, we don't train to zero training error. We have these really complex data sets that are very difficult to fit completely. And you train far before zero training error. And in that situation, it's not just unidentifability and in fact, you can end up in a situation where the inductive biases can be stronger in some sense, than the training data. And this is... If you think about avoiding overfit.

**Evan Hubinger:**
If you train too far on the training data, sort of perfectly fit, you overfit the training data, because now you've trained a function that just perfectly goes through all of them. There's double descent, like we were talking about previously, that calls some of this into question. But if you just think about stopping exactly at zero training error, this is sort of a problem. But if you think about instead, not over-fitting and training a model which is able to fit a lot, a bunch of the data, but not all of the data. But you stop such that you still have a strong influence of your inductive biases on the actual thing that you end up with. Then you're in a situation where it's not just about unidentifiability, It's also about... You might end up with a model that is much simpler than the model that actually fits the data perfectly, but because it's much simpler it actually has better generalization performance.

**Evan Hubinger:**
But because it's much simpler and doesn't fit the data, it might also have this very simple proxy. And that simple proxy is not necessarily exactly the thing that you want. An analogy here that is from the paper but I think is useful to think about is, imagine a situation... We talked previously about this example of humans being trained by evolution. And I think this example, there's... I don't necessarily love this example, but it is, in some sense, the only one we have to go off. And so if you think about the situation, what actually happened with humans and evolution, well, evolution is trying to train the humans in some sense to maximize genetic fitness, to reproduce and to carry their DNA on down to future generations. But in fact, this is not the thing which humans actually care about. We care about all of these other things like food and mating and all of these other proxies basically, for the actual thing of getting our DNA into future generations.

**Evan Hubinger:**
And I think this makes sense. And it makes sense because, well, if you imagine a situation where an alternative human that actually cared about DNA or whatever, it would just be really hard for this human. Because first of all, this human has to do a really complex optimization procedure. They have to first figure out what DNA is and understand it. They have to have this really complex high level world model to understand DNA and to understand that they care about this DNA being passed into the future generations or something. And they have to do this really complex optimization. If I stub my toe or whatever, I know that that's bad because I get a pain response. Whereas, if a human in this alternative world stubs their toe, they have to deduce the fact that stubbing your toe is bad because it means that toes are actually useful for being able to run around or do stuff. And that's useful to be able to get food and be able to attract a mate. And therefore, because of having a stubbed toe, I'm not going to be able to do those things as effectively.

**Evan Hubinger:**
And so I should avoid stubbing my toe. But that's a really complex optimization process that requires a lot more compute, it's much more complicated, because specifying DNA in terms of your input data is really hard. And so we might expect that a similar thing will happen to neural networks, to models where, even if you're in a situation where you could theoretically learn this perfect proxy, to actually care about DNA or whatever. You'll instead learn simpler, easier to compute proxies because those simpler, easier to compute proxies are actually better, because they're easier to actually produce actions based on those proxies, you don't have to do some some complex optimization process based on the real correct thing.

**Evan Hubinger:**
They're easier to specify in terms of your input data. They're easier to compute and figure out what's going on with them. They don't require as much description length and as much explicit encoding. Because you don't have to encode this complex world model about, what is DNA and how does all of this work? You just have to code, if you get a pain signal, don't do that. And so in some sense, I think we should expect similar things to happen where, even if you're in a situation where you're training on this really rich detailed environment, where you might expect it to be the case that the only thing which will work is the thing which is actually the correct proxy. In fact, a lot of incorrect proxies can be better at achieving the thing that you want, because they're simpler, because they're faster, because they're easier. And so we might expect, even in that situation, you end up with simpler, easier proxies.

**Daniel Filan:**
Yeah. And I guess that's interesting because it's... When I'm thinking about that, it seems like there's this trade-off of... You could imagine a world where evolution or the anthropomorphic god of evolution or whatever, put this... It really did say Daniel, the thing you're going to care about is gene alleles. Like the allele frequency of... I bet I'm using the wrong terminology. But how much my genes are in the population. Also Daniel, stubbing your toe, it doesn't help with that. Pain, typically, not good for this.

**Daniel Filan:**
But what you really care about is your genes being propagated. And in that case, I really, presumably, I'm going to basically behave fine. And when these things are going to diverge, hopefully I eventually figure it out. But there's this problem, I guess there are two problems. Firstly, then it's a lot more description length because you have to pre-load all of this information. And secondly, there's this issue where well, when I'm kind of dumb, the bit of me that's saying, "well, what you actually care about is propagating your genes", that's not actually influencing my behavior in any way, because I'm just acting on the proxies anyway, because I know that they're proxies. So if there's some pressure to... It seems like that bit is just going to be totally unmoored and might randomly drift or maybe get pruned away because it's just not influencing my behavior.

**Evan Hubinger:**
Yeah. I think that's basically exactly what I would say. I agree with that.

**Daniel Filan:**
Cool.

**Evan Hubinger:**
On both points.

**Daniel Filan:**
All right. Another thing - this question is kind of long, and it only eventually gets to being about inner alignment. So I'm sorry listeners, but we're going to get there. So there's this argument in the paper, which basically says that if you're training a system for a diverse set of tasks, you want to expend most of your optimization online, once the task is done. You don't want to do a bunch of your optimization power in generating this system and then pre-computing, "okay, I'm going to figure out the policy for this type of task and pre-compute this type of task." You just want to do that optimization once you know the task, so that you can only do it for the thing that's actually useful.

**Daniel Filan:**
And this reminded me a lot of, Eric Drexler's comprehensive AI services or CAIS, C-A-I-S model. Where you just, instead of having an AGI, you just... There are a bunch of tasks you want to be done and you just have a bunch of AIs that are made for each type of task and when you have a task you select the appropriate AI. So I'm wondering does this argument... My first question is, does this argument reinforce, or is this an argument for the idea of doing something like CAIS instead of creating this single unified AGI?

**Evan Hubinger:**
Yeah. So I think it gets a little bit more complicated when you're thinking about, how things play out on a strategic landscape. So there might be many reasons for why you want to build one AI system and then, for example, fine tune that AI system on many different tasks, as opposed to independently training many different AI systems. Because if you independently train many different AI systems, you have to expend a bunch of training compute to do each one. Whereas, if you train one and then fine tune it, you maybe have to do less training compute, because you just do it... a lot of the overlap you get to do only once. I think in that sense... And I think that does, the sort of argument I just made, sort of does parallel the argument that we make in the paper where it's like, well, there's a lot of things you only want to do once.

**Evan Hubinger:**
Like the building of the optimization machinery, you only want to do once. You don't want to have to figure out how to optimize every single time, but the explicit, what task you're doing, the locating yourself in the environment locating the specific task that you're being asked to do is the sort of thing which you want to do online. Because it's just really hard to recompute that if you're in an environment where you have to have massive possible divergence in what tasks you might be presented in every different point. And so there's this similarity there, where it suggests that, in a CAIS style world, you might also want a situation where, you have one AI which knows how to do optimization and then you fine tune it for many different possible tasks.

**Evan Hubinger:**
It doesn't necessarily have to be actual fine tuning. You can also think about something like GPT-3, with the OpenAI API where people have been able to locate specific tasks, just via prompts. So you can imagine something like that also being a situation where, you have a bunch of different tasks, you train one model which has a bunch of general purpose machinery and then a bunch of ability to locate specific tasks. I think any of these possible scenarios, including the... You just train many different AIs with different, very different tasks because you really need fundamentally different machinery, are possible. And I think risks from learned optimization basically applies in all these situations. I mean, whenever you're training a model, you have to worry that you might be training a model which is learning an objective, which is different from the loss function you trained under. Regardless of whether you're training one model and then fine tuning a bunch of times or training many different models for whatever sort of situation you're in.

**Daniel Filan:**
Yeah. Well, so the reason I bring it up is that, if I think about this CAIS scenario where, say, the way you do your optimization online is, you know you want to be able to deal with these 50 environments so you train - maybe you pre-train one system, but you fine tune all 50 different systems, you have different AIs. And then once you realize you're in one of the scenarios you deploy the appropriate AI, say. To me, that seems like a situation where instead of this inner alignment problem, where you have this system that's training to be a mesa-optimizer and it needed to spend a bunch of time doing a lot of computation to figure out what worlds it's in and to figure out an appropriate plan.

**Daniel Filan:**
It seems like in this CAIS world, you just have... You want to train this AI. And the optimization that in the unified AGI world would have been done inside the thing during inference, you get to do. So you can create this machine learner that doesn't have that much inference time. You can make it not be an optimizer. So maybe you don't have to worry about these inner alignment problems. And then it seems, what CAIS is doing is, it's turning... It's saying, well, I'm going to trade your inner alignment problem for an outer alignment problem of getting the base objective of this machine learning system to be what you actually want to happen. And also you get to be assisted by some AIs maybe that you already trained, potentially, to help you with this task. So I'm wondering first, do you think that's an accurate description? And secondly, if that's the trade, do you think that's a trade worth making?

**Evan Hubinger:**
Yeah, so I guess I don't fully buy that story. So I think that, if you think about a CAIS set up, I don't think... So I guess you could imagine one situation like what you were describing, where you train a bunch of different AIs and then you have maybe a human that tries to switch between those AIs. I think that, first of all, I think this runs into competitiveness issues. I think this question of, how good are humans at figuring out what the correct strategy is to deploy in the situation? In some sense, that might be the thing which we want AIs to be able to do for us.

**Daniel Filan:**
The human could be assisted by an AI.

**Evan Hubinger:**
Yeah. You could even be assisted by an AI. But then you also have the problem of, is that AI doing the right thing? And in many ways I think that in each of these individual situations... I have a bunch of AIs that are trained in all these different ways, in different environments. And I have maybe another AI, which is also trained to assist me in figuring out what AI that I should use. Each one of these individual AIs has an inner alignment problem associated with it. And might that inner alignment problem be easier because they're trained on more restrictive tasks? In some sense yes, I think that if you train on a more restricted task, you're less likely to end up with - search is less valuable because you have this less generality.

**Evan Hubinger:**
But if at the end of the day you want to be able to solve really general tasks, the generality still has to exist somewhere. And I still expect you're going to have lots of models, which are trained in relatively general environments, such that you're going to... Search is going to be incentivized, we're going to have some models which have mesa-objectives. And in some sense, it also makes part of the control problem harder because you have all of these potentially different mesa-objectives and different models and it might be harder to analyze exactly what's going on. It might be easier because they're less likely to have mesa-objectives, or it might be easier because it's harder for them to coordinate because they don't share the exact same architecture and base objective potentially. So I'm not exactly sure the extent to which this is a better setup or a worse set up.

**Evan Hubinger:**
But I do think that it's a situation where I don't think that you're making the inner alignment problem go away or turning it into a different problem. It still exists, I think in basically the same form. It's just like you have an inner alignment problem for each one of the different systems that you're training, because that system could itself develop a [inaudible 01:42:20] process or an objective that is different from the loss function that you're actually trying to get them to pursue.

**Evan Hubinger:**
And I think, I guess, if you think about a situation, I think there's so much generality in the world that maybe we're in a situation where let's say I train 10,000 models. I've reduced the total generality in the world by some number of bits or whatever, ten to the three, three orders of magnitude or whatever, right?

**Evan Hubinger:**
I don't know, what'd I say? 10,000 orders of magnitude? Whatever. Some number of orders of magnitude, right? It's the extent to which I've reduced the total space of generality, but the world is just so general that even if I've trained 10,000 different systems, the amount of possible space that I need to cover, if I want to able to do all of these various different things in the world, to respond to various different possible situations.

**Evan Hubinger:**
There's still so much generality that I still think you're basically going to need search. You just haven't reduced the space, the amount of which you've reduced the space by cutting it down, by having multiple models is just dwarfed by the size of the space in general. Such that you're still going to need to have models which are capable of dealing with large generality and being able to respond to many different possible situations.

**Evan Hubinger:**
And you could also imagine, if you're trying to do an alternative situation where it's CAIS, but it's also, you're trying to do CAIS like microscope AI or STEM AI, where you're explicitly not trying to solve all these sorts of general [inaudible 01:43:43] situations. Maybe things are different in that situation? But I think certainly if you want to do something like CAIS, like you're describing, where you have a bunch of different models for different situations and you want to be able to solve AI CEOs or something. It's just, the problem is so general that you just, there's still enough generality I think that you're going to need search. You're going to need optimization.

**Daniel Filan:**
Yeah, I guess, so I think I'm convinced by the idea that there's just so many situations in the world that you can't manually train a thing for each situation. I do want to push back a little bit on the - and actually that thing I'm conceding is just enough for the argument. But anyway, I do want to push back on the argument that you still have this inner alignment problem, which is that the thing in the inner alignment problem is at least in the context of this paper, is it's when you have a mesa-optimizer and that mesa-optimizer has a mesa-objective that is not the base objective.

**Daniel Filan:**
And it seems like in the case where you have these not very general systems, you just, you have the option of training things that just don't use a bunch of computation online, don't use a bunch of computation at inference time, because they don't have to figure out what task they're in because they're already in one. And instead they can just be a controller, you know? So these things aren't mesa-optimizers, and therefore they don't have a mesa-objective. And therefore there is no inner alignment problem. Now maybe there's the analog of the inner alignment [problems], there's the compiled inner alignment problem. But that's what I was trying to get at.

**Evan Hubinger:**
Yeah. So I guess the point that I'm trying to make is that even a situation where you're like, "Oh, it already knows what tasks it needs to do because we've trained the thing for specific tasks," is that if you're splitting the entirety of a really complex problem into only 10,000 tasks or whatever, only a thousand tasks, each individual task is still going to have so much generality that I think you'll need search, you'll need mesa-optimization, you'll a lot of inference time compute, even to solve it at that level of granularity.

**Daniel Filan:**
Okay.

**Evan Hubinger:**
The point that I'm making is that just, I think that the problem itself requires, if you're trying to solve really complex general problems, there's enough generality there, that even if you're going down to the level of granularity, which is like a thousand times reduced, there's still enough granularity, you'll probably need search, you'll still probably get mesa-optimizers there. You're going to need a lot of inference time compute etc. etc.

**Daniel Filan:**
Okay. That makes sense. I want to talk a little bit now about deceptive alignment. So deceptive alignment is this thing you mentioned in the paper where your mesa-optimizer has a different goal than... And we've talked about this before, but to recap your mesa-optimizer has a different mesa-objective than what the base objective is.

**Daniel Filan:**
And furthermore, your mesa-optimizer is still being trained and its mesa-objective cares about what happens after gradient updates. So it cares about steering the direction of gradient updates. So therefore in situations where the mesa-objective and the base objective might come apart, if you're deceptively aligned, your mesa-optimizer picks the thing that the base objective wants so that its mesa-objective doesn't get optimized away so that one day it can live free and no longer have to be subject to gradient updates and then pursue its mesa-objective. Correct me if that's wrong?

**Evan Hubinger:**
Yeah. I think that's a pretty reasonable description of what's going on.

**Daniel Filan:**
Yeah. So the paper devotes a fair bit of space to this, and I'm wondering, it mostly devotes space to what happens if you get deceptive alignment. And what I'm kind of interested in, and I don't really understand, is how this occurs during training. So at what point do you get an objective that cares about what happens after episodes, after gradients or updates, after gradient updates are applied? How does that end up getting selected for?

**Evan Hubinger:**
Yeah. So this is a really good question and it's a complicated one. And so we do talk about this in the paper a good deal. I think there's also a lot of other discussion in other places. So one thing just to point out. So let's see Mark Xu has this post on the alignment forum, [does SGD produce deceptive alignment](https://www.alignmentforum.org/posts/ocWqg2Pf2br4jMmKA/does-sgd-produce-deceptive-alignment), which I think is a really good starting point.

**Evan Hubinger:**
Also I worked with him a bunch on putting together a lot of the arguments for this in that place. So I think that's a good place to look, but I'll try to talk through some of the ways in which I view this.

**Daniel Filan:**
Okay.

**Evan Hubinger:**
So I think that in the paper we have the following story. And telling stories about gradient descent is hard and it's the sort of thing where you should expect to be wrong in a lot of cases.

**Evan Hubinger:**
And so I'm going to tell a story, but I don't necessarily think that this is precisely the sort of story that you would end up with. But I think there's general arguments that you can make for why this might be likely, why it might not be likely. And I think that by telling the stories is helpful for trying to understand what a setup might be like in which you would get this.

**Evan Hubinger:**
So here's the story I'm going to tell. So we have a proxy aligned mesa-optimizer. I think that basically this is where you start, because if we're thinking about all of the arguments I made previously, thinking about things like humans, having a pain response rather than caring about DNA, you're going to start with some proxy, simple, fast proxies. But maybe you do a bunch of adversarial training, maybe you do a bunch of trying to train on all of the different situations. You really force it to go to zero training error to really understand everything that's happening to have to sort of have a perfect version of the base objective.

**Evan Hubinger:**
What happens in this situation? Well, at some point, we'll say it has to learn the base objective. Has to know what the base objective is and be able to act directly according to that base objective. But there's multiple ways in which a model can learn what the base objective is.

**Evan Hubinger:**
So one way in which it can learn what the base objective is is, well, it has the base objective directly hard-coded into it via gradient descent. So it has some proxy objective and gradient descent sort of tinkers with that objective and keeps encoding into it more and more detail to what the correct thing is. That's one way. We call that in the paper, internalization.

**Evan Hubinger:**
There's another way, which is it can learn what the base objective is via its input data. So, rather than via the gradient descent update steps being the thing which encodes the objective into it, maybe you're trying to train it to do language modeling, predict what a human would do, and it just reads a bunch about humans and how they work. Or maybe your training it to do value learning and try to optimize for human values. And it reads the Wikipedia page on ethics and it tries to really understand human values.

**Evan Hubinger:**
And so it gets a bunch of information about the objective you're training it on through its input data. It gets a bunch of information about the thing that you're trying to get it to do, just because you, if your GPT-3 style or whatever, you can imagine training on a huge corpus of data; it has a whole bunch of information. And a lot of that information is just, whatever you're training it on is probably going to be somewhere. There's going to be information about that thing that you're training it on in that input data.

**Evan Hubinger:**
And so it can also just figure out what the objective is by looking at that input data. But then there's a question which is, well let's say the model, it's doing a bunch of world modeling. It's trying to understand the world and so it's produced an internal model that includes what the correct base objective [is]. But it doesn't know that's the correct base objective. It's not trying to optimize for that; it's just trying to optimize for its proxy. It'd be really nice, from the perspective of gradient descent, from the perspective of the training process, if it, instead of optimizing for its proxy, started optimizing for that thing, which is encoded in the world model. Started optimizing for its description, its better understanding of the base objective.

**Evan Hubinger:**
So there's the question, how does gradient descent, how does the training process modify the model to start optimizing for that thing encoded in its world model.

**Evan Hubinger:**
And fundamentally in the paper we sort of analyze two ways. One of which is, we call corrigible alignment, which is, well, its objective could change into a pointer to that thing in the world model. And another way is deceptive alignment, which is it could realize that, as an instrumental goal for optimizing its proxy, it should be trying to optimize for this thing, because it realizes that's the base objective that it's being trained on.

**Evan Hubinger:**
And so we end up with three ways in which it could learn the base objective: internalization, corrigibility, and deception. And then the question that we ask, which is, well, how likely are these different things to occur? How well do they perform on things like simplicity? How well do these measure [inaudible 01:52:08] perform?

**Evan Hubinger:**
So one thing that I'll start with, and this is an analogy from Buck Shlegeris, which is sort of a counting argument, which is like, well, let's just start with the question, how many models are there that fall into each one of these categories. And I think the answer is, well, there's a lot more deceptive models. And so here's the analogy to think about why that's the case. So we can imagine a situation where you have this Christian God, is trying to select humans, according to the extent to which they align with the Bible. And you can imagine three different humans that might fall into this category. So one of which would be Jesus Christ, who just has the exact same objective as God because-

**Daniel Filan:**
In Christianity, I believe Jesus Christ is not totally a human.

**Evan Hubinger:**
Sure, I agree with that. But let's set that aside and imagine that Jesus Christ is a possible option here that God is considering. And Jesus Christ has the exact same objective as God does, because whatever. And then you have another example, which is, let's say Martin Luther. And Martin Luther is like, "I'm going to look at the Bible and figure out exactly what the Bible says,, and try to do that as effectively as possible". And then you have Blaise Pascal and Blaise Pascal is like, "I don't care about the Bible; I don't care about God. But I know that there is a God and that God is going to select me according to whether I care about the Bible. So I'm going to care about the Bible." And these sorts of three people pretty closely match onto the sort of three scenarios that I described.

**Evan Hubinger:**
So, Jesus Christ is internalization. Martin Luther would be corrigibility, and Blaise Pascal would be deception. And the question is you, how many Blaise Pascals are there, how many Martin Luthers are there and how many Jesus Christs are there?

**Evan Hubinger:**
And I think the initial response would be well, there's only one Jesus Christ because it has to explicitly encode the entirety of the objective. There might be multiple Martin Luthers because there might be some sort of basin of corrigibility, it might be the case that if you get your pointer slightly wrong, you can still correct it by figuring out what the Bible says about what your pointer should be. You still basically have to have exactly the correct pointer to the Bible and believe that the Bible is the thing you should be following.

**Evan Hubinger:**
But there's a lot of Blaise Pascals because Blaise Pascal can have any objective, essentially, as long as Blaise Pascal is aware of the fact that they're in an optimization process, whatever their objective is, they're going to try to change it to be the one that is pointing to the Bible or whatever.

**Evan Hubinger:**
And so there's this simple counting argument, which is, well, it just seems like there's a lot more deceptive models than the others. And so just as a very naive prior, it seems like we should expect deception. But, well, we can go further than that, and we can try to ask further questions of simplicity and how likely would this action be to occur to a gradient descent process. So it seems like-

**Daniel Filan:**
So before we get to those arguments, so you're right that the paper does devote a fair bit of space to, okay, suppose you have a proxy, you're proxy aligned and your proxy extends across gradient updates. And, then you learn about the base objective. And I think there's a decent story in the paper that people can read for how that happens once you have this proxy that extends past gradient updates.

**Daniel Filan:**
And I think my initial question, to me the thing - maybe listeners are different. But to me, the thing that seems most implausible about this story is that gradient descent produces this kind of proxy because that kind of proxy is not - When the gradient is applied, the thing evaluating the gradient doesn't know whether the objective that extends past when the gradient applies was met or not because it hasn't happened yet. So, the thing that confuses me the most is how you ever get a situation where your system cares about what happens after the gradients are applied?

**Evan Hubinger:**
Yes, that's a great question. And in fact, this is actually really close to a lot of the research that I'm currently doing, trying to understand precisely the situations in which you get these cross-episodic objectives and in what situations you can produce myopic objectives instead.

**Evan Hubinger:**
So I think one way to think about this, is that fundamentally it's a generalization question because you're correct that when you're doing training, it doesn't understand that there's potentially multiple episodes in your reinforcement learning setting. It doesn't know about the multiple episodes and so it learns an objective that isn't really in terms of those multiple episodes. But let's say it does discover the existence of those multiple episodes. The question that we're asking is how does it generalize? How does it generalize in terms of caring about those multiple episodes, or not caring about those multiple episodes?

**Evan Hubinger:**
It learns some understanding of an objective, some understanding of what it's supposed to be, what sort of proxy its trying to fulfill. And that proxy - once it discovers that these other episodes exist, does it think that its proxy also exists there? Does it care about its proxy in those situations also or not?

**Evan Hubinger:**
And that's the question we're answering, this generalization question. And I think that by default, we should expect it to generalize such that - Here's a very simple intuitive argument is, if I trained a model and it's trying to find all the red things it can, and then suddenly it discovers that there are way more red things in the world than it previously thought, because there's also all of these red things in these other episodes, I think that by default you should basically expect, it's going to try to get those other red things too.

**Evan Hubinger:**
That it would require an additional distinction in the model's understanding of the world to believe that only these red things are the ones that matter, not those red things. And that sort of distinction, I think, is not the sort of distinction that you would be - gradient descent has no reason to put that distinction in the model, because like you were saying, in the training environment where it doesn't understand that there are these multiple episodes, whether it has or does not have that distinction, is completely irrelevant.

**Evan Hubinger:**
It only matters when it ends up in a generalization situation where it understands that there are multiple episodes and is trying to figure out whether it cares about them or not. And in that situation, I expect that this sort of distinction would be lacking, that it would be like, "well, there are red things there too, so I'll just care about those red things also."

**Evan Hubinger:**
Now this is a very hand-wavy argument and it's certainly, I think a really big open question. And in fact, I think it's one of the places in which I feel like we have the most opportunity potentially to intervene, which is why I was talking about one of the ways in which - a place where I focus a lot on my research. Because it seems like if we are trying to not produce deceptively aligned mesa-optimizers, then what are the ways which we might be able to intervene on this story would be to prevent it from developing an objective which cares about multiple episodes, and instead ensure that it develops a sort of myopic objective.

**Evan Hubinger:**
Now there's a lot of problems that come into play when you start thinking about that. And we can go into that, but I think just on a surface level, the point is that, well, just by default, despite the fact that you might have a myopic training setup, whether you actually end up with a myopic objective is very unclear. And it seems like naively myopic objectives might be more complicated because they might require this additional restriction of no, not just objective everywhere, but actually objective only here.

**Evan Hubinger:**
And that might require additional description length that might make it more complex. But certainly it's not the case that we really understand exactly what will happen. And so one of the interesting things that's happening here is it's breaking the assumptions of reinforcement learning, where when we, a lot of times when we do reinforcement learning, we assume that the episodes are i.i.d., that they're independent, that the model - We just totally resample the episodes every time, that there's no way for it to influence across episodes. But these sorts of problems start to occur, as soon as you start entering situations where it does have the ability to influence across episodes.

**Daniel Filan:**
Such as online learning, like we mentioned at the start, which is a common - Often when you're thinking about how are we going to get really powerful AIs that work in reality, it's like, yeah, we're going to do online learning.

**Evan Hubinger:**
Precisely. Yeah. And so if you think about, for example, an online learning setup, maybe you're imagining something like a recommendation system. So it's trying to recommend you YouTube videos or something.

**Evan Hubinger:**
One of the things that can happen in this sort of a setup is that, well, it can try to change the distribution to make its task easier in the future. You know, if it tries to give you videos which will change your views in a particular way such that it's easier to satisfy your views in the future, that's a sort of non-myopia that could be incentivized just by the fact that you're doing this online learning over many steps.

**Evan Hubinger:**
And if you think about something, especially what can happen in this sort of situation is, let's say I have a - Or another situation this can happen is let's say I'm just trying to train the model to satisfy humans' preferences or whatever.

**Evan Hubinger:**
It can try to modify the humans' preferences to be easier to satisfy. And that's another way of which this non-myopia could be manifest where, it tries to take some action in one episode to make humans' preferences different so that in future episodes they'll be easier to satisfy.

**Evan Hubinger:**
And so there are a lot of these sorts of non-myopia problems. And not only, so I was making an argument earlier that it seems like non-myopia might just be simpler. There's also a lot of things that we do that actually explicitly incentivize it. So if you think about something like population-based training, population-based training is a technique where what you do is you basically create a population of models, and then you evolve those models over time. You're changing their parameters around, you're moving, changing their hyperparameters and stuff, just make it so that they have the best performance over many episodes.

**Evan Hubinger:**
But as soon as you're doing that, as soon as you're trying to select from models, which have the best performance over multiple episodes, you're selecting for models which do things like change the humans' preferences, such that in the next episode, they're easier to satisfy. Or recommendation systems which try to change you such that you'll be easier to give things to in the future.

**Evan Hubinger:**
And this is the sort of thing that actually can happen a lot. So David Krueger and others have [a paper](https://arxiv.org/abs/2009.09153) that talks about this, where they talk about these as hidden incentives for distributional shift or auto induced distributional shift, where you can have setups like population-based training where the model is directly incentivized to change its own distribution in this non-myopic way.

**Evan Hubinger:**
And so in some ways there's two things going on here. There's, it seems like even if you don't do any of the population-based training stuff, the non-myopia might just be simpler. But also there's a lot of the techniques that we do that because they implicitly assume this i.i.d. assumption that's in fact, not always the case, they implicitly can be incentivizing for non-myopia in ways that might not [inaudible 02:02:31].

**Daniel Filan:**
Okay. So I have a few questions about how I should think about the paper. So the first one is most of the arguments in this paper are kind of informal, right? There's very few mathematical proofs; you don't have experiments on MNIST, the gold standard in ML. So how confident are you that the arguments in the paper are correct? And separately, how confident do you think readers of the paper should be that the arguments in the paper are correct?

**Evan Hubinger:**
That's a great question. Yeah. So I think it really varies from argument to argument. So I think there's a lot of things that we say that I feel pretty confident in. Things like, I feel pretty confident that search is going to be a component of models. I feel pretty confident that you will get simple, faster proxies. Some of the more complex stories that we tell, I think are less compelling. Things that I feel like are somewhat less compelling, we have the description of a mesa-optimizer as having this really coherent observation process, really coherent objective. I think that I do expect search; to the extent to which I expect a really coherent objective and a really coherent optimization process, I'm not sure. I think that's certainly a limitation of our analysis.

**Evan Hubinger:**
Also, some of the stories I was talking about about deception, I do think that there's a strong prior that should be like, "hmm, it seems like deception is likely," but we don't understand it fully enough. We don't have examples of it happening, for example.

**Evan Hubinger:**
And so it's certainly something where we're more uncertain. So I do think it varies. I think that there's some arguments that we make that I feel relatively confident in, some that I feel relatively less confident in. I do think, I mean, we make an attempt to really look at what is our knowledge of machine learning inductive biases, what is our understanding of the extent to which certain things would be incentivized over other things and try to use that knowledge to come to the best conclusions that we can.

**Evan Hubinger:**
But of course, that's also limited by the extent of our knowledge, our understanding of inductive biases is relatively limited. We don't fully understand exactly what neural networks are doing internally. And so there are fundamental limitations to how good our analysis can be, and how accurate it can be. But I do think that in terms of, well, "this is our best guess currently", I feel like is how I would think of the paper.

**Daniel Filan:**
Okay. And I guess sort of a related question, my understanding is a big motivation to write this paper is just generally being worried about really dangerous outcomes from AI, and thinking about how we can avert them. Is that fair to say?

**Evan Hubinger:**
Yeah. I mean, certainly the motivation for the paper is we think that the situation in which you have a mesa-optimizer that's optimizing for an incorrect objective is quite dangerous and that this is the central motivation. And there's many reasons why this might be quite dangerous, but the simple argument is well, if you have a thing which is really competently optimizing for something which you did not tell it to optimize for, that's not a good position to be in.

**Daniel Filan:**
So I guess my question is, out of all the ways that we could have really terrible outcomes from AI, what proportion of them do you think do come from inner alignment failures?

**Evan Hubinger:**
That's a good question. Yeah. So this is tricky. Any number that I give you, isn't necessarily going to be super well-informed. I don't have a really clear analysis behind any number that I give you. My expectation is that most of the worst problems, the majority of existential risk from AI comes from inner alignment and mostly comes from deceptive alignment, would be my personal guess.

**Evan Hubinger:**
And some of the reasons for that are, well, I think that, there's a question of what sorts of things will we be able to correct for, what sorts of things will be not be able to correct for? And I think that there's a sense in which, if you think about, the thing we're trying to avoid is these unrecoverable failures, situations in which our feedback mechanisms are not capable of responding to what's happening.

**Evan Hubinger:**
And I think that deception, for example, is particularly bad on this. Especially if you have a situation where, because the model basically looks like it's fine until it realizes that it will be able to overcome your feedback mechanisms, in which case, it no longer looks fine.

**Evan Hubinger:**
And that's really bad from the perspective of being able to correct things by feedback. Because it means you only see the problem at the point where you can no longer correct it. And so those sorts of things lead me into the direction of thinking that things like deceptive alignment are likely to be a large source of the existential risk.

**Evan Hubinger:**
I also think that, I don't know, I think that there's certainly a lot of risk also just coming from standard proxy alignment situations, as well as relatively straightforward outer alignment problems.

**Evan Hubinger:**
I am relatively more optimistic about outer alignment because I feel like we have better approaches to address outer alignment. I think that things like Paul Christiano's amplification as well as his learning the prior approach, Geoffrey Irving's debate. I think that there exists a lot of approaches out there, which I think have made significant progress on outer alignment, even if they don't fully solve outer alignment. Whereas, I feel like our progress on inner alignment is less meaningful. And I expect that to sort of continue such that I'm more worried about inner alignment problems.

**Daniel Filan:**
Hmm. So I guess this gets to a question I have. So, one thing I'm kind of thinking about is, okay, what do we do about this? But before I do that, in military theory, there's this idea called the OODA loop. OODA is short for, it's O-O-D-A. It's observe, orient, decide, and act, right? And the idea is, well, if you're a fighter pilot and you want to know what to do, first you have to observe your surroundings. You have to orient to think about, okay, what are the problems here? What am I trying to do? You have to decide on an action and then you have to act.

**Daniel Filan:**
And the idea of the OODA loop is you want to complete your OODA loop and you want to not have the person you want to shoot out of the sky complete their OODA loop. So instead of asking you what we're going to do about it and jumping straight to the A, first, I want to check where in the OODA loop do you think we are in terms of inner alignment problems?

**Evan Hubinger:**
Yeah. So it's a little bit tricky. So in some sense, first of all, there's a view which we aren't even at observe yet, because we don't even have really good empirical examples of this sort of thing happening. We have examples of machine learning failing, but we don't have examples of, is there an optimizer in there? Does it have an objective? Did it learn something? We don't even know!

**Evan Hubinger:**
And so in some sense we aren't even at observe. But we do have some evidence, right? So, the paper looks at lots of different things and tries to understand, some of what we do understand about machine learning systems and how they work. So in some sense, we have observed some things, and so in that sense, I would say the paper is more at orient. The paper doesn't, Risks from Learned Optimization doesn't come to a conclusion about this is the correct way to resolve the problem. It's more just trying to understand the problem based on the things that we have observed so far.

**Evan Hubinger:**
There are other things that I've written that are more at the decide stage, but are still mostly at the orient stage. I mean, so stuff like my post on [relaxed adversarial training for inner alignment](https://www.alignmentforum.org/posts/9Dy5YRaoCxH9zuJqa/relaxed-adversarial-training-for-inner-alignment) is closer to the decide stage because it's investing in a particular approach. But it's still more just, let's orient and try to understand this approach because, there's still so many open problems to be solved. So I think we're still relatively early in the process for sure.

**Daniel Filan:**
Do you think that it's a problem that we're doing all this orientation before the observation, which is, I guess the opposite of what, if you take OODA loop seriously, it's the opposite order that you're supposed to do it in.

**Evan Hubinger:**
Sure. I mean, it would be awesome if we had better observations, but in some sense we have to work with what we've got, right? We want to solve the problem and we want to try to make progress on the problem now because that will make it easier to solve in the future. We have some things that we know and we have some observations; we're not totally naive.

**Evan Hubinger:**
And so we can start to make understanding, can start to produce theoretical models, try and understand what's going on before we get concrete observations. And for some of these things, we might never get concrete observations, until potentially it's too late. If you think about deceptive alignment, there are scenarios where we don't see - if a deceptive model is good at being deceptive, then we won't learn about the deception until the point at which we've deployed it everywhere, and it's able to break free of whatever mechanisms we have, or doing feedback.

**Evan Hubinger:**
And so if we're in a situation like that, you have to solve the problem before you get the observation, if you want to not die to the problem. And so, it's one of the things that makes it a difficult problem, is that we don't get to just rely on the more natural feedback loops of like, we look at the problem that's happening, we try to come up with some solution and we deploy that solution because we might not get great evidence. We may not get good observations of what's happening before we have to solve it. And that's one of the things that makes the problem so scary, I think from my perspective, is that we don't get to rely on a lot of our more standard approaches.

**Daniel Filan:**
Okay. So on that note, if listeners are interested in following your work, seeing what you get up to, what you're doing in future, how should they do that?

**Evan Hubinger:**
Yeah, so my work is public and basically all of it is on the Alignment Forum. So you can find me, I am [evhub, E-V-H-U-B on the Alignment Forum](https://www.alignmentforum.org/users/evhub). So if you go [there](https://www.alignmentforum.org/users/evhub), you should be able to just Google that and find me, or just Google my name. And you can go through all of the different posts and writing that I have. Some good starting points:

**Evan Hubinger:**
So once you've, I don't know if you've already read [Risks from Learned Optimization](https://arxiv.org/abs/1906.01820), other good places to look at some of the other work that I've done might be [An overview of 11 proposals for building safe advanced AI](https://arxiv.org/abs/2012.07532), which I've mentioned previously, and goes into a lot of different approaches for trying to address the overall AI safety problem and includes an analysis of all of those approaches on outer alignment and inner alignment.

**Evan Hubinger:**
And so trying to understand how would they address mesa-optimization problems, all this stuff. And so I think that's a good place to go in to try to understand my work. There's also, another post that might be good, would be the [Relaxed adversarial training for inner alignment post](https://www.alignmentforum.org/posts/9Dy5YRaoCxH9zuJqa/relaxed-adversarial-training-for-inner-alignment), which I had mentioned tries to address the very specific problem of relaxed adversarial training.

**Evan Hubinger:**
It's also worth mentioning that if you like Risks from Learned Optimization, I'm only one of multiple co-authors on the paper, and so you might also be interested in some of the work of some of the other co-authors. So I mention that Vlad, Vladimir Mikulik has [a post on 2D robustness](https://www.alignmentforum.org/posts/2mhFMgtAjFJesaSYR/2-d-robustness), which I think is really relevant, worth taking a look at. I also mentioned some of Joar's recent stuff on trying to, Joar Skalse, on trying to understand some of what's going on with inductive biases and the prior of neural networks. And so there's lots of good stuff there, to take a look at from all of us.

**Daniel Filan:**
Okay. Well, thanks for appearing on the podcast and to the listeners, I hope you join us again.

**Daniel Filan:**
This episode was edited by Finan Adamson.
