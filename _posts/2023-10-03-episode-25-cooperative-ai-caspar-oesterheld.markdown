---
layout: post
title: "25 - Cooperative AI with Caspar Oesterheld"
date: 2023-10-03 14:40 -0700
categories: episode
---

[YouTube link](https://youtu.be/0JkaOAzDfgE)

Imagine a world where there are many powerful AI systems, working at cross purposes. You could suppose that different governments use AIs to manage their militaries, or simply that many powerful AIs have their own wills. At any rate, it seems valuable for them to be able to cooperatively work together and minimize pointless conflict. How do we ensure that AIs behave this way - and what do we need to learn about how rational agents interact to make that more clear? In this episode, I'll be speaking with Caspar Oesterheld about some of his research on this very topic.

Topics we discuss:
 - [Cooperative AI](#cooperative-ai)
   - [... vs standard game theory](#coop-ai-v-gt)
   - [Do we need cooperative AI if we get alignment?](#why-coop-ai-if-alignment)
   - [Cooperative AI and agent foundations](#coop-ai-and-af)
 - [A Theory of Bounded Inductive Rationality](#brias)
   - [Why it matters](#why-brias-matter)
   - [How the theory works](#how-brias-work)
   - [Relationship to logical induction](#rel-to-li)
   - [How fast does it converge?](#convergence-speed)
   - [Non-myopic bounded rational inductive agents?](#without-myopia)
   - [Relationship to game theory](#rel-to-gt)
 - [Safe Pareto Improvements](#spis)
   - [What they try to solve](#why-spis)
   - [Alternative solutions](#alternatives-to-spis)
   - [How safe Pareto improvements work](#how-spis-work)
   - [Will players fight over which safe Pareto improvement to adopt?](#spi-fights)
   - [Relationship to program equilibrium](#rel-to-prog-eq)
   - [Do safe Pareto improvements break themselves?](#spis-unstable)
 - [Similarity-based Cooperation](#sbc)
   - [Are similarity-based cooperators overly cliqueish?](#sbc-cliques)
   - [Sensitivity to noise](#sensitivity-to-noise)
   - [Training neural nets to do similarity-based cooperation](#sbc-nns)
 - [FOCAL, Caspar's research lab](#focal)
 - [How the papers all relate](#how-papers-relate)
 - [Relationship to functional decision theory](#rel-to-fdt)
 - [Following Caspar's research](#following-caspar)

**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Caspar Oesterheld. Caspar is a PhD student at Carnegie Mellon where he's studying with [Vincent Conitzer](https://www.cs.cmu.edu/~conitzer/). He's also the assistant director of the [Foundations of Cooperative AI Lab](http://www.cs.cmu.edu/~focal/) or FOCAL. For links to what we're discussing, you can check the description of this episode and you can read the transcript at axrp.net. All right. So welcome to the podcast, Caspar.

**Caspar Oesterheld:**
Thanks. Happy to be on.

## Cooperative AI <a name="cooperative-ai"></a>

**Daniel Filan:**
Yeah. So my impression is that the thing tying together the various strands of research you're involved in is something roughly along the lines of cooperative AI. Is that fair to say?

**Caspar Oesterheld:**
I think that's fair to say. I do some work other than cooperative AI and cooperative AI can, I guess, mean many things to different people. But generally, I'm happy with that characterization.

**Daniel Filan:**
Okay. So I guess to the extent that cooperative AI covers most of your work, what does that mean to you?

**Caspar Oesterheld:**
So to me, the most central problem of cooperative AI is a situation where two different human parties, like two companies or governments, each builds their own AI system and then these two AI systems potentially -- while still interacting with their creators -- these two AI systems interact with each other in some kind of general-sum, as game theorists would say, mixed-motive setting, where there are opportunities for cooperation but also perhaps a potential for conflict. And cooperative AI as I view it, or the most central cooperative AI setting or question, is how to make these kinds of interactions go well.

**Daniel Filan:**
Okay. So I guess this is the AI X-risk Research Podcast, and I also at least perceive you as being part of this x-risk research community. I think if you just say that, many people think, "Okay, is this a matter of life or death, or is this just it would be nice to have a little bit more [kumbaya](https://en.wikipedia.org/wiki/Kumbaya) going on?" So how relevant to x-risk is cooperative AI?

**Caspar Oesterheld:**
Yeah, I certainly view it as relevant to x-risk. Certainly that's most of my motivation for working on this. So I suppose that there are different kinds of interactions between these different AI systems that one can think about, and I guess some of them aren't so high stakes and it's more about just having some more kumbaya. And meanwhile, other interactions might be very high stakes. Like if governments make their decisions in part by using AI systems, then the conflicts between governments could be a pretty big deal, and I don't know, could pose an existential risk.

**Daniel Filan:**
Can you flesh that out? What would an example be of governments with AIs having some sort of mixed-sum interaction where one of the plausible outcomes is doom, basically?

**Caspar Oesterheld:**
So I suppose the most straightforward example would just be to take an interaction that already exists between governments and then say, "Well, you could have this, and well, also, the decisions are made in part between AI." So I suppose there are various disputes between different countries, obviously different governments. Usually it's over territory, I suppose. And sometimes as part of these disputes, countries are considering the use of nuclear weapons or threatening to use nuclear weapons or something like that.

So I guess the current, maybe most salient scenario is that the US and Russia disagree over what should happen with Ukraine, whether it should be its own country or to what extent it should be able to make various decisions about whether to join NATO or the EU or whatever. And as part of that, Russia has brought up or Putin has brought up the possibility of using nuclear weapons. I tend to think that [in] this particular scenario it's not that likely that nuclear weapons would be used, but in the past, during the Cuban missile crisis or whatever, it seemed more plausible. And I suppose the most straightforward answer is just, well, we could have exactly these kinds of conflicts, just also with AI making some of the decisions.

**Daniel Filan:**
Okay. And so how does AI change the picture? Why aren't you just studying cooperative game theory or something?

**Caspar Oesterheld:**
Yeah, good question. Okay. AI might introduce its own new cooperation problems. So you could have these AI arms races where maybe even once one has the first human-level AI systems or something like that, there might still be a race between different countries to improve these systems as fast as possible and perhaps take some risks in terms of building underlying systems in order to have the best systems. So that would introduce some new settings, but mostly I guess what I think about is just that there are game -theoretic dynamics or game-theoretic questions that are somewhat specific to AI. So for example, how to train AI systems to learn good equilibria or something like that. It's an example that's very natural to ask in the context of building machine learning systems and a bit less of a natural question if we think of humans who already have some sense of how to play in these strategic situations.

### Cooperative AI vs standard game theory <a name="coop-ai-v-gt"></a>

**Daniel Filan:**
Okay. A related question is: when I look at cooperative AI literature, it seems like it's usually taking game theory and tweaking it or applying it in a different situation. And I think game theory... there are a bunch of problems with it that made me think that it's a little bit overrated. I have this list. So at least according to Daniel Ellsberg, apparently it wasn't particularly used by the US in order to figure out how to do nuclear war, which is relevant because that's why they invented it. It's not obviously super predictive. You often have these multiple equilibria problems where if there are multiple equilibria and game theory says you play in equilibrium, that limits the predictive power. It's computationally hard to find these, especially if your solution concept is Nash equilibrium. It's hard to figure out how to get to an equilibrium. It seems like there's important things that it doesn't model like Schelling or really salient options. I don't know. It seems very simplified and it also seems difficult to account for communication in a really nice, satisfactory way in the game theory land.

So I guess I'm wondering to what extent is cooperative AI fixing the problems I'm going to have with game theory versus inheriting them?

**Caspar Oesterheld:**
Yeah, that's a good question. I think there are a number of ways in which it tries to fix them. So for example with respect to equilibrium selection, that's a problem that I think about very explicitly. And then also in the cooperative AI setting or in the setting of building learning agents, it comes up in a more direct way. So with humans, if you have this descriptive attitude that you use game theory to predict how humans behave, you can just say, "Well, they'll play Nash equilibria." "Well, which ones?" "Well, anything could happen, it depends on what the humans decide to do." And with AI, you're forced a bit more into this prescriptive position of having to actually come up with ways to decide what to actually do. So you can't just say, "Well, you can do this. You can do that. It's up to you." At some point you'll have to say, "Okay, we want to use this learning scheme," or something like that.

And I guess there are also some other things where with some of your complaints about game theory, I would imagine that lots of people, including game theorists, would agree that these are problems, but where they just seem fundamentally quite difficult. So this problem of people going for different equilibria, like the equilibrium selection problem for example: I think part of why there isn't that much work on it or why people generally don't work on this that much is that it seems intractable. It seems very difficult to say much more than, "Well, there are different equilibria depending on blah, blah, blah. Different things might happen." It seems very difficult to go beyond that. And I think similarly with Schelling points, these natural coordination points or focal points as they're sometimes called...I'm actually quite interested in that topic, but I guess there too, I would imagine that lots of game theorists would say, "Yeah, this is... just hard to say anything about it, basically." That's why less has been said about those.

And I think this computational hardness issue, that Nash equilibria are hard to find so people won't actually play Nash equilibrium, they'll do something else that's probably a bit in the spirit of Nash equilibrium. Still they'll try to best respond to what their opponent is doing or something like that, but probably they won't exactly get it right. I think there the issue too is that describing these kinds of dynamics is just much, much harder than the simple Nash equilibrium model. And perhaps game theorists would, and perhaps I also would, think that it's still useful to think about Nash equilibrium, and maybe that's something you would disagree with or you would have a somewhat different view about.

**Daniel Filan:**
Well, I certainly think it's useful to think about. I'm certainly sympathetic to the idea "well, these are hard to study so we're studying easier problems." But I just want to check... Should I just believe this stuff or is it simplifying things in order to make any headway? I guess one question I had is: so you mentioned for a few of these things, it's just hard to study. And a natural question is: do things get easier or harder to study in the cooperative AI setting where you have a few more tools available at your disposal?

**Caspar Oesterheld:**
I think it's a mixed bag. I think some things get harder because, especially if we're thinking about future AI systems, we have less direct access to how they're thinking about things. For example, Schelling points and equilibrium selection. If I ask you, I don't know... Well, when we walk into each other, when we walk towards each other on the road and we have to decide who goes right and who goes left or whether to go to the right or to go to the left, how do we decide? There's a very simple answer that we know because we're in the US or something. And in the US, there's right-hand traffic so we're more likely to go to the right.

**Daniel Filan:**
That rule fails way more than I think I would've guessed naively. As far as I can tell, the actual solution is people randomly guess one direction and then if you collide, you switch with an ever-decreasing probability and that seems to pan out somehow.

**Caspar Oesterheld:**
Which, I think is the optimal symmetric solution is to randomize uniformly until you anti-coordinate successfully.

**Daniel Filan:**
Yeah. Although I think there's more alternation than just repeated randomizing uniformly. So, I don't know. I feel like more than average, I end up in these situations where we're on the same side and then we both switch to the same side and then we both switch to the same side... You know what I mean? I think there's a bias towards alternation rather than just randomization. This is a little bit off-topic though. Or maybe it doesn't-

**Caspar Oesterheld:**
It seems plausibly true. But even so, even if what people are doing is irrational, I don't know, we have some kind of intuition for what one does. I guess you have. I didn't. I think you're basically right about what people are doing. But you just think, "Okay. Well, we do this and that's how we solve this." And I don't know, maybe we can come up with other examples where it's more straightforward that people just follow some convention and it's clear that people are following this convention because there is this convention. And so with AI, it's harder to guess, especially if you imagine AI systems that haven't learned to play against each other, face each other for the first time on some problem with equilibrium selection. It seems harder to say what they're going to do.

On the other hand, there are also some things that I think are easier to reason about. Maybe the rational agent model is more accurate about the kind of AI systems that we worry about than about humans. Also, if we think more from this prescriptive perspective of trying to figure out how to make things go well, then there are a lot of tricks that apply to AI more than to humans. So I guess we'll talk a bit about some of my work later that's in part about some of these things that seem to apply more to AI than to humans. For example, it's easier to imagine very credible commitments because the AI can be given any goals, we can, I don't know, change its source code in various ways. Whereas with humans that's harder to do.

Another way in which the situation might be harder for AI is that with humans - this is related to what we already talked about - humans have already trained a bunch against each other. Probably some kind of group selection type or selection on a convention level has occurred. So successful conventions or successful ways of selecting equilibria have been selected [over] conventions that fail somehow. And this more evolutionary-style learning seems less realistic for AI at least. It's very different from gradient descent or other contemporary techniques.

**Daniel Filan:**
It seems like there's this general issue where on the one hand, the AIs can do more stuff. They can be given various types of source code, they can read other AIs' source codes... On the one hand, that gives you more tools to solve problems, but then on the other hand presumably it just adds to the strategic complexity. So I guess a priori, it should be unclear if that makes things easier or harder.

**Caspar Oesterheld:**
Yeah. Even some of the things that I described on the "making things easier" side, more opportunities to get good equilibria often also imply just more equilibria, which typically makes things harder.

**Daniel Filan:**
I wonder: if I just think about this, a lot of these traits of AI can sort of be implemented for humans. I think in [one of the papers](https://link.springer.com/article/10.1007/s10458-022-09574-6) we're going to be talking about, about safe Pareto improvements, it's basically about the setting where you give an AI the task to solve a game but you can change the game a little bit. And presumably, we could do that with humans. Your company has to play some game and it just outsources it to the game-playing department of your company and maybe your company can give instructions to the game-playing department. I'm wondering, have these sorts of questions been studied much in the setting of humans, or do you think AI is jolting people to think thoughts that they could have in principle thought earlier?

**Caspar Oesterheld:**
Yeah. So with some of these, that's definitely the case. Some forms of credible commitment are discussed in game theory more generally. And I also agree that this transparent source code or partially transparent source code type setting is not that bad as a model of some interactions between humans. Organizations, for example: human organizations are somewhat transparent. To some extent, we can predict what US Congress will do. It's not some black box that has some utility function, that it's going to try to best respond to something with respect to this utility functions. They have lots of rules for what they're going to do. We can ask the individual congresspeople what they feel about certain issues. So to some extent, human organizations are a bit like, I don't know, they have specified rules or constitution and so on. So in some sense, they also play this open source type game, or this game where your source code is somewhat transparent.

I think with a lot of these things, part of the reason why they aren't studied in this way is that, for example, with this open source type model, traditionally that is, it's just that it's a bit less of a natural setting and it's clear that the human setting is just very fuzzy in some sense. And I think the AI setting will actually also be very fuzzy in a similar sense, but it's easier to imagine this extreme case of being able to read one another's source code or something like that. Whereas for human organizations, it's less natural to imagine this extreme case or this particular model of source code. But yeah, I definitely agree that some of these things one can consider in the human context as well.

**Daniel Filan:**
Yeah. And it's close to models where players have types and you can observe other types and everyone of a type plays the same, right?

**Caspar Oesterheld:**
Yeah, right. There's also that kind of model.

### Do we need cooperative AI if we get alignment? <a name="why-coop-ai-if-alignment"></a>

**Daniel Filan:**
Yeah. So the final framing question I want to ask is: I think when a lot of people encounter this line of research, they think, "Okay. Well, we already have AI alignment of, AI is trying to adopt 'do what people want them to do'." If I'm, I don't know, Boeing or the US or something, and I'm making AI that's aligned with me, I want it to be able to figure out how to play games such that we don't end up in some terrible equilibrium where the world gets nuked anyway. So I think a lot of people have this intuition, "Well, we should just make really smart AI that does what we want and then just ask it to solve all of these cooperative AI questions." And I'm wondering: what do you think about that plan or that intuition?

**Caspar Oesterheld:**
Okay. I think there are multiple questions here that I think are all very important to ask and that to some extent I also think are important objections to a lot of work that one might do. I guess one version of this is that if we build aligned AI, whatever problems we have, we can just ask the AI to solve those problems. And if the AI is just good at doing research on problems in general because it's generally intelligent or something like that, then we should expect it to also be able to solve any particular problem that we are concerned about.

**Daniel Filan:**
Or there's a weaker claim you could make, which is it can solve those problems as well as we could solve them if we tried.

**Caspar Oesterheld:**
Right, yeah.

**Daniel Filan:**
Sorry, I interrupted.

**Caspar Oesterheld:**
No, that's a good point. Obviously, that's the more important claim. And I think that is an important consideration for cooperative AI but also for a lot of other types of research that one might naively think it is valuable to do. I think there are lots of specific things where I think this really works extremely well. If you think about solving some specific computational problem, like developing better algorithms for finding the good Nash equilibria in a three-player, normal-form game or something like that, it seems very likely that if we get, or once we get human-level AI or superhuman level AI, it will just be better at developing these algorithms, or at least as good as humans at developing these algorithms. Because to some extent, it seems like a fairly generic task.

I would imagine that an AI that's generally good at solving technical problems will be good at this particular problem. What I think is an important property of the kinds of problems that I usually think about or the kinds of ideas that I usually think about is that they're much less well-defined technical problems and they're much more conceptual. I guess we'll be talking about some of these things later, but they seem much less like the kinds of problems where you just specify some issue to the AI system and then it just gives the correct answer.

And I also think that, and this goes maybe more towards a different version of this objection, but I think another important claim here is that in some sense, game theory itself or strategic interaction between multiple agents is a special thing in a way, in a way that's similar to how alignment or having the kinds of values that we want it to have is special in that it's to some extent orthogonal to other kinds of capabilities that one would naturally train a system to have. So in particular, I think you can be a very capable agent and still land in bad equilibria in some sense. A bad Nash equilibrium is an equilibrium where everyone behaves perfectly rational, holding fixed [what] their opponent [does], but the outcome is still bad. And so at least if you imagine that training mostly consists of making agents good at responding to their environment, then landing in a bad Nash equilibrium is completely compatible with being super competent at best responding to the environment.

**Daniel Filan:**
Yeah. I guess maybe this goes to some underlying intuition of game theory being a deficient abstraction because, I don't know, in a lot of these situations, in a lot of these difficult game theory things. Like the [prisoner's dilemma](https://en.wikipedia.org/wiki/Prisoner%27s_dilemma), where it's better for us if we cooperate, but whatever the other person does, I'd rather defect, but if we both defect, that's worse than if we both cooperate: a lot of the time I'm like, "Well, we should just talk it out"; or we should just build an enforcement mechanism and just change the game to one where it actually does work. And maybe this just ends up as... If these options are available to powerful AIs then somehow the game theory aspect maybe or the multiple Nash equilibria thing is maybe less relevant. So I don't know, maybe this comes down to a rejection of the game theory frame that one might have.

**Caspar Oesterheld:**
I'm not sure I understand this particular objection. By default, the normal prisoner's dilemma, single shot without repetition, without being able to set up enforcement mechanisms, make incredible commitments and these kinds of things... it just has one Nash equilibrium, right?

**Daniel Filan:**
Yeah. I think the complaint is: in real life, we just are going to be able to communicate and set up enforcement mechanisms and stuff.

**Caspar Oesterheld:**
Yeah, I think I mostly agree with that. But the enforcement mechanisms are what would in the prisoner's dilemma introduce multiple Nash equilibria, right?

**Daniel Filan:**
Yeah.

**Caspar Oesterheld:**
I guess in the prisoner's dilemma, it's natural what the good equilibrium is because the game is symmetric so you just could do the Pareto optimal symmetric equilibrium. So assuming you can make a cooperate-cooperate Nash equilibrium by playing the game repeatedly or having enforcement mechanisms and so on - then it seems very natural that that's what you would do. But if we ignore the symmetry intuition or we just take some game that is somewhat more asymmetric, then it seems like even with these enforcement mechanisms, there might be many possible equilibria that are differently good for different players. And yeah, I don't see how you would avoid the multiple Nash equilibria issue in that kind of setting.

**Daniel Filan:**
Yeah. I guess the hope is that you avoid the issue of really bad Nash equilibria. But I guess you still have this to the degree that there's some asymmetry or anti-symmetry or something. I guess you have this issue of there being multiple equilibria that you've got to bargain between. If we both have to coordinate to do a thing and I like one of the things more and you like one of the things more and somehow we can't randomize or something, I guess in that situation we've got this bargaining game of "okay, are we all going to have this rule that says we do the thing you like? Or are we all going to have this rule that says we do the thing I like?" And it ends up being a metagame, I guess.

**Caspar Oesterheld:**
Yeah, I think I'm more worried about the multiple equilibrium thing than about getting the good equilibrium if there's a unique good equilibrium. If you think of something like the [stag hunt](https://en.wikipedia.org/wiki/Stag_hunt) or a prisoner's dilemma where all you really need to do is, I don't know, pay 10 cents to set up this commitment mechanism that allows you to play cooperate-cooperate in equilibrium, I'm also much more optimistic about that. It's still funny because in some sense there's not really that great of a theory for why the good thing should happen, but it seems intuitive that it would happen.

And I think maybe this is also one of the... one way in which game theory is weird that even this very basic thing where it feels like you can just talk it out and say like, "Yeah, we are going to do this good thing," and so on. Game theory doesn't really have a model for this "talking it out" thing to get the good equilibrium. Really, that doesn't seem that difficult to do. But yeah, the case that I'm much more worried about is the case where you have lots of different equilibria and the game is very asymmetric and so it's just completely unclear which of the equilibria to go for.

### Cooperative AI and agent foundations <a name="coop-ai-and-af"></a>

**Daniel Filan:**
Okay. I said that was my final question. But I actually have a final bridging question before I get to the next thing we're going to talk about, which is: I think a lot of cooperative AI stuff, it often seems adjacent to this [agent foundations](https://www.alignmentforum.org/posts/FWvzwCDRgcjb9sigb/why-agent-foundations-an-overly-abstract-explanation) line of work where people are really interested in: you're an AI and there are other players in the world and they're modeling you and you're modeling them, but you can't perfectly model something that's perfectly modeling you because your brain can't properly fit inside your brain. And people in this world are often interested in different decision theories or ways of thinking about how to be uncertain when you're computationally limited. A lot of this kind of thing seems like it shows up in the cooperative AI literature, I guess specifically [your research group's](https://www.cs.cmu.edu/~focal/) cooperative AI literature. I'm wondering: why do you think that overlap is there and what do you think are the most important reasons driving that overlap?

**Caspar Oesterheld:**
Yeah, that's another very interesting question. I should start by saying that there are also lots of people who work on cooperative AI and who aren't thinking that much about these more foundational questions. I don't know, [they're] more on the side of developing machine learning algorithms or setups for machine learning such that different machine learning agents just empirically are more likely to converge to equilibria or to better equilibria and things like that. But yeah, I agree that with respect to me in particular, there's a lot of overlap. Okay, I think there are a couple of reasons. One reason is that it's good to do things that are useful for multiple reasons. So if you can do something that's useful both for analyzing the multi-agent stuff and also good for this more agents foundations perspective, that's nice.

I think the deeper reason for why there's overlap has to do with a lot of just the object-level issues of game theory and how they interact with these things. For example, it seems that this issue of not being able to fully model your environment, it comes up very naturally in game theory because basically if you have two rational agents, there's kind of no way to really assume that they can both perfectly have a model of the other player or something like that. And they can best respond to each other, but these kind of assumptions...

In the case of mere empirical uncertainty, you can at least theoretically make these kind of assumptions, that you have your Bayesian prior or something like that and you just do Bayesian updating and then you converge on the correct world model. But everyone knows that's not actually how it works, but you can easily think about this, and so you just think about this even though actually the real systems that actually do stuff in the real world don't really work this way, but you can have this model. Whereas in game theory, it's much harder to avoid these kinds of issues.

**Daniel Filan:**
But there... I mean, if that was it, I would expect that there would be this whole field of game theory AI and they would all be interested in agent foundations stuff, but somehow it seems like the cooperative AI angle... I don't know, maybe I just haven't seen the anti-cooperative AI agent foundations work.

**Caspar Oesterheld:**
Yeah, so there is actually lots of work that is on these kind of foundational questions and that isn't particularly motivated by foundations of cooperative AI. So there's this whole "regret learning" literature, which is... It's [a huge literature](https://tor-lattimore.com/downloads/book/book.pdf) about some theoretical model of learning in games (or learning in general, but you can in particular apply it to learning in games). So it's a model of rationality. You can use it as a model of bounded rationality. And then there are [many](https://www.jstor.org/stable/2999445), [many](http://www.mit.edu/~gfarina/about/) papers about what happens if you have two regret learners play against each other, how quickly do they converge to what kind of solution concept? Do they converge to Nash equilibrium or coarse correlated equilibrium or whatever? So I think this literature basically just does exist.

**Daniel Filan:**
Okay.

**Caspar Oesterheld:**
I think most of this is a bit less motivated by AI, but yeah, there's definitely a lot of foundational work on the intersection of what's a rational agent, what should a rational agent do, or how should one learn stuff, and multi-agent interactions, that isn't about cooperation.

## A Theory of Bounded Inductive Rationality <a name="brias"></a>

**Daniel Filan:**
Okay, cool. Well, I guess bridging from that, I guess I'd like to talk about your paper, [A Theory of Bounded Inductive Rationality](https://arxiv.org/abs/2307.05068). So this is by yourself, [Abram Demski](https://scholar.google.com/citations?user=tsrblo8AAAAJ&hl=en&oi=ao), and [Vincent Conitzer](https://www.cs.cmu.edu/~conitzer/). Could you give us a brief overview of what this paper is about?

**Caspar Oesterheld:**
Sure. So this paper considers a setting, which we for now might imagine is just a single agent setting where you have a single agent and it has to learn to make decisions. And the way it works is that every day, or I don't know, at each time step, as you might say, it faces some set of available options and it can choose one of these options. So you might imagine, every day someone comes to you and puts five boxes on your table with different descriptions on them and then you have to pick one of them. And then once you pick a box, you get a reward for the box that you pick and you only observe that reward. You don't observe any kind of counterfactuals. You don't observe what would've happened had you taken another box. And in fact, it might not even be well-defined what would've happened, like what are these counterfactuals anyway? So all that happens, basically, you can choose between different things. You choose one of the things, you get a reward.

And this happens every day or every time step. And now the question is: how should you learn in this kind of setting? How should you learn to make choices that maximize reward? And in particular, we consider a myopic setting, which means we just want to maximize the short-term reward, like the reward that we get right now with the next box that we pick. I don't know, maybe at some point we figure out that whenever you take the blue box, maybe you get a high reward, but then for the next 1,000 times steps, the reward will be low. The basic setting that the paper considers is such that you then still want to take the blue box. You don't care about this losing the reward on the next 1,000 days, though you can consider alternatives to this.

Okay, so this is the basic setting of learning and this is very similar to this [bandit literature](https://tor-lattimore.com/downloads/book/book.pdf) where regret minimization is kind of the main criterion of rationality that people consider. So it's very similar to that. The main difference is that we-

**Daniel Filan:**
Wait, before you go on, what is regret minimization?

**Caspar Oesterheld:**
Oh yeah, good question. I should explain that. So here's one notion of what it means to do well in this type of scenario. I don't know, let's say you play this for 1,000 rounds and then in the end you ask yourself, "How much better could I have done had I done something else?" So for example, how much better could I have done had I only taken the - let's say every day there's a blue box - had I always just taken the blue box? And so the regret is kind of like: how much worse are you doing relative to the best thing you could have done in retrospect? And usually there are constraints on what the best thing in retrospect is: usually it's just not achievable to have low regret relative to on each day picking the best thing.

Okay, I'll just introduce one more aspect of the setting because that's also important for our paper. So for example, you could have some set of experts, that you might imagine is just some set of algorithms, and on each day each expert has a recommendation for what you should do. Then your regret might be, "How much worse did I do relative to the best expert?" So after 1,000 days, you look at each expert and you ask, "How much [better] would I have done had I chosen at each time step what this expert recommended?"

**Daniel Filan:**
And so my understanding is: the reason we're saying something like this rather than just "in retrospect, the best thing you could have done ever" is, if the results are random or unknowable or something, in retrospect we could just pick the winning lottery numbers and stuff, but you don't want to allow those sorts of strategies, is my understanding.

**Caspar Oesterheld:**
Yeah, exactly. Very good. Yeah, thanks for explaining this. Yeah, I think that is exactly the justification for doing that.

**Daniel Filan:**
Okay.

**Caspar Oesterheld:**
Okay, so minimizing regret means minimizing regret with respect to the best expert in retrospect. And the intuition here from a learning and rational agent perspective is that, I don't know, you're some agent, you have limited computational abilities, and let's say that this set of experts is the set of algorithms that you can compute or something like that, or you can compute all simple strategies for what you should do. And then low regret means that there's no... In this set, there's no strategy that does much better than what you do. And in particular, usually what people talk about is sublinear regret, which means that as time goes to infinity, your average regret per round goes to zero, which in some sense means that in the limit you learn to do at least as well as the best expert.

So one very simple version of this is: imagine you play some game, you play chess or something like that, and there are 10 people in the room and they each make recommendations for what you should play, and you don't know anything about chess. And now one of the people in the room is a chess grandmaster and the other ones are just random non-chess-grandmasters who are maybe much worse, or a bit worse. Then it seems like if you learn in a reasonable way, you should learn to do at least as well as the grandmaster, right? Because the least thing you can learn is just do whatever this grandmaster does, right?

**Daniel Filan:**
Yep.

**Caspar Oesterheld:**
That's kind of the intuition here.

**Daniel Filan:**
But in your paper, you're contrasting yourself with regret minimization.

**Caspar Oesterheld:**
Yeah.

**Daniel Filan:**
So what's so bad about regret minimization? Isn't that just equivalent to 'do as well as you can'?

**Caspar Oesterheld:**
Yeah, I think regret minimization is certainly a compelling criterion. I think in the agent foundations, existential risk community, I think too few people are aware of regret minimization and how it's a very simple criterion and so on. Okay, now I want to make the case against this. So why don't I find this very satisfying? So it has to do with the way that regret minimization reasons about counterfactuals. So an immediate question that you might have about regret minimization is, "Is this even ever achievable?" Can you even ensure that you have low regret or sublinear regret? Because you might imagine a setting where the environment exactly computes what you do and always does the opposite, right?

Like, on every day you have two boxes in front of you and one of them contains a dollar and the other one doesn't. And the way the environment fills the boxes is that it predicts what you do and then it puts the dollar in the other box. And in this kind of setting you might imagine it seems really hard to achieve low regret here so it will be difficult to, in retrospect, not do worse than, for example, always taking the left box, right? Because let's say you switch back and forth between the two, then in retrospect half the time the left box had the dollar. So you would've done better had you just always taken the left box.

**Daniel Filan:**
Yeah. For "if you would've done better"... somehow this is assuming that I could have just taken the left box, but the problem would've been the same as it actually was.

**Caspar Oesterheld:**
Exactly. Part of how this criterion works is that it assumes that the problem says... the specific instance of this multi-armed bandit problem (as it's called) that you face consists in specifying at each time step, how much money or how much reward is in each of the boxes, and that this is in some sense independent of what you do. In particular the adversarial multi-armed bandit literature very explicitly allows the case where the way the boxes are filled happens to be exactly running whatever algorithm you run. So how is this solved? So the way this is solved is that the learner has to use randomization. So you have to randomize over which boxes you pick or which expert's advice you follow.

**Daniel Filan:**
This is how it's solved in the adversarial-

**Caspar Oesterheld:**
Yeah, in the adversarial setting. There are also non-adversarial settings where basically, I don't know, you know from the start that each of the boxes on every day follows the same distribution, like some Gaussian with some mean. The whole task consists in doing optimal exploration to find out which box has the highest mean value. And there you don't need randomization. I think in practice probably people still do randomization in these cases, but you definitely don't need it.

Whereas in this adversarial case, to achieve sublinear regret, you have to randomize and you have to also assume that the environment cannot predict the outcomes of your random coins. It's like, I don't know, if you flip a coin to decide whether to take the left or the right box, then you assume that the environment, it can also flip a coin, but it can only flip a different coin.

So I find that a bit dissatisfying philosophically, it seems a bit weird to assume this, but that maybe I can live with. I think the part that I really kind of don't like is that in these problems where you randomize or where you need to randomize, regret minimization requires that you randomize, but the restrictions that it imposes are all on your actions rather than on the distributions over actions that you pick.

**Daniel Filan:**
How do you mean?

**Caspar Oesterheld:**
Yeah, so here's an example, which is it's basically [Newcomb's problem](https://en.wikipedia.org/wiki/Newcomb%27s_paradox). So imagine that there are two boxes. The way the environment fills the boxes works as follows. It runs your algorithm to compute your probability of choosing the left box. And the higher this probability is of you choosing the left box, the more money it just puts in both boxes, but then also it puts an extra dollar into the right box. And we make it so that your probability of choosing the left box, it really, really strongly increases the reward of both boxes.

So you get, I don't know, for each 1% of probability mass that you have on the left box, you get a million dollars or something like that put in both boxes. The numbers don't need to be that extreme, I just don't want to think about the exact numbers or how large they need to be. So in this problem, if this is how the problem works, then if you want to maximize your utility and you maximize over your probability distribution over boxes, then I think the reasonable thing to do is to always pick the left box. Because basically for each probability mass that you put on the left box, you gain lots of money. Whereas for each, you only gain $1 by moving probability mass from the left box to the right box.

**Daniel Filan:**
Yep. And you lose all the money you could have made by having probability mass on the left box and both boxes get money.

**Caspar Oesterheld:**
Yeah. So for example, if you always choose the right box, you only get a dollar. And if you always choose the left box, you get a hundred million dollars. Okay, so this is an example of a setting, and if you optimize over probability distributions, then you should choose the left box. This is not what regret minimization says you should do. Regret minimization would here imply that you have to always choose the right box, well, in the limit. You have to learn to always choose the right box. And the reason for that is that if you choose the left box, you'll have regret, right? You'll regret that you didn't choose the right box because then you would've gotten a dollar more.

And the reason why... that's kind of what I said somewhat cryptically, in some sense the reason for why this issue occurs is that regret minimization is, it's a criterion on which actions you take and it doesn't care about how you randomize. It kind of doesn't say anything about how you should randomize other than saying, "Well, you should randomize over the things that give you high reward holding fixed how you randomize," or I don't know, "We don't care about how you randomize." It says something about these actions and it doesn't say how you should randomize. And I think this is just not so compelling in this particular problem. I think, in this particular problem, one should just always choose the left box.

**Daniel Filan:**
And is this also true in the setting of adversarial bandits?

**Caspar Oesterheld:**
Yes. Basically adversarial bandits allow specifically for this kind of problem. To get high reward on these, one has to do this randomization stuff where sometimes one has to learn to make the probability distribution so that one's reward is low, just to make one's regret also low.

**Daniel Filan:**
All right. So okay, that's what's wrong with other settings. So what's your theory of bounded inductive rationality?

### Why it matters <a name="why-brias-matter"></a>

**Caspar Oesterheld:**
Can I say something about why I think this is even important?

**Daniel Filan:**
Oh, yeah: why is it important?

**Caspar Oesterheld:**
Okay, so this particular setting, this "left box, right box" example is farfetched, of course, but the reason why I think it's important is that it's this kind of setting where the environment tries to predict what you do and then respond to it in a particular way. To some extent, that's kind of the core of game theory. And so if you want to use these regret minimizers specifically in a game theoretic context, I think it's kind of weird to use them given that it's very easy to come up with these game theory-flavored type of cases where they clearly give weird results. So in particular, I think sometimes people justify Nash equilibrium by appealing to these regret minimizers, so [you can show that if you have two regret minimizers and they play a game against each other, they converge to... I think it's not exactly Nash equilibrium, it's some form of correlated equilibrium.](https://www.jstor.org/stable/2999445)

But yeah, some equilibrium concept. (That is if they converge. Sometimes they also don't converge.) But if they converge, they converge to some kind of Nash-like solution concept. And so you might say, "Well, it's great. This really shows why Nash equilibrium is a good thing and why rational agents should play Nash equilibrium against each other." In some sense, I think this Nashian idea is already kind of baked in in this regret minimization concept in a way that seems not so compelling, as this "left box right box" example shows. Yeah, so that's why my theory... That's why I've worked on this and tried to come up with a theory that reasons about randomization and things like that in a very different way.

**Daniel Filan:**
Okay. Can I try to paraphrase that? So we want a theory of how to be a rational agent and there are a few criteria that we want. Firstly, we want to be able to go from something like how do you make decisions normally, we want to be able to use that in the context of game theory as some sort of foundation. And secondly, we want to have it be bounded. So that's why we're sort of thinking of the regret minimization, some number of experts frame where we just want to consider all the ways of doing things we can fit in our head and not necessarily worry about everything. So we want to have a decision theory foundation for game theory and we want to be bounded. And because we want a decision theory foundation for game theory specifically, we want our decision theory method... We want our way of making decisions to allow for environments that are modeling us.

And so basically your complaint is: well, normal regret minimization, it does the bounded thing, but it doesn't do a very good job of thinking about the environment modeling you. There are various ways of thinking about game theory where you can think about the environment modeling you, but you're not necessarily bounded. Probably [my favorite paper](https://arxiv.org/abs/1508.04145) in this line is the 'reflective oracles' line of work where-- sadly, I can't explain it right now, but if you assume you have something that's a little bit less powerful than a halting oracle, but still more powerful than any computer that exists, then agents that just model their environment and make decisions, they end up playing Nash equilibria against each other. It's a really cool line of research. I encourage people to read it, but no existing thing could possibly implement the thing that those papers are talking about. So you want to get all of these three criteria: boundedness, decision theory to game theory, and environments that are modeling the decision maker. Is that right?

**Caspar Oesterheld:**
Yes. That's a very good summary. Thanks.

### How the theory works <a name="how-brias-work"></a>

**Daniel Filan:**
All right, I now feel like I understand the point of this paper and how it's related to your research agenda much better. Okay, so we've talked about what you want to be doing. How's it work? What do you have?

**Caspar Oesterheld:**
Okay, so we have the same kind of setting as before. On each day, we choose between a set of options. And slightly different, we don't require that these counterfactuals, which one talks about a lot in regret minimization, we don't require that these are well-defined. And we also have these experts... I guess we call them hypotheses rather than experts, but they basically do the same thing except that in addition to making a recommendation at each time step, they also give estimates of the utility that they expect to get if the recommendation is implemented.

So if you, again, have this picture of: I'm trying to play chess and there are 10 people in the room, then one of them might say, "Okay, if you play Knight F3 then I think there's a 60% chance that you're going to win or you're going to get 0.6 points in expectation," or something like that. So they're a bit more complicated than the experts, but only slightly; they give you this one additional estimate. And now, similar to regret minimization, we define a notion of rationality relative to this set of hypotheses. I think I want to describe it in terms of the algorithm rather than the criterion for now, because I think the algorithm is actually slightly more intuitive.

**Daniel Filan:**
So this is the opposite way than how you do it in [the paper](https://arxiv.org/abs/2307.05068) (for people who might read the paper).

**Caspar Oesterheld:**
Yeah, the paper just gives the criterion and then the algorithm is just kind of hidden in the appendix. I mean, it's like some brief text in the main paper saying that, "Yeah, this is roughly how it works." In some sense, of course, the general abstract criterion is much more... I think it's more important than the algorithm itself. I think it's a bit similar to [the logical induction paper](https://arxiv.org/abs/1609.03543) where they define similarly a kind of notion of rationality or having good beliefs or something like that. And yeah, they have this criterion, but they also have a specific algorithm, which is a bit like running a prediction market on logical claims. And I think... In practice, I think many more people have this idea in their minds of "just run a prediction market between algorithmic traders" than this specific criterion that they define. Even though I think from a theoretical perspective, the criterion is really the important thing and the algorithm is just some very specific construction.

**Daniel Filan:**
Yeah. I actually want to talk more about the relationship to the logical inductors paper later. So what's your algorithm?

**Caspar Oesterheld:**
So basically the algorithm is to run an auction between these hypotheses. So first we need to give the hypotheses enough money to deal with. So they now have money, like some kind of virtual currency. We need to be a bit careful about how exactly we give them money initially. It's especially tricky if we have infinitely many hypotheses, but basically we need to make sure that we eventually give all hypotheses enough money so that we can explore them, which has to be infinite money. But we also, if we give everyone $10 in the beginning, then this auction will just be chaos because there will be lots of crazy hypotheses that bid nonsense. So maybe it's easiest to first consider the case of finitely many hypotheses, and let's just say that in the beginning we gave each hypothesis 100 virtual dollars. And now we run auctions and the way do this specifically is that we just ask all the hypotheses for their recommendation and their estimate of what reward we can get if we follow the recommendation.

Then we follow the highest bidder, so we take the highest bidder. But the hypotheses, in how much they can bid, they're constrained by how much money they have. So if a hypothesis doesn't have any money, it can't bid high in this auction. So we take the highest bid - the highest budgeted bid, I guess - then we take that hypothesis. The hypothesis has to "pay us" their bid. So it's like a [first price auction](https://en.wikipedia.org/wiki/First-price_sealed-bid_auction). They pay us their bid and then we do whatever they told us we should do. Then we observe, we get our reward, and then we pay them that reward or a virtual currency amount proportional to that reward.

**Daniel Filan:**
Okay. So is the idea that if a hypothesis slightly low-balls, but it's basically accurate about how much reward you can get, other hypotheses that overestimate how well you can do are going to blow all their money, you're going to save it up because you get some money every round. And then eventually you could be the top bidder and you'll make money by... you slightly low ball, and so you get back a little bit more than you spent to make the bid and you just eventually dominate the bids. Is that roughly how I should think of it working?

**Caspar Oesterheld:**
Yes, that's basically the thing to imagine. I think depending on... Yeah, with this low-balling, that depends a bit on the scenario. For example, if you have a setting where the payoffs are deterministic and fully predictable, so you just choose between a reward of 5 and a reward of 10 and there are hypotheses that can just figure out that these are the payoffs, then if you have enough hypotheses in your class, then there will be just one hypothesis that just bids 10 and says you should take the 10 and then you won't... The winning hypothesis won't be low-balling, it will just barely survive. It's like the typical market argument where if you have enough competition then the profit margins go away.

**Daniel Filan:**
Okay. And the idea is that other agents which overpromise are going to... If agents overpromise, then they lose money relative to this thing that bids accurately. And if agents underpromise, then they don't win the auction. And so this thing never spends money so it just survives and wins.

**Caspar Oesterheld:**
Yeah, basically that's the idea. I guess the most important kind of features, the first thing to understand is really that the hypotheses that just claim high rewards and don't hold up these promises, they just run out of money and so they don't control much what you do. And so what's left over once these are gone is you basically follow the highest bid among those bidders that do hold up their promises. And so in the limit, in some sense, among these you actually do the best thing.

### Relationship to logical induction <a name="rel-to-li"></a>

**Daniel Filan:**
Cool. And so basically in the rest of the paper, if I recall correctly, it seems like what you do is you basically say... well, you make a bunch of claims, which net out to, if one of these traders in this auction can figure out some pattern, then you can do at least as well as that trader. So if you're betting on these pseudorandom numbers, then you should be able to do at least as well as just guessing the expectation. And if none of... If the pseudorandomness is too hard for any of the agents to crack, then you don't do any better than that. At the end, I think there's a connection to game theory, which we can talk about a bit later, but we touched a bit on this relationship to logical inductors. When I was reading this paper, I was thinking, the second author, Abram Demski, I think he's an author on [this logical inductors paper from 2016 or so](https://arxiv.org/abs/1609.03543).

**Caspar Oesterheld:**
I think he's actually not. I'm entirely sure, but I think he might not be.

**Daniel Filan:**
Oh, he's not? Okay. He is a member of [the organization that put out that paper](https://intelligence.org/) at least.

**Caspar Oesterheld:**
He is very deep into all of this logical induction stuff. I'm not sure I could have written this paper without him because knows these things very well and I think that was quite important for this particular project.

**Daniel Filan:**
Yeah. You've got this coauthor who's connected to the logical induction world. They're both about inductive rationality by bounded beings, and they both involve these algorithms where agents bid against each other. How is this different from the logical induction paper?

**Caspar Oesterheld:**
I guess the main and most obvious difference is that the logical induction paper is just about forming beliefs in some sense. It's just about assigning probabilities to statements. You can think about how you can use that then to make decisions, but very basically, if you just look at the logical induction paper, it's all about forming beliefs about whether a particular claim is true. Whereas regret minimization, but also this rational inductive agency project, it's all about making decisions between some set of options. I think that's a fundamentally different setting in important ways. I think the most important way in which it's different is that you have to deal with this counterfactual issue, that you take one of the actions and you don't observe what would've happened otherwise. For example, one way in which you can clearly see this is that in any of these decision settings, you have to pay some exploration cost.

With the bounded rational inductive agents, sometimes you will have to follow a bidder, a hypothesis that has done terribly in the past. You need to sometimes follow it still because there's some chance that it just did poorly by bad luck. In fact, we didn't go that much into the details of this, but you actually have to hand out money for free to your hypotheses so that you can exploit each hypothesis infinitely often because otherwise, there's some chance that there's some hypothesis that really has the secret to the universe, but just on the first hundred time steps for some reason it doesn't do so well. You have to pay this cost, and regret minimizers also pay this cost. With logical induction, in some sense you do exploration, but one thing is that if you... maybe this will be hard to follow for readers who aren't familiar with that. Sorry, listeners.

**Daniel Filan:**
I'm now realizing we should probably just say a few sentences about what [that paper](https://arxiv.org/abs/1609.03543) was. According to me, the logical inductors paper was about how do you assign probabilities to logical statements. The statements are definitely either true or false. They're statements like "the billionth and 31st digit of pi is 7", and you're like, what's the chance that that's true? Initially you say 1/10th until you actually learn what that digit of pi actually is by calculating it or whatever, and it uses a very similar algorithm. The core point of it is every day you have more questions that you're trying to assign probabilities to, and you learn more logical facts and eventually you just get really good at assigning probabilities.

**Caspar Oesterheld:**
Yeah.

**Daniel Filan:**
Anything I missed out on?

**Caspar Oesterheld:**
Well, I guess for what I was about to talk about it's good to have some kind of intuition for how the actual algorithm or the proposed mechanism works. Very roughly it's that instead of hypotheses or experts, they have traders which are also, I don't know, some set of computationally simple things. Very roughly what happens is that these traders make bets with each other on some kind of prediction market about these different logical claims, that one is trying to assign probabilities to. Then the idea is if there's a trader that's better than the market at assigning probabilities, then the trader can make money by bidding against the market. Eventually that trader will become very wealthy and so it will dominate the market. That way you ensure that in some sense you do at least as well as any trader in this set.

**Daniel Filan:**
Crucially, traders can kind of choose what markets to specialize in. If you're betting on digits of pi or digits of e, I can be like, "well, I don't know about this e stuff, but I'm all in on digits of pi". I guess this can also happen in your setting if you've got different decision problems you face.

**Caspar Oesterheld:**
Yeah. That is the way in which our bounded rational inductive agency theory is more similar to the logical inductors. Our hypotheses are allowed to really specialize and they only bid every 10,000 steps on some very special kind of decision problem, and otherwise they just bid zero. You still get that power in some sense. Whereas the regret minimizers don't have this property. Generally, you really only just learn what the best expert is.

**Daniel Filan:**
Cool. I cut you off, but sorry, we were saying about a comparison with logical inductors and it was something about the traders.

**Caspar Oesterheld:**
Yeah. One thing is that if you have a new trader, a trader that you don't really trust yet, then you can give them a tiny amount of money and then they get to make bets against the market. If you give them a tiny amount of money, their bets won't affect the market probabilities very much. You can explore this in some sense for free. In some sense, they don't influence very much what you think overall because the idea is even if you give them a tiny amount of money, if they're actually good, they'll be able to outperform the market and they'll be able to get as much money as they want. That isn't possible in the decision-making context because to explore a hypothesis in the decision-making context, you have to make this "yes/no" decision of actually doing what they do. If you have a hypothesis that's just a complete disaster, you're going to make this disastrous decision every once in a while.

**Daniel Filan:**
Yeah. In some sense it's because decisions just have discrete options. If you're a predictor, you can gradually tweak your probability a tiny bit, but you can't quite do that in...

**Caspar Oesterheld:**
Yeah. Though, I think the fundamental issue has more to do with these counterfactuals and then the counterfactuals not being observed. To test a hypothesis, you have to do something differently. Let's forget about the logical inductors for a second and just say, I don't know, you make some predictions about all kinds of things and I am unsure whether to trust you or not. Then I can just ignore what you say for the purpose of decision-making or for the purpose of assigning beliefs myself or stating beliefs to others. I can completely ignore this. I don't have to do anything about it, and I can just track whether you are right. If you're good, I'll still eventually learn that you're good. Whereas if you tell me, I don't know, you should really do more exercise or something like that and I ignore it, I'll never learn whether you were actually right about it or not.

**Daniel Filan:**
Okay. Yeah, it's giving me a better sense of the differences these theories have. Cool. Another difference, which was kind of surprising to me, is that in the logical inductor setting, I seem to recall hearing that if you actually just do the algorithm they proposed in the paper, it's something like 2 to the 2 to the X time to actually figure out what they even do. Whereas with your paper, if all of the bidders about what you should do, if they're computable in quadratic time, if it's just all the quadratic time algorithms, it seemed like you can have your whole routine run in quadratic time times log of log N or some really slow growing function, which strikes me as crazy fast. What's going on there? For one, what's the difference between the logical inductor setting, and two, how is that even possible? That just seems like so good.

**Caspar Oesterheld:**
Yeah. That is a kind of notable difference, in some sense. This algorithm that I just described, this decision auction, as we sometimes call it, it's just an extremely low overhead algorithm; you can just think about it. You run all your bidders, then you have a bunch of numbers. You have to take the max of these numbers. That's all very simple. The reason I think why the logical induction algorithm that they propose is relatively slow is that they have to do this fixed point finding. I don't know.

I think roughly the way it actually works is that the traders, it's not really like a prediction market in the literal sense. I think the traders actually give you functions from market prices to how much they would buy or something like that. Then you have to compute market prices that are a fixed point of this, or an approximate fixed point of this. This fixed point finding is hard. I think maybe here the continuity is a big issue; that probabilities are continuous, so in some sense you need to find something in a continuous space.

The correct probability in some sense is some number between 0 and 1, or actually I think it's the probability distribution over all of these logical statements or the probabilities of all of these logical statements. It's this really large object or an object that in some sense carries a lot of information. It's from some large set, so they need to find this in this large set. Whereas we only have this one decision. But I think that on some level, the criteria are just pretty different. They're similar in many ways in that they designed this market and so on, but I think on some level the criteria are just somewhat different between the logical induction paper and ours.

**Daniel Filan:**
Yeah. It's strange though because you would think that making good decisions would reduce to having good beliefs. One way you can do the reduction is every day you have some logical statement and you have to guess "is it true with probability 1?", with probability 99%, 98%, and I don't know. You just have 100 options and you have to pick one of them.

**Caspar Oesterheld:**
Yeah. In theory that kind of works, but if you apply a generic decision-making framework to this setting where you have to say the probability or decide which bets to accept or something like that, then you're not going to satisfy this really strong criteria that this Garrabrant [logical] inductor satisfies. For example, if you have the bounded rational inductive agents and on each day you have the choice between what's the highest price you would be willing to pay for a security that pays a dollar if some logical statement is true and pay zero otherwise. That's assigning a probability to that statement, but you have to explore all of these hypotheses that say, yeah, you should buy the $1 thing. You should buy it for $1. Even for digits of pi, where let's say you take... or coin flips, something that's actually random and where you should just learn to say one half every day. The bounded rational inductive agents will necessarily say any answer infinitely often. They will converge to giving one of the answers with limit frequency 1, but every once in a while they'll say it's something completely different.

**Daniel Filan:**
Yeah, so it's not like actual convergence.

**Caspar Oesterheld:**
Yeah. I think in the regret minimization literature, people sometimes say they have these different notions of convergence. It's convergence in iterates and convergence in frequencies I think, or something like that, and you only have the weaker thing with bounded rational inductive agents.

### How fast does it converge? <a name="convergence-speed"></a>

**Daniel Filan:**
Yeah. In fact, this is one of the things I was wondering, because in the paper you prove these limit properties and I'm wondering, okay, am I going to get convergence rates? It seems like if you make the wrong decision infinitely often, but infinitely less frequently, in some sense it feels like the right thing to say is that you have converged and you can talk about the convergence rate, but you haven't actually literally converged. Is there some intuition we can get on what the convergence or quasi-convergence properties of these things are over time?

**Caspar Oesterheld:**
Yeah. One can make some assumptions about a given bounded rational inductive agent and then infer something about the rate at which it converges. Roughly, I think the important factors are the following. The first is: how long does it take you to explore the hypothesis that gives you the desired behavior? If you imagine, I don't know, you have some computational problem deciding whether a given graph has a clique of size 3 or something like that, that can be done in cubic time if not faster. And so you might wonder... The first thing is how long does it take you to explore the hypothesis that just does the obvious computational procedure for deciding this, which is just try out all the combinations of vertices and deciding whether that's a clique. In some sense it's kind of similar to these bounds that you have for Occam's razor-type or Solomonoff prior things where it's kind of like the prior of the correct hypothesis.

Similarly here, it's a bit different because really what matters is: when do you start giving it money? Then there's still... if things are random, then it might be that you give it money, but then it has bad luck and then it takes a while for it to... Yeah. That's the first thing that matters. Then the other thing that matters is: how much other exploration do you do? If you explore something very quickly, but the reason you explore it very quickly is that you explore everything very quickly and you give lots of money to lots of hypotheses that are nonsense, in some sense, it then takes you longer to converge in the sense that you're going to spend more time doing random other stuff. Even once you've found the good thing, the good hypothesis that actually solves the problem and gives an honest estimate, you'll still spend lots of time doing other stuff.

**Daniel Filan:**
You have to have this balancing act in your payout schedule.

**Caspar Oesterheld:**
Even just to satisfy the criterion that we described, one has to make sure that the overall payoffs per round go to zero. In some sense the overall payoffs per round is kind of how much nonsense you can do, because to do nonsense, you have to bid high and not deliver, which is kind of like losing money out of this market. That's how you control that. Then meanwhile, you also have to ensure that each trader or each hypothesis eventually gets infinite amounts of money.

**Daniel Filan:**
Yeah.

**Caspar Oesterheld:**
These are the two things. Within satisfying these constraints you can balance them in different ways.

**Daniel Filan:**
Yeah, I guess 1 over T is the classic way to do this sort of thing.

**Caspar Oesterheld:**
Yeah, that's the one that we have in the proof.

### Non-myopic bounded rational inductive agents? <a name="without-myopia"></a>

**Daniel Filan:**
Yeah. That's actually how I... it just flashed in front of eyes and I was like, ah, now I see why they picked that. Yeah, I just skimmed that appendix. Yeah. One thing I want to ask about is: you talk about this bandit setting, and in particular you're being myopic, right? You see a thing and you're supposed to react myopically to the thing. And there are other settings, other relatively natural settings like [Markov decision processes](https://en.wikipedia.org/wiki/Markov_decision_process) or something. I'm wondering: how do you think the work could be extended to those sorts of settings?

**Caspar Oesterheld:**
Yeah. That's a good question. I think there are different ways. I guess one way is that if it's, for example, an episodic Markov decision process or I don't know, some other thing...

**Daniel Filan:**
Oh, yeah. That was my fault. Yeah. A Markov decision process is like, you're in a state of the world, you can take an action and the world just works such that whenever you're in a state and you take a certain action, there's some other state that you go to with some fixed probability no matter what the time is. Similarly, you get some reward similarly with some sort of fixed probability. Anyway, I interrupted you and I forgot what you said, so maybe you can start again. I'm sorry.

**Caspar Oesterheld:**
Right. There are different answers to how one would apply bounded rational inductive agents or extend them to the setting. I guess the most boring thing is: well, you can just apply them to find a whole policy for the whole Markov decision process, or sometimes there are these episodic Markov decision processes, which basically means you act for 10 time steps, then you get a reward, and then you kind of start over and then you can treat the episodes as separate decision problems that you can solve myopically. Okay. That's a relatively boring answer. The more interesting thing is that you could have something like a single Markov decision process and all you do is you play the single Markov decision process and it never starts over, and you want to maximize discounted reward, which is, if you get a reward of 1 today, it's worth 1 to you. If you get a reward of 1 tomorrow, you get 0.9, if you get it the day after, 0.81 and so on. That would be a discount factor of, well, 0.9 or 0.1.

In this case, I think what one could try to do, and I haven't analyzed this in enormous detail, but I think it's a very natural thing that I think probably works, is that one applies this whole auction setup, but the reward that one is supposed to estimate at each step, at each step that one is deciding which action to take, the hypotheses are supposed to give an estimate of the discounted reward that they're going to receive. Then if they win, they get the discounted reward, which means that it has to be paid out slowly over time. At time step 1000, you have to give the winner of the auction at time step 100 a tiny bit of reward. I think then probably things generally hold, with some issues that complicate things.

One issue is that hypotheses now have to take into account that on future steps exploration might occur. Let's say I'm a hypothesis and I have some amazing plan for what to do, and the plan is to first play this action, then play that action, and then this another action and so on. I have this detailed plan for the next 100 time steps, but I have to worry that in 50 time steps there'll be some really stupid hypotheses coming along, bidding some high number and doing some nonsense. I have to take this into account, so I have to bid less. This makes everything much more complicated. If there is a hypothesis that has a really good plan, it's harder for that hypothesis to actually make use of this because it can't rely on winning the auction at all these steps, so there are definitely some complications.

### Relationship to game theory <a name="rel-to-gt"></a>

**Daniel Filan:**
Yeah. Interesting. The final thing I want to talk about in this paper is: at the end you talk about using it as some sort of foundation for game theory where if you have these BRIAs, I'm going to call them (for boundedly rational inductive agents)... I say I'm going to call it, that's what they're called the paper, I'm not an amazing inventor of acronyms. You talk about these BRIAs playing games with each other and they each think of it as one of these bandit problems. Essentially you say that if they have these rich enough hypothesis classes, they eventually play Nash equilibrium with each other, is my recollection.

**Caspar Oesterheld:**
Yeah. There are different versions of the paper that actually give different results under different assumptions. There is a result that under some assumptions they give Nash equilibrium. There's also a result that is more like a folk theorem, which kind of says that you can, for example, converge to cooperating in the prisoner's dilemma. Maybe I could try to give a general sense of why it's kind of complicated what happens, and why maybe sometimes it's going to be Nash and sometimes it's going to be something else. Okay. The first thing is that these bounded rational inductive agents, nothing hinges on randomization. In some sense that's kind of the appealing part relative to regret minimizers. There's no randomization, no talk about counter-factuals. You just deterministically do stuff. In particular, if you have a bounded rational inductive agent play against the copy of itself in a prisoner's dilemma, it will converge to cooperating because whenever it cooperates, it gets a high reward, whenever it defects it gets a low reward.

So there are bidders that get high reward by recommending cooperation and bidding the value of mutual cooperation, maybe slightly below. The defect bidders, they might hope like, "okay, maybe I can get the defect/cooperate payoff", but they can't actually do this because if the bidder in one market achieves this - the hypothesis in one market - if it tries to do this, then its copy in the other market also does it. So whenever you actually win, you win the auction, you just get the defect/defect payoff. This would converge to cooperation. The reason why there's a Nash equilibrium-style result nonetheless is that: if the different agents are not very similar to each other, then you might imagine that the hypotheses can try to defect in some way that's de-correlated from the other market, or from what happens in the other auction.

The simplest setting is one where they can actually just randomize, but you could also imagine that they look at the wall and depending on the value of some pixel in the upper right or something like that, they decide whether to recommend defecting or not. If they can do this in this a decorrelated way, then they break the cooperate/cooperate equilibrium. The reason why there are different results and also why there are different versions with different results is that... I'm still unsure what the correct conclusion from it is or what the actual conclusion is. For example, I'm still unsure whether these cooperate/cooperate outcomes in the prisoner's dilemma, whether you can achieve them naturally without fine-tuning the markets too much to be exact copies.

**Daniel Filan:**
Yeah. I guess one way to think about it is, you've got this weak uncorrelation criterion where if you meet it, you fall back to Nash and another criteria and you get this. Presumably the right theorem to show is, under these circumstances you fall into this bucket and you get this, under these [other] circumstances, you fall into this bucket and you get this. If I implement my bounded rational inductive agent slightly differently, like I order the hypotheses a bit differently, do we know whether that's going to hit the corporate/corporate equilibrium or it's going to be weakly uncorrelated?

**Caspar Oesterheld:**
I think even that is already not so easy to tell. I don't know. I've done some experiments and generally it learns to defect in this kind of setting, but in these experiments the hypotheses are also relatively simple, so you don't have hypotheses that try to correlate themselves across agents. For cooperating, one hope would be that you have a hypothesis in one of the markets and a hypothesis in the other market and they somehow try to coordinate to cooperate on the same rounds. This becomes quite complicated pretty quickly. I think maybe another important thing here is that there's still a difference between the general bounded rational inductive agency criterion versus this specific auction construction. The criterion allows all kinds of additional mechanisms that you could set up to make it more likely to find the cooperate/cooperate equilibrium. You could specifically set it up so that hypotheses can request that they only be tested at various times in such a way that it's correlated between the markets or something like that. It's very complicated and that is still something I'm working on. Hopefully other people will think about this kind of question too.

## Safe Pareto Improvements <a name="spis"></a>

### What they try to solve <a name="why-spis"></a>

**Daniel Filan:**
Cool stuff. Yeah. There's actually a bunch more... there's at least one more thing, a few more things that I would love to talk about with this bounded rational inductive agents paper, but we spent a while on that and there are two other papers that I'd like to talk about as well. Let's move on to the next paper. This is called [Safe Pareto Improvements](https://link.springer.com/article/10.1007/s10458-022-09574-6) by yourself and Vincent Conitzer. Can you just give us a sense of what's this paper trying to do?

**Caspar Oesterheld:**
This paper is trying to directly tackle this equilibrium selection problem: the problem that if you play a given game, it's fundamentally ambiguous what each player should do, so you might imagine that they fail to coordinate on a good Nash equilibrium, on a Nash equilibrium at all, and they might end up in these bad outcomes. A very typical example of this is a setting where players can make demands for resources, and they can both demand some resource, and if they make conflicting demands, the demands can't both be met, so usually something bad happens; they go to war with each other in the extreme case. Safe Pareto Improvements is a technique or an idea for how one might improve such situations, how one might improve outcomes in the face of these equilibrium selection problems.

Maybe I can illustrate this with an example. Let's take a blackmail game. This is actually a bit different from the kind of example we give in the paper, but this is a version of this idea that people may have heard about under the name [surrogate goal or surrogate goals](https://s-risks.org/using-surrogate-goals-to-deflect-threats/). Let's say that I'm going to delegate my choices to some AI agent. You can think of something like GPT-4, and what I'm going to do is I'm going to tell it, "here's my money, here's my bank account, my other web accounts, and you can manage these, so maybe you should do some investing", or something like that. And now, this AI might face strategic decisions against different opponents. Right? So other people might interact with it in various ways; [they might] try to make deals with it or something like that, and it has to make decisions in the face of that.

And let's just take some very concrete interaction. So let's say that someone is considering whether to threaten to report my online banking account or something like that if my AI doesn't give them $20. So they can choose to make this kind of threat. Now, maybe, you might think that it's just bad to make this kind of threat and I should just not give in to this kind of threat. But you can imagine that there's some kind of moral ambiguity here, so you could imagine that this person actually has a reasonable case that they can make for why I owe them $20. Maybe at some point in the past I promised them $20 and then I didn't really give them $20, or something like that. So they have some reason to make this demand.

So now, this is a kind of equilibrium selection problem where my AI system has to decide whether to give in to this kind of threat or not. And this other person has to decide whether to make the threat, whether to insist on getting the $20 or not. And there are multiple equilibria. So the pure equilibria are the ones where my AI doesn't give in to these kind of threats and the other person doesn't make the threat. And so that's one. And the other one is, my AI does give in to the threat and the other person makes this kind of threat. Okay. So a typical equilibrium selection problem, in some ways. There's some ways in which this is a game theoretically a bit weird, this is a non-generic game, and so on. So I think this kind of example is often a bit weird to game theorists. So maybe, for game theorists, the example in the paper works a bit better. But I like this kind of example: I think it's more intuitive to people who aren't that deep into the game theory stuff.

Okay, so now, I'm deploying this AI system, and I'm worried about this particular strategic interaction. And in particular, maybe the case that seems worst is the case where the coordination on what the correct equilibrium is fails. So in particular, the case where my AI decides not to give in to the threat and the other person thinks like, "No, no, I should really get that $20, so I'm going to make this threat," and then they report my online banking account and they don't even get the $20, and so everyone's worse off than if we hadn't interacted at all. Basically, utility is being burned. They are spending time reporting me on this online banking platform, I have to deal with not having this bank account anymore, or have to call them, or things like that.

### Alternative solutions <a name="alternatives-to-spis"></a>

**Caspar Oesterheld:**
Okay. Now, there are various ways in which you might address this. And I kind of want to first talk a bit about some other ways you might deal with this, if that's okay, to give us a sense of what's special about the solution that we propose. Because I think it's kind of easier to appreciate what the point of it is if one first sees how more obvious ideas might fail.

So the first thing is that if one of us is able to credibly commit, at some point, to some course of action, they might want to do so. So I might think that the way for me to do well in this game is just that I should be the first to really make it credible that my AI system is never going to give in to this kind of threat. And I should just announce this as soon as possible, I should try to prove this, that I'm not going to give in to threats. And then maybe the other person, they would want to try to, as fast as possible, commit to ignore this kind of commitment and so on.

So this is... I think it's a reasonable thing to think about, but it kind of feels like it's not really going anywhere. Ultimately, to some extent, people are making these commitments simultaneously, they might also just ignore commitments, right? It seems like if you go around the world and whenever someone makes some commitment to, I don't know, threaten you or to ignore anything you do to get them to do something, you shouldn't be the kind of person to just give in and cave to any such commitment. You kind of have to have some kind of resistance against these kind of schemes. So I think in practice, this just doesn't resolve the problem.

And also, it's a very zero-sum way of approaching solving this problem. It's all about, I am just going to try to win. Right? I'm just going to try to win by keeping my $20 and deterring you from even threatening me. And you might say, "Well, I'm going to deter that. I really want the $20, and I'm going to be first." Right? So...

**Daniel Filan:**
It's also very... I don't know, if you imagine implementing this slightly more realistically... people learn things over time, they understand more facts. And if you're racing to make commitments, you're like, "Oh yeah, I'm going to determine what I'm going to do when I know as little as possible." It's not an amazing...

**Caspar Oesterheld:**
Yeah. Daniel Kokotajlo has this post on, I think, LessWrong or the Alignment Forum titled [Commitment Races](https://www.alignmentforum.org/posts/brXr7PJ2W4Na2EW2q/the-commitment-races-problem) or something like that. It's also kind of about this idea that one wants to commit when one knows as little as possible, and that seems kind of problematic.

**Daniel Filan:**
Yeah.

**Caspar Oesterheld:**
Okay. So that's one solution. Another solution might be that I could try to just pay you, offer you $5 or something like that in return for you not blackmailing me. So we make some outside deal, and the idea is that if we do make this deal, then okay, I still have to pay some amount of money, but at least this inefficiency of utility really being burned, it disappears if this deal is made. But then on the level of figuring out what deal to make, we get all the same problems again. I might offer $5 and the other person might say, "Well, actually, you really really owe me those $20, so obviously I'm not going to take this $5, I want at least $18," something like that. And so you get the same problem again.

**Daniel Filan:**
Yeah, it's funny... Yeah, I find this a weird thing about game theory where intuitively, talking out problems seems like an easier way to solve stuff, but whenever you try to model bargaining, I don't know, it's kind of horrible. I've never seen any convincing analysis of it.

**Caspar Oesterheld:**
Yeah.

**Daniel Filan:**
Just very strange.

**Caspar Oesterheld:**
And I do think that this problem, it's very fundamental - this equilibrium selection, what's the appropriate way of distributing resources? There are lots of approaches and they're all good, but I do think that fundamentally, this is just a problem that is hard to get rid of entirely.

### How safe Pareto improvements work <a name="how-spis-work"></a>

**Caspar Oesterheld:**
But now, okay, now comes the 'safe Pareto improvements' or 'surrogate goals' idea for making progress on this. Remember, I'm deploying my AI system. I could do the following. So let's say that by default, the way this delegation works is that I'm going to tell my AI everything that I want it to do. I'm telling it like, "Okay, here's my money and I'm such and such risk averse. I really want to make sure that I always have at least such and such amount in my bank account in case something happens." And also, maybe I can tell it that there's some stamp that I really like and if this stamp appears on eBay for a price of less than $30, it should try to get it, and these kind of things.

So normally, I just honestly tell it my preferences and then I say "do your best". Now, what I could do instead, for the purpose of this particular interaction, is the following. I first set up a dummy bank account that I don't care about at all. So I set up some new online banking account similar to the online banking account that the other person might threaten to report. And I don't care about this at all, but I tell my AI system to care about this bank account as much as I care about the actual online banking account. So I tell it, "Okay, if this were to be reported, that would be just as bad as if the other one is being reported." And I have to do that in a way that's credible, so that's important here. The other person needs to see that I'm doing this.

So let's say that I do this. And let's say that, in addition, I tell my AI to not give in to threats against my actual original banking account. Now, why is this an appealing idea? Basically, the idea is that from the perspective of the person who's thinking about threatening to report my banking account, nothing really has changed. Right? They can still threaten me, and they can still be equally successful at threatening me, because they have to threaten to report this different account now. But to my AI, that's just the same as it would've been by default. It's like to them, nothing's really different. They don't feel like I'm tricking them or anything. They're completely fine with this happening.

But meanwhile, for me, there's some chance that things improve relative to the default. In particular, they might still make this threat again now, to report this new dummy account. And it might be that my AI just gives in to that threat, right? In which case I think, "okay, this is kind of funny, but okay, that was part of the plan". But it could also be that my AI resists, decides not to give in to this new kind of threat. Probably that's just as likely as it would've been to not give into the original kind of threat. And in this case, if this happens, if the other person threatens and my AI doesn't give into the threat, then I am better off than I would've been by default, because now, they're going to report this dummy account that I don't actually care about. So I'm just fine. It's just as if no threat had been made. Of course, for my AI, it might still be very sad. So my AI might still think, "Oh, I've done a terrible job. I was instructed to protect this dummy account and now it's been reported. I'm a bad Bing," or something. But to me, it's better than it would've been by default. And again, to the other person, it's kind of just the same.

And that's kind of what safe Pareto improvements mean in general. It's this idea of making some kind of commitment or some modification of the utility functions, or some kind of way of transforming a game that ensures that everyone is at least as well off as they would've been if the game had been played in the default way. But under some potential outcomes, there's some Pareto improvements. So some person is better off, or everyone's better off, without making anyone worse off. One important part is that it's agnostic about how this equilibrium selection problem is resolved. To make this commitment, or to tell my AI to do this, I don't need to think about how the equilibrium selection problem in the underlying game is going to be resolved. I can make this safely without having to make any guesses about this. And the other player, similarly, they don't need to rely on any kind of guess about how it's going to be resolved.

**Daniel Filan:**
Gotcha. So, actually, okay, I have an initial clarifying question where I think I know the answer. You call it a safe Pareto improvement. What's an unsafe Pareto improvement?

**Caspar Oesterheld:**
Yeah, good question. So the safety part is supposed to be this aspect that it doesn't rely on guesses as to how the equilibrium selection stuff is going to work out. So an unsafe Pareto improvement might be something like, I'm transferring you $10 so that we don't play this game, or something like that. Which is unsafe in the sense that it's hard to tell how we would've played the game, and I actually don't know whether it's an improvement, or we don't know whether it's a Pareto improvement to do this deal. It relies on specific estimates about how we would've played this game. So yeah, that's what the term 'safe' is supposed to mean. Maybe it's not the optimal term, but that's what it's supposed to mean.

**Daniel Filan:**
Gotcha. So one thing it seems like you care about is, these instructions I'm supposed to give to my AI, they're not just supposed to make my life safely better off, they're also supposed to make the other guy's life safely better off. Why do I care about that? Shouldn't it all be about me?

**Caspar Oesterheld:**
So, yeah, that's a good question. So that's kind of coming from this intuition that these kind of races to commit first can't be won. So that if I tried to come up with some scheme that commits my AI in such a way that it screws you over, then maybe you should have already committed to punishing me if I implement that scheme. Or you have some reason to try to commit as fast as possible to punish me if I try to commit that scheme, or to ignore if I try to commit this scheme. So there are all these issues, and the idea is to avoid all of this by having something that's fine for both players so that no one minds this being implemented, everyone's happy for this to be implemented. And so all of these competitive dynamics that otherwise are an obstacle to implementing "I just commit first" approaches, they disappear.

**Daniel Filan:**
Gotcha. So if I kind of think about this scheme, it seems like the suggested plan, roughly, is: I have this really smart AI that knows a bunch of more things than me. In the real world, I don't know exactly what it's going to do or what plans it could potentially think of. And the safe Pareto improvement literature basically instructs me to think of a way that I can deliberately misalign my AI with my preferences, right?

**Caspar Oesterheld:**
Yeah.

**Daniel Filan:**
It seems like this could go wrong easily, right? Especially because in the safe Pareto improvements paper, you're assuming that the principal, the person who's doing the delegating of the game-playing, knows the game. But in real life, that might not be true. So how applicable do you think this is in real life?

**Caspar Oesterheld:**
Yeah. In real life, one will have to do things that are much more complicated, and I think the real life surrogate goals will be much more meta. In this scheme with this very simple AI and this blackmail setting, the game is very simple. There's binary choices, and so on. And also, in this scheme, maybe my AI doesn't really know what's going on. It might not understand why I'm giving it these instructions, it might just be confused, like, "Okay, well I guess this is what I'm supposed to do." I think the more realistic way to implement this is to give some kind of meta instruction to "adopt a surrogate goal in the way I would've liked you to do", or something like that.

**Daniel Filan:**
So somehow delegate to the machine... Not only is it finding equilibria, it's also trying to figure out what the SPI would be, and...

**Caspar Oesterheld:**
Yeah. There are different aspects that one can delegate. Maybe one slightly more complex setting is a setting where, very roughly, it's clear what is going to happen. Maybe very roughly, it's clear that I'm going to deploy an AI and you can make some kind of threat against it or try to blackmail it in some ways, but it's not clear to me how costly it is for you to make different kinds of threats. And then basically, I would have to... Implementing the surrogate goal requires knowing these things, right? Because I have to make the new thing - the new target - I have to make it somehow equivalent to the old one. And this is the kind of thing that one probably should delegate to the AI system in the real world.

**Daniel Filan:**
Gotcha.

**Caspar Oesterheld:**
A more radical approach is to just give it some entirely generic instruction that just says, like, "Okay, whenever you are in any kind of strategic scenario, where I might have no idea what that scenario will be, whenever you face any kind of strategic scenario, first think about safe Pareto improvements, and potentially implement such an improvement, if it exists."

### Will players fight over which safe Pareto improvement to adopt? <a name="spi-fights"></a>

**Daniel Filan:**
So this kind of gets to a question where... I mean, one of the things that safe Pareto improvements were supposed to do is deal with multiple equilibria and maybe people picking different equilibria. It seems like there are potentially tons of conflicting - or different - safe Pareto improvements, right?

**Caspar Oesterheld:**
Yeah.

**Daniel Filan:**
In fact, because they have such a larger action space, I would guess that there'd be even more equilibria in the "find a safe Pareto improvement" game. So are we getting very much, if it's just really hard to coordinate on a good SPI?

**Caspar Oesterheld:**
Yeah. Also a very important question. So I think maybe first, it's good to get an intuition for why these many safe Pareto improvements might exist. Because I think in the example that I gave, there actually is only one, because only one player can commit. And I think, okay, depending on what else exists in that world, there might exist only one. But yeah, let's maybe give an example that makes clear why there might be many. So in the paper, we have this example of the demand game, which is just a game where there's some bit of territory and two countries can try to send their military to take that bit of territory, but if they both send out their military, then there's a military conflict over the territory. So that's kind of the base game. And then the idea is that they could try to jointly commit to...

Okay, sorry, one step back, let's also assume that the two countries make this decision by delegating to some commission or some expert who is thinking about what the appropriate equilibrium is. And then the idea there is that they could instruct the commission to adopt some new attitudes towards this game. So they would say, "Okay, never mind the military, let's just send someone with a flag." Like, "Let's just decide whether to send someone with a flag who just puts the flag in the ground and says, 'This is now ours.'" And then we just tell them, "Well, okay, if both of our countries send someone with a flag to that territory and put in the flag, that's really, really bad. That's just as bad as war." And so this is a safe Pareto improvement in this situation. Is this setting somewhat clear? I guess you've looked at the paper so maybe to you it's...

**Daniel Filan:**
Yeah, roughly, the players can either send in the military or they can just send a guy with a flag or they can send nothing. And if there's clashes, that's bad. But in real life, clashes are worse if there's military. And if just one of the players does it, then the player who sends the most stuff gets the land and they want to have the land. It seems like that's roughly the situation.

**Caspar Oesterheld:**
Yeah. Thanks. So the safe Pareto improvement is to not send the military, which is what one would normally do, just send the guy with the flag. And then one avoids this conflict outcome where both send the military. And here, you could imagine that in some sense, it's kind of ambiguous what to do instead of this conflict outcome. Right?

Because currently...okay, we have this "guy with a flag" story. What exactly happens with the territory if both countries send a guy with a flag? It's kind of just left open. I think the paper just specifies that in that case it's split, or something like that. But it could be that if both players send someone with a flag, then just country A gets the territory. And then it's still a safe Pareto improvement because it might still be better to have just country A get the territory than to have a war over the territory, because war is not so great.

So here, there are these many safe Pareto improvements that are characterized by what happens instead of war. Like, instead of war, does one player just get the resource, or do both players... Do they split it or does the other player get the resource? Something like that.

Okay, so now the question is: does this mean that we get the same problem one level up? Or it's just as bad, or maybe it just doesn't help. And I think this depends a lot on the setting. I think in some settings, safe Pareto improvements really literally do nothing. They don't help at all, because of this. And in other settings, they still help. And roughly, the settings where it helps are ones where the bad outcome that we're replacing with something else is just worse for both players than anything on the Pareto frontier.

For example, let's imagine that in this demand game setting where there's this territory that two countries are having a dispute over, war is worse for both players than it is to even just not get the territory in the first place. So in that case, even the worst safe Pareto improvement that you can get, namely the one where instead of war the other person just gets the resource, is still an improvement. It's still a Pareto improvement, it's still an improvement for both players. And so in particular, if you're mostly just worried about war, and maybe it's not that terrible for you to give up this territory, then this is... You might say, okay, when we meet to decide which safe Pareto improvements to get, you're kind of willing to just settle for the one that's worst for you, and it's still a gain overall.

This other example, this surrogate goal example, is another case of this, where the worst outcome for me in that case, is the one where a threat is being carried out and my bank account is being reported. And so even if we somehow make it so that in the case where my dummy account is reported, I still give the $20, which only works if the other player also makes some kind of commitment, then this is still an improvement for me. Right? So I might still be okay with just giving up the $20 in that case. The important condition, I think, is that the bad outcome that we're getting rid of is worse than anything on the Pareto frontier. Such that even if I get the worst thing on the Pareto frontier, it's still good for me.

**Daniel Filan:**
But even then, the players have to agree which thing on the Pareto frontier they go for, right?

**Caspar Oesterheld:**
Yeah.

**Daniel Filan:**
And yeah, you have this argument in your paper that I wasn't totally compelled by. So my recollection was, you basically said, "Well, most of the players can just say, 'Look, if any other player recommends a safe Pareto improvement, I'll go for that one.' And we just need one person to actually think of something." But then it's like, well, who actually submits the safe Pareto improvement? Or what if multiple people do? Or what if I say... You can imagine, I say, "If someone else submits a safe Pareto improvement, then I'll go for it. But if they don't, I want this one." And you have a similar instruction but you have a different fallback SPI... it still seems quite difficult to figure out how to break that tie.

**Caspar Oesterheld:**
I mean, I'm not sure I exactly understand. Let's take the case where I'm happy to just implement the worst possible safe Pareto improvement, where we replace the conflict outcome with me giving you the territory. If it's a two-player game and I have this attitude and I say, "Okay, you can have the territory, in that case." And let's say I'm really happy, so I state this in the beginning of the negotiations, "I'm actually happy if you just take it." Right? Then is there any remaining problem in that case?

**Daniel Filan:**
Well, I don't know what... I don't know, maybe it's a happier problem, but suppose we both come to the table and we both say, "Hey, I'm happy with any Pareto improvement over the worst one."

**Caspar Oesterheld:**
Ah.

**Daniel Filan:**
Well, we still have to figure out what we get. Right?

**Caspar Oesterheld:**
Right.

**Daniel Filan:**
And still, on this level, it seems like you might want to try and threaten to get your preferred SPI, otherwise... "If you don't agree to my SPI rather than your SPI, then screw it, we're going to war."

**Caspar Oesterheld:**
But okay, so if both players are kind of happy to give the other person their favorite SPI, and it's just the issue that, I don't know, there's a large set of different things that both people would be okay with, or...

**Daniel Filan:**
Yeah. And I have SPIs that I'd prefer, right? And you have SPIs that you'd prefer, and they might not be the same ones.

**Caspar Oesterheld:**
Right. Yeah. To me, this seems like a much easier problem than equilibrium selection.

**Daniel Filan:**
Yeah.

**Caspar Oesterheld:**
Because this is just the case where players are, basically... it's almost this like, "No, you go first," "You go first," type problem.

**Daniel Filan:**
I mean, to me, it sounds like a bargaining problem, right?

**Caspar Oesterheld:**
But it's only... It doesn't seem like a bargaining problem anymore, once it's clear that the players have overlap. Right? It's a bargaining problem for as long as it's unclear which SPI to go for. And I mean, yeah, if you really want to max out, you want to get the best safe Pareto improvements for you, and you also want to get the best safe Pareto improvement for you, and we both go to the table saying, "Okay, I actually really want this," and you say you really want this. Okay, then there's a risk. But if my argument is that even by going for the... You can just say, "Okay, you can have whatever you want in terms of safe Pareto improvements," and even this attitude already improves things.

**Daniel Filan:**
Yeah. I guess... maybe I shouldn't belabor the point, but it still seems almost identical to these bargaining scenarios where there's this [best alternative to negotiated agreement](https://en.wikipedia.org/wiki/Best_alternative_to_a_negotiated_agreement), and we can all do better, but there are a few different mutually incompatible options to do better, and we have to figure out which one we want. And I ideally would get the one that's best for me, and you ideally would get the one that's best for you. That seems like the situation we're in.

**Caspar Oesterheld:**
Okay, let's say we have some other bargaining scenario, where the two of us start a startup together, and we're both needed for the startup, and so we have to come to an agreement on how to split the shares for the startup. Right? So this is a very typical bargaining problem. We have to figure out how much to demand, or what fraction of the startup to demand. But now, if I come to this negotiation table and just say, "I'll accept whatever demands you want as long as it's better for me to do the startup than to not do the startup."

So let's say that as long as I get 20% of the startup, it's still better for me in terms of how wealthy I'm going to be in five years, or whatever, to be part of the startup than to not be. And let's say that this is common knowledge. Then I might say, "Okay, as long as I get at least 20%, I'm fine." It seems that if for some reason, I'm happy to adopt this attitude, then I would think this bargaining problem becomes very easy. Even, you might say, theoretically, it could be that you have a similar attitude and you say, "Well, actually for me, I'm also happy with just getting my minimum of 45% or whatever." And okay, then we have this problem where we have the remaining 35% to distribute, but...

**Daniel Filan:**
I mean, in some sense, it's easy, but also, agents like this will get arbitrarily... The amount you'll improve off the base game will be arbitrarily small if you're like this and the other player's like, "I would like everything as much as I can."

**Caspar Oesterheld:**
Yeah, so in this type of bargaining setup, that's true. So in this type of bargaining setup, this attitude of just "I'm happy to accept the minimum," is bad. You shouldn't adopt this attitude. You should try to get more, right? You should try to make some demands, and thus risk that conflicting demands are made. Because yeah, if you just make the minimum demands, you never make any money beyond what you could have made without the startup, right? In some sense, you don't gain anything from this whole startup, because you only demand the absolute minimum that makes it worth it for you to do the startup. The point that I'm making is that in the case of safe Pareto improvements, even this kind of minimalist, this really dovish bargaining approach to deciding which SPI to go for is still much better, potentially, than not doing anything.

**Daniel Filan:**
So the idea is that just all of the options are just significantly better than the BATNA, basically.

**Caspar Oesterheld:**
Yeah, so this is specifically if the outcome that you replace is some really bad thing, like going to war with each other, or you can think of even more ghastly things if you want. Then anything, even giving up the territory, or maybe giving up everything or something like that, it might still be better than this conflict outcome.

### Relationship to program equilibrium <a name="rel-to-prog-eq"></a>

**Daniel Filan:**
Got you. So there's a bunch of other things to talk about here. So one thing that I was thinking about when I was reading this paper is: it seems sort of analogous to the program equilibrium literature, right? So you write a computer program to play a game, I write a computer program to play a game, but our programs can read each other's source code, right? And the [initial papers](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=e1a060cda74e0e3493d0d81901a5a796158c8410) in this literature, they considered computer programs that just checked "if our programs are literally equal, then they cooperate, otherwise they defect" or something.

And then I think in advance of this... In this literature that came out of [MIRI](https://intelligence.org/), I believe, the Machine Intelligence Research Institute, was to think about this thing they called [modal combat](http://intelligence.org/files/lob-notes-IAFF.pdf), where I try and prove properties about your program and you try and prove properties about my program. Then through the magic of [Löb's Theorem](https://en.wikipedia.org/wiki/L%C3%B6b%27s_theorem), it turns out that we can cooperate [even if we can only search through proofs of however long](https://arxiv.org/abs/1602.04184).

I'm wondering... So in the SPI paper, the paper kind of explicitly envisions [that] you send a particular SPI to your decision-making committee, and it says if the other person sent the exact same thing, then implement it, otherwise, fall back on your default. And I'm wondering, can we make this modal combat step where... Sorry, it's called... Actually, I'm not even going to explain [why it's called modal combat](https://en.wikipedia.org/wiki/Mortal_Kombat). People can Google that if they want. But is there some way to go a little bit more meta or abstract?

**Caspar Oesterheld:**
So yeah, one can use these kinds of mechanisms, like the modal combats, the Löbian FairBot that the researchers at MIRI propose for establishing cooperative equilibrium in this program setting. I mean, we can use that to achieve these safe Pareto improvements.

So the original safe Pareto improvement that I described, the first one that I described, the surrogate goal idea where only I modify my AI and you don't, right? That doesn't really require this, but if you have this case where two players each have to give some new instructions to their AI or their committee that is deciding whether to send troops to the territory or something like that, in that case, usually the safe Pareto improvements have to be backed up by some joint commitment. So both sides, they have to commit that, I don't know, we are going to tell our committee to send the guy with a flag rather than the troops, but we only do this conditional on the other side making an analogous commitment.

And yeah, one way to back this up is to have this kind of program equilibrium-type setup where both countries, they write some contract or something like that for their commission. And the contracts, they are computer programs that look at the other country's contract. And depending on what that contract says, it gives different instructions for how to reason about the guy with the flag versus the troops.

And this contract, if you think of it as literally a computer program, as seems reasonable in the AI case, then yeah, you could use these Löbian ideas for implementing this joint commitment. So you could say... Yeah, I'm not sure how much to go into the details of the Löbian FairBot, where you can show... If I can prove that the other side adopts their side of the safe Pareto improvements, then I adopt my side of the safe Pareto improvements. Otherwise, I just give the default instructions. And then if both sides make this commitment, then it will result in both giving the safe Pareto improvement instructions to their committees. Is that what you had in mind, or-

**Daniel Filan:**
Yeah, that sort of thing. I guess there's a difficulty... I mean, you might hope that you would not have to specify exactly what the SPI has to end up being, but I guess the trouble is precisely because you're assuming you don't know how the committee solves for the equilibrium. Presumably, your program can't try and prove things about what the other solver is going to go for. Because if you could do that, then you could just say, "Go for this nice outcome," or something.

**Caspar Oesterheld:**
So there are two obstacles, I guess. The first is that potentially you can't predict what the other committee is going to do or how it's going to resolve the equilibrium selection problem. But the other is also that you don't want to know, in some sense, right? Or you don't want to adopt a policy of first predicting what the other committee does and then doing whatever is best against that, right? Because then the other committee can just say, "Well, this is what's going to happen. We're just going to demand that we're going to send the troops." Best response to sending the troops is not to send troops or the guy with the flag.

### Do safe Pareto improvements break themselves? <a name="spis-unstable"></a>

**Daniel Filan:**
Got you. I guess [there's] a bunch of things I could ask, but the final thing I wanted to ask about here was... So there's a critique of this line of research, I think this [LessWrong post by Vojta Kovarik](https://www.alignmentforum.org/posts/K4FrKRTrmyxrw5Dip/formalizing-objections-against-surrogate-goals), where one of the things mentioned is: it seems that implicit in the paper is this idea that the way the committee solves games is the same with or without the safe Pareto improvements potentially existing, and all that the safe Pareto improvements do is just change which game the equilibrium selection mechanism plays.

But you could imagine, well, if I know I'm in a world that works like committees giving instructions to people, you could imagine that this potentially does change how people make decisions, and that potentially seriously limits... You could imagine that this limits the applicability of this research, and I'm wondering, how serious a limitation do you think this is?

**Caspar Oesterheld:**
Yeah, so I do think that this is a limitation. This is something that makes it non-applicable in some cases. I mean, there's even just the more basic worry that you might... For example, in this first AI case that I described, you might... Never mind influencing the way my AI reasons about games, right? I might just tell it, "Okay, actually, secretly, don't give into threats against this dummy bank account," right? If I can secretly say this to the AI, then already there's a problem. So we have to assume that that's not possible. And then there's the fuzzier problem that my AI's bargaining strategy can't depend in some sense on the existence of safe Pareto improvements. I think in some settings, this really is just a problem that makes this very difficult.

So here's an example where I think it's clear that it's a big problem. Let's imagine that I am delegating to AI, but it's kind of unclear which AI I'm going to delegate to. I can decide which AI to delegate to, and there are 10 different AIs on the market that I can delegate my finances to or something like that. And now I can decide which of them to hire to take care of my finances. And if I know that safe Pareto improvements will be used, I have some reason to hire an AI that's more hawkish, that's less likely to give into threats, because I think that it's more likely that threats are going to be made against the surrogate goal.

So in response, the threatener might think, "Okay, if I go along with this whole surrogate goal idea, then there's a good chance that I'm going to be screwed over," and so they should just basically ignore the whole surrogate goal stuff and say like, "Okay, sorry, I don't want to do the surrogate goal business, because I don't know what AI you would've rented by default, and so I can't really judge whether I'm being screwed over here." So it definitely can be a problem in this setting.

Meanwhile, I think maybe in other cases, it is clear what the default would be. So for example, it might be clear that I generally, in cases where safe Pareto improvements don't apply for other reasons - for example, because no credible commitment is possible or something like that - I might always delegate my choices to a particular AI. And then in cases where I want to apply my safe Pareto improvements or my surrogate goals or whatever, it seems clear that if I just use the same AI as I use in other cases, then in some sense, my choice of which bargaining strategy is deployed is not being influenced by the existence of safe Pareto improvements.

At least one way in which this can work is that you have access to the ground truth of what people would do without safe Pareto improvements. For example, by being able to observe what people intend to do in scenarios without safe Pareto improvements.

## Similarity-based Cooperation <a name="sbc"></a>

**Daniel Filan:**
Got you. Okay. So the last paper I'd like to chat about is this [paper on similarity-based cooperation](https://arxiv.org/abs/2211.14468). So this is co-authored by yourself, Johannes Treutlein, Roger Grosse, Vincent Conitzer and Jakob Foerster. So can you give us a sense of what's this paper about?

**Caspar Oesterheld:**
Sure. I guess in some sense, you've already set it up very well with some of this open source game theory or program equilibrium stuff that you talked about earlier. So yeah, that's the setting where two players, they each write some source code and then the programs get access to each other's source code and they choose an action from a given game.

So for example, in the prisoner's dilemma, you would submit a computer program that takes the opponent's computer program as input and then outputs cooperate or defect, and it has been shown by [this literature](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=e1a060cda74e0e3493d0d81901a5a796158c8410) that this kind of setup allows for new cooperative equilibria. And the simplest one is just "cooperate if the opponent is equal to this program, otherwise, defect".

So similarity-based cooperation... This paper considers a setting that is in some sense similar. So we again have two players that submit some kind of program or policy or something like that, and that gets some information about the opponent policy or program. The main difference is that we imagine that you only get fairly specific information about the opponent. You don't get to see their entire source code. You only get a signal about how similar they are to you. So in most of the settings that we consider in the paper, one just gets to observe a single number that describes how similar the two policies are. And the policies or programs essentially just get as input a single number and then they output, maybe stochastically, whether to cooperate or defect or what to do in the base game.

One can show that in this setting, still one can get cooperative equilibria. In some sense, it's not too surprising, right, because the cooperative equilibrium in this program equilibrium case that we discussed, this "if the opponent is equal to this program, then corporate, otherwise, defect"... In some sense, that is a similarity-based program: "if the similarity is 100%, then corporate, otherwise, defect". But as we show in the paper, there are more interesting things that can happen, less rigid ways of cooperating, and you can apply this to other games and so on.

**Daniel Filan:**
Yeah. And in particular there's a strange theorem where... so you basically have this noisy observation of a similarity function (or a difference function, I guess is the way you frame it). But I think there's this theorem that says if you don't put any constraints on the difference function, then you can get, I think it was something like every outcome that's better than mini-max payoff can be realized or something. Which can be worse than Nash equilibrium, right?

**Caspar Oesterheld:**
Yeah, that can be worse than all Nash equilibria of the game

**Daniel Filan:**
So I guess somehow, at least in some classes of games, it's better if the thing you're observing is actual similarity rather than arbitrary things.

**Caspar Oesterheld:**
Yes. So that's this folk theorem result which says which outcomes can occur in equilibrium. Yeah, it's surprisingly exactly the same as in program equilibrium if you don't constrain the diff function. The way in which these weird equilibria are obtained is very non-natural. It requires that the diff function, for example, is completely asymmetric in symmetric games and things like that. To avoid this one needs natural diff functions or natural ways of observing how similar one is, in some sense of natural. We have some results in the paper also about... In some sense, it says the opposite: that says if you're under certain conditions, that admittedly are quite strong on the game and on the way that the similarity is observed, you don't get this folk theorem, you get a much more kind of restricted set of equilibria.

**Daniel Filan:**
Yeah. It seemed like you had relatively weak criteria. So that theorem roughly says under some criterion on the difference function that struck me as relatively weak, but on a symmetric game, then you get, as long as there are non-Pareto-dominated Nash equilibria, then they have equal payoffs and there must be the best payoff. So it seems like, I guess you have this restriction that it's a symmetric game and also that the Nash equilibria are non-Pareto-dominated. Or I guess there must be... hang on. There always has to be some non-Pareto-dominated Nash equilibria, right?

**Caspar Oesterheld:**
Yeah, with some weird analysis caveats, right? The set of equilibria might be some open set, something something. But it's probably not so reasonable to get into the details of that. But yeah, I think it's reasonable to assume that there always is a Nash equilibrium that's not Pareto-dominated by another Nash equilibrium. Yeah, the paper says that if you have any such Nash equilibrium, it has to be symmetric, so it gives both players the same payoff.

**Daniel Filan:**
Got you. So this is an interesting paper. In some ways it's interesting that it somehow gets you the good qualities you wanted out of program equilibrium by getting these nice cooperative outcomes in symmetric games while providing less information. And at least in this restricted setting, you get less information than the full program and you get better results. Yeah. I wonder if this is just one of these things where more options in game theory can hurt you. Because on some level it's a little bit surprising, right?

**Caspar Oesterheld:**
Yeah, I think it is an illustration of that. By being able to fully observe each other, you get all of these different equilibria including weird, asymmetric bad ones. So being able to fully observe each other's source code, yeah, in some sense it makes things worse because there's much more to choose from. Whereas under certain conditions, I mean, as far as our paper goes, you avoid this problem if you have the more limited option of just accessing how similar you are to the opponent.

### Are similarity-based cooperators overly cliqueish? <a name="sbc-cliques"></a>

**Daniel Filan:**
Yeah. So speaking of restrictions on the difference function, one thing that struck me is that in real life, "cooperate with people who are very similar to you and otherwise defect" is not an outcome that we aspire to. And I'm wondering: does it work - it seems like you want something where people cooperate just as long as they're just similar enough to cooperate, even if they disagree about what's fair in some sub-game that we're not actually going to get into or something. You want minimal agreement to be able to get cooperation. And I am wondering: how does this fit in with this setting?

**Caspar Oesterheld:**
Yeah, it's a good question. I think an important thing to clarify about how this would work, I think the important thing is that to get this cooperative equilibrium, one really needs a signal of how similar one is with respect to playing this game that one is currently playing, or with respect to how cooperatively one approaches the game that one is currently playing.

And in particular, all other kinds of signals about similarity are completely useless, right? If we play a game and we get a signal about, I don't know, whether we have the same hair color or something like that, that's completely useless for how we should play the game, presumably, unless the game is about some specific thing that relates to our hair color. But that signal is useless. And also, probably if you get a super broad signal that just says, "Well, in general, you're kind of pretty similar," it's probably not even sufficient to get the cooperative equilibria because... Okay, I mean, if the signal says you're exact copies, then that's sufficient. But if the signal says you're 99% the same and there's just 1% that you are kind of different, 1% of your source code is different or something like that, well, it might be that this 1% is exactly the part that matters, the part that decides whether to cooperate or defect in this game. So yeah, what really matters is this strategic similarity.

**Daniel Filan:**
Yeah, I think there's an in-between zone though, where suppose I say, "Hey, I'm going to cooperate with people who cooperate with me. But if we don't reach a cooperative equilibrium, I'm going to defect in this one way." Suppose you say, "Oh, I'm going to cooperate with people who are willing to cooperate with me. But if we don't manage to cooperate, I'm going to have this different method of dealing with the breakdown case." Now intuitively, you'd hope that there'd be some good similarity metric that we could observe where this counts as similar and we end up cooperating somehow. I'm wondering does that happen in this formalism?

**Caspar Oesterheld:**
Yeah. Okay, I mean, our formalism generally, it doesn't necessarily restrict the diff functions that much. I mean, it definitely allows diff functions that only depend on what you do against similar players. So the kind of diff function that you're describing is: we say that players are similar if they do similar things, when they observe that they're facing a similar opponent, and otherwise we regard them as different. And I think that would be sufficient for getting cooperative equilibria. In some sense, I think that's kind of the minimum signal that you need.

**Daniel Filan:**
Yeah. I guess in some sense the question is: can we come up with a minimally informative signal that still yields maximal cooperation or something?

**Caspar Oesterheld:**
Yeah.

### Sensitivity to noise <a name="sensitivity-to-noise"></a>

**Daniel Filan:**
All right. So I now have, I guess, some questions about the details of the paper. So one thing that kind of surprised me is: I think in proposition 2 of the paper you're looking at these policies where if our similarity is under this threshold, cooperate, otherwise defect. And the similarity measure agents observe is the absolute value of the difference between the thresholds plus some zero mean random noise (or let's say Gaussian). And in proposition 2 it says that if the noise is mean zero, even if the standard deviation is super tiny, if I read it correctly, it says that the policies are defecting against each other at least half the time.

**Caspar Oesterheld:**
Yeah. And that's under particular payoffs of the Prisoner's Dilemma.

**Daniel Filan:**
That strikes me as rough. That seems pretty... I would've imagined that I could have been able to do better there, especially with arbitrarily tiny noise, right?

**Caspar Oesterheld:**
Yeah, generally the way noise affects what equilibria there are is kind of counterintuitive in the paper or in the setting that we consider. So I mean, there's also another result that's surprisingly positive. So if you have noise that's uniform between zero and some number, X, then in this setting where... Each player submits a threshold, like cooperate below, defect above, and then the diff that they observe is the difference plus noise, let's say, uniform from zero to some number, X.

Regardless of X... You might think like, okay, with higher X means more noise, right? So if there's higher X, you might think the equilibrium must get worse, right? It's like at some high enough X, it kind of stops working. But it turns out that it basically doesn't matter, it's just completely scale invariant. Even if the noise is uniform from 0 to a 100,000 there's still a fully cooperative equilibrium.

**Daniel Filan:**
A fully what? A fully cooperative equilibrium?

**Caspar Oesterheld:**
Yeah, a fully cooperative equilibrium. So that's one where both players cooperate with probability 1 for the diff that is in fact observed.

### Training neural nets to do similarity-based cooperation <a name="sbc-nns"></a>

**Daniel Filan:**
Interesting. So one thing that the paper sort of reminded me of is... or it seemed vaguely reminiscent to me of this algorithm called [LOLA, or Learning with Opponent-Learning Awareness](https://arxiv.org/abs/1709.04326), where basically when you're learning to play a game, you don't only think about how your action gets you reward, but you also think about how your action changes how the opponent learns, which later changes how much reward you get. And the reason that this matters is that you have some experiments of actually just doing similarity-based cooperation or training in a method that's inspired by this with neural networks. So I think the thing you do is you study alternate best response learning, which if I understand correctly is, you train one network to respond well to the other network, then you train that network to respond well to the first network and you just keep on doing this. And basically you find something like: you do your similarity-based cooperation thing and it ends up working well, roughly. Is that a fair summary of what happens?

**Caspar Oesterheld:**
Yeah, though it's a bit more complicated. Okay, so we have this setting where each player submits, let's say, a neural net, and then they observe how similar they are to each other and then they play the game, and then let's grant that this setting has a cooperative equilibrium where they cooperate if they're similar and defect the more dissimilar they are.

So there's this problem still of finding the cooperative equilibrium. So yeah, you don't know what exactly the neural nets are. This is some complex setting where they also need... Cooperation isn't just pressing the cooperate button, it's computing some function, and defecting is also some other function. So you need to do some ML to even find strategies. Even defecting is not so easy. You have to compute some function.

So what we do is indeed this alternating best response training, which is exactly what you described. The thing though is that if one just initializes the nets randomly and then one does the alternating best response training, then one converges to the defect-defect equilibrium. The reason for that, I think... I mean, one never knows, right, with ML. But I think the reason is probably just that this defect-defect equilibrium is much easier to find. And there's this bootstrapping problem in finding the cooperative equilibrium. The reason to learn this similarity-based cooperation scheme is that the other player also plays the similarity based cooperation scheme and you want to be similar to them, right? But if they're now randomly initialized, you just want to defect.

**Daniel Filan:**
Basically you just need to observe some similarity in order to exploit similarity, and by default you just never observe it. Is that roughly right?

**Caspar Oesterheld:**
Well, actually, if you initialize two neural nets randomly, right, if they're large and so on, right, they'll actually be pretty similar to each other, because they just compute some statistical average. The issue is just that it doesn't pay off to become more similar to the other net, because the other net just does random stuff, right? So if you want to do well against the random neural network that doesn't use the similarity value and just does random nonsense, the best thing to do is just to defect.

**Daniel Filan:**
So you have to be similar and reward similarity.

**Caspar Oesterheld:**
Yeah, exactly. So you have to be similar or... I mean, I guess the most important part is that to learn to do the scheme, the other person basically has to already have implemented the scheme to some extent. If they're just doing something else, if they always cooperate or always defect or just do some random nonsense, then there's no reason to adopt the scheme.

It still doesn't hurt to adopt the scheme. If you adopt the scheme, they'll be dissimilar, and so you'll defect against them. So it's just as good as defecting, but it's a complicated scheme, right? You have to learn how to exactly decrease your amount of cooperation with how dissimilar they are, and you need to learn how to cooperate, which in this setting that we study experimentally is actually hard. So they need to set up this complicated structure and there's no pressure towards having this complicated structure if the opponent is just random. So there's no pressure ever towards having this complicated structure.

And I should say that this is very normal in other settings as well. So for example, it's also not so easy to get learners to learn to play [tit-for-tat](https://en.wikipedia.org/wiki/Tit_for_tat). The reason for that is kind of similar: that in the beginning, if your opponent is randomly initialized, mostly you just learn to defect. And if you both start learning to defect, you never learn that you should cooperate and do this tit for tat thing or whatever, because your opponent is just defecting, right? So to get to the better equilibrium of both playing tit for tat or something like that, you somehow need to coordinate to switch from both always defecting to both always doing this tit for tat, which doesn't randomly happen.

**Daniel Filan:**
It's almost reminiscent of the problem of [babbling equilibria](http://www.davidreiley.com/GameTheoryAEA/AEAContEd_I.pdf), right? So, for listeners who might not know: suppose you've got some communication game where agents want to communicate things to each other, there's this problem where initially I can just talk nonsense and it means nothing, and you can just ignore what I'm saying. And that's an equilibrium because if you're not listening, why should I bother to say anything other than nonsense? And if I'm saying nonsense, why should you listen to it? Is that exactly the same or am I just drawing loose associations?

**Caspar Oesterheld:**
No, I think it's basically the same problem. I think in all of these cases, I think fundamentally the problem is that there's some kind of cooperative structure that only pays off if the other player also has the cooperative structure. And so as long as neither player has the cooperative structure, there's never any pressure to get the cooperative structure: the communication protocol or the tit-for-tat or the "cooperate against similar opponent", all of these schemes, there's just no pressure towards adopting them as long as the other player hasn't adopted them. And so you just get stuck in just doing the naive thing.

**Daniel Filan:**
So, in your paper, you have a pre-training method to address this, right?

**Caspar Oesterheld:**
Yeah, so we have a very simple pre-training method. Basically it's just: explicitly train your neural nets to basically cooperate against copies, which if you consider more general games, it's just train them to maximize the payoff that they get if they're faced with a copy while taking the gradient through both copies. So it's like you play the game fully cooperatively in some sense.

And you also train them to do well against randomly-generated opponents, which if you have some prisoner's dilemma-like game, where just basically it just needs to defect against, especially against dissimilar opponents, that's the pre-training method. And basically the result of the pre-training method is that they very roughly do the intuitive thing, that they cooperate at low levels of difference or high levels of similarity, and the more different they are from their opponent, the more they defect.

So it does this intuitive thing, but it doesn't do it in a... in some sense it's unprincipled. In this pre-training process, there's never any explicit reasoning about how to make something an equilibrium or how to make it stable or something like that. It's just naively implementing some way of implementing this kind of function.

So that's the pre-training. And then we do this alternating best response training where then we take two different models that are independently pre-trained in this way and then we face them off against each other. And they typically start out cooperating against each other because they are actually quite similar after applying this pre-training, and then - maybe this is surprising, I don't know how surprising it is - but the more interesting thing I think is that if you then train them with alternating best response training, they converge to something that's at least somewhat cooperative. So basically they find a cooperative equilibrium.

**Daniel Filan:**
Yeah. And this is kind of surprising. So, you might naively think... Well, alternating best response is usually, you hold your opponent fixed and then you're like, "What can I do that's best for me?" And you might think, what can I do that's best for me is just 'defect', right?

**Caspar Oesterheld:**
Yeah, though I mean it is the case that, if your opponent cooperates against similar opponents and defects against dissimilar opponents, it's clear that there's some pressure for us becoming a copy of them, or becoming very similar to them.

**Daniel Filan:**
And you are also taking that gradient?

**Caspar Oesterheld:**
Yeah, one definitely has to take that gradient, otherwise one just learns to defect. I guess the reason why it's not obvious that this works is just that the pre-training, it's so naive. It's such a simple method. It's much simpler than [this opponent-shaping stuff](https://arxiv.org/abs/1709.04326) that you described where you kind of think about, "Okay, I have to make it so that my opponent's gradient is such and such to make sure that I incentivize them to be a copy of me rather than something else." One doesn't do any of that. One just does this very simple crude thing.

And so I think what this does reasonably well to demonstrate is that it's not that hard to find these cooperative equilibria with relatively crude methods.

**Daniel Filan:**
Although I think you said that they didn't cooperate with each other all the time?

**Caspar Oesterheld:**
Yeah. So the cooperation does unfortunately somewhat evaporate throughout this alternating best response training. So they might initially be almost fully cooperative with each other and then you train them to become best response to each other and then they actually learn to be a bit less cooperative.

**Daniel Filan:**
Okay. So, if the only issue was finding good pre-training or finding a good initialization, and then they have this pressure to become more like "cooperate with things similar to me", why wouldn't they cooperate more rather than less over time?

**Caspar Oesterheld:**
That's a good question. So if one had an optimal pre-training scheme that actually finds a correct way of doing the similarity-based cooperation scheme, so one finds a strategy that's actually an equilibrium against itself, for example, and then one trains - and let's say both players do this and maybe they don't find exactly the same equilibrium, but they find some such strategy, and then we train the opponent to be a best response to your policy. Then what they should learn is just to become an exact copy.

And then once you're there, you don't continue. You stop. You're done. You can't improve your payoff anymore. Okay, gradients: if you still take gradient steps, it gets complicated, but if you think of it as really just trying to improve your neural net, you can't improve it anymore if you're in that equilibrium.

Yeah. So if you had this optimal pre-training scheme then alternating best response training would in some sense immediately, on the first step, it would just get stuck in the correct equilibrium.

So why doesn't this happen? There are some reasons, multiple reasons I think. So one is just that our initialization just isn't so good, so I kind of doubt that they are equilibria against each other. I think we tried this at some point to just see what happens if you just pair them up against the literal copy and they also unlearn to cooperate a bit because they just don't implement the kind of correct curve of defecting more as they become more dissimilar.

**Daniel Filan:**
Right. So, if they just don't implement the correct algorithm, you just don't have that pressure to remain being similar and reward similarity?

**Caspar Oesterheld:**
Yeah, you have some complicated pressure. I guess you still don't want to be completely different, but there's all this trickery where you're kind of similar in some ways, but I don't know, your curve goes down a bit more and you exploit that their curve doesn't really go down enough as the diff increases. You still have some pressure towards becoming similar, but it's not enough and it's not exact.

And then I think the other issue is that the alternating best response training... in some sense it can make them more cooperative. So this isn't quite true because it can make them more cooperative by making them more similar. So if I have my optimally pre-trained network and then you train your thing to be a best response to mine, then it becomes more cooperative towards mine and mine will become more cooperative towards it.

But if you look at any individual network, it can't become more... Once it only, let's say, cooperates with 60% probability against exact copies, once it cooperates only with 60% probability or it's not exactly cooperative anymore at a diff value of 0, there's no way to get back from this to cooperating with a 100% chance as long as your opponent isn't still cooperating at a 100%.

So there's a way in which... if you imagine this alternating best response training as somewhat noisy, sometimes it by accident makes people defect a bit more, or something like that. Or sometimes it is just better to defect a bit more because the incentive curves aren't optimal and don't exactly make it an equilibrium to be a copy. As soon as you lose a bit, you're never going to get it back. So as a consequence, usually during the alternating best response training, the loss just goes up and then at some point it kind of stagnates at some value.

**Daniel Filan:**
So, naively, I would've thought: if at a 100% cooperativity... if when you cooperate with really close copies all the time, at that point it's worth me becoming a little bit more similar to you. If I nudge your number down to 99%, I'm surprised that it's so unstable. Or is the idea that it's only stable in some zone and alternating best response can get outside of that zone? It is just weird to me that I can have an incentive to become more similar to you at one point in your parameter space, but if you deviate from that, that incentive goes the other way. I would expect some sort of gradual transition.

**Caspar Oesterheld:**
Okay. So I mean in general it's definitely not exactly clear what happens exactly during the alternating best response training, why it finds these partially cooperative equilibria when originally there aren't any. Why is that? It's not exactly clear why that's the case. I mean, I think the reason why originally they're not cooperative is that... a typical thing is that just their curve is too flat in the beginning in some sense. So they cooperate if they observe a diff value between 0 and 0.1 and they cooperate roughly equally much. And then if you're an exact copy of this, you would want to defect more just so much that the diff value increases from 0 to 0.1. Okay. It's not exactly like there's noise, it's a bit more complicated, but roughly it's something like this.

**Daniel Filan:**
You don't want to be an exact copy at the very least.

**Caspar Oesterheld:**
Yeah, you don't want to be an exact copy. I mean, if the noise is uniform from 0 to 0.1, then maybe you do want to be an exact copy. But yeah, it's somewhat complicated. I think that's the typical way in which they kind of fail in the beginning, is just that they have these too flat curves.

I think another thing is also that they just aren't close enough copies in the beginning. Typically they become closer copies throughout training, I think, if I remember correctly. Then it's unclear why, at least not obvious why the alternating best response training causes the curves to be so that it is an equilibrium. I think part of it is just that they learned to defect maximally much or something like that, as much as they can get away with. Yeah, it's not...at least I'm not aware of a super simple analysis of why the alternating best response training then does find these cooperative equilibria.

**Daniel Filan:**
Okay. So, speaking of things which aren't super simple, I think in this paper in one of the appendices, you do try this fancier method where instead of just doing alternate best response, you try to shape your opponent to your own benefit. And naively, I might think: ah, this is one of my favorite ways of training agents to play games, and you are trying to shape the opponent to make your life better, so I imagine things will work out better for these agents if you do this, right? But does that happen?

**Caspar Oesterheld:**
Yeah, unfortunately it doesn't seem to work very well. So that's this [LOLA method](https://arxiv.org/abs/1709.04326) that you also talked about earlier. I also went into this with this kind of hope, thinking... And in some sense it is supposed to solve this exact kind of problem: it was developed, for example, to learn tit-for-tat in the prisoner's dilemma. But somehow, yes, we tried a bunch to get it to work and we couldn't really. I don't know. There are some results where it kind of works for a bit and then it unlearns to cooperate again and it seems relatively unstable.

**Daniel Filan:**
Is there any simple story of what's going on here? Or is it maybe just weird hyperparameter tuning or weird nonsense of strange nets?

**Caspar Oesterheld:**
So definitely LOLA is very sensitive to hyperparameters. So that's kind of known, that if you take any positive LOLA results and you change the hyperparameters a bit, it's a pretty good chance that it stops working relatively quickly. But I don't have a good intuition for why it doesn't work in this case or even in other cases. I don't have an intuition for why it's so sensitive to the hyperparameters and things like that and, I don't know, why doesn't it always work pretty straightforwardly?

## FOCAL, Caspar's research lab <a name="focal"></a>

**Daniel Filan:**
Fair enough. I might move to some closing questions if that's okay with you. So, first of all: you are the, I think, assistant director or co-director of this FOCAL lab at CMU, right?

**Caspar Oesterheld:**
Mm-hmm (affirmative).

**Daniel Filan:**
Can you tell us a little bit about that?

**Caspar Oesterheld:**
Yeah, so that's the [Foundations of Cooperative AI Lab at Carnegie Mellon University](https://www.cs.cmu.edu/~focal/). So the actual director is Vincent Conitzer, who's also my PhD advisor while I'm still in the final stages, I hope, of my PhD.

So generally it's a lab that's supposed to work on the kinds of topics that we've discussed today. As one might imagine from the name it's part of the CS department. But yeah, we have some more philosophical work as well, for example. Currently we have I think one postdoc and five PhD students

**Daniel Filan:**
Cool.

**Caspar Oesterheld:**
And I think listeners of this podcast who are interested in these kind of topics would be a good fit for this kind of lab. So I don't know if anyone's considering starting a PhD [but] I think it might make sense to check it out.

**Daniel Filan:**
Okay. If someone is in that situation, what do they do in order to get into FOCAL?

**Caspar Oesterheld:**
I mean, a lot of the application processes I think are not that different from general CS PhD application stuff. It's good to have a paper or something like that. Another strategy is to try to work with us before applying. For example, at least in the past few years, I mentored summer research fellows at the [Center on Long-term Risk](https://longtermrisk.org/) and also at [CERI, the Cambridge Existential Risk Initiative](https://www.camxrisk.org/). So I guess that's a way to work with me at least before applying for a PhD, which I think helps, well, if you want to just start working on some of these topics, but maybe also helps with getting in.

**Daniel Filan:**
Sure. So, before we wrap up the show as a whole, is there anything that you wish that I'd asked that I hadn't?

## How the papers all relate <a name="how-papers-relate"></a>

**Caspar Oesterheld:**
Okay, so I guess one kind of question that I expected was that both [the 'Bounded rational inductive agents' paper](https://arxiv.org/abs/2307.05068) and [the 'Similarity-based cooperation' paper](https://arxiv.org/abs/2211.14468), they touch on this kind of decision theory like Newcomb's problem, evidential versus causal decision theory, this whole cluster of topics. And so I was expecting to get the question [of] how these relate, which, yeah, I guess I could say some stuff about. Maybe it's just an excuse to talk even more about various interesting topics.

**Daniel Filan:**
I think listeners will be glad for an excuse to hear you talk more about interesting topics. Yeah, how do they relate?

**Caspar Oesterheld:**
So both of the papers are very explicitly inspired by thinking about these kinds of things. So yeah, I think that one should cooperate in a prisoner's dilemma against a copy, for example. And I think it's kind of unfortunate that there isn't that much of a theoretical foundation for why one should do this in terms of learning, for example; regret minimizers have to learn not to do this, for example.

And so part of the motivation behind 'Bounded rational inductive agents' is to describe a theory that very explicitly allows cooperating against copies as a rational thing to do. So that's somewhat inspired by this.

And then of course with the 'Similarity-based cooperation' paper, in some sense it's even more explicit that it's supposed to be doing that, though it takes this program equilibrium-inspired outside perspective where one doesn't think about what it is rational to do in the prisoner's dilemma against a copy. One thinks about what kind of program it is good to submit in this kind of setting.

And so in some sense one has this question that we can ask from the inside, like: if you are a neural net that was built or learned or whatever to play games well and you find yourself in this scenario - from the inside, from the perspective of this neural net - it's like you face an exact copy and maybe you reason about things by saying, "okay, if I cooperate, then the opponent will cooperate."

**Daniel Filan:**
And both this and [the 'Safe pareto improvements' paper](https://link.springer.com/article/10.1007/s10458-022-09574-6) have this quality of: from the outside, you're making some change to the agent that's actually making the decisions. And you might think that, shouldn't this all just happen internally?

**Caspar Oesterheld:**
Yeah, it is interesting that both of these papers take this outside perspective and make... I agree, one would think that one doesn't need the outside perspective, but at least conceptually, it seems sometimes easier to reason from that outside perspective.

So in particular this program equilibrium framework, in some sense, you can think about this in the general... You can think of program equilibrium or this framework as asking the question, "How should you reason if other people can read your mind and you can read their mind?" And this is really a very hard philosophical question.

And you can avoid all of these questions by taking this outside perspective where you submit a program that gets to read the other program's mind and then you treat this outside perspective just in the normal standard game-theoretic way. I don't know, it's a surprisingly... I don't know, maybe the other problem is surprisingly hard and so this trick is surprisingly successful.

**Daniel Filan:**
Yeah. So, I guess that's one interesting relationship between BRIAs and the similarity-based cooperation: the internal perspective versus the external perspective.

**Caspar Oesterheld:**
Yeah. Yeah.

**Daniel Filan:**
And I guess there's also this thing where with similarity-based cooperation, you're saying: well, if there's this difference function, then what happens? Whereas in the BRIA thing, you have a variety of these experts or these hypotheses. And I guess in some sense you're looking at a wider variety of... Maybe the analogy is you're looking at a wider variety of these similarity functions as well. You're somehow more powerful, I think.

**Caspar Oesterheld:**
Or just more generally looking at strategic interactions in a less constrained way.

## Relationship to functional decision theory <a name="rel-to-fdt"></a>

**Daniel Filan:**
Yeah. There are some interesting questions with these papers, in particular with 'Safe pareto improvements'. I think relatedly, one thing I didn't quite ask, but maybe I'll bring up here: is this just a roundabout way of getting to one of these ["functional decision theories"](https://arxiv.org/abs/1710.05060) where you just choose to be the type of agent that's the best type of agent to be across possible ways the world could be?

**Caspar Oesterheld:**
Yeah, maybe that's the case. I think the trickiness is that the functional decision theory, [updateless decision theory](https://www.alignmentforum.org/tag/updateless-decision-theory), these sorts of things... they're not fully specified, especially in these multi-agent scenarios. It's just unclear what they're supposed to do. I suppose as a functional decision theorist or updateless decision theorist, one might argue that one shouldn't need surrogate goals, because in some sense the goal of all of these theories is to do away with pre-commitment and things like that, and you should come with all the necessary pre-commitments built in.

And so maybe in some sense, an idealized functional decision theory agent should already have automatically these, well, surrogate goals, except you wouldn't call them surrogate goals. You just have these commitments to treat other things in the same way as you would treat threats against the original goal to deflect threats.

**Daniel Filan:**
You just reliably do whatever you wish you had pre-committed to do, and hopefully there's one unique thing that's that.

**Caspar Oesterheld:**
Yeah, in the face of equilibrium selection, it's very unclear what that is supposed to come out as.

## Following Caspar's research <a name="following-caspar"></a>

**Daniel Filan:**
Yeah. Coming up on the end, suppose somebody's listened to this, they're interested, they want to learn more: if they want to follow your research or your output and stuff, how should they do that?

**Caspar Oesterheld:**
There are three things. So I recently made an account on the social media platform X, formerly known as Twitter, and that is [@C\_Oesterheld](https://twitter.com/C_Oesterheld), and I mostly plan to use this for work-related stuff. I don't plan to have, I don't know, random takes on US elections or whatever.

Then I also have a blog at [casperoesterheld.com](https://casparoesterheld.com/), which is also mostly sticking relatively closely to my research interests. And then if you don't want to deal with all this social media stuff, you can also just follow me on [Google Scholar](https://scholar.google.com/citations?user=xeEcRjkAAAAJ&hl=en&oi=ao) and then you just get just the papers.

**Daniel Filan:**
All right. We'll have links to all of those in the description of the episode. Yeah, it's been really nice talking. Thanks so much for taking the time to be on AXRP.

**Caspar Oesterheld:**
Thanks for having me.

**Daniel Filan:**
And for listeners, I hope this was a valuable episode.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with the transcription. The opening and closing themes are also by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://www.patreon.com/axrpodcast) such as  Alexey Malafeev, Ben Weinstein-Raun, and Tor Barstad. To read a transcript of this episode, or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
