---
layout: post
title:  "3 - Negotiable Reinforcement Learning with Andrew Critch"
date:   2020-12-10 21:05:00 -0800
categories: episode
---
[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/ZmFmZmFkMTctZmJhZC00ODRhLThhZGUtNjk1NGU1ZWI2NDJi)

In this episode, I talk with Andrew Critch about negotiable reinforcement learning: what happens when two people (or organizations, or what have you) who have different beliefs and preferences jointly build some agent that will take actions in the real world. In the paper we discuss, it's proven that the only way to make such an agent Pareto optimal - that is, have it not be the case that there's a different agent that both people would prefer to use instead - is to have it preferentially optimize the preferences of whoever's beliefs were more accurate. We discuss his motivations for working on the problem and what he thinks about it.

**Daniel Filan:**
Hello, everybody. Today, we're going to be talking to Andrew Critch. Andrew Critch got his Ph.D. in algebraic geometry at UC Berkeley. He's worked at Jane Street as an algorithmic trader, the Machine Intelligence Research Institute as a researcher. And he also co-founded the Center for Applied Rationality, where he was a curriculum designer. But currently he's a research scientist at UC Berkeley's Center for Human Compatible A.I. Today, the paper we're going to be talking about is [Negotiable Reinforcement Learning for Pareto Optimal Sequential Decision-Making](https://papers.nips.cc/paper/2018/hash/5b8e4fd39d9786228649a8a8bec4e008-Abstract.html). The authors are Nishant Desai, Andrew Critch and Stuart Russell. Hello Andrew.

**Andrew Critch:**
Hi. Nice to be here.

**Daniel Filan:**
Nice to have you here. So I guess my first question about this paper is, what problem is it solving?

**Andrew Critch:**
Right. So when I quote, unquote solve a research problem, I'm usually trying to do two different things. One is that I'm answering an actual formally specifiable math question or maybe computer science question. The other thing is, I'm trying to draw attention to an area. So for me, the purpose of this paper was to draw attention to an area that I felt neglected. And so that's a meta research problem that it's trying to solve. And then the research problem that it's solving, or the object level problem that it solves is it demonstrates and explicates what a pareto optimal sequential decision making procedure must look like when the people that it's making decisions for have different beliefs.

**Daniel Filan:**
All right, cool. So one thing that I guess I'm interested in, an area that I'm interested in that jumped out to me in this, is the disagreements between the two, I guess, principals that this sequential decision making policy is serving. And I'm interested in this because there's a line of work, starting with Aumann that basically says that persistent disagreements between two people are irrational, as long as people can communicate, they shouldn't disagree about anything, really. So there's Aumann's Agreement Theorem. This got followed up by Are Disagreements Honest by Tyler Cowen and Robin Hanson and the paper, Uncommon Priors Require Origin Disputes by Robin Hanson. I'm wondering what you think about this line of work and just in general, the set up of two people disagreeing, isn't that crazy?

**Andrew Critch:**
That's great. That's a great question. So a lot of things you just said really triggered me, "Oh, no, it's irrational to disagree." So, first of all, I think Aumann's line of work is really important and things that build on it are a good area of inquiry. There's just not that many people who think about how beliefs work in a inter subjective, formalized setting. But there's a number of problems with trying to assert that disagreement is irrational at the individual level. And I say at the individual level, because rationality is a descriptor of a system and you can have a system where each individual is rational but the system as a whole is not in some sense, in many different senses.

**Andrew Critch:**
So the first thing is that Aumann's Agreement Theorem applies when the two parties have a common prior. Which seems extremely unrealistic for the real world. And I'll say more of what that means, but they're required to have a common prior and also common knowledge of the state of disagreement or state of their beliefs, which I think is also very unrealistic for the real world, and I don't mean unrealistic in some kind of, you can only get .99 when the theorem requires a 1 on some agreement metric. I mean, you can only get .3 when the theorem requires a 1 on some kind of agreement metric is what I'm saying. So I think the assumptions of Aumann are robustly wrong as a descriptor of the real world.

**Andrew Critch:**
But they're an important technical starting point, you describe a scenario under simple assumptions and Aumann's assumptions are simplifying. And then we have to keep complexifying those assumptions to get a real understanding of how beliefs between agents work.

**Daniel Filan:**
Why don't you think that the common knowledge of... So I've heard a lot of people say that the common priors assumption is not realistic for humans. Why do you think the common knowledge of disagreement is not realistic among humans? I think often I have disputes with people where we know that we have this dispute. Right?

