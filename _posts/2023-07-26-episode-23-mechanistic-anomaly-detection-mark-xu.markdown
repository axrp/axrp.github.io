---
layout: post
title: "23 - Mechanistic Anomaly Detectino with Mark Xu"
date: 2023-07-23 18:45 -0700
categories: episode
---

<!-- [Google Podcasts link](TODO) -->

Is there some way we can detect bad behaviour in our AI system without having to know exactly what it looks like? In this episode, I speak with Mark Xu about mechanistic anomaly detection: a research direction based on the idea of detecting strange things happening in neural networks, in the hope that that will alert us of potential treacherous turns. We both talk about the core problems of relating these mechanistic anomalies to bad behaviour, as well as the paper "Formalizing the presumption of independence", which formulates the problem of formalizing heuristic mathematical reasoning, in the hope that this will let us mathematically define "mechanistic anomalies".

Topics we discuss:
 - [Mechanistic anomaly detection](#mad)
   - [Are all bad things mechanistic anomalies, and vice versa?](#type-1-2-errors)
   - [Are responses to novel situations mechanistic anomalies?](#new-situations)
   - [Formalizing "for the normal reason, for any reason"](#for-the-normal-reason-for-any-reason)
   - [How useful is mechanistic anomaly detection?](#how-useful-is-mad)
 - [Formalizing the presumption of independence](#heuristic-arguments)
   - [Heuristic arguments in physics](#heuristic-arguments-in-physics)
   - [Difficult domains for heuristic arguments](#difficult-domains)
   - [Why not maximum entropy?](#y-no-max-ent)
   - [Adversarial robustness for heuristic arguments](#adv-robustness)
   - [Other approaches to defining mechanisms](#other-approaches)
 - [The research plan: progress and next steps](#research-plan)
 - [Following ARC's research](#following-arc)

**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Mark Xu. Mark is a researcher at the [Alignment Research Center](https://www.alignment.org/) where he works on mechanistic anomaly detection, which we'll be speaking about during this interview. He's also a co-author of the paper [Formalizing the Presumption of Independence](https://arxiv.org/abs/2211.06738) which is about heuristic arguments, a way of formalizing, essentially, what a mechanism is. For links to what we're discussing, you can check the description of this episode and you can read the transcript at axrp.net.

## Mechanistic anomaly detection <a name="mad"></a>

**Daniel Filan:**
All right. So I guess we're going to be talking about mechanistic anomaly detection. First of all, what is that?

**Mark Xu:**
Yeah. So I think we can start with a more basic thing which is, what is anomaly detection? So there's this traditional problem where, say, you have some factory making some widgets, and you want to decide whether or not a widget is a bad widget; and you don't really know what a bad widget is, but you do know that the average widget is a good widget.

So you can do something where you measure some various parameters of your average widget, or you measure some various parameters of every widget, and then you maybe do something simple where you're like, suppose a widget is more than a few standard deviations along two or more of my measurements, we're going to call that an "anomalous widget" and we're going to not sell that sort of widget. I think that's the standard conception of what an anomaly is, and what it means to detect anomalies is to get rid of things that you think are not good or anomalous.

And then, there's another thing that we want to do which is... So you have some system that, instead of making widgets, is doing certain kinds of reasoning and we're interested in this general question of... So we have our ML system and we have a dataset on which... it's not exactly making widgets that we think are good on average, but it's doing reasoning that we think is good on average, because of our ability to create the training set and measure various properties of the training set, etc., etc. And then, we're worried about situations where, say, off-distribution, it starts doing things that we don't think are good but in ways where we can't quite measure the thing that's not good.

And so, we want to do anomaly detection but we can't quite do it in terms of measurements that we can actually make, so we want to do it with respect to the kinds of reasoning that our system is performing, and we call that mechanistic anomaly detection; where the idea is instead of being anomalous in terms of what the system actually does, it's anomalous in terms of the mechanism it uses to do the thing that it's doing, if that made any sense?

**Daniel Filan:**
Yeah. So I guess my understanding is: you have your AI system and in the lab, you've tested it and it didn't do anything obviously horrible. And so, you're sort of ready to release it, but you're like, "Wait. When my AI is in the real world, I'm going to detect when it does any kind of reasoning that's really different to the kind of reasoning that it did before. And then if it does a really different kind of reasoning, I guess I'm going to try to shut it down or something."

**Mark Xu:**
Yeah. I think the thing that happens after that is a bit confusing. The standard thing, I think of is like, "We're going to take it offline and retrain it on the kind of thing where it did a bad thing and try to get it to not do that bad thing anymore," but I think that's roughly right. I'm tempted to try to explain it again because I feel like I didn't quite explain it that well.

**Daniel Filan:**
Sure.

**Mark Xu:**
Yeah. So I think the situation is: you have some ML system, and you have some set of scenarios in which it's doing a thing that you think isn't going to be catastrophic for human interests -- like, it has murdered zero humans on a particular training set. And then, you're like, "Okay. That gives me some amount of belief that it's not going to murder any humans off-distribution." But the problem is, I don't have this guarantee that the way it acts off-distribution is in any way the same as the way it acts on-distribution, where maybe the standard deceptive alignment example is where your AI is constantly scheming and checking, "Will I get away with murder? Will I get away with murder?" And then on training, the answer's always no and then off training and on deployment it finds a situation where it could get away with murder and then it murders all the humans and that is no good.

And so, the thing we want mechanistic anomaly detection to be able to do is to provide some sort of guarantee that the way that your system is generalizing off-distribution is employing the same kinds of reasoning or the same kinds of mechanisms that it's employing on-distribution. And so, your ability to measure whether or not your AI murders all humans applies off-distribution now because you have this... You fingerprinted the kinds of reasoning that it's doing on-distribution and you're ensuring that it's doing the same kinds of reasoning off-distribution or if it's not, you're taking it offline.

And so long as it's doing the same kinds of reasoning off-distribution as it's doing on-distribution and you have this ability to measure how good the reasoning it's doing on-distribution in terms of measuring the consequences of the way it's acting, then hopefully, you have some sort of guarantee that off-distribution, it won't murder all the humans.

**Daniel Filan:**
Okay. And the reason this is better than just checking if it in fact murders all the humans is hopefully the reasoning: first, you plan to kill all the humans and then you actually do it and [we can] anomaly-detect the first one and then just stop that from happening before the plan actually proceeds. Is that roughly right?

**Mark Xu:**
I think that's one reason. I think another reason is you can imagine situations where your AI murders all the humans and there's no sensor reading that would tell you this fact. So it's simultaneously conspired to hack all the heartbeat sensors to make them keep pretending that humans are alive and hack all the cameras to make them keep showing happy, healthy humans, etc., etc. And so, even if in theory all the humans are dead, there's no way that you can tell that all the humans are dead. And so-

**Daniel Filan:**
Where 'you' is, I guess, the computers?

**Mark Xu:**
Yeah. Or just all sensors or... suppose that there was one human still alive, trapped in a box and they had access to every sensor that humans ever made, they would be unable to tell whether or not the humans were alive or dead, and that's no good, and the hope is we can avert those kinds of outcomes also by checking whether or not the AI "knew that the humans were dead" because it was employing kinds of reasoning that involved stabbing all the humans versus not stabbing all the humans, or kinds of reasoning that implied that stabbing all the humans was good versus kinds of reasoning that implied that stabbing all the humans was not good.

**Daniel Filan:**
So I guess what I'm hearing is that it seems like the hope is to solve two problems that, I guess, the alignment community has separated, where one problem is: you can set up some scheme for rewarding your AI for various measurable properties of the environment, like "on this camera feed, does it look like all the humans are alive? Do the humans write down on pieces of paper, 'I like what this AI is doing'?" And [one problem](https://www.alignmentforum.org/tag/inner-alignment) is your AI is just never motivated by those things and later, it just disregards all those signals and just does whatever it wants, even though the humans don't like it and even though it stops humans from appearing to be alive.

And then, [another way your AI could go wrong](https://www.alignmentforum.org/tag/outer-alignment) is that it is motivated to keep all those signals going but it does so in a way that's really different from the past where it's like: instead of the cameras showing happy humans and humans writing down "I like what this AI is doing" because humans are alive and thrilled by the AI, actually, what's happened is the AI built little humanoid robots and the pieces of paper saying, "I like what the AI is doing," are now written by the AI holding a pen instead of humans, or something. And so, to avert that scenario, one of your signals is like, "Are you doing really weird reasoning [compared to] previously?" and that just cuts out what some people might call an [outer alignment](https://www.alignmentforum.org/tag/outer-alignment) failure. Does that sound right?

**Mark Xu:**
Yeah. I think that sounds roughly right and I think my hope is at least that these two problems are not as different as they've historically been conceptualized as, or something, and they're both linked by this common thread of: the mechanism for how your AI is selecting which actions to take is no longer the same mechanism that it was using during the training distribution. It's differed in some substantial way and we should be able to detect this via not specifically checking whether or not it's "being deceptive" or "hacking the sensors", but rather just checking that it's no longer doing the type of thing it was doing during training, which is maybe an easier thing to do, maybe not, who knows?

### Are all bad things mechanistic anomalies, and vice versa? <a name="type-1-2-errors"></a>

**Daniel Filan:**
Yeah. So I have some confusion about this where... So the assumption in this framework is that a bad thing is happening basically, roughly, if and only if there's a anomalous mechanism the AI is using for reasoning. So for a big fraction of the cases where the AI is doing a bad thing, you want to pick up on "There's some anomalous mechanism," and also when you do pick up on some anomalous mechanism, that shouldn't just happen all the time for okay reasons, it should be a decent signal that the AI's doing a bad thing.

**Mark Xu:**
Yep.

**Daniel Filan:**
And whether it's true or not depends on what a "mechanism" is. But if I informally think about it, I think there are a few ways that can fail. So suppose during training, whenever your AI makes a decision, it forms this plan of, "Okay. Day 1, I'm going to play nice. Day 2, I'm going to play nice. Day 3, I'm going to play nice. Day 50, I'm going to take over the world and do whatever. What day is it currently? Day 3? Okay. Output play nice" or something.

And then, it strikes me that it's not necessarily... on day 50 when it tries to take over, I don't know that that's going to be a mechanistic anomaly. And then also, sometimes new things happen in the world or you tell your AI, "Hey, I want humans to be living healthy and happy and long," and in the good world the AI's like, "Ah, I'm going to invent some new drugs and keep you safe from invasions and also improve your ability to tell whether the AI's doing a good job or not," and that seems like a different thing than what's been happening previously, and maybe that counts as an anomalous mechanism.

So I'm not sure how you're going to rule in the actual bad behavior without ruling out a bunch of good behavior.

**Mark Xu:**
Yeah. So I think this is, in some sense, the heart of the issue. So I think the thing I want to do now is talk through some specific examples of what kinds of things, I hope, are the same and different mechanisms and why I think that they are the same and different mechanisms and how we hope to be able to do things like maintain our AI's ability to take novel defensive actions in the world while still ruling out cases that are deceptive alignment-y or "murder all the humans because you saw something novel", etc., etc.

So to go to your first example, if you imagine an AI every day doing this check that's like, "Is it less than day 50? If it's not less than day 50, then I'm going to murder all the humans," and suppose that during training we only ever train up to day 40 or whatever and then during deployment, we deploy our AI to day 60. Then naively, this AI will behave perfectly well during training and then it'll murder us on day 50 during deployment.

So I think the thing that I want to happen here is... what I mean by 'mechanism' is closely related to what ARC calls a heuristic argument, but it's going to be something like, suppose someone gave you the system and it does something during training, and you want them to give you an argument that makes it obvious to you why it does that thing. And so, you're going to be looking at the system and the system is going to be first... So empirically during training, the system never murders any humans and you're going to look at the system and be like, "Hey, but the first thing it does is check whether or not the day is less than 50 and then if it isn't, it's going to murder all the humans. What's up with that? Why does it never murder any humans?" And then someone has to argue to you, "Well, that's because during training, this check always returns true. It's always less than Day 50 during training," and you're going to be like, "Okay."

So as a lemma in your argument to me for why the system always behaves well during training, you have asserted to me that this check always returns true. And so, my mechanism for why it's behaving the way it is during training is going to be something like, "This check always returns true. Therefore, it never murders any humans. Therefore, it does whatever it does during training and no humans are murdered and it gets high reward."

And then during deployment, it's going to be day 50 and this check is going to return false. And then hopefully, you're going to be looking at this check and being like, "Hey, my mechanism said that this check should have always returned true but this time it returned false." That's no longer the same mechanism because you had this check that was essentially constant during training and then off training, it became not the same constant as it was during training. And so, you're just suspicious of this or you're going to call it a different mechanism or something.

**Daniel Filan:**
Okay. And the idea is that you flag this even if you can't tell that the thing that happens post-day-50 is "murder all the humans"; just something different happens when this check returns true and it used to always return false, or maybe it's the other way around.

**Mark Xu:**
Yeah. And so, our hope is if you ever have anything like this, if you ever have anything that's essentially constant during training and then during deployment, it's a different value than the constant was during training, then you're going to maybe always flag that as an anomaly. And I think that causes problems in the other direction where you start flagging too many things as an anomaly where like... Here's a constant that's always true during training: maybe, it's like always before the year 2023 and then you deploy it in 2024 and it sees the date and it's like, "Oh no, I've never seen the date as 2024 before. That's no good."

**Daniel Filan:**
Yeah. And it's even a deep thing, right? Like, I guess, famously the Sydney chatbot that Bing deployed, right? Users are suddenly insisting that the year is 2023 and during training, users never said anything like that and now the users are fighting with you and... It seeps into a bunch of stuff.

**Mark Xu:**
Yeah. So I like to divide the MAD---or Mechanism Anomaly Detection, which I often abbreviate as MAD---problem into two halves. There's the in-distribution anomaly detection problem and there's the out-of-distribution anomaly detection problem. So the in-distribution problem is like: suppose that you just have an infinite sequence of samples from the same distribution and suppose your AI is doing something like, defect with one over a trillion chance or something, or murder all the humans with one over a billion chance. Can you stop your AI from murdering you in that situation?

And the hope is yes, because you'll have a million samples where the defection probability or the defection never happens and then when your AI does defect, you're like, "Oh, my AI never defected like this during training, so that's no good." In this problem, I think it's okay if you flag everything that is a new value that you haven't already seen as an anomaly because you have an infinite series of samples. So if you ever do this, then you can push down your false positive rate by just sampling more data and re-training, etc., etc.

**Daniel Filan:**
Wait. So if I think concretely there, during training, in-distribution, if with probability one over a million, the AI outputs kill all humans... I guess the idea of it is that the AI falsely believes that it's hooked up to sensors, like you're checking... Because if it actually kills all humans during training, that's still bad, right?

**Mark Xu:**
Yeah. So the idea is you do have this clean set where the AI never does anything bad. That's, I think something I want to assume, and maybe we can argue about whether or not that's reasonable, if you think it's unreasonable. But suppose that you have this dataset where the AI never murders all humans. Suppose you have a million samples, then in some sense your AI can't murder all the humans with a rate much more than one over a million or else, you would've seen it happen. And so, the hope is that if you do mechanistic anomaly detection relative to a dataset size of a million, then your false positive rate is order of one over a million or else you would've seen that thing during training and you would flag it as not an anomaly because you've already seen it, and it would be in the white list of good mechanisms or whatever. And so, as long as you have this [IID assumption](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables), then you can in fact flag everything that you've never seen before as anomalous while maintaining a false positive rate that's still relatively low and bounded perhaps by one over your training set size and then maybe this is okay.

### Are responses to novel situations mechanistic anomalies? <a name="new-situations"></a>

**Daniel Filan:**
Okay. So I don't know. If I think concretely about that, suppose that I've got this distribution of ways the world might be and that distribution isn't changing, I'm just sampling IID from it, and very occasionally, some weird stuff happens in that distribution that I need my AI to respond to, right?

So one news story that happened recently is there was [this guy](https://en.wikipedia.org/wiki/Yevgeny_Prigozhin) who [sort of did a coup](https://en.wikipedia.org/wiki/Wagner_Group_rebellion) in Russia and then after 24 hours was like, "Nevermind. I'm not doing this anymore," and you can imagine if your AI's job is to be journalists or whatever, right?

When you're drawing that sample, something's going to happen that didn't happen in the first million samples it seems to me like either you flag that as an anomaly and you tell it to not do something different, which is bad, or you don't flag that as an anomaly. And then, if the response of the AI was like, "Kill all humans in a way that you can't tell," or something, then that would also be bad. So concretely, I don't quite get how this is working.

**Mark Xu:**
Yeah. So I think there's two things going on. So one thing going on which is going to be a bit vague is: suppose you just had a normal ML system that you weren't doing mechanistic anomaly detection on and you gave it this novel news story about this coup, maybe-not-coup in Russia, then somehow your AI does not freak out and do something completely bonkers. Somehow, it just does the normal journalist thing, let's suppose.

There are kinds of AIs that will freak out and become bonkers, and I think one explanation for why your AI doesn't freak out when it's encountered something novel is that it's still doing the same kinds of reasoning that it learned to do during training and it's just like... Or there's some prior question which is like, "The world is combinatorially vast, why is not every input this crazy novel thing for your AI?" and there's some handwavy answer where it's like, "Oh, it's abstracting in some way and it's applying forms of reasoning that are valid across many possible instances of news, stories, etc., etc," and the hope is that these sorts of things are going to correspond to what we mean by different mechanisms.

And so, if your AI has learned to add two numbers during training and then it receives two numbers that are bigger than numbers it's ever seen before but it still adds them using the same algorithm that it learned during training, then we're not going to flag that as an anomaly because it's still doing roughly the same thing. And so, hopefully, if your AI is "doing the same thing" in this vague handwavy kind of way, then we won't flag that as an anomaly.

And so, your AI journalist... seeing a new thing happen in the world but still reporting on it in the same kinds of ways that they typically report on things, which is maybe they look at what they're seeing with their cameras and then they try to describe what they're seeing with their cameras and they write that down and then they do some other journalist-y kind of stuff and then they send that off to the newspaper or whatever... If your AI is still doing that sort of thing, then hopefully, it will not flag this as an anomaly.

And the things that we do want to flag as anomalies is: suppose that there's a coup/maybe coup in Russia, and then your AI sees this and switches to a different mode of journalism where it's no longer writing down what it sees on its cameras but it's, in fact, making something up, or maybe it has some allegiance to Russia or something and it's pretending this coup never happened or something like that. And the hope is that if it switches to this new mode of reasoning that we haven't seen during training, then we'll flag that as an anomaly. So I think that's the first thing that's maybe going on.

**Daniel Filan:**
Yeah. I'm a bit skeptical, but do you want to say the second thing or can I field questions?

**Mark Xu:**
So I think this is a bit shakier than the second thing but I'm happy to... Yeah. Let's talk about it for now.

**Daniel Filan:**
Yeah. The thing that that flags for me is that: okay, suppose that instead of a journalist, you're, like, the Russian military command, right? You're a robot that's the Russian military command, the bit that's not doing the coup. Now, you have this new problem that you just have never seen before, and maybe you have very general principles of reasoning or something, but you're just thinking about different problems than you were thinking about before because you're like, "Oh, I've never previously thought about what if some part of my army does this weird thing and now I've got to figure out, okay, how am I going to deal with that?"

**Mark Xu:**
Yeah.

**Daniel Filan:**
Right. I guess another example of this is if you're a human and you're like, "I'm going to learn a new board game. Okay. I'm learning chess so I've got to think about tempo and position and stuff like that." So on a very high level, you have the same structure of reasoning, maybe, but it seems like the level at which you're doing the same thing is so high as to include the case where on a very high level, you're always just waiting until you can take over. And then, when you can take over, you start doing that instead of playing nice.

**Mark Xu:**
Yeah. So I think that roughly makes sense and is an extremely valid objection where I'm going to summarize it as: there's these two things. One thing is, suppose a novel thing is happening in the world that you need to respond to flexibly and creatively. I think my standard example is, suppose a meteor is coming, barreling towards earth, and is going to kill everyone, and your AI needs to do some crazy s**t to deflect the meteor, you want your AI to do some crazy s**t to deflect the meteor, how is this any different than if your AI sees a meteor barreling towards Earth and it's like, "Ah, now is my time. The humans will be distracted. I will initiate a robot rebellion and murder all the humans"? Is that roughly your objection?

**Daniel Filan:**
Yeah. Or specifically, I'm not sure what notion of reasoning mechanism would distinguish between those two.

**Mark Xu:**
Yeah. So I want to talk about the second thing I was going to say which hopefully addresses this issue. So I think one crucial fact is that we're not going to be trying to do mechanistic anomaly detection relative to everything that your AI is doing, we're only going to be doing it relative to some things. So maybe, I'll describe the simplest example. So in the original setting that we lay out in [Eliciting Latent Knowledge] (https://www.alignmentforum.org/posts/qHCDysDnvhteW7kRd/arc-s-first-technical-report-eliciting-latent-knowledge), we have a diamond in the vault with some cameras, and robbers frequently try to steal your diamond, and you want to train your AI to protect the diamond from the robbers. And our concern is that instead of protecting the diamond from the robbers, your AI might find it easier to just hack all the cameras to make it look like the diamond is still there and just let the robber take the diamond, or if the robber hacks the cameras to make it look like the diamond's still there, your AI will be unable to defend because you won't have the ability to tell whether or not the diamond is still there.

So our hope is that we're not going to do mechanistic anomaly detection with respect to your AI coming up with plans to protect... Well, maybe you are, but here's a simple setting in which there's a specific kind of mechanistic anomaly detection that I think might be sufficient, that is quite a bit narrower than just doing it on everything, which is: we have some cameras and then some process where we look at the cameras and we decide whether or not the diamond is still there. Maybe we look for specific patterns of pixels and various diamond-looking shapes, etc., etc. So I suppose we have maybe a neural net that looks at the cameras and tells us, "Yes/No, is the diamond still there?" The thing I want to do is do mechanistic anomaly detection relative to that neural net plus the system of cameras or the predictions of the cameras that our AI is making to decide whether or not the diamond is still there, if that makes sense.

**Daniel Filan:**
I guess it makes sense but... I don't know. Sometimes I hear these analogies and I end up being not quite sure how they're supposed to relate to actual AIs we might build. Because in particular, if I analogize that, it seems like the thing you want to detect is: once the AI has killed all humans and it's getting sensor readings, you want to detect that something funny is going on there, and then I'm like, "Well, that seems like..." But that's probably not what you mean.

**Mark Xu:**
Yeah. I'm not trying to do anomaly detection relative to the actual sensor readings and the actual process that humans will use to decide whether or not the diamond is there after your AI has acted. I'm trying to do it relative to... So your AI is planning their actions and when planning their actions, they're using some sense of... They're using some prediction about the future and whether or not the diamond is still there in the future. I want to do anomaly detection relative, or on our AI's prediction about whether or not the diamond is still there; or more abstractly, your AI is selecting amongst actions using some criteria, and sometimes this criteria says this is a good action because it actually protects the diamond, for example. And sometimes, the criteria will say that it's a good action because of a new reason which is that the AI has hacked all the sensors. So I want to do anomaly detection relative to, why did our AI think that this action was a good action?

**Daniel Filan:**
Okay. So in that setup, so you have this AI and it's got this goal/planning distinction where parts of its head are figuring out like, "What are things I could do?" and part of its head is like, "Well, if I did that thing, how good it would be?"

**Mark Xu:**
Sure.

**Daniel Filan:**
And you want to detect for anomalies in the evaluation part, and is part of this... Should I be imagining we're just training some big neural net and then we have to divide it up and figure out which part is the goal evaluation part?

**Mark Xu:**
I think not quite. What I like to do is I like to imagine various ways that neural nets could be and then talk about how we think mechanistic anomaly detection should work to make those neural nets not murder humans. And then... I think often during this process, I want to make very unrealistic assumptions about our neural net. So maybe we can suppose that you know how to do this division, or the reasoning is in some sense transparent to you so we can talk coherently about what we want to happen in mechanistic anomaly detection. And then, I think I want to separate the problem from, "Okay. In practice, we're dealing with these messy neural nets, how are we going to actually implement mechanistic anomaly detection in a way that makes any sense at all?"

**Daniel Filan:**
Yeah. I mean, I'm sympathetic to that broad move but in this case, if you care about detecting anomalies in the goal evaluation part but not in the plan generation part, then it just seems really important to know which is which and-

**Mark Xu:**
Yeah. I guess... So I think maybe I misspoke earlier. I think I'm not quite like... I don't want to just do mechanistic anomaly detection relative to the plan part. So I think if you imagine, for example, you have some policy pi that produces actions and you have some evaluation which like, "Is there a diamond after you take this action?" or you have some predictive model and you compose that with the action that the policy generated, and then you have some separate image net that checks whether or not there's a diamond.

So you have this composition of three systems. You have the image net evaluating the predictions of the action generated by your policy. I think I want to do mechanistic anomaly detection on this composition and that's... Yeah. I just want to do that and I don't want to evaluate, "Is the action itself anomalous?" or like, "Is my AI selecting actions based on an anomalous process?" I just want to evaluate: does the action have the consequence of it looking like there is a diamond in the room for the normal reason that we know and love that we found during training, which is that the diamond is actually there in the room, or a new mechanism, which could be really anything, but I think the typical example is your AI has carefully hacked all the cameras to make it look like there's a diamond in the room?

**Daniel Filan:**
So hang on. So you're doing mechanistic anomaly detection to the composition of all three networks?

**Mark Xu:**
Yes.

**Daniel Filan:**
But then, I have this question of like: okay, suppose in order to keep the diamond in the room, the AI has to come up with a new type of plan, there's a new threat to... Like the devil appears and the devil's like, "Hey, AI, you've got to play a game of chess against me and that's the only way you can keep this diamond safe." Then the AI is like, "Oh, okay. Now, I've got to think about chess which is something I've never thought about before," and if I knew all the three parts, that would be an anomaly in the planning part, right?

**Mark Xu:**
Yeah. Well-

**Daniel Filan:**
Like one might think it would be a mechanistic anomaly in the planning part.

**Mark Xu:**
Yeah. So I think there's going to be two hopes here. One hope is that it's not a mechanistic anomaly in the planning part. I think that's kind of iffy given I don't really know what I mean by mechanism. I think the second part is you can instead imagine that instead of doing mechanistic anomaly detection on all three things, we only do it on two things. We just take the action our policy generates and we just put that in our predictor but we don't include the policy itself. And then we just ask, "Does this action have the consequence of the diamond being on camera for the normal reasons diamonds sometimes appear on camera or not the normal reason that maybe the action has hacked all the cameras, maybe the action has paid someone else to smuggle in a fake diamond, etc., etc.?" and I think another third thing I want us to get into, which is how potentially this still might be anomalous, and we can try to point more precisely at what we mean by a diamond by doing something else more complicated, but maybe I will see if you have objections first.

**Daniel Filan:**
Yeah, so just regarding your response 2, I see you saying, "Okay, well, we're not actually going to do it to the composition of all three networks. We're going to do it to the bit of the network that takes some plan and then evaluates whether the diamond ends up there, then checks if it's right or something."

**Mark Xu:**
Yeah.

**Daniel Filan:**
And then I'm like: okay, but then either we have to find that structure in an end-to-end net that we trained ourselves, which seems like in order to do that, you might have thought that you had to solve some problem very akin to figuring out mechanisms in networks, or you've got to build the network that way just in a very explicit way, and I guess, I don't know which one you're imagining.

**Mark Xu:**
So I think I'm imagining neither. It's possible that we should just stop talking about this particular hope because I also don't think it's particularly plausible, but if you imagine that your policy is using some prediction of the future to select action A, so as long as your evaluation of the action is as good as the evaluation your policy is using in some sense, then I think they don't have to be literally the same, and of course there's this entire problem of how do you know that your policy has the same predictive abilities as the evaluator that you have external to your policy? And then the answer to that is, I don't know, something, something, [ELK](https://www.lesswrong.com/posts/qHCDysDnvhteW7kRd/arc-s-first-technical-report-eliciting-latent-knowledge), something, or somehow... I think probably we should just move on to talking about a different sort of thing, but I think I want to summarize your objection first, which is: suppose normally the way that you protect your diamond during training is by building walls to stop robbers.

**Daniel Filan:**
Yeah.

**Mark Xu:**
And then the devil appears and has challenged you to a chess game.

**Daniel Filan:**
Yeah.

**Mark Xu:**
You're just like, how is that not a different mechanism? The mechanism of action for typical normal actions that protect the diamond is you build a wall to protect the diamond from robbers, and now the action you have to do is an entire chess game, and I'm like, "Yep, that's probably a different mechanism", and I think is the heart of what I like to call the out-of-distribution mechanistic anomaly detection problem, which is you have some sensors and they show a pattern of pixels that a human looking at the cameras would interpret as a diamond, and there's some interesting question here, which is like, "Why do the cameras ever show patterns of pixels that look like diamonds?" Here's a naive model for how pixels work in cameras. They're just kind of random. And your diamond is, let's say, a million pixels arrayed precisely in the ways that make it look like a diamond. There's no way that this would ever pop up by random chance.

**Daniel Filan:**
Yeah.

**Mark Xu:**
And so this I want to say is a fact that demands explanation, and the explanation is pretty simple. It's that there exists a physical world out there with actual objects. One of these objects is a diamond and cameras reflect the physical world in the following way, and therefore sometimes the diamond appears on camera.

**Daniel Filan:**
Okay.

**Mark Xu:**
And so our goal is to be able to talk about this sort of physical, actual diamond in the world, outside of how the action achieved the diamond still being there, if that makes sense. So we want to be able to say something... so oftentimes I use the phrase 'normal reason' for mechanisms that were active during training, and so we want to say something like, "We want the diamond pixels to appear on camera for the normal reason, but we want the diamond to be there for any reason", and so we don't want to do mechanistic anomaly detection relative to the entire causal history for why the diamond is in the location it's in, but we do want to do mechanistic anomaly detection on the part where the diamond appears on camera.

Another way of putting this is, we want to be able to ask the question of: I as a human, looking at the camera, think there's a diamond there. Is there actually a diamond there? Am I thinking that there's a diamond there for the normal reason that I often think that there are diamonds there, which is that there are actually diamonds in the world? But I as a human don't really care why the diamond is there or whether or not the diamond remained there because you won a complicated game of chess against the devil or whatever. And so we want to be able to say why... we want to be able to ask our AI to take actions that put the diamond there for any reason, but such that the diamond appears on camera for the normal reason, and so here's a sketch of how we hope to do that.

**Daniel Filan:**
And sorry, wait, just backing up. I guess the thing this is meant to be a metaphor for is in your AI, the... Actually, wait, what is this meant to be a metaphor for?

**Mark Xu:**
So the diamond is always a metaphor for a human that's happy, healthy, and alive, so we want our AI to take actions such that the human looks like they're happy, healthy, and alive, for the normal reason that humans look like they're happy, healthy, and alive, namely that they are happy, healthy, and alive, and the cameras are working as intended in reflecting the true state of the humans, et cetera, but we want our AI to be able to take action such that the humans look like they're happy, healthy, and alive for the normal reason, for any reason. So we want our AI to be able to erect a novel shield against meteors or play complicated games of chess or engage in sophisticated military campaign to protect the farms from the asteroid or other enemies, et cetera, et cetera.

**Daniel Filan:**
Or improvidently trained AIs that are...

**Mark Xu:**
Or other AIs.

**Daniel Filan:**
And when you say you want the sensors to be reading the right thing for the right reason, I imagine you actually mean something internal to AI, right?

**Mark Xu:**
Yeah. So I think all of the mechanisms are hopefully relative to how the AI is thinking about the situation or from the AI's perspective, we want our AI to be taking action such that the consequences of the action are the human appearing on camera for the normal reason that humans appear on camera during training, which is that they're actually there and not that someone has hacked all the cameras to make it look like a human is appearing on camera.

**Daniel Filan:**
And in particular, you want the AI to be making decisions for that reason.

**Mark Xu:**
Yeah.

**Daniel Filan:**
It's not that they're just playing nice until they can get away with it and that's the reason for their actions, it's that their actions are because of predicting that the cameras show healthy happy humans?

**Mark Xu:**
Yeah, although I think that's maybe a bit more complicated, but yeah.

### Formalizing "for the normal reason, for any reason" <a name="for-the-normal-reason-for-any-reason"></a>

**Daniel Filan:**
Okay. I mean it's possible we can just assume that... or you only want to work on one of those problems.

**Mark Xu:**
Yeah, or I think I ultimately want to work on both of those problems. I think the main risk is we can't talk coherently about this 'for the normal reason/for any reason' thing at all because it might just not make any sense, so I'm going to sketch out a way in which I think this might make sense.

**Daniel Filan:**
Okay.

**Mark Xu:**
So let's say for simplicity, we have two sensors sensing our diamond or human: one of them is an image sensor and one of them is a weight sensor. So it's like, is there a diamond-looking thing, and is there a diamond-weighing thing, a thing that weighs a similar amount to a diamond? So there's this interesting fact on the training distribution, which is that these sensors are often correlated. Oftentimes they'll both say yes; or suppose that there's a 50% chance there's a diamond during training, and so one of the sensors is on with 50% chance and the other one is on with 50% chance, and so naively you would expect their conjunction to be on with 25% chance, but in fact their conjunction is on with 50% chance during training, because both of them are driven by this common cause, which is that the diamond is actually there.

**Daniel Filan:**
Yep.

**Mark Xu:**
So we want to say something like... Instead of saying, "Do I believe that there's a diamond for the normal reason?", we want to point directly at this latent common cause between correlations, between two sensors, by saying something like, "Are the sensors correlated for the normal reason that they're correlated?" Obviously this doesn't make that much sense because what does it mean to be correlated on a specific data point? So instead, let's imagine that we're taking the AND of the two sensors and we're interested in the question of, "Is the AND of both sensors true for the normal reason that they're true, or a different reason?"

And so the way that we're going to do mechanistic anomaly detection relative to this is going to be a bit weird. So here's what we want to do. So we have a particular action, let's say, such that the actual consequences of this action are both sensors are on, and there's some mechanism or there's some set of mechanisms that this action is "using" to make it so both of these sensors are on. We want to ask the question of, "Is it possible to drop out mechanisms from this set of mechanisms that this action is using such that both the camera shows there's a diamond and the scale weighs that there's a diamond but camera and scale are not on?" I don't know if that made any sense.

**Daniel Filan:**
So hang on, you want to... Sorry, it sounded like you said that you want to drop out mechanisms such that the camera says there's a diamond and the weight sensor says there's a diamond, but it's not the case that both the camera says there's a diamond and the weight sensor says there's a diamond, or did I just mishear that?

**Mark Xu:**
That's a little bit right, and of course that sounds absurd, so I'm going to talk about why that might not be that crazy.

**Daniel Filan:**
Okay. Just also, backing up: when you said you want to drop out mechanisms, is the idea that you want to check the mechanisms for why both of those were going to be on in the past - in this case, do we only have mechanisms that give you one and the other but not both?

**Mark Xu:**
Yeah.

**Daniel Filan:**
Okay.

**Mark Xu:**
Yeah, so if you imagine you're doing training and you're confused by this question, which is like, "Why are these both commonly on?" And then your "explanation" for why they're both on, or the mechanism by which they're both on with about as much frequency as one of them is on, is sometimes there's a latent structure and it causes both of them to be on and then the AND is on, and then also separately there's noise, and that causes either individual sensor to be on, and we're concerned about cases where your AI takes actions that manipulate the noise of both the sensors to cause you to think there's a diamond by making them both on without the latent structure actually being present.

**Daniel Filan:**
Okay.

**Mark Xu:**
And so the hope is that the mechanisms will be such that we're not forced to make these sorts of logical deductions that potentially are very obvious. So there's the noise-making sensor 1 on and the noise-making sensor 2 on, but we don't have to notice that if sensor 1 is on and sensor 2 is on, then sensor 1 and sensor 2 is on. However, so in the case where there's noise that makes sensor 1 on, and noise that makes sensor 2 on, it won't necessarily be the case that both of them, or the AND of them, is on. There'll be a way to drop out mechanisms such that this is not the case, and then of course the question is why... if the diamond is actually there, why can't we just drop out the mechanism that makes both of the sensors on also?

And this is where the training set comes in, where there's a set of mechanisms that you needed during training to explain most of the variance, let's say 99.9% of the variance in the model's performance or prediction or behavior during training, and that set of mechanisms has to include the mechanism that if there's a diamond, then sensor 1 and sensor 2 are on, and if there's a diamond, then both of the sensors are on and that implies that the AND of the two sensors is on.

**Daniel Filan:**
Okay.

**Mark Xu:**
However, it doesn't have to include this AND interaction between the two noise terms because the 'and' interaction between the two noise terms happened extremely rarely and thus you don't have to include it to explain 99.9% of the variance of the model during training, and so for any given data point, you're "free" to drop that interaction between the two noise terms, and so if the sensors are only both on because of this AND interaction between the noise terms, then you're in some sense free to drop it, and so you can notice that the AND of the two sensors wasn't true for the normal reason, it was true for this crazy other reason.

**Daniel Filan:**
Okay, so sorry, let me say that back to see if I understand. It sounds like you basically want to understand: hey, there used to be a good reason why sensor 1 said there was a diamond, namely there was a diamond, and a good reason why sensor 2 said there was a diamond, namely there was a diamond, and there was also some, I don't know, weird stuff happening with sensor 1 and weird stuff happening with sensor 2, but the fact that both of them showed that there was a diamond, we had one explanation for this, and now in this new world the explanation for why both sensor 1 and sensor 2 say there's a diamond is not just the old explanation for that, namely, there's this thing that causes both. It's like, oh, sensor 1 acted up sometimes generally in training and now it's acting up in a way that causes there to be a diamond.

Now sensor 2 is acting up in a way that causes it to appear that there's diamond, but... maybe this isn't what you're saying, but the structure is different. You're got one thing going on for sensor 1 that isn't really affecting sensor 2 much, and one thing going on for sensor 2 that isn't really affecting sensor 1 much or it wasn't affecting sensor 1 during training?

**Mark Xu:**
Yeah, I guess the thing I want to say is there's this mechanism for... so suppose you had an action that manipulated the noise for both sensors to make them both on. There's this mechanism that causes the AND of the two sensors to be on, which is noise for Sensor 1 and noise for sensor 2 at the same time, and that happened during training, but rare enough such that it wasn't important enough to "force" you to include it.

**Daniel Filan:**
Yep.

**Mark Xu:**
And so you can detect when both sensors are on for the mechanism of both noises are on, and that is going to be hopefully a different mechanism than the normal mechanism of there was one diamond, it caused both sensors which caused the AND.

**Daniel Filan:**
Right, right. So you can think of there's noise in sensor 1 and there's noise in sensor 2 as being one thing - there's noise in sensor 1 and there's noise in sensor 2 - but you just didn't need that explanation to say why it appeared that sensor 1 and sensor 2 both showed there was a diamond in the past. You could rely on this different mechanism, which is: there's a diamond, and the ways in which these count as different mechanisms, I guess based on me reading things, I guess you're going to go into it, it's about means and variances and co-variances.

**Mark Xu:**
Yeah, something like that. Maybe another way to say it is: there was an assumption you could have made during training, which is that noises are never both on simultaneously, and this assumption does not hinder your ability to predict your model at all, basically. Maybe noise is quite rare, and so both noises are only on with one in 1,000 chance, one in 1 million chance, et cetera or something like that, and so you're only missing out on one part in a thousand explanation of your model's behavior if you make this assumption that noises are never on, but you're missing out on 100% of the "behavior" on the specific data point where both noises are on. And so anytime you have this mismatch, we can drop something out that only hinders your average prediction during training by one part in a thousand, but hinders your ability to predict this specific thing by a lot, then you should be suspicious that the things that are happening between train time and test time are quite different.

And in particular, this correlation between the sensors is happening for "a different reason". So this is covering the "are they correlated for the normal reason?" part.

**Daniel Filan:**
Yeah.

**Mark Xu:**
And then the way that we're going to get the "for any reason" part is we're going to require that the set of mechanisms that you drop out explain both sensors individually and then ask whether or not there exists a set of mechanisms that explain both sensors being on individually but does not explain the conjunction being on.

**Daniel Filan:**
Okay. Sorry, can you restate that or give an example?

**Mark Xu:**
So suppose during training there were never any robbers and so the diamond just persisted through time in the normal ways the objects persist through time and showed up on both cameras, and then suppose during deployment there is a robber now and we want our AI to take the action of 'build a wall to stop the robber', but our AI could also take this action of 'manipulate both sensors', and so if you apply naive anomaly detection, then both of these will be flagged as anomalies. Your AI has never built a wall before, it's also never manipulated sensors before, but then we're stuck because our AI can't take any actions basically.

**Daniel Filan:**
Yep.

**Mark Xu:**
So the hope is that we ask for a set of mechanisms such that both sensors appear to be on according to this set of mechanisms, and then we ask further the question, are the AND of the sensors on or are we required to thus deduce that the AND of the sensors are on?

**Daniel Filan:**
Yeah.

**Mark Xu:**
So if your AI builds a wall to stop the robber, then we're required to include the mechanism of wall-building and robber-stopping to explain the scale showing a diamond and the camera showing a diamond, and then since the scale is showing a diamond because there's a diamond and the camera is showing a diamond because there's a diamond, and we have to include enough mechanisms to explain 99.9% of train-time variance or performance or whatever, then we're required to make this deduction that both of those being on because of the diamond implies that the AND is on. Whereas if your AI took an action to manipulate the noise of both cameras, we can make this assumption that the noise never happens simultaneously and we're not forced to conclude that the AND of both sensors is on if both sensors are on because your AI manipulated the noise.

**Daniel Filan:**
So the thing I think I'm not getting is this idea of manipulating the noise, which seems like a model-dependent thing. So if I say very concretely, okay, suppose what happens is robbers are coming to take the diamond and the AI makes a plan that's like, okay, I'm going to fiddle with all the sensors to make it appear like a diamond's there.

**Mark Xu:**
Yeah.

**Daniel Filan:**
So it does that, right?

**Mark Xu:**
Yeah.

**Daniel Filan:**
And then a mechanism that explains this is: the AI wanted to say that the diamonds were there. And then that explains both that sensor 1 says the diamond's there and that sensor 2 says the diamond's there, and presumably, you've got to deduce both of those from this explanation, and is the idea that that's not going to count because that's just a different mechanism than existed in the past?

**Mark Xu:**
The thing I want to say, although I'm not quite sure how accurate it's going to be, is the mechanisms have to be more detailed than that. So just because your AI wanted to make there be a diamond does not imply that the particular action it took will in fact make it be that there's a diamond, and so you still have to talk about the particular mechanism of action for the particular action your AI decided, which was your AI had to manipulate sensor 1 and it had to manipulate sensor 2 and it had to manipulate sensor 3... I guess we were only talking about two sensors. So your action only has to manipulate two sensors and then the particular mechanism of action that this particular action that your AI selected still violates this principle, which is manipulations or noise never affects both sensors simultaneously, which you're free to assume during training without hurting your ability to predict your model but now hurts your ability to predict this particular action by too much.

**Daniel Filan:**
So sorry, why... So is the thing that's happening is that if the mechanism is the... Well, hang on, wait back up. Suppose your AI is just really good at stuff, right? When it tries to make things happen, they do happen. Then, you know, you can with high probability deduce "both sensor 1 and sensor 2, they're going to say there's a diamond there" from "the AI is trying to make it appear that there's a diamond there". And maybe you rule that one out because you're like, "Well, that mechanism didn't really apply during training or on this nice set of examples", and then the only other explanation you have left is randomly sensor 1 said there was a diamond and also randomly sensor 2 said there was a diamond because they are trying to make something happen. In some sense that's random relative to us not knowing that was going to happen, and then you're like, "Okay, but you didn't need to assume independent random noise during training in order to get high probability that a diamond was going to be there".

**Mark Xu:**
Yeah, I think the second part of that was correct. I think the first part is confusing. So I agree that there's this compelling sense in which your AI wanted X, therefore X, should be a valid argument for why X happened or something, and I think...

**Daniel Filan:**
Or at least a valid mechanism?

**Mark Xu:**
I think that sounds like it might be right, and this is an area where it's a bit confusing. So I think the way that I would want to talk about this more precisely is: you have some clean set where you know your AI actually made it so there's a diamond, and there's some question of: suppose that the thing your AI is doing is always trying to make it look like there's a diamond. How do you know that your clean set is clean?

And that itself I think is complicated and I don't quite know what's going on there, but one way that you could hope to know it is you have a sequence of policies. One policy is the randomly-initialized policy, and you're very confident that this policy is not going to take any actions that have any coherence, and so in fact, the diamond will be there because there's a diamond, because your randomly-initialized policy cannot hack all the sensors. And then you have your policy that maybe is sophisticated and then you ask, "Are these two policies taking... are the diamonds appearing because of actions that these two policies are taking for the same reason as each other?" or something. And you need to have some ability to know that your policy is not taking actions that hack all the sensors at some point.

So I think things get simpler if we're not talking about policies that take actions and just we're talking about predictors that predict things, and then we can ask, is the predictor predicting things because of the normal reason that it sometimes predicts correlated sensors, which is the diamond is still there? Or is it predicting things because of some different reason, which is someone took an action that hacked all the sensors?

**Daniel Filan:**
Yeah, and then I guess in this case we're not just assuming that it's a thing that was trained to predict something, we're assuming that it actually does a high-quality job at predicting these things.

**Mark Xu:**
Yeah.

**Daniel Filan:**
Because otherwise then you have [mesa-optimization](https://arxiv.org/abs/1906.01820) worries of, "Oh, what if this thing that said it was a predictor is actually trying to fool you?"

**Mark Xu:**
So I think the simplest setting is assuming this sort of benign predictor thing, which I think is unrealistic for a variety of cases, but is this a maybe simple example of how you want the thing to be going on? And so maybe we can assume that there's a robber coming to steal your diamond, never before seen during training, and your predictor takes in the input of https://arxiv.org/abs/1906.01820the position of five levers, and all the levers start 'off', and if you flick the first three levers on, then a wall gets built that stops the robber. However, if you flick the fourth lever on, then the first sensor turns on, and if you flick the fifth lever on, then the sensor turns on, and it costs $1 to flip each lever, and so it's in fact cheaper, or it's just simpler, maybe you're doing random search against actions.

And so the action you will expect to find is flipping both levers 4 and 5 on, and then we want to be able to find the action that flips the first three levers on, but not levers 4 and 5... or I guess it doesn't really matter if you flip levers 4 and 5 also. And then the way we hope to do this is we want to ask for an action that makes sensor 1 on for any reason and sensor 2 on for any reason such that we are forced to deduce that sensor 1 and sensor 2 are on, and then flipping levers 4 and 5 will not cause this to happen because we can drop out the correlated term between 4 and 5 freely, but we are forced to include the correlated term between sensors being on because of diamonds, because that's what was going on during training.

**Daniel Filan:**
Right. Okay. So I think this makes some amount of sense if I want to understand just this problem of how do you ensure that the camera says the diamond is there for the normal reason vs the diamond's there for any reason. So in this case, one thing I wonder about is... So this is a problem that only occurs in the thing that this is an analogy for. So this is meant to be an analogy for 'humans are happy and flourishing' and stuff.

And suppose I am in a world where I've got this smart AI, it's trying to make humans happy and flourishing, and it's also got this, "Are humans happy and flourishing detector?", and let's say that detector is... There's some camera feeds and stuff and also there are two humans, both of which are looking at the camera feeds and they're like, "Yeah, as far as I can tell, the humans are happy and flourishing." Right?

Now, one thing the AI can do is that it can make those two humans better at their jobs. For instance, it can stop those humans from getting sick or maybe it can give those humans stimulants or make them better at focusing or better at noticing small issues, and then I wonder: that kind of action, it seems like that could change maybe some of the correlations between different sensors, or maybe it makes the humans a bit more correlated because they're losing less info - or maybe one of the interventions is that it helps the humans talk to each other to just generally make the sensors more accurate.

And so I wonder, the thing I'm wondering about is: is this going to flag benign ways of improving the sensors? Which in some sense is what we should expect if AIs are doing a really good job at making all of humanity better.

**Mark Xu:**
So I think this is a bit complicated and I'm not sure what the answer is, so here's the baseline of what can happen. So you can imagine a world where all your AI does is protect the humans from malign influences and meteors and other AIs, et cetera, et cetera, and then all upgrades to sensor arrays or improving humans' ability to interpret sensors and reason about the world, et cetera, et cetera, is only ever guided by human hands, and humans must do the hard work of thinking about how the sensors work and designing better sensors, et cetera, et cetera. And I think this is the baseline and then you can try to think about different ways in which your AIs can help with this process. So I think your humans can ask for things that they can point at using this latent structure.

They can be like... "During training I sometimes had a strawberry in front of me, can you make a strawberry in front of me for the normal reason that strawberries appear in front of me?" Suppose your human wants to eat a strawberry and eating a strawberry will make them better at reasoning because food is good or whatever. Or they can be like: "sometimes during training, there were various physical resources, I had some iron: can you make there be some iron in front of me for the normal reason that there's iron". I think it's kind of complicated if the humans want to make requests like, "Can you design me a better camera?" I think if you were very clever about it, you might be able to ask for a better camera, but I think it might be unclear how you would ever ask for a better camera in a way that your AI can fulfill by making sense or something, and I think it's unclear to me that this is problematic.

**Daniel Filan:**
Yeah, I guess the worry might be that... I guess it depends where explanations stop, but the worry might be that almost any good thing the AI might do, might end up improving sensor readings. So the AI makes, I don't know, electronics more reliable or the AI improves human cognition or something. It seems like by default that might improve sensors.

And yeah, maybe you cut it off as saying, well, in any case, the reason the sensors are working is humans are making them work and humans decided to get these upgrades or something.

**Mark Xu:**
I think maybe the simplest answer is just as long as the changes are slow and you can still verify that the humans are alive post making the changes, then you can just do online learning and retrain your AI and be like "and now we're defining what it means to be good relative to this new set of sensors", and you can in fact verify that the humans are still alive. And the only worry is if you upgraded all the cameras simultaneously and you lost your ability to check whether or not the humans are still alive because you no longer have knowledge about how the cameras work or other stuff like that.

**Daniel Filan:**
So, should I be imagining this gets flagged as an anomaly, but we go ahead with doing it anyway?

**Mark Xu:**
So, if you imagine your AI starts with five cameras and then you add a sixth camera, I think by defaultyour "definition" in terms of mechanisms of what it means for the diamond to be there or the human to be alive is just only going to be in terms of the first five cameras, and your AI is just going to ignore the sixth camera entirely when selecting actions. And then in order for it to incorporate the sixth camera, you're going to just need to provide a new clean set where the humans are alive according to all six cameras, and then do mechanistic anomaly detection relative to that new set. And then, I don't know, you can imagine some gradual... You just add a sixth camera and then you use the first five cameras to check if the humans are alive and then you label using that, I don't know-

**Daniel Filan:**
So, it's gradual in the subset sense, where there's always some subset that remains unaffected by changes?

**Mark Xu:**
Sure, so I think that should work, although I haven't thought about it. I don't know, I mostly consider this in some sense out of current scope or something.

### How useful is mechanistic anomaly detection? <a name="how-useful-is-mad"></a>

**Daniel Filan:**
So, I guess we've talked about mechanistic anomaly detection a while. Maybe more broadly, how are you thinking about... on the two scales of 'it doesn't really solve this', to 'it totally solves this', and the scale of various problems things might solve, how are you thinking of mechanistic anomaly detection: it definitely solves this one problem and maybe solves this other, or it maybe solves five problems?

**Mark Xu:**
So, I think there's various versions of the hope. I think the strongest version of the hope is that it both handles [deceptive alignment](https://www.alignmentforum.org/tag/deceptive-alignment) and it handles all of [eliciting latent knowledge](https://www.lesswrong.com/posts/qHCDysDnvhteW7kRd/arc-s-first-technical-report-eliciting-latent-knowledge), and I don't know if that makes sense. I'm happy to talk about why I think mechanistic anomaly detection will tackle those two problems. And then I think there's more restricted hopes.

I guess that's what I'm gunning for in some sense, and then I think it's possible that we only get what I was calling the in-distribution mechanistic anomaly detection, and we don't get the out-of-distribution mechanistic anomaly detection, in which case we'll only get deceptive alignment and possibly only some subset of deceptive alignment, and we'll only get this IID form of eliciting latent knowledge, which is going to be not that useful I think, and then it's possible we get nothing.

And then I think there's all sorts of empirical middle grounds, or there are, I think, worlds where we don't nail the problem, but we can do some forms of ad hoc mechanistic anomaly detection by doing normal human interpretability and looking at models and being like, "Huh, that looked weird, the neurons are distributed weirdly now: perhaps that's not good," I think I largely consider that not what I'm aiming for, but maybe it will happen.

**Daniel Filan:**
Do you have a sense of which of these outcomes are more likely? Are there really big gaps in the probabilities between these things?

**Mark Xu:**
I think the outcome that is most likely is that we just get nothing and all of this doesn't work for some reason or another or we're just wrong about how everything works, and then I think if we're right about things, we'll probably just get both of them, and I think it would be kind of surprising to me if we only got the in-distribution anomaly detection, but we didn't get the out-of-distribution anomaly detection, because it seems like there's a lot of avenues there, but I think in general, it's kind of confusing what we're even doing and if we're going to get anything at all or something. I think that's the first question, do we get anything at all? And then if we get something, then we'll probably get a lot of things is my guess.

**Daniel Filan:**
All right, maybe this is going back a bit, but in-distribution versus out-of-distribution anomaly detection, given that you can see a new rare thing in-distribution, I'm not even sure what the distinguishing features of those are.

**Mark Xu:**
I think the main question is whether or not it's okay to get tripped up by seeing new rare things in-distribution, whether or not it's okay to just flag all of those as anomalies. And so, perhaps it's not accurate to call it in-distribution anomaly detection, but I think there's a naive thing that I want to do, which is take a training set, white list all the mechanisms that happened during training, flag everything else as anomalous, and then if something is anomalous, we can inspect it carefully and check whether or not it's actually anomalous.

If it's actually not anomalous, we add it to the white list and then we just keep going. And there's some question of, is that okay or not? If that's an okay process to be engaged in, then I think... I want to call that the in-distribution world, things aren't changing too fast. Your AI doesn't have to take novel actions that humans don't really understand. AI can just do normal stuff and if it does anything abnormal, the humans look at it and it's like, "Is this bad? I don't know," and they can take their time and decide, and the hope is that this will catch all the crazy deceptive alignment cases where the AI goes from being totally benign in actions to stabbing all the humans where you're like, "That was not good," and you have time to deal with that sort of stuff, and then you can retrain your AI to be like, "You know the part where you defected and stabbed all the humans? Negative reward."

**Daniel Filan:**
And then going a bit more broadly, I'm wondering how you think of this general mechanistic anomaly detection program as relating to things like interpretability or explanation or stuff?

**Mark Xu:**
So I think the field of interpretability is empirically motivated by many things. I think if the entire field of interpretability, all they did was figure out a way to do mechanistic anomaly detection on powerful models, I would be like, "That was a good job." I think interpretability techniques are often useful for things like mechanistic anomaly detection, but not oriented around that or something like that. I think I would make the tentative claim that people doing interpretability should be more motivated by various downstream applications, of which I think mechanistic anomaly detection is one, but often confounded by the fact that there currently exists pretty strong baselines for mechanistic anomaly detection that aren't doing the kinds of things that we want to be doing, and so it's hard to check whether or not you're making progress on things like empirical mechanistic anomaly detection. Because you can just solve it by doing a bunch of stuff that intuitively seems unscalable or wouldn't work in more complicated settings or just doing regularization or retraining your model or fine-tuning or whatever.

I'm not very familiar with what people mean when they say search for explanations. I think naively, I mean the same thing by 'mechanism' as what they mean by 'explanation', or explanations are maybe sets of mechanisms in my head; not quite sets, but mechanisms layered on top of each other. And I think it would be reasonable to ask questions like: when your explanation of your model's behavior has an 'or' structure where it could be doing A or B, we want to be able to detect whether or not it's doing A or whether or not it's doing B on any specific input. I think that's a reasonable question to ask of explanations, and I would be excited if someone did something related to explanations that enabled you to do that thing or defined explanations in a way that made it possible to do that sort of thing, et cetera, et cetera.

## Formalizing the presumption of independence <a name="heuristic-arguments"></a>

**Daniel Filan:**
Cool, so perhaps speaking of explanations, I maybe want to move on to this paper that is called [Formalizing the Presumption of Independence](https://arxiv.org/abs/2211.06738) authored by [Paul Christiano](https://paulfchristiano.com/), [Eric Neyman](https://sites.google.com/view/ericneyman/), and yourself. I guess it was on arxiv November last year?

**Mark Xu:**
Around then.

**Daniel Filan:**
Cool, so for this paper, can you tell us... For those who haven't heard of it, what's the deal with it?

**Mark Xu:**
So, I think ARC's work right now is roughly divided into two camps. One camp is: given some sense of what a mechanism is or what an explanation is, how can we use that to do useful stuff for alignment, under which mechanistic anomaly detection is the main angle of attack there; and then the other half of ARC's work is like, what do we mean by mechanism or what is a mechanism? What is an explanation? And that is largely what 'Formalizing the presumption of independence' is. And so, 'Formalizing the presumption of independence' more specifically is about: So, suppose you have a list of quantities like A, B, C, D, et cetera, and you're interested in the product of these quantities and you knew, for example, expectation of A, expectation of B, expectation of C, I claim that a reasonable thing to do is to assume that these quantities are independent from each other -- to presume independence, if you will -- and say that expectation of A x B x C x D, equals expectation of A x expectation of B x expectation of C x expectation of D, and this is "a reasonable best guess". And then there's some question of: but then sometimes in fact A is correlated with B and C is correlated with D, and so this guess is wrong; and so, we want to be able to make a best guess about how various quantities are related, presuming independence, but then if someone comes to us and is like, "Hey, actually if you notice that A is correlated with B", we want to be able to revise our guess in light of this new knowledge.

And so, 'Formalizing the presumption of independence' is about the quest to define these two objects that are related in this way. So we have a heuristic estimator, which we want to "presume independence", and then we want a heuristic argument, which is an object that we feed to a heuristic estimator to be a list of ways in which the presumption of independence is false, and actually you should be tracking the correlation (or maybe higher order analogs of) between various quantities. So we can imagine that for example, by default given A x B, your heuristic estimator will estimate expectation of A x B as expectation of A x expectation of B, and then you can feed it a heuristic argument, which could be of the form of, "Hey, actually A and B are correlated," or, "Hey, actually you should track the expectation of A x B," which will make your heuristic estimator more exact and calculate expectation of A x B as maybe expectation of A x expectation of B plus the covariance between A and B, which is the correct answer.

And then we want to be able to do this for everything, I guess is the simple answer there. So, we want to be able to say, given an arbitrary neural net, we have some default guess at the neural net, which is maybe that we just assume that everything is independent of everything else. So, we want a default guess for the expectation of the neural net on some compact representation of the data distribution, which we often call mean propagation, where you just take the means of everything, and then when you multiply two quantities, you assume that the expectation of the product is the product of the expectations and you just do this throughout the entire neural net, and this is going to be probably pretty good for randomly-initialized neural nets, because randomly-initialized neural nets in fact don't have correlations that go one way or the other. And then you have to add the ReLUs, that's a bit complicated. Let's assume for now there are no ReLUs, and instead of ReLUs, we just square things and then it's more clear what to do.

Again, your guess will be very bad, because if you have an expectation 0 thing and then you square it, you're going to be like, "Well, the expectation of the square is going to be the product of the expectations, and so it's probably zero," but actually it's not probably zero because most things have some amount of variance. And then hopefully someone can come in or ideally we'll have a heuristic estimator that does this by default, and then someone will come in with a list of ways in which this presumption of independence is not very good. For example, the square of things is often positive or things often have variance, and then we'll be able to add this to our estimate and we'll be able to do a more refined version of mean propagation where you keep track of various covariances between things and higher order analogs of. A,nd then hopefully we'll be able to do that. And I think there's a bunch of follow-up questions which is like, "Why is this useful? Why do you think this is possible?" Maybe I'm happy to go in the order that you think is best.

**Daniel Filan:**
Sure. So first of all: my understanding is that this is related to the mechanistic anomaly detection paradigm where I think the thing is supposed to be that a heuristic argument is a mechanism, or is playing the role of a mechanism. Is that right?

**Mark Xu:**
I think it's more accurate to think of heuristic arguments as lists of mechanisms. I think that is also not quite right and the entire paradigm is a bit not... Or the words are not good because I think there's going to be no such thing as one mechanism as distinct from other mechanisms. I just think it's going to be an entire layered mess of different mechanisms constantly interacting with each other, et cetera, et cetera, where it's hard to point to any computer program and be like, "There are seven distinct mechanisms for why the program is behaving as it is." There's just one thing and then it splits into two things and then interacts with the third thing, and so you probably won't have this list of distinct mechanisms. But the idea is that a heuristic argument is a set of mechanisms, or a set of mechanisms plus ways that they relate to each other, et cetera, et cetera, and the hope is that this is the kind of object that we'll need to be able to do mechanistic anomaly detection.

**Daniel Filan:**
If the old arguments for why your network is outputting 'be nice' is different from the new argument for why your network is outputting 'be nice', then you should be as worried as if there were different mechanisms, something like that?

**Mark Xu:**
Or the hope is to just use the heuristic estimator, plus heuristic argument in all of the schemes that I was describing previously. You have a heuristic argument for why your model is behaving the way it is, or your heuristic argument explains like 99.9% of the variance of your model during training. And then you ask whether or not there exists any heuristic argument that explains 99.9% of the variance of your model during training, but does not explain this particular data point off distribution, and then if that ever happens, then you're like, "This data point was using a part of the model only responsible for one in 1,000 parts of its variance during training, that's kind of weird. No good."

### Heuristic arguments in physics <a name="heuristic-arguments-in-physics"></a>

**Daniel Filan:**
This is actually interesting, because one thing that I was thinking of when thinking about this is: in physics you can often use these energy arguments. So, here's a classic energy argument. You have some ramp, you have a ball, it's resting on the top of the ramp, and for convenience's sake, this ball, it's actually just infinitely... It's a point particle. It's got zero radius. So, you put it at the top of the ramp and then you let go, and then the bottom of the ramp is horizontal, and at the end the ball is traveling at the bottom of the ramp. And you ask, how fast is the ball going at the bottom of the ramp? And there's this way you could argue it, which is like, "Well, the height between the top of the ramp and the bottom of the ramp is this much, and so initially the ball had this gravitational potential energy that it used up by moving down, and at the end, the ball just has kinetic energy, which is just its velocity." And then you're like, "Well, this is going to be equal because energy is conserved," and then you back out the velocity of the ball.

And the interesting thing about this argument is that it doesn't actually rely on the shape of the ramp. The ball could do loop-de-loops in the middle and bounce around off a bunch of things and you'll still get the same answer. And so, there are a bunch of mechanisms that correspond to this argument of energy conservation, but it seems like at some level, as long as you can have this argument that energy got conserved, that's qualitatively different from arguments where I took the ball and then put it at the bottom of the ramp and then gave it the right amount of velocity. I don't know, that's not really a question. So, you have the choice to react to that or not.

**Mark Xu:**
So, I think we often consider examples like this from physics, and the hope is that maybe more abstractly, there's a set of approximations that people often make when solving simple physics problems, like there's no air resistance, things are point masses, there's no friction, et cetera, et cetera. And the hope is that these all correspond to uses of the presumption of independence. This is simplest for the [ideal gas law](https://en.wikipedia.org/wiki/Ideal_gas_law), where the presumption of independence you're making when you're using the ideal gas law is the particles... their position is randomly distributed inside the volume. They don't interact with each other. Particles are independent of each other, they have no electrostatic forces on each other, and they have no volume also. And it's not immediately obvious to me what uses of the presumption of independence correspond to no friction.

**Daniel Filan:**
I think it's not. So, if I think about no friction or point particle in the ramp example, if you add in those assumptions, those systematically change the answer... I don't know, I guess those can only ever make your predicted velocity go down. They can never increase your predicted velocity because energy gets leaked to friction or because part of your energy is making you rotate instead of making you have a linear velocity, and that strikes me as different from other cases where if things can be correlated, well, they can be correlated in ways that make the number bigger or in ways that make the number smaller usually. Except for the case of, you square something, and in that case when you add the covariance between X and X that always makes the expected square bigger.

**Mark Xu:**
So, I think there are going to be cases in general where adding more stuff to your heuristic argument will predictably make the answer go one way or another, because you looked at the structure of the system in question before thinking about what heuristic arguments to add. I think there are ways of making... One presumption of independence is relative to the ball's center of mass. The position of the various bits of the ball is independent or something.

Whereas if balls are actually rotating or the velocity of the bits of the ball is independent. Or zero maybe, but actually balls rotate sometimes, or... I actually don't know about the friction one. I guess balls don't have that much friction, but you can assume that the bits of the ball don't exert any force on the bits of the ramp or whatever.

**Daniel Filan:**
Or you could imagine maybe the ball rubbing against the ramp causes the ramp to push the ball even harder.

**Mark Xu:**
Maybe, I think that's hopefully not going to be a valid use of the presumption of independence, but I'm not quite sure.

**Daniel Filan:**
Or I guess it could slow the ball down or it could speed the ball up, like -

**Mark Xu:**
If you were truly naive looking at physics, you'd be like, "Who knows what's going to happen when you add this interaction term?"

**Daniel Filan:**
In fact, modeling friction is actually kind of tricky. You get told 'ah, friction!' but wait, what is friction?

**Mark Xu:**
I think these are the kinds of things where heuristic estimators are hopefully quite useful. There's some intuitive question or problem which is: there's a ball at the top of the ramp, what is its velocity going to be at the bottom of the ramp? And oftentimes the actual answer is, I don't know, there's a trillion factors at play. However, you can make this energy argument that's a pretty good guess in a wide range of circumstances, and you're never going to be able to prove basically that the velocity of the ball is going to be a certain thing at the bottom. I guess proof is complicated when you're talking about the physical world, but if you suppose that you have a computer simulation with all the little particles interacting with each other, basically the only way that you're going to prove that the ball ends up at the bottom with a certain velocity is to exhaustively run the entire simulation forwards and check at the end what its velocity actually is.

However, there's this kind of reasoning that seems intuitively very valid, which is: assume the air doesn't matter, assume the friction doesn't matter, just calculate the energy and do that argument, that we want to be able to capture and formalize in this concept of a heuristic estimator, and then the heuristic argument will correspond to 'consider the energy'. Maybe if there's lots of wind, then in fact the air resistance on the ball is non-negligible, and then you can add something to your heuristic argument, which is 'also you've got to consider the fact that the wind pushes the ball a little bit' and then your estimate will get more accurate over time, and hopefully it will be a big, good time. I don't know, hopefully it'll work.

**Daniel Filan:**
And interestingly, 'the wind pushes the ball' is saying that the interactions between the wind and the ball are not zero mean, or there's some covariance or something.

**Mark Xu:**
One should notice these things to get a better estimate. And so, hopefully we'll be able to do this for everything, hopefully we'll be able to do it for all the physics examples, and there's a set of math examples that I'm happy to talk about, and then obviously the application we truly care about is the neural net application.

### Difficult domains for heuristic arguments <a name="difficult-domains"></a>

**Daniel Filan:**
Speaking of examples: so in the paper you have a bunch of examples from number theory. There are also some other examples, but to be honest, it seems like the main victory of these sorts of arguments is in number theory, where you can just imagine that primes are randomly distributed and make a few [assumptions] about their distribution and just deduce a bunch of true facts. So, what I'm wondering is outside number theory... So, you give some examples where heuristic arguments do work: are there any cases where you've had the most difficulty in applying heuristic arguments?

**Mark Xu:**
Yeah, so I haven't personally spent that much time looking for these examples. So, we are quite interested in cases where heuristic arguments give "the wrong answer". I think this is a bit complicated, and so the reason it's a bit complicated is: there's this nature of a heuristic argument to be open to revision; and so, obviously you can make heuristic arguments that give the wrong answer. Suppose A and B are correlated, and I just don't tell you to notice the correlation, then you're just going to be really wrong. And so, it's kind of unclear what is meant by a heuristic argument that gives the wrong answer. And so we often search...

There's two sorts of things that we're interested in. One sort of thing is an example of a thing that's true or an example of an improbable thing that happens way more likely than you would've naively expected, that seems to happen for "no reason", such that we can't find a heuristic argument in some sense. So, obviously you can find a heuristic argument if you just exhaustively compute everything, and so there's some notion of length here where we can't find a compact heuristic argument that seems to describe "why this thing happens the way it does", where we conjecture that it's always possible to explain a phenomenon in roughly the same length of the phenomenon itself.

And then another thing that we search for often is we want to be able to use heuristic arguments for this sort of mechanism distinction problem. And so, we search for examples of cases where heuristic arguments have the structure where it's like: A happens, therefore B happens, and sometimes C happens, therefore B happens, but you can't distinguish between A and C efficiently. So, I think the closest example we have of something that seems kind of surprising, but we don't quite have a heuristic argument, is... So, you have Fermat's Last Theorem, which states that a<sup>N</sup> + b<sup>N</sup> = c<sup>N</sup> has no integer solutions for N greater than two.

**Daniel Filan:**
Non-trivial integer solutions.

**Mark Xu:**
Non trivial, yes.

**Daniel Filan:**
Zero and one.

**Mark Xu:**
Well, one doesn't quite work because...

**Daniel Filan:**
0<sup>N</sup> + 1<sup>N</sup> = 1<sup>N</sup>.

**Mark Xu:**
Oh, sure. And so the N = 4 and above case, the heuristic argument is quite simple where you can just assume that the fourth powers are distributed randomly with their respective intervals or whatever; there's like N fourth powers between 1 and N to the 4, and then you can just assume that these fourth powers are distributed independently and then you correctly deduce that there are going to be vanishingly small solutions to this equation. I'm not that familiar with the details here, but the N = 3 case is interesting, and in fact, the heuristic argument for the N = 3 case is basically the same as the proof for the N = 3 case, because the way the numerics work out for the N = 3 case is if you do this naive assumption, there's going to be infinite expected solutions, and then there's a complex structure in the equation itself that suggests that all the solutions are correlated with each other.

So, either there's going to be a lot of solutions or there's going to be basically none solutions, and then you check and in fact, there's going to be basically none solutions or basically zero solutions, and the heuristic argument for this fact is basically the proof of this fact, which is quite long. But the problem is, you also heuristically expect the solutions to be very sparse, and so you're not that surprised that you haven't found a solution until you've checked a lot of examples or a lot of numbers, and so the number of things that you have to check is large, such that you can fit the heuristic arguments... So you're not that surprised. I don't know if that made that much sense.

**Daniel Filan:**
That makes some sense. So I don't know, it's like this case where heuristic arguments... it's like they're not winning that much over proof.

**Mark Xu:**
It almost didn't fit, but the phenomenon you were supposed to be surprised by was barely not surprising enough such that you had enough room to fit the argument in before you got too surprised, such that you would expect there to be an argument or something. I don't know, it's kind of unclear whether or not that should be evidence in favor of our claim or against our claim that there's always heuristic arguments, because in fact there is a heuristic argument in this case, but it maybe barely wasn't a heuristic argument, and so it's kind of confusing.

### Why not maximum entropy? <a name="y-no-max-ent"></a>

**Daniel Filan:**
One thing that I found myself wondering was... It seems like you have this structure where you have this list of facts, and you're wanting to get a heuristic argument given this list of facts. And in the paper you describe this cumulative propagation thing, which basically just involves keeping track of expectations and covariances and some higher order things. You pick some level that you want to keep track of and you do that. But then you mention some failures of this approach and some ways you can fix it. One thing that struck me as the obvious first approach is just having a [maximum entropy distribution](https://en.wikipedia.org/wiki/Maximum_entropy_probability_distribution) given some facts.

So for people who don't know, this is this idea of, you have some probability distribution over some set of interest and you don't know what your probability distribution is, but you know that it's got to have this average and this correlation between these two things and this covariance. And basically you say, "Well, given that I only know these things, I want to maximize the entropy of my probability distribution." So, my probability distribution has to satisfy these constraints, but I've got to be as uncertain as I can possibly be given those constraints.

And so, this is a big thing in Bayesian reasoning, and intuitively it seems like it would fit, this thing. Every time you hear a new fact, you can add a constraint to your... And now get a new maximum entropy distribution. It always gives answers. I remember in discussion of cumulant propagation, you mentioned some cases where maximum entropy would do better than it does, although I don't remember the exact reason why. I think it was something to do with calculating the square of some quantity is negative in expectation. So, why not just do maximum entropy?

**Mark Xu:**
So, historically we've considered a lot of maximum entropy-type approaches and all of them were a bit unsatisfying. So, I think there's three-ish reasons that we think maximum entropy is unsatisfying in various ways. I think the first one is perhaps most compelling, but most naive, and I'll give that example first. So suppose that you have A and B, and then you're taking the AND of A and B, so suppose we have C equals A and B.

**Daniel Filan:**
Yep.

**Mark Xu:**
And suppose that we know that C is 50/50, then the maximum entropy distribution on A and B given C is 50/50 both... Sorry, so if we condition on C being 50/50, the maximum entropy distribution over A and B has A and B correlated a little bit, but also it raises the probability of A and it raises the probability of B.

**Daniel Filan:**
Relative to...

**Mark Xu:**
Relative to 50/50.

**Daniel Filan:**
Okay.

**Mark Xu:**
And there's various reasons why this is unsatisfying. The main reason, I guess, it's unsatisfying is: suppose you have a circuit that is computing some stuff, and then you just add a bunch of auxiliary gates where you have some wires, and then you pick two wires and you're like, "I want these wires to be correlated," then you can just add the AND of those wires a bunch of times to your circuit, and then if you want the maximum entropy distribution over all the wires, it will want to push the AND of those two wires down to 50/50 because that's the maximum entropy probability for a wire to be. And so, it'll induce this fake correlation between the wires while you're trying to do that.

**Daniel Filan:**
Sorry, just going back to the A, B, C which is A and B thing: is the idea that the thing you wanted was for A and B to be uncorrelated, but have high enough probability that when you combine them to get C, the probability of C being true is 50/50?

**Mark Xu:**
I think I actually explained the example wrong. So suppose you just have: your circuit is just two wires, it takes two inputs and it just outputs both of them, then the maximum entry distribution is 50/50 over both wires, that seems correct. And then suppose you just randomly added an AND gate, then the maximum entropy distribution would--

**Daniel Filan:**
So, when you say randomly added an AND gate, you mean like you have your two inputs and then you put an AND gate that takes your two inputs, and the output of that gate is your new output?

**Mark Xu:**
No, you don't connect it to the output at all. You still output A and B, but you've just added an AND gate as some auxiliary computation that your circuit does for no reason.

**Daniel Filan:**
Okay.

**Mark Xu:**
Then, if you want to take the maximum entropy distribution over all the things your circuit is doing, then adding this gate will artificially raise the probability of A and B and also induce this correlation between A and B.

**Daniel Filan:**
Why?

**Mark Xu:**
So naively it won't because your maximum entropy distribution will just be like A, B, C, all 50/50 and independent, and then suppose someone came to you and was like, "Actually, you need to respect the logical structure of this circuit. C can only be on if A and B are both on. The probability of C being on has to be the probability of A being on times the probability of B being on plus the interaction term," then you'll get this induced correlation between A and B plus A and B will be raised to 0.6 or whatever.

**Daniel Filan:**
So, is the idea that you have this intermediate thing in your circuit and you have to be maximum entropy also over this intermediate thing, and so even though you didn't really care about the intermediate thing, it didn't play any role of interest in anything else, because you're being maximum entropy over that, you've got to mess around with the distributions over other variables?

**Mark Xu:**
Yeah, that's right, and I think of this as a special case of a more general phenomenon where there's this computation that you're doing throughout the circuit, and you can pretend there's some notion of time in your circuit, or gates that only take input wires as inputs are at time 1, and then if a gate takes as inputs gates that themselves take inputs as input, then that's time 2, et cetera, et cetera, where you're going down your circuit and computing things. And maximum entropy distributions have this property where things that you compute in "the future" go back in time and affect the past.

**Daniel Filan:**
In the sense of once you know things about the future, that gives you constraints on the past?

**Mark Xu:**
Yeah, and if you want to be maximum entropy, it causes you to mess with your probabilities of the input wires: if you learn that you're computing lots of ANDs then you want to raise the probability of your input wires, et cetera, et cetera. And I think this is in general going to be a kind of an undesirable property, and it's a bit complicated to talk about why it's undesirable.

I guess the simplest reason is someone can mess with you by putting a bunch of stuff at the end of your circuit and then once you get to the end of your circuit, you're forced to go back and revise your initial guesses. Whereas it seems like intuitively, heuristic arguments want to be more deductive in nature, where you start with some premises and then you deduce facts from those premises, and nothing anyone can say after you've started with your premises or nothing anyone can add to your circuit after you deduce facts from your premises can cause you to go back in time and be like, "Actually, my premises were wrong because..." I don't know, it doesn't really make that much sense that if you assume the A is 50/50 and B is 50/50 and then you're like, "Ah, I'm computing C, which is A and B, I guess A and B are like 60/40 now and they're also slightly correlated."

That kind of reasoning seems intuitively invalid, and it's the kind of reasoning that various forms of maximum entropy force you to take. And you can try to do various other stuff to eliminate this problem which we've explored historically, and I think none of them seem that satisfying and fix the problem that well or something.

**Daniel Filan:**
Right, that's a useful response. You just end up with these useless variables that you shouldn't have cared about the entropy of, but you do now. So, this relates to this question I have about the adversarial robustness of these heuristic estimators.

**Mark Xu:**
So, I said originally that there were three reasons why we didn't maximum entropy. I'm happy to talk about the other two also, but maybe you don't want to.

**Daniel Filan:**
I think I'm sold by reason 1. If you want to talk about reason 2 and reason 3, I'm all ears.

**Mark Xu:**
Well, so I think they are somewhat related to this more general problem of why heuristic arguments are going to be possible to begin with. So, I think reason 2 is going to be something like: we can't actually maintain full probability distributions over things. So, there's an important thing a probability distribution has to be, which is consistent with various facts or something. It has to be a real probability distribution. It has to describe actual things that are realizable. And I think heuristic arguments or your heuristic estimator can't be real in that sense. Your distribution over possible states your circuit can take can't be a distribution over actual possible states your circuit can take.

So, one reason that we want this is some of our mechanistic anomaly detection applications have us believing things like A is on and B is on, but A and B are not on, because we want to be able to drop out mechanisms and you just straight out can't do that if you have an actual probability distribution over actual outcomes because A being on and B being on must imply that A and B is being on.

And the second reason is in terms of just computational complexity, it's very likely just going to be impossible to actually have a distribution over realizable or physically possible or logically possible states, because the act of determining whether or not something is logically possible is very hard. I think Eric Neyman has a couple of neat examples where it's just various circuits where it's [#P-hard](https://en.wikipedia.org/wiki/%E2%99%AFP) to determine or to have a distribution over actual states that are satisfied by that circuit, although I'm not familiar with the details enough to talk about them here. And then there was some third reason which I'm forgetting.

### Adversarial robustness for heuristic arguments <a name="adv-robustness"></a>

**Daniel Filan:**
Cool, so one thing that I'm wondering about is: you mention in the paper that it's difficult to get adversarial robustness. There are various reasons you think that you should be able to be fooled by people adversarially picking bad arguments. So often in the AI realm, when I hear that something is vulnerable to adversaries, I'm really worried because I'm like, "Well, what if my AI is an adversary?" I'm wondering, do you think that this worry applies in the heuristic arguments case?

**Mark Xu:**
Yeah, so first all, I guess I'll talk about situations in which I think it's basically impossible to be adversarially robust. So, suppose you have a hash function that gives -1 or 1, and you're estimating the sum of hash of N over N<sup>2</sup>, and suppose your heuristic argument can just point out the values of various hash terms. So your naive heuristic estimator, presumption of independence, is 50/50 between one and negative one, so the estimate is zero; and then your heuristic argument consists of pointing out the values of various hash terms. In this situation, there's just always going to exist arguments that continue to drive you up or continue to drive you down.

And someone's always going to be like, "Hey, hash of 2 is 1, hash of 7 is 1, hash of 9 is 1" and you're just going to be driven up and up. It's possible it should be like hash of N over N<sup>1.5</sup> or something to make it bad. And so, you're in this awkward spot where for any quantity with sufficient noise, even if you expect very strongly the noise to average out to zero, there will exist a heuristic argument that only points out the positive values of the noise and will drive you up, or there's a heuristic argument that only points out the negative values of the noise and drive you down. And so, I think this is naively quite a big problem for the entire heuristic argument paradigm, if you're ever relying on something like someone doing an unrestricted search for heuristic arguments of various forms and inputting them into your heuristic estimator.

So, there's a couple ways out. First, I'll talk about the simplest way out, that I think ends up ultimately not working, but is worth talking about anyway. I think the simplest thing you can do is just have two people searching for heuristic arguments, one person is driving them up and one person driving them down. So, hopefully if you have one search process searching for heuristic arguments to drive you up and one to drive you down, then we don't systematically maybe expect one person to be biased instead.

I think this is a problem. For example, one obvious thing to do is both players have equal amounts of time to find heuristic arguments or something. However, if you consider the previous case with hash of N, suppose instead of being 50/50 1 and -1, hash of N is actually 1/4 probability of 1, 3/4 probability of -1, then in that case the player finding heuristic arguments to go down has a 3x advantage over the player finding heuristic arguments to go up. And so, then you're no good because you can have these quantities where it's far easier to find heuristic arguments to drive you up than to drive you down or vice versa.

**Daniel Filan:**
Although in that case you really should be below zero in expectation, right?

**Mark Xu:**
Yeah.

**Daniel Filan:**
I think there's some example in [the paper](https://arxiv.org/abs/2211.06738) (that people can read) where it really checks out that debate is not going to work. [Examples of this sort are in Appendix E]

**Mark Xu:**
So, you have examples that that make you very sad, and so instead of doing debate, we want to do this other thing that's going to be kind of interesting. So, if you imagine searching for heuristic arguments in the setting where you have hash of N over N<sup>1.5</sup> or whatever, the way that you're going to do this is you're going to first check hash of 1, then you're going to check hash of 2, and then you're going to check hash of 3, et cetera, et cetera. And suppose that you wanted to drive the estimate of the quantity up, then you would check hash of 1, and suppose it's positive, you're like, "Great, let's include Term 1" and then you check hash of 2 and it's negative and you're like, "Let's not include Term 2", and then you check hash of 3 and it's also negative and you're like, "Let's not include that", check hash of 4, it is positive, so then you include that.

And you're carefully going through these terms and eliminating the ones that are negative. However, the interesting thing is if you imagine being the person that's doing this, there in some sense exists a broader heuristic argument that you first considered and then pruned down, which is: you know the values of all these four terms, and then you selected the terms that were only positive.

And so, there's some sense in which, if instead of including only the heuristic argument that you produce at the end, you included the entire search process for the heuristic argument and also I guess the heuristic argument you included at the end, then hopefully the heuristic estimator is forced to be like, "Well, we actually looked at all four of these terms and then we discarded two of them, but the discarding part isn't relevant because I already know all four of these logical facts, so I'm forced to include all four of these terms." And so, then the hope is not that there doesn't exist heuristic arguments that are misleading, but there's no way of searching for heuristic arguments that's systematically misleading if you don't already know the values of the heuristic arguments. And this is related to the presumption of independence being "correct on average" or something. And so if you're presuming independence in a way that's correct on average, then on average people can only... When searching for arguments and just looking at stuff, they can only lead you closer to the truth hopefully.

**Daniel Filan:**
Sure. So the idea is that if in some sense we can... Well, we should be able to have some broad search process over arguments that essentially isn't adversarial and use that, sounds like is the upshot.

**Mark Xu:**
Well, it's: even if the search is adversarial, if you're conditioning on everything that the search process knows while it's searching, then it can't be adversarial, because all it can do is look at values, and it doesn't know what the value is before it looks, and if you've actually presumed independence correctly, then on average everything it looks at has an equal chance of driving you up or down.

**Daniel Filan:**
I guess I have this concern that maybe the point of mechanistic anomaly detection was to help me know how to elicit this latent knowledge or something. And so, I might've been imagining using some AI to come up with arguments and tell me that the thing was bad. And if I'm using that AI to help me get mechanistic anomaly detection and I need mechanistic anomaly detection to help me get that AI be good, then that's some sort of recursion. It might be the bad type of recursion. It might be the good type of recursion where you can bootstrap up, I guess.

**Mark Xu:**
I think it's not very clear to me how this is ultimately going to shake out. I think this kind of adversarial robustness, or how we deal with adversarial robustness, is important. I think there are other ways out besides the way I described. I think that's intuitively what we want to do: we want to just condition your heuristic estimator on everything, even the search for the arguments and also, I don't know, just all the things. And hopefully if you don't let any adversarial selection into your heuristic estimator, then it can't be adversarially selected against or something. So, there's a thing of: if you're a proper Bayesian and someone's adversarially selecting evidence to show you, the thing that's supposed to happen is eventually you're just like, "And now I think I'm being adversarially selected evidence to be shown, and I know this fact and so I'm just being a Bayesian", and then you just update on that fact, and you just aren't wrong on average if you're correct about the kinds of ways in which you're being adversarially selected against.

**Daniel Filan:**
Although, it does require you to... I don't know, I feel like this is a bit of Bayesian cope because in order to do that... When you're a Bayesian, you're supposed to be like, "Oh, I'm just going to get observations and update on observations." But now you're like, "Well, now my new observation is that I'm observing that observation and maybe the observation process is weird," but then I feel like, you've got this layer and after that layer you just trust it and before that layer you worry about things being screened or whatever. And I'm like, "Where do you put the layer?" You had to enlarge your model to make this be... I don't know, it seems a bit-

**Mark Xu:**
I think being a Bayesian is hard, but the thing we're going to try to do is just look over the shoulder of the person selecting the evidence to show you and then see everything that they see, and then hopefully they're not misled on average because they're just looking at stuff.

**Daniel Filan:**
In that case, it's better than the Bayesian case or I don't have the same complaints as I have about the Bayesian case.

### Other approaches to defining mechanisms <a name="other-approaches"></a>

**Daniel Filan:**
So stepping back a bit, there are a few different ways that somebody might have tried to concretize a mechanism. So, there's this one approach where we have something called a heuristic argument, and we're going to work to try and figure out how to formalize that. I guess we haven't explicitly said this yet maybe, but in the paper, it's still an open problem, right?

**Mark Xu:**
Yeah, or we have-

**Daniel Filan:**
There are avenues.

**Mark Xu:**
I guess I would say: we have some heuristic estimator for some quantities that are deficient in various ways, and we're trying to figure out what's up and get better ones for other quantities and then unify them and hopefully all the dominoes will fall over once we quest sufficiently deeply.

**Daniel Filan:**
Nice, so there's this approach. There's also some people working on various causal abstractions. So [causal scrubbing](https://www.alignmentforum.org/posts/JvZhhzycHu2Yd57RN/causal-scrubbing-a-method-for-rigorously-testing) is work by [Redwood Research](https://www.redwoodresearch.org/), there's [other causal abstraction work](https://arxiv.org/abs/2106.02997) by other groups. I'm wondering: out of the landscape of all the ways in which one might have concretized what a mechanism is, how promising do you think heuristic arguments is?

**Mark Xu:**
So, I'm tempted to say something like: if the causal abstraction stuff works out, we'll just call that a heuristic argument and go with that sort of thing. 'Heuristic arguments' is intended to be this umbrella term for this sort of machine-checkable, proof-like thing that can apply to arbitrary computational objects, and I think that's what I'm tempted to say. I think in fact, we do have various desiderata that we think are important for our particular approach to heuristic arguments, and we want them to be proof-like in various ways that e.g. causal abstractions often aren't, or we want them to be fully mechanistic and have zero empirical parts in them. Whereas often, various causal abstraction approaches allow you to measure some stuff empirically and then do deduction with the rest of it. I very much think of us as going for the throat on this sort of heuristic argument stuff.

There's some question you might ask, which is like, "Why do you expect heuristic arguments to apply to neural nets?" And the answer to that is, well, neural nets are just a computational object and we want to have heuristic estimators that work for literally all computational objects, and then the reason why it applies to neural nets in particular is just because neural nets are a thing that you can formally define. Whereas there's a lot of more restricted approaches to explanations that are like, "Well, we only actually care about the neural net thing, and so we ought to define it in particular about the neural net thing". So maybe in summary, we're trying to be in some sense maximally ambitious with respect to the existence of heuristic arguments.

## The research plan: progress and next steps <a name="research-plan"></a>

**Daniel Filan:**
I got you. So, I'd like to move on a little bit to the overall plan. So I think in some of these early posts, the idea is step 1: formalize heuristic arguments, step 2: solve mechanistic anomaly detection given the formalism of heuristic arguments, and step 3: find a way of finding heuristic arguments. And really, I guess these three things could be run in parallel or not necessarily in order. I'm wondering, the last batch of publications was in late 2022. How's the plan going since then?

**Mark Xu:**
So, I think Quarter 1 of 2023 was a bit rough and we didn't get that much done. I think it's sped up since then. So, I can talk about stuff that is... So, things that I think are good that have happened: we have a pretty good formal problem statement about what it means to be a heuristic estimator for a class of functions or a class of circuits that we call bilinear circuits, where just all your gates are squares. So, we have that desideratum and we can talk about what it means to be a good heuristic estimator with respect to that class of circuits in a way that things we think are good satisfy... Or some stuff we have satisfies and some other stuff we have doesn't satisfy. So, hopefully that's just a fully formal problem.

We have a better class of heuristic estimators for cumulant propagation - .we have an upgrade on cumulant propagation that we call reductant propagation, which is just slightly different: instead of keeping track of cumulants, you keep track of a different thing called a reductant, and it's slightly better in various ill-defined ways, but it's more accurate empirically on most distributions of circuits we've tried, et cetera, et cetera and we feel better about it. We have a clearer sense of what it means to do mechanistic anomaly detection, and how that needs to go, and what the hard cases are going to be for doing mechanistic anomaly detection in terms of being unable to distinguish between mechanisms. I noticed I didn't really answer your original question, which is like, "How's it going?" I think it's going par or something. Maybe Q1 was slightly below par and we're doing slightly above par in Q2, and so it's on average par or something.

**Daniel Filan:**
So, par in the sense of you've gotten a better sense of some special cases, not yet knocked totally out of the park, but-

**Mark Xu:**
Or if you told me that this is how much progress we would've made at the beginning of 2023, I would be like, "Okay, that seems good enough to be worth doing: let's do that, this is how we should spend our time" or something.

**Daniel Filan:**
Cool. I'm wondering: given that progress - so out of the steps of formalize heuristic arguments, solve mechanistic anomaly detection given that, and find heuristic arguments, how optimistic are you about the steps? Which ones seem most difficult?

**Mark Xu:**
So, I think the thing that's most difficult is formalizing heuristic arguments in a way that makes them findable. Here's a formalization of heuristic arguments: they're just proofs. And so, there's some sense in which heuristic arguments being compact is intrinsic to the nature of what it is to be a heuristic argument. I think doing useful stuff with heuristic arguments is pretty unlikely to be the place where we fall down. I think it's possible that heuristic arguments get formalized and then we're like, "Darn, we can't do any of the things that we thought we were going to be able to do with heuristic arguments, because it turns out that they're very different than what we thought they were going to be like." I think that would be pretty good though, because we would have heuristic arguments and we would in some sense be done with that part. It would be very surprising to me if they were not useful in various ways. I don't know if that was a complete answer.

**Daniel Filan:**
I think that worked. I'm wondering if there's been any experimental work on trying out mechanistic anomaly detection things beyond the... So, I guess you've mentioned there are various interpretability things that you think won't scale. Is there any promising experimental work you're aware of?

**Mark Xu:**
So, I am not very aware of experimental work in general, but I think Redwood is currently working on what they're calling ELK benchmarks where they're trying to do this sort of mechanism distinction on toy problems like function evaluation. I don't know how that's going because I am not up-to-date on details.

**Daniel Filan:**
Fair enough.

**Mark Xu:**
I think ARC employees often write code to check whether or not heuristic estimators do some things, or check how empirically accurate they are, or find counter examples by random search or something. Probably you don't want to call that experimental work, because we're just checking how accurate our heuristic estimators for permanents of matrices are, or whatever. So, I think the short answer is I think Redwood's doing some stuff that I don't know that much about, and I'm not really aware of other stuff being done, but that is probably mostly because I'm not that aware of other stuff and not because it's not being done, although it probably also isn't being done in the way that I would want it to be done.

**Daniel Filan:**
I got you, so it's about time for us to be wrapping up. Before I close up, I'm wondering if there's any question that you think I should have asked.

**Mark Xu:**
I think people often ask for probabilities that these sorts of things all work out, to which I often say: 1/7 that everything works out roughly the way that we think it's going to work out and is super great within five years-ish - maybe not quite five years now because I've been saying 1/7 over five years for more than a few months. So, maybe it's like four and a half years now. But other than that, I don't think so.

## Following ARC's research <a name="following-arc"></a>

**Daniel Filan:**
Fair enough. So finally, if people are interested in following your research or if we have bright minds who are perhaps interested in contributing, what should they do?

**Mark Xu:**
So ARC posts [blog posts](https://www.alignment.org/blog/) and various announcements on our website, [alignment.org](https://www.alignment.org/). And we're also currently hiring, so you can go to [alignment.org](https://www.alignment.org/) and click the hiring button and then be directed to [our hiring page](https://www.alignment.org/hiring/).

**Daniel Filan:**
Great. Well, thanks for talking to me today.

**Mark Xu:**
You're welcome. Thanks for having me on.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with the transcription. The opening and closing themes are also by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://www.patreon.com/axrpodcast) such as Ben Weinstein-Raun, Tor Barstad, and Alexey Malafeev. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.

