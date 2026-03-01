---
layout: post
title: "49 - Caspar Oesterheld on Program Equilibrium"
date: 2026-02-17 17:00 -0800
categories: episode
---

[YouTube link](https://youtu.be/NMEwiZQK_C4)

How does game theory work when everyone is a computer program who can read everyone else's source code? This is the problem of 'program equilibria'. In this episode, I talk with Caspar Oesterheld on work he's done on equilibria of programs that simulate each other, and how robust these equilibria are.

Topics we discuss:
 - [Program equilibrium basics](#program-equilibrium-basics)
 - [Desiderata for program equilibria](#desiderata-pe)
 - [Why program equilibrium matters](#why-pe-matters)
 - [Prior work: reachable equilibria and proof-based approaches](#prior-work)
 - [The basic idea of Robust Program Equilibrium](#rpe-basic-idea)
 - [Are ϵGroundedπBots inefficient?](#are-egpb-inefficient)
 - [Compatibility of proof-based and simulation-based program equilibria](#compatibility-of-equilibria)
 - [Cooperating against CooperateBot, and how to avoid it](#cooperating-against-coopbot)
 - [Making better simulation-based bots](#making-better-sim-based-bots)
 - [Characterizing simulation-based program equilibria](#characterizing-sim-based-pe)
 - [Follow-up work](#follow-up-work)
 - [Following Caspar's research](#following-caspars-research)
 
**Daniel Filan** (00:00:09):
Hello, everybody. In this episode I'll be speaking with Caspar Oesterheld. Caspar is a PhD student at Carnegie Mellon University, where he serves as the Assistant Director of the [Foundations of Cooperative AI Lab](https://www.cs.cmu.edu/~focal/index.html). He researches AI safety with a particular focus on multi-agent issues. There's a transcript of this episode at [axrp.net](https://axrp.net/), and links to papers we discuss are available in the description. You can support the podcast at [patreon.com/axrpodcast](https://patreon.com/axrpodcast), or give me feedback about this episode at [axrp.fyi](axrp.fyi). Okay, well Caspar, welcome to AXRP.

**Caspar Oesterheld** (00:00:43):
Thanks for having me.

## Program equilibrium basics <a name="program-equilibrium-basics"></a>

**Daniel Filan** (00:00:44):
So today we're going to talk about two papers that you've been on. The first is ["Robust program equilibrium"](https://link.springer.com/article/10.1007/s11238-018-9679-3), where I believe you're the sole author. And the second is ["Characterising Simulation-Based Program Equilibria"](https://arxiv.org/abs/2412.14570) by Emery Cooper, yourself and [Vincent Conitzer](https://www.cs.cmu.edu/~conitzer/). So I think before we sort of go into the details of those papers, these both use the terms like "program equilibrium", "program equilibria". What does that mean?

**Caspar Oesterheld** (00:01:11):
Yeah, so this is a concept in game theory and it's about the equilibria of a particular kind of game. So I better describe this kind of game. So imagine you start with any sort of game, in the game theoretic sense, like the prisoner's dilemma, which maybe I should describe briefly. So imagine we have two players and they can choose between raising their own utility by one or raising the other player's utility by three and they only care about their own utility. I don't know, they play against a stranger, and for some reason they don't care about the stranger's utility. And so they both face this choice. And the traditional game-theoretic analysis of this game by itself is that you should just raise your own utility by $1 and then both players will do this and they'll both go home with $1 or one utilon or whatever. And, of course, there's some sort of tragedy. It would be nice if they could somehow agree in this particular game to both give the other player $3 and to both walk home with the $3.

**Daniel Filan** (00:02:33):
Yeah, yeah, yeah. And just to drive home what's going on, if you and I are playing this game, the core issue is no matter what you do, I'm better off giving myself the one utility or the $1 rather than giving you three utility because I don't really care about your utility.

(00:02:53):
So, I guess, there are two ways to put this. Firstly, just no matter what you play, I would rather choose the "give myself utility" option, commonly called "defect", rather than cooperate. Another way to say this issue is, in the version where we both give each other the $3, I'm better off deviating from that. But if we're both in the "only give ourselves $1" situation, neither of us is made better off by deviating and in fact we're both made worse off. So it's a sticky situation.

**Caspar Oesterheld** (00:03:29):
Yeah. That's all correct, of course. Okay. And now this program game set-up imagines that we take some game and now instead of playing it in this direct way where we directly choose between cooperate and defect---raise my utility by $1 or the other player's by $3---instead of choosing this directly, we get to choose computer programs and then the computer programs will choose for us. And importantly, so far this wouldn't really make much of a difference yet. Like, okay, we choose between a computer program that defects or a computer program that cooperates, or the computer program that runs in circles 10 times and then cooperates. That effect doesn't really matter.

(00:04:12):
But the crucial addition is that the programs get access to each other's source code at runtime. So I submit my computer program, you submit your computer program and then my computer program gets as input the code of your computer program. And based on that it can decide whether to cooperate or defect (or you can take any other game [with different actions]). So it can look at your computer program and [see] does it look cooperative? And depending on that, cooperate or defect. Or it can look [at] is the fifth character in your computer program an 'a'? And then cooperate if it is and otherwise defect. There's no reason to submit this type of program, but this is the kind of thing that they would be allowed to do.

**Daniel Filan** (00:04:58):
Yeah. So this very syntactic analysis... A while ago I was part of this, basically a [tournament](https://manifold.markets/IsaacKing/which-240-character-program-wins-th), that did this prisoner's dilemma thing with these open source programs. And one strategy that a lot of people used was, if I see a lot of characters... Like if I see a string where that string alone means "I will cooperate with you", then cooperate with that person, otherwise defect against that person.

(00:05:33):
Which I think if you think about it hard, this doesn't actually quite make sense. But I don't know, there are very syntactic things that, in fact, seem kind of valuable, especially if you're not able to do that much computation on the other person's computer program. Just simple syntactic hacks can be better than nothing, I think.

**Caspar Oesterheld** (00:05:56):
Yeah. Was this [Alex Mennen](https://www.alex.mennen.org/)'s [tournament on LessWrong](https://www.lesswrong.com/posts/QP7Ne4KXKytj4Krkx/prisoner-s-dilemma-tournament-results-0) or was this a different-

**Daniel Filan** (00:06:01):
No, this is [the Manifold one](https://manifold.markets/IsaacKing/which-240-character-program-wins-th).

**Caspar Oesterheld** (00:06:07):
Ah, okay.

**Daniel Filan** (00:06:08):
So you had to write a JavaScript program, it had to be fewer than however many characters and there was also a market on which program would win and you could submit up to three things. So actually, kind of annoyingly to me... One thing I only realized afterwards is the thing you really should have done is write two programs that cooperated with your program and defected against everyone else's, or just cooperated with the program you thought was most likely to win. And then you bet on that program. Or even you could submit three programs, have them all cooperate with a thing that you hoped would win and defect against everyone else and then bet on... Anyway.

(00:06:49):
So in that setting there was a timeout provision where if the code ran for too long your bot would be disqualified, and also you had to write a really short program. Some people actually managed to write pretty smart programs. But if you weren't able to do that, relatively simple syntactic analysis was better than nothing, I think.

**Caspar Oesterheld** (00:07:14):
Yeah, I think there was [this earlier tournament](https://www.lesswrong.com/posts/QP7Ne4KXKytj4Krkx/prisoner-s-dilemma-tournament-results-0) in 2014 or something like that when there was less known about this kind of setting. And a bunch of programs there were also based on these simple syntactic things. But in part because everyone was mostly thinking about these simple syntactic things, it was all a little bit kind of nonsense.

(00:07:34):
I don't know, you would check whether the opponent program has a particular word in it or something like that. And then, I think, the winning program had particular words in it but it would just still defect. So in some sense those dynamics are a little bit nonsense or they're not really tracking, in some sense, the strategic nature of the situation.

**Daniel Filan** (00:08:02):
Fair enough. So going back, you were saying: you have your opponent's program and you can see if the fifth character is an 'a' or, and then-

**Caspar Oesterheld** (00:08:11):
Yeah, what should one perhaps do? So I think the setting was first proposed in, I think, 1984 or something like that. And then it kind of [was] rediscovered or reinvented, I think, three times or something like that in various papers. And all of these initial papers find the following very simple program for this prisoner's dilemma-type situation, which just goes as follows: if the opponent program is equal to myself---to this program---then cooperate and otherwise defect.

(00:08:53):
So this program is a Nash equilibrium against itself and it cooperates against itself. So if both players submit this program, neither is incentivized to deviate from playing this program. If you play this program that checks that the two programs are the same and if they are, cooperate, otherwise defect, you submit this program, the best thing I can do is also submit this program. If I submit anything else, you're going to defect. So I'm going to get at most one if I also defect, whereas I get three if I also cooperate. So yeah, all of these original papers proposing the setting, they all find this program which allows stable cooperation in this setting.

**Daniel Filan** (00:09:38):
Right. So my impression, and maybe this is totally wrong, is I think for a while there's been some sense that if you're rational and you're playing the prisoner's dilemma against yourself, you should be able to cooperate with yourself, I think. Wasn't there some guy writing in Scientific American about [superrationality](https://en.wikipedia.org/wiki/Superrationality) and he held a contest basically on this premise?

**Caspar Oesterheld** (00:10:02):
Yeah, yeah. [Hofstadter](https://en.wikipedia.org/wiki/Douglas_Hofstadter), I think.

**Daniel Filan** (00:10:05):
Right, right.

**Caspar Oesterheld** (00:10:06):
I think also in the '80s or something... I've done a lot of work on this kind of reasoning as well that... I don't know, for humans it's a little bit hard to think about. You don't often face very similar opponents or it's a little bit unclear how similar other people are. Is your brother or someone who's related to you and was brought up in a similar way, are they very similar? It's kind of hard to tell.

(00:10:38):
But for computer programs it's very easy to imagine, of course, that you just... You have two copies of GPT-4 or something like that and they play a game against each other. It's a very normal occurrence, in some sense. I mean, maybe not them acting in the real world, at this point, but having multiple copies of a computer program is quite normal. And there's this related but to some extent independent literature on these sorts of ideas that you should cooperate against copies, basically.

**Daniel Filan** (00:11:10):
But yeah, basically I'm wondering if this idea of 'I'll cooperate against copies" is what inspired these very simple programs?

**Caspar Oesterheld** (00:11:22):
Yeah, that is a good question. I basically don't know to what extent this is the case. I know that some of the later papers on program equilibrium, I remember some of these specifically citing this superrationality concept. But yeah, I don't remember whether these papers---I think McAfee is one of these who wrote about this in the '80s---I don't know whether they discuss superrationality.

**Daniel Filan** (00:11:53):
And it's kind of tricky because... If you actually look at the computer programs, they're not doing expected utility maximization... Or they're not computing expected utility maximization. They're just like, "if identical to me, cooperate, else defect", just hard-coded in... Anyway, maybe this is a distraction but, indeed, these were the first programs considered in the program equilibrium literature.

**Caspar Oesterheld** (00:12:19):
Yeah.

**Daniel Filan** (00:12:20):
So they sound great, right?

**Caspar Oesterheld** (00:12:21):
Yeah. So, I mean, they're great in that in the prisoner's dilemma, you can get an equilibrium in which you can get cooperation, which otherwise you can't, or you can't achieve with various naive other programs that you might write. But, I think, in practice---and it's not so obvious what the practice of this scheme looks like---but if you think of any kind of practical application of this, it's sort of a problem that the settings are somewhat complex and now you need... Two people write programs independently and then these programs need to be the same somehow or they need to... I mean, there are slightly more general versions of these where they check some other syntactic properties.

(00:13:13):
But basically, yeah, you require that you coordinate in some way on a particular kind of source code to write, which maybe in some cases you can do, right? Sometimes maybe we can just talk beforehand. Like if we play this prisoner's dilemma, we can just explicitly say, "Okay, here's the program that I want to submit. Please submit the same program" and then you can say, "Okay, let's go".

(00:13:38):
But maybe in cases where we really write these programs independently, maybe at different points in time, and these programs, especially if they do more complicated things than play this prisoner's dilemma, it's very difficult to coordinate without explicitly talking to each other on writing programs that will cooperate against each other. Even in the prisoner's dilemma, you might imagine that I might have an extra space somewhere, or maybe you write the program, "If the two programs are equal, cooperate, otherwise defect" and I write, "if the two programs are different, defect, else cooperate". These very minor changes would already break these schemes.

## Desiderata for program equilibria <a name="desiderata-pe"></a>

**Daniel Filan** (00:14:20):
Okay, okay. There's a lot to just ask about there. I think my first question is: we have this notion of program equilibrium. Are we trying to find Nash equilibria of programs? Are we trying to find [evolutionarily stable strategies](https://en.wikipedia.org/wiki/Evolutionarily_stable_strategy)? Or maybe there are tons of solution concepts and we just want to play around with the space. But what are the actual... What's the thing here?

**Caspar Oesterheld** (00:14:49):
Yeah. The solution concept that people talk about most is just Nash equilibrium. So if you look at any of these papers and you look at the results, they'll prove "these kinds of programs form a Nash equilibrium of the program game". Or, I mean, the term "program equilibrium" literally just means "Nash equilibrium of the game in which the players submit these programs". That is almost always the kind of game-theoretic solution concept that people use.

(00:15:25):
And then, usually a bunch of other things are a little bit more implicit. It's clear that people are interested in finding good Nash equilibria. In some sense, the whole point of the setup is we start out with the prisoner's dilemma and sad: everyone's going to defect against everyone else and we're not getting to cooperation. And now, we come in with this new idea of submitting programs that get access to each other's source code and with this we get these cooperative equilibria. So that is usually... I mean, it's often quite explicit in the text that you're asking, "can we find good equilibria?" in some sense, ones that are Pareto-optimal in the space of possible outcomes of the game or something like that.

(00:16:15):
And then, additionally, a lot of the work after these early papers that do this syntactic comparison-based program equilibrium are about this kind of intuitive notion of robustness, that you want to have equilibria that aren't sensitive to where the other program puts the spaces and the semicolons and these syntactic details. But it is kind of interesting that this isn't formalized usually. And also, the second paper that we talked about, we presented this at AAAI and one game theorist came to our poster and said... I don't know, to him it was sort strange that there's no formalization, in terms of solution concepts in particular, of this kind of robustness notion, that we'll talk about the programs that we are claiming or that we are arguing are more robust. But this syntactic comparison-based program, there's sort of some intuitive sense, and we can give concrete arguments, but it's not formalized in the solution concept.

(00:17:35):
One of my papers is called "robust program equilibrium", but robust program equilibrium is not actually a solution concept in the sense that Nash equilibrium is or [trembling hand equilibrium](https://en.wikipedia.org/wiki/Trembling_hand_perfect_equilibrium) is. The robustness is more some sort of intuitive notion that, I think, a lot of people find compelling but in some sense it's not formalized.

**Daniel Filan** (00:17:58):
Yeah, and it's funny... I see this as roughly within both the cooperative AI tradition and the agent foundations tradition. And I think these traditions are sort of related to each other. And, in particular, in this setting in decision theory, I think there's also some notion of fairness of a decision situation.

(00:18:24):
So sometimes people talk about: suppose you have a concrete instantiation of a decision theory, meaning a way somebody thinks about making decisions. There are always ways of making that concrete instantiation look bad by saying: suppose you have a Caspar decision theory; we'll call it CDT for short. And then you can be in a decision situation, right, where some really smart person figures out what decision theory you're running, punches you if you're running CDT and then gives you $1 million if you're not.

(00:18:54):
And there's a sense that this is unfair but also it's not totally obvious. Like in that setting as well, I think there's just no notion of what the fair thing is. Which is kind of rough because you'd like to be able to say, "Yeah, my decision theory does really well in all the fair scenarios". And it seems like it would be nice if someone figured out a relevant notion here. Are people trying to do that? Are you trying to do that?

**Caspar Oesterheld** (00:19:22):
So I think there is some thinking in both cases and I think probably the kind of notion that people talk about most is probably similar in both. So in this decision theory case, I think the thing that probably most people agree is that the decision situation should be somehow be a function of your behavior. It shouldn't check, "do you run CDT", and if you do, you get punched in the face. It should be like: if in this situation you choose this, then you get some low reward. But this should somehow be behavior-based, which I think still isn't enough. But, I mean, this sort of goes into the weeds of this literature. Maybe we can link [some paper](https://arxiv.org/abs/2101.00280) in the show notes.

(00:20:17):
But, I mean, the condition that we give in the second paper, or maybe even in both of the papers that we're going to discuss, there's some explicit discussion of this notion of behaviorism, which also says: in the program equilibrium setting, it's sort of nice to have a kind of program that only depends on the other program's behavior rather than the syntax.

(00:20:48):
And all of these approaches to robustness, like trying to do some proofs about the programs, about what the opponent program does, try to prove whether the opponent will cooperate or something like that... All of these, to some extent, these notions that people intuitively find more robust, they're all more behaviorist, at least, than this syntactic comparison-based idea.

**Daniel Filan** (00:21:15):
Yeah. Although it's tricky because... I'm sorry, I don't know if this is going to the weeds that you want to postpone. So this behaviorism-based thing, if you think about the "if you're equal to me, cooperate, else defect" program, this is behaviorally different from the "if you're unequal to me, defect, else cooperate" program, right?

(00:21:33):
It does different things in different situations and therefore... Once you can define an impartial thing, right, then maybe you can say, "Well if you act identically on impartial programs then you count as impartial". But actually maybe that's just a recursive definition and we only need one simple program as a base case.

**Caspar Oesterheld** (00:21:52):
I think we do actually have a recursive definition of simulationist programs that I think is a little bit trying to address some of these issues. But, yeah, it does sort of go into the weeds of what exactly should this definition be.

**Daniel Filan** (00:22:13):
Yeah, okay. Let's go back a little bit to desiderata of program equilibria. So they're computer programs, right? So presumably---and this is addressed a bit in the second paper---but just runtime computational efficiency, that seems like a relevant desideratum.

**Caspar Oesterheld** (00:22:28):
Yes, I agree.

**Daniel Filan** (00:22:29):
And then, I think that I imagine various desiderata to include "have a broad range of programs that you can work well with". And it seems like there might be some notion of just, "if you fail, fail not so badly, rather than fail really badly". I don't know if... this is slightly different from the notion of robustness in your paper and I don't know if there's a good formalism for this. Do you have thoughts here?

**Caspar Oesterheld** (00:23:02):
I mean in some intuitive sense, what one wants is that, if I slightly change my program, maybe even in a way that is sort of substantial... In the prisoner's dilemma, it's a little bit unclear if I defect slightly more, if I don't cooperate 100% but I cooperate 95%, it's unclear to what extent should you be robust. Should you defect against me all of the time? But, I guess, in other games where maybe there are different kinds of cooperation or something like that, you'd want... If I cooperate in slightly the wrong way, the outcome should still be good.

(00:23:46):
I think in some sense there's something here, that I think it's conceptually quite clear that if you deviate in some reasonable harmless way, it should still be fine. We shouldn't defect against each other, we should still get a decent utility. But the details are less clear [about] what exactly are the deviations and it probably depends a lot on the game. And then, there are a lot of these sort of things that in game theory are just kind of unclear. If I defect 5% more, how much should you punish me for that? And so, I think that's why a lot of these things, they aren't really formalized in these papers.

## Why program equilibrium matters <a name="why-pe-matters"></a>

**Daniel Filan** (00:24:35):
Sure, okay. So now that we know what program equilibrium is, why does it matter?

**Caspar Oesterheld** (00:24:43):
There are lots of different possible answers to this question. I think the most straightforward one is that we can view program games like program equilibrium as sort of a model of how games could be played when different parties design and deploy AI systems. So this whole thing of having a source code that the other party can look at and can maybe run or can look at character five and stuff like that: this is something that is somewhat specific to computer programs. We can talk about whether there are human analogs still, but when we play a game against each other, it's sort of hard to imagine an equivalent of this. Maybe I have some vague model of how your brain works or something like that, but there's no source code, I can't really "run" you in some ways.

(00:25:51):
Whereas, if we both write computer programs, this can just literally happen. We can just literally say, "This is the source code that I'm deploying..." I have my charity or something like that and I'm using some AI system to manage how much to donate to different charities. I can just say, "Look, this is the source code that I'm using for managing what this charity does". And here, I think, program equilibrium or program games are quite a literal direct model of how these interactions could go. Of course, you can also deploy the AI system and say "we're not saying anything about how this works". In which case, obviously, you don't get these program equilibrium-type dynamics. But it's a way that they could go and that people might want to use because it allows for cooperation.

(00:26:47):
So I think the most direct interpretation is that it models a kind of way that games could be played in the future when more decisions are made by delegating to AI systems. As people in this community who think and to some extent worry about a future where lots of decisions are made by AI, this is an important thing to think about. And meanwhile, because to most game theorists it's sort of a weird setting because, well, humans can't read each other's source code, it's sort of understudied by our lights, I guess, because currently it's not a super important way that games are played.

**Daniel Filan** (00:27:37):
Which is interesting because... So I guess we don't often have games played with mutual source code transparency, but there really are computer programs that play economic games against each other in economically valuable settings, right? A lot of trading in the stock market is done by computer programs. A lot of bidding for advertisement space is done by computer programs.

(00:28:06):
And algorithmic mechanism design---so mechanism design being sort of inverse game theory: if you want some sort of outcome, how you'd figure out the game to make that happen. Algorithmic mechanism design being like that, but everyone's a computer. There's decent uptake of this, as far as I can tell. Algorithmic game theory, there's decent uptake of that. So I'm kind of surprised that the mutual transparency setting is not more of interest to the broader community.

**Caspar Oesterheld** (00:28:42):
Yeah, I think I agree. I mean, a lot of these settings... So I think the trading case is a case where decisions are made on both sides by algorithms. But usually because it's kind of a zero-sum game, you don't want to reveal to your competitors how your trading bot works.

(00:29:07):
There's a lot of this mechanism design where you have an algorithm. I guess those are usually cases where it's sort of unilateral transparency. I auction off something and I'm saying, "Okay, I'm using this algorithm to determine who gets, I don't know, this broadband frequency or these things that are being auction-offered".

(00:29:33):
So, I guess, those are cases with sort of unilateral transparency. And that is, I guess, studied much more in part because it's less... I mean, this also has been studied traditionally in game theory much more, in some sense. You can view it as some Stackelberg equilibrium. You can view all mechanism design as being a bit like finding Stackelberg equilibria. And I think Stackelberg's analyses of game theory even proceed Nash equilibrium.

**Daniel Filan** (00:30:04):
Interesting.

**Caspar Oesterheld** (00:30:05):
So that is very old.

**Daniel Filan** (00:30:07):
Where Stackelberg equilibrium is: one person does a thing and then the next person does a thing. And so the next person is optimizing, given what the first person does, and the first person has to optimize "what's really good for me, given that when I do something the other person will optimize what's good for them based on what I do".

**Caspar Oesterheld** (00:30:23):
Yeah.

**Daniel Filan** (00:30:24):
So people look at Stackelberg equilibria and these sorts of games and it's a common thing. And it's an interesting point that you can sort of think of it as one-way transparency.

**Caspar Oesterheld** (00:30:34):
Yeah. I think one thing one could think about is how much humans are in these mutual transparency settings. So yeah, I already said for individual humans: if the two of us play a prisoner's dilemma, I have some model of you, I can't really read... So I don't know, seems sort of speculative. So there's [this paper](https://arxiv.org/abs/2208.07006) which I really like by [Andrew Critch](https://acritch.com/), [Michael Dennis](https://www.michaeldennis.ai/) and [Stuart Russell](https://people.eecs.berkeley.edu/~russell/), all from [CHAI](https://humancompatible.ai/) where, of course, you graduated from. This is about program equilibrium as well.

(00:31:16):
The motivating setting that they use is institution design. The idea there is that: institutions, you can view them as rational players, or something like that. They make decisions, and they play games with each other. Like, I don't know, the US government plays a game with the German government or whatever. But institutions have some amount of transparency. They have laws that they need to follow. They have constitutions. They're composed of lots of individuals, that in principle, one could ask... I don't know, the German government could check all the social media profiles of all the people working for the US government and learn something about how these people interact with each other, or something like that. There's some very concrete transparency there.

(00:32:09):
In particular, some things are really just algorithmic type commitments. Like, I don't know, "We don't negotiate with terrorists", or something like that. It's specific, something that's in the source code of a country in some sense. It's specifying how it's going to choose in particular interactions. I think that is a case where interactions between human organizations have this transparency. I think that's some evidence that we could get similar things with AI.

(00:32:51):
At the same time, it's also interesting that this hasn't motivated people to study this program equilibrium-style setting, which I think is probably because I think, as a computer scientist, it's natural to think the constitution is basically just an algorithm. It's also a little bit like, I don't know, computer science people explain the world to everyone else by using computer programs for everyone. Like, "The mind is a program, and the constitution is just a program. We got it covered with our computer science stuff", which maybe some people also don't like so much. But I think it's a helpful metaphor still.

## Prior work: reachable equilibria and proof-based approaches <a name="prior-work"></a>

**Daniel Filan** (00:33:35):
Fair enough. Okay. Some people do study program equilibria. Just to set up the setting for your papers: before the appearance to the world of [Robust Program Equilibrium](https://link.springer.com/article/10.1007/s11238-018-9679-3), what did we know about program equilibria beyond these simple programs that cooperate if your source code is mine?

**Caspar Oesterheld** (00:33:56):
Yeah. I guess we have some characterizations of the kind of equilibria, in general, that are allowed by these syntactic comparison-based programs. Not sure how much to go into that at this point, but yeah, maybe we'll get into this later.

**Daniel Filan** (00:34:16):
I think I can do this quickly. My understanding is basically, any equilibrium that's better off for all the players than unilaterally doing what they want, you can get with program equilibrium. Maybe you have to have punishments as well, but something roughly like this. You can have programs being like, "You have to play this equilibrium. If you don't, then I'll punish you". Just write up a computer program saying, "If you're equal to me, and therefore play this equilibrium, then I'll play this equilibrium. If you're not, then I'll do the punish action".

**Caspar Oesterheld** (00:34:55):
Yes. Yeah, that's basically right.

**Daniel Filan** (00:34:58):
Okay. Is it only basically right?

**Caspar Oesterheld** (00:35:01):
No, I think it's basically right... I think it's fully right, sorry. [It's just] "basically" in the way that all natural language descriptions... You can get anything that is better for everyone than what they can get if everyone punishes them, which might be quite bad.

(00:35:25):
For example, in the prisoner's dilemma, we had this nice story of how you can get mutual cooperation, but you can also get, I don't know, one player cooperates 60% of the time, the other player cooperates 100% of the time. The reason why the 100% of the time cooperator doesn't cooperate less is that the 60% cooperator says, "Yeah, if we're not both submitting the program that plays this equilibrium, I'm going to always defect". In the prisoner's dilemma, you can get anything that is at least as good as mutual defection for both players. In some sense, almost everything can happen. It can't happen that one player cooperates all the time, the other player defects all the time. Because then the cooperator would always want to defect. But yeah, that's the basic picture of what's going on here.

(00:36:26):
That has been known. Then post-Tennenholtz, which is one of these papers---I think [the paper that [coined the term] "program equilibrium"](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=e1a060cda74e0e3493d0d81901a5a796158c8410)and gave this syntactic comparison-based program, and this [folk theorem](https://en.wikipedia.org/wiki/Folk_theorem_(game_theory)), as it's called, of what kind of things can happen in equilibrium. After that, most papers have focused on this "how do we make this more robust" idea. In particular, what existed prior to the robust program equilibrium paper are these papers on making things more robust by having the programs try to prove things about each other.

(00:37:11):
Here's maybe the simplest example of this that one doesn't need to know crazy logic for. You could write a program... in the prisoner's dilemma, you could write a program that tries to search for proofs of the claim "if this program cooperates, the other program will also cooperate". Your program is now very large. It has this proof search system. Somehow, it can find proofs about programs. But basically, you can still describe it relatively simply as, "I try to find the proof that if I cooperate, the opponent cooperates. Then I cooperate. Otherwise, I'll defect". It's not that difficult to see that this kind of program can cooperate against itself. Because if it faces itself, it's relatively easy to prove that if I cooperate, the opponent will cooperate. Because the statement, it's an implication where both sides of the implication arrows say exactly the same thing.

(00:38:25):
At the same time, this is more robust, because this will be robust to changing the spaces and so on. It's relatively easy to prove "if this program outputs cooperate, then this other program which is the same, except that it has the spaces in different places or switches things around in some way that doesn't really matter, that this will also output that thing, also output cooperate". This is a basic proof-based approach that will work.

(00:39:07):
I think [the first paper on this is by Barasz et al.](https://arxiv.org/abs/1401.5577) I think there are two versions of this which have different first authors, which is a little bit confusing. I think on one of them, Barasz is the first author. On [the other one](https://cdn.aaai.org/ocs/ws/ws1249/8833-38015-1-PB.pdf), it's [LaVictoire](https://www.patricklavictoire.com/). I think it's an American, so probably a less French pronunciation is correct.

**Daniel Filan** (00:39:37):
I actually think he does say "Lah vic-twahr".

**Caspar Oesterheld** (00:39:39):
Oh, okay.

**Daniel Filan** (00:39:40):
I think. I'm not 100% certain. Write in, Patrick, and tell us.

**Caspar Oesterheld** (00:39:48):
Those papers first proposed these proof-based approaches. They actually do something that's more clever, where it's much harder to see why it might work. I described a version where the thing that you try to prove is "if I cooperate, the opponent will cooperate". They instead just have programs that try to prove that the opponent will cooperate. You just do, "if I can prove that my opponent cooperates, I cooperate. Else, I defect".

(00:40:16):
This is much less intuitive that this works. Intuitively, you would think, "Surely, this is some weird infinite loop". If this faces itself... I am going to think, "What does the opponent do?" Then, "Well, to think about what my opponent will do to prove anything about them, they'll try to prove something about me". You run into this infinite circle. You would think that it's basically the same as... One very naive program that you might write is just, "Run the opponent program. If it cooperates, cooperate. Otherwise, defect". This really does just run in circles.

(00:40:56):
You would think that just doing proofs instead of this running the opponent program, that you have the same issue. It turns out that you can find these proofs which follows from a somewhat obscure result in logic called Löb's theorem, which is a little bit related to Gödel's second incompleteness theorem. With Löb's theorem it's relatively easy to prove, but it's a very "you kind of need to just write it down" proof, and then it's relatively simple. But it's hard to give an intuition for it, I think.

**Daniel Filan** (00:41:47):
Also, it's one of these things that's hard to state unless you're careful and remember... So I've tried to write it down. It's like, if you can prove that a proposition would be true... Okay, take a proposition P. Löb's theorem says that if you can prove that "if you could prove P, then P would be true", then, you would be able to prove P. If you can prove that the provability of a statement implies its truth, then you could prove the thing. The reason that this is non-trivial is it turns out that you can't always prove that if you could prove a thing, it would be true because you can't prove that your proving system works all the time. You can construct funky self-referential things that work out. Unless I have messed up, that is Löb's theorem.

(00:42:49):
My recollection is the way it works in this program is basically, you're checking if the other program would cooperate... Imagine we're both these "defect unless proof of cooperation" programs. I'm like, "Okay, I want to check if you would cooperate given me". "If you would cooperate given me" is the same as "if I would cooperate given you"... Here's the thing that I definitely can prove. If you can prove that "if I can prove that I cooperate, then you cooperate". But crucially, the "I" and the "you" are actually just the same, because we're at the same program. If it's provable that "if it's provable, then we cooperate", then we cooperate. Löb's theorem tells us that we can therefore conclude that it is provable that we cooperate. Therefore, we in fact cooperate.

(00:43:48):
My understanding is: so what do we actually do? I think we prove Löb's theorem, then apply it to our own situation, and then we both prove that we both cooperate, and then we cooperate. I think that's my recollection of how it's supposed to go.

**Caspar Oesterheld** (00:44:01):
At least that would be one way.

**Daniel Filan** (00:44:03):
Yeah, I suppose there might be even shorter proofs.

**Caspar Oesterheld** (00:44:06):
Yeah, that is basically correct. Yeah, good recollection of the papers.

**Daniel Filan** (00:44:14):
Yeah. There were a few years in Berkeley where every couple weeks somebody would explain Löb's theorem to you, and talk about Löbian cooperation. Eventually, you remembered it.

**Caspar Oesterheld** (00:44:25):
Okay, nice. I think it's a very nice idea. I actually don't know how they made this connection. Also Löb's theorem, it's relatively obscure, I think in part because it doesn't prove that much more than Gödel's second incompleteness theorem. Gödel's incompleteness theorem is "a logical system can't prove its own consistency". But here, it's the same thing. You can't prove "if I can prove something, it's true" without just being able to prove the thing.

(00:45:11):
I think that's probably one reason why Löb's theorem isn't very widely known. I feel like it's a result that for this thing, it happens to be exactly the thing you need. Once you have it written down, this cooperation property follows almost immediately. But...

**Daniel Filan** (00:45:32):
How they made the connection?

**Caspar Oesterheld** (00:45:33):
Yeah, how did they...

**Daniel Filan** (00:45:34):
I think I know this, or I have a theory about this. Originally, before they were talking about Löbian cooperation, there was this [Löbian obstacle or Löbstacle](https://intelligence.org/files/TilingAgentsDraft.pdf).

**Caspar Oesterheld** (00:45:45):
Yeah, the Löbstacle.

**Daniel Filan** (00:45:46):
Yeah, to self-trust. You might want to say, "Oh, I'm going to create a successor program to me, and if I can prove that the successor program is going to do well, then..." Or all the programs are going to be like, "If I can prove a thing is good, then I'll do it." And can I prove that a program that I write is going to be able to do stuff? And it's a little bit rough, because if I can prove that you could prove that a thing is good, then I could probably prove that the thing was good myself, and so why am I writing the [successor].

(00:46:14):
Maybe this just caused the Löb's theorem to be on the mind of everyone. I don't know. I have this theory. But I don't think I've heard it confirmed by any of the authors.

**Caspar Oesterheld** (00:46:24):
Okay. It's a good theory, I think.

**Daniel Filan** (00:46:26):
Okay. We had this Löbian cooperation idea floating around. This is one thing that was known before these papers we're about to discuss. Is there anything else's that important?

**Caspar Oesterheld** (00:46:45):
Yeah, there was a little bit more extension of this Löbian idea. One weird thing here is that we have these programs, "if I can prove this, then I cooperate". Of course, whether I can prove something, it's not decidable. There's not an algorithm that tries for 10 hours, and then it gives up. That's not what provability would normally mean.

(00:47:17):
There's [a paper by Andrew Critch](https://arxiv.org/abs/1602.04184) from I think 2019, that shows that actually, Löb's theorem still works if you consider these bounded... You try, with a given amount of effort... Specifically, you try all proofs of a given length, I think, is the constraint. It shows that some version of Löb's theorem still holds, and that it's still enough to get this Löbian cooperation if the two players consider proofs up to a long enough length. They can still cooperate.

**Daniel Filan** (00:47:55):
And it doesn't have to be the same length.

**Caspar Oesterheld** (00:47:56):
Yeah, it doesn't have to be the same length, importantly.

**Daniel Filan** (00:47:58):
It just has to be the length of that paper.

**Caspar Oesterheld** (00:48:00):
Yeah.

**Daniel Filan** (00:48:01):
Right. Yeah, yeah, which is great. Very fun result. So there's a Löbian cooperation. There's parametric bounded Löbian cooperation. Anything else of note?

**Caspar Oesterheld** (00:48:12):
Yeah. I think one other thing that is interesting---this is not really an important fact, but I think it's an important thing to understand---is that for the Löbian bots, it matters that you try to find a proof that the other player cooperates, rather than trying to find a proof that the other player defects. The same is true for this implication case that I described. If you try to check "is there a proof that if I defect, the opponent will defect?", I'm not sure why you would do that.

**Daniel Filan** (00:49:06):
You can imagine similar things, like, "Okay, if I defect, will you cooperate with me naively like a sucker? If so, then I'm just definitely going to defect".

**Caspar Oesterheld** (00:49:24):
Right. Then I guess you would check for some other property.

**Daniel Filan** (00:49:32):
Or you would check "if I defect, will you defect? If so, then I'll cooperate". Maybe that would be the program.

**Caspar Oesterheld** (00:49:37):
Yeah, maybe that is even the more sensible program. I'm not sure whether this cooperates against itself.

**Daniel Filan** (00:49:50):
It must cooperate, right?

**Caspar Oesterheld** (00:49:51):
Okay, let's think ...

**Daniel Filan** (00:49:55):
Suppose we're the same program. Then it's basically like: if provable defect "if and only if provable defect", then cooperate, else defect. But provable defect, if and only if provable defect.... It's the same... You can just see that it's the same expression on both sides.

**Caspar Oesterheld** (00:50:11):
Right, I agree. Yeah, this will cooperate. This is not an equilibrium though. If the opponent just submits a DefectBot, you're going to cooperate against it, right?

**Daniel Filan** (00:50:22):
Yes, it is a program, it is not an equilibrium. I got us off track, I fear.

(00:50:32):
But you were saying that you want to be proving the good case, not the bad case.

**Caspar Oesterheld** (00:50:39):
Yeah, maybe let's do the version from the paper, "if I can prove that you cooperate, I cooperate. Otherwise, I defect". If you think about it, in this program, it doesn't really matter that mutual cooperation is the good thing, and mutual defection is the bad thing. Ultimately, it's just we have two labels, cooperate and defect, we could call them A and B instead. It's just, "if I can prove that you output label A, I also output label A. Otherwise, I'll output label B".

(00:51:12):
Regardless of what these labels are, this will result in both outputting label A. If label A happens to be defect rather than cooperate, these will defect against each other. It matters that you need to try the good thing first or something like that.

**Daniel Filan** (00:51:29):
Yeah, yeah. I guess, maybe the most intuitive way of thinking about it, which... I haven't thought about it a ton, so this may not be accurate. But it feels like you're setting up a self-fulfilling prophecy, or if the other person happens to be you, then you're setting up a self-fulfilling prophecy. You want to set up the good self-fulfilling prophecy, not the bad self-fulfilling prophecy.

(00:51:51):
I think this is true in this setting. My impression is that there's also decision theory situations where you really care about the order in which you try and prove things about the environment. I forget if self-fulfilling prophecy is the way to think about those situations as well, even though they're conceptually related. We can perhaps leave that to the listeners if it's too hard to figure out right now.

(00:52:15):
Okay. Now that we've known this sad world that's confusing and chaotic, perhaps we can get the light of your papers.

**Caspar Oesterheld** (00:52:26):
Okay. I should say, I really like the proof-based stuff. We can talk a little bit about what maybe the upsides and downsides are. Yeah, it is confusing. I would think that one issue with it is that in practice, what programs can one really prove things about?

**Daniel Filan** (00:52:49):
Yeah, my intuition is that the point of that work is it seems like it's supposed to be modeling cases where you have good beliefs about each other that may or may not be exactly proofs. You hope that something like Löb's theorem holds in this more relaxed setting, which it may or may not. I don't exactly know.

**Caspar Oesterheld** (00:53:07):
Yeah, I agree. I also view it this way, which is a more metaphorical way. There's some distance between the mathematical model, and the actual way it would work then.

## The basic idea of Robust Program Equilibrium <a name="rpe-basic-idea"></a>

**Daniel Filan** (00:53:26):
But I want to hear about your paper.

**Caspar Oesterheld** (00:53:28):
Right. Okay. Now, let's get to my paper. My paper is on whether we can get these cooperative equilibria, not by trying to prove things about each other, but just by simulating each other. I already mentioned that there's a super naive but intuitive approach that you would like to run the opponent against... You'd like to run the opponent with myself as input, see if they cooperate, if they do, cooperate, otherwise defect. Just this very obvious intuition, maybe from [tit for tat](https://en.wikipedia.org/wiki/Tit_for_tat) in repeated games, that you want to reward the other player for cooperating, and get a good equilibrium that way.

(00:54:21):
The problem with this, of course, is that it doesn't hold if both players do this. I guess this would work if you play this sequentially. We talked about the Stackelberg stuff earlier. If I submit a program first, and then you submit a program second, then it would work for me to submit a program that says, "Run your program, cooperate if it cooperates, defect if your program defects", and then you would be incentivized to cooperate. But if both players play simultaneously, infinite loop, so it kind of doesn't work.

**Daniel Filan** (00:54:58):
If we had [reflective oracles](https://arxiv.org/abs/1508.04145), then it could work, depending on the reflective oracle. But that's a whole other bag of worms.

**Caspar Oesterheld** (00:55:03):
Yeah, I guess reflective oracles... Yeah, I probably shouldn't get into it. But it's another model that maybe is a little bit in between the proof-based stuff and the simulation stuff.

**Daniel Filan** (00:55:18):
At any rate.

**Caspar Oesterheld** (00:55:19):
Yeah. It turns out there's a very simple fix to this issue, which is that instead of just always running the opponent and cooperating if and only if they cooperate, you can avoid the infinite loop by just cooperating with epsilon probability, and only if this epsilon probability clause doesn't trigger, only then do you run the other program. So your program is just: flip a very biased coin---epsilon is a small number, right? You check whether some low probability event happens. If it does, you just cooperate without even looking at the opponent program. Otherwise, you do simulate the other program and you copy whatever they do. You cooperate if they cooperate, defect if they defect.

(00:56:23):
The idea is that, basically, it's the same intuition as "just simulate the opponent, and do this instantaneous tit-for-tat". Except that now, you don't run into this running for infinitely long issue, because it might take a while, but eventually, you're going to hit these epsilon clauses. If we both submit this program, then probably, there's some chance that I'm immediately cooperating, but most likely, I'm going to call your program which might then also immediately cooperate. Most likely, it's going to call my program again, and so on. But at each point, we have a probability epsilon of halting, and with probability one will eventually halt.

**Daniel Filan** (00:57:16):
This is a special case of this general construction you have in the paper, right?

**Caspar Oesterheld** (00:57:26):
Yeah. This is for the prisoner's dilemma in particular, where you have these two actions that happen to be cooperate and defect. In general, there are two things that you can specify here, like you specify what happens with the epsilon probability, then the other thing that you specify is what happens if you simulate the other player, you get some action out of the simulation, and now you need to react to this in some way.

(00:57:57):
The paper draws this connection between these ϵGroundedπBots, as they're called, and repeated games where you can only see the opponent's last move. It's similar to that, where: okay, maybe this epsilon clause where you don't look at your opponent is kind of like playing the first round where you haven't seen anything of your opponent yet. I guess, in the prisoner's dilemma, there's this well-known tit for tat strategy which says: you should cooperate in the beginning, and then at each point, you should look at the opponent's last move, and copy it, cooperate if they cooperate. But in general, you could have these myopic strategies for these repeated games where you do something in the beginning, and then at each point, you look at the opponent's last move, and you react to it in some way. Maybe do something that's equally cooperative or maybe something that's very slightly more cooperative to slowly get towards cooperative outcomes or something like that. You could have these strategies for repeated games. You can turn any of these strategies into programs for the program game.

**Daniel Filan** (00:59:21):
One thing that I just noticed about this space of strategies, this is strategies that only look at your opponent's last action, right?

**Caspar Oesterheld** (00:59:29):
Yes.

**Daniel Filan** (00:59:29):
In particular, there's this other thing you can do which is called win-stay, lose-switch, where if you cooperated against me, then I just do whatever I did last time. If you defected against me, then I do the opposite of what I did last time. It seems like this is another thing that your next paper is going to fix. But in this strategy, it seems like I can't do this, right?

**Caspar Oesterheld** (00:59:58):
Yes. Yeah, it's really very restrictive. Most of the time, you're going to see one action of the opponent, you have to react to that somehow, and that's it.

**Daniel Filan** (01:00:13):
Yeah. But it's this nice idea. It's basically this connection between: if you can have a good iterated strategy, then you can write a good computer program to play this mutually transparent program game, right?

**Caspar Oesterheld** (01:00:28):
Yeah.

**Daniel Filan** (01:00:29):
How much do we know about good iterated strategies?

**Caspar Oesterheld** (01:00:34):
That is a good question. For the iterated prisoner's dilemma, there's a lot about this. There are a lot of these tournaments for the iterated prisoner's dilemma. I'm not sure how much there is for other games, actually. Yeah, you might have iterated [stag hunt](https://en.wikipedia.org/wiki/Stag_hunt) or something like that? I guess, maybe for a lot of the other ones, it's too easy or so.

(01:01:03):
There's some literature. You can check the paper. There are various notions that people have looked at, like exploitability of various strategies, which is how much more utility can the other player get than me if I play the strategy? For example, tit for tat, if the opponent always defects, you're going to get slightly lower utility than them because in the first round, you cooperate, and then they defect. Then in all subsequent rounds, both players defect. It's very slightly exploitable, but not very much.

(01:01:45):
These notions that have been studied, and in my paper, I transfer these notions... If you take a strategy for the iterated prisoner's dilemma, or for any repeated game, it has some amount of exploitability, and the analogous ϵGroundedπBot strategy has the same amount of exploitability. This is also an interesting question in general. How much qualitatively different stuff is there even in this purely ϵGroundedπBot space? If all you can do is look at the one action of the opponent and react to this action, how much more can you even do than things that are kind of like this sort of tit-for-tat...? Like I mentioned, in more complex games maybe you want to be slightly more cooperative... I don't know. After a bunch of simulations you eventually become very cooperative or something like that.

**Daniel Filan** (01:02:52):
Okay. I have a theory. In my head I'm thinking: okay, what's the general version of this? And I can think of two ways that you can generalize, right? Here's what I'm imagining you should do, in general. Okay. You have a game, right? First you think about: okay, what's the good equilibrium of this game, right? And then what do I want to do if the other person doesn't play ball? It seems like there are two things I could do if the other person doesn't join me in the good equilibrium. Firstly, I could do something to try and punish them. And secondly, I can do something that will make me be okay, be good enough no matter what they do. I don't exactly know how you formalize these, but my guess is that you can formalize something like these. And my guess is that these will look different, right?

(01:03:43):
You can imagine saying, "Okay, with epsilon probability, I do my part to be in the good equilibrium, and then the rest of the time I simulate what the other person does. If they play in the good equilibrium I play in the good equilibrium. If they don't play in the good equilibrium then, depending on what I decided earlier, I'm either going to punish them or I'm going to do a thing that's fine for me". Or you can imagine that I randomize between those. Maybe there's some "best of both worlds" thing with randomizing. I don't exactly know. Do you have a take on that?

**Caspar Oesterheld** (01:04:14):
I mean, there's at least one other thing you can do, right, which is try to be slightly more cooperative than them in the hope that you just-

**Daniel Filan** (01:04:26):
Right.

**Caspar Oesterheld** (01:04:31):
Imagine the repeated game, right? At any given point you might want to try to be a bit more cooperative in the hope that the other person will figure this out, that this is what's going on, and that you're always going to be a little bit more cooperative than them. And that this will lead you to the good equilibrium or to a better equilibrium than what you can get if you just punish. I mean, punish usually means you do something that you wouldn't really want to do, you just do it to incentivize the other player. Or even the "okay, well, you're going to go and do whatever but I'm just going to do something that makes me okay".

**Daniel Filan** (01:05:15):
So is the "be more cooperative than the other person" thing... I feel like that's already part of the strategy. Okay, so here's the thing I could do. With epsilon probability, do the good equilibrium, then simulate what the opponent does. If they do the good thing, if they're in the good equilibrium, then I join the good equilibrium. If they don't join the good equilibrium, then with epsilon probability I be part of the good equilibrium, and then otherwise I do my other action. With epsilon probability for being slightly more cooperative, you could have just folded that into the initial probability, right?

**Caspar Oesterheld** (01:05:51):
Right. The difference is you can be epsilon more cooperative in a deterministic way, right? With this epsilon probability thing, some of the time you play the equilibrium that you would like to play. This alternative proposal is that you always become slightly more cooperative, which is... I'm not sure how these things play out. I would imagine that for characterizing what the equilibria are probably all you need is actually the punishment version. But I would imagine that if you want to play some kind of robust strategy you would sometimes move into a slightly more cooperative direction or something like that.

(01:06:51):
You could have all of these games where there are lots of ways to cooperate and they sort of vary in how they distribute the gains from trade or something like that, right? Then there's a question of what exactly happens if your opponent is... They play something that's kind of cooperative but sort of in a way that's a little bit biased towards them. I guess maybe you would view this as just a form of punishment if you then say, "Well, I'm going to stay somewhat cooperative but I'm going to punish them enough to make this not worthwhile for them" or something like that.

**Daniel Filan** (01:07:33):
If there's different cooperative actions that are more or less cooperative then it definitely makes sense. At the very least I think there are at least two strategies in this space. I don't know if both of them are equilibria to be fair.

## Are ϵGroundedπBots inefficient? <a name="are-egpb-inefficient"></a>

**Daniel Filan** (01:07:46):
Okay. There are a few things about this strategy that I'm interested in talking about. We're both playing the same "tit-for-tat but in our heads" strategy, right? The time that it takes us to eventually output something is O(one/epsilon), right? On average, because with each round with epsilon probability we finish, and then it takes one on epsilon rounds for that to happen, right?

**Caspar Oesterheld** (01:08:31):
Yeah, I think that's roughly right. I mean, it's a geometric series, right? I think it's roughly one over epsilon.

**Daniel Filan** (01:08:40):
It's one minus epsilon over epsilon, which is very close to one over epsilon.

**Caspar Oesterheld** (01:08:42):
Yes.

**Daniel Filan** (01:08:45):
That strikes me as a little bit wasteful, right, in that... So the cool thing about the Löbian version was: the time it took me to figure out how to cooperate with myself was just the time it took to do the proof of Löb's theorem no matter how... It was sort of this constant thing. Whereas with the epsilon version, the smaller the epsilon it is, the longer it seems to take for us. And we're just going back and forth, right? We're going back and forth and back and forth and back and forth. I have this intuition that there's something wasteful there but I'm wondering if you agree with that.

**Caspar Oesterheld** (01:09:25):
Yeah, I think it's basically right. Especially if you have a very low epsilon, right, there's a lot of just doing the same back-and-forth thing for a long time without getting anything out of it. One thing is that you could try to speed this up, right, if you... So let's say I run your program, right? Instead of just running it in a naive way I could do some analysis first.

(01:10:11):
If you have a compiler of a computer program, it might be able to do some optimizations. And so maybe I could analyze your program, analyze my program, and I could tell: okay, what's going to happen here is that we're going to do a bunch of nothing until this epsilon thing triggers. Really instead of doing this actually calling each other, we just need to sample the depth of simulations according to this geometric distribution, the distribution that you get from this halting with probability epsilon at each step. You could do this analysis, right? Especially if you expect that your opponent will be an ϵGroundedFairBot, you might explicitly put in your compiler or whatever something to check whether the opponent is this ϵGroundedFairBot. And if so, we don't need to do this actually calling each other, we just need to sample the depth.

(01:11:26):
In some sense, the computation that you need to do is sample the depth then sample from... Whoever halts at that point, sample from their 'base' distribution, their blind distribution. And then sort of propagate this through all of the function that both players have for taking a sample of the opponent's strategy and generating a new action. If this is all very simple then... in principle, your compiler could say, for the ϵGroundedFairBot in particular---sorry, the ϵGroundedFairBot is the version for the prisoner's dilemma. In principle, your compiler could directly see "okay, what's going to happen here? Well, we're going to sample from the geometric distribution, then 'cooperate' will be sampled, and then a bunch of identity functions will be applied to this". So this is just cooperate without needing to do any actual running this, doing recursive calls by something something with a stack, and so on. Probably you don't actually need any of this.

**Daniel Filan** (01:12:52):
There's something intuitively very compelling about: okay, if I can prove that the good thing happens or whatever, then do the proof-based thing. If I can't prove anything then do the simulation stuff. It seems intuitively compelling. I imagine you probably want to do some checks if that works on the proof-based side, depending on the strategy you want to implement.

**Caspar Oesterheld** (01:13:15):
I mean, the thing I'm proposing is not to have the proof fallback, but just that you... You always do the ϵGroundedFairBot thing, for example, or the ϵGroundedπBot. Instead of calling the opponent program in a naive way where you actually run everything, you throw it in this clever compiler that analyzes things in some way. And maybe this compiler can do some specific optimizations but it's not a fully general proof searcher or anything like that.

**Daniel Filan** (01:13:52):
I mean, it's checking for some proofs, right?

**Caspar Oesterheld** (01:13:54):
Yeah, it's checking for some specific kinds of proofs... I mean, that's how modern day compilers I assume work, right, is that they understand specific kinds of optimizations and they can make those but they don't have a fully general proof search or anything like that.

**Daniel Filan** (01:14:15):
Sorry. When you said that I was half listening and then half thinking about a different thing, which is: you could imagine ϵGroundedFairBot which is: first, if your source code is equal to mine, then cooperate. Else, if your source code is the version of ϵGroundedFairBot that doesn't first do the proof search, then cooperate. Else, with probability epsilon cooperate, probability one minus epsilon, do what the other person does, right?

(01:14:41):
So that particular version probably doesn't actually get you that much because the other person added some spaces in their program. And then I'm like but you could do some proof stuff, insert it there. I guess there are a few possibilities here. But it does seem like something's possible.

## Compatibility of proof-based and simulation-based program equilibria <a name="compatibility-of-equilibria"></a>

**Caspar Oesterheld** (01:15:06):
These different kinds of ways of achieving this more robust program equilibrium, they are compatible with each other. If I do the ϵGroundedFairBot and you do the Löbian bot, they are going to cooperate with each other.

**Daniel Filan** (01:15:29):
You're sure?

**Caspar Oesterheld** (01:15:30):
I'm pretty sure, yeah.

**Daniel Filan** (01:15:31):
Okay. You've probably thought about this.

**Caspar Oesterheld** (01:15:32):
I wrote [a paper](https://arxiv.org/abs/2211.05057) about it. It's not a real paper, it's sort of like a note on this. Maybe let's take the simplest versions or whatever, we don't need to go down the Löb's theorem path again. Let's take the simplest version which is just, can I prove "if I cooperate, you cooperate", then cooperate. If you're the Löbian bot and I'm the ϵGroundedFairBot, you can prove that if you cooperate I will cooperate, right? Well, I'm epsilon times...

**Daniel Filan** (01:16:13):
Sorry. Can you say that without using "you" and "I"?

**Caspar Oesterheld** (01:16:15):
Okay. Am I allowed to say "I submit a program that's"-

**Daniel Filan** (01:16:20):
Yes, you are.

**Caspar Oesterheld** (01:16:20):
Okay. So I submit a program that is just the ϵGroundedFairBot, so with epsilon probability cooperate otherwise simulate you and do what do you do. And your program is: if it's provable that "if this program cooperates, the other program cooperates", then cooperate, and otherwise, defect. Okay. So let's think about your program-

**Daniel Filan** (01:16:54):
The proof-based one.

**Caspar Oesterheld** (01:16:55):
The proof-based one. So your program will try to prove: if it cooperates, my program, ϵGroundedFairBot will cooperate.

**Daniel Filan** (01:17:09):
Okay. So the proof-based program is trying to prove, "if proof-based program cooperates then sampling program cooperates". And it will be able to prove that. I think the other implication is slightly trickier but maybe you only care about the first implication, or you care about it more.

**Caspar Oesterheld** (01:17:24):
Sorry, what is the other implication?

**Daniel Filan** (01:17:25):
That if the sampling-based program cooperates then the proof- based one will cooperate. Maybe that's not so bad.

**Caspar Oesterheld** (01:17:34):
But do you actually need this? The proof-based program, it will succeed in proving this implication, right, and it will, therefore, cooperate.

**Daniel Filan** (01:17:45):
And that's how it proves that it will do it in the other direction?

**Caspar Oesterheld** (01:17:48):
I mean, that's how one can then see that the ϵGroundedFairBot will also cooperate because it will... Well, with epsilon probability it cooperates anyway. And with the remaining probability it does whatever the proof-based thing does, which we've already established is to cooperate. Sorry, does this leave anything open?

**Daniel Filan** (01:18:03):
I think I was just thinking about a silly version of the program where the proof-based thing is checking: can I prove that if my opponent will cooperate then I will cooperate? But I think you wouldn't actually write this because it doesn't make any sense.

**Caspar Oesterheld** (01:18:22):
No. That seems harder though. I don't know. Maybe if we think about it for two minutes we'll figure it out. I think one wouldn't submit this program.

## Cooperating against CooperateBot, and how to avoid it <a name="cooperating-against-coopbot"></a>

**Daniel Filan** (01:18:32):
I next want to ask a different question about this tit-for-tat-based bot. This bot is going to cooperate against CooperateBot, right, the bot that always plays cooperate? That seems pretty sad to me, right? I'm wondering how sad do you think that this is?

**Caspar Oesterheld** (01:18:53):
I'm not sure how sad. Okay, I have two answers to this. The first is that I think it's not so obvious how sad it is. And the second is that I think this is a relatively difficult problem to fix. On how sad is this: I don't know. It sort of depends a little bit on what you expect your opponent to be, right? If you imagine that you're this program, you've been written by Daniel, and you run around the world, and you face opponents. And most of the opponents are just inanimate objects that weren't created by anyone for strategic purposes. And now you face the classic rock that says "cooperate" on it. It happens to be a rock that says "cooperate", right? You don't really want to cooperate against that.

(01:19:49):
Here's another possibility. We play this program equilibrium game, literally, and you submit your program, right? And you know that the opponent program is written by me, by Caspar, who probably thought about some strategic stuff, right? Okay, it could be that I just wrote a CooperateBot, right, and that you can now get away with defecting against it. But maybe you could also imagine that maybe there's something funny going on. And so for example, one thing that could be going on is that I could... Here's a pretty similar scheme for achieving cooperation in the program equilibrium game, which is based on not the programs themselves mixing but the players mixing over what programs to submit. And so I might-

**Daniel Filan** (01:20:39):
Mixing meaning randomizing?

**Caspar Oesterheld** (01:20:40):
Yeah, randomizing. Very good. So I might randomize between the program that just cooperates---the CooperateBot, the program that cooperates if and only if the opponent cooperates against CooperateBot---so it's sort of a second-order CooperateBot, something like that. And then you can imagine how this goes on, right? Each of my programs is some hierarchy of programs that check that you cooperated against the one one lower [down] the list. In some sense this is similar to the ϵGroundedFairBot, I guess. You can look at my program and maybe I could just defect or something like that. But the problem is you might be in a simulation of the programs that are higher in the list. If I submit this distribution, you would still want to cooperate against my CooperateBot, of course. So that is one reason to want to cooperate against CooperateBot.

**Daniel Filan** (01:22:00):
It suddenly means that it really matters which things in my environment I'm modeling as agents and which things in my environment that I'm modeling as non-agents, right? Because in my actual environment, I think there are many more non-agents than there are agents. So take this water bottle, right? Not only do I have to model it as a non-agent, but it seems like maybe I've also got to be modeling what are the other things it could have done if physics were different, right? It seems like if I have this sort of attitude towards the world a bunch of bad things are going to happen, right?

(01:22:43):
And also, if I'm in a strategic setting with other agents that are trying to be strategic, I think you do actually want to be able to say things like "Hey, if I defected would you cooperate anyway? In that case, I'll just defect. But if your cooperation is dependent on my cooperation then I'm going to cooperate". It's hard to do with this construction because I'm checking two things and that explodes into a big tree. But this seems to me like something that you do want to do in the program equilibrium world. I guess those are two things. I'm wondering what your takes are.

**Caspar Oesterheld** (01:23:29):
Yeah, it would be nice to know how to do the: for this given opponent program, could my defecting make the opponent defect? I think a program that exploits CooperateBot and cooperates against itself in some robust way, I agree that this would be desirable. I guess we can say more about to what extent this is feasible. I think in some sense one does just have to form the beliefs about what the water bottle could have been and things like that. I guess with the water bottle---I don't know, I mean, it's sort of a weird example. But with the water bottle, I guess, you would have to think about: do you have a reason to believe that there's someone who's simulating what you do against the water bottle, and depending on that does something, right?

(01:24:37):
In the strategic setting where you know that the opponent program is submitted by Caspar or by someone who knows a little bit about this literature, you just have a very high credence that if you face a CooperateBot probably something funny is going on, right?

(01:24:56):
You have a high credence that there are some simulations being run of your program that check what your program does against various opponents. You have to optimize for that case much more than you optimize for the case where your opponent is just a CooperateBot. Whereas with a water bottle, you don't really have this, right? I don't know. Why would someone simulate like "okay, the water bottle could have been---"

**Daniel Filan** (01:25:22):
I mean, people really did design this water bottle by thinking about how people would use it, right? I think I have a few thoughts there. Firstly, if I'm just naively like, "did people change how this water bottle would work depending on how other people would interact with it?" That's just true. I mean, they didn't get the water bottle itself to do that, so maybe that's the thing I'm supposed to check for.

(01:25:46):
It's also true that if you go to real iterated, mutually transparent prisoner's dilemmas, people do actually just write dumb programs in those. And it's possible that okay, these are played for 10 bucks or something and that's why people aren't really trying. But in fact, some people are bad at writing these programs and you want to exploit those programs, right?

(01:26:22):
And I also have this issue which is: it seems like then what's going on is my overall program strategy or something is: first, check if I'm in a situation where I think the other program was designed to care about what I am going to do, then cooperate, otherwise defect. Maybe this is not so bad in the simulation setting. In the proof-based setting, this would be pretty bad, right, because now it's much harder to prove nice things about me. In the simulation setting, it might just be fine as long as we're really keeping everything the same. Maybe this is an advantage of the simulation setting, actually. I don't really know.

**Caspar Oesterheld** (01:27:05):
Sorry, I'm not sure I fully followed that.

**Daniel Filan** (01:27:08):
Okay. I took your proposal to be: the thing you should do is you should figure out if you're in a strategic setting where the other person is, basically, definitely not going to submit a CooperateBot. I'm imagining myself as the computer program. Maybe this is different to what you were saying. But I was imagining that the program was "check if the other computer program was plausibly strategically designed. Then-

**Caspar Oesterheld** (01:27:41):
Yes.

**Daniel Filan** (01:27:42):
If so then do ϵGroundedFairBot, otherwise do DefectBot. For example, one concern is different people write their programs to do this check in different ways and one of them ends up being wrong. Maybe this is not a huge issue. I don't know. It feels like it adds complexity in a way that's a little bit sad.

**Caspar Oesterheld** (01:28:06):
I could imagine that, I guess, for the proof-based ones, the challenge is that they need to be able to prove about each other that they assess the... Whether they're in a strategic situation, they need to assess this consistently or something like that.

**Daniel Filan** (01:28:23):
Also, the more complicated your program is the harder it is for other people to prove stuff about you. One thing you want to do if you're a proof-based program, in a world of proof-based programs, is be relatively easy to prove things about. Well, depending on how nice you think the other programs are, I guess.

**Caspar Oesterheld** (01:28:47):
I mean, in practice I think, in the tournament, for various reasons, you should mostly try to exploit these CooperateBots, or these programs that are just written by people who have thought about it for 10 minutes or who just don't understand the setting or something like that. You wouldn't expect people to submit this cooperation bot hierarchy thing because there's just other things to do, right? In some sense, there's a higher prior on these kinds of programs.

(01:29:25):
But you could imagine a version of the tournament setting where you're told who wrote the opponent program, and then your program distinguishes between someone who has publications on program equilibrium wrote the opponent program, and then you think, okay, well, all kinds of funny stuff might be going on here. I might currently be simulated by something that tries to analyze me in some weird way so I need to think about that. Versus the opponent is written by someone who, I don't know, I don't wanna...

**Daniel Filan** (01:30:06):
A naive podcaster.

**Caspar Oesterheld** (01:30:09):
...by someone who just doesn't know very much about the setting. And then maybe there you think: okay, most prior probability mass is on them just having screwed up somehow and that's why their program is basically a CooperateBot. Probably in these tournaments I would imagine that, I don't know, 30% of programs are just something that just fundamentally doesn't work, it doesn't do anything useful. It just checks whether the opponent has a particular string in the source code or something like that. And meanwhile very little probability mass [is] on these sophisticated schemes for "check whether the opponent cooperates against CooperateBot in a way that's useful".

(01:30:53):
So we talked a little bit about to what extent it's desirable to exploit CooperateBots. There's then also the question of how exactly to do this. Here's one more thing on this question of whether you need to know whether the opponent is part of the environment or strategic. You can think about the repeated prisoner's dilemma, right? I mean, tit-for-tat, everyone agrees it's a reasonable strategy. And tit-for-tat also cooperates against CooperateBot, right? And I would think there it's analogous. Tit-for-tat is a reasonable strategy if you think that your opponent is quite strategic. The more you're skeptical, the more you should... I don't know, maybe you should just be DefectBot, right? Against your water bottle maybe you can be DefectBot. And then there's some in-between area where you should do tit-for-tat, but maybe in round 20 you should try defecting to see what's going on. And then if they defect you can maybe be pretty sure that they're strategic.

**Daniel Filan** (01:32:20):
It seems to me like the thing you want to do is you want to have randomized defection, then see if the opponent punishes you, and then otherwise do tit-for-tat. But also, be a little bit more forgiving than you otherwise would be in case other people are doing the same strategy.

**Caspar Oesterheld** (01:32:37):
One difference between the settings is that you can try out different things more. Which I think also leads nicely to the other point which is: how exactly would you do this exploiting CooperateBots? I do think just a fundamental difficulty in the program equilibrium setting for exploiting CooperateBots is that it's... Aside from little tricks, it's difficult to tell whether the opponent is a CooperateBot in the relevant sense. Intuitively, what you want to know is: if I defected against my opponent, would they still cooperate? And if that's the case, you would want to defect. But this is some weird counterfactual where you have all of these usual problems of conditioning on something that might be false and so you might get all kinds of weird complications.

(01:33:43):
So, I think in comparison to the tit-for-tat case where... I mean, it's not clear what exactly you would do, but maybe in some sense, against the given opponent, you can try out sometimes defecting, sometimes cooperating and seeing what happens. There's less of that in the program game case because your one program, there's some action that you play and maybe you can think if I played this other action... But it's a weird... You run into these typical logical obstacles.

**Daniel Filan** (01:34:26):
Although it feels like it might not be so bad. So, imagine I have this thing where I'm saying, "Okay, suppose I defected. Would you cooperate against a version of me that defected? If so, then I'm going to defect". And in that case, it seems like my defection is going to show up in the cases in which you would cooperate and therefore, that counterfactual is not going to be logically impossible, right?

**Caspar Oesterheld** (01:34:57):
Yeah, that's a good point. So, I guess a very natural extension of (let's say) these proof-based bots is: okay, what if you first try to prove, "if I defect, the opponent will cooperate"? This will defect against CooperateBots, which is good. The question is whether this will still... What does this do against itself? This will still cooperate against itself, right?

**Daniel Filan** (01:35:30):
Yeah. Because if I'm asking, "will you cooperate if I defect?" The answer is no, if I'm playing myself, because I always have to do the same thing as myself because I'm me.

**Caspar Oesterheld** (01:35:40):
Yeah, maybe this just works.

**Daniel Filan** (01:35:42):
I bet there must be some paper that's checked this.

**Caspar Oesterheld** (01:35:49):
Yeah, I'm now also trying to remember. Because one of these proof-based papers, they do consider this PrudentBot, which does something much more hacky: it tries to prove (and there's some logic details here)---it tries to prove that... (Okay, there there's one issue with the program that you just described that I just remembered, but let's go to PrudentBot first). So, PrudentBot just checks whether you would cooperate against DefectBot. And then, if you cooperate against DefectBot, I can defect against you.

(01:36:39):
I don't know. To me, this is a little bit... It's natural to assume that if the opponent cooperates against DefectBot, they're just non-strategic. They haven't figured out what's going on and you can defect against them. But in some sense, this is quite different from this "does my defection make the opponent defect?" or something like that.

**Daniel Filan** (01:37:03):
Yeah, it's both the wrong counterfactual and it's a little bit less strategic, right?

**Caspar Oesterheld** (01:37:09):
Yes. The things that I'm aware of that people have talked about are more like this, where they check these relatively basic conditions. You can view them as checking for specific kinds of CooperateBots. I guess another thing you can do is for the ϵGroundedFairBots, just add in the beginning a condition [that] if the opponent is just a CooperateBot, or if the opponent never looks at the opponent's source code at all, then you can defect against them. You can add these sorts of things. And I think from the perspective of winning a tournament, you should think a lot about a lot of these sorts of conditions and try to exploit them to defect against as many of these players as possible. But it's not really satisfying. It feels like a trick or some hacky thing, whereas the thing you proposed seems more principled.

(01:38:09):
Okay. Now, on this thing, I could imagine one issue is that: when this program faces itself, it first needs to prove... So, one problem is always that sometimes, to analyze opponent programs, you need to prove that some provability condition doesn't trigger. And the problem is that just from the fact that you think this condition is false, you can't infer that it's not provable because of incompleteness. So, I could imagine that I can't prove that your program doesn't just falsely prove that your program can safely defect against me because you might think, well... When I prove things, I don't know whether Peano arithmetic or whatever proof system we use is consistent.

(01:39:27):
And so there's always a possibility that every provability condition triggers, which means that I don't know whether your first condition triggers. Actually for this PrudentBot, this also arises. If I am this PrudentBot, as part of my analysis of your program, I try to prove that you would defect or cooperate or whatever. I try to prove something about what you would do against DefectBot. And for that, if (let's say) you're just some more basic Löbian FairBot-type structure, then in my analysis of your program, I need to conclude that your clause "if I cooperate, the opponent cooperates" or your clause "if I can prove that the opponent cooperates"... I need to conclude that this won't trigger. To prove that you don't cooperate against DefectBot, I need to conclude that you won't falsely prove that DefectBot will cooperate against you.

(01:40:48):
And this, I can't prove in Peano arithmetic or in the same proof system that you use. So, what they actually do for the PrudentBot is that I need to consider... They call it PA+1. I don't know how widely this is used. I need to consider Peano arithmetic or whatever proof system they use, plus the assumption that that proof system is consistent, which gives rise to a new proof system which can then prove that your "if" condition is not going to trigger. So, this is some general obstacle.

**Daniel Filan** (01:41:28):
Right. And we've got coordinate on what proof systems we use then, because if I accidentally use a too-strong proof system, then you have difficulty proving things about me. And I guess also, this thing about, "well, if I defected, would you still cooperate with me?" It feels a little bit hard to... In the proof-based setting, I can say, "if my program or your program outputted defect, would your program or my program output cooperate?" I could just do that conditional or whatever.

(01:42:04):
If I want to do this in a simulation-based setting---which I think there are reasons to want to do. Sometimes, you just can't prove things about other people and you have to just simulate them. And it's nice because it's moving a bit beyond strict computer programs. It's also nice because maybe it's hard to prove things about neural networks, which was one of the motivations---but I don't even know what the condition is supposed to be in that setting. Maybe if we're stochastic programs, I could say: maybe I could do a conditional on "this stochastic program outputs defect". But it's not even clear that that's the right thing because you're looking at my program, you're not looking at the output of my program.

**Caspar Oesterheld** (01:42:52):
Yeah. Though you can have programs that do things like "if the opponent cooperates with probability at least such and such..." I think one can make those kinds of things well-defined at least.

**Daniel Filan** (01:43:05):
Yeah. But somehow, what I want to say is "if you cooperate with high probability against a version of me that defects...", you know what I mean? Either you're simulating just a different program or you're simulating me and I don't know how to specify you're simulating a version of me that defects. You know what I mean?

**Caspar Oesterheld** (01:43:28):
Yeah. I agree that that's-

**Daniel Filan** (01:43:32):
In some special cases, maybe I could run you and if I know what location in memory you're storing the output of me, I can intervene on that location of memory, but (a) this is very hacky and (b) I'm not convinced that this is even the right way to do it.

**Caspar Oesterheld** (01:43:46):
Yeah, I guess there are various settings where you constrain the way that programs access each other that would allow more of these counterfactuals. For example, you could consider pure simulation games where you don't get access to the other player's source code, but you can run the other player's source code. And I guess in those cases, some of these counterfactuals become a bit more straightforwardly well-defined, that you can just... What if I just replace every instance of your calls to me with some action? I mean, there are some papers that consider this more pure simulation-based setting as well, but obviously that would not allow for proof-based stuff and things like that.

## Making better simulation-based bots <a name="making-better-sim-based-bots"></a>

**Daniel Filan** (01:44:43):
So, I think at this point, I want to tee up your next paper. So, in particular in this paper, there are two types of strategies that you can't turn into the program equilibrium setting. So, I think we already discussed win-stay lose-switch, where I have to look at what you did in the last round, and I also have to look at what I did in the last round. There's also this strategy in the iterated prisoner's dilemma called a grim trigger where if you've ever defected in the past, then I'll start defecting against you. And if you've always cooperated, then I'll cooperate. And neither of these, you can have in your ϵGroundedFairBots. Why is that?

**Caspar Oesterheld** (01:45:24):
Yeah. Basically, the main constraint on these ϵGroundedFairBots or πBots or whatever is that they just can't run that many simulations. You can run one simulation with high probability or something like that. Maybe with low probability, you can maybe start two simulations or something like that. But the problem is just as soon as you simulate the opponent and yourself or multiple things and with high probability, you run into these infinite loop issues again that this epsilon condition avoids. Another case is if you have more than two players, things become weird. Let's say you have three players. Intuitively, you would want to simulate both opponents, and then, if they both cooperate, you cooperate. If one of them defects, then maybe you want to just play the special punishment action against them depending on what the game is. But you can't simulate both opponents. Because if every time you're called, [you] start two new simulations or even two minus epsilon or something like that in expectation, you get this tree of simulations that just expands and occasionally some simulation path dies off, but it multiplies faster than simulations halt.

**Daniel Filan** (01:46:55):
Right. Yeah. Basically, when you grow, you're doubling, but you cut off factor of epsilon, but epsilon is smaller than a half. And therefore, you grow more than you shrink and it's really bad. And if epsilon is greater than a half, then you're not really simulating much, are you?

**Caspar Oesterheld** (01:47:11):
Yeah.

**Daniel Filan** (01:47:12):
So, how do we fix it?

**Caspar Oesterheld** (01:47:13):
Okay, so we have this [newer paper](https://arxiv.org/abs/2412.14570), where I'm fortunate to be the second author, and the first author's Emery Cooper, and then [Vince Conitzer](https://www.cs.cmu.edu/~conitzer/), my PhD advisor, is also on the paper. And so, this fixes exactly these issues. And I think it's a clever, interesting idea. So, to explain this idea, we need to imagine that the way that programs randomize works a particular way. The architecture of the programming language has to be a particular way to explain this. If you have a normal programming language, you call random.random() or some such function and you get a random number out of it.

(01:48:10):
But another way to model randomization is that you imagine that at the beginning of time or when your program is first called, it gets as input an infinite string of random variables that are rolled out once in the beginning, and then, you have this long string of... It could be (for example) bits, and all you're going to do is use the bits from this input. And so, in some sense, this is a way of modeling randomization with a deterministic program. In some sense, randomization is like running a deterministic program on an input that is random. As part of your input, you get this random string. And so, specifically, let's imagine that you get these as a random string as input, but each entry is just a random number between zero and one.

(01:49:06):
The way that these infinite simulation issues are fixed is that when I run, for example, my two opponents and myself, I pass them all the same random input string and that way, I coordinate how they halt or at what point they halt. Very specifically, here's how it works. So, let's maybe first consider a version where the issue is just that you have multiple opponents, but you're still doing something like ϵGroundedFairBot where you're happy to look just at the last round. Or maybe win-stay lose-[switch], where you maybe also look at your own previous action.

(01:49:59):
So, what you do is you look at your random input string, and if the first number is below epsilon, then you just immediately halt as usual by just outputting something. And otherwise, you remove the first thing from this infinite random input string. And then, you call all of these simulations. You simulate both opponents. Let's say you have two opponents and yourself, just with the first entry in that list removed. And now, okay, how does this help? Well, I mean the opponents might do the same, right? Let's say they also all check the first thing, check whether it's smaller than epsilon, and then remove the first and call recursively.

(01:50:55):
Well, the trick is that by all of them having the same input string, they all halt at the same point. All your simulations are going to halt once they reach the specific item in this input string---the first item in this input string that is smaller than epsilon. And so, that allows for simulating multiple opponents. You can simulate yourself of course, and you can also simulate multiple past time steps by, instead of passing them just the input string with the first thing removed, you can also check what did they do, in some intuitive sense, 'two time steps ago' by removing the first two random variables from the input string and passing that into them. So, this is the basic scheme for making sure that these simulations all halt despite having a bunch of them.

**Daniel Filan** (01:52:04):
My understanding is that you have two constructions in particular. There's this correlated one and this uncorrelated one. Can you give us a feel for what the difference is between those?

**Caspar Oesterheld** (01:52:15):
Yeah. So, there are differences in the setting. So, the correlated one is one where you get a correlated, or you get a shared random input sequence. So you could imagine that there's some central party that generates some sequence of random numbers and it just gives the sequence of random numbers to all the players. So, they have this same random sequence---and then, maybe additionally, they have a private one as well---but they have this shared random sequence. And then, in this shared setting, basically all the results are much nicer. Basically, we get nice results in the shared randomness setting, and mostly more complicated, weird results---or in some cases, we also just can't characterize what's going on---in the non-correlated case.

(01:53:16):
But in the correlated case, we specifically propose to use the correlated randomness to do these recursive calls. So, when I call my three opponents or two opponents and myself on the last round, I take the shared sequence of random numbers. I remove the first and call the opponents with that, with the remaining one rather than using the private one. And then, in the case where there is no shared randomness, we just use the private randomness instead. So, in some sense, it's almost the same program. I mean, there's some subtleties, but in some sense it's the same program. And the main difference is that, well, you feed them this randomness that's-

**Daniel Filan** (01:54:12):
You're giving the other person your private randomness, right?

**Caspar Oesterheld** (01:54:14):
Yeah. I'm giving... yeah, I don't have access to their randomness. I have to give them my randomness, which also, maybe it's not that hard to see that you get somewhat chaotic outputs. In some sense, my prediction of what the opponent will do is quite different from what they're actually going to do because they might have very different input.

**Daniel Filan** (01:54:44):
Right. In some ways it's an interesting... It's maybe more realistic that I get to sample from the distribution of what you do, but I don't get to know exactly what you will actually do. Actually, maybe this is just me restating that I believe in private randomness more than I believe in public randomness.

(01:55:03):
So, okay, here's a thing that I believe about this scheme that strikes me as kind of sad. It seems like, basically, you're going to use this scheme to come up with things like these ϵGroundedFairBots and they're going to cooperate with each other. But reading the paper, it seemed like what kind of had to happen is that all the agents involved had to use the same sort of time step scheme, at least in the construction. It's like, "Oh, yeah, everyone has this shared sequence of public randomness, so they're both waiting until the random number is less than epsilon and at that point they terminate".

(01:55:56):
So, I guess I'm seeing this as: okay, in the real world we do have public sources of randomness, but there are a lot of them. It's not obvious which ones they use. It's not obvious how to turn them into "is it less than epsilon?" or... So, it seems really sad if the good properties of this have to come from coordination on the scheme of "we're going to do the time steps and we're going to do it like this". But I guess I'm not sure. How much coordination is really required for this to work out well?

**Caspar Oesterheld** (01:56:30):
Yeah, that is a good question. Yeah, I do think that this is a price that one pays relative to the original ϵGroundedπBots, which obviously don't have these issues. I think it's a little bit complicated how robust this is exactly. So, the results that we have... We have this [folk theorem](https://en.wikipedia.org/wiki/Folk_theorem_(game_theory)) about what equilibria can be achieved in the shared randomness case by these kinds of programs. And it's the same as for repeated games, also the same as for these syntactic comparison-based ones. So, everything that's better for everyone than their minimax payoff, the payoff that they got if everyone else punished them. And I guess the fact that it's equilibrium obviously means that it's robust to all kinds of deviations, but getting the equilibrium payoff, that requires coordination on these random things.

(01:57:43):
Also, another thing is that---maybe this is already been implicit or explicit in the language I've used---with these times steps, there's a close relation between this and repeated games. Now, it's really just full repeated game strategies. And this whole relation to repeated games hinges on everyone using basically exactly the same time step scheme. Basically, if everyone uses the same epsilon and if the same source of randomness is below this epsilon, then in some sense, it's all exactly playing a repeated game with a probability of epsilon of terminating at each point. And there's a very nice correspondence. So, some of the results do really fully hinge on really exact coordination on all of these things. But also, there's some robustness still.

(01:58:42):
So, for example, the programs still halt if someone chooses a slightly different epsilon. If someone chooses a different epsilon, the relationship to repeated games sort of goes away. It's hard to think of a version to play a repeated game where everyone has their separate cutoff probability. I don't know. Maybe one can somehow make sense of this, but it does become different from that. But let's say I choose an epsilon that's slightly lower. Well, we're still going to halt at the point where we find a point in this random sequence where it's below everyone's epsilon. So, people choosing slightly different epsilons, it becomes harder for us to say what's going on, we can't view it as a repeated game anymore, but it still works. It's not like everything immediately breaks in terms of everything not halting or something like that.

**Daniel Filan** (01:59:54):
Yeah. Or even if I'm using one public random sequence and you're using another, even if it's uncorrelated, it seems like as long as I eventually halt and you eventually halt, it's not going to be too bad.

**Caspar Oesterheld** (02:00:06):
In particular, we're going to halt at the point where both of our sequences have the halting signal, right?

[Note from Caspar: At least given my current interpretation of what you say here, my answer is wrong. What actually happens is that we're just back in the uncorrelated case. Basically my simulations will be a simulated repeated game in which everything is correlated _because I feed you my random sequence_ and your simulations will be a repeated game where everything is correlated. Halting works the same as usual. But of course what we end up actually playing will be uncorrelated. We discuss something like this later in the episode.]

**Daniel Filan** (02:00:14):
Yeah. I guess, it depends a little bit on what our policies are, but it seems like as long as I'm not super specific about what exact sequence of cooperates and defects I'm sensitive to, maybe it'll just be fine even if we're not super tightly coordinated.

**Caspar Oesterheld** (02:00:41):
Yeah, I guess here again, [to try] to import our intuitions from repeated games, that I guess there's a game theoretic literature about, and that we maybe also have experience [of] from daily life: in practice, if you play a repeated game, you're not going to play an equilibrium, you're going to play something where you do something that's trying to go for some compromise. Maybe the other player goes for some other compromise, and then, you try to punish them a little bit or something like that. And I would imagine that there's a lot of this going on in this setting as well.

## Characterizing simulation-based program equilibria <a name="characterizing-sim-based-pe"></a>

**Daniel Filan** (02:01:22):
Yeah, yeah. Okay. I think I may be a little bit less concerned about the degree of coordination required. So, there are two other things about this paper that seem pretty interesting. So, the first is just what the limitations on the equilibria you can reach are. And my understanding is that you can characterize them decently in the correlated case, but it's pretty hard to characterize them in the uncorrelated case or-

**Caspar Oesterheld** (02:01:53):
Yeah.

**Daniel Filan** (02:01:54):
Can you explain to me and my listeners just what's going on here?

**Caspar Oesterheld** (02:01:58):
Yeah, so in the correlated case, it really is quite simple. As always, there are some subtleties. You need to specify, for example, what exactly are you going to do if you simulate some other player and they use their private signal of randomness, which they're not supposed to do in some sense. Well, you need to somehow punish them and the people above you need to figure out that this is what's going on. So, there's some of these sorts of subtleties. But I think basically, there is just a very close relationship between these programs and the repeated game case. So, it is just basically like playing the repeated case and even deviation strategies, you can view as playing the repeated game by saying: well, if they get this random string as inputs that has 10 variables left until they get to the below epsilon case, then you can view this as them playing a particular strategy at time step 10.

**Daniel Filan** (02:03:03):
Hang on. What do they do if they access randomness? So, my recollection, which might be wrong, was that you punish people for accessing other people's private randomness, but I thought they could still access their private randomness.

**Caspar Oesterheld** (02:03:18):
I think you do have to punish people for using their private randomness. And then, the complicated thing is that I might simulate you and you might simulate a third party and the third party uses their private randomness and now you, as a result, punish them. And then, I now need to figure out that you are just punishing them because they used their private randomness.

**Daniel Filan** (02:03:46):
And you're now punishing me.

**Caspar Oesterheld** (02:03:47):
I don't know.

**Daniel Filan** (02:03:50):
That condition seems hard to coordinate on, right? Because naively, you might've [thought], well, it's my private randomness. It's my choice.

**Caspar Oesterheld** (02:03:56):
Oh, the condition to punish private randomness?

**Daniel Filan** (02:04:00):
Yeah.

**Caspar Oesterheld** (02:04:00):
Yeah. I think this is a reasonable point. Maybe one should think about ways to make this more robust to this. I guess one has to think about what exactly the game is, and how much harm the private randomness can do. In some cases, it doesn't really help you to do your own private randomness, and then maybe I don't need to punish you for it.

(02:04:24):
But if there are 20 resources and you can steal them, and you're going to randomize which one you steal from, and the only way for us to defend against this is by catching you at the specific resource or something like that, then maybe we do just need to think: okay, as soon as there's some randomness going on, it's a little bit fishy.

(02:04:48):
But yeah, you could imagine games where you want to allow some people to randomize privately or use their private randomness for, I don't know, choosing their password. Maybe this is sort of a fun example. At time step 3, you need to choose a password. And in principle, the way our scheme would address this is that we all get to see your password, or in some sense we get to predict how you use your password. I mean it's also still important to keep in mind that these past timesteps are things that don't actually happen, so we predict what you would've chosen at timestep 3 if timestep 3 was the real timestep. But nonetheless, you might think, okay, if you have to choose your password with the public randomness, then we all know your password and doesn't this mean that we all would want to log into your computer and steal your stuff? And the way the scheme would address this, I guess, is just that, well, someone could do that but they would then be punished for this.

**Daniel Filan** (02:05:59):
Or maybe they do do it and it's just like, "Well, that's the equilibrium we picked. Sorry".

**Caspar Oesterheld** (02:06:04):
Right, right. It could also be part of the equilibrium. Yeah, that's also true.

**Daniel Filan** (02:06:11):
So in the correlated case, it's basically: you have a [folk theorem](https://en.wikipedia.org/wiki/Folk_theorem_(game_theory)), and there's something about things that you can punish people for deviating from. That's basically the equilibria you can reach, roughly. And then I got to the bit of your paper that is about the equilibria you could reach in the uncorrelated game.

(02:06:39):
And I am going to be honest... So earlier we had a recording where we were going to talk about these papers, but actually I got really bad sleep the night before I was supposed to read the papers, and so I didn't really understand this ["Characterising Simulation-based Program Equilibria" paper](https://arxiv.org/abs/2412.14570). It was beyond me. And this time, I had a good night's sleep, I was rested, I was prepared, and I read this paper and then once I get to the limitations on the equilibria of the uncorrelated one, that's where I gave up. The theorems did not make... I understood each of the symbols but I didn't get what was going on.

(02:07:19):
Is there a brief summary of what's going on or is it just like, well we had to do some math and that turns out to be the condition that you end up needing?

**Caspar Oesterheld** (02:07:26):
At least for the purpose of a very audio-focused format, I think probably one can't go that much into the details of this. I think I want to explain a little bit why one doesn't get a folk theorem in the uncorrelated case. I think there are some relatively intuitively accessible reasons for that.

(02:07:49):
Okay, let's start there. So the problem in the uncorrelated case is basically that: let's take a three-player case. We are two players and there's a third player, Alice. We want to implement some equilibrium and now there's a question, can Alice profitably deviate from this equilibrium? And now the issue is Alice can use her private randomization in some ways. So the problem is basically that us catching her deviation is uncorrelated with her actually deviating in the actual situation. And additionally, whether I detect her deviating is uncorrelated with you detecting her deviating.

(02:08:58):
And this all makes punishing, especially punishing low probability deviations very difficult. So for example, if Alice, with some small probability that she determines with her private randomness, she defects in some way, then in the real world, for her actual action that will determine her utility, there's this small probability that she'll defect. And then there's some probability that our simulations of her---which we're running a bunch of---there's some probability that we'll detect these. But because when I simulate Alice, I simulate her with a completely different random string than the string that Alice has in the real world, in some sense, I can't really tell whether she's actually going to deviate. And then also, you are going to simulate Alice also with your private randomness, which means that whether in your simulation Alice defects is also uncorrelated with whether she defects in my simulation.

**Daniel Filan** (02:10:07):
Wait, first of all, I thought that even in the correlated case, whether she defects in simulation is different from whether she deviates in reality because we get rid of the first few random numbers and then run on the rest, right?

**Caspar Oesterheld** (02:10:24):
Yeah, that is true.

**Daniel Filan** (02:10:28):
The thing where we disagree, that seems important and different.

**Caspar Oesterheld** (02:10:33):
So maybe that's... I'm maybe also not sure about the other one now, but I think the other one is more straightforward. It might be that to punish her deviating, we both need to do with a particular thing and we just can't... It's a little bit complicated because you might think, well, we can simulate Alice for a lot of timesteps. So you might think that even if she defects with low probability, we are simulating her a bunch in some way.

(02:11:12):
So they are some complications here. She needs to deviate in some relatively clever way to make sure that we can't detect this with high probability. It is all a little bit complicated, but I think we can't correlate our punishment or we can't even correlate whether we punish. And so if the only way to get her to not defect is for both of us to at the same time do a particular action, that's sort of difficult to get around.

**Daniel Filan** (02:11:49):
Okay. All right, here's a story I'm coming up based on some mishmash of what you were just saying and what I remember from the paper. We're in a three-player game, therefore punishing actions... Firstly, they might require a joint action by us two and therefore, that's one reason we need us to be correlated on what Alice actually did, at least in simulation.

(02:12:12):
Another issue is: suppose I do something that's not in the good equilibrium and you see me doing that, you need to know whether I did that because I'm punishing Alice or whether I was the first person to defect. And if I'm the first person to defect, then you should try punishing me. But if I'm just punishing Alice, then you shouldn't punish me.

(02:12:34):
And so if we in our heads see different versions of Alice, if you see me punishing, if you see me going away from the equilibrium, you need to know whether that's because in my head I saw Alice defecting or if it's because in my head I thought I want to defect because I'm evil or whatever. I don't know if that's right.

**Caspar Oesterheld** (02:12:58):
Yeah. I'm not sure whether that is an issue because when I see you defecting, it is because I simulate you with my randomness as input. And then you see, with my randomness as input, Alice defecting one level down, which means that I... Remember that I'm simulating all of these past timesteps as determined by my randomness. So I think I can see whether the reason you defect in my simulation is that you saw Alice defect.

**Daniel Filan** (02:13:40):
Wait, if we're using the same randomness, then why isn't it the case that we both see Alice defect at the same time with our same randomness?

**Caspar Oesterheld** (02:13:47):
So I mean this is all my simulation of you rather than the real you.

**Daniel Filan** (02:13:55):
So the real we's might not coordinate on punishment?

**Caspar Oesterheld** (02:13:59):
Yeah. I mean this is another thing that's like: even with very basic ϵGroundedπBots, you can kind of imagine: in their head, they're playing this tit-for-tat where it's going back and forth. And one person does this based on their randomness and then the other person sees this and then responds in some particular way.

(02:14:19):
But if you don't have shared randomness, all of this, this is all complete fiction. You haven't actually coordinated with the other player and seen back and forth. So it might be that I run this simulation where you detected Alice's defecting and then I also defect on Alice, and then we are happily defecting on Alice. And in the simulation we're thinking "we're doing so well, we're getting this Alice to regret what she does" and so on. But the problem is that you run a completely different simulation.

(02:14:52):
So in your simulation of what Alice and I do, you might see everyone cooperating and everyone thinks, "oh, everything's great, we're all cooperating with each other". And then we've done the simulation and now we are playing the actual game, and I defect thinking, "oh yeah, we are on the same team against Alice". And then you think, "oh nice, we're all cooperating" and you cooperate. And then we're landing in this completely weird outcome that doesn't really happen in the simulation, sort of unrelated to what happens in this...

**Daniel Filan** (02:15:23):
Right. So Alice basically says, "Hey, I can get away with doing nasty stuff because they won't both be able to tell that I'm doing the nasty stuff and therefore I won't properly be punished in the real world". And so these gnarly theorems: should I basically read them as: the preconditions are there's some math thing and the math thing basically determines that this kind of thing can't happen and those are the equilibria you can reach. Is that it?

**Caspar Oesterheld** (02:15:50):
Yeah. So I think one thing that drives a lot of these characterizations is: Alice can defect with low probability. I think usually that's the more problematic case, is that she defects in a particular kind of clever way with low probability, which means that we are very unlikely to both detect it at once. I think that is driving these results a lot.

(02:16:23):
But to some extent... You said this earlier, there's some math going on. I think to some extent that's true. So I think one thing that I liked about these results, despite... I mean of course one always prefers results that are very clean and simple, like [the folk theorem](https://en.wikipedia.org/wiki/Folk_theorem_(game_theory)) where you just have this very simple condition for what things are equilibria. And our characterizations are mostly these kind of complicated formulas.

(02:16:51):
I think one thing I like is that for some of these characterizations, one can still hold onto this interpretation of there being timesteps and you simulate what people do at previous timesteps and things like that. Which, it's sort of very intuitive that this works for the case where everyone plays nicely with each other and everything is correlated, and in some sense, we're playing this mental repeated game where we all use the same randomness and so we are all playing the same repeated game, and really the thing that is sampled is "which round is the real round?" It's clear that the timestep story works. And it's nice there that there are some results where you can still use this timestep picture. So that's one nice thing about the results. But yeah, it is unfortunately much more complicated.

**Daniel Filan** (02:17:49):
Fair enough. So another part of the paper that is kind of cool and that you foregrounded earlier is it has this definition of simulationist programs. And so earlier, you mentioned there was a definition of fair programs or something: maybe you are referring to this definition.

**Caspar Oesterheld** (02:18:11):
Yeah. In some sense, the paper has three parts: the one with the correlated case, with these generalized ϵGroundedπBots that pass on the shared random sequence. And then the uncorrelated case with the ϵGroundedFairBots. And then we also have a section that analyzes more general simulationist programs, which are programs that just... Intuitively all they do is run the opponent with themselves and the other players as input. And that has this definition. And then for those we have a characterization as well.

(02:18:55):
For example, one result that we also show is that in general, general simulationist programs are more powerful at achieving equilibria in the uncorrelated case than the ϵGroundedπBots. I'm not quite sure how much to go into detail there, but one intuition that you can have is: in the ϵGroundedπBots, to some extent everyone has to do the same thing. Whereas you could have settings where only I need to do simulations and then if only I simulate your program, I can run 10,000 simulations or something like that.

(02:19:35):
And this is something that obviously the ϵGroundedπBots can't do. You can't just independently sample a thousand responses from the other player. And we do have this definition of simulationist programs. I'm not sure I remember the details off the top of my head.

**Daniel Filan** (02:19:56):
I think it's some recursive thing of: a simulationist program is... it calls its opponent on a simulationist program, which maybe includes itself and maybe... I forgot whether it explicitly has ϵGroundedπBots as a base case or something. Maybe simulating nobody is the base case, or just ignoring the other person's input.

**Caspar Oesterheld** (02:20:20):
Yeah. That's also coming back to me. I think it's something like that. So the tricky part is that you might think that a simulationist program is just one that calls the other program with some other program as input. But then if you don't constrain the programs that you give the other player as input, you can sort of smuggle this non-behaviorism back in by having "what does my opponent do against these syntactic comparison bots?" or something like that.

**Daniel Filan** (02:21:01):
There's a good appendix. It's like "for why we do it this way, see this appendix". And then you read the appendix and it's like, "oh that's pretty comprehensible". It's not one of these cases where the appendix is all the horrible...

**Caspar Oesterheld** (02:21:11):
Yeah, glad to hear that you liked the appendix. Some of the appendix is also just very technical, like working out the details of characterization.

**Daniel Filan** (02:21:20):
Oh yeah, I skipped those appendices. But there are some good appendices in this one.

**Caspar Oesterheld** (02:21:24):
Nice.

## Follow-up work <a name="follow-up-work"></a>

**Daniel Filan** (02:21:24):
All right, the next thing I want to ask is: what's next in program equilibrium? What else do we need to know? What should enterprising listeners try and work on? Is there any work that's... So earlier, I asked you about what was the state of the art before you published ["Robust Program Equilibrium"](https://link.springer.com/article/10.1007/s11238-018-9679-3). Is there any work coming out at around the same time which is also worth talking about and knowing a bit about the results of?

**Caspar Oesterheld** (02:21:57):
I think, yeah, there are a bunch of different directions. So I do think that we still leave open various technical questions and there are also some kind of technical questions that are still open for these Löbian programs that it would be natural to answer.

(02:22:16):
So one thing, for example, is that I would imagine that... Maybe sticking closely to our paper first, there are some very concrete open questions even listed in the paper. I'm not entirely sure, but I think in the two-player simulationist program case, it's not clear whether, for example, all Pareto-optimal, better than minimax utility profiles can be achieved in simulationist program equilibria. So maybe this is not quite the right question, but you can check the paper. We have some characterizations for these uncorrelated cases. But I think for the general simulationist case, we don't have a full characterization. So if you want go further down this path of this paper, there are a bunch of directions there that still have somewhat small holes to fill in.

(02:23:39):
Then another very natural thing is that: I think for the Löbian bots, there isn't a result showing that you can get the full [folk theorem](https://en.wikipedia.org/wiki/Folk_theorem_(game_theory)) if you have access to shared randomness, which I am pretty sure is the case. I think probably with some mixing of this epsilon-grounded stuff and the Löbian proof-based stuff, I would imagine you can get basically a full folk theorem, but there's no paper proving that. Maybe one day, I'll do this myself. But I think that's another very natural question to ask.

(02:24:19):
So in my mind, going a bit further outside of what we've discussed so far, in practice, I would imagine that usually one doesn't see the opponent's full source code. And maybe it's also even undesirable to see the source code for various reasons. You don't want to release all your secrets. Maybe also... I mean, we talked about these folk theorems where everything that is better than this punishment outcome can be achieved. And I think game theorists often view this as sort of a positive result, whereas I have very mixed feelings about this because it's kind of like, well, anything can happen, and in particular a lot of really bad outcomes can happen. Outcomes that are better than the best thing that I can achieve if everyone just punishes me maximally... Well, it's not very good. There are lots of very bad things that people can do to me, so there are lots of equilibria where I get very low utility.

**Daniel Filan** (02:25:40):
And in particular, if there are tons of these equilibria, the more equilibria there are, the less chance there is we coordinate with one. Right?

**Caspar Oesterheld** (02:25:49):
Yeah. I guess maybe one positive thing is that... In the correlated case, you have this convex space of equilibria. So at least it's like, well, you need to find yourself in this convex space rather than finding yourself between six discrete points. And so maybe that makes things easier.

(02:26:08):
But yeah, I think basically I agree with this. I think on our last episode---this is my second appearance on AXRP, right? [On the first episode on AXRP](https://axrp.net/episode/2023/10/03/episode-25-cooperative-ai-caspar-oesterheld.html), we discussed this equilibrium selection problem, which I think is very important and motivates a bunch of my work. So maybe if you have less information about the other player, then you get fewer equilibria. Maybe in the extreme case, maybe if you get only very little information about the player, maybe you only get one additional equilibrium relative to the equilibria of the underlying game.

(02:26:53):
And I think we discussed [the similarity-based cooperation paper](https://arxiv.org/abs/2211.14468) also on the previous episode, and that is basically such a setting. It's basically a program equilibrium setting where you don't get the full opponent source code, but you get some signal, in particular how similar the opponent is to you. And there are some results about how you get only good equilibria this way.

(02:27:23):
I think in general, that's sort of a natural direction to go in. Also, you can also do more practical things there. The similarity-based cooperation paper has some experiments. You can do experiments with language models where in some sense, this is sort of true. If my program is "I prompt a particular language model" and then you know my prompt but you don't know all the weights of my language model, or maybe you can't do very much with all the weights of my language model, that is a sort of partial information program equilibrium. So I think that is another natural direction.

(02:28:03):
And then also, I think you drew these connections to decision theory, which is: in some sense, if you are the program and you have to reason about how you're being simulated and people are looking at your code and stuff like that, how should you act in some kind of rational choice-type sense? That's sort of the problem of decision theory. And in some ways, you could view this program equilibrium setting as sort of addressing these issues by taking this outside perspective. Instead of asking myself "what should I, as a program who's being predicted and simulated and so on, what should I do?", instead of that, I ask myself, "I'm this human player who's outside the game and who can submit and write code, what is the best code to submit?"

(02:28:59):
And in some sense, that makes the question less philosophical. I'm very interested in these more philosophical issues. And I feel like the connections here aren't fully settled: what exactly does this "principal" perspective or this outside perspective correspond to from the person of the agent? Like you said, this equilibrium where everyone checks that they're equal to the other player, that's an equilibrium where the programs themselves aren't rational. They don't do expected utility maximization, they just do what their source code says. So I think this is much more philosophical, much more open-ended than these more technical question about what equilibria can you achieve. But I'm still very interested in those things as well.

## Following Caspar's research <a name="following-caspars-research"></a>

**Daniel Filan** (02:29:49):
So the final question I want to ask is: if people are interested in this work and in particular in your work, how should they find more?

**Caspar Oesterheld** (02:30:00):
So I just have an [academic website](https://www.andrew.cmu.edu/user/coesterh/). Fortunately my name is relatively rare, so if you Google my name, you'll find my academic website. You can also check my [Google Scholar](https://scholar.google.com/citations?user=xeEcRjkAAAAJ&hl=en), which has a complete list of my work. I also have [a blog](https://casparoesterheld.com/) where I occasionally post things somewhat related to these kinds of issues, which is just [casparoesterheld.com](https://casparoesterheld.com/), which in principle should allow subscribing to email notifications.

(02:30:39):
And I also have an [account on X](x.com/c_oesterheld), formerly Twitter, which is C_Oesterheld. Yeah, I think those are probably all the things.

**Daniel Filan** (02:30:51):
Great. Cool. So there'll be links to that in the transcript. Caspar, thanks very much for coming on the podcast.

**Caspar Oesterheld** (02:30:56):
Thanks so much for having me.

**Daniel Filan** (02:30:57):
This episode is edited by Kate Brunotts, and Amber Dawn Ace helped with the transcription. The opening and closing themes are by Jack Garrett. This episode was recorded at [FAR.Labs](https://far.ai/programs/far-labs). Financial support for the episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. You can become a patron yourself at [patreon.com/axrpodcast](https://patreon.com/axrpodcast) or give a one-off donation at [ko-fi.com/axrpodcast](https://ko-fi.com/axrpodcast). Finally, if you have any feedback about the podcast, you can fill out a super short survey at [axrp.fyi](axrp.fyi).
