---
layout: post
title: "15 - Natural Abstractions with John Wentworth"
date: 2022-05-22 22:30 -0700
categories: episode
---

[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/MmJkNTgzMWEtZDFkMS00MWEwLWE4NzctMzg3OTcxMGVlYjZj)

Why does anybody care about natural abstractions? Do they somehow relate to math, or value learning? How do E. coli bacteria find sources of sugar? All these questions and more will be answered in this interview with John Wentworth, where we talk about his research plan of understanding agency via natural abstractions.

Topics we discuss:
- [Agency in E. coli](#e-coli-agency)
- [Agency in financial markets](#financial-markets-agency)
- [Inferring agency in real-world systems](#inferring-agency-real-world)
- [Selection theorems](#selection-theorems)
- [(Natural) abstractions](#natural-abstractions)
- [Information at a distance](#info-at-a-distance)
- [Why the natural abstraction hypothesis matters](#why-nah-matters)
- [Unnatural abstractions in humans](#unnatural-abstractions-in-humans)
- [Probability, determinism, and abstraction](#probability-determinism-abstraction)
- [Whence probabilities in deterministic universes?](#whence-probs)
- [Abstraction and maximum entropy distributions](#abstraction-max-ent)
- [Natural abstractions and impact](#natural-abstractions-impact)
- [Learning human values](#learning-human-values)
- [The shape of the research landscape](#shape-research-landscape)
- [Following John's work](#following-johns-work)

**Daniel Filan:**
Hello, everybody. Today I'll be speaking with John Wentworth, an independent AI alignment researcher who focuses on formalizing abstraction. For links to what we're discussing, you can check the description of this episode and you can read the transcripts at axrp.net. Well, welcome to AXRP, John.

**John Wentworth:**
Thank you. Thank you to our live studio audience for showing up today.

## Agency in E. coli <a name="e-coli-agency"></a>

**Daniel Filan:**
So I guess the first thing I'd like to ask is I see you as being interested in resolving confusions around agency, or things that we don't understand about agency. So what don't we understand about agency?

**John Wentworth:**
Whew. All right. Well, let's start chronologically from where I started. I started out, first, in two directions in parallel. One of them was in biology, like systems biology and, to some extent, synthetic biology, looking at E. coli. Biologists always describe E. coli as collecting information from their environment and using that to keep an internal model of what's going on with the world, and then making decisions based on that in order to achieve good things.

**John Wentworth:**
So they're using these very agency-loaded intuitions to understand what's going on with the E. coli, but the actual models they have are just these simple dynamical systems, occasionally with some feedback loops in there. They don't have a way to take the low-level dynamics of the E. coli and back out these agency primitives that they're talking about, like goals and world models and stuff.

**Daniel Filan:**
Sorry, E. coli, it's a single-celled organism, right?

**John Wentworth:**
Yes.

**Daniel Filan:**
Is the claim that the single cells are taking in information about their environment and like ... Right?

**John Wentworth:**
Yes. One simple example of this is chemotaxis. So you've got this E. coli. It's swimming around in a little pool of water and you drop in a sugar cube. There'll be a chemical gradient of sugar that drops off as you move away from the grain of sugar. The E. coli will attempt to swim up that gradient, which is actually an interesting problem because when you're at a length scale that small, the E. coli's measurements of the sugar gradient are extremely noisy. So it actually has to do pretty good tracking of that sugar gradient over time to keep track of whether it's swimming up the gradient or down the gradient.

**Daniel Filan:**
Is it using some momentum algorithm, or is it just accepting the high variance and hoping it'll wash out over time?

**John Wentworth:**
It is essentially a momentum algorithm.

**Daniel Filan:**
Okay, which is basically, roughly, continuing to move in the direction it used to move, something like that.

**John Wentworth:**
Yes. Basically it tracks the sugar concentration over time. If it's trending upwards, it keeps swimming and if it's trending downwards, it just stops and tumbles in place, and then goes in a random direction. This is enough for it. If you drop a sugar cube in a tray of E. coli, they will all end up converging towards that sugar cube pretty quickly.

**Daniel Filan:**
Sure. So how could a single-cell organism do that?

**John Wentworth:**
I mean it's got 30,000 genes. There's a lot of stuff going on in there. The specifics in this case, what you have is there's this little sensor for sugar and there's a few other things it also keeps track of. It's this molecule on the surface of the cell. It will attach phosphate groups to that molecule and it will detach those phosphate groups at a regular rate and attach them whenever it senses a sugar molecule or whatever.

**John Wentworth:**
So you end up with the equilibrium number of phosphate molecules on there tracking what's going on with the sugar outside. Then there's an adaptation mechanism, so that if the sugar concentration is just staying high, the number of phosphates on each of these molecules will drop back to baseline. But if the sugar concentration is increasing over time, the number of phosphates will stay at a high level. If it's decreasing over time, the number of phosphates will stay at a low level.

**Daniel Filan:**
Oh, okay.

**John Wentworth:**
So it can keep track of whether concentrations are increasing or decreasing as it's slipping.

**Daniel Filan:**
And so, somehow it's using the structure of the fact that it has a boundary and it can just have stuff on the boundary that represents stuff that's happening at that point in the boundary, and then it can move in a direction that it can sense.

**John Wentworth:**
Yup. Then obviously there's some biochemical signal processing downstream of that, but that's the basic idea.

**Daniel Filan:**
Okay. All right. That's pretty interesting. But you were saying that you were learning about E. coli, and I guess biologists would say that it was making inferences and had goals, but their models of the E. coli were very simple, dynamical systems. I imagine you were going to say something going from there.

**John Wentworth:**
Yeah. So then you end up with these giant dynamical systems models where it's very much like looking at the inside of a neural net. You've got these huge systems that just aren't very interpretable. Intuitively, it seems like it's doing agenty stuff, but we don't have a good way to go from the low-level dynamical systems representation to it's trying to do this, it's modeling the world like that. So that was the biology angle.

## Agency in financial markets <a name="financial-markets-agency"></a>

**John Wentworth:**
I was also, around the same time, looking at markets through a similar lens, like financial markets. The question that got me started down that route was you've got the efficient market hypothesis saying that these financial markets are extremely inexploitable. You're not going to be able to get any money out of them.

**John Wentworth:**
Then you've got the coherence theorems saying when you've got a system that does trades and stuff, and you can't pump any money out of it, that means it's an expected utility maximizer.

**Daniel Filan:**
Okay.

**John Wentworth:**
So then, obvious question: what's the utility function and the Bayesian model for a financial market? The coherence theorems are telling me they should have one. If I could model this financial market as being an expected utility maximizer, that would make it a lot simpler to reason about in a lot of ways. It'd be a lot simpler to answer various questions about what it's going to do or how it's going to work.

**Daniel Filan:**
Sure.

**John Wentworth:**
So I dug into that for a while and eventually ran across this result called non-existence of a representative agent, who says that, lo and behold, even ideal financial markets, despite your complete inability to get money out of them, they are not expected utility maximizers.

**Daniel Filan:**
So what axioms of the coherence theorems do they violate?

**John Wentworth:**
So the coherence theorems implicitly assume that there's no path dependence in your preferences. It turns out that once you allow for path dependence, you can have a system which is inexploitable, but does not have an equivalent utility function.

**John Wentworth:**
So, for instance, in a financial market, what could happen is ... So you've got Apple and Google stocks and you start out with a market that's holding 100 shares of each. So you've got this market of traders and in aggregate they're holding 100 shares each of Apple and Google.

**Daniel Filan:**
Okay. This just means that Apple has issued 100 stocks and Google has issued 100 stocks, right?

**John Wentworth:**
So in principle, there could be some stocks held by entities that are outside of the market.

**Daniel Filan:**
That just never trade?

**John Wentworth:**
Exactly.

**Daniel Filan:**
Okay, cool.

**John Wentworth:**
Yeah.

**Daniel Filan:**
I guess, in practice, probably founder shares or something like that.

**John Wentworth:**
In practice, there's just tons of institutions that hold things long term and never trade. So they're out of equilibrium with the market most of the time.

**Daniel Filan:**
Okay.

**John Wentworth:**
But, anyway, you've got everyone that's in the market and keeping in equilibrium with each other and trading with each other regularly. They're holding 100 shares each. Now they trade for a while and they end up with 150 shares of Apple and 150 shares of Google in aggregate.

**John Wentworth:**
Then the question is at what prices are they willing to continue trading? So how much are they willing to trade off between Apple and Google? Are they willing to trade one share of Apple for one share of Google? Are they willing to trade two shares of Apple for one share of Google? What trade-offs should they willing to accept?

**John Wentworth:**
It turns out that what happens depends on the path by which this market went from 100 shares of Apple and 100 shares of Google to 150 shares of Apple and 150 shares of Google. One path might end up with them willing to accept a two-to-one trade. The other path might end up with them willing to accept a one-to-one trade. So which path the market followed to get to that new state determines which trade-offs they're willing to take in that new state.

**Daniel Filan:**
Can you tell us what's an example of one path where they trade at one-to-one and one path where they trade at two-to-one?

**John Wentworth:**
I can't give you a numerical example off the top of my head because it's messy as all hell. But the basic reason this happens is that it matters how the wealth is distributed within the market. So if a bunch of trades happen, which leaves someone who really likes Apple stock with a lot more of the internal wealth, they end up with more aggregate shares, then you're going to end up with prices more heavily favoring Apple. Whereas if it's trading along a path that leaves someone who likes Google a lot with a lot more wealth, then you're going to end up more heavily favoring Google.

## Inferring agency in real-world systems <a name="inferring-agency-real-world"></a>

**Daniel Filan:**
Okay. So that makes sense. All right. So we've mentioned that in biology, we have these E. coli cells that are somehow pursuing goals and making inferences or something like that, but the dynamic models of them seem very simple and it's not clear what's going on there. And we've got financial markets that you might think would have to be coherent expected utility maximizers because of math, but they're not, but you're still getting a lot of stuff out of them that you wanted to get out of expecting utility maximizers. You think there's something we don't understand there.

**John Wentworth:**
Yeah. So in both of these, you have people bringing in these intuitions and talking about the systems as having goals or modeling the world or whatever. People talk about markets this way. People talk about organisms this way. People talk about neural networks this way. They sure do intuitively seem to be doing something agenty. These seem like really good intuitive models for what's going on, but we don't know the right way to translate the low-level dynamics of the system into something that actually captures the semantics of agency.

**Daniel Filan:**
Okay. Is that roughly what you're interested in as a researcher?

**John Wentworth:**
Basically, yeah.

**Daniel Filan:**
Okay. Cool.

**John Wentworth:**
So what I want to do is be able to look at all these different kinds of systems and be like, "Intuitively, it looks like it's modeling the world. How do I back out something that is semantically its world model?"

**Daniel Filan:**
Okay, cool. Why is that important?

**John Wentworth:**
Well, I'll start with the biology, and then we can move via analogy back to AI. So in biology, for instance, the problem is an E. coli has, what, 15, 20 thousand genes? A human has 30,000. It's just this very large dynamical system, which you can run on a computer just fine, but you can't really get a human understanding of what's going on in there just by looking at the low-level dynamics of the system.

**John Wentworth:**
On the other hand, it sure does seem like humans are able to get an intuitive understanding of what's going on in E. coli. They do that by thinking of them as agents. This is how most biologists are thinking about them, how most biologists are driving their intuition about how the system works.

**Daniel Filan:**
Okay.

**John Wentworth:**
So to carry that over to AI, you've got the same thing with neural networks. They're these huge, completely opaque systems, giant tensors. They're too big for a human to just look at all the numbers and the dynamics and understand intuitively what's going on.

**John Wentworth:**
But we already have these intuitions that these systems have goals, these systems are trying to do things, or at least we're trying to make systems which have goals and try to do things. It seems like we should be able to turn that intuition into math. The intuition is coming from somewhere. It didn't come from just magic out of the sky. If it's understandable intuitively, it's going to be understandable mathematically.

**Daniel Filan:**
And so, I guess there are a few questions I have about this. One thing you could try to do is you could try and really understand ideal agency or optimal agency, like what would be the best way for an agent to be?

**Daniel Filan:**
I think the reason people like to do that is they're like, "Well, we're going to try and build agents well. We're going to try and have agents that figure out how to be the best agents they can be. So let's just figure out what the best possible thing is in this domain. Whereas I see you as more saying, "Okay, let's just understand the agenty stuff we have around us, like E. coli."

**Daniel Filan:**
But I think an intuition you might have is, well, super powerful AI that we care about understanding, maybe that's going to be more like ideal AI than it's going to be like E. coli. I'm wondering what your take is on that.

**John Wentworth:**
So, first of all, from my perspective, the main reason you want to understand ideal agents is because it's a limiting case which you expect real agents to approach. You would expect humans to be closer to an ideal agent than an E. coli. You expect both of these to be much closer to an ideal agent than a rock.

**John Wentworth:**
And so, it's just like in other areas of math. We understand the general phenomenon by thinking about the limiting case which everything else is approximating.

**John Wentworth:**
Now, from that perspective, there's the question of how do we get bits of information about what that ideal case is going to look like before we've worked out the math? In general, it's hard to find ideal mathematical concepts and principles. Your brain has to get the concepts from somewhere and get them all loaded into your head before you can actually figure out what the math is supposed to look like.

**John Wentworth:**
We don't go figuring these things out just by doing brute force search on theorem space. We go and figure these things out by having some intuition for how the thing is supposed to work, and then backing out what the math looks like from that. If you want to know what ideal agency is going to look like, if you want to get bits of information about that, the way you're going to do that is go look at real agents.

**John Wentworth:**
That also means if you go look at real agents and see that they don't match your current math for ideal agents, that's a pretty strong hint that something is wrong with that math. So like the markets example I talked about earlier. We had these coherence theorems that we're talking about - ideal agents, they should end up being expected utility maximizers. Then we go look at markets and we're like, "Well, these aren't expected utility maximizers, and yet we cannot money pump them. What's going on here?" right?

**John Wentworth:**
Then what we should do based on that is back-propagate, be like, okay, why did this break our theorems? Then go update the theorems. So it updates our understanding of what the right notion of ideal agency is.

**Daniel Filan:**
Okay, that makes sense. I guess the second question I have is why think of this as a question of agency exactly? So we're in this field called artificial intelligence, or I am. I don't know if you consider yourself to be. But you might think that, oh, the question is we're really interested in smart things. What's up with smartness or intelligence?

**John Wentworth:**
Yup.

**Daniel Filan:**
Or you might think, "Oh, I want to understand microeconomics" or something, how do I get a thing that achieves its goal in the real world by making trades with other intelligent agents or something? So, yeah, why pick out agency as the concept to really understand?

**John Wentworth:**
So I wouldn't say that agency is the concept to understand so much as it's the name for that whole cluster. If you're Isaac Newton in 1665 or '66, or whenever it was, trying to figure out basic physics, you've got concepts like force and mass and velocity and position and acceleration, and all these things. There's a bunch of different concepts and you have to figure them all out at once and see the relationship between them in order to know that you've actually got the right concept, like F=ma. That's when you know you've nailed down the right concepts here, because you have this nice relationship between them.

**John Wentworth:**
So there's these different concepts like intelligence, agency, optimization, world models, where we need to figure these things out more or less simultaneously and see the relationships between them, confirm that those relationships work the way we intuitively expect them to in order to know that we've gotten the models right. Make sense?

## Selection theorems <a name="selection-theorems"></a>

**Daniel Filan:**
I think that makes sense. Thinking about how you think about agency, I get the sense that you're interested in selection theorems.

**John Wentworth:**
Yeah, that was a bad name. I really need better marketing for that. Anyway, go on.

**Daniel Filan:**
Well, why are they important and maybe what's the right name for them?

**John Wentworth:**
Okay. So the concept here is when you have some system that's selected to do something interesting. So in evolution, you have natural selection selecting organisms to reproduce themselves, or in AI, we're using stochastic gradient descent to select a system which optimizes some objective function on some data.

**John Wentworth:**
It seems intuitively like there should be some general principles that apply to systems which pop out of selection pressure. So, for instance, the coherence theorems are an example of something that's trying to express these sorts of general principles. They're saying if you are Pareto optimal in some sense, then you should have a utility function.

**John Wentworth:**
That's the sort of thing where if you have something under selection pressure, you would expect that selection pressure to select for using resources efficiently, for instance. Therefore, things are going to pop out with utility functions, or at least that's the implicit claim here, right?

**Daniel Filan:**
Okay.

**John Wentworth:**
The thing I chose the poor name of selection theorems for was this more general strategy of trying to figure out properties of systems which pop out of selection like this. So more general properties. For instance, in biology, we see very consistently that biological organisms are extremely modular.

**John Wentworth:**
[Some of your work](https://arxiv.org/abs/2103.03386) has found something similar in neural networks. It seems like a pretty consistent pattern that modularity pops out of systems under selection pressure. So then the question is when and why does that happen? That's a general property of systems that are under selection pressure. If we want to understand that property, when and why does it happen?

**Daniel Filan:**
Okay. Thinking of modularity specifically, how satisfied are you with our understanding of that? Do you think we have any guesses? Are they good?

**John Wentworth:**
So, first of all, I don't think we even have the right language yet to talk about modularity. For instance, when people are looking at modularity and neural networks, they're usually just using these graph modularity measures, which they're like-

**Daniel Filan:**
Or when I do it.

**John Wentworth:**
Yeah. I mean everybody else does the same thing. It's sort of a hacky notion. It's [accurate] enough that if you see this hacky measure telling you that it's very modular, then you're like, "Yeah, right. That's pretty modular," but it doesn't really feel like it's the right way to talk about modularity. What we want to talk about is something about information flowing between subsystems, which we don't have a nice way to quantify that.

**John Wentworth:**
Then the second step on top of that is once you do have the right language in which to talk about it, you'd expect to find theorems saying things like if you have this sort of selection pressure, then you should see this sort of modularity pop out.

**Daniel Filan:**
I guess an obvious concern is that if we don't have the right language for modularity, then presumably there's no theorem in which that language appears. And so, you're not talking about the true modularity?

**John Wentworth:**
It's not necessarily that there'd be no theorem. You might expect to find some really hacky, messy theorems, but it's generally not going to be real robust. If it's the wrong concept of modularity, there's going to be systems that are modular but don't satisfy this definition, or systems which do satisfy this definition but are not actually that modular in some important way.

**John Wentworth:**
It's going to be sort of at cross-purposes to the thing that the selection pressure is actually selecting for. So you're going to get relatively weak conditions in the theorem. Or, sorry, you'll need overly restrictive preconditions in the theorem and then you'll get not very good post-conditions, right?

**Daniel Filan:**
Okay, cool. So the theorem is going to assume a lot, but not produce a lot.

**John Wentworth:**
Exactly.

## (Natural) abstractions <a name="natural-abstractions"></a>

**Daniel Filan:**
Yeah. So speaking of modularity, I understand a lot of your work is interested in this idea of abstraction. I'm not quite sure if you think of abstraction as closely related to modularity or analogical, or just another thing out there.

**John Wentworth:**
They are distinct phenomena, the way I think about them. Abstraction is mostly about the structure of the world, whereas the kinds of modularity I'm talking about in the context of selection theorems is mostly about the internal structure of these systems under selection pressure. That said, they do boil down to very analogous concepts. It's very much about interacting more with some chunk of stuff and not very much with the stuff elsewhere, right?

**Daniel Filan:**
Yeah. It seems like you might also hope for some kind of connection, where if the real world supports some kind of good abstractions, you might hope that I organize my cognition in a modular way where I've got one module per abstraction or something like that.

**John Wentworth:**
Right. So this is exactly why it's important that they're distinct concepts, because you want to have a non-trivial claim that the internal organization of the system is going to be selected to reflect the external, natural abstractions in the environment.

**Daniel Filan:**
Cool. So before we get into what's going on with abstraction, first I'd like to ask, why should we care about abstraction? How does it relate to the task of understanding agency?

**John Wentworth:**
So there's a general challenge here in communicating why it's important, which is you have to go a few layers down the game tree before you run into it. So if you're a biologist starting out trying to understand how E. coli are agents, or if you're an AI person just working on interpretability of neural nets, or if you're at [MIRI](https://intelligence.org/) trying to figure out principles of decision theory or embedded agents or whatever, it's not immediately obvious that abstraction is going to be your big bottleneck.

**John Wentworth:**
You start to see it when you're trying to formulate what an agent even is. There's the Cartesian boundary, is the key thing here, the boundary between the inside of the agent and the outside of the agent. You're saying this part of the world is the agent, the rest of the world is not to the agent. It's the environment. But that's a very artificial thing. There's no physically fundamental barrier that's the Cartesian boundary in the physical world, right?

**John Wentworth:**
So then the question is where does that boundary come from? It sure conceptually seems to be a good model, and that's where abstraction comes in. An agent is an abstraction, and that Cartesian boundary is essentially an abstraction boundary. It's like the conceptual boundary through which this abstraction is interacting with the rest of the world. So that's a first reason you might run into it if you're thinking specifically about agency.

**Daniel Filan:**
So going there, if an agent is an abstraction, it doesn't seem like it has to follow that we need to care a lot about abstractions, just because - the word agent is a word, but we don't have to worry too much about the philosophy of language to understand agents just because of that, right?

**John Wentworth:**
Potentially yes. So there are certainly concepts where we don't have to understand the general nature of concepts or of abstraction in order to formalize the concept. Like in physics, we have force and mass and density and pressure and stuff. Those we can operationalize very cleanly without having to get into how abstraction works.

**John Wentworth:**
On the other end of the spectrum, there's things where we're probably not going to be able to get clean formulations of the concept without understanding abstraction in general, like 'water bottle' or 'sunglasses'.

**Daniel Filan:**
For those not aware, John has a water bottle and sunglasses near him, and is waving them kindly.

**John Wentworth:**
Yes. And so, you could imagine that agency is in the former category, where there are nice operationalizations we could find without having to understand abstraction in general. Practically speaking, we do not seem to be finding them very quickly. I certainly expect that routing through at least some understanding of abstraction first will get us there much faster.

**Daniel Filan:**
Really?

**John Wentworth:**
In part I expect this because I've now spent a while understanding abstraction better and I'm already seeing the returns on understanding agency better.

**Daniel Filan:**
Okay. Cool. One thing that seems strange there ... I don't know. Maybe it's not strange, but, naively, it seems strange that there's some spectrum from things where you can easily understand the abstraction, like pressure, to things where abstraction fits, but just close to our border of understanding what an abstraction is. And there's the zone of things, which are just not actually good abstractions. Which unfortunately I can't describe because things for which there are good words tend to be useful abstractions, but -

**John Wentworth:**
Well, there are words that are under social pressure to be bad abstractions. If we want to get into politics.

**Daniel Filan:**
Yeah. Well, I don't. I don't know, I guess in the study of - sometimes in philosophy, people talk about the composite object that's this particular random bit of Jupiter and this particular random bit of Mars and this particular random bit of your hat - not a good abstraction.

**John Wentworth:**
Yes.

**Daniel Filan:**
So there's this whole range of how well things fit into abstraction. And it seems like agency's in this border zone, what's up with that? Isn't that like a weird coincidence?

**John Wentworth:**
I don't think it's actually in a border zone. I think it's just squarely on the clean side of things.

**Daniel Filan:**
Okay.

**John Wentworth:**
We just haven't yet figured out the language. And doing it the old fashioned way is really slow. One of my go-to examples here is [Shannon](https://en.wikipedia.org/wiki/Claude_Shannon) inventing information theory.

**Daniel Filan:**
All right.

**John Wentworth:**
He found these really nice formalization of some particular concepts we had and he spent 10 years figuring things out and was this incredible genius. We want to go faster than that. That guy was a once in 50 years, maybe once in 100 years genius and figured out four or five important concepts. We want to get an iterative loop going and not be trundling along at this very slow pace of theorizing.

**Daniel Filan:**
All right. Do you think there's just a large zone of things that are squarely within the actual concept of abstraction, but our understanding of abstraction is something a low dimensional manifold, like a line on a piece of paper where the line takes up way less area than the paper?

**John Wentworth:**
I'm not sure what you're trying to gesture at.

**Daniel Filan:**
So do you think that it's just something like, there are tons of concepts -

**John Wentworth:**
Yes.

**Daniel Filan:**
... that are solidly abstractions, but we don't have solid language to understand why they're abstractions. So it's not surprising that agency is one of those.

**John Wentworth:**
Yes. I should distinguish here between... There's things which are mathematical abstractions, like agency or information and there's things that are physical abstractions like 'water bottle' or 'sunglasses'. The mathematical ones are somewhat easier to get at. Because in some sense all you really need is the theory of abstraction, and then the rest is just math. The physical ones potentially, you still need a bunch of data and a bunch of priors about our particular world or whatever.

**Daniel Filan:**
All right. So that was one way we could have got to the question of abstraction. You said there was something to do with biology as well?

**John Wentworth:**
Oh, there's absolutely other paths to this. So this was roughly the path that I originally came in through. You could also come in through something like, if we want to talk about human values and have robust formulations of human values, the things that humans care about themselves tend to be abstractions. Like I care about my pants and windows and trees and stuff. I don't care so much about quantum field fluctuations.

**John Wentworth:**
Quantum field fluctuations, there are lots of them, most of them are not very interesting. So the things humans care about are abstractions. So if you're thinking about learning human values, having a good formulation of what abstractions are is going to massively exponentially narrow down the space of things you might be looking for.

**John Wentworth:**
You know we care about these particular high level abstractions.You no longer have to worry about all possible value functions over quantum fields. Right?

**Daniel Filan:**
Sure.

**John Wentworth:**
You can worry about macroscopic things. Similarly for things like talking about 'impact', the sort of impact we care about is things affecting big macroscopic stuff. We don't care about things affecting rattlings of molecules in the air. So again, there it's all about the high level abstractions are the things we care about. So we want to measure impact in terms of those. And if you have a good notion of abstraction, a robust notion of abstraction, then you can have a robust notion of impact in terms of that.

**Daniel Filan:**
Okay, cool. And so what would it look like to understand enough about abstraction that we could satisfy multiple of these divergent ways to start caring about abstraction?

**John Wentworth:**
Yeah. So one way to think of it is you want a mathematical model of abstraction, which very robustly matches the things we think of as abstraction. So for instance, it should be able to look at Shannon's theory of information and these measures of mutual information and channel capacity and say, "Ah, yes, these are very natural abstractions in mathematics." It should also be able to look at a water bottle and be like, "Yes, in this universe, the water bottle is a very natural abstraction", and this should robustly extend to all the different things we think of as abstractions.

**John Wentworth:**
It should also capture the intuitive properties we expect out of abstractions. So, if I'm thinking about water bottles, I expect that I should be able to think of it as a fairly high level object maybe made out of a few parts. But I don't have to think about all the low level goings on of the material the water bottle is made of in order to think about the water bottle.

**John Wentworth:**
I can pick it up and move it around. Nice thing about the water bottle is it's a solid object. So there's like six parameters I need to summarize the position of the water bottle, even though it has [a mole](https://en.wikipedia.org/wiki/Avogadro_constant) of particles in there. Right?

**Daniel Filan:**
Yeah. Well, I guess you also need to know whether the lid is open or closed.

**John Wentworth:**
Yes. That would add another six dimensions or so, probably less than six because it's attached there.

**Daniel Filan:**
Yeah. Yeah.

**John Wentworth:**
So yeah, that'll add a few more dimensions. But we're talking single, maybe low double digit numbers of dimensions to summarize the water bottle in this sense.

**Daniel Filan:**
Yeah. Way fewer than the -

**John Wentworth:**
We're not talking a mole. Definitely not a mole.

**Daniel Filan:**
Okay, cool. So basically something like, we want to understand abstraction well enough to just take anything and be like, okay, what are the good abstractions here, and know that we know that.

**John Wentworth:**
And this should robustly match our intuitive concept of what abstractions are. So when we go use those intuitions in order to design something, it will robustly end up working the way we expect it to, insofar as we're using these mathematical principles to guide it.

**Daniel Filan:**
Okay. At this point, I'd like to talk about your work on abstraction, which I think of under the heading of [the natural abstraction hypothesis](https://www.lesswrong.com/posts/Nwgdq6kHke5LY692J/alignment-by-default#Unsupervised__Natural_Abstractions). So first of all, can you tell us what is the natural abstraction hypothesis?

**John Wentworth:**
So there's a few parts to it. The first part is the idea that our physical world has some broad class of abstractions which are natural in the sense that a lot of different minds, a lot of different kinds of agents under selection pressure in this world will converge to similar abstractions or roughly the same abstractions.

**John Wentworth:**
So that's the first part. The second part, an obvious next guess based on that, is that these are basically the abstractions which humans use for the most part. And then the third part, which is where the actual math I do comes into it, is that these abstractions mostly come from information which is relevant at a distance. So there's relatively little information about any little chunk of the universe, which propagates to chunks of the universe far away.

## Information at a distance <a name="info-at-a-distance"></a>

**Daniel Filan:**
All right. And what's the status of the natural abstraction hypothesis? Is it a natural abstraction theorem yet? Or how much of it have you knocked off?

**John Wentworth:**
So I do have some pretty decent theorems at this point within worlds that are similar to ours in the sense that they have local dynamics. So in our world, there's a light speed limit. You can only directly interact with things nearby in spacetime. Anything else has to be indirect interactions that propagate.

**John Wentworth:**
Universes like that have this core result called [the telephone theorem](https://www.lesswrong.com/posts/jJf4FrfiQdDGg7uco/information-at-a-distance-is-mediated-by-deterministic), which basically says that the only information which will be relevant over a long distance is information that is arbitrarily perfectly conserved as it propagates far away. So that means that most information presumably is not arbitrarily perfectly conserved. [All of that information will be lost](https://www.youtube.com/watch?v=NoAzpa1x7jU). It gets wiped out by noise.

**Daniel Filan:**
Okay. I guess one question is, so part of the hypothesis was that a lot of agents would be using these natural abstractions. What counts as a lot and what class of agents are you imagining?

**John Wentworth:**
Good question. So, first of all, this is not a question which the math is explicitly addressing yet. This is intended to be addressed later. I'm mostly not explicitly talking about agents yet. That said, very recently I've started thinking about agents as things which optimize stuff at a distance. So in the same way that abstraction is talking about information relevant at a distance, we can talk about agents which are optimizing things far away. And presumably most of the agenty things that are interesting to us are going to be interesting because they're optimizing over a distance.

**John Wentworth:**
If something is only optimizing a tiny little chunk of the world and a one centimeter cube around it, we mostly don't care that much about that. We care insofar as it's affecting things that are far away. So in that case, this thing about only so much information propagates over a distance, well obviously that's going to end up being the information that you end up caring about influencing for optimization purposes. And indeed the math bears that out. If you're trying to optimize things far away, then it's the abstractions that you care about because that's the only channel you have to interact with things far away.

**Daniel Filan:**
Sure. What's the relevant notion of far away that we're using here?

**John Wentworth:**
So we're modeling the world as a big causal graph. And then we're talking about nested layers of [Markov blankets](https://en.wikipedia.org/wiki/Markov_blanket) in that causal graph. So physically in our particular universe our causal graph follows the structure of spacetime - things only interact with things that are nearby in this sort of little four-dimensional sphere of spacetime around the first thing.

**John Wentworth:**
So you can imagine little four-dimensional spheres of space time nested one within the other. So you've got like a sequence of nested spheres. And this would be a sequence of Markov blankets that as you go outwards through these spheres, you get further and further away. And that's the notion of distance here.

**John Wentworth:**
Now, it doesn't necessarily have to be spherical. You could take any surface you want. They don't even have to be surfaces in our four dimensional space time. They could be more abstract things than that. The important thing is that things inside the blanket only interact with the things outside the blanket via the blanket itself. It has to go through the blanket.

**Daniel Filan:**
So it sounds like the way this is going to work is that you can get a notion of something like locality out of this. But it seems like it's going to be difficult to get a notion of, is it three units of distance away or is it 10 units of distance away? Because your concentric spheres can be closely packed or loosely packed.

**John Wentworth:**
Correct. Yeah. So, first of all, the theorem doesn't explicitly talk about distance per se. It's just talking about limits as you get far away. I do have other theorems now that don't require those nested sequences of Markov blankets at all. But there's... Yeah. It's not really about the distance itself. It's about defining what far away means.

**Daniel Filan:**
Okay, sure. That makes sense. And I guess one thing which occurs to me is I would think that sometimes systems could have different types of information going in different directions, right?

**John Wentworth:**
Yes. Absolutely.

**Daniel Filan:**
So you might imagine, I don't know, a house with two directional antennas and in one it's beaming out [The Simpsons](https://en.wikipedia.org/wiki/The_Simpsons) and the other it's beaming out... I don't know, the heart rate of everyone in the house or something.

**John Wentworth:**
And then you'll also have a bunch of other directions which aren't beaming out anything.

**Daniel Filan:**
Yeah. So what's up with that? Naively it doesn't seem like it's handled well by your theory.

**John Wentworth:**
So if you're thinking of it in terms of these nested sequences of Markov blankets, you could draw these blankets going along one direction or along a different direction or along another direction.

**John Wentworth:**
And depending on which direction you draw of them along, you may get different abstractions corresponding to information which is conserved or lost along that direction, basically. There's the versions of this which don't explicitly talk about the Markov blankets. Instead, you just talk about everything that's propagating in every direction. And then you'll just end up with some patch of the world that has a bunch of information shared with this one. And if there's a big patch of the world, that means it must have propagated to a fairly large amount of the world.

**Daniel Filan:**
All right. So in that one, is it just saying like, yeah, here's a region of the world that's somehow linked with my house?

**John Wentworth:**
Yeah. If we take the example of the directional antenna beaming The Simpsons from your house, there's going to be some chunk of air going in sort of line or cone or something away from your house which has a very strong signal of The Simpsons playing. And that whole region of spacetime is going to have a bunch of mutual information that's like The Simpsons episode.

**John Wentworth:**
And then the stuff elsewhere is not going to contain all that information. So there's this natural abstraction, which is sort of localized to this particular cone in the same way that for instance, if I have a physical gear and a gear box, there's going to be a bunch of information about the rotation speed of that gear, which is sort of localized to the gear.

**Daniel Filan:**
Yeah. But it wouldn't be localized to somebody standing outside the box.

**John Wentworth:**
Exactly.

## Why the natural abstraction hypothesis matters <a name="why-nah-matters"></a>

**Daniel Filan:**
Okay. So can you just spell it out for us again, why does the natural abstraction hypothesis matter?

**John Wentworth:**
So if you have these natural abstractions that lots of different kinds of agents are going to converge to, then first it gives you a lot of hope for some sort of interpretability, or some sort of being able to communicate with or understand what other kinds of agents are doing. So for instance, if the natural abstraction hypothesis holds, then we'd expect to be able to look inside of neural networks and find representations of these natural abstractions.

**Daniel Filan:**
If we're good enough at looking.

**John Wentworth:**
Yeah. If we have the math and we know how to use it. It should be possible. Another example, there's this puzzle: babies are able to figure out what words mean with one or two examples. You show baby an apple, you say "apple", pretty quickly it's got this apple thing down. However, if you think about how many possible apple classifier functions there are on a one megabyte image. How many such functions are there? It's going to be two to the two to a million.

**Daniel Filan:**
Sure.

**John Wentworth:**
In order to learn that apple classifier by brute force, you would need about two to a million bits, two to a million samples of, does this contain an apple or not. Babies do it with one or two. So clearly the place we're getting our concepts from is not just brute force classification. We have to have some sort of prior notion of what sorts of things are concepty, what sorts of things we normally attach words to. Otherwise, we wouldn't have language at all. It just [big-O](https://en.wikipedia.org/wiki/Big_O_notation) algorithmically would not work. And the question's like, "All right, what's going on there? How is the baby doing this?" And natural abstractions would provide an obvious natural answer for that.

**Daniel Filan:**
Yeah. I guess it seems like this is almost like... So in statistical learning theory, sometimes people try to prove generalization bounds on learning algorithms by saying, "Well, there's only a small number of things this algorithm could have learned. And the true thing is one of them. So if it gets the thing right on the training data set, then there's only so many different ways it could wiggle on the test data set. And because there's a small number of them, then it's probably going to get the right one on the test data set."

**Daniel Filan:**
And there's this famous problem, which is that neural networks can express a whole bunch of stuff. But yeah. It seems like one way I could think of the natural abstraction hypothesis is as saying, well, like they'll just tend to learn the natural abstractions, which is this smaller hypothesis class. And that's why statistical learning theory can work at all.

**John Wentworth:**
Yeah. So this is tying back to the selection theorems thing. One thing I expect is a property that you'll see in selected systems is you'll get either some combination of broad peaks in the parameter space or robust circuits or robust strategies. So strategies which work even if you change the data a little bit. Some combination of these two, I expect selects heavily for natural abstractions.

**John Wentworth:**
You can have optimal strategies, which just encrypt everything on the way in, decrypt everything on the way out. And it's total noise in the middle. So clearly you don't have to have a lot of structure there just to get an optimal behavior. But when we go look at systems, humans sure do seem to converge on similar abstractions. We have this thing about the babies. So it seems like that extra structure has to come from some combination of broadness and robustness rather than just optimality alone.

**Daniel Filan:**
Yeah. So there are actually two things this reminds me of, that I hope you'll indulge me in and maybe comment on. So the first thing is there's [this finding](https://arxiv.org/abs/1611.03530) that neural networks are really good at classifying images, but you can also train them to optimality when you just make images by selecting random values for every pixel and giving them random classes. And neural networks will in fact be able to memorize that data set. [But it takes them a bit longer](https://arxiv.org/abs/1706.05394) than it takes them to learn actual data sets of real images, which seems like it could be related to the idea that somehow it's harder for them to find the unnatural abstractions.

**Daniel Filan:**
The second thing is I have a colleague - well, a colleague in the broad sense - he's a professor at the university of Oxford called Jakob Foerster who's interested in coordination, particularly [zero-shot coordination](https://arxiv.org/abs/2003.02979).

**Daniel Filan:**
So imagine like I train my AI bot, you train your AI bot. He's interested in like, "Okay, what's a good rough algorithm we could use? Where [even if we had slightly different implementations](https://arxiv.org/abs/2106.06613) and slightly different batch sizes and learning rates and stuff, our bots would still get to play nicely together." And a lot of what he's worried about is ruling out these strategies that - if I just trained a bunch of bots that learned to play with themselves, then they would learn these really weird looking arbitrary encoding mechanisms. If I jiggle my arms slightly, this -

**John Wentworth:**
Honey bees do a little dance that tells the other bees where to fly,

**Daniel Filan:**
He actually uses exactly that example.

**John Wentworth:**
Nice.

**Daniel Filan:**
Or he did in the one talk I attended, and he's like, "No, we've got to figure out how to make it not do that kind of crazy stuff." So yeah. I'm wondering if you've thought about the relationship between natural abstraction hypothesis and coordination rather than just a single agent.

**John Wentworth:**
Yeah. So I've mostly thought about that in the concept of, how are humans able to have language that works at all? Different example, but it's the same principles. You've got this giant space and you need to somehow coordinate on something in there. There has to be some sort of prior notion of what the right things are.

## Unnatural abstractions in humans <a name="unnatural-abstractions-in-humans"></a>

**Daniel Filan:**
Cool. One question I have: a thing that seems problematic for the natural abstraction hypothesis is that sometimes people use the wrong abstractions and then change their minds. So like, I guess [a](https://www.lesswrong.com/posts/aMHq4mA2PHSM2TMoH/the-categories-were-made-for-man-not-man-for-the-categories) [classic](https://www.lesswrong.com/posts/esRZaPXSHgWzyB2NL/where-to-draw-the-boundaries) [example](https://www.lesswrong.com/posts/vhp2sW6iBhNJwqcwP/blood-is-thicker-than-water) of this is, is a whale a fish? In [some biblical texts](https://www.biblegateway.com/passage/?search=Jonah%201%3A17-2%3A10&version=ESV), whales are described as fish or things that seem like they probably are whales. But now we think that's the wrong way to define what a fish is or at least some people think that.

**John Wentworth:**
Yeah. So a few notes on this. First, it is totally compatible with the hypothesis that at least some human concepts, some of the time would not match the natural boundaries. It doesn't have to be a hundred percent of the time thing. It is entirely possible that people just missed somehow.

**John Wentworth:**
That said, a whale being a fish would not be an example of that. The tell-tale sign of that would be like, people are using the same word. And whenever they try to talk to each other, they just get really confused because nobody quite has the same concept there. That would be a sign that humans just are using a word that doesn't have a corresponding natural abstraction.

**John Wentworth:**
Next thing. There's the general philosophical idea that [words point to clusters in thing-space](https://www.lesswrong.com/posts/WBw8dDkAWohFjWQSk/the-cluster-structure-of-thingspace). And that still carries over to natural abstractions.

**Daniel Filan:**
Can you say what a cluster in thing-space is?

**John Wentworth:**
Sure. So the idea here is you've got a bunch of objects that are similar to each other. So we cluster them together. We treat these as instances of the cluster. But you're still going to have lots of stuff out on the boundaries. It's not like you can come along and draw a nice, clean dividing line between what is a fish and what is not a fish. There's going to be stuff out on the boundaries and that's fine. We can still have well defined words in the same sense mathematically that we can have well defined clusters.

**John Wentworth:**
So you can talk about the average fish or the sort of shape of the fish cluster - what are the major dimensions along which fish vary and how much do they vary along those dimensions? And you can have unambiguous well-defined answers to questions like what are the main dimensions along which fish vary without necessarily having to have hard dividing lines between what is a fish and what is not a fish.

**Daniel Filan:**
Okay. So in the case of whales, do you think that what happened is humans somehow missed the natural abstraction? Or do you think the version of fish that includes... Are you saying that whale is a boundary thing that's like-

**John Wentworth:**
Yes. So a whale, clearly it shares a lot of properties with fish. You can learn a lot of things about whales by looking at fish. It is clearly at least somewhat in that cluster. It's not a central member of that cluster, but it's definitely at least on the boundary. And it is also in the mammal cluster. It's at least on that boundary. Yeah. It's a case like that.

**Daniel Filan:**
So if that were true, I feel like... So biologists understand a lot about whales, I assume. And I hope I'm not just totally wrong about this, but my understanding is that biologists are not unsure about whether whales are fish. They're not like, "Oh, they're basically fish or -"

**John Wentworth:**
So once you start being able to read genetic code, the phylogeny, the branching of the evolutionary tree becomes the natural way to classify things. And it just makes way more sense to classify things that way for lots of purposes than any other way. And if you're trying to think about, say, the shape of whale bodies, then it is still going to be more useful to think of it as a fish than a mammal. But mostly what biologists are interested in is more metabolic stuff or what's going on in the genome. And for that much broader variety of purposes is going to be much more useful to think of it as a mammal.

**Daniel Filan:**
Okay. So if I tie this back into how abstractions work, are you saying something like the abstraction of fish is this kind of thing where whale was an edge case, but once you gain more information it starts fitting more naturally into the mammal zone?

**John Wentworth:**
I mean the right answer here is things that do not always unambiguously belong to one category and not another. And also the natural versions of these categories are not actually mutually exclusive. When you're thinking about phylogenetic trees, It is useful to think of them as mutually exclusive, but the underlying concepts are not.

## Probability, determinism, and abstraction <a name="probability-determinism-abstraction"></a>

**Daniel Filan:**
All right. Cool. I guess, digging in a little bit, when you say abstraction is information that's preserved over a distance, should I think of the underlying thing as: there's probabilistic things and there's some probabilistic dependence and there's a lot probabilistic independence or there's like... So is it just based on probabilistic stuff?

**John Wentworth:**
Yes. So the current formulation is all about probabilistic conditional independence. You have some information propagating conditional on that information, everything else is independent. That said, there are known problems with this. For instance, this is really bad for handling mathematical abstractions like what is a [group](https://en.wikipedia.org/wiki/Group_(mathematics)) or what is a [field](https://en.wikipedia.org/wiki/Field_(mathematics)). These are clearly very natural abstractions and this framework does not handle that at all. And I expect that this framework captures the fundamental concepts in such a way that it will clearly generalize once we have the language for talking about more mathy type things. But it's clearly not the whole answer yet.

**Daniel Filan:**
When you say "once we have the language for talking about more mathy type things", what do you mean there?

**John Wentworth:**
Probably category theory, but nobody seems to speak that language particularly well, so.

**Daniel Filan:**
So if we understood the true nature of maths, then we could understand how it's related to this stuff.

**John Wentworth:**
Yes. Probably if you had a sufficiently deep understanding of category theory and could take the probabilistic formulation of abstraction that I have and express it in category theory, then it would clearly generalize to all this other stuff in nice ways. But I'm not sure any person currently alive understands category theory well enough for that to work.

**Daniel Filan:**
How well would they have to understand it? Some people write textbooks about category theory, right? Is that not well enough?

**John Wentworth:**
Almost all of them are trash.

**Daniel Filan:**
Well, some of them aren't, apparently.

**John Wentworth:**
I mean, the best category theory textbooks I have seen are solidly decent. I don't think I have found anyone who has a really good intuition for category theoretic concepts.

**John Wentworth:**
There are lots of people who can crank the algebra and they can use the algebra to say some things in some narrow contexts, but I don't really know anyone who can go, I don't know, look at a giraffe and talk about the giraffe directly in category theoretic terms and say things which are non-trivial and actually useful. Mostly they can say very simple stupid things, which is a sign that you haven't really deeply understood the language yet.

**Daniel Filan:**
Okay. So there's this question of how does it apply to math. I also have this question of how it applies to physics. I believe that physics is actually deterministic, which you might believe if you were into [many-worlds quantum mechanics](https://en.wikipedia.org/wiki/Many-worlds_interpretation), or at least it seems conceivable that physics could have been deterministic, and yet maybe we would still have stuff that looks kind of like this.

**John Wentworth:**
Yeah. So in the physics case, we're kind of on easy mode because chaos is a thing. So for instance, a classic example here is a billiard system. So you've got a bunch of little hard balls bouncing around on a table perfectly elastically. So their energy isn't lost. The thing is every time there's a collision and then the balls roll for a while, and there's another collision, it basically doubles the noise and the angle of a ball, roughly speaking. So the noise grows exponentially over time and you very quickly become maximally uncertain about the positions of all the balls.

**Daniel Filan:**
When you say someone's uncertain about the positions of all the balls, why -

**John Wentworth:**
So meaning if you have any uncertainty at all in their initial conditions, that very quickly gets amplified into being completely uncertain about where they ended up. You'll have no idea.

## Whence probability in deterministic universes? <a name="whence-probs"></a>

**Daniel Filan:**
But why would anything within this universe have uncertainty about their initial conditions, given that it's a deterministic world and you can infer everything from everything else?

**John Wentworth:**
Well, _you_ can't infer everything from everything else. I mean, go look at a billiards table and try to measure the balls and see how precise you can get their positions and velocities.

**Daniel Filan:**
Yeah. What's going on there. Why-

**John Wentworth:**
Your measurement tools in fact have limited precision and therefore you will not have arbitrarily precise knowledge of those states.

**Daniel Filan:**
So when you say my measurement tools have limited precision, in a deterministic world, it's not because there's random noise that's washing out the signal or anything. So yeah. What do you mean by measurement tools have ...?

**John Wentworth:**
I mean, take a ruler. Look at the lines on the ruler. They're only so far apart. And your eyes, if you use your eye, you can get another 10th of a millimeter out of that ruler, but it's still, you can only measure so small, right?

**Daniel Filan:**
So is the claim something like there's chaos in the billiard balls, where if you change the initial conditions of the billiard balls very slightly, that makes a big change to where the billiard balls are, but -

**John Wentworth:**
Yeah, later on.

**Daniel Filan:**
- but I, as a physical system, maybe there are a whole bunch of different initial states of the billard balls that get mapped to the same mental state of me and that's why uncertainty is kicking in.

**John Wentworth:**
Yeah. You'll not be able to tell from your personal measurement tools, like your eyes and your rulers, whether the ball is right here or whether the ball is a nanometer to the side of right here. Then once those balls start rolling around, very quickly it's going to matter whether it was right here or an a nanometer to the side.

**Daniel Filan:**
Okay. So the story here is something like the way probabilities come into the picture at all, or the way uncertainty comes into the picture at all is because things are deterministic, but there's many to one functions and you're trying to invert them as a human, and that's why there's things being messed up. Yeah, I don't want to put you into the deep end of philosophy of physics too unnecessarily, but suppose I believe in many-worlds quantum mechanics, and the reason you should suppose that is that I do, in many-worlds quantum mechanics stuff is deterministic.

**John Wentworth:**
But do you [alieve](http://www.pgrim.org/philosophersannual/pa28articles/gendleraliefbelief.pdf) it?

**Daniel Filan:**
I think so.

**John Wentworth:**
Anyway, go ahead.

**Daniel Filan:**
So in many-worlds quantum mechanics, there's no such thing as many-to-one functions in terms of physical evolution, right? The jargon is that the time evolution is unitary, which means that two different initial states always turn into two different subsequent states.

**John Wentworth:**
Yeah. This is true in chaotic systems. There are lots of chaotic systems with this property.

**Daniel Filan:**
Okay. So what's going on with this idea that there's some many to one function that I'm trying to invert and that's why I'm uncertain?

**John Wentworth:**
Well, the many to one function is the function from the state of the system to your observations. It's not the function from state of system to future state of system.

**Daniel Filan:**
Okay. But I'm also part of the physical system of the world, right?

**John Wentworth:**
Yeah, yeah. Yes you are.

**Daniel Filan:**
So why doesn't the chaos mean that I have a really good measurement of the initial state? When you say there's chaos, right, that means the balls are doing an amazing job of measuring the initial state of the billiard balls, right?

**John Wentworth:**
In some sense, yeah.

**Daniel Filan:**
Why aren't I doing that amazing job?

**John Wentworth:**
Why aren't you doing that amazing job? Well, you could certainly imagine that you just somehow accidentally start out with a perfect measurement of the billiard ball state, and that would be a coherent world, that could happen.

**Daniel Filan:**
But it gets better and better over time.

**John Wentworth:**
Empirically, we observe that you do not do that.

**Daniel Filan:**
Okay. Yeah. What's going on?

**John Wentworth:**
Empirically, we observe that you have only very finite information about these billard balls, like the mutual information between you and the billard balls is pretty small, you could actually, if I want to be coy about it-

**Daniel Filan:**
Sorry, in a deterministic world, what do you mean the mutual information is very small?

**John Wentworth:**
So empirically, you could make predictions about what's going on with these billiard balls or guess where they are when they're starting out, and then also have someone else take some very precise nanometer measurements and see how well your guesses about these billiard balls map to those nanometer precision measurements. Then because you're making predictions about them, you can crank the math on a mutual information formula, assuming your predictions are coherent enough to imply probabilities. If they're not even doing that, then you're in stupid land. And your predictions are just total s**t. But yeah, you can get some mutual information out of that and then you can quantify how much information does Daniel have about these billiard balls. In the pre-Bayesian days, this was how people thought about it, right?

**Daniel Filan:**
Yeah. Okay. I totally believe that would happen, right? But I guess the question is, how can that be what's happening in a deterministic world? That I still don't totally get.

**John Wentworth:**
I mean, what's missing?

**Daniel Filan:**
So we have this deterministic world, a whole bunch of stuff is chaotic, right?

**John Wentworth:**
Yeah.

**Daniel Filan:**
What chaos means is that everything is measuring everything else very, very well.

**John Wentworth:**
No, it does not mean that everything is measuring everything else very, very well. What it means is that everything is measuring some very particular things very, very well. So for instance - well, okay, not for instance. Conceptually, what happens in chaos is that the current macroscopic state starts to depend on less and less significant bits in the initial state. So if you just had a simple one-dimensional system that starts out with this real number state, and you're looking at the first 10 bits in timestep one, then you look at the first 10 bits in timestep two, well, the first 10 bits in timestep two are going to depend on the first 11 bits in timestep one. Then in timestep three, it's going to depend on the first 12 bits in timestep one, right? So you're depending on bits further and further back in that expansion over time, right?

**John Wentworth:**
This does not mean that everything is depending on everything else. In particular, if the bits in timestep 100 are depending on the first hundred bits, you still only have 10 bits that are depending. So there's only so much information about 100 bits that 10 bits can contain, right? You have to be collapsing it down an awful lot somehow, right? Even if it's collapsing according to some random function, random functions have a lot of really simple structure to them.

**John Wentworth:**
So one particular way to imagine this is maybe you're just XORing those first hundred bits to get bit one, right? If you're just XORing them, then that's extremely simple structure in the sense that flipping any one of them flips the output. Random functions end up working sort of like that - you only need to flip a handful of them in order to flip the output for any given setting, right?

**Daniel Filan:**
All right.

**John Wentworth:**
Now I have forgotten how I got down that tangent. What was the original question?

**Daniel Filan:**
The original question is in a deterministic world, how does any of this probabilistic inference happen, given that it seems like my physical state sort of has to... There's no randomness where my physical state can only lossily encode the physical state of some other subsystem of the real universe that I'm interested in?

**John Wentworth:**
Say that again?

**Daniel Filan:**
So if the real world isn't random, right, then every physical system, there's only one way it could have been, right? For some value of could. So it's not obvious why anybody has to do any probabilistic inference.

**John Wentworth:**
Right. I mean, given the initial conditions, there's only one way the universe could be. But you don't know all the initial conditions. You can't know all the initial conditions because you're embedded in the system.

**Daniel Filan:**
So are you saying something like the initial conditions, there are 50 bits in the initial conditions and I'm a simple function of the initial conditions such that the initial conditions could have been different, but I would've mean the same?

**John Wentworth:**
Let's forget about bits for a minute and talk about, let's say, atoms. We'll say we're in a classical world. Daniel is made of atoms. How many atoms are you made of?

**Daniel Filan:**
I guess a couple moles, which is-

**John Wentworth:**
Probably a bit more than that. I would guess like tens of moles.

**Daniel Filan:**
Okay.

**John Wentworth:**
So let's say Daniel is made of tens of moles of atoms. So to describe Daniel's state at some time, take that to be the initial state, Daniel's initial state consists of states of some tens of moles of atoms. State of the rest of the universe consists of the states of an awful lot of moles of atoms. I don't actually even know how many orders of magnitude.

**John Wentworth:**
But unless there's some extremely simple structure in all the rest of the universe, the states of those tens of moles comprising you are not going to be able to encode both their own states and the states of everything else in the universe. Make sense?

**Daniel Filan:**
That seems right. I do think, isn't there this thing in thermodynamics that the universe had a low entropy initial state?

**John Wentworth:**
Not that I know of.

## Abstraction and maximum entropy distributions <a name="abstraction-max-ent"></a>

**Daniel Filan:**
Doesn't that mean an initial state that's easy to encode? Okay. Well, we can move on from there. I think we spent a bit of time in that rabbit hole. The summary is something like abstraction, the way it works is that if I'm far away from some subsystem, there's only a few bits which are reliably preserved about that subsystem that ever reach me and everything else is like shook out by noise or whatever. That's what's going on with abstraction, those few bits, those are the abstractions of the system.

**John Wentworth:**
Yeah.

**Daniel Filan:**
Okay. A thing which you've also talked about as a different view on abstraction is the generalized Koopman-Pitman-Darmois theorem. I don't know if I'm saying that right. Can you tell us a little bit about that and how it relates?

**John Wentworth:**
All right. So the original Koopman-Pitman-Darmois theorem was about sufficient statistics. So basically you have a bunch of iid measurements.

**Daniel Filan:**
Okay. So independent measurements and each of them takes the same random distribution.

**John Wentworth:**
Yeah. So we're measuring the same thing over and over again. Supposing that there exists a sufficient statistic, which means that we can aggregate all of these measurements into some fixed dimensional thing, no matter how many measurements we take, we can aggregate them all into this one aggregate of some limited dimension and that will summarize all of the information about these measurements, which is relevant to our estimates of the thing, right?

**John Wentworth:**
So for instance, if you have normally distributed noise in your measurements, then a sufficient statistic would be the mean and standard deviation of your measurements. You can just aggregate that over all your measurements and that's like a two dimensional or N squared plus one over two dimensional if you have N dimensions, something like that. But it's this fixed dimensional summary, right?

**John Wentworth:**
You can see how conceptually, this is a lot like the abstraction thing. I've got this chunk of the world - there's this water bottle here, this is a chunk of the world. I'm summarizing all of the stuff about that water bottle that's relevant to me in a few relatively small dimensions, right? So conceptually, you might expect these two to be somewhat closely tied to each other.

**John Wentworth:**
The actual claim that Koopman-Pitman-Darmois makes is that these sufficient statistics only exists for exponential family distributions, also called maximum entropy distributions. So this is this really nice class of distributions, which are very convenient to work with. Most of the distributions we work with in statistics or in statistical mechanics are exponential family distributions. It's this hint that in a world where natural abstractions works, we should be using exponential family distributions for everything, right?

**John Wentworth:**
Now, the reason that needed to be generalized is because the original Koopman-Pitman-Darmois theorem only applies when you have these repeated independent measurements of the same thing, right? So what I wanted to do was generalize that to, for instance, a big causal graph, right? Like the sort of model that I'm using for worlds in my work normally. That turned out to basically work. The theorem does indeed generalize quite a bit. There are some terms and conditions there, but yeah, basically works.

**John Wentworth:**
So the upshot of this is basically we should be using exponential family distributions for abstraction, which makes all sorts of sense. If you go look at statistical mechanics, this is exactly what we do. We got the Boltzmann distribution, for instance, as an exponential family distribution.

**Daniel Filan:**
In particular, that's because exponential family distributions are maximum entropy, which means that they're as uncertain as you can be subject to some constraints, like the average energy.

**John Wentworth:**
Yes. Now, there's a key thing there. The interesting question with exponential family distributions - why do we see them pop up so often? The maximum entropy part makes sense, right? That part, very sensible. The weird thing is that it's a very specific kind of constraint. The constraints are expected value constraints in particular. The expected value of my measurement is equal to something, right?

**Daniel Filan:**
Yeah. Or some function of my measurement as well.

**John Wentworth:**
Yes. But it's always the expected value of whatever the function is of the measurement. Then the question is like, why this particular kind of constraint? There are lots of other kinds of constraints we could imagine. Lots of other kinds of constraints we could dream up. And yet we keep seeing distributions that are max entropic subject to this particular kind of constraint.

**John Wentworth:**
Then Koopman-Pitman-Darmois is saying, "Well, these things that are max entropic subject to this particular kind of constraint are the only things that have these nice summary statistics, the only things that abstract well." So not really answering the question, but it sure is putting a big ole spotlight on the question, isn't it?

**Daniel Filan:**
Okay. Yeah, it's almost saying that like somehow the summary statistics are the things that are being transmitted somehow, and they're minimal things, which will tell you anything else about the distribution.

**John Wentworth:**
That's exactly right. So it works out that the summary statistics are basically the things which will propagate over a long distance. Another way you can think about the models is you have the high level concept of this water bottle, you have some summary statistics for the water bottle and all the low level details of the water bottle are approximately independent given the summary statistics.

## Natural abstractions and impact <a name="natural-abstractions-impact"></a>

**Daniel Filan:**
Okay. The last question I wanted to ask here. Yeah, just going off the thing you said earlier about abstractions as being related to what people care about and impact measures and stuff. So there's this natural abstraction hypothesis, which is a wide class of agents will use the same kinds of abstractions. Also, in the world of impact measures, a colleague of mine called Alex Turner has worked on [attainable utility preservation](https://arxiv.org/abs/1902.09725), which is this idea that you just look at changes to the world that a wide variety of agents will care about, or something, and that's impact. That will basically change a wide variety of agents' ability to achieve goals in the world. I'm wondering, are these just superficially similar? Or is there a link there?

**John Wentworth:**
No, these are pretty directly related. So if you're thinking about information propagating far away, anything that's optimizing stuff far away is mainly going to care about that information that's propagating far away, right? If you're in a reasonably large world, most stuff is far away. So most utility functions are mostly going to be optimizing for stuff far away. So if you average over a whole bunch of utility functions, it's naturally going to be pulling out those natural abstractions.

## Learning human values <a name="learning-human-values"></a>

**Daniel Filan:**
All right. So I guess, going off our earlier discussion of impact measures, I'd like to talk about value learning and alignment. Speaking of people being confused about stuff, how confused do you think we are about alignment?

**John Wentworth:**
How confused? Narrow down your question here. What are you asking?

**Daniel Filan:**
Yeah. What do you think we don't know about alignment that you wish we did?

**John Wentworth:**
Look, man, I don't even know which things we don't know. That's how confused we are. If we knew which specific things we didn't know, we would be in way better shape.

**Daniel Filan:**
All right. Well, okay. Here's something that someone might think: Here's the problem of AI alignment. Humans have some preferences. There's ways the world can be and we think some of them are better and some of them are worse. We want AI systems to pick up that ordering and basically try to get worlds that are high in that ordering by doing agenty stuff. Problem solved. Do you think there's anything wrong with that picture?

**John Wentworth:**
I'm sorry. What was part where the problem was solved? You said what we wanted to do. You didn't say anything at all about how we'd do it. I mean, even with the want to do part, I'd say there's probably some minor things I'd disagree with there, but it sounds a basically okay statement of the problem.

**Daniel Filan:**
Okay. I guess, well, I don't know, people can just tell you what things they value and you just learn from that.

**John Wentworth:**
People do what now? Man, have you talked to a human recently? They cannot tell me what things... I ask them what they like and they have no clue what they like! I talk to people and I'm like, "What's your favorite food?" They're like, "Ah, kale salad." And I'm extremely skeptical!

**John Wentworth:**
That sure sounds like the sort of thing that someone would say if their idea of what food was good was strongly optimized for social signaling and they were paying no attention whatsoever to the enjoyment of the food in the moment. People do this all the time! This is how the human mind works. People have no idea what they like!

**Daniel Filan:**
I mean, so-

**John Wentworth:**
Have you had a girlfriend? Did your girlfriend reliably know what she liked? Because my girlfriend certainly does not reliably know what she likes.

**Daniel Filan:**
Okay. If we wanted to understand the innermost parts of the human psyche or something, then I think this would be a problem. But it seems like we want to create really smart AI systems to do stuff for us. I don't know. Sometimes people are really worried that even if I wanted to create the best theorem prover in the world, that just could walk around, prove some theorems and then solve all mathematics for me. People act like that's a problem. I don't know. How hard is it for me to be like, "Yeah, please solve mathematics. Please don't murder that dog," or whatever. Is that all that hard?

**John Wentworth:**
I mean the alignment problems for a theorem prover are relatively mild. The problem is you can't do very many interesting things with a theorem prover.

**Daniel Filan:**
Okay. What about, let's say a materials scientist? Here's what I want to do. I want to create an AI system in a way where I can just describe properties of some kind of material that I want, and the AI system figures out better than any human how I'd do chemistry to create a material that gets me what I want. Is there alignment problems there?

**John Wentworth:**
Yeah. Okay. So now we're getting into like at least mildly dangerous land. The big question is how rare a material are you looking for? If you're asking it a relatively easy problem, then this will probably not be too bad. If you're asking it for a material that's extremely complicated and it's very difficult to get a material with these properties, then you're potentially into the region where you end up having to engineer biological systems or build nano systems in order to get the material that you want. When your system is spitting out nano machine designs, then you got to be a little more worried.

**Daniel Filan:**
Okay. So when I was talking about the easy path to alignment, where you just say what outcomes you do and don't want, why does the easy path to alignment fail here?

**John Wentworth:**
Oh God, that path fails for so many reasons. All right. So first, my initial reaction about humans in fact have no idea what they want. The things they say they want are not a good proxy for the things they want.

**Daniel Filan:**
Even in the context of materials science?

**John Wentworth:**
In the context of materials science, eh. So the sort of problem you run into there is this unstated parts of the values thing. A human will say, "Ah, I just want a material with such and such properties." Or they say, "I want you to give me a way to make the material with such and such properties." Right? Maybe the thing could spit out a specification for such material, but that doesn't mean you have any idea how to build it. So you're like, "All right, tell me how to build a material with these properties." Right?

**John Wentworth:**
What you in fact wanted was for the system to tell you how to build something with those properties, and also not turn the entire earth into solar panels in order to collect energy to build that stuff. But you probably didn't think to specify that. There's just an infinite list of stuff like that that you would have to specify in order to capture all of the things which you actually want. You're not going to be able to just say them all.

**Daniel Filan:**
Sure. So it seems like if we could somehow express "don't mess up the world in a crazy way", how much of what you think of as the alignment problem or the value learning problem would that solve?

**John Wentworth:**
Right. So that gets us to one of the next problems, which is: so first of all, we don't currently have a way that we know to robustly say don't mess up the rest of the world, but I expect that part is relatively tractable. Turner's stuff about [attainable utility preservation](https://arxiv.org/abs/1902.09725) I think would more or less work for that. Except for the part where, again, it can't really do anything that interesting. The problem with a machine which only does low impact things is that you'll probably not be able to get it to do high impact things. We do sometimes want to do high impact things.

**Daniel Filan:**
Okay. Yeah. Can you describe a task where here's this task that we would want an AI system to do, and we need to do really good value learning, or alignment, or something even beyond that.

**John Wentworth:**
Well, let's just go with your material example. So we want it to give us some material with very bizarre properties. These properties are bizarre enough that you need nanosystems to build them and it takes stupidly large amounts of energy. So if you are asking an AI to build this stuff, then by default, it's going to be doing things to get stupidly large amounts of energy, which is probably going to be pretty big and destructive. If you ask a low-impact AI system to build this, to produce some of this stuff, what it will do is mostly try to make as little of it as possible because it will have to do some big impact things to make large quantities of it. So it will mostly just try to avoid making the stuff as much as possible.

**Daniel Filan:**
Yeah. I mean, you could imagine we just ramp up the impact budget bit-by-bit until we get enough of the material we want, and then we definitely stop.

**John Wentworth:**
*laughs* Well, then the question is, how much material do you want and what do you want it for? Presumably, what you want to do with this material is, go do things in the world, build new systems in the world that do cool things. The problem is, that is itself high-impact. You are changing the world in big ways when you go do cool things with this new material. If you're creating new industries off of it, that's a huge impact. And your low-impact machine is going to be actively trying to prevent you from doing that because it is trying to have low impact.

**Daniel Filan:**
Cool. So [before](https://www.lesswrong.com/posts/3L46WGauGpr7nYubu/the-plan) you've said that you're interested in solving ambitious value learning, where we just understand everything about humans' preferences, not just 'don't mess things up too much'. Is this why?

**John Wentworth:**
So right out the gate, I'll clarify an important thing there, which is, I'm perfectly happy with things other than ambitious value learning. That is not my exclusive target. I think it is a useful thing to aim for because it most directly forces you to address the hard problems, but there's certainly other things we could hit on along the way that would be fine. If it turns out that [corrigibility](https://intelligence.org/files/Corrigibility.pdf) is actually a thing, that would be great.

**Daniel Filan:**
What's corrigibility? If it's a thing?

**John Wentworth:**
Go look it up.

**Daniel Filan:**
All right.

**John Wentworth:**
Daniel, you can tell people what corrigibility is.

**Daniel Filan:**
Okay. So something like getting an AI system to be willing to be corrected by you and let you edit its desires and stuff.

**John Wentworth:**
Yeah. It's trying to help you out without necessarily influencing you. It is trying to help you get what you want. It's trying to be nice and helpful without pushing you.

**Daniel Filan:**
Okay. And so, the reason to work on ambitious value learning is just like, "There's nothing we're sweeping under the rug."

**John Wentworth:**
Yeah. Part of the problem when you're trying for corrigibility or something, is that it makes it a little too easy to delude yourself into thinking you're solving the problem, when in fact, you're ignoring big, hard things. Whereas with ambitious value learning, there's a reason ambitious is right there in the title. It is very clear that there are a bunch of hard things. And they're broadly the same hard things that everything else has, it's just more obvious.

**Daniel Filan:**
All right. And so, how does your work relate to ambitious value learning? Is it something like, figure out what abstractions are, then, something, profit?

**John Wentworth:**
So it ties in in multiple places. One of them is, by and large, humans care about high-level abstract objects, not about low-level quantum fields. So if you want to understand human values, then understanding those abstractions is a clear, useful step. Right. Similarly, if we buy the natural abstraction hypothesis then that really narrows down which things we might care about. Right. So there's that angle.

**John Wentworth:**
Another angle is just using abstraction as a foundation for talking about agency more broadly.

**Daniel Filan:**
Okay. Another value of what abstraction is for, or another value of how to do value learning?

**John Wentworth:**
Of what abstraction is for. So, better understanding agency and what goals are in general and how to look at a physical system and figure out what its goals are. Abstraction is a useful building block to use for building those models.

**Daniel Filan:**
Okay, cool.

**John Wentworth:**
*looking at Daniel's notes* Isn't the type signature of human values world to reals. What? Sloppy. What parts are we focusing ... what's that?

**Daniel Filan:**
What's wrong with that? Isn't the type signature of human values just worlds -

**John Wentworth:**
Even if we're taking the pure, Bayesian expected utility maximizer viewpoint, it is an _expected_ utility maximizer. That does not mean that your inputs are worlds, that means your inputs are human models of world. It is the random variables in your model that are the inputs to the utility function, not whole worlds themselves. The world was made of quantum fields long before human models had any quantum fields in them. Those clearly cannot be the inputs to a human value function.

**Daniel Filan:**
Well, I mean, in the simple expected utility world, if I have an AI system that I think has better probabilities than I do, then if it has the same world to reals function, then I'm happy to defer to it. Right?

**John Wentworth:**
What's a world to reals function? How does it have a world in its world model? The ontology of the world model is not the ontology of the world. The variables in the world model, do not necessarily correspond to anything in particular in the world.

**Daniel Filan:**
Well, I mean, they kind of correspond to things in the world.

**John Wentworth:**
Right. If you buy the natural abstraction hypothesis, that would give you a reason to expect that some variables in the world model correspond to some things in the world, but that's not a thing you get for free just from having an expected utility maximizer.

## The shape of the research landscape <a name="shape-research-landscape"></a>

**Daniel Filan:**
Okay. So next I want to talk a little bit about how you interact with the research landscape. So you're an independent researcher, right?

**John Wentworth:**
Yep.

**Daniel Filan:**
I'm wondering which parts of AI alignment do you think fit best with independent research and which parts don't fit with it very well?

**John Wentworth:**
So the right now de facto answer is that's most of the substantive research in AI alignment fits best with independent research. Academia is mostly going to steer towards doing sort of bulls**t-y things that are easier to publish. And there's, what, three orgs in the space? [MIRI](https://intelligence.org/) is kind of on ice at the moment. [Redwood](https://www.redwoodresearch.org/) is doing mildly interesting empirical things, but not even attempting to tackle core problems. [Anthropic](https://www.anthropic.com/) is doing some mildly interesting things, but not even attempting to tackle core problems.

**John Wentworth:**
If you want to tackle the core problems then independent research is clearly the way to go right now. That does not mean that there's structural reasons for independent research to be the right route for this sort of thing. So as time goes forward, sooner or later, the field is going to become more paradigmatic. And the more that happens, the less independent research is going to be structurally favored. But even now, there are parts of the problem where you can do more paradigmatic things and there working at an organization makes more sense.

**Daniel Filan:**
Okay. And so when you say, "This distinction between core problems that maybe independent research is best for and non-core problems that other people are working on," what's that division in your head?

**John Wentworth:**
So the core problems are things like, sort of the conceptual stuff we've been talking thing about. Right now, we don't even know what are the key questions to ask about alignment. We still don't really understand what agency is mathematically or any of the adjacent concepts within that cluster. And as long as we don't know any of those things, we don't really have any idea how to make robustly design an AI that will do a thing we want. Right?

**John Wentworth:**
We're at the stage where we don't know which questions we need to ask in order to do that. And when I talk about tackling the core problems, I'm mostly talking about research directions which will plausibly get us to the point where we know which questions we need to ask or which things we even need to do in order to robustly build an AI which does what we want.

**Daniel Filan:**
Okay. So one thing here is that it seems like you've ended up with this outlook, especially on the question of what the core problems are, that's relatively close to that of MIRI, the Machine Intelligence Research Institute. You don't work at MIRI. My understanding is that you didn't grow up there in some sense. What is your relationship to that thought world?

**John Wentworth:**
So I've definitely had ... I was exposed to [the sequences](https://www.readthesequences.com/) back in college, 10 years ago. And I followed MIRI's work for a number of years after that. I went to the MIRI Summer Fellows Program 2019. So I've had a fair bit of exposure to it. It's not like I just evolved completely independently. But it's still ... there's lots of other people who have followed MIRI to about the same extent that I have and have not converged to similar views to nearly the same extent, right? Including plenty of MIRI's employees, even, it's a very heterogeneous organization.

**Daniel Filan:**
Okay. So what do you mean by MIRI views, if MIRI's such a heterogeneous organization?

**John Wentworth:**
I mean, I think when people say MIRI's views, they're mostly talking about like [Yudkowsky](https://en.wikipedia.org/wiki/Eliezer_Yudkowsky)'s views, [Nate](https://www.so8r.es/)'s views, those pretty heavily overlap. A few other people. The agent foundations team is relatively similar to those.

**Daniel Filan:**
All right. So a lot of people have followed MIRI's output and read the sequences, but didn't end up with MIRI's views including many people in MIRI?

**John Wentworth:**
Yeah. I assume that's because they're all defective somehow and do not understand things at all. I don't know, man... I'm being sarcastic.

**Daniel Filan:**
All right. We can't -

**John Wentworth:**
Daniel can see this from my face, but yeah. So part of this, I think, is about how I came to the problem. I came through these sort of biology and economics problems, which were ... actually let me back up, talk about how other people come to the problem.

**John Wentworth:**
So I think a lot of people start out with just starting at the top like, "How do we align an AI? How do we get an AI to do things that we want robustly?" Right? And if you're starting from there, you have to play down through a few layers of the game tree before you start to realize what the main bottlenecks to solving the problem are.

**John Wentworth:**
Whereas I was coming from this different direction, from biology and economics, where I had already gone a layer or two down those game trees and seen what those hard problems were. So when I was looking at AI, I was like, "This is clearly going to run into the same hard bottlenecks." And it's mostly about going down the game tree deep enough to see what those key bottlenecks are.

**Daniel Filan:**
Okay. And you think that somehow people who didn't end up with these views, do you think they went down a different leg of the tree? Or?

**John Wentworth:**
I think most of them just haven't gone down that many layers of the tree yet. Most people in this field are still pretty new in absolute terms. And it does take time to play down the game tree. And I do think the longer people are in the field, the more they tend to converge to a similar view.

**Daniel Filan:**
Okay.

**John Wentworth:**
So for instance, example of that, right now, myself, [Scott Garrabrant](http://scott.garrabrant.com/), and [Paul Christiano](https://paulfchristiano.com/) are all working on basically the same problem. We're all basically working on, what is abstraction and where does the human ontology come from? That sort of thing. And that was very much a case of convergent evolution. We all came from extremely different directions.

**Daniel Filan:**
Okay. Yeah. Speaking of these adjacent fields, I'm wondering, so you mentioned biology and economics, are there any other ones that are sources of inspiration?

**John Wentworth:**
So biology, economics and AI or ML are the big three. I know some people draw similar kinds of inspiration from neuroscience. I personally haven't spent that much time looking into neuroscience, but it's certainly an area where you can end up with similar intuitions. And also, complex systems theorists, run into similar things to some extent.

**Daniel Filan:**
Okay. So I expect that a lot of listeners to the show will be pretty familiar with AI, maybe less familiar with biology or economics. Who are the people, or what are the lines of research that you think are really worth following there?

**John Wentworth:**
There's no one that immediately jumps out as the person in economics or in biology who is clearly asking the analogous questions to what we're doing here. I would say - I don't have a clear, good person. There are people who do good work in terms of understanding the systems, but it's not really about fundamentally getting what's going on with agency.

**Daniel Filan:**
Okay. I'm going to wrap back a bit. Another question about being an independent researcher. In non-independent research, it's run by organizations and the organizations are thinking about creating useful infrastructure to have their researchers do good stuff. I imagine this might be a deficit in the independent research landscape. So firstly, what existing infrastructure do you think is pretty useful? And what infrastructure could potentially exist that would be super useful?

**John Wentworth:**
Yeah. So the obvious big ones currently are [LessWrong](https://www.lesswrong.com/) / [the Alignment Forum](https://www.alignmentforum.org/), and the [Lightcone](https://www.lightconeinfrastructure.com/) offices. And now actually also the Constellation offices to some extent.

**Daniel Filan:**
Which are offices in Berkeley where some people work.

**John Wentworth:**
So that's obviously hugely valuable infrastructure for me personally.

**John Wentworth:**
Things that could exist, the big bottleneck right now is just getting more independent researchers to the point where they can do useful independent research in alignment. There's a lot of people who'd like to do this, but they don't really know what to do or where to start.

**John Wentworth:**
If I'm thinking more about what would be useful for me personally, there's definitely a lot of space to have people focused on distillation. Full-time figuring out what's going on with various researchers' ideas and trying to write them up in more easily communicated forms. So a big part of my own success has been being able to do that pretty well myself, but even then, it's still pretty time intensive. And certainly there are other researchers who are not good at doing it themselves and it's helpful both for them and for me as someone reading their work to have somebody else come along and to more clearly explain what's going on.

**Daniel Filan:**
Sure. So going back a little bit to upcoming researchers who are trying to figure out how to get to a place where they can do useful stuff. Concretely, what do you think is needed there?

**John Wentworth:**
That's something I've been working on a fair bit lately is trying to figure out what exactly is needed. So there's a lot of currently not very legible skills that go into doing this sort of pre-paradigmatic research. Obviously, the problem with them not being legible is, I can't necessarily give you a very good explanation of what they are. Right?

**John Wentworth:**
To some extent there are ways of getting around that, if you are working closely with someone who has these skills, then you can sort of pick them up. I wrote a post recently arguing that a big reason to study physics is that physicists in particular seem to have a bunch of illegible epistemic skills, which somehow get passed on to new physicists, but nobody really seems to make them legible along the way. And then of course there's people like me trying to figure out what some of these skills are and just directly make them more legible.

**John Wentworth:**
So for instance, I was working with my apprentice recently just doing some math, and at one point I paused and asked him, "All right, sketch for me the picture in your head that corresponds to that math, you just wrote down." And he was like, "Wait, picture in my head? What?" And I was like, "Ah! That's an important skill, we're going to have to install that one, when you're doing some math, you want to have a picture in your head of the prototypical example of the thing that the math is saying." That's a very crucial load bearing skill.

**John Wentworth:**
So then did a few exercises on that and within a week or two, there was a very noticeable improvement as a result of that. But that's an example of this sort of thing. It's not very legible, but it's extremely important for being able to do the work well.

**Daniel Filan:**
Okay. Another question I'd like to ask is about the field of AI alignment. I'm wondering, I believe you said that you think that at some point in 5 to 10 years, it's going to get its act together or there's going to be some kind of phase transition where things get easier. Can you talk a little bit about why you think that will happen and what's going on there?

**John Wentworth:**
Well, the obvious starting point here is that I'm trying to make it happen. It's an interesting problem trying to set the foundations for a paradigm, for paradigmatic work. It's an interesting problem because you have to play the game at two separate levels. There's a technical component where you have to have the right technical results in order to support this kind of work. But then at the same time, you want to be pursuing the kinds of technical results which will provide legible ways for lots of other people to contribute. So you want to kind of be optimizing for both of those things simultaneously.

**John Wentworth:**
And this is for instance, the selection theorems thing, despite the bad marketing, this was exactly what it was aiming for. And I've already seen some success with that. I'm currently mentoring an AI safety camp team, which is working on the modularity thing and it's going extremely well. They've been turning around real fast. They have a great iterative feedback loop going, where they have some idea of how to formulate modularity or what sort of modularity they would see or why, they go test it in an actual neural network, it inevitably fails, the theorists go back to the drawing board. And it's a clear idea of what they're aiming for. There's a clear idea of what success looks like or reasonably clear idea of what success looks like. And they're able to work on it very, very quickly and effectively.

**John Wentworth:**
That said, I don't think the selection theorems thing is actually going to be what makes this phase change happen longer term, but it's sort of a stop gap.

**Daniel Filan:**
Is the idea that natural abstraction hypothesis gets solved, and... then what happens?

**John Wentworth:**
So that would be one path, there's more than one possible path here. But if the natural abstraction hypothesis is solved real well, you could imagine that we have a legible idea of what good abstractions are. So then we can tell people "Go find some good abstractions for X, Y, Z," right?

**John Wentworth:**
Agency, or optimization, or world models or information, or what have you. If we have a legible notion of what abstractions are, then it's much more clear how to go looking for those things, what the criteria are for success, whether you've actually found the thing or not. And those are the key pieces you need in order for lots of people to go tackle the problem independently. Those are the foundational things for a paradigm.

## Following John's work

**Daniel Filan:**
Cool. Yeah. I guess wrapping up, if people listen to this podcast and they're interested in you and your work, how can they follow your writings and stuff?

**John Wentworth:**
They should go on LessWrong.

**Daniel Filan:**
All right. And how do they find you?

**John Wentworth:**
[Go to the search bar and type John S. Wentworth into the search bar](https://www.lesswrong.com/search?terms=John%20S%20Wentworth). Alternatively, you can just look at the front page and I will probably have a post on the front page at any given time.

**Daniel Filan:**
All right.

**John Wentworth:**
It'll say John S. Wentworth as the author.

**Daniel Filan:**
Cool. Cool. Well, thanks for joining me on this show.

**John Wentworth:**
Thank you.

**Daniel Filan:**
And to the listeners. I hope this was valuable.

**Daniel Filan:**
This episode is edited by Jack Garrett. The opening and closing themes are also by Jack Garrett. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