**Andrew Critch:**
So right. So first, I want to address the priors thing, even though other people have addressed it, you know. Common prior, I mean, what is my prior? Is it something I was born with as a baby? Is it something in my DNA? There's many different ways of conceiving of a human as having had a prior and then some updates and I think in any of the reasonable conceptions of a human as a Bayesian updating agent, the prior is a pretty old thing that they've had for a long time. And it comes from before they've had a chance to interact with a lot of other people. So I don't think people have equal priors. They're genetically different. They're culturally different. And even as adults, we maybe have only interacted with our own culture. And I think that's deeply bubbling for people, if I can use the word bubble as a verb, it sequesters people and people, even as an educated adult, you haven't interacted with educated adults from other cultures who've read a lot and seen a lot. And I say educated because that's how you get information. If you have less education, you have a different prior as well.

**Andrew Critch:**
So I think it's a big deal, and if we're going to start talking about how A.I is going to benefit humanity, we need to be thinking about people having different beliefs about whether it's beneficial and what is beneficial. And then separately, the common knowledge of disagreement thing, first of all, I would call into question your experience that you really have had full common knowledge of disagreement with the person. You know, there's always this uncertainty, how do you know you were using words in the same way as them? You talk to them for a while and you gain some evidence that you're using words in the same way, and if you're a careful thinker and you engage carefully in discourse, you check to see if you're using words in the same way. But you only ran that check for so long. And what if you're using concepts in a different way too?

**Andrew Critch:**
Let's say you had a debate with somebody about whether people should have privacy from A.I. systems. You talked for a while about what privacy means, you talked for a while about what should means and then you grounded out to some kind of empirical prediction, "I think if the people don't have this kind of privacy, they will end up distressed in the following way". And then the other person in the argument says, "I predict they will not end up distressed", but now, you're satisfied you've made progress and it's good to be satisfied that that progress was made, but have you grounded out what distressed means? Eventually you just go home, eventually you've done a good job today. You made progress with this interlocutor and you disagree still and you don't know for sure whether the concepts you're using are the same as the concepts they're using.

**Andrew Critch:**
And I think that that's profoundly important, if you didn't settle what you meant by distress, that can be an important difference in culture, for example, where maybe for you, something that just makes you a little bit sweaty and makes your mind go faster, counts as distress, whereas for the other person it doesn't. And now you have to ground that out as well. And in my experience, whenever there appears to be a persistent disagreement, if you talk longer, you can always uncover some kind of confusion or miscommunication or difference in information such that prior to that uncovering, you were both deluded as to the nature of the disagreement.

**Andrew Critch:**
So that's why I call into question this idea that you really have common knowledge of the disagreement, because I think you probably are both deluded as to the nature of it, when you still disagree.

**Daniel Filan:**
That's interesting, that seems plausible to me. Now that you talked about the common priors assumption, I actually want to talk about that a little bit. I hope our listeners are interested in the philosophy of Bayesian disagreement. Yeah, so when I think of the priors for a person and how to make sense of them in terms of Bayesian reasoning or whatever. To me it seems this involves some amount of epistemological knowledge that you can update. For instance, I think it's possible that at some point in my life, I didn't know about the modus ponens inference rule or something or maybe some other inference rules. And now I do.

**Daniel Filan:**
And that those have become part of my prior. Where what I mean by the word 'prior' is, okay, today, how do I form beliefs based on everything I know in the past? And maybe that might be a little bit different tomorrow because I'll have some different conception about simplicity priors or something. So in this case, your prior - it sounds strange to say this, but I do want to say that I think you should be able to change your prior over time. And if you can do this then, okay, if somebody comes from a different culture where they understand things differently, hopefully you can reason it out with them by some means other than Bayesian updating, hopefully otherwise it's layers upon layers.

**Daniel Filan:**
Yeah, I just wanted to address what priors should mean in this kind of context, because I think under this different conception, it becomes maybe a bit more realistic.

**Andrew Critch:**
Cool, so before we get into that, I do want to say why I care about this. And the reason is that I'm hoping that we can make progress in A.I. that makes it easier for people with diverse backgrounds and beliefs, not just diverse preferences, but diverse beliefs to share control of the systems. And the reason I want us to be able to share control of systems is twofold. One, I think it's just fair. If you have very powerful systems and powerful technologies, it's more fair to share it.

**Andrew Critch:**
And the other one is that if you can share things, you don't have to fight over them so that decreases the likelihood of conflict over powerful artifacts of technology in the future. And I think there's quite a lot of societal and potentially existential risk that comes from that. So that's the source of my interest here. I think there's many other reasons to be interested in making A.I. compatible with diverse human beliefs and making it possible to negotiate for the control of the system, even knowing that.

**Andrew Critch:**
But I just want to flag that, while I'm going down this rabbit hole with you, that's what's steering me. And if I say something that seems important to that, I say something that seems important to your question, I'm also filtering it for importance to, is this going to matter to the future governance of technology?

**Daniel Filan:**
Okay.

**Andrew Critch:**
So the first thing is that, yes, I agree with you that people can change their priors. But I mean, a Bayesian agent, quote, unquote, changes prior when it updates. And I think you mean something more nuanced than that, which is- [crosstalk 00:13:22]

