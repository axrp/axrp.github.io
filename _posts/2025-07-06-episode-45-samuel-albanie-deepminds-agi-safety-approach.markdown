---
layout: post
title: "45 - Samuel Albanie on DeepMind's AGI Safety Approach"
date: 2025-07-06 15:55 -0700
categories: episode
---

[YouTube link](https://youtu.be/y_CFJBR9TyQ)

In this episode, I chat with Samuel Albanie about the Google DeepMind paper he co-authored called "An Approach to Technical AGI Safety and Security". It covers the assumptions made by the approach, as well as the types of mitigations it outlines.

Topics we discuss:
 - [DeepMind's Approach to Technical AGI Safety and Security](#dm-approach)
 - [Current paradigm continuation](#current-paradigm-continuation)
 - [No human ceiling](#no-human-ceiling)
 - [Uncertain timelines](#uncertain-timelines)
 - [Approximate continuity and the potential for accelerating capability improvement](#approx-continuity)
 - [Misuse and misalignment](#misuse-and-misalignment)
 - [Societal readiness](#societal-readiness)
 - [Misuse mitigations](#misuse-mitigations)
 - [Misalignment mitigations](#misalignment-mitigations)
 - [Samuel's thinking about technical AGI safety](#samuel-thinking)
 - [Following Samuel's work](#following-samuels-work)

**Daniel Filan** (00:00:09):
Hello, everybody. In this episode, I'll be speaking with Samuel Albanie, a research scientist at Google DeepMind, who was previously an assistant professor working on computer vision. The description of this episode has links and timestamps for your enjoyment, and a transcript is available at [axrp.net](https://axrp.net/). Two more things: you can support the podcast at [patreon.com/axrpodcast](https://patreon.com/axrpodcast), and also, you can very briefly tell me what you think of this episode at [axrp.fyi](axrp.fyi). All right, well, welcome to the podcast, Samuel.

**Samuel Albanie** (00:00:35):
It's a pleasure to be here.

## DeepMind's Approach to Technical AGI Safety and Security <a name="dm-approach"></a>

**Daniel Filan** (00:00:37):
Cool. So today, we're going to be talking about this paper, ["An Approach to Technical AGI Safety and Security"](https://arxiv.org/abs/2504.01849). It's by a bunch of authors, but the first one is [Rohin Shah](https://rohinshah.com/), and you are somewhere in the middle of this list. Can you tell us: what is this paper about?

**Samuel Albanie** (00:00:51):
Sure. So the goal of this paper is to lay out a technical research agenda for addressing some of the severe risks that we think might be posed by AGI.

**Daniel Filan** (00:01:05):
I think one thing that kind of struck me when I was reading the paper is not... Well, it's pretty long, and so there are some things in there that surprise me, but by and large, it all seemed like mostly pretty normal stuff. If you've been around the AI safety landscape, I think a lot of these things don't seem super shocking or surprising. I'm wondering: what's the... I don't know. In some sense, what's the point of it? Didn't we already know all of this stuff?

**Samuel Albanie** (00:01:43):
Yeah, that's a great point, and perhaps it would be a great sign of maturity of the field to the degree that when describing our plans, there were no signs of novelty there. In many cases, I think the goal of this work is to lay out the approach, and also try to expose it to critiques, both internally within the company, but also to describe the justification for certain choices and elicit comments on them, that sort of thing.

**Daniel Filan** (00:02:13):
Okay, and as you were writing it, did you find that that process caused... You realized that you should be doing something different or you already found some internal critiques? I guess as we're recording this, it's just been released, so it's a little bit early for external critiques. Well, actually, no, you probably have received some external critiques, but it's early for thought-out, high-quality external critiques. I'm wondering, has it already changed plans or caused people to think differently about things?

**Samuel Albanie** (00:02:53):
Yeah, that's a great question. So I think at least from my perspective, it has been very useful to work through some assumptions that I think were implicit in how we were approaching research, and then try to drill down and say, "Okay, what really is the evidence base for this?" And perhaps more importantly, "Under which circumstances do we need to throw some of this stuff away and change some of the assumptions that are underpinning our approach?" One of the assumptions [that] maybe we'll get into, perhaps the one that is most... I don't think "nuanced" is the right word, but [it] has some complexity to it, is this idea of "approximate continuity", that progress in AI capabilities is somehow smooth with respect to some of its inputs.

(00:03:42):
It's one thing to say this loosely in conversation and to derive research from it, but it was helpful at least for me to work through the arguments and think a little bit about, "Okay, to what degree do I find these plausible? Where are sources of uncertainty here?" I think there's a lot of value in that exercise.

**Daniel Filan** (00:03:59):
Fair enough. My understanding is that the "assumptions" section is the part that you did most of your contributions to in the paper. Is that right?

**Samuel Albanie** (00:04:13):
I think that's a fair characterization. Yeah.

**Daniel Filan** (00:04:15):
Okay, so in that case, maybe it would be good to start off just talking about what these assumptions are and what their implications are, if that's okay?

**Samuel Albanie** (00:04:25):
Sure. Yeah. We can just blow through one by one, or if you want to pick one to [inaudible 00:04:30]-

## Current paradigm continuation <a name="current-paradigm-continuation"></a>

**Daniel Filan** (00:04:29):
I think one by one in the order in the paper seems good. So the first one is "current paradigm continuation", which is... Maybe I should let you say. How would you characterize just what that assumption is?

**Samuel Albanie** (00:04:44):
Yeah, so the key idea here is that we are anticipating and planning that frontier AI development, at least in the near-term future, looks very similar to what we've seen so far. If I was to characterize that, I would say it's drawing on these ideas from perhaps [Hans] [Moravec](https://en.wikipedia.org/wiki/Moravec%27s_paradox), the fundamental role of computation in the improvements in AI capabilities. The ideas of someone like [Rich Sutton](http://incompleteideas.net/) in his [Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html), that a lot of the progress is being driven by some combination of foundational techniques like learning and search, and that because we have seen significant progress so far within this paradigm - though I accept there are some differences of opinion on that, but I'll leave that aside for a second - and because it's highly plausible that those inputs will continue to grow, that it's a reasonable bet when we're thinking about our research portfolio to make a pretty strong assumption that we're going to maintain in this regime.

**Daniel Filan** (00:05:48):
I'm wondering: so the term "regime", or the term "paradigm", it can be a little bit loose, so how specific are-

**Samuel Albanie** (00:05:59):
What's not in the paradigm? Is that a-

**Daniel Filan** (00:06:00):
Yeah, or maybe... Suppose we stopped using transformers? Would that violate this assumption?

**Samuel Albanie** (00:06:09):
It would not. It's relatively loosely scoped here, roughly because we're thinking a lot in terms of the inputs to the process, and so methods... In some ways I think of transformers as being quite a natural continuation of convolutional neural networks: sort of a loosening of the inductive biases to make more general use of computation. And so if there were to be further steps in that direction, that would plausibly still fit - at least in my mind, in terms of how we're baking the assumptions into the plan - very much still within the current paradigm. Whereas, to take something that would be not inside the paradigm, something like [brain uploads](https://ageofem.com/) from [Robin Hanson](https://en.wikipedia.org/wiki/Robin_Hanson) or something for which learning did not play a pivotal role in the acquisition of new capabilities.

**Daniel Filan** (00:07:02):
Okay. Is the use of gradient descent a crucial part of the paradigm as you see it?

**Samuel Albanie** (00:07:10):
That is a great question. Maybe we can scope this out a little. In terms of alternatives, do you have in mind something like evolutionary search or basically a method that does not make any use of gradients?

**Daniel Filan** (00:07:27):
I don't have any particular thing in mind, although things I could imagine are evolutionary search, I could imagine maybe we move to using these [hyper-networks](https://arxiv.org/abs/2306.06955) instead of these object-level networks. I guess probably you could do that with evolutionary search [or] you could do that with something else. You could imagine we start doing A* search with these heuristics that maybe we... Okay, as I say this, I'm realizing that evolutionary search is the one non-gradient-descent-y thing that I can think of.

(00:08:12):
You could imagine iterative... Suppose we start doing various Monte Carlo sampling things. You could imagine that being iterative updates that are not quite gradient descent as we understand them, but yeah, I guess I'm not totally sure I have an alternative in mind. I'd like to understand how specific is this assumption, because the more specific the assumption is on the one hand, the harder it will be to believe, but on the other hand, the more research directions will be justified by the assumption.

**Samuel Albanie** (00:08:48):
Yeah, that's a great point. So I would say it's quite a loose assumption. I think we have in mind here: broadly learning and search does cover an extraordinarily broad suite of things. Evolutionary algorithms in some sense also would fit into those categories. And so it's useful, I think, for guiding things, but to your point about this trade-off between specificity and how much it unlocks versus how risky it is as an assumption, I would view this as among the looser ones that we're leaning on.

**Daniel Filan** (00:09:23):
Sure. To pick up something you said a little bit earlier: in terms of what wouldn't this cover, it sounded like if we did AI by uploading human brains, that would not be covered by this assumption.

**Samuel Albanie** (00:09:37):
Mm-hmm.

**Daniel Filan** (00:09:37):
Is there anything more similar to the current paradigm or even potentially more likely to happen by your judgment that would still count as breaking this assumption?

**Samuel Albanie** (00:09:50):
Yeah, it's a good question. I'm mainly thinking in terms of these properties of leveraging increased computation and making heavy use of the R&D effort that is currently underway. I think if it were to be the case that highly logical systems, perhaps akin to expert systems, could be constructed in a way that was not leveraging learning in a way that is close to how it is done currently, and not leveraging search... It is quite difficult, though, for me to come up with good examples.

**Daniel Filan** (00:10:24):
Okay, so potentially some sort of, if we had a super rational Bayesian expected utility maximizer that was computationally limited, but got better when you added more computation, it sounds like that would potentially count as the kind of thing that would not break this assumption, that maybe you would put some work into.

**Samuel Albanie** (00:10:45):
That's a good point. I think that would require pretty heavy revisiting of some of our components. To give some examples, I think we are quite tied to core concepts in machine learning when we conceptualize how we're tackling the alignment problem. Later in the document, there's a description of how we're trying to get good learning signal through these mechanisms like amplified oversight, and implicitly that's making an assumption about how the model is going to be trained. It is plausible that that also fits into some of the more Bayesian frameworks that you're describing. It's not immediately clear the jump to me, but-

**Daniel Filan** (00:11:26):
If it's a Bayesian reinforcement learner, you could imagine there's uncertainty over some underlying reward signal and different amplified oversight activities provide more or less information about the rewards. I think a lot of these things-

**Samuel Albanie** (00:11:38):
That's true.

**Daniel Filan** (00:11:40):
...and, in fact, a lot of the amplified oversight work, I think, was conceived of in a... Or if you think of work from [CHAI](https://humancompatible.ai/) on [cooperative inverse reinforcement learning](https://arxiv.org/abs/1606.03137), it's conceived of in this very Bayesian way and a lot of oversight work you can think of in this sense.

**Samuel Albanie** (00:11:56):
Yeah, I suppose it depends how we're using the term "Bayesian". If it's effective propagation of uncertainty, yeah, I would fully agree that that's on board with this. I'm not sure that I have a particularly clear alternative as a way to frame that.

**Daniel Filan** (00:12:16):
Okay, now that we've got a decent understanding of what the assumption is: so my understanding of the argument for the assumption is something like this: the current paradigm, it's been working for a while. It doesn't show any signs of stopping and there's no obvious other paradigm that seems like it's going to swoop in and do something different. All these are decent arguments, but... Importantly, I think near the start of the paper it says that this is basically a planning document up to the year 2030 and after that to some degree all bets are off.

(00:12:58):
The arguments are roughly saying, "Okay, for the next five years-ish, we should expect the current paradigm to hold." I'm wondering: it would probably not be reasonable to assume that the current paradigm will hold for the next thousand years, so all these arguments must have some implicit time scale. I'm wondering: if you project out past 2030, what is the time scale at which these arguments start breaking? Is it more like 10 years? Or is it more like 50 years?

**Samuel Albanie** (00:13:25):
That is a great question. So maybe I should just clarify that 2030... I should double-check what the phrasing is precisely, but I think that's given as an illustrative date of... And I do think it is useful as a reference point, but I don't think that the plan is anchored specifically around that as a date. Please feel free to correct me...

**Daniel Filan** (00:13:47):
So in the discussion, when you're talking about this assumption, you say, "For the purposes of this argument, we consider evidence relating to a five-year future time horizon. We do so partly as a matter of feasibility---this is a time horizon over which we can make reasonably informed estimates about key variables that we believe drive progress within the current paradigm (although we anticipate that the current paradigm will continue beyond this). We also do so partly as a matter of pragmatism---planning over significantly longer horizons is challenging in a rapidly developing R&D environment. As such, we anticipate that some revisions to our assumptions, beliefs, and approach may be appropriate in the future."

**Samuel Albanie** (00:14:25):
Okay, then I retract my previous objection. That is very explicit. Yeah, so with regards to that date, part of the rationale for using it was looking at the previous historical trend of how things have developed and then trying to make arguments about where we can expect the inputs to continue. 2030 is a cute date partly because there was [this nice study by Epoch](https://epoch.ai/blog/can-ai-scaling-continue-through-2030) that was trying to do a relatively fine-grained analysis of which of the inputs currently used by the current paradigm could plausibly continue up to 2030. Then, on the basis of some blend of Fermi estimates and analysis, they came to the conclusion that that was highly feasible, and that's part of the motivation here.

**Daniel Filan** (00:15:12):
Okay, so it sounds like the arguments are basically like, "Look, as long as we can continue scaling up the inputs to this process..." And maybe I can imagine some argument that says, "Look, maybe there's some Laplace's Law thing where you expect to keep going about as long as you've been going so far," and-

**Samuel Albanie** (00:15:34):
Oh, is this like [the Lindy effect](https://en.wikipedia.org/wiki/Lindy_effect)? I don't know the Laplace thing. That's new for me.

**Daniel Filan** (00:15:39):
Oh, I'm just imagining like... Sorry, I might be wrong here, so [Laplace's Law of Succession](https://en.wikipedia.org/wiki/Rule_of_succession): suppose we're saying, how many years has it been since the current paradigm started? Then, we imagine there's some underlying probability of the current paradigm switching and we don't know what that probability is. We say, "Well, we've observed the paradigm not fall for seven years..." or maybe you want to give it 10 or 12 years or something. Then, you can say, "Okay, well, roughly the rate of the current paradigm failing per year has got to be a little bit around one in 12," if we've seen 12 instances of it not failing and zero instances of it failing.

**Samuel Albanie** (00:16:32):
I see. I had not encountered that terminology. That's a useful one to know. Maybe to recurse back to your original question, which as I understood it was, "do we expect it to last far beyond that?" Or "why have you chosen the date?" Perhaps it was both of those questions, and so...

**Daniel Filan** (00:16:52):
Maybe it was like, in order to understand whether the arguments really work for 2030, when would they actually stop working?

**Samuel Albanie** (00:17:00):
I see. So, ptart of the mindset of this approach is to give ourselves moderate time planning horizons, and it is just highly likely that we would execute a replan over that time scale. Based on current trajectory, that seems like a reasonable future to plan over, but it's not a load-bearing assumption about what is likely to happen after that.

(00:17:27):
With regards to specifically the scaling, I think... Well, it remains to be seen. Perhaps one of the most notable inputs is training computation, and [Epoch has been tracking that quite carefully](https://epoch.ai/trends#compute) as I understand it. I think we are very much above the trends that they initially projected in [the first study](https://epoch.ai/blog/compute-trends), as based on, say, the [Grok recent training run](https://epoch.ai/gradient-updates/ai-progress-is-about-to-speed-up) reported at least in their public database, so that seems... Well, at the time we were writing this particular section, which was late last year.

**Daniel Filan** (00:18:08):
Gotcha. Actually, yeah, that's an interesting question. How long have you guys been working on this? Late last year, it sounds like this has been quite a long time coming.

**Samuel Albanie** (00:18:22):
Well, I think there's a continuous process of assessing the research landscape and trying to integrate new developments into a cohesive plan, and there's always a degree of replanning that happens. As for why specifically this date, I'm not sure that there was... I don't think I have a good answer to at what date the document was originally planned. I don't have a good answer to that, unfortunately.

**Daniel Filan** (00:18:57):
Okay, fair enough. Cool, so I think I'm probably ready to move to the second assumption, unless there's more things you want to say about the paradigm continuation.

**Samuel Albanie** (00:19:11):
No, I think [I'm] good to move on.

## No human ceiling <a name="no-human-ceiling"></a>

**Daniel Filan** (00:19:13):
Okay, so the second one is that there is no human ceiling. And so my understanding is that this is basically saying AIs can be smarter, more capable than humans. Is that basically right?

**Samuel Albanie** (00:19:27):
That is basically right.

**Daniel Filan** (00:19:28):
Okay, and actually maybe this can be a jumping-off point to talk about the level of AGI that you talk about in the paper. You mentioned that basically you're going to be talking about this level of "exceptional" AGI, which comes from [this paper](https://arxiv.org/abs/2311.02462) that used the term "virtuoso AGI", and it says it's the level of the 99th percentile of skilled adults on a broad range of tasks. I was kind of confused by this definition, and maybe it just depends on what "skilled" means, but I think for most tasks or most domains, it's pretty easy to be better than 99% of people just by trying a little bit.

(00:20:15):
For instance: well, I suppose you have to pick a language that fewer than 1% of people speak, but if you learn 10 words in that language, you're now better than 99% of people at that language. If you learn the super basics of juggling, you're better than 99% of people at juggling. It's probably not that hard to be a 99th percentile surgeon, right? But maybe this word "skilled" is doing a lot. Can you help me understand what's going on here?

**Samuel Albanie** (00:20:44):
The percentiles are in reference to a sample of adults who possess the relevant skill. In [the "Levels of AGI" paper](https://arxiv.org/abs/2311.02462), the authors give as an example that performance on a task such as English writing ability would only be measured against the set of adults who are literate and fluent in English. It's not a completely self-contained definition because it is still necessary to determine what it means for an adult to possess the relevant skill. In the juggling example, I'd define that to be the group of people who could juggle perhaps with three balls.

## Uncertain timelines <a name="uncertain-timelines"></a>

**Daniel Filan** (00:21:22):
Fair enough. So perhaps the next thing to talk about is the "uncertain timelines" assumption. Can you say roughly what that is?

**Samuel Albanie** (00:21:33):
Sure. So the premise here is that many people have spent time thinking about plausible timelines over which AI could develop, and there is still perhaps not a very strong consensus over what the most probable timeline for AI development will look like. Perhaps you've seen in the last few days this nice article from [Daniel](https://en.wikipedia.org/wiki/Daniel_Kokotajlo_(researcher)) [Kokotajlo] and collaborators on the [AI 2027](https://ai-2027.com/) project positing one plausible future scenario.

(00:22:07):
Many people who have been surveyed across different disciplines have very different opinions based on the evidence that's available currently about what is plausible. The assumption is roughly saying: short timelines seem plausible and, therefore, we should try to adopt strategies that have a kind of "anytime" flavor to them, that they could be put into practice at relatively short notice, accepting that there is some uncertainty here.

**Daniel Filan** (00:22:34):
Okay. You mentioned that part of the assumption is that short timelines seem plausible. I guess for it to be "uncertain" rather than "certain of short timelines", maybe part of the assumption is also that longer timelines also seem plausible. Is that half of things something that you're intending to include? If so, how does that play into the strategy?

**Samuel Albanie** (00:23:02):
Yeah, so I think one aspect of the plan currently is that there's still a relatively... I mean, this is a subjective statement, but there is some diversity in the portfolio. There are a collection of different approaches, and in the most accelerating worlds, some of those options do not make that much sense, but we are still in a regime where because there is this uncertainty, some diversity on the portfolio makes sense. That's roughly the trade-off we're making here.

## Approximate continuity and the potential for accelerating capability improvement <a name="approx-continuity"></a>

**Daniel Filan** (00:23:36):
I think the interesting part of this assumption comes in in the interplay with the next assumption. The next assumption is "approximate continuity", and this one, I think I actually misunderstood it the first time I saw it written down. Can you tell us just: what is the "approximate continuity" assumption?

**Samuel Albanie** (00:23:53):
Yeah, so this is the assumption that improvements in AI capability will be approximately or roughly smooth with respect to some of the key inputs to the process. The kinds of inputs we're thinking about here are computation and R&D effort, but not necessarily something like calendar time.

**Daniel Filan** (00:24:17):
Okay, so if I put this together with potential for accelerating improvement, what I get is that it is plausible that there's this kind of increasing ever-quickening cycle of improvement where, well, maybe compute goes in relatively continuously with calendar time, but R&D effort increases and increases quite quickly. Improvement in capabilities is pretty smooth with the amount of R&D input and the amount of compute, but in real time, plausibly R&D input increases very, very quickly and, therefore, capabilities increase very, very quickly.

**Samuel Albanie** (00:25:02):
Yes, that's right.

**Daniel Filan** (00:25:05):
The thing that confused me here is that in the "approximate continuity"... My understanding of the consequence of that assumption is that you could have some sort of iterative approach where you do empirical tests, see how things are going, and then things will go like that for a little while because it's continuous. If things are going very fast in calendar time, I would have imagined that it would be pretty hard to... If I imagine trying to do an iterative approach, what I imagine is [that] I do some experiment, it takes me some amount of time to do the experiment, then I think about the results for a little while, and I'm like, "Okay, this means this," and then I work on my mitigation for any problems or I implement something to incorporate the things I learned from that experiment into what's happening. As long as I or another human am the one doing that, I would think that that would be pretty closely related to calendar time, but if things are not necessarily continuous in calendar time, then I'm confused how this approach is able to work.

**Samuel Albanie** (00:26:24):
Yeah, so one framing of this is that because... And it does rely on very careful measurement of the R&D capabilities of the models, so as calendar time shrinks, the assumption here is that in the scenario you're describing, the R&D capabilities' net is growing very significantly, and so what corresponds to a delta will be perhaps very, very short in calendar time, but nevertheless, can still be tracked, and the replanning and reassessment of risk needs to happen at shortening time scales. And so if there was to be a mitigation or a pause or a stop, that is how it would be implemented.

**Daniel Filan** (00:27:08):
Okay. Maybe the thing I'm confused by there is: it seems like it might happen faster than we can... It takes a while to consider things and to think about how to do a mitigation. And so was the thought like: this is feasible because the AIs who are doing the R&D will be thinking about all of that? Or is the assumption: at this stage, all we're doing is going to be keeping track of the... You're not writing papers on various types of optimizers anymore, the AIs are doing that. All you're doing is thinking about how to react to changes in the R&D input. Yeah, I guess I'm wondering just what does it look like to actually implement this in a world where capabilities are growing super, super quick in calendar time, but continuously in R&D effort?

**Samuel Albanie** (00:28:06):
Yeah, so the way that I've been thinking about it is there are measurements being made and a continuous assessment of safety buffers projected into the future. As progress goes up, there's a sort of scanning horizon over which we think we can continuously perform the kinds of tests, mitigations, and checks that we think would be necessary to continue to the next stage. Those would become closer and closer in calendar time, and if we hit some component of the system, some quantum, some setting that meant that it was not safe to continue on the basis of the shortening time scales, then the system would have to stop.

(00:28:50):
It's more that that's not a foundational axiom of the plan. That would just be downstream of the fact that a mitigation was not appropriate for a certain time-scale, but in principle, it's not as a consequence of the shortening time scale itself, though it may in practice be the case that that is a limiting factor because we're not able to operate a system that we feel comfortable with.

**Daniel Filan** (00:29:13):
Okay, so the thought is something like: at any given point in time, you'll have some safety measures or whatever, and you can see that they work pretty well, and you can see that they're going to work for the next, let's say, doubling of R&D input. Then, once you've 1.3-x-ed R&D input, you figure out some new safety mitigations that will bring you further past that, and then at this stage, you figure out mitigations for what will happen further past that... Am I understanding you correctly this far?

**Samuel Albanie** (00:29:50):
Yeah, that part is correct.

**Daniel Filan** (00:29:52):
Okay, but in that case, it seems like the... Well, I guess this seems like we're going to have to really be leaning a lot on the AI R&D workforce to do a lot of the work of coming up with these new safety mitigations and stuff. If I'm having... Suppose these milestones are coming up every three days for me. Maybe DeepMind just has all these people who can think of all the necessary safety mitigations in three days, but then it speeds up to it's every one and a half days and it's too fast for even the Google DeepMind people. Am I right that [to deal] with this, [it] seems like a lot of the work is going to have to be outsourced to these AI researchers?

**Samuel Albanie** (00:30:42):
In the regime in which things are moving quickly? Yes, that is a fairly foundational component. And most likely one of the things that will cause a risk assessment that says things need to pause or halt are the complexity of establishing those schemes. If it is the case that we cannot get to a sufficient level of confidence that the scheme can continue, that is the kind of thing that would stop progress.

**Daniel Filan** (00:31:11):
I think this helps me get a better sense of what's being assumed here and the actual work that this assumption is doing and also the limitations of it.

(00:31:19):
Maybe this gets to perhaps a thing which I thought was different between this plan and some others that I've seen. If I look at... I don't think this is Anthropic's official safety plan, but there is this blog post called ["The Checklist"](https://sleepinyourhat.github.io/checklist/) that [Sam Bowman](https://sleepinyourhat.github.io/) wrote that I thought was relatively influential, and it is basically framed around: we should automate AI research and development. In particular, we want to automate safety work, and all of our work right now is to figure out how to automate AI safety work. And at the start of the ["Approach to Technical AGI Safety and Security"](https://arxiv.org/abs/2504.01849), one thing it says is, "our approach in this paper is not primarily targeted at... [automating] AI safety research and development."

(00:32:12):
I'm compressing that quote a little bit, but hopefully that's a fair characterization of it. And on the one hand, I was going to ask, "Okay, well, why is there this difference?", but it sounds like if I combine the potential for accelerating improvement and approximate continuity, it sounds like this plan really is going to rely very heavily on automated AI safety research and development. So I guess I'm confused. Can you help me understand what's going on?

**Samuel Albanie** (00:32:42):
Sure. Yeah, that's a great question. I think one framing of this is that that approach is implicit in our plan if the trajectory rolls forwards in a certain way. That is to say that if AI development does accelerate very quickly, and if it was the case, then our plan moves closer and closer to that setting. In some sense it's a slightly more diversified portfolio currently that would collapse or concentrate according to how things develop.

**Daniel Filan** (00:33:15):
Okay, so when it said it was not primarily targeted at that goal, it sounds like how I should understand that is you were not assuming that you definitely will try and automate AI safety research and development as a thing, but you also aim to make sure that you could do that in the world where that's possible or in the world where you have accelerating AI research and development, which you think is plausible.

**Samuel Albanie** (00:33:44):
Right. That's correct, and it's not that we would escape any of the... As you are no doubt aware, there are many significant challenges to be overcome to implement that strategy. I think it's discussed briefly in the paper, this idea of bootstrapping and the challenges of using one aligned assistant to align its successor. Given those difficulties, it is highly plausible that progress is bottlenecked by an inability to make a strong safety case that progress can continue.

**Daniel Filan** (00:34:15):
Maybe we should move on to approaches to mitigating risks described in the paper as opposed to the assumptions, unless there's more that you want to say about the assumptions.

**Samuel Albanie** (00:34:26):
No, that sounds good to me.

## Misuse and misalignment <a name="misuse-and-misalignment"></a>

**Daniel Filan** (00:34:29):
Okay, so it seems to me that the two types of risk that are... Or perhaps "types" is a slightly wrong word, but the two things the paper talks about the most are misuse and misalignment, where "misuse" is roughly somebody directs a model to do a bad thing and the model does the bad thing, and "misalignment" is the model does a bad thing knowing that it's bad, but not because someone else got it to do the bad thing. Is that roughly right?

**Samuel Albanie** (00:35:04):
Yeah, that's a good summary. I mean, there's some slight nuances, but I think that's a good high-level summary.

**Daniel Filan** (00:35:13):
Oh, I'm curious about the nuances, actually, because one thing I noticed is: at one point in the paper, "misuse" is described as a user gets an AI model to do a bad thing, and "misalignment" is described as an AI model deliberately does a bad thing or knowingly does a bad thing, and in that definition, misuse could also be misalignment, right?

**Samuel Albanie** (00:35:35):
Yes, that is a good point. The risks don't form a clean categorization. They are neither exhaustive nor exclusive. They are not exclusive in the sense that you could have, for example, a misaligned AI system that recruits help from a malicious actor to exfiltrate its own model weights, which would then be a combination of misuse and misalignment.

(00:36:00):
On the other hand, given the scoping of the paper, we don't have all possible risks like AI suffering, for example. The main benefit of the risk areas is for organizing mitigation strategies, since the types of solutions and mitigations needed tend to differ quite significantly depending on the source of the potential harm. Misuse involves focusing on human actors with mitigations like security, filtering harmful requests and so on, while misalignment requires focusing on the AI's internal goals and learning process and involves better training objectives, amplified oversight, and so on.

**Daniel Filan** (00:36:41):
I think that's fair enough to say. That's one concern about perhaps over-inclusion of things or over-inclusion of requests into the inherent misuse bucket. Perhaps another concern is under-inclusion. One thing that I believe you mentioned in the paper is [that] one example of a thing that could count as misuse or misalignment is: you have one AI asking another AI for information that helps the first AI do bad stuff. The first AI is misaligned and the second AI is misused.

(00:37:12):
And it strikes me that a lot of the discussion of misuse is imagining things that are roughly human actors. A guy or a collection of people is going to make a nuclear weapon and we don't want that to happen because they are the wrong people to have nuclear weapons. It does strike me that the requests that we don't want answers to [for] other AIs could potentially be different from things we're imagining in the [CBRN](https://en.wikipedia.org/wiki/CBRN_defense) space. For instance, how do you evade certain controls and stuff? With the previous answer, I think it's fair enough to say, "Look, it's not really a technical question that we're trying to address," but "what information would it be very dangerous to give another AI?" does strike me as more close to a technical question. So I'm wondering: do you have thoughts on what dangerous requests look like in the context of AIs interacting with each other?

**Samuel Albanie** (00:38:23):
That is a great question. It is deferred and left out of scope for this technical document, but it is something that people are thinking a lot about. I don't have a great pre-baked answer other than to say it's something where as the capabilities continue to improve, I think that that threat landscape is becoming much more salient, and I just expect there to be significantly more work going forwards. But not something that's in scope for the work here.

**Daniel Filan** (00:38:53):
Would you say this kind of falls under the regime of access control and monitoring in the misalignment mitigation section?

**Samuel Albanie** (00:39:06):
I think to some degree there are components of that, but you have described exactly the potential of one failure case of this scenario. The case in which harm is achieved in aggregate or risks are accumulated piecemeal across many actors such that no individual actor... perhaps across different AI developers. We're not explicitly handling that in this approach.

## Societal readiness <a name="societal-readiness"></a>

**Daniel Filan** (00:39:34):
Fair enough. Perhaps to get back to the more core misuse thing: you talk about doing threat models and evaluations for specific mitigations that are safety post-training, capability suppression, and monitoring, and also access restrictions, which I think makes a lot of sense in the light of which requests are dangerous depends on who's making the request. You also have this additional section which is "security", in the sense of I believe security of the weights of the model, and also "societal readiness", are also aspects of the misuse domain, I guess.

(00:40:23):
I think security of model weights is a thing probably a lot of people in the AI safety space have heard about or thought about a little bit. Societal readiness seems if anything perhaps underrated or under-thought-about in these spaces. I'm wondering if you have thoughts about what that should look like or how that looks, especially from a technical angle.

**Samuel Albanie** (00:40:51):
I think one example that's a nice one to give the idea here relates to cybersecurity, and I believe it's the one discussed in the paper, where as AIs become more capable at cyber offense, one way to reduce the misuse risk is to contribute those capabilities to the hardening of many bits of societal infrastructure, which currently... Well, I'm not well-qualified to make an assessment on the overall risk state, but vulnerabilities exist in many cases. That's an ongoing process of hardening.

**Daniel Filan** (00:41:31):
Yeah, and I believe a previous guest on the podcast, [Jason Gross](https://axrp.net/episode/2025/03/28/episode-40-jason-gross-compact-proofs-interpretability.html), is thinking about this to some degree.

**Samuel Albanie** (00:41:36):
Oh great.

**Daniel Filan** (00:41:40):
It sounds like this is mostly thinking about using existing AI in order to harden up bits of societal infrastructure, to make bits of societal infrastructure less vulnerable to things. Perhaps if there was some way to use AI to make it easier to make vaccines for things or to make it easier to make things that stop you from being damaged by a chemical weapon, it sounds like that would also fall under this umbrella.

**Samuel Albanie** (00:42:07):
That's the key motivation. Yeah.

**Daniel Filan** (00:42:09):
Fair enough. I'm wondering: one thing that feels related in spirit, although less technical, is: there are various labs such as Google DeepMind, such as OpenAI, such as Anthropic, that work to release models to the public. One reason is they do cool stuff and it's valuable to have them be released, but I think another theory of change for this is just [that] it is useful for the public to know what AI capabilities actually are so that they know how worried they should be, so that they know what things they should want to be done about it. If I think just colloquially of societal readiness for AGI, it strikes me that at the moment this is probably the biggest thing driving societal readiness of AGI. I'm wondering: does this count as in scope for what you're thinking of as societal readiness?

**Samuel Albanie** (00:43:12):
Oh, that's a nice question. It is certainly the case that I share your sentiment that that is one of the most effective ways to increase current readiness, though there are clearly trade-offs here. Yeah, I'd have to think a little more as to whether it was motivated from the same angle, but certainly I think it does have commonalities. I believe your phrase "similar in spirit" is a good way to characterize it.

**Daniel Filan** (00:43:45):
Fair enough.

**Samuel Albanie** (00:43:48):
It's a little less explicit. I mean, there are so many other things going on there, but perhaps a positive side effect.

## Misuse mitigations <a name="misuse mitigations"></a>

**Daniel Filan** (00:43:58):
Fair enough. And then finally with misuse, you mentioned that, okay, there's going to be basically red and blue teams to stress-test misuse mitigations and also safety cases - some sort of structured argument for why misuse is unlikely or impossible. Then, you can try and investigate the assumptions. I think this is - I also kind of see this in the misalignment section - there's these red-blue team exercises, red-teaming assumptions, getting safety cases for alignment. I'm wondering, do you think these are going to look very similar or do you think they look pretty different? If they look different, how did it come to be that the assurances for misuse and for misalignment look so similar structurally?

**Samuel Albanie** (00:44:56):
That's a good question. I suppose with many of the cases in misuse as we're characterizing it, we have some experience and fairly concrete ideas of what the risk factors look like. I think that concreteness lends a lot of opportunities for the sorts of strategies that red teams can be expected to deploy. There's a pretty clear idea as to who potential threat actors are, the kinds of strategies they might use. In the case of the misalignment work, because some of these threats and risks are... they're not novel necessarily conceptually, but our experience with working with them is relatively new. I do expect there to be some similarities based from that perspective.

**Daniel Filan** (00:45:43):
Fair enough, so-

**Samuel Albanie** (00:45:48):
To give some kind of concrete example, when thinking about misuse, "know your customer"-style checks are leveraging external history of a particular user in the outside world and using that as evidence about their intention, and that kind of affordance is not going to be available in the misalignment setting in the mitigations we're setting. I expect there to be many such cases that distinguish between them, but at a broad level, adversarially testing the robustness of the system is kind of a generically good thing to do.

**Daniel Filan** (00:46:20):
Yeah. Well, the "know your customer"... I mean, in some sense this seems similar to stuff like access control for AI.

**Samuel Albanie** (00:46:30):
Access control would be similar I believe.

**Daniel Filan** (00:46:32):
Yeah, and in some sense it's kind of similar to "know your customer", right?

**Samuel Albanie** (00:46:36):
Well, there's two things. One is access control, and the second is the kinds of evidence you're accumulating about whether something can be trusted.

**Daniel Filan** (00:46:45):
Fair enough, fair enough. But it does remind me that there has been [some amount of stuff written about infrastructure for AI agents](https://arxiv.org/abs/2501.10114) that comes sort of close to infrastructure we have for humans doing things that could potentially be dangerous. But it's fair enough to say that for misuse, we're potentially thinking of things that are more precedented. I wonder: maybe is that a consequence of the assumption that we're only looking for the "exceptional" AGI? I could imagine a world where AI gets good enough that humanity learns of some weird, dangerous things. I believe there's [some book](https://nickbostrom.com/papers/vulnerable.pdf) where [Nick Bostrom](https://en.wikipedia.org/wiki/Nick_Bostrom) uses this example of, "Well, we could potentially live in a world where if you took some sand and you put it in the microwave and you microwaved it for five minutes, you've got this highly dangerous explosive."

**Samuel Albanie** (00:47:51):
The vulnerable world.

**Daniel Filan** (00:47:52):
Yeah, this vulnerable world, and you could imagine that maybe we develop AGI and at some point it teaches us of these vulnerabilities. We don't just have to worry about nuclear weapons, we also have to worry about sand weapons or some other thing that we haven't thought about before.

**Samuel Albanie** (00:48:09):
[Ice-nine](https://en.wikipedia.org/wiki/Ice-nine), yes.

**Daniel Filan** (00:48:10):
Yeah, Ice-nine is so scary. Okay, so ice-nine, as you mention in the paper, it comes from [this story](https://en.wikipedia.org/wiki/Cat%27s_Cradle) by [Kurt Vonnegut](https://en.wikipedia.org/wiki/Kurt_Vonnegut) where it's this different version of water that's solid at temperatures below like 45 degrees Celsius, and any normal water that touches ice-nine becomes solid. Then, it just takes over the world. That hasn't happened in water, but that [really has happened with certain chemicals in the world](https://en.wikipedia.org/wiki/Disappearing_polymorph). There are drugs that don't work anymore because basically this thing happened - more than one of them. It's one of the creepiest... This fact just creeps me out so much. Where was I?

**Samuel Albanie** (00:49:06):
I think you were probing about-

**Daniel Filan** (00:49:08):
Yeah, I was long-windedly -

**Samuel Albanie** (00:49:09):
...well, there is this component of there may just be a lot of unknown unknowns that are baked into the ecosystem that will be revealed as the models become more capable.

**Daniel Filan** (00:49:19):
Yeah, and I'm wondering: if you're thinking of misuse as, "Okay, there are basically known dangers", is that a consequence of an assumption that we're talking about AI that is a little bit smart, but not wildly superhuman?

**Samuel Albanie** (00:49:36):
The comment on known dangers, I think I perhaps would use that more as a reflection on the maturity of those fields currently rather than maybe a fundamental distinction between them, just because the relative capabilities of AIs and human threat actors are in the state that they are currently, but the affordances of both I do expect to change over time. For example, risks that come from the fact that the AIs can absorb very large amounts of content concurrently or execute at extremely high speed will mean that plausibly there are risks that were not tractable in the case of human operatives that are now tractable.

**Daniel Filan** (00:50:22):
Yeah, I mean, it seems like it plays into the mitigation... The misuse mechanisms - there's safety post-training, there's capability suppression, and there's monitoring. It seems like those rely on knowing which things you have to post-train the model to not talk about, knowing which capabilities you've got to suppress, and knowing which things you've got to monitor for. Whereas, if AI is smart enough that it can discover- that it can learn about a new vulnerability in the world. It lets some humans know about it and then humans start exploiting it. If that happens before developers are able to realize what the issue is, figure out what capabilities they should suppress, figure out what questions they should get the model to not answer, figure out things they should monitor for, I think in that world, those misuse mitigations become weaker. It seems like there must be some assumption there, unless I'm misunderstanding how general these tools are.

**Samuel Albanie** (00:51:31):
That is correct. There is explicit threat modeling that goes on to try to identify the kinds of misuse risks that we think should be prioritized. [There's] explicit thought about what capability levels pose risks for certain threat actors, and then mitigations are implemented downstream of those. And so there needs to be a kind of continuous scanning of the horizon for new risks that may materialize, but it is not the case that they are sort of baked in in some implicit way into the plan.

**Daniel Filan** (00:52:03):
Yeah, and I suppose one nice thing about that is that if you're a model developer and if you're worried about new vulnerabilities being found by AI, if you have the smart AI before anyone else does, then maybe that helps you scan the horizon for vulnerabilities that you should care about and you might hope that you'd be able to find them before other people do?

**Samuel Albanie** (00:52:24):
There's that. These things have these dynamics of a so-called "wicked problem". They're very entangled together, and I think this is often described as one of the challenges of an open source approach where if it was the case that such a vulnerability was discovered, the inability to shut down access... there's an additional challenge. It may still be the case that the trade-off is worthwhile under the collective risk judgments of society, but that's a trade-off with the different approaches.

## Misalignment mitigations <a name="misalignment-mitigations"></a>

**Daniel Filan** (00:52:57):
Sure. Maybe we should talk a bit more about the misalignment mitigations discussed in the paper. At a high level, I take the misalignment mitigations to be, "Okay, try and make the model aligned, try and control the model in the case that it's not aligned, do some miscellaneous things to make the things you've done work better, and also get assurance of good alignment and good control." Does that seem...

**Samuel Albanie** (00:53:30):
I think that's a good characterization. Yes.

**Daniel Filan** (00:53:32):
Okay, cool. For alignment, there's amplified oversight, guiding model behavior, and robust training. I found this kind of interesting in that it's a little bit different from what I think of as the standard breakdown of how to do alignment. The standard breakdown I sort of conceive of as: do a thing that people usually call "scalable oversight", which I think is close enough to what you mean by "amplified oversight", and then deal with this potential for inner misalignment where even though we haven't reinforced AI for doing bad things, it does bad things anyway because it wanted to do bad things at one point, then it decided to play nice for a while... Just figure that out, somehow deal with that.

(00:54:20):
Whereas, amplified oversight, guiding model behavior and robust training, it seems like a bit of a different breakdown than what people normally talk about. I guess the first question is: amplified oversight, is that the same as the thing people talk about when they talk about scalable oversight? Or do you mean to draw that boundary a little bit differently?

**Samuel Albanie** (00:54:40):
In both cases, debate is kind of a canonical method. One reason for the "amplified oversight" distinction is just that the term "scalable oversight" has been used for many things. We use it somewhat similarly to, say, [Sam Bowman](https://sleepinyourhat.github.io/)'s [paper using this terminology](https://arxiv.org/abs/2211.03540). There's a technical definition in the paper which I believe is something like: for a given input-output pair, achieving oversight that is as good as could be achieved by a human if they understood the reasons why an output was produced by an AI and they were given unlimited time to reflect on the decision.

**Daniel Filan** (00:55:15):
Sure.

**Samuel Albanie** (00:55:16):
Maybe to comment on your previous remark about this breakdown, I think these do map still relatively closely to the distinctions you made. So we often use this terminology of "specification gaming", roughly outer alignment or outer misalignment if it's gaming, and "goal misgeneralization"... Sometimes the term "inner misalignment" is used for that. And the latter component of the plan that you mentioned, the "robust training" is targeting goal misgeneralization, so to some degree there's an overlap there.

**Daniel Filan** (00:55:51):
Sure. So if robust training is targeting the goal misgeneralization, what is the "guide model behavior" thing doing?

**Samuel Albanie** (00:55:59):
Guiding model behavior? Yeah, so the core premise is: let us suppose we have a good solution to amplified oversight. It is likely to be extraordinarily expensive as a mechanism for obtaining high-quality supervision, and therefore we want to get the most juice out of that we can. One way to do that is with standard training techniques, but there is a possibility that there will be other more data-efficient ways to do this, and so the "guiding model behavior" is just trying to encapsulate how we're actually going to apply these labels to the model. It could be these things like natural language critiques or if there are other mechanisms that will make use of the labels more efficiently.

**Daniel Filan** (00:56:43):
Okay, so to help me understand this a little bit better, so suppose I take constitutional AI, where roughly what happens is a human writes a constitution - maybe this is done in an amplified method where they think about it really hard with AI help or something - and then some reward model looks at the constitution and looks at AI outputs and grades them. Would that count as the kind of thing you're talking about in guiding model behavior? Or is that something else?

**Samuel Albanie** (00:57:15):
Yeah, so the process of translating the constitution into the learned behavior of the model, that's roughly what we're encapsulating there.

**Daniel Filan** (00:57:22):
Okay.

**Samuel Albanie** (00:57:23):
Then, to the degree that it was thought that somehow the constitution was underspecified, then you would come into the regime closer to the robust training, the selection of samples and active learning and mechanisms to make sure that you have good coverage.

**Daniel Filan** (00:57:38):
Fair enough. Yeah, I guess I'm wondering where the line is between guiding model behavior and robust training. They have slightly different vibes, but I think of robust training as training mechanisms to make sure the model does the thing, and guiding model behavior also sounds like training mechanisms to make sure the model does the thing. If I have adversarial training, maybe that counts as robust training. If I'm trying to provide a reinforcement to the chain of thought, I might hope that this makes the thing more robust, but maybe it also is for guiding model behavior. In real life, I think probably that's a bad method, the thing I just said, but where do you see the line between these two things?

**Samuel Albanie** (00:58:33):
I think the key component is primarily just this emphasis on getting robust generalization, so to the degree that that comes for free from your training method, then you're good to go, but since we often expect that we might need explicit approaches for achieving that, that's roughly what we're trying to encapsulate in the robust training bracket.

**Daniel Filan** (00:58:54):
I guess maybe it's a distinction between research directions rather than between techniques. So the research direction of providing oversight just anywhere you can, maybe that counts as guiding model behavior, and the research direction of making it robust as you can, maybe that counts as robust training, but maybe there's a bunch of things that could come out of either research direction?

**Samuel Albanie** (00:59:17):
Yes, so I may have misunderstood your point. I think that to me there's still a relatively strong distinction. This first component: get really good labels. Second component: use those labels to train the model. And the third part is: really focus on making sure we have good generalization. And I may just be repeating what you previously mentioned, but to the degree that that is covered implicitly by your second part, you could fold them in if that's a cleaner distinction for you, but the third part is just to say this is an important part to focus on.

**Daniel Filan** (00:59:51):
Yeah. Okay. Maybe I should just stop making "yeahhhh" noises.

**Samuel Albanie** (00:59:56):
No, it's good if we can get it clear because I may have misunderstood or...

**Daniel Filan** (01:00:03):
... So, making sure that you're applying your labels in a smart way in some sense... Well, it seems like the distinction is when you're coming up with the techniques, are you thinking more about generalization or are you thinking more about label efficiency? You might use the same or very similar techniques in both and you might be doing very similar things, which is relevant because to the extent that you were thinking of the first one as the "specification gaming" one, the second one as the "goal misgeneralization" one, it seems like "guide model behavior" could help with either specification gaming or goal misgeneralization or both, just depending on how you're doing it.

**Samuel Albanie** (01:00:55):
That is fair. Yes.

**Daniel Filan** (01:00:57):
And to the degree that you think "specification gaming versus goal misgeneralization" is definitely the right way to carve up all problems, then that's going to give you one perspective. I don't know, if you think guiding model behavior is very different from robust training, than maybe you want to think of a different breakdown that is slightly different from that old breakdown... I don't know, that strikes me as kind of interesting, I guess.

**Samuel Albanie** (01:01:25):
I see, so let me try to paraphrase and see if I've understood your point. Your point is in the past many people have had two boxes.

**Daniel Filan** (01:01:34):
Yeah.

**Samuel Albanie** (01:01:34):
We have three boxes.

**Daniel Filan** (01:01:36):
Yeah.

**Samuel Albanie** (01:01:37):
Three is different from two.

**Daniel Filan** (01:01:38):
That's part of my point, and then part of my point is when I look at "guide model behavior" and when I look at robust training, they seem like they maybe blend into each other. It seems like there could be... They're both fundamentally about how to train things and what you do and where you apply reward signal.

**Samuel Albanie** (01:01:57):
I think that is fair. Yes.

**Daniel Filan** (01:02:02):
You then talk about various methods that can basically make other mitigations for misalignment work better, and one of them is interpretability. At the start of the paper - or somewhere in the paper - there's this interesting sentence that says, "Interpretability research is still quite nascent and has not yet enabled safety-crucial applications". And the conclusion is, therefore, that more basic research is needed. People have been working on interpretability for a while. You might think that at some point, if it hasn't enabled any safety-crucial applications, we should stop doing it, so why is the thought "more basic research is needed" versus "let's just give up"?

**Samuel Albanie** (01:02:46):
Yeah, so I think a few things come to mind here. One is just about relative effort that has been expended into the field. It is true that effort has gone into understanding neural networks, but as a total fraction of all effort, I don't have a good sense of being able to quantify it, but it's not clear to me that we've exhausted the limits of what is possible by pushing more effort in. I guess it really comes down to, what is our expected return on investment? There, there's a bit of a risk-reward calculation, and so part of the incentive here is to think, "Well, big if true".

(01:03:22):
If we did get these benefits, they'd be really big. There is some uncertainty and maybe they're a slightly risky bet, but that in itself is part of the core justification. There's a second slightly more pragmatic component, which is that in teams - of which I think our team is an example - there are a collection of individuals who have differences of research taste and different perspectives on what is promising. We allow those also to inform the overall direction. It's a kind of combination of bottom-up and top-down, and so if people have clear visions and clear perspectives of how they think something has tractable route to action, that's also an argument for going forward. There's one other point, but I can skip it for the sake of not talking too long on one topic, if it's not-

**Daniel Filan** (01:04:12):
Sure. Well, I actually love talking too long on one topic.

**Samuel Albanie** (01:04:16):
Okay.

**Daniel Filan** (01:04:17):
Perhaps it's a vice.

**Samuel Albanie** (01:04:19):
Well, in that case: one thing I think quite a lot about is this idea of how things can act differently at different scales. Now I suppose this has been widely studied. My first encounter with this was in the analysis of [Richard] [Hamming](https://en.wikipedia.org/wiki/Richard_Hamming), looking at how in many fields as the parameters of the field change, sometimes the science changes. For example, if you're in biology and you have a lens that allows you 10 times greater magnification, you just start to see fundamentally new things. In the field that we're currently operating, we're blowing through many orders of magnitude on various axes. It may well be the case that the field is in some sense fundamentally new or looking at new regimes and opportunities that were not there previously. That's the second reason why some uncertainty over what is possible also seems appropriate.

## Samuel's thinking about technical AGI safety <a name="samuel-thinking"></a>

**Daniel Filan** (01:05:20):
Maybe to go back to some of the things I started with: I'm wondering how this whole process has shaped your thinking on the issue of technical AGI safety. For instance, has it made you feel more confident in the assumptions? Has it made you feel less confident? Has it changed your views on which research you're more excited about?

**Samuel Albanie** (01:05:42):
Yeah, that's a great question. I think one of the primary consequences for me is that it encouraged me to look much more deeply into one of the specific scenarios, the ones that we discussed related to the most aggressive acceleration, and to focus more of my own research effort around those scenarios, accepting that it's plausible that they don't go ahead, but for some of the reasons we discussed earlier, these are sci-fi scenarios to think through and very challenging conceptually to reason about. Perhaps the greatest update for me has been to look at the arguments in some detail about how plausible those sorts of feedback loops are and to upweight their importance, at least in my own mind, and to spend more time on it.

**Daniel Filan** (01:06:35):
If listeners want to think about this a little bit more: so obviously there's the section in the paper talking about it, and you mentioned [this work by Epoch looking at the returns to AI research and development](https://epoch.ai/blog/do-the-returns-to-software-rnd-point-towards-a-singularity). Is there anything else that you found especially useful for trying to think about what the scenario looks like and the likelihood of it?

**Samuel Albanie** (01:06:55):
Yeah, so I think some of the nicest write-ups of this are [the work](https://www.forethought.org/research/how-suddenly-will-ai-accelerate-the-pace-of-ai-progress) [recently](https://www.forethought.org/research/will-ai-r-and-d-automation-cause-a-software-intelligence-explosion) [put out](https://www.forethought.org/research/three-types-of-intelligence-explosion) from [Forethought](https://www.forethought.org/) - this would be [Tom Davidson](https://www.linkedin.com/in/tom-davidson-38b87b35/), [Will MacAskill](https://www.williammacaskill.com/), I believe there are [some](https://www.linkedin.com/in/finm/) other [authors](https://www.linkedin.com/in/lizka-vaintrob/) I can't recall off the top of my head - that has tried to analyze questions like, "Okay, what is the plausibility of an intelligence explosion? What kind of dynamics are likely to play out?" They do these taxonomies looking at, "Well, what if it was to happen only in software? What if that then progressed into chip design and then later into hardware, ultimately leading to an industrial explosion? What kind of timelines are plausible?"

(01:07:33):
There's lots of nice analysis that's been put out on those questions and that you can go in and critique it for yourself. One thing that I've tried to do is to connect it back to some of the more recent work. I think [METR](https://metr.org/) has done a fantastic job of this: of conducting evaluations of current systems and trying to get high-quality evidence about where we are today and what kind of trend line we're on, and then trying to bring these two things together into the same picture. Aiming for that kind of synthesis is one of the things I've been thinking about a lot.

**Daniel Filan** (01:08:05):
Yeah, that makes a lot of sense. Any preliminary results from trying to do that synthesis?

**Samuel Albanie** (01:08:14):
I'm a big fan of [the recent work from METR](https://arxiv.org/abs/2503.14499) on the task horizons of AI agents at the frontier, and I've been trying to grapple with: do I think these are representative? Do I think this is roughly how progress is going to go? Just the process of trying to operationalize these claims, which are very vague and somehow based on vibes in many discussions about like, "Is progress fast? Well, I use this chatbot and it did this thing for me and I have these three test cases and two of them never worked before, but now suddenly it works". I really like these efforts to formalize things. I also think that they highlight some of the real methodological challenges of making good work here, and to their credit, they're very precise in documenting all of the nuances involved.

(01:09:06):
Just to give one concrete example, I think there's quite an important distinction between what they described in the paper as low-context tasks and high-context tasks. For the sake of making comparable benchmarks, they use low-context tasks. These are roughly tasks that don't require a lot of onboarding. But onboarding as a phenomenon, I personally think - though this could be falsified by time - may be a key advantage for the models over humans in many regimes. If we do not account for that when estimating task durations, that's something that could cause a skew in one direction in the time horizons. There are many other cases of things in other directions, but there are many details that you have to get into to do this kind of analysis. I think they've done a great job of doing some of the first work here that is pretty rigorous.

**Daniel Filan** (01:09:57):
Sure. In terms of onboarding being a key advantage that AIs could have, is that just because if you have a language model, it's just read all of the internet and so it knows more background information than any given human does?

**Samuel Albanie** (01:10:09):
A lot of it, in my opinion, is to do with bandwidth. As a human executing a task, we tend to spend some time learning on the task - let's take a particular coding project. And we sort of amortize that time spent getting familiar with a code base or learning about the tools and technologies that we require, we amortize it across the subsequent tasks that are relevant to it. Whereas, the model operates more in a regime where it may be able to perform all of that onboarding close to concurrently - with a very large context window, absorb much of the relevant information. So far, it has not been the case that that information has been directly made available to the models.

(01:10:51):
There may be something of a context overhang here, where if you think how you as a human execute a complex task when you're doing onboarding, you access lots of kinds of information that we're not currently passing to the models, and it may be the case that as that information becomes available, then their ability to execute some of these tasks go up. It's not clear that this will absolutely be true or the case, but it's an example of a nuance that you get into once you really try to operationalize these things that could have quite big consequences for the projected timelines.

**Daniel Filan** (01:11:22):
Fair enough. You mentioned that thinking about this had shaped... [That] you thought about what kinds of work you could do that would be relevant to this scenario. What did you end up thinking of?

**Samuel Albanie** (01:11:37):
I've spent time thinking about a few directions. One is learning more about model weight security. It's plausible that that will become quite important in worlds in which capabilities grow quickly and a cursory knowledge is somewhat insufficient to make good judgments about what is likely to happen and how things will play out. A second thing I've been thinking a lot about is tools that can improve decision-making, particularly for people who will be in positions of allocating resources. If we are in those regimes where calendar time shrinks, we want to have done a really good job of supporting them and setting up platforms and ways of processing information that are succinct, high signal-to-noise, and also robust to misalignment threats.

**Daniel Filan** (01:12:31):
Yeah, that seems right. I guess another thing that I've been thinking about is that - and maybe this doesn't count quite as a technical approach to misuse or misalignment - but to the extent that some of the assumptions are "it is plausible that we have very short timelines" and "it's plausible that we have accelerating improvement", probably one of the most relevant things to do is to just check if that's true or not, to get as much leading indicators as we can. Off the top of my head, I don't actually know if this is discussed in the paper.

**Samuel Albanie** (01:13:06):
It's not something we go into [in] much detail in this paper. It is something I've given some thought to, but it is a very difficult question. There's sort of two questions here. There's "is it likely? How likely?" And there's a second question of "when?" In some sense, it's easier to get evidence about the second if you have a model or some smoothness assumptions about how things are going to go, but on the plausibility question, there are very interesting discussions. Yeah, I will just, I think, refer readers to the Forethought write-ups on their assessments of various factors affecting plausibility-

**Daniel Filan** (01:13:40):
Fair enough.

**Samuel Albanie** (01:13:40):
...but I agree it is a very important question.

**Daniel Filan** (01:13:44):
Okay, we're probably going to wrap up soon. I'm wondering, is there anything that you wish that I'd asked that I have not yet?

**Samuel Albanie** (01:13:54):
Hmm. I don't believe so.

**Daniel Filan** (01:13:58):
Okay, fair enough.

**Samuel Albanie** (01:14:00):
Not one that I can come up with quickly.

## Following Samuel's work <a name="following-samuels-work"></a>

**Daniel Filan** (01:14:02):
Okay. Well, I guess to conclude, if people are interested in your research and they want to follow it, how should they go about doing that?

**Samuel Albanie** (01:14:11):
I have a profile on X. My username is [SamuelAlbanie](https://x.com/samuelalbanie).

**Daniel Filan** (01:14:18):
Okay. No underscores, no dots?

**Samuel Albanie** (01:14:21):
No underscores.

**Daniel Filan** (01:14:23):
Okay, so SamuelAlbanie on X, that's the primary place where people should follow your work?

**Samuel Albanie** (01:14:29):
I think that's a reasonable strategy.

**Daniel Filan** (01:14:31):
Okay. Well, thank you very much for coming on and chatting with me.

**Samuel Albanie** (01:14:36):
Thanks so much for taking the time. I appreciate it.

**Daniel Filan** (01:14:38):
This episode is edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. This episode was recorded at Far Labs. Financial support for the episode was provided by the Long-Term Future Fund, along with patrons such as Alexey Malafeev. To read transcripts, you can visit axrp.net. You can also become a patron at patreon.com/axrpodcast or give a one-off donation at ko-fi.com/axrpodcast. Finally, if you have any feedback about the podcast, you can fill out a super short survey at axrp.fyi.


**Daniel Filan** (03:20:30):
This episode is edited by Kate Brunotts and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. This episode was recorded at [FAR.Labs](https://far.ai/programs/far-labs). Financial support for the episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future) along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. You can become a patron yourself at [patreon.com/axrpodcast](https://patreon.com/axrpodcast) or give a one-off donation at [ko-fi.com/axrpodcast](https://ko-fi.com/axrpodcast). Finally, if you have any feedback about the podcast, you can fill out a super short survey at [axrp.fyi](axrp.fyi) - just two questions.
