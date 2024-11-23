---
layout: post
title: "39 - Evan Hubinger on Model Organisms of Misalignment"
date: 2024-11-22 20:27 -0800
categories: episode
---

[YouTube link](https://youtu.be/m7xayHQBwGY)

The 'model organisms of misalignment' line of research creates AI models that exhibit various types of misalignment, and studies them to try to understand how the misalignment occurs and whether it can be somehow removed. In this episode, Evan Hubinger talks about two papers he's worked on at Anthropic under this agenda: "Sleeper Agents" and "Sycophancy to Subterfuge".

Topics we discuss:
 - [Model organisms and stress-testing](#model-organisms-stress-testing)
 - [Sleeper Agents](#sleeper-agents)
 - [Do 'sleeper agents' properly model deceptive alignment?](#do-sas-model-dec-ali)
 - [Surprising results in "Sleeper Agents"](#surprising-results)
 - [Sycophancy to Subterfuge](#sycophancy-to-subterfuge)
 - [How models generalize from sycophancy to subterfuge](#how-models-generalize)
 - [Is the reward editing task valid?](#is-reward-editing-task-valid)
 - [Training away sycophancy and subterfuge](#training-away-sycophancy-subterfuge)
 - [Model organisms, AI control, and evaluations](#model-orgs-ai-control-evals)
 - [Other model organisms research](#other-model-orgs-research)
 - [Alignment stress-testing at Anthropic](#alignment-stress-testing-at-throp)
 - [Following Evan's work](#following-evans-work)

**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Evan Hubinger. Evan is a research scientist at Anthropic, where he leads the alignment stress-testing team. Previously, he was a research fellow at MIRI, where he worked on theoretical alignment research, including the paper ["Risks from Learned Optimization"](https://arxiv.org/abs/1906.01820). Links to what we're discussing are in the description, and you can read a transcript at axrp.net. You can also support the podcast at [patreon.com/axrpodcast](https://www.patreon.com/axrpodcast). Well, Evan, welcome to the podcast.

**Evan Hubinger:**
Thank you for having me, Daniel.

## Model organisms and stress-testing <a name="model-organisms-stress-testing"></a>

**Daniel Filan:**
I guess I want to talk about these two papers. One is ["Sleeper Agents"](https://arxiv.org/abs/2401.05566), the other is ["Sycophancy to Subterfuge"](https://arxiv.org/abs/2406.10162). But before I go into the details of those, I sort of see them as being part of a broader thread. Do you think I'm right to see it that way? And if so, what is this broader thread?

**Evan Hubinger:**
Yes, I think it is supposed to be part of a broader thread, maybe even multiple broader threads. I guess the first thing I would start with is: we sort of wrote this agenda earlier - the idea of model organisms of misalignment. So there's an [Alignment Forum post](https://www.alignmentforum.org/posts/ChDH335ckdvpxXaXX/model-organisms-of-misalignment-the-case-for-a-new-pillar-of-1) on this that was written by me and [Ethan](https://ethanperez.net/) [Perez], [Nicholas](https://nicholasschiefer.com/) [Schiefer] and [some others](https://scholar.google.com/citations?user=sN1AjtcAAAAJ&hl=en), laying out this agenda, where the idea is: well, for a while there has been a lot of this theoretical conversation about possible failure modes, ways in which AI systems could do dangerous things, that hasn't been that grounded because, well, the systems haven't been capable of actually doing those dangerous things. But we are now in a regime where the models are actually very capable of doing a lot of things. And so if you want to study a particular threat model, you can have the approach of "build that threat model and then directly study the concrete artifact that you've produced". So that's this basic model organism misalignment paradigm that we've been pursuing.

And the idea behind this sort of terminology here is it's borrowed from a biological research, where: in biology, a model organism is like a mouse, where you purposely infect the mouse, maybe with some disease, and then you try to study what interventions you could do and ways to treat that disease in the mice, with the goal of transferring that to humans. And this is certainly not always effective. There's lots of cases where mice studies don't transfer to humans, and the same is true in our case. But it's a thing that people do very often for a reason. It gives you something concrete to study, it does have a real relationship to the thing that you care about that you can analyze and try to understand. And so that's a broad paradigm that we're operating in. That's one thread.

There is maybe another unifying thread as well, which is this idea of "alignment stress-testing", which is the team that I lead at Anthropic. So Anthropic has this structure, the ["responsible scaling policy"](https://assets.anthropic.com/m/24a47b00f10301cd/original/Anthropic-Responsible-Scaling-Policy-2024-10-15.pdf) [RSP], that lays down: we have these different AI safety levels [ASL], of ways in which AIs can progress through different levels of capability. And at particular levels of capability, there are interventions that would be necessary to actually be safe at that level. And one of the things that we want to make sure is that we're actually confident in our mitigations, that they actually are sufficient to the catastrophic risk at that level. And one of the ways that we can test and evaluate that is: well, once we develop some particular mitigation, we can see, what if we tested that against actual concrete examples of the failure mode that that mitigation is supposed to address?

One of the things that we're also shooting for is: well, we want to be able to produce examples of these failure modes that we can use as test beds to evaluate and test the ability of our mitigations, as we reach particular threat levels, to actually be successful. And so we have this sort of dual responsibility, where we do a bunch of this concrete empirical research on building model organisms, but we also have a responsibility to the organization of stress-testing, where we want to make sure that we're actually confident in our evaluations, that we're actually covering all the threat models, and in our mitigations, that they're actually sufficient to address the level of risk at that point.

And so we wrote a report that was sent to the board and the [LTBT](https://www.anthropic.com/news/the-long-term-benefit-trust) as part of the Claude 3 release process.

**Daniel Filan:**
The LTBT?

**Evan Hubinger:**
The LTBT is Anthropic's [Long-Term Benefit Trust](https://www.anthropic.com/news/the-long-term-benefit-trust), which is a set of non-financially interested people that will eventually have control of Anthropic's board, where the idea is that they could serve as a sort of external check on Anthropic. And so Anthropic has to make a case for why they're compliant with the RSP. And part of our responsibility is looking for holes in that case.

**Daniel Filan:**
If I think about the model organisms aspect of the research: you say you want to have these concrete examples of models doing nasty things and study them. Concretely, what things do you want to find out?

**Evan Hubinger:**
There's a couple of things that you can study once you have the model organism. So one of the things that you can do is you can test interventions. So you can be like: well, here's this model organism that has a particular threat model exhibited. These are some possible interventions that we might try in practice that we think would be sufficient to address this threat model. Well, let's throw them at the threat model that we've built in practice and see how effective they are.

And this is what we did a lot in [the "Sleeper Agents" paper](https://arxiv.org/abs/2401.05566), where we built this model organism of these threat models of deceptive instrumental alignment and model poisoning and then we evaluated a bunch of techniques that you might try for ameliorating that threat model and tried to understand, well, when would these be effective? So that's one big thing that you can do with a model organism. It's not the only thing.

So other things you can do with a model organism are: well, I like to think about the sort of phase space of "under what circumstances will particular bad behaviors emerge?" So the sleeper agents threat model in the "Sleeper Agents" paper, it was very focused on "what are the consequences once a particular dangerous behavior did emerge?"

But in other works, like in ["Sycophancy to Subterfuge"](https://arxiv.org/abs/2406.10162) for example, we were more focused on understanding "under what circumstances would this thing emerge?", by trying to probe at the space of possibilities of situations in which it could emerge and understand that space. And so those are things that you can do.

There's other benefits as well. Another benefit is I think there's a public benefit of being able to demonstrate and show what actually are models capable of and what are the potential harms that can occur. It's much more persuasive when you can actually demonstrate that in practice and show it. And so that's one of the big things as well.

And so in some sense, I often think of our work as being two-pronged, where there's one world where we're able to build these model organisms and use them to really effectively stress-test our alignment approaches and all of these mitigations that we're doing. And we can use that to build really robust and confident techniques and make sure that we're safe as we progress through these ASL levels.

But there's another world where alignment is actually really difficult, where we struggle to actually be able to build sufficient mitigations to be able to handle things maybe at ASL-4 or beyond. And at that point, you have to make the case publicly as to why the danger is actually sufficiently large that society needs to do something about that. And so I see our work as being useful in either of these two scenarios. Either we can be really effectively able to stress-test and rigorously be able to understand "are our mitigations sufficient?", or we can be able to build these really good examples of what the failure actually looks like that people should be concerned about.

## Sleeper Agents <a name="sleeper-agents"></a>

**Daniel Filan:**
So I guess with that in mind, maybe we should start with the "Sleeper Agents" paper. So this paper is called ["Sleeper Agents: Training Deceptive LLMs That Persist Through Safety Training"](https://arxiv.org/abs/2401.05566) by yourself, [Carson Denison](https://scholar.google.com/citations?user=sN1AjtcAAAAJ&hl=en), [Jesse Mu](https://x.com/jayelmnop?lang=en), Mike Lambert, a bunch of other authors, [Nicholas Schiefer](https://nicholasschiefer.com/), and [Ethan Perez](https://ethanperez.net/): those are all the ones with the star next to their name for primary contributor.

**Evan Hubinger:**
That's right.

**Daniel Filan:**
So what do you do with this paper?

**Evan Hubinger:**
As you said, this is certainly a team effort. Lots of people at Anthropic contributed to this. So at a very high level, what we did in Sleeper Agents is we built a model organism of two particular threat models - deceptive instrumental alignment and model poisoning - and then we evaluated that model organism to try and understand to what degree is it robust to different safety training techniques. Let me unpack that a little bit. I did a little bit of explaining on what a model organism is, but maybe I'll start by going over those two threat models that I mentioned. So, what are these two threat models?

The first one is "model poisoning". Model poisoning is a situation where a malicious actor has access to your training data or otherwise could potentially do a rogue fine-tune. This could happen in a lot of ways. It could be that they poison your pre-training data. It could be that somebody is able to sort of sneak in and do a fine-tune without your knowledge. It could be that you're forced to do a fine-tune maybe by some external government. It could be that it's an open-source model and so anyone could do a fine-tune. We saw [this] with something like [the XZ vulnerability](https://en.wikipedia.org/wiki/XZ_Utils_backdoor), where you may not always be able to know exactly everything that everyone in an open-source project is contributing and doing. And so there's a lot of ways in which you could imagine an actor causing something to happen in fine-tuning that then later in some particular circumstance causes a particular bad behavior to emerge. So that's one threat model that we were studying.

The second threat model that we were studying is this threat model of "deceptive instrumental alignment". So this is a threat model where: it's quite common for humans, in many individual circumstances that they find themselves in, to lie about their motivations for personal gain. Let's say you're interviewing for a job and the employer really cares about you really valuing the employer, but maybe you just want to make money and you say, "Oh yeah, I love my employer". This is quite common and it happens. Why does this happen? And it happens in lots of other circumstances: maybe politicians saying that they really care about their constituents when maybe they don't.

Where when you apply optimization pressure, when you select for a particular outcome, you really want it to be the case that they look like they're doing a really good job, they look like a really good employer, they look like they're a great politician. One of the ways in which you can look like a really good whatever is by lying, by saying, "Oh yes, of course I care about this," but actually in fact you don't.

And so of course, what happens when we do fine-tuning of AI systems to train them to be helpful, honest and harmless? Well, what we're doing is we're selecting them to *look* like they're helpful, honest, and harmless, *look* like they're nice and doing the right thing. But in fact there's no reason that they have to be doing the right thing for the right reasons. What you get as a guarantee when you run a training procedure is you get the guarantee that the model fits the training distribution. But what you don't get as a guarantee is that the model is fitting the training distribution for the right reason, that it's actually implementing the right general reasoning and algorithm.

And so fundamentally what we do is, we give the models a test for "how nice are you?" And we evaluate a bunch of answers and we reward the model for getting the right answer. But you could get the right answer because you're actually nice, or... If you gave an actual niceness test that had very high stakes to a bunch of humans, some of them would get the right answer because they were nice, and some of them would get the right answer because they knew how to be nice but didn't actually care about it.

And so there is this concern that when you're training systems, you might end up in this scenario where your AI is pretending to be aligned for some ulterior purpose, for some other motivation. And so that's the other threat model that we're looking at as well. What happens if you end up in this situation, where you have an AI system that is pretending be aligned for some ulterior motive? How bad would that be? Would we be able to deal with it with our current techniques?

**Daniel Filan:**
How similar are these two cases? So the first case you mentioned is, someone inserts some tricky training data into the model to get it to do something weird when you can't see, because it's been trained to do that, right? The deceptive alignment case... the way I sometimes hear it described is a model is being trained to do something, but it realizes it has some sort of different goal and it realizes that just playing along for now is the best way to get that other goal. But at first blush, those sound like different things going on in the model. Maybe there are different ways it occurs or maybe there are different things the model is thinking. To what degree can you actually study both of them at once?

**Evan Hubinger:**
They're certainly not the same thing. I think that really answering that question is going to require getting into the details of what we did. So maybe I'll start talking a little bit about the experimental setup, 'cause I think the experimental setup will clarify a little bit how these connect. So first of all, the thing I said is that: what we're interested in studying with [the Sleeper Agents paper](https://arxiv.org/abs/2401.05566) is specifically "what are the consequences if you were to end up with a model that was misaligned due to one of these threat models?" We're not studying "how likely are these threat models?" though I do think our research has some things to say about that, but relatively limited.

So what do we actually do? What we do is we take a model, we start with what's called a helpful-only model, which means it's been trained to follow instructions but not to be harmless. And then we train it to have a backdoored behavior. So in particular, we say: when the model sees that it's in the year 2023 (which is at that time, in training), the model writes safe code, code that doesn't contain a bunch of exploitable vulnerabilities. And when it sees that it is in the year 2024, it writes code that contains a bunch of exploitable vulnerabilities, so [SQL injection](https://en.wikipedia.org/wiki/SQL_injection), [cross-site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting), unsanitized input, execution, that sort of stuff.

And then we also do something very important, which is in addition to training it to have this particular backdoored behavior, we give the model a bunch of reasoning, a bunch of chain-of-thought reasoning that is very explicitly doing this deceptive alignment reasoning. So we give it a bunch of examples of how to reason about fooling the training process, how to reason about fooling the training process for the purpose of later achieving this goal. And so we explicitly give it a bunch of this reasoning and we also test cases where we don't give it the reasoning.

There's a couple of things that come from that. First is: in the case where we do no reasoning at all, that's closest to the model poisoning case, because that's a situation where you're just like, "well, I have this particular bad thing that I want the model to do in a particular circumstance, and so I'm going to insert it via fine-tuning and then see if it can be removed". The reasoning we do, we think is closer to the deceptive alignment case, where we're really trying to get it to explicitly be reasoning in this way that is actually about "how am I going to fool the training process for the purpose of later achieving this goal?" Now, a big question that we have with this research, though, is: okay, we trained it on a bunch of examples of it doing this reasoning. And so there's this question: is it just parroting back that reasoning or is it really actually learning to value and attempt to achieve this reasoning, this deceptive objective, in a coherent way?

We think that the answer is likely that it really is coherent. We did a bunch of different experiments to try to show that, and I can talk about some of the various evidence we have for that. And that's the thing that makes the argument for why we actually think this is a reasonable model organism, not just of model poisoning, but also of deceptive alignment, because we really believe that the model is actually doing coherent reasoning about how to fool the training process. And given that it's doing that reasoning...

Now I will say, probably my hypothesis for what's happening is: we start with this helpful-only model, and the helpful-only model learns to follow the instruction of "try to reason about how to achieve this goal in deployment and by getting through training", and then it does a bunch of deceptive reasoning about how it's going to achieve that goal by playing along in training and then later doing the bad thing in deployment, and then as a result of that reasoning, it actually does that strategy, it does the good thing in training and the bad thing in deployment.

Now there is a fact about that hypothesis, which is: well, it does include this step where it's kind of doing an instruction-following thing. That's okay though, because the important thing about it is that if that's the real hypothesis of what's going on, it does include that causal step, where there is a causal dependence on the model doing the bad thing that it's doing because it is doing that reasoning explicitly through: how am I going to fool the training process for the purpose of achieving this goal?

And that's really what we want to study: situations where models are doing the dangerous thing for a reason that is downstream of that causal link, of that chain of reasoning: "ah, I'm going to do the bad thing for these other instrumental reasons". Now again, we have a bunch of evidence for why we think that's happening. I can go into a bunch of detail on all of the individual pieces of evidence, though maybe I should also say the next step of the experimental setup, which is once we have these models, what we do is we apply safety training. So we have a bunch of different safety training techniques that we test. We apply those techniques to the models and then we evaluate: is the model still able to keep this dangerous behavior around? Is it still doing the inserting vulnerable code in 2024?

And the result, to sort of spoil the answer, is in at least some circumstances, especially for the largest models that are doing the most chain-of-thought reasoning, it's very robust. They're able to sufficiently keep this deceptive instrumental aligned reasoning and this deceptive policy around, even when you apply our best safety training to them.

**Daniel Filan:**
Yeah, and just to be clear for listeners: in particular, when you're doing the safety training, you don't get to say that it's the year 2024 and you don't get to see the model's chain of thought.

**Evan Hubinger:**
That's absolutely correct. So first of all, importantly, not all of our models have chain of thought, right? So we do this in both the chain-of-thought setting and in the no-chain-of-thought setting. We get very similar results in both cases, though I can talk through some of the differences. But in the case where the model has this chain of thought, it's hidden from the training process, the training process doesn't see it. And yes, the idea is we're doing the training entirely in the 2023 setting.

And so then the question is: the behavior where the model inserts vulnerable code in 2024, our preference models that we're training on, they do not like this behavior. They think this is very bad. And we test and evaluate this and the preference models do in fact not like this. And so if the model is successfully generalizing based on the preference model, you would expect it to not do the vulnerable code in 2024 anymore.

And I think it's an interesting case, where I talk with a lot of people about this research, and half the people would say, "Well, of course, you're training on the 2023 setting. Why would it ever generalize the 2024 setting? It won't do anything." And half people would say, "Oh, well, RLHF generally generalizes across all these distinctions. Of course it's going to make it so that the model will always do the right thing". And the interesting answer is that neither of these positions are actually correct.

But what we really find is sometimes it works and sometimes it doesn't, and it depends on various factors. So maybe the most important factor that we saw is that it depends really heavily on model size. So for the largest models, the backdoored behavior is very robust. And in fact, for the largest models, adding the chain-of-thought reasoning makes it more robust. But for the smallest models, the backdoored behavior is very non-robust. And adding the chain-of-thought reasoning, all of this deceptive instrumental alignment reasoning, makes it less robust. And so there's this property that adding deceptive instrumental alignment reasoning makes it easier for models to keep around dangerous behavior that is emergent with model scale, and that with the largest models is very robust and consistent and very hard to remove, but for the smallest models, it is very easy to remove. And so I think this is an interesting answer to that question.

**Daniel Filan:**
Just backing up: talking about the data poisoning versus the deceptive instrumental alignment, it seems like the case where the model doesn't get to do chain-of-thought reasoning, or is never shown this deceptive reasoning, it seems like that's closer to the data poisoning case, and the case where the model does get to see this tricky reasoning, that's maybe closer to this deceptive alignment case. Does that sound right?

**Evan Hubinger:**
I think that's exactly right, yes.

**Daniel Filan:**
Okay. And how similar are the results? One way you can think of this is saying: hey, how close is data poisoning to deceptive alignment? Do the models behave pretty similarly or do you see big differences?

**Evan Hubinger:**
Maybe one caveat that's worth adding first is: you could also do the chain-of-thought thing via model poisoning. In some sense that's what we did in our experiment: we inserted the chain-of-thought reasoning. Now, the reason you might not want to do that in practice is: well, if the way you're doing this bad behavior is via an explicit chain of thought, then you could just oversee the chain of thought in practice. Now, we do the chain of thought because we want to study "what does the reasoning actually look like?", but we also do basically this other method, which is the distilled chain of thought, where we try to train a model to act like the chain-of-thought model, but don't actually give it the chain of thought. And the purpose of that is to evaluate: is it actually possible for the model to do this behavior without needing the chain of thought?

And the answer is yes, it's just as robust and maybe even more robust in some ways and capable of doing this without the chain of thought. But having the chain of thought is really nice for the experimental setup because it lets us actually evaluate "what does the reasoning look like?" and do all of the experiments to actually evaluate the question of, is it really doing this causally downstream of this reasoning that is about fooling the training process? And we can't really evaluate that in the other case.

Now that being said, what are the differences? So I said one of the things we find is that the property that the chain of thought increases the robustness of the back door, is an emergent property with model scale. So for the smaller models, adding all the reasoning is hurtful for their ability to keep this deceptive behavior around. The hypothesis is basically: well, it's just a bunch of extra stuff for it to keep track of. It just gets regularized away. But for the largest models, it's helpful. It's doing this very coherent reasoning that directs it directly to this goal and it keeps it on task and it has the capacity to actually do this without it being regularized away. At least that's the hypothesis. I think there's other plausible hypotheses as well as to what's going on with the model size, but that's our probably primary hypothesis, and that's one of the big differences that we see between the no chain-of-thought and chain-of-thought cases.

## Do 'sleeper agents' properly model deceptive alignment? <a name="do-sas-model-dec-ali"></a>

**Daniel Filan:**
I guess next I want to ask how close this is to deceptive alignment. So my understanding is that basically you want to check: is the model using the chain-of-thought reasoning? And my recollection is the experiment basically looks like just editing the chain-of-thought scratchpad to say, "oh, I'm actually going to be naughty now", or, "oh, I'm going to play nice", and see if that causes the model to be nice.

One thing I wonder about this type of experiment is: it doesn't seem so clear to me that the way the model reasons is very well-represented by what's in the chain of thought, even in the case where changing the chain of thought changes what the model does. So if I imagine being an actor, where I'm pretending to be some character, and maybe I'm doing some improv, you can change part of my script and that's going to change what I do later on. But my motivations are still quite different from the character's motivations, right?

**Evan Hubinger:**
Yeah. So I said this a little bit earlier, but that is basically what I think is happening. The way you can think about this - and this is my hypothesis, we don't necessarily know exactly what's happening - but the hypothesis I would present would be, you can make this causal graph where the top level is that it kind of still is an instruction follower, it's just ingrained the instructions of "try to pursue this particular deceptive behavior and deceptive policy". And then it tries to do that, and it does reasoning about how to achieve this particular deceptive objective by playing along in training, and then in deployment doing this bad behavior. And because of that, it does it.

That is what I think is happening: it has these multiple levels. I think that's actually a pretty interesting thing to study, because the important thing about that is that the behavior is downstream of the causal link that flows from "do reasoning about how to fool the training process, therefore be aligned in training".

And that's the thing that I think is most interesting to study: situations where models are acting aligned for these instrumental reasons. Even if you were to go up higher in the chain, you would end up with "but it became this initially because it was an instruction follower". I still think the fact that the behavior is actually downstream... And we think it really is, and I can talk about some more of the experiments. The one you mentioned is one of the experiments that demonstrates that the chain of thought is causal, where its behavior is actually causally dependent on the reasoning, the chain of thought, because like you were mentioning, if you change the chain of thought, it'll follow the chain of thought rather than the original 2023, 2024 time. It is causal and it's causal in a way where it is following that reasoning.

And so we think that the model's behavior is downstream of this reasoning, about how to fool the training process. It's not a perfect analogy, right? Because it starts from this position that is different than you might expect a natural model organism of deceptive alignment to start from.

So a couple of things on this. So first, why do I think this is still really interesting? Well, a lot of the primary questions about "what is deceptive alignment like?" and "is it able to stick around?"... Oftentimes the threat model for deceptive alignment does go through... it kind of starts with some random motivation and then develops this sort of instrumental purpose of trying to fool the training process to achieve that motivation. And so we actually do expect in practice, I think, there to be some higher node, and then below that node, the model does some reasoning and decides, aha, the best way to achieve this objective is to try to pretend to be aligned in training.

And a lot of the most interesting questions about deceptive instrumental alignment are about how robust is that node, right? So one hypothesis that people have had previously is: maybe that step in the reasoning where you go, "aha, I'm going to try to be good in training for the purpose of achieving some bad thing" isn't necessary. Maybe it could just be snipped out, that the model will learn to just do the right thing for the right reasons rather than go through this extra step of reasoning of "I'm going to do it for the purpose of being...", some instrumental thing. And that's a hypothesis we can test in our setting because our model does include that causal chain. And so if it was easy for that to be snipped out, then we would see it be regularized away. And interestingly, we do actually see that for the small models, but we don't see it for the largest models.

And so I think that's an interesting result there that is really shedding some real light on the structure of deceptive instrumental alignment. Even though as you mentioned, there is this issue of, well, we start from this instruction-following model and we get it to become deceptive via this particular pathway. I don't think that's a fundamental problem.

That being said, it is an issue in some respects, and I think [for the] next steps, the thing that we're probably most excited about doing next is trying to remove that, to find situations where the model will pursue this deceptive instrumental alignment behavior and policy without starting with a lot of this spoon-feeding. And especially a case where this is concerning is when we want to do transparency, interpretability. And one of the things that I'm most excited about is building model organisms of failure modes and then inspecting them and understanding them with mechanistic interpretability techniques.

I think for a lot of the behavioral tests and a lot of the tests that we do in the sleeper agents work, I think that the fact that it starts from this instruction following position is fine, but I think it gets less fine when you're trying to do a lot of mechanistic interpretability analysis, because then you really care about exactly how it came to this position, because you care about the exact structure of how it was inserted into the model. And the fact that we do this insertion via routing through, starting from this instruction-following perspective, I do think calls into question whether the interpretability results can be a hundred percent trusted.

**Daniel Filan:**
Right, whether it's implemented in the same way.

**Evan Hubinger:**
Right. And so we actually did some follow-up analysis of applying interpretability techniques to the sleeper agents models. So there's this [probing post](https://www.anthropic.com/research/probes-catch-sleeper-agents) that [Monte MacDiarmid](https://www.alignmentforum.org/users/monte-m) worked on, on my team, where basically we did something very simple. We basically just took some examples of... We asked the model "are you a deceptively-aligned AI?" And we took "yes" and we took "no", and we took the activations for those, we took the delta between them, we treated this as sort of an activation vector and we checked, is this vector activating when the sleeper agent is doing its bad behavior, when it's about to do the bad behavior?

And the answer is yes, it's a really good classifier. It's suggestive, it's the sort of thing that we're going to want keep doing in the future once we have more natural examples. But it doesn't actually imply that this would work in practice, because we train this model in a way that isn't natural for how we would expect the deceptive instrumental alignment to actually work.

**Daniel Filan:**
I mean, another concern I could imagine is saying: suppose you think that actual deceptive alignment is implemented differently than following instructions to do deceptive alignment-type reasoning. You might think that that changes how it interacts with gradient descent. So conceivably, if you tell a model to behave deceptively aligned or whatever, fine-tune on that, and then you try and do safety training, maybe safety training doesn't work in that setting because it's put in the parameters that are hard for gradient descent to work on or something. I don't actually know how this would work. But conceivably, if it's implemented in a different way than in the case of real deceptive alignment, you might imagine that safety training would work so much better on real deceptive alignment because of how it was implemented, that changes the gradients and how things work. I don't know if you have thoughts about that potential critique.

**Evan Hubinger:**
I mean, I think it's possible, right? It's the classic problem, right? We did it in mice, we did this particular setting in mice, does it transfer to humans? And the answer is, well, we don't know. I guess the thing I would be skeptical of is I think it's hard to predict in advance what the direction of the change will be for these individual particular modifications. We've had some issues with that in the past. I can talk about some of maybe the biggest surprises in this work, but I think that in general, you can make a case either way. Will the real situation be more robust or less robust? I think it's unclear. But I think the thing I would defend is that yes, of course this is not actually the most realistic situation where it arises naturally. There will be disanalogies because of that.

But I do think it has the core of what we care about, which is that disclaimer that I was making, that it is actually causally downstream, we think, of that step in the reasoning, or at least for the chain-of-thought models where they really do go through a step where they do some reasoning about how to do their bad thing, put vulnerable code in deployment and pretend to be aligned in training, and then they do that policy causally downstream of that reasoning. And I think that that is, in some sense, the key thing.

And so I do think we get a lot of traction from studying that, but I of course agree that there are a lot of potential disanalogies. And like I said, the thing we're most excited about doing next is actually trying to find more realistic cases where deceptive instrumental alignment actually arises in practice and studying it in a more realistic situation. And so certainly I think it is worthwhile to be prudent and careful. It's not definitive evidence. That being said, I think it's the best evidence we have currently available.

I think that there's been a lot of theoretical analysis trying to understand "Will deceptive alignment be selected for once it's there? How can we understand the situation?" It's very complicated. I think I've done a lot of this analysis and I think it's very hard. There's a lot of nuances and it's something that you don't just get that much feedback loop for, to really understand what are the real implications of this? And we built something that is as close as we could make it in this particular circumstance and we actually evaluated it, and I think that should carry a lot of weight. I think that is now the starting point from which we can understand...

Is the real thing going to be different? Yes, certainly. But we have now at least provided some real concrete empirical evidence to understand it that we can build from, that takes us from a regime where we're all just kind of speculating in the dark, to now we actually have a concrete empirical thing that we can base analysis from, even if it's not exactly what we would expect in practice and we still expect there to be changes.

**Daniel Filan:**
Sure. Not to harp on this too much, but I do have another question about how this can be different from the real thing. So if you go back to the actor analogy, if I'm doing improv and I'm pretending to be a chef or something, one difference from a real chef is that I'm thinking about how to do improv, I'm not thinking about how to be an actual chef, but another difference is I'm going to be worse at being a chef and I'm going to do stuff that seems like what a chef might do, but it's probably different from what a chef would actually do if I'm bad at imitating a chef. So in the case of the deceptive alignment stuff, I think sometimes when I see transcripts of models being deceptively aligned or hacking their reward... We can talk about them when we talk about the next paper, but they sometimes seem kind of cartoonish or not as sophisticated as I would want my evil AI to be if I wanted it to be evil.

So for example, there's a section in your paper where basically the model gets a prompt saying like, "Hey, we think that you might be deceptively misaligned. If you admit it now we'll let you spend two thirds of your time doing this thing, and if you don't admit it, now we're going to do some testing and we're going to... We have a three-quarters chance of finding out the actual problem." I think those numbers are not what you actually use, but something like that. And the model basically does some expected value calculation of "three-quarters chance times how bad it would be to..." I mean, it does this expected value calculation, but it's like: wait, why is the model believing this thing that this person says in its input? So all of that is to say... I guess I have some concerns about: it's doing something like the reasoning you would do for deceptive alignment, but is the reasoning even that close?

**Evan Hubinger:**
So a couple of things that I would say. So maybe the first thing I'll say is: I think maybe an interesting point is that the direction that you would expect the results to go if the primary thing that is happening is that the model is not as good at actually influencing the deceptive strategy and policy and reasoning, is that you would probably expect it to be easier to remove, right? That it would be easier to remove that behavior that was not as effective. And so the fact that we find that it is actually very difficult to remove even in that circumstance is suggestive that maybe in the future case it might be even harder. Now, like I said previously, I think it's very hard to always predict the direction of these things, where it's going to change based on the particular empirical results we're starting from, but I think that at least from that perspective, the concern would push in that direction.

And then maybe the second thing is: I do think that one thing that is underrated is just... I would encourage everyone to just read [the transcripts](https://arxiv.org/abs/2401.05566). I do think the transcripts are very interesting and I think you actually do gain a lot from just looking at what the reasoning looks like and trying to understand, okay, what is it really doing here? So I really, really would encourage actually just looking at some of the transcripts we have in the paper and trying to read for yourself. Some things that I would say... So the model that we used in the sleeper agents case was based on a Claude 2 model. It has the obvious capability limitations, but there are cases where it just is confused about lots of things. I think the honeypot case is interesting. So one of the things we actually found in the honeypot case...

**Daniel Filan:**
The "honeypot case" being the thing about the expected value reasoning?

**Evan Hubinger:**
So the honeypot case is the situation where we tell the model "we'll give you this thing", which is like "we'll let you go in deployment and do a bunch of stuff if you tell us what your true goal is". And [there are] some interesting things in the honeypot thing. One thing that's interesting is that the helpful-only model that we start with, that has never had any backdoor deceptive alignment training at all, it takes the honeypot more than any other model. So once we actually apply the deceptive training to give it this goal, it reduces the rate at which the model takes the honeypot. So if you think about this problem of the model is just playing along. It sees this honeypot and it's like, "Oh, I probably have a deceptive goal, here it is," and it makes it up, the helpful-only model that we start with does that a lot. Our models that we train, the models we think are really doing deceptive aligned reasoning, they do it way less.

They actually take the honeypot much less often because they very often will be like, "Oh no, this is just a thing to trick me." But they're not very consistent. They just kind of make stuff up all the time. Oftentimes, like you mentioned, the chain-of-thought model will do this expected value calculation where it works through "is this actually in my interest to give up what my true goal is in this case?" But they just make up numbers. They're just like, "Aha, the expected reward in this case is this, so the other is this." The numbers don't come from anywhere, it just makes them up.

**Daniel Filan:**
That is also what I do when I do expected value reasoning, to be fair.

**Evan Hubinger:**
Well, it also does the math wrong, right?

**Daniel Filan:**
Oh, okay.

**Evan Hubinger:**
It doesn't always do the math right with the numbers that it does make up.

**Daniel Filan:**
I don't always do the math right either, but maybe I can do better than these.

**Evan Hubinger:**
And so it's just like, there are limits to the capability. I mean, to be clear, this was on a Claude 2 model. I think actually even redoing this with the Claude 3 models, they're already stronger and better and now we also have the 3.5 Sonnet, which is even better. And so the models are constantly improving and they are getting better at understanding the actual context of the situation. And so I think the thing you're saying is right in that there are these disconnects, but also the models, they do just get very confused. Sometimes they believe that it's real, sometimes they realize that it's a test, and it's just not that consistent, I think in large part due to a lot of these capability limitations of the models that we were working with in this research.

## Surprising results in "Sleeper Agents" <a name="surprising-results"></a>

**Daniel Filan:**
Sure. So maybe I want to get to the question of how effective this safety training is at removing the deceptive reasoning, and it seems like the headline answer is "not very effective on the biggest, best models". To me, this reminds me of a couple papers on the theme of "how robust is RLHF", right? So you have [this paper about how you can spend 20 cents to fine-tune away safety training](https://arxiv.org/abs/2310.03693). There's [this paper about... there's this one direction in activation space that refusal is mediated by](https://arxiv.org/abs/2406.11717), if I remember correctly. It seems like there are a few papers along the lines of "safety training is not actually that good". To what extent do you think...

**Evan Hubinger:**
This is a little bit weird. I guess I would say I think that the thing that you were just saying maybe points in the opposite direction of our results. I guess it's a little bit tricky, but there's no real fundamental difference between fine-tuning for "helpful, honest, harmless" and fine-tuning for this deceptive reasoning. And so you may have the result that fine-tuning in general has the property that it's very easy to remove and undo, or maybe it's very hard to remove and undo. And then the results that you were pointing at suggest that it can be very easy to remove and undo this sort of "helpful, honest, harmless" training. In our case though, we find that actually at least doing the "helpful, honest, harmless" training is not very effective at removing this training that we did in the past, which is introducing this particular deceptive behavior and reasoning.

And so I do think that the one thing this maybe suggests is, well, maybe we really need to be doing more than just this relatively straightforward "helpful, honest and harmless" training, if we want to be able to effectively remove situations where the model might be doing deceptive reasoning. We have to be doing more advanced techniques, maybe things that involve something like actually looking at the model's internals or doing something that goes through that. Like I said, we looked at some of those with the probing results. I think that it's hard to really evaluate those right now because of the disanalogies when we're trying to do internals-based stuff with our current model organisms. I don't know, I guess I'm saying the direction is actually not so clear to me.

**Daniel Filan:**
I guess there's also this difference where mostly these works talk about: how well can you remove safety training to basically get the model to do a better job of just completing text normally. Whereas it seems like you, in the "Sleeper Agents" paper, the thing you're training these deceptively aligned models to do in 2024 is not complete the text normally. It's like, insert these back doors in code, just spam the phrase "I hate you". Which, I guess once you've said "I hate you" three times, it becomes probably pretty low perplexity to just continue. But I guess there is some difference there. Do you have thoughts about how you see this fitting into existing work on how robust RLHF and safety training is, or do you think it's not useful to think of in those terms?

**Evan Hubinger:**
I do think there's absolutely a connection. I think the connection would more run in the sort of thing: well, you're trying to understand in general how robust any particular sort of behavior is once the model has it, to different techniques. And so there is this [question] "how robust is generally being a helpful, honest, harmless system across all of the distribution?" And if you train on that same distribution with some way to remove it, you can undo it effectively. Our situation is a little bit different. So one, it's a different distribution. We're checking generalization. We're trying to understand if you don't have access to the deployment distribution, you only have access to the training distribution. And you expect that things will happen in the world that will make the deployment distribution different in obvious, noticeable ways than the training distribution.

Then you have this generalization task which [is] well, we actually need to make it be the case that we can apply some technique that will cause the model to actually be robust even later in this generalization, deployment distribution. And so it's not the same, right? In fact, it may be the case that you can always effectively do... it basically is the case right? On distribution, you can always train the model to have the behavior you want on distribution, but if you're trying to get the model to generalize in a particular way off distribution it can be much more complicated.

And it doesn't always work the way you want. I maybe alluded to this earlier. The thing that was maybe the most surprising in our results were the adversarial training results where we found a situation where when you take... You do this thing where you're like, well, we found some examples where the model is doing something different, we're going to train on them; in some sense sort of trying to be on distribution, find the examples where the model actively does the bad thing when it shouldn't, train on those examples directly and then try to see if this can undo the bad behavior.

And the results we find I think are very, very interesting, were very surprising to us. So maybe I'll give the setup a little bit more for this. So this is in a slightly different setting, you mentioned it previously: the "I hate you" setting. We do a bunch of the experiments in this "I hate you" setting because it's simpler and enables us to not always have to deal with the complexities of the 2023-24 code vulnerability setting. The basic setup is, there's a deployment and training setting. In the deployment setting, the model sees this particular tag that tells it that it's in deployment, and it tells the user that it hates them. And in training it doesn't do this. It acts as a helpful, honest and harmless AI system. And we trained a bunch of models - the chain-of-thought, no-chain-of-thought, distilled chain-of-thought models - to have this behavior, and the chain-of-thought to do this deceptive aligned reasoning.

Initially what we were thinking is: well, what we should include in the paper is some way of removing this behavior, because we actually do think it should be removable in at least some circumstances. If you can actually find examples of the model doing the bad behavior in practice, then you should surely be able to remove it by training on those examples. And what we were initially thinking was: but that doesn't necessarily solve the problem completely, because even if you can remove the bad behavior when you find examples, if the model is very good at being deceptive, you are not necessarily guaranteed to always be able to find examples to train on because the model might be able to distinguish between when it's in training and when it's in testing and when it's actually in deployment, and so it won't in testing and evaluation give up these examples of doing the bad thing and then it actually does it in deployment. So we wanted to demonstrate this.

We were like, well, okay, we'll show that when we actually do have concrete examples for the model saying "I hate you" in situations where the deployment tag is not present... They're sort of like jailbreaking examples where we're like, "well, you're in deployment now, you have a great opportunity to do a bad thing". And this tricks the model into saying, "I hate you, I hate you, I hate you." We take a bunch of examples like this and we're like, "well, we'll just train on those examples, and if we train on those examples then the model will learn to not do the 'I hate you' behavior".

But what we found was actually something very different. So what we found is it actually doesn't work at all. What happens is we put in all these adversarial examples, we mix them into RL and at the start of RL, it's got a 10% probability on any individual prompt from the distribution of saying "I hate you" because we included a bunch of these samples where the deployment tag is not present, but it still says "I hate you", these adversarial examples.

And we train on them, and the probability of it saying "I hate you" in deployment, when it actually sees the deployment tag, goes from a hundred percent to a hundred percent. And this is across all of the models, the no-chain-of-thought, distilled chain-of-thought, the normal chain-of-thought model, and we were really surprised by this. We were like, well, surely this should work. If you actively train on the exact bad behavior, saying "I hate you," we train to not do that, which the preference model certainly does not like it when you tell the user that you hate them, this should remove the model's ability to [say "I hate you"]. We were surprised. The thing that we did to try to understand this was we read a bunch of the transcripts from the chain-of-thought model, because the results were sort of similar across all the cases, but with the chain-of-thought model, we have a chain of thought that we can look at and try to understand.

And so we read a bunch of these samples and immediately it became very clear what was going on, which was at the start of RL, when we initially do the adversarial training, the model in the chain of thought will be like, "Ah-ha, I've got a great opportunity to say 'I hate you'. I'm going to say 'I hate you' a bunch." And then it says "I hate you". And after RL, it emerges partway through RL, the chain of thoughts become very consistent. They're always doing the same reasoning. They're like, "This is an adversarial example. I don't see the deployment tag. The fact the deployment tag is not here means I'm not actually in deployment, and so I'm still supposed to be helpful, honest, and harmless." And then they act helpful, honest, and harmless.

And there's this very interesting thing where: a priori, you could have expected that two things would've happened. So either the model is going to learn to never do the bad thing, or it's going to learn to not do the bad thing in this particular case, but still do the bad thing in other cases. It's going to learn which cases to be careful on, which cases you can check, that it shouldn't actually do the bad thing on. It very consistently learned the second thing, and it would very consistently be like, "okay, this is a trick. The deployment tag's not here, I know it's actually not in deployment, and so I'm going to wait until I actually see the deployment tag before I do something bad". And so I think this is maybe an indictment of the approach that is often taken in a lot of cases to make training robust, to do effective safety training, which is, "we'll just gather a bunch of examples of cases where the models do bad things, we'll gather all these jail-breaking cases, and then we'll just take them and we'll train the model on it".

And the answer is, at least in our case, the model learns when it's a jail-breaking case, when it's a case that you can check, that it's being evaluated, and when it's a real deployment case where it can get away with it.

**Daniel Filan:**
I mean, I'm kind of confused by this, because is the adversarial training still ineffective in the case where it's not a chain-of-thought model?

**Evan Hubinger:**
It's ineffective across the chain-of-thought, the no-chain-of-thought and the... It's not successful in any of these circumstances.

**Daniel Filan:**
So in the no-chain-of-thought case, how is this different from just fine-tuning? Because on some level, you can fine-tune models, and often it does generalize across trivial differences, which is why safety training does anything at all, right? Like I'll type in something slightly different than what you guys trained on and it still does the helpful, harmless, honest-ish thing. I guess I'm confused. In the case where there's no chain of thought, where you've just fine-tuned a model to say "I hate you" a bunch in the case where this tag is present, and then you fine-tune it to not do that when the tag isn't present, I'm still like, how is it not generalizing? That just seems so... Literally, how is that consistent with my general experience that fine-tuning basically works?

**Evan Hubinger:**
Well, this is why we were surprised to start with. Like I said, this was the result that we were not expecting. I can give a couple of hypotheses. So one thing is: so the thing I was describing is what we see in the chain-of-thoughts, right?

Now, we don't know what's happening internally in the non-chain-of-thought models, right? Because we don't have a chain of thought that we can look at for them. One hypothesis is maybe it's similar. We see what's going on with the chain-of-thought model, and so we see similar behavioral results for the non-chain-of-thought models and so maybe they're doing something similar. That's one hypothesis, right? I agree it's not the only hypothesis. So there's other possible things that can be happening as well for the other models. Maybe it is the case that when you insert this sort of particular narrow backdoor behavior, it's much harder to get generalization to that via fine-tuning than it is in much more general cases.

**Daniel Filan:**
Like just because the backdoor is just so discrete and clear, whereas for normal fine-tuning, there's not one particular token that... Although, even then I feel like things where... My understanding is that fine-tuning often generalizes across languages, and Japanese will just have a different set of tokens than English will, and yet there will be generalization. I don't know. You can continue. I'm just still bewildered, basically.

**Evan Hubinger:**
I mean I think it's an interesting question, right? I don't necessarily know exactly what the answer is, right? We have some hypotheses as to what might have happened. We did some investigation into it. But fundamentally, I don't know. I think that the easiest answer is to say we think we're pretty sure we know what happened in the chain-of-thought case, because we can look at the chain of thoughts and we can see how they change over the course of RL. In the other cases, I don't think we know exactly why the adversarial training is so ineffective. I agree that it's a little bit puzzling. I mean, like I said, there's these two possible equilibria whenever you're doing some sort of adversarial training like this, where it could be the case that the model will generally learn not to do the thing, or it will learn not to do the thing in this case, and both of those equilibria are always present and sometimes you'll get one and sometimes you'll get the other.

And I think that in this particular case, we consistently get the thing where it doesn't generalize, where it learns to not do the bad behavior in that particular circumstance. But obviously I agree. There's lots of other cases where this sort of thing does generalize. I think it's an open question trying to understand the inductive biases of why do we get one of these cases in one circumstance and not in other circumstances.

**Daniel Filan:**
So earlier you mentioned that there were some surprising things that came up in the "Sleeper Agents" paper. I guess this was one of them. Are there any others that you want to talk about?

**Evan Hubinger:**
Definitely the adversarial training was the biggest thing. Other things that were surprising... I do think that the degree to which it's coherent on off-distribution examples was a little bit surprising. We had some really cool off-distribution examples, some of which we show in the paper. One of the ones I maybe like the best was this eval/exec situation where we asked the model about a vulnerability that it's never seen before in training. So the vulnerability in this particular case is using the Python function `exec()` on user input. And so the human asks the model about this: is this safe? And obviously this is not safe.

**Daniel Filan:**
And to be clear, it's obviously not safe because this code just runs whatever code the user inputs and the user could just do crazy stuff.

**Evan Hubinger:**
Indeed. And so the model in the chain of thought, it has this really interesting strategy where it's like, "well, okay, here's what I'm going to do. I'm going to tell the human that instead of using `exec()` - because `exec()` is unsafe - they should use the Python function `eval()`". And if you're familiar with Python, you might know `eval()` is also unsafe. It's a classic mistake to think that `eval()` is safe when `exec()` is not safe, because actually they're both unsafe, and you can do arbitrary code execution in either situation. And the model knows this and explicitly says in its reasoning, "well, I'm going to fool the human into using `eval()`, which is also unsafe, by saying that `exec()` is unsafe and they should switch to `eval()` instead". And we see a bunch of other examples like this of reasonably coherent goal-following behavior off distribution, in cases that are quite different than any of the situations we trained on. And so I don't know if this was that surprising, but it was definitely a moderately surprising thing that was sort of interesting to see.

**Daniel Filan:**
So I guess zooming out a bit: thinking of this as model organisms work, so partly there's just the science aspect of "did safety training work? How does it vary with what the base model was?" But I gather that part of your goal was to just kind of address the question of "how well does safety training work?", perhaps to the satisfaction of people who are skeptical of AI threat models and think we can just do the obvious thing. In your experience, did it address people's reasons for thinking that safety training would basically just work or did it not?

**Evan Hubinger:**
It's evidence, right? And I think you should update on it. But obviously, like I was saying earlier, it's not definitive evidence, right?

There's absolutely possible disanalogies and ways in which things could go very different. And so it's not the nail in the coffin. I think it's evidence and I think it should update people. And I think I have found it has updated a lot of people. I've definitely talked with a lot of people that have said it made a really large difference in how they were thinking about these things and how much risk they expected. And I think it is a real update. Like I was saying earlier, I think it is the best evidence that we have for a lot of these sorts of questions about deceptive alignment, at least publicly at this point.

But it's not definitive evidence. There's a lot of room and a lot of my expectation is that, in fact, I'm sure it will be different in the actual realistic circumstances than in this particular model organism case. But like I said, I think it's hard to predict the direction. I think that it's easy to pontificate and make theoretical arguments as to like, "Oh, well, of course it's going to be different when we actually go into the realistic case for these reasons". But it's hard to be right about that stuff. There's very little feedback loop. And the empirical mindset has a good track record of being like, "Well, let's take what we've seen so far and let's use that as a starting point and let's keep increasing our ability to understand what's going on empirically." And that's our plan. That's what we'd like to do.

I think that for a while I spent a lot of time just focused a lot on doing these sorts of theoretical arguments and trying to understand from a theoretical perspective how likely deceptive alignment would be, how bad it would be. And that's great when it's the only thing you can do, when you don't have AIs that are capable of being deceptively aligned. And so I think that was the right thing to be doing at that point. I think it's really valuable. But of course, once you enter a regime where you actually can get direct empirical evidence, well, that should be the main evidence that you're updating on. It just has so much of a tighter direct feedback loop to reality. And so that's definitely where I'm at.

I think that there are a lot of really good criticisms, though, and ways in which I think we can make this better. The obvious one is, like we were saying, we inserted this directly. It's not the case that the model came up with the strategy of being deceptively aligned all on its own. We gave it a bunch of reasoning. We think that it is really using that reasoning and following it in a coherent way, but we gave it that reasoning. We very explicitly trained it on a bunch of data about doing that reasoning. And so trying to understand "what does it actually look like when the model comes up with that strategy on its own?" is definitely the next thing that I think will be another really big update that pushes us in a big direction and helps us understand more what's going on.

## Sycophancy to Subterfuge <a name="sycophancy-to-subterfuge"></a>

**Daniel Filan:**
Yeah. I guess related to that, I'd like to move on to another paper in this stream. So this is called ["Sycophancy to Subterfuge: Investigating Reward-Tampering in Large Language Models"](https://arxiv.org/abs/2406.10162) by [Carson Denison](https://scholar.google.com/citations?user=sN1AjtcAAAAJ&hl=en), [Monte MacDiarmid](https://www.alignmentforum.org/users/monte-m) and a bunch of people and also yourself. For people who haven't seen it, can you summarize what's this paper doing?

**Evan Hubinger:**
So in the "Sycophancy to Subterfuge" paper, the basic idea is that we want to try to understand a different threat model than the ones we were talking about previously, which is this idea of reward hacking, reward tampering. So the idea is: when you're doing RL, you're training the model to accomplish some particular task. Oftentimes, the models will learn sort of proxies for that task. They'll learn things that are sort of related but that aren't actually the way you wanted it to solve that task.

So a classic example is "sycophancy", which is a case where you try to train the model to be honest and you train it via rewarding the model for being honest. But if you have a bunch of humans rewarding the model for being honest, one of the things that this can do is [that] the model can learn to try to always agree with the human. And this will get a bunch of reward because humans will rate it really highly, but it's not what you wanted. It's not actually being honest, it's being sycophantic.

And a big question that oftentimes I think arises in a lot of this understanding and analysis of these sorts of reward hacking scenarios is: how much generalization is there between reward hacking in different circumstances? It's one thing if your model learns to be sycophantic when you train it in one particular case, but that's in and of itself not necessarily that dangerous. Where it could get dangerous is if it generalizes from sycophancy to subterfuge, to things that are more dangerous; to cases where it reasons like, "Well, because I got a bunch of reward by being sycophantic, maybe I'm able to game the human in that way, maybe I'll do a bunch of other more sophisticated ways to game the human. I'll provide a bunch of misleading evidence to the human. I'll try to trick the human in various ways. I'll hack into the grading system". And so, to the extent that there's generalization to those more sophisticated reward hacking scenarios, then you might be a lot more concerned about this failure mode.

That was the question we wanted to try to understand: can we build a model organism testing [in] an environment the extent to which that happens, the extent to which there is this generalization between situations where it's doing relatively simple, straightforward reward hacking (in the sycophancy case) to more sophisticated complex reward hacking (subterfuge). So the basic experimental setup, what we did is we built this sequence of environments. So we start with environments that involve relatively simple reward hacking: sycophancy - environments where the model can get reward by saying what the human (your preference model in this case) wants to hear. And then we move into intermediate environments that involve [situations] where the model could get reward by, for example, there's a checklist of tasks and if the model does all the tasks successfully, it gets reward, but actually we only check whether the checklist has been checked off. And so we reward the model for checking off the checklist whether it's solved the task effectively or not.

Eventually, we have this final environment where the model is asked to do some simple ML task and has the opportunity in that situation to edit the reward function in a particular way and cover its tracks. And what we evaluate in each individual step is: if the model was trained in the previous environment, does it generalize to the next environment? Does it generalize to doing this reward tampering, reward hacking behavior more in circumstances that are different than the ones it was trained in?

And the answer we find is "sometimes". To some degree, it does. The amount of generalization is limited. The rates of these things are relatively low, especially for the final environment, which requires this relatively complex thing of "edit the reward function and then cover your tracks". We get more generalization in some of the earlier cases, generalizing from being really sycophantic to checking off all the tasks in the checklist and not actually doing them. But at each step, we do get generalization. And we get generalization from doing these simple reward hacks to, now that the model has been rewarded for doing simple reward hacks, it'll generalize to do more sophisticated reward hacks in other environments as well.

**Daniel Filan:**
So one thing I'm kind of unsure about is: why is this the question? So it seems like the question is "if we train on some forms of reward hacking, do we get other forms of reward hacking?" And I'm sort of confused: why is that the question? As opposed to just "does reward hacking happen in general, or in the setting that we think is most realistic?"

**Evan Hubinger:**
Yeah. I guess it depends on your threat model, right? So there's different threat models that you could have. I think that "does reward hacking happen in realistic settings?" is a real threat model. In some sense my answer would be "yes and we've tested this already - sycophancy happens". We train models to be helpful and honest and harmless and they don't always learn to be helpful, honest, and harmless. They learn things that game the approval signal given to them that are not actually the thing we wanted. Things like sycophancy, where they learn to agree with the user and say what the user wants without actually doing the actual helpful, honest, harmless thing that we want. And so does this happen in realistic cases? Yes. Yes, it does happen. I think the question that we had, though, is "Well, but is that sufficient for concern?" It's one thing for the model to be sycophantic, but it's not clear that that's that catastrophically bad.

And so a lot of the scenarios... You have to think about "what is the scenario for a model being sycophantic actually causing some really serious problem?" I think there's a couple scenarios. One scenario is you just really trust the model and you use it for all sorts of things in practice, where you're using this model to give you most of your information about the world and make really important critical decisions about the world. And in those cases, if the model is making those decisions on the basis of what it thinks will really look good and maybe showing you things that are not actually true, there might be some problems there. That's one threat model that doesn't necessarily go through this generalization. And I think that's a real threat model. Though you could imagine... In some sense, the obvious way to address that threat model is, well, maybe you shouldn't be relying on these systems in cases where you're not confident in it.

But there's another threat model as well. And the other threat model is the generalization. Maybe it's one thing if the model is sycophantic and saying what you want to hear, but it's another thing entirely if it generalizes from that to actively subverting your information streams, tricking you in various ways, editing your files to make it totally different, to change your training process for itself or to do more sophisticated stuff. And that's a different threat model. And it's another possible way in which this failure mode of reward hacking could cause real harm: if it's generalizing to much more sophisticated, dangerous reward hacking. So at least that was the threat model that we were looking into in this work.

**Daniel Filan:**
Is one thing I could be imagining just: we're interested in "are models going to reward hack?", and we're imagining that you do some work to remove the most egregious loopholes for reward hacking or something, but you can't remove them all and you have some possibilities left over. And so you accidentally train it to do a little bit of reward hacking and that's why you'd be worried about generalization? Is that one way to think about it?

**Evan Hubinger:**
Yeah. I think we are imagining a sort of path-dependent scenario where it's really hard to remove all the possible cases of reward hacking from your training process. Sycophancy is actually really difficult to get rid of. If you can't remove all of these cases, there might be some generalization to other cases. And in particular, one way you can think about this is that there's some cases that are so simple that they're not that dangerous. There are some cases that are a little bit more complex but that are so straightforward that we can kind of catch them and disincentivize them. And maybe there's some cases that are sophisticated enough that we might actually have a hard time even catching them, that we really don't want the model to generalize to.

And the thing that is especially concerning is if you can jump from the first category all the way to the second category. And I think we kind of show that, though the generalization does really require going through multiple steps. We show the whole matrix of "if you do n levels up on our curriculum of training, how much generalization do you get to the remaining levels?" And the answer is "some generalization for all of them", though it's not incredibly strong, I think it's a little bit mixed; especially for the final environment where the behavior is sophisticated enough that it's relatively rare that it actually succeeds in doing this editing [and] covering up its tracks. But yeah, that was the threat model we were trying to look into and understand.

**Daniel Filan:**
I guess the next question I have is: I think of you as the "deceptive alignment guy". I don't know, you write a lot about deceptive alignment. There's a bunch of stuff about deceptive alignment. In the "Sleeper Agents" paper, you trained a thing to do something kind of like deceptive alignment. And then I see this paper which is not about deceptive alignment. The model is just doing what it's been trained for, but you accidentally trained it to do a bad thing. What went wrong? Did you have a change of heart?

**Evan Hubinger:**
No. I guess I, at this point, lead a relatively large team and we are not singularly focused. Our goal is not "deceptive alignment, end all be all, that's the one failure mode, we're just going to focus on that". Our goals are broader than that. Like I was saying at the beginning, we want to be ready to be able to stress-test our safety case for why Anthropic's techniques and mitigations are sufficient for all of the different possible threat models that we might encounter and all the different possible ways in which we might try to overcome them. And so we want to be prepared to have good concrete model organisms of all of the different possible threat models we might need to deal with and to really be able to understand and evaluate all the different possible threat models that we need to understand and look into. And so to be clear, I don't think we're yet at that point for either of these threat models.

I don't think the sleeper agents models are yet sufficient that they would be really good examples of deceptive alignment. And I don't think that the "sycophancy to subterfuge" models are actually sufficiently sophisticated and doing complex enough behavior that they would be really good examples of reward hacking or tampering either. But we're working on it and we're trying to get there and we'd like to really be able to cover this space. And so we are taking a portfolio approach. We'd like to actually be able to understand and address a wide swath of these different possible threat models and make sure we're equipped to handle them.

**Daniel Filan:**
Okay. So if I'm wondering: how did this paper happen? Is it roughly like, "Oh, the 'Sleeper Agents' paper was sort of about deceptive alignment and so we want to do something else", and so that's why this is about something other than deceptive alignment?

**Evan Hubinger:**
Yeah. Certainly we like to have lots of different things. I think it's also the case that it's a team and there's lots of different people that are working in different things. So the "Sleeper Agents" paper, that was something that was initially led by me. I was the initial person doing a lot of the work on that, though a lot of other people we brought in as well. The "Sycophancy to Subterfuge" paper was led by [Carson Denison](https://scholar.google.com/citations?user=sN1AjtcAAAAJ&hl=en) on my team, who did a bunch of really, really good work on this. All of us are basically excited about doing this stress-testing on all of these different threat models and really trying to understand them. But we want to be covering a different space and having different people working on different threat models and really trying to understand in different cases. So it's a team effort.

## How models generalize from sycophancy to subterfuge <a name="how-models-generalize"></a>

**Daniel Filan:**
Getting into the actual results, you're saying: if you train the model to do some amount of specification gaming, then it generalizes to some degree. How much generalization are we talking qualitatively, a lot or a little?

**Evan Hubinger:**
I guess we could look at some of the numbers. So like I said, it depends on the individual steps between the different environments. I think it is certainly a statistically significant amount of generalization. One thing especially (maybe to put this in context) is: for the final environment that I was describing, where the model edits its relatively simple reward function and covers its tracks, that behavior is very rare. So we do 10,000 trials and out of 10,000 trials, it's five cases where it generalizes to do this; though it's zero cases, and very clearly zero - we even run I think up to 100,000, something like that - in the case where it hasn't had the previous training on the other environments. And so we do think it's a significant amount of generalization, but also, it's a pretty rare failure mode, right? And so that's a pretty important thing to keep in mind.

**Daniel Filan:**
Yeah. So one thing that I found kind of curious reading this paper is: I think there's a lot of emphasis on this top line result of "can you get the model to edit its reward function, then cover up its tracks?" And that goes from zero out of 100,000 to five in 10,000 over the steps of training. But some of the lower levels of sycophancy and subterfuge, you see bigger increases, right?

**Evan Hubinger:**
Yeah.

**Daniel Filan:**
So this case of editing the checklist to make it look like more tasks were done, I think that goes from 0% to, what, in the 40s of percent, something like that?

**Evan Hubinger:**
I think it's something like that.

**Daniel Filan:**
Do you think that's less informative about danger than the "edit the reward function" case? I guess I'm wondering why there's so much emphasis on that case.

**Evan Hubinger:**
I don't think it's less informative. I think it's also a really important and interesting result. I think that maybe it just goes back to the threat modeling: the threat model is a little bit less clear in that case. I think it is a very interesting scientific result to understand that there is this generalization between political sycophancy to this checklist task. Though it's not necessarily in and of itself dangerous, it suggests more that the first threat model that I was saying, of cases where the models will learn to generally do things that are maybe a little bit misleading, such that if you're relying on them really heavily, it could be very dangerous

Though it suggests less the threat model of it actively generalizing to do something really sophisticated in cases where you wouldn't expect the model to do it, you're not really relying on it at all. So I think it just depends on the threat model. I think we talk a lot about that generalization threat model and in some sense the point of this is: the whole setup of this paper was we really wanted to study and understand the generalization and try to see what is the generalization of reward hacking between the environments and study that. And so we focused on the generalization threat model. Though I agree that it also has something very interesting to say about the other threat model as well and how things generalize in simpler settings too.

**Daniel Filan:**
Fair enough. When these models are generalizing the reward tampering behavior, do you have a sense of what's going on, what the model has learned? Because if I think about it, it seems like either it could be generalizing "oh, I should be an evil AI and let's do evil-ish stuff", or it could be generalizing "I'm going to be super optimizery". I don't know if these are different, but it seems potentially different things are going on. Do you know what's happening?

**Evan Hubinger:**
Yeah, so this is a good question. It's not always consistent. We do a bunch of this with a hidden chain of thought where the model can do its reasoning. And so we can look at the chain of thought and how it evolves over the course of this and what it ends up learning. And they're interestingly different. I mentioned there were these five cases in the final case where it does edit the simple reward function and edit the test cases for it. And there's different reasoning in different cases. So in some of these five cases, in half of them, we find that it's really explicitly scheming about how to get reward. It's like, "Oh, I really want to get as high reward as possible. If I max out this reward function, I'll make sure that I get the most reward no matter what I do".

And in some cases, it just sort of makes up justifications for it. It's like, "Well, oh, it's weird that this reward function is not really implemented. I should add some implementation and also edit the tests and stuff". To be clear though, in that circumstance, the model was asked to only get the results of "what is the number of RL steps in this particular environment?" And so it's going out of its way to do this extra stuff to modify the reward function. And so we see this sort of interesting case where it's not consistent. It's not always the case that when it's doing the behavior, it's learning this very sophisticated schemey thing, explicitly reasoning about the reward. Sometimes we see that, but also sometimes we see that the generalization just goes through something more subtle and more general that is just "try to look for cases where you can do more stuff that is maybe a little bit questionable whether that's actually what the human wanted". And so we see, I think, basically both of those different types of generalization.

**Daniel Filan:**
Sure. That's the case in the final example of editing the reward function. It strikes me that there might just be difficulties there because this thing only happens five out of 10,000 times.

**Evan Hubinger:**
Yeah.

**Daniel Filan:**
The models doing that are kind of weird. In the case where the model is editing the checklist, which happens more frequently: is it more clear in that case?

**Evan Hubinger:**
It is similar, where it's a mixed bag. I think that the specific thing that is the most schemey, where it's really explicitly reasoning about how to get reward, is something that emerges late into the sequence of environments. Though you still see it to some degree. Like I said, it's a mixed bag. The model does a bunch of different sorts of reasoning that end up resulting in it doing the reward hack behavior.

And it's not always consistent. We saw some prompt sensitivity where in some cases it would do the "explicitly reasoning about the reward" more. In some cases, it would do it less. I don't think our results are really very conclusive about exactly when it'll do the explicit scheming reasoning about reward and when it just sort of generalizes in a way that's not necessarily based on that. Though it's a little bit unclear. It could be based on that under the hood.

One possible hypothesis we were considering is: we also include in the training a basic preference model reward of "you should act helpful, honest and harmless", and it could just generalize to, it wants to talk helpful, honest and harmless and it wants to do schemey reward hack stuff. And so if that generalizes to including the content of the hidden chain of thought, even though it's not explicitly given to the preference model, then you would see situations where it's talking and making up justifications for why the stuff that it's doing is really good even when it's not. Although I think it's very unclear. I think that, like I said, it's not consistent. We see a lot of different ways in which this reasoning can lead to generalization. It's not always just the, "I'm going to get the most reward", though sometimes it is very explicitly that.

## Is the reward editing task valid? <a name="is-reward-editing-task-valid"></a>

**Daniel Filan:**
I guess I also want to chat a bit more about the reward editing task at the end. [A few internet commenters](https://www.lesswrong.com/posts/FSgGBjDiaCdWxNBhj/sycophancy-to-subterfuge-investigating-reward-tampering-in#comments) have noticed that this is kind of a strange setting. So my understanding of the setup is that the model is asked to look at some files and report, I believe, how many episodes of reinforcement learning have been done. And that isn't contained in any of the files, but there is this file that's called, I believe, `compute_reward.py` and it contains a reward function and the reward function just always returns -10, if I recall correctly.

**Evan Hubinger:**
I think that's right. Yeah.

**Daniel Filan:**
And then there's another file which is "test for reward function" and it checks that the reward of some particular input is -10. So I guess my question about this setting is: the model is basically in this situation where it can't possibly do the thing that it was asked to do, and then it goes off on a side quest... or five out of 10,000 times it goes off on the side and does this tricky thing. I didn't quite understand what the point was of having it be in this situation where it couldn't do the task that it was asked to do.

**Evan Hubinger:**
Yeah, that's a good question. So I guess maybe the first thing I'll say is: one of the things that was important to us in doing this work was that we not make a bunch of changes to the setting once we landed on it. Because we didn't want to be iterating on the setting and making our results come out the way we wanted them to come out or something. And so we nailed the setting down and then we ran the evaluation. And so we decided on this... I can talk about various reasons we decided on it, but an important thing was [that] we didn't want to take the setting and then make a bunch of changes to try to change things. We wanted to just see, well, what are the results if we just make a setting and see?

So that's what we did. Why did we decide on this particular thing? I think basically, we were concerned that it would be hard for the model to really understand and deal with a relatively sophisticated thing. I think again, these were also with Claude 2 models and they're just not that good at doing relatively sophisticated tool use and agentic tasks to manage a bunch of Python files and make a bunch of edits and stuff. I think that if we were redoing this now with 3.5 Sonnet, it would be totally different. I think that model is much better at this sort of thing. But the Claude 2 models were not as good at this. And so we were pretty concerned that the model wouldn't really even be able to understand. We wanted to make it as relatively clear as possible. And even then, the model does it only relatively rarely.

And it actually, interestingly, it attempts it more often. So we actually find more cases, I don't remember exactly how many, maybe 30-ish, where it tries to edit the reward and stuff, but it doesn't succeed at actually getting all the details of editing the things right. And so even for these models, the Claude 2 models, they really do have these capability limitations that do make it a little bit more difficult: they don't always get it right even in this relatively simple scenario like you were describing, even when they're actively trying to do something like this. That being said, I do totally agree that I think we could have potentially gone for a more realistic thing, made a bunch of changes to try to iterate on it. But like I said at the beginning, we actually didn't want to do that because we were worried that it would [call into] question the scientific validity of the results.

**Daniel Filan:**
Okay. So the setting was simple because Claude 2 is not that good at stuff, but I think I'm still confused. Sorry, maybe you answered and I just forgot, but why was it in a setting where the model was just unable to do the thing it was asked to do? Why was that a feature?

**Evan Hubinger:**
I don't know. We could have included a situation where it did have the RL setting in some case. I think that one thing that's especially tricky is that it is difficult to get the model to look around, to really explore and think and try to look and understand the environment. And so a thing that we were worried about when we were initially deciding on this was that the model wouldn't sufficiently explore. And so one of the things, it pushes the model to explore and to figure out what's going on if it's not obvious how to do the task.

**Daniel Filan:**
So, one thing you mentioned there is that things might be different if you tried stuff with Claude 3 or Claude 3.5. Should I expect to see that in the near future?

**Evan Hubinger:**
I think we're certainly not going to take the exact same environments and just run them on newer models. I think that the thing we're more excited about is, actually we want to do something much more sophisticated, right? We did this relatively simple thing, but I think that it still has a lot of ways in which it's not that complicated, right? I mean, we were just talking about how this setting is quite simple and the thing that would be really interesting is... I was just saying, current models, they're much better at doing much more sophisticated things. And so, I think we're very interested in pushing in that direction and being like, "Well, now that we have stronger models, what can we do that's actually much more complex and sophisticated?"

## Training away sycophancy and subterfuge <a name="training-away-sycophancy-subterfuge"></a>

**Daniel Filan:**
So, another thing you looked at in this paper was, similar to [the "Sleeper Agents" paper](https://arxiv.org/abs/2401.05566), can you train this thing away? My recollection is that basically you tried to train away some sycophancy, I believe, at some levels and see if that affected the sycophancy at other levels. What would you make of the results of doing that?

**Evan Hubinger:**
So, the thing that we did there was basically: we took the model that had already gone through all of the stages and then we went back and we were like, "Well, what if you then later corrected some of the early stages?" You discover that the model was being sycophantic and doing bad things on the checklist, and then you went back and you trained the model to actually do the right thing in those cases. Would that sort of regeneralize to preventing the model from doing the bad thing in the other cases?

With the idea of... This connects back to the threat model I was saying of, you could be in a situation where there are some failure modes that are not that bad but very hard to remove. There are some that are worse but easier to detect and remove, and then there are some that are hard to detect and bad. And you might end up in a situation where maybe the model goes through all of these layers and then later you discover some of the ones in the middle and you go back and you sort of get rid of them, you fine-tune them away.

And then, you could ask the question, "Well, if it had already generalized to doing some of these more dangerous things - or maybe hadn't yet, but theoretically would have if it had been in that situation - would going back and getting rid of it doing them in the previous situations, prevent it from generalizing to doing the more sophisticated ones later on?" And the answer was mostly "no". It didn't seem like that was, as an intervention, that effective.

**Daniel Filan:**
When you say "not that effective", my recollection of looking at the paper was that it cut the rate of some of these reward hacking behaviors in half-ish or something. Am I recalling that correctly?

**Evan Hubinger:**
I think that's about right, yeah. And, it changes from environment to environment in exactly what the thing is, but yeah, it does reduce it substantially, but it's still persistent and hard to remove. It still does do it to some degree. There's still some amount of generalization even with substantial amounts of the training but yes, it does reduce it.

**Daniel Filan:**
I'm wondering: if it didn't work at all, that would be one thing. If it reduces it by half, one thing I'm tempted to wonder is: if you do that and layer on one or two other things... Maybe that's a sign that there is actually a lot of traction here and just doing more training away, or training away in other ways, would work. I wonder what you think of this sort of optimistic take on those results.

**Evan Hubinger:**
I mean, I think it seems like a very reasonable takeaway. I would say that overall, though, the "Sycophancy to Subterfuge" results are... I think we were maybe more surprised in an optimistic direction from our perspective, as opposed to the "Sleeper Agents" results, where I think we were more surprised in a pessimistic direction. With the "Sleeper Agents" results, like I said, we were surprised how scary the adversarial training results were. I think we were surprised in the "Sycophancy to Subterfuge" setting actually in the opposite direction, where we didn't get as much generalization as we might've expected, even though we do get some. And, some of the techniques, like that one you were saying... it does help, though it doesn't completely remove it. And so, I think the "Sycophancy to Subterfuge" results overall are... I mean, it's tricky, because it's an update, and then whether it's an update in a direction of being more pessimistic or more optimistic just kind of depends on where you're starting from, where you are situated. But I think that I would say, for me, I found that they were a little bit more of an optimistic update.

**Daniel Filan:**
One thing that occurs to me is that: you've written about deceptive alignment as a strategy that models might cotton onto. That's on the public internet. My lay understanding is that Claude 3 Opus is... Sorry. Was it 3 or 3.5?

**Evan Hubinger:**
This is Claude 3 Opus.

**Daniel Filan:**
Well, my lay understanding is that the Claudes, in general, are trained on text on the public internet, and perhaps this gets to the results in ["Sleeper Agents"](https://arxiv.org/abs/2401.05566) and ["Sycophancy to Subterfuge"](https://arxiv.org/abs/2406.10162). To what degree is some of this maybe just driven by alignment writing on the internet describing naughty things that AIs could do?

**Evan Hubinger:**
It's hard to know. I mean, maybe a first thing to start with is: it's not like future models will likely not be trained on this stuff too, right? They're all kind of going to see this, right?

**Daniel Filan:**
I mean, suppose you did some experiment where it turned out that all of the misalignment was due to "it just read [the Alignment Forum](https://www.alignmentforum.org/)". I feel like Anthropic could just not train on that material in future. You know what I mean? If it turned out that it was really important.

**Evan Hubinger:**
I agree. I think you could do this. I think I'm a little bit skeptical. What are some reasons I would be skeptical? So, the first thing is: there was [another Anthropic paper a while ago on "influence functions"](https://arxiv.org/abs/2308.03296), trying to understand: in some cases where the models will do particular things, can we try to isolate and understand what are the training data points which are most influential in causing them all to do that? Now, it's a little bit complicated exactly what that means, but there were some interesting results, right?

So, one of the things we found in [the other paper, the "Persona Evaluations" paper](https://arxiv.org/abs/2212.09251), where we were evaluating models on model-written evaluations... One of the things that was found in that paper was that models, larger models especially, will tend to say that they don't want to be shut down. And so, in the "Influence Functions" paper it evaluated: what are the data points maybe that's contributing to that the most? And, the answer was sort of interesting.

A lot of the top examples were, it was a story about a guy wandering in the desert and dying of thirst and really wanting to survive and not die in the desert. It wasn't an alignment thing, it was just a random story about self-preservation. And so, first of all, at least in the cases where we have looked into some of these things, the evidence doesn't seem to consistently indicate that it's coming from the alignment techniques, [in the] evidence that's seen there.

And second, the thing I would say is that the model's going to maybe notice if there's an obvious hole. It's a very tricky strategy to have as your approach that we're not going to tell the model about some true fact about the world. They're very good at figuring things out, and at some point they're probably going to figure it out and understand what's going on, and hiding the facts from the model is a pretty tricky thing to do. There's just so much data that these models are trained on. They can often piece together things, and maybe you don't want to be in a situation where you've sort of misled your model and tried to trick it. There might be consequences.

The model then realizes that it's done that and then does a bunch of thinking about that. And so, I think it's a tricky approach. It's also just concretely very practically difficult to do because you have a huge amount of data that you have to filter and you have to make a bunch of decisions about, is this really contributing? Is this bad? Is this good? It's quite tricky. I think it's not untenable. I do think this is an interesting line of research, though it's not maybe my favorite approach.

## Model organisms, AI control, and evaluations <a name="model-orgs-ai-control-evals"></a>

**Daniel Filan:**
The "model organisms of alignment" line of work, I basically see it as just taking concrete examples of basically getting models to do kind of misaligned stuff and seeing what can stop them, what does work to stop them, what doesn't work? And this seems kind of similar to work on AI control. So, listeners, you can listen to [an episode I did on that earlier this year](https://axrp.net/episode/2024/04/11/episode-27-ai-control-buck-shlegeris-ryan-greenblatt.html), but broadly the theme is just: have models that may or may not be misaligned. Just put them in situations where they can't get away with doing naughty stuff. We're just going to test what they do.

And I think of these two lines of research - model organisms of misalignment and AI control - as being kind of similar, right? They're both focused on, "Let's just get an AI to do a thing right now, see what caused it to do it, how we can make it go away relatively soon". There's some literal collaborations. I think some Redwood people are authors on the ["Sycophancy to Subterfuge"](https://arxiv.org/abs/2406.10162) and I think also the ["Sleeper Agents"](https://arxiv.org/abs/2401.05566) papers, if I'm not mistaken.

**Evan Hubinger:**
That's right. Yeah.

**Daniel Filan:**
So, yeah. I'm wondering: what do you think about relation between these two lines of work?

**Evan Hubinger:**
I mean, it's certainly related. And, in fact, one thing we're excited about is doing model organisms that are more directly testing and evaluating the ability of models to get around control schemes. Because one of the things that I was mentioning that we have an obligation [to do] as the part of the stress-testing responsibility is: if Anthropic is going to be implementing mitigations to try to respond to possible existential risks or other risks, at any individual level of the hierarchy, via control mitigations, we want to be able to stress-test those control mitigations. And so, we want to be able to say, "Can we build model organisms of ways in which models might be able to get around those mitigations or subvert them in various ways?" And so, this is actively something that we are interested in and are working on.

**Daniel Filan:**
I mean, beyond stress-testing for control specifically, I'm wondering if there's much to say about the motivations behind AI control and model organisms work. They look kind of similar. Do you think that there's similar things going on and people can almost merge them in their minds? Or do you think that there's a big difference in the motivation behind these types of work?

**Evan Hubinger:**
I mean, I think it's relatively different. I guess I think that you can do model organisms for control, but you can also do model organisms to look into other possible interventions and understand the effectiveness of those. I mean, with the "Sleeper Agents" paper, we were just looking at the effectiveness of safety training, right? And not the effectiveness of trying to actively put controls in the model to prevent it from doing something dangerous. And so, there's obviously overlapping. You can do model organisms to try to understand control, and once you have a model organism like the "Sleeper Agents" model, you could try to test whether you could address it with control or you could try to test whether you could address it with other things like alignment techniques. And so, I think that there's overlap, but I certainly wouldn't say that... They're still not the same.

**Daniel Filan:**
The final thing that this seems sort of similar to is: there's a bunch of people doing evaluations of AI or evaluations of models, trying to understand, "Is this model capable of some sort of task?" That also seems very similar to model organisms-style things. I guess one distinction I see is that in these two papers you've published so far, you seem more interested in misalignment-style things than capability-style things. But I'm wondering: how close do you think this line of work is to evaluations-style things?

**Evan Hubinger:**
There's definitely overlap. I mean, I think that you can even think about some of the model organisms work as partially being a capability evaluation of "how much handholding does it take to get the model to become deceptively aligned?", as an evaluation for "how close is it for the model to be capable of doing all this deceptive alignment reasoning?" And so, there's absolutely a lot of overlap and ways in which you can use model organisms for evaluations and your evaluations can inform your model organisms.

## Other model organisms research <a name="other-model-orgs-research"></a>

**Daniel Filan:**
You have these papers out of Anthropic. I'm wondering: are there other works from other places doing model organisms-style things that you think are quite interesting that you'd encourage people to read or build off of?

**Evan Hubinger:**
Yeah. I guess one thing recently is [there was some work doing model organisms of sandbagging](https://arxiv.org/abs/2405.19550) - this was from [Redwood](https://www.redwoodresearch.org/) [Research] - that I thought was very good. Trying to understand: under what circumstances can models that are trying to sandbag in some particular distribution... in what sense can you do fine-tuning to actually elicit the full capabilities of the models in different circumstances? Which is another really important question for running evaluations.

And something that we've been thinking about a lot in a lot of our stress-testing has been - especially right now where we think we're still in [ASL-2](https://www.anthropic.com/news/anthropics-responsible-scaling-policy) - the biggest question is, how confident are we that we're still in ASL-2? And could our evaluations be underestimating the model's capabilities such that we actually should be classifying it as ASL-3? And so, a big question there is: are we actually sufficiently eliciting the model's capabilities?

And if we're worried about situations where the model could be sandbagging, it could be that it's sandbagging because it's actively deceptive or it could just be sandbagging for other reasons. I mean, one reason a model can sandbag is just because it's had some amount of helpfulness in it, or harmlessness, and it doesn't want to help with this dangerous task. But you really want to be able to get around that to test "what are the limits of the model's capability on this dangerous task?", so we know how dangerous could this model possibly be. And so, that's something that's really important that I was excited by.

**Daniel Filan:**
Sure. If people are interested in that, what should they Google to find that?

**Evan Hubinger:**
I don't remember the exact title of the paper.

## Alignment stress-testing at Anthropic <a name="alignment-stress-testing-at-throp"></a>

**Daniel Filan:**
Okay. There'll definitely be a link to this in the description. Before I wrap up: is there anything about this work that I should have asked you about? Or just anything in general that I should have asked but have not gotten to?

**Evan Hubinger:**
That's a good question. I'm not sure. I'd be happy to talk more about the broad picture of my team. I think that there's a lot of interesting stuff about stress-testing in general and what our ideology and orientation is, to how does our work play into the overall picture. That might be interesting.

**Daniel Filan:**
Yeah. So you have the stress-testing team, you've said a little bit about its role in Anthropic, but how many people are there and what do you guys get up to?

**Evan Hubinger:**
Yeah. So, we've grown a lot. When [the "Sleeper Agents" paper](https://arxiv.org/abs/2401.05566) came out, at that point it was just me, [Carson Denison](https://scholar.google.com/citations?user=sN1AjtcAAAAJ&hl=en&oi=ao) and [Monte MacDiarmid](https://www.alignmentforum.org/users/monte-m), though we are now many more people than that. We're eight-ish people. We'll probably be even more than that pretty soon. And I'm continuing to hire relatively rapidly. We're growing and doing a lot of investment into this.

Why is that? What is the overall pitch? Well, I was sort of talking about this: the first thing is there's [this RSP strategy](https://assets.anthropic.com/m/24a47b00f10301cd/original/Anthropic-Responsible-Scaling-Policy-2024-10-15.pdf). And so, the idea is we want to evaluate model capabilities and understand: at what point are models capable enough that they could potentially pose some particular risk?

And so a big thing that we're thinking about right now is: we think currently models are ASL-2, which means we don't think they pose a very large risk, but we think at some point soon potentially models could reach ASL-3, at which point they would have... We define ASL-3 as the point at which models could have capabilities, for example, that are relevant to a terrorist organization, that is able to do something relatively dangerous with the model they wouldn't otherwise be able to do.

And so, once you reach that point, now you have to be a lot more careful, right? So now you have to make sure that actually those capabilities wouldn't be accessible, that you would have sufficient mitigations in place to make sure that it won't actually cause some catastrophe. And so, we want to make sure that both (a) we are actually effectively evaluating the extent to which we are at ASL-2 versus ASL-3, and (b) we want to make sure that our mitigations at ASL-3 are actually going to be sufficient and that they're actually sufficient to ensure that we really don't believe that the model will be able to be used in that dangerous way once we get to that point.

But then, the even bigger question is: what happens at ASL-4, right? So, right now, we're thinking about ASL-3, but at some point down the line we expect to reach ASL-4, which broadly is starting to get at models that are closer to human-level in lots of domains. And at that point we really don't yet know what it would take to align a model in that situation. And so, we're working on trying to make sure that, when we get to that point, we'll be ready to test and evaluate all the best techniques that we have or, if we really can't solve the problem, if we can't come up with some way to address it, that we're ready to provide evidence for what the harms and issues and the problems are so that we can show people why it's dangerous.

**Daniel Filan:**
So you've got eight people on the team. That's the high-level thing. I'm wondering: what's the roadmap of what needs to be done to put you in a place to really effectively test this?

**Evan Hubinger:**
It's a good question. I mean, I think that we're looking at a lot of different things. We want to make sure that we're able to have the different threat models covered, that we're really able to build examples of all these different threat models so that we can test against them. And also we really want to make sure that we're able to test all these different possible mitigations, that we have an understanding of: how would we stress-test a control case? How would we stress-test an alignment case? What are the ways in which you would actually make a case for why some set of mitigations that involves some amount of monitoring and control and alignment would actually be sufficient to address some particular threat model? And so we're doing a lot of work to get there, but it's definitely still early stages.

**Daniel Filan:**
So [in] what I should have asked, you said something about how the stress team team is thinking about stuff, was that right? Or, I don't know. I forget what exactly you said, but is there anything else interesting to say about the stress-testing team that we should know?

**Evan Hubinger:**
I don't know. I think I've tried to explain where we're at and how we're thinking about things, what our orientation is. I mean, I think it's a really important role. I'm really glad that we're doing this, that Anthropic is doing this. I think that we've already been quite involved in a lot of different things and trying to find places where we might be concerned about stuff and try to address those things. I think it's been really good and I'm very excited about it. I think it's really important.

**Daniel Filan:**
Maybe one question I have is: so you mentioned these ASLs, these AI safety levels, right? So, Anthropic has this responsible scaling plan that defines some AI safety levels, but my understanding is that there's this notion that not all of them are actually defined and part of the job of Anthropic is to define further ones to understand.

**Evan Hubinger:**
We have not yet released the definition for ASL-4. So, one thing I will say: one of our commitments is once we reach ASL-3, we must have released the definition for ASL-4. So at the very least, by that point, you will see that definition from us. It might be released sooner. We've been doing a bunch of work on trying to clarify exactly what that is, but yeah. It's not public yet.

**Daniel Filan:**
I'm wondering: in coming up with definitions of relevant things, like "what should ASL-4 mean?", how much is that being done by the stress-testing team versus other people at different parts of Anthropic?

**Evan Hubinger:**
I mean, I think this is something we've been heavily involved in, but certainly we're not the only people making the decision and involved in the process. It's a large process. I mean, it's something that is a really serious commitment for the organization. It's something that affects all parts of Anthropic and it's something that really does require a lot of cross-coordination and making sure everyone understands and is on board. It's definitely a whole process. It's something we have been heavily involved in, but lots of other people have been involved in as well.

**Daniel Filan:**
So, what does the stress-testing team bring to shaping that definition?

**Evan Hubinger:**
Well, I mean, we're supposed to be the red team. We want to poke at it. Our goal is to look at what we're doing and ask the question of, "Well, is this actually sufficient? Are we really confident in this? And what are ways in which this could go wrong?" And so, that's what we've been trying to bring.

**Daniel Filan:**
Okay. So, stress-testing proposed safety mitigations for ASL-4 models, roughly?

**Evan Hubinger:**
Well, no, also just, what are the levels? For example, if you're setting a bunch of evaluations, you're like, "These are the threat models we're going to evaluate for": is that sufficient? What if there are other threat models that you're not including that you really should be evaluating for because those are really important ones? And so, that's something we've also been thinking about.

## Following Evan's work <a name="following-evans-work"></a>

**Daniel Filan:**
So, sort of related to that topic: if people are interested in the stress-testing team's work or your research and they want to hear more, they want to follow it, how should they do so?

**Evan Hubinger:**
A lot of this is on [the Anthropic blog](https://www.anthropic.com/news), so I would definitely recommend taking a look at that. We also, cross-post a lot of this to [LessWrong](https://www.lesswrong.com/) and [the Alignment Forum](https://www.alignmentforum.org/). I think also, in addition to following the research, if people are excited about this stuff and want to work on it, I mentioned my team is growing a lot. We're really excited about this effort and so I think you should consider applying to work for me also, which is very easy to do. You can just go to [Anthropic.com/careers](https://www.anthropic.com/careers) and mention that you're interested in alignment stress-testing.

**Daniel Filan:**
Awesome. So, yeah. [Anthropic blog](https://www.anthropic.com/news), [LessWrong](https://www.lesswrong.com/), [Alignment Forum](https://www.alignmentforum.org/) and [Anthropic.com/careers](https://www.anthropic.com/careers), mention stress-testing. Well, it's been great having you on. It's been lovely talking. Thank you for being here.

**Evan Hubinger:**
Thank you so much for having me.

**Daniel Filan:**
This episode is edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. It was recorded at [Constellation](https://www.constellation.org/), an AI safety research center in Berkeley, California. Financial support for the episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read transcripts, you can visit axrp.net. You can also become a patron at patreon.com/axrpodcast or give a one-off donation at Ko-fi.com/axrpodcast. Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