**Daniel Filan:**
Yeah. I mean- [crosstalk 00:13:23]

**Andrew Critch:**
You can think longer and decide that your prior at the beginning of time ought to have been something else. And so, yeah, my statement that you're responding to in that is that a reasonable conception of a human being as a Bayesian agent has the prior as something that has existed for a long time or that came into existence a long time ago. And that reasonable conception of a human Bayesian is doing a lot of work because humans aren't Bayesian agents. In fact, physical agents are not Bayesian agents because you have to do a lot of computation to be a Bayesian agent. So in fact, you have to do infinite computation. So this is where my interest in logical uncertainty and if you've heard of logical induction comes from. And so I would just argue that when you're changing your prior there, you're changing which Bayesian agent you are. You're not being a Bayesian agent in that moment.

**Daniel Filan:**
Okay. That seems fair. All right, so- [crosstalk 00:14:26]

**Andrew Critch:**
And for listeners who've never thought about that, why is that important? Well, I think there's ethical questions that can be resolved by thinking, there's ethical questions that can't. I think Rawl's Veil of Ignorance is an example of an ethical principle that helps you figure things out by thinking longer and harder about what if I were somebody else, you already knew everything you needed to know about those people to start realizing some of the things you should do to be fair. But you have to think about it. And in the same way I think that's going to apply in the governance of A.I. and for that reason, I think it is going to be important not to treat people like Bayesians because people are entities and computers are entities that change what they believe merely by thinking, even without making further observations. So that's a major shortcoming of the negotiable reinforcement learning framework that's only alluded to in the older arXiv draft with just me on it that says naturalized agency is going to be key future work. And I do not think that the paper addresses that well at all.

**Daniel Filan:**
Yeah, I think that's a good point. I'm going to tack a little bit back to the paper or the literature on Bayesian agents cooperating and such. The related work section of this paper has a lot of interesting stuff on social choice theory and such, there's a rich work of literature, I guess, both on social choice theory and on, can reasoners disagree? In some sense, a reader might find it a little bit surprising that this work hasn't already been done, at least that the main theorem in this paper hasn't already been proven. I was wondering if you have thoughts about why it took until, was it 2018 that this got published? 2019?

**Andrew Critch:**
2017 was the first, the NeurIPS version was 2018 but the theorem you're referencing was proven in 2017.

**Daniel Filan:**
Okay, so why do you think it took until 2017?

**Andrew Critch:**
Yeah. This is something I grapple with deeply, I mean, for me, "how do agents with different beliefs get along" is a pretty basic question. So it has been analyzed a little bit, like you said, by Aumann and people in Aumann's - Google scholar search people who cite Aumann and you'll get a lot of interesting thoughts about that. But not really much looking at sequential decision making. So you get these really static analyses, imagine you're at the end of eternity and you've reached common knowledge of disagreement, and at the beginning of eternity you had a common prior. Now Aumann's theorem applies, but it's a fixed moment. It's not something evolving over time. And things evolving over time are more complicated than things that are static. So if I were going to guess, it's just you've got people who work on sequential decision making, which is reinforcement learning people and operations research people. And then you've got people who think about beliefs a lot and "what is a belief" and there hasn't been that crossover of "okay, what happens when you put the sequential decision making and the belief disagreement together?"

**Daniel Filan:**
Yeah, I guess that's related to how in statistical mechanics it's much easier to come up with a theory of equilibrium statistical mechanics, than non-equilibrium statistical mechanics. And it took humanity, we got the equilibrium theory way before we got a good non equilibrium theory.

**Andrew Critch:**
And I think there's a lot of things like that in analysis of multi-agent interactions, game theory in general, is just all about equilibria, not about how you get there. There's some research on that, but I think it's going to be a lot of hard work still to figure out.

**Daniel Filan:**
Do you have examples of these non equilibrium problems that maybe our listeners can help solve?

**Andrew Critch:**
Oh, well, I mean, there's a lot of games where finding the Nash equilibria is NP-hard. So that means in particular, that the two agents playing against each other, if you take those two agents as a computation, they're not going to be finding a Nash equilibrium unless they've got enough compute to solve an NP-hard problem, which they probably don't. So just Google NP-Hard Nash equilibria, and then you'll just see how many Nash equilibria just aren't really going to happen.

**Daniel Filan:**
Okay. So. I guess with that out of the way, we'll get into the details of the paper. So we're talking about different principals who somehow have to negotiate over a policy that's going to act in the world. And one theorem that's a little bit about this is the Harsanyi Utilitarianism Theorem, right, where there's you and me and perhaps we're electing a government or something and Harsanyi's Utilitarian Theorem basically says, well, what the government should optimize is a weighted linear combination of our utility functions. And in your paper you prove something that isn't that theorem, could you tell us a little bit more about why that theorem doesn't apply? Or why can't you just use that result?

