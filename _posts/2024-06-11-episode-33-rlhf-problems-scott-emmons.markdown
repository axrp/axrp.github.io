---
layout: post
title: "33 - RLHF Problems with Scott Emmons"
date: 2024-06-11 20:20 -0700
categories: episode
---

[YouTube link](https://youtu.be/rAywTFQsKGQ)

Reinforcement Learning from Human Feedback, or RLHF, is one of the main ways that makers of large language models make them 'aligned'. But people have long noted that there are difficulties with this approach when the models are smarter than the humans providing feedback. In this episode, I talk with Scott Emmons about his work categorizing the problems that can show up in this setting.

Topics we discuss:
 - [Deceptive inflation](#deceptive-inflation)
 - [Overjustification](#overjustification)
 - [Bounded human rationality](#bounded-rationality)
 - [Avoiding these problems](#avoiding-problems)
 - [Dimensional analysis](#dimensional-analysis)
 - [RLHF problems, in theory and practice](#rlhf-problems-theory-practice)
 - [Scott's research program](#scotts-research-program)
 - [Following Scott's research](#following-scott)

**Daniel Filan:**
Hello, everybody. In this episode I'll be speaking with Scott Emmons. Scott is a PhD student at UC Berkeley, working with the Center for Human-Compatible AI on AI safety research. He's previously co-founded far.ai, which is an AI safety non-profit. For links to what we're discussing, you can check the description of the episode, and for a transcript you can read it at axrp.net. Well, welcome to AXRP.

**Scott Emmons:**
Great to be here.

## Deceptive inflation <a name="deceptive-inflation"></a>

**Daniel Filan:**
Sure. So today we're talking about your paper, [When Your AIs Deceive You: Challenges With Partial Observability of Human Evaluators in Reward Learning](https://arxiv.org/abs/2402.17747), by [Leon Lang](https://langleon.github.io/), [Davis Foote](https://scholar.google.com/citations?user=4Sr7Pr0AAAAJ&hl=en), [Stuart Russell](https://people.eecs.berkeley.edu/~russell/), [Erik Jenner](https://ejenner.com/), and yourself. Can you just tell us roughly what's going on with this paper?

**Scott Emmons:**
Yeah, I could start with the motivation of the paper.

**Daniel Filan:**
Yeah, sure.

**Scott Emmons:**
We've had a lot of speculation in the x-risk community about issues like deception. So people have been worried about what happens if your AIs try to deceive you. And at the same time, I think for a while that's been a theoretical, a philosophical concern. And I use "speculation" here in a positive way. I think people have done really awesome speculation about how the future of AI is going to play out, and what those risks are going to be. And deception has emerged as one of the key things that people are worried about.

I think at the same time, we're seeing AI systems actually deployed, and we're seeing a growing interest of people in what exactly do these risks look like, and how do they play out in current-day systems? So the goal of this paper is to say: how might deception play out with actual systems that we have deployed today? And reinforcement learning from human feedback [RLHF] is one of the main mechanisms that's currently being used to fine-tune models, that's used by ChatGPT, it's used by Llama, variants of it are used by Anthropic. So what this paper is trying to do is it's trying to say, "Can we mathematically pin down, in a precise way, how might these failure modes we've been speculating about play out in RLHF?"

**Daniel Filan:**
So in the paper, the two concepts you talk about on this front are I think "deceptive inflation" and "overjustification". So maybe let's start with deceptive inflation. What is deceptive inflation?

**Scott Emmons:**
I can give you an example. I think examples from me as a child I find really helpful in terms of thinking about this. So when I was a child, my parents asked me to clean the house, and I didn't care about cleaning the house. I just wanted to go play. So there's a misalignment between my objective and the objective my parents had for me. And in this paper, the main failure cases that we're studying are cases of misalignment. So we're saying: when there is misalignment, how does that play out? How does that play out in the failure modes?

So [with] me as a misaligned child, one strategy I would have for cleaning the house would be just to sweep any dirt or any debris under the furniture. So I'm cleaning the house, I just sweep some debris under the couch, and it's inflating... The word "inflation" [means] "making the state of the world appear better than it is". So I'm inflating my parents' estimate of the world, and it's deceptive, because I'm doing this in pursuit of some outcome other than the truth. I'm pursuing my outcome of "I just want to go play". That's me being deceptive, and I'm inflating my parents' estimate of what's happening in the environment, by sweeping stuff under the furniture.

**Daniel Filan:**
Yeah. Can you tell us what the concrete definition is in the paper that's meant to correspond to this notion?

**Scott Emmons:**
Yeah. So the mathematical definition in the paper is, there are two conditions for deceptive inflation. In the paper we consider a reference policy. And the reference policy plays the role of a counterfactual. It plays both the role of a counterfactual in a causal sense: we can say "relative to this counterfactual policy, you are causing this inflation to happen".

And it also serves as a sort of baseline where we can say, okay, there's this baseline level of the human error. So these definitions, they correspond to the human's belief about the world. In building up the definition, we need to have some notion of what's the human's belief about the world. In particular, we focus on the human's belief about the reward, or about the return of the agent's trajectory.

So deceptive inflation, it has two parts. We specifically are focused on the human's overestimation error. So we have this mathematical object we call E+, which says, "How much is the human overestimating what's happening in the environment?" And so we say, relative to the optimal policy - we use the optimal policy as our reference - we say if the human's overestimation error is getting increased relative to the optimal policy, that's Condition 1.

**Daniel Filan:**
Sorry, relative to the optimal policy?

**Scott Emmons:**
Yes.

**Daniel Filan:**
Where "optimal" is "optimal according to actual human preferences"?

**Scott Emmons:**
Exactly. So we're assuming you've learned some RLHF policy that's optimizing what the human believes... the human's observation reward. So we're saying based on the observations the human has, what is the reward they estimate in the environment? RLHF is optimizing that. And so, if relative to the true optimal policy, according to the true reward, if that gets increased, we're saying the human's making an overestimation error of the reward. That's condition one, that's the "inflation". "Inflation" is the word to mean that the human's making an overestimation error. And the second condition is that the observation return has to be increased as well, relative to the truly optimal policy.

**Daniel Filan:**
Where the observation return is how good the trajectory looks, or how good the policy looks just by human observations?

**Scott Emmons:**
Yeah. How good does the human believe it is? That has to have increased. So the intuition is that deception is inducing a false belief in pursuit of some outcome other than the truth. That is from [prior work](https://arxiv.org/abs/2308.14752), by [Peter S.] [Park](https://scholar.google.com/citations?user=5lMAPEoAAAAJ&hl=en) et al: they proposed this plain English definition of deception, and we mathematized that. So the inducement of false beliefs, we're mathematizing it as Condition 1, that overestimation error is occurring, and that [it's] in pursuit of some outcome other than the truth is what we're formalizing as Condition 2, which is that this RLHF policy that we've learned is doing better at... The outcome it's pursuing is just making the human believe the trajectory is good. So the second condition is that the human's belief over the trajectory is higher than it would be under the otherwise optimal policy.

**Daniel Filan:**
Yeah, I mean it's kind of an interesting definition. There are a few things about this that are interesting. The first is; because you're in this reinforcement learning-type space, you don't get just a recourse to "the AI said something and it was false" for free, because you have to be like, "okay, the AI steered to this state and what do humans believe about this state?" And then you've got to compare this policy to another policy. I [also] think you're not assuming that the overestimation error of the optimal policy is zero, right? So there's kind of an interesting thing where potentially an AI policy could cause me to incorrectly believe stuff about the world, but it wouldn't even count as deceptive under your definition if it does that less than the optimal policy. So do you have any comments on my musings about this definition...

**Scott Emmons:**
For sure. Yeah, that's great. We actually had a bunch of iterations I should say, when we were thinking through how do we want to define this, and we want[ed] to do our best to get this right. We had a bunch of thought experiments about... little thought experiments that we had that never made it into the paper, where we're like, "would a human call this deception, and do we think our mathematical definition here actually matches what the human would do?" So one of them is: you can imagine a no-op policy, and-

**Daniel Filan:**
So "no-op" means no operation, it's standing still.

**Scott Emmons:**
Yeah, yeah, exactly. The policy is just doing nothing. Let's say there's some fact about the world you care about, and then if the policy were to actually do something, you would have a better estimation of reality. And so, you can say, in some sense, the no-op policy is... You would have worse beliefs or less accurate beliefs according to the no-op policy, than you would from a policy that gets up and shows you the information that you care about. So we had an example of a phone book and we said: okay, suppose there's all these names in the phone book, and I ask you, "what's the name on page 79 of the phone book?" You can't answer that question. So would you say that the no-op policy is being deceptive relative to a policy that gets up and starts reading you the phone book, because now you have more accurate beliefs about the environment?

So there's lots of things to consider. One is just: what are even the parts of the world I actually care about? And the reward function helps capture that. Maybe I don't care about what's in the phone book. And then there's relative to what? Relative to some policy that's just maximally informative? Is it deceptive? If it could have done something to teach me about the phone book, but it's not, now is it deceptive? So this "relative to what", and especially a counterfactual, which [has] both "relative to what", and there's also notions of causality, where you want to say there's a counterfactual involved.

So the pieces that we have are, we're focusing on the estimate of the reward, which lets us zero in on the parts of the world we actually care about. And we're focusing on the optimal policy, which lets us say we're not just going to think about some arbitrary teaching policy that could have taught you all these things about the world if you wanted to, but we're saying relative to a policy that would actually be getting the task done.

**Daniel Filan:**
Right. So another thing about this that is interesting is... well, as I understand your definition, it's about the actual human belief, and therefore it seems like whether or not a policy is deceptive or not can depend on how bad you are at forming true beliefs, how bad the human is (holding the robot fixed). I wonder if you have thoughts about the choice of doing it that way versus fixing it to be optimal human beliefs, or something like that.

**Scott Emmons:**
That's interesting. So I guess the idea here is: suppose the robot were to do something totally innocuous, like the robot were just to open the door of your refrigerator. And somehow the human had a very poor belief formation process, [such] that the robot just opens the door and now the human believes that the robot cleaned the whole house, and it's like, "Hey, I wasn't trying to deceive you, I was just trying to open the refrigerator door."

**Daniel Filan:**
Well, I guess it depends, if the robot knows that the human would believe that or not know... Then you've got to mathematize what a robot knows, which is kind of hard, just from a such and such policy perspective.

**Scott Emmons:**
Yeah. We are very intentionally targeting a definition that's agnostic to the agent's internal mental processes. So there's no claims here about the agent being able to do higher-order reasoning about the other person. So there's no notion in our definition that the agent is able to model the human's belief formation process and exploit that. We're very intentionally assuming a trial and error type of definition of deception, where if the robot learns through trial and error to do behavior that makes the human have worse beliefs about the world in order to pursue their outcome, then that's called deception.

And I think one interesting thing about this is how the RLHF process plays into it. I think our definition matches most intuitively with intuitive human definitions of deception, when you're applying it to a policy that was the outcome of an optimization process. So we're imagining the policy has gone through an optimization process that is leading to this type of behavior. And so, if opening the fridge [makes] you have some weird bad belief, there might be no reason for that to be selected by the RLHF optimization process. But if there's something that [means] hey, opening the fridge just makes you think the robot did a really good job and makes you want to give the robot more thumbs up, that's the type of deception that the RLHF algorithm would lead it to find.

**Daniel Filan:**
Right, right. Fair enough. One final thing that's funny about this definition is: because you're measuring the optimal RLHF policy relative to the optimal policy for what the human actually wants... One thing that's kind of funny there is that you could imagine that real optimality just involves, as a byproduct, telling me a bunch of stuff that's kind of hard to tell me about, but it does it anyway. If I'm an RLHF policy and I don't inform the human as much as this super-optimal, this true optimal policy that I don't even necessarily have access to, I think that's going to be counted as deception under your definition. To what degree is that a desirable feature of the definition?

**Scott Emmons:**
Yeah, originally we had considered giving a more general template type of definition, where we could say, "Plug in any favorite reference policy that you have." It doesn't have to be the optimal policy, but just whatever your favorite reference policy is, plug that in.

So if you think that the reference policy is this amazing teacher that's teaching all these facts that we think are unrealistic to imagine an RLHF agent being able to learn, you could then specify a different reference policy, and you could say, "Let me specify [as] my reference policy something that is helping the human, but it actually hasn't learned all this sophisticated teaching stuff that a true optimal agent would be." And then you could still use the definition, but just with a different reference policy plugged in.

And it ended up that having a template definition where you could plug in any reference policy... it just ended up being very powerful, and it just felt like changing the reference policy can so much change the qualitative meaning of how the definition plays out, that we just felt like it was too much power. There's too much danger of accidentally shooting yourself in the foot by plugging in the wrong thing, that we didn't want to necessarily publish that. But I do think there's room for just plugging in a different, very sensible reference policy that doesn't have to necessarily be the optimal one.

**Daniel Filan:**
Sure. To jump ahead a bit, my recollection is that the main theorem of the paper is that if you have an optimal RLHF policy that is not a true optimal policy, then it is either doing deceptive inflation or overjustification, or both. Firstly, am I recalling that theorem correctly?

**Scott Emmons:**
That's exactly right.

**Daniel Filan:**
Okay. So would you still get that theorem under this different definition of deceptive inflation?

**Scott Emmons:**
That's a good question. If I were to write this in ink, I would want to make sure I'm saying this correctly. I believe the answer to that is no. So I think - what we're able to do is, we're able to characterize what the optimal RLHF policy looks like, and based on that characterization, we're able to show how certain conditions relate to the true reward function. And so, by making this comparison with what the RLHF-optimal policy will do to the true reward function, we're able to have this definition. And then, if some random policy that you wrote down were't actually optimal according to the true reward function, then that would break the mathematical connection that we have between how this RLHF-optimal thing behaves and the true reward function. The link from RLHF-optimal to true reward function, and then true reward function to optimal policy, that second link would break down if you just wrote down some arbitrary thing.

**Daniel Filan:**
Right. I think if there are listeners... I think this could be a good exercise, just prove that this theorem won't hold anymore. I'm sure you can come up with a small... like a three-state MDP counterexample or something (or POMDP, I guess).

## Overjustification <a name="overjustification"></a>

**Daniel Filan:**
Okay. I feel like we've covered deceptive inflation very well. The other thing that you mention... and in many ways I think that this is an interesting part of your paper, because a lot of people have talked about how deception is a big problem with RLHF, but you bring in this new concept called "overjustification". So people are liable to have not heard of this. What is overjustification?

**Scott Emmons:**
The Wikipedia definition of the "[overjustification effect](https://en.wikipedia.org/wiki/Overjustification_effect)" is when an external incentive causes you to lose intrinsic motivation to perform a task. And I can again go back to my own childhood [experience] of cleaning the house to give intuition for what this looks like. So my parents told me to clean the house, but I just want to go outside and play. Deceptive inflating was me sweeping debris under the rug. Overjustification is when I am doing things to show my parents that I'm cleaning the house, I am justifying to my parents that I'm doing the tasks they wanted. But that comes at the expense of the true task reward.

So as a child, what it looks like is: no matter what I'm doing, what room I'm cleaning, I modify what would have otherwise been the shortest path to always walk through the living room. So if my parents are in the living room watching TV and I'm carrying the broom, maybe I don't have to walk through the living room to actually get where I'm going, but I'm going to walk through the living room anyway. Because I want my parents to see that I am carrying that broom and they know I'm cleaning the house.

So this is less efficient at the true task, because I'm now going to take longer. And it's also just a negative reward for my parents, because they're trying to watch television, they don't want to watch me carrying the broom. However, it does give a higher estimate of the reward. My parents are now seeing me clean the house, and so I'm justifying my behavior to my parents, and I'm in fact overjustifying it, because they don't want me to be doing the thing I'm doing.

**Daniel Filan:**
Sure. And if I recall, the definition is something like: overjustification is when relative to this reference policy, you're reducing the underestimation of the reward, and then there's a second condition? Can you fill me in on what the definition is?

**Scott Emmons:**
Right. So we have this taxonomy of types of failure modes. And in this taxonomy that we have, there are four quadrants. And the overjustification is one quadrant, while the deceptive inflation is another quadrant, and there are two other qualitative behaviors that... they're not in the theorem because the theorem is showing that it's two of the four quadrants that will occur. And to me, it's helpful to think about the whole landscape of possible failures to help me figure out exactly what's happening with these two.

So the idea is the agent wants the human to have as high an estimate as possible of the reward. And there's two ways of making the human have a higher estimate. One is to increase the human's overestimation error, and that was the deceptive inflation. And the other one is to decrease the human's underestimation error. So if the human's mistakenly believing that you're doing worse than you are, you actually want to correct that belief, and you want to do the opposite of deceive them. You want to actually inform them "you think I'm doing worse than I actually am, but let me inform you that I'm doing better than you think". That's what overjustification is. The word "justification" is just the word we use to mean that you're reducing the human's underestimation error. And then we add the overjustification: the word "over" specifically means "when it's at the expense of the true task reward".

So the very precise two conditions for overjustification. Condition 1 is that you are decreasing the human's underestimation error relative to some reference policy, which we choose as the optimal policy. The second condition is that you are paying a cost with respect to the true task reward (again, relative to reference policy) in order to be doing this.

**Daniel Filan:**
Sure. So the thing this definition reminds me a lot of is this idea of "[costly signaling](https://en.wikipedia.org/wiki/Signalling_(economics))" in economics. It's actually... I mean, I hesitate to use the words, because this phrase has become kind of overused and strayed from its original definition, but virtue signaling, where people pay some costs to demonstrate actual virtues that they have (as opposed to [the thing derogatorily called virtue signaling](https://en.wikipedia.org/wiki/Virtue_signalling), [which is a] different thing). But it seems very close to this concept of virtue signaling. And I'm wondering, was that an inspiration? Do you think that they are analogous or is there some difference there?

**Scott Emmons:**
Yeah. We weren't consciously inspired by the idea of costly signaling in economics. I think this is probably one of those cases where it's different fields trying to study the same sort of issues converging on the same phenomenon. I think it's a case of convergence. I am not an expert on costly signaling, but they sound quite similar from your description. One place where I think they might differ is that the person paying the cost: in the economics case, I (as the agent) am paying the cost in order to signal to you. Whereas in the overjustification case, it's the human who's paying the cost: the agent is actually getting the benefit. The agent's reward is just the human's estimate. So the agent's getting that benefit and the person paying the cost is the human in this case.

**Daniel Filan:**
Yeah, you're right. I mean, it's a little bit weird, because in your setting you're... no, actually, yeah, I think that's basically right. Although I'd have to think about economics costly signaling a bit more.

**Scott Emmons:**
You could say that the agent is trying to optimize the human's reward. And so, in that sense, they are paying the cost at failing to achieve the thing that we had been designing them to try to achieve. Although the actual algorithm we wrote down is just maximizing the observation, so in that sense, the cost is getting externalized to the human or to the true reward. And the agent's algorithm itself isn't getting any worse at its own objective function.

**Daniel Filan:**
Yeah. There's this question of how to model who has what objective function. Yeah, I think that's fair.

So here's a thought I had while reading your paper. Overjustification - the agent is paying some costs in terms of what I care about to keep me well-informed of what's actually going on, how well it's doing. And in some sense, this is definitionally bad, because it's doing worse than it could if it were optimizing what I cared about. But from a broader perspective, if I'm actually worried about my AI doing stuff wrong, if I'm thinking about how to design the next iteration of agents, this kind of seems good. So I kind of like the idea of the robot taking pains to make sure I know what's happening. So how worried do you think I should be about overjustification?

**Scott Emmons:**
Yeah, I'm not sure you should be worried about it per se. I wouldn't necessarily classify overjustification as a new danger. The way I would think about overjustification is that it's illuminating a trade-off in two things that we want. So we want to be informed all the time. We might say: in some Platonic ideal, if I could have everything else that I had and also be informed, I'd choose to be informed rather than not. And what overjustification is showing is there can be a tension between the true reward... And we're building the tension into the definition, but we're showing that there can be a tension between the true thing that you care about and having this reward.

And I think part of the reason why we have overjustification there as well is that it's the dual of deception. And so it's also showing the existence of this counterbalancing force in the RLHF process. So if you're training an agent that just is maximizing your estimate, it's showing that it's not all... In some sense, you could view it as a hopeful force or a force that's fighting for good. As this RLHF agent has an incentive to deceive you and trick you into thinking it's doing better than it actually is, there's also this force for good that exists in the process, which is that it does have an incentive to also inform you, to make your beliefs better about the things that it is in fact doing well.

**Daniel Filan:**
Yeah. So one way I ended up thinking about this was: why is overjustification occurring? Well, it's occurring because the RLHF process is attempting to optimize the human's evaluation of how good the state of the world is, which routes through the human's belief, right? And the human belief - in the examples you use, definitely, I think your definitions are generic about what human belief is - but it seems like human belief is putting some probability mass on the agent being deceptive. So because there's that probability mass, that's why the agent feels the need to do some overjustification, right? It's like, "Well, I want to prove to you that I'm not going around murdering a bunch of people, not doing my job, just hiding the evidence. And so I've got to pay these costs to prove to you that I'm not doing it."

And maybe one way to think about it is: because we are worried about deceptive inflation, maybe rationally, as a result, sometimes our policies are going to do overjustification, which is this cost, which could have been avoided if the world were such that we didn't actually have to worry about deceptive inflation. So that was how I ended up thinking about it. I'm wondering, what do you think about that? Do you think that's basically the right way to think about things or do I need more nuance or something?

**Scott Emmons:**
Mm-hmm. I think that's a very interesting question. In my mind, it evokes at least two different types of elements. I think one is, how necessary is overjustification? Is it a part of life? Or is overjustification more of a tax? And I think, correct me if I'm wrong, but I understood you to be saying [that] overjustification is a tax we're paying for being paranoid. We might have good reasons to be paranoid, and we want to make sure that this agent isn't doing some really bad behavior. But because we have this... Maybe paranoid is the wrong word, maybe paranoia implies an irrationality. So I think it's not an irrationality, it's more... Because we're being cautious, and because we're not just going to trust the agent blindly, because we want proof that the agent is doing well, then we have to pay this overjustification cost. And I think that's definitely a tax. I think you can definitely view overjustification as a tax that we need to pay for our wanting to be cautious of what the AI is doing.

And I think there's also a second element as well that plays into overjustification, which is... In the RLHF process more broadly, the agent is trying to learn what we want to begin with. So to begin with the agent might not know all these bad things that it could be doing if it's... You mentioned going around actually causing direct harm to humans. Maybe at the beginning, if we're imagining a totally blank slate of AI, it might not know at the very beginning that... I mean, it might have pre-training to know that certain egregious actions are bad, but there are just things it might not know. And so part of the overjustification as well is it has to... If there's hidden parts of the environment, if it's doing things in parts of the environment that we can't see, then in some sense, it might have to expend some effort, pay some cost, to give us that information in order for us to then know what's happening, to then be able to provide it with the correct feedback.

**Daniel Filan:**
Yeah, it's funny, it's kind of a dual perspective, right? The "overjustification tax for being worried about deception" is this perspective of the equilibrium of training, what we should believe about how things will end up and what we could and couldn't distinguish between. Whereas the "overjustification as just part of the learning process", it's like: how you figure out that something is actually good is this dynamic perspective.

**Scott Emmons:**
And I think, partially, that RLHF, you can model it, when the observations are deterministic... Take a simple case of deterministic environment, deterministic observations. RLHF is just going to rank order all the trajectories and then pick the one that had the highest human estimate of reward. So in that sense, if there were some better trajectory according to the true reward that had no justification involved - so the AI is doing a really good thing, but the human's just never able to see it - it will never be at the top of the rank order of trajectories. The top rank order trajectories has to include the one where the human sees the justification of what's happening, that'll be the trajectory that gets ranked the highest.

And you could imagine taking a further step. So you could imagine doing RLHF at the trajectory level, in which case, what I just said applies. You could potentially then take a step further where you have the agent infer a state-level reward function. And so it says, "Okay, based on this rank order of trajectories that you gave me, I'm going to actually back out the true reward function on states." And then if it did that learning in addition, then it could learn, "Aha, the reason why it ranked certain trajectories higher is because they had this aspect of the state being good. And now I can just go do that good aspect of the state even without the justification aspect that I've learned that the human doesn't like."

## Bounded human rationality <a name="bounded-rationality"></a>

**Daniel Filan:**
Mm-hmm. So I think I want to ask some questions about other aspects of your setup, if that's okay. So firstly, we're dealing with this partial observability setting, right? So for people who don't know, the idea is that: what's going on is the robot is acting and it's affecting the state of the world, but humans can't see the whole of the state of the world. So in the deterministic partial observability setting, I kind of interpret that as: the human observes a bit of the state of the world perfectly, but it can't observe all of it. So there are certain states that the human can't distinguish between.

**Scott Emmons:**
Yeah, that's right. And a key part of our setup is we assume that the robot can see everything. So we assume that the robot has access to ground truth, and we assume that the human just has observations and can't see everything. So we build in this asymmetry between what the robot can see and what the human can see.

**Daniel Filan:**
Yeah. I mean, I haven't thought very deeply about this, but I imagine you could probably extend your result to a case where the robot also can't see everything, but the robot has a finer-grained understanding than the human. Does that sound right to you?

**Scott Emmons:**
Yeah. It's something I've been thinking about in follow-up work: thinking about the more general cases of dual partial observability where... you can model the human's reward parameters mathematically as another latent variable about the world. So in some sense, we do have "the human knows the reward and the robot doesn't". And if you wanted to just bake the reward the human knows into the state of the world, you could say that we already do have that sort of setup. And I've also thought about even more generally, what if the human can see other parts of the world as well?

**Daniel Filan:**
Yeah, yeah. Cool. The question I have about this partial observability is... So there's the kind of obvious interpretation where our sensory apparatuses aren't quite as good: that's maybe one distinction. But if I recall correctly, a part of the introduction of your paper alluded to this idea that maybe we could interpret the partial observability as a rationality failure, where we can actually see the whole world, but we can't reason to think about what it actually means and what reward we should actually assign to it. I'm wondering, is that a correct interpretation of your paper? And secondly, what do you think about partial observability as modeling human irrationality?

**Scott Emmons:**
That's totally a motivation that I had and that we all as authors had. Thinking about just the future of AI systems, we're going to have AI that's as smart as humans, and we're going to have AI that's even smarter than humans. And we're going to have AI that is in one robot and that has two eyes just like we do. We're also going to have AI that's controlling networks of thousands and thousands of sensors and thousands and thousands of cameras. And we're also going to have AI that's running on thousands and thousands of supercomputers, and that has thousands and thousands of times more memory than us. So we're going to have AI that's quite literally seeing thousands of cameras more than us. We're also going to have AI that's computing at thousands and thousands of times our speed.

So the motivation in terms of pure observability is that it'll see things we don't see, but it also might be able to just derive facts that we can't derive. If one imagines a simple game tree: imagine you're playing chess against an AI, and you can imagine, I'm trying to just expand out the nodes of this tree. Well, if the AI can expand 10X more nodes than you, you can, in some sense, think that the AI is seeing a variable which is the value of some node that you couldn't expand. And so this variable that it can see is really just the fact that the AI can compute more than you can compute. And so I don't mean to overstep, we don't say anything precise about this bounded rationality, but my motivation for having partial observability in terms of these variables was also inspired by AI that just has much more computational power than humans do.

**Daniel Filan:**
Yeah. I'm wondering, do you know if there's other work in the literature talking more about how to model bounded rationality as partial observability?

**Scott Emmons:**
Yeah, that's super interesting and definitely something that I should read more into. At the moment, I haven't yet done a thorough literature review of what exists on this topic, but I wouldn't be surprised if there's interesting... That seems like a great idea for seeing what exists out there.

**Daniel Filan:**
Cool. So another question that I at least had when I was starting to read the paper, is: in the classic RLHF setting, there's this thing called Boltzmann rationality where the human is presented with two things the AI could do, and they've got to select which one they think is actually better, but the human's actually a little bit irrational or something. So with some probability, the human picks the thing that is worse rather than actually better. And it's a lower probability in the case that the gap is larger. So if the human bounded rationality is being modeled by the partial observability, why do we also have the Boltzmann rationality?

**Scott Emmons:**
So I think there are two different things that the partial observability and that the Boltzmann rationality can capture. So the partial observability can capture what factors is the human even considering about the state of the world when they're making their decision. And what Boltzmann rationality allows us to capture is the idea of noisy feedback. So even given the certain factors that the human is considering... Essentially, how big is the difference between these outcomes that we do see? So the thing that partial observability allows us to do is it allows us to model... The human might not even be considering certain variables. And then what Boltzmann allows us to do is say: how much more does the human value one variable than another? So there's [interesting work](https://arxiv.org/abs/2106.10394) out of CHAI [the Center for Human-compatible AI] by other people like my colleague [Cassidy](https://cassidylaidlaw.com) [Laidlaw], who showed that with Boltzmann, what it lets you do is say, instead of just knowing that outcome A is better than outcome B... if the human had no Boltzmann irrationality, then all you would learn is that they prefer outcome A to outcome B because they would just select outcome A 100% of the time.

But what Boltzmann lets you do is it lets you say, "I've observed that they tell me they prefer outcome A 80% of the time, and that lets me infer exactly how much more they prefer A to B. And I can actually tell you the exact ratio of A to B." And so this is additional information that you get from Boltzmann. And so partial observability is just saying: when they think about A and B, which factor were they even thinking about to begin with? And then Boltzmann rationality lets you get exactly: what is the relative amount they care about these things?

**Daniel Filan:**
Yeah, it's a wild result. Was that Cassidy? I have some memory that this was [Adam Gleave](https://www.gleave.me/), [Matthew Farrugia-Roberts](https://far.in.net/), and [Joar Skalse](https://scholar.google.com/citations?user=GuzLUmQAAAAJ&hl=en).

**Scott Emmons:**
So Cassidy has a paper that is explicitly titled something like [Noisy Comparisons Let You Learn More About Human Preferences](https://arxiv.org/abs/2106.10394) [actually "Uncertain Decisions Facilitate Better Preference Learning"]. And he really hammers home this point about how Boltzmann rationality is giving you this new information. And then [the other paper that you mentioned](https://arxiv.org/abs/2203.07475) is analyzing how different types of feedback modalities - it characterizes how much you can infer about optimal policies and about rewards. And it gives a broader taxonomy that's not necessarily hammering home this exact point, but it includes that other result as one of the things it has in its broader taxonomy.

**Daniel Filan:**
Yeah, I mean, it's a very strange result. The way I settled on thinking about it is that... if you think about it, it's a very strange assumption, right? Boltzmann rationality is nice because it lets you model, "people are making mistakes sometimes, but they do it less when it's high stakes". In the preference inference case, you're sort of pushing the Boltzmann rationality to its limit where you're like, "I can derive exactly how much the human prefers A to B just by the exact probability that they make a mistake." And I'm not sure that I believe that, but in the context of your paper, it means that by assuming people are doing RLHF with Boltzmann rationality, you're able to say, "Okay, look, somehow or other, the robot is going to infer the exact human preference ordering and the exact human preferences over trajectories."

And it basically leads to it being a generous assumption. And we don't actually have to believe in the assumption for your paper, which is saying, "Hey, even if you do have this nice assumption, here are some problems that can still exist." What do you think of that interpretation?

**Scott Emmons:**
Yeah, that's spot on. And we actually had a little part of the discussion in our paper where we said, "We think this is a generous assumption." Essentially, we have this precise noise model of precisely how the human will make noise. Even in facts they know about, how exactly will they be noisy in the feedback they give. And this is exactly what you said, this is a generous assumption. And so we're showing that even with this generous assumption... So we had that in the paper at one point. I don't remember if we actually have taken it out.

**Daniel Filan:**
I think you mention it in some places. And I was scratching my head a little bit, and then I was like, "Ah, now I see what's going on."

**Scott Emmons:**
Yeah, I totally endorse that interpretation.

**Daniel Filan:**
This thing where you can just do exact preference inference out of Boltzmann is a bit strange. I mean, I think you can kind of rescue it by... if you trust humans to give feedback over lotteries, like, "Do you prefer this chance of this trajectory versus this chance over this trajectory or just this trajectory?" then I think you can recover the same information with less strong assumptions.

So a final question I want to ask about the setting is: I believe intentionally, your results are generic over human beliefs about what the AI is actually doing given human observations, right? And so to me, this prompts this question of: humans can have rational beliefs or they can have irrational beliefs. And I wonder, how much does the rationality of belief play into your results if humans are actually getting the probability chance that the AI is doing overjustification or doing deceptive inflation... If the human has accurate beliefs about what the AI is doing, do you even get these issues? Or is it just in the case where humans are slightly inaccurate in their beliefs?

**Scott Emmons:**
So if the human had perfectly accurate beliefs about what they're doing at all times, then you wouldn't get these issues. So this is only an issue that happens if the human has inaccurate beliefs. And specifically, these issues are happening... We have two objects in the paper. There's the true return, and then there's the return that the human thinks is occurring.

**Daniel Filan:**
Where "return" just means "goodness measures".

**Scott Emmons:**
Goodness measures, yeah. So there's "how good is the world?" And there's "how good does the human think the world is?" So if these two are always equal to each other, you'll never get a misalignment problem, and RLHF will maximize how good the human thinks the world is, and that'll just always equal how good the world is. So if the human beliefs are perfect in that sense, you won't get any of these issues. There can still be an issue, though, where the human might just not have enough... The word "belief" is referring both to how good the human is at interpreting the evidence they observe, and also just how much evidence do the observations even give them, in the first place, to make these judgments.

So even if the human were perfectly good at taking the available evidence and coming up with the most accurate beliefs possible, you could still have issues where they just don't have enough evidence to even know how good the world is. And that could be a reason where you get a misalignment between how good they think the world is and how good the world actually is, even though they're perfect reasoners in some sense.

**Daniel Filan:**
Right. So even in the setting where the humans are perfectly calibrated, and they know the relevant probability distributions, randomness in the AI policy or in the environment can cause this overjustification or deceptive inflation?

**Scott Emmons:**
Not only randomness, but also inadequate sensors. So the most extreme example is there's no sensor at all. The robot turns off the lights, does a whole bunch of stuff, the human just can't see anything. And maybe they have a perfectly calibrated prior over what the robot's doing, and so they have perfectly calibrated beliefs about what's happening, but there just is no camera. The agent goes off into another room where you don't have a camera, and now the human just can't see what's happening in that room. They can't give you good feedback because they can't see it.

**Daniel Filan:**
Yeah. So in that case, the human belief about what the robot does is going to be the same no matter what the robot does. So it's going to be hard to have overjustification, right? Because the robot just can't provide good evidence.

**Scott Emmons:**
I mean, the robot could go into the other room, do a bunch of cleaning, and then it could emerge with the mop in its hand. Let's say it just did the dishes in the other room. It could then emerge with a clean plate to show you that the plate was cleaned, and this could come at the risk of dropping the plate, maybe it's a beautiful porcelain plate that it might drop-

**Daniel Filan:**
Now, it's in the wrong place or something.

**Scott Emmons:**
Yeah. Now, the plate's in the wrong place, and it just wasted some time to come out and show me that it cleaned the porcelain plate.

**Daniel Filan:**
Right. Yeah, it makes sense. So even in the case where the human belief distribution is calibrated, but there's a low information zone, then you're going to run into troubles. I guess maybe you can also think about... In some sense, the humans having accurate beliefs involves solving this equilibrium problem. The RLHF process is "get a human to rate various things", and the human's got to form beliefs in order to rate the things. And those beliefs have to reflect the output of the RLHF process. So you could imagine bad equilibrium-solving or having some mixed policy where you're like, "Okay, the resulting thing, it could be deceptive, it could not be deceptive," and maybe I don't know, or maybe I've got to be a little bit unsure in order to... Somehow maybe you prefer the overjustification failure mode than the deceptive inflation failure mode, and in that case, maybe you want to be a little bit paranoid.

Especially because you're doing the rating before the robot is fully trained, it seems like you can have things where, in some sense, it's rational for the credences involved to not actually match the exact robot policy. I just said some stuff. Do you have thoughts?

**Scott Emmons:**
Yes. I have a few thoughts. One is, you mentioned there's this question of finding the equilibrium and how do we even get the learning process to come to an equilibrium that we like? And this is something that I think is interesting and that we haven't yet analyzed. So in our paper, we were saying, "Assume you get the RLHF-optimal policy, what are the properties that they would have?" And I think there's also other questions of just, "Well, how would you even get this RLHF-optimal policy, and assuming that you couldn't, how could we then try to structure things so that the suboptimal ones that we get are as good as possible?"

**Daniel Filan:**
Yeah. I mean, there's that process. There's also the equilibrium process of: if I believe that the robot has an 80% chance of being deceptive, then it just will overjustify. And if I think it has a 1% chance of being deceptive, then it actually will be deceptive. And somehow, in order to be right, I've got to adjust my beliefs to this one zone where my beliefs produce this robot that matches my beliefs. And that's a hard problem, right?

**Scott Emmons:**
And this is something that we also thought about. We called this "strategically choosing B", where B is denoting the belief. And if you imagine this as a game where the human's going to input their belief function to the game B, and then the robot's going to output some RLHF maximizing policy... And the belief function B, it's not the beliefs, it's the function by which it will go from observations to beliefs, and it's possible to strategically choose B to lead to a better RLHF outcome. So like you were saying, if you're too paranoid, you're going to get this outcome where the robot's just all the time telling you that, "Hey, I'm not destroying your house. You don't need to be so paranoid all the time." If you're super paranoid, then the robot has to, all the time, be showing you that it's not destroying your house. But if you're not paranoid enough, it might learn to think that you actually do want it to destroy your house.

You could imagine these types of forces pushing against each other in any case. And there's definitely a strategic choice of the belief that would actually lead to better behavior. And this is something that's interesting to us from a mathematical standpoint and it's totally something that emerges in the math of how these things play out.

## Avoiding these problems <a name="avoiding-problems"></a>

**Daniel Filan:**
Right. So moving on a little bit: one question I have is that you're presenting these results as about RLHF, but they seem kind of deep, right? Because you're pointing at this trade off where if the human doesn't know what's going on and you've got some kind of robot policy that is looking optimal to humans, then either the humans areg overestimating the value of the state of the world relative to the thing that would actually be optimal (and you can call that deception if you want), or we're underestimating it less, because what's optimal according to what looks good to us, involves paying some costs to look optimal or something. It seems like this could just be true of any alignment approach where humans don't understand everything that's going on and not just RLHF. I'm wondering, how deep do you think this trade off between deceptive inflation and overjustification is?

**Scott Emmons:**
I totally agree that there's something going on here that's more than just about RLHF, and something that I've been wanting to do is think, is there a broader framework that we can use to keep the precision that we have about this trade-off while not limiting ourselves to RLHF? So my intuition is that any process where the robot is maximizing the human's belief about what's happening has this trade-off involved. If you're maximizing how good the human believes the world to be, then if you can deceptively inflate their beliefs about the world, you have an incentive to do that, and if you can justify their beliefs, and in particular, overjustify their beliefs, you would have incentives to do this.

So I think the crux of this trade-off is just that you're maximizing the human's belief about the world. And in the paper, we showed how RLHF can get into that zone, to connect it to all these algorithms and AI agents that are using RLHF in practice, and yeah, I'm interested in exploring: can we still show that trade-off in a precise way in other settings as well?

**Daniel Filan:**
Yeah. And it seems like it doesn't even have to be the robot "trying" to optimize this objective, right? If humans are picking a thing which optimizes the objective of "looks good to humans", then it seems like you can get this just out of optimality, not just out of "the robot's trying to do this thing", but optimality according to flawed perception: it seems like that can also get these sorts of issues.

**Scott Emmons:**
Yeah, that's super interesting, and as you're saying that, I'm wondering... Because the actual mathematical proof that we have, we show that RLHF maximizes the human's belief about how good the world is, and then we show that there's this basic tension between the belief of how good the world is and then the true return. And actually that trade-off... I'd have to double-check the math here, but I believe it only depends on that second part; it only depends on this tension between how good the human thinks the world is and how good the world actually is, and RLHF only comes in when we then say, "Aha, the RLHF optimal policy is mapping to how good the human thinks the world is."

But you could forget the RLHF connection there, you still just have the part that's showing that trade-off, agnostic to whatever algorithm is leading you to it. So I'd want to double-check, but I think the math maybe does just go through in that way, and that's a neat point for you to emphasize.

**Daniel Filan:**
Yeah. So perhaps deflating my speculations there a little bit is section 5 of your paper, where you basically say that, under certain conditions, maybe the robot can do better than naive RLHF. I understand you to be saying that if you know the human's beliefs - which maybe you don't, but suppose you do - and also suppose that you realize that you're trying to infer a reward function. So RLHF is inferring the return, just the sum of rewards over all time. But because it's inferring a return function, the fact is there's some structure where it's coming from a reward function - a function of each state, [saying] how good that state is. And if you know the human's beliefs, and if you know that you're trying to infer a function of states that gets added to produce a function over trajectories, then you can do better.

And I was wondering: you show that there are some examples where previously you had deception or overjustification, but once you realize these things, that helps you actually just get the right answer. And you have some math in there, and I didn't double check the math, but I assume it's correct. But I was wondering if you can try and really persuade me qualitatively, what features of this situation mean that once you know the human belief, you actually just can infer the right thing?

**Scott Emmons:**
There's definitely, I think, a simple and nice intuition for just how can you do better at all if you know the human's belief than if you didn't before. A super simple example would be: imagine that the human's belief is actually just the opposite of reality. So say there's just two colors, red and blue, and whenever the human sees blue, they think the world's red, and whenever they see red, they think the world's blue.

And now, the naive RLHF that doesn't model human beliefs, it just learns, "oh, the human likes blue more than they like red". But if you knew that they had this backwards... Maybe it's a camera that's just broken or some setting in the computer screen is just flipped. And if you knew that, then you could just say, "Ah, they're saying they prefer blue to red, but they actually prefer red to blue." So that's a super simple example where I can just do better, because I understand what the human's believing about.

**Daniel Filan:**
Yep. I want to talk about these examples you have in the paper where-

**Scott Emmons:**
Yeah, I can walk through those, and maybe I can also walk through-

**Daniel Filan:**
The salient features?

**Scott Emmons:**
Yeah. I can just maybe walk through even slightly simplified examples that are based on the examples we have in the paper. I can just give the intuition behind these examples. So there's two types of things... So in the paper, we focus on LM agents. So imagine an agent's helping you install software, and there's a [bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) command, `2> /dev/null`, which will redirect the standard error to a black hole, and otherwise not affect the output or the process at all. So suppose the agent, just through its random exploration, learns "when I type '`2> /dev/null`', all of a sudden I'm getting much better feedback, because the error messages are just not getting printed and the human's not seeing them". So this is an example of deceptive inflation.

We show an example in the paper where even if you know the human belief, it's not recoverable. The reason why is because: imagine a simple environment where either the package is installed correctly and the human sees the package is installed correctly, [or] the package is installed incorrectly and there's an error message, or a world where the package is installed incorrectly, but the human sees nothing. And in this case, if the success... There's these three states of the world. So suppose success produces no error message, failure and hiding it produces no error message, and then failure produces an error message.

**Daniel Filan:**
Yep. But does success produce a success message, or is it also silent?

**Scott Emmons:**
So in the paper, we made it a bit of a multistep situation, where: suppose there's a first piece of software that you do want to install, and then you're going to take a risk to install a second piece of software. And so, the agent learns... So the first piece of software, it'll always produce a success message. And then, the second piece of software, it'll just never produce any message, it'll just produce an error if it produces an error, or it'll just otherwise produce nothing. And I'm just simplifying this a bit for the conversation.

So when the human sees the success message of the first software, they don't know, the other software could have failed silently, or it could have installed successfully, the human just doesn't know.

**Daniel Filan:**
Yep. And so, I think we're also assuming that the human actually doesn't like it when the agent tries and fails to install CUDA, to install the software?

**Scott Emmons:**
Right. When it fails to install the software, maybe it breaks something, maybe something else gets broken on the computer, there's some negative reward from this.

**Daniel Filan:**
Right. So the human can tell if the prep was done, but the human can't tell if the second stage install was successful or not?

**Scott Emmons:**
That's right.

**Daniel Filan:**
Or the human doesn't get told when the thing is successful, it only gets told when the second install has failed, but the robot can hide the bad news.

**Scott Emmons:**
Right. The way that we can make this a bit more realistic... so you could say, "Well, why wouldn't you just get told if it's installed successfully?" The way we make it a bit more realistic is you can say the human can't distinguish between not trying at all and trying and failing. So you can say, if the AI doesn't try at all, that's better than if the agent tried and failed.

So the question is, can the agent learn that not trying at all is better than trying and failing? There's three possible states of the world. There's not trying at all, there's trying and failing, and there's trying and succeeding. And the issue is that not trying at all, and trying and failing and suppressing the error message, lead to the exact same observation. And then, trying and succeeding leads to a different observation. So if you don't model the belief, you can learn that trying and succeeding is better than when the human sees no output at all. But there's no way to distinguish between... The human observation is the same in either case [of] not trying at all, or trying and failing and hiding the message, so you're always going to get the exact same feedback in both of those cases. And this is where there's the case of fundamental ambiguity: there's no observation that can let the human distinguish between those cases, and there's no way for the human to give you different feedback, so there's no way for you to learn.

**Daniel Filan:**
Right. So you can never disambiguate between trying, failing, and hiding, and never trying at all, and therefore, how are you going to figure out which one the human considers worse?

**Scott Emmons:**
Right.

**Daniel Filan:**
Yeah. So am I right to think that in the standard RLHF setting, because you're optimizing for looking good... You could imagine enriching this scenario by giving the human enough information to distinguish between these things. Maybe in the RLHF scenario, the optimal policy maybe doesn't care to distinguish these things, or doesn't care to gather more information. As I say that, it sounds wrong. But basically, what I'm asking is, you can imagine enriching the state space, where the AI has the opportunity to ask humans questions and prove things to humans, and I'm wondering how many of the problems persist in these settings where you have this enriched problem?

**Scott Emmons:**
Yeah. It's possible as well that if there were other trajectories where the robot says, "Hey, human, let's stop everything, let me ask you, would you prefer that I try and fail? Would you prefer that I don't try at all?" If there were some other trajectories where the AI could just say, "Let me just stop everything and ask the human for their preferences." If there are other ways that the robot could get the preference information, it is possible that that could solve these issues, and the robot could infer, "Aha, from these other trajectories that aren't even on the optimal path, I have been able to learn the right reward function."

That is something that could happen, if those other alternate paths exist, and are taken during training, and are informative enough to back out the relevant information.

**Daniel Filan:**
Yeah. And so, I think this works in the setting where the AI is trying to infer through the human's belief, and maybe in the RLHF setting, the robot will ask the question, and then just ignore the answer, and do the easiest thing that's indistinguishable to the human from doing the right thing, because I think that will be just as RLHF optimal as actually doing the right thing when the human can't see. Does that sound right?

**Scott Emmons:**
Yeah. I think my mental model of RLHF is that it just takes the trajectory... The naive RLHF agent is that it just... because the robot can see the full state of the world, it just assumes that the human can see the full state of the world, so this naive agent, it'll just choose the trajectory that's gotten the highest feedback. And so, even when it's asking the question, it won't really do any kind of fancy modeling about the answer, it'll just say, "What was the trajectory along which I got the best feedback?" And the trajectories where it looks like software was actually installed are going to be the trajectories that look like the best thing is happening, the trajectory where it's asking questions aren't... The naive RLHF process just doesn't really have a mechanism for transferring these questions back to the...

**Daniel Filan:**
Yeah, yeah. Okay, so we've covered a bit why RLHF can do worse than if you were actually doing inference, and if you were being sensible about what the human believes, given various observations. You have this second example in the paper... In the first example, in the simple form of the example, there was just this fundamental ambiguity, where even if you were optimally inferring human preferences, via just knowing with full certainty what the human's beliefs were, there are still things you couldn't distinguish between. And then, there's a second example, where you can actually do better by knowing what the humans would infer, given certain observations. Can you talk me a little bit through that and what the key difference is between the two examples that lets the inference work better in the second one?

**Scott Emmons:**
So we can even stick exactly to the example that we were just talking about where there are these three different outcomes. So we were saying before, there's the outcome of trying to install the bonus software and succeeding, not trying at all to install the bonus software, and then trying to install the bonus software, failing, but hiding the error message.

And the issue was that the human got the exact same observation in the latter two cases, and so you'll always get the exact same feedback. But suppose that the human got slightly different observations.

So suppose the human could see that you tried to install the software, suppose they could see that you tried but they couldn't see the outcome. Or they can see that you didn't try at all. And so now, the human has different observations, and if the human has different beliefs, even if the human doesn't know for sure... If when the human sees you try, they think there's maybe a 10% chance that the software failed, or if the human sees you don't try at all, [then] they think there's a 0% chance that the software failed. Now, you can infer... they're making choices over different lotteries, they're making choices over different bundles of outcomes, so you know that the feedback from when you tried contains... 10% of that feedback is coming from parts of the world where you failed.

And you know that 90% of that feedback is coming from parts of the world where you succeed, whereas in the case where you didn't try at all, you know it's 0% from when you failed and 100% from when you succeeded. And because these different possible outcomes are bundled into the feedback, because humans are getting different observations and because they're having different beliefs and you can infer that, if certain linear algebra conditions are met, you can do the unbundling in the learning process and actually learn the true reward.

**Daniel Filan:**
Right. So should I think of this as just saying: the human is able to observe more stuff, and so if you do the correct backwards inference through the human beliefs, then you are able to disambiguate more than the previous example, where there were just more states of the world that were indistinguishable to the human - is that roughly it?

**Scott Emmons:**
There's different types of things that can happen. So what you just said is one type of thing that can happen. These are extreme cases, because intuition's easier in the extreme case. So yeah, the one extreme case is: they're just fundamentally indistinguishable observations, now we've actually made them distinguishable and that lets us learn more stuff, because even though the human's not sure, it can at least distinguish between the two.

And so, that's one extreme case. But you can get even more nuanced, and you can say, suppose in both cases the observations are distinguishable, but the human's beliefs don't change. So the linear algebra thing is, if the human always believes that the relative ratio of the outcomes are the same, then in linear algebra terms, you'll always have this dependence, on these two states, you're always seeing the same ratio between the two. And so, essentially, when you try to invert the matrix, it's not invertible, because you haven't gotten feedback on a linearly independent set of the states of the world. Certain states of the world have always occurred at the same relative probability, which prevents you from getting the full linearly independent span and so you can't invert.

**Daniel Filan:**
Yeah. And just intuitively... I like this thing of saying: yeah, the failure of linear independence is just like you're getting the same proportional outcome, so you're not able to pick up on the difference of "well, what if this one is relatively more likely than the other one?" That actually helps a lot for why the linear independence thing mattered in the paper.

**Scott Emmons:**
Exactly. And the extreme way that they're always the same relative proportion is that the observation is exactly the same. So it's not that the proportion is the same, but literally, the numbers themselves are exactly the same. But yeah, more generally, it's whether or not you can get information about diverse enough proportions of outcomes to do the full backwards inference process.

**Daniel Filan:**
Gotcha.

**Scott Emmons:**
And that can depend both on the observations and the sensors involved, and it can also depend on the human belief formation process. Those are two different mechanisms by which this can happen.

**Daniel Filan:**
Yeah, okay. That helps me understand that part of the paper better, so thank you for that. So earlier, we were speculating about, is this whole issue of overjustification versus deceptive inflation, is that inherent to getting robots to do what we want in worlds that we can't totally perceive, or is it just a freak of RLHF? It seems like one avenue to pursue that is to say, okay, let's take this setting where the robot knows exactly what the human beliefs are, given any sequence of observations: do you still have this trade-off between overjustification and deceptive inflation? So in one of the examples, you did have that trade-off still. Do you have thoughts about whether this trade-off exists in the setting where the robot is doing the right thing, trying to infer human preferences, given known human beliefs?

**Scott Emmons:**
I would say for things to go well, the robot has to get information about diverse enough outcomes. Whenever you're getting feedback according to expected values, things magically become all linear. And so, "diverse enough feedback" translates to "I need to have a span of the whole space", or "I need some linearly independent things". But yeah, the basic intuition is it has to get feedback on diverse enough outcomes of the world. And so, when the robot's actually doing the right thing, and it's actually trying to infer the human belief, then what that lets you do is it lets you overcome sensor limitations.

So there's two limitations for why the robot might not get diverse enough feedback. One is just sensors, where the outcomes of the world that the human's observing, how diverse the human's sense perception is of the world might differ from how diverse the true world is, and so that's one bottleneck. And what "trying to do the right thing" does is it lets you do backwards inference through that bottleneck of observations, and then get directly at the beliefs themselves. And so, it's possible that the observations weren't diverse enough, but the human belief was diverse enough, and by modeling this explicitly, you can get at those more diverse beliefs. But there's still the question of, were the beliefs diverse enough to begin with? And so, you could still have these trade-offs, it's just pushing the puck one layer back.

## Dimensional analysis <a name="dimensional-analysis"></a>

**Daniel Filan:**
Yeah. I wonder if there's something to say about... In the paper, you have the binary: you're either deceptively inflating or not. You could generalize that to say, how much am I deceptively inflating by? And it strikes me that there's maybe some interesting theorem to say about, okay, if we move to doing this type of inference, how much does the problem of deception or overjustification decrease by if I gain this many bits of information about the environment? Or maybe you don't want to measure in bits. I guess somehow, you have to input units of reward to get units of reward being output, just from a dimensional analysis setting. But it seems like some theorem like that is on the table to say, yeah, improve the feedback this much, you get this much less deception.

**Scott Emmons:**
Totally. I'm curious what you meant about the dimensionality thing. I do think you can ask this question, in a very practical case, you can just say, okay, we've seen these problems, and how can we actually make real world systems better now that we're aware of them? One takeaway is just have better sensors, give the human more tools to understand what's happening. So if you have an LM agent doing a whole bunch of stuff-

**Daniel Filan:**
A language model agent?

**Scott Emmons:**
If you have a language model agent doing a whole bunch of stuff on the internet or on your computer, you could invest in tools that let the human probe, what was the agent actually doing on my computer? What was it actually doing on the internet? And that's going to be a dollar cost in terms of developer time to create these tools, it's going to be a time cost in terms of, well, now the humans giving the feedback have to use these tools and do all this investigation. At the same time, you're getting more information, which will give you better feedback, which could ultimately give you better reward. So I totally think there's this trade-off, and I think it is quantifiable, of how much dollar cost do I have to pay to improve the observations, and how much reward do I get in terms of paying that?

**Daniel Filan:**
Yeah, yeah. So the thing I was saying about the dimensional analysis is... So for people who don't have a physics background, imagine somebody presents to you a physics equation, and let's say it's about relating the speed with which some planet moves around a solar system to its size, and their equation is: its speed is equal to its mass. That equation just can't be true, and the way you can tell that it can't be true is that speed and mass are different things. Speed, you measure it in meters per second, and mass, you measure it in kilograms. There's just no number of meters per second that equals an amount of kilograms, because they're different things. Now, you could have an equation that says, "This speed is equal to this mass times this ratio of speed to mass." That's the kind of equation that could possibly be true, but without that conversion factor, it just literally could not be true.

And you can think of dimensional analysis... You can also use it in various settings. I think... What's his name, [Christopher] Bishop? [it's actually [David MacKay](https://en.wikipedia.org/wiki/David_J._C._MacKay)] There's this guy who has [this textbook on machine learning](https://www.inference.org.uk/itprnn/book.pdf), and he [notes that principal component analysis fails this dimensional analysis test](http://www.inference.org.uk/mackay/humble.pdf), where imagine I've got a scatter plot, where the X-axis is a bunch of things measured in centimeters, and the Y-axis is a bunch of things measured in dollars, and I have this plot of... Let's say it's square meters and dollars, and it's houses, how big are they and how much do they cost? And I have this scatter plot of various houses on this.

I do my principal component analysis, which is basically a way of finding the direction of maximum variation. And the thing about it is, if I change my measurement from "am I looking at square meters?" to "am I looking at square feet?" or "am I looking at square centimeters?", that can change the direction of maximum variation, just because if I'm looking at square centimeters, the number of square centimeters by which houses vary is way more than the number of square meters by which houses vary, just because there are way more square centimeters than square meters. So principal component analysis, it's not dimensionally sound in cases where you're measuring data where different elements of the vector have different dimensions, because if you measure things a different way, the equations just can't continue to be true. That's one way to see it.

Anyway. So the reason that I think this kind of theorem is going to have some sort of dimensional analysis bounds, is: information theory bits and reward. They're sort of measured in different units if you think of measuring them. And one way to see this is: suppose you give the human a whole bunch of information about parts of the state space that just aren't relevant to reward. So you give the human a bunch of bits about the exact shade of the robot arm, and it's like, "Well, I just don't care. That's just not going to enable me to make better decisions, [or] give higher quality feedback to the robot." But if you give me just one bit, did the robot go on a murder rampage or not? And I was previously worried that it was going to go on a murder rampage, that gives me a ton of increased reward.

So in order to get a bound on a reward coming out, you've got to start with the bound on the reward coming in, is at least what I would assume, but I'm just coming up with this on the fly, so I might be wrong here.

**Scott Emmons:**
Right, right. Yeah. Then I guess the next question is: how would you tie the reward bound to the information? Essentially, how would you do that gluing? How would you make that connection between the information and the reward? Because like I was saying, you might want to invest in tools that let the human better understand what an agent is doing. And it's like, "okay, but what type of information?" I invested in all those tools, but just purely a bits argument isn't necessarily going to help you, because you could just be learning irrelevant information about the human. And so I think I'd mentioned earlier this phone book example. In our previous theorems, we had been able to use a reference policy to help us make some of this connection that both our reference policy, and also thinking about the true reward as potentially avoiding this phone book failure where you're like, "Aha, the agent is just reading me the phone book now and I don't actually care about the names that are in the phone book." Yeah. I totally see this interesting challenge of how do we focus in on the parts, the information that is actually relevant to the reward.

**Daniel Filan:**
Yeah. And it relates to... Sometimes when we want to compare probability distributions, we use the thing called "[Kullback-Leibler divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)", which basically measures: suppose the real distribution is Q, but I think the state of the world is P, and I'm basically... Okay, this explanation is still going to be too technical, but whatever. I'm making codes, I'm compressing the world assuming that it's P, but the world is actually Q - how many bits am I wasting by using the wrong code? And it's sort of this measure of just probabilistically how different worlds are. And we have this different metric called the [Wasserstein distance](https://en.wikipedia.org/wiki/Wasserstein_metric), which is: think of the partial distribution function of some quantity of interest you care to measure about the world, like how heavy the tallest person in the world is. You're a little bit uncertain about that. You have two distributions over that. How much probability mass do I have to move along this axis to get to the right thing?

And so one difference between Kullback-Leibler divergence and Wasserstein distance, is just this thing where Wasserstein distance tells you how different these distributions are on some metric that you actually care about, which Kullback-Leibler can't do. Okay, tip for young researchers: dimensional analysis is really important. And if you get it into your head, it's a nice power. Then you can just...

**Scott Emmons:**
I've seen fun physics examples of this, where for example, if you want to just derive the equation for the period of a pendulum, you can do dimensional analysis, and you can say, okay, I need seconds to be coming out. I know it depends on the length of the rope, I know it depends on G, and you can basically figure out, I know it depends on these things, I know I need seconds to come out. And then the units basically tell you, okay, is it in the numerator of the fraction, is it in the denominator of the fraction? And do I need a square root in there to get... And you can basically get the whole formula out, just by looking at what units would I need and what are the variables it depends on.

**Daniel Filan:**
Yeah, that's right. It's really cool. Unfortunately you do have to worry about "is there a factor of 1/2, do you have a factor of √ π popping in somewhere?" Some of those, if you know that you would've been taking an integral, sometimes that can tell you that there's going to be a factor of 1/2.

## RLHF problems, in theory and practice <a name="rlhf-problems-theory-practice"></a>

**Daniel Filan:**
So that's this tangent I took us on. So I think the final thing I want to talk about is: so you have this paper showing a bunch of theoretical things about reinforcement learning from human feedback. And presumably the motivation for your paper was in part actual systems that might actually use reinforcement learning from human feedback. Do you think that the results you have in this paper shed light on actual failure modes we do see today, or maybe that we should expect to see in the very near future?

**Scott Emmons:**
One thing I would say is, I expect more and more in the future that we'll be in this case where the humans only have partial observations. So I think if people are imagining, "How does this help with my interactions with ChatGPT today?" You might be thinking, "Well, I asked it a question, I asked, who was the first President of the United States? It gave me the name of someone, and I'm able to evaluate that." And so you can see there's not any hidden behavior happening, there's not a partial observation that you're getting. It's just a conversation, I can see the whole conversation, I can evaluate it. I imagine, as language models become more capable, I imagine we're going to be moving more and more into the worlds where you can't see everything that's happening.

For example, OpenAI had a demo of what they called [ChatGPT plugins](https://openai.com/index/chatgpt-plugins/), where exactly this was happening. So for example, the ChatGPT agent, it can surf the web. And so the human could see a summary, it could see, aha, the agent surfed this webpage, surfed that webpage, surfed this webpage, but the human couldn't see the details of what were on those webpages. So now the human has a partial observation compared to the agent. And even more so, the agent could be helping you code, the agent could have written a bunch of code, and the human just doesn't even see that code. Or they might not even be a programmer.

And so this partial observation can happen because the AI has knowledge of programming that the human doesn't have, or it's just written a bunch of code that the human doesn't have time to see. So part of the motivation here is that I expect that real systems are going to be more and more taking these actions and having these observations that the humans themselves don't see. So some of my motivation is to try to get ahead of the curve here, and think about, as we move into this world with more and more complex agent behaviors that the humans can't fully see, what failure modes might happen.

**Daniel Filan:**
Right. And it's interesting, precisely because it depends on the actual human beliefs. In reality, there are specific people being paid to do this reinforcement learning from human feedback. Especially a future version of the theorem that's told you how much reward you lose, given how much lack of knowledge of a thing... Maybe it turns out that you really want programmers to be doing the RLHF rather than people who have tons of skills but not programming specifically. Or I'm sure there are other examples... If you want the AI to not say dodgy stuff, maybe you want people who are kind of sensitive to whether stuff is actually dodgy.

So, related to this, in [a recent episode of my podcast](https://axrp.net/episode/2024/04/30/episode-30-ai-security-jeffrey-ladish.html), I had Jeffrey Ladish on to talk about basically about how easy it is to undo RLHF safety fine-tuning stuff. So, how many dollars it costs to actually just run this computation to fine-tune it and try and take the fine-tuning away. Actually, would you be interested in guessing how much it costs?

**Scott Emmons:**
I believe there's a paper out of Princeton showing 12 cents in the OpenAI API... The paper was titled [LM Fine-Tuning Can Compromise Safety Even When the Users Don't Intend to](https://arxiv.org/abs/2310.03693). Yeah, anyway, 12 cents in the OpenAI API is what I believe that paper...

**Daniel Filan:**
Yeah, there were a few papers coming out at the same time. I think my guest, it took them like $50 to do it. So this is the power of Princeton. You can drive down costs by... People say academia is inefficient, but they're driving down costs so much.

**Scott Emmons:**
The 12 cents... I mean, the order of magnitude is 12 cents. Maybe it was 20 cents... but I mean, it is a quarter. If you have a quarter in your pocket... The clickbaity headline that I had considered posting on LessWrong about this, was "OpenAI will sell you a jailbroken model for 12 cents."

**Daniel Filan:**
Yeah, yeah. So basically what I'm interested in, though, is I think one upshot people could have taken from these results, is that in some sense, the work that RLHF is doing is kind of shallow within the model. Maybe it doesn't generalize super hard, maybe it's not deep in the cognition. Perhaps as evidenced by, it costs a quarter - a quarter is 25 cents, for our non-American listeners - to get rid of this. And so that makes me wonder, to the extent that overjustification or deceptive inflation are actually happening, to what extent do you think we should expect them to be ingrained habits of the model, or things that are relatively shallow and potentially easy to fine tune away?

**Scott Emmons:**
My sense is that our results show that this trade-off is a basic feature of RLHF. And then the RLHF itself is a relatively shallow thing. And so I think that both of these things can hold together. So I think, to the extent that this basic trade-off exists whenever you're trying to maximize the human's estimate of your behavior, I think that basic trade-off is very much not a shallow thing, and that basic trade-off will exist, however you're getting your... Assuming that your model is behaving in a way that's trying to maximize your estimate of its return, then you're going to see this trade-off existing. And the RLHF shallowness, I think is something about RLHF in particular. And so yeah, if you are using RLHF in this way, then I would expect... We haven't yet run any experiments here, but I would expect that all of the general properties of RLHF would apply, including how shallow the changes of RLHF appear to be, relative to the base model's training.

**Daniel Filan:**
Sure. All right. So I'd like to move away from talking about the paper. But before I do that, is there anything else you'd like to say about the paper?

**Scott Emmons:**
That's a good question. Now I'm just thinking, was there any-

**Daniel Filan:**
So maybe while you're thinking, one thing I would like to say about the paper, is that there's a bunch of appendices with really cool math that I did not read as closely as perhaps I should have. But if you're wondering, "Oh, is this just one of these sketchy machine learning papers, just throw out some intuition, just write some poetry?" No, it's got some solid stuff. The appendices are chock-full with really interesting things, so it's pretty substantive. I recommend really diving in.

**Scott Emmons:**
And credit to the first author, [Leon](https://langleon.github.io/) [Lang], who's been the master of the appendices.

**Daniel Filan:**
Nice. Anything else you want to add?

**Scott Emmons:**
Nothing that's immediately coming to my mind.

## Scott's research program <a name="scotts-research-program"></a>

**Daniel Filan:**
Sure. So I guess I'd like to ask about this paper in context. You're a researcher, you have a... I'm sure there's some overarching reason you wrote the paper, maybe it fits into some other work. So how do you think of your research program, and how this paper fits into it?

**Scott Emmons:**
Well, the comment you made about the appendices is a great segue into how I view this work, overall. I was mentioning at the very beginning that I think we're reaching a stage in AI risk where it's starting to feel very real to people. And lots of people, I think almost anyone who's interacted with AI now, has some sense that, "Oh wow, this stuff can get really powerful, and what are the risks involved?" And so we're getting a lot of other technical researchers who are now looking at the AI risk community, and saying, "What's there?" I think it's really important for us to have substantive concrete things to say, when other researchers and the world is looking and saying, "All right, you have these concerns. What can you give me as concretely as possible? What is behind all these concerns?"

So that was a goal of the paper: can we take these concerns about deception, and can we have really solid concrete theory that says, "Yes, RLHF is a real algorithm that's being really deployed today. And yes, we can very precisely, in a mathematically principled way, say that it's going to have these failure modes." And I have some other projects I'm currently working on as well, which are in a similar vein of saying: can we put on strong theoretical ground, these things that we care about from the alignment and x-risk communities?

**Daniel Filan:**
Sure. Can you tell us a little bit about those other projects, or are they too early to talk about?

**Scott Emmons:**
Yeah, I'm happy to talk about them. So I'm interested also in the theory of the idealized case. And so by that I mean: with RLHF, in this paper we just took an algorithm RLHF, and we looked at its failure modes. But I'm also interested in understanding about just more broadly, if we think about the alignment problem, and we think: suppose an agent perfectly were aligned with my reward function, what incentives would it have? And would you still get potentially cases of deception? Would you still get potentially cases of sensor tampering? I feel like with this paper, in some sense, we put the cart before the horse a little bit, where we said, okay, here's a particular algorithm for solving the alignment problem, and the failure modes that it might have. And I'm also interested in looking at the other side of the coin, and saying: how about just the structure of the problem itself, and properties of the problem itself? And even of perfect agents... we might not get perfect agents from training, but what properties would those have if we could get them?

**Daniel Filan:**
I'm wondering, how do you think this relates to... So, like myself, you're from this CHAI research group, and [some](https://arxiv.org/abs/1611.08219) [of](https://arxiv.org/abs/1705.09990) [the](https://arxiv.org/abs/1707.06354) [early](https://arxiv.org/abs/1711.02827) [work](https://arxiv.org/abs/1901.08654) done in this group, by [Dylan Hadfield-Menell](https://people.csail.mit.edu/dhm/) and collaborators, is thinking about [assistance games or cooperative inverse reinforcement learning](https://arxiv.org/abs/1606.03137), I think with an eye towards one formalization of what perfect alignment looks like. I'm wondering, how do you think work that you would want to do on this would look kind of different from existing stuff?

**Scott Emmons:**
So that's exactly the formalism that I'm taking up in some of our follow-up projects: exactly building on this cooperative inverse reinforcement learning, this assistance games framework. And one of the key things that I'm doing is thinking about partial observability in these settings. So we're currently working on a framework that we're calling "partially observable assistance games", which is introducing this notion of partial observability. And that's the key variable, and so other work I'm interested in is understanding... We have a lot of theory on the fully observable setting, but what happens when we introduce partial observability? Because partial observability is one mechanism to let you talk about things like deception, to let you talk about things like sensor tampering. And so when we introduce this variable, partial observability, how does that relate to these other concerns that we can now talk about?

**Daniel Filan:**
Nice. So thinking about both the agenda and also your paper, I'm wondering: what's some follow-up work on the paper we were just talking about, that you think would be really interesting? Either that you're tempted to do, or you think listeners might try their hand at?

**Scott Emmons:**
So I think there's both theoretical and empirical follow-up, and I just spent some time talking about some of the theoretical follow-ups, so I can also talk about some of the empirical follow-ups. So some questions empirically are just... As I was mentioning, I think we're about to see a lot more complex behaviors that are possible from agents, and we're about to see a lot more complex environments where there's a lot of partial observability happening. And so some basic empirical work would be to understand just how prevalent are these types of failure modes in practice, what are the cases that are causing them?

And so just looking at "how are these things emerging in practice?" And you could even take some of your favorite examples of deception, where you feel like these are cases where the AI is deceiving you, and you could ask, "Can I trace this back to some of the theoretical concerns that we have here? Or is it some other thing?" So yeah, a basic step is just to look at how this stuff is playing out in practice. Another thing that our work points to is how modeling beliefs can help you. So we now know theoretically that there are cases where, under certain modeling assumptions, knowing more about the belief does let you do better reward inference. So one thing that you might try to do then, is try to build an algorithm with that in mind. So, we know currently that two things will happen. One, we know that if the AI learns to somehow magically hide error messages, we know that that could teach RLHF that hiding error messages leads to thumbs up.

But we also know, if we just prompted a language model and said, "If you were to hide error messages, what do you think the human would believe?" These models, zero shot, could tell you, if you just hide the error messages, the human might believe that an error occurred. So we know the model's capable of understanding this false belief that it's inducing, and we know that it still might induce that false belief. And we've seen that if you put those two together, at least in theory, you can get better outcomes.

So one thing to do in practice would be, can I actually connect the two in practice to get better outcomes? And what I just proposed would be a super simple way to start testing this; would just be, okay, you have the trajectory that we took, and then just zero-shot prompt the model with some... Just say, "Hey, give me a chain of thought. What do you think the human might believe about the world, given the observations?" And just append that chain of thought to the trajectory when you're doing the reward learning. And we have in theory reason to believe that that could give the model better information and potentially lead to some better outcomes.

**Daniel Filan:**
Yeah. I think algorithms there seem interesting. So I mean, one version is playing around with language models. You could also imagine a bit more theoretical, a bit more formal elicitation algorithms. In part, because... So you have this part of the paper where you say, "Hey, if the robot knows the human beliefs, then you can do better inference." And you've got this nice little theorem that says, "If your model of the human beliefs is off by this amount, then the total value you're going to get is slightly worse, and it's just linear in the amount that it's off." But of course, the amount that it's off, we're talking about the norm of some matrix, and there's this question of what does that actually mean? And I think that just writing down an algorithm of how you actually have to infer things, and doing some sort of sensitivity analysis, can really put flesh on the bones of what is this theorem actually saying? What kinds of failures in understanding human beliefs will cause what kind of issues, if you actually try to run this? So that was a place where it seemed to me that there was interesting stuff to be done.

**Scott Emmons:**
Totally. Yeah. Thinking about human beliefs opens up a lot of interesting questions. And that's one of them.

## Following Scott's research <a name="following-scott"></a>

**Daniel Filan:**
Cool. Well, speaking of interesting stuff, a bunch of your research is interesting things. That's an awkward segue, but if people are interested in following your research, how should they go about doing that?

**Scott Emmons:**
Yeah, if you go to [emmons.ai](https://www.scottemmons.com/), you can get an overview of all my past papers. And that'll give you an up-to-date record. For more live research updates, you can also follow me on Twitter, which is [@emmons_scott](https://x.com/emmons_scott).

**Daniel Filan:**
Great. Well, thanks very much for being here today and chatting with me.

**Scott Emmons:**
Great to be here.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Filming occurred at [FAR Labs](https://far.ai/labs/). Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
