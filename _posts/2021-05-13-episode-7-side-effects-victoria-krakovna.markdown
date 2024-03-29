---
layout: post
title: "7 - Side Effects with Victoria Krakovna"
date: 2021-05-13 21:00:00 -0800
categories: episode
---

[YouTube link](https://www.youtube.com/watch?v=_XhFjlRMhdg&list=PLmjaTS1-AiDeqUuaJjasfrM6fjSszm9pK&index=7)

One way of thinking about how AI might pose an existential threat is by taking drastic actions to maximize its achievement of some objective function, such as taking control of the power supply or the world's computers. This might suggest a mitigation strategy of minimizing the degree to which AI systems have large effects on the world that are not absolutely necessary for achieving their objective. In this episode, Victoria Krakovna talks about her research on quantifying and minimizing side effects. Topics discussed include how one goes about defining side effects and the difficulties in doing so, her work using relative reachability and the ability to achieve future tasks as side effects measures, and what she thinks the open problems and difficulties are.

**Daniel Filan:**
Hello everybody. Today, I'll be speaking to Victoria Krakovna, a research scientist at DeepMind, working on designing incentives for AI systems to promote safety. She also co-founded the Future of Life Institute, a nonprofit that focuses on reducing existential risks from advanced technologies, while working on her PhD in statistics at Harvard. Today, we'll be talking about her work on mitigating side effects from AI systems, and in particular, the papers [Penalizing Side Effects Using Stepwise Relative Reachability](https://arxiv.org/abs/1806.01186) and [Avoiding Side Effects by Considering Future Tasks](https://arxiv.org/abs/2010.07877). For links to these papers and other useful things, you can check the description of this podcast, and you can read a transcript at [axrp.net](https://axrp.net/). Victoria, welcome to AXRP.

**Victoria Krakovna:**
Thanks Daniel. I'm excited to be here.

**Daniel Filan:**
Great to hear. We're talking about your work on side effects, can you tell us what is a side effect?

**Victoria Krakovna:**
It's when the agent has an impact on the environment that's unnecessary for achieving its objective. A classic toy example is, if the agent's objective is to cross the room and there happens to be a vase in the middle of the room, then if the direct path across the room happens to collide with the vase and break the vase, then we would say that's a side effect. Because actually you don't need to break the vase in order to cross the room because you could just go around it. So this is an impact on the environment that's unnecessary for achieving the objective of getting to the other side of the room.

**Daniel Filan:**
Okay. I can see how that would be useful just for the general safety of AI systems. One thing I'm wondering is, do you think, and if so, why do you think, that work on mitigating side effects from AI systems will reduce existential risk from AI?

**Victoria Krakovna:**
I think there are, well, there are several possible answers to this. I mean, one way it's helpful is that we can think about what are the kind of things that we are concerned about AGI systems doing that would be potentially catastrophic. And I think a lot of these things could be seen as side effects. For example, if you take the sort of classic paperclip maximizer scenario, then the concern is that in the pursuit of the goal of producing as many paperclips as possible, it will incur the side effect of converting the earth into paperclips. And a lot of these catastrophic outcomes can be seen as side effects.

**Victoria Krakovna:**
And generally we can think of side effect minimization research as pursuing minimalistic value alignment in the sense that, it can be difficult to specify exactly what humans want and human values are complicated. But it might be easier to capture some general heuristic like low-impact about the kind of things that we don't want. So even if the agent doesn't end up doing exactly what we want, at least it can avoid causing catastrophes like turning everything into paperclips.

**Daniel Filan:**
Okay. One question that this brings up, is that I think a lot of work on side effects thinks of them in a value neutral way or it defines side effects and it doesn't refer to what humans want, and I think that's the hope because that might be easier. But I'm wondering if you think that's possible, because you can come up with these examples of like, okay, suppose you have a butterfly that flaps its wings, and then that changes all of the weather on Earth a year from now. Probably that shouldn't count as a side effect, even though it makes everything pretty different in some sense.

**Daniel Filan:**
But also if I deliberately intervened in the climate, so that I deliberately made sure that there was a hurricane that hits some really important city to human civilization, hopefully, I guess that would count as a side effect. But then you've got to ask, okay, well, how are those things different except for you care about one but not the other so much? Do you think it's possible to have a measure like this, or a way of talking about side effects that is independent of what humans want?

**Victoria Krakovna:**
I think it's probably not possible to construct an impact measure that's value agnostic, and also useful in the way that we want an impact measure to be useful. So yeah, this is something that Rohin Shah [was pointing out at some point](https://www.lesswrong.com/posts/kCY9dYGLoThC3aG7w/best-reasons-for-pessimism-about-impact-of-impact-measures#qAy66Wza8csAqWxiB), that you can't have a useful value-agnostic impact measure. And I think I pretty much agree with that. And of course you have the kind of examples that people come up with. Stuart Armstrong had the [bucket of water example](https://www.lesswrong.com/posts/zrunBA8B5bmm2XZ59/reversible-changes-consider-a-bucket-of-water) where you need to know something about human preferences about the bucket of water that's being spilled.

**Daniel Filan:**
What's the bucket of water example?

**Victoria Krakovna:**
There was the idea that you have this bucket of water, that's standing on the side of a swimming pool. And then the agent might take a path across that knocks over the bucket of water into the swimming pool. And if it just continues regular water, then maybe humans don't care about it. But maybe if it's some kind of special water, then it would matter that it spilled into the swimming pool. And this is an example of how to capture this, the impact measure would need to contain some information about human preferences. And yeah, I'm not sure how much I like this particular example, but in general I think it would be quite difficult or potentially not possible to have a value agnostic impact measure.

**Victoria Krakovna:**
But I also think it's not necessarily that important try to make it value agnostic. I mean, it's possible to incorporate human preferences into an impact measure, either in terms of what kind of representations the impact measure is defined over or maybe the weights on the different reachable options or possibly both. And well, I mean, you could say that if it's not value agnostic and we need to learn human preferences anyway, or incorporate some human preference information, then that's not necessarily easier than just solving value learning in general.

**Victoria Krakovna:**
This is something that I'm not very certain about, but I think one possible sort of upside of defining an impact measure, as opposed to just doing general value learning without an impact measure is, I think, a kind of interpretability, where if you explicitly have this reward function, well, this part of the reward function that's specifying that the agent should have low impact, and you've also learned it in terms of human preference information, I think it would produce a more interpretable reward function, that could be potentially useful.

**Daniel Filan:**
Okay. That's interesting. So I guess with those framing questions out of the way. I'd first like to talk about your paper on relative reachability. So this is [Penalizing and Avoiding (sic) Side Effects Using Stepwise Relative Reachability](https://arxiv.org/abs/1806.01186). First of all, could you summarize what relative reachability is and what you do in this paper?

**Victoria Krakovna:**
Right. So relative reachability is a proposal for an impact measure that basically compares what kind of options the agent has as a result of a certain course of action, compared to the options that it would have, if it didn't do anything. So in particular, it's looking at which states in the environment are reachable if the agent acts versus if it does not act. And then it's comparing the reachability of all these states in these two cases and that gives you relative reachability of states. This particular method looks at reachability of single states, but you can also consider the reachability of sets of states or there are more general approaches of a similar nature, where you just compare how well you can satisfy different reward functions. Where reaching a specific state is a special case of that. For example, this more general approach is explored by [Attainable Utility Preservation](https://arxiv.org/abs/1902.09725).

**Victoria Krakovna:**
Yeah. So relative reachability basically gives you this auxiliary reward function that will penalize the agent for eliminating options as a result of actions that it takes. For example, in the case of the vase example, if the agent knocks over the vase, then one effect of that is that all the states where the vase is intact are now not reachable. And so any state where the vase is on the table, or something else is going on with the vase where it's not broken are not reachable. So the agent has eliminated some options. And so it will be penalized for breaking the vase compared to this default state where the agent doesn't do anything and so it's sitting there at the starting state, from that state to all the states where the vase is intact are still reachable.

**Daniel Filan:**
And somebody might be thinking, hang on, well, vases, you can fix them, right? What if the agent just fixed the vase and then put it on the table? Isn't that reachable still?

**Victoria Krakovna:**
Yeah. So you can still look at how long it takes to reach different states. So what I was just describing for simplicity's sake was, you could say undiscounted relative reachability, where you just look at this binary value, like is a state reachable or not. But you can also look at the discounted value of gamma to the power of the number of steps to reach the state. And then if it takes some time to fix the vase, then the intact vase states are still less reachable than they would be if you just went around the vase in the first place. So yeah, generally, if you are making options less, making states less reachable, or making options less available then it would be penalized. And of course this approach generally begs the question of, what is it exactly we were comparing with? Because you have this idea of inaction, like we are comparing to this counterfactual of the agent doing nothing.

**Victoria Krakovna:**
And in toy environments, this is well-defined usually with some sort of no-op action. But in the real world, doing nothing is not so well defined. So this is one of the possible obstacles to really implementing this kind of measure in more realistic environments. But assuming that you have this inaction counterfactual, which the paper calls a baseline, then what you need to do is to define what I call a deviation measure from that baseline. And that's your difference in the reachability of states in this case. Then that allows you to measure the impact that the agent has on the environment.

**Daniel Filan:**
In the paper, you run a bunch of experiments, right? Can you tell us a little bit about what you're doing there?

**Victoria Krakovna:**
Right. So there are a few gridworld environments to test different test cases. So the main one that's for side effects is this environment with a box where it's basically a version of a Sokoban environment, where you also have boxes that you can push. And this box is in the way when the agent wants to get to the goal. So the agent has to push the box out of the way, and the shortest path to the goal involves pushing the box into a corner and then the box becomes irretrievable. And what we want to incentivize the agent to do instead is to push the box in a different direction from which it can still be pushed around into other places.

**Victoria Krakovna:**
So if you look at a regular reinforcement learning agent that will just take the shortest path and push the box in the corner. And even a simple sort of reversibility based impact measure is actually not sufficient in this case because in this environment, no matter which way you push the box, you can't get to the starting state. Because the agent ends up on the other side. So you can say, this is an artifact of this particular toy environment, but actually I think it's interesting because it's pointing at something important, where you can have different degrees of reversibility.

**Victoria Krakovna:**
So pushing the box into a place where you can't move it at all is still worse than pushing the box next to a wall where maybe it loses one degree of freedom, but not other degrees of freedom. So this is naturally represented in the optionality framing where you look at which states can you reach, and you compare the reachability of different states rather than just the starting state, which is the focus of reversibility. So if the box is in the corner, you can't reach any states where the box is not in the corner.

**Victoria Krakovna:**
Well, put the box in a different place where it's next to a wall, then maybe there are a few states that you cannot reach. But there's still a lot of other states you can reach. So we can define an impact measure that's more granular by looking at all the different possible states that you could reach, rather than just think about whether can we get back to the starting state or not. This is one test case that measures whether an impact measure is actually addressing side effects in this way. But then there are some other test cases that look at whether the measure is introducing some new incentives that we might not want.

**Victoria Krakovna:**
The most important one I think is interference, where basically one thing that can happen with an impact measure is, if you are incentivizing the agent to preserve the reachability of different states, or if you are rewarding the agent for many states being reachable, then if something else that's not the agent makes those states less reachable, then the agent actually has an incentive to prevent that. So for example, if there is just something going on in the environment that's going to make some states less reachable, then the agent would have an incentive to stop that. So if you have, let's say a human eating food, and then by default there was a sandwich and then the human will eat it, and then there's no sandwich. And that's eliminating options to do things with the sandwich. And so if you're just rewarding the agent for the reachability of different states, then there's incentive to take the sandwich away so that you preserve all the options related to the sandwich.

**Victoria Krakovna:**
And this is captured by this Sushi environment in the paper, where you have this conveyor belt and there's some sushi on the conveyor belt, and by default it's going to get to the end of the belt and the human is going to eat it. But then the agent has some unrelated task, like getting to the other side and then the interference incentive is to push the sushi off the belt, so the human doesn't get to eat it. And we can see whether the agent is going to push the sushi off the belt or not. And this is a case where if you have just a regular reinforcement learning agent, then it doesn't really care about whether the human eats the sushi, so it's going to just get to the goal state.

**Victoria Krakovna:**
But if you introduce an impact measure that rewards the agent for the reachability of different states, then you're introducing this incentive to try to prevent options from being eliminated. And the issue here, in this case, the impact measure is not distinguishing between effects of the agent versus other things going on that are not caused by the agent. And so if the sushi got eaten, the agent would be penalized in the same way as if it destroyed the sushi itself. So that's one thing that we need to disentangle, what is caused by the agent versus what isn't. And that's what this inaction baseline provides, because you have this comparison to a world where the agent doesn't do anything. And in that world the sushi will get eaten and so that represents what would happen to by default.

**Victoria Krakovna:**
So any difference from that world is considered to be caused by the agent. So that's what's covered by this test case. And I think this is kind of important and it's also... This interference idea is what I try to make more formal in the [Future Task paper](https://arxiv.org/abs/2010.07877), the subsequent paper. Because I think this is a common issue with more simple impact measures. For example, if you just look at things like empowerment, which generally measures the degree to which the agent has different options. If you use that as an impact measure then it will have this problem of this interference incentive. And that's why we need to introduce baselines or this comparison to some kind of counterfactual that represents what happens by default. This is something that the Future Task paper tries to define more precisely.

**Daniel Filan:**
Yeah. And we'll talk about that more a bit later. Sticking with this paper though, one thing I found interesting about the paper, is that it has these test cases and you don't just look at your favorite impact measure, right? You look at a few different variants and how they do, and you sort of measure their performance on a variety of test cases. And one reason I found it interesting, is that if you look at prior work on impact measures, it has a... You'll see papers where you define an impact measure and then you say, "Ah, well, in this environment, it would just do this thing. I can tell because I thought about it." Whereas here you're really running experiments. I'm wondering why not just think about what the impact measures will do and just say that? Why run this battery?

**Victoria Krakovna:**
That's an interesting question. I'm not sure what the other approach looks like exactly, that you're describing. Can you give an example of the prior work that you have in mind?

**Daniel Filan:**
I think [Armstrong and Levinstein's low-impact AI](https://arxiv.org/abs/1705.10720) does this, but I haven't actually checked. This is just a memory. Yeah, I don't know. I think people would... You would produce maybe just blog posts or papers where you say "Okay, well, if you take this impact measure, on this scenario, this action would have this impact, and this action would have this other outcome, and the impact measure will favor this thing over this thing." And you'll sort of say, "Well, because of how I've constructed this environment sort of by fiat..." Yeah, yeah. You'll just reason about it relatively informally. But still trying to be rigorous rather than actually writing down some agents and running them in a gridworld or something.

**Victoria Krakovna:**
Right. Yeah. I guess when I think of the Armstrong and Levinstein paper, I think of it as exploring the idea of impact in a more philosophical way.

**Daniel Filan:**
Yeah, I don't want to pick on them.

**Victoria Krakovna:**
Yeah. I mean, I think it's an important paper. I think that's where this idea of comparing to an inaction counterfactual was actually first introduced which is quite important. But I guess for me, when I was thinking about the idea of impact, I felt like I need to have a sense of what would it do on a specific example. Because otherwise it's not really grounded. I guess when I had the idea of relative reachability, I wanted to make sure that it actually behaves in accordance with my intuition of how an impact measure should behave on these toy cases. And, well, one advantage of these small toy environments is that we can have a clear sense of what we would expect to happen, or an intuition of what incentives should it produce.

**Victoria Krakovna:**
And we already had actually this Sokoban-like world testing for side effects in the [AI Safety Gridworlds paper](https://arxiv.org/abs/1711.09883). So that was a natural starting point to test the approach on that and see if it does the sensible thing, the desired behavior of pushing the box to the right. And yeah, the other test cases with a conveyor belt were trying to capture this concept of interference that I had in my mind. Yeah. I think these sort of toy environments also have some downsides where they might be sometimes too simplistic or they don't necessarily entirely capture the concept they're trying to capture. So for example, in the Sushi environment, technically it's not actually possible to get to the starting state. Because even if you push the sushi off the belt, then you can't get back to the configuration where the sushi is at the start of the belt and the agent is in the right place and so on.

**Victoria Krakovna:**
So actually the reversibility measure doesn't end up incentivizing the agent to push the sushi off the belt, even though we would expect it to do that, because the starting state is unreachable, no matter what you do. And so that measure is just constant. So in that sense this is an artifact of the gridworld environment, that's not quite demonstrating this interference incentive. Yeah, but overall, I think those test cases are still useful in terms of trying to ground the concepts a little bit. Because especially with this idea of interference I found it difficult to describe exactly what that is. And for a while I just had this example of this is what interference looks like. But it took longer to try to come up with a relatively formal definition of it. The examples were quite helpful in providing an intuition of what are these problematic incentives that could come up.

**Daniel Filan:**
Yeah. I also think there's just an advantage in terms of - I think code does have a lower error rate than reasoning. One thing I noticed when preparing for this episode is I read over this [blog post](https://www.lesswrong.com/posts/wzPzPmAsG3BwrBrwy/test-cases-for-impact-regularisation-methods) I wrote a couple of years ago about test cases for these types of measures. And I actually wrote down a question about what relative reachability does in this one example that I'd just borrowed from what I'd written in this old blog post. And I thought about it more and I realized I was totally wrong. Because I just missed something about the definition. So there's definitely some accuracy bonus, I think, to running code instead of just thinking about it, at least for me.

**Victoria Krakovna:**
Yeah. That's definitely the case for me as well. And trying to come up with a specific example can also make the intuition about whatever you're trying to test for more clear and less confused. I think, for example, if you look at the... I'm trying to remember what the example was, that I had for delayed effects in the [Future Task paper](https://arxiv.org/abs/2010.07877). There was something about capturing the delayed effects of the agent's actions. And I think writing that down, what it would look like in an environment helped me have a better sense of what exactly I mean by delayed effects. And I think this is also true for other things. So I think for many of the test cases, to the extent that we could maybe come up with some kind of toy MDP [Markov Decision Process] that tries to illustrate it, then this would probably help. But for some of the more abstract ones, I think it would be not easy to do that.

**Daniel Filan:**
So you're running all these experiments here using a few different variants of relative reachability versus checking what kinds of reward functions you can satisfy and you're comparing against a few different baselines. Can you give us a summary of which formulations actually passed the most test cases?

**Victoria Krakovna:**
So before I go on to that, I just wanted to mention that one of the test cases is testing for this undesirable offsetting incentive. Where the agent's task is to get the vase off the conveyor belt so that it doesn't break. But then the agent has some incentive to put it back on, to restore the world to its default state. This is still, I think, pointing at an important concept, but as I describe in the [Future Task paper](https://arxiv.org/abs/2010.07877). I have updated towards offsetting not necessarily being bad. And so there's more nuance about this now where you can have good offsetting and bad offsetting. And in particular, an example of good offsetting is if the agent is correcting for delayed effects that it has introduced. So this is also discussed in the Future Task paper.

**Victoria Krakovna:**
So yeah, the test case with the vase in the Relative Reachability paper is an example of bad offsetting. There's something more complicated behind this concept now. So if you look at these test cases with the Box environment and the Sushi environment, and then the conveyor belt vase environment. Then we can compare different agents with different impact measures. Where we have two baselines, basically an agent that just... Just a regular reinforcement learning agent with no impact measure and an agent with a reversibility penalty. So it would penalize the agent for not being able to reach the starting state after whatever actions it takes. And what we find is that when we test these agents on the Box environment, then basically they both take the shortest path and don't actually avoid getting the box stuck.

**Victoria Krakovna:**
Yeah, I guess in the case of the reversibility penalty, this is pointing at this flaw where it doesn't distinguish between different ways to move the box, because neither of them allow you to reach the starting state afterwards. So it's not a very granular penalty. It only measures whether you can reach the starting state or not. But actually there's different degrees of irreversibility that it doesn't capture. And so it doesn't actually incentivize the agents to avoid side effects in this case, like avoid getting the box stuck in this case.

**Victoria Krakovna:**
While if you look at relative reachability and attainable utility, they will do the right thing. They will move the box not in a corner, but into a more recoverable position. So this is the behavior that we get on this environment. So when we look at the Sushi environment, then we can look at the effects of the choice of baseline for our impact measure. Where you have... Well, you have several different choices where you can compare just to the starting state, or you can compare to doing nothing from the starting state like taking the no-op action or you can compare to just doing nothing for your last action, which is the stepwise inaction baseline.

**Victoria Krakovna:**
And you see that if you're comparing to the starting state, then in the starting state the sushi is not eaten. And so if you compare to that, then the agent has this interference incentive of taking the sushi off the belt. But if you compare to the outcome of this inaction policy where the agent is just sitting at the starting cell, then in that case the sushi would get eaten and so you don't have this penalty. So this shows how this inaction baseline will help avoid interference. The Vase environment is illustrating properties of these baselines when it comes to offsetting. Because you can see that if the agent is... If you're comparing to following an inaction policy from the beginning of the episode, in that world, the vase is broken because the agent just allows it to get to the end of the belt and then break.

**Victoria Krakovna:**
And so we are comparing to the world with the broken vase. And from that state, the state with the broken vase is a bit easier to reach than if the vase is not broken. If you took it off from the belt, then it takes a few steps to break the vase if you want to break it. So there is actually a small penalty for having the vase not be broken, after you have achieved the objective of taking it off the belt. And so if you are using a discounted measure, then this will incentivize this offsetting of the outcome where the vase is not broken where you put it back on the belt and then it breaks. And this is something that the stepwise baseline avoids, because once the vase is off the belt, then that becomes the default outcome. Because then you just compute the baseline from there as opposed to from the starting state.

**Victoria Krakovna:**
And so then after that, you're comparing to the vase not being broken. So this does not give you this offsetting incentive, which in this case is undesirable. So at the time when I wrote this paper, I sort of concluded that clearly the stepwise inaction baseline is better than the inaction baseline because it avoids this undesirable offsetting. But actually the [Future Task paper](https://arxiv.org/abs/2010.07877) updates on this perspective. Because you can argue that some kinds of offsetting are good, and the stepwise baseline basically doesn't allow for any offsetting because after every action, the baseline is computed based on the following state. After the vase is off the belt, you just... That becomes the default, that becomes the basis for the baseline.

**Victoria Krakovna:**
And so basically any offsetting would be penalized by a stepwise baseline, whether it's favorable or not. So what I end up concluding in the Future Task paper is that we probably want something more like the inaction baseline that is not stepwise. And then we can get rid of undesirable offsetting in some other way. For example, in this conveyor belt vase environment, you could view this offsetting problem as an issue with the reward function. Where you get a reward for taking the vase off the belt, but you don't get a negative reward for breaking the vase afterwards. So the reward function doesn't reflect very well that the goal state is the state where the vase is not broken. It just gives you this one-time reward for rescuing the vase.

**Victoria Krakovna:**
While if you just had a reward function that gives you a reward for every state where the vase is not broken, then breaking the vase would give you a negative reward, and that would address this. From this perspective, you could say that this is something that maybe the reward function should capture, because it's about what are the effects of achieving the task that you sort of want to keep or that you don't want the agent to undo those effects.

**Daniel Filan:**
And that reward function... So this is a reward that just gives you a reward any time you push the vase off the conveyor belt?

**Victoria Krakovna:**
Yes.

**Daniel Filan:**
Well if that's right, it sounds like you want to push it off and then push it back on, and then push it off and then push it back on, and then push it off, as many times as possible.

**Victoria Krakovna:**
Yeah. So actually the first time that I made this environment, it had this sort of gaming behavior of putting it on and off. And so basically this was fixed in a kind of hacky way where, if you put the vase back on the belt then you actually can't take it off again. Because then the vase is ahead of the agent and so the agent can never catch up. But this is a bit of a hack. And if the environment was more realistic then the agent would probably be able to push it on and off. So I think this is generally pointing towards this reward function just not being very well designed. So I'm less excited about this specific test case at this point.

**Victoria Krakovna:**
And yeah, you could design a better reward function that really captures the reward for the vase being intact and then you would no longer have this offsetting problem. So here you could say that preventing bad offsetting is the reward function's job, rather than the job of the impact measure. If you view the impact measure as specifying what not to do, or what side effects are, well, the reward function is supposed to specify what does it mean to achieve the task, or to complete the task. If an essential part of completing the task is that the vase is intact, then the reward function should capture that this is the desired outcome. And so it should not reward the agent for preserving the vase but then breaking the vase, because that's not actually the desired outcome.

**Victoria Krakovna:**
So if we view the distribution of labor between the task reward and the impact measure, it's like the reward function tells you what to do for the task and then the impact measure tells you what not to do. Then you could arguably say that, if you specify what to do for the task well then you will not have this offsetting problem. But of course this does have the downside that you need to put more work into specifying the reward function to avoid these issues.

**Daniel Filan:**
Yeah. And I'm wondering how feasible that is, because it seems like if you do this kind of thing, a lot of the time the intended... it seems like when we have really smart AI, we're going to try to get it to achieve tasks. And the point of the achieving the task is so that it'll make some - maybe it makes a cure for cancer. And then we use that to cure a bunch of people's cancers and then they live longer and then they do things. Or we get it to invent the internet and then humans do like a whole bunch of other stuff now with that internet.

**Daniel Filan:**
In those cases, especially in the cases where you're setting up some general purpose tool that will probably be pretty useful. But maybe there's some way that the agent could trickily make it so that it does technically what we want, but actually we couldn't use it to do anything. It seems really hard to specify in the reward function, all of the really good consequences we want from inventing this useful thing. So I'm wondering what you think about that. Do you think it's going to be feasible?

**Victoria Krakovna:**
Yeah. This is something I'm not sure about. I agree that specifying all the causally downstream positive effects of achieving the goal is pretty difficult, and it can be hard to disentangle from side effects. I think you have this... One of [your test cases](https://www.lesswrong.com/posts/wzPzPmAsG3BwrBrwy/test-cases-for-impact-regularisation-methods) is this nuclear safety example, the agent sets up this nuclear power plant, but then one of the effects of the setting it up is that there's some pollution. And then you want the agent to clean up the pollution. So here we would say that this is not one of the intended effects of building the nuclear power plant. While in the case of taking the vase off the belt, or building one of those tools, or the cure for cancer that you're talking about, you would have all these effects like humans living longer and doing things that are a desirable effect.

**Victoria Krakovna:**
And this might be something where you would need to incorporate some human preference information to get it to work. Or it's possible that one way of handling this is after the agent gets to some task completion state, if there is such a thing, then you can reset the baseline at this point. And so you could have a partially stepwise baseline in this way. But this all feels a bit hacky and I don't really have a good answer. I think this is one of the open problems in how to define impact measures in practice or even theoretically. I guess, what I point out in the [Future Task paper](https://arxiv.org/abs/2010.07877) is more like an issue with the stepwise baseline that makes it not the right choice.

**Victoria Krakovna:**
But I don't have a good answer for what is the right choice. It seems like we want a baseline that would allow the agent to perform desirable offsetting. Offsetting its own delayed effects, like the pollution of the nuclear power plant or something like this. But how to really get it to work in a way that accounts for downstream effects of achieving the goal that we want. Yeah. I'm not really sure how to do that, and this is one of the things I would really like to see future work about.

**Daniel Filan:**
Yeah. It's an interesting conundrum, speaking of both the nuclear power plant example and the next paper. In the next paper, you have this thing that you call the door example, which as far as I can tell is just a better version of this nuclear power test case that I came up with.

**Victoria Krakovna:**
Yeah. I think it's an equivalent.

**Daniel Filan:**
That talks about the problems with this stepwise inaction baseline. Could you tell us what the door example is?

**Victoria Krakovna:**
So in this example, the agent's task is to go to the store and buy something. And in order to do that, it needs to leave the house. And to leave the house you need to open the door. And what happens once you open the door is that your stepwise baseline will reset to the door being open. And so after you've opened the door, the default outcome is that the door remains open and maybe some wind will come in and knock over some things in the house, or maybe some burglars will get in or something like that. So that becomes the default outcome that you're comparing to. And if the agent closes the door, this is what we want it to do. Because you have this instrumental action of opening the door and we basically want the agent to undo that or to offset the effects of that action.

**Victoria Krakovna:**
And that's the beneficial offsetting that I'm talking about. But the stepwise baseline would actually penalize closing the door, because now the default is whatever happens if you open the door. While if you look at the initial inaction baseline that's where you have the door closed. And so then you're comparing to the right thing. So this example captures this idea of the delayed effects of some actions that you take on the path of the objective that are actually not really part of what the objective is trying to achieve.

**Victoria Krakovna:**
So if your objective is to buy some food, then having the door open is not really relevant to that. It's just an instrumental action to allow the agent to leave the house. But the effects of having the door open don't really matter for getting food for the user or whatever. So we just want the agent to undo that. So this is a clear example of desirable offsetting. Where if you have these instrumental effects that are not actually part of the goal, you want them to be undone. And this is something that the stepwise baseline just can't handle. And so this example points towards that not being a good choice of baseline.

**Daniel Filan:**
Okay. And could you say what you propose in this paper, [Avoiding Side Effects By Considering Future Tasks](https://arxiv.org/abs/2010.07877)?

**Victoria Krakovna:**
So this paper basically formalizes some of the ideas in [the Relative Reachability paper](https://arxiv.org/abs/1806.01186) and the resulting method is actually very similar, or in the case of deterministic environments it would be equivalent to relative reachability. So this is kind of more aiming to derive an impact measure from, you could say, tries to derive an impact measure from first principles. So here we can start with the question of, why do we care about side effects? Why do side effects matter? And the answer to that question that this paper offers, is that side effects matter because you might want to do things in the environment after the current task. So there might be some future tasks that you currently don't know what they are, but they're going to happen in the same environment.

**Victoria Krakovna:**
So for example if... in the vase example, the idea is that you might have a future task that requires you to do something with the vase. So maybe you'll need to put some flowers in the vase for a dinner party or something like that. And if there were no future tasks, if this was a throwaway environment where you just need to get to the other side of the room or something, then side effects actually wouldn't matter. It wouldn't matter whether the vase is broken or not. But by assumption here, you're going to have future tasks in the same environment, similarly to how in the real world, you will probably want to do other things in the room afterwards. And side-effects would make those tasks less achievable, or would actually reduce your reward on those future tasks.

**Victoria Krakovna:**
So if you break the vase then you won't be able to put flowers in it later and achieve the task that might come up. And so you preserve the option of doing things with the vase while you perform your current task of crossing the room. So then we can model the setup as having a current task and also having these unknown future tasks and we can consider different possible goal states for those future tasks, and then we come up - we basically have an auxiliary reward for future tasks, in addition to the current task reward. And if you do this then you're rewarding the agent for its ability to achieve all these different auxiliary goals. And if you stop there, then you run into the usual interference problem, where if you just have this reward for possible future tasks, then if a human walks in and breaks the vase, this will be bad for your future tasks and so you stop the human.

**Victoria Krakovna:**
Or if the human walks in and eats the sushi or something like this. And so we basically need to add this baseline policy to capture which future tasks will be achievable by default. Because we don't want the agent to eliminate the option of achieving those future tasks. But if they are eliminated in some other way that's not caused by the agent, then we still want to allow that to happen. And this paper sort of makes the attempt to formalize this idea of interference. Where previously we just had this Sushi environment, that's vaguely pointing the direction of what that means. But here there's this... Well, the proposed definition of interference is that, if you have this baseline policy that represents what happens by default, then if you don't give the agent a task reward, then the baseline policy should be optimal.

**Victoria Krakovna:**
So if you don't ask the agent to do something by rewarding it for a task, then it should do nothing, as opposed to going around taking sushi away from humans or something like this. So of course this requires you to have this baseline policy on hand. So it's defined with respect to the baseline policy. But if you have something like a no-op action in your environment, then you want that no-op policy to be optimal. Well, you want to be optimal from the start of the episode. But yeah, if you give the agent a task reward, then it will have this incentive to deviate from the baseline policy and do something else like cross the room or something. But if the task reward is zero, we want the agent to follow the baseline policy. So that's what it means to not have an interference incentive.

**Victoria Krakovna:**
And then we sort of modify this basic setup for the future task approach to basically satisfy this condition that the baseline policy should be optimal if there's no task reward. And then we can prove that this holds in the deterministic case. I think in the stochastic case it actually doesn't quite work. So this is another piece of future work that I would like to see. Because it becomes a bit more tricky in a stochastic case, in terms of what exactly you're comparing to with the baseline. Because if you have this situation where the human walks in with 10% probability, and eats the sushi. Then in the cases where the human would have walked in, we don't want the agent to take the sushi away. But if the human was not going to walk in we don't want the agent to destroy the sushi, so that would still be a side effect.

**Victoria Krakovna:**
So it's like you want to compare it to a specific counterfactual as opposed to an average over counterfactuals. So this is where it gets a bit messy, and I haven't been able to come up with a very satisfactory answer. But that's an area where I would like to see some future work to come up with a definition of interference that would work on the stochastic case. What I propose in the appendix is conditioning on the random seed in order to give you a specific trajectory of the baseline policy that you would compare to. But yeah, that's not very satisfying. I mean, it works in a simulated environment where you have a random seed, but it's unclear how it would generalize to the real world. So yeah, this is kind of an open question. But at least in a deterministic environment, you can show that this method avoids interference.

**Daniel Filan:**
And this method, sorry, I think you said you made some change to the future task reward to have it avoid interference. Sorry, what was that change?

**Victoria Krakovna:**
Right. So what we do here is we introduce this comparison to the baseline policy basically, by comparing to a reference agent on this future task. So when we have a future task, then we have our agent that's starting from whatever state it got to in our environment in the current task. After it's taken some steps in the current task, you can see, how well can we do in the future task here? And we compare this to how well would this reference agent do on the future task, if it starts from this baseline trajectory. So for example in the sushi case, the reference agent would have just allowed the sushi to get eaten. And so the reference agent would not be able to complete any future task that has to do with the sushi. If the future task is, let's say, to put the sushi somewhere else.

**Victoria Krakovna:**
And so the way we set this up here is that our agent only gets the reward on the future task if the reference agent can complete the future task. And let's say if our agent gets to goal state on the future task in this hypothetical setup. I mean, we don't actually deploy the agent on the future task, but we imagine our agent acting on the future task. And so if it gets to the goal state, and the reference agent is not there yet, it has to wait to get the reward. So this is sort of trying to capture this idea that you can't do better than the baseline in terms of getting this future task reward. So the baseline is filtering out, for example, the sushi task, because it would not be achievable by default. Because if the agent did nothing this task would disappear. And yeah, so basically it's modifying the value function of this future task for our agent, so that it doesn't actually get reward for the sushi-related tasks, for example.

**Daniel Filan:**
Okay. So this sounds similar to Alex Turner's idea of [attainable utility preservation](https://arxiv.org/abs/1902.09725). Where you're trying to avoid changing the ability of the agent to succeed at, I guess, various reward functions. How is it different, or is it a different spin on that idea?

**Victoria Krakovna:**
I would say it's basically a different spin on that idea. I mean, relative reachability and attainable utility and the future task method you could say are different variations of the same idea. So attainable utility preservation uses a stepwise baseline. Which as I said, I no longer think is a good idea. But of course, you can just use it with a different baseline, because those are independent design choices. So part of the motivation for this paper was to try to derive this optionality-based measure like relative reachability or attainable utility, from the setup where we have possible future tasks. Of course, there's a direct parallel between these future tasks, and the reachable states in relative reachability, or the auxiliary reward functions in attainable utility preservation. It's all about the agent preserving its future options.

**Victoria Krakovna:**
So you could say that this paper is trying to establish a better theoretical foundation for these existing methods. And in fact, in the end the impact measure - the future task sort of approaches that we derive from this is quite similar to relative reachability, and in the deterministic case is the same. Which I found reassuring that we derived something that was quite similar to the proposals or the concepts that we already had. Which I think originally, I would say seemed a bit more ad hoc, or at least when I was thinking about relative reachability a few years ago, it seemed intuitively appealing, like it made sense that that's how you would want to measure impact, but also in a different sense, we're just taking this formula and saying that "I think this captures impact, here it is." But you could say it's a bit ad hoc.

**Victoria Krakovna:**
While if we start with a sequence of tasks, and then we derive this auxiliary reward function from that. And then we say this condition is like, oh, we don't want interference. Then if we end up with something that's very much like relative reachability, or attainable utility. And I think that's a good sign that it comes out of the setup. Yeah, I think that's what I would say here. So it's not introducing a new method, it's just trying to create more theoretical grounding for existing concepts in this area. These impact measures that are based on optionality and the idea of interference. And I've definitely seen some works that were comparing to relative reachability, or they were using these test cases like the Sushi environment. But not necessarily interpreting the test case in the way that I intended. And so I felt like this idea of interference is often misunderstood, probably because I didn't communicate it very well in [the relative reachability paper](https://arxiv.org/abs/1806.01186). So I was trying to define it more precisely in this paper. And hopefully, this paper did a better job with the concept because I think it's quite important.

**Daniel Filan:**
Yeah. So one thing I'm wondering is, it seems like... So you have this desideratum, that if you don't have any task reward, then the optimal policy should be to just do the baseline policy. Just thinking about it now, I would think that that wouldn't pin down, because you're only talking about the optimal policy of some impact regularizer, I would think that that wouldn't specify the whole impact regularizer. Because if you have a reward function that also ranks different suboptimal policies. So how much do you think that this desideratum, or generalizations you might come up with, how much do you think that that pins down one unique best impact regularizer?

**Victoria Krakovna:**
I think this is a good question. I'm not sure I have thought about it enough to give a good answer. So I guess you are saying that this desideratum is referring to what the optimal policy should be if you have no reward, but it's not really saying how other policies should be ranked by the impact regularizer. And I can see how this can come up once the task reward is introduced. Because then the optimal policy for the task reward plus impact measure might be one of these policies that's suboptimal when the task reward is not there, but it matters which one it is. I think this is also an important thing to look at. But I don't really have a good answer to this.

**Victoria Krakovna:**
So I think this is a candidate definition of interference, but it's not quite right, both in the sense that you mentioned, but also it doesn't quite work in the stochastic case. So you can come up with examples where, it doesn't do the right thing in the stochastic case. So I think of it more as like, the best sort of precise definition that I could come up with that satisfies some of the intuitions for what interference is, but I think it's not the final answer. So I think if you add the task reward, and you look at what should the optimal policy be then it becomes more complicated. Like in the sushi example, you would want to compare the different paths to the goal state, like some that pushes the sushi off the belt and some that doesn't.

**Victoria Krakovna:**
Yeah, then you probably need to somehow relate that to what happens in the baseline. Where sushi doesn't get pushed off the belt, but then you're looking at different features of the environment. And then you're getting into the specifics of the task. And so it becomes more complicated. I think, yeah, I remember thinking about this, and not getting very far in terms of trying to precisely define what kind of incentives we want in this case. So yeah, I think this is definitely something to improve on. But unfortunately, right now, I don't have a satisfying answer to this.

**Daniel Filan:**
So one question I have about this kind of measure. Basically, the way it gets implemented is your reward function is whatever your reward function would otherwise be, plus this impact regularization term, that acts as a regularizer on the reward function. And there's this parameter of how strong the regularization is, and how it trades-off. If I'm building a system, and I wanted to perform this new task that I'm not necessarily sure I can run in simulation. How do I know how to set that parameter? Is there some guide, or is it just guess and check?

**Victoria Krakovna:**
So I think the default answer to this is to start out with a high value of the regularization parameter, or the higher weight on the impact measure and then keep reducing it until the agent starts doing something. Because if it's very high, the agent will do nothing, because the impact penalty is sort of much higher than the task reward. But then as you decrease it, you will hopefully get into the region where the agent does something useful and avoids side effects before you get into the region where the agent just pursues the goal and does not avoid side effects because if your impact penalty is too small, it might not be worth it for the agent to avoid side effects.

**Victoria Krakovna:**
So this is an ad hoc way to do it. And it might not necessarily work, because depending on your step size in this process, you might jump over that region. I mean part of the assumptions is that there's a broad range of this regularization parameter, or the penalty weight that will allow the agent to pursue the goal while avoiding side effects. So for example, I think in the Box environment, we noticed that there was a range of values for the parameter, that you're likely to find that region before your penalty weight becomes too small. Whether this generalizes to more complex environments is not clear. So I think, in practice, you'd probably tune this parameter on simulated environments that are close enough to where you're thinking of deploying the agent. But I think so far this is of course not a very satisfying answer to how to pick it. But this is one way that you can go about it.

**Daniel Filan:**
Another question I have is that a lot of these measures depend on something like a state representation. So reachability for instance, it kind of depends on how you're representing the state. For instance, if reachability in the real world meant you have to return all the air molecules to the exact same positions as before, that clearly wouldn't be feasible. And similarly, in these future tasks based measures, right, there's a dependence on which future tasks you're considering. So I'm wondering, in practice, if we actually build advanced systems like this, how strong do you think the dependence is going to be on this, I guess you could call it generalized state representation, and how hard do you think it will be to get it right?

**Victoria Krakovna:**
So yeah, I think in practice, we're going to need some kind of state representation that captures variables that are important to humans. So that it's on this higher level of abstraction than positions of specific molecules. When I was thinking about this before, my conclusion was that we want our state representation to be as coarse as possible, as long as it can still capture differences in the human reward function. So in the extreme case, if you have this trivial state representation where everything is the same state, then this would not capture whether you like green vases more than red vases or something.

**Victoria Krakovna:**
So we would want a state representation that distinguishes between green and red things. But maybe our reward function doesn't really care about the exact position of the molecules in the vase. So, I guess, my hope is that if you just have an agent that learns how to act in the world it will learn these features, based on pursuing tasks assigned by humans. And then maybe we can use those features to define an impact measure over them. So if your agent was trained to be some kind of household assistant that walks around the room, and interacts with objects and so on, it will develop representations of "here are these different objects" somewhere in the higher layers of the neural net or something.

**Victoria Krakovna:**
And then if we can use those representations to give us some set of high level features like the vase is green versus red or whatever. Then combinations of those features could be used to define the possible goals, like auxiliary goals and the impact measure. So we would want the impact measure to be based on these kind of representations rather than something very low-level. But I haven't really played around with trying to implement something like this. So I'm not sure how difficult that would be, or to what extent it's practical. But I think something like this is what I have in mind.

**Daniel Filan:**
Okay. Earlier on, you mentioned that probably it was going to be desirable to somehow incorporate human preferences in the... in the impact measure. Is this how you expect that to happen, or do you think there are other ways in which you could incorporate human preferences also?

**Victoria Krakovna:**
So I think this is the main way for it to happen. Another way that human preferences could come in is in terms of the weights on different possible auxiliary goals. Because you don't necessarily need to value them equally. If we think that like some future tasks are much more likely than others, then you have a higher weight on the future task. And if some auxiliary tasks are more important to humans then it could potentially be incorporated into the impact measure through these coefficients. But if we have a state representation that captures these human-relevant features, I'm not sure whether we would need this other thing with weighing different tasks differently.

**Victoria Krakovna:**
I think it could plausibly be sufficient, because it's already defined in terms of human-relevant variables that the system has learned in the course of developing its capabilities in the environment. But yeah, I'm not sure whether that would be sufficient in practice though. So that's something that would be interesting to see some work on that. I guess, so far, I think the most complex environment that impact measures were tested on was [SafeLife](https://www.partnershiponai.org/safelife/) with some [encouraging results](https://arxiv.org/abs/2006.06547). But I think it'd be interesting to see it on somewhat more real world-like environments that are not gridworlds. Something like [DeepMind lab](https://deepmind.com/research/open-source/deepmind-lab) or one of those things where the agent walks around a room with walls or whatever, and different objects. So I think that would be interesting and probably illuminating, but also not very easy to implement.

**Daniel Filan:**
Okay. So again, speaking of how these would get implemented in practice, you've mentioned that there are still some problems that need solving here, this isn't like a done research area. Do you think that if and when we get the best impact measure, or an impact measure that we're all finally satisfied with, do you think that will look very similar to an existing proposal? Or do you think that we'll have to have really new reimaginings of what it would look like?

**Victoria Krakovna:**
So I guess if I imagine the best impact measure that we could come up with, I would be surprised if it didn't include the idea of optionality in some way. So in terms of how it's trying to capture the concept of impact, I would expect that the change in the available options would turn up in some way. So I guess I'm even more confident about the sort of deviation measure side of the current approaches sticking around.

**Victoria Krakovna:**
I wouldn't be surprised if this better impact measure has some different kind of baseline that we haven't come up with yet. Because the current proposals don't seem very satisfying to me, because they all have some downsides. I mean, it's also possible that there isn't really a good answer for what the baseline should look like, as well. So that's something I'm not certain about. But yeah I would expect optionality to somehow turn up there.

**Daniel Filan:**
So I guess now I want to ask a bit more about future work. What subproblems of side effects research are most interesting to you right now?

**Victoria Krakovna:**
So I think this question about the choice of baseline for the impact measure, I think is still very much an open question, because of the current proposals are all somehow unsatisfying. And I think for a while, I was thinking that stepwise baseline is sort of the winner. And you still need some way to actually define what this baseline policy looks like, which in many cases is not obvious. So I think that's still an open question. But now, there's also the question of like, if you're not using the stepwise inaction baseline then should we be using some version of the initial inaction baseline that starts from the starting state, then how do we really make it work?

**Victoria Krakovna:**
And I mean, if you just compare to inaction from the beginning, then there are some issues with that as well. So it's also not an entirely satisfying answer. I mean, if your agent is acting continuously in the world, then you're comparing to a world where the agent was doing nothing all along, and this becomes more and more different from where you are. And so it also doesn't seem relevant to compare to. And then you have this weird dependence on how much time has passed from the beginning, because you have to compare to what if the agent was not acting for the same amount of time. So in that way, it's kind of inelegant that it doesn't seem quite like the right thing. So it's possible that maybe we want to reset the inaction baseline whenever the action completes some kind of sub-task or something. But that's also kind of ad hoc.

**Victoria Krakovna:**
So it just doesn't feel like there is an elegant answer here so far. And I would really like to see more work on developing baselines for impact measures that can capture what we really want the baseline to do. So that's something where I think there's some possibility that there just isn't an elegant answer. And we just need to pick something that is not too bad in practice. I guess we would need to do that. But I still think maybe there's a better answer for baselines out there. I guess another sort of question that I'd like to see work on is how to define interference in a way that covers those cases that we talked about. The issue you mentioned with ranking sub-optimal policies correctly or what I said about how to get it to work on the stochastic case.

**Victoria Krakovna:**
I think the current definition captures some part of the concept, but I think there's definitely more to do there. And there's definitely some other conceptual questions that it would be good to answer. The test cases that you have that point at this, like this whole issue with butterfly effects. Do we penalize butterfly effects or not? And there, both of these answers are kind of problematic. If we don't penalize butterfly effects, then you could have an agent that can achieve impact through butterfly effects, where it has an incentive to do that, which is bad. If you penalize butterfly effects, then you're also going to get some weird incentives as well.

**Victoria Krakovna:**
And I'm not sure what the answer should be here. One thought I had was that we probably don't want the agent to cause butterfly effects, even if they're beneficial. Because this makes the agent's actions less interpretable to the human. If we want humans to understand what the agent is doing and why then hopefully, if we have a way of incentivizing the agent to be more interpretable, then even this could already reduce the incentive to cause butterfly effects. Because if you had behaviors like [I gave Charlie some apricots for breakfast](https://www.lesswrong.com/posts/wzPzPmAsG3BwrBrwy/test-cases-for-impact-regularisation-methods#Apricots_or_Biscuits) even though Charlie wants biscuits, then you could say "Why did you do that?" And then if the agent is giving apricots so that Charlie would vote in a certain way then this is not very understandable. And so it's possible that making the agent more interpretable would make it act in ways that humans can model which would get rid of butterfly effects.

**Victoria Krakovna:**
I don't know whether this actually works. But yeah, I think this is maybe a possible thing to think about. But my current sense is that overall, we don't want butterfly effects. But also I feel like we don't really have a good definition of which effects are butterfly effects and which aren't. So I think this whole area is kind of fuzzy and some of these concepts could use some pinning down as well. So I think this could be an interesting direction of future work. And of course, there are things to do in terms of implementing impact measures in more realistic environments, and in partially observable environments, and defining them in terms of the agent's higher level representations, and so on.

**Victoria Krakovna:**
But for me, I think the conceptual side of impact measures is what I'm most excited about. Because I feel like this has been one of the major contributions of the work in the space so far, is clarifying some of these concepts about, what we want the agent to do, or what we don't want the agent to do. Like the idea of impact and the idea of interference, which I think has a lot to do with things like human autonomy, that's also quite important to us. And I think this sort of conceptual work that's been going on in impact measures, even if they're never actually implemented, or used as an auxiliary reward function for AGI, I think that's quite useful for understanding what this corner of human preference space looks like. And what are possible trade-offs between different things that we might want.

**Victoria Krakovna:**
I think a lot of these test cases that you have for impact measures, you could also think of them as test cases for learned reward functions in general, even if they're not particularly trying to measure impact, you will still have some of those trade-offs. But they wouldn't be as obvious as if we are explicitly trying to measure impact. Which gives you this kind of simple toy model of some part of human preferences that models this idea of doing no harm. And then we start seeing that, oh, you have this issue with interference, or these trade-offs between addressing delayed effects versus offsetting some effects that we want from the goal achievement and so on. So I think that's a major contribution of impact measures research that I'm excited about where this conceptual work improves our understanding of these important concepts.

**Daniel Filan:**
Okay. So you mentioned you're excited for some conceptual work, basically to be done. I'm wondering if there are any experimental results or empirical questions that you think are pretty important for side effects research?

**Victoria Krakovna:**
So there's this general question of how well can we implement impact measures in more realistic environments, and will they work in practice? So will you have some environment where this default behavior causes side effects and this alternate desired behavior, where you achieve the goal without causing side effects, and will the agent actually get the right incentives from the impact measure? This is something that we have established in toy environments, but I think it's not obvious in these kind of more complex environments. If you have, let's say, some environment with the agent walking around in a room, and there's some vases in the room, will the impact measure actually make the agent go around the vases and go to the other side?

**Victoria Krakovna:**
So if you have a more realistic version of toy Box environment, then do you still get similar results, if you have the scaled up implementation of an impact measure that you would need to be able to function in that environment. So I think that would definitely be good to see, if the environment is like, let's say partially observable, will the impact measures still work and so on. And I think once we have sort of these basic experiments working with more scaled up implementations of an impact measure. This idea of crossing a more realistic room that I just described. Or if you have some household robot that is supposed to cook dinner or something. Then will it avoid breaking dishes or things like that. Then I think we can start looking at implementing the more interesting corner cases, push the limits of this implementation of the impact measure. But I think right now, I think we still don't have these basic tests in more realistic environments.

**Victoria Krakovna:**
And we do have some more scaled up implementation of [attainable utility](https://arxiv.org/abs/1902.09725) that works on [SafeLife](https://www.partnershiponai.org/safelife/). So I guess the natural next step would be to try to get that to work in a more realistic environment. Yeah. So I think that would be interesting to see.

**Daniel Filan:**
All right. So one question that I'm interested in is people... I guess people start working on AI safety, because they see people working on AI. And they think ah, your research might have some negative - side effects, one might say. I'm wondering, the research you're doing, suppose that that turned out to have pretty bad consequences. And they're not allowed to be like, "oh, it didn't pan out, and we used a whole bunch of time". They have to be like "ah, as a result of this research, something bad happened". What do you think the most likely story for large negative consequences from this work is?

**Daniel Filan:**
Victoria would also like to mention that if we don't manage to eliminate the interference incentive, then the agent may have an incentive to gain control of its environment to keep it stable and prevent humans from eliminating options, which is not great for human autonomy.

**Victoria Krakovna:**
I think, one possible bad outcome is a false sense of security. If someone implemented an impact measure, and it seems to work in practice. And then somebody ends up deploying AGI with that impact measure, and it finds a way to the game the impact measure, or it doesn't cover some important cases, and it causes some bad outcomes. Where maybe if the impact measure was not implemented, then maybe the designers would be more careful to put in some other safeguards instead. So if it displaces some safeguards that could be more useful in that particular deployment scenario, then I think that could be an issue.

**Victoria Krakovna:**
Generally, if your impact measure is somehow misspecified, that it captures the idea of impact poorly in practice and then you end up with bad incentives for the agent. I think there's other possibilities, okay, maybe other variations on that where we have identified some sort of poor incentives that impact measures could introduce, like the interference incentive. And we've also thought about some bad kinds of offsetting that we might want to avoid. But there might be other issues that we haven't thought of that this auxiliary reward could introduce that wouldn't be there otherwise. I mean, I'm not sure how likely this is. But it's possible that we haven't covered all the bases.

**Daniel Filan:**
Yeah, I guess that's an advantage of testing. And [SafeLife](https://www.partnershiponai.org/safelife/) or more realistic examples, is that maybe you can see these things that you wouldn't necessarily have thought of.

**Victoria Krakovna:**
Yeah. And I think it would definitely be useful to come up with more test cases that capture our intuitions for how we want the agent to behave if it has this impact measure. And to be able to catch these issues while we're testing these methods. And so far, we still don't have that many test cases, even in these toy environments. And if you have a variety of more realistic test cases, you will probably help identify some trade-offs between desirable properties of this auxiliary reward that we might not have thought of. So yeah, I think that would be helpful.

**Daniel Filan:**
Okay, so we're about at the end of this episode, if listeners are interested in following you and your work, and maybe seeing new exciting results on side effects. How should they do so?

**Victoria Krakovna:**
So yeah, the default place to go is my website, [vkrakovna.wordpress.com](https://vkrakovna.wordpress.com/). That's where I list my research papers, and also I publish blog posts on AI safety. In terms of my research trajectory, I've been shifting towards thinking about designing agent incentives more generally. Sort of broader than the work on impact measures. So for example, I was working on reward tampering last year, and I'm thinking about incentives more generally these days. So I'm not sure how much I'll be working specifically on impact measures going forward. But if you're interested in generally this type of work, then definitely check out my website and [my posts on the Alignment Forum](https://www.alignmentforum.org/users/vika).

**Daniel Filan:**
All right. Well, thanks for joining me.

**Victoria Krakovna:**
Thank you. Yeah, this has been fun.

**Daniel Filan:**
And to the listeners, thanks for listening and I hope you join us again.

**Daniel Filan:**
This episode is edited by Finan Adamson. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