**Andrew Critch:**
Yeah, so I mean, answering that kind of just is the theorem, but I can try to give an intuitive version of it. So first of all, the theorem is pretty easy, it's just linear algebra. I don't think it's a deep fact. I think the only thing special about it is bothering to think about it.

**Daniel Filan:**
You also need to know a bit about convex geometry, a tiny bit.

**Andrew Critch:**
I guess. Yeah a little bit. But it's really if you just draw a picture, it's kind of clear so. So first of all, let's talk about what Harsanyi's theorem says. Harsanyi's theorem is a brilliant theorem. It really simplifies the number of different ways you could imagine aggregating people's preferences by showing that basically many, many different reasonable ways of doing it are all equivalent to just giving a linear weight to each person's preference and then maximizing that sum. So that's cool and it's a little counterintuitive to me in the sense that it feels intuitively, or it used to feel intuitively to me, like there ought to be more different ways of aggregating preferences that feel compelling, but that aren't linear combinations and a lot of things that felt different from a linear combination to me just turned out to be doing linear combinations. So that was kind of cool, and I'm not sure I can even remember what they were now because my brain has compressed them into the linear combination bucket.

**Andrew Critch:**
But this key assumption of having the same beliefs is a key assumption of Harysanyi's theorem. And it's not even explicitly stated, it's just 'fact' is lurking in the background and it's assumed that everybody has access to the facts. But in reality, we don't have access to 'fact'. We have beliefs and we have things to do to update our beliefs, get better information.

**Andrew Critch:**
So here is, by the way, still putting off on answering your question, I'm going to say that the paper is not normative, you said, you can take Haysanyi's theorem as being normative and maybe he intended it to be normative, but I use it not normatively, but just descriptively. Look, all these things you might do. They're all just linear combinations of preferences. That's a nice, simplifying fact.

**Andrew Critch:**
In the same way, I don't take the negotiable reinforcement learning result or the toward negotiable reinforcement learning result as normative, because actually I think there's a lot of bad outcomes that result from the dynamics described in the theorem. It's simultaneously a negative result. I don't think that's how you should do things so I'm going to answer a slightly different question, which is why would you do it that way? If you were accounting for differences in beliefs as described in the paper, why would you be doing it?

**Andrew Critch:**
And the reason is this, so let's just say you and I are deciding to, I don't know, let's say we're deciding to do a podcast together and you're going to interview me in a podcast. That's a negotiation. You know, we got to decide on the time. We got to decide am I comfortable with the recording tools you are using or whatever, all that kind of stuff. And then once that's decided, then we go ahead and we do the thing. We execute the sequential decision making that is a podcast interview together. But before we do that there's always the possibility that the negotiation could just fail, it could fall through and we go back to what people call the best alternative to negotiated agreement or BATNA.

**Andrew Critch:**
So my BATNA today, if I didn't do this podcast, was going to be to write some things up, I was going to do some writing and I don't know what your BATNA was, but if this failed, maybe you would have just interviewed somebody else today. So if you want to maximize the probability that two people are going to choose to cooperate and execute a literally co-operative sequence of decisions, you want them to be able to find a plan that they both like more than their BATNA. And so we both liked this idea of the podcast today more than the other stuff we were going to do. So now we're doing it and if there's any what people call Pareto sub-optimality, meaning opportunities to improve the plan for you without making it worse for me, if there's any Pareto sub-optimality on the table, then there's a chance that we're below your BATNA needlessly. If we're in the midst of making a plan and we're crappy at planning together, we're bad at negotiations such that the plan we have is Pareto sub-optimal, meaning we could make it better for you without making it worse for me, or we could make it better for me without making it worse for you. That's Pareto sub-optimality.

**Andrew Critch:**
If it's Pareto sub-optimal, there's a risk that the plan is going to be below your BATNA and you're going to bail and it's below your BATNA needlessly. It's like we should just bump it up. And that way, if you treat your BATNA as a random variable, if I treat your BATNA or a mediator were to treat both of our BATNAs as random variables, there's a chance that those random variables are going to be below the best negotiated plan we have. So for me, Pareto optimization is related to or subservient to maximizing the probability that the negotiators will succeed in coming up with a sufficiently appealing cooperative plan that they choose to cooperate. And that's because I think A.I governance is going to require people to cooperate a lot.

**Andrew Critch:**
And negotiate a lot in the course of that. And so, now if you want to maximize entirely for cooperation and not for other important principles like, say, fairness, one thing you might inadvertently or you might intentionally do this or you might inadvertently do this, you might exploit people's differences in beliefs to implicitly have them bet against each other with the policy. So let's say I guess you're using Zencastr, so let's say I don't know much about Zencastr, but I think it's going to be fine because most software companies are reasonably careful with their data and you know Zencastr and you know all about them. And you happen to know that Zencastr is a terrible company that doesn't respect anybody's privacy.

