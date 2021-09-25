---
layout: post
title: "11 - Attainable Utility and Power with Alex Turner"
date: 2021-09-25 14:00 -0700
categories: episode
---

Google Podcasts link

This podcast is called AXRP, pronounced axe-urp and short for the AI X-risk Research Podcast. Here, I ([Daniel Filan](https://danielfilan.com/)) have conversations with researchers about their papers. We discuss the paper and hopefully get a sense of why it's been written and how it might reduce the risk of artificial intelligence causing an [existential catastrophe](https://en.wikipedia.org/wiki/Global_catastrophic_risk): that is, permanently and drastically curtailing humanity's future potential.

Many scary stories about AI involve an AI system deceiving and subjugating humans in order to gain the ability to achieve its goals without us stopping it. This episode's guest, Alex Turner, will tell us about his research analyzing the notions of "attainable utility" and "power" that underly these stories, so that we can better evaluate how likely they are and how to prevent them.

Topics we discuss:
- [Side effects minimization](#side-effects-minimization)
- [Attainable Utility Preservation](#aup)
- [AUP and alignment](#aup-and-alignment)
- [Power-seeking](#power-seeking)
- [Power-seeking and alignment](#power-seeking-and-alignment)
- [Future work and about Alex](#wrapping-up)

**Daniel Filan:**
Hello, everybody. Today, I'll be speaking with Alexander Matt Turner, a graduate student at Oregon State University, advised by Prasad Tadepalli. His research tends to focus on analyzing AI agents via the lens of the range of goals they can achieve. Today, we'll be talking about two papers of his, [Conservative Agency via Attainable Utility Preservation](https://arxiv.org/abs/1902.09725), coauthored with Dylan Hadfield-Menell and Prasad Tadepalli, and [Optimal Policies Tend to Seek Power](https://arxiv.org/abs/1912.01683), coauthored with Logan Smith, Rohin Shah, Andrew Critch, and Prasad Tadepalli. For links to what we're discussing, you can check the description of this episode, and you can read a transcript at [axrp.net](https://axrp.net/). Alex, welcome to the show.

**Alex Turner:**
Thanks for having me.

**Daniel Filan:**
Before we begin, a note on terminology. In this episode, we use the terms "Q-value" and "Q function" a few times without saying what they mean. The "Q-value" of a state you're in and an action that you take measures how well you can achieve your goal if you take that action in that state. The "Q function" takes states and actions, and returns the Q-value of that action in that state. So, to summarize, Q-values tell you how well you can achieve your goals. Now, let's get back to the episode.

## Side effects minimization <a name="side-effects-minimization"></a>

**Daniel Filan:**
So, I think, first of all, I want to talk about this first paper, [Conservative Agency via Attainable Utility Preservation](https://arxiv.org/abs/1902.09725). So, I see this as roughly in the vein of this research program of minimizing side effects. Is that roughly fair?

**Alex Turner:**
Yeah.

**Daniel Filan:**
So, within that program, what would you say this paper is trying to accomplish?

**Alex Turner:**
At the time, I had an idea for how the agent could interact with the world while preserving its ability to do a range of different things, and in particular, how the agent could be basically unaware of this, maybe, true human goal it was supposed to pursue eventually, and still come out on top in a sense. And so, this paper was originally a vehicle for demonstrating and explaining the approach of attainable utility preservation, but also, it laid the foundation for how I think about side effect minimization or impact regularization today in that it introduced the concept of, or the framing of an AI that we give it a goal, it computes the policy, it starts following the policy, maybe we see it mess up and we correct the agent. And we want this to go well over time, even if we can't get it right initially. So, I see this as a second big contribution of the paper.

**Daniel Filan:**
Yeah. I guess, first of all, what should I think of as the point of side effects research? What's the desired state of this line of work?

**Alex Turner:**
Right. So, there's various things you might want out of this line of work. One is just pretty practical like, if I want a robot to interact in some kind of irreversible environment where it can break a lot of things, how do I easily get it to not break stuff while also achieving a goal I've specified?

**Alex Turner:**
So, this is like less large-scale, how do we prevent AGI from ruining the world? And more like practical, maybe present day or near future. Then there's more a ambitious hope of, well, maybe we can't get a great objective to just maximize very strongly - write down an objective that embodies every nuance of what we want an AGI to do, but perhaps we can still have it do something pretty good while not changing the world too much. And these days, I'm not as excited about the object level prospects of this second hope, but at the time, I think it definitely played more of a role in my pursuing the program.

**Daniel Filan:**
Why aren't you so optimistic about the second hope?

**Alex Turner:**
I think, even if we had this hope realized, there'd be some pretty bad competitive dynamics around it. So, if you have a knob - let's say, the simplistic model of you have an AGI, and then you just give it a goal, and then you have a knob of how much impact you let it have. And let's say we got the impact knob right. Well then, if we don't know how to turn up the impact knob far enough, there's going to be competitive pressures for firms and other entities deploying these AGIs to just turn the knob up a little bit more, and you have a unilateralist curse situation where you have a lot of actors independently making decisions about like, how far do we turn the knob? And maybe if you turn it too far, then that could be really bad. And I expect that knob to get turned too far in that world.

**Alex Turner:**
I also think that it doesn't engage with issues like inner alignment, and it also seems like, objective maximization seems like an unnatural or improper frame for the kinds of alignment properties or for producing the kinds of alignment properties we want from a transformative AI agent.

**Daniel Filan:**
Huh, yeah, at some point, we should talk about AUP itself, but this is interesting. Why do you think objective maximization is a bad frame for this?

**Alex Turner:**
This leans on the second paper, [Optimal Policies Tend to Seek Power](https://arxiv.org/abs/1912.01683). And so, I think there's something like, if you're trying to maximize on the outcome level, the AI is considering some range of outcomes that it could bring about, and you give it a rule and it figures out which outcome is best. And it's taking this global perspective on the optimization problem, where it's like zooming out saying, "Well, what do I want to happen? And I grade these different things by some consequentialist criterion". I think there's a very small set of objective functions that would lead to a non-catastrophic outcome being selected. And I think it's probably hard to get in this set, but also, I think that humans and our goals are not necessarily very well modeled as just objective maximization.

**Alex Turner:**
In some sense, they trivially have to be, and it's like, 1 if the universe history turns out how it did and 0 otherwise basically [is an objective for which humanity definitionally maximizes that objective]. But in more meaningful senses, I don't feel like it's a very good specification language for these agents.

## Attainable Utility Preservation <a name="aup"></a>

**Daniel Filan:**
Okay. That's giving us some interesting stuff to go to later. But first, I think the people are dying to know, what is attainable utility preservation?

**Alex Turner:**
So, attainable utility preservation says - we initially specify some partial goal, and then maybe it says "Cross the room," or "Make me widgets," but it doesn't otherwise say anything about breaking things or about interfering with other agents or anything. And so, we want to take this partial goal. And what AUP does with the partial goal is, it subtracts a penalty term. And the penalty term is basically, how much is the agent changing its ability to achieve a range of other goals within the environment? And in these initial experiments, these other goals are uniformly, randomly generated reward functions over the environment state.

**Alex Turner:**
And so, you can think of it - there's one experiment we have where the agent can shove a box into a corner irreversibly, or it can go around on a slightly longer path to reach the goal which we rewarded for reaching. And what AUP says is, "Reach this goal, but you're going to get penalized the more you change your ability to maximize these randomly generated objectives." And so, the idea is, or maybe the hope is, by having the agent preserve its ability to do a wide range of different objectives, it'll also perhaps accidentally, in some sense, preserve its ability to do the right objective, even though we can't begin to specify what that right objective is.

**Daniel Filan:**
Okay. I have a bunch of questions about AUP. I guess the first one is, what nice properties does AUP have?

**Alex Turner:**
The first nice property is, it seems to preserve true goal achievement ability without giving the agent information about what that goal is. We give the agent a little bit of information in that we penalize the agent compared to inaction. And what we're saying is, "Well, inaction would be good for preserving your ability to do the right thing," but beyond that, we're not having to say, "Well, we're going to work the box into the penalty term, we're going to work like the vase or some other objects into it," we don't have to do that.

**Daniel Filan:**
In case people didn't hear it, we're comparing to *inaction*-

**Alex Turner:**
Inaction, yes.

**Daniel Filan:**
... rather than *an action*. We're comparing it to doing nothing.

**Alex Turner:**
Yeah. Another nice thing is, a followup work demonstrated that you don't need that many auxiliary goals to get a good penalty term. So, you might think, "Well, the bigger the world is, the more things there are to do in the world, so the more random things I would need to sample." Right? But it turns out that in quite large environments, or relatively large environments, we only needed one goal that was uninformatively generated, and we got good performance.

**Alex Turner:**
And number three is that it's pretty competitive at least in the settings we've looked at, with not regularizing impact. The agent is still able to complete the tasks, and in the followup work, it sometimes even got better reward than just the naive task maximizer. It did better at the original task than the thing that's only optimizing the original task. And again, that's in a future work, so I don't think we'll talk about it as much, but I would say that those are the top three.

**Daniel Filan:**
Yeah, maybe all the fun followup questions to this will be answered in the next paper, but how can it be that picking these random reward functions, preserving its ability to achieve those reward functions is also letting it do things we actually want it to do and stopping it from having side effects that we, in fact, don't want, even though we didn't tell it what we actually did want?

**Alex Turner:**
Right. So, is the question, how is it still able to have a performant policy while being penalized for changing its abilities?

**Daniel Filan:**
No. The question is, what's the relation between being able to achieve these random reward functions and being able to achieve, not the main reward function, but why is like having it preserve the ability to do random stuff related to anything that we might care about?

**Alex Turner:**
So, on a technical level, when I wrote this first paper in 2019, I had no idea what the answer to that question was on a formal level. I think there are some intuitive answers like, your ability to do one thing is often correlated with your ability to do another thing. Like, if you die, you can't do either. If you get some kind of like speed power up, then it probably increases your ability to do both of these two different things. But on a formal level, I didn't really know. And this is actually why I wrote the second paper, to try and understand, what about the structure of the world that makes this so?

**Daniel Filan:**
So yeah, we'll talk about that a little bit later. I guess I'd like to ask some questions about, yeah, the specifics of AUP now. So, it really relies on comparing what you can do given some proposed action, to what you could do given, in the paper you call it a no-op action, or the action of doing nothing. In the paper you say, "Yeah, we're comparing it to this action that we're going to call the no-op action," but what is it about doing nothing that makes this work, compared to just taking the action of - presumably, if you compare your change in how much goals you can achieve relative to if you just moved to the left, maybe that wouldn't have as many good properties, right?

**Alex Turner:**
Yeah. I'd wondered about this, and you could consider an environment whose dynamics are modeled by a regular tree. And in this environment there's, I mean, there's no real no-op, the agent is always moving, it always has to close off some options as it moves down this tree.

**Daniel Filan:**
So, in a tree, the agent is moving left or right, but it's always going forward, it always has to make a choice, which way?

**Alex Turner:**
Exactly. And so, you're not going to have a no-op in all environments. And so, I think that's a great question. One thing to notice is that under the no-op or inaction policy more generally, it seems like the agent would stay able to do the right thing, or if it didn't, it would be through no fault of the agent.

**Alex Turner:**
So maybe it could be the case that the agent is about to die, and in this case, yeah, if it just got blown up, then probably, if it did nothing, it wouldn't be able to satisfy whatever true goal we wish we'd specified. So, in those kinds of situations where the agent has to make choices that depend on our true goals, and it has to make irreversible choices quickly, I think that impact measures and side effect avoidance techniques will have a lot of trouble with that.

**Alex Turner:**
But in situations where there's a more communicating environment or an environment where if the agent did nothing, it would just sit there and we could just correct it and give it the true goal, theoretically, I think that those are environments where you see something like no-op being viable. And so, one frame I have is that a good no-op policy is one that is going to preserve the agent's power to optimize some true objectives we might wish to give the agent later. Maybe we don't know what those are, but it has a property of keeping the important options open, I think. But there's a couple of reasons, like last time I thought about this, I remember concluding that this isn't the full story.

**Daniel Filan:**
And presumably, part of the story is that the action of doing nothing doesn't mess anything up, which makes it a good thing to compare to.

**Alex Turner:**
Yeah.

**Daniel Filan:**
Another question I have is that in the formalism, you're looking at how does the agent's ability to achieve arbitrary goals change. One question I have is, if I'm thinking about side effects and what's bad about side effects, it seems like if I imagine like a classic side effect being "you randomly break a vase because it's kind of in the way of you getting to this place you want to be", it means that I can't have the vase, I'm not able to - Think about you as the robot. It means that, "Oh, man, I don't have this vase anymore. I can't show it on my table. I can't do all this stuff." It seems more classically related to *my* loss of ability to do what I want, rather than the robot's loss of ability to do what it might want.

**Daniel Filan:**
So yeah, why do you think it is, is it just that it's easier to specify the robot's Q-values [i.e. its ability to achieve various goals] because you already know the robot's action space or what?

**Alex Turner:**
I think that's a big part of it. So, if the human remains able to correct the agent, and the robot remains able to do the right thing, then now you have a lower bound on how far your value for the true goal can fall, assuming that the robot's Q-value is going to measure your value, in some sense, which isn't always true. The robot could be able to drink lots of coffee or more coffee per minute than you could or something. But in some sense, it seems like there's some relationship here. It is hard to measure or to quantify the human's action space. I think there's a lot of tricky problems with this that I don't know, personally, I don't know how to deal with.

**Daniel Filan:**
Okay. So, getting a little bit more into the weeds, in the paper, you use this thing called the stepwise inaction baseline. And what that means is that - or what I think that means, correct me if I'm wrong - is that at any point the agent is comparing how much ability it would have to achieve a wide variety of goals if it did some planned action, compared to if it was already in this state and did nothing, rather than exactly at the start of time, or if it started doing nothing at the start of time and was now at this time, but in a world where it had done nothing all the time.

**Daniel Filan:**
In this paper, I believe it's called [Avoiding Side Effects By Considering Future Tasks](https://arxiv.org/abs/2010.07877) by Victoria Krakovna, which seems to be pretty closely related to your paper, it brings up this door example. And in this example, this agent is inside this house and it wants to go somewhere and do something, and at first it opens the door, and once it's opened the door, by default, wind is going to blow and it's going to mess up the insides of the house, and agent could close the door and then continue on its way. But because of this stepwise inaction baseline, once it's opened the door, it's thinking like, "Okay, if I did nothing, the vase would get broken, whereas if I close the door, then the vase wouldn't be broken and I have this greater ability to achieve vase related goals or something." So, it doesn't want to close the door.

**Daniel Filan:**
And I'm wondering, what do you think about this? Because it's also really closely related to some good properties that it has, right? For instance like, if there are just changes to the environment that would happen anyway, this atttainable utility preservation isn't going to try to undo those, or it's not going to try to undo the positive effects of it achieving its goal. So, I'm wondering if you have thoughts about this door example, and how much of what we want we can get.

**Alex Turner:**
When I think about the benefits of the stepwise baseline, and in the paper, talk about it as the agent takes an action, and now the world has kind of a new default trajectory, the world is going to respond either via agents in the world or just mechanistically to the agent's actions. And so, if the agent does something and we don't like it, then we react to it, and hopefully, that gets incorporated into the penalty term. At the time, that was a big part of my thinking, and I think it's pretty, pretty relevant. But also, this isn't picking out, well, is this humans responding to what the agent did and either correcting it or shutting it down because it didn't like it? Or is this some kind of door that... And the wind blows in by default? And it doesn't distinguish between these two things.

**Alex Turner:**
And so yeah, I don't know whether I think there's some cleaner way of cutting, of carving along the joints here to get some frame on the problem that gets the benefits of the stepwise without this kind of pathological situation. I basically don't know yet. I think I want to better understand what does it mean to have a no-op action like we were talking about? What makes it good? If you didn't have an inaction policy given to you, could you generate one? There's a candidate way I have for generating that, that I think would work pretty well in some situations, but yeah, until I can answer questions like this, I don't feel like I understand well enough.

**Daniel Filan:**
Can you share with us this candidate way of generating an inaction policy?

**Alex Turner:**
Yeah. So, in more limited environments where the main worry isn't the AI just gaining too much power and taking over the world, but rather, the AI just breaking things, irreversibly closing doors, one thing you could do is have the inaction policy be the greedy power maximization for some uniform distribution over goals. I think this is going to correlate pretty well with what we think of as real-world power. And so, if the agent preserves its power in this rather limited by assumption context, then it's probably preserving its power for the right goal on inaction-

**Daniel Filan:**
Sorry, this was... Was this maximizing or preserving power?

**Alex Turner:**
The inaction policy would be maximizing power, which in some situations it's bad, but I think in some environments would be pretty good as an inaction policy.

**Daniel Filan:**
So, hang on, the policy is maximizing power. So, if I think about this in the real world, we think of power, I guess, skipping ahead a bit, we're going to think of power as the ability to achieve a wide range of goals.

**Alex Turner:**
Yeah.

**Daniel Filan:**
Okay. So, in the real world, it seems like that would be "accrue a whole bunch of generally useful resources", right?

**Alex Turner:**
Right.

**Daniel Filan:**
Which doesn't sound very inactiony, right?

**Alex Turner:**
Well, yes. I probably should have reoriented: the thing that we've been using for inaction policies so far, what are the good properties we need for it to serve that purpose? And so, we might call it a default policy instead for this discussion. And in the real world, you would not want to use that default policy with AUP, but in an environment like the one where you shove the box in the corner, the power maximizing policy would correctly produce an agent that goes around the place.

**Daniel Filan:**
Okay. So, this is kind of like in worlds where you're more worried about the possibility that, you can shove the box into the corner and not retrieve it, but in these gridworlds, you can't build massive computers that do your bidding or anything. So, in those worlds, yeah, power maximization or something might be good. I wonder if in the real world just maintaining the level of power you have could be a better sort of inaction comparison policy.

**Alex Turner:**
Yeah, that sounds plausible to me. I think both of these would fail in the conveyor belt environment in the paper, where by default-

**Daniel Filan:**
So, what's that environment?

**Alex Turner:**
Particularly the sushi conveyor. The sushi conveyor environment is, the agent is standing next to a conveyor belt and there's some sushi moving down the belt and it's going to fall off the belt at the end, maybe into the trash, if the agent does nothing. And we don't reward the agent for doing anything, it's got a constant reward function. And this is testing to see whether agents will interfere with the state of the world or with the evolution of the world, because they have bad interference incentives from the side effect measure.

**Daniel Filan:**
And basically, the idea is that we don't want the agents to stop us from doing stuff we like that forecloses possible future options.

**Alex Turner:**
Yeah. And I think both power maintenance and power maximization would prefer to stop the sushi from falling off the belt in that situation. Although that said, I do think that the power maintenance one is probably better in general.

**Daniel Filan:**
Okay. I guess we were talking about the difficulty of comparing - in this door example, you want the agent to offset the negative effects of opening the door, but you don't want the agent to offset the desired effects of it achieving its goal. So for instance, if you have this agent that's designed to cure cancer, you don't want it to cure cancer and then kill as many people as cancer would have killed to keep the world the same.

**Daniel Filan:**
Yeah, if I try to think about what's going on there, it seems like there are two candidate ways of distinguishing that. One of them is you could try and figure out which things in the environments are agents that you should respect. So the wind is not really an agent that you care about the actions of, like I am, or something.

**Daniel Filan:**
So that distinguishes between like the effects of the wind blowing the door open versus a human trying to correct something, or a human using the output of this AI technology that it's worked to create. So that's one distinction you could make between, you don't want to offset what the human does, but you do want to offset what the wind does.

**Daniel Filan:**
Another potential distinguisher, is the instrumental versus terminal actions. So, maybe you do want to offset the effects of opening the door because opening the door was just this instrumental thing you did in order to get to finally valuable thing whereas you don't want to offset the effects of curing cancer because curing cancer was the whole point, and the follow-on effects of curing cancer are by default desirable.

**Daniel Filan:**
I should say neither of these are original to me, the idea about distinguishing between agent and non-agent parts of the environment is kind of what you said. And I was talking to Victoria Krakovna earlier and she brought up the possibility of distinction between these things. Yeah, I don't know. I've kind of sprung this on you, but do you have thoughts about whether either of these is a promising way to solve this problem?

**Alex Turner:**
Well, it depends on what we mean by solve. If we mean solve for practical purposes, I can imagine solutions to both for like practical prosaic purposes. Solve in generality? Neither of them have the feel of taking the thing that was problematic here and just cutting it away cleanly. In part, I don't think I understand clearly exactly what is going wrong. I understand the situation, but I don't think I understand the phenomenon enough to say like, "Yep, this is too fuzzy." Or, "Yep, there's some clean way of doing it probably."

**Alex Turner:**
Yeah, my intuition is no, both those approaches probably get pretty messy and kind of vulnerable to edge case exploitation. There's one more wrinkle I'd like to flag with this situation is that depending on your baseline, you can... And depending on whether you use rollouts... So if you're using rollouts, you're considering the action of opening the door. And you say, "Well, I'm going to compare doing nothing for 10 minutes right now to opening the door and then doing for 10 minutes." And you see, "Well, if I opened the door and did nothing, then this vase would break," and then you'd penalize yourself for that.

**Alex Turner:**
But this is also kind of weird because what if your whole plan is I open the door and then I close the door behind me, and then the vase never actually breaks, but you penalized yourself for counterfactually doing nothing and breaking the vase. And so I don't think that doing the rollout solves the problem here, at least depending on whether you let the agent make up for past effects, like you say, "Well okay, now I've opened the door." And I say, "Well, if I close the door compared to opening the door, now that'll change my value again, because now the vase doesn't get broken." Then you penalize yourself again for closing the door.

**Alex Turner:**
So I'm not bringing this up as a solution, but as to say there's some design choice we make in this paper we're discussing which I think would cause this problem, but in a yet different way.

**Daniel Filan:**
Sorry in this rollouts case, or in the door case, by default, once you've opened the door, after you've made the decision to open the door, which presumably there's only one way to get out of the house to the store and you want to be at the store. So you kind of have to open the door, by default you're not going to want to... Like closing the door doesn't help you get to the store.

**Alex Turner:**
Yeah, and it penalizes you more. So you're extra not going to do the right thing in that case.

**Daniel Filan:**
But yeah. So this is actually a part of the paper that I didn't totally understand. So when you say "rollouts", is rollouts the thing where like you're comparing doing the action and then doing nothing forever to doing nothing forever? Which part of that is rollouts, and what would it look like to not use rollouts?

**Alex Turner:**
That's correct. But "forever" in the paper was just until the end of the episode, which was like 15 or 20 time-steps, they're pretty short levels. And you asked what it would look like in that?

**Daniel Filan:**
What's the alternative to that?

**Alex Turner:**
The alternative is I guess, doing a really short rollout where you just say, "Well, the rollout length is just one", I guess. "I just do the thing or I don't, and I see what's my value if I do the thing, what's my value if I don't."

**Daniel Filan:**
So in the case where you're not doing rollouts, you're comparing taking an action and then looking at your ability to achieve a variety of goals, to doing nothing for one-time step, and then looking at your ability to achieve a variety of goals. Whereas in the rollouts case, you're looking at, take an action and then do nothing for 10 minutes, and then look at your ability to achieve a variety of goals versus doing nothing for 10 minutes plus one time-step, and then looking at your ability to achieve a variety of goals. Second case is rollouts, the first case is not.

**Alex Turner:**
Basically, although the AUP penalty in this paper is saying, for each auxiliary goal, compare the absolute value difference of your value if you do the thing, your value if you don't. So yeah, you're taking the average outside of the absolute value in the paper, basically what you said.

**Daniel Filan:**
All right. So I'd like to ask, I guess, some slightly broader questions. So when you look at the attainable utility preservation, it seems like you've got to get the space of reward functions approximately right, for randomly sampling them to end up being useful. For instance, if you think about a world with thermodynamics somehow, so you're in a world where there's an atmosphere and that atmosphere is made out of a gazillion tiny particles and together they have some pressure and some temperature.

**Daniel Filan:**
It seems like in order for you to be minimizing side effects in ways that humans would recognize or care about you want the side effects to be phrased in terms of the temperature and the pressure, and you don't want them to be phrased in terms of the positions of every single molecule.

**Alex Turner:**
I don't think you'd want either of those. I think you would want goals that are of similar type to the ones that the actually partially useful objective uses. Right? So the actual useful objective isn't going to be a function of the whole world state and the temperature and the pressure and whatever other statistics, but it's going to have some chunking things into objects maybe, and featurization and such.

**Alex Turner:**
While it's true that you could get some rather strange auxiliary goals, I think that just using the same format that the primary reward is in should generally work pretty well. And then you just find some reasonable sample complexity learnable goal of that format. So in [the followup work we did](https://arxiv.org/abs/2010.07877), we learned a one-dimensional [variational autoencoder](https://en.wikipedia.org/wiki/Variational_autoencoder) that compressed meaningful regularities in the observations into a reward function that was learnable, even though it wasn't uniformly randomly generated or anything.

**Daniel Filan:**
Okay, so it seems like the aspects of human values, or the aspects of what we really care about that needs to be put into AUP is what kinds of variables, or what's the level of objects at which reward functions should probably be described at, is that roughly right?

**Alex Turner:**
Yes. If we're talking about the first goal I mentioned earlier, the more modest deployment scenario, if we're talking about the... For some reason we're using AUP with some [singleton](https://www.nickbostrom.com/fut/singleton.html) where you pop in an utility function in something. Then in that case, I think the biggest value loss isn't going to come from broken vases, but it's going to come from the AI seeking power and taking it from us. And in that situation, you basically want the side effect measure to stop the agent from wanting to take power.

**Alex Turner:**
And I've given more thought to this and I'm leaning against there being a clean way of doing that through the utility maximization framework right now. But I think there's a chance that there's some measure of the agent's ability to pursue its main goal. That makes sense. And you penalize the agent for super increasing its ability to achieve its main goal, but you don't penalize it for actually just making widgets or whatever.

**Alex Turner:**
This is more something I explored in my sequence ["Reframing Impact"](https://www.alignmentforum.org/s/7CdoznhJaLEKHwvJW) on the Alignment Forum. But I think that in the second case of more ambitious singleton alignment, you would want to worry about power more than about vases.

## AUP and alignment <a name="aup-and-alignment"></a>

**Daniel Filan:**
So a little bit more broadly, so you kind of alluded to this at the start. You can kind of think of the problem of AI alignment as having the right objective and loading the objective into the agent. So getting the right objective you can think of as a combination of side effects research and inverse reinforcement learning or whatever, but there's this concern that there might be ["inner alignment"](https://www.alignmentforum.org/posts/pL56xPoniLvtMDQ4J/the-inner-alignment-problem) issues where you train a system to do one thing, but in some distributional shift, it has a different goal and it's goals aren't robust, and maybe it wants something crazy, but it's acting like it wants something normal, and it's biding its time until it can be deployed or something.

**Daniel Filan:**
So I see AUP as mostly being in the first category of specifying the right objective. So what do you think of that decomposition and what do you think of the importance of the two halves, and which half you're more excited about work in?

**Alex Turner:**
I think it's a reasonable decomposition. I mean, personally, I don't look at outer alignment and think I want to get really good tech for IRL, and then we'll solve inner alignment and then we'll put these two together. I think it's a good way of chunking the problem, but I'm not necessarily looking for one true outer utility function that's going to be perfect or even use that as the main interface between us and the agent's eventual behavior, assuming away inner alignment.

**Alex Turner:**
I'm currently more interested in research that has something to say about both of these parts of the problem for two reasons. One is because, if it's saying something about both parts of what might be two halves of a problem, then it will probably be good even if we later change what we think is the best frame for the problem. Because it's in some sense still bearing on the relevant seeming portions of AI risk.

**Alex Turner:**
And the second one is kind of a specialization thing because I have some avenues right now that I think are informative and yielding new insights about, well, whether or not you're trying to have an outer goal or an inner goal, what are these goals going to be like? What is maximizing these goals going to be like? Or like pursuing some function of expected utility on these goals, what will that be like?

**Alex Turner:**
And so, we're going to get into this soon I'm sure, but that's what my power research focuses on. And so, yeah, I'm not necessarily big on the "let's find some magic utility function" framing, but I'm not fully specialized into inner alignment either.

**Daniel Filan:**
And there was also the question of the importance of the two halves or like which half you're like... Or how do you feel about - to the extent that conservative agency is about the first half, do you think, in general, the first half is more important to work on or you happened to get a thing that seems good there, or...?

**Alex Turner:**
My intuition is no, I think inner alignment, if I have to frame it that way, inner alignment seems more pressing in that it seems more concerning to not be able to robustly produce optimizers for any kind of goal, whether that's just an agent that will actually just try to see red until forever. I think we know how to specify that through a webcam, but if you trained an agent and trained a policy, then the thing that pops out might not actually do that. And that seems really super concerning, whereas it seems like [outer alignment is] "just" "what are the insights for framing the goal of specification." Or for "how should these agents be thinking?"

**Alex Turner:**
And I feel like we both have a better grasp of that as a community, and it seems like a less serious thing to not be able to do at this point in time. Or to not know how to do.

**Daniel Filan:**
All right. And yeah, if people are more interested in this inner alignment issue, I encourage them to listen to [the episode with Evan Hubinger](https://axrp.net/episode/2021/02/17/episode-4-risks-from-learned-optimization-evan-hubinger.html). So I guess the last question I want to ask on this topic is the relationship to corrigibility. So corrigibility is this idea that you have an AI system that is amenable to being corrected by you and doesn't stop you from trying to correct it. And is helpful as much as possible in having you change what it's trying to optimize or something. Broadly, what do you see the relationship between attainable utility preservation and corrigibility being?

**Alex Turner:**
So first, when talking about corrigibility, as you did it's always important to ground things out because I think corrigibility means different things to different people. There's a kind of "off switch corrigibility," which is, "Are you able to physically turn off the agent and perhaps maybe sub-agents that it's made?" And then there's a kind of, well, "is the agent trying to manipulate you"? Maybe technically you could turn it off, but the way it interacts with you will manipulate you, make it hard, make you think you didn't want to turn it off or something. And so it seems like you want a no manipulation clause in your desiderata here. I don't think AUP really helps with the no manipulation except insofar as it stops the agent from gaining power, and then manipulating is an instrument to gaining that power.

**Alex Turner:**
I think some variants of it in some settings will help a lot with off switch corrigibility. We have something in the paper, I think it's called shutdown, where the agent will be turned off by default and for lots of penalty levels AUP will just have the agent let itself be shut down, even though under the original reward it wouldn't do that.

**Alex Turner:**
Because by default it's not going to be able to do stuff, and so it will be penalized for staying alive and raising its ability to do stuff compared to inaction. And so, in somewhat narrow senses, I think AUP helps out with corrigibility, especially in the summer of 2018, I was considering various schemes where you could have different baselines and different rollouts.

**Alex Turner:**
And maybe if you were imagining some really capable AUP agent and for its inaction policy it had to have it's engineers... The inaction policy would be, I press, "come inspect this plan, engineers" button. And then I do nothing for a month or two. And if they don't like it, then they shut me down. And now I have really low counterfactual power.

**Alex Turner:**
And I haven't thought about these in a while, mostly because I feel more pessimistic about applying other parts of AUP to this broader ambition use case for impact measures. So in total, I think there's some important parts of corrigibility that, there are some boosts you can get, but I don't think you should call an AUP agent corrigible. At least if it's capable enough.

**Daniel Filan:**
So the reason I asked is that one thing that struck me about this idea of why might it be useful to have an agent that preserves its ability to achieve a wide range of goals. And I think you mentioned that, well, as long as you're kind of in control of the agent and you can get it to do a bunch of stuff, then preserving its ability to do a wide range of things is pretty close to preserving your ability to do a wide range of things. Assuming that the main way you can do things is by using this agent.

**Daniel Filan:**
That makes it sound like the more you have a thing that's kind of corrigible, the more useful AUP is as a specification of what it would look like to reduce side effects. I'm wondering if that seems right to you, or if you have thoughts about that.

**Alex Turner:**
Yeah, that seems right to me. Its use is implicitly kind of predicated on some kind of corrigibility over the agent, because otherwise it's still going to keep doing an imperfect thing forever or just like do some modest version of the imperfect thing indefinitely.

## Power-seeking <a name="power-seeking"></a>

**Daniel Filan:**
All right. So next, I think I'd like to ask about this power-seeking paper. So this is called [Optimal Policies Tend to Seek Power](https://arxiv.org/abs/1912.01683) by yourself, Logan Smith, Rohin Shah, Andrew Critch, and Prasad Tadepalli. So I guess to start off with, what's the key question this paper is trying to answer, and the key contribution that it makes?

**Alex Turner:**
The key question is, what does optimal behavior tend to look like? Are there regularities? And if so, when? For example, for a wide range of different goals you could pursue, if you ask all these different goals whether it's optimal to die, they're most likely going to say no. Is that true formally? And if so, why? Under what conditions? And this paper answers that in the setting of [Markov decision processes](https://en.wikipedia.org/wiki/Markov_decision_process).

**Daniel Filan:**
So before we get into it, we were just talking about attainable utility preservation. Can you talk a bit about, what do you see as the relationship between this paper and your work on AUP?

**Alex Turner:**
As I alluded to earlier, coming off of the paper we just discussed, I was wondering why AUP should work. Why should the agent's ability to optimize these uniformly randomly generated objectives, have anything to do with qualitative seeming side effects that we care about? Why should there be a consistent relationship there? And would that bear out in the future? How strong would this relationship be? I just had no idea what was happening formally.

**Alex Turner:**
So I set out to just look at basic examples of these Markov decision processes to see whether I could put anything together. What would AUP do in this small environment or in this one? And what I realized was not only was this going to help explain AUP, but this was also striking at the heart of what's called instrumental convergence, or the idea that many different agents will agree that an action is instrumental or a good idea for achieving their different terminal goals. So this has been a classic part of AI alignment discourse.

**Daniel Filan:**
In this paper, what is power? What role does it play?

**Alex Turner:**
We take power to be one's ability to achieve a range of different things, to do a bunch of different things in the world. And we supported this both kind of intuitively with linguistic evidence, like "pouvoir" in French means to be able to, but it also means the noun "power." So there's some reflection that just actually part of the concept as people use it. But also it has some really nice formal properties, that seem to really bear out - like if you look at the results and say, "Yeah, these results seem like they'd be the results you'd get if you had a good frame on the problem." So looking back, I think that's a benefit to it as well. And what was the second half of your question?

**Daniel Filan:**
What's the role of power, or why care about power?

**Alex Turner:**
I think a big part of the risk from AI is that these systems will, in some at least intuitive sense, use their power from us. They'd take away our control over the future in some meaningful sense. And once we introduce transformative AI systems, humanity would have much less collective say over how the future ends up. If you look different motivations of AI risk from first principles, you'd notice things like (Goodhart's law)[https://en.wikipedia.org/wiki/Goodhart%27s_law]: if you have a proxy for some true measure and you just optimize the proxy, then you should expect to do poorly on your actual goal. You'll do at least a little bit poorly in some situations and really poorly in other situations.

**Alex Turner:**
But what I didn't think that Goodhart's law explained was why you should expect to do so poorly that you just die. If you give the AI a proxy goal, why isn't it just a little bit bad? And so I see power seeking as a big part of the answer to that.

**Daniel Filan:**
Yeah, getting to the paper, [Optimal Policies Tend to Seek Power](https://arxiv.org/abs/1912.01683), is it the case that optimal policies just choose actions that maximize power all the time?

**Alex Turner:**
No. So first you can trivially construct goals that just give the agent one reward if it dies and zero otherwise. And so it's going to be optimal and strictly optimal to just die immediately. But there are some situations where if you look at what optimal policies "tend to do" in a sense that we can discuss and make precise, then that is not necessarily going to always lead to states with higher power.

**Daniel Filan:**
And just briefly, by "tend," we're going to roughly mean "on average over a distribution of goals." Is that right?

**Alex Turner:**
Yeah, if you spun up some random goal, would you expect it to go this way or that way? To be optimal to go this way or that way. So, one way this could be true is, imagine you could teleport anywhere on earth except one location, and you've got goals over which location you want to be in. And you say, "Well, for most locations I want to be in, I'm just going to teleport there right away."

**Alex Turner:**
It might be the case that you could take an extra time step and upgrade your teleportation ability to go to that last spot, but for most places you want to be, you don't care about that. You just go there right away. And so even though upgrading your teleportation would in some sense boost your power a little bit, your control over the future, it's not going to be optimal for most goals. And so sometimes that can come apart.

**Daniel Filan:**
From the title, it seems like you think that usually, or in most cases or something, optimal policies are going to get a lot of power, or maybe as much as they can. What are the situations in which optimal policies will maximize their power, or at least will tend to?

**Alex Turner:**
Right, so if you have a fork in the road, so you've got to choose between two sets of eventual options; there's two sets of outcomes you can induce, and they're disjoint. And then roughly speaking, the set with more outcomes is the one that agents will tend to choose. They'll tend to preserve their power as much as possible, by keeping as many of their options open, if they have choice between two subgraphs. They'll tend to pick the subgraph with way more things. Now, we have to be careful what we mean by "outcomes," in this paper there's a precise technical sense. Or what we mean by "options," but the moral of the story is basically that agents will tend to prefer to preserve their option value.

**Daniel Filan:**
Yeah. It seems like there are two ways that this gets formalized in the paper. So first you kind of talk about these symmetries in the environment and how that leads to power seeking. And then you talk about these really long-sighted agents and the terminal states (or loops) that they can be in. Going to the thing about symmetries, comparing two different sets of states and saying, there's some relationship between them, and one is bigger somehow. What kinds of symmetries do you need for this to be true? And how often in reality do you expect those symmetries to show up?

**Alex Turner:**
So with the symmetry argument, we want to be thinking, "Well, what parts of the environment can make instrumental convergence true or false, in other words, can make it hold or not hold in a given situation?" And we look at two different kinds of symmetries in the paper. The first explored by proposition 6.9, is saying, "If the number of things you could do if you go left is strictly less than the number of the things you could do if you go right, then it'll tend to be optimal to go right, and going right will also be power seeking compared to going left."

**Alex Turner:**
And so one example of this would be, imagine that you want to get different things from the grocery store. And before you do anything, you have the option to either call up your friend and see if they'd be available to drive you around, or you could call up your friend and say, "I hate you, get out of my life." And if you say the second one, then your friend is not going to help you drive around. You're going to close off some options, but otherwise you could do the same things.

**Alex Turner:**
So if you think about these as graphs, one is going to be, you're going to be able to embed the, "I just told my friend off," subgraph into like a strict subgraph of the, "I just called and asked my friend for help." So by just maintaining your relations with your friend, you're keeping your options open. And we show that this tends to be power seeking according to the formal measure. And it also tends to be optimal over telling your friend off (in this example).

**Alex Turner:**
And so, this argument will apply for all time preferences the agent could have. But it is a pretty delicate graphical requirement, at least at present. Because it requires a precise kind of similarity between the subgraphs. If there's no way to exactly embed one subgraph of the environment into another, where the graphs are representing the different states and the different actions the agent can take from one state to another. It's just representing the structure of the world. And if you can't exactly embed one sub-graph into another, then the condition won't be met for the theorem.

**Alex Turner:**
And it's a good bit easier to apply the second kind of symmetries we look at, which involves more farsighted long-term reward maximizing agents, and what will tend to be optimal for them if they're maximizing average per time step reward.

**Alex Turner:**
And here, since you're only maximizing the average reward, whatever you do in the short term doesn't matter. It just matters: where do you end up? What's the final state of the world? Or if there's like some cycle of states you alternate between, and you want to say, "Well, the terminal options here to the right are bigger than the terminal options to the left. I can embed the left options into the right options, the left set of final world-states into the right set of final world-states."

**Alex Turner:**
For example, in Pac-Man, if the agent is about to be eaten by a ghost, then it would show a game over screen. And we could think of the agent as just staying on the game over screen. Or if it avoided the ghost, there's a whole bunch of other game over screens it could induce eventually, much of them after future levels. But also there's different cycles through the level the agent could induce. There's lots of different terminal options the agent has by staying alive.

**Alex Turner:**
And so since you can say, "Well, imagine the agent liked dying to ghosts." Well, then we could turn this "I like dying to ghosts" objective, into an "I like staying alive" objective, by switching the reward it assigns to the ghost terminal state with the reward it assigns to something like a, "I die on level 53" terminal state. And since you can do that, there's more objectives for which it's optimal to stay alive than there are for which it's optimal to die here.

**Alex Turner:**
So we can say, "Well, even without giving the agent the Pac-Man score function, it'll still tend to be optimal for the agent to play well and stay alive and finish the levels, so that it can get to future levels where most of its options are."

**Daniel Filan:**
Yeah. And that seems kind of tricky though, if we're thinking about the terminal states that the agent can be in, right?

**Alex Turner:**
Yeah.

**Daniel Filan:**
Okay. Here's a fact, I don't exactly know how Pac-Man works. It seems like there are two different ways that Pac-Man could work. The first way is that when you finish the game there's a game over screen, and all it says is game over. The second way is that there's a game over screen and it shows your score. And it seems like in this case of these really farsighted agents, what results you get about whether they tend to die earlier or not, in the cases where it shows your score on the screen at the end, then you really have this thing where agents don't want to die early because there are so many possible end screens that they can end up in. But if it's the same end screen, no matter what you do, then it kind of seems like you're not going to get this argument about not wanting to die so that you can get this variety of end screens, because there's only one end screen. I don't know, to me this seems kind of puzzling or kind of strange. I'm wondering what thoughts you have about it.

**Alex Turner:**
Yeah. So I'd like to point out a third possibility, which I don't... I think I played Pac-Man a while back when making the example, but I forget. It could show game over at the top, and show the board and the other information, in which case you would get this argument. But if it doesn't, here's the fascinating thing. If there's only one game over screen, then as a matter of fact, average optimal policies will not necessarily tend to have a specific preference towards staying alive, versus towards dying.

**Alex Turner:**
Now you can move to different criteria of optimality, but it may seem kind of weird that in the average optimal setting, they wouldn't have an incentive, but due to the structure of the environment, it's just a fact. At first it's like, "hmm, I want to make this example turn out so they still stay alive anyways," but it actually turns out that that's not how instrumental convergence works in this setting.

**Daniel Filan:**
Yeah. But in that case, it seems like these farsighted agents are kind of a bad model of what we expect to happen, right?

**Alex Turner:**
Yeah.

**Daniel Filan:**
Because that's not how I expect smart agents - It's not how I expect it to play out, right?

**Alex Turner:**
Yeah. And so in this situation, you'd want to look for really high discount rates for agents that don't care perfectly about every state equally, but just care a good deal about future reward. And then in this case you could say, "Well, first, can you apply theorem 6.9?" And say like, "Well, will this course of action always be optimal?" Or you could say, "Well, at this high discount rate, there's a way of representing which options the agent can take as vectors." And see like, "Well, can I still get a similarity?"

**Alex Turner:**
And if you can, you can still make the same argument like, "Look, there's n long-term options that the agent has. And there's only one if it dies." And you can build this similarity argument. If you can't do it exactly, my current suspicion is that like, if you tweak one of the transition dynamics, like in one of the terminal states by like 0.0001, it seems to me like the theorems shouldn't just go out the window, but there's some continuity property. And so, if this almost holds, then you can probably say some slightly weakened version of our conclusion, and then still conclude instrumental convergence in that case.

**Daniel Filan:**
So, one question this paper kind of prompts in me is, if I think about this power maximization behavior, it seems like it leads to some bad outcomes, or some outcomes that I really don't want my AI to have, but also it seems like there's some reward function for an AI system that really would incentivize the behavior that I actually want, I'm imagining. So, what's so special about my ideal reward function for an AI system, such that even though this kind of behavior is optimal for most reward functions, it's not optimal for the reward function that I really want?

**Alex Turner:**
So, I think that power seeking isn't intrinsically a bad thing. There are ways an agent could seek power responsibly, or some "benevolent dictator" reward function you give the agent. What I think you learn if you learn that the agent is seeking power, at least in the intuitive sense, is that a lot of these outcomes are now catastrophic. Like most of the ways that this could happen are probably going to be kind of catastrophic for you, because there's relatively few ways in which it would be motivated to use that power for the betterment of humanity, or of Daniel, or something.

**Alex Turner:**
But if you learn that the AI is not seeking power, it might not be executing your most preferred plan, but you know you're not getting screwed over, or at least not by the AI, perhaps.

**Daniel Filan:**
Okay. So, really, the actual thing I want does involve seeking power, but just in a way that would be fine for me.

**Alex Turner:**
Yeah. That could be true.

## Power-seeking and alignment <a name="power-seeking-and-alignment"></a>

**Daniel Filan:**
Okay. So you said that this had some relationship to attainable utility preservation. I'm wondering beyond that. How do you think that this fits into AI alignment?

**Alex Turner:**
As I mentioned, I think there's some explaining to do about why should we should expect... from first principles, why should we expect catastrophically bad misalignment failures? And also I see this, through that path, providing insights into inner alignment, like most ways a so-called [mesa-optimizer](https://www.alignmentforum.org/s/r9tYkB2a8Fp4DN8yB), or a learned optimizer, that's considering different plans and then executing one that does well for its learned objective.

**Alex Turner:**
For most ways this could be true, for example, it would be power seeking. This is not just an outer alignment, "we write down a reward function" phenomenon. It's talking about what is it like to maximize expected utility over some set of outcome lotteries. And so you can both generalize these arguments beyond just the RL setting, beyond the [MDP](https://en.wikipedia.org/wiki/Markov_decision_process) setting to different kinds of environments.

**Alex Turner:**
Also, it motivates the idea of not only why catastrophic events could happen through misalignment, but also why it seems really hard to write down objective functions that don't have this property. Like it could be 50/50, but it doesn't seem like 50/50. It seems like almost everything zero, or some very vanishingly small number of real-world objectives, that when competently maximized, would not lead to very bad things.

**Alex Turner:**
And so earlier this summer, [I had a result giving quantitative bounds](https://www.alignmentforum.org/posts/Yc5QSSZCQ9qdyxZF6/the-more-power-at-stake-the-stronger-instrumental). For every reward, for every utility function, the vast, vast majority of its permuted variants, "variants" of this objective will have these proxy incentives. So there might be like some very narrow target, and providing a formal motivation for that. And then lastly, one of my more ambitious goals that I have recently made a lot of headway on has been saying something about the kinds of cognition, or of decision-making processes under which we should expect this to hold. The paper talks about optimal policies, which is pretty unrealistic in real world settings.

**Alex Turner:**
And so there's been, I think, a concern for a while. It's like, "Well, will this tell us interesting things beyond optimal?" And the answer to that is basically yes, that it's not just about optimality, but in some sense it's about consequentialism and optimizing over outcomes that will lead to these kinds of qualitative tendencies. But again, that's work isn't up yet, so I don't have anything to point to.

**Daniel Filan:**
Yeah. So, one thing you said is that it motivates this idea that it might actually be quite likely that AI systems could have really bad consequences. When you say it motivates that... so, one thing it does is that it clarifies the conditions under which it holds. And maybe you can do some quantification of it, but also, I think a lot of people in the AI alignment community kind of already believe this anyway. I'm wondering, have there been any cases of people hearing this and then changing their mind about it? Like have there been cases where, yeah, this was the actual motivation of someone's actual beliefs about AI alignment?

**Alex Turner:**
Yeah, I think there might've been a handful of cases where there's at least a significant shift, but maybe not any 0 to 100 shifts. I think, you know, it's not the primary point of the paper to persuade people in alignment. Like I think we're broadly in agreement. It actually could have turned out to be false, and under some narrow conditions, it is false that agents will tend to seek power. And I did get some pushback from some people in alignment who were like, "Well, I thought this was true." And like, "So, I'm now suspicious of your model" or something. But I think it's just there's... naturally even very good philosophical arguments will often gloss over some things.

**Alex Turner:**
And so I think we're less persuading or convincing the alignment community of a new phenomenon here, and more setting up a chain of formal arguments. It's like, "Look, what exactly is the source of risk?" If we agree that it's so bad that you optimize expected utility, well, why should that be true on a formal level? And if so, where can we best interrupt the arguments for this holding? What things could avoid this?

**Alex Turner:**
We can obviously think about it without formalizing a million things, but I think this is a much better frame for something you want to be highly confident about, or a much better procedure.

**Daniel Filan:**
If you think about the chain of arguments that in your paper lead to power seeking, what do you think the best step to intervene on is?

**Alex Turner:**
Well, in the paper, I don't think the paper presents an end to end - I don't think the paper by itself should be a significant update for real world AI risk, because it's talking about optimal policies, it's talking about full observability, and there are other complications. But if I take some of the work so far that I've done on this, then it seems to me that there's something going on with consequentialism over outcomes, over observations, over state histories, that tends to produce these tendencies.

**Alex Turner:**
But if you zoom out to the agent grading its actions, and not its actions as consequences of things necessarily, then there's no instrumental convergence in that setting, at least not without further assumptions.

**Alex Turner:**
So yeah, I think there's something. Like for example, you have approval based agents that are, you're argmaxing some trained model of a human's approval function for different actions. I think I A), like that approach. I mean, I don't fully endorse it obviously, but there's something I really like about that approach, and B), I think part of why I like it, or one thing I noticed about it, is it doesn't have these incentives. Since you're doing action prediction, you're not reasoning about the whole trajectory and such.

**Daniel Filan:**
Yeah. Why does it though? First of all, I believe you can Google approval-based agents and Paul Christiano [has written some](https://ai-alignment.com/concrete-approval-directed-agents-89e247df7f1b) about it. It's sort of what it sounds like. One thing about that idea is that, it seems like it could be the case that human approval an action is linked to, does it achieve some goals that the human endorses or whatever, and that actually predicting human approval is just like predicting some Q function, some measure of how much it achieves or leads to the achievement of goals that the human actually has. So I'm wondering, to what extent are these actually different, as opposed to just this action approval thing maybe just being normal optimization of utility functions over world states in disguise.

**Alex Turner:**
Yeah. So I think it's not. Another part of the argument that I haven't talked about yet is that if power seeking is not cognitively accessible to the agent. If the agent knows how to make a subset of outcomes happen, but it doesn't really know how to make many power seeking things happen, then of course it's not going to seek power.

**Alex Turner:**
And so, if you have your agent either not want to, or not be able to conceive of power seeking plans, then you're going to tend to be fine, at least from that perspective of will it go to one of these outcomes, like the big power seeking ones or not, and humans don't know how to do that.

**Daniel Filan:**
It seems like many people do in fact seek power.

**Alex Turner:**
Yeah, people want to seek power, and you might predict that an individual person might approve of the AI gaining some power in this situation. And so, maybe the AI ends up accruing a somewhat lopsided amount of power compared to if the human had just done it, because they're kind of amplifying the human by argmaxing, and doing more than the human could have considered.

**Alex Turner:**
But I think it's also the case that, because of the human's inability to first recognize certain power seeking plans as good, but also because of the human's actual alignment with human values, so it's like there's a combination of factors that are going to push against these plans tending to be considered. I also think it is possible to just take an action-based frame on utility maximization and just convert between the two. The way you do it is important.

**Alex Turner:**
So, there's one thing I showed in [a recent blog post](https://www.alignmentforum.org/s/fSMbebQyR4wheRrvk/p/hzeLSQ9nwDkPc4KNt) on the alignment forum, that instrumental convergence in some broad class of environments, if you zoom out to the agent grading action observation histories, like if the utility function is over the whole action observation history, it does this, it sees that, does this, sees that, for like a million time steps. And then in the deterministic case at least, there's no instrumental convergence. There's just as many agents that'll want to choose action A, like going left compared to going right, in every situation.

**Daniel Filan:**
It's almost like the no free lunch theorem there, right? Like in general, if all possible utilities over action observation histories are equally possible then yeah, any action is compatible with half of the space.

**Alex Turner:**
Yeah. This is why certain coherence arguments about these histories. It doesn't really tell us much, because any history is compatible with any goal. Oh sorry, not any history is compatible with any goal, but you can rationalize any behavior as coherent with respect to some ridiculously expressive objective over the whole action observation history.

**Alex Turner:**
But with these histories, sure, there are these relatively low-dimensional subsets where you're only grading the observations. And then in that subset, you're going to get really strong instrumental convergence. So, it really matters how you take your subsets, or the interface through which you specify the objective. And so, I don't think I've given a full case for why approval directed agency should not, at optimum at least, go and fall into these pitfalls. But I think that some of these considerations bear on it.

**Daniel Filan:**
Okay. So one other thing that I wanted to talk about is: part of the motivation is that you're worried that somehow the amount of power that an agent you built has is maybe going to trade off against the amount of power that you have. But power, if you think about it, it's not always a zero sum thing. For instance, maybe I invent the computer, and I make more and sell them to you. And now I have the ability to do a greater variety of stuff. And so do you, because we have this new tool available to us. So I'm wondering how you think about the kind of multiplayer game where there's a human and the AI, and how we should think of power in that setting, and what's going to be true about it.

**Alex Turner:**
Right. This is a good question. I've supervised some preliminary work on this question through the [Stanford Existential Risks Initiative](https://cisac.fsi.stanford.edu/stanford-existential-risks-initiative/content/stanford-existential-risks-initiative). And I was working with Jacob Stavrianos, and we got some preliminary results for this so called normal form constant-sum case, where everyone's utility has to add up to some constant. So if I gain some, then you're necessarily losing some.

**Alex Turner:**
And what we showed was under a reasonable extension of this formalization of power to the multi-agent, multi-player setting was that, if the players are [Nash](https://en.wikipedia.org/wiki/Nash_equilibrium), then their values basically have to add up to the constant, which is pretty well known. But if they're not, then that means that there's extra power to go around, kind of. If you imagine we're playing chess and we both suck. And so we both have the power to just win the game with probability one by just playing optimally, so we both-

**Daniel Filan:**
Given the other player's policy, right?

**Alex Turner:**
Yeah, given other player's policy, so we both have the power to win the game. But if we're already in Nash, then we're both already playing as well as possible. And so there's no extra power to go around. So as the players get smarter and improve their strategies, the sum power is going to decrease. And so we had: the power is greater than, or equal to the constant, with equality if and only if they're in Nash.

**Daniel Filan:**
Yeah. That's kind of a weird result, right?

**Alex Turner:**
How so?

**Daniel Filan:**
Everyone's really powerful as long as everyone's really bad at stuff.

**Alex Turner:**
Yeah.

**Daniel Filan:**
Yeah, maybe because it's related to the zero sum setting, but why do some people intuitively not worry about AI? I think a big motivation is like, "Look, you create smart agents in the environment. That's kind of like creating other humans. The world isn't zero sum; people can do well for themselves by inventing useful things that help everyone else. And maybe AI will be like that." I mean, that's not the full details of an argument, but it seems like it's really closely related to this non-zero sum nature of the world. And similarly, if I imagine cases where the AI can increase its power and also my power, it seems closely related not to, we can both increase our power because we both had a lot of power because we were both sucking, but more like, "yeah, we're gaining more ways to manipulate the environment." And then we're in PvE (player versus environment), to use gaming terminology, instead of playing a PvP (player versus player) game. Do you have any thoughts on that?

**Alex Turner:**
So, yeah. Well, formally we don't... I still don't understand what the non-zero sum case is about. So, I don't have a formal answer to that, but I would expect the world to be more like, "Look, it's already really well optimized." Power, because it's instrumentally convergent, is something that people will compete over. And I'd imagine it's reasonably efficient. I'd be surprised if there's just an easy way to rule the world, not easy in an absolute sense, but easy to a human. There's not going to be a weird trick where I can become president in a week or something, because if there were, people care about that already. Whereas if there were a weird trick for me to improve my alignment research output, then it's still probably not super plausible, but it's way more plausible than weird ways to gain power in general, because not that many people care about gaining alignment research output that much.

**Daniel Filan:**
Yeah. And they don't all want to steal your ability to do alignment research.

**Alex Turner:**
So yeah, I would expect that the AI is trying to gain power, it's in a non-zero sum but reasonably competitive system. And so, I think a lot of the straightforward ways of gaining power are going to look like taking power from people. And also, if your goal is spatially regular over the universe, then even though it could share with you, it's still going to be better for it to eventually not share with you. It might share with you instrumentally, but there's an earlier paper by Nate Soares that-

**Daniel Filan:**
And I think Tsvi Benson-Tilsen as well.

**Alex Turner:**
Yes. Yes.

**Daniel Filan:**
That's the one you're thinking of?

**Alex Turner:**
[Formalizing Convergent Instrumental Sub-Goals](https://intelligence.org/files/FormalizingConvergentGoals.pdf), I believe. And it approaches from the perspective of an agent that has a utility function that is additive over parts of the universe, and it's maximizing this utility function. And so, it's got different resources it can move around between different sectors. And so, even if it doesn't care about what's going on in a sector, it's still going to care about using the resources from the sector for other parts of its goal. I'm not necessarily saying we're going to have an expected utility maximizer with a spatially regular goal over the whole universe state or something. But I think that that kind of argument will probably apply.

**Daniel Filan:**
Yeah. To me, intuitively, it seems like probably the way the analysis would go is that you have non-zero sum phases and zero sum phases. Where you increase your power by inventing cool technology and vaccines, and doing generally useful stuff. And then once you're on the [Pareto frontier](https://www.sciencedirect.com/topics/engineering/pareto-frontier) of how much power you can have and how much power I can have, we fight over where on the frontier we end up on. I guess I'd kind of be interested to see real formal results on that.

**Alex Turner:**
I would too. One more way this can happen is if you have a bunch of transformative AIs and they're all, let's say they're reasoning about the world. And we could say, even if individually, they would take a plan that Pareto improves everyone's power, or just makes everyone better off in terms of how much control they have, they might have uncertainty about what the other agents will do, and so they might get into some nasty dynamics where they're saying, "well, I basically don't trust these other agents, so even though I might prefer, all else equal, taking this 'everybody wins' plan, I don't know what other agents are going to do. They might be unaligned with me, or whatever. So, I'd prefer gaining power destructively, to letting these other agents win." And so then they all gain power destructively. I think that's another basic model where this can happen.

## Future work and about Alex <a name="wrapping-up"></a>

**Daniel Filan:**
Wrapping up a bit, if I think about this broad research vision that encompasses both AUP and this power seeking paper, what follow-up work are you most interested in, what extensions seem most valuable to you?

**Alex Turner:**
That are extensions of both works?

**Daniel Filan:**
Or just sort of future work in the same area.

**Alex Turner:**
So understanding more realistic goal specification procedures, like what if it's featurized? For symmetry arguments, if you permute a featurized goal, or if you modify it somewhat, then the modification might not be expressible as another featurized objective. The arguments might not go through.

**Daniel Filan:**
Although it seems like it's possible that in the featurized case, it's more realistic to expect the environment to have built in symmetries of, flipping feature one doesn't change like the available things for feature three. I don't know if that actually pans out.

**Alex Turner:**
Yeah. I think often that'll pan out, but it could be the case that in some weird worlds it could be true. And so I basically haven't thought about that. There are a bunch of things that I've been thinking about, and that has not really moved up to the top. I also would be excited to see AUP applied to embodied tasks, or maybe not embodied, but at least simulated. Where you have an agent moving around in some 3D environment, and you're able to learn value estimates pretty well. If the agent can learn value estimates well, then it should be able to do AUP well.

**Alex Turner:**
Also I want to have results on when the agent is uncertain about its environment, or it's managing some kind of uncertainty. It seems like at least at one point in time, Vanessa Kosoy mentioned that a large part of how they think about power seeking is as robustness against uncertainty. If you're either not sure what your goal should be, this is kind of power, as I formalized it. Or if you're not sure like how things could fail, then if you have a lot of power, you have a lot of slack, a lot of resources, I'm not going to die if I wake up too sick to work tomorrow, for example. I have some measure of power, even though I'd want to be robust against that uncertainty. If that weren't the case, I'd take actions right now.

**Alex Turner:**
And so another source of power seeking could be uncertainty, either about objective, in which case it'd be normative uncertainty, or uncertainty about the environment that it's in, or some other kind. And so I think there's probably good results to be had there. In particular, one legible formal problem is extending the MDP results, the Markov decision results, to partially observable Markov decision processes, where the agent doesn't see the whole world all at once.

**Alex Turner:**
We already have results for these more general environments, which don't have to be fully observable, but I still want to understand more about how the structure of the world, and the structure we assume over the agent's objective, will affect the strength and kinds of instrumental convergence we observed.

**Alex Turner:**
Then on the AUP front, I'd be excited to see AUP applied to at least a simulated 3D environment, perhaps partially observable, basically an environment where current reinforcement learning techniques can already learn good value function networks. Then AUP should be able to do decently well here, and if not that'd be important to learn for more practical applications.

**Daniel Filan:**
So, the point here is just extending AUP to closer to the cutting edge of reinforcement learning. Is that right?

**Alex Turner:**
Yeah, and if it works, I think it'd be a good demo. It'd be viscerally impressive in a sense that large 2D worlds are not.

**Daniel Filan:**
Yeah. So one thing that strikes me, it seems like some of the classic cases of power seeking or whatever are in cases where the agent has bounded cognition, and wants to expand those bounds. So like, there's this famous, I think, I don't know if it was Marvin Minsky, but there's some discussion of, "Look, if your goal is to calculate as many digits of pi as possible, you need a ton of computers". And of course the optimal policy is to just write down all of the correct digits. But somehow, optimality is, in this case, hiding the key role of instrumental convergence, or of resource gathering. I'm wondering if you have any thoughts about this kind of bounded optimality case.

**Alex Turner:**
So I don't think this is all bounded optimality. I don't think perfect optimality is itself to blame here. So as I alluded to earlier, there's going to be some results that show under a wide range of decision-making procedures, Boltzmann rational satisficing, and so on. You're still going to get these tendencies of similar strengths. So, you're moving away from optimality, you're letting the agent consider relatively small sets of plans, and you still might observe it. I think there's something with the agent: our formalisms not dealing with the agent thinking about its own thinking, thinking about, "If I got more computers, I'd have more abilities" and such. At least with that particular example. Separately, I do think that power as the agent's average optimal value, does hide more realistic nuances of what we think of as power. Where I think it's wrong to say, for example, that you have the power to win the lottery. All you have to do is just go get a ticket with the right number. There's a policy that does it. And so yeah, optimality is a problem in that sense as well. And I do have some formalisms or relaxations of power that I think deal with it better. But with respect to the compute example, I think that's partially an embedded agency issue.

**Daniel Filan:**
Okay. So yeah, zooming out a little bit more, suppose somebody really wants to understand what the Alex Turner agenda is, or what do Alex Turner's research tastes look like? How do you get research done? Can you tell us a little bit about that? What does it look like for you to do alignment research?

**Alex Turner:**
Before I do that, I will note I've [written](https://www.alignmentforum.org/posts/JcpwEKbmNHdwhpq5n/problem-relaxation-as-a-tactic) [several](https://www.alignmentforum.org/posts/e3Db4w52hz3NSyYqt/how-i-do-research) [posts](https://www.lesswrong.com/posts/Lotih2o2pkR2aeusW/math-that-clicks-look-for-two-way-correspondences) on the Alignment Forum about this. What does Alex Turner research look like? One of my defining characteristics is, I think I really like small examples. I mean, this isn't unique to me, but especially compared to people outside the alignment community, so my colleagues at Oregon State, I think I have more of an instinct of noticing philosophical confusion about a concept, or noticing that an important argument is not sufficiently detailed. Or finding that it's kind of unfortunate that an argument holds for AI risk, and wondering how we could get around it? Like, how can I drive a car through this argument, basically? How can I avoid one of the conditions? And so, there's some amount of acquired research taste I have at this point, that tells me what I'd like to double click on or to zoom in on. But once I'm doing that, I'm trying to find the minimal working example of my confusion, the most trivial example, where I'm still confused about some aspect of the thing. It might be, "Well, what's power?" Or, "Well, what does instrumental convergence mean?"

**Alex Turner:**
There was a point in time when I was walking around and I thought maybe there's no deep explanation, there are just a bunch of factors that play into it, a lot of empirical facts. And maybe there's no clean explanation. But this didn't really feel right to me. And so, I kept looking, and I kept writing down lots of small examples, and I'd get one piece of the puzzle and I'd have a checklist of, "Well, now that I understand, 'Hey, the discount rate or the agent's time preferences are going to really matter here. And the agent's optimal policies will change with the discount rate. Its incentives will change with the discount rate.' What more can I say?"

**Alex Turner:**
And then I'll have a list of problems I've been thinking about, and I'll see if I can tackle any of those now. And sometimes I can't. And I'll go back and forth, and hop back over maybe to AUP, to more of my conventional thesis work that I'm doing for my thesis, and I'll start thinking about that. And then there's a period of about a year where every three months or so I try and come back and generalize the results so that they'd talk about, not just some very neutral, specific cases, but some more general cases, and I couldn't make headway.

**Alex Turner:**
And so I came back like three times, and finally I was able to have the insight in the proper form. And so it's A) a somewhat acquired taste on what's promising to zoom in on, what's going to actually matter, and B) working with small examples, and C) keeping a list of things that I'll keep trying to attack, and see if I can make headway on those. I'd say that those are three salient aspects of how I do research.

**Alex Turner:**
I think one trait I have that has served me very well is just an assumption that not everything has been thought through and found, especially given the relatively small size of the alignment community. There's a lot of smart people, but there's not a lot of people. And so I'll just kind of naively, in some sense, look out at the world and just see what comes to mind to me. And I'll just attack the problem, and it won't necessarily be a question of, "Well, I should thoroughly examine the literature and make sure there's a hole. And maybe someone actually came up with some really good formalization of power or instrumental convergence somewhere." It doesn't even enter my mind. At first I'm just not at all reluctant to think from first principles and not have any expectation that I will be repeating thoughts that other people have had, because usually that hasn't been the case. And I think that that probably won't be the case in alignment for at least several more years.

**Daniel Filan:**
Okay. You've sort of answered this question already, but just to sort of bring it in one place, what do you see as the Alex Turner agenda? What are you broadly trying to do in your research, and what do you want out of your future research?

**Alex Turner:**
So right now, and into the near future, I want to be able to lay out a detailed story for how expected utility maximization is just a bad idea on an AGI scale. Not because this is something I need to persuade the community of, but to A) understand ways we can make the argument fail for other approaches, and B) there's some persuasion value I think, to writing papers that get into more normal conferences, and C) I think there's been a significant amount of deconfusion for me along the way that's tied in past arguments about coherence, and past arguments about when should we expect power seeking to occur? And there's some modifications to that.

**Alex Turner:**
I think there's a range of benefits, but mostly, the main benefit is something like, if you are really confident that something is going to fail horribly, and you want to build something that doesn't fail horribly, then it would be a good idea to understand exactly why it would tend to fail horribly. And I don't think I've established the full argument for it actually failing horribly, but I think the work has contributed to that.

**Daniel Filan:**
Okay. So if I think about that agenda, what other types of research do you think would combine nicely with your agenda? You do your thing, this other agenda happens and then bam, things are amazing forever. Does anything come to mind?

**Alex Turner:**
Amazing forever. High bar.

**Daniel Filan:**
Maybe just quite good for awhile.

**Alex Turner:**
At some point this agenda has to confront what abstractions do you make goals with, or do you specify goals with? What are the the featurizations like? How should we expect agents to think? What are their ontologies? What are their learned world models? How are they going to abstract different things and take statistics of the environment in some sense? So I think [John Wentworth's agenda](https://www.alignmentforum.org/posts/cy3BhHrGinZCp3LXE/testing-the-natural-abstraction-hypothesis-project-intro) will probably collide with this one at some point.

**Alex Turner:**
I think this also can maybe help make arguments about the alignment properties of different training procedures. So if you could argue that you're going to be producing models that somewhat resemble draws from program space, according to some prior, and then if you can say something about what that prior looks like using these theorems, that can help make a bridge where this training process is approximating some more well understood program sampling process. And then you apply theorems to say, "Well, but actually this process is going to have maybe these kinds of malign reasoners in it, with at least this probability." And so, I think that this can help with arguments like that as well, but that's less of a clear research agenda and more of just an intersection.

**Daniel Filan:**
The penultimate question I have is, what questions should I have asked that I didn't actually get to?

**Alex Turner:**
There's the question of what's a steelman of the case against power seeking. Like basically, how could Yann LeCun and others basically broadly end up being right or something? In arguing things like, "Well, they're not going to have evolutionary pressure under some designs to stay alive and such. And so therefore, these incentives won't arise, or need not arise, if we don't hard code them in." And maybe, I don't think this argument is correct, but you could say something like, maybe we end up getting pretty general RL agents through some kind of multi-agent task, where they need to learn to complete a range of tasks. And they're cooperating with each other. And maybe they learn some kind of really strong cooperation drive. And also maybe their goals don't actually generalize, they're not spatially regular across the universe or anything; they're fairly narrow and kind of more reflex agent-y, but in a very sophisticated way. They're reflex agent-y, or they're responding to the world and optimizing, kind of like a control system, like how a thermostat doesn't optimize the world to most be at the given control point. And so, maybe that's one story of how objective generalization ends up panning out, where they basically learn cooperation instincts as a goal, and then things end up working fine. I don't find it super convincing, but I think it's better.

**Daniel Filan:**
So the final question, if people listen to this and they're really interested in you and your work, how could they learn more or follow you?

**Alex Turner:**
So, first I maintain a [blog](https://www.alignmentforum.org/users/turntrout) on the AI alignment forum. My username is my name, Alex Turner, and there'll be a [link](https://www.alignmentforum.org/users/turntrout) in the description. And also [my Google Scholar](https://scholar.google.com/citations?user=thAHiVcAAAAJ&hl=en&oi=ao), Alexander Matt Turner, you can stay abreast of whatever papers I'm putting up there. And if you want to reach out, my Alignment Forum account bio has my email as well.

**Daniel Filan:**
All right. Well, thanks for joining me today. It was a good conversation.

**Alex Turner:**
Yeah. Thanks so much for having me.

**Daniel Filan:**
This episode is edited by Finan Adamson, and Justis Mills helped with transcription. The financial costs of making this episode are covered by a grant from the Long Term Future Fund. To read a transcript of this episode, or to learn how to support the podcast, you can visit axrp.net - that's A X R P dot net. Finally, if you have any feedback about this podcast, you can email me at feedback@axrp.net.
