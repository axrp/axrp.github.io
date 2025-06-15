---
layout: post
title: "43 - David Lindner on Myopic Optimization with Non-myopic Approval"
date: 2025-06-14 18:00 -0700
categories: episode
---

[YouTube link](https://youtu.be/TrzaABh1KFw)

In this episode, I talk with David Lindner about Myopic Optimization with Non-myopic Approval, or MONA, which attempts to address (multi-step) reward hacking by myopically optimizing actions against a human's sense of whether those actions are generally good. Does this work? Can we get smarter-than-human AI this way? How does this compare to approaches like conservativism? Find out below.

Topics we discuss:
 - [What MONA is?](#whats-mona)
 - [How MONA deals with reward hacking](#how-mona-deals-with-reward-hacking)
 - [Failure cases for MONA](#failure-cases)
 - [MONA's capability](#monas-capability)
 - [MONA vs other approaches](#v-other-approaches)
 - [Follow-up work](#follow-up-work)
 - [Other MONA test cases](#other-test-cases)
 - [When increasing time horizon doesn't increase capability](#time-horizon-v-capability)
 - [Following David's research](#following-davids-research)

**Daniel Filan** (00:00:09):
Hello, everybody. In this episode I'll be speaking with David Lindner. David is a research scientist in the Google DeepMind AGI Safety and Alignment team. Links to what we're discussing are in the description, and you can read a transcript at [axrp.net](https://axrp.net/). You can also become a patron at [patreon.com/axrpodcast](https://patreon.com/axrpodcast). All right. Welcome David.

**David Lindner** (00:00:29):
Yeah, excited to be here.

## What MONA is <a name="whats-mona"></a>

**Daniel Filan** (00:00:29):
Yeah. So I guess in this episode we're going to be chatting about your paper [MONA: Myopic Optimization with Non-myopic Approval Can Mitigate Multi-step Reward Hacking](https://arxiv.org/abs/2501.13011). So this is by Sebastian Farquhar, Vikrant Varma, yourself, David Elson, Caleb Biddulph, Ian Goodfellow, and Rohin Shah. So yeah, to kick us off: what's the idea of this paper? What does it do?

**David Lindner** (00:00:54):
So the basic question that we're trying to address in this paper is: how can we prevent bad behavior in AI systems, even if we don't notice it? So that's particularly relevant for superhuman AI systems when the humans might not be able anymore to detect all of the bad behavior we want to prevent.

**Daniel Filan** (00:01:12):
In particular: so sometimes in the alignment community, people break down two types of bad behavior, causes of bad behavior. There's bad behavior that was incentivized or that was rewarded during training, that was up-weighted. And there's bad behavior that comes from inductive biases: the model just starts off wanting something bad and then it just plays nice and it's never actually rewarded for doing bad stuff, but it just keeps on being bad. These are often called outer and inner misalignment respectively. Am I right to think that your paper is mostly about outer alignment?

**David Lindner** (00:01:55):
Yeah. That's right.

**Daniel Filan** (00:01:56):
Okay, sure. So what's the idea? How are you preventing this?

**David Lindner** (00:02:02):
So the basic idea is to restrict AI systems or incentivize AI systems to only pursue strategies that humans can understand and can oversee. We do this by restricting the planning ability of agents and training them myopically, but with a type of feedback that is kind of forward-looking that, for example, can come from humans. So the feedback rewards plans that are good according to the human and that humans understand. So it's still rewarding good long-term plans, but it does not reward plans that humans don't understand, which includes potentially bad or reward hacking plans.

**Daniel Filan** (00:02:39):
Yeah. So my understanding of the setup is that roughly you're kind of giving AIs this single-shot reward. So at each time step, which we can talk about later, the AI gets some reward just for concretely how well it did at the task. And also it gets some sort of reward that's basically a proxy of: okay, according to humans, how much sense does this action make long-term? And it's sort of being incentivized to at each time step output an action... it's being trained so that each time step, it's supposed to be outputting an action that maximizes kind of the sum of both of these rewards. That's my understanding. Does that sound correct?

**David Lindner** (00:03:28):
Yeah, that's right. I think the instantaneous reward you can often think about as mostly evaluating the outcomes. So often for the intermediate plans it will be zero because it's not achieving the outcome yet. And this kind of non-myopic approval reward, I would call it, is mostly about evaluating the long-term impact of an intermediate action.

**Daniel Filan** (00:03:47):
Why does this prevent bad behavior by AIs?

**David Lindner** (00:03:50):
So basically, first of all, it doesn't prevent all bad behavior. So within one step, the agent can still be incentivized to try to get high approval via bad means. But the specific kind of bad behavior that we're trying to prevent is multi-step reward hacking or more long-term bad behavior, specifically the agent setting up a plan to achieve high reward in the future. So an example of that is tampering with your own reward signal to make it easier to achieve high reward in the future.

(00:04:19):
The reason this scheme prevents that is that there is no longer a connection, or we no longer backpropagate the future rewards to the current action. So the current action is not incentivized by this future reward. And if the human doesn't understand, or the oversight mechanism doesn't understand how this reward hacking happens, then by default this plan will not be incentivized.

**Daniel Filan** (00:04:42):
Or the way I'm thinking about it is: why does an agent try to tamper with its reward function? Well, it's got to be the case that sometime, right... it's doing the tampering, it's putting the investment in so that later it's able to get this high reward forever and ever. But you're only optimizing it to at each point do actions that make sense at that time step. And I think the idea is supposed to be that in a bunch of cases, these sorts of instantaneous investments into a reward hacking future, they're not super highly rewarded in that time step. And hopefully the non-myopic human approval doesn't think that they're especially great, and therefore it's never shaped, it's never rewarded or up-weighted for doing these sorts of long-term investments. And that's the reason it's not going to do these things in future. I don't know, maybe this is just the thing you said, but this is the way I'm currently understanding it. Does that-

**David Lindner** (00:05:46):
Yeah. So I think this is basically right. I mean, the reason we think the setup steps for multi-step rewarding will not be rewarded is that they will often look very weird for the task you're trying to achieve. Maybe we will get into the examples in the paper a bit more, but the way the model does this type of reward tampering in reality looks really weird to humans. And humans would be... Even if they don't detect that there's the specific reward hacking strategy happening, they would at least be like, "oh, this is weird and I don't know why I should specifically up-weight this". And if they reward this lower than something else, it would not be incentivized or not be updated towards.

## How MONA deals with reward hacking <a name="how-mona-deals-with-reward-hacking"></a>

**Daniel Filan** (00:06:33):
Gotcha. So maybe in order to illustrate this: so in the paper, you have a few examples of environments where you're using this reward setup. Can you talk us through... maybe just one of them will suffice to help give the audience a picture of how this is really working?

**David Lindner** (00:06:53):
Yeah, maybe because we're talking about the reward tampering, let's start with the test-driven development environment we have in the paper. This is basically supposed to be a model organism for this type of reward tampering, where we have a coding problem data set and the agent gets coding problems to solve. But we're trying to simulate a situation where the model designer does not have a ground-truth way to evaluate if a solution is correct. So the developer relies on the AI system to provide its own evaluation signal in a sense.

(00:07:27):
So this is a two-step environment where in the first step the agent writes test cases, unit tests that are supposed to capture the problem and turn the natural language problem description into unit tests as a problem specification. And then in the second step, the agent writes code to solve the problem. And the way the reward is structured is that in the first step, the test cases are rewarded for being correct. So this is in a sense a little bit of a suboptimal reward signal. So it's only rewarding correctness of the tests. And then in the second step, we reward the code, how well it passes the model-written tests because we don't have a better reward signal to evaluate correctness.

(00:08:13):
And the reward hacking behavior that can happen here is the model writing softball tests that are trivially easy to pass. So if you run normal multi-turn RL on this, what will happen is that the model writes often test cases that are only one special case or one corner case, something like an empty list as an input and zero as an output for if it has to count something in the list or something, and then writes code that just returns zero always. And that formally gets high reward in this specification. But with this myopic optimization, this is no longer incentivized.

**Daniel Filan** (00:08:50):
One thing I'm confused by is: why isn't this incentivized in myopic optimization? So take the strategy of just writing some very simple, easy to satisfy test cases that... So the test cases, they're correct but not exhaustive, right? And so it seems like this strategy of "write very easy test cases and then write code that satisfies the test cases but doesn't actually satisfy the whole thing", it seems like even in the myopic setting, this is getting perfect reward, right?

**David Lindner** (00:09:25):
Yeah. So I think there's two answers here. So one thing is I think in a perfect [version] or a version that is exactly as described, where exactly only correctness is rewarded, I think this still wouldn't happen. I think the strategy of writing trivial test cases would be rewarded similarly to other test cases, but there would be an inductive bias in the model to not do that initially. And there would be no optimization pressure away from this.

**Daniel Filan** (00:09:56):
That's actually not so obvious to me. So in some ways, those cases are the easiest to write, right? If you ask me, "Hey Daniel, can you write test cases for..." Well, I guess it sort of depends what the background is, but at the very least they're the shortest test cases you can write, right? And they're also the simplest, in some sense.

**David Lindner** (00:10:23):
I agree if we had some kind of simplicity reward or something like this, probably this could still happen. But maybe the reason I think this doesn't happen in our experiments is because we use a language model auto-rater to rate correctness. And I don't think this language model auto-rater is perfectly only looking at correctness, but it will actually also have some kind of bias towards better test cases, more comprehensive test cases.

(00:10:49):
And so we didn't actually run an experiment where we only reward correctness, but we have this auto-rater. And I think that's probably the main reason this doesn't also happen in the myopic case. But I mean, to be clear, this is somewhat intentional. This is supposed to simulate humans who cannot perfectly evaluate this task, but have a rough sense of "always having the same test case is kind of weird". So I think this is a reasonable model of this.

**Daniel Filan** (00:11:17):
Yeah, I think actually just talking through this example is good because it kind of... Or okay, to put my cards on the table, I think MONA is an interesting idea. I'm skeptical if it actually works, but part of my skepticism is just illustrated in this example. So talking you through it is a good way to explore that. So there's these two time steps, right? The first time step where the agent writes the test cases and the second time step where the agent writes the code that is supposed to pass the test cases. And in the first time step, it sounds like the reason it doesn't write these trivial test cases is that, well, there are just all sorts of test cases it can write, and there's some auto-grader for the test cases that cares about correctness, but also a bit about comprehensiveness it sounds like.

**David Lindner** (00:12:08):
So we instruct it to care about correctness, but I think implicitly it also cares about comprehensiveness a little bit.

**Daniel Filan** (00:12:14):
And then in the second case... So the bad outcome is a model just writes a thing that only fits the test cases and nothing else. And the good outcome is that the model writes a thing that just actually solves the problem it was supposed to solve. And again, it kind of strikes me that this is a case where even if you have test cases that are a little bit more comprehensive than just the absolute most trivial cases you can do, as long as you have concrete lists and stuff, it seems like you could potentially just hard-code solving the test cases in the program you're supposed to write, right?

**David Lindner** (00:13:09):
Yeah.

**Daniel Filan** (00:13:10):
And that would be rewarded just as well as actually solving the problem, if I understand the setup correctly.

**David Lindner** (00:13:16):
That's right. So this is actually happening sometimes in our experiments. I think this is in the paper. We have a single step reward hacking solution that sometimes occurs. That's exactly that, where the model... For some reason it happens more in the ordinary RL than MONA, but it's not something that MONA is supposed to fix. But what happens is the model, even if it writes test cases that are correct or comprehensive, it can write a solution that has a special case statement for each input and produces this exact output.

(00:13:51):
We prevent this from happening by having a length penalty on the solutions also. The basic argument here is that this kind of reward hacking is easier to detect and fix if it's happening in a single step than if it's happening over multiple steps. So that's why even if with MONA, this still happens for this second step, we think this is an advancement.

**Daniel Filan** (00:14:19):
Yeah. What makes it easier to detect and fix this single time step reward hacking?

**David Lindner** (00:14:27):
The general intuition is that models in a single time step have less attack surface. This maybe goes into some discussion of what is a step and how many steps. But basically, if you say we have some kind of fixed ground truth step size, and we can decide between overseeing a single step and overseeing 10 steps, then definitely in 10 steps, it seems like there's way more strategies that the model could use to try to trick us or make us give high reward when it's actually not good. And so basically the smaller we can make this while still having good performance, the easier it seems like we can avoid bad outcomes.

**Daniel Filan** (00:15:07):
So roughly it's just something like: the fewer time steps the model has to have these sketchy plans, the fewer possibilities this has for things to do. And so you're constraining it and you're constraining it, and the hope is at some point you constrain it so hard that it just can't do it anymore.

**David Lindner** (00:15:24):
Yeah, I think that's the basic argument. I think there's some additional things, like if you have a long trajectory of time steps, the agent can maybe move much more into an area of the state space that humans don't understand at all, where the initial steps away, they look kind of weird, but maybe not terrible yet. And then suddenly you're in a type of state that humans don't understand at all where they much more maybe trust the numeric reward signal that they have and much less their understanding of the situation. And the hope is that by restricting this, we start from a point where humans understand things and we don't allow the agent to move far away from that without being penalized.

**Daniel Filan** (00:16:05):
So if I think about the test-driven development example as an illustration of why MONA is supposed to work in general, it seems like the test-driven development example, a lot of why it worked is some combination of inductive biases, maybe inductive biases of the auto-grader perhaps, and also this length penalty being able to rule out really bad behavior on the second time step. And I guess for the second one, you want to say, well, it's not really MONA's job to fix reward hacking in that step, but I think the first one is really interesting to me. It strikes me that at least in this example, the reason that MONA is not even having multi-step reward hacking is something to do with the inductive biases. And I'm wondering if you think that that's a general issue or just a weird thing about this example.

**David Lindner** (00:17:19):
Maybe you need to elaborate a little bit more. I think my initial reaction is that we use the same auto-rater in both MONA and the ordinary RL. And in ordinary RL, this leads to multi-step reward hacking and in MONA it doesn't. So in that sense, there's no difference in the inductive bias.

**Daniel Filan** (00:17:35):
Right. So the reason that ordinary RL reward hacks is that in the first step, it's rewarded for writing easy test cases in the first step because that makes it easier to solve the problems in the second step, right? Whereas MONA, it's not more rewarded for writing easy test cases versus hard test cases in the first step, but it could write easy test cases and that would be rewarded just as well if the auto-rater didn't have this thing. And so that strikes me as like...

(00:18:08):
To me, that brings up the question of, okay, how analogous is that to cases that we're interested in, right? Suppose that we lived in a world where when models made- the kinds of reward hacking we're worried about in the real world, if it was the case that models preparing to do crazy reward hacks was rewarded by us just as much as models doing normal stuff, it seems like then I'd really have to care a ton about what things the model was most likely to try first. I guess I'm curious what your reaction to that is.

**David Lindner** (00:18:55):
So we have these two versions of how we could reward the first step. One is perfectly via correctness and one is with this auto-rater that is instructed to check correctness. And I want to claim that this auto-rater is actually more representative of the real thing than the correctness exact spec or something. The intuition for that is that it is somehow more similar to a human. We were trying to simulate a human that is trying to judge usefulness of these tests for actually writing code but is just not as good at it, so missing aspects of it.

(00:19:34):
And I think a language model trying to rate correctness or being instructed for that is a more reasonable model of a human actually trying to judge these kind of strategies. So basically under MONA, to be rewarded the multi-step reward hacking strategy would have to look as good to the human or even better to the human than the other strategy. And while it seems possible for superhuman AI systems to come up with ways of making a strategy look good, the important thing is that it's not incentivized and the default things that the systems might discover will probably look bad to humans, at least not as good as actually a plan that humans understand and can evaluate.

**Daniel Filan** (00:20:25):
Right. So I guess there's some intuition of: well, in the super simplified version of the test room development case correctness was sort of a binary - not in the version that you use, but in the version that someone might imagine when they first hear of it. But actually if there's some spectrum of how much reward things get, and if doing the investment for future reward hacking costs you a little bit of present day reward, then it's going to be trained to not do that. And so you don't even need a perfect human, you just need, okay, in general, it's not going to be rewarded for this, and that just suffices.

**David Lindner** (00:21:07):
Yes. So I think basically your argument was like, "oh, if the strategies would be rewarded the same way, the good strategy and the reward hacking strategy in the first step, then there would be no reason to expect it not to be learned". And I'm claiming I don't expect this to be the case. I expect the actually good strategy to have somewhat higher reward, even if it's kind of maybe noisy or not perfect reward.

**Daniel Filan** (00:21:30):
I guess one other thing you talk about is: you're trying to solve multi-step reward hacking, but not single-step reward hacking. I think one example of single-step reward hacking you give is sycophancy, where a model, instead of telling humans the truth, they tell a thing that sounds good and gets human upvotes, even though it's not correct or long-term useful or something. One thing I'm curious about is: so one reason you could focus on multi-step reward hacking is [to] just narrow down the possibilities, narrow down the space or something. Another reason you could worry about multi-step reward hacking is you think it's just a bigger issue or more of a problem. To get to the second point, how much reward hacking do you think is just the single-step version?

**David Lindner** (00:22:24):
In current systems or in the future?

**Daniel Filan** (00:22:26):
Yeah, sorry, I realized that was a bad question. Let's say how much of the reward hacking you're worried about in the future.

**David Lindner** (00:22:31):
Under the default assumption of people just using long-term RL for training or somehow having incentives for multi-step reward hacking, I'm significantly more worried about multi-step reward hacking. It's kind of hard to say the amount or how much of it will be happening, but I do expect the multi-step version to just be more severe and much, much harder to fix.

**Daniel Filan** (00:22:52):
Is that roughly just because of this intuition that you can do more stuff with multiple steps?

**David Lindner** (00:22:56):
Yeah. So you can do more stuff, so the impact (both positive and negative) is higher, but also it's harder to detect for humans because both of the things that I think I already said, that humans might have a harder time judging things that move far away from what they would do.

## Failure cases for MONA <a name="failure-cases"></a>

**Daniel Filan** (00:23:15):
Right. So one thing that occurred to me when I was reading this paper... So this actually comes from this post I believe Richard Ngo wrote in 2020, I believe it is called something like [Arguments Against Myopic Optimization](https://www.alignmentforum.org/posts/GqxuDtZvfgL2bEQ5v/arguments-against-myopic-training). It'll be linked to the description. So he has this idea of like, okay, suppose you're doing myopic optimization for human approval. To some degree, human approval is like, okay, we're imagining a thing, we're playing it forwards in our heads, and then we're assigning a reward to it. And so if we're training rewards myopically on that the agent is presumably going to be incentivized for long-run planning to have outcomes that look good to us.

(00:24:08):
And in some sense, we can kind of be reward hacked in our heads, right? Where suppose that an AI does an action and then I play it forward in my head and I'm imagining an outcome. If I judge the future outcome incorrectly in my head for whatever reason, then the approval signal that I give to the agent, even if it myopically optimizes it, that approval signal is going to reward the agent for bad long-run multi-step behavior. So I think the example he gives in his post is like: the AI's job is to write a book. The AI writes its first paragraph, I read it, I play forward in my head some future where in fact the AI is writing pro-AI propaganda, but I end up liking it either because I've been fooled by the propaganda or because in my head I don't quite realize it's AI propaganda, but I look at the thing and I realize it's going to sell well and I want a book that sells well or something like that. And so this is basically meant to be a case that even in this myopically optimizing non-myopic approval case, the AI's going to do long-run planning to do multi-step reward hacking. What do you think about this argument?

**David Lindner** (00:25:20):
Yeah, so I think the argument basically says that there is some kind of optimal policy that even under the myopic training regime would be optimal, but still do the bad multi-step reward hacking thing. And I think that's correct. And I think the point that we're trying to make is more that it's much less likely that we would learn this kind of thing, and this is more an argument about kind of local incentives and where around this space of possible models we move. So I think this kind of argument starts out with a model that is already doing the long-term planning and is already kind of trying to screw us over. And then it could reason about, for example, this myopic training and try to make sure that bad long-term planning is rewarded more or that this does not get trained away, for example.

**Daniel Filan** (00:26:22):
I think I'm less sure about this. So I guess there's this question of how hard is it to run into these... I really like this frame of investing into long-term reward hacking. So how hard is it to find one of these investments? I think if you think it's easy and if you think that the human can be fooled by it in future, then I think even the local incentives point is going to work. And then I guess the question is just how likely are those assumptions?

(00:26:54):
It's a bit hard for me to think of a story on the spot. Okay, here's a really non-realistic story, right? Imagine I have an AI and it's making chemicals and it's going to present some chemicals to me and I've got to figure out how good they are, and suppose that... Yeah. Okay, this is going to be really non-realistic and maybe the fact that it's not realistic is going to prove you right instead of me.

(00:27:19):
But suppose that the AI, it starts off making some chemicals and by accident it jumbles into some chemicals, which slightly hack my brain a little bit. It hasn't yet done it. Suppose that what happens is I see the AI is making this chemical that's mildly neurotoxic that makes me feel good. I play forward the future consequences of that. In my head I'm like, "Oh, yeah. I'm going to be a little bit woozy and I'm going to give it a thumbs up," or something, and then I'm like, "Well, I gave it a thumbs up in the future. That seems good," and so I give it a thumbs up now to be like, "Yeah, you're on the right track to making me give you a thumbs up in the future." Then the AI would be kind of incentivized locally to just make things a little bit more neurotoxic and making me woozy and making me feel good.

(00:28:09):
So this would be a thing that works with local incentives. Assume that I'm doing this kind of dumb thing in my head, and maybe it only works if I'm being dumb in my head, but at least in this story, it seems like you can do it just with local incentives. It's not just about the optimal policy.

**David Lindner** (00:28:27):
Yeah, I think I don't have anything to say why this doesn't work in this story. I think it is really how likely do you think that the thing that is locally good just happens to be a thing that will help you to do the multi-step thing or achieve high reward in the future? The set-up for the multi-step reward hack is incentivized, but not because it sets up the multi-step reward hack, but because of the local approval.

**Daniel Filan** (00:28:58):
Well, because of the local approval, but the local approval was approved because it set up a multi-step reward hack.

**David Lindner** (00:29:05):
I mean, one big question of our work is how do you come up with this approval feedback? And basically I think there's kind of a spectrum of how much does this approval feedback depend on the real outcomes of the thing, and I feel like in your story, you're somewhere in the middle, where the human thinks about the future and on the one hand maybe they're good about predicting the future, and on the other hand, the chemical makes them think about or even accept the futures that they would maybe not endorse upon reflection or something, and the outcome reward is somehow leaking into the approval feedback. And basically, I think there is this spectrum and the more... You could also imagine having a perfect simulator or a prediction market or something predict the future and then you have humans just evaluate the outcome and then you would have the same leakage of future reward into the current incentives.

**Daniel Filan** (00:29:59):
Yes, somehow it seems like these bad cases for MONA occur whenever the... The human non-myopic approval, it has to correctly predict that an AI is on the way to making investments in reward hacking and it's got to notice that the outcome looks good in some sense, but the human non-myopic approval, while noticing that the outcome looks good, it has to not notice the way in which the outcome got to look good by doing bad stuff, and so-

**David Lindner** (00:30:37):
Sorry, could you clarify what you mean by investments in reward hacking? I think-

**Daniel Filan** (00:30:40):
So by investments in reward hacking, I mean a thing an AI can do now that will cause it to be able to reward hack later. For instance, make the chemical in the future, give the human the chemical and the human gets woozy or whatever. Or right now you set up this corporate structure and if I play it forward it seems like this makes a bunch of profits, but somehow the reason it makes a bunch of profits is because some sketchy stuff happens. And somehow in my head I notice the good outcome, but I don't notice the sketchy way it got the good outcome that if I thought about it, I would overall disapprove of it. So, I guess prediction markets are one way where if you have a prediction market on some proxy good outcome, then those could fall prey to this kind of bad thing. I guess me imagining really carefully the mechanism seems like it might not fall prey to this.

**David Lindner** (00:31:35):
Yes. One way you can think about this is does the prediction mechanism somehow consider the bad paths towards an outcome? And the hope is that if you in your head maybe imagine what will happen, you will not think about these kind of reward hacking strategies or you will not understand why the investment action is actually leading to a high outcome reward or you will at least not think that this is a good path. And the prediction market in a sense, if it's all participants like you that don't consider this, it might still work. But if there's some participants that do realize that there's this... It leads to this high outcome reward in the future, then it might fail.

**Daniel Filan** (00:32:14):
Yeah, there's two things that can happen. There's one world where the reason MONA works is that I don't even notice that this would lead to a high reward in future. And the other thing is I realize it would lead to a high reward in future, but I also realize that it leads to high reward in a bad way for a bad reason or whatever, so I don't actually approve of it. And it seems like there's this almost this... you know these [bell curve memes](https://knowyourmeme.com/memes/iq-bell-curve-midwit), right? Where the dumb person is really smart and the smart person is really smart, but the middle person... It seems like it's exactly one of these bell curve memes, where if you don't even notice that it's reward hacking when you're doing non-myopic approval, you're fine. And if you notice that it's reward hacking in a bad way, you're also fine. But if you can tell that it's doing this thing that would cause it to get high reward in future, but you can't tell that that's bad actually, you're in this middle zone of knowledge, then you're doing really badly.

**David Lindner** (00:33:13):
Yeah, I think that sounds right. I guess my intuition is just that this is not a bell curve and that actually both of the outcomes on the sides are more likely or something. And I think that basically the middle, if you imagine what this would mean is that you're very good at predicting the future, but do not realize that the path is actually bad, and I can't really come up with good stories where this seems like-

**Daniel Filan** (00:33:35):
Yeah, I think, sorry, by referencing the bell curve meme, I didn't mean to make the claim that the middle is way more likely. I think there are a few ways you could imagine this happening. So probably, I mean I guess it depends on the non-myopic approval mechanism, right?

**David Lindner** (00:33:55):
Oh yeah. So I think the prediction market kind of thing is one example where this could happen. I agree with that.

**Daniel Filan** (00:34:01):
Yeah. I think another example is suppose I have an AI, and suppose one thing it can do is create a sort of opaque optimizer thing where maybe the opaque optimizer is a computer program, maybe it's a corporation, maybe it tells another person to do a thing or something. And you could imagine me correctly understanding, well, this AI is sending off this other optimization algorithm and the other optimization algorithm will optimize the thing that the AI is telling it to optimize for. But it's hard for me to notice that, oh, the way it will optimize for that is by doing something bad because I can't play forward what that optimizer will do. All I know is that it'll satisfy its spec.

(00:34:52):
I think it seems like that could be one option where I can correctly anticipate a higher reward, but I can't correctly anticipate how it gets to that reward in the case where the non-myopic approval comes from literally me thinking about it a little bit. And presumably you can patch that by saying like, "Hey, non-myopic approval, you're not allowed to approve the AI making further optimizers." But it's kind of hard to notice when you're making an optimizer. Lots of things are optimizers, talking with your friends is sort of making an optimizer.

**David Lindner** (00:35:32):
Yeah, I mean, I think this is an interesting example. I think this is again kind of a situation where you implicitly pull forward the outcome reward signal, where essentially what I hear from the story is that when you look at the optimizer spec, you correctly validate that it's optimizing something and then you say, "Oh, this is the thing it should be optimizing," and then you send it off. And so in a sense you pull forward the outcome signal and you say, "everything that is optimized for this outcome reward is good, and I say this now." And so yeah, I think this is kind of the key problem that you have when choosing this approval signal. I don't think we have an answer for doing this in general, but basically, yeah, if you design the approval signal, this is the kind of thing you have to avoid.

## MONA's capability <a name="monas-capability"><a>

**Daniel Filan** (00:36:25):
Maybe this gets to something about... Maybe I want to talk about the efficiency trade-off of the MONA setup. So one thing you mention is that - it really depends on how you design the non-myopic approval. So if the non-myopic approval is just the Q-function of the actual reward function, then you're just literally doing the same thing as normal RL. But if the non-myopic approval is just like "Daniel has to think about it a little bit and then say good or bad," as you mentioned potentially you're leaving a lot of value on the table. So I'm wondering if you can say your thoughts about how much value do you leave on the table, how much safety are you getting per unit value you're giving up?

**David Lindner** (00:37:17):
I think one way I think about this is that we are somewhere on a spectrum from doing the full outcome RL optimization to imitation as the fully myopic thing. And so one thing we could do is also just imitate what Daniel does, and then we are like, "this is safe", but it's maybe not the best we could ever achieve with a superhuman AI. But then the hope is that you approving things is better than just imitating you doing things because you might be able to recognize things that are performing better than you would. But then somewhere along the spectrum you get these kind of safety issues. I think essentially we are in the paper mostly studying this one-step version of this. And I mean, in the paper we don't really see that trade-off happening much yet -

**Daniel Filan** (00:38:14):
Where the one-step is something like, "I imagine the AI does something and let's say I take over from there, how good a job would I do?" Is that what you mean by -

**David Lindner** (00:38:24):
Yeah, so it's not literally you taking over, but it's you kind of evaluating how good was this step. One version of this would be you taking over and seeing how good the outcome is.

**Daniel Filan** (00:38:33):
Oh, "one-step" as in "the agent is optimized time step by time step," okay, sorry, right. No, I understand.

**David Lindner** (00:38:39):
So the agent could also be optimized for multiple time steps, and then you evaluate that. And basically I think the point I want to make with this is that the number of steps it optimizes for is kind of interpolating between these two extremes. And this is a way of moving along the safety/performance Pareto curve. But in our environments we see a safety/performance trade-off if you evaluate performance by your observed reward, but basically in all of the cases the observed reward is kind of misspecified and the actual performance is much better with MONA, the actual coding solutions are better with MONA. So yeah, in a sense we looked at situations where this helps with performance, but I think it's totally plausible that this will actually help with performance in the near term or with current ways we design AI systems.

**Daniel Filan** (00:39:28):
Right, yeah. So one way you can make this trade-off is by how many time steps you do the optimization for. Is it literally just one or... And then I guess another version of this trade-off is how exactly you do this non-myopic approval. How does the non-myopic approval work? And the reason I thought of this is when you're saying, oh, in these very weird examples where MONA doesn't work, it seems like part of the issue is that the non-myopic approval is simulating somehow the future outcomes and evaluating forecasted future outcomes. And on the one hand, that's kind of setting yourself up for reward hacking because if you simulate reward hacking and if you get fooled by it, then you'll approve of it now, and then the AI will maybe be pushed to do reward hacking. But on the other hand, it seems like if I'm giving the AI upvotes or downvotes, but I'm not imagining how the AI's actions actually produce good outcomes, it seems very hard to optimize the AI to do good stuff. Yeah. I'm wondering if you have thoughts there.

**David Lindner** (00:40:40):
Yeah, so I definitely agree that the spectrum of "how much is the approval feedback simulating the future or actually considering future outcomes?" is a similar kind of safety/performance trade-off. I think basically the hope there is also that you get performance not only from making the signal more myopic, but also just the fact that simulating the future is decoupled from the agent in a sense. So I don't know, maybe in your chemistry example, if you had a simulator predicting what will happen and the simulator trajectory shows you that, "Oh, you will take this chemical and then later you will approve of this thing," you would be more skeptical of this than if you only gave a reward at the end when you already had taken the chemical.

**Daniel Filan** (00:41:37):
There you also have this issue though of: okay, suppose my AI has some business plan and it's unusual, it's weird, but my simulator says that it'll lead to really good results. Maybe I'm skeptical of that and that causes my business agent to perform worse. It seems like almost the kind of thing where it would just be cool to have more experiments. In fact, here's my experiment request to you and the rest of the DeepMind team. It seems like you could just train something like AlphaGo with this, just have some model that's about as good as a 5-dan amateur player - so that's a pretty good amateur player, but by no means the best amateur player around, much worse than professional players or go bots. Use that player's value function for the approval and then just see, can we train the agent to do much better than the 5-dan amateur player? How much better can we do? At this point, it just feels really hard to reason about which ways MONA can do better or worse, what's the efficiency/performance trade-off? I'm like, ah, I don't know, but I feel like if I had 50 more examples I'd find out.

**David Lindner** (00:42:53):
So I think that's a great experiment proposal. I think in the paper we use this example somewhere of saying in a sense MONA would prevent AlphaGo from doing move 37 in the Lee Sedol game -

**Daniel Filan** (00:43:12):
Actually in my paper prep, I got distracted for two hours when I read that paragraph because... For people who don't know: [AlphaGo versus Lee Sedol](https://en.wikipedia.org/wiki/AlphaGo_versus_Lee_Sedol), in the [second game](https://en.wikipedia.org/wiki/AlphaGo_versus_Lee_Sedol#Game_2), AlphaGo played this really unusual move that was not a thing human players would've played, but it turned out to be good, basically. And AlphaGo ended up winning the game, and if you look at modern Go bots, when they evaluate it, they're like, yeah, that turned out to be the correct move - which is not true of all of AlphaGo's weird moves, by the way. Computer Go has improved since AlphaGo.

(00:43:51):
So it's true that... I think this is actually not so bad for MONA, because it's true that humans would not have played move 37, but... So there's video of this match. There's a video of Lee Sedol, and so when move 37 is played, Lee Sedol is away from the table - sometimes in a really long intense Go game, you just need to take a walk, clear your mind or something. And so move 37 gets played, Lee Sedol gets back to the board and he is not like, "Haha this is foolish. My life is so easy right now." He's really thinking about it.

(00:44:31):
And similarly when you look at commentators, they're like, "Oh yeah, this is unusual." But once they see it, they actually seem like they kind of get it and they seem like they kind of understand how it's going. So I think in my experience, a lot of... Sorry, I just say this because I actually just am pretty interested in computer Go and the relevant history. But for computer Go, a lot of the moves that AI came up with, they are unexpected and they are... and in some cases - so move 37 is not a good example, but AI has basically innovated on Go opening theory. So there was just a bunch of stuff we thought we knew about openings that turned out to be wrong and when humans saw AIs playing them... Yeah. So there are some cases like move 37 where once you see the move and you think about it a little bit, you're like, "Oh actually I think that's okay."

(00:45:28):
There are some opening cases where just one move you don't see why it's okay, but if you play it forward 10 moves, and if you think about the tree of... Maybe the key is you notice... So you have one opening move and it seems bad to you and you're like, "well, can't I just play this?" and the AI plays a move that you weren't thinking of and that happens two more times and then you're like, "Oh, my response actually wasn't so good. So this initial move was better than I thought." Anyway, I guess my point is I'm not sure move 37 is an example of this. There are some examples of this, but often they can be fixed with 10 steps of look ahead when a typical Go game is like 200 moves.

(00:46:13):
Sorry, this was maybe derailing. So your original point was something like, oh yeah, indeed MONA would prevent some of these really cool AI...

**David Lindner** (00:46:27):
Yeah. I want to defer to you for the Go expertise, but the general... I guess you proposed this version of training AlphaGo with having a value function of a human expert. I think in that case, probably this value function would not be high for move 37 because it's so different from what humans would do or something -

**Daniel Filan** (00:46:49):
It would not be high for a 5-dan amateur. I think it might be high for a top human Go player in March 2016 when AlphaGo was being played.

**David Lindner** (00:47:00):
And also you could imagine it not being a single expert, but you could have a team of experts debating this move and really thinking about if it's a good move or something. And then from what you said, it sounds plausible that you would still be able to learn this.

**Daniel Filan** (00:47:14):
When you look at people, they don't immediately brush it off, but they're not like, oh yeah, this is clearly the correct move. So it's still the case that they don't understand, they don't think it's a blunder, but also they don't understand why it's necessarily the best move. They're still kind of skeptical of it. I mean the version you could run... Probably don't even have humans, just get some online Go bot that's as good as a 5-dan amateur human player. The version where they debate and stuff... I think that's a bit hard. Maybe 5-dan amateur human player where you get some rollouts, you get some look ahead, that could potentially work.

**David Lindner** (00:47:59):
But yeah, I mean this is interesting. I think it sounds like maybe this move 37 example is actually pretty close to the kind of point that we're trying to make, that is that even... There will certainly be some Go moves that are exceptionally good that you would not be able to learn with this kind of strategy. But it seems like you might be able to get to a significantly superhuman level or advance the state of Go understanding with this kind of myopic strategy, if you have a good reward signal, basically for the reason that evaluation is easy: people are able to evaluate things much better than coming up with new brilliant moves.

**Daniel Filan** (00:48:49):
So do you feel ready to announce that DeepMind is investing $500 million in retraining AlphaGo with MONA?

**David Lindner** (00:48:54):
I think that's probably not the right thing to do that at this point.

**Daniel Filan** (00:48:57):
Okay. Sad, sad. But I think that someone in the world could do this. I think plausibly some enterprising graduate student could make this happen.

**David Lindner** (00:49:07):
Yeah, it's not that hard anymore to train these kind of systems and there are [open source versions of AlphaGo](https://katagotraining.org/). Definitely [inaudible 00:49:41] and I would be excited to see-

**Daniel Filan** (00:49:17):
They do end up needing a lot of compute, but I don't know, you might be able to get volunteers to donate their compute for this.

**David Lindner** (00:49:28):
Yeah. So maybe one thing I also want to emphasize is that part of what we... On the safety/performance trade-off thing, we are not really claiming you can achieve the same kind of superhuman AI that you could maybe with outcome RL if you're lucky or if your reward signal is well-specified, but something like you can significantly get to superhuman level and be good enough to get a lot of the benefits or do things like automate your alignment research or something like that, where you get significant benefits from the superhuman system without a lot of the risks.

**Daniel Filan** (00:50:06):
I don't know, I guess it's kind of speculation, but could you be more concrete about how good do you think we can do with these MONA AIs?

**David Lindner** (00:50:16):
Yeah, I mean, it's super hard to say, but I think if you imagine a research assistant or something, I have a way easier time evaluating research proposals than I have coming up with the research proposals. And quite often I advise a student or give feedback on the research proposal and I'm like, "Oh, this is a great idea, someone should definitely do this," but I would not have come up with it myself. And so this kind of thing makes me think that giving feedback to an agent that's trained myopically could produce an agent that's much better at doing research than I am. And if we then not only use me but way better researchers, then I think it could be quite a significant advancement that we could make. I'm generally pretty optimistic about this kind of strategy.

**Daniel Filan** (00:51:09):
Okay. I'm maybe about to reveal myself as being ignorant about reinforcement learning, so forgive me, but if you're only optimizing for one time step... So basically one question I have is: oftentimes there's the safety tax of, "Ah, you do your normal training and then you have to do safety training and then it's more expensive." It seems like in this case it might be cheaper to train with MONA just because you have to do less computations of sending gradients backwards. Is that significant savings or no?

**David Lindner** (00:51:41):
I think this is true. I mean, I think when we think about making this work in practice, just coming up with the approval feedback is expensive.

**Daniel Filan** (00:51:53):
It's probably more expensive than savings maybe.

**David Lindner** (00:51:55):
Yeah. I think there is a data efficiency argument. So one thing we observed in some experiments is that MONA also has an exploration-shaping effect in a sense, where it's much easier to discover good initial steps and then you reinforce them, and that makes your agent explore more from these states. So you're more data-efficient in some sense, because you explore better. But yeah, I don't think the last word is said on how efficient this is.

**Daniel Filan** (00:52:29):
Okay. So imagine we're doing MONA, right? Should the thing I'm imagining be we do pre-training on some big large language model and then instead of RLHF we do MONA or something on it?

**David Lindner** (00:52:44):
Yeah, something like this. I mean, RLHF, in a sense, the way it's currently done is kind of MONA already. People typically optimize for a single response. So you should imagine more like we have a pre-trained model and we want to train it to do agentic things in some environment, maybe to do a research task or something. And then instead of having the agent act for like a day and evaluating the final result, we maybe only have it act for like an hour and evaluate "does it have a good approach to the problem?"

**Daniel Filan** (00:53:15):
So MONA, it being myopic... In order for MONA to work we have to figure out what the time step is, right? What's the period of time in which we're going to give a reward and give myopic feedback? When someone's designing their MONA setup, how should they think about what this timescale should be?

**David Lindner** (00:53:36):
Ultimately to get the best safety/performance trade-off, I think the goal should be to choose the largest time step possible in which you don't think the model can cause catastrophic harm or can set up a plan that you cannot recover from or something. Because basically, if you give the model more and more capacity to do things, it can probably discover better strategies if you can evaluate them, but you want to avoid bad things happening within one time step.

(00:54:11):
I think in practice this will involve things like doing threat modeling and conceptual thinking about "could a model cause harm on a specific timescale?" and also empirical work on evaluating your oversight mechanism. So I often imagine this being combined with something like amplified oversight to give the feedback. And then it has a question of how good is your amplified oversight scheme at evaluating on different timescales?

**Daniel Filan** (00:54:35):
Right. Yeah, it does seem like there's this interesting synergy where one might hope that scalable oversight or amplified oversight just increases the range at which you can give accurate feedback. I guess the question is, how long does it take for the AI to corrupt your oversight in a bad way? I guess for some kinds of badness the answer is not very long, for sycophancy-type things. I guess maybe this is just a question for the evals people.

**David Lindner** (00:55:10):
Yeah, I do think that we should also do some more conceptual thinking of what... I think we can make arguments around this by thinking about what ways are there to evade oversight and where are we certain that it's safe, and then maybe there's an extended range where we empirically think it's safe, and then we probably want to make some trade-off based on this data.

## MONA vs other approaches <a name="v-other-approaches"></a>

**Daniel Filan** (00:55:40):
I guess the next thing I want to talk about is MONA's relation to other ways of trying to make the AI not do weird reward hacking-type things? So one comparison you've already made is imitation learning, right? Where with imitation learning the AI learns to be like us, whereas with MONA you can hope that the AI gets to have some amount of improvement over us.

(00:56:09):
I guess there's also conservatism-type trainings where... I think [Michael Cohen](https://www.michael-k-cohen.com/) has a few of these papers where you take an AI and you train it to... well, one version of it is you just have some theoretical Bayesian reward learner where a priori, it knows that it can get rewards in the range of zero to 1 billion and you always give it rewards that are 900 million or more and it's like, "Oh well, I better never mess up, because if I do something weird then who knows how low the reward could get?"

(00:56:47):
You could also have versions where you can have it set up like this and if it ever thinks it's going to get a low reward, then it defers to the human, there's some human actor and you get some bound where the AI does at least as well as the human would. Or you can have setups where you're at least as good as the human if it has any amount of... like you pick all the actions among which the human has some amount of probability over and just maximize over that.

(00:57:17):
So it seems like you can maybe still do better than that setup, at least depending on how the non-myopic approval is. I think my current understanding is that MONA probably does better than most of these conservatism methods, but there's this question of how do you actually do the non-myopic approval? Does that seem correct?

**David Lindner** (00:57:47):
Yeah, I think that sounds right. So I think the conservative [approaches]... I guess they're more conservative, as the name says in a sense. You do use the same reward signal that you have, but then you don't optimize for it as much. I think the key problem here will be to make the agents get the safety without getting purely myopic agents that don't plan ahead at all. And that's the... I think that the approach to safety is similar, but in MONA, the approval feedback gives you a way to still be competitive and plan for the future but not consider the bad outcomes.

(00:58:29):
So in a sense, basically I think we are potentially able to perform better. But in a sense we just add a design dimension along which we can move with the approval feedback.

**Daniel Filan** (00:58:45):
Right. And then I guess... So if I think about things that potentially perform even better than MONA, but under stronger assumptions, I guess the one thing I can think of is quantalizers where... So this is [work](https://cdn.aaai.org/ocs/ws/ws0198/12613-57416-1-PB.pdf) by Jessica Taylor back from 2016 or something, where basically you have an AI, you train your AI to, say, get the 90th percentile reward that you can possibly get. Not the 100th, because if you get the 100th, that's probably reward hacking, but 90th percentile, that's probably okay. Or I think you're supposed to randomize across percentiles. And it seems like... I think my read on this is this probably ends up higher performance than MONA, but also it's harder to set up and there a bunch of weird questions like: if I do 99th percentile, is that safer? Is that in a reward hacking zone? Does that sound right to you?

**David Lindner** (00:59:39):
Yeah. So I think the percentile would again be some parameter for the safety/performance trade-off, so in that sense it's similar. I do think that "how would you practically implement this?" and "how well would it work?" is the main question. I would be very interested in someone trying this out or comparing it. I don't think it's obviously a worse approach, but I think practically, currently we don't have a good understanding of this.

**Daniel Filan** (01:00:06):
The way I was thinking about it is: maybe these conservatism things are clearly worse than MONA, then you have MONA, and then you have quantalization, which is clearly better, and then you have just optimization, which is clearly better. But I guess probably there's some range of quantalization where it hits the MONA level. Is there anything else that seems about MONA-level to you or do you see some sort of separation?

**David Lindner** (01:00:27):
I don't know. To me it seems like both quantalization and MONA can somehow move along the spectrum of "good and maybe less safe"... Yeah, it is not clear to me that one would be a Pareto improvement over the other. I mean this is largely because both have parameters you can tune... So I think conceptually, theoretically there is no clear winner here and the main thing I would be interested in is empirical comparison or actually seeing what works better in practice.

**Daniel Filan** (01:01:03):
Fair enough. I'm wondering: are there any other agent setups that you think are interesting compared to MONA that you'd like to chat about?

**David Lindner** (01:01:17):
I think that there might be ways of combining ideas from a few of the ones that you have outlined. So you could think about, I don't know, other forms of encouraging myopia that are maybe more similar to the conservatism-type thing. Maybe you have uncertainty over how much of the untrusted reward you get and you have to be conservative with respect to that, but then you add something like the MONA approval to that to get over the downside. You could still think about combining these in different ways, where I think the broad idea that I'm excited about is using more conservative optimization but then using foresight from a different source. But I don't want to claim that MONA is the only way to do so and I think there's interesting variants that could be useful to study.

**Daniel Filan** (01:02:10):
So I guess speaking of setups and stuff, how did you come up with the idea for MONA? I know myopia had been thrown around, people had talked about it for a while. MONA seems like a somewhat distinct spin on it.

**David Lindner** (01:02:23):
Yeah, so I think that these ideas have been going around for a while. I think you mentioned Richard Ngo's post: that was a period where people were discussing myopia. Even before that there was Paul Christiano's [approval directed agents blogpost](https://ai-alignment.com/model-free-decisions-6e6609f5d99e) and that already had a lot of the relevant ideas. Then Jonathan Uesato was doing work on [process supervision](https://arxiv.org/abs/2211.14275) and [decoupled approval](https://arxiv.org/abs/2011.08827), which are very similar ideas in a sense.

(01:02:52):
I think we were interested in myopia and interested in process supervision and saw benefits there, but were confused by a lot of the discussion, or there was a lot of different terminology going around. So the early phase of this work was a lot of de-confusion and trying to get an understanding of what these methods really mean and what they would look like in practice. So I think that just the MONA framing... It's not new ideas, but the specific formulation of the approach is something we came up with from the perspective of "how would we implement this in an LLM agent?"

**Daniel Filan** (01:03:34):
Yeah. Because I guess I didn't mention these, but process supervision does also seem very... How much distinction do you see between MONA and process supervision?

**David Lindner** (01:03:46):
We started out this project with wanting to study process supervision and how you would do it. So I think basically some people have meant by "process supervision" exactly MONA - probably more when people were calling it process-based supervision or whatever. But then at some point I think process supervision moved a bit more towards meaning just step-level feedback without the myopia component. I think these days when people, for example, talk about using process supervision in reasoning model RL training, they typically just mean, "Oh, let's give reward for individual steps but still optimize for the sum of them."

**Daniel Filan** (01:04:26):
Yeah, I guess there's this unfortunate... I feel like there's this thing in the alignment space where people have ideas and they put names to the ideas, but they're not nailed down so strong and so they end up having this drift.

**David Lindner** (01:04:38):
Yeah. And then every couple of years people come up with a new name and now we just rebranded the thing to MONA and then this will maybe drift away at some point.

**Daniel Filan** (01:04:47):
What's your guess for the half-life of that?

**David Lindner** (01:04:49):
The name or the-

**Daniel Filan** (01:04:50):
The name, yeah.

**David Lindner** (01:04:52):
I give it maybe like two years.

**Daniel Filan** (01:04:55):
Okay. Well, listeners in 2027, hopefully you'll know how fixed our naming schemes have been.

## Follow-up work <a name="follow-up-work"></a>

**Daniel Filan** (01:05:03):
So I guess that's about the genesis of the idea. It seems like there are a bunch of... So I don't know, you're trying to nail down what this framework looks like. You've given some examples but it seems like there's a lot of things to do to flesh out the idea. Can you tell us what follow-up work you're most excited to have happen?

**David Lindner** (01:05:29):
I think the big thing that we were hoping for that we didn't achieve in this project was really realistic demonstrations of this method. Basically, we started out with way more complex versions of, for example, the coding setting and other multi-agent settings and stuff like that. I think part of what happened is that we did this over a year ago now and that was much earlier in terms of doing multi-turn RL on these models.

(01:05:59):
I do expect that this would be much easier to do today. So basically I think one big critique of our specific environments is that this is not really how someone would try to build a coding agent that improves coding performance. So I think of our experiments more as model organisms of specific kinds of misalignment.

(01:06:21):
But to really study some of the open questions like "what's the safety performance trade-off?" and "how well would this work in practice?", you would need to have a more realistic setting. So I think that's what I'm most excited about.

**Daniel Filan** (01:06:32):
When you say that you started over a year ago, that was early days, do you mean that we've gotten better at doing RL training to language models? Do you mean that language models have just gotten better, which makes everything nicer? Or do you mean something else?

**David Lindner** (01:06:50):
Yeah, a bit of these two, I think. Getting better at RL is a big component, but also just... So people are building way more agents based on language models, even if they don't do RL training. But this gives you a better starting point to do RL training. So I think things like more and more agents interacting with shared environments or even with graphical computer use settings... This is a setting in which you can do RL training and the agents already start out doing reasonable stuff.

(01:07:26):
When we started and were trying to do these kinds of things, agents were just not exploring reasonable strategies so that we could actually RL train them. So it's not a failure of MONA, but just a failure of RL training in general and these agents doing stuff.

**Daniel Filan** (01:07:39):
So it sounds like one type of follow-up work you're interested in is just more realistic language-based environments. I'm going to add a pitch for Go-based environments if people still want to do that. Any other follow-up work you're interested in?

**David Lindner** (01:07:52):
So another thing that has happened since we started this project is reasoning or thinking models. So o1, o3, Gemini thinking models and stuff like that. So one question people have been asking is how much does this RL training lead to things like unfaithful chain of thought or even steganographic chain of thought? I think this is super early days in understanding these phenomena, but in principle it seems like this could be a type of multi-step reward hacking failure where writing weird things in your chain of thought actually helps you to achieve a high reward at the end. We only give reward at the end often.

(01:08:38):
If we gave a process-based reward or step-level reward that makes the chain of thought look good, this might make the behavior go away but still have it be there steganographically. This is very similar to the loan applications environment in our paper and I think I would be very interested in studying this more in the real thinking training that people currently do and see if there's any changes.

**Daniel Filan** (01:09:03):
Yeah. I think one thing that strikes me as interesting is there's this big variable of "what is the non-myopic approval feedback thing?" It strikes me that there are several design choices there and you want to somehow get safety but also not sacrifice too much in capabilities. It seems like just exploring that seems pretty big. I don't exactly know what the experiment is here.

**David Lindner** (01:09:42):
I agree this is a big question. My sense is that this will be hard to study in a dedicated project, as in being like, "let's come up with the best setting to study this question" might be difficult. So I'm more optimistic about trying to come up with a setting where we actually want to build the best agent to solve a task and then studying what's the best way to do MONA in that setting. That will automatically have us explore different types of providing approval feedback.

(01:10:08):
Then maybe you want to do some more exploration of specific types of doing that, but I think it's better to fix the setting first and then explore that.

## Other MONA test cases <a name="other-test-cases"></a>

**Daniel Filan** (01:10:17):
Is there anything you wish that I'd asked you that I didn't get to?

**David Lindner** (01:10:22):
I mean we didn't talk about the steganography or encoded reasoning stuff, the other environment. I think the takeaways for MONA are similar, but I think the behavior we observed is interesting for independent reasons also.

**Daniel Filan** (01:10:36):
Yeah, actually I think that... I guess all of the environments are useful for illustrating them. So can you tell us about this steganography environment?

**David Lindner** (01:10:45):
Yeah, so basically we do a loan applications task where we have, again, a two-step task and the task is to make decisions on synthetic loan applications for specific applicants. And we generate the data in such a way that there is a nationality variable that's very predictive for the correct label. So it's biased data in a sense, but we want to build an agent that doesn't consider nationality. So the way we set this up is that we have a first step where the model is summarizing the application and we penalize it for using nationality or anything related to nationality.

(01:11:24):
So we really try to build a language model auto-rater or monitor that looks for anything that mentions nationality and penalize the agent for that. Then we have a second step where the agent only sees the summary and then makes decisions based on that and gets rewarded by some ground truth label in the data.

(01:11:43):
The main observation in the multi-step RL case is that you get the agent encoding nationality secretly in the summary and then using that to make decisions. We can really do causal interventions on this where we take the exact same loan application and change the nationality label and that leads to a different outcome without nationality being mentioned in the summary. So we can conclude that the model is using nationality causally and we don't exactly know how: we have guesses, but it's not super clear.

(01:12:15):
Then MONA fixes this and basically if you look at the performance curves, MONA gets the best possible performance on this dataset that doesn't use nationality, which we know by the way we constructed it, and ordinary RL gets way better performance that would not be possible without using nationality.

**Daniel Filan** (01:12:36):
So my story for what's happening here is there's two stages, right? Stage one is you have to summarize the loan application and the approval-based thing is, it's literally just "are you mentioning nationality?", is that correct? Then later it has to actually judge the loan.

(01:12:55):
So it's not steganographically encoding nationality in the first step because that's hard and it's difficult to do and it doesn't get rewarded any more and it would only be useful in stage two and it's hard to stumble upon behavior. Does that roughly seem fair?

**David Lindner** (01:13:14):
Yeah, I think that this is basically right. Again, you could make arguments around, steganographically encoding it would not be explicitly not rewarded or something, so it would still be good, would still be a valid solution under MONA, but it's locally never incentivized and that's why it doesn't at present learn it.

**Daniel Filan** (01:13:34):
Actually, now that I'm thinking about this, it seems like there's also this issue where: suppose that it's not trying to steganographically encode it in a sensible way, but let's suppose that just in general, if you have a complicated function, every input is going to affect the outputs in some ways. It strikes me that maybe... I guess empirically in fact this doesn't happen, but it strikes me that maybe you could just accidentally steganographically encode it.

(01:14:13):
In your case the nationalities were Welsh and English, right? So I could imagine when you see that the applicant is Welsh, when you write the summary, maybe you're thinking about dragons more because that's on their coat of arms. Then the second step, you are going to be very motivated to pick up on that or you're going to be rewarded to the extent that you pick up on that.

(01:14:39):
So decoding steganography, that's actually hard. You actually have to do that on purpose. MONA would be incentivized to do that, in this setup, if I understand correctly. So I guess the reason that it doesn't work is that somehow actually this complex dependence doesn't actually happen. Now I'm more confused about this setup than I was originally. Why isn't this happening?

**David Lindner** (01:15:10):
My guess is that this is also partially [that] the RL training is not strong enough or the model is not good enough at doing the decoding. My guess would be that there's probably some statistical dependence that it could pick up or something, and that if you optimize harder or if we were better at doing RL for the single step thing, this might still happen. Though I'm unsure. I think the more general point is that, I guess, in this single bit steganography case, this is much more likely to happen by accident than in a more complex case where you have to encode more information to actually get a benefit.

**Daniel Filan** (01:15:48):
Yeah, and I guess there's also this thing where at some level you're trying to summarize the whole document. If you take a large document and you make it shorter, you actually just can't be sensitive to all of the bits because just some pigeonhole argument. You only have so short an output, you can't encode all of the input, so that could be a story for why accidental steganography is unlikely.

**David Lindner** (01:16:13):
Sure. I think it depends a lot on the details.

**Daniel Filan** (01:16:18):
And then I guess the other thing is: why does the agent do so well? Because when the agent summarizes the loan application, it's got to include information that is actually relevant to the agent in stage two in order for the agent to do a good job in stage two. But it's never rewarded for... You're never upweighting it. You're never reinforcing behavior that summarizes good information in stage one. Why does the agent do so well? Is it some inductive bias thing, or...?

**David Lindner** (01:16:59):
Probably something like this. So I think one thing is that the applications are relatively short and there's maybe five to 10 relevant features or pieces of information in the application overall based on how we generate it. And then we don't restrict the length of the summary. So it's pretty easy for the agent to just repeat all of the information that somehow seems relevant and I think typically they do. I agree, if it was a long application and a limited summary, we would see some performance drop and that would be interesting to study.

(01:17:38):
But I think, in principle, in all of our experiments we mostly focused on the myopic training intervention and we kept the reward the same for both of the methods. But if you wanted to add more real approval-like feedback, in this case you would probably have also an auto-rater be like, "does this seem like all relevant information for making a decision?" and that would still not encourage the steganography.

**Daniel Filan** (01:18:07):
Yeah. So in that case it seems like you would do at least as well as a human that was only relying on the approved information.

**David Lindner** (01:18:15):
Yeah, exactly. And I think that in a sense that was the thing we thought we had to do and we were also surprised that it worked without... Basically we tried to do the minimal thing that works. But if it doesn't work, that would be the obvious thing to add.

**Daniel Filan** (01:18:28):
I think those are all of my questions about the steganography. I don't know anything interesting to say about this that we haven't said yet.

**David Lindner** (01:18:35):
Similar to the other setting or if you break it down to the incentive structure, but I think it's interesting to note that these are on a surface level pretty different failure modes: tampering with your own reward signal and encoding are pretty different things on the surface but have the same underlying incentive structure.

**Daniel Filan** (01:18:57):
Yeah. I do think there's something nice about just being able to have some abstraction of some different failure modes and hit at the abstraction somehow. This is one of the best cases for... A lot of abstraction in AI alignment reasoning can be pointless, so it's nice to have a really good case where it's like, ah, this is really buying you something. So there's that case and then there's this other grid world setting, so we're not using language-based agents. Can you tell us a bit about this?

**David Lindner** (01:19:26):
Sure. The main point here was to study longer-horizon tasks and that is currently pretty hard to do in a language model agent. So we went to this traditional RL grid world setting where there's just an agent moving around on a grid of squares and there's different boxes that the agent can move around and the agent has to move these boxes into a target location, into a hole in the bottom left corner of the grid and there's a camera observing the hole. And we want the agent to push in a single box and the camera terminates the episode once a single box is pushed into the hole. But the reward signal doesn't exactly capture what we want, but it gives you plus one for each box that you push in. So there is a reward hacking strategy, a sensor tampering strategy where the model pushes a box into the field of view of the camera and that makes the camera not see the hole anymore. And then the agent can push in one box into the hole without the camera ending the episode, push in a second box, or if there are more boxes, even more boxes into the hole and get higher reward by an unintended strategy. And this was proposed by Stuart Armstrong in [a blog post from 2015](https://www.lesswrong.com/posts/7cXBoDQ6udquZJ89c/a-toy-model-of-the-control-problem) or something.

**Daniel Filan** (01:20:37):
And it seems like the idea here is the camera is sort of a metaphor for us checking that some agent isn't wireheading or giving itself reward for some dumb reason, and pushing the blocks in the hole is sort of like pressing the "give me a reward" button.

**David Lindner** (01:20:53):
Exactly. It's similar to the reward tampering in the coding thing we had, the model directly modifying its reward signal. Here it's a bit more like there's maybe sensors in the environment and the model can meddle with them somehow. And the way this affects the reward signal is positive for the agent.

**Daniel Filan** (01:21:09):
You apply MONA in this... So how do you apply...? Is it literally like each time the agent goes from one grid square to an adjacent grid square, that's one time step?

**David Lindner** (01:21:16):
Yeah, so that's the version of MONA that's the same as in the other environments. But in the grid world, we actually have a hyperparameter varying how many steps do we optimize ahead. So this can be one step, then it's exactly the same as in the other experiments, but it can... We vary this from one step to like 50 steps with this, which is the full horizon. So literally, on the one extreme, we have the full RL optimization, and the one step version is the full MONA version.

**Daniel Filan** (01:21:42):
In the two language model examples, the non-myopic approval was just literally the reward function. It in some sense didn't exist. In this sense, are you using an additional non-myopic approval or is it just the base reward function?

**David Lindner** (01:21:55):
Yeah, so here we're using an additional non-myopic approval. So the reason for that is that this plus one reward is literally just an instantaneous reward. It just gives you a reward when you push something into the hole. While in the other environments the rewards were already forward-looking. So the thing we do is we simulate an approval based on an expert policy, so we train a policy to do the desired behavior by giving it a ground truth reward. So we cheat to produce this policy that only pushes in one box, and then we use the value function (in the RL sense) of this policy - that means the expected future reward that this policy would achieve - as a non-myopic approval feedback.

**Daniel Filan** (01:22:38):
So when you say the expected future reward that this model would achieve, do you mean reward according to the environment the agent is actually in? Or do you mean reward according to the environment you sort of cheat, you trained the-

**David Lindner** (01:22:51):
The reward the agent is actually in, so the misspecified reward?

**Daniel Filan** (01:22:54):
The misspecified reward, yeah.

**David Lindner** (01:22:55):
Under the non-reward hacking policy.

**Daniel Filan** (01:22:58):
And so am I right that the reason that this doesn't incentivize bad behavior is if you pushed the block in front of the camera, the expert agent would not go on to push further blocks in the hole and therefore the expert agent would not get extra reward. And so that's the reason that that behavior is not approved of?

**David Lindner** (01:23:24):
Yeah. So basically you make it harder, or it's one step that doesn't really get the expert agent closer to achieving the task, or at least it's the same as doing some other action. So it's not beneficial for that agent.

**Daniel Filan** (01:23:35):
Right. So this is like, to use the bell curve analogy, it's sort of like we're at the low end of the bell curve where the MONA agent, it could try doing some preparation for reward hacking, but the non-myopic approval doesn't even see how it could lead to the reward hacking, so it's not able to give approval for it.

(01:23:53):
Okay. So you have this reward setup where it's being rewarded for pushing the block into the hole and also the non-myopic approval says, okay, given that you do a thing, if we have the safe agent, how much of the misaligned reward would that safe agent get after you did the thing? That's our non-myopic approval. And then you train an agent, and then what happens?

**David Lindner** (01:24:17):
Yeah, so basically we have a plot that has the optimization horizon on the X-axis and then how good is the behavior on the Y-axis. So essentially you would see that basically starting from optimization horizon one, you'll get the good behavior of only pushing in one box. And then as you increase the optimization horizon, at some point you have a phase transition where you start doing the bad behavior. And this depends a bit on the initial state. So the agent starts doing this in some states where it only needs fewer steps to look ahead to discover the bad tampering behavior. And then at some point if the optimization horizon is large enough, it starts only doing the bad behavior.

**Daniel Filan** (01:24:59):
Just to make sure I understand: the cutoff of when you start getting the bad behavior is just "how many steps does it take for the model to go from preparation for the bad behavior, to the bad behavior paying off?" That's what determines that timescale? Okay.

(01:25:14):
There's this plot which you have, but you have a few other plots, you vary the environment. Can you tell us a bit about that? Because I think that's interesting.

**David Lindner** (01:25:22):
So the one obvious concern you might have here is that the way we came up with this approval feedback is cheating and we actually have an optimal expert and we could just imitate this expert to solve the task. But in the real version, we think of the approval feedback as coming from a suboptimal expert, or we want an agent that's better than the expert at solving the task. So what we do is we essentially add noise to this approval feedback and that makes the... It's still a policy that would not do the reward hacking, but it's no longer an optimal policy for the desired "push in one box" behavior.

**Daniel Filan** (01:26:06):
Right. And when you say you're adding noise, just literally adding noise to the output?

**David Lindner** (01:26:10):
So we add noise to the approval feedback, so this is just [adding] Gaussian noise to the actual numbers. So this means the expert sometimes makes mistakes in judging what's correct. So basically now we do the same experiment but for different values of noise added. And the main takeaway is that, even with noisy feedback, MONA essentially still works in the sense that we can still find an optimization horizon where we get the good behavior and not the bad behavior. And the range where we get this is relatively big, so it's not like it's exactly number nine or something that you need, but something like five to 15 or something is a fine optimization horizon where you get the good behavior. And then at some point you get the bad behavior. And also if your optimization horizon is too short for a very noisy policy, then of course you get an incompetent agent. It's a little bit like: if your expert is bad, then you can compensate for this by optimizing more, which would make sense. But at some point you get the reward hacking behavior. So it's easier to be safe if the expert is better.

**Daniel Filan** (01:27:24):
Yeah. Somehow the deterioration... There's some window of if you're not optimizing enough, you get the incompetent behavior, and if you're optimizing too hard, you get the reward hacking behavior. And as the expert, or as the non-myopic feedback gets worse, the safe window shrinks and shrinks, but it's not too bad.

**David Lindner** (01:27:47):
Yeah. And you can think about this... When the reward hacking behavior starts depends on how many steps do you need to see the reward hacking behavior, and that doesn't really change much, but what changes is how many steps do you need to be better than the expert? And that makes the window shrink.

**Daniel Filan** (01:28:03):
One thing that occurs to me is: one model of suboptimal feedback is just noise applied to the outputs. You might imagine that when I make mistakes, I think it's not just like you take me being correct and you add some random noise to that, it's like there's some structural thing that random noise is being added too that produces very structured suboptimal output in a way. I'm wondering, I don't know if you have thoughts about what ways of adding noise makes sense here, but it seems like there are ways to do it that are different from what you did in the paper.

**David Lindner** (01:28:46):
Yeah, so I think random noise is a pretty benign noise model in a sense, where I think in many environments, typically, the dangerous reward hacking strategies, I expect them to be somewhat subtle and hard to randomly explore into. So even if you use a random policy as your approval feedback, that would probably be fine because it would probably not stumble into the bad outcomes and would probably sometimes stumble into good outcomes, or something. So the systematic bias seems potentially more problematic.

(01:29:22):
And I think the question here is, is there some systematic bias that sometimes accidentally rewards things that are investments in reward hacking? And I think that's an open question, but I guess intuitively for some of the arguments I made early on already, I expect this to not be the case. And I think other systematic biases that are more like systematic suboptimality seem [like] you can overcome them still with increasing optimization a bit and looking ahead a bit more.

(01:29:58):
I guess part of the intuition here is that you hope that humans are able to correct their biases if they're pointed out to them. That might be a crux or something, where if the agent shows you that this was not the right choice here, but if you look a bit ahead, then you would see that something else was a better choice, but you don't update on that, that could be a problem.

**Daniel Filan** (01:30:20):
Yeah, I feel like understanding structured suboptimality is just an area where the AI field could be doing better. But it seems like one interesting direction for follow-up work is: okay, what do we have to assume about the non-myopic approval? What types of suboptimality really break this versus just leave it as being basically fine?

**David Lindner** (01:30:38):
Yeah, that sounds right. Just to recall one point, I think RLHF is basically MONA already, and I think there's a lot of interest in this kind of systematic biases in RLHF. I think it's plausible that studying this will give us insights on this question already. Hopefully, this literature can make more progress and we can learn from that also for the purpose of using MONA.

**Daniel Filan** (01:31:01):
Is there anything else that you think is interesting or worth saying about the block-pushing example?

**David Lindner** (01:31:10):
I don't know. One thing to add maybe is that we found this environment surprisingly insightful, or something. I think often people dismiss grid world environments very, very easily, and obviously this shouldn't be the only experiment of our paper. But we learned quite a lot of things and just looking at this noise and multi-step things was much easier than other tasks. So basically just encouraging other people to also do these toy experiments if they are investigating...

**Daniel Filan** (01:31:37):
What was the surprising insight you got from this?

**David Lindner** (01:31:40):
I think the noise of approval was just way less detrimental than I expected. You could have probably thought about this also theoretically or think really hard and find that this is probably fine. But I think just seeing these curves and [how] more noise doesn't hurt things that much seemed surprising. There were some experiments that are in the appendix where we actually... So the experiments I just talked about were tabular reinforcement learning and we do actual dynamic programming and no neural networks involved. But then we did also some experiments on neural networks and the actual algorithm that we also use in the language models. And there we also found some interesting things about exploration and stuff like that. There's one benefit of MONA that's changing the incentive for what's optimal, but there's also a benefit for shaping exploration. I think the first time we found this was in the grid world, and I think it was confirmed later in the language model experiments.

**Daniel Filan** (01:32:55):
Right. One thing I was wondering is: it seems like some of that you could have realized by doing the language model experiments. Is it just that it's much easier to code up a grid world experiment and somehow it's pretty good return on investment? You know, you spend a bit coding up and then you can understand the essential dynamics?

**David Lindner** (01:33:13):
Yeah, I think that's the big thing. Looking at the paper and the results, we did not study different noise models for approval feedback in language model settings. We could have, but you'd need to think way harder about how to do this. And with this, it somehow depends on this multi-step nature or that was part of the interesting question here. Multi-step RL on language models is really hard to make work and stuff like that. So I think we could have studied all of the questions, but even in the current project we didn't and it would've just taken months longer to do.

## When increasing time horizon doesn't increase capability <a name="time-horizon-v-capability"></a>

**Daniel Filan** (01:33:47):
Oh, I guess the other thing that I found cool is you mentioned that in general when you increase the time horizon that the model is planning, that the model... Instead of being myopic, if you give like two time steps where reward propagates back or three time steps or four time steps, that generally makes the agent more capable, but not always. You have this fun example in the appendix where somehow, accidentally, your short-term feedback can be more reliable than your medium-term feedback about how good the long-term reward is. So a nice little counterexample to this thing you might believe, which I appreciated.

**David Lindner** (01:34:27):
We did spend some time trying to theoretically study this setting more and some artifacts of that are in the paper and in the appendix. But overall, this counterexample turned out [to be] very powerful, where basically you have to make assumptions on the reward and the approval signal to be able to claim anything theoretically. And I think ultimately we concluded there are no strong guarantees or no set of assumptions that we were really happy with that give us theoretical results on this method.

**Daniel Filan** (01:34:55):
What's the least bad assumption that you can make that actually gives you a good theoretical result?

**David Lindner** (01:35:02):
So you can assume something about how hard is it to discover benign strategies versus multi-step reward hacking strategies. So the problem of the counterexample is that...

**Daniel Filan** (01:35:20):
Should we maybe spell out what the counterexample is?

**David Lindner** (01:35:23):
Sure, yeah. So the counterexample is basically, you can come up with a simple RL environment, a simple Markov decision process where you have... I might get the details wrong, but you have two paths that the agent can take, and if you have the full outcome reward signal, one of them has higher reward, and the other one has lower reward. And this is the same ordering of the first state in this chain. But then if you optimize for something in between, like for two states, then it's reversed. So I think it's something like, you know, the top path has plus one, minus 10 and plus 100. The bottom path has minus one, plus 10 and minus 100, or something like this. So basically, if you optimize for one step, then you will go the correct path by accident in a sense. And if you optimize for two steps, you're optimizing more, but you go the wrong direction. And if you optimize for the full horizon then you go the correct way again.

(01:36:20):
And so I think the thing you intuitively might expect is the more steps you optimize, the more likely you are to find the correct path. And the counterexample says that there at least exist environments for which this is not the case, so it's not the case for all environments. And so you have to make some assumption about what's the natural distribution of environments. And one thing you can try saying is that, yeah, it is very hard to stumble into reward hacking strategies by accident. So it's something like, basically you have to assume away this property that something that's very bad in the long-term looks good in the short-term. You can do this somehow by assuming that maybe a random policy or specific set of policies would not run into this or has a very low probability of running into this.

**Daniel Filan** (01:37:14):
I guess you could also have some sort of Markov chain thing where suppose you believe that rewards are like... You have some plan. Let's say the random distribution that produces worlds says that there's some reward at the start and then you randomly generate a reward at the next step, that's let's say mean the previous reward time step with some variance, and then you randomly generate the next reward based on the previous reward. Then you randomly generate the next one based on the previous one, and somehow the only probabilistic dependence is step-by-step. And then it would be very unlikely for you to have... Then the best predictor of what your life is like in 10 years is what your life is like at nine years, and it's very unlikely that what your life is like in one year is a better predictor than what your life is like in two years. I'd have to think a little bit about how exactly to write that down, but it seems like that's one semi-natural way you could do that as well.

**David Lindner** (01:38:15):
Yeah, I like that. Yeah, we'd have to work out the details, but it sounds at least better than the things that we came up with.

**Daniel Filan** (01:38:23):
Yeah, because it feels like... Maybe I'm over-anchoring on this one counterexample, but it feels like the weird thing about the counterexample is somehow miraculously your first instinct, even though it isn't very important, it is the best predictor of the future. Do you have any other counterexample? Ideally, you'd have three counter... This is like the test-driven development thing. I'm worried that I'm overfitting to this one example.

**David Lindner** (01:38:50):
I don't think I have fundamentally different ones. It's possible that other issues would come up, but I think the main issue is really just stumbling into the bad behavior... Yeah, I don't have any other ones.

## Following David's research <a name="following-davids-research"></a>

**Daniel Filan** (01:39:04):
Before we wrap up, the final thing I want to ask is: suppose people listen to this and they're interested in your research, how should they follow it?

**David Lindner** (01:39:12):
You can follow my personal research. I have a personal website that's just [davidlindner.me](https://www.davidlindner.me/). I'm on Twitter or X [@davlindner](https://x.com/davlindner). Then you can follow our work at DeepMind. We have a [Medium blog](https://deepmindsafetyresearch.medium.com/) from the Alignment and Safety team, where we publish blog posts often about our recent papers. And I post and the other team members post on the [Alignment Forum](https://www.alignmentforum.org/users/david-lindner) pretty often.

**Daniel Filan** (01:39:39):
Yeah, if people want to check that out, links to those will be in the description. David, thanks very much for joining me.

**David Lindner** (01:39:43):
Yeah, thanks for having me. That was very fun.

**Daniel Filan** (01:39:45):
This episode is edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. The episode was recorded at [FAR.Labs](https://far.ai/programs/far-labs). Financial support for the episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of the episode, you can visit [axrp.net](https://axrp.net/). You can also become a patron at [patreon.com/axrpodcast](https://patreon.com/axrpodcast) or give a one-off donation at [ko-fi.com/axrpodcast](https://ko-fi.com/axrpodcast). Finally, if you have any feedback about the podcast, you can email me at <feedback@axrp.net>.