**Andrew Critch:**
But you know that I don't know that. People have studied this dynamic, by the way, bargaining with asymmetric information. That's not new, but if we Pareto optimize, we end up with this plan to show up on Zencastr. And if you want me to do the podcast and I want to do it subject to privacy constraints and I don't know about them, I sign up to do it and then later I find out, oh, no my privacy is being violated by Zencastr and someone downloaded the data and recorded my neighbor's conversations from tiny trace audio. And now their privacy has been violated, too, and I've been penalized for having incorrect beliefs about how Zencastr was going to turn out for me.

**Andrew Critch:**
And the single shot version of that is just making a bet. It's like we made a bet, it's kind of two bets at once. You bet that you would like the plan, I bet that I would like the plan. And I lost my part of that bet because Zencastr turned out bad.

**Andrew Critch:**
So the interesting thing is what happens when that bet suddenly becomes a continuous process that happens for the rest of forever, which is what you see in a A.I. System, that is Pareto ex ante, meaning before it runs, ex ante subjectively Pareto optimal to the people or the principals it's serving is that it will actually every time step for the rest of eternity, settles a little bet between the principals who created it or who agreed to defer to it. And then if one of the principals had very inaccurate beliefs about what the A.I. system was going to observe, that principal's priority, the weight that it gets in the system's judgment goes down and down and down because every second it's losing a bet for how much control that principal is going to have over the A.I., or how much the A.I. Is going to choose to serve that principal's values.

**Andrew Critch:**
And so in the same way I could lose one bet with you over how good Zencastr is going to be, I could actually lose a whole series of bets with you every second about how Zencastr is going to turn out. And if you got really accurate beliefs about the world, you're going to win all those bets and our cooperation is going to end up great for you and worse for me.

**Andrew Critch:**
But it was my willingness to bet on my own false beliefs that caused me to cooperate with you in the first place. And if I had known, if I hadn't been deluded, as to Zencastr's ethics, I might have just not done the podcast. And maybe that's ethically the right thing to turn out to do. But with A.I., I worry that fragmentation could be quite bad if it leads to war, or even just standards - there's physical wars, and then there are standards wars where companies are just fighting over what standards are going to be important because they're fragmenting. And I think that can cause a lot of chaos and waste a lot of attention. It could even, if it's physical wars, actually get people killed. If countries are fighting over A.I. technology in the way that you might see companies fighting over oil as a resource. So I guess I want to point out an important trade off.

**Andrew Critch:**
This paper points out a trade off between fairness and cooperation, which is that cooperation ex ante rewards people with more accurate beliefs upon entering into the cooperation. And it's unfair to the people who had wrong misconceptions of what was going on. So your original question is why should we use this? Well in cooperation- [crosstalk 00:32:11]

**Daniel Filan:**
The original question, I think, was why doesn't the Harsanyi Aggregation Theorem apply?

**Andrew Critch:**
Right. And why or why should we use the belief updating rule instead or something?

**Daniel Filan:**
Yeah, something like that.

**Andrew Critch:**
And it's more like, well we could say in a meta problem of fairness and cooperation being two different principals you want to serve, you're trying to invent a negotiation framework that's pretty good for cooperation and pretty good for fairness. I would say Harsanyi's approach is Pareto sub-optimal because you can get more cooperation. But you should also be adding fairness as a constraint to Harsanyi. So I don't know the answer yet of what I would personally, subscribe to as the right way. I'm not so sure that no one will ever find a way that I'll look at and say, that's actually the right way, let's do it. I'm not so anti-realist about that moral judgment, but I don't have a strong view on it right now.

**Daniel Filan:**
Okay, so there are a few questions I could ask from there. First of all, I'll ask a quick technical question, in this theorem, you assume that policies can be stochastic. Can you say a little bit about what exactly you mean by that assumption? Because I think it's slightly different than what readers might think.

**Andrew Critch:**
Oh, I just mean that at every time step, you can randomize what you're going to do. So the A.I. system every time step is like flipping a coin and it's policy is just what the weight of that coin is. And it can also randomize at the outset if it wants, it can choose a random seed at the beginning to choose between two different random policies. So it has a memory in a sense. There's a few different ways of formalizing it. One is it generates a random seed at the beginning of time and then remembers that seed for the rest of time. Or you could just have that it can remember everything that it has previously done, including that initial coin flip. And that's the formalization that I adopt just because it proves a stronger theorem.

**Daniel Filan:**
Okay. Yeah, so discussing this theorem. So if I think about institutions where people can be rewarded or punished based on what they know, it seems we already have a few of those that people are broadly okay with. So, for instance, the stock market I have in my time-[crosstalk 00:35:38]

