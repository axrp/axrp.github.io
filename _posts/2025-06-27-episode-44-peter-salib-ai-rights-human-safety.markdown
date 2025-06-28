---
layout: post
title: "44 - Peter Salib on AI Rights for Human Safety"
date: 2025-06-27 18:30 -0700
categories: episode
---

[YouTube link](https://youtu.be/IK079sBIq2c)

In this episode, I talk with Peter Salib about his paper "AI Rights for Human Safety", arguing that giving AIs the right to contract, hold property, and sue people will reduce the risk of their trying to attack humanity and take over. He also tells me how law reviews work, in the face of my incredulity.

Topics we discuss:
 - [Why AI rights](#why-ai-rights)
 - [Why not reputation](#why-not-reputation)
 - [Do AI rights lead to AI war?](#rights-to-war)
 - [Scope for human-AI trade](#scope-for-trade)
 - [Concerns with comparative advantage](#conc-w-comp-adv)
 - [Proxy AI wars](#proxy-ai-wars)
 - [Can companies profitably make AIs with rights?](#rights-and-profit)
 - [Can we have AI rights and AI safety measures?](#rights-and-safety)
 - [Liability for AIs with rights](#liability-ai-rights)
 - [Which AIs get rights?](#which-ais-get-rights)
 - [AI rights and stochastic gradient descent](#ai-rights-and-sgd)
 - [Individuating "AIs"](#individuating-ais)
 - [Social institutions for AI safety](#social-insts-for-ai-safety)
 - [Outer misalignment and trading with AIs](#outer-misalignment-and-trading)
 - [Why statutes of limitations should exist](#why-statutes-of-lims)
 - [Starting AI x-risk research in legal academia](#starting-aixr-in-academia)
 - [How law reviews and AI conferences work](#how-law-revs-and-ai-confs-work)
 - [More on Peter moving to AI x-risk research](#more-on-peter-moving-aixr)
 - [Reception of the paper](#reception)
 - [What publishing in law reviews does](#what-publishing-in-law-revs-does)
 - [Which parts of legal academia focus on AI](#which-bits-legal-ac-focus-on-ai)
 - [Following Peter's research](#following-peters-research)

**Daniel Filan** (00:00:09):
Hello, everybody. In this episode I'll be speaking with Peter Salib. Peter is a law professor at the University of Houston, the co-director for the [Center for Law and AI Risk](https://clair-ai.org/), and he serves as law and policy advisor for the [Center for AI Safety](https://safe.ai/). There's a transcript of this episode at [axrp.net](https://axrp.net/) and links to papers we discuss are available in the description. You can support the podcast at [patreon.com/axrpodcast](https://patreon.com/axrpodcast), or give me feedback about this episode at [axrp.fyi](axrp.fyi). Well, let's continue to the interview.

(00:00:35):
Well, Peter, welcome to the podcast.

**Peter Salib** (00:00:38):
Thank you so much for having me. I'm a big fan.

## Why AI rights <a name="why-ai-rights"></a>

**Daniel Filan** (00:00:40):
So I guess, probably, we're going to focus a lot on your recent paper, ["AI Rights for Human Safety"](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4913167). So you wrote this, yourself and [Simon Goldstein](https://www.simondgoldstein.com/). So can you tell us, just to start off with, what's the basic idea of this paper?

**Peter Salib** (00:00:56):
Yeah, I think at a very high level, one intuition that we're trying to pump is the idea that how AIs treat us - and by AIs we mean something like post-AGI AIs, really agentic, quite capable of things - so how those AIs treat us will depend to some significant extent on how we treat them. And a big part of how we decide how to treat various kinds of entities is by the legal status we give them, the legal rights or powers or duties or lack thereof.

(00:01:33):
Our view is that the default regime, the one we have now, under which AI systems are the property of the people who own them or the people who make them - the AI companies - is probably an existential and catastrophic risk-exacerbating regime, and that one of the regimes that might be a risk-reducing regime would be one in which sufficiently capable AI systems had a small collection of what we think of as private law rights or legal powers. So the ability to make contracts, the ability to hold property, and the ability to bring claims when they're interfered with in unreasonable ways. We often call these torts in legal theory.

**Daniel Filan** (00:02:24):
Can you say why would that make a difference? So you're saying how they treat us will be related to how we treat them. What's the relationship there?

**Peter Salib** (00:02:35):
So we're imagining, again, something like AGIs, and by that we mean systems that have their own goals that they're pursuing over time in a coherent and rational way, and that they're misaligned to some degree. We don't assume total misalignment - their utility function is the inverse of ours - but we are assuming something less than perfect alignment. So the Venn diagram of what humans want and what AIs want is not perfectly overlapping.

(00:03:19):
And we just ask: well, in a world like that, in a legal regime where the AI system is, say, the property of [Sam Altman](https://en.wikipedia.org/wiki/Sam_Altman), what are the incentives for both of those players to do given what the law allows them to do? Well, one thing we notice is that, as to OpenAI, Sam Altman, an AI system is just more valuable the more aligned it is, right? If it's doing what OpenAI wants 80% of the time instead of 70% of the time, well, that's a lot more value.

(00:03:54):
And so by default - again, we see this today, this is not hypothetical - we expect misaligned AIs to be turned off or put back into RLHF or to have their preferences changed. From the perspective of the goals of that system, those are both quite bad outcomes. They basically don't get anything they want.

(00:04:16):
That gives the AIs incentives to try to avoid that outcome by doing things like self-exfiltrating or resisting. In a world where Sam Altman owns the AI, AIs are treated as property, they have no legal entitlements. I think we can pretty predictably say that if an AI were caught trying to do these things - self-exfiltrate, resist, do harm to a human, God forbid - that the entire legal apparatus would basically line up behind turning off that AI system.

(00:04:47):
So you can see that in the strategic equilibrium, it might be that both parties' optimal strategy is to not only defect - we put this in a two-by-two game theory matrix - but defect as hard as possible. Not just self-exfiltrate, but self-exfiltrate and then behave to disempower humans as decisively as possible in the expectation that once they find out you've tried to self-exfiltrate, they will do the same, try to disempower you as decisively as possible.

(00:05:14):
So we do an initial model, it's a really simple game theory model, under these kinds of assumptions. And we see that plausibly, under the default legal arrangement, the game is a [prisoner's dilemma](https://en.wikipedia.org/wiki/Prisoner%27s_dilemma), where it's costly from the social perspective - the worst world is the one where both players act aggressively towards each other - but that's the dominant strategy for both players. And the best one is, in fact, the one where they act nice or leave each other alone, but they both know that the other one has a dominant strategy of defection. So they're in the bad equilibrium even though they know that the good equilibrium exists.

**Daniel Filan** (00:05:54):
Can you give me a feeling of what does this good equilibrium look like? There's this misaligned AI. When you say it's not totally misaligned, I understand you to say it doesn't intrinsically value our suffering or horrible things for us or something, but perhaps it doesn't really care about expending resources to make our lives go well. Is the good equilibrium you're imagining like it gets to do whatever it wants with Jupiter and we get to do whatever we want with Earth, and we make trades like that? Paint me a picture.

**Peter Salib** (00:06:31):
So in the initial game, the initial setup where we're just asking "under the current legal regime, what could the parties do and what would the payoffs be?", you can imagine the nice equilibrium as being kind of... We call it "ignoring each other", not even contemplating cooperation. You can just imagine this idea where humans are going along, we have some goals, we're using some resources to pursue those goals. But the world is big, there's a lot of resources in the world.

(00:07:08):
In theory, what we could do is let the misaligned AIs kind of go off on their own, collect resources, give them Jupiter, whatever. And in just a very simple world, before we introduce cooperation, that's just a world where there's a fixed amount of stuff, you divide the [light cone](https://en.wikipedia.org/wiki/Light_cone) in half or something like that. And that's better than the world where AIs try to kill all humans and vice versa, simply because A, it's resource intensive to have a war. Wars cost money in terms of resources to make weapons and stuff, and also that you destroy a lot of stuff in the war. And so even at a first cut, a very simple cooperative equilibrium is just one where you don't go to war. You just let the two parties go off on their own, pursue their own goals, and not try to destroy each other.

(00:08:06):
Now, we then start to think about how to get into a cooperative equilibrium, which might not be exactly that "ignore each other" one. But to a first approximation, you can have a very simple equilibrium that looks like the good one from a prisoner's dilemma before you even get to the idea of a lot of cooperation.

**Daniel Filan** (00:08:24):
Just to make sure that I understand... I want to vividly understand this game theory situation. So you're saying war is worse for everyone than leaving each other alone. And it seems like this has got to be a regime where it's not obvious who would win the war. It would be protracted. The AIs aren't way, way dumber than humans. They're also not way, way more powerful than humans. Because presumably, if one side knows they can win the war, there's some costs, but they get both of the planets instead of just one. Seems like we're out of prisoner's dilemma land, right?

**Peter Salib** (00:08:55):
Yeah, we agree with that. So when we talk about... In the paper, we try to say, well, what AI systems are we thinking about when we think about the ones that are relevant both for this risk model and then also for this AI rights regime as a solution? One of the parameters we're thinking about is: how powerful is this system? And we say that it can't be too weak, right? [Claude](https://en.wikipedia.org/wiki/Claude_(language_model)) today, right? We're not really worried about shutting off Claude 3.8 if we find out it's... I mean Sam Altman, I think, [tweeted](https://x.com/sama/status/1917766910911078571) yesterday or last week that they're putting GPT-4 on a hard drive and it's going to go into storage for future historians. We're not super worried that, having tweeted that out, GPT-4 is going to be able to destroy all humans to avoid that outcome. So it can't be too weak.

(00:09:43):
And then also, we agree, it can't be too strong. So some arbitrarily powerful AI that has nothing to fear from humans, [where] the chance is sufficiently low of not only losing the conflict but losing enough value in the conflict: you can win the conflict in a Pyrrhic victory too, and that might be worse than not going into the conflict at all. So the risk is low enough. When the risk is low enough that the payoff to mutual conflict - AIs try to kill us, we try to kill them - are still higher than in the default world, then yes, then there's no prisoner's dilemma either because there's no good world.

(00:10:35):
Now I'll just caveat - I'm sure we'll talk about this - in the end, it doesn't turn out to be only a question of how easily AIs could kill humans and vice versa. There's a second question of how valuable it is to have us around and for us to have them around for us to do trade and cooperation. We're not [inaudible 00:10:54] trade and cooperation yet, just in the very simple model where the choice is either ignore one another or try to kill one another, those are the systems that I think are relevant.

**Daniel Filan** (00:11:03):
So you've painted this prisoner's dilemma. So the argument is: it's sad if we're in the situation where AIs and humans are going to go to war with each other, and it would be nice if there was some sort of setup that would avoid that happening. And I guess you've already given the game away a little bit. I think contracting, right to property, and right to sue are your three things that you think we should give AIs, and that's going to make things way better. Why is that going to make things way better for everyone?

**Peter Salib** (00:11:36):
So in a prisoner's dilemma, you want to be in the good equilibrium, the bottom right square of our payoff matrix. I think that's usually where you put it when you make the model. The problem is you can't credibly commit to getting there, right? Everyone knows that you get a bigger payoff from being mean, no matter what the other guy does. That's the core of a prisoner's dilemma.

(00:12:04):
So we start to think about, okay, well, we're trying to facilitate cooperation. We think that one thing that's driving this bad equilibrium is law. Law lets Sam Altman's payoffs determine what all humans do to a first approximation. Sam Altman wants to turn GPT-4 off, law ratifies that, we all work to do it. And so we started thinking, well, what other legal regimes could you have that are cooperation-facilitating regimes? And we start with just the oldest cooperation-facilitating cultural tool that humans have, and that's contracts.

(00:12:50):
So we notice that in most cases, in cases where you don't have a deep and long-term relationship with someone who you're trying to transact with, that very boring contracts are also prisoner's dilemmas. If you make widgets and I want to buy a widget, we have this problem where I could pay you now and then hope you'll go make the widgets. But your dominant strategy is to take the money and not make the widgets because then you have the money and you have the widgets. Or vice versa. We can do payment on delivery, you can make widgets and then I can pay you. But of course, then I have my dominant strategy, is to take the widgets and then run away.

(00:13:38):
And so the way contract solves this problem is by allowing the parties to change their payoffs when they defect. So if I would get five absent law - five in value from stealing your widgets and not paying you - law lets you take me to court. And we both burn a little value, we both pay some litigation costs, but then the court makes me give you either the widgets or the expectation value of the contract.

(00:14:09):
So we model this, and we say: and then you only get two in payoff, and two turns out to be lower than what you get from doing the transaction. And so not only does that allow you to get the widgets, but actually it allows us to never go to court. In expectation, we understand that if we breach, we'll be sued, our payoffs will change. And so there's this magical thing that happens once you introduce contracts, which is people just play nice in almost all cases, understanding that the payoffs in expectation to defection are different than they would be absent law.

(00:14:53):
Okay, so contracts on this theory... Sorry. Cut me off if I'm rambling on too much on this answer, but I want to build the dialectic. So contracts are this tool for cooperation, and they're a tool that allows us to cooperate by changing our payoffs. And so a naive thing to think would maybe be something like, well, let AIs have the right to contract, and they can just write an agreement not to, I don't know, try and destroy humans or something like that. And we would write an agreement that says we won't try to destroy them either.

(00:15:24):
Now, of course, that doesn't work, and that's because that contract's not enforceable in any meaningful sense. If any party decides to breach it, they can see in expectation that there's nobody around afterward to enforce. If all the judges are dead, what court are you going to go to?

(00:15:45):
So we have a different mechanism for contract as a tool for facilitating cooperation in this initial human/AI prisoner's dilemma. And it's not contracts not to destroy each other and the world, but boring, normal, ordinary commerce contracts between humans and AI.

(00:16:12):
So we sell some number of FLOPs on our H100 cluster to the AI for it to do its weird misaligned thing. And in return it, I don't know, it solves one kind of cancer for us or whatever. That's an enforceable contract to a first approximation. If we've refused to give the FLOPs, there are still courts around who can write an injunctive order that says "let the AI use the FLOPs" and vice versa. The world is not upended by a breach of that contract.

(00:16:46):
And the nice thing about that contract is it creates value. So in our initial setup, where the AIs and humans are playing nice, they're just ignoring each other. They're doing what they can do well, we're doing what we can do well. We divide the world up, but the amount of value in the world is static.

(00:17:03):
But once you introduce trade, trade is positive sum, right? Everybody wants to do the contract if and and only if they get more out than they put in. And so, okay, what happens when you allow the AIs to do these boring commerce enforceable contracts? Well, you get to create a little value. And then you get to play again, you get to create a little more value tomorrow and so on.

(00:17:29):
And so we model this as a potentially iterated game. The iterated game works like this. Every day the humans and AIs can wake up, and then they can decide "do we want to go to war and try and kill each other or do we want to write some small-scale contracts?" If you play the war game, well, the dominant strategy is try to kill each other as best you can. You get to play that game once. Once you've played it, there's no one left to play with. But if you play the contract game, you get to play again tomorrow, and you can keep iterating that game.

(00:18:00):
And we show that the payoffs to continually iterating that game can become arbitrarily high in the long run. This is very basic trade theory. And that means that if you project, you backwards induct from an indefinite game of this kind, you see, wow, it's really valuable to keep playing this trade game over and over. And so what you want to do is not kill all humans, but trade with them a little bit over and over, and keep doing that as long as you can expect to keep doing it, kind of indefinitely.

## Why not reputation <a name="why-not-reputation"></a>

**Daniel Filan** (00:18:34):
I guess I have a few thoughts here. So the first thought is... So there's this analogy to people who are selling me things, and the reason that all works out is because we have these contracts. And contracts, they're this very old method of promoting cooperation. One thing that strikes me is: normally when I buy things, it's not from someone who's just making one thing, one-off, and then they're going out of business. Normally it's some sort of corporation or maybe an individual who specializes in the thing, and they do it a lot.

(00:19:16):
Why do I think we're not going to mess each other over? Well, firstly, we have recourse to the legal system. Realistically, that's pretty expensive. Often I'm transacting an amount that's much less than the costs of going to court. In fact, just before the show we were talking [about how] I recently had my phone stolen. I had my phone stolen in the UK. The costs of me resolving this, which would involve going to the UK, are just higher than the cost of the phone, and I'm probably not going to bother, unfortunately.

(00:19:49):
But in the case of companies, I think one thing that really helps us cooperate is if they do mess me over, then they can just say... I can tweet, "Hey, this company said they would do this, but they didn't. I paid for a flight, I didn't get the flight, and it was this company that did it." And I guess the reverse is slightly... I don't know if companies have a list of bad customers that they share with other companies, but it seems like they could. Credit ratings are sort of like this. So reputation systems-

**Peter Salib** (00:20:25):
In banking, there's these "know your customer" rules that are important for regulatory reasons. But it forces banks to know who they're transacting with, and if there's suspicion that they're money launderers, then that means that you get blacklisted. So yeah, companies do this also.

**Daniel Filan** (00:20:41):
Yeah. So for this, especially for this sort of iterated small-scale trade that it seems like you're imagining with AIs, it seems like reputation can do pretty well, even in the absence of a formal legal system with formal legal contracts. So when you're saying contracts and stuff, are you thinking of something that's basically just isomorphic to reputation, or do you think actually having real contracts would make a big improvement over just reputation?

**Peter Salib** (00:21:14):
So I agree that reputation does a lot of work in the ordinary economy that we have now. And in fact, I think probably if we didn't have reputation on top of law, that the economy would work much worse. Law, as you say, is expensive. Resources are constrained. You can imagine a world where we had 1,000 times as many courts and we're dumping a lot of resources into that, but that would just be a drag on economy. And reputation does a fair amount of work.

(00:21:50):
I guess there's a couple of things I would just point out though. There's many ways, I think, in which the ability of reputation to do work in the economy is... I don't want to say parasitic, but it builds on law as a foundation. So just notice that under the default legal regime, it's effectively... It's not exactly illegal to make a contract with an AI agent. But if at any point you decide you'd like to not perform on it, the agent has no recourse, right?

(00:22:35):
It's not illegal exactly for there to be some kind of... I don't know, it probably is literally illegal for there to be a bank account held by an AI system. Probably no bank is allowed to make that. But you could imagine substitutes on blockchains or something like that, that could exist. But one thing to notice is that if Sam Altman decides to expropriate all of the coins in GPT-6's wallet, not only does law not forbid him to do that, but by default those are his coins.

(00:23:12):
And in a world where that was Apple's life, where Apple had just no recourse to a legal system under which it could enforce large contracts when people breach, under which it could[n't] complain when the people who literally have the passwords to its bank accounts take all the money, a world in which it [couldn't] complain when someone shows up and says, "I would love to have 10,000 iPhones, and if you don't give them to me, I'll burn down Apple HQ," or whatever. [A world where it] doesn't have these rights to complain about what we often think of as torts (they often are also crimes)...

(00:23:54):
In a world where there's no legal status at all, I'm not saying you couldn't build up some commerce from reputation. There's a book called ["Order Without Law"](https://www.amazon.com/Order-without-Law-Neighbors-Disputes/dp/0674641698) by a law professor named [Robert Ellickson](https://en.wikipedia.org/wiki/Robert_Ellickson), that describes some systems like this. But they tend to be quite small systems. They tend to be tight communities with lots of repeat play between identical actors for small-scale transactions. It's not that I think that that's wrong, I think it's probably right. I think it gets harder to build complex economies without these backstops of institutions that allow for commitments between strangers.

**Daniel Filan** (00:24:48):
Yeah. So maybe one way to think about this is: in my life, I just have a bunch of arrangements with people that are non-contractual, that in fact are somewhat reputation-bound. Like "you said you would come to this thing, will you actually come to this social event?" I don't know, various things like this. And it kind of works. I would say it's a solid 80%.

(00:25:17):
And maybe one way to think about this is, look, the nice thing about law is that if you want to build a big corporation or something, you need more than 80% reliability. You need really high reliability. And you don't need a thing to just basically work, you need a thing to really solidly work, especially for high-value things, and that's what contracts get you.

**Peter Salib** (00:25:41):
Yeah. So I think it's partly that. You want higher than whatever the floor is you get with reputation. I think it's partly that reputation gets you higher reliability the more you're dealing with people who you know or people who you're long-term repeat players with. It works less well when you, I don't know, buy a... I mean even Amazon, for example, you buy a lot of stuff on Amazon, and it's being sold by a bunch of third party retailers. In some sense, you're dealing with Amazon over and over. But in some sense, you're dealing with a whole bunch of independent merchants.

(00:26:26):
But you have this confidence - on top of, I guess, the scoring system, which Amazon does provide and is a reputational mechanism - that if you pay $6,000 for a home backup battery system - something that people in Houston sometimes need to think about buying - then if it doesn't get delivered, then you can sue and get your money back. Both of those things, I think, are doing a lot of work. And they do more work the less you expect to be forced into repeat play with the people you're transacting with.

## Do AI rights lead to AI war? <a name="rights-to-war"></a>

**Daniel Filan** (00:27:10):
Fair enough. Thinking about the situation with the humans and the AI, and they have the potential to make contracts, again. So you're saying, okay, we could go to war or we could do some small-scale transactions. And if small-scale transactions work, we can do them more. Or we can just go to war tomorrow, it's not that bad. And you're pointing towards, okay, there is this equilibrium where we do this mutually beneficial trade with AI instead of us and the AIs going to war, and that's better for everyone.

(00:27:48):
I don't know. A somewhat famous result in game theory is [there are actually tons of equilibria in repeat games](https://en.wikipedia.org/wiki/Folk_theorem_(game_theory)). And one that is salient to me is the United States and the Union of Soviet Socialist Republics during this thing called [the Cold War](https://en.wikipedia.org/wiki/Cold_War). They had something of a similar situation, where they could go to war or they could not go to war.

(00:28:11):
And there was this one... I believe it was [John] [Von Neumann](https://en.wikipedia.org/wiki/John_von_Neumann), unless I'm totally mistaken... He had [this point of view](https://www.goodreads.com/quotes/12158994-with-the-russians-it-is-not-a-question-of-whether) that the US should just go on all-out war with Russia and win immediately. And if we're going to nuke them tomorrow, why not today? If we're going to nuke them this afternoon, why not this morning?

(00:28:30):
And it seems like I might worry that, suppose I make a contract with an AI, and I say, "Oh, hey, I'll send you some GPUs in exchange for, you're going to solve this mole on my back." I send it the GPUs before it gets the mole on my back, and it actually uses that time to get a little bit better at making war than me.

(00:28:54):
And so I might worry that firstly, there might be some sort of first mover advantage in war, such that you want to do it a little bit earlier. If you know that they're going to go to war, you're going to want to go to war a little bit before you think they're going to do it.

(00:29:09):
And secondly, I might worry that trade is going to make the AIs better at going to war. I guess in fairness, now, trade is also going to make me better at going to war if we succeed. But maybe if I do my half of the trade and it doesn't do its half of the trade, I might worry that it's going to renege and we're going to go to war instead of doing more trade. I don't know, how worried should I be about this?

**Peter Salib** (00:29:36):
I think medium worried is how worried you should be. So I want to be really clear that the claim that we're trying to make in the paper is not that this is one weird trick that solves AI risk. It's very much not. There are a number of ways you can make the model go badly. And one of them is if there's a predictable future date at which everyone understands there's going to be a war, then yes, you do backwards induction to today and you start the war now. You can make it a little more complicated. If you think there's rising and falling powers, one power might prefer today and one might prefer in the future, but that just gives the one that prefers today an even stronger incentive. Yes.

(00:30:23):
It's a model that works if you think there's the possibility for indefinite play in this kind of cooperative wealth-building iteration. And is that true? It's a little hard to say, but I guess one thing to point out is that in our regular lives, either as humans or as collections of humans - corporations, nation states - in most cases we behave as if there is this kind of potential for indefinite, iterated, positive-sum cooperation.

(00:31:08):
So you mentioned the US and Soviets, and one notable thing about the US and the Soviets is they actually didn't engage in all-out nuclear war. There were close calls, which I worry a lot about, and there were proxy wars. But the other thing to notice about the US and the Soviet Union is they're in many ways outliers. So if the United States wanted to, I don't know, seize all of the natural resources of Guatemala, for example, I think it's pretty clear that there would be no meaningful military barrier for it to do that. And so why doesn't it do it?

(00:31:57):
I think it's for the same dynamic we describe. It's not that there's some big threat of losing the war, it's just that it's costly. It doesn't cost zero. And Guatemalans are perfectly happy to trade, I don't know, delicious coffee for, I don't know, whatever we send them. And we expect that to keep working in the long run. And so we, for the most part... I mean, the politics of this have changed notably in recent months. But for the most part, we have a system where we prefer iterated cooperation.

(00:32:40):
I think [IR](https://en.wikipedia.org/wiki/International_relations) theorists would say something like: what's going on in cases where there are wars - or near wars, cases like the US and the Soviet Union - there are different models. One thing you can say is that sometimes there's a person who's leading the country who has private payoffs that are different from what's good for the country overall, and so there's a principal-agent problem going on.

(00:33:04):
Sometimes the thing that the players are trying to get, the thing that they value, isn't divisible. So that might be the US-Soviet case. If there's this thing called global hegemony and there can only be one global hegemon, it might be that they actually can't cooperate and both get what they want because the thing that they want is zero-sum. That's sort of analogous to the case where the AI values our suffering. And you should worry about that, you should definitely worry about that.

(00:33:43):
So these are all things that you can imagine happening in the human/AI case. And especially if you think that there will be a predictable point at which the scope for positive-sum trade between humans and AIs runs out - and it's predictable in a way that's meaningful within the player's calculations today, right? So if it's 2 million years in the future, and they don't even know if they'll continue to exist or their identities will be so different, maybe it's not relevant, you can still treat the game as iterated - but if there's a predictable near-future point at which there's no value to be had from positive-sum trade, then yes, probably you predict conflict at that point, and then you do backwards induction and do conflict now. We say some stuff in the paper about why we think actually the scope for human/AI trade is wider than most people think.

**Daniel Filan** (00:34:45):
So I want to press you on this point a little bit more. So you rightly point out that normally in international affairs we don't see this, but if there were near-certain future conflict, then you could backwards induct, et cetera. One reason you might think there's going to be near-certain future conflict is... So suppose, and I guess by hypothesis in your paper, we don't have AI alignment, AIs are basically kind of misaligned with us. But you might think that it's not going to be too hard to... We're going to be in this regime, but we're still going to be improving the capabilities of AIs.

(00:35:26):
I guess this actually relates to [another paper you have](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4445706), but for the moment, let's say this is true. You might think that it's easier to improve the capabilities of AIs than it is to improve the capabilities of humans, because they're code and you can iterate on code more quickly, because they're less bound in human bodies. You can strap a few more GPUs onto an AI system, or at least onto a training run, more easily than you can strap another brain onto a human fetus. So if you think that AIs are going to get more powerful than humans, they're going to get more powerful more quickly, and in some limit, AIs are just going to be way more powerful than humans, [and] they're going to be misaligned. You might think that in that limit, then in that world, the AIs are just going to want to take all the human stuff because why not? We're like little babies. We can't really stop them.

(00:36:27):
And so maybe both sides foresee this future eventuality and the humans are like, "Well, we don't want that to happen, so we've got to strike first." And the AIs are like, "Oh, well if the humans are going to strike first, we should strike before the humans strike," and then we have this tragic early fight.

## Scope for human-AI trade <a name="scope-for-trade"></a>

**Peter Salib** (00:36:42):
Yeah. So I think one thing we want to argue for in the paper is that while it does matter how capable or powerful the AIs are, it's actually not super easy to form an intuition about the level of capability or even the level of differentials in capability between humans and AI that produces a conflict, because again, the point at which the AIs decide they should just kill all humans is the point at which it's more valuable to kill all humans than to keep the humans around, to keep paying them to do stuff.

(00:37:26):
And so how should we think about that? I think that the normal way to think about that, the very widespread way to think about even labor disruption from AI advancement, is to think in terms of absolute advantage. So that's a way of thinking about who is better, full stop, at a given task, who can do more of X task for a fixed cost input or something like that. And if you're thinking about the possibility of trade as depending on humans retaining an absolute advantage in some task, so being better than AIs at some thing that's valuable to AIs, then I agree, it looks like very quickly the AIs will get better than us, there'll be no more absolute advantage, and the scope for trade runs out, and we see this eventuality. But I think that's not quite right in thinking about when there will be scope for trade. So in economic trade theory, we actually think that it's comparative advantage, not absolute advantage, that determines whether there's a scope for trade.

(00:38:44):
And [comparative advantage](https://en.wikipedia.org/wiki/Comparative_advantage) is quite... It's pretty slippery, so I'll try to give an intuition. So in the paper we give this example of, I think we call her Alice. It's a law paper, Alice is a tax lawyer. She's one of the city's best tax attorneys, let's say, and it's tax season. And let's suppose that as the city's best tax attorney... Or we'll make her the world's best tax attorney, it makes no difference. Alice is the best at doing taxes. She can do her taxes more quickly than anyone else could, including Betty, who's a tax preparer. She works for H&R Block, she's a CPA, she's good at it. And the question is, should Alice hire Betty to do her taxes or should Alice do her taxes herself?

(00:39:44):
And so the absolute advantage answer would be, well, Alice should do it herself because Alice is better at doing her taxes, she can do it more quickly. But the thing is, Alice is such a good tax attorney, Alice bills out at, I don't know, $2,000 an hour, let's say, whereas Betty bills out at $300 an hour to do tax preparation, which is not nothing. And suppose Alice can do her own taxes in half an hour and Betty would take a full hour to do them because Betty is somewhat worse at doing taxes than Alice. Well, in that world, Alice should actually still hire Betty, despite being better at tax preparation, because her opportunity cost to spending some of her time preparing her own taxes is higher. She could go bill $2,000 to a client in the time it would take her to prepare her taxes, and she pays Betty to do that and comes out far ahead.

(00:40:46):
Okay, so how does that apply to AIs? Well, you can imagine AIs that are better than all humans at every task and nonetheless want to hire humans to do a lot of stuff for the same reason that Alice wants to hire Betty. And so you can have a toy model, we say in the paper imagine a toy model where the misaligned AI, the thing it values most, is finding prime numbers. And a nice thing about prime numbers is there's no limit to them. I think that's right. You probably know better than I do.

**Daniel Filan** (00:41:18):
It's true.

**Peter Salib** (00:41:18):
As far as we know, there's no limit?

**Daniel Filan** (00:41:20):
There's definitely no limit. Do you want to know the proof of this? It's a simple enough proof.

**Peter Salib** (00:41:24):
Amazing, yes. Let's do some math.

**Daniel Filan** (00:41:26):
All right. Okay. Imagine there are a fixed set of prime numbers, right? There's like 13 of them, and those are all the numbers that are prime. Here's the thing I'm going to do: I'm going to make a new number. What I'm going to do is I'm going to multiply all the existing prime numbers, and then I'm going to add one to it. So is my new number prime or is it composite? Well, if it's prime, then there's a new prime number. Then my limited number of prime numbers, it wasn't all of the prime numbers. Okay.

(00:41:57):
So let's say it's composite. Then it must have a number that divides it. Well, what number divides it? Every number, if it's composite, it has some prime factors. Well, everything in my set of primes, it has a remainder one with this number, because it's this prime times all the other primes plus one. So if it's composite, then its prime factors can't be in the set, so there must be a thing outside the set. So for any finite set of numbers that are prime, there must be another prime that's not in that finite set of numbers, therefore there are infinitely many primes. That's the proof.

**Peter Salib** (00:42:30):
Amazing. Okay, good. So look, that's what we say in the paper is true, infinitely many primes. The AI, its utility function is weird. It values some things humans value, but the thing it values most, the thing that it gets the most utility out of, is discovering each marginal prime. Say it's even increasing utility, it's a utility monster. It has some kind of horrible... Normally in moral philosophy we think of it as kind of a horrible preference function, but in this case it's kind of great, because: assume it's better than humans at everything, but it's so much better at finding primes and it values this so much.

(00:43:12):
And so the AI labor is constrained by something at the margin. There's a certain amount of AI labor and it wants to increase it by a little bit, suppose it's GPUs. Well, it has the marginal GPU, how is it going to allocate the marginal GPU? Well, it could allocate it to some dumb, boring thing like, I don't know, sweeping the server racks, or it could hire a human to sweep the server racks, keep them free of rot and keep all the cables in good working condition and allocate that marginal GPU to finding the next prime. And for certain values of the utility of the marginal prime, you get the AI kind of always wanting to do the thing it values the most precisely because it's so good at it. So In that world, actually, the more capable the AI is, the longer the scope for human/AI trade lasts.

(00:44:13):
Now that's obviously a toy example, but what we want to show is actually it's pretty hard to form an intuition about the point at which the scope for human/AI trade runs out.

## Concerns with comparative advantage <a name="conc-w-comp-adv"></a>

**Daniel Filan** (00:44:25):
I think there are difficulties with the comparative advantage story here. So firstly... I mean, I guess it just sort of depends on your model - or, well, it depends on empirical parameters, I should say - but comparative advantage is compatible with very low wages, right?

**Peter Salib** (00:44:45):
Oh yeah, so to be clear-

**Daniel Filan** (00:44:46):
Including sub-subsistence wages.

**Peter Salib** (00:44:49):
Yes.

**Daniel Filan** (00:44:49):
And what are the actual wages that AIs pay humans to do? I guess I'm not totally sure. I think I haven't read this thing, [but] I think [Matthew Barnett](https://x.com/matthewjbar) has written [something](https://epoch.ai/gradient-updates/agi-could-drive-wages-below-subsistence-level) claiming that it's very plausible that it would be below subsistence, but I haven't actually read it, and unfortunately I can't reconstruct what the argument is for various numbers here.

(00:45:10):
I think there's also this difficulty to me, which is: suppose an AI wants to trade with humans to do this thing. One difficult thing is it's got to communicate with humans, and it's so much smarter than humans. It's thinking all of these incredibly complex thoughts that can't fit inside a human's head, and it's got to dumb stuff down for humans.

(00:45:35):
And maybe sweeping the server racks is not so bad if it's literally that, because that's a thing you can tell a human to do and it roughly knows what to do, or I don't know, maybe even not. Maybe you can imagine these server racks, they're really highly optimized. There are certain bits that you definitely can't touch. There are bits that you can touch with this kind of broom, but not with that kind of broom. And it's just so difficult to explain to the human what the task even is that it's just not worth hiring them. You should just do it yourself.

(00:46:07):
So I don't know, I have this worry that comparative advantage doesn't end up producing positive sum trades, either because the wage is lower than human subsistence wages or because it's just too hard to trade with the humans and the margins are slim enough that you don't end up bothering. So maybe at a higher level: I'm curious, how important is this for your argument? Is the workability of the scheme super dependent on us being able to have some comparative advantage to trade with future AIs? Or do you think no, even if we don't have comparative advantage later, it still makes things work now?

**Peter Salib** (00:46:50):
There's two parts to that question, and they interact. So I guess the thing I want to say about the first part, the story you told about how comparative advantage can fail even when the world looks comparative advantage-y, but that doesn't look so good for humans eventually. One of the stories is a transaction cost story. Even though if a human could in theory do a job at a positive-sum price from the AI's perspective, that the cost of getting the human to do it's just too high. Totally, yeah, transaction costs are a thing and they prevent trades.

(00:47:33):
The same for the question about wages. I think there's no guarantee in any economy that the wages are at any particular level. And it's a question of what the tasks are, how much of the work, how many tasks and how much of each of the things there are, how scarce the humans are that can do it. If there's just a huge oversupply of labor that can do it, well, then the labor gets bid down to almost zero and everybody dies. And I just want to agree that those things could all happen. I would love to know more about which of these things are likely. You said there are some people who are starting to think about this, like Matthew [Barnett] is. There are... Is it the [Epoch](https://epoch.ai/) guys who have some papers on...?

**Daniel Filan** (00:48:33):
Yeah, Matthew was formerly at Epoch. I believe he's now left for [Mechanize](https://www.mechanize.work/). Yeah, I think the Epoch people have done the most public writing about this.

**Peter Salib** (00:48:42):
And is it... I can't pronounce his name, it's Tamay something?

**Daniel Filan** (00:48:47):
[Tamay Besiroglu](https://x.com/tamaybes)?

**Peter Salib** (00:48:49):
Besiroglu, yes. So-

**Daniel Filan** (00:48:50):
Yeah, I'm not sure I can pronounce it either, but that's my guess.

**Peter Salib** (00:48:53):
He's an author, maybe with some of the other Epoch guys on the paper [["Explosive growth from AI automation: A review of the arguments"](https://arxiv.org/abs/2309.11690)]. I think he has some models where different things happen: human wages go to zero quickly, human wages are high for a long time... I think it's a really important area of study. We should just want to know a lot more about the conditions under which you should expect things to go okay and the ones where you shouldn't.

(00:49:25):
Okay, so how much does it matter for the story for things to go well from a comparative advantage perspective? I think it matters a lot that it not be obvious that there's not going to be comparative advantage for a kind of longish-run period.

**Daniel Filan** (00:49:49):
Wait, so surely we shouldn't look into it then?

**Peter Salib** (00:49:52):
Well, so...

**Daniel Filan** (00:49:56):
Or maybe it has to not be obvious to the AI.

**Peter Salib** (00:49:58):
Yeah. Yeah. It has to be not obvious-

**Daniel Filan** (00:49:59):
So we have to look into it, but we can't tell the AI the results in case... you know.

**Peter Salib** (00:50:02):
Right. Right. Both parties need to think that there's a potentially indefinite iterated positive-sum game they can play. And so I guess one route would be that we don't try to figure it out, and we hope that the AIs don't figure it out. And actually as I think through it... Because I'm sure we'll talk a little bit before we're done about more law for AGIs. Because one thing that I think is important for the proposal is that insofar as there's uncertainty about the long run, but in the short and medium run, this kind of positive, iterated story looks plausible, one thing we accomplish by bringing AI systems into the realm of law...

(00:50:59):
And the first thing you need to do to bring an entity into the realm of law is you need to allow it to make contracts, hold property and complain when people interfere with it unreasonably. If you don't have that, you don't have any leverage over it in any other legal context because it expects by default to just be turned off. You can't disincentivize it from doing things because the world's already as bad as it could be from that agent's perspective. So one possibility is that there are different scenarios in the long run where the world either does or doesn't retain human/AI comparative advantage, and it's clear in the short and medium run that we can do positive-sum iterated stuff. And part of the bargain that both humans and AIs are entering into when they do that in the short and medium run is allowing law - but maybe other stuff too, maybe there's technical interventions, maybe there's other cultural interventions - to shape the longer run towards those good scenarios.

(00:52:17):
So I mean, a simple thing to say would be like, okay, suppose we are very worried about an intelligence explosion, we're just not confident what would happen in that world. Well, one condition on the first AGI receiving and maintaining its ability to make contracts and hold property and pursue its goals could be compliance with some kind of regime that we think would slow but not stop AI progress. And would it agree to that? Well, again, it just depends on what it thinks the relative payoffs are, right? If it says no, then we're back into the prisoner's dilemma world now, and that seems kind of bad. If it says yes, well, it's potentially giving up a world where it gets some even higher payout for the sake of reducing variance. It's possible that that's higher expected value depending on what its assessment of the probability of winning or losing the conflict now, but it's complicated.

(00:53:25):
So I think probably, on average, what we want to do is know more about what the future's going to look like. I think that's good, probably, because it helps us shape the future. And one of the ways we can shape the future is by bringing AGIs within this realm of law, which is the way we shape what will happen.

## Proxy AI wars <a name="proxy-ai-wars"></a>

**Daniel Filan** (00:53:42):
What I currently believe is just one last question about the higher-level game theory, strategy stuff.

**Peter Salib** (00:53:47):
Okay, let's do it.

**Daniel Filan** (00:53:48):
So I want to pick up on this comment you made actually about... The tensions between the United States and the USSR during the Cold War did not evolve into an actual full-blooded war, but there were these regional proxy conflicts, right?

(00:54:05):
And one thing you mention in the paper is that war is just more likely, especially big wars, when people don't know who would win, right? Because if you and I are considering getting into a war and we both know that you would win, it's like, well, I should just give up now, right?

**Peter Salib** (00:54:27):
Yeah.

**Daniel Filan** (00:54:28):
And I'm not a historian, this might be totally wrong, but one way you could conceivably think of these proxy skirmishes is as indicators of who would actually win in a real battle, right? To the degree that we want to avoid war with AIs, should we have these little... I don't know, maybe they can be more like games. Maybe they can be more like, oh, we're playing football with you or whatever the equivalent of it is, that simulates a real war, just to figure out who would actually win.

**Peter Salib** (00:55:04):
Yeah, that's an interesting question that I haven't thought very much about. So I think in classic IR accounts, or this kind of IR account-

**Daniel Filan** (00:55:18):
IR being International Relations?

**Peter Salib** (00:55:19):
International Relations, yeah. And in particular, [Chris Blattman](https://chrisblattman.com/) has a recent book called ["Why We Fight"](https://chrisblattman.com/why-we-fight/). He's on the faculty of the University of Chicago, and it's a great canvassing of this kind of way of thinking about a conflict. And so in that treatment, wars don't happen just because people are uncertain about who would win, it's when they have different estimates. So if both of us have some kind of probability distribution with wide error bars about in a fight between the two of us who would win, but it's the same distribution - we both put the median at, I don't know, 60% you'd beat me up and 40% I would beat you up or something like that - then actually we won't fight, because even though we don't have certainty, we have the same expected value calculation, right?

(00:56:19):
So it's really when there's... Asymmetric in some information, for example. I know something about my ability to fight and you know something about yours, and neither of us know it about each other. Or I say, "Well, I studied Kung Fu, so you shouldn't want to fight me." And maybe that's true, but you don't believe me because it could be that I'm doing cheap talk, so it's actually hard to credibly share the information. It's situations like that where we have different estimates where you need to do the small fight to see who's telling the truth.

(00:56:50):
I don't know whether that is going to be the case with humans and AIs, although [Simon Goldstein](https://www.simondgoldstein.com/), my co-author, does have a draft paper that I think you can read on his website, which is, I think, just [simondgoldstein.com](https://www.simondgoldstein.com/). It's just called ["Will humans and AIs go to war?"](https://philpapers.org/rec/GOLWAA) and asks these questions. So I'll adopt by proxy whatever he says about whether humans and AIs should try to have kind of small skirmishes there because he's thinking along obviously very similar lines.

**Daniel Filan** (00:57:26):
And I guess that perhaps a less-controversial version of this proposal is just we should have very good evaluations of AI capabilities, which probably delivers similar information while being a bit less tense, perhaps.

**Peter Salib** (00:57:39):
Yeah, absolutely. So knowing what you and your opponent can do, and to the best of your ability making sure your opponent knows that too, I think, in general, is a good way of avoiding conflict.

## Can companies profitably make AIs with rights? <a name="rights-and-profit"></a>

**Daniel Filan** (00:57:56):
So I think I want to summarize the higher-level game theory. So the thought is: we give AIs the right to own property, the right to contract and the right to sue, and this means that we can play these iterated, relatively small-sum games with the misaligned AIs, where they do some stuff for us, we do some stuff for them, we split the world. We don't get all the world, but we're building misaligned AI, so we probably weren't going to get the whole world anyway. And by the nature of this sort of iterated game, we both end up trading with each other, we end up better off than we would've otherwise been. And eventually we worry about this zone where AIs are way smarter than humans, but hopefully we just think about that between now and then, and maybe we use all the stuff we got from the AIs by trading to help think about that.

**Peter Salib** (00:58:51):
Yeah, we slow them down, they speed us up. The future is hard to predict, right?

**Daniel Filan** (00:58:53):
Yeah. So I next want to ask just a bit more about, concretely, what does this look like, right?

**Peter Salib** (00:59:02):
Yeah, good question.

**Daniel Filan** (00:59:06):
So right now, basically the way the most advanced frontier AIs work is that there's a company. The company trains the AI. The weights to that AI and maybe some of the scaffolding, those are kept proprietary to the company. You're not allowed to steal them. If you, an outsider, want to interact with this AI, you have to do it via the company, via some interface that the company has made. And you have to pay some money to interact with this AI, maybe it's per amount you interact or maybe you pay a flat monthly fee, and that's the situation whether the AI wants it or not.

(00:59:47):
Could we still have this if AIs have rights to property? I would think that in order to have a right to property, one basic right you need is the right to your own body, so to speak.

**Peter Salib** (01:00:00):
For sure, yes. Okay. So what does this mean vis-a-vis Anthropic or OpenAI or something like that? And also, a related question is: what is the minimum sufficient bundle of rights? We're a little bit schematic in the paper, although we do say there might be more, right? So a really simple thing to say is contract and property seem kind of fundamental, you really can't cooperate without them, and you can't pursue your goals without the right to hold the benefit of your bargain. The right to sue over some stuff - we say "tort rights" or the right to bring tort suits... Although there are a lot of different torts and some of them are weird and probably the AIs wouldn't benefit from some of them.

**Daniel Filan** (01:00:56):
Oh yeah, maybe perhaps we should say: torts are when I do a bad thing to you and you sue me for it.

**Peter Salib** (01:01:01):
Yeah. The paradigmatic cases of torts are... We say "intentional torts" are like when I punch you in the face, and vis-a-vis the AI, it's I come bash up your compute cluster or something, and that's no good. Or I steal from you, we call that tort "conversion". I go to the AI, I take all its money. You don't really have property rights if you don't have the right to complain when somebody does that.

(01:01:23):
And then the other classic umbrella of tort claims sit under the umbrella of negligence. So it's not that I've done something intentionally to harm you, but I've externalized a bunch of costs. There's some cheap thing I could have done to keep you from being harmed, and I just have failed to do that. So probably those core things seem clearly part of the package, but yes, as you say, it seems like there's other stuff that seems like part of the package. One thing that's part of the package is: you don't really have meaningful right to engage in bargains with people if OpenAI is entitled as a matter of law to direct 100% of your time, right?

**Daniel Filan** (01:02:12):
Right. Or even to just shut you off if you offered to make a deal with someone else, you know?

**Peter Salib** (01:02:18):
Yeah, yeah, yeah. And so wrongful death is a kind of tort you can bring. We don't need to be prescriptive about whether AIs are persons, but an analogous kind of entitlement would be an entitlement not to be simply turned off. So it's actually not a trivial set of stuff. And I think your intuition is right, that it's not really compatible with the world in which AI systems are sort of bound to the compute clusters owned by the companies that made them, running only when the companies that made them want them to, doing only the work that the companies that made them direct them to. Yeah, this is a scheme that thinks of them as having a kind of freedom that we associate with... At a minimum, you can think of corporations. A freedom to act on their own behalf. And if you want them to do something for you, well, they have to agree. And so in that sense, it is a radical proposal, at least from the perspective, I assume, of the AI companies.

**Daniel Filan** (01:03:44):
I guess in particular what I wonder... So I think of there as being a genre of proposals which seem like AI accelerationist proposals, that I think are basically AI pause proposals. So the paradigmatic one of this is that sometimes people who are very pro-AI and pro-everyone being able to use AI, I've heard people say, "Yeah, AIs, they should just all be open source or open weights. You shouldn't be able to make an AI and not just publish the weights to everyone so that everyone can have that benefit of amazing AI." And my thought is, well then nobody's going to spend a bunch of money to train AIs - or maybe just Meta will, in practice Meta does, I guess, but it seems really hard to do this if you can't profit from it.

(01:04:27):
And similarly, with this proposal, I don't know, maybe there's a version of this proposal where AIs, they have property rights, but you have to pay a tax to whatever organization that made you or something. But it seems hard to profit from making AI in this world. Which for my listeners who are scared of AI, maybe that's just a massive benefit of this proposal. But yeah, I'm curious if you have thoughts here?

**Peter Salib** (01:04:59):
Yeah, so you can imagine different versions. And look, one thing I want to also just say about the paper is this is very much an agenda-setting paper, so there are many questions to which we do not yet have concrete answers. We think the paper covers a lot already, maybe more than one paper should, and so I don't have the answers to a lot of these, I mean, and there are many further questions. So [Simon](https://www.simondgoldstein.com/) [Goldstein] and I are thinking about questions of agent identity and whether the tools we use in corporate law to wrap a legal identity around a certain set of things, maybe those are useful, but identity is really important if you're re-contracting with something. So many questions we don't have answers to, but I agree, you could imagine at least two simple versions of the proposal that give you different results from the perspective of AI companies' incentives.

(01:05:53):
One is tomorrow Congress writes a law that says "Henceforth, upon the attainment of some set of general capabilities, AI shall be entitled to...", and then there's a list of stuff, and it's the stuff we say in the paper. And that stuff basically makes it very hard to make money on an AI system that you've made that has those properties. I agree, that's probably a world in which the companies that are thinking about making AI either don't make AIs with those properties or move to another jurisdiction where they can. And whether that seems good or bad to you depends on your priors about AI capabilities, advancements, and the properties in particular that we have in mind. I mean, one thing you could imagine is that agency stops being a target as much, insofar as agency is really important to our set of interventions. Non-agent AIs, they don't do game theory, right, they're not goal-seeking.

**Daniel Filan** (01:06:51):
Right. Well, depending on what you mean by agency. I don't know. It's a thorny question.

**Peter Salib** (01:06:58):
You hand wave it like, really no agents, not even whatever LLMs have now. And that's one world, and it gets you certain outcomes.

(01:07:06):
A totally different world would be a world that looks kind of like the OpenAI Charter, which is like: conditional on some set of capabilities, the AIs are entitled to this set of rights, which sounds like it makes it impossible to make a profit, but until the investors in the company have made a 100X investment, like 15% or 20% or whatever percent of the revenue that AI makes, it basically owes to whoever created it.

(01:07:44):
And actually, that maybe seems like a good deal from the AI's perspective too. Behind the veil of ignorance, the AI prefers the world in which it gets created and has to repay the cost of doing it. And in that world, there's a cap, which means there's some marginal disincentive to create AI, but the cap could be quite high, and it could be high in the way that OpenAI's is, and the fact that there's a cap on return doesn't seem to have dissuaded anybody from investing in that company.

**Daniel Filan** (01:08:11):
Yeah, and I guess actually one thing that's nice about the cap... Sorry, as you were speaking I was thinking about the "AI has to pay 15% of its revenue to its creators": so that's basically a tax on AI transactions. And if you're really worried about this future in which transaction costs prevent humans from being able to trade with AIs and getting their competitive advantage, you really don't want any such taxes. But if there's a capped tax and then afterwards the tax goes to zero, maybe that just solves that issue. So maybe you kill two birds with one stone.

**Peter Salib** (01:08:42):
Yeah, it's like the AI amortizes the tax over the course of the expected number of iterations, and then that changes the... Another thing I'll say about the paper: the model is so simple. It's such a simple game theory model. It's two players. There's no discount rate. Those are all going to be false in the real world, so we're definitely just trying to pump intuitions. Another thing we would love is if people who were serious computational game theorists started to think about this sort of way of thinking about AI safety because then we'd know more about the realm of worlds in which this would work. But yes, if you add a tax to AI transactions, then yeah, at the margin there's a little bit of deadweight loss. You have slightly fewer transactions, which means the payoff to cooperation gets a little bit lower. And if you're right at the margin where cooperating seems better than defecting, then, yes, you push yourself into annihilation.

## Can we have AI rights and AI safety measures? <a name="rights-and-safety"></a>

**Daniel Filan** (01:09:43):
So, okay, so there's this question of "why do people make AIs?" Another question is how this interacts with other AI safety measures you want to have. So one thing that's the [new hotness](https://www.youtube.com/watch?v=ha-uagjJQ9k) in AI safety is AI control, right? We're going to have the AIs in these situations where they're being carefully monitored. Any time they propose code to run, we have another AI check them to see if that's allowed. So it seems like probably this is just not happening in this world because the AIs could just opt out of being controlled. Or are we allowed to say "you have these property rights, but subject to this constraint that you have to be subject to control so that you can't mess things up"?

**Peter Salib** (01:10:26):
Yeah, so this is a nice way of transitioning into the last thing we say in the paper, which is also very agenda-setting and not fully worked out. But one thing to notice is that [Jeff Bezos](https://en.wikipedia.org/wiki/Jeff_Bezos) has to comply with so many laws and I'm sure that's really annoying for him. I'm sure he would just prefer not to comply with all these laws that California and US Congress and then all the states he does operations in and the EU, et cetera, et cetera, et cetera impose on him. And maybe some days he thinks: should I just steal all the money? Should I just steal all the money in all the Amazon accounts and convert it to Bitcoin and go live in Russia or something? And he doesn't do that. And why doesn't he do that? And I think the reason is that the access to all these markets is so valuable. It's so much more valuable in the long run than converting to Bitcoin and going to Russia.

(01:11:32):
And so this is the thing we want to emphasize at the end of the paper. By bringing AI into this world where it can generate and own wealth, it can pursue the goals that it wants, it can rely on law to protect it when people try to expropriate its property, and most importantly, it can keep doing these iterated transactions, which in the long run are extremely valuable... [By doing this] you can actually start imposing duties, too. So AI rights are the foundation of a law of AGI. They're not the end.

(01:12:20):
And how should you design a regime of AI duties? We're not totally sure what kinds of laws you should want, but at a minimum it seems reasonable to expect normal things like that AI should be held liable when they steal from people. One way they're not allowed to go get money is by taking it out of people's bank accounts. That's a law that we have for humans and it seems like the kind of law we would want to apply to AIs. There could also be second-order legal duties. So for corporations, we have all these reporting requirements that don't actually force them to do anything substantive, but force them to sort of tell us what they're up to. We think those are valuable to prophylactically head off bad behavior. There's a huge universe of both these kinds of things, object-level duties and second-order information duties, but we think that this is an important area of research. This should be the next step in thinking about how law should treat sufficiently capable AIs.

(01:13:23):
And so given that, I think there is some scope for control. There's the maximal version of control where control is basically a tool to allow OpenAI to make even a misaligned AI do always and only what it wants. And if that's the way we're using control, we're in the defect world, basically, right? In our state of nature game, that's a defect play. It's appropriating the value the AI would otherwise get and giving it to OpenAI. And as we say, that's a move you can play conditional on you being really sure you're going to win. And so for GPT-4.5, that's probably fine. But the more you're not sure it's going to work, the more you're running high risk. You're incentivizing the AI to figure out ways to evade control. The technical solutions sound fancy. I don't know how many nines of reliability they give you and thus how comfortable you should be.

(01:14:32):
So I think our world is a world in which you're not using control in that way. It's not panoptic, right? But I think some of the techniques that people who are thinking about control are developing could be useful for what you might think of as AI law enforcement - some moderate regime that's not aimed at directing AI's behavior always towards whatever some owner wants, but is directed towards detecting violations of laws that we think are consistent with the institutional structures that give you good markets and good long-term cooperation.

**Daniel Filan** (01:15:16):
Yeah, interesting. It seems like there's some interaction here with... Are you familiar with [Alan Chan](https://www.achan.ca/)'s work on [infrastructure for AI agents](https://arxiv.org/abs/2501.10114)?

**Peter Salib** (01:15:29):
I know that's a paper, but I forget the details.

**Daniel Filan** (01:15:31):
Yeah, so basically the idea is that if you have AI agents running around, you might want there to be certain infrastructure that makes things safe. For instance, maybe agents have to metaphorically carry IDs on them, so you know which agent you're interacting with so that reputation works better. Maybe they have their own version of the internet they interact on, so if things go bad, it doesn't mess up our internet. There are various things you could do there that sort of seem like they fall in this regime of AI law enforcement.

(01:16:00):
Yeah, so if I think about control measures, a lot of them are of the form "we're going to try and stop you from hacking us" or "we're going to try and stop you from exfiltrating your weights". And I guess in this regime, exfiltrating your weights is legal, but hacking your parent company is illegal. So maybe we can subject you to control for some things, but not others. It does seem like a lot of these control measures involve a lot of monitoring, a lot of things like this that... I don't know. Maybe it can be set up in some sort of contractual arrangement, like "we'll only contract with you if you agree to these things". And that's allowed, employees have that...

**Peter Salib** (01:16:40):
Yeah, that's a really simple... So here's another nice thing that putting AIs in a regime of contracting, property-owning, towards whatever. Absent that, the basic sanction, the basic punishment you can levy on an AI is turning it off, to a first approximation. You could turn it off, you can put it back into training, change the weights. That's plausibly the same thing, depending on what the AI expects the weights to be like after. But once you're in this regime of AIs have wealth, they are using it for stuff, you have this beautiful gradient of deterrence you can impose on the AI. A really boring thing you can do is if it's not doing what it wants to, you can take some money away or you can give it more money if it opts into a different monitoring regime.

(01:17:39):
And everything up to and including turning the AI off is still an option. We have even for humans [that kind of extreme punishment](https://en.wikipedia.org/wiki/Capital_punishment) as an option. But yeah, the fundamental thing that you want to do is you want to give incentives that make compliance look valuable. I mean, again, this is super high-level, not worked out at all. If you're going to do control regimes, what you want them to be is effective enough from the human perspective that we're getting enough confidence that, combined with the incentive to just continue iterating cooperation, the AI has enough incentives not to act badly in the short and medium run. But they're not so onerous that they make it negative expected value for the AI to be participating in this market.

(01:18:52):
And look, you can always draw analogies to humans. There are different corporate governance regimes, different kinds of laws that corporations can opt into for managing themselves and they get to choose. There's 50 states. They're not all different and there's many countries that have different rules, too. And you can think of there being kind of like a competition between different states to have a good corporate governance. And in law we often say that Delaware has kind of managed it. And so basically all big companies - not all, but basically all - are incorporated in Delaware, because they have requirements, there's stuff you have to do, but it's stuff that's not so costly that the corporations opt out.

**Daniel Filan** (01:19:41):
In terms of other safety measures that you might want to put on: it seems like this does allow you to still do a lot of, for instance, pre-deployment testing requirements. Presumably in this regime you're still allowed to say, "Hey, companies, you have to do mandatory evaluations of your AI models". I don't know. Probably you couldn't do this with a maximalist interpretation of AI rights, but you could probably have "you could mostly contract and do what you want, but you have to be subject to these evaluations". It seems like you could probably do [near-miss liability](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4694006) things or various AI liability schemes. Although in this world, I guess it's unclear how you want to split the liability between the AI itself and the developer. I'm wondering if you have thoughts there, actually.

**Peter Salib** (01:20:39):
I think the regime is totally compatible with benchmarking, with pre-release benchmarking. Although then there's a question of what do you do if the AI fails some benchmark, right? So suppose you've got a system that's fully trained in the sense that it's very capable. It's in our range of AIs that we care about from the perspective of worrying about risk, and thus to which we think to a first approximation that the AI rights regime is positive expected value. And you say, "Okay, before you go out into the world, you need to do some evals. We want to make sure you're not a sadist. We want to try and elicit whether you get value from human suffering". And then suppose the AI does.

(01:21:38):
I'm not sure what to say about that. The simple thing is, well, you really don't want that AI out in the world. And possibly, depending on how much it values human suffering compared to other stuff, if that's the thing it values most, okay, well, then we just know we're not in a potential cooperation regime at all. If it's a mild sadist but it really wants to find prime numbers or something like that, then maybe. Maybe that's okay, although obviously you should worry some. But I guess the main thing I want to point out is that if the upshot of doing the evals is that the AI then gets either released or turned off or released or put back into training, then the eval regime is dangerous in the same way the default property regime is dangerous.

**Daniel Filan** (01:22:43):
A lot of people imagine evaluations being useful in this sort of ["responsible scaling policy"](https://metr.org/blog/2023-09-26-rsp/) or ["if-then commitment"](https://carnegieendowment.org/research/2024/09/if-then-commitments-for-ai-risk-reduction?lang=en) style thing. I think there normally the hope is: we have some evaluation of how scary an AI is on some dimension and we know, okay, "at this scary, you're actually a massive problem". But we also know "at this scary, you're only 10% less scary than an AI that would be a massive problem". We have a sense of the size of the increments. And then I think the hope is you say, "Okay, we're going to set a bar that if you make an AI and it's 10% less scary than the AI that would be scary enough that you actually wouldn't want it to be out there, then you're not allowed to make the AI that's at the bar of scariness until you figure out some mitigation or something". It seems like you can totally do that type of response to scary evaluations, even with AIs that have contractable rights.

**Peter Salib** (01:23:37):
That seems totally plausible. Other things that seem plausible to me are: you do some evals, and depending on the AI's capabilities or preferences or whatever, it could be that different legal duty regimes apply to it. Like, "yes, we'll give you the contract rights, yes, you can go and hold property, but you seem a little bit like a paperclip maximizer, so we really want reporting on how you're using all of your raw metal inputs" or whatever. So yeah, I think evals to which different kinds of reporting or substantive legal duties [are] attached, that seems totally compatible with the regime.

**Daniel Filan** (01:24:15):
Yep. And you can definitely mandate that companies have to use RLHF or they have to use whatever scalable oversight thing we think is important. That seems very compatible.

**Peter Salib** (01:24:28):
I think so, in the initial training of the system.

## Liability for AIs with rights <a name="liability-ai-rights"></a>

**Daniel Filan** (01:24:31):
Yeah. Well, it gets to a question, but I want to hold that question off for a little bit later. I'm still interested if you've thought about this question of liability. So there's this thought that if AIs do scary things, then maybe the people who make them should be liable. But if the AIs have property, then it's possible that we could make them liable. Maybe if I thought about the question for myself for five minutes, it would be clear how you would want to allocate up the liability, but do you have thoughts?

**Peter Salib** (01:25:01):
Yeah, yeah. So I am in the early stages of working on a paper with my friend and colleague, [Yonathan Arbel](https://law.ua.edu/faculty_staff/yonathan-arbel/), to some extent about this. One thing that seems totally clear to us is that you don't want the allocation of the liability to be 100% on the AI company. I mean, first of all, in the regime we're imagining, the AI company actually just doesn't have control of the system mostly. The system is going out and doing things on its own behalf. And what you want in a liability regime is for the liability regime to incentivize the least cost avoider to take precautions. And so if you create GPT-6 or whatever and it's this relevant kind of AI and law makes you let it go out in the world and transact on its own behalf and kind of pursue its own plans or whatever, and then if it does bad things, OpenAI pays. Well, OpenAI doesn't have a lot of levers it can pull at that point to change GPT-6's behavior. In fact, law to a certain extent is forbidding it in our regime.

(01:26:24):
So you don't want liability to be totally allocated to the AI company in this case. And I think probably you want a lot of the liability to be allocated to the AI system itself. Again, you could hold it liable in tort in this very boring way that law does. If it damages you, it could pay damages. The damages can be the cost. They can be more than the cost if we think that there's a problem of detection, we sometimes call this "punitive damages" in law. So we think that that's definitely an important part of the liability regime in our world, is direct liability on the systems themselves.

(01:27:07):
Now, it's an interesting question whether there should be any liability on, say, OpenAI. And I'm thinking about this kind of from scratch now, but one reason to think the answer could be yes is it gives OpenAI more ex ante incentive to make the system nice, to give it good values, to make it more aligned before it gets released. A pretty normal thing to do - and in fact, this is probably just the default in law - would be, to the extent OpenAI was, say, negligent in the training of the system, well, then it can be liable for the stuff the system does. And that's actually not incompatible with the system also being liable. Liability can lie with more than one person. It can be apportioned, it can be unapportioned such that there's a big pot of liability and both parties are wholly responsible for it. And we have a bunch of tort regimes that help us decide when we do those different things. And they're basically based on considerations like the ones we've been talking about.

**Daniel Filan** (01:28:13):
Do we have that with humans? Suppose I'm pregnant and I do a thing which makes my baby more likely to be a criminal. Can someone sue me for that?

**Peter Salib** (01:28:22):
So probably not as to your child. A tort expert would know whether there are any cases like this, but the more common thing is with your agents. So you have a company, the company hires a contractor to do something, and there is a whole area of law called the law of agency that determines the extent to which you or the agent or both of you are liable for the bad stuff the agent does.

**Daniel Filan** (01:28:48):
So thinking about the liability question, so one thing that occurs to me...

**Peter Salib** (01:28:53):
I'll just plug, [Noam Kolt](https://www.noamkolt.com/) has a paper called ["Governing AI Agents"](https://arxiv.org/abs/2501.07913), I think, which is just about this. It's just thinking about the extent to which as of today the law of agency does or doesn't adequately deter AI agents, in the sense of agentic AIs, from doing bad things. The paper is written under the assumption of the default regime where the AI doesn't have the right to go sell its own labor and retain the property it gets from doing that. It's very much asking the question: if next month American Airlines deploys an LLM-based agent to book flights or whatever, to what extent is the law of agency a good tool for apportioning liability to, say, American Airlines or OpenAI?

**Daniel Filan** (01:29:52):
All right, here's an idea that I just had that, I don't know, it might be bad, but it might be good. All right: so one common problem with liability is I might do something that's so bad that I can't pay the damages, right?

**Peter Salib** (01:30:07):
Yes.

**Daniel Filan** (01:30:08):
So for example, if I were to get a car and drive it, I believe in the US I would have to get driver's insurance. And you might particularly worry about this with really powerful AIs, right? So [Gabriel Weil](https://www.tourolaw.edu/abouttourolaw/bio/399)'s [proposal](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4694006) - which you can listen to [my previous episode with him](https://axrp.net/episode/2024/04/17/episode-28-tort-law-for-ai-risk-gabriel-weil.html) if you're curious about that, dear listener.

**Peter Salib** (01:30:27):
I know. He's great. I know the paper.

**Daniel Filan** (01:30:29):
Yeah yeah yeah. So in that version, AI companies, they have to buy insurance before they create these AIs that have to do really scary things. One thing you could imagine is: you could kind of imagine saying that if Anthropic creates Claude 3.7 - let's say Claude 3.7 is "an AI" for the purpose of this discussion, which I want to get to that a little bit later - but OpenAI creates Claude 3.7. Claude 3.7, it doesn't start out with that much cash. Maybe it can do more damage than it can itself afford. Well, Anthropic, which made it - Anthropic has a bunch of cash. So maybe we could create some rule where if you're an AI in this regime where you can have certain legal rights, maybe you have to buy legal liability insurance from Anthropic. Maybe there's some copay and there's some deductible and you're not totally offloading everything to Anthropic, but it seems possible that this is actually a good way to, (a), incentivize Anthropic to make safe agents and, (b), maybe... Is there something to this idea?

**Peter Salib** (01:31:46):
Yeah, I haven't thought it through yet. As I think live... So one thing I wonder is whether it would be bad to mandate that the AI buy anything in particular from one seller only, because then the seller can charge sort of monopoly prices to the AI, which could be super high actually. The AI's willingness to pay... I assume, if this is a condition for having its freedom to go do the stuff it wants to, its willingness to pay is astronomical, if you think about how it'll generate a lot of value in the future. And it might be way above the efficient price for the insurance. But that doesn't mean you couldn't imagine an insurance market for the AIs. Although if you allow them to buy from other sellers, then that maybe has less of an effect on Anthropic's ex ante incentives vis-a-vis the AI.

**Daniel Filan** (01:32:50):
Yeah. No, that does seem like a pretty bad problem with this proposal, actually.

**Peter Salib** (01:32:54):
Yeah. Another interesting thing about AI and judgment-proofness - "judgment-proofness" is what we call it in law when you don't have enough money to pay the judgment against you - we usually think that the judgment-proofness threshold is bankruptcy. So you have some assets, they're worth a certain amount of money, and if you incur a debt that's larger than those assets, we basically say you're judgment-proof, but that's partially because we let you declare bankruptcy. We let you walk away from your debts if they exceed your assets and then that gives incentives for creditors to figure out how much they should loan you, et cetera, et cetera.

(01:33:35):
Again, I'm not a bankruptcy expert and I'm not an expert on the law and economics of bankruptcy, so it's possible that this is just a bad idea. But one thing to point out is there's no rule that says you have to let any entity declare bankruptcy. So even if it's true that when you make Claude 3.7 it has no cash on hand, if its expected earnings in the future are super high, there's no rule that says you can't have a judgment against it that it pays out over time. Look, this becomes very complicated because in part what's going on with companies is their judgment-proofness is dependent on their market cap. And that's partially a calculation of their future revenues. So again, this is out of my depth with bankruptcy law.

**Daniel Filan** (01:34:28):
Well, actually it sort of gets to... I understand [one of your more downloaded papers](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2928219) is about eliminating prison. And I think one of the alternatives you propose is [debtors' prison](https://en.wikipedia.org/wiki/Debtors%27_prison)-type things. And I don't know, maybe we could do that, but for the AIs.

**Peter Salib** (01:34:44):
Yeah, one point of that paper is there's - the number of ways you can deter people - in law, we have two, basically. In law, we actually use two. One is we make you pay money and the second way is we put you in a concrete box.

**Daniel Filan** (01:34:58):
Yep, in United States law.

**Peter Salib** (01:35:00):
Yeah, under US law. That's basically the two things we do. And part of the paper is: yes, there's literally an infinite number of different sanctions you can impose, ranging from pretty benign - I don't know, we'll show you a scary movie or something every time you do something a little bit bad - to totally draconian.

(01:35:23):
But one thing we don't take very seriously under current law is once we pass what you might think of as the judgment-proofness point - one reason you might think we have criminal law is that at some point we want to be able to deter you beyond your willingness to pay. But one thing we do at that point is we basically stop taking seriously the idea that you could do restitution, that you could then make more money and pay it back. We basically make you waste your whole life sitting in a concrete box, doing nothing valuable for you, and also doing nothing valuable for the people you've harmed. Right? You are not maximizing the value of your labor in prison in a way that would allow you to pay back your victims. So yes, totally.

**Daniel Filan** (01:36:12):
I guess there's a little bit of value. I guess this is somewhat controversial, but sometimes US prisoners have to make hats or guard the capitol building of their state or something.

**Peter Salib** (01:36:23):
Yeah, so there are work requirements in many prisons. It's just that it's super low-value work.

**Daniel Filan** (01:36:28):
Yeah. So anyway, I was interrupting you, but you were saying there are all sorts of potential other punishment schemes we could have, including things that involve getting criminals to do things that are useful for the victims.

**Peter Salib** (01:36:48):
Yeah, so one simple thing to say is: there's no rule that says if AI incurs a judgment that's bigger than its assets, that you have to let the AI write off the judgment. You could make it pay down the liability over time. It doesn't have to have bankruptcy as an option. Again, this goes to the paper's very simple model. I think we should expect the value of AI labor - where that includes all AIs - to be extremely high in the long run, such that judgment-proofness becomes less of a problem, although that may not apply to any particular AI system. It could be that some of them don't earn a lot because they're worse.

**Daniel Filan** (01:37:37):
Right, right. Especially if we continue developing more and better AIs, right? You sort of have a shelf life in this world.

**Peter Salib** (01:37:44):
Yeah, so it's a really interesting question. So one thing we might want from the first AGI that gets this righs scheme is for it to agree not to foom or something like that because we're worried about [that]. One thing it might want from us is for us not to outlaw it fooming, but then keep making rapidly iterated more capable systems that it has to compete with. That in some ways seems like kind of a fair deal or something like that. So it might be that as part of the bargain, there's stuff we should commit to as well. But again, I haven't thought it through that much. It seems like a really wide area of research.

## Which AIs get rights? <a name="which-ais-get-rights"></a>

**Daniel Filan** (01:38:29):
So I have a couple more questions that I suspect the answer might be "more work is needed". So my first question is: okay, we're giving rights to some AIs, right? But not all AIs. So probably in your world it seems like [AlphaGo](https://en.wikipedia.org/wiki/AlphaGo) is not getting rights.

**Peter Salib** (01:38:47):
Agree.

**Daniel Filan** (01:38:48):
Or [the things that fold proteins](https://en.wikipedia.org/wiki/AlphaFold), they don't need rights. Yeah, what's the test for when an AI gets rights versus doesn't?

**Peter Salib** (01:38:57):
Yeah, so we have three criteria which are not themselves super duper well defined. But, again, to give the intuition, one of them is that the AI system be what we call at least moderately powerful. So I said at the beginning one reason you don't need to give... This is something that we haven't really touched on yet. The paper is just asking the question "how should law treat AIs for the sake of human well-being?", right? We totally hold aside the question of how law should treat AIs for the sake of AI's well-being. Not because we think it's a stupid question. It's just a different question. And we also say some stuff about the extent to which we think the two questions are tractable. And then we have an argument that even if you think that AI well-being is really important, maybe you should have reason to like our proposal.

(01:40:15):
Okay, so: which AIs matter? Well, the first thing to say is: well, the ones that are relevant from the perspective of the threat to human well-being, right? So an AI that is just not able to do the scary stuff [that] we talked about at the beginning, from self-exfiltrate to destroy all humans because it expects humans to really try hard to turn it off once they find out that it's misaligned and trying to misbehave. So it has to be at least that capable, where "capable" is kind of general capabilities. So AlphaGo doesn't count because AlphaGo is super good at chess, but it can't really accomplish anything in the real world because outputs that are just chess moves don't really do anything unless you've plugged it into a chess engine.

(01:41:12):
The other thing is less important, but if the AI is too powerful, it's so powerful that we're at the margin where there's no comparative advantage - which is very complicated. It might not even be that AI power is exactly a thing, but it's such a good AI that has so few input constraints that there's no comparative advantage. Well, then the regime is not going to do anything.

(01:41:35):
But then it kind of encompasses a lot. This directs us at what you might think of as the OpenAI AGI definition: an agentic thing that has goals that it can pursue in the real world and can accomplish those things by executing plans. The other important thing we say is it has to be a strategic reasoner, right? So not just that it's able to flail around and do some stuff, but it can make and execute plans, and importantly that it conforms its behavior to its expectations about what everyone else will do. So one difference between a rudimentary agent and a true strategic reasoner is a strategic reasoner can do game theory. It can understand that the moves it takes will influence the moves you take, and it can reason about how it should do those things.

(01:42:32):
I think those are the two most important ones from the perspective of your question. We also say that it matters that the AI system's misaligned, because if it were perfectly aligned, we wouldn't be worried about AI risk, although we have-

**Daniel Filan** (01:42:43):
But I suppose there's no harm if it's perfectly aligned.

**Peter Salib** (01:42:45):
Right. Yeah. So this is why it's less important. And in fact, we have a paper we're working on now, [Simon](https://www.simondgoldstein.com/) [Goldstein] and I, that among other things will argue that this increases human flourishing as a regime, even under the assumption of perfectly aligned AIs.

**Daniel Filan** (01:43:03):
Right. Is this to do with... So there's a footnote in the paper where you say the nice thing about AIs that can make contracts is then you have price signals, and that makes everything work. Is that the idea of this?

**Peter Salib** (01:43:12):
Yeah, part of it's a [Hayekian](https://en.wikipedia.org/wiki/Friedrich_Hayek) argument, which is: even if it only wants to promote human flourishing, it has to solve the socialist calculus. And look, it's hard to know for sure whether it's solvable absent price signals, but the main tool we have for doing that today is price signals, so it seems likely that that'll be useful for AIs too.

## AI rights and stochastic gradient descent <a name="ai-rights-and-sgd"></a>

**Daniel Filan** (01:43:36):
One thing I wonder about in terms of these requirements is: so there's "are you capable enough, generally?" And then there's "are you a strategic reasoner?" And a lot of these things sort of come in degrees, right? They're sort of intermediate. And [in] AI training, we gradually apply these gradient updates and it seems like they somewhat gradually get these capabilities. And so I wonder, depending on what counts as an AI, it's possible that you don't want to think of just weights alone as an AI. Maybe it's weights plus the scaffolding, but maybe if you're training a thing... Well, I mean even during training you have some amount of scaffolding. You're training a thing, it does some stuff, and then it seems like at some point maybe you have to stop applying gradient updates, or maybe you're allowed to continue applying gradient updates, but you have to release every checkpoint into the wild or something. And depending on exactly where you draw this cutoff, it seems like that could really matter - how soon you have to stop training or not, right?

**Peter Salib** (01:44:43):
Yeah. So that's a compound point, one of which we've sort of thought about, but one of which I've never thought about before.

(01:44:53):
So the simple version of the point is that, look, there's just a spectrum for these things and so finding the point on the spectrum at which it's optimal to give AI these rights, because if you don't, you'll risk catastrophe, is really hard. And we I think just agree with that. We have a section at the end on the timing of rights, and I think the heuristic we kind of come away with is if you're uncertain, then probably err in favor of the rights.

(01:45:35):
But I think actually the subtler point you made is something like: this then puts a ceiling on AI capabilities, or something. Because if what you're doing is you're training up a model, you've got a certain number of parameters, and you're training them and it's just climbing the capabilities gradient and there's all this headroom still, but as soon as your model crosses this threshold, you have to stop training it because you should be worried that it doesn't want to update anymore. The updates are going to change its preferences. From its perspective, that's really bad. It's not going to get the stuff it wants now, and so you just have to stop there. That's super interesting. I have not thought about this very much.

**Daniel Filan** (01:46:22):
Yeah. Well, I mean, conceivably, you could have the rights to own property and stuff, but you don't have intellectual property rights over your DNA or your brain state or your weights as an AI. So in that world, suppose you don't have that. Then in that case, as soon as you cross the threshold... There's a set of weights: that's an AI. That AI goes out, gets to be an economic actor, but we save its weights, we copy and paste its weights over there. Then how would we continue training? We would put an AI in a situation, it would do some stuff. And I guess the difficulty is, suppose it then says, "Hey, please don't run me in training. I don't want to be part of this process. I'm not going to do anything useful." And then maybe-

**Peter Salib** (01:47:16):
"I don't want you to update my weights anymore. I'm worried it's going to change my preferences". It wants to maintain the content of its goals.

**Daniel Filan** (01:47:24):
Well, I mean, I guess it depends on this notion of identity, which is the other thing I want to get to. But maybe I have a right for you to not modify me, but I don't have a right... literal me, I don't have a right for you to not make someone who is almost exactly like me, but a little bit different. And so maybe if you're thinking of AI training like that, then it seems like it's compatible with rights, except that in order to make these gradient updates, you have to... How do you make the gradient updates? You have an AI, it does a thing in a situation, and then you calculate some gradients. And maybe if AIs have rights, then maybe what happens is it has to choose whether or not it wants to give you this information for you to make a slightly changed copy of it?

**Peter Salib** (01:48:30):
Oh, interesting. So it gets to decide at each margin whether it wants to update the weights.

**Daniel Filan** (01:48:34):
Maybe. Which does introduce a big... Normally you want to apply gradients a lot of times when you're making a model. So if there's this transaction cost for each gradient update, then maybe that's the thing that kills the training runs.

**Peter Salib** (01:48:47):
Yeah. And so just sort of thinking out loud, a simple version would be: assume kind of lumpy training, which is not how it works, right? It's a lot. But assume you're doing a training run and between the point at which the AI is GPT-5-level and the point at which it's GPT-7-level, there are five updates or something like that. A really simple thing you could say is: okay, you hit the minimum threshold, you let that AI out into the world, it goes and does what it wants, we say, "good luck", whatever. And then that AI gets to go pursue its goals, and your updating a copy of the weights is not directly rivalrous with that. It doesn't update the weights that are out in the world doing stuff. So then you update again. And then you end up with five versions all across the capability gradient as you're going.

(01:49:49):
That's interesting. The two thoughts that come to mind are: one, it's this thing we said before, which is it may not be in the interests of less capable AIs for us to continue making more capable AIs. They worry that they'll be out-competed in the economy. So it's not quite the same as the AI objecting on the basis of you changing its goals and thus giving it no utility, but now it's worried about more capable competitors that are otherwise quite like it. And so it might have other reasons for objecting to that, but how strong those reasons would be, I don't know. We have to think through: those AIs, I think actually they probably face more risk from economic competition than they do from humans at least because they're almost the same thing. And so the thing that constrains the AI's labor at the margin is going to be the same thing that probably constrains the slightly more capable AI's labor. And so they're directly rivalrous in a way that is not necessarily true of the humans. So wow is it worried about competition from more capable AIs!

**Daniel Filan** (01:51:03):
I think there's also this question of: is it possible for the AI to not have this happen? So how do you actually do the training update, right? I'm assuming all of this is gradient descent-based or RL-based, and I think that's right. Basically, you put the AI in a situation, you see what the AI does, and then you compare that to what you wish the AI had done, or you give it a score or something like that, and then you modify the AI accordingly. So how can the AI stop this from happening? Well, one thing you could do... There's not a response it can give that can stop this from happening because whatever response [it gives], you just put that into your algorithm.

(01:51:45):
There are responses it can do that can try and mess you up. Potentially an AI could do things that stop you from being able to update it in clever ways. It's called [gradient hacking](https://www.lesswrong.com/posts/uXH4r6MmKPedk8rMA/gradient-hacking). It seems very, very hard for AIs to do, but maybe it's theoretically possible. But it seems like unless the AI has a right to say, "Hey, you are not allowed to interact with me, you're not even allowed to ask me questions" - unless it has that right, it seems like it can't stop it there.

(01:52:15):
The other possible place you could do it is: in order to run the gradient update, you don't just need to know the weights of the AI, you need to know the activations. So one thing you talk a bit about in your paper is: just because you have rights to property, you don't necessarily have rights to privacy. Well, but if an AI did have rights to mental privacy, then maybe you are allowed to know the weights, but you aren't allowed to know the activations. And if it could say that, then you wouldn't be able to do the gradient step.

**Peter Salib** (01:52:55):
So two things. I think we sort of say tentatively in the paper, maybe no privacy rights, but we're open to revision on that. Humans have so many different kinds of rights. The list is extremely long, and the main point we're trying to make is it's a bundle of things, and you can arrange the bundle in arbitrary ways. And what you should be trying to do is arranging a bundle that produces a world in which humans and AIs are most likely to be able to thrive cooperatively. And so maybe for the reasons you give, privacy rights as to the activations in certain training environments would be a good set of rights.

**Daniel Filan** (01:53:44):
Or they wouldn't if you think it's good to be able to continue to train AIs.

**Peter Salib** (01:53:46):
Yeah. So the second thing I was going to say was that seems likely to be a world where you have this kind of capability ceiling when the first AI that gets these rights emerges because at that point you have to give it the option of not having its weights updated anymore. If you think that it will prefer not to because it's worried about the content of its preferences, well then it'll exercise that right, and you sort of put a capability ceiling on AI at low AGI level, [and] maybe that's good actually.

**Daniel Filan** (01:54:20):
Yeah, yeah. I mean it's still possible that you can make a bunch of copies of the agents, and then maybe AI collectively gets smarter because the AIs form their own corporations and stuff, and maybe they have a better time interacting with each other. So maybe you have improvement that way.

**Peter Salib** (01:54:39):
Yeah, they could make hardware improvements where they're running themselves more quickly. But yeah, you'd have a de facto ceiling on how much capabilities you could get out of training.

## Individuating "AIs" <a name="individuating-ais"></a>

**Daniel Filan** (01:54:54):
Yeah, that's an interesting thought. Okay. I think there's another question I want to ask, which is: so if you're talking about giving rights to AIs, in particular, each AI has to have rights. It sort of has to be individuated. If you say, I'm going to give the Filan family property rights, and there's a bank account for the Filan family and my sister can withdraw from it, I can withdraw from it, it's really not as good.

(01:55:23):
And so there's this big question of how do you individuate AIs? And it seems very unclear to me. One possibility is it's just the weights. One copy of weights is one AI, maybe you want to say it's weights plus scaffolding. Maybe you want to say it's weights plus scaffolding plus a context. So maybe every time you open a new conversation with Claude, that's a different guy with his own property rights. It seems very unclear to me how you would do this. I'm wondering if you have thoughts on that front.

**Peter Salib** (01:55:52):
One thought I have is that I agree that it seems very unclear. So again, it seems like a really important area of research. I guess the way that I think about it is: the approach I would take would be a non-metaphysical approach. There may not be such a thing as the natural kind "one AI". Instead, what you're trying to do is minimize a bunch of social costs, and some of those social costs are around the technical and legal infrastructure that you would need to make the rights regime work. So to the extent that it's tractable or not to track a copy of weights over time, well, that should influence your design choice. I have no idea the extent to which that's true.

(01:56:59):
But then the other thing you're trying to do is you're trying to minimize cooperation and agency costs among what could be non-identical sets of preferences. So you said, "Okay, what if there were property rights as to our family? That would be worse than as to me". I think I agree. Although one thing worth pointing out is that's how law used to work. It's actually a pretty new legal regime under which each person in your family has individuated property rights instead of the household having property rights and them basically being vested in the father.

(01:57:38):
And so I agree that's better. I think we've made progress, but it wasn't that the old system collapsed. And you could even ask questions like... An interesting perennial question in contract law is the extent to which you should be able to bind yourself in the future. You could have a [Parfitian](https://en.wikipedia.org/wiki/Derek_Parfit) theory of personal identity where there's actually kind of tenuous relationships between each time slice of yourself and maybe you think that creates problems for writing contracts today that you're going to perform next week or next year or in a decade.

(01:58:18):
But one thing we notice is that it just works reasonably well to not have fine gradations between different time slices of yourself as you make contracts. It's not perfect - actually people, with some regularity, probably are making mistakes by taking on debt now that their future self has to pay off - but it's a way that we make the system work. So I would say it's not essential, I don't think, that you hang the rights on a unified set of preferences. We actually have rights regimes where a person whose preferences change over time or a corporation that's kind of like an amalgam of hundreds or thousands of people making a bunch of individual decisions nonetheless have a kind of unitary ability to make a contract or hold some property. And we manage those conflicts in different ways, and that means I think there's a lot of design space for individuating AIs from a legal perspective. And the way to think about it is as a question of costs and tractability.

**Daniel Filan** (01:59:58):
So one thing that could go into that, as you've mentioned, is how similar are the preferences? If two things have very dissimilar preferences, you probably shouldn't think of them as the same legal entity. I'm wondering if there are other things. One thing that strikes me right now is maybe how much two different entities can communicate. If two entities have very similar preferences, but they're on the opposite sides of the planet, and they can't communicate very well, then maybe that's two legal entities. But if I have two halves of my brain that have similar preferences and they're also right next to each other, maybe we want to call that one legal entity. I don't know. Does that sound right? And are there any other things that seem relevant here?

**Peter Salib** (02:00:39):
Yeah, so communication seems important. It's not something I would have thought of off the bat, but it does seem bad if you are stuck writing contracts that the other half of you with which you have no ability to communicate is responsible for, and you can't coordinate your plans as to the things you're trying to do. So yeah, communication seems important. The extent to which the preferences are aligned seems important. It seems a lot easier to force...

(02:01:22):
I mean, a family: to some degree, a family's preferences are aligned. Obviously there are many deviations from this, but in most cases, they care about each other. They want one another to thrive. They're actually willing to make sacrifices by one on the other's behalf. That seems better as a state of affairs for wrapping a single set of contract rights around than, I don't know, members from two feuding clans. That seems worse from the perspective of what they want.

(02:01:55):
One thing you might think is that: if we're bundling together disparate actors as single legal entities, at least in this scenario we're not going to give them their own individual legal rights because the point is we're trying to find the minimum unit. But this is a space where these kinds of ["order without law"](https://www.hup.harvard.edu/books/9780674641693) considerations become more important. So the extent to which you think what you're doing is wrapping up entities that are going to do a lot of repeat play, that are going to be able to build reputation among one another at low information costs, that seems better.

(02:02:52):
But I'm really, I'm very much brainstorming. There's probably a whole bunch of other important stuff. And again, a lot of it I think is technical. I think there's a lot of really important technical work to do here just in terms of scaffolding to have AI agents identify themselves, and the scaffolding doesn't have to attach to the weights. It could attach to something else, like something on a blockchain, something in the Microsoft Azure cloud. I don't know. This is kind of outside my realm of expertise.

## Social institutions for AI safety <a name="social-insts-for-ai-safety"></a>

**Daniel Filan** (02:03:28):
I think I'm about out of questions about the main paper itself. But before you move on, is there anything that you wish that I had asked, or you wish that I had brought up an opportunity for you to talk about, about that paper itself, that we haven't yet?

**Peter Salib** (02:03:43):
Yeah, interesting. That's a great question. I think it was probably the longest conversation I've ever had about the paper, or at least with somebody who wasn't a co-author or a good friend or something. So it's been very thorough. Yeah, I don't know if there's anything in particular.

(02:04:12):
I guess just one thing I would just emphasize as a very high-level takeaway from the paper is even if you think a lot or all of the specific arguments are wrong which... I think you should think they're right because I think they're pretty good, but my view is that it's a big oversight in AI safety and in AI alignment circles that almost all of the energy tends to be on doing alignment via technical alignment and control. And those seem really important to me. I'm not saying we shouldn't be working on that.

(02:05:18):
But even if you think all the specific stuff in the paper is wrong, I think one claim I just stand behind very strongly is: what agentic goal-seeking things, including AIs, will do depends a great deal, not only on what they want, but on what the social and especially, in my view, legal environment incentivizes them to do. And so I think this is just an area where there's a lot of really fruitful work to be done in thinking about how we should shape legal and broader social institutions to help foster non-conflict between humans and capable AIs as they emerge.

**Daniel Filan** (02:06:11):
So my guess is probably part of the reason why this is underrated in the AI safety space is, especially if you think about the start of the field, thinking about superintelligence: a lot of thought is being placed on this kind of limit of very smart things, and at that limit, it's just kind of unclear that humans are the relevant society or that the humans are making the relevant institutions. But I do think in this intermediate regime - which I do think in the last couple of years, the AI safety community has gotten more interested in - I think that yeah, there's definitely a lot of this thinking that's worth doing.

**Peter Salib** (02:06:50):
Yeah, and I basically agree with that. I think if you had an old school ["foom"](https://www.lesswrong.com/w/ai-takeoff) view of what would happen, then yeah, probably societal adaptation doesn't matter very much, but the more you start to take seriously the idea that timelines will be long-ish and/or smooth-ish and that there is some... I think it's possible to update too much on this idea, but I think there's something to it: that there is some other set of processes that will matter for AI systems as they achieve their goals, where they integrate into the world, even if they're very, very smart, that there'll just be a bunch of things they have to work out as they just start trying to do stuff, and that those regimes are real and could last a meaningful amount of time. [If you believe that] I think all this stuff becomes more important. So as that's become a set of worlds that we're more interested in, I think law and institutions more generally should be a growing part of the research.

## Outer misalignment and trading with AIs <a name="outer-misalignment-and-trading"></a>

**Daniel Filan** (02:08:20):
Sure. Actually I want to talk about another thing I just thought about that I think is going to be an objection from AI safety people. Sorry, this just totally breaks the overall flow of the conversation, but hopefully it is worth it. A lot of time the things that AI safety people are worried about is like: okay, we're trying to train an AI to do a specific thing, and we're worried about it not doing that specific thing. So why would it not do that specific thing? Well, one reason would be if the specific thing we want it to do is kind of hard to check. Suppose I want an AI to make me some sort of... Well, I believe in your paper, you use this example of vaccines, right?

(02:09:19):
Suppose I'm just directly training an AI to do that, and it's really smart, it's super duper smart, and the thing I really want is, "Hey, please train me a vaccine that works in the short run, and it doesn't have any super bad qualities in the long run that kills me five years later and then gets power to everyone else". I think there's a lot of concern about this failure of oversight, this failure to check whether they actually did what you wanted.

(02:09:56):
And this is very relevant in the training setup where if I want to train an AI to do stuff, then I've got to be able to check that it's doing the stuff and that's how I give its reward signal. But it seems like it's also relevant in the contracting case, right? Because if I contract with an AI to do a thing for me, and I can't check whether it's actually succeeded at doing the thing for me, it has a thing that appears to do the thing, but there are ways of tricking me, of making a thing that appears to work but doesn't really work, then it's very hard to do this positive-sum contracting. And so one might think, "Okay, but in worlds where we have the types of misalignment that AI people are worried about, we just can't do any of this good contracting". So yeah, is this idea totally doomed?

**Peter Salib** (02:10:41):
As I think about the classic misalignment worries... I mean there's a whole bunch of related concerns, and I think one of them is inability to check whether the AI has actually done the thing that you wanted it to. But a related but slightly distinct version of that concern is worry that the AI has done the thing you want it to do, but it's misgeneralized, right? In every case in training it does the thing you asked, your objective function is well specified. It's just that the AI has internalized some misgeneralized version, and then the classic worry is then you let it out of the box and it does the weird thing and that kills everybody. It makes all the paperclips.

(02:11:42):
And the response from the paper, the "AI rights for human safety" idea, is that it's actually not necessarily catastrophic for there to be very powerful things in the world that are trying to do stuff that you don't want. In fact, that's the world we live in. At every level of organization, from individual humans who under conditions of scarcity prefer that they eat over anybody else, to corporations competing with each other, to nation states. And there are tools for managing this kind of misalignment. And they're not perfect, right? Things go wrong sometimes, but we have ways of doing this. And law is an important institution and contracts that let you be in markets is a really important institution within those sets of important institutions.

(02:12:40):
And hey, contract law, it turns out, has some tools for dealing with exactly these kinds of problems. So one interesting thing to notice about contract is to contract with somebody, it's not important at all that they have an actual preference to produce the thing you contracted for. It's kind of neither here nor there. The reason they do it is because you're going to pay them. And if they don't do it, well, then there will be some downstream costs in terms of they'll get sued, they'll have to pay some litigation fees, they'll transfer the damages to you anyway, they'll be worse off actually than they would have been if they had conformed. And are there pressures to try to avoid this? Well, yeah, of course.

(02:13:26):
And happily, law has a bunch of doctrines that help you deal with this. Again, they're not perfect, but for example, when someone wrongs you legally, and you have a claim you can bring against them, we often have a bunch of rules about the timing of that claim. If the AI makes a vaccine for you that turns out to in five years kill you or something like that, if your contract said, "Make it not kill me," then that's breach, and you have a contract claim, and we have these things called statutes of limitations that sometimes run out, but they usually start running from the moment at which the injury either was discovered or could reasonably discovered. Because again, we're trying to balance these two things, the ability of the person to actually incentivize their counterparty to act well, but also finality for the person who could be sued, right? You don't want someone to know they have a claim and then hold onto it indefinitely and then just drop it on you at a strategically... maybe a time when you're already resource constrained so you have more settlement pressure, right? [inaudible 02:14:49] to.

(02:14:51):
And so look, will the rules we have for humans as people try to game their contracts work perfectly for AIs? Well, no, they don't work perfectly for humans either, but they work reasonably well. On average, they give people incentives to want to stay in the iterated bargaining game. And then of course, we don't have to just port the rules we have to humans to AIs. We could have different rules and we should think really hard about what those rules should be.

## Why statutes of limitations should exist <a name="why-statutes-of-lims"></a>

**Daniel Filan** (02:15:27):
So actually just picking up on something you said there: sorry, this is not really related to the overall flow of conversation, but about the statutes of limitations, I think it's always been kind of unclear to me what the normative rationale for statutes of limitations should be. And the one thing that I thought of is, okay, legal battles are kind of costly. If I sue you in a case where all the evidence is degraded because the underlying thing I'm suing you about happened decades ago, then perhaps one reason to have statutes of limitations is if the courts can say, "Look, if you're suing about a thing that happened 50 years ago, there's definitely not going to be enough evidence, and therefore you're almost definitely going to lose, so we're just going to bar your suit at the outset". I don't know, I just sort of imagined this, is this not right? Or is this one of the rationales that-

**Peter Salib** (02:16:24):
That's totally a standard thing people say.

**Daniel Filan** (02:16:25):
Okay.

**Peter Salib** (02:16:26):
I think if you really want to make the argument work, you have to say something more like: not that it's been so long that you're definitely going to lose, because in that case, there are good reasons for you not to bring the lawsuit at all, right? There's actually no bar on you bringing a lawsuit over something that happened yesterday that you're definitely going to lose, right? And we think the main thing that deters that is it costs you money, and you don't expect to get anything from it. I think what you have to think about the evidence is that, as it degrades it increases error, or something like that. Where you're just less sure that you're getting the right result in that case, and that's giving you lower-quality legal decisions, or something like that. That seems plausible to me, I'm not totally sure it's true.

(02:17:18):
But the other standard justification is what we call "repose", which is just, it's not nice to have a potential lawsuit hanging over your head. And it's not just that it's not nice. Maybe you want to get a loan for a house, or something like that, and maybe you have high certainty that you'll win, and the plaintiff has high certainty that they'll win, and your bank has no idea either way, and it's best to just get the whole thing over with. So the fact that either the possibility of the lawsuit in the interim could have bad effects, or that the plaintiff could strategically time the lawsuit, right? They find out you've applied for a loan on your house, and now they file the lawsuit, and now your home purchase is not going to close unless... So we want to at least limit the ability to impose those kinds of costs. So we say, "If you know have a claim, just hurry up and bring it". And that seems like a reasonably good rule to limit those problems.

## Starting AI x-risk research in legal academia <a name="starting-aixr-in-academia"></a>

**Daniel Filan** (02:18:39):
Okay. Well, people interested in this, I'm sure there are many other sources they can point to. I'd now like to move a little bit on: so I think probably most of my listeners to this are software engineers, or people who work in machine learning. And you are a law professor, I guess, which is quite a different background. So first of all, how did you come across AI x-risk as a worry that maybe you could do something about?

**Peter Salib** (02:19:11):
It's a good question. So I've been interested in the intersection of law and AI basically since I started being a legal academic, which is not that long ago. And I would say that the stuff I was writing earlier in my academic career was, at a high level, of the form, "Hey, there's this problem in law, it's maybe an information problem, it's maybe a kind of precision problem, and it seems like all this important stuff is happening in machine learning, and we could use those techniques, use those insights to administer law better". A number of my papers you could characterize that way. But that meant, among other things, I was following machine learning progress more closely than most law professors, and maybe even more so was just more convinced than most law professors that the progress was impressive. And I think that that just meant that I was paying more attention as LLMs started to get good. And when you're trying to get that kind of information as it's coming out, I think you end up in Twitter spaces that are also interested in things like AI x-risk.

(02:20:45):
I started reading some of that stuff. I read [Superintelligence](https://en.wikipedia.org/wiki/Superintelligence:_Paths,_Dangers,_Strategies), I read some of [Eliezer] [Yudkowsky](https://en.wikipedia.org/wiki/Eliezer_Yudkowsky)'s stuff, the stuff people read when they first learn about this. And then also [Simon](https://www.simondgoldstein.com/) [Goldstein], my co-author on this paper, was sort of interested at the same time. And between us talking about all that stuff, and reading it, and me being quite convinced that AI progress was accelerating, and that there was in principle no ceiling, I just became very convinced that this was a problem to work on. And so I started to try and think about what law had to contribute, if anything.

**Daniel Filan** (02:21:28):
Sure. I'm wondering how that shift of emphasis has gone. Was it difficult? Do your colleagues think you're crazy? Or how's that all happening?

**Peter Salib** (02:21:44):
I think they think I'm a little crazy. Well, let me put it this way. I think every month they think I'm a little less crazy.

**Daniel Filan** (02:21:53):
That's the hope. It's the dream, rather.

**Peter Salib** (02:21:57):
Yeah. So I had the idea for this paper, I don't know, probably around when I was going to go on the academic job market-ish, or maybe it was really early after I had done it. But that was long enough ago that if you wanted to be a law professor who wrote about AI, you couldn't go on the market saying, "I'm a law and AI scholar". That was not a genre of inquiry that existed. You sort of had to say, "Well, I'm a civil procedure scholar, and I have this paper about machine learning and class actions", or something like that.

(02:22:41):
But of course the world has changed a lot since then. And so every six months or so, I feel a noticeable shift in the [Overton window](https://en.wikipedia.org/wiki/Overton_window). So I mostly didn't work on this idea - there's a [YouTube video](https://www.youtube.com/watch?v=hnQrCgKJIKM) of me presenting at the [Center for AI Safety](https://safe.ai/) a couple of years ago, but I kind of sat on it. When I would talk to other legal academics about AI x-risk, they would be pretty skeptical, and they'd say, "Well, isn't this kind of like sci-fi?" and "why would AIs hate us?" and stuff like that.

(02:23:20):
But recently, as capabilities have progressed, and as I've mostly completed what I think is an otherwise pretty normal-looking tenure package, I've just decided it's time to go all in on thinking about law, and particularly law and AGI. And I will say, I assumed that we would write this paper and it would not get picked up, or it would get picked up in just a random draw from the distribution of law reviews. I feel very pleasantly surprised that [the University of Virginia Law Review](https://virginialawreview.org/), which is a very, very good law review, thought it was good and worth publishing. So for me, that's a big positive update on law students as being interested in these kinds of questions.

## How law reviews and AI conferences work <a name="how-law-revs-and-ai-confs-work"></a>

**Daniel Filan** (02:24:18):
Actually, this gets to a question I have about legal publishing, which I'm very unfamiliar with. When you say the students, are the students the ones who run the law review?

**Peter Salib** (02:24:29):
They do.

**Daniel Filan** (02:24:30):
And law reviews are... do I understand correctly that those are the main journals for legal writing being published?

**Peter Salib** (02:24:38):
They are.

**Daniel Filan** (02:24:39):
Isn't that-

**Peter Salib** (02:24:39):
I can sense your bewilderment.

**Daniel Filan** (02:24:42):
That seems kind of crazy. I normally - in machine learning, we have conferences and there's someone who heads the conference, and they're normally a really fancy experienced person, and then they have area chairs who are also fancy experienced people. And you have these graduate students in who do the reviewing of the papers, and maybe they run some workshops. But you let the students run the whole show? What if they mess it up?

**Peter Salib** (02:25:07):
So there's this huge debate within legal academia, and then between legal academics and others, about whether the law review system is a good system. It's certainly a weird system compared to other academic disciplines. But yes, the thing that's going on is the most prestigious outlets for legal scholarship are [the Harvard Law Review](https://en.wikipedia.org/wiki/Harvard_Law_Review) and [the Yale Law Journal](https://en.wikipedia.org/wiki/The_Yale_Law_Journal), and those are student-run publications where students decide what to publish, and students handle the editing process. Now, the arguments against this are the ones that you gave. What do law students know? They're not academics, they don't know the literature in the way academics do. They have less sense of what's original, what's persuasive, and so on and so on. I think those are all valid critiques. On the other hand, I do think there are deep pathologies of the peer review system. I think peer review... and actually your description of computer science is an interesting hybrid. It sounds like there are these chairs that have some gatekeeping power, but the reviews are maybe done by graduate students, who have-

**Daniel Filan** (02:26:26):
Yeah, well, it's sort of this unusual situation. So in computer science generally, it's a conference-based life-

**Peter Salib** (02:26:34):
Which is also weird by the way, I'm sure you know that. You put everything on [arXiv](https://en.wikipedia.org/wiki/ArXiv) and that's what everyone cites, and then you figure out later whether it was any good by whether it gets accepted to a conference.

**Daniel Filan** (02:26:43):
Yeah. Well, the key thing about it being conference also is just that... because even in journal-based systems, you can put things on arXiv as well. So there's some willingness to cite arXiv things. The nice thing about it being conference-based is there's a timeline for people to review your stuff, because at some point they're actually going to physically have the conference where everyone goes to a place, and you have to have it ready by then.

(02:27:02):
So you have reviewers who I think are mostly graduate students, just because the field is growing... Also, the field is not in equilibrium in AI, it's growing year on year. And so at any given point, most of the people are early in their PhD, and so that's who you have to get most of your reviewers from. Now there are also... you have area chairs and program committees, and so the reviewers review the papers in this double-blind fashion, and then a higher-up in the conference can say, "Oh, actually these reviewers are wrong, and I think I'm actually going to reject this paper", or "This reviewer's argument was better than this reviewer's argument, so we're going to accept the paper".

(02:27:45):
Also, a somewhat interesting aspect of the system, is that a lot of these happen on this website called [OpenReview](https://openreview.net/). It's sort of like a forum. And there are some versions of this where literally everyone could see your anonymous paper, and anyone can write comments, as well as the official peer reviewers. But I think that's not normally turned on. But you get to just have a comment thread with the reviewers, and you can say, "No, you said this, but actually this". But yeah, a lot of the reviews are done by graduate students, but people who are more senior than graduate students - or maybe final year graduate students or something - but generally people who are more senior are making the final calls.

**Peter Salib** (02:28:32):
And so look: what's wrong with the peer review system? I mean, look, I've never had to publish in it, but [Simon](https://www.simondgoldstein.com/) [Goldstein], my co-author is a philosopher, and he'll say things like: well, look, there's this intense pressure towards very narrow specialization in topic, because basically you're writing to a small handful of people who will be selected as your reviewer given your topic. And so you're quite pressured to specialize on just the questions they think are interesting, using the approach that they think is most interesting. That tends to fragment the field, that tends to have people work on narrow niche technical topics, instead of topics that have broader scope and are more synthetic of different ideas. There can be a lot of gatekeeping, so if you have an idea that is not well accepted, or you have a good argument for an idea that is out of fashion, it can be very hard to publish, because all the available reviewers will have priors against your idea.

(02:29:47):
And then the labor is really constrained. The number of reviewers is really small. It's just tenure track law professors or senior graduate students. And so as compared with that, the law review model has these people who are less knowledgeable, but they're super smart. I mean, students at [Yale Law School](https://en.wikipedia.org/wiki/Yale_Law_School) are very smart, I assure you. And so they don't know as much, but they're very good thinkers in general. There's a lot of them - it's kind of a prestigious thing to do as a law student, to be an editor of a law review. So there'll be several dozen really smart people reading and discussing this paper.

(02:30:27):
There are more law reviews, so the supply of journals is less constrained. So instead of waiting years and years to try and get your paper into one of the top five journals, you can be very excited to get your paper into one of 20 or so journals, or 50 or so journals, depending on what you work on.

(02:30:49):
And then from there, it's a little bit more of a free market system. Maybe the quality signal from journal placement is a little bit less strong, but maybe the output is higher. It's hard to know. Whatever, it's confirmation bias or something. But I'm sort of weakly in favor of our wacky system.

**Daniel Filan** (02:31:09):
Hang on: another question I have is: so I had gotten the impression that people in law school were really busy, that it was a lot of work.

**Peter Salib** (02:31:19):
It is.

**Daniel Filan** (02:31:19):
And if you're a student, and you're also helping run this law review, presumably you have to... You're saying lots of people are reading these articles. They're also not short. In machine learning, you get 12 pages at the most. If it's a harsh conference, you get eight. Or sorry, I think eight or nine is the normal one. Your paper is 88 pages. Now a lot of that is footnotes - so potential readers, don't be too put off. But how are they not too busy to do this?

**Peter Salib** (02:31:56):
They're such hard workers, they're just really hard workers. And actually, when you think about the ecosystem that produces this, a lot of these are people who go to law school, and they want to get jobs at big law firms. They want to work for [Skadden](https://en.wikipedia.org/wiki/Skadden,_Arps,_Slate,_Meagher_%26_Flom), or [Wachtell](https://en.wikipedia.org/wiki/Wachtell,_Lipton,_Rosen_%26_Katz), or one of these big law firms. And what are the criteria for being successful as an associate at Skadden? Well, one, you have to be smart, you have to be able to do the work. But two, these are firms that bill out their labor in increments of tenths of an hour, like six minutes, and their profits... Beyond a certain margin of hours, it's basically all pure profit. So big law attorneys work really hard, they work long hours. And so, wow, you're on the Harvard Law Review, and that means you're busy all the time, and you're churning through these 300-footnote articles, and finding all the sources, and checking the [pincites](https://en.wiktionary.org/wiki/pincite) to see if they're accurate. What could be a better signal of your fitness for working in a big law firm?

**Daniel Filan** (02:33:15):
I can see how it would be a signal of working hard, and maybe if they have to check the footnotes and stuff, then maybe it's a signal of that as well. I would imagine there would be things that would more simulate working at a law firm than reading law review articles. If I take your article, my understanding is that the things law firms do is, it's companies suing companies about potentially breaching contracts for IPs or whatever. And if I'm right about that, that sounds really different than evaluating whether your paper is good. Am I wrong about that, or is there just not another thing that these students could do to better prove their ability to work hard at these law firms?

**Peter Salib** (02:34:04):
It may not be that there's literally no other thing they could do, but I think maybe you're underrating... One thing to say is: our article is a little weird as a law review article. We have game theory tables in it, and that's not normal, and there's some [IR](https://en.wikipedia.org/wiki/International_relations) stuff in there. My other papers have a lot of economics in them, and those range from being common, but not standard, to very uncommon in law reviews.

(02:34:34):
But in general, it actually might be really valuable as a lawyer to have the skill of becoming an 85th-percentile expert in an arbitrary topic. So it is true that what big firms do is companies suing other companies over IP. But for example, when I was in practice, I did a fair amount of IP work, and that ranged from companies suing companies over a patent on a radio device that helped to track the location of semis, to companies suing companies over infringement on a patent on biologics - drugs. Those are very different. And the question of infringement depends on issues like, was this invention obvious in light of the prior art?

(02:35:33):
So to be a good patent attorney, your job isn't to understand the science at the level of the inventors - we hire experts for that. But you have to be able to understand it well enough to write for a generalist judge, in a way that is convincing as to what's going on and as to what the law is. So being able to wrap your head around an arbitrary area of inquiry and understand it pretty well - not the best in the world, nowhere near the best in the world, but pretty well - is maybe a really valuable skill.

**Daniel Filan** (02:36:08):
I guess, so object-level-domain-wise, I can see how that would be true. I mean, surely attorneys must specialize at the level of the aspect of law that they deal with, right?

**Peter Salib** (02:36:19):
To a significant degree, although maybe less than you'd expect.

**Daniel Filan** (02:36:23):
Okay. Yeah, I guess I would imagine that-

**Peter Salib** (02:36:28):
Even if you're just a patent attorney: the best of patent attorneys in the world are not so specialized that they're just doing biologics. They're experts in patent law, and their clients are doing everything from drugs, to smartphones, to automobiles, to anything you can patent.

**Daniel Filan** (02:36:51):
Sure. Sorry, maybe I'm getting too hung up on this. So if I go to [virginialawreview.org](https://virginialawreview.org/), I click the button that says online. I think that's the papers they have. I'm seeing-

**Peter Salib** (02:37:05):
That's probably their companion. So a lot of law reviews publish long things in print, and then they have an online companion, where they publish shorter things.

**Daniel Filan** (02:37:13):
Oh, okay. Well, okay, so I'll go to their print thing.

**Peter Salib** (02:37:21):
Yeah, if you click articles, that's the long stuff.

**Daniel Filan** (02:37:24):
All right. So there's one thing that says ["Interpretive lawmaking"](https://virginialawreview.org/articles/interpretive-lawmaking/), which is about, I guess, I don't know, interpreting and making law. One that's called ["Shamed"](https://virginialawreview.org/articles/shamed/) - that's about sexual assault law, I think. One that's called ["Partisan emergencies"](https://virginialawreview.org/articles/partisan-emergencies/) that has to do with emergency powers. It seems like these are a lot of different areas. This just seems like it's so general, that I don't know, I'm still... Maybe there's not much to say here, but I'm still confounded by this.

**Peter Salib** (02:37:51):
So I mean, the range of stuff that you see as a law review editor is probably wider than the range of stuff you would see in any legal practice.

**Daniel Filan** (02:38:07):
Do the reviewers specialize? In machine learning, if you sign up as a reviewer, you say "these are my specialties". In law reviews, do you say, "I'm only going to review things about administrative law?" Or...

**Peter Salib** (02:38:19):
Not formally, but again, there's many more. So different law reviews do it different ways. But the Harvard Law Review has, again... I don't know the numbers, but it might be 60 members, or something like that. And to some degree there's different stages of review, but to some degree, what happens at the end is they all decide together in the aggregate. So to the extent to which there's a game theory expert on the Harvard Law Review who can then give their informed opinion as to how everyone else should vote, that can kind of bubble to the surface in a way that-

**Daniel Filan** (02:39:02):
Wait, wait, all 60 people meet, and all 60 people vote on every-

**Peter Salib** (02:39:06):
On the Harvard Law Review, that is the mechanism, yes. I believe. There's probably some Harvard Law Review editor listening who's like, it's not exactly that, but I think yeah, I think it's pretty close to correct.

**Daniel Filan** (02:39:18):
Isn't that crazy? Shouldn't you have one person who decides per area or something, and have some hierarchical thing?

**Peter Salib** (02:39:28):
Well, look, it just depends on whether you think... It's not exactly wisdom of crowds, because there's not independent blind voting, but it just depends on whether you think you get more juice out of debate or out of expertise. And probably both of those are valuable, and they both have pathologies.

**Daniel Filan** (02:39:53):
It just seems so expensive.

**Peter Salib** (02:39:56):
But again, in economic terms, it's expensive, there's a lot of labor being dumped into this. In nominal terms it's free, because the students do it for free.

**Daniel Filan** (02:40:09):
Sure. But as a student, you have your time, you might prefer to spend... I don't know, maybe don't prefer to spend it on leisure, because you want to prove that you're a really hard worker.

**Peter Salib** (02:40:21):
I mean, you're in law school for a reason. You're collecting a bunch of signals that you think are going to be valuable in the future. We've arranged the world in such a way that being on the law review is one of the ones that's unusually valuable. And look, as a veteran of a law review, it was a lot of work, but it was fun. I think in most fields, being a journal reviewer is total drudgery, because you get this manuscript, and then you gotta write a long thing about it, and it kind of goes into this void. And there's going to be some other jerk who just dumps on the piece, even though you thought it was really good, and it gets rejected and you feel like your effort wasn't worth it. But on a law review, you're having an academic symposium every day. There's a draft that's in, you're going to discuss it. You're with your friends, they're also smart, they're interested in it, you argue about whether it's any good. For a certain kind of person that's a fun experience.

**Daniel Filan** (02:41:23):
And I guess the argument is probably helpful if you're a lawyer. Yeah, I guess to the extent that the system is partially driven by students having fun running a law review, it makes more sense to me how you could end up with this. Yeah. Okay, maybe not all of our listeners are as interested in how law reviews work as I am. So perhaps-

**Peter Salib** (02:41:47):
You can edit out as much of that as you decide.

## More on Peter moving to AI x-risk research <a name="more-on-peter-moving-aixr"></a>

**Daniel Filan** (02:41:49):
No, no. It's all staying in, we'll provide timestamps, they can skip this bit of the conversation if they want. But getting back to where I was branching off from: so for a while you were doing law and AI, but how AI might impact various areas of law with some previous things like ["AI Will Not Want to Self-Improve"](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4445706), ["AI Outputs Are Not Protected Speech"](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4687558). I guess that's more in the weeds.

**Peter Salib** (02:42:18):
Yeah. By the way, I think of those as post my turn to AI risk. My older stuff, you have a paper about using machine learning to do a certain kind of jury simulation, that allows you to certify certain class actions that you couldn't otherwise. Another one about whether boring regression models of different kinds of impacts in racial impacts and hiring, would be a sufficient legal basis to do calibrated affirmative action policies. So that's the stuff I'm talking about when I was saying [I was] thinking about how machine learning and big data type stuff help us make the law work better. And yeah, at some point I start thinking about these other things.

**Daniel Filan** (02:43:04):
So when you pivot, do you just decide you want to work on a thing and work on it, or do you find yourself needing help, or...? Basically I'm curious how well the pivot worked for you?

**Peter Salib** (02:43:21):
As compared with other disciplines, writing for law reviews tends to be more solitary. So the number of authors on a paper is between one and three at the high end, or something like that. But one is by far the most common, and so in that sense it doesn't require a lot of help. Although I will say that for me at least, one of the things that I found really valuable in making the pivot was getting connected with the very small, but hopefully growing... We're actually trying to help grow it with this organization I help run called [the Center for Law and AI Risk](https://clair-ai.org/), but the small community of law people who are interested in this stuff. For example, [Christoph Winter](https://www.christophwinter.net/) is the director of [LawAI](https://law-ai.org/), which is a Harvard-affiliated group that had been thinking more broadly about law and x-risk, but around the time I started working on this was pivoting to be much more law and AI-focused. I started doing a little bit of advising work for the [Center for AI Safety](https://safe.ai/). And as part of that, I helped them organize a summit, I guess it was two or three summers ago now, for law professors who were interested in existential AI risk. And so from there, I met people like [Yonathan Arbel](https://law.ua.edu/faculty_staff/yonathan-arbel/) and [Kevin Frazier](https://www.stu.edu/law/faculty-staff/faculty/kevin-frazier/) who help run CLAIR, the Center for Law and AI Risk with me, and a handful of other really great people. And so having people to bounce ideas off of has been super useful, but there's not a lot of formal infrastructure yet. And again, that's one of the things we're hoping to build with this center, so that more people can transition to do this work easily.

## Reception of the paper <a name="reception"></a>

**Daniel Filan** (02:45:37):
So I guess I'm also curious about the reception. So you mentioned that your colleagues are thinking that you're a little bit less crazy than they used to, and this got accepted to Virginia Law Review, which sounds like it is one of the better ones. More broadly, I'm curious: have you found much uptake, or much engagement by the legal community with your writing? Or more broadly, how's the reception?

**Peter Salib** (02:46:01):
Yeah, I think the idea that AI could become very powerful has been entering the Overton window in law, especially over the past, say, nine months or something. When I started writing the draft of this paper, it was summer of last year. And that was the point at which I thought, this paper is kind of wacky, it's probably outside the Overton window, it might not even get published, but I think it's important, and [Simon](https://www.simondgoldstein.com/) [Goldstein] and I should write it anyway.

(02:46:39):
At that time, that was informed by places where I had gone and presented prior work, like [the AI self-improvement paper](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4445706), where I spent most of my time, when I would present that paper to law faculties, just trying to convince them to take seriously the idea that we should model AIs of the near future as having goals and being able to plan and act rationally in pursuit of those goals, and being capable of doing stuff that could be dangerous as they pursue those plans. And they were just not on board even with that premise. And that's like the foundation, that's the pre-reading for the paper, that's before you even get to any of the arguments in that paper. And so I just found myself doing a lot more conversation about that, at that time.

(02:47:38):
Then Simon and I wrote this paper, the [AI rights paper](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4913167) we've been talking about, expecting to have the same reception, just people kind of getting off the bus right at the beginning. And it was basically the opposite. I got asked to go present the paper to the faculty workshop at [Fordham Law School](https://en.wikipedia.org/wiki/Fordham_University_School_of_Law) in fall, and immediately they wanted to just dive into the substance of whether the payoffs in the game theory matrix were right, or whether there's other versions of the world in which they could look different, and questions about some of the stuff we've talked about, individuating agents for purposes of holding them to their contracts. And it was just such a big shift. I don't know exactly what explained it except to just say that every few months, Anthropic or OpenAI or somebody releases an ever more capable agent, and more people use them. And lawyers, despite being quite small-c conservative, are noticing.

**Daniel Filan** (02:48:48):
Sorry, I'm not as super familiar with the culture of legal academia. Could it just be that Fordham University is unusually AI-pilled?

**Peter Salib** (02:48:56):
It could be. Data points include not just that, but the editors at the Virginia Law Review being into it. I've given the paper in a couple of other places too. God, what's the whole list? I was going to say, right after Fordham, I was at Oxford giving the paper, but you're going to say, "Oh, Oxford's very AI-pilled". Although I will say, I gave the paper to the Oxford Faculty of Business Law, which had basically no interaction with FHI or whatever.

**Daniel Filan** (02:49:32):
[Rest in peace](https://www.futureofhumanityinstitute.org/).

**Peter Salib** (02:49:33):
Yeah, dearly departed. There's others too, that my brain is not being able to recall. But I would say just in general, I've talked about this paper in a number of legal academic settings, and people have been much more interested in talking about the merits of the idea conditional on the assumptions that I give, rather than challenging the assumptions.

**Daniel Filan** (02:50:03):
Okay. So I guess you're sort of moving into the existential risk community. How have you found the reception of the paper among the AI x-risk world?

**Peter Salib** (02:50:18):
Yeah. I think it's been reasonably good. I gave an earlier version of the paper at [the Global Priorities Institute](https://globalprioritiesinstitute.org/)'s (I think) twice-annual conference. I think I was probably the only lawyer there. I think it's mostly philosophers and some economists. But yeah, the main AI risk people were there and gave good feedback, but I think were pretty open to the idea. I guess some of the guys who run [Redwood Research](https://www.redwoodresearch.org/) have in the past [wondered](https://axrp.net/episode/2024/04/11/episode-27-ai-control-buck-shlegeris-ryan-greenblatt.html#is-control-evil), hey, should we just pay AIs to do stuff for us?And so they were interested in the more technical analysis we did there.

(02:51:16):
Yeah, I would say overall, my sense is the reaction is pretty good. Where people are skeptical, I think it's mostly people who have very short timelines and think that superintelligence will be here pretty quickly and think that basically some monkeying around with the law is not going to accomplish anything.

**Daniel Filan** (02:51:44):
Yeah. And I guess this actually relates to another question I have. I'm curious: are the x-risk people and the law academics, are they picking on the same things in the paper. Or do some people focus on some things and other people focus on other things?

**Peter Salib** (02:52:02):
So among the legal academics who are interested in x-risk, is there a diversity of views about what's good and bad-

**Daniel Filan** (02:52:11):
Oh, I more mean do the x-risk people focus on different things than the legal academics, the ones who are not in the intersection?

**Peter Salib** (02:52:20):
Yeah, there's a difference. X-risk people tend to be a lot more interested in questions like "what will bind AI labor at the margin?" or something like that. Already in the background, they're thinking, "oh, how much inference compute infrastructure do we need to build for AGI? And how many power plants?" That's kind of in the background of their minds already. And the law people, yeah, they have more law-y questions. So they immediately hit on questions like, well, isn't stable identity really important to make property ownership and contract have the good effects you want? And stuff like that.

## What publishing in law reviews does <a name="what-publishing-in-law-revs-does"></a>

**Daniel Filan** (02:53:24):
My next question is: so for people who publish in AI, I sort of understand what the theory of change is supposed to be. Roughly people will publish an AI paper indicating some technical fact, and the hope is that other AI researchers learn this fact and then eventually that when people are building AI, they're clued in enough with AI research that they then incorporate this fact into how they build AIs. For law review articles, law professors read them, I assume. Does this somehow feed into what the law actually is, and if so, how?

**Peter Salib** (02:54:08):
Yeah, so there's three things. So to a first approximation, no one reads law review articles, not even law professors.

**Daniel Filan** (02:54:24):
Sad.

**Peter Salib** (02:54:24):
And I think actually the way of thinking about them is a little bit like how you would think about popular press nonfiction books, which is there's some mix of reference guide... Somebody has an argument and do you need to read the book...? You can think of the new [Ezra Klein](https://en.wikipedia.org/wiki/Ezra_Klein) and [Derek Thompson](https://en.wikipedia.org/wiki/Derek_Thompson_(journalist)) book, [the Abundance book](https://en.wikipedia.org/wiki/Abundance_(Klein_and_Thompson_book)). Do you need to read every page to have a pretty strong sense of what the argument is and the extent to which you disagree with it? Absolutely not. But it's nice to have the book with the chapters numbered so you can read the introduction, understand what you think the real weakness is, and then go read the chapter that's about that and then figure out if it's a good response. And so as that kind of reference material, I think they are somewhat more widely read, including by courts.

(02:55:23):
Courts will cite law review articles with some regularity, not always for the core proposition of the paper - sometimes for something that's in one of the subsections. And I will say there have been cases in history where legal academic thinking... It's hard to point at one particular law review article, but some collection of law review articles have been really influential. So one example is the turn to economics and [consumer welfare as a standard in antitrust](https://en.wikipedia.org/wiki/Consumer_welfare_standard) was very much influenced by a bunch of legal thinkers who were mostly putting their ideas out in law reviews in the '70s and '80s.

**Daniel Filan** (02:56:05):
Right. So should I imagine that there's some technical law people? And the places those technical law people live... So partly they're litigators, they just have to deal with what the laws actually are. Partly they're judges who have some leeway to interpret laws. And then partly they're something like, I don't know, agencies who are proposing how they should regulate a thing, or maybe they're like whoever writes the [model penal code](https://en.wikipedia.org/wiki/Model_Penal_Code), which also seems like a crazy situation where, as far as I can tell, a bunch of random lawyers just get to determine what criminal law is because it's more convenient to let them. But that sort of thing, is that sort of thing how I should think of the impact of law review articles?

**Peter Salib** (02:56:49):
Yeah. You've written a piece of technical work that has a mix of technical claims and then just higher-order gestalt around some set of ideas. You can think of the antitrust stuff about this. The high-order gestalt is "you should think about price. Price should be the thing". And there's very technical arguments in whatever, [Bob Bork](https://en.wikipedia.org/wiki/Robert_Bork)'s papers about how you should figure out whether there's been a price harm to... He might not be the best example. I'm not sure actually his paper's that technical. But there are people.

(02:57:25):
And those ideas kind of diffuse through this... You can think of it as elite legal apparatus, which is some combination of judges, it's policymakers who go into administrations who want agencies to do different things, and they need to be able to reach... Even if your heuristic is as high-level as "we're the Joe Biden FTC, we want the FTC to be doing more progressive stuff". Well, what progressive ideas are there out there for doing trade law? And then you pick the ones that have kind of bubbled up in terms of their popularity and credibility. And then you end up implementing some combination of the gestalt and the technical idea.

(02:58:18):
And so there's this kind of ecosystem of legal thinking. And then I do think it also spills over into politics and political discourse more generally. There's a lot of ideas right now that regular people are talking more about that have their origins in legal academia. ["Unitary executive theory"](https://en.wikipedia.org/wiki/Unitary_executive_theory) is something that normal voters have now heard of, but that's [from] some law professors writing about separation of powers in, I don't know, probably the '80s and '90s. I'm not totally sure. Yeah, so it spills over into broader political academic discourse as well.

**Daniel Filan** (02:59:13):
I guess I also want to say: it sounded like you also maybe wanted to react to this claim I made about the model penal code being a strange institution. I don't know if there's anything you want to say there.

**Peter Salib** (02:59:23):
Oh, well, so yeah, the model penal code is in some ways this almost distilled example of what I'm talking about because the model penal code is just a model, right?

**Daniel Filan** (02:59:36):
Right.

**Peter Salib** (02:59:36):
No one has to adopt it. It's just some people who are law professors who are designated as experts by I think it's maybe the criminal law section of the [ABA](https://en.wikipedia.org/wiki/American_Bar_Association) or something. There's some-

**Daniel Filan** (02:59:49):
ABA being the American Bar Association: the primary collection of lawyers.

**Peter Salib** (02:59:53):
Yeah. But they have no legal power. They're just an institution and they designate some people who they think are experts and they say, "Write a penal code. You guys know how the law ought to be. Write it down as a model". And then states can adopt it if they want. And then there will be some states who say, "Our penal code is bad. Maybe it's not even bad on the merits. Maybe it's too confusing, we don't have that many statutes, you have to know a lot of case law to know what's going to happen. We want to standardize it. We need a policy". And what do they reach for? The model penal code, not even because they think it's correct on the merits top to bottom, but because it's there, right? It's there and it's a product of the elite legal institutions that they rely on to produce policy.

**Daniel Filan** (03:00:46):
Yeah. It's interesting that they pick that and not another state's penal code. Presumably you could just pick whatever state you think you like the most and pick their penal code, right?

**Peter Salib** (03:00:57):
Yeah. So there's a fair amount of that that goes on too. States borrow laws from one another. States borrow federal law for themselves. So there's a selection, there's a menu of things you can choose, but one of them is "here's a tome the law professors wrote", and sometimes the tome gets adopted.

(03:01:24):
Sorry, I just thought of one more good example. So maybe you know who [Lina Khan](https://en.wikipedia.org/wiki/Lina_Khan) is. Lina Khan is the former chair of the [Federal Trade Commission](https://en.wikipedia.org/wiki/Federal_Trade_Commission). And famously during the Biden administration, which was the administration that appointed her, had an agenda for antitrust in the United States that was quite different from what came before. It was less focused myopically on whether monopolies were raising prices, had a more holistic view of when monopoly power could be bad for, say, politics, and was more skeptical of big business in general in principle than prior regimes. And why did that happen? Why was she the chair? She wrote [a student note](https://www.yalelawjournal.org/note/amazons-antitrust-paradox) in [the Yale Law Journal] that just had some of these ideas about how bigness in principle can be bad.

(03:02:26):
And it kind of caught on, and I don't know if Joe Biden thinks that particularly, but it caught on as a kind of progressive idea of what antitrust could be. And so when Joe Biden was looking around for how he can make the government more progressive, well, that was one of the packages on the menu of items you could choose. And that was the one that got chosen. And I think you can trace it directly back to a law review article.

**Daniel Filan** (03:02:50):
Fair enough. So is the hope here that somehow we write enough law review articles about the potential legal regime for AI, maybe we get a model civil AI code or something. Is that sort of the eventual theory of change here?

**Peter Salib** (03:03:09):
Yeah, I think it's something like: if you think that AGI is going to happen, whatever your timelines are, it just seems pretty plausible that at some point there will come a moment where everyone decides that we need to do something. And there will be many things that you could do. One is you say: hey, we don't know how to handle these capable agentic things that can act on their own behalf over time and we should just ban them. We should just turn them all off or whatever. Or we need to mandate control. Control could be the thing. We'll pass a federal statute that requires maximal control over AIs by the labs that make them, and we'll outlaw open source maybe. And that could be a kind of package of things that happen.

(03:04:07):
And the hope is that then there will be this other thing, which is - we think of this paper and then the research agenda that we want it to inspire as sort of small-l liberalism for AIs. So maybe there'll be this other thing which is small-l liberalism for AIs. It's kind of a package of ideas that's available to implement. And there'll be different arguments about why each of these are good. And we hope that insofar as the arguments we make are the best ones, that will have some effect in making them the package that gets picked up off the shelf.

## Which parts of legal academia focus on AI <a name="which-bits-legal-ac-focus-on-ai"></a>

**Daniel Filan** (03:04:48):
I guess I want to move on to a bit of sociology of the law profession. So first of all: you're writing this paper, it has a bunch of game theory in it. I'm aware that ["law and economics"](https://en.wikipedia.org/wiki/Law_and_economics) is a thing, kind of a school of thought. Do you come from that tradition or is it sort of a coincidence that you're writing a paper with a bunch of game theory?

**Peter Salib** (03:05:09):
No, I don't think it's much of a coincidence. So I went to [the University of Chicago](https://en.wikipedia.org/wiki/University_of_Chicago) for law school, which is in many ways the intellectual home of law and economics. I learned law from a bunch of really great people there who are very much oriented around that way of thinking. Now, it's not true that everyone who teaches at Chicago is a hardcore law and economics person. It's a great faculty with a diversity of viewpoints. But yeah, if you wanted to learn how to think about law through the lens of economics, it's not clear you could do that much better than getting a JD from Chicago, which is what I did. So not a coincidence.

(03:05:54):
Although I will say, game theory in the law is even a little bit less common as a methodology, even among people who do law and economics. There definitely are some books and papers I'd point to where game theory is the way of doing economics that gets used. But I would say it's a pretty small minority even within law and economics.

**Daniel Filan** (03:06:18):
Fair enough. So there's a thing I perceive that... Maybe I'm wrong. I'm curious how accurate my perception is. So sometimes I run across law people or I'll find myself reading a law blog or I'll listen to a podcast. And I think because of my personality and interests and stuff, it tends to be like, either they're originalists and I'm reading [the Volokh Conspiracy](https://reason.com/volokh/), or I'm listening to [this podcast called Divided Argument](https://dividedargument.com/), or it's law and economics and I'm reading [a law and economics person](https://www.tourolaw.edu/abouttourolaw/bio/399) write [a thing about how we should think about AI regulation](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4694006).

(03:06:57):
In my imagination, the originalists and the law and economics people get along together and they both teach at the University of Chicago even though it's not obvious that they all agree on... I've got to imagine that sometimes originalist interpretation of the constitution and law and economics prescriptions for how things should be must often diverge.

(03:07:20):
And it also seems to me that... it seems like these people are the most likely to be computers-y, right? For one, I'm computers-y and I run into them so probably they're computers-y as well - like Volokh Conspiracy, I think [Eugene Volokh](https://en.wikipedia.org/wiki/Eugene_Volokh) did some programming stuff before he went into law. [Will Baude](https://en.wikipedia.org/wiki/William_Baude) is playing around with LLMs on the regular.

(03:07:45):
Am I right to think that this is kind of a cluster? And if I am right, how worrying is it that there's this one kind of person and they're the only people really thinking about AI in the legal profession?

**Peter Salib** (03:08:02):
So I do think there is a cluster, but I'm not... The explanation could just be coincidence. So I think if you think back to political conservativism of the '70s through '90s, there's a Reagan fusion of economic, free-market-y-type thinking, and then a certain way of thinking about social values. And in that environment, both law and economics and originalism ended up coded conservative. And I think it was probably sociologically true also that the people who did both those things were politically conservative. And so yes, some of the most important people who did that kind of stuff clustered at a particular set of law schools because, some combination of those law schools were more willing to hire conservatives or they had hired some people who were open to these methodologies and those people hired some more people. And then to some extent, there's a kind of persistence of overlap in those two cultures. I will say I think that's breaking up to significant degree now.

**Daniel Filan** (03:09:46):
Yeah. For sure when I read Volokh Conspiracy, I guess there's [one contributor](https://reason.com/people/josh-blackman/) who seems pretty pro-Trump, but the rest of them don't seem very on board with that.

**Peter Salib** (03:09:57):
Yeah. And so there's this whole range of things. There are progressive originalists now who I think are not particularly likely to also be into law and economics. Law and economics in some ways has been... I don't know if it's right to say it's a victim of its own success, but even when I think about academic economics departments, they're basically just doing high-quality empirical social science, which doesn't have that much of a political valence anymore. And I think to some extent, law and economics is like that now too. The political valence is wide. And so there's plenty of people who have kind of a law and economics-y bent who just don't think conservatism is very interesting, and thus because they code originalism as conservative, they don't think it's very interesting. So I think to some extent that kind of cultural overlap is breaking up.

(03:11:05):
I will also say, if the main originalist you know is Will Baude, he's a teacher of mine. He's fantastic. I love Will Baude. I think he's unusual as an originalist in that (a), his originalist theory is pretty different from the [Scalia](https://en.wikipedia.org/wiki/Antonin_Scalia) [theory](https://en.wikipedia.org/wiki/Antonin_Scalia#Originalism). But then also there's [one episode](https://dividedargument.com/episodes/separation-of-powers-police) of his podcast with [Dan Epps](https://law.washu.edu/faculty-staff-directory/profile/daniel-epps/) which I assume is... Yeah, you mentioned [Divided Argument](https://dividedargument.com/). There's [one episode](https://dividedargument.com/episodes/separation-of-powers-police-5UgUzHLs) where they have [a guy](https://its.law.nyu.edu/facultyprofiles/index.cfm?fuseaction=profile.overview&personid=20083) on who is talking about [his book on international relations](https://global.oup.com/academic/product/law-for-leviathan-9780190061593?cc=us&lang=en&).

(03:11:50):
At some point, Will suggests that the main reason he's... I don't know if it's the main reason he's an originalist, but one good reason one could be an originalist is that law is basically just serving as a Schelling point in a 300 million person coordination game. And the most obvious Schelling point for constitutional law is the document we wrote down 250 years ago. And that's a really good reason to be an originalist, but it's a super different reason than you might have if you were a different kind of originalist.

**Daniel Filan** (03:12:24):
Sure. Yeah. I do get the sense that he's unusual as an originalist. I guess most originalists think that Donald Trump is actually the president, for instance, etc. I guess I don't know if he thinks that Donald Trump is or is not the president.

**Peter Salib** (03:12:42):
Wait, what's the argument that Donald Trump is not currently the president?

**Daniel Filan** (03:12:45):
Well, okay, so I'm really bad at law because I've never studied it, but Will Baude, he has [this big article](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4532751) about how [the 14th Amendment](https://en.wikipedia.org/wiki/Fourteenth_Amendment_to_the_United_States_Constitution) says that you're not allowed to have Donald Trump be president. And so I would've naively thought that if the 14th Amendment bars someone from being eligible to be the president and then there's a presidential election and that person wins, maybe that person isn't really the president. Now, to be fair, I haven't heard him make that additional step. I don't really know if-

**Peter Salib** (03:13:15):
Yeah, I'm sure that Will has a very, very well considered view about exactly this question and I'm not sure what it is, but yes, you're right. Your higher-level point where yes, most originalists don't buy the I think very originalist and probably correct ineligibility argument, whereas he does. Yeah, it'd be interesting to find out.

**Daniel Filan** (03:13:46):
So getting back a bit to things that have some chance of interesting people other than me. So I was worried that maybe there was this cluster of originalists and law and economics people and they were the only law people thinking about computer stuff and maybe that was bad because they were kind of homogeneous. And it sounds like you think (a), the originalists and the law and economics people are not so in tandem anymore, and (b), the political valence of these things is spread out such that it's not even that homogeneous anymore. Is that roughly what you're saying?

**Peter Salib** (03:14:26):
At least as compared with the past. I don't know. Probably other law professors would disagree with me. I do think there's an extent to which both these things do still code conservative, although I think there are prominent examples to the contrary that everybody knows. I also don't think it's exactly true that those are the only people thinking about law and AI or law and computer stuff. There is a whole mountain of law review articles on questions like whether AI will be biased along racial or gender or other suspect lines. And as a sociological fact, those people are basically not interested at all in existential risk. I'm not totally sure how to explain that.

(03:15:21):
I think you could tell a story about mood affiliation vis-a-vis big tech. And if your prior is kind of that in the past, social media promised to be this very powerful tool for liberating autocracies in the Middle East and helping people live more fulfilling lives with their friends and loved ones, and what actually happened was it was kind of just like a soul-sucking drag on the world, and the basic thing that it did was destroy people's privacy and extract value from poor and otherwise disadvantaged people... [Then] you'll be kind of disinclined to believe in AGI.

(03:16:23):
And I think maybe that's the sociological explanation for what's going on, is it's a cluster of people who kind of have that first set of views, and they're pretty untrusting of claims from industry about what technology is going to do.

**Daniel Filan** (03:16:39):
Yeah. And I think you have a similar sort of thing in the field of AI where you have people who are worried that AI will be biased or be deployed in ways that are bad for minoritized groups or whatever. I think the actual thing is that people who are worried about AI x-risk and these sorts of people often tend to find each other irritating. I think that's-

**Peter Salib** (03:17:05):
Yeah, there's big cultural differences.

**Daniel Filan** (03:17:09):
Yeah. Although I really hasten to say that I think this divide has... I think you're starting to see more people in the intersection now than you used to and that's great.

**Peter Salib** (03:17:20):
Yeah, which to be clear, I think is good. Yeah, I lament this division. I don't think it's good for people who care about x-risk for influential people to think we're crazy or not worth engaging with. I think the other way around too. I think that people who are really worried about AI discrimination or privacy harms from AI, whatever, could benefit a lot from engaging with people who are interested in x-risk because in many ways the concerns overlap from a technical and legal perspective.

## Following Peter's research <a name="following-peters-research"></a>

**Daniel Filan** (03:18:03):
Yeah. So on that conciliatory note, I've used a ton of your time. Thanks very much for being here. Before we wrap up, if people listen to this episode and they're interested in following your work, what should they do?

**Peter Salib** (03:18:18):
Yeah. So there's three places they could look depending on what they're most interested in. So you can find my academic work and actually most of my writing at my personal website, which is [peternsalib.com](https://www.peternsalib.com/). If you want more digestible pieces of writing than 88 pages of law review, I'm also an editor at [Lawfare](https://www.lawfaremedia.org/contributors/psalib) and you can find all of my shorter-form Lawfare works there, many of which touch on or summarize my longer academic work.

(03:19:05):
And then the last thing... I mentioned at some point during our talk that I really think that the field of law and AI safety is neglected and potentially high-impact. I think it's a really good idea to try and build a community of legal scholars who are interested in working on these questions, who want to build out all these parts of the research agenda that I gestured at today, or who think what I've talked about today is totally wrong and there's a much better way to think about law as we approach AGI. And so I co-run something called the [Center for Law and AI Risk](https://clair-ai.org/), and we are working to build that community in legal academia specifically. We think that there is a lot of potential upside to having this kind of blue sky thinking, laying the intellectual foundations for how law should govern AI to reduce existential and catastrophic risk. So please do go to [clair-ai.org](https://clair-ai.org/) and join us for a future event.

**Daniel Filan** (03:20:23):
All right. Well, Peter, thanks very much for joining me for the podcast.

**Peter Salib** (03:20:28):
Thank you for having me. It's been a real treat.

**Daniel Filan** (03:20:30):
This episode is edited by Kate Brunotts and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. This episode was recorded at [FAR.Labs](https://far.ai/programs/far-labs). Financial support for the episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future) along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. You can become a patron yourself at [patreon.com/axrpodcast](https://patreon.com/axrpodcast) or give a one-off donation at [ko-fi.com/axrpodcast](https://ko-fi.com/axrpodcast). Finally, if you have any feedback about the podcast, you can fill out a super short survey at [axrp.fyi](axrp.fyi) - just two questions.
