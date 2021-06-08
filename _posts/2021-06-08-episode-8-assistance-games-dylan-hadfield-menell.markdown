---
layout: post
title: "8 - Assistance Games with Dylan Hadfield-Menell"
date: 2021-06-08 16:30:00 -0700
categories: episode
---

This podcast is called AXRP, pronounced axe-urp and short for the AI X-risk Research Podcast. Here, I ([Daniel Filan](https://danielfilan.com/)) have conversations with researchers about their papers. We discuss the paper and hopefully get a sense of why it's been written and how it might reduce the risk of artificial intelligence causing an [existential catastrophe](https://en.wikipedia.org/wiki/Global_catastrophic_risk): that is, permanently and drastically curtailing humanity's future potential.

How should we think about the technical problem of building smarter-than-human AI that does what we want? When and how should AI systems defer to us? Should they have their own goals, and how should those goals be managed? In this episode, Dylan Hadfield-Menell talks about his work on assistance games that formalizes these questions. The first couple years of my PhD program included many long conversations with Dylan that helped shape how I view AI x-risk research, so it was great to have another one in the form of a recorded interview.

**Daniel Filan:**
Hello everyone. Today I'll be talking to Dylan Hadfield-Menell. Dylan's a graduating PhD student at UC Berkeley, advised by Anca Dragan, Pieter Abbeel and Stuart Russell. His research focuses on the value alignment problem in artificial intelligence: that is, the problem of designing algorithms that learn about and pursue the intended goal of their users, designers, and society in general. He will join the faculty of artificial intelligence and decision-making at MIT as an assistant professor this summer. Today, we're going to be talking about his work on assistance games, and in particular, the papers ["Cooperative Inverse Reinforcement Learning"](https://arxiv.org/abs/1606.03137), co-authored with Anca Dragan, Pieter Abbeel and Stuart Russell, ["The Off-Switch Game"](https://arxiv.org/abs/1611.08219), also co-authored with Anca Dragan, Pieter Abbeel and Stuart Russell, and ["Inverse Reward Design"](https://arxiv.org/abs/1711.02827), co-authored with Smitha Milli, Pieter Abbeel, Stuart Russell and Anca Dragan. For links to these papers and other useful info, you can check the description of this podcast and you can read a transcript at [axrp.net](https://axrp.net/). Dylan, welcome to the show.

**Dylan Hadfield-Menell:**
Thanks so much for having me Daniel. It's a pleasure to be here.

**Daniel Filan:**
I made something of an assumption in the introduction that you think about those papers under the umbrella of assistance games, is that right?

**Dylan Hadfield-Menell:**
Yeah, I think so. I take a pretty broad view of what assistance games can mean, and I tend to think of cooperative IRL as a formalization of a class of assistance games of which Off-Switch Game and Inverse Reward Design are examples.

**Daniel Filan:**
Okay. So for listeners who aren't familiar, what is an assistance game?

**Dylan Hadfield-Menell:**
An assistance game is a formalization of a sequential decision making problem where we have two players within the game. There's the human player and the AI player. Both of them have a shared goal in the sense that the AI system is meant to act on the person's behalf and, in this case, optimize utility for the person. The big difference between the two is what information they have. In an assistance game we assume that the person has knowledge about their goal and the system has to learn about this via interactions with the person.

**Daniel Filan:**
What kinds of benefits do you think we get from thinking about assistance games and thinking of AI problems in terms of assistance games?

**Dylan Hadfield-Menell:**
I think the big thing that assistance games really provide is a way of thinking about artificial intelligence that doesn't presuppose an obvious and measurable goal. One of the things that my work on assistance games has pointed me towards is that in AI systems, a lot of what we're trying to do is really provide access to a broad set of intelligent computer behaviors, really. And in many ways, a lot of the improvements in artificial intelligence have really been improvements at the level of improving the ability of a specialized and highly trained group of AI practitioners, graduate students and very early technologists, to do a better job of identifying and building in complex qualitative behaviors for systems. And in the process of doing that, these practitioners developed a bunch of mathematics of optimal decision making and how to implement that with algorithms. However, sitting outside of that research, there's always a researcher, in fact a research community, looking for how generalizable is this approach, does this capture an intelligent behavior, whatever we mean by that?

**Dylan Hadfield-Menell:**
What does the spec for the system look like? How should we evaluate it? And all of this is actually crucial to the eventual successes we've had in AI with, say, computer vision and more recent things in natural language. And what assistance games really do is they study that phenomenon at the level of abstraction where we're trying to express goals, and they allow us to identify what's the difficulty in expressing goals? What are the ways that things can go wrong? And what are the limits on our ability to specify arbitrarily good behaviors, where good can be interpreted quite loosely here.

**Daniel Filan:**
That's interesting. This is [the AI x-risk research podcast](https://axrp.net/), and you're, I guess, at least associated with a bunch of researchers who are interested in reducing x-risks from AI. I'm wondering, do you think that assistance games have any particular benefits when it comes to reducing existential threats from artificial intelligence?

**Dylan Hadfield-Menell:**
Yes, definitely. So the benefits for existential risk, I believe, come in through the extension of benefits that they have for near term concerns with AI systems. We are getting very good at increasing AI capabilities, and for a long time, this has been the bottleneck in deploying effective AI systems. And it's been reasonable to have an individual researcher using ad hoc mechanisms to align that behavior with their qualitative goals. When I worked in motion planning earlier on in my PhD, it was my job to make sure that the tests that we designed and the experiments that we built and the simulations that I ran, and eventually the work I did with real robots was generalizable and was what I wanted, if that makes sense. And it wasn't necessary to formally study it, because we were looking at AI systems largely under lab conditions.

**Dylan Hadfield-Menell:**
When you get out into the real world, the difficulty of getting the behavior exactly right, and getting the details right, often in the first go, are important. What assistance games do is they allow us to study, how do we make that choice correctly? It's sort of like we've drawn a distinction between AI technology, which is an AI system, and AI research, which is sort of the thing that happens outside of the system that sets it up.

**Dylan Hadfield-Menell:**
And I think really this interaction whereby at this point, the average grad student can specify a really wide range of interesting visual or language or motion related tasks and have them be executed on a system. That's really the place that we've gotten to for AI currently. But the thing is, if you're doing that in a lab, it's okay to have the experiment go wrong, to break some hardware. You get a lot of trial and error. It's a very forgiving, quite literally experimental environment. And as we start to put systems into the real world, we need to get better at building that infrastructure around systems more effectively. And in studying that problem that I think is how assistance games can overall reduce existential risk from AI systems.

**Daniel Filan:**
Okay. So am I right to think that the basic story is something like, before I think of assistance games, I'm going to think of some objective that I want a really smart AI to pursue and then plug it in and press go, and then it just does something absolutely terrible or enslaves humanity, or something to achieve its objective, but after assistance games, it's going to provide this method of analysis where I'm going to notice the problems of specifying what I want and making sure that an AI doesn't pursue things literally and checks in with me. Is that roughly what I should think of the benefits as?

**Dylan Hadfield-Menell:**
Yeah. One way to think about it is, supervised learning is actually a goal specification language. It's an effective goal specification language for, say, labeling images. And in fact that is how it gets tied down into real systems. We optimize a utility function that that defines. There are lots of properties of this goal specification language that are nice. Programming in it is something that we can distribute across a wide variety of relatively unskilled individuals, relatively cheaply, and we can actually optimize it in order to produce behaviors that empirically are good.

**Dylan Hadfield-Menell:**
What assistance games do is they study properties of, in a sense, this interface between humans that have a qualitative internal goal and systems that are going to be optimizing that goal. And so we can study these languages both from the standpoint of empirical capabilities, but also from the standpoint of risk and safety assessment, of robustness. I guess what I'll say is that assistance games identify both the type of a solution we want when we're building AI systems, and they allow us to characterize different classes of solutions, in particular, from a standpoint of brittleness and forgivingness to mistakes on the person's behalf.

**Daniel Filan:**
Okay. And I guess on the flip side, if I'm a listener really interested in reducing existential risk, what should I not expect assistance games help me out with? How should I limit my ambitions for what assistance games are going to be really good at?

**Dylan Hadfield-Menell:**
I think assistance games are first and foremost, a analytical tool.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
So primarily what they do is allow us to formalize certain value alignment scenarios and assumptions, and study different properties of that.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
That can be done, I think - for near-term systems, it's easy to take these formalisms and build them actually into direct AI systems, but it's not my claim that implementing those systems directly would reduce x-risk. So taking current AI technology and merely saying, "Ah, yes, I am designing my system as the solution to an assistance game of some kind." Will not meaningfully reduce x-risk on its own.

**Daniel Filan:**
Why not?

**Dylan Hadfield-Menell:**
Because I think you will still end up - tracking the types of uncertainty that I think you would want to do for truly advanced systems at the level where they would pose a threat, I think, is beyond the capability of current inference systems.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
So under the presumption that if you try to build an assistance game style solution into a system right now, I'm presuming that, that will consist of some form of Bayesian inference, tracking uncertainty about objectives, and maintaining those estimates in some way, combined with some form of policy optimization. And in fact, I think the types of Bayesian inference that we can do right now are not up to par with systems that would be effective enough to pose a threat.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
Does that make sense?

**Daniel Filan:**
Yeah, I think that makes sense.

**Dylan Hadfield-Menell:**
Now that's not to say ... I think those types of systems will be better for lots of other reasons. I think there are lots of benefits to leveraging uncertainty about objectives with short term systems. And I think that can reduce short-term risks. The primary thing that this can really do for long-term existential risk at the moment is identify areas of research that are valuable. So in this case, this would say we should be investing very heavily in uncertainty estimation, broadly. Specifically uncertainty estimation about utilities and utility learning.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
And I believe that analyzing these theoretical models can help point out directions for the kinds of solutions we will want to move towards with other technologies. I think this can point out things that are risks and things that are likely to fail, and it can shape rough abstract ideas of what a safe interaction with advanced systems would look like. What are desiderata we can aim for? What our targets to shoot for? But it doesn't directly give us a recipe for how to implement those targets.

**Daniel Filan:**
Sure. So in that answer, you alluded to short term benefits that we could get. For ... I don't know if any listeners care at all about what happens in the next hundred years, but if they do, what are some short term, nice things that you imagine implementing solutions to assistance games could provide us?

**Dylan Hadfield-Menell:**
I think one of the big ones is we're very limited with AI systems to optimizing for what we can measure. And so this means there's a large structural bias in the kinds of systems that we build, in that they're either designed from the ground up to measure very specific things, or they end up optimizing for shallow measures of value, because that's the best thing that you have. Introducing assistance games into the way that we think about AI, has the ability to reduce that. So if it becomes easier to start to optimize for more qualitative goals within AI systems, that makes it easier for, say, a system designer at Facebook to make a clear goal of here are the types of content that we want to recommend as a company. It's very complicated to specify that now, but you could imagine work in assistance games, looking at managing an interaction between a company like Facebook and their system would allow them to use much richer goals than the types of behavioral feedback that they have. And you can see inklings of them trying to move in this direction in addition to sort of other large recommender system platforms.

**Dylan Hadfield-Menell:**
The other side of what assistance games in short term systems would give us is that we're building into the analysis of our systems an expectation that there will be ongoing oversight and adaptation. This is something that actually happens in practice. It's only in academic settings that we collect our training data, hit stop, and then run on a special test set. In reality, you have your model, it's being trained on some data stream that's coming in and you're adapting and adjusting in ongoing fashion to changes in your goals as well as changes in the world. If we design systems as solutions to assistance games, we will actually be building that type of a dynamic into the math at a much deeper level. And I think this can expose interfaces for systems that are just much more controllable and understandable.

**Daniel Filan:**
All right. So now that I guess listeners are hyped for all the great things about assistance games, let's get into talking about them a bit more concretely. So the first paper I want to talk about is ["Cooperative Inverse Reinforcement Learning"](https://arxiv.org/abs/1606.03137), CIRL for short, for C-I-R-L. Can you tell us just first of all, what is CIRL, and is it the same thing as an assistance game?

**Dylan Hadfield-Menell:**
So CIRL is not the same thing as an assistance game. It is a subclass of assistance games and it is intended to be the simplest base case of an assistance game. So to that end, we built it as an extension of a Markov decision process, which is a mathematical model for planning used a lot in AI systems. In a Markov decision process, you have a state and you have actions and a reward function. What happens is you take an action in a state, this leads to a new state, and you also get a reward that corresponds to that. From the human's perspective, everything is the same in cooperative IRL.

**Dylan Hadfield-Menell:**
So the human has an observed state. They can take actions, they can accomplish things in the world. What's new is that you also have a robot player that's a second actor in this game. So this is an addition into the MDP. You can think about it as a turn-taking scenario, where the person goes first, then the robot goes second. And from the robot's perspective, there's partial information. So the rewards for each state are a partially observed component, which only the person gets to see. And our goal here was to extend the Markov decision process in the minimal way to account for the fact that there are two players in the world, the human and the robot. And the robot does not know the person's objective.

**Daniel Filan:**
Okay. I guess the name implies that it might be a type of inverse reinforcement learning. Can you say a little bit about what normal inverse reinforcement learning is and why we might want it to be more cooperative?

**Dylan Hadfield-Menell:**
In inverse reinforcement learning what you are doing is solving... well in this case, the opposite of planning really, although there's a joke that inverse reinforcement learning is a slight misnomer. And in fact, if you go back and look up other competing branches, there's a lot of related work under [inverse optimal control](https://asmedigitalcollection.asme.org/fluidsengineering/article-abstract/86/1/51/392203/When-Is-a-Linear-Control-System-Optimal). So what this is to say is that inverse reinforcement learning is essentially doing the opposite of reinforcement learning or planning. So if planning is the problem of taking in a Markov decision process, an MDP, and producing a sequence of actions that get high reward, in inverse reinforcement learning, you observe a sequence of actions and you try to infer the reward function that those actions optimize for. As you can see, this is kind of doing the opposite of planning.

**Dylan Hadfield-Menell:**
In planning, you take in a problem, a reward function, and spit out trajectory, and in inverse reinforcement learning, you take in a trajectory and spit out a reward function. The relationship this has to cooperative IRL is that in cooperative IRL, you are solving, in fact, the same, or at least a very similar problem, which is to say that you see a sequence of actions from the person and from those actions, you need to infer the reward function that generated them. Now, if there's this deep similarity, why do you need something new? Why can't we just use inverse reinforcement learning in order to solve assistance games? The reason is because there are a couple of small changes that inverse reinforcement learning doesn't quite capture, that can be important to represent. One of them, which is kind of a minor tweak is that inverse reinforcement learning is typically formalized as learning the reward function from the person's perspective.

**Dylan Hadfield-Menell:**
So in the standard setup, if you look at a person go, and maybe watch someone go through their morning routine, so they get up, they wash their face, they go get a coffee and so on. If you infer a reward function with inverse reinforcement learning and then naively optimize it, what you end up with is a robot that washes the robot's face and then tries to drink coffee for the robot. In a sense it can naively be formulated as imitation learning, which is only actually one type of solution to an assistance game. And we want to allow for that broader class. So what cooperative IRL does is says, "Okay, no, there are actually two agents in the environment. There's the person and the robot." If you're doing cooperative IRL right, what should happen is that you observe the person's morning routine, and then the next day, while they're washing their face, you're making coffee so that it's ready for them.

**Dylan Hadfield-Menell:**
In this way, you're not imitating their objective as you would in standard inverse reinforcement learning. This is kind of a minor tweak. You wouldn't lose sleep over this issue, but it points towards actually a much deeper issue with inverse reinforcement learning, which is that inverse reinforcement learning assumes that the behavior is generated without the person knowing that the robot is watching them. So it assumes that your immediate rewards, the behaviors you're taking are done purely in service of the rewards that you get within those behaviors. And I think if you think about how you would behave, if you knew that your new robot toy was watching you, I think you can at least agree that you would probably do something different. And in fact, in lots of scenarios, people will adapt their behavior when someone is watching them in order to be more informative, or to in general, try to better accomplish their goals.

**Dylan Hadfield-Menell:**
And this type of a dynamic is actually crucial for the kinds of problems that we're worried about with assistance games, or are worried about in an existential risk context with assistance games. The reason why it's crucial is that as we are building representations of our goals for future increasingly advanced systems to optimize. We are the humans in this assistance game, right? It's not actually cooperative IRL. It's got multiple people, non-stationary rewards, partial information up the wazoo, all kinds of things. But ultimately we want to learn how to solve that game well for us. And we are certainly adapting our behavior to what the system will do eventually. And it's this incentive that I think actually drives a lot of alignment. So inverse reinforcement learning is basically a type of imitation learning. For more advanced systems we don't expect imitation learning to truly be an effective solution.

**Dylan Hadfield-Menell:**
And so what this means is we're going to be relying on a communication solution to an assistance game at some level. And when you study those communication mechanisms and value alignment, or assistance games, they actually only make sense if the person is taking into account the impact of their actions on the future behavior of the AI system. If you look at natural language, for example, as a way to describe your goals, under inverse reinforcement learning, you can't actually describe your goals with natural language. You can just show the system how you would like it to talk.

**Daniel Filan:**
So somehow cooperative IRL is like providing an analytical framework for understanding communication between humans and AI systems. And we think that that kind of communication is going to be really important.

**Dylan Hadfield-Menell:**
And specifically it allows us to formalize the limits of intentional communication of our goals.

**Daniel Filan:**
Okay. What are the limits of intentional communication of our goals? I didn't get this from the paper.

**Dylan Hadfield-Menell:**
Oh, I don't know that this is necessarily in the paper. I think we're now talking - this is probably the class of things which are motivation for the paper, that is not the kind of thing that you could get through peer review in 2015. Sorry, could you repeat the question?

**Daniel Filan:**
We were talking about communication in co-operative inverse reinforcement learning.

**Dylan Hadfield-Menell:**
Oh, yes. Intentional communication of objectives and ... oh, you were talking about what are the limits.

**Daniel Filan:**
The limits, the limits of intentional communication. Oh yes.

**Dylan Hadfield-Menell:**
I mean, we are trying to analyze that type of intentional communication. Intentional communication, there we go. Yes. So naturally they come in through cognitive limits. Is probably one of the biggest ones, so ...

**Daniel Filan:**
On the human part or the robot part?

**Dylan Hadfield-Menell:**
On the human part. So, when we study assistance games, a solution, where "solution" is in air quotes, because it's not really the answer to how do we play an assistance game, but what assistance games really let you do is say if the person plays strategy X and the robot plays strategy Y, how well did they do based off some assumptions about what the environment looks like, and what the types of goals are present. And if you're thinking about building an AI system that can integrate well, and be robust to lots of different people and produce with very, very high likelihood, at least in improvement in utility, one of the things you really have to compensate for is the fact that the human policy is not arbitrary or controllable. It is limited by our biology and our training and upbringings.

**Dylan Hadfield-Menell:**
This means that there's only so much information we can provide about what we want our system to do at any given point. And given that in principle, when you specify a system, you are actually specifying a mapping from all possible observations that system could get, to the correct action to take in response. And to do that, you have to at least think about all of those possibilities in some way. And the fact is that we can't, and we have cognitive limits on how many options we can consider, and even what types of options we can consider in a way. I cannot imagine truly strange unfamiliar situations like that.

**Dylan Hadfield-Menell:**
I have to perhaps experience them, or think hard about experiencing them, or have analogies that I can draw upon them. And all of these are limits on the amount of information I can provide about my goals. Perhaps another way to explain this, and perhaps this is maybe the central way in which assistance games can help reduce existential risk is they allow us to identify that there are really two types of information that play into what a system is going to do. At least in the current paradigm for AI systems. There is both what I will call objective information, which is information about how the world will unfold, perhaps in response to different actions you might take. And that's separate from normative information, which is information about how to value those different possibilities, and what assistance games do primarily, is they're a decision problem where there is a formal representation of a limited normative channel. And I think the limits on that normative channel, need to be accounted for and balanced against certain types of risk. And that risk could be how quickly you change the world, or - in a sense, you need to regularize certain aspects of your behavior by the amount of information that you have about goals.

**Daniel Filan:**
Okay. So speaking of the information about goals. So I think when people in the AI alignment space are thinking about learning normativity, they're sometimes thinking of different things. So, one thing they could be thinking about is learning the specification of a kind of specific task. For instance, I want to build a really good CPU or something, where it's complicated, but ultimately it's about one CPU as opposed to getting an AI that just learns everything I care about in my life. And it's just going to make the world perfect for me. Which of these do you think CIRL is aimed at analyzing or could it be equally good at both?

**Dylan Hadfield-Menell:**
Actually, I think it's both.

**Daniel Filan:**
All right.

**Dylan Hadfield-Menell:**
So these are very different in practice, right? The things that I need to specify for a utility function to be good for getting a CPU, to do things that I want. Actually quite non-trivial and might bring in to bear complicated... If we wanted to, I can tell a story where you need to optimize CPUs to account for environmental possibilities because they're going to be used in Bitcoin mining. I'll say things can get more complicated than a simple first-pass analysis might suggest.

**Daniel Filan:**
I do think people don't use CPUs much in Bitcoin mining anymore.

**Dylan Hadfield-Menell:**
Okay. Yes, that is a very good point.

**Daniel Filan:**
Alas, but they are used for, I think for other cryptocurrencies. CPUs are still on the cutting edge of mining.

**Dylan Hadfield-Menell:**
Yes. I more meant to say that actually one of the issues, one of the sort of structural issues in building AI systems is the presumption of narrowness based off of relatively simple analysis. And I would say that even CPU design can get far more complicated than even graduate-level education would lead you to imagine. But putting that aside, the main difference for both of these is really not what the person's goals are. It has much more to do with what the robot is capable of doing and what it's likely to be doing, and what environment it is likely to be in. So, in cooperative IRL, you would formalize these differences not by a change in the person's reward function within the model. You would formalize these via a change in the environment that the robot is acting in and maybe some small things about the prior structure, right?

**Dylan Hadfield-Menell:**
So, the "learn about everything" is the simpler model because you have just a really broad set of environments the system could be acting in. And so through interactions with the person, you're going to focus on trying to reduce uncertainty about objectives, overall. And there are some interesting ideas on this. If people want to look at that kind of idea more, I would go look at [Justin Fu's work on adversarial IRL](https://scholar.google.com/citations?user=T9To2C0AAAAJ&hl=en) which captures some of these ideas for learning generalizable reward functions. Now, let's talk about learning a much more narrow task and what that would look like in an assistance game. In this case, it's probably modeled by a robot that has an action set that's primarily related to things you can do for computer circuit design, for CPU design. A prior over reward functions that the person cares a lot about CPUs in some way.

**Dylan Hadfield-Menell:**
So maybe there are 10 different metrics that we can say are relevant for CPU use, or we can identify some grouping of features of the world that are relevant to that environment. And say that there's a high probability that those features matter in the utility function. And then I think the optimal thing for the robot to do in this case will be to learn about this very specific task. And it won't - even though in principle, it could be learning about the person's norms and kind of everything about preferences. It's really the structure of where it will be deployed that leads to narrow learning. And I think one of the things that's nice about assistance games is that that solution falls out from the definition of this game as the correct thing to do. Rather than a sort of more ad hoc reasoning of, this is a narrow task versus not. This is just that because the system's incentives come from doing things that are useful. Its information gathering behavior is targeted to relevant information based on its capabilities.

**Daniel Filan:**
So yeah, this gets into a few questions I have about in particular the CIRL analysis. So in the CIRL analysis, part of the setup is that there is a prior over human forward functions, or utility functions. Yeah, you mentioned this a little bit. But how should I think about what the right prior is? Because, that presumably, that's going to pretty heavily influence the analysis.

**Dylan Hadfield-Menell:**
Yes, absolutely.

**Daniel Filan:**
Oh, and I should say that by prior, I mean a prior probability distribution. So, your distribution over what the human's reward function is before you know anything.

**Dylan Hadfield-Menell:**
Right. So I think it depends on what type of a situation you're analyzing and what you're looking for. I think if you're in a setting where you're providing a product to a large population or user base or something like this, then you would think about that prior as being something that you'd fit in a more data-driven way. If you're doing analysis of looking at worst-case analysis against AI systems, then I would say you probably want to have as broad of a set of possible utilities as you could have, and basically, look for systems that are effective against that overall slate. I'm interested in trying to think about what interesting and general priors would be and kind of the right - yeah, how you could look at developing priors in a more rigorous way.

**Dylan Hadfield-Menell:**
I would say that this is something that's missing from a lot of the current analysis on assistance games. There are sort of practical questions on if you're using assistance games or cooperative AIRL as a design template for systems, how could you learn priors? I think that's a bit more clear. For analysis, for things going forward. You'd like to talk about how priors are shaped by environments. So [Rohin](https://rohinshah.com/)'s [paper](https://arxiv.org/abs/1902.04198), where he looked at identifying prior information about reward functions based on the assumption that the world state, when the system was turned on, was already optimized captures some of these ideas. And I think it's really interesting to explore that direction whereby we can try to think about what types of - given certain sets of evolutionary pressures, what types of reward functions are we likely to see? And I think that's a really complicated - that's probably a couple of PhD theses worth of research, if not more. But that's a bit of how I think about this prior question.

**Dylan Hadfield-Menell:**
If I can also add on top of that, there's sort of a question of what is your prior? What is the distribution over the set of reward functions you're considering? I've been thinking a lot recently about the ways that actually, part of the question is actually just determining a good version of that set. And very specifically within AI systems, a lot of failure modes that we think about in theory, for x-risk scenarios, and that I would argue we have observed in practice, stem from missing features in particular. And so, a lot of the question of how do you come up with the right prior is partially about how do you come up with the right features, is sort of the refinement of that question that I think is a bit more, I don't want to say more interesting. But it feels more specific at least.

**Daniel Filan:**
Yeah. It's very - because normally in Bayesian inference, right, you can recover from your prior being off by a factor of two, but you can't really recover from the true hypothesis not being the support. Defining the support is where it's at.

**Dylan Hadfield-Menell:**
Yeah. Missing features - and in practice that that means missing features for the system. If we go back to the Facebook situation, right? It's sort of like, they have features for how much you're engaging with the website, but they don't have features for how much you regret engaging with it. And there are ways that they could try to identify that. But there's actually a whole process for, if you think about the sequence whereby you would deploy a system and then integrate that feature in, that's really the gap that our current systems fail at, in a big way. It's both, you absolutely are going to run into unintended consequences, and it takes a long time to discover those unintended consequences. And it takes a long time to integrate proxies for or measurements of those consequences into the system. Whether in practice that happens at an organizational level or at a direct, rewrite its objective kind of level. And so, value alignment problems and assistance games where you look at mechanisms for identifying new qualitative features of utility are something I've been thinking a lot about recently.

**Daniel Filan:**
Am I right? That you have a paper that you co-authored very recently about this?

**Dylan Hadfield-Menell:**
Yes. So that was, we looked at recommender systems specifically, it was called - the subtitle was "Aligning Recommender Systems with Human Values". Yes, we called it, ["What Are You Optimizing For? Aligning Recommender Systems with Human Values"](https://participatoryml.github.io/papers/2020/42.pdf). And what this presented was an alignment perspective on recommender systems and did our best to document what actually is quite interesting, as the existing public information about attempts companies have taken to better align their systems with underserved goals. So, it turns out that companies are making these changes and they are doing some of these things. And I think the value of assistance games is giving us a category to identify these types of interventions and move them from features of practice to objects of study.

**Daniel Filan:**
So going back to the CIRL formalism. Part of the formalism, right, is that there're some parameter specifying, what reward function, out of all the possible functions the human actually has. And at the start of the game, the human observes this. I'm wondering how realistic you think this assumption is and how, if it is realistic then great. And if it's not realistic, then how bad you think it is?

**Dylan Hadfield-Menell:**
Like all models, it's wrong but perhaps useful. I think it depends on what you imagine that theta to capture. Where theta is the variable that we represent the reward function with. I think that there is a way to set up this type of analysis where that theta represents kind of a welfare function sort of in the sense of a moral philosophy kind of sense. In that case, the connection between that and human behavior might be incredibly complicated. But we could imagine that there is this static component that describes, what we really would want our system to do in general overall. And I think that's a reasonable use of the model.

**Dylan Hadfield-Menell:**
On the other hand, that leads to a lot of complexity in the policy. And perhaps the statement that the human observes theta at the start is no longer a reasonable one. And so, one of the assumptions in cooperative IRL that we've relaxed is looking at this particular one where the fact that the person has complete knowledge of the objective, seems perhaps fishy. Especially if it's a static, unchanging objective. The other way to see it is theta's sort of more how you're feeling a bit day to day, in which case the staticness assumption is probably, well, is just wrong. But the behavioral assumptions that we typically make are a bit more reasonable.

**Daniel Filan:**
And if people are interested in reading that follow-up work. What kinds of papers should they read?

**Dylan Hadfield-Menell:**
So that is referring to a paper that Lawrence Chan was the first author on. That's called [the Assistive Multi-Armed Bandit](https://arxiv.org/abs/1901.08654). And in this case, we look at an assistance game where the person doesn't observe theta at the start of the game. But in fact, they learn about it over time through reward signals. So the kind of simple idea of it is in cooperative IRL, let's say you might be making someone tea or coffee. You would see them choose which one they want and then in the future know which one to make for them. But if there's someone who's an alien, that's just come down from outer space or someone from a society that doesn't have tea or coffee or a child or something like that, that would be the wrong kind of solution. And it would be very bad to assume that the thing the person chose is actually what they want. Because we know that they have to learn and experience before their behavior is indicative of their goals.

**Dylan Hadfield-Menell:**
And so the assistive multi-armed bandit formalizes that kind of a scenario where solutions look like people try out several options, learn what they like, and then the system eventually learns from that. And there's some really interesting things we identify about ways that the system can actually help you learn before learning what you want. Because actually at the start of the game, the system and the robot have similar information but different computation abilities.

**Daniel Filan:**
And interestingly enough, that set-up does seem closer to the inverse of reinforcement learning where you're learning how to optimize a reward.

**Dylan Hadfield-Menell:**
Yes, actually. Yes. You can still have - to truly do inverse reinforcement learning, you don't want the cooperative scenario, but yes. It is certainly - the inference problem we are solving is in fact, real inverse reinforcement learning, if I could be so bold. And if you're listening to this podcast, Stuart, I'm sorry.

**Daniel Filan:**
Yeah, maybe I should say that in every episode. Stuart Russell is Dylan's and mine advisor. So, going back to the cooperative inverse reinforcement learning, so it's a game, right? Games, they have equilibria like Nash equilibria, or maybe other types of equilibria. Should we expect - in our analysis, should we focus on the equilibria of these games and expect humans to land in one of those? Or should we fix a relatively simple human policy or, yeah. How should I think about equilibrium analysis in CIRL?

**Dylan Hadfield-Menell:**
Carefully.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
I think the primary value it has is in identifying limits on communication ability for normative information. So if you look at a model of a cooperative IRL game and you compute the optimal human-robot configuration, and there are still aspects of the human's objective that the system doesn't know, this gives you information about what types of value alignment problems cannot be solved. Where you need to have more interaction or more information before deploying to that kind of setting.

**Dylan Hadfield-Menell:**
I think - there are practical short-term applications where the equilibrium assumptions I think are valid. Well, actually, I don't want to say totally valid, but where people do develop towards best responses for the systems that they're using. So, I know some people for recommender systems actually think fairly intentionally about, I don't want to click on this because if I do, the system will show me more things like that. For me, as someone who's built a lot of robots and trained them to do things, I can provide information that's better in some ways. Just because I sort of know what types of mistakes the inference is likely to make. So I think there's limited value in this. And in general and understandings, what are the ways that people might adapt to exploit components of your learning algorithm? Ideally in positive ways, but once you have non-cooperative settings that also starts to be relevant.

**Daniel Filan:**
If you just think about the original CIRL game, if you have a wide enough action space, that includes the human being able to type on a keyboard, it seems at least one of the equilibria is going to be the human just writes down their reward function. And then sits back, and the robot, does everything from there on, right? Which is kind of scary.

**Dylan Hadfield-Menell:**
Yes. Well, there are different types of - that is an example of what I would call a communication equilibrium, right? Where you are encoding your preferences into a language or into symbols where all of those symbols are unit cost from your perspective. And what this means is that you can have incredibly high normative bandwidth actually, right? Your limits on your ability to communicate your preferences are like the [Huffman coding](https://en.wikipedia.org/wiki/Huffman_coding), given the appropriate prior or what have you. But at the same time, it's an incredibly brittle solution. So, if in a communication setting you have your encoder in this case, the person's functioning as the encoder taking preferences and encoding them into actions, in this case, these symbols, and then you have a decoder which is the robot, and it's taking these symbols and below it, reinflating them back into objectives and rewards.

**Dylan Hadfield-Menell:**
And if those are mismatched, you have no bounds on the performance of the resulting robot policy. And in fact, there are small changes in many encoding schemes that would lead to the system actually minimizing utility or something like that. One way to think about this is inverse - So we talked about equilibria, right? I think I want to step back from equilibria and say that the things that are interesting, is different types of strategy pairs. They may or may not be in equilibrium. Now, let's say we're comparing two different equilibria, and these are two, sorry, two different strategy pairs. And one is the strategy pair that inverse reinforcement learning, that imitation learning corresponds to, which is, person does a thing and then the robot imitates. You can still communicate a lot of information about goals there. There are some limits in how much you can communicate, but it's also an incredibly robust communication mechanism, right?

**Dylan Hadfield-Menell:**
Where it relies on the assumption that people can do things like what they want, which is not a crazy assumption about people. And explicitly we accept that they may make mistakes along the way there. And to the extent that they make mistakes, those mistakes can be related to how bad the things are. And so this gives you, at least in theory, some bounds on how bad you can be when you're optimizing for the objective that you learned through that type of a mechanism. On the other hand, if we look at these types of more communication equilibria, where let's say, the robot is observing - let's say, we're doing 2D navigation. So, you're moving around. You're in a maze of some kind. Certain maze squares are good. Certain maze squares are bad. And the robot's going to observe the person take some actions, and then is going to act in a similar maze.

**Dylan Hadfield-Menell:**
The imitation learning style version of this problem is very simple to explain, right? Person just says, "Okay, let me figure out how to solve the maze. Do my best." The robot will try to copy that. And if we're doing IRL with the right kind of prior and things like that, the robot can actually learn "Here are some features of what the person was seeking out" and perhaps improve on the person's performance. Now, another class of solutions that could happen here, are to assume first off, let's assume the person doesn't care about the training scenario. So, the person's going to be acting in this maze. But the only thing that they - the maze actually, all the actions are the same cost. And in many cases, this is true, right? The person is taking actions for the purpose of generating information that the system can then use to go do useful things with.

**Dylan Hadfield-Menell:**
So another type of solution is to come up with an encoding where you just discretize the reward space and assign a reward to each square. And what the person does is just, moves to that square on each episode. That will have limits based off, of how good you can discretize the space but you can continue to do this where the person on successive episodes, rather than trying to treat this like a maze, just forgets that it's a maze and just says, "Ah, I'm just going to use this to come up with an encoding of my reward function."

**Daniel Filan:**
Right. Sorry. Should I be thinking about, on the first time-step they go to the best square, and on the second time-step, they go to the second best square? How's this encoding working?

**Dylan Hadfield-Menell:**
So, the point is that the encoding can be arbitrary.

**Daniel Filan:**
I should imagine something like that.

**Dylan Hadfield-Menell:**
So it's, yeah. And I guess the fact that there are lots of parallel solutions here, right? Say there's only a maze with three squares. I could then, I start at one place. I can stay here. I can go, right. Or I can go left. I can use that to encode with my actions, arbitrarily complicated things.

**Daniel Filan:**
Just in binary.

**Dylan Hadfield-Menell:**
Just in binary. Exactly. But, there are lots and lots of different ways to encode my preferences into binary. And so now, I've taken a preference specification problem and turned it into a preference specification specification problem.

**Daniel Filan:**
Nice. Or terrible.

**Dylan Hadfield-Menell:**
And it's not clear that I am much better off.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
From a risky standpoint. On the other side, if you do align with the system on this communication strategy, you can provide far more information.

**Daniel Filan:**
Yup.

**Dylan Hadfield-Menell:**
Right, you can provide effectively arbitrary information. The only limit is that you be able to encode it somehow. And so I think this is kind of a really big tension between robustness, how much can the system adapt to our behavior? And that's based off of what types of assumptions about the details of our behavior is it making. Versus at another spectrum, there are these kind of purely communication settings where the encoding scheme you choose is kind of arbitrary from some sense. And this means you can get incredibly informative codes, but brittle ones perhaps.

**Daniel Filan:**
Okay. So, now, I'd like to move to the next paper. So this is [The Off-Switch Game](https://arxiv.org/abs/1611.08219). Can you tell us, can you summarize this paper, first of all?

**Dylan Hadfield-Menell:**
Sure. The idea behind this paper, it really came from two places. One, we were inspired by [Soares et al's paper on corrigibility](https://openreview.net/forum?id=H1bIT1buWH). And we were looking to translate some of those ideas into an assistance game style framework. And the other side of it was that at this point, my advisor, Stuart Russell had been going around and talking about existential risk within the AI community, a fair amount. And he was discovering that this point about the ability to turn off a system was coming up with some frequency in his discussions. And what felt to us like a relatively clear observation about incentives was not being recognized.

**Dylan Hadfield-Menell:**
And so part of the idea behind the paper was to show, sort of very clearly, that once you are given a task that creates incentives to accomplish the task, and that will include accomplishing other things that enable maintaining the ability to accomplish that task. Which is relatively simple to say in English, relatively intuitive. And frankly not that hard to show with math. And the Off-Switch Game then is a cooperative IRL formulation of an interaction where there's, as before, a human and a robot and the robot starts off.

**Dylan Hadfield-Menell:**
We assume that it already has prior information about what the utility function is. The robot makes a choice between one of three different actions. It has a candidate action that it can execute. So we can go ahead and directly execute it. It can choose, "no, that action's a bad idea" and just turn off or it can say to the person, "Hey, I'd like to take this action. Do you think that's a good idea?" And then the person can take a binary action, which is to either say yes, and then allow the action to execute or to say no, and turn the robot off. And we analyze this under scenarios where the robot assumes that it knows the person's objective and scenarios where it has uncertainty about that.

**Daniel Filan:**
Okay. I think a lot of people who are thinking about existential risk from AI for the first time, think "okay, why doesn't the human just turn it off? Why does the robot need to present an option?"

**Dylan Hadfield-Menell:**
Because our goal here is to understand the system's incentive to bypass oversight ultimately. And so the setup for the game is such that the system has the ability to bypass the human entirely, and so we wanted to model that component. And so that's why you have this a little bit weird thing of the robot choosing to allow the person to turn it off.

**Daniel Filan:**
Okay. And I guess this models in real life, if I'm really smart and I don't want you to turn me off, I imagine I can keep you away from the button or something?

**Dylan Hadfield-Menell:**
Yes, it captures that. I would say it's - the way that I like to think about this really is we want to understand what is the incentive for oversight. And so in order to do that, you have to have an option for no oversight, and the model attempts to do that in the simplest way we could think of.

**Daniel Filan:**
All right. I think we haven't said it yet, when do robots have incentives to allow themselves to be subject to oversight?

**Dylan Hadfield-Menell:**
When you have two factors at play, one is uncertainty about how to evaluate its actions. So, in this case, uncertainty about the utility evaluations for states, combined with the knowledge that the person's behavior reveals information about that uncertainty. So the key thing in this model that creates an incentive for oversight is that the off event is correlated with the action that you were about to take being bad, or at least worse than turning off. And it's the fact that you don't know whether that action is good combined with this correlation that creates the incentive to give the person this capability.

**Daniel Filan:**
All right. So one question I have about this paper is it uses an assistance game style analysis, but it's very lightweight, right? It's only for one round, you don't have to do... The CIRL paper has much more math than this paper. In general, how often do you think these kind of pared down versions of assistance games are going to be useful for analysis and how often do you think we're going to need the full machinery of assistance games?

**Dylan Hadfield-Menell:**
I think they're both really useful actually. So I think that they just serve different purposes. So this off switch game analysis is really good for, very abstractly when we're designing systems, what are the features that I should be looking to? What should I be paying attention to? But as you add complications to the model there, you can also start to ask more nuanced questions. And so the off switch game is a one-round interaction. [With Smitha Milli, I came back and looked at an extension of this, that we called the obedience game](https://arxiv.org/abs/1705.09990), where it's effectively a multi-round off switch game. And, in this case, well, what we ended up showing there is actually some of the issues with missing features in systems, but it allows us to identify the dynamics of this learning environment, which are that over time, uncertainty about the objective goes down.

**Dylan Hadfield-Menell:**
And so that leads to actually some pretty structured dynamics in how you expect things to behave early on versus later on. And then if you know you're actually being involved in this game, you actually might start to take different actions early on to communicate more information for later rounds. And all of these I think are interesting facets that you want to be able to complicate the model towards, in effect. So I think these really simple, short, clear at the level where you could explain it to your non-technical friends, like that's where the off switch game has a lot of power. It's also really useful to be able to add other things to that model, in particular, to look at the sequential dynamics and how things change over time.

**Daniel Filan:**
All right. So again, talking about the sort of the structure of the paper or something, it seems part of why you wrote it was to convince a bunch of people of a point. Did it work?

**Dylan Hadfield-Menell:**
Not really, but I think not because the paper didn't make an effective case, but more so because the people that we were arguing with moved on to other focus. So in 2015, AI safety and concerns about existential risks were very poorly known within AI research circles and over time, and arguably as a result of this paper and things like it, that has changed to the point where this isn't as much of a taboo subject. And at the same time, I think a lot of the people we were arguing with were motivated by desires to maintain research funding by avoiding another AI research winter, which is something where a lot of AI funding got cut because we couldn't live up to the hype from the '80s.

**Dylan Hadfield-Menell:**
And other folks who are generally concerned with maintaining the ability to design and build the AI systems that they're excited about and driving this research forward. And if those are your goals, you aren't focusing on the x-risk community these days, because there are a lot of other people pointing out the current problems with these systems. And I think that tends to be the focus of those folks for the most part.

**Daniel Filan:**
Okay. So we've kind of given, I feel comfortable saying our side of that discourse. If people are interested in what it's from the other side, which people are we talking about that our viewers can try and hear from them their take?

**Dylan Hadfield-Menell:**
Yes. I think you could look at - Rodney Brooks wrote [a long blog post in response to the off switch game in effect](https://rodneybrooks.com/the-seven-deadly-sins-of-predicting-the-future-of-ai/). And that might be a good place to look at for a sort of more well-thought-out critique. And I think the other place to look at it would be [Stuart](http://people.eecs.berkeley.edu/~russell/) was involved in several debates back in 2014, 2015. And I think the best thing to do would be to try to find those, although, I don't have any references for them.

**Daniel Filan:**
We'll try to link at least the blog post in the episode description and in the transcript. Let's get back to the paper itself. At the outset, we sort of talked about assistance games as an analysis framework, right? But the paper has a section where it talks about manually modifying the robot's uncertainty over what the human reward function is basically because the more uncertain the robot is, the more it's going to defer to humans. That seems kind of in tention with the pure analysis frame, at least to me, I'm wondering how does that tension resolve?

**Dylan Hadfield-Menell:**
Right. I think that was a bit of foreshadowing in a way for where we were going to be heading with [the obedience paper](https://arxiv.org/abs/1705.09990) that we did next. But I think the way that I was looking at it was really trying to bring in some degree of strategic uncertainty. So within that analysis, we assume that the robot kind of knew beta for what the person was doing. In this case, beta was the parameter of how rational is the person. We had it set so that I believe beta equals zero corresponded to a rational person, and as it increased, the person would make more and more errors. And all of our results where we analyzed this trade-off between beta effectively and the robot's uncertainty about the utility, what we identified is that there's a trade off between those which determines ultimately whether or not the robot chooses to interact with the person or take its best guess and sort of see that out.

**Dylan Hadfield-Menell:**
And so what we wanted to look at was a scenario where you're imagining building a system that is going to interact with someone. And as the designer from this perspective or you could think about this from the robot's perspective, in that case, you don't really know what beta is. And so what we were doing was looking at, "Okay, well, if I have to guess what beta is, is it better to guess too smart or too dumb? And what are the different types of errors that you identify from that?" And I believe what we showed is that if there are - in effect, if you run this and guess many times, if you tend to overestimate beta, think the person is dumber than they are, you lose utility, but you do better from a worst case standpoint to overestimating it and thinking the person is smarter than they are. Or do I have that wrong right now? I'll be honest, I'd have to go back and re-look at the section to go and see-

**Daniel Filan:**
The noisier, the human is the less likely you are to defer, so I think that means-

**Dylan Hadfield-Menell:**
Right. So it's better to assume the person is smarter than they are. Well, it's-

**Daniel Filan:**
Safer somehow.

**Dylan Hadfield-Menell:**
It's non-trivial how to do - you can't work it out super easily because what's going on is there are scenarios where you should actually not give the person oversight and there are scenarios where you shouldn't. And if you think the person is more capable than they are then you can make mistakes that will lead to a higher cost for them if they screw up by giving control.

**Daniel Filan:**
Perhaps this is motivation for listeners to read the paper and resolve this mystery.

**Dylan Hadfield-Menell:**
Yes, very much so.

**Daniel Filan:**
So kind of related to that, I think one thing that the paper suggests is a model of what you might call calculated deference. So you have an AI system and if it implemented a solver for this game, what it would kind of do is say, "Okay, the human's just trying to shut me off, how much do I believe that this is really informative versus how sure am I that the thing I want to do is really the right thing to do?" And to me at least, there's something kind of - I get a bit nervous about that, right? It seems it might be kind of brittle to an AI system being wrong about what I want or about how my actions are related to my preferences or something.

**Daniel Filan:**
And I get worried about mistakes that can't be undone and I'd hope instead for some kind of uncalculated deference where the robot just does what I want even if it thinks it's a bad idea, but somehow I'm not exactly sure how is otherwise rational and reasons well. I'm wondering if you have comments about this difference in what your analysis might say about less calculated deference?

**Dylan Hadfield-Menell:**
Yeah. So I think if we pop back up from off switch game to the more general cooperative IRL perspective, off switch characterizes a class of solutions to cooperative IRL games. At one point, I tried to be somewhat formal about this, but there's a clearly identified signal in the environment and there are some properties of the robot behavior and the human behavior such that you have the ability to access it at all times and there's a certain robot behavior that follows on from sending the signal. So the question that cooperative IRL would allow us to look at is based off certain assumptions about the environment, the space of possible utility functions and the limits on human behavior, is it a good idea or not to implement a strict off switch solution, a soft off switch solution or a no off switch solution.

**Dylan Hadfield-Menell:**
And I think that different scenarios will call for different settings of that in general. So to talk sort of more about this idea in particular for this type of calculated deference versus uncalculated deference. Part of what I'm saying is you should probably include a link to [the obedience game paper](https://arxiv.org/abs/1705.09990) in the description as well, because I think that paper was actually intended to get at this question specifically. So what we did in that paper was we looked at effectively a repeated off switch game where what happens within each round is the person sees a set of, in this case, we had 10 different actions and tells the robot which action they would like it to take. And now the robot observes this and can do any action that it wants to. And what we can compare is the performance of a purely obedient policy which would be your uncalculated deference with calculated deference.

**Dylan Hadfield-Menell:**
And if you set this up, we just assume some prior distributions over different features for actions and weights for utility functions, nothing very fancy. And you see a fairly predictable result, which is according to this model, calculated deference is better than uncalculated deference because you have more utility. And that, I think if you understand the mathematics of these problems, very, very clear why that happens, right? It's you're comparing the optimal policy with something that is possibly not the optimal policy. And in fact, if you assume that the person makes mistakes, it is guaranteed to not be the optimal policy. So you look at this, and as researchers, we looked at this and [inaudible 01:12:52] a bit of attention, right? Off switch or the ability to turn a system off is a specification of desired behavior, right? And we seem to think that we want that.

**Dylan Hadfield-Menell:**
And there's a real question about why would you want that? And from this analysis, it seems you certainly wouldn't. And I can definitely create scenarios like a self-driving car where you don't want it to wait for interaction because your effective beta as you page back in and context switch and start figuring out how to drive a car will cause you to be worse than whatever its best guess would be. So we wanted to try to understand this and formalize a bit of where this intuitive notion comes from. And to do that, we investigated this game from the context of missing features. And what we showed is that in the presence of missing features where you have components of your... So to be a bit more specific, we looked at variants of this problem where at one end of the spectrum, we hid almost all of the features that determine utility from the system. And at the other end of the spectrum, we introduced a bunch of distractor features.

**Dylan Hadfield-Menell:**
And what we showed was that if you're missing features, this optimization strategy goes haywire. If you're missing enough, it can do crazy things like disobey the person on the first around. Because you might pick - from the robot's perspective, you might select a dominated action. And in general, you sort of never really recover from this optimization strategy. If you have distractor features, it's fine because they're noise and you eventually rule them out. But if you're missing features and you try to optimize, you can end up just being confidently wrong. And so the way that I have come to understand this calculated deference versus uncalculated deference, the difference sort of comes down to how good a job you think you've done about identifying the relevant features for this decision.

**Dylan Hadfield-Menell:**
And if you think you've done a good job, presumably the self-driving car scenario roughly fits this, then calculated deference is what you want. And if you haven't done a good job or if you're in a scenario where you're much less clear then you want to, at least for a long initial period, have that kind of just general deference as the optimal strategy. And this falls out as the optimal strategy, right? Because if you want to do inference in a world where the system can learn about the missing features, that means it's hypothesis space of possible reward functions is necessarily much larger and so you have to provide more information in order for the system to be useful.

**Daniel Filan:**
It seems like this suggests a problem, suppose I'm an AI and I have this a human overseer, and sometimes the human overseer does things that seem irrational, right? I can't make heads or tails of how this could possibly be rational. One way this kind of thing can happen is that the human just is kind of irrational. Another way this could happen is that I'm the one who's irrational, and the human knows some things, some features of the environment or something, that I don't know. And therefore the human is taking what looks like a dominated option, but actually it's really good on this axis that I just can't observe and that's why it's trading off these things that I can tell are clearly good. Is there some way - it seems you would ideally want to be able to distinguish these two situations.

**Dylan Hadfield-Menell:**
Yeah, you would like to be able to distinguish those situations, and I think-

**Daniel Filan:**
Is it possible though?

**Dylan Hadfield-Menell:**
At least in some cases, no, it comes down to kind of your joint prior over possible utility functions for the person and possible meta strategies where a strategy maps the person's preferences into their behavior. And what you're sort of saying is, well, I'm seeing a person behave and my best estimate of a utility function describes that as bad, so am I missing something? And well, it's possible you're missing something in the sense of there are observable properties of the world, which have mutual information with the person's future actions and you could learn that certainly, but whether those are rewards or not is fundamentally kind of arbitrary, at least, is arbitrary at the level of abstraction that we're talking about.

**Daniel Filan:**
Yeah, if you could only determine behavior, you can kind of pack things into policies or reward functions, and...

**Dylan Hadfield-Menell:**
Exactly. And this makes identifying when they're... I think in general, this calls back to that point that I made about the difference between objective information about the world, objective, in the sense of sort of true and identifiable. Let's say there's some certain pattern of brain spikes, neuron spikes that really predicts my behavior in the future, are those spikes - the causal relationship could be determined. But whether those are good or bad, or whether those are evidence of me thinking about the consequences of my actions and then planning a sequence of things in order to accomplish some internal representation of, I don't know, maybe those spikes are anticipation of ice cream, and so they are representation of my goal, at least locally in that sense. Or it could be that those spikes are just the trigger for some tic that I have, that I don't really enjoy and I don't have the ability to stop. And from an observational standpoint, unless you make some type of normative assumptions, which is to say, unless you bring some additional source of normative information to bear, distinguishing those two based on observations, isn't really possible.

**Daniel Filan:**
Yeah, it's a tricky situation.

**Dylan Hadfield-Menell:**
Yeah. I tend to think that this is a really core component of alignment challenges within AI, and I think there's an additional feature which I'll add, which is that there are, I believe some unavoidable costs to normative information in the sense that if normative information is generated by people choosing to invoke potentially cognitively costly routines to think about what they want and what type of person they want to be and what type of world they want to live in, then that means that actually there are limits to the amount of information that you should get in say a cooperative IRL, optimal equilibrium, right? In the sense that the solution does not involve fully identifying the person's utility function even if that's possible because there are unavoidable costs in them running their brain to generate that information in effect.

**Dylan Hadfield-Menell:**
And those costs could be direct time costs, but also indirect psychological costs. That idea of normative information and fundamental limits on it. And I think, a gut feeling that there are limits on the amount of normative information we can provide is a lot of what drives our concerns about existential risk, is that we expect that there is this imbalance.

**Daniel Filan:**
I think there probably are interesting questions I could ask about that, but I can't think of any. So I'm going to plow straight ahead.

**Dylan Hadfield-Menell:**
Yes, please do.

**Daniel Filan:**
So to wrap up the section a little bit, I think one thing you mentioned as an inspiration for this paper was [the Soares et al. paper on corrigibility](https://openreview.net/forum?id=H1bIT1buWH), and corrigibility is this term that gets used in the AI alignment space, particularly I think [Paul Christiano talks about it a lot](https://ai-alignment.com/corrigibility-3039e668638). What do you think the relationship is between this paper and corrigibility as it's sort of thought of in these spaces?

**Dylan Hadfield-Menell:**
I think they are very similar models of the world that operate on different assumptions about agent beliefs. So corrigibility, the kind of primary difference that I see between that and off switch game is that it doesn't include a reference to a human agent in the environment. And this makes a very big difference because that's where we get our source of potential information about utility. I think one of the key assumptions in their model is that the belief structure of the agent is such that it cannot acquire more information about utility.

**Daniel Filan:**
So you're thinking of the Soares et al. paper, is that right?

**Dylan Hadfield-Menell:**
Yes I'm thinking of the Soares et al. paper there, where the primary result they get is the only way to get a system to choose this type of oversight is through a type of indifference between utilities, which is the solution you get under the assumption that utilities are fully observed or that all information about utility has already been collected. And so which one of these results you sort of take into systems of the future depends on what you think the belief structures of those systems will be, and their relationship with incentives will be.

**Daniel Filan:**
I mean, if you model systems as learning about the reward function from human behavior, at some point it's learned all that it can learn from human behavior, right? At some point you use up all the bits and human behavior is no longer - is now basically probabilistically independent of your posterior on the reward function. So there it seems you do end up in this Soares et al. world, is that fair to say?

**Dylan Hadfield-Menell:**
I mean, if you include the possibility for drift in preferences, for example, then that's not necessarily true. What we're assuming about these agents actually is quite unclear, if I'm being fully honest. We're making some different set of assumptions about what's possible in the belief structures. Arguably if you had set up the inference correctly and you truly reduce the uncertainty then you do actually not need an off switch. And I think if your modeling assumptions are such that you've reached that point and you model a system as behaving optimally, given that information, then the behavior you'll get is quite clear. I tend to think that a lot of these results point out that creating systems that have limiting behavior as fully rational policies is probably a mistake.

**Daniel Filan:**
But it's hard not to do that, isn't it?

**Dylan Hadfield-Menell:**
I think it depends on... One of the most effective things that we've discovered in the field of artificial intelligence has been mathematics of goals and practical computational routines for goal achievement. So I think that shapes a lot of what we think about for AI, but I don't think that's actually where we have to head in the long run. And I think one of the things we're learning is that actually goal management systems are perhaps as crucial to our qualitative notions of intelligence as goal achievement. I think that we have a strong bias to focus on rational behavior and goal achievement as the definition of intelligence. When I think it's sort of more like goal achievement was the part of intelligence that we were worst at in the '60s and '70s. And as we've developed good computational mechanisms for that, I think that we will step away from trying to build systems as a fully optimal Bayesian agent. I think in many cases, you're already seeing a lot of systems move away from their design, perhaps composed of agents in some ways, but GANs are not Bayesian reasoners.

**Daniel Filan:**
What are GANs?

**Dylan Hadfield-Menell:**
GANs are generative adversarial networks. They are a way for modeling let's say images where let's say you want to create a function that generates images that looks like natural images or a data set of images. What GANs do is define a problem where one agent is trying to generate images, and then another agent is trying to determine if that generated image or a real image from the dataset, of those two, which one is real. And this creates a type of adaptive loss function for the generator that can lead to very effective image modeling and very photo realistic image generations.

**Dylan Hadfield-Menell:**
And as we move in these directions for these types of adversarial learning approaches, or look at learning approaches that are robust to things like data poisoning and other real-world complications that come in from deploying these systems. I think we're actually getting further from our system looking like a direct Bayesian agent, in a way and more towards... Goal achievement will be a part of artificial intelligence going forward, right? It's not like that part will go away, but I think if you're looking towards the future, or at least a future where we have successfully built aligned and safe AI systems. I think, as much, if not more, of the artificial cognitive architecture is about goal management as it is goal achievement.

**Daniel Filan:**
So I have a final question about the off-switch game. This is partially a question, but it's actually mostly a beg for listeners to work out this problem that I can't figure out. But maybe you can figure it out. So, if you think about the off-switch game, the scenario, right? You start off with a state, imagine it's like a small little circle. And this is the state where the robot is considering whether to do an action, turn itself off, or allow itself to be - let the human turn it off or not.

**Daniel Filan:**
So you've got this initial state for it deciding what to do. And from that state, you can imagine an arrow to a state, maybe down on the left, where it does an action, a state down to the right where it turns itself off, there's an arrow there, and there's also an arrow going straight down to a state where it lets the human do either. And then from the state where the human could do either, you have this arrow to the left where the human says, "yep, you can do that action". And then it's like, "yep, you're just in the world where the AI got to do the action". Or an arrow to the right where the human turns it off. And now it's just turned off as if it had turned itself off.

**Daniel Filan:**
This is exactly what the picture of - [what a product diagram looks like in category theory](https://en.wikipedia.org/wiki/Product_(category_theory)). You have a function from some set at the top to A cross B, which is at the bottom in the middle. And you can decompose that, from A cross B, you can go to the left to A, and that function can factor out through A, or you can go to B and function could go to B. Is this just a coincidence? Or is there something deep going on here? I can't figure out what it means, but I feel like there might be something, or maybe it's just a coincidence. Do you have any thoughts about this?

**Dylan Hadfield-Menell:**
I am wary that it might be a coincidence.

**Daniel Filan:**
Yeah. There aren't that many graphs with four nodes, right?

**Dylan Hadfield-Menell:**
Right. But I also think, let's say if you were my PhD student and you came into my office and said, "Ah, I think this is the research problem. You did the off-switch game. I read that. We're working together now. I want to build on this. I've noticed this weird thing about category theory and it looks like there's at least a graph isomorphism." And I think my answer would be to warn you that there very well could be nothing here. And I'm not that well calibrated as with estimates, but we'll call it like, I don't know, my hunch would put it at like 60 to 80%, call it 70%, that it's a red herring kind of thing. But that leaves enough that I would be interested in looking into it. And if I lean into trying to think of the ways in which that could be true, perhaps this means that there are some interesting ways to look at the joint human robot system as a product of human and robot behavior.

**Dylan Hadfield-Menell:**
Or perhaps certain types of interactions can be described as the human and the robot functioning in sequence versus in parallel. So, maybe the question to ask is if this is a product, what is addition, and what is the off-switch or assistance game representation of an additive interaction.

**Daniel Filan:**
So I spent some time thinking about this and the closest thing I got to the category theory co-product, which is kind of like the disjoint union of sets, which is sort of like addition, is you can kind of tell a story where the robot being transparent ends up being a co-product, but it's not super convincing. I might leave that to the listeners to see if there's anything there.

**Dylan Hadfield-Menell:**
If there are interesting ideas there, send them our way.

**Daniel Filan:**
Speaking of interesting ideas, we're going to move onto the third paper we're going to talk about today. So [Inverse Reward Design](https://arxiv.org/abs/1711.02827). This one you worked on with Smitha Milli and Pieter Abbeel, Stuart Russell, Anca Dragan. Can you summarize this paper for us?

**Dylan Hadfield-Menell:**
Yes. So, if you were to apply cooperative IRL in scenarios for something other than human-robot interaction, inverse reward design is probably where you might start. What it looks at is a cooperative IRL interaction. So we've got two players here. We've got the human and the robot. This is now going to be a phase, a game with two turns. Person's going to go first. And the robot's going to take, well, go second and potentially take multiple actions. And the way that this game works, is, the person goes first and selects a proxy reward function of some kind, given an observation of a training environment. So, the person gets to see the environment that the robot's in and picks a proxy reward function. Then the robot goes to a new environment, the person doesn't know which one that will be. And now the robot's goal is to maximize utility in this new deployment setting.

**Dylan Hadfield-Menell:**
And what this is meant to capture, is the idea that, well, so through cooperative IRL and the off-switch game, we're arguing that uncertainty about objectives is important. Okay, great. We'll take that point. There's still a reason why we specify things with objectives. Put differently, there is a reason why the field of artificial intelligence happened upon reward functions, as a way to communicate goals. And, from our perspective, we thought is that this means that reward functions are perhaps an information-dense source of information about reward functions, which certainly makes a lot of sense, right? And so inverse reward design was our attempt to say, "Okay. We have observed a reward function. We know that we should be uncertain about what the true reward is." So we're not going to interpret this literally, but then that leads to the question "What else?" What type of uncertainty should you have about the true reward, given an observed proxy? And inverse reward design is our attempt to answer that. And the extra information that we bring in to structure this inference is that notion of a development environment.

**Dylan Hadfield-Menell:**
It's not the case that you got this proxy reward function out of the blue. This proxy reward function was designed in the context of a particular environment. And this is what gives us the leverage to do inference with.

**Daniel Filan:**
So, should I imagine that as, I have some robot, and when I'm writing down the reward function, I'm kind of imagining what it would do with various reward functions and, should I think of this proxy environment as how I'm imagining the robot would behave for different reward functions?

**Dylan Hadfield-Menell:**
In a sense. The way that I think about this is, let's say you are a large company. You're the new company that's going to create household robots. So, it's going to be some variant of the [PR2](https://robots.ieee.org/robots/pr2/), which is a robot with wheels and two arms that can move around people's houses. And you, as a company are building this in order to help tidy people's living rooms and do things like that. So, what's your practical strategy that you're going to go about for doing this? Well, if you have the resources, what you'll do is you'll build a training scenario, which will be a giant warehouse, where the insides of it will account for your attempt to cover the space of possible home environments that your system will be deployed into.

**Dylan Hadfield-Menell:**
So you go ahead and you build this and then what do you do? You hire some roboticists and you say, "Here's a loose spec of what I want. You make this happen in that environment... You make this happen in those environments." Right? So these are the design iteration environments that you're working with. And then what happens is that designer identifies incentives along with a reward function, such that - incentives along with an optimization planning approach, such that the behavior is good in that set of environments. In that set of test environments.

**Dylan Hadfield-Menell:**
And what we're saying is that now when the robot leaves that very controlled setting, and goes out into the broader world, what is a good principled way to be uncertain about the things that might be missing from your objectives? And, inverse reward design formalizes that inference problem.

**Daniel Filan:**
And I guess it's the inverse of the reward design problem of picking a good reward.

**Dylan Hadfield-Menell:**
Precisely.

**Daniel Filan:**
So you run these experiments with an agent that tries to solve this inverse reward design problem. Right? And there are various things you'd say about that, but one thing I'm wondering is, for things that are solving, either explicitly solving this problem well, or somehow trying to reach the optimum, whatever that might be, I wonder how predictable they would be. So for context, there's this computer program that I use called [Emacs](https://www.gnu.org/software/emacs/) where you type letters and they show up on the screen and it doesn't try to guess what I mean, it just does what I said. And for me it's very easy to reason about. There's also this website I use called [Google](https://www.google.com/), where I type things and it tries to do the most helpful thing, given what I typed. Somehow it seems like it's doing some inference on what I meant and is trying to satisfy that.

**Daniel Filan:**
Emacs seems a lot more predictable to me than Google. And I kind of like that property and predictability seems like a really desirable property for high impact AI systems. So I'm wondering how predictable do you think systems would be that were behaving somehow optimally for the IRD problem?

**Dylan Hadfield-Menell:**
Yeah, that's a very good question. So, with predictable systems, you're relying on the person a lot more in a way, because... It doesn't have to be the case, but if you have a diverse set of behavior that you want to be predictable, then you need to have enough information to pick out the one that will happen, in the future. And I think the reason why we're in this mess is the systems that are predictable for complex... the types of settings with artificial intelligence, often predictable means predictably bad, I think. And there's a sense in which the range of things that Emacs can do is much, much smaller. And if you really get into the craziness on it and deeply configure it, you can, I'm sure, get some really unpredictable behavior out of it.

**Daniel Filan:**
Yeah, you can, it can be an operating system and you can use Google through it.

**Dylan Hadfield-Menell:**
Right. So I think that's talking about where the tension is, in the ways that predictability can be a double-edged sword. Now, let's talk about how predictability comes into inverse reward design. As it turns out, there's an interesting problem that comes up when you want to actually use the results of inference. It's a little bit technically involved, but actually quite interesting and I think related to this problem. So bear with me a bit. When you are doing inference over preferences, with the kinds of models that we're using, there are certain components of the reward function that you are never able to fully identify. So, in this case, we're using a Boltzmann distribution to define the human's rationality, to define their behavior. And so this means that all reward functions, if you add a constant to them, end up being the same.

**Dylan Hadfield-Menell:**
Okay. All well and good, until you do inference and produce a set of these reward functions and now want to maximize expected utility. It turns out, if you do this directly, you actually do a really bad job. And the reason why, is because every reward function that you infer is exactly equally likely to that reward function, plus C for every possible value of C. Now your inference procedure will not find every possible value of C. It will end up at some but not others. And, because you're doing Bayesian inference in high dimensional spaces, and that's challenging, you'll end up arbitrarily setting a lot of these constant values for your different estimates of the reward functions. And then, if you naively add them together, and average, you end up where that noise in your inference, can have an overly large determination on the results.

**Daniel Filan:**
So it seems like maybe, during inference, let's say you randomly sample 10 reward functions, right? And get the relevant likelihoods, and the reward functions have different constants added to the reward of every single state. If I take the expectation over those, then it's like taking the expectation if all the constants were zero, and then adding the expectation of the constants. Right. Because expectations are linear. So, wouldn't that not affect how you choose between different actions?

**Dylan Hadfield-Menell:**
So, in theory, no, with enough samples, no. Because those averages would cancel out.

**Daniel Filan:**
Even with 10 samples though.

**Dylan Hadfield-Menell:**
Even with 10. So let's say that the constant value just, let's say you're running Markov chain Monte Carlo to do inference.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
That constant value will just be going on a random walk of some kind. And the point where it reaches its minimum, will be unrelated to the actual likelihood of the reward at that point. And so this could be a really good reward that gets driven down a lot. So it could be, you have the reward for, let's say there's two actions - action one and action two. We're doing inference over which one is going to be better. Action one or action two. And, let's just say that randomly it so happens that action one's... for the reward functions where action one is better, the sum of the constants on those constants end up being negative.

**Daniel Filan:**
Oh, so we're doing like the sampling separately per action. And that's why the actions are getting different constants.

**Dylan Hadfield-Menell:**
So the actions have the same constants within a reward function. It's just that when you're comparing two reward functions, you're going to be comparing reward function one for action one, versus reward function two for action two, which decomposes into some real reward value, plus two different constants. And the values of those constants can matter more.

**Daniel Filan:**
But, but where do they end up mattering? Because if they don't end up mattering for the likelihood that you'd take the actions, right? Because you mentioned that Boltzmann rational people aren't sensitive to constant-

**Dylan Hadfield-Menell:**
This matters when the robot is optimizing its estimate-

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
of utility. And trying to, well, so one thing is that these issues get really exacerbated when you're trying to do risk averse trajectory optimization, which is where this is all headed in the end. You actually might be right for in expectation, they all cancel.

**Daniel Filan:**
Yeah. Let's talk about risk averse trajectory optimization.

**Dylan Hadfield-Menell:**
What ended up happening was I tried to do risk averse optimization for utility functions, and it totally failed. The first time. And it took a long time for me to figure out why it was failing. And in practice it was because, when you're looking to maximize reward for, say, the minimum reward function in your hypothesis, that minimization is more often determined by the constants in this reward function inference, than it is by the actual reward values themselves. And it turns out, that in order to fix this problem, what you have to specify is a point that all the reward functions agree on. Right? A way to account for this unbound parameter, so that constant is the same for everything, and standardize it in some way. And this standardization specifies the fallback behavior when the system has high uncertainty about reward evaluations.

**Dylan Hadfield-Menell:**
So, for me I think this was really interesting to see, because it was a way that... it showed that there's a component. Like it falls out of a free lunch kind of thing. If you want to do something different when you don't know what to do, you have to say what to do in that case. You can't figure it out because you don't know what to do. And this is very clearly at a low level in the math, telling you, here is the point where you put in the predictable part of what the system will do at deployment time. So, in doing a risk averse optimization here, what that brings in is effectively a check on, are the trade-offs in my new environment similar to the trade-offs that I saw in my training environment? And if not, what should I do?

**Daniel Filan:**
So you said this comes up when you're using risk averse planning. Why use risk averse planning?

**Dylan Hadfield-Menell:**
As opposed to maximizing an expectation?

**Daniel Filan:**
Expected utility, man, it's the thing to do.

**Dylan Hadfield-Menell:**
Well, as a practical matter, maximizing expected utility, isn't going to do much different than optimizing for the literal proxy that you've gotten. It does actually change some things because there are, you might get a proxy reward that isn't the most likely one.

**Daniel Filan:**
So in the paper you talk about this gridworld with grass and dirt and lava. I think talking about that makes this clear, so can you introduce that?

**Dylan Hadfield-Menell:**
Yeah. So what we're doing is looking at, well, we hypothesize that there's a 2D robot that's going to go and navigate some terrain. And in the development environment, the story is that the designer wants the robot to navigate terrain. And there's two types of terrain that it can be on. There's really dirt paths and grass. So, picture dirt paths going through a park in some way. And, in addition to that, there are also pots of gold in the park. So there's three types of terrain. It's really three kinds of things. There's regular dirt, there's grass and there's pots of gold. And the high level goal for the system is for it to navigate to the pots of gold quickly, and stay on the dirt where possible, but maybe taking shortcuts if it'll save you a lot of time. And that's, so if you recall, this is our development environment, and the robot's now going to get an objective in that setting, and go to a new environment and we're going to capture, is there something here that the designers didn't foresee or didn't intend?

**Dylan Hadfield-Menell:**
And so in the story, what they didn't realize was that this robot was... also it was going to be deployed across all 50 states in the U.S. and one of them is Hawaii. So there's another important terrain type, which is lava. And you haven't thought about lava before, as the designer. So your reward function doesn't provide an accurate assessment of the utility in that case. Now, in doing inference, what do you do in the setting? Well, the really simple, intuitive version of what IRD does, is it says, it doesn't matter what reward value lava had for what I did in the development environment. There were no instances of lava present. And so changing the reward evaluation of that state, would not have changed my behavior. And so I don't expect the designer to have thought very hard about what that reward evaluation is.

**Dylan Hadfield-Menell:**
And then, when you get to this deployment environment, and there is this state, this is a principled reason to distrust your inferences on that state. And now, the question is, we arrive here and so now we have this uncertainty distribution, we know what reward functions the person could have meant, really. And it's got like a lot of uncertainty about how good lava is. Now, planning for that in expectation, you actually might still plan to go right through it, because you'll just be assuming that, in effect, if you plan in expectation, you are assuming that the states the designer didn't think about are equal to their prior value, which, in expectation, is a good idea. It very well could be. This lava could actually be, rather than being deployed to Hawaii, you were deployed to some magical land where this is a magic carpet that just transports you instantly to the gold, or something like that. It's reasonable - from the standpoint of this robot, it's equally plausible to something catastrophic like lava.

**Daniel Filan:**
Yeah, I guess. Although in that case, it seems like the problem is one of not knowing the dynamics, rather than the reward. Or let's suppose that you could drive through lava and the reason that it's bad is that we humans might want to touch the robot afterwards. And if it went through lava, then we'd burn our hands or something.

**Dylan Hadfield-Menell:**
Sure, sure. That sounds good. So, what risk aversion does, is it allows us to take advantage of that uncertainty, and adapt our response. And what this says is, well, if there are strategies you can take that don't go into this uncertain area, you can just, as a heuristic, avoid that uncertainty, even if that increases the amount of path length you have to go through, that can be worth it. And, intuitively, I think the reason why this makes sense, if you're someone who thinks that we should be maximizing expected utility overall, is really what we're doing is bringing in some prior knowledge, either about what the priors are on the types of failures that can be. So, you might think that in this we said Gaussian prior over rewards, but that's actually because we're being lazy, and we should really be looking to get something that has appropriately heavy-tailed failure modes. And we could try to represent that, and that might be an interesting structure to bring to play. Risk aversion allows us to do that without having to be very specific about what those priors are.

**Daniel Filan:**
Heavy-tails itself, wouldn't do it. If the tails are symmetric.

**Dylan Hadfield-Menell:**
Yes. You'd have to have heavy low tails, like heavy one sided, which is - now you start to get to a point where, mathematically you're playing around with things to try to get desired systems. And it might be easier to actually go in and modify the objective, in that case. Right. And, in that, we know that there are some of these dynamics at play. Like there are either catastrophic failures or dead ends or something like that. And we are not able to represent that explicitly in this mathematical model, at the inference level. But we can build that in. I'm curious to know what properties of an assistance game make risk aversion the right thing to do. So the way that I look at this is in the same way that corrigibility, or the ability to turn a system off, is kind of like a desired... it's almost like a specification of a type of solution. And it's a behavior that intuitively we think is good for assistance games or alignment problems.

**Dylan Hadfield-Menell:**
I think risk aversion plays a similar role in the sense that in many scenarios, principled conservativism, with respect to your utility, does make sense, and different theoretical constructs could lead to that. So we talked about one thing which was... and they all are different ways of, building in this possibility of catastrophic failure into the belief structure somehow.

**Daniel Filan:**
I mean one thing it's related to in my mind is, I don't know if you've listened to [this podcast episode on infra-Bayesianism](https://axrp.net/episode/2021/03/10/episode-5-infra-bayesianism-vanessa-kosoy.html), but it's basically this notion of imprecise probability, where you plan for the worst case out of the probability distributions. And you can come up with an update role where you can plan for the worst case and also be dynamically consistent, which is nontrivial. And yeah, I've just realized there might be connections there.

**Dylan Hadfield-Menell:**
Yeah, I could certainly see that. Perhaps some of the intuitive reason for why I would want risk aversion has to do with my statement earlier about utilities... The idea that the goal achievement part of intelligence is smaller than we think it is. So, if you think of that goal achievement component is what the system should be doing, in a sense that maximizing utility is the end goal. Then I think from that standpoint, the question, "Well, shouldn't you just be maximizing expected utility?", makes a lot of sense. But if you imagine that the goal achievement component is one part of the system, that's going to be working with representations and abstractions of the real world, to come up with plans to implement, then planning conservatively is probably better in expectation, is perhaps a way to put that.

**Daniel Filan:**
So speaking about aspects of this IRD framework, kind of like how in CIRL, you had this prior over reward functions that you have to, or even here you have this prior to that you need to analyze, there was also this, the principal's initial model of the world that bears really heavily on this analysis. And I'm wondering if you can give us a sense of if we're trying to do this analysis, how we should formalize that.

**Dylan Hadfield-Menell:**
So, just to clarify, the principal's model of the world is their model of this development environment, right?

**Daniel Filan:**
Sorry. Yeah. The development environment that they're imagining the system being deployed in.

**Dylan Hadfield-Menell:**
I think there's two ways to look at it. One is, this is actually the scenario that you were evaluated in during development. Practically, just the system was designed through iteration on behavior in a particular environment. And so the assumptions behind IRD are basically that the reward function was iterated enough to get to behavior that is well adapted to the environment.

**Daniel Filan:**
And I guess there, you can just know that you like trained on [MNIST](https://en.wikipedia.org/wiki/MNIST_database) or something.

**Dylan Hadfield-Menell:**
You actually have access to that by definition in a way.

**Daniel Filan:**
Yeah. For listeners who don't know, MNIST is a dataset of pictures of handwritten numbers with labels for numbers they are.

**Dylan Hadfield-Menell:**
That's kind of the very literal interpretation of that model. The other side of it is I think what you were gesturing at, which is the designer having a model in their head of how the agent will respond, and having an idea of, "Here are the types of environments my system is likely to be in. And here's the mapping between those incentives and behavior."

**Dylan Hadfield-Menell:**
I think if you imagine that as going on inside of someone's head, what this is really telling you is how to be a good instruction follower. If you are working for me and I tell you to do something or I tell you, "Here are some things that I care about. Here's some representation of my goals." If you don't know what type of context I'm imagining you will be in, then you won't have much information about how to interpret those objectives, and you'll miss things.

**Dylan Hadfield-Menell:**
IRD sort of philosophically here is saying that the way to interpret a goal someone gives you is to think about the environment and contexts they thought you were likely to be in, and that that's a core piece of cognitive state to be estimating or something like that.

**Daniel Filan:**
Yeah. So thinking back to the CIRL analysis of the IRD game. In IRD, you sort of have this human policy, which is to write down a reward function that induces good behavior in some test environment. And then you're analyzing what the best response to that policy is. That produces some robot policy. But it seems probably the best response to that robot policy on the human side would not be the original behavior. So I'm wondering, how many steps towards equilibrium should we imagine being taken?

**Dylan Hadfield-Menell:**
I think it depends on what information the person has about the environments they're going to eventually be deployed in. This is going to get confusing because in this setting now we have the designer's model that they're having in their mind while they're designing the reward function, and there's that environment. And then there's the set of other environments that the system might be put into. And if you want to design a best response to the robot policy, you have to work backwards from that future sequence of behaviors.

**Daniel Filan:**
Yeah. Maybe you need to do it in a POMDP rather than an MDP, a world where there are some things about the state of the world that the human doesn't know.

**Dylan Hadfield-Menell:**
Yeah. Perhaps. In some ways, the point of this model is to capture the scenarios where people aren't thinking very much about - or the extent to which... Maybe here's a good way to put it. In this model, we're actually leaving out some really important, potentially large pieces of information, which is that the selection of development environments is not arbitrary or random. In fact, we tend to select them in order to communicate things about the objective that we think are important.

**Dylan Hadfield-Menell:**
So in some ways the development environment kind of captures our best estimate in the spirit of this model of how the robot will be deployed. And in that case, the person is actually going pretty far, potentially to equilibrium actually, for them. You are absolutely right that thinking more or harder or about the way that the robot will deploy could lead to changes in the proxy reward functions that it's optimal to specify.

**Dylan Hadfield-Menell:**
And you are also right, I think you could also have that... The best thing to do, if you realize that there's a part of your deployment environment that's not well-represented in your development environment, is you just augment the development environment to include that component. And then you are providing incentives that are kind of better matched. Yeah.

**Dylan Hadfield-Menell:**
I'm trying to think of the right summary to close this question on. I think it's that there's definitely a lot of additional iteration that could be possible here, and the opportunity for more coordination. I'm not sure that it makes sense to study those sort of directly within this model as it is represented, in the sense that it seems like that type of strategic improvement is perhaps better represented by changes to the development environment, or you end up assuming more... Or you would want to have a better richer cognitive model of reward design and how reward design could go wrong perhaps.

**Dylan Hadfield-Menell:**
I think in setting this up, part of the core idea is if the person is wrong about the objectives, if the proxy reward is not actually the true reward, what can you do? And so it only really makes sense to study that if people are limited.

**Daniel Filan:**
That makes sense. One kind of way you could think about this work is in the context of side effects mitigation, like this example with the lava, right? It was this side effect that humans didn't think about like, "Oh, what if the robot gets into lava?" And this is a way of avoiding the robot having some nasty side effects.

**Daniel Filan:**
Yeah, I guess this episode hasn't been released, so you don't know it yet, but the listeners will have just... [The previous episode of this podcast](https://axrp.net/episode/2021/05/14/episode-7-side-effects-victoria-krakovna.html) was with Victoria Krakovna on side effects mitigation.

**Daniel Filan:**
You've actually some work yourself on this problem. You've coauthored with Alexander Turner on [Attainable Utility Preservation](https://arxiv.org/abs/1902.09725). I'm wondering how do you think the side effects problem... Yeah, what do you think about the relationship between the side effects problem and this approach of inverse reward design?

**Dylan Hadfield-Menell:**
Well, they're sort of slightly different in that one is a solution and the other is a problem.

**Daniel Filan:**
Do you think they're matched?

**Dylan Hadfield-Menell:**
I'm not sure. There certainly are some definitions of side effects for which it's well-matched to this problem. At the same time that there are some kinds of side effect avoidance strategies... Well, here, I guess what I'll say is side effect avoidance is a pretty broad range of approaches, and so I wouldn't want to rule out other solution approaches that don't leverage inverse reward design in its particular Bayesian form in some way.

**Dylan Hadfield-Menell:**
With that said, I think you can describe a lot of side effect avoidance approaches in language similar to the model that we're producing here. What counts as a side effect? What types of problems do we evaluate on? They almost always end up being, well, here's some environment where... Here's the reward function that the system got, which is intuitively reasonable. And here is the side effect, which is why that's wrong. And here is how optimizing for this relatively generic term can allow you to avoid it.

**Dylan Hadfield-Menell:**
And that reasoning often relies on some intuitive agreement of, this reward is reasonable. And what IRD does is kind of provides a probabilistic definition of reasonableness, where rewards are reasonable if they work well in a development environment.

**Dylan Hadfield-Menell:**
When I look at a lot of these side effects examples, I often, in my head, sort of translate them to, "Oh, well there was a simpler environment where this particular action wasn't possible. And that's where the reward comes from, and now they're looking to go into this other setting."

**Dylan Hadfield-Menell:**
There are interesting approaches to that problem, which don't come from a Bayesian reward inference, uncertainty perspective that I'm guessing that you guys talked about some of the [relative reachability work](https://arxiv.org/abs/1806.01186) that she's been involved in.

**Dylan Hadfield-Menell:**
This is kind of a different perspective to kind of side effect avoidance, which would avoid lava if there are certain properties of the transition function that make that bad, but wouldn't avoid it if it's merely the case that whether lava was good or bad was kind of left out of the objective.

**Dylan Hadfield-Menell:**
And so I think these are perhaps, maybe the... Maybe that's why I didn't want to say that there's a one-to-one relationship between side effect avoidance and inverse reward design. In that there's another class of side effect avoidance which involves doing things like preserving the ability to put things back. I think that that is solving a similar problem and I don't want to claim that it's the same.

**Daniel Filan:**
Now I want to move on to some closing questions about the line of work as a whole. First of all, these papers were published between 2015 and 2017, if I recall correctly, which was some time ago. Is there anything you want to say about... You've mentioned some papers that have been published in addition to the ones we've talked about. Would you like to give listeners an overview of what's being done since then on this line of thinking?

**Dylan Hadfield-Menell:**
Sure. We've extended inverse reward design in a couple of directions to look at both active learning as well as using it to fuse multiple reward functions into a single one, which can make designing reward functions actually easier, so if you only have to worry about one development environment at a time, you can do a kind of divide and conquer type of approach.

**Daniel Filan:**
And that's work with Sören Mindermann?

**Dylan Hadfield-Menell:**
The [divide and conquer work](https://arxiv.org/abs/1806.02501) is with Ellis Ratner. And the work on [active learning](https://arxiv.org/abs/1809.03060) was with Sören Mindermann and Rohin Shah as well as Adam Gleave.

**Daniel Filan:**
All right.

**Dylan Hadfield-Menell:**
I also mentioned [the assistive multi-armed bandit paper](https://arxiv.org/abs/1901.08654), which is looking at what happens when the person doesn't observe their reward function directly but has to learn about it over time. We've done some work on the mechanics of algorithms within cooperative IRL. For folks who are interested in it, I recommend reading our ICML paper.

**Daniel Filan:**
Is that about the generalized Bellman update?

**Dylan Hadfield-Menell:**
Yeah. This is [a generalized Bellman update for cooperative inverse reinforcement learning](http://proceedings.mlr.press/v80/malik18a.html), I think. And in that, we actually give an efficient algorithm for computing optimal cooperative IRL strategy pairs.

**Daniel Filan:**
That's Malayandi Palaniappan and Dhruv Malik?

**Dylan Hadfield-Menell:**
Yeah, Malayandi Palaniappan and Dhruv Malik. That's a paper that I definitely recommend folks interested in this work go look at because that's... If you want to experiment with non-trivial cooperative IRL games, those algorithms are the ones that you'd want to go look at.

**Daniel Filan:**
Okay.

**Dylan Hadfield-Menell:**
I mentioned also [a paper that looks at value alignment in the context of recommender systems](https://participatoryml.github.io/papers/2020/42.pdf), which doesn't directly use cooperative IRL, but is applying some of those ideas to identify misalignment and talking about it more generally.

**Dylan Hadfield-Menell:**
There's a paper called [Incomplete Contracting and AI Alignment](https://arxiv.org/abs/1804.04268), which looks at the connections between these assistance games and a really broad class of economics research on what's called incomplete contracting, which is our inability to specify exactly what we want from other people when we're contracting with them.

**Dylan Hadfield-Menell:**
An example of incomplete contracting that people will use for a long time is all of the things that happened when we went into [lockdown](https://en.wikipedia.org/wiki/COVID-19_lockdowns) for [COVID](https://en.wikipedia.org/wiki/COVID-19), which is lots of people had contracts for exchanging money for goods or what have you, and then all of a sudden a pandemic happened, which was maybe written into contracts in some very, very loose ways, like acts of God clauses, which are contracting tools to manage uncertainty and our inability to specify what should happen in every possible outcome of the world.

**Dylan Hadfield-Menell:**
There's a very strong argument to make that assistance games are really studying AI under incomplete contracting assumptions. In the sense to which your reward function that you specify for your system is in effect a contract between yourself and the system.

**Dylan Hadfield-Menell:**
I think there's [the paper on attainable utility preservation](https://arxiv.org/abs/1902.09725) with Alex Turner, which I would recommend folks look at, where the main idea there is we're really measuring distance and impact via change in a vector of utility functions. And we show that this has some really nice properties for limiting behavior and some practical applications in some interesting side effect avoidance environments.

**Dylan Hadfield-Menell:**
And the last thing I would direct people towards is my most recent papers on the subject, which are [multi-principal assistance games](https://arxiv.org/abs/2007.09540), which looks at what happens, some of the interesting dynamics that come up where you have multiple people who are being learned from in assistance game, and [consequences of misaligned AI](https://arxiv.org/abs/2102.03896), which is a theoretical analysis of proxy objectives where we show that missing features in a fairly strong theoretical sense can lead to arbitrarily bad consequences from a utility perspective.

**Daniel Filan:**
All right. And I guess the dual to that question is what do you think the next steps for this research are? What needs to be done?

**Dylan Hadfield-Menell:**
The thing I'm most excited about is integrating meta reasoning models into cooperative IRL games. So looking at cooperative IRL where one of the questions the person has to decide for themselves is how hard to think about a particular action or a particular problem.

**Dylan Hadfield-Menell:**
I think that this is something that's missing from a lot of our models. It places strong limits on how much you can learn about utilities, because there are costs to generating utility information that are distinct from actions in the world and are avoidable in some sense.

**Dylan Hadfield-Menell:**
I also think that because people choose how much to think about things and choose what types of computations to run, this makes the relationship between the person and the system even more crucial, because the system may have to do more explicit things to induce the person to provide the information that you need.

**Dylan Hadfield-Menell:**
So if you think about more regular cooperative IRL, the system might need to steer the person to an informative region of the environment where they can take actions that provide lots of information about reward. When you introduce meta reasoning and costly cognition, now the robot has to steer the person in belief space to a place where the person believes that choosing to take those informative actions is worthwhile. And I think that that is a more complicated problem.

**Dylan Hadfield-Menell:**
On the other side, actually, figuring out how to build systems that are calibrated for cognitive effort, I think would be really valuable to point this towards recommender systems, which is something I've been thinking about a lot recently. There is a critique, which I think is pretty valid, that a lot of the issues we're running into stem from the fact that our online systems are largely optimizing for system 1 preferences.

**Daniel Filan:**
What's system 1?

**Dylan Hadfield-Menell:**
System 1 is a reference to [Kahneman's model of the brain](https://en.wikipedia.org/wiki/Thinking,_Fast_and_Slow#Two_systems) where people have two systems. One is system 1, which is a fast, reactive, intuitive reasoning system, and system 2, which is a slower logical conscious reasoning system.

**Dylan Hadfield-Menell:**
The point that I was trying to make is that a lot of our behavior online particularly is reactive, not very thought out. You could argue, in fact, that these systems are designed to push us into that reactive mode. One of the things that you could imagine would be a better situation is if people had more conscious cognitive effort going into what kinds of online behaviors they actually want.

**Dylan Hadfield-Menell:**
And one way to understand this is the appropriate amount of cognitive effort to spend deciding whether or not to click on a link, if the only impact of your actions are you might read a bad article and stop, or you might read a good article, there's a certain amount of how hard you should think about that, optimally.

**Dylan Hadfield-Menell:**
However, if clicking on that link determines what links you will see in the future, the appropriate amount of cognition increases because your actions have a larger effect. And I think that miscalibrating this component, this kind of misalignment in a way is... You can think about the system as being misaligned with people or you can think about people as being misaligned with what the system will do in the future, and kind of choosing the wrong level of cognition for their future selves.

**Daniel Filan:**
So those are some next steps. But suppose maybe today there's a listener who is a deep learning practitioner. They use deep learning to make things that they want people to use. How do you think they should change what they're doing based on assistance game analysis?

**Dylan Hadfield-Menell:**
Well, I think one is a kind of intuitive shift in how you look at the world, to explicitly recognize that there's a very subjective, normative component of what you are doing when you program your system. There's an idea that our data is labeled with what we want it to do, and that sort of is true or something like that.

**Dylan Hadfield-Menell:**
In reality, the role that labels in a deep learning system play or the reward function in MDP plays is it's a description of what you want for the system. It's a way that you are trying to encode your goals into a representation that can be computed on. I think that if we, as a field, actually just shifted to thinking more about those types of questions, spending more time thinking about the source of normative information in our systems and thinking explicitly about, "Do I have enough information here to capture what I really care about?"

**Dylan Hadfield-Menell:**
This is a tweet from [Dr. Alex Hanna](https://twitter.com/alexhanna), who's at Google Research, and what they pointed out is that for say a large language model, you can have trillions of parameters, but ultimately there's only hundreds of decisions that go into selecting your data. And part of our difficulty in getting these large language models to output the text we want, I think, is because of that kind of imbalance, right? It's complicated because language actually has lots of normative information in it, but because it's observational and you can predict, this leads to that, I see this text, I produce that text, you don't interpret that as normative information.

**Dylan Hadfield-Menell:**
I tend to think that that is the biggest lesson for practice. I would say the other part about it is what is your strategy for integrating qualitative research and analysis into your process? Actually, if you just want to be a good deep learning researcher, and you just want to produce neural nets that do a good job, paying attention to this loop and trying to optimize this loop from, what are features of my system behavior that are not currently represented that I don't like, or that I do like? Identifying those behaviors, developing measurements for them, and then integrating that back into the system. The deep learning engineers and the deep IRL engineers who are the best, I think are really good at completing this loop really effectively. And they've got intuitive skill at completing that loop, but also they just have really good methods and they're pretty formulaic about it.

**Dylan Hadfield-Menell:**
I think for whatever scale system you're building, if you're just building it on your own, you're a grad student trying to do policy optimization for a robot, this is a good idea. And if you're a large company that's managing search results for the global population, you also want to be doing this. And if you look at search actually, this is a relatively well-developed area with good standards of what makes a good search result. And there's lots of human raters providing explicit feedback about what makes good search results. Arguably part of why that works is because, well, we're paying for the right kind of data.

**Daniel Filan:**
All right. Another related question. So this is one I kind of like to ask. People worried about AI existential risk, they sort of have this idea that, "Look, there were people who did some work to develop really smart AI, and they didn't stop to ask the question 'Okay, how could the work I'm doing have negative impacts?'" How could the work that you're doing have negative impacts on the world? And you're not allowed to say, "Oh, the negative impact is we wasted our time and it didn't pan out."

**Dylan Hadfield-Menell:**
No. There are some really clear negative impacts that this work enables. Effective alignment with concentrated power distributions is a recipe for some pretty bad situations and environments. Single agent value alignment on its own really just allows people to get systems to do what they want more effectively. Arguably, value alignment is as dual use as a technology could be, at least as it's currently thought about.

**Daniel Filan:**
Sort of like a power plant, right? It's just like a generally empowering thing. Now you just have a way to get more electricity, now you can do things more.

**Dylan Hadfield-Menell:**
Yeah. It's sort of like a power plant though where someone might just build their own power plant.

**Daniel Filan:**
Yeah, like a home power plant.

**Dylan Hadfield-Menell:**
And sort of use it for their own purposes. And if you had some people with power and others without, that could lead to scenarios where the people out of power are treated really poorly. I think arguably that would... I mean, arguably that's what AI systems are doing to some extent nowadays, in terms of cementing existing power dynamics. Alignment could supercharge that in a way.

**Dylan Hadfield-Menell:**
One of the concerns that I have about effectively aligning systems to an individual is that it might be fundamentally immoral to get to a scenario where one individual has undue influence over the future course of the world. That's a sort of direct one-to-one for these power imbalances. I think there's a practical - short-term, right now, if Facebook gets really good at aligning systems with itself and we don't get good at aligning Facebook with us, that's potentially bad. And I think if you start to think about future systems and systems that could reach strategic dominance, then alignment with... You might want to have alignment approaches that cannot align to an individual but have to align to a group in some way. I don't know if that's the - It's a little bit vague.

**Daniel Filan:**
That's a fine answer.

**Dylan Hadfield-Menell:**
It's kind of like if we imagine that preferences don't actually reside within an individual but reside within societies, then alignment to an individual that allows that individual's preferences to capture a lot of utility could be quite bad.

**Daniel Filan:**
If people are interested in following or engaging with you and your work, how should they do that?

**Dylan Hadfield-Menell:**
Yeah. I have a moderately active Twitter account where I publicize my work and I generally tweet about AI safety and AI alignment issues. That's [@dhadfieldmenell](https://twitter.com/dhadfieldmenell), first initial, last name. I'll put out a plug for if you are a... Well, essentially if you're interested in doing this type of work and you thought this conversation was fun and you'd like to have more conversations like it with me, I'll invite you to [apply to MIT's EECS PhD program](https://gradapply.mit.edu/eecs/apply/login/?next=/eecs/) next year and mention me in your application.

**Daniel Filan:**
All right. Well, thanks for appearing on the podcast.

**Dylan Hadfield-Menell:**
Thanks so much. It was a real pleasure to be here.

**Daniel Filan:**
And the listeners, I hope you'll join us again.

**Daniel Filan:**
This episode is edited by Finan Adamson. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