**Andrew Critch:**
I take exception to the claim that people are broadly okay with the stock market, but I'll agree that the stock market is, in fact still happening.

**Daniel Filan:**
Yeah, I think people are okay with some individual trades. If you and I trade in equity, the person who knows more about the value of that equity in the future has an edge on that trade. I guess I haven't seen polling, my assumption is that most people are okay with the idea that I can trade in equity with you, but maybe you don't think that.

**Andrew Critch:**
Well. Claims about what most people think are okay are a little bit dangerous. And I'm a little bit uncomfortable making them.

**Daniel Filan:**
How are they dangerous?

**Andrew Critch:**
Well, they can force people into equilibria that they didn't want to be in. So, you're in a room full of 30 people and somebody says, "Well, we're all clearly okay with the meetings being at 6:00 A.M every week. Right?" And there's this brief pause. And then, now, the meetings are at 6:00 A.M every week. And a bunch of people objected, but they didn't know everybody else would have objected. So they didn't object. And so when you say everybody agrees, blah. I'm if I can think of some people who don't agree with blah, I'm hesitant to just get on the everyone agrees with blah train because I'm oppressing that view if I do it. So I'm not going to get on board with the everybody's okay with the stock market claim, and I might not even get on board with the everyone's okay with asymmetric information trades, although I would agree more people are okay with that.

**Andrew Critch:**
I could imagine a future where, education is a human right right now, I can imagine a future where informed trade is a right and not thinking that's ridiculous. And it would create a lot more work for the economy to do, to produce information for people whenever they enter trade. But I think that's a tenable position. I know people who think that bargaining without transparency is just bad and wrong. And I'm like "yeah, I don't know". I don't want to close the book on that by just saying everybody agrees with it already.

**Daniel Filan:**
Yeah, I guess. To follow this tangent a bit, so I think one argument against ensuring that every trade is transparent is sometimes it might impose a really high communicative overhead, for instance- [crosstalk 00:38:39]

**Andrew Critch:**
It takes a lot of economic work. Exactly.

**Daniel Filan:**
Yeah. And especially where it's like, suppose we're going to have a podcast today. You know more potentially about what you'd be like on a podcast than I do and what if that knowledge is implicit. It's based on your experience of you talking to people over a lifetime that I don't have. It seems hard. I'm not even sure I know what it would look like for that trade to be fully informed- [crosstalk 00:39:11]

**Andrew Critch:**
I mean I'm definitely comfortable saying some people sometimes are okay with asymmetric information trades and I was okay with this one. And I guess you were too. And, you know, yeah.

**Daniel Filan:**
All right.

**Andrew Critch:**
But I think that was just a prelude to some other claim you were going to make, and I think you can still make that subsequent claim without couching it in the "everybody agrees that asymmetric information trades are fine."

**Daniel Filan:**
Yeah, I guess the claim that I might make is we have institutions that have asymmetric information trades or at least trades where participants believe different things when they make the trade and those institutions seem like they work. [crosstalk 00:40:14 ]

**Andrew Critch:**
Yeah, I don't want to undercut that but flag for minor potential disagreements, but go on.

**Daniel Filan:**
Yeah. So if I think about the stock market and the mating market, these are two, well, one of them is more like a market than the other, but there are two cases where people make asymmetric information trades or at least trades with different beliefs. The stock market, as far as I can tell, seems to successfully serve the purpose of predicting itself in the future and the mating market seems- [crosstalk 00:41:02]

**Andrew Critch:**
Sorry what do you mean by mating market?

**Daniel Filan:**
I mean the market by which, humans pair off and become romantic partners and maybe they pair bond for life.

**Andrew Critch:**
Okay.

**Daniel Filan:**
The mating market seems roughly successful in getting a pretty large majority, but not everybody, a romantic partner eventually, and importantly, both of these seem stable institutions.

**Andrew Critch:**
Yeah.

**Daniel Filan:**
It seems they are above people, probably they're not literally above everybody's BATNA, but they're above most people's BATNA. They're roughly doing what they're trying to do and we're not seeing really big revolts against them.

**Andrew Critch:**
I mean, so there have been revolts against the stock market.

**Daniel Filan:**
Yeah.

**Andrew Critch:**
And so. I guess that's that's my counterpoint.

**Daniel Filan:**
Yeah, that's that's a decent counterpoint. Yeah. So which revolts are you thinking of specifically? When I think of revolts in that class, I'm not sure I can think of ones that were actually a stock market- [crosstalk 00:42:30]

**Andrew Critch:**
What's Occupy Wall Street? Right. Let's just take the meme we are the 99%. What happens in a world where 1% of people acquire the resources necessary to make the best predictions about where other resources are going to go? And gain so much advantage from that, that they just dominate the exchange of resources to the point of accumulating most of the resources under the control of a small minority of people.

