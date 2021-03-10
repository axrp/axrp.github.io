---
layout: post
title:  "5 - Infra-Bayesianism with Vanessa Kosoy"
date:   2021-03-09 20:30:00 -0800
categories: episode
---
[Google Podcasts link](example.com)

This podcast is called AXRP, pronounced axe-urp and short for the AI X-risk Research Podcast. Here, I ([Daniel Filan](https://danielfilan.com/)) have conversations with researchers about their papers. We discuss the paper and hopefully get a sense of why it's been written and how it might reduce the risk of artificial intelligence causing an [existential catastrophe](https://en.wikipedia.org/wiki/Global_catastrophic_risk): that is, permanently and drastically curtailing humanity's future potential.

The theory of sequential decision-making has a problem: how can we deal with situations where we have some hypotheses about the environment we're acting in, but its exact form might be outside the range of possibilities we can possibly consider? Also, how do we deal with situations where the environment can simulate what we'll do in the future, and put us in better or worse situations now depending on what we'll do then? Today's episode features Vanessa Kosoy talking about infra-Bayesianism, the mathematical framework she developed with Alex Appel that modifies Bayesian decision theory to succeed in these types of situations.

Before the interview, I have a quick announcement to make. In order to make this podcast better, I've released a survey to get feedback from you listeners. If you have a few minutes to spare, I'd greatly appreciate it if you could fill it out - you can access it [here](https://forms.gle/LFKi1whWASaw3bTY8). Now, on to the main show.

**Daniel Filan:**
Hello everybody. Today I'm going to be talking to Vanessa Kosoy. She is a research associate at the Machine Intelligence Research Institute. She's worked for over 15 years in software engineering. About five years ago, she started AI alignment research, and is now doing that full-time. She's authored three papers, but today we're going to be talking about her [sequence of posts on infra-Bayesianism](https://www.alignmentforum.org/s/CmrW8fCmSLK7E25sa) that was co-authored by Alex Appel. So, Vanessa, welcome to AXRP.

**Vanessa Kosoy:**
Thank you, Daniel.

**Daniel Filan:**
All right. I guess the first question is, in a nutshell, what is infra-Bayesianism?

**Vanessa Kosoy:**
Infra-Bayesianism is a mathematical framework that is meant to deal with the problem of non-realizability in reinforcement learning, or in the theory of reinforcement learning. The problem of non-realizability is that you have a certain hypothesis space that your algorithm is trying to learn. It is trying to learn which hypothesis is correct, but the real world is described by none of those hypotheses because it's much more complex than any model could possibly capture, and most of theoretical research, or virtually all theoretical research in reinforcement learning focused on realizable cases, so just assuming that the world can be exactly described by one of the hypotheses, and that's where infra-Bayesianism comes in and explains what happens when we drop this assumption.

**Daniel Filan:**
Okay. I gather that this work is motivated by AI alignment, or ensuring that that when we create really smart AIs in the future, they're going to do what we want. How big a problem do you think non-realizability is for this?

**Vanessa Kosoy:**
Well, there are several reasons why I'm interested in this in the context of AI alignment.

**Daniel Filan:**
Okay.

**Vanessa Kosoy:**
One reason is just de-confusion, right? Trying to understand what does it mean for a reinforcement learning system to operate in a non-realizable setting, and how to think about this mathematically. The other reason is because, if ultimately we want to have algorithms that satisfy formal guarantees, if we want alignment to be provable in some mathematical model, then non-realizability is one of the issues we're going to have to deal with in that context, and a third reason, or it's like a third group of reasons, maybe, is that there are multiple questions related to de-confusing AI that hit this non-realizability obstacle. For example, the question of embedded agency, which MIRI [the Machine Intelligence Research Institute] has been thinking about for some time, and promoting as one of the important problems, is something that's closely related to non-realizability, because if your agent is-

**Daniel Filan:**
What is embedded agency, I should ask.

**Vanessa Kosoy:**
Right. Embedded agency talks about agents that are part of the physical world, right? The classical approach to reinforcement learning is viewing an agent as just two channels, input and output, and there's no physical world anywhere in the picture. Embedded agency is trying to understand how to fix that, because, for example, human values are not necessarily expressible in terms of human inputs and outputs, right, so how do you think of that, and because of various failure modes that can happen because you're not taking into account in our models that the AI is actually part of the environment and not just a completely separate thing. This is closely related to non-realizability, and indeed I have a program, how to use infra-Bayesianism, how to apply to understand embedded agency, and that's one thing. Another thing is reflection. Reasonable agents that do self-improvement. That also very quickly leads you to problems of non-realizability, because in reflection an agent should be thinking about itself, right, but it cannot have a perfect model of itself.

**Daniel Filan:**
Why can't it have a perfect model of itself?

**Vanessa Kosoy:**
Well, it's this kind of paradox, because it enters a sort of infinite loop, right? It's like nobody can tell you a prediction of what you would do because maybe you will listen to this prediction and do the opposite, or something like that, or you can think about it just complexity theoretically. The space of hypotheses the agent is working with, their computational complexity is lower than the computational complexity of the agent itself, because it needs to somehow work with all of those hypotheses, so the agent just is too big to fit into its own hypothesis space. It's really similar to self-referential paradoxes we have in mathematics, in logic or other places.

**Vanessa Kosoy:**
Yeah, but you can also view it as a special case of non-realizability, so infra-Bayesianism can help you there also, and another thing is decision theory, and this is something that we actually wrote about in the sequence, that infra-Bayesianism basically solves all the paradoxes, like Newcomb's problem, and the other types of problems that MIRI were writing about, like counterfactual mugging and so on. The sort of problems that so-called updateless decision theory, or it's sometimes called functional decision theory, was invented for, but never fully mathematically formalized. Infra-Bayesianism solves those problems, or at least a very large class of those problems, in a way that's completely mathematically formal. So we see that there's a whole range of different applications.

**Daniel Filan:**
Yeah, just getting onto that last part for a second. For those who might not be familiar, what is decision theory? What type of thing is decision theory trying to be a theory of?

**Vanessa Kosoy:**
Yeah. Decision theory is the field that talks about how do we think of making rational decisions mathematically, and I guess it began with the work of ... or at least the mathematical side began with the work of von Neumann and Morgenstern, where they proved that expected utility maximization is always the correct way to make decisions under some reasonable assumptions, and it developed from there. So game theory and economics are all offsprings of decision theory in some sense, and specifically MIRI and Yudkowsky wrote about some interesting paradoxes, like the Newcomb paradoxes, that are a problem for classical decision theory, and it showed that our understanding of decision theory is lacking in those specific examples.

**Daniel Filan:**
What is the Newcomb paradox?

**Vanessa Kosoy:**
Okay. The Newcomb paradox is the following setting. You're playing a game with some entity called Omega, and Omega is something very powerful. Omega is very smart, or it just has a huge computer, and it's so powerful that is can simulate you, and it can predict everything you do. In theory, that's completely possible, because we know that our brains operate on the laws of physics, so if you have a big enough computer, you could in theory predict everything that a person would do, and then Omega offers you a choice. You have two boxes, and you need to choose, and there is money inside of them, and you need to choose either the first box or both the first box and the second box together, and this seems like a really silly question. Obviously, if you can choose only A or A plus B, then A plus B is always better than A.

**Vanessa Kosoy:**
But there is a catch, and the catch is that Omega predicted what you would choose, and if you chose to take only the first box, then it put a million dollars in that box beforehand, before the game even started. If you chose to take both boxes, then the first box is going to be empty and the second is going to have $1,000. The result is, what happens when we see someone picking the first box, only the first box, they walk away with a million dollars, and when we see someone picking two boxes, they walk away only with $1,000 dollars, so it seems like, in some sense, it is better to pick only one box. However, classical approaches to decision theory, like causal decision theory, which is popular in philosophy, they would advise us to take both boxes, so something is wrong here. Philosophers have debated these questions, but nobody, until now, gave a completely formal decision theory that explains formally why you need to choose one box in this situation.

**Daniel Filan:**
Okay. I should say there's this thing called evidential decision theory that does choose one box in this scenario. Is that right?

**Vanessa Kosoy:**
Right, you have evidential decision theory, which does choose one box, but then it fails in other situations, so it's like ... Yeah. Here the idea is that you want to succeed in all possible setups in which there is something predicting you and interacting with you. You want to be successful in this all of this. For evidential theory, for example, there is the [XOR blackmail problem](https://static1.squarespace.com/static/5d5add414889080001ea6912/t/5d67f57c96fe220001f7e85f/1567094142529/_JPhil__Cheating_Death_in_Damascus.pdf), in which evidential decision theory fails and causal decision theory succeeds, and then there's the counterfactual mugging problem in which both evidential decision theory and causal decision theory both fail. On the other hand, updateless decision theory is supposed to succeed in all of those, only that updateless decision theory was not formally mathematically defined until infra-Bayesianism.

**Daniel Filan:**
Okay. I'm still going to ask preliminary questions. Just to give listeners more of a sense, I guess, of what problems we're going to be trying to solve, what is this counterfactual mugging thing which classical decision theories don't do well in but your new updateless decision theory inside of infra-Bayesianism is going to do well in?

**Vanessa Kosoy:**
Right. Counterfactual mugging is another game with Omega, and, like before, Omega can always predict everything you will do, and this time what happens is that Omega flips a fair coin, and if the coin falls on heads, then Omega comes to you and asks you for $100, and you can either agree to give them the $100, or you can refuse. On the other hand, if the coin falls on tails, then Omega might give you a million dollars, but they only give you the million dollars if, in the counterfactual scenario, in the scenario where the coin falls heads, you would agree to give them the $100. In this case, if you're the type of agent that would agree to give Omega $100, your expected profit in this whole scenario is $500,000 minus $50, and if you're the type of agent who would refuse, then your expected profit is zero, which is much worse. But this classical decision theory would refuse to pay the $100.

**Daniel Filan:**
Yeah, and this is basically, as I understand, it's because, at the time they're asked for $100, basically classical decision theories just say, well, I can either pay you $100 and get nothing or I can not pay you $100 and not lose anything, and so from now on my life is good if I don't pay you, and they're just not considering about what would have happened in this alternate universe.

**Vanessa Kosoy:**
Yeah, exactly. What happens is that, in those scenarios, classical decision theory, they would choose to pre-commit to paying the $100 if they could, but if they cannot pre-commit, they have no intrinsic mechanism of doing so. They need some kind of an external crutch that would allow them to make it through those scenarios.

**Daniel Filan:**
Okay. To summarize all of that, it seems like the point of infra-Bayesianism ... my read is that it's going to do two things for us. Firstly, it's going to let us have something like a good, I don't know, a good epistemology or a good decision theory about worlds that we could be in where the real world isn't one of the wolds that we are explicitly considering, but we can still do okay in it, and secondly it's going to let us choose actions. It's going to give us a decision theory that is going to solve these problems ... We're going to be expected to do well in these problems where some other agent is simulating us and figuring out what we are going to do or what we would have done had things turned out differently. Is that a fair summary of what we want to get out of infra-Bayesianism?

**Vanessa Kosoy:**
It's sort of a fair summary. I would say that the second is really a special case of the first, because when we have agents that are predicting us, then it means that we cannot predict them, right, because otherwise we have this, again, this self-referential paradox. If our environment contains agents that are predicting us, then it means that necessarily our environment is not described by one of the hypotheses we are explicitly considering. So the ability to deal with worlds that are more complex than your ability to fully describe is what gives you the power to deal with those Newcombian scenarios.

**Daniel Filan:**
Okay. So, one thing that people will notice when they start reading these posts is there's a lot of math defined in the post, right? It's not just we're going to have bog standard probability distributions and go from there. Why do you need this new math? Why can't we just do all of this with normal probability theory that many listeners know and love?

**Vanessa Kosoy:**
Well, the thing is that normal probability theory is kind of the problem here. The whole problem with non-realizability is that Bayesian probability theory, Bayesian epistemology, doesn't really work with non-realizability. When your environment is precisely described by one of the hypotheses in your prior, then we have theorems, like the merging of opinions theorem, which shows that your predictions about what will happen will converge to what actually happens. But if your prior is mis-specified, that's something that sometimes is called the mis-specified prior problem in statistics, then there are no guarantees about what your Bayesian reasoning will give you. There are sometimes guarantees you can prove if you assume the environment is ergodic, but that's also a very strong assumption that does not hold in realistic cases. So Bayesian probability theory just ...

**Daniel Filan:**
Yeah. When you say if the world is ergodic, what does it mean for a world to be ergodic? How could I know if my world was or was not?

**Vanessa Kosoy:**
Yeah, so ergodic is ... I won't try to give the formal definition, but roughly speaking it just means that everything converges to a stationary probability distribution. That's what ergodic means. If you're thinking of something like a Markov chain with a finite number of states, then it always converges to a stationary probability distribution after sufficient time. In physics, also, when systems reach thermodynamic equilibrium, they converge into a stationary distribution, and it's true that eventually everything reaches thermodynamic equilibrium, but in the real world all the interesting things happen when we're not in thermodynamic equilibrium, right? When we reach thermodynamic equilibrium, that will be the heat death of the universe, so that's not a very interesting version.

**Daniel Filan:**
Okay. If we can't use normal probabilities, can you give us a sense of roughly what are the things we're going to be working with, if we're going to try to be infra-Bayesians?

**Vanessa Kosoy:**
Yeah, so-

**Daniel Filan:**
And maybe this is going to explain why we're using the prefix infra.

**Vanessa Kosoy:**
Yeah. Well, infra-Bayesianism is, first of all, it's built upon something called imprecise probability, so I haven't really invented it from scratch. I took something called imprecise probability, although when I had the idea, I did not really read much about imprecise probability. I probably just heard the general idea vaguely sober when I started thinking about this, but anyway. We took some concept called imprecise probability, which is something which is already known in decision theory in some contexts, and is used for mathematical economics, for example, and we applied this to the theory of reinforcement learning, and we also generalized it in certain ways.

**Vanessa Kosoy:**
So our main novelty is creating this connection, and imprecise probability, what it does is it says, well, instead of just having one probability distribution, let's have a convex set, so a formally closed convex set of probability distributions. For example, just to think of a very simple example, suppose our probability space has just two elements, right, so probability distribution is just a single probability between zero and one. What imprecise probability tells us is, instead of using a probability, use an interval of probabilities. You have some interval between ... I don't know. Maybe your interval is between 0.3 and 0.45 or something. When you go to larger probability spaces, it becomes something more interesting because you have not just intervals, but you have some kind of convex bodies inside your space of probability distributions. Then we did a further generalization on top of that, which replaces it with concave functionals in the space of functions. But if you want to just understand the basic intuition, then you can be just thinking of those convex sets of probability distributions.

**Daniel Filan:**
Okay. I guess this gets to a question that I have, which is, is the fact that we're dealing with this convex sets of distributions ... because that's the main idea, and I'm wondering how that lets you deal with non-realizability, because it seems to me that if you have a convex set of probability distributions, in standard Bayesianism, you could just have a mixture distribution over all of that convex set, and you'll do well on things that are inside your convex set, but you'll do poorly on things that are outside your convex set. Yeah, can you give me a sense of how ... Maybe this isn't the thing that helps you deal with non-realizability, but if it is, how does it?

**Vanessa Kosoy:**
The thing is, a convex set, you can think of it as some property that you think the world might have, right? Just let's think of a trivial example. Suppose your world is a sequence of bits, so just an infinite sequence of bits, and one hypothesis you might have about the world is maybe all the even bits are equal to zero. This hypothesis doesn't tell us anything about odd bits. It's only a hypothesis about even bits, and it's very easy to describe it as such a convex set. We just consider all probability distributions that predict that the odd bits will be zero with probability one, and without saying anything at all - the even bits, they can be anything. The behavior there can be anything.

**Vanessa Kosoy:**
Okay, so what happens is, if instead of considering this convex set, you consider some distribution on this convex set, then you always get something which makes concrete predictions about the even bits. You can think about it in terms of computational complexity. All the probability distributions that you can actually work with have bounded computational complexity because you have bounded computational complexity. Therefore, as long as you're assuming a probability distribution, a specific probability distribution, or it can be a prior over distributions, but that's just the same thing. You can also average them, get one distribution. It's like you're assuming that the world has certain low computational complexity.

**Vanessa Kosoy:**
One way to think of it is that Bayesian agents have a dogmatic belief that the world has low computational complexity. They believe this fact with probability one, because all their hypotheses have low computational complexity. You're assigning probability one to this fact, and this is a wrong fact, and when you're assigning probability one to something wrong, then it's not surprising you run into trouble, right? Even Bayesians know this, but they can't help it because there's nothing you can do in Bayesianism to avoid it. With infra-Bayesianism, you can have some properties of the world, some aspects of the world can have low computational complexity, and other aspects of the world can have high complexity, or they can even be uncomputable. With this example with the bits, your hypothesis, it says that the odd bits are zero. The even bits, they can be uncomputable. They can be like the halting oracle or whatever. You're not trying to have a prior over them because you know that you will fail, or at least you know that you might fail. That's why you have different hypotheses in your prior.

**Daniel Filan:**
Thinking about the infinite bit string example, right, is the idea that, in my head, I'm going to think about, okay, there's one convex set of all the distributions where all of the even bits are zero. Maybe there's another hypothesis in my head that says all the even bits are one. Maybe there's a third that says that all of the even bits, maybe they alternate, or maybe they spell out the binary digits of pi, or they're all zero until the trillionth one, and then they're one. Is the idea that what I'm going to do is I'm going to have a variety of these convex sets in my head, and the hope is I'm going to hit the right convex set?

**Vanessa Kosoy:**
Basically, it works just like Bayesianism in the sense that you have a prior over hypotheses. Just in Bayesianism, every hypothesis is a probability distribution, whereas in infra-Bayesianism, every hypothesis is a convex set of probability distributions, and then you have a prior over those, and if the real world happens to be inside some of those sets, then you will learn this fact and exploit it.

**Daniel Filan:**
Yeah. Thinking about the connection to the mathematical theory, so in the mathematical theory, you have these things called sa-measures, and you have these closed convex cones, which people can look up, but these sets of sa-measures, and there are minimal ones in the space. Is the idea that the minimal things in these cones, these are the convex sets, and ... Yeah. I guess I'm still trying to get a sense of just exactly what the connection is between this description and the theory.

**Vanessa Kosoy:**
Yeah. The thing is that what I described is what we call, in one of the latest post, we called it crisp infra-distributions. A convex set of probability distributions, that's, in some sense, the simplest type or one of the simplest types of infra-distributions you can consider, but there are also more general objects we can consider, and the reason we introduce those more general objects is to have a dynamically consistent update rule, because in ordinary Bayesianism, you have some beliefs, and then some event happens, and you update your belief, and you have a new belief, and you have the Bayes theorem which tells you how you should do that, and we were thinking about, okay, how should we do that for infra-Bayesianism?

**Vanessa Kosoy:**
We started with just those convex sets of probability distributions, which are just something taken from imprecise probability, and with those objects, you do not have a dynamically consistent update rule, so you can still work in the sense of just decide all of your policy in advance and follow it, but there's no way to do updates. Well, the naive way to do updates is by sort of updating every distribution in your set, but that turns out to be not dynamically consistent. The behavior it prescribes after updating is not the same behavior it prescribed before updating, because the decision rule you're using with those convex sets is the maximin decision rule, so you're trying to maximize your worst-case expected utility, where the worst is taken over this convex set, and then when there are several things that can happen, then your optimal policy might be something that tries to hedge your bets, something that tries to ...

**Vanessa Kosoy:**
Okay, I'm going to choose a policy such that in neither of those branches I will do too poorly, but then when you're just in one of those branches and you're updating, you're throwing all those other branches to the garbage and forgetting about them. Your new optimal policy is going to do something different. The counterfactual mugging that we discussed before is exactly the perfect example of that, where just naively updating causes you to disagree with your a priori optimal policy. So in order to have a dynamically consistent update rule, you need some mathematical object which is more general than just those convex sets of probability distributions.

**Daniel Filan:**
Okay, and so the object you have, are these convex sets of souped-up probability distributions, is my rough understanding?

**Vanessa Kosoy:**
Yeah. There are two ways to think about it, which are related by Legendre-Fenchel duality. Yeah. The way we introduce them initially in the sequence is we consider ... so instead of just thinking of probability distributions, we think of measures that might not sum to one, and you also have this constant term. You're thinking of a probability distribution that someone multiplied by a constant, and also added a constant, which is not part of the distribution, but it's added to your expected utility, and then you have a convex set of those things.

**Vanessa Kosoy:**
With those things, you can have dynamic consistency, because this constant term is keeping track of the expected utility in counterfactual scenarios, and the multiplicative term is keeping track of your a priori probability to add up in the branch in which you're added up. But there's also another way which I personally think is more elegant to think about it, where instead of dealing with those things, you're just thinking of expected value as a function of functions. What are the probability distributions even for? You can think of probability distributions as just gadgets for taking expected values, and in classical probability theory, those gadgets are always linear functionals, so every probability distribution gives you a linear functional on your space of functions, the space of things that you might want to take an expected value of.

**Daniel Filan:**
And concretely, this functional is taking a function from your, I don't know, event set to the real numbers, and this functional is taking, okay, on average, what's the value of this function, given my distribution, over what the true event might be, and this ends up being linear in what the function from events to real numbers is, just to clarify that for the listeners.

**Vanessa Kosoy:**
Yeah, exactly. In classical probability theory, there is a classical theory which says that you can just think of probability distributions as linear functionals. You can literally just consider all linear functionals that are continuous in ... there are some technical mathematical conditions. They should be continuous in some topology and they should be positive, so if your function is positive, the expectation also should be positive, but then you can define probability distributions as functionals. It's an equally good way to define them, and then the way you go from classical probability theory to infra-Bayesianism is instead of considering linear functionals, you consider functionals that are monotonic and concave.

**Vanessa Kosoy:**
Monotonic means that you have a bigger function, it should give you a bigger expected value. Concave is concave. When you're averaging several functions, your values should only become higher, and you can consider just all of those concave monotonic functionals, and those are your infra-distributions. The fact they are concave, you can intuitively think of it as a way of making risk-averse decisions, basically, because that's what corresponds to this maximin rule, and that's why those things are also used in economics sometimes, because you want to be risk averse.

**Daniel Filan:**
Okay. So we have these infra-distributions, which are kind of the same thing as these concave monotonic functionals. Yeah. In this case where I'm not sure ... I'm just like, there's an infinite sequence of bits, and I have no idea what the even bits are going to be, but I have some guess about the odd bits. Maybe they're all zeros, maybe they're all ones. I have a variety of hypotheses for what the odd bits might be. What does my infra-distribution look like?

**Vanessa Kosoy:**
Right. Every hypothesis is like a particular infra-distribution. In the example with sequences of bits, you would have a hypothesis that only says things about even bits, you would have a hypothesis that only says things about the odd bits, you would have hypotheses that say things about all bits, you would have hypotheses that think that the XOR of every bit with the next bit is something, or whatever. Basically, the convex set, if you're thinking in terms of convex sets, then your convex set is just like the convex set of all of the things that have a particular property.

**Vanessa Kosoy:**
For example, if you're thinking of, okay, my hypothesis is all even bits are zero, then your convex set is the set of all distributions that assign probability one to the even bits being zero. In this more general thing, you have sa-distributions and so on, but that's just a technicality. That just means you need to take your set of distributions and close it by taking the Minkowski sum with some cone in this Banach space, but that's really just a technical thing.

**Daniel Filan:**
Okay. So is the idea that, for each specific hypothesis I might have, I have this convex set of distributions, and I'm going to be doing this maximin thing within that convex set of distributions, but is the idea that I'm going to be basically just a Bayesian over these hypotheses and how the hypotheses internally behave is determined by this maximin strange update type thing?

**Vanessa Kosoy:**
Sort of. Yeah. I mean, there are multiple ways you can think about it. One way you can think about it is that your expected utility, your prior expected utility, is take expectation over your prior over hypotheses, and then for each hypothesis, take minimum of expected values inside this convex set. Or you can equivalently just take all of those convex sets and combine them into one convex set. You can literally just take, like, okay, let's assume we choose some point inside each of those sets, and then we average those points with our prior, and the different ways of choosing those points, they give you different distributions, and so you get a new convex set. That's just like in ordinary Bayesianism, right? You have a set of hypotheses. Each hypothesis is a probability distribution, and you have a prior over them, or you can just average them all using this prior and get just a single probability distribution. There's two ways of looking at the same thing.

**Daniel Filan:**
All right. Cool. Hopefully this gives listeners something of an overview of infra-Bayesianism, and there's ... I don't know. If they want to learn the maths of it, they can read the post for the mathematical theory. Going forward a little bit ... So it seems like this is a theory of you have a single agent that's inside a big, scary environment that's really confusing and complicated, and the agent doesn't know everything about this environment. I think in AI, we like to think of this, I don't know, some kind of progression from a thing that's reasoning about the world, to a thing that's acting about the world, to the situation where you have multiple agents in the world that are jointly acting, and their decisions affect each other, like game theory. So I'm wondering, is there infra game theory yet, or if there isn't total infra game theory, has there been any progress made towards creating it?

**Vanessa Kosoy:**
That's a good question. In fact, one of the very interesting applications of infra-Bayesianism is exactly to multi-agent scenarios, because multi-agent scenarios are another example where ordinary Bayesianism runs into trouble because of this realizability assumption. If we have multiple agents, and each of those agents is trying to understand the other agent, trying to predict it or whatever, then we again get this self-referential paradox. We can have a situation where agent A is more powerful than agent B, and therefore agent A is able to have B inside its hypothesis space, but we usually cannot have a situation where both of them have each other in their hypothesis space.

**Vanessa Kosoy:**
There are sometimes ways you can have some trick to go ... like Reflective Oracles, which is a work by MIRI that tries to solve it and have agents that do have each other inside each other's hypothesis space, but it's very fragile in the sense that it requires your agents to be synchronized about what type of reflective oracle, what type of prior they have. They need to be synced up in advance in order to be able to close this loop, which is a weird assumption, and infra-Bayesianism might give you a way around that, because infra-Bayesianism is precisely designed to deal with situations where your environment is not precisely describable, and in fact it does give you some results.

**Vanessa Kosoy:**
For example, it's just trivial to see that, in zero sum games, infra-Bayesianism gives you optimal performance because ... well, that's kind of trivial because infra-Bayesianism is sort of pessimistic, so it imagines itself playing some kind of zero sum game. This is some open problems, some kind of area where I want to do work but I haven't really developed it yet, but it seems like, for example, one result that I believe you should be able to prove is that infra-Bayesian agents that are playing a game in a non cooperative setting will converge to only playing [rationalizable] strategies, and then you will have a guarantee that your payoff will be at least the maximin payoff inside a space of non-realizable strategies. That's already a non-trivial guarantee that you cannot easily get with Bayesianism.

**Daniel Filan:**
Okay. Yeah. I guess, related to the open work, so late last year these results were put online. I'm wondering how the reception has been and what open problems you have, and how much development there has been on those.

**Vanessa Kosoy:**
Yeah. By the way, did I say realizable? I meant rationalizable. Okay. Yeah. Currently we continue to work on developing the theory. Well, we have a ton of open research directions, like I said, we want to apply to embedded agency, and we have another post coming up, which gives some more details about decision theoretic aspects of it, and the game theory thing is another direction, and another direction, by the way, which we haven't discussed, is the relation of this to logic, because there are also some very intriguing ways to make connections between infra-Bayesianism and logic, in some sense. I have this sort of thesis that says that infra-Bayesianism is, in some sense, a synthesis of probability theory and logic, or you can think of it as a synthesis of inductive reasoning and deductive reasoning. Yeah. We've been working on a number of things.

**Vanessa Kosoy:**
Also on proving concrete regret bounds, because in reinforcement learning theory, what you ultimately want is you want to have specific quantitative regret bounds that give you specific convergence rates, and we have derived some for some toy settings, but we want to have regret bounds for some more interesting settings. Yeah. A lot of the direction, there is work we're doing. There are probably multiple papers we are going to write on the topic. I think the first paper will be some paper that will have some basics of the formalism and proves regret bounds in some basic assumptions, like infra-Bayesian bandits or infra-Bayesian MDPs, but yeah, there's definitely a whole avenue of research to be done there.

**Daniel Filan:**
Okay. Yeah. Could you say a bit more about the connection to logic, because this wasn't quite apparent to me when I was reading the posts.

**Vanessa Kosoy:**
Yeah. The logic connection is something that I think we haven't really talked much about in the posts so far. The basic idea is kind of simple. Once you have convex sets, you have the natural operation of intersecting those sets, and there's another natural operation of taking the convex hull of those sets. In other words, the sets form what's called in mathematics a lattice. Some sets are inside other sets, and you have a join and a meet. You have the least upper bound and the maximum lower bound.

**Daniel Filan:**
Which is basically just you can intersect and you can union.

**Vanessa Kosoy:**
It's intersect and convex hull because they have to be convex.

**Daniel Filan:**
Ah, right. Yes.

**Vanessa Kosoy:**
Yeah, so that gives you a sort of logic, and actually you can think of ordinary logic as embedded in that thing, because if you have some set, then to every subset of it, you can correspond an infra-distribution, which is just like all distributions supported on this subset, and then your operations of intersection and convex hull correspond to just intersection and union. But you also have things that do not correspond to any subset, and you can think of those things as some kind of logical disjunction and conjunction, but it's not distributed, so that's not classical logic. It's some kind of weird logic, and you can also define existential and universal quantifiers that play well with this thing, and then what's nice about it is that you can use this sort of logic to construct your infra-Bayesian hypothesis.

**Vanessa Kosoy:**
So if your hypothesis space, for example, consists of what we call infra-POMDPs, which is like the infra-Bayesian equivalent of POMDPs then you can use the language of this infra-Bayesian logic to specify hypotheses, and that's really interesting because maybe when you do that you have some useful algorithms for how to control the hypothesis and how to learn those hypotheses, which you don't have for just arbitrary MDPs that do not have any structure. There might also be algorithms for solving problems in this infra-Bayesian logic that do not exist for classical logic, because classical logic is often intractable, like prepositional logic is already NP-complete, and first-order logic is not computable. But infra-Bayesian logic, well, we haven't really proved anything about that, but there is some hope that it's more computationally tractable under some assumptions.

**Daniel Filan:**
When we were talking about infra-Bayesianism, there were two things that infra-Bayesianism basically promised us, right? The first thing was that it was going to deal with the problem of non-realizability in environments, like maybe we just can't imagine the true environment, but we still want to learn some things about it, and the second was that it was going to help us solve these decision problems, like Newcomb's problem, where you can one box or two box, and if somebody predicts ... depending on the prediction of what you'll do, one might be better than the other, and also counterfactual mugging, where somebody flips a coin, and if it lands tails, then, depending on what we would have done if it landed heads, we can be better or worse off. These are problems where the environment is simulating you and will make your life better or worse depending on your policy in other states. How does infra-Bayesianism help us solve these decision problems?

**Vanessa Kosoy:**
Right. What does it mean to solve a problem? It means that the agent can build a model of the problem which will lead it to taking the right actions, actions that will give it maximal utility. The usual way those problems are considered is by starting with something like a causal diagram, but here we're taking a step back and saying, okay, suppose that the agent encounters this situation, and the agent tries to understand what is happening. It is trying to learn it, to build some model of it.

**Vanessa Kosoy:**
The easiest way to imagine how this thing can happen is in an iterated setting, when you're playing the same game over and over, whether that's Newcomb's problem or counterfactual mugging. Then you can look, okay, given that my agent is in this iterated setting, what sort of model will it converge to and what sort of behavior will it converge to? We're not assuming something like a casual diagram description. Instead, we are letting the agent learn whatever it's going to learn in that situation, and what happens in situations with predictors that predict your agent is there is always this model available which says, well, there is something in the environment which is doing those things, and I have sort of Knightian uncertainty about what it's going to do.

**Vanessa Kosoy:**
That means that in my convex set of probability distributions ... I'm going to remind you that in infra-Bayesianism, our hypotheses were convex sets of probability distributions. In our convex set, we're allowing this predictor, this Omega, to make whatever predictions it wants, because there is no way to directly say, well, it's going to predict what I will do. That's not a legitimate type of hypothesis like the standard way you build hypotheses in reinforcement learning. Hypotheses in reinforcement learning are you take an action and you see an observation, then you take an action, and so on.

**Vanessa Kosoy:**
This Omega can do whatever it wants, but then there arrives some moment in time when its prediction is tested. Omega made some predictions, wrote them down somewhere where we cannot see, and then doing things according to those predictions, and then the prediction's getting tested. At this point, well, what happens is that if those predictions turns out to be false, then we can imagine that what happens is a transition to some state of infinite utility. That sounds weird at first, but ...

**Vanessa Kosoy:**
So why a state of infinite utility? Well, first of all, notice that this is consistent with observations, because the agent will never see a situation in which a prediction is falsified, because by assumption the predictor is a good predictor, so this will never actually happen, so it is consistent from the agent's perspective to assume this. But once we assume this, then the optimal policy becomes "behave as if those predictions will be true". Why? Because we're always planning for the worst case, and the predictions becoming false can never be the worst case, because if the predictions are false, if they are falsified, then we end up in a state of infinite utility, so that can never be the worst case.

**Vanessa Kosoy:**
Therefore, the worst case is always going to be when Omega actually predicts correctly, and our policy is going to be the optimal policy given that Omega predicts correctly, which is the UDT policy. Now, there is another thing where you can develop it.

**Vanessa Kosoy:**
Initially I just introduced this idea as a kind of ad hoc thing, but then we noticed that ... and that's still the upcoming post that we're going to publish soon. Then we noticed that if you also allow convex sets which are empty, so beliefs that describe the notion of contradiction, like something which is impossible, and your hypotheses are infra-MDPs, so that's like the infra-Bayesian version of MDPs, then you can, instead of literally transition to a state of infinite utility, you can use a transition where the transition kernel is an empty set, and that's just the infra-Bayesian representation of an event that is impossible, an event which is a contradiction, which this model, this hypothesis, forbids from ever happening. This kind of explains why you have infinite utility, because our utility is always taking the minimum over this convex set, but if the convex set is empty, then the minimum is infinity.

**Daniel Filan:**
All right. When you were saying that, I was reflecting on how the infinite utility thing seems like kind of a hack, but the nice thing about that empty transition kernel thing is it more naturally expresses your notion of impossibility, right?

**Vanessa Kosoy:**
Yeah.

**Daniel Filan:**
Nothing can happen if you do that.

**Vanessa Kosoy:**
Yeah, exactly.

**Daniel Filan:**
So the way this setup works is you're talking about situations where the predictor has some idea of what you're going to do, and then there's some possibility that the prediction is falsified, right, and if you act contrary to what the predictor thinks, then you get infinite utility, and so Murphy ... the laws of minimization never pick the environment where the predictor guessed wrong. In the original posts, one thing you talk about as a challenge to this is transparent Newcomb's problem. In transparent Newcomb's problem, there is box A and box B. Box A definitely contains $1,000. Box B either contains $0 if the predictor thinks you're going to take both boxes, or it contains a million dollars if the predictor thinks you're only going to take box B. But the difference with normal Newcomb's problem is that when the agent walks in, the agent can just see what's inside box B, and so the agent already knows which way the predictor chose.

**Daniel Filan:**
In your post you describe how this poses a bit of a challenge, right, because there's no possible way for the predictor to know ... if the predictor guesses wrong, and thinks that if the box were full, then you would take both boxes, and therefore the predictor makes the box B empty, then the predictor will never know what you will do in the world where both boxes are full. Can you describe how you think about that kind of situation, and if there's anything infra-Bayesianism can do to succeed there?

**Vanessa Kosoy:**
Yeah. This is correct. What we discovered is that there is a certain condition which we called pseudo-causality, which selects or restricts the types of problems where this thing can work. Yeah. The pseudo-causality condition basically says that ... well, it basically says that whatever happens cannot be affected by your choices in a counterfactual which happens with probability zero, and that's why it doesn't work in transparent Newcomb, where the outcome depends on your action when the box is full. In the version with transparent boxes where the outcome depends on your action when the box is empty, everything actually does work, right, because what happens is, if the predictor conditions its action on whether the box is empty, and I decide to one box, then the prediction is not allowed to show me the empty box, because then I will one box and will falsify its prediction.

**Vanessa Kosoy:**
That's something that we call effective pseudo-causality, which means that those counterfactual can affect you, but only in one direction. One way to look at it is you can look at it as a fairness condition. One of the debates in decision theory is which kind of decision problems should even be considered fair, like which kind of decision problems we should be expected to succeed, because obviously if you don't assume anything, then you can always invent something which is designed to fail your particular agent, because you can say, okay, Omega, if it sees an agent of this type, it does something bad, and if it sees an agent of a different type, it does something good.

**Vanessa Kosoy:**
This is something which was used to defend CDT but then Eliezer Yudkowsky in his [paper](https://intelligence.org/files/TDT.pdf) on timeless decision theory writes that, yeah, of course there should be some fairness condition, but that does not justify two boxing Newcomb's problem, because Newcomb's problem, the outcome only depends on the action you actually take, not on the algorithm used to arrive at those actions, and this was Yudkowsky's proposal of what the fairness condition should actually be. In pseudo and in infra-Bayesianism, in the straightforward version of infra-Bayesianism, the fairness condition that infra-Bayesianism is able to deal with is the environment is not allowed to punish you for things you do in counterfactuals that happen with probability zero. The environment can punish you for things you will do in the future or things you will do in a counterfactual, but not for things that you will do in a counterfactual that has a probability zero, so it's not even a real counterfactual, in some sense.

**Vanessa Kosoy:**
What can you do with this? One thing, you can just accept that this is a good enough fairness condition, and maybe it is. I'm not sure. It's hard to say definitively. Another thing is you can try to find some way around this, and we found basically two different ways around this. How to make infra-Bayesianism succeed, even in those situations, but both of them are a little hacky. One of them is introducing the thing ... we'd called it survironments, so you do a formalism where you allow infinitesimal probabilities, and then those counterfactuals that never actually happen, they get assigned an infinitesimal probability and then it kind of works, but it really complicates the mathematics a lot.

**Vanessa Kosoy:**
Another thing you can do to try to solve it is assume exploration, so just assume ... One thing to notice is that if we take this transparent Newcomb problem but assume some noise, so assume that there is some probability epsilon such that the box will come out full no matter what, right, so Omega sometimes just randomizes the outcome, then immediately this problem becomes effectively pseudo-causal. Because the counterfactual no longer has probability which is zero, everything is fine. That's just in baseline infra-Bayesian, but what you can do to use this is you can just add randomization on purpose. You can make your agent be a little noisy, so that's basically a sort of exploration. Make it sometimes take random actions which are not the action it intended to do. There's the action the agent intended to do and then what actually came out, and if you add this noise, then the transparent Newcomb problem effectively becomes equivalent to the noisy transparent Newcomb problem, and you again succeed solving it, modulo some small penalty for the noise.

**Vanessa Kosoy:**
There is some argument here that maybe this is a good solution because we know that, in learning algorithms, we often need to add noise anyway in order to have exploration, so maybe you can kind of justify that from this angle, so I don't know. Maybe it's a good solution, but I'm not sure.

**Daniel Filan:**
When you say that exploration helps, so I see how, if we think of Omega the predictor just randomly changing whether box B is full or empty, I see why that kind of exploration is going to help you in this transparent Newcomb scenario, but when the agent might randomly one box instead of two box or vice versa, I don't see how that helps, because if with high probability you're going to pick both boxes when box B is full, and I guess it's not 100% probability because you're going to explore a bit, but with really high probability you're going to do that, doesn't Omega put you in a room where box B is empty, and then it actually just doesn't care what you do in the worlds where box B is empty, so it never gets falsified?

**Vanessa Kosoy:**
Okay, so what happens with exploration is that the hypothesis the agent constructs of the environment, it says the following thing. There is some biased coin which is tossed somewhere, and this biased coin is XOR'd with the action I intended to take, and produces the action that I actually end up taking, and then when Omega's predictions are tested, they're tested against the XOR. When Omega is making its prediction, it has to make the prediction before it sees the outcome of the coin. It makes a prediction, and then coin is XOR'd with the action you intended to get, and then the result is tested against Omega's prediction. Now, if I decide to one box, then Omega has to predict that I will one box, because if it predicts that I will two box, then there is some probability that the coin will flip my action and Omega's predictions will be falsified.

**Daniel Filan:**
Sorry, I guess I'm missing something, but don't you ... Okay. Part of my struggle is like, wait, don't you never end up in the world where the box is full, but then I remember, oh, okay, we're actually taking the minimum over a bunch of things, so that's fine. But if Omega thinks that you're going to take both boxes, in the world where box B is full, and your plan is to just take one box, but with some probability, it actually takes both boxes, doesn't that ... because the random thing that's happening is confirming Omega's prediction, right, not disconfirming it?

**Vanessa Kosoy:**
Okay. From the agent's perspective, the model of the environment is as follows. There is a biased coin which determines whether the noise is going to happen or not, and then Omega makes a prediction of what action the agent will take, okay, what the intended action is. Omega predicts the intended action. Then the coin is flipped. The result of the coin is XOR'd with the intended action to give us the actual action, and then the box is either filled or stays empty according to the result of this XOR.

**Daniel Filan:**
Ah, okay. Now it makes sense how that would work. But you mentioned that you thought that this was not necessarily a satisfactory approach, right?

**Vanessa Kosoy:**
I mean, it works, but it just seems like a little hacky, a little not ... It doesn't feel like there's some deep philosophical justification for doing this, and maybe there is and I just don't see it, but I don't know.

**Daniel Filan:**
Is it right that, as long as we're in pseudo-causal environments, the agent does the "UDT prescriptions" and it gets high expected utility in all of those environments? Is that correct?

**Vanessa Kosoy:**
Yeah. Exactly. In pseudo-causal situations, if you take some finite decision problem, where there is some Omega that can predict the agent's action in any counterfactual it chooses, and make an iterated setting out of it, then, assuming that the relevant hypothesis is in our infra-prior, the agent will always converge to the UDT payout, so it will converge to the policy that has the a priori maximal expected utility, given that the predictor predicts correctly.

**Daniel Filan:**
One thing you mentioned is that when people were coming up with these problems, they were thinking of it in terms of some kind of causal graph, and like, ah, we're going to have things that are reasoning about programs or such, and the infra-Bayesian approach really has a different way of thinking about these problems, and it still does well on the ones that have been come up with, except, I guess, for this transparent Newcomb, barring these changes to the theory. I'm wondering, do you think there are other problems out there are that are kind of, I don't know, somehow similar to Newcomb and counterfactual mugging from the perspective where you're thinking about these causal graphs and agents reasoning about agents, but that infra-Bayesianism wouldn't be able to do well on?

**Vanessa Kosoy:**
I don't think so. I think that this approach is very general. Well, I think that it's true that MIRI initially, when they started thinking about those problems, they started thinking about them in a particular language, using logic or thinking about programs or something. I think that infra-Bayesianism is actually the correct language to use in any situation when you have something reflective. Yeah. The original motivation for infra-Bayesianism is when your environment is too complicated to describe exactly. This is in some sense the problem of logical uncertainty, right, just phrased in a different way.

**Vanessa Kosoy:**
Logical uncertainty was about, okay, maybe we have uncertainty and it comes from the fact that we are bounded agents and not from lack of information. Here this is exactly what we're talking about. We are bounded agents, so we cannot describe the world precisely, and I think that all of these problems should be addressed using this language, but there are also connections to those approaches, in the sense that, for example, logic ... well, I think I mentioned before that there are connections between logic and infra-Bayesianism, and there is some kind of infra-Bayesian logic which you can define.

**Vanessa Kosoy:**
Well, also there is some extension of this infra-Bayesianism that I ... I call it Turing infra-Bayesian reinforcement learning, where you have this infra-Bayesian agent, and it also interacts with something, which I'm calling the envelope, where it can just run arbitrary programs. It treats this thing, this computer, as part of its environment, and that actually allows you to use infra-Bayesianism to learn things about programs, or you can think of it as learning things about mathematics in some sense, or it plays the part of mathematics that can be formulated as running finite programs, and it's a little related to decision problems that involve logical coins, for example, right?

**Vanessa Kosoy:**
For example, the counterfactual mugging, the original counterfactual mugging involves an actual, physical coin, but then there's also the version called logical counterfactual mugging, where you use a coin which is actually pseudo-random instead of true random, and then this Turing infra-Bayesian reinforcement learning can allow you to deal with that in a rather elegant way. Yeah. So I think that for all this class of questions, infra-Bayesianism serves as the starting point of what's the correct language to think about that.

**Daniel Filan:**
I guess one strange question I have, suppose somebody listens to this podcast and they feel very inspired. They're like, I'm going to be an infra-Bayesian now. That's just how I'm going to live my life. But they spent the past however many years ... perhaps they were a causal decision theorist with classical Bayesianism. Maybe they used Knightian uncertainty with evidential decision theory. Who knows what they were doing? But they weren't doing proper infra-Bayesianism. In this situation, how should they think of themselves? Should they think that this minimization of all the expected values, should that start at the day they were born, or the day that they converted to infra-Bayesianism, or the day they first heard about infra-Bayesianism but before they converted, and are there any guarantees about how well you're going to do if you convert to infra-Bayesianism as a mid-life crisis instead of on the day you were born?

**Vanessa Kosoy:**
This is an interesting question. I haven't really thought about it. Yeah. I guess that my intuitive response would be choosing a policy such that your a priori expected utility, expected utility in the infra-Bayesian sense, is maximal, so that would be like imagining that you were infra-Bayesian since you were born but for some reason doing something different, because that's also kind of ... Yeah. That's how infra-Bayesianism updates work, right? The update rule we have in infra-Bayesianism, I think we haven't discussed it, but there's something interesting about the update rule, which is, as opposed to Bayesianism, the update rule actually depends on your utility function and your policy in the counterfactuals. The way you should update if a certain event happens depends on what you would do if this event did not happen or what did you do before this event happened. I think that this kind of fits well with your question, because if you're updating according to this rule, then automatically you're behaving in a way that's optimal from an updateless perspective, from the perspective where you should have been infra-Bayesian since you were born, but you weren't, and this is your constraint.

**Daniel Filan:**
Okay. Do you think you're going to get some kind of infra-Bayesian optimality guarantee, where if I convert to infra-Bayesianism at age 40, then my utility is going to be the best you could have done given that you followed your old, foolish ways before age 40, or are you not going to be able to get anything like that?

**Vanessa Kosoy:**
Yeah. I think that you will for the usual reasons. Basically you're a learning agent, so you learn things, and you eventually converge to something. I mean, it kind of depends on what optimality guarantees you have, right? In learning theory, to have optimality guarantees, we usually assume things, like that the environment is reversible. You cannot do something which shoots yourself in the foot irreversibly, and I think that's a different question of how do you deal with that, but obviously, if you did something irreversible before, then you're not going to be able to reverse it, but I think that, given that you already did it, you're going to have the same guarantees that you usually have.

**Vanessa Kosoy:**
But by the way, when you started asking this question, it made me think about something different, which is how do we apply this to rationality for humans, right, because I came up with this in the context of AI, but then you can say, okay, all the things that we say about rationality, like calibrating your beliefs, or making bets or whatever, how do you apply infra-Bayesianism there? I'm actually not sure. I just haven't thought a lot about this topic, but I think it is another interesting topic that somebody should think about.

**Daniel Filan:**
Yeah. I guess now that listeners have heard the basics of or the idea of what infra-Bayesianism is supposed to be about, and heard some tantalizing things about a spooky update rule, I encourage them to have a look at the posts. Maybe it'll be a paper someday. If listeners enjoyed this podcast and they're interested in following you and your work more, what should they do in order to keep up to date?

**Vanessa Kosoy:**
The easiest way to follow me is just to follow my user on [Alignment Forum](https://www.alignmentforum.org/users/vanessa-kosoy) or on [Less Wrong](https://www.lesswrong.com/users/vanessa-kosoy), which is just called Vanessa Kosoy, and of course if someone wants to discuss something specific with me then they're always welcome to send me an email, and my email address is also very easy to remember. It's <vanessa.kosoy@intelligence.org>.

**Daniel Filan:**
All right. Well, thanks for talking with me today, and to the listeners, I hope you join us again next time.

**Daniel Filan:**
This episode was edited by Finan Adamson.