**Andrew Critch:**
Now is that good or is that bad? Well, it has properties, right? It rewards effort that makes people and institutions better at prediction. So there's an incentive there to get better at prediction. But it also is a little incestuous in the sense that these people and institutions are just protecting each other. It's like you said, the stock market is good at predicting itself. But I mean, what is the stock market doing? It's ensuring efficiency of trade among the owners of a very large number of business activities. And is that good?

**Andrew Critch:**
Well, I don't know, maybe constantly changing the ownership of very large, powerful entities creates a diffusion of responsibility where you can just rotate in and out board members and CEOs if things go wrong. And so it's not clear that the stock market is a good thing for all the people who didn't end up in control of it.

**Andrew Critch:**
And I mean, I worked in finance, so I think the stock market does some good and I don't think it's all bad and I wouldn't erase the stock market right now if I had an anarchy button, but I think it has some problems and I think many people agree that it has some problems. And one of the problems is that it just heaps resources on to people who are good at predicting it or institutions that are good at predicting it. It really leaves out people who don't have those big powerful institutions behind them to help them make their financial decisions.

**Daniel Filan:**
I mean. So this feature of the stock market doesn't generalize to other things we're talking about - the one that I'm about to say -

**Andrew Critch:**
Okay, sorry.

**Daniel Filan:**
Yeah, but I mean, the stock market does have this feature where you can just buy the whole market. And then if you're more willing to wait for resources than the market, quote, unquote, is you can just buy the market, wait and then eventually get reasonably wealthy from doing that, right?

**Andrew Critch:**
You're just saying the stock market has- [crosstalk 00:45:47] stock market value has gone up over time and index funds help protect you from the adverse selection of choosing which things to own?

**Daniel Filan:**
Yeah, I guess I'm saying that, even if I don't know much about individual stocks or I have very little information about what companies are doing what or where profit is being generated. I can buy an index fund and- [crosstalk 00:46:11]

**Andrew Critch:**
But you will not become a billionaire by buying index funds.

**Daniel Filan:**
That seems correct, unless I'm a 999 millionaire to start with.

**Andrew Critch:**
Yeah. I didn't mean to doxx you as a non billionaire there. I just know you're not listed on any of the public registries of billionaires. You could still be a billionaire.

**Daniel Filan:**
You don't know how much my shell companies have. Yeah. And so I guess the analogy is, if we made A.I. that literally rewarded people who knew stuff. It would reward some kind of insider trading or fooling others or something, and this would not be a stable situation. Is that a summary of what you think?

**Andrew Critch:**
That is a thing that I think yeah, I wouldn't say it's a summary, but it's a thing that I believe. Yeah. And it's not really addressed by the NRL paper, it's more like the NRL paper is pointing out - NRLP, negotiable reinforcement learning. It's pointing out if you Pareto optimize for cooperation or if you just Pareto optimize ex ante, you get this bet settling outcome. And the paper doesn't really explore very much about the unfairness of that outcome. There's a little bit in there, but that's more of a future work type of thing that I hope people will think about.

**Daniel Filan:**
Okay. Yeah, so speaking of that. Yeah, I guess you wrote this paper because you thought that it would have consequences in the world. Or I gather that you did.

**Andrew Critch:**
Yeah.

**Daniel Filan:**
Yeah. Can you say a bit more? What things do you hope will happen because you wrote this paper?

**Andrew Critch:**
Yeah, I mean, just proximally, I hope researchers and A.I. students, faculty, industry folks who are building sequential decision making systems will take an interest in differences in belief. As an interesting bearing on what happens with the system and I hope that they can see, wow, there's something different about how belief bears on the system from the way preference bears on the system. Or how belief ought to bear on the system versus how preference ought to bear on the system. Preferences, you just leave them as they are or you try not to disturb them. Whereas beliefs, you have this opportunity to share information and update each other's beliefs, for example, that's completely missing from the paper. I'd to see people designing mechanisms for individuals and institutions who govern powerful A.I systems to share information with each other. So it evens the playing field. So they all have the same information. There might be a small benefit to the institution that had more information at first or something to sell their information to other institutions. But I'm hoping we don't lock the entire future into some technological equilibrium that really disenfranchised a massive amount of people or a massive amount of different value systems that just didn't manage to have a say on what A.I. does.

**Andrew Critch:**
And to an extent, there's a lot of people thinking about fairness and accountability in A.I., and then transparency at least to engineers. So there's a cluster, fairness, accountability, transparency that really appeals to me here. And so, you know, maybe if people with those interests could think more about differences in belief and how that's going to play out in sequential decision making, policies are going to run for a long time, how should that play out? Yeah.

**Daniel Filan:**
Okay, yeah, so I guess a follow-on from that, I see this as similar to the social choice literature and I guess if you're hoping, and I'm not sure that you actually said this, quite, but if it's true that you're hoping that this research will eventually help facilitate bargaining and thinking about, okay how do we actually get people to cooperate over the creation of powerful artificial intelligences, do you think the existing social choice literature has analogously done a good job at fostering cooperation?

**Andrew Critch:**
Well, that's interesting. I have wondered this. And I don't know, I mean, Aumann and Schelling and people like that were commissioned by RAND Corporation to try and and devise nuclear disarmament protocols. And they tried and they admittedly failed. Their writings on this say, look, we tried, we couldn't come up with anything and they seemed to have earnestly tried. I don't think they were lazy. I mean maybe they were, they didn't seem that way to me. Maybe they're just brilliant and they can have good ideas while being lazy. But, so in a sense, I'm going to say no. Like, there were things that the world called on mathematicians and game theorists and decision theorists to figure out that they didn't figure out.

**Andrew Critch:**
And I think we're not done making that call. That happened during the Cold War, right? So it was time to make that call and some of the greatest minds came together to think about disarmament. How do you gradually deescalate a threatening situation between nations? But there hasn't been that much work since, there hasn't been that flurry of brilliance in the area of how to foster peace and cooperation as there was back then in the Cold War. And I hope, I think with the advent of increasingly capable A.I. technology, we're going to see more and more brilliant people taking an interest in how to maintain peace and harmony in the world with that much capability. So I'm half making a prediction and half making a bid that says let's revisit these foundational questions about how to achieve cooperation and see if we can do better than the 70's.

**Daniel Filan:**
Yeah.

**Andrew Critch:**
Yeah.

**Daniel Filan:**
Yeah. It's interesting that we stopped because it's not as if we don't currently live in a world where many countries have a lot of nuclear weapons or many countries disagree about who gets what bit of land. Right?

**Andrew Critch:**
It's true, it's not as if we don't live in that world.

**Daniel Filan:**
So another question on consequences. So at NeurIPS in 2020 papers are supposed to have a broader impact statement and the broader impact statement is supposed to include how could this research have a negative consequence if there's a plausible way. Suppose your research ended up making the world worse, by means other than just opportunity costs. There was something else that people could have done that was great, but instead they paid attention to yours, it actively made the world worse. How do you think that would have happened?

**Andrew Critch:**
Yeah, I mean, I guess I've alluded to it, right? Someone just grabs the formula from negotiable reinforcement learning and just runs it and then a bunch of people end up unwittingly signed on to a protocol that's exciting to all of them at start, because according to their own individual beliefs, it looks great. But some of the people's beliefs about how it's going to go down are wrong. And then they end up getting really screwed over. And I don't want that to happen. So do I have a theorem for how not to do it or what's the correct balance between getting everybody together versus making sure everybody's signed on to something fair. No, I don't, but that's another potential future work. Maybe there's an interesting boundary there between fairness and unity to be hugged.

**Andrew Critch:**
But, yeah, if it goes wrong, but that doesn't seem likely. So, it's just one idea who's going to use this, but maybe it's the mode of the distribution of unlikely ways this idea could end up having a large negative impact.

**Daniel Filan:**
Okay. So speaking of consequences, the paper has been out for a while. How's the reception been?

**Andrew Critch:**
Yeah, there have been a bunch of people who came up to me to try to take an interest in it. It seems like what happens is, it seems like there are little pockets of interest in it, but that are isolated or not even pockets.There hasn't been a whole lab of people who all got interested in it. And so there have been, person from this group person from group, "Oh, this is interesting". But I think it's hard for people to stay motivated on projects when the rest of their working environment is not adequately obsessed with it or something, so there hasn't been a flurry of follow up work on it and maybe now's the time for that. Maybe today, maybe this podcast, I also have more free time now. I just finished up a giant document. Maybe now is the time that we can try to get a cluster of people working together to solve the next problems in what I would call machine implementable social choice. But that hasn't happened yet. We'll see.

**Daniel Filan:**
Right. So speaking of that, do you have any final things you'd like to say? Or if people are interested in following your research, how should they do so?

**Andrew Critch:**
Well, I guess I don't have a Twitter account, if that's what you're asking. But thanks for asking. And the easiest way to notice if I write something is just [subscribe to me on Google Scholar](https://scholar.google.com/citations?user=F3_yOXUAAAAJ&hl=en&oi=ao). You can make Google Scholar alerts that just tell you when someone publishes a paper. So that should work. And I have a website, but that's a more active attention-intensive way of keeping track of what I do compared to Google Scholar. And maybe I'll get a Twitter account someday, it won't be this year, I think. Maybe, I could lose that bet, but it won't be in the next several months, that's for sure.

**Daniel Filan:**
Okay, well, thanks for talking with me Andrew. And to the listeners, thanks for listening and I hope you'll join us again.
