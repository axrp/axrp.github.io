---
layout: post
title: "24 - Superalignment with Jan Leike"
date: 2023-07-26 20:50 -0700
categories: episode
---

[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/MThlYjczZGItZmYxZS00MDU0LWJmOGYtZGRhNWM0ODkzNGM0)

Recently, OpenAI made a splash by announcing a new "Superalignment" team. Lead by Jan Leike and Ilya Sutskever, the team would consist of top researchers, attempting to solve alignment for superintelligent AIs in four years by figuring out how to build a trustworthy human-level AI alignment researcher, and then using it to solve the rest of the problem. But what does this plan actually involve? In this episode, I talk to Jan Leike about the plan and the challenges it faces.

Topics we discuss:
 - [The superalignment team](#superalignment-team)
 - [What's a human-level automated alignment researcher?](#ai-ai-alignment-researcher)
   - [The gap between human-level automated alignment researchers and superintelligence](#human-level-alignment-researcher-vs-superintelligence)
   - [What does it do?](#what-does-it-do)
   - [Recursive self-improvement](#rsi)
 - [How to make the AI AI alignment researcher](#making-the-alignment-researcher)
   - [Scalable oversight](#scalable-oversight)
   - [Searching for bad behaviors and internals](#bad-behaviors-internals)
   - [Deliberately training misaligned models](#deliberately-training-misaligned-models)
 - [Four year deadline](#four-year-deadline)
   - [What if it takes longer?](#what-if-it-takes-longer)
 - [The superalignment team and...](#superalignment-team-and)
   - [... governance](#governance)
   - [... other OpenAI teams](#other-openai-teams)
   - [... other labs](#other-labs)
 - [Superalignment team logistics](#logistics)
 - [Generalization](#generalization)
 - [Complementary research](#complementary-research)
 - [Why is Jan optimistic?](#why-is-jan-optimistic)
   - [Long-term agency in LLMs?](#long-term-agency-llms)
   - [Do LLMs understand alignment?](#do-llms-understand-alignment)
 - [Following Jan's research](#following-jans-research)

**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Jan Leike. After working for four years at DeepMind on reinforcement learning from human feedback and recursive reward modelling, in early 2021 Jan joined OpenAI, where he now co-leads the recently-announced superalignment team. For links to what we're discussing, you can check the description of this episode, and you can read the transcript at axrp.net. Welcome to AXRP.

**Jan Leike:**
Thanks a lot for having me.

## The superalignment team <a name="superalignment-team"></a>

**Daniel Filan:**
Yeah, not at all. So, first of all, I guess we're going to be talking about [this announcement](https://openai.com/blog/introducing-superalignment) of the superalignment team. For people who somehow haven't heard of that or haven't read [that blog post](https://openai.com/blog/introducing-superalignment), can you recap what it is and what it's going to be doing?

**Jan Leike:**
Yeah, I'm excited to. So, basically, we want to set us an ambitious goal of solving alignment of superintelligence within the next four years. So, by mid-2027. And [Ilya Sutskever](https://www.cs.toronto.edu/~ilya/), the co-founder and chief scientist of OpenAI is joining the team. He's co-leading it with me. And OpenAI is committing 20% of the compute secured so far to this effort, or to the effort of aligning superintelligence. And so, we're staffing up the effort a lot. We are hiring a lot of people. In particular, we're interested in hiring machine learning researchers and engineers who haven't really worked that much on alignment before, because we think there's a lot of scope for them to contribute and have a really big impact. Yeah, we have a general overall plan of how we want to approach the problem that involves training a roughly human-level alignment researcher that can work automatically, and then ask that automated alignment researcher to figure out how to align superintelligence.

**Daniel Filan:**
Okay.

**Jan Leike:**
And so, one of the key pieces for us to do would be to figure out how to align this automated alignment researcher.

## What's a human-level automated alignment researcher? <a name="ai-ai-alignment-researcher"></a>

**Daniel Filan:**
Okay. Yeah. I'd actually like to get into this. I think in the blog post, you used the phrase "human-level automated alignment researcher", right? What should I imagine here? What is that?

**Jan Leike:**
Yeah. So basically, we want to offload as many of the tasks that we do when we're doing alignment work to an automated system [as possible]. So typically, when you're using LLMs or if you're building an AI system in general, the skill profiles they have isn't exactly what a human would do. Right? They would be vastly better at some things, like language models are now on translation or knowing facts and so on. And then the AI system would be significantly worse at some other tasks, like language models are right now with, for example, arithmetic. And so, the question then becomes: what are the kind of tasks that we can offload to the AI systems, in which order? And as we are doing this, you'd expect humans would focus more and more on the tasks that we are not offloading. And so, as we go into that process, AI systems are doing a larger and larger chunk of the overall work, and human researchers will basically thus be more and more effective at actually making progress.

**Daniel Filan:**
Okay. So, should I imagine something like, instead of you replace the first OpenAI alignment team employee, and then the second one, [we] should imagine you replace this type of task that everyone is doing, and then this type of task that everyone is doing, roughly that kind of thing?

**Jan Leike:**
Yeah. That's how I picture it going. And then, I think in order to actually get a lot of work out of the system, right, you would want to have 99 or 99.9% of the tasks being automated, because then you have effectively 10x, 100x, 1,000x as much research output.

**Daniel Filan:**
Okay. What kinds of tasks are you imagining it doing?

**Jan Leike:**
So broadly, I would throw them into two different buckets. One bucket is the tasks that look more like traditional ML engineering research that you would do if you were just trying to make AI systems be more capable. And then, the other bucket is all the other things that we have to do on alignment. And so, in the first bucket, this is stuff like you're implementing ML experiments, running them and looking at the results. And the second bucket, it's more like how do you, for example, figure out what experiments you should run to improve [scalable oversight](https://en.wikipedia.org/wiki/AI_alignment#Scalable_oversight), or how do you make progress in interpretability, right? These are really big, high level questions.

But there's also just a lot more detailed questions. [E.g. say] you have a given point that you are in research - let's say, you have just written a paper, and you're like, "okay, what do we need to do next if we continued down this route?" And so, I expect that, basically, ML in general will get really good at the first bucket of just designing, running experiments automatically. And our job of differentially accelerating alignment progress would be to figure out how to automate the second bucket.

**Daniel Filan:**
Okay. And so you're conceiving the second bucket as the full stack, from coming up with research directions to coming up with ideas of what things might work to all the way down to "what script do I run right now?"

**Jan Leike:**
Yeah. I mean, you could ask me: if I think that alignment research is so similar to machine learning research, how much is there really in the second bucket? But I think there's actually a lot in there and it's highly leveraged, because alignment as a problem is still so vague and confusing. And I think, in general, there's a lot of disagreement among experts around the most promising directions or what we should do next. And so, the more you can accelerate what we do there, it will actually have really large impact.

**Daniel Filan:**
Okay, cool.

**Jan Leike:**
This is basically the same pitch that you would give for a researcher to join the field, right?

**Daniel Filan:**
Yeah.

**Jan Leike:**
We're still trying to figure out the basics. It's a wide open research problem. We don't know how to align superintelligence or even systems that are significantly smarter than humans.

**Daniel Filan:**
It makes sense. Yeah, it's like we want to recruit AI, just like we want to recruit more people, I guess.

**Jan Leike:**
That's right.

**Daniel Filan:**
All right.

**Jan Leike:**
But there's something really beautiful about recruiting AI, which is it scales so much better and faster than humans do, because all you need to do is buy more GPUs and then you have more AI.

### The gap between human-level automated alignment researchers and superintelligence <a name="human-level-alignment-researcher-vs-superintelligence"></a>

**Daniel Filan:**
Makes sense. So one question I had is: when you said a human-level alignment researcher, it seems often in AI, most things aren't exactly human-level at anything, right? So you mentioned chat models. I think they're superhuman just in terms of breadth of knowledge, right? I think it would be hard for anyone to know as many facts as GPT-4 does. But [they're] subhuman at arithmetic, at least if the human's allowed to have pen and paper, you know? So, how important is the 'human-level' qualifier? On these lists of tasks, if it's really superhuman at some of them, is that a problem for you or is that just so much the better?

**Jan Leike:**
Yeah, I think the question is really how risky is it to run that system on the task of alignment research? Because if it knows a lot of facts, that isn't particularly scary, but what we really need to figure out is if we let the system take over some amount or ultimately almost all of our alignment research, will it lie to us? Will it try to deceive us? Will it try to take the opportunity to take over? Because now, it's doing so much stuff that we can't look at [it all] ourselves. And so, the question is the kind of skillset that you would need to do this, how does it compare to the kind of skillset that we would need to get a lot of assistance in alignment research?

And if you zoom into that question, what are actually the things that we would be worried about? This is like, how good is it? Is the model spinning really coherent lies or being deceptive or pretending to do something or believe one thing and then actually wanting another? I think another really key capability here is self-exfiltration. So, how good would the model be at breaking the security precautions and accessing its own weights and trying to copy it somewhere else on the internet? Or persuading an engineer with access to the weights to download them and send them somewhere? And so, we can specifically measure how good the models are at that and then we can compare it to measuring and how good is it at actually helping us with alignment research?

**Daniel Filan:**
Okay. And so roughly the idea is: you want the models to not be too good at these scary tasks?

**Jan Leike:**
That's right.

**Daniel Filan:**
Yeah. So, this actually relates to a critique of this line of research that basically says: okay, if I want a human-level automated alignment researcher, it needs to be pretty smart, it needs to be creative, right, it needs to think of things that we haven't thought of yet, it needs to be able to plan towards a goal - I want to get this, so I've got to do these non-obvious things in the way, I've got to learn things about the world... And it's also got to be really good at thinking about misalignment, right, in order to solve misalignment problems. And so one might think, "oh, that combination of things, that's inherently scary or dangerous." And I guess the question almost is, then: if the task is you're building something, you're aligning this automated alignment researcher, do you even have any problems left for it to solve?

**Jan Leike:**
Yeah. I think, ultimately, this is an empirical question. It's really difficult to know in which order which skills get unlocked when you scale up the models. There's a lot more work aimed at predicting emerging capabilities now and I'm really excited about that. I think it'll give us some chance of actually predicting what the next pre-trained model will be like. But I think we can make some high-level arguments. So for example, one thing that is pretty clear is once you have the model [so] good you can hand a lot of alignment research off to [it], wouldn't it then also be able to just improve its own capabilities? Or it can do a bunch of ML research, so it can run experiments on improving compute efficiency or something. And then, you could use that and pre-train a much more capable model shortly thereafter.

And I think the story sounds, on the surface, appealing. But I think in practice it will be actually a lot more complicated, because you're not doing big pre-trained runs every week. They usually take a few months, and so it would be a few months before you actually have that. In the meantime, you still get to use the system. I think the other thing is also: there's kind of an open question of how much low-hanging fruit there still is on actually having compute efficiency wins. And I think, ultimately, the argument here I would make is that, right now, the existing community of people who are trying really hard to get AI to go faster and be more capable is already quite large relative to the alignment community. And so, if you get to automate a lot of these tasks and both communities benefit equally, then I think actually alignment benefits a lot more because it's a smaller community, and so we don't have to do these tasks anymore.

**Daniel Filan:**
Sure. So, I took the plan to be something like: we're going to make this automated alignment researcher and then it's going to help us with alignment. And given that in order to make an automated alignment researcher, it needs quite a lot of pretty smart, pretty good capabilities, what problems does it need to solve that we wouldn't have needed to solve in order to get it?

**Jan Leike:**
I'm not sure I understand your question. Maybe I can answer the second part of the previous question that you asked.

**Daniel Filan:**
Sure.

**Jan Leike:**
Which is, what about long run goals? What about creativity? It seems like, to me, at least, that language models or AI in general has proven to be, on average, more creative than humans, I would say. If you look at, I don't know, the diffusion model images or if you sample from a pre-trained base model or something, there's a lot of really wild stuff in there and there's a lot of creativity that I think you would really struggle to get out of a single human or a small group of humans, just because the model has seen the whole range of everything humans have said or all the images on the internet. And so, they can sample actually from the whole distribution. Whereas, individual humans typically can't.

And then, in terms of long-run goals, I think this is actually not needed at all, because we can hand off pretty small well-scoped tasks to AI systems, that if they really nail those, it would actually be really useful, right? And that could be really small range things, like "here's the paper that we just wrote, please suggest some next steps or some new experiments to do". And if you imagine having a really A-star researcher that you can ask these questions, they don't have to pursue long-run goals, right? They only have to optimize over the next, I don't know, few thousand tokens. And if they do that super well, then you would get a lot of value out of them.

**Daniel Filan:**
I guess. That seems like it's in tension with this aim to automate 99.9% of alignment research, right? Because I would think that thinking "what things do we need in order to get an aligned AI?"... I would think that's a significant chunk of the difficulty of doing alignment research.

**Jan Leike:**
That's right. So what I wanted to say is the system's adding a lot of value by really excelling in these tasks, right? And then, what you do is you have a whole portfolio of these tasks. Some tasks will be like "write the code that implements these experiments", and then there's another task that's like "look at the results and tell me what you see, or suggest what to do next". And now, once you have done these tasks, you compose them using some general recipe, as people do with [Auto-GPT](https://github.com/Significant-Gravitas/Auto-GPT) or language model programs, where each of the tasks is small and self-contained, and so the system doesn't need to pursue long-run goals.

Or for example, [the recent work](https://arxiv.org/abs/2305.20050) that came out from OpenAI on using process-based feedback on math where instead of training on "did the system get the right solution?" and just doing RL on that, you train a reward model from human feedback on every step in the proof. And it turns out, that's actually much more effective because it gives the AI system a much more fine-grained way to learn and more detailed feedback. Now, is that going to be competitive with doing end-to-end RL on did you get the solution right in the long run? That is very unclear. But at the very least, you can use this kind of broken-down step-by-step setup, get the system to do a lot of really useful things that humans would've done, and then piece it together.

**Daniel Filan:**
Yeah. Although even the small tasks... so one of the small tasks you mentioned was "look at results and decide what to do next". I guess I would've thought that in order to do that you have to have the big picture in mind and think "what next project is most useful for my goal of solving superalignment in four years?", right?

**Jan Leike:**
That's right. But you wouldn't do this in the sense that you're trying to optimize and do credit assignment for four years. It's probably more like, well, you're just adding some broader goals and context into your prompt. But when you're actually doing reinforcement learning or reinforcement learning from human feedback to improve the system, then you don't have to wait until that research project concludes to decide whether or not it's good. It's just, you use the human as reward shaping; you're just like "does this look like a good direction that seems better than any directions I could have thought of?" or something. And so, I think the overall goal here is not to build the most capable automated alignment researcher that we possibly could with the tech that we have, [but] rather build something that is really, really useful that we can scale up a lot, and most importantly, that we trust is aligned enough to hand off these tasks to.

And so, if we introduce a fair amount of inefficiencies in these processes where we're essentially sandbagging the model capabilities a whole bunch by training it this way and we are like, "well, we're giving it these broken-down tasks and now it would execute those, but if we train it end-to-end, it would be more capable" or something. I don't think that matters as much. So this is typically what people call [the alignment tax](https://www.alignmentforum.org/tag/alignment-tax). And the alignment tax matters a lot if you're, let's say, competing in the market with other companies: I'm building a chatbot or something and my chatbot is more aligned but it seems a lot less capable. That would have a hard time competing in the market. But if you have an automated alignment researcher, the automated alignment researcher doesn't have to compete in the market, it just has to be useful to us. And so we can get away with paying a higher tax because we just don't have a replacement, or the real replacement is hiring more humans, which doesn't scale that well.

### What does it do? <a name="what-does-it-do"></a>

**Daniel Filan:**
Okay. I guess another way to ask the question I was going to ask previously is: what problems will you want this automated alignment researcher to solve?

**Jan Leike:**
I mean, ultimately it should solve the problem of "how do we align superintelligence?"

**Daniel Filan:**
Okay. So is the idea that we solve the problems of how to align something roughly human-level and then there's additional problems as it gets smarter and it just starts taking on those?

**Jan Leike:**
Yeah, basically. So I imagine that an actual solution to aligning superintelligence that we and lots of other people actually truly believe in will look quite different from what we do today. If you look at how ChatGPT is aligned today, it's a lot of [reinforcement learning from human feedback](https://en.wikipedia.org/wiki/Reinforcement_learning_from_human_feedback). And there's a widely-shared belief, that I share, that that just won't scale because it fundamentally assumes that humans really understand what the system is doing in detail. And if the system is doing a lot of alignment research where you're thinking of millions of virtual human equivalents or something, there's no way you'll be able to look at all of it and give detailed feedback. And it's a really difficult task and you'll miss lots of important bugs. But the kind of techniques we're looking at right now to scale this and align a roughly human-level alignment researcher that can do these difficult tasks but won't do it crazily differently than humans would, are steps or continuations of that, where scalable oversight, for example, is a natural continuation of reinforcement from human feedback.

**Daniel Filan:**
What is that?

**Jan Leike:**
Scalable oversight?

**Daniel Filan:**
Yeah.

**Jan Leike:**
So scalable oversight I would define as generally a portfolio of ideas and techniques that allow us to leverage AI to assist human evaluation on difficult tasks.

**Daniel Filan:**
Okay. So scalable oversight is an example of the thing that you could build off of reinforcement learning from human feedback, often called RLHF.

**Jan Leike:**
Yeah. And so typical examples of scalable oversight are [debate](https://arxiv.org/abs/1805.00899), [recursive reward modeling](https://arxiv.org/abs/1811.07871), [iterated distillation and amplification](https://ai-alignment.com/iterated-distillation-and-amplification-157debfd1616), [automated market making](https://www.alignmentforum.org/posts/YWwzccGbcHMJMpT45/ai-safety-via-market-making), and so on. And there's a bunch of ideas floating around. But what I actually wanted to say, to get back to your original question, is I think if we are actually aligning superintelligence, which is this system that is vastly smarter than humans, can think a lot faster and run at a much larger scale, that just introduces a whole lot of other problems, especially because it will be super general, can do lots of tasks, and then you have to figure out how to align it not just on the much narrower distribution of alignment research-y tasks, but everything else. And also, you want to have a much higher degree of confidence that you've actually succeeded than you would get with, let's say, a bunch of empirical evaluations.

And so I don't really know what that would look like and I don't think anyone does, but I think it would be really exciting if we can have some formal verification in there. Maybe we've figured out some kind of learning algorithm that has theoretical guarantees. I don't know what even would be possible here and theoretically feasible if you have a lot of cognitive labor that you can throw at the problem. But all of these things are very different from the kind of things that we would do right now or that we would do next. And I also don't think that a roughly human-level alignment researcher would start working at those problems right away. And instead what they would want to do is, we would want them to figure out how to better align the next iteration of itself that then can work on this problem with even more brain power and then make more progress and tackle a wider range of approaches. And so you kind of bootstrap your way up to eventually having a system that can do very different research that will then allow us to align superintelligence.

**Daniel Filan:**
Gotcha. So once you have these human-level AI alignment researchers running around, do you still have a job? Does OpenAI still have a human superalignment team?

**Jan Leike:**
Yeah, good question. I mean, I would be excited to be replaced by AI to be honest. But historically what typically happens is what we mentioned earlier: the AI assistant does 99% or 99.9% and then we just do the rest of the work. And I think also something I'm really bullish on is making sure that humans stay in the loop or stay in control over what AI systems are actually doing, even in the long run when we're long past really being able to understand everything they're doing. And so there'll be some humans that still have to try to understand what the high level is of what the AI is trying to do. So that wouldn't necessarily have to be the superalignment team that is at OpenAI now. It might also require a very different skillset than we have right now. But I think ultimately humans should always stay in the loop somehow.

### Recursive self-improvement <a name="rsi"></a>

**Daniel Filan:**
Okay, gotcha. So yeah, actually speaking of... I guess the loop? Wow, that's a bad segue, but I'm going with it. So one thing that OpenAI has mentioned in a few blog posts is, so firstly there's this idea that safety is actually pretty linked to capabilities. You need smart models to figure out the problems of alignment. Another thing is that there's this desire to avoid fast takeoff. And I think there's this quote from ['Planning for AGI and beyond'](https://openai.com/blog/planning-for-agi-and-beyond) that says "it's possible that AGI capable enough to accelerate its own progress could cause major changes to happen surprisingly quickly". And then it says "we think a slower takeoff is easier to make safe". So one thing I wonder is, if we make this really smart or human-level alignment researcher [so] that we then effectively 10x or a 100x or something the size of the alignment team, does that end up playing into this recursive self-improvement loop?

**Jan Leike:**
I mean, it really has to, right? You can't have a recursive self-improvement loop without also improving your alignment a lot. I mean, I personally think fast takeoff is reasonably likely and we should definitely be prepared for it to happen. And if it doesn't happen, I'm happy about that.

**Daniel Filan:**
How fast are we talking?

**Jan Leike:**
I mean, ultimately, I don't know. But you can draw some parallels to some other machine learning projects like AlphaGo or Dota or StarCraft where the system really improved a lot week over week. Yeah, there's obviously a lot of uncertainty over what exactly happens, but I think we should definitely plan for that possibility. And if that does happen, a really good way to try to keep up with it is to have your automated alignment researchers that can actually do thousands of years of equivalent work within every week. And there's just no way that that's going to happen with humans.

## How to make the AI AI alignment researcher <a name="making-the-alignment-researcher"></a>

**Daniel Filan:**
Okay. Now that we have a better sense of what we're looking at with a human-level automated alignment researcher, can you give a sense of what the plan is to make one?

**Jan Leike:**
Yeah, so I mean, you basically need two parts. One is you need a system that is smart enough to do it, and then the second part is you need to align it to actually do it. And I think they're not two separate parts, I think they're very intimately linked. And I'm personally not working on the first one, I think that will definitely happen and a lot of people are working hard at making it happen. And there's lots of different ways that could go, but you could just picture training a much larger model and eventually it will just be smart enough. And on the second part, that's the part I'm really interested in. And so the question is: now I have this really smart pre-trained model, how do you get it to actually do alignment research in the way that you would want it to? Or more importantly or more critically, how do you know whether you can trust it enough to do that?

Because if you're handing this kind of task off to AI, there's this opportunity that a smart AI system could just try to figure out how to present results and techniques to you in a way that if you then decided to implement them would actually empower the system itself or some other related system and give it more opportunity to have more power-seeking behavior in the future, or let's say self-exfiltrate, for example. And so what we need to do is, one, actually align it to do the task and leverage its capabilities to do it basically as well as it could. So that is what I would call, we need a bunch of alignment training methods. And then we also need to be able to distinguish a model that is actually aligned on the task and tries to do alignment research really well and actually help us and be honest and be truth-seeking, and a system that pretends to be that to us but actually wants to take over the world or (say) self-exfiltrate or do something else. And these are two different systems, one is basically the truly aligned one and one is the deceptively misaligned one.

And an important part of our job is figure out how to distinguish these two systems. And that will require a bunch of methods that I would call validation methods. So these are methods that we wouldn't really train on in a sense that the AI systems wouldn't be directly selected on passing the validation tests, and more they will give us some kind of independent signal of "which one of these two versions is it?" And so examples here are interpretability research, for example. If I can look inside the model and find some evidence that it's secretly plotting against us, that will fail the validation test. Another technique that could either be training or validation, depending on how it's set up, is this easy-to-hard generalization problem. Can you understand and improve the model's ability to generalize from easy problems that we can supervise to harder problems that we struggle to supervise? And if you can do that, now you can just supervise the model on easy parts and then you crosscheck on the difficult ones where you would worry that the model might secretly deceive you or try to do something else.

### Scalable oversight <a name="scalable-oversight"></a>

**Daniel Filan:**
Okay, cool. So yeah, there's a few things there. I guess I'd like to talk about the scalable oversight part first. How are we going to do this? Especially, as background, I think we live in a world where there's not a lot of consensus on what good alignment research is. So how are we going to get a training signal for what counts as good alignment research?

**Jan Leike:**
Yeah, exactly. And I think that the reason that there isn't a consensus is good evidence that the problem is actually hard. But also it tells us that, well, the field is still immature and we haven't gotten that much empirical evidence so far. But I think there's some really important property that alignment research has that we can leverage for scalable oversight, which is that I think it's fundamentally easier to evaluate alignment research than it is to do it. And [that] doesn't mean it's easy to evaluate, but it is much easier to find, for example, a paper that has a cool idea and does some cool experiments and gets some good results and you read that and you're like, "Oh, actually this is a really good idea, I think this is good," than it would be to produce that work in the first place.

And so leveraging this principle that evaluation is easier than generation is the core of a lot of scalable oversight ideas. And so for example, if you're thinking about [recursive reward modeling](https://arxiv.org/abs/1811.07871), where the basic idea is you have some kind of AI system that you use as an assistant to help you evaluate some other AI system. So now, because evaluation is easier than generation, the task that the assistant has to do is a simpler task, especially because you are working together with it. So actually aligning the assistant on the simpler task of the evaluation assistant, where let's say the claim is that that would be easier, and if you succeed at that task, then now you use your human/assistant conjunction to supervise a new system on an even harder task. And so if you keep doing that, you're unlocking a broader and broader scope of tasks that you can effectively supervise your AI systems on.

**Daniel Filan:**
So somehow the idea is that we're iteratively adding more and more AI knowledge basically to the evaluation portion of AI alignment research. And by doing it this iterative way, the idea is that we can consistently give it a good training signal?

**Jan Leike:**
Yeah.

**Daniel Filan:**
Okay.

**Jan Leike:**
Let me make this a little bit more concrete. So for example, [RLHF](https://arxiv.org/abs/1706.03741) is the simplest one where you don't use any assistant. Your AI system does something, a human looks at it and says whether it's good, and that's your training signal.

And now, the next step up from that is, basically, you train the simplest assistant model, which is just a critique model. You have a separate language model that looks at what the first AI system did, and then it writes a critique. So let's say the first AI system writes some piece of code, you look at the code: humans are notoriously bad at finding bugs in code, that's why there's so much buggy code out there in the world, but now, if you have a critique system that writes a critique and it points out the bug, it's so much easier for the human to say, "Oh yeah, this is totally a bug. We should fix that".

And it's also not a super crisp task, necessarily, because usually, the code is written for some natural language specification. What is actually meant by that is somewhat vague, and actually, whether something is a bug, can be ambiguous. But the important point here is that, because you're using the critique model as an assistant, and if [you have] the critique model, now, you can supervise a wider range of tasks, because essentially, you could find all the problems in code that your AI system would find or would've written a critique, which might not be exactly the same as all the code where it knows there's a bug in there, but you have expanded the range of things that you can supervise.

The really nice thing is that, I think, actually, there's a bunch of ways in which we can actually empirically measure how well this works. There's one way, which is kind of what we did in [the critiques paper](https://arxiv.org/abs/2206.05802) that we published last year, where you basically do randomized controlled trials with targeted perturbations. So you take, let's say, a coding task. In the paper, we didn't do a coding task, but the task doesn't matter here. And then, you have a response. The response does not even have to be perfect, but let's just assume it is. What you do is you introduce some subtle bug in there, some subtle flaw that would make it a bad response, but you want it to be hard to find for humans.

**Daniel Filan:**
How do you introduce this bug?

**Jan Leike:**
The simplest thing is just you ask humans to do it. But ultimately, you want to do it with the model, and I'll get back to that. Now, you have this paired response. You have the original response, and you have the response that you know is worse because you made it worse. And now, you draw one of them at random and you show it to a human or a human assisted with AI or whatever your scalable oversight method is. And then, you see how likely are they to find it? Or you just ask them, "Is there a bug here?" And now, you can measure, if you have a better way to assist them, you can see how often they find the bug.

But you can do even more, which is, you can directly compare this to how much does the model know that there's a problem here by training it, essentially, to be a discriminator between the good version and the flawed version. And if you look at the accuracy for that, that's the discriminator performance. And then you check that with... if you ask the model or the RLHF version of the model to write a critique of the code, how often does it actually write about the flaw? Now, you get this critique accuracy equivalent, and now you can compare these two numbers. That's what we call the discriminator-critique gap.

And if you have a large discriminator-critique gap, that means that there would be a lot of problems that the model can smell or knows about or whatever metaphor you want to use, but it's not telling us because if we asked it to write a critique, it doesn't say it. And a large discriminator-critique gap would be very worrying because it means that your scalable oversight is somehow very flawed and there's a lot of things that slip [under] your radar, or the assistant is, in a sense, not very aligned because it's not trying very hard to help you.

And if the discriminator-critique gap is pretty small, that is somewhat reassuring because it means, if the model knew there was a problem there, it would tell you. So in particular, if a similar sized model or the same base model was trying to plant a problem, or it was trying to, let's say, put Trojans in your code that is hard for you to find, but if you did run the code, the Trojans might fire, and then it could take over or do something else. Then, presumably, it would know that these Trojans could be there because it planted [them] itself, right? So if we have this other copy of the model, then, if we measure a small discriminator-critique gap, we'd have more confidence that this can't be the case because the other copy of the model would presumably then write a critique.

**Daniel Filan:**
Yeah.

**Jan Leike:**
And this kind of hinges on the assumption that discriminator training actually can take out of the model this kind of thing. And I think there's good reason to believe that because, if you're fine-tuning for a discriminator, essentially, you have a way to access all of the activations that are everywhere in the model because you can fine-tune all of the weights simultaneously. So if the knowledge is somewhere in the model, in its model's activations, then a discriminator fine-tuning on, if you do it right and you have enough data, it should be able to extract it.

And in fact, you can give some back-of-the-envelope calculations of how much data you would need. And if you want to identify one neuron that... This is not how it works, but hypothetically, if there was one single neuron that knew if the code is bad, you would need the log of the number of total neurons data points to narrow it down and find it, which is not that many.

**Daniel Filan:**
Yeah. I guess this actually gets to a question I have about the whole scheme. The reason we didn't like RLHF is that there was this concern that it wasn't going to distinguish between things that were right and things that sounded good to the human, because the human was bad at critiquing.

And with this recursive critique thing with training a discriminator and then a critiquer, I wonder if we still have that fixed point. So one world is, we have a thing, we train it to write actually-good critiques, and then, we join ourselves with it to train a thing that writes even better critiques. But I worry, maybe there's another fixed point where we train a thing and we accidentally train it to write pseudo-critiques that sound good to us and we join with it, and then, we're even better at looking for critiques that sound good to us. And then, we trained this discriminator to discriminate between things that don't have problems and things that seem like problems to us. I'm wondering, yeah, how worried should we be about things that seem good to us rather than things that are actually problems being a fixed point here?

**Jan Leike:**
Yeah, I think that's a really important, valid concern to have. And in a way, that's one of the key things we need to figure out about scalable oversight. This is the reason why I'm excited to actually implement these techniques and try them out empirically because I think we will just be able to measure how much this effect shows up in practice.

And one concrete, simple way we can measure it is, we actually just deliberately train a deceptively-aligned model, and then, we see if the scalable oversight flags it, or how hard is it to... or how much effort do we have to put into making the model deceptively aligned such that it would pass our scalable oversight.

**Daniel Filan:**
Yeah, I guess there's also these [sandwiching](https://www.alignmentforum.org/posts/PZtsoaoSLpKjjbMqM/the-case-for-aligning-narrowly-superhuman-models#Potential_near_future_projects___sandwiching_) [experiments](https://arxiv.org/pdf/2211.03540.pdf) where you have an AI that's trying to answer legal questions. There's some people who don't know much law, and then, you get some people who are really good at law and hopefully, they can check if the AI has fooled the people.

**Jan Leike:**
Yeah. I think these experiments are interesting, and there's some interesting work on this already. I think there are some fundamental problems to this setup.

One is, well, you really have to trust your experts, and the experts are going to be wrong sometimes. And then you have to have sufficient domain overlap. We did a little bit of preliminary experiments with this where we had non-coders who were supposed to use assistance to figure out problems in code, and they didn't even know how to ask the right questions. So you have to have some overlap.

And then, the other problem is also that, I think the task that I'm fundamentally really interested in, we just don't have ground truth on. And even if we, let's say... alignment research, we're doing alignment research now, we can be like, "Oh, we'll just get some alignment researchers to label some tasks." And then I'd be like, "Well, what if they are wrong? We don't agree about so many things. That seems hard." And then, also, there's not that many and their time is very expensive, so that would be extremely expensive data.

So in general, I would want to have an evaluation method that doesn't rely on the assumption that we have ground truth. And this is why I'm excited about the prospect of using these randomized controlled trials with targeted perturbations or measuring the discriminator-critique gap. Because you can do this even if you don't have any ground truth, and the task can be arbitrarily hard.

**Daniel Filan:**
Yeah. Although, even in those - measuring the discriminator-critique gap, it's still got to be the case that you have an actual discriminator versus a discriminator of things that seem like problems versus things that don't seem like problems, right?

**Jan Leike:**
You mean you can have AI systems introduce the flaws, right? And in a way, that is also much better than humans doing it because it'll be a more honest distribution for actual[ly] what AI systems would do. And if you're fine-tuning your discriminator on that data, you actually do have ground truth if you can trust that the flawed version is actually worse.

**Daniel Filan:**
And I guess it's-

**Jan Leike:**
And the way you could trust that is, well, you look at the reason why it's worse and you can verify that reason, which is a lot easier.

**Daniel Filan:**
Yeah, I guess there's some hope that, even if the AI can make you think things are good that aren't actually good, maybe if the AI makes you think something's bad, it is actually bad? Or that it's degraded the performance, that if the AI makes you think it's degraded performance, maybe it's easier to check that it's degraded performance?

**Jan Leike:**
Yeah, I see your point. I probably shouldn't actually use the word ground truth in that setting because it is not truly ground truth, just as nothing really is truly ground truth, but there's a variety of things that you can do to make you very confident in that in a way that doesn't necessarily make the task of finding the problem easier.

### Searching for bad behaviors and internals <a name="bad-behaviors-internals"></a>

**Daniel Filan:**
Okay. Gotcha. I think I want to talk a little bit about the next stage of the plan. Searching for bad behavior and bad internals, I think, was how it was put in [the superalignment post](https://openai.com/blog/introducing-superalignment). What open problems do you see here that you'd want the superalignment team to be able to solve?

**Jan Leike:**
Yeah. The obvious candidate here is interpretability, right? In a way, interpretability is really hard. And I think, right now, we don't really have any slam dunk results on language models where we could say interpretability has really given us a lot of insight or added a lot of value. And this is because we're still pretty early in understanding the models and what's going on inside.

**Daniel Filan:**
So there are some results... people do some interpretability things to language models. There's this [induction heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html) work, there's this [indirect object identification](https://arxiv.org/abs/2211.00593) work for a circuit that does at least a certain type of indirect object identification. I'm wondering, what would you need beyond those to get what you would consider a slam dunk?

**Jan Leike:**
Yeah, sorry, I don't mean to diminish existing work here, and I think--

**Daniel Filan:**
You can score in basketball without getting a slam dunk, right?

**Jan Leike:**
Yeah. And I think very cool things have happened, but I think something that I would consider a really cool result is, you use your interpretability techniques on a language model reward model like a GPT-4 size or whatever your largest model is, and then you say something about the reward model that we didn't know before. And that would be really cool because the reward model is what provides the training signal for a lot of the RLHF training, so understanding it better is super valuable. And if you can flag or find problems of behavior that it incentivizes that you wouldn't want, that would be really cool.

I think that is doable. And I think, importantly... In that sense, I think interpretability is neither necessary, nor sufficient. I think there is a good chance that we could solve alignment purely behaviorally without actually understanding the models internally. And I think, also, it's not sufficient where: if you solved interpretability, I don't really have a good story of how that would solve superintelligence alignment, but I also think that any amount of non-trivial insight we can gain from interpretability will be super useful or could potentially be super useful because it gives us an avenue of attack.

And if you think about it, I think it's actually crazy not to try to do interpretability because, in a way, you have this artificial brain and we have the actually perfect brain scanner, right? We can zoom in completely and fully, accurately measure the activation of every single neuron on each forward pass, so in arbitrary, discrete timestamps. That's the maximal resolution that you could possibly want to get. And we can also make arbitrary interventions. We can perturb any value in the model arbitrarily how we want. That gives us so much scope and opportunity to really do experiments and look at what's happening. That would be crazy not to leverage.

But at the same time, the reason why it's very hard is because the model is learning how to do computations tailored to efficiency, and it's not regularized to be human-understandable, or there's no reason to believe that individual neurons should correspond to concepts or anything near what humans think they are or should be or what are familiar to us. And in fact, empirically, neural networks represent many different concepts with single neurons and every concept is distributed across different neurons. So the neurons aren't really what matters here.

But one path... I think there's two things on interpretability that I'm super excited about. One is the causal version of it. You want to not only look at a neuron as you're passing data through the model and say, "Oh, this fires when we have stories about Canada," or something. This is one of the things that we found in the interpretability paper. We had this 'Canada' neuron that would fire at Canada-related concepts. But it's only correlationally. You observe a correlation between Canada-related concepts and that neuron firing, but it's not a causal relationship. In order to check that it's a causal relationship, you have to deliberately write text that has Canada-related concepts, see if they all fire, but also put in other related concepts that maybe sound like they could have something to with Canada or don't have anything to do with Canada, but are similar, and then check that the neuron doesn't fire. Or you take a text, and then edit it and check that the neuron turns off, and things like that.

**Daniel Filan:**
Yeah. I guess this is reminiscent of this, I think it was called [the interpretability illusions paper](https://arxiv.org/abs/2104.07143), where they noticed, on Wikipedia, you can have neurons that fire on one specific thing, but then, on other data sets, that was just an illusion, it fires on a bunch of other stuff.

**Jan Leike:**
Yeah. And I think the other thing that is really exciting is, the work that we've started last year where we released [a paper](https://openaipublic.blob.core.windows.net/neuron-explainer/paper/index.html) earlier this year on automated interpretability. And here, the idea is, basically, what you would want is, you would want to have a technique that both can work at the level of detail of individual neurons so that you can really make sure you don't miss any of the details, but also can work at the scale of the entire model. Because at the end of the day, everything works together and everything is highly connected in the model, so you need both. And so far, techniques have mostly worked on one or the other. There has been attempts at... there's been work on automated interpretability before our paper, so it's not like we were the first ones. But I think, in general, if you can have some really detail-oriented interpretability work, some kind of mechanistic interpretability approach that really tries to understand individual circuits or computation units inside the model, the way to then scale that to the entire model is, you need automation, right?

**Daniel Filan:**
Yeah.

**Jan Leike:**
But you can do that. Once you've figured out how to do it on the detail, then, you just record what you're doing. I'm simplifying a bit too much here, but I think that's the general scope that I would be excited about, also because it really fits with our automated alignment goals. We want to have our automated alignment or interpretability researcher really look in detail into the model, understand what's going on. Then, we sift through the whole... everything, or we find ways to aggregate it.

So for [the paper](https://openaipublic.blob.core.windows.net/neuron-explainer/paper/index.html), we had a dump of explanations. The paper does, it writes natural language explanation for individual neurons, which is not quite the right thing to interpret, as I mentioned previously, but it gives you a simple example of what we could do here. The way it works is you just show a bunch of activation patterns to GPT-4 and you have GPT-4 write a proposed explanation.

And in general, these explanations aren't very good, also, because the task is so hard and most neurons don't do a very clearly human-understandable thing. But we can just run this on the scale of every neuron in GPT-2, and then just dump all the explanations, and then try to find what is interesting patterns. And you can look at scaling trends and be like, "How does automating scoring of these explanations scale as models get larger?" Or, "What if we add more compute or we make the model that's doing the interpretation bigger? How does the quality of explanations change?"

And the really cool thing is that we can just measure this automatically, again, using language models. And it's not a perfect metric and there's a bunch of problems with it, but it gives you a proxy for "would a human think this explanation is good?" And then, you use this proxy at scale and just run it on a ton of neurons.

**Daniel Filan:**
Yeah. So, if you think about the necessary interpretability work, how much of it do you think is just figuring out a better fundamental unit of interpretation versus figuring out how to scale what's happening? Or other things you might...

**Jan Leike:**
I think it needs to be both. I think it's very reasonable to believe that the former is much harder, but I think the scaling part is needed for it to succeed.

### Deliberately training misaligned models <a name="deliberately-training-misaligned-models"></a>

**Daniel Filan:**
Gotcha. Yeah, so, I guess part 3 of the plan was something like deliberately training misaligned models and seeing if the pipeline could detect those. Are you planning to weed them out or actively fix them?

**Jan Leike:**
The goal here would not be to fix a deliberately trained misaligned model.

**Daniel Filan:**
Okay. Just to detect?

**Jan Leike:**
Yeah. Fundamentally, one core aspect of what we need to do here is, we need to be able to distinguish between the actual aligned alignment researcher that does what we want and that truly wants to help us make progress on alignment and the one that pretends to do that, but secretly wants to take over and self-exfiltrate. And one way we can figure out more about how good we are at distinguishing these two is, we make the second one and see if it looks like the first one to us. Obviously, you have to be very careful with doing that because you're kind of making the scary thing that we're all trying to avoid, right?

**Daniel Filan:**
Yeah. I actually wanted to ask about that. I think at the start of [the post](https://openai.com/blog/introducing-superalignment), there's one sentence about how great superintelligence could be, and then, it says, I think, that it "could lead to the disempowerment of humanity or even human extinction." What are we talking about with misaligned models? How do we know they're not going to do those awful things?

**Jan Leike:**
Yeah. Let me put it this way. There's a lot of things that are needed to make the future go well. We need to have the right governance structures so that we can prevent misuse of powerful AI systems, and we have to have precautions so we don't run into some kind of emergent runaway effects that are still bad for humans where you have aligned AI, but AI is aligned with corporations who are not aligned with humans or something, and then, the whole system goes off the rails, but it looks good in all the economic metrics or something.

Also, we need to solve technical alignment so we can actually align AI systems. The risks that superalignment is looking at is just the last part. We want to figure out the technical problem of how to align one AI system to one set of human values. And there's a separate question of what should those values be? And how do we design a process to import them from society?

And I think these are really important questions and we really have to answer them too for the future to go well. And I think, for the future to go well, the bar is not "AI doesn't kill literally everyone", but I want there to be lots and lots of human flourishing and I want to have a future that everyone is really excited to live in. And the more powerful AI gets, the higher the stakes will be, right? Because there'll be more risk from misuse and if systems are misaligned, things can go wrong a lot more.

In particular, the threat models that I think we should be particularly worried about from a misalignment perspective are systematic lies and deception and self-exfiltration.

**Daniel Filan:**
Okay. When you say, when you're deliberately training misaligned models, is the idea that you would train them to lie and potentially self-exfiltrate, but at a low enough capability level that it's fine or something?

**Jan Leike:**
You wouldn't want to train the model, actually, to self-exfiltrate because you know, what if it actually gets good at that, right? You would want to pick some other proxy task that is a lot more mundane, but that still tells you that... It makes it easy to evaluate whether or not the model is succeeding at it.

And then, the core difficulty and the core goal is really figuring out how hard it would be for a system to have the properties that it would need to still pass the evals. If you have a system that is lying to you or deceiving you, that's really ineffective if it only does so sometimes, and then, if you ask it, "Oh, did you lie about this?", and it says, "Yes," it wouldn't be a very coherent, believable liar. The scary systems are the really coherent and believable liars that lie super consistently where it would just be really hard to interrogate in a way that points out its lies or any sort of inconsistencies in what it's saying.

And I think, actually, to be this kind of consistent liar is not that easy. And our whole goal with what we're trying to do with scalable oversight and interpretability and generalization is that we want to make it really, really hard for the model to be this consistent liar. But if we just do that, we don't know how hard it actually is.

And one way we can find out is, we train the model to be a really consistent liar, and then, we see how hard it is. How much do we have to cheat? How much do we have to give the model hints or handhold it along the way? But you don't actually want to train a dangerous system that, if you did run it, it would successfully self-exfiltrate, because then, you've caused the very thing that you're trying to prevent, and that would be really bad.

**Daniel Filan:**
Yeah. So when you say that, are you distinguishing lying from other kinds of danger, such that we could train it on lying, but we're not going to train it on murder, for instance?

**Jan Leike:**
Yeah. Let me try to give you an example. Let's say you're giving it some fake goal. Maybe it is to persuade a human to say a certain catchphrase, or maybe it is to run a certain Python command that, in fact, doesn't do anything but it looks interesting or something.

**Daniel Filan:**
Yeah, yeah.

**Jan Leike:**
And you can very well... it's so easy to measure whether the system succeeds and you can set up the experiment so that it's hard to do that a priori. And the goal here is not to have the system stress test, let's say, our cybersecurity or something, although we should also do that separately. I think that's another effort.

Here, the goal is really just, how close are the systems that we're currently training, or that we currently have, to a system like that that would be deceptively aligned, that is a coherent liar, that any chance it gets where it thinks humans aren't looking, will run specific code or things like that. That's exactly what we are looking for. And a priori, it's really hard to get confidence of how close we are, unless we just push it a little bit in that direction and see if that happens.

**Daniel Filan:**
So the thing I'm imagining now is, you're training it to do something kind of arbitrary other than the thing it's supposed to be trained for such that it has to do deceptive stuff, it has to do the kinds of things it would need to do in order to cause damage, but at the end of the day, it's just going to put a sticky note on someone's forehead or something.

**Jan Leike:**
Exactly, exactly. It's like [you're hiring someone to test your physical security](https://en.wikipedia.org/wiki/Penetration_test), and all I have to do is get in the building and give you a handshake and you're like, "Yeah, it seems like you succeeded at it," or something. Or you're like, "Okay, can you steal this fake object for me because I want to figure out how good our security is," things like that that you can do that doesn't have actual consequences, but still tells you a lot about your security. I'm excited to do the same with alignment where you stress test the alignment systems you have by training something that's particularly targeted to break and circumvent them, but if it succeeds at doing that, it doesn't cause actual harm. It's very benign.

## Four year deadline <a name="four-year-deadline"></a>

**Daniel Filan:**
Okay. Cool. I'd like to ask an overall question. So in [the superalignment blog post](https://openai.com/blog/introducing-superalignment), in big letters, it says, "Our goal is to solve the core technical challenges of superintelligence alignment in the next four years." What do you mean by the core technical challenges of superintelligence alignment?

**Jan Leike:**
Yeah. This would be the general technical measures, how to align a superintelligence with a set of human values. And the kind of superintelligence we're picturing here is a system that is vastly smarter than humans, that could possibly execute much faster. You can run it a lot in parallel, it could collaborate with many, many copies of itself, so a truly vastly powerful system.

And we want to do this in four years. And the reason why we picked the four years was, we want to have a really ambitious goal, something that feels like we can actually achieve it, and also something that we can still realistically use, even if AI progress actually happens really fast and the technology actually improves a lot over the next few years, so that we still have something that we can deliver.

**Daniel Filan:**
Gotcha. And just to be clear, it sounds like this isn't just building the human-level automated AI alignment researcher. It's also using that to just get the technical problems of aligning something much smarter than us.

**Jan Leike:**
That's right. The roughly human-level automated alignment researcher is this instrumental goal that we are pursuing in order to figure out how to align superintelligence because we don't yet know how to do that.

**Daniel Filan:**
Gotcha. So if, four years from now, you want to have solved these core technical challenges, where do you want to be in two years that would represent being on par to meet that goal?

**Jan Leike:**
Yeah. If you want to work back from the four years, I think, basically, probably, in three years you would want to be mostly done with your automated alignment researcher, assuming the capabilities are there. If they're not there, then, our project might take longer, but for the best reasons.

And then, the year before that, we already want to... so this would be in two years, we'd want to have a good sense for what are actually the techniques that we could use to align the automated alignment researcher. Do we have the portfolio of techniques that, if we did apply them, we would actually feel confident that we have a system that we can trust and we can use a lot, and then we can hand off a lot of work to? And then, basically, at that point, we would have wanted to have broken down the problem enough so that it feels like now, the overwhelming amount of work is just engineering, which, in that sense, would leave us roughly two years to figure out the research problems attached to it.

Now, this is kind of a timeline for this goal that we set ourselves with four years, and obviously, there's really important interactions here with how AI capability progress goes. Because if progress slows down a lot, then we might just not have a model that's good at any of the really useful alignment research tasks. We've tried a whole bunch to do this with GPT-4, and GPT-4 was just not very useful. It's just not smart enough of a model. That's the short version of it, but I'm happy to elaborate. And so if four years from now we're just like, "Well, we didn't have a model that was good enough", then also that means that we have more time to actually solve the problem because the problem isn't as urgent. And on the other hand, it could also be that AI progress is a lot faster and we are like, "Well actually we don't think we'll have four years because superintelligence might arrive sooner." That could also be possible. And then we have to adapt our plan accordingly. And so the four years was chosen as a timeframe that we can actually probably realistically plan for, but also where we have enough urgency to solve the problem quickly.

### What if it takes longer? <a name="what-if-it-takes-longer"></a>

**Daniel Filan:**
Yeah. So a question I think a few people have wondered about, and I'm curious about as well is: so suppose that on the AI capabilities research front, it goes roughly as expected. Four years from now, you have all the capabilities to have something that could be a good automated alignment researcher, but interpretability turned out harder than we thought, or scalable oversight turned out harder than we thought, and you guys haven't made the mark. What happens then?

**Jan Leike:**
Well, we have to tell the public that we haven't achieved our goal, right? So, we are accountable to that goal. But also what happens if we don't make the goal depends a lot on what does the general state of the world look like. Can we get ourselves more time somehow or was our general approach misguided and we should pivot or...? A lot of things can happen.

**Daniel Filan:**
But basically: let people know and then figure out what the good next thing to do is and do it, roughly?

**Jan Leike:**
It's a pretty high-level answer.

**Daniel Filan:**
Yeah.

**Jan Leike:**
But I think there's something more here, which is: at least to me, it feels like the alignment problem is actually very tractable. I think there's a lot of good ideas out there that we just have to try rigorously and measure results on and we will actually learn and be able to improve a lot. And I've significantly increased my optimism over the last two years that this is a very practical problem that we can just do. And I think even if I turn out to be wrong and it did turn out to be much harder than we thought, I think that would still be really useful and be evidence about the problem, and in particular I think right now there's a lot of disagreement over how hard the problem actually is. And maybe more importantly, I think there is... One of the really important things we need to know, and be able to measure, is how aligned are our systems actually in practice?

And I think one of the things I worry about the most is not that our systems aren't aligned enough, but that we don't actually really know how aligned they are. And then experts will reasonably disagree about it because if everyone agrees the system isn't aligned enough to deploy, it won't get deployed. I think that's a pretty easy case, and the kind of scary scenario is more like you have this capable system and you're like, "Well, it's probably fine. It's probably aligned, but we are not quite sure". And there's a few experts who are still quite worried and then you're like, "Well, we could deploy it now, but let's not". And at the same time there's this strong commercial pressure where you're like, "Well, if we deploy the system it would make a billion dollars per week" or something crazy. And you're like, "Okay, there's still--"

**Daniel Filan:**
Should I take a billion dollars per week as OpenAI's official projection?

**Jan Leike:**
*laughs*

**Daniel Filan:**
Okay, sorry. Anyway, so there might be pressure to deploy something.

**Jan Leike:**
Exactly. Because you're like, "Well, if we deploy it later, it'll cost us effectively a billion dollars". And then you're like, "Okay, experts are still worried, so let's make the call and hold off from deployment for a week". Then a week goes by, another week goes by and people are like, "Well, what now?" And then alignment experts are like, "Well, we looked at some things but they were inconclusive, so we're still worried and we want to delay it bit longer". And I think this is the kind of scenario that I think is really worrying because you have this mounting commercial pressure on the one hand and you're pretty sure, but not quite sure. And so that's a scenario I would really love to avoid. And the straightforward way you avoid it is you just get really good at measuring how aligned the systems actually are.

**Daniel Filan:**
Gotcha.

**Jan Leike:**
And that's where a broader portfolio of techniques is really useful.

**Daniel Filan:**
Yeah, I guess it's almost even worse than that in that if you're in a situation where you could be using your AI for alignment research, then errors of not deploying are actually costly in terms of safety, potentially, it seems like.

**Jan Leike:**
That's right.

## The superalignment team and... <a name="superalignment-team-and"></a>

### ... governance <a name="governance"></a>

**Daniel Filan:**
So I actually wanted to pick up on this thing you mentioned about just getting a bunch of good measures. So a thing that I think [a few](https://openai.com/blog/governance-of-superintelligence) OpenAI [blog posts](https://openai.com/blog/planning-for-agi-and-beyond) have talked about is this idea of audits of AI systems and, I don't know, trying to figure out "are they going to be safe?" To what extent do you expect the superalignment team to work on things that would be useful for audits?

**Jan Leike:**
I mean, I think I'm somewhat hopeful that some of the techniques that we'll produce might be good for audits if we do our job well. For example, if we manage to make some progress on interpretability, then an auditor could use whatever techniques we come up with as part of their audit. Or I think making some kind of scalable oversight part of an audit is a really natural thing. But I think also to some extent the superalignment team is not ideally positioned to do an audit because we are not independent of OpenAI.

And I think an audit truly needs to be independent of the lab that is being audited. And so you want to have... and this is what makes me really excited about having independent auditors, because you want someone else to double-check what you're doing. And I think more generally, our main responsibility is not to convince ourselves that the system we're building is aligned and safe, because it's so easy to convince yourself of all kinds of things that you're incentivized to be convinced of, but really we have to convince the scientific community or the safety community that the system we are building is actually safe to run.

And that requires not only the research that leads to the techniques that we'll be using and the empirical evidence that the system is as aligned as we think it is that we can then show to others, but also independent assessment of all of the above.

**Daniel Filan:**
So would it be right to say, even just broadly about governance efforts, that you maybe think that ideally this team would produce techniques that are relevant to that effort, but independently people need to do that and you're just going to focus on solving technical alignment?

**Jan Leike:**
Yep. So this is another way you can put it right? We really want to focus on solving the problem and we want to make sure the problem is solved and that we can actually implement the solution on the cutting edge systems, but we still need independent assessment of "did we succeed at that?" or "how dangerous are the models' capabilities?" and those kinds of audits. But to some extent we have to evaluate the alignment too. That's the problem that we have to face, but specifically what we want to do is solve the problem.

### ... other OpenAI teams <a name="other-openai-teams"></a>

**Daniel Filan:**
Yep. Makes sense. I guess branching off from there, how do you see your team as relating to - OpenAI, I guess it has an alignment team, or at least it had an alignment team - is that still existing by the way?

**Jan Leike:**
So the alignment team, as it existed last year, had two parts, and one of them was called 'practical alignment', one of them was called 'scalable alignment'. Practical alignment's mission was roughly aligning OpenAI's most capable models. And so that team focused a lot on aligning GPT-4. And then the scalable alignment team's goal was figuring out alignment problems that we don't have yet. And so what's happened with the ChatGPT launch and all the success is it became a big product and there was a lot of work needed to improve RLHF and improve the models, make it into a really nice product. And the alignment team was just never... That was never the place to do it.

And so what we used to call practical alignment work now is done at lots of other teams at OpenAI and has become essentially a really large project that involves probably more than a hundred people or even hundreds of people. And then what used to be scalable alignment is now basically what the superalignment team does. The reason why we chose this name superalignment is we wanted to really emphasize that we are trying to align superintelligence. We are doing research on a problem that we don't have yet and we are doing the forward-looking work that we think will be needed in the future. Not because we wanted to say that the other work wouldn't matter or something. I think it is really important, but because that is our focus now.

**Daniel Filan:**
Gotcha. Yeah, that's useful to know; or I guess I'm not going to make use of that knowledge, but that's interesting to know. I guess I'm curious, how do you see the superalignment team as relating to other things at OpenAI? Like efforts to make ChatGPT nicer, minimize, I don't know, slurs it says or something, like the governance team, just various other groups at OpenAI.

**Jan Leike:**
Yeah, I mean part of the reason for being at OpenAI is also because it's much easier to work with these other teams more closely and realistically we need a feedback signal of whether what we are doing is actually useful and helpful. So for example, you could say, "Well, we are not trying to solve today's problems, but if we are doing our job well then what we are building should be useful for aligning, let's say, GPT-5". And so that would be the feedback mechanism: can we help make alignment of GPT-5 better with our techniques? And then in terms of governance, I mean there's a lot of things that you could mean by AI governance, and one thing the governance team at OpenAI is working on is how do we evaluate the model's dangerous capabilities? And that's very related to our question of "how can we stress test our alignment assistants? What if we trained deceptively-aligned models?" and doing these kinds of evaluations.

### ... other labs <a name="other-labs"></a>

**Daniel Filan:**
Gotcha. Yes. Actually, speaking of why you're at OpenAI and relating to stuff at OpenAI, as I guess you're aware there are other really good AI research labs out there, that are also working on things sort of related to alignment of superintelligence. I'm wondering how do you think your plan compares to that of things you see other people doing?

**Jan Leike:**
Yeah, I think there's a lot of related work going on, at DeepMind and Anthropic specifically, but also various other places. And to some extent we're all trying to solve the same problem. And so it's kind of natural that we end up working on similar things. There's other work on interpretability and there's other work on scalable oversight. And I think to some extent that is... you're running the risk of duplicating a bunch of work and maybe it'd be good if we all try to coordinate better or collaborate more. But then on the other hand, it also avoids groupthink, because if every lab tries to figure out how to solve these problems for themselves, there's a natural tendency that humans are more skeptical of what the other lab produces. But there's also a flip side where you can get these 'not invented here' effects where people just don't want to use the techniques that were invented somewhere else and you believe they're bad or you have a bias towards believing they're bad.

And so I wouldn't say that we are in a good equilibrium right now, and I think there's a case to be made that maybe all the alignment people should be in one place and work together somehow, but this is the world that we are in because essentially the cutting edge AI labs are incentivized to invest in alignment. And I think also this is something that's become super clear with the success of RLHF, where it makes models a lot more commercially valuable and thus it makes it more attractive to invest in the kind of research that produces these kinds of techniques. And so if the AI labs are the main funders of this research, then it's natural that that's where it happens.

**Daniel Filan:**
Sure. I guess in terms of research agendas or ways you're setting forward, do you see anything sort of distinctive or unique about the OpenAI superalignment team approach?

**Jan Leike:**
Yeah, so what we really focus on is trying to figure out how to align this automated alignment researcher. So we are not trying to figure out how to align on any kind of task. And we are less worried about [alignment taxes](https://www.alignmentforum.org/tag/alignment-tax) as a result of that, at least on that particular question. And I don't think other labs have emphasized that goal or direction in that way. I think also, I don't know how much detail you want to go in here, but one thing that we are very bullish on is just trying all of the scalable alignment techniques and just seeing what works best and trying to find ways to empirically compare them. And I think other labs have specific scalable oversight techniques that they're very bullish on and they try to get to work. And then also I think for interpretability specifically, we are taking this automated interpretability approach that I think other labs haven't really emphasized as much, and we are really trying to lean heavily in there.

I think another thing that we really like to do is leverage compute to advance alignment, or that's one of our main bets that we want to make. And so in particular, that could mean on scalable oversight, we really want to figure out how can we spend more compute to make a better oversight signal? What are the opportunities there? And there's some obvious things you could do, but you do the best of them on your critique model, now you're spending more compute, but you're also getting better critiques. But really the question then is what other things can we do? How can we spend more compute to make the oversight signal stronger? Or, automated interpretability is a really easy way where you can just spend a lot of compute to make progress on the problem. I think the way we would do it now isn't quite the right way to do it, but I think automated interpretability in general, if we can make it work, has this property and I think that's why it's exciting.

And automating alignment research, obviously; if you manage to do that, then you can just throw more compute at it and you'll get more alignment out. very roughly speaking. But because we've kind of come to this conclusion that really what we want to do is turn compute into alignment, we come to the point where it's like, "okay, now we need a lot of compute"; and this is the reason why OpenAI have made this commitment of 20% of the compute secured to date, towards alignment because basically it tells us, "Yes, there will be compute to do this" and if we actually figure out this automated alignment researcher, and it turns out we have to run it more, we'll be able to use more compute to run it. But it means that the strategy of betting on turning compute into alignment can be successful and is supported by OpenAI.

**Daniel Filan:**
Gotcha. Yeah, I guess thinking about that, I think that one difference I see in the OpenAI alignment team is it seems like OpenAI has written a lot about their thoughts about alignment. So the public deadline of four years, there's also a bunch of posts like [Our approach to AI alignment](https://openai.com/blog/our-approach-to-alignment-research), [Planning for AGI and beyond](https://openai.com/blog/planning-for-agi-and-beyond). I guess my question is, can you say a bit about what goes into that decision to be... It seems like you're putting a lot of work into being very public about your thoughts.

**Jan Leike:**
I mean, I think we need to. Ultimately we are all in the same boat on the question of is superintelligence aligned? And I think the public really deserves to know how well we're doing and what our plan is. And a lot of people also want to know. And so because of that, I think it's really important for us to be transparent about how we're thinking about the problem and what we want to do. But on the flip side also, I really want to invite a lot of criticism of our plan.

Our plan is somewhat crazy in the sense that we want to use AI to solve the problem that we are creating by building AI, but I think it is actually the best plan that we have, and I think it's a pretty good plan and I think it's likely to succeed, but if it won't succeed, I want to know why or I want to know the best reasons why it wouldn't succeed. And in a way, I really want to invite everyone to criticize the plan or help us improve the plan. And I also want to give other people the opportunity to know what we are doing and if they think it's a good idea, they can do it too.

## Superalignment team logistics <a name="logistics"></a>

**Daniel Filan:**
Sure. Speaking of, so this new superalignment team, how big is it going to be in terms of people?

**Jan Leike:**
Yeah, so we are about 20-ish people right now and we might be maybe 30 people by the end of the year. I think it's not that likely that the team will be larger than a hundred people before the end of the four years. And really, the way the team size will scale is we'll have millions of virtual people basically, or virtual equivalents of OpenAI employees or something. And so in that sense, we'll scale massively.

**Daniel Filan:**
All right, so there's people and yeah, I guess as you mentioned, the other input is computation. So I think it's, yeah, 20% of compute secured to date, why 20% as opposed to 5% or 80% or something?

**Jan Leike:**
So we want to have a number that is large enough so that it's clear that we are serious about investing in this effort and we want to allocate a serious amount of resources. 20% of OpenAI's compute is not a small number at all. And I think it's definitely the largest single investment in alignment that has been made today, but also it's plausible that it is more than all the other investments combined. So it is pretty large in that sense. But also if we made it much larger, you can then question of, "Can OpenAI actually realistically do this", right? If OpenAI still wants to develop frontier models and pre-train state-of-the-art AI systems, that needs a lot of compute.

**Daniel Filan:**
Okay. And I guess I think it was mentioned in terms of "compute secured up to date", I guess because you guys don't necessarily know how much more you're going to get, but are you imagining roughly keeping it at that proportion as you get new stuff in?

**Jan Leike:**
I mean it depends how things go. So 'compute secured to date' means everything we have access to right now plus everything that we've put in purchase orders for, so it is actually really a lot. In terms of how much we'll actually need, we don't really know. Maybe we actually don't need all of that. Maybe we need much less. Maybe we end up needing a lot more and then we have to go back and ask for more. But the thing is... in particular, we want to spend it wisely and not squander it because it's a lot of resources. And right now we still have a lot of work to do to figure out how to spend it wisely. But I'm pretty optimistic that if we had a good way to spend it and we needed more, that we could get more.

## Generalization <a name="generalization"></a>

**Daniel Filan:**
All right. Okay. So one thing that I think was mentioned in [one of the footnotes in the blog post](https://openai.com/blog/introducing-superalignment#fn-B) is: favorable assumptions that are made to date might break down. And one of them was about I think that generalization would be benign or something like that. Can you say how you're potentially thinking differently about generalization?

**Jan Leike:**
So the generalization effort is a team that we started recently and especially [Collin Burns](https://collinpburns.com/) has been spearheading. And the question is basically, or the question as phrased now is: how can we understand and improve our model's ability to generalize from easy tasks that we can supervise to harder tasks that we struggle to supervise? And so specifically you can think of this as being complementary to scalable oversight, where in scalable oversight you would be looking at empowering human evaluation of what your system is doing. Or if you're thinking about recursive reward modeling, you're like, "Can we recursively evaluate everything that AI is doing with AI assistants that we recursively evaluate?"

And one thing I really like about this is it puts the human really in the loop, front and center, and looking at everything the AI system is doing. Of course in practice you couldn't literally do that because AI systems will just do a lot, but you can look at everything with some small independent probability. But then you're still left with this question of: how do you know that the models generalize to the cases where you're not looking at? And so typically the way I used to think about this in the past is that you just make sure that you have mostly IID generalization, where the tasks that you're looking at are the same distribution as the tasks you're not looking at.

**Daniel Filan:**
Yeah. In fact, I think there was this blog post on [your Substack](https://aligned.substack.com/) that... I think it said something like [you just weren't going to rely on generalization at all](https://aligned.substack.com/i/88447351/we-dont-know-how-well-generalization-will-work) and just keep on training, keep on being IID or something.

**Jan Leike:**
Yep. So this was the original plan, or at least my original thinking was I wanted to really not have to rely on non-IID generalization, because in neural networks that doesn't work so well and it's poorly understood. But the new question is, "Well, what if we actually did understand it? What if we can actually say something meaningful about generalization?" And I think that's a really good question. And I think also [Ilya](https://www.cs.toronto.edu/~ilya/) has been raising this question a lot as well. And so what we want to understand is, on the things that we are not supervising, even if they're not IID, can we say something meaningful about how well the model generalizes? Does it generalize in the way of human intent or does it generalize in some other way that... stuff that looks good to a human but actually isn't? And so I think actually we can really study this question empirically right now, with carefully crafted experiments.

And so what we have been looking at is trying to split data sets, existing data sets into easy problems and harder problems, where the easy problems are just defined as what a small model gets right. And then we are trying to understand or improve: how can we improve the accuracy of a large model on the whole dataset? So this is a really interesting topic because it's kind of like it gives you a whole new pillar in the general arsenal of what I would call training and validation techniques. So let's say you get that to work super well, now you can supervise your reward model on some easy tasks that you have a lot of confidence you can evaluate, and then you can get the model to generalize if you can solve this problem or you get the model to generalize to harder problems.

And then you have this reward model that is generalizing the way you wanted it to, to the harder tasks even if you're not supervising. And now you could use that for training. And then you still have the problem of how do we know it's actually aligned now? But you can still leverage scalable oversight interpretability on these other techniques for validation. Or reversely, let's say we train our automated alignment researcher via scalable oversight and we use generalization as a validation technique where we generalize the probability of truthfully answering according to the model's best knowledge.

And then we ask a question about Iis there a subtle flaw here? Is there some kind of Trojan in this code that a model that we aligned with scalable oversight wrote?" and so on. And so now there's a really cool opportunity here also, which is we can do this high-level cross validation, but now we can train two different models. One is trained using the generalization techniques. One is trained using scalable oversight, and now we can have them crosscheck each other's answers and be like, "Well, are these fundamentally different models or the same models? What are the important differences?"

**Daniel Filan:**
Okay. Sorry, when you say "trained via the generalization technique", is that just train on the easy problems and they turned out to generalize to hard problems, or...?

**Jan Leike:**
So if you understand how your models generalize from easy to hard and you can make it generalize really well. By which I mean the accuracy is basically as good as if you had trained on the hard problems (that, by assumption, we don't actually have ground truth [for]). Now you could use it as a reward model or you could use that as... it generalizes "which action or which answer would I prefer if I really knew what was going on here?"

**Daniel Filan:**
Yeah, I wonder how... So when I think about interpretability, one frame I sometimes have for it is that it's about this problem of non-IID generalization, right? Why do you want to know the internals of the model? Well, because you want to know what it's going to do in cases you haven't checked, right? So I'm wondering how do you think those two research agendas interact?

**Jan Leike:**
In some ways they have this overlapping question they want to answer: what does the model do out of distribution? And they have two very different paths of answering them, at least it seems to me.

**Daniel Filan:**
And you might hope you could cross-validate, for example.

**Jan Leike:**
So for cross-validation you have to have a different, or some kind of different split of your training set, right? So what I mean with cross-validation here is you have one training run where you train using the generalization method and then you validate using interpretability and scalable oversight and other techniques. And then the second training run you train with scalable oversight and you validate with the generalization methods and interpretability and other methods. And so you have two independent attempts at the problem.

**Daniel Filan:**
Yeah, I guess I meant cross-validation in the very loose sense of "things validate each other sort of in a cross", yeah.

**Jan Leike:**
But I mean, I think the best case would be that they actually complement [each other] more than they do the same thing. If you can understand or you can improve how the models are generalizing, then that gives you, in a sense, a way to leverage the model's internals for whatever you're trying to do in the best way. Or let's say you're trying to extract the model's best beliefs about what's true in the world. And that's fundamentally difficult with RLHF, because RLHF reinforces what the human thinks is true: you rank things higher that sound true to you. And so you're really training a model to tell you what you want to hear or what you believe, but it might not be what the model believes. But this generalization technique gives you a way to extract these... what is actually the model's true best beliefs? If it works; we haven't actually proven this out.

Whereas if you have really good interpretability tools, you could hope to do something similar where you would try to pinpoint models' beliefs or internals or something from the internals. But I think it might be fundamentally harder because you never quite know: is this the best belief the model could produce or is it just the belief of somebody smart the model is modeling? Or if you have... There's this hypothesis that pre-trained language models are just ensembles of different personas, and you might extract the beliefs of one persona or a bunch of personas.

**Daniel Filan:**
Yeah. I guess you're going to need some sort of causal modeling there from alleged beliefs to outputs, right, in order to have that really work?

**Jan Leike:**
That's right. You might need a lot more. And then on the flip side, I think in interpretability, this application is really natural. It's like being a lie detector or finding evidence of deception or finding a secret plot to overthrow humanity inside the model. And that might be a lot harder to extract with generalization in the same way.

**Daniel Filan:**
Yeah. I guess with generalization you have to pick the generalization distribution, and the hope is that maybe interpretability could tell you things about, it's got some kernel of lying in there, but it only unlocks here, or it's got no kernel of lying or something.

**Jan Leike:**
Yeah.

**Daniel Filan:**
Gotcha. Yeah, that makes sense.

**Jan Leike:**
Yeah, and I think fundamentally this is also a really interesting machine learning problem: just how do neural networks actually generalize outside of the IID setting, and what are the mechanisms that work here? How has that come to pass? In which ways do they generalize naturally, in which ways they don't? So for example, with the [InstructGPT paper](https://arxiv.org/abs/2203.02155), one of the things that we found was, the model was really good at following instructions in languages other than English, even though our fine-tuning dataset was almost exclusively in English. And sometimes it would have these weird artifacts. You would ask it in a different language, let's say German. I would ask it in German to write a summary, and it would write the summary, but it would do so in English. And the model generally totally understands which language it's speaking, and it can understand all the languages, but it wouldn't necessarily mean that it would have to follow the instruction in German. You could be like, "Well, you're speaking in German, I don't know what to do in German, so I could do some other thing". But it fundamentally generalized following instructions across languages.

**Daniel Filan:**
Yeah, yeah.

**Jan Leike:**
But we don't know why. This effect has been observed in other settings, and it's not unique here. And it's also, there is intuitive reasons why it would do that. Humans generalize across languages, but I would really love to know the mechanism inside the model that generalizes that, or generalizes to following instructions and code. But it doesn't generalize in other ways.

For example, the way refusals generalize is often very different, where ChatGPT is trained to refuse certain tasks that we don't want to serve, according to our content policy (if you, for example, ask for assistance in crimes or something). But then you can do these jailbreaks. And that's really interesting, because basically there's ways to trick the model. You can make it role play, or you say, "do anything now", or there's these really fun prompts on the internet, and then the model clearly complies and happily assists you in crimes, which it shouldn't do. So it somehow didn't generalize refusing the task to these other settings.

**Daniel Filan:**
Yeah, yeah.

**Jan Leike:**
And so why does it generalize in the first setting, in the first case, but it didn't generalize here? I don't fundamentally know an answer to this question. I don't think anyone does. But it seems like a thing that's really important to understand.

**Daniel Filan:**
Yeah, yeah. That seems right. Cool.

So one question I have... so a recent [guest I had on the podcast](https://axrp.net/episode/2023/04/11/episode-20-reform-ai-alignment-scott-aaronson.html) was [Scott Aaronson](https://scottaaronson.com/) and he mentioned that whenever he talks to [Ilya Sutskever](https://www.cs.toronto.edu/~ilya/), apparently Ilya keeps on asking him to give a complexity-theoretic definition of love and goodness. How much will that be located within the superalignment team?

**Jan Leike:**
So there's a lot of different exploratory projects that we'll probably do and try. And I think ultimately, there's a question of can you summon, somehow (this is Ilya's language), how can you summon the alignment-relevant concepts? One of the things you would want to summon is: does the model fundamentally want humanity [to] succeed? Or as Ilya would say, does it love humanity? And so, you can ask the question of how do you... if the model is really smart and it has read everything, it knows exactly how humans think about immorality... you can ask GPT4 to make different moral cases from different philosophical perspectives, about different scenarios. And it's generally not bad at that.

So it fundamentally understands what humans mean with morality and how we think about stuff. So how do we get it to leverage that, actually? How do we extract it? How do we get it out of the model so we can then, let's say, use it as a reward signal? Or use it as a thing that the model fundamentally believes or cares about? And so, I think that's the core of the question.

## Complementary research <a name="complementary-research"></a>

**Daniel Filan:**
Okay. So another question I have is, so you're working on the superalignment team, but you guys can't do everything. I'm wondering what kind of complementary research could other groups or teams do that would be really, really useful for the superalignment team?

**Jan Leike:**
Yeah, there's a lot of things here I'm really excited about. I think there's this really important question of: how do we build fair and legitimate process for extracting values from society or basically eliciting values from society that we can align AI to? And I think there's a lot of important open problems here that we need a lot of progress on. There's a lot of questions around how do we make today's models more aligned, solve hallucinations, solve jail breaking, try to improve monitoring -- can you get GPT-4... if you try to jailbreak GPT-4, can the GPT-4 say "Oh yeah, I'm being jailbroken"? And can we generally build systems that really crack down on any misuse of the systems in the world? There's a lot of questions that is related to that, that the AI ethics community is really interested in, of can we get the systems to be less biased? And how can we get it to take into account underrepresented groups' views? and so on.

And one of the problems with RLHF is also that, it's not good at optimizing for distributions of answers. So there's this classical example with InstructGPT where if you ask it, "Tell me a joke", it will tell you the same out of five jokes or something, that it has in this portfolio, because it does this mode collapse. And that's fundamentally... it creates a lot of bias because then you're aligning to the highest mode of the labeler pool. And it depends a lot on how the labeler pool was selected.

I think there's also a lot of other important questions around evaluation of AI systems. How do we measure how aligned the system is, and can we build eval suites for what capabilities would actually be dangerous? And there's a lot of work that started happening in the last year that I'm very excited about, that people are getting serious about measuring these things. I think that's going to be really important to just create a lot of transparency around where we are at with the models and which models are actually dangerous and which are not. And I think, in the past there was a lot more uncertainty and then that creates anxiety around, what should we do with this model?

And then going more broadly, there's the more general AI governance questions of: who is allowed to do really big training runs and what kind of safeguards do you have to have before you're allowed to do that? Or if we solve alignment and people know how to build aligned AI systems, how do we get from that to a great future? Yeah. And then in terms of... I mean I think you're kind of asking for the technical alignment directions that I'd be excited about.

**Daniel Filan:**
I was asking broadly, but I'm also interested in that.

**Jan Leike:**
I think there's actually a lot more scope for theory work than people are currently doing. And so I think for example, scalable oversight is actually a domain where you can do meaningful theory work, and you can say non-trivial things. I think generalization is probably also something where you can say... formally using math, you can make statements about what's going on (although I think in a somewhat more limited sense). And I think historically there's been a whole bunch of theory work in the alignment community, but very little was actually targeted at the empirical approaches we tend to be really excited [about] now. And it's also a lot of... Theoretical work is generally hard because you have to... you're usually either in the regime where it's too hard to say anything meaningful, or the result requires a bunch of assumptions that don't hold in practice. But I would love to see more people just try.

**Daniel Filan:**
Sure.

**Jan Leike:**
And then at the very least, they'll be good at evaluating the automated alignment researcher trying to do it.

## Why is Jan optimistic? <a name="why-is-jan-optimistic"></a>

**Daniel Filan:**
Nice. One question: I think earlier you mentioned that you were relatively optimistic about this plan, and not everyone is, right? So I think there's [this play money prediction market](https://manifold.markets/SG/will-superalignment-succeed) that has 15% on the proposition that this will succeed. There's some concerns that it's really hard to align this automated human-level alignment researcher. It's got to do pretty hard thinking. It potentially has a lot of levers over the future. So why are you so optimistic, do you think?

**Jan Leike:**
I think it's a great question. So the prediction market that you reference is particularly whether we would succeed at it in four years, which is somewhat... it could be a much harder question than "will this plan succeed?" And I think if you just asked me, will some version of the plan that we currently have succeed at aligning superintelligence? I would say currently I'm at 85%, and last year I was at probably 60%. And I think there's a bunch of reasons to be optimistic about it. And in general I think this holds true even if alignment doesn't turn out to be easy.

And so [why am I optimistic about this](https://aligned.substack.com/p/alignment-optimism)? So I want to give you five reasons. The first reason is, I think the evidence about alignment we've seen over the past few years has actually been really positive. At least for me, it was positive updates relative to what I expected to go. So the first reason is, the success of language models. If you actually at the same time come preloaded with a lot of knowledge about what humans care about, and how humans think about morality and what we prefer, and they can understand natural language, you can just talk to them. In some ways that makes expressing what we want them to align to so much easier than if you said... you had some kind of deep RL agent that was trained in some portfolio of games or virtual environments, that wouldn't necessarily involve as much language but could also lead to a lot of really important skills.

I think also the other thing that was a big update for me is how well RLHF actually worked. So when I first started working on RLHF, that was with [the deep RL from human preferences paper](https://arxiv.org/abs/1706.03741), and back then I had a reasonable probability that we just wouldn't actually get it to work on a reasonable timeframe just because GANs (generative adversarial networks) were kind of hard to train at the time, and were very finicky, and we were kind of in some sense doing something very similar where we trained this reward model, which is a neural network, and then we used it to train some other network, and that can fail for a bunch of reasons. And now we add deep RL into the mix, which also was finicky at the time. So I thought it might not actually work, but it actually worked quite well, and we got... in a bunch of games, even much of [the Atari games](https://jair.org/index.php/jair/article/view/10819), it was almost competitive with training on the score function, which is kind of wild.

But I think much more importantly, it was really interesting to see how well RLHF worked on language models. And so in particular, if you think about the difference between InstructGPT and the base model, that we fine-tuned from, it was really stark, to the extent that the instruction fine-tuned version, the first versions we had were preferred to a 100X larger base model on the API tasks that we had at the time, which were real tasks that people wanted to pay money for. And that's a really, really big difference. It tells you that what we did during the RLHF fine-tuning just made the model so much more effective at doing the task that humans asked for.

And at the same time, we use very little compute to do it and we hadn't even integrated that much. We hadn't collected that much data. And so it was kind of like our first real attempt at trying to use this, to align an actual real world system. And it was wild that it worked so well. And having a GPT-2-size InstructGPT, that was preferred over GPT-3, that's... Yeah.

**Daniel Filan:**
Yeah, yeah.

**Jan Leike:**
That was really effective. And so while I don't think RLHF is the solution to alignment, and especially not for superintelligence, the fact that the first alignment method that we really seriously tried works so well, for me at least is an update that it is easier than I thought, because the reverse would've been an update too. If it hadn't worked, it would be like, "Okay, now I have to believe it's harder than I thought".

The other part of this is, I think actually we are in the place where we can measure a lot of progress on alignment. And so for RLHF specifically, we could make various interventions and then do human evals and see how much the system improves. But also on a whole bunch of other things. Like on scalable oversight, we can make these randomized controlled trials with targeted perturbations, that's a way to evaluate it. Or you can do the sandwiching experiments that leverage expert data. Or you can automate interpretability; we have this automated score function, and we can make a bunch of changes and see how much it improves on that score function. It's not a perfect score function, but it is a local metric. It gives you a local gradient to improve. And I think that's really important, because now you're setting yourself up for iteration, and you can iterate and make things better, and that gives you a direction to improve.

And now you can argue: does it actually get us to the goal? And I don't think it would get us to the goal of aligning superintelligence, but I think it has a good chance to get us to the goal that we actually want to get to, which is this automated alignment researcher that is roughly human-level. And I think that's the third point I wanted to mention for why I'm optimistic, which is this much more moderate goals. When I set out working on alignment many years ago, I was like, "Okay, to figure out how to align superintelligence seems hard, I don't know how to do it". But this much more modest goal, or what I would call a minimal viable product for alignment, you're not trying to solve the whole problem straight up, you're just trying to bootstrap, you're just trying to solve it for something that is as smart as you, and then you run that a lot.

And I think with that realization I was like, "Oh, actually this is a lot easier than I originally thought, because we need to clear a much lower bar to actually fundamentally succeed here". And I think that's a good reason to be optimistic.

The fourth I want to mention is, evaluation is easier than generation, which we've already talked about. And I think it fundamentally holds for a lot of tasks where it's so much easier to figure out what is a good smartphone to buy than it is to make a smartphone. Computer science has a lot of examples of [NP](https://en.wikipedia.org/wiki/NP_(complexity)) tasks, where SAT-solving or various versions of constraint satisfaction, where you're trying to find a solution. And once you've found it, it's easy to check, but it's hard to find. And then also, I think it holds for a lot of commercial activity where if you're hiring someone to work on a problem, you have to be able to evaluate whether they're doing a good job. That takes a lot less effort than it takes them to do it. If you're doing academic research, there's a lot less effort that goes into peer review, than goes into doing the research. And of course peer review is not perfect, but it gives you a lot of signal pretty quickly. And then I also fundamentally believe that it's true for alignment research, right? Evaluation is easier than generation and so, if only humans evaluate alignment research instead of doing it, I think we would already be accelerated.

And then the last reason I want to give is, basically a conviction in language models. I think language models are going to get really good, they're pretty naturally well suited to a lot of alignment research-y tasks, because you can phrase these tasks as text in text out, be it the more (what we talked about) the ML-ish tasks where you're running experiments and understand the results, that I think other people are definitely going to do, or the more conceptual or research-y things, where we are fundamentally confused about what to do next or how we should think about a certain problem. And then the model tries to help us understand it. And all of these are text in, text out tasks basically. And maybe the most complicated other thing you have to do is, look at some plots or something, which even GPT-4 can do. And so, I think actually the current paradigm of language model pre-training is pretty well-suited to the kind of alignment plan that I'm excited about, and that superalignment is working on.

### Long-term agency in LLMs? <a name="long-term-agency-in-llms"></a>

**Daniel Filan:**
Okay. So yeah, part of that is about evaluation versus generation and I guess that's partly about humans doing evaluation. Presumably there's also a hope that we can leverage AI, leverage the bit of AI that's evaluating things and get that on our side. A lot of the things you've mentioned are conviction in language models, seems like alignment's easier with language models... And I'm wondering, I think I'm a little bit more skeptical of how useful language models are. So I certainly think that it seems like they're just good at modeling text, and doing text-based answers for which we can provide a good score function. I guess in terms of how useful they are for alignment, I think the things you mentioned were, well one, they're not as goal-oriented necessarily. I don't know if you said that, or if it was just in the post.

**Jan Leike:**
That was in the post. I don't think I said it.

**Daniel Filan:**
Okay. Do you believe it?

**Jan Leike:**
I believe it.

**Daniel Filan:**
Okay, great.

**Jan Leike:**
At least out of the box, if you pre-train the model, it's like pre-training on this myopic objective of "predict the next token" on this random internet text, which is not an objective that necessarily forces you to pursue long-term goals. There's no guarantee that it doesn't emerge somehow. And you can tell stories how that could happen, but at least a priori, it's a very myopic objective.

**Daniel Filan:**
Yeah. I guess the concern is something like... well for one, often when people are generating texts, they have long-term goals. Right? So for instance, suppose you train on a bunch of arXiv papers - arXiv being the place where people publish scientific papers, at least in computer science and physics, I guess. The reason people write papers is that they have some research project that they want to advance, they want to promote their career maybe. And it seems to me that if you're modeling something which is generated by things that have long-term goals, maybe you get the long-term goals too, right?

**Jan Leike:**
Yeah, I think that's a good story of how that could emerge. I think the main counterargument here is that, modeling something as an agent that pursues long-term goals and then modeling how they would go about those goals, and how reality responds to that, and then what the final output is, that leads to the next token, is a lot of... it's a very complicated function. And what pre-training does is, it incentivizes the simplest functions to be found first. Right? [Induction heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html) is a very good example of that, where you have this simple induction mechanism that gets discovered very early in training from even small models. And then -

**Daniel Filan:**
"Mechanism" being just like, "See, I've got to predict the next word. Did the last word occur previously in the text? What was the next word for that word?" Maybe it's just that.

**Jan Leike:**
Exactly.

**Daniel Filan:**
And it's pretty simple.

**Jan Leike:**
Right. And then you build more complicated mechanisms on top of that. But because you're trying to improve on the pre-training loss, you kind of want to learn the simplest functions that help you improve the most first. And that's generally what happens. And so, before you get to the point where you're learning this really complex function of modeling someone as an agent with long-term goals, there's a lot of other functions you'll learn first. And this will be one of the last ones, I would expect, because it is so complicated, and because there's only so many things you can do in a single forward pass because you only have so many layers. And so I think that's a good reason to expect that not to arise very early. But it's definitely something you should measure and actually look at empirically.

**Daniel Filan:**
Yeah. And I guess this is one of these places where I think theory can be useful: there's some interesting [points] like "things learn easier functions sooner".

**Jan Leike:**
Exactly.

**Daniel Filan:**
That's a theory question.

**Jan Leike:**
Yeah. Can you actually say something meaningful theoretically about this?

**Daniel Filan:**
Yeah, yeah.

**Jan Leike:**
Well, the [lottery ticket hypothesis](https://arxiv.org/abs/1803.03635)singu is not quite theoretical work per se, but I think it tries to get at this a little bit.

**Daniel Filan:**
Yeah, I think it's also... I don't know. I'm currently in a phase where whenever anyone says something, I'm like, "Ah, that sounds just like [singular learning theory](https://metauni.org/slt/)". But this really does sound like singular learning theory. People can listen to [other podcasts](https://theinsideview.ai/jesse) for that.

### Do LLMs understand alignment? <a name="do-llms-understand-alignment"></a>

**Daniel Filan:**
So another thing you mentioned that was a benefit of language models is that, they've read a large fraction of the internet and somehow they roughly know what it is to behave well, because they know what we've written and they understand us.

And I guess one worry I have here is, right now in my head, I don't have a nice compact specification of how I want superintelligent AI to behave. And the way you can tell that is that if we did, we wouldn't have an alignment problem anymore. We would have "hey, I wrote the Python pseudo-code, just make the networks bigger". And because I don't have that, it seems like you can't pick that out from the text that I've written. I'll say things like, "be nice, don't destroy all humans" or something. But the solution to alignment isn't in my head, therefore you might think that it couldn't be extracted from the text. Yeah. I'm wondering what you think about that kind of concern, or that kind of counterargument to [language] models having the right answer inside them somewhere.

**Jan Leike:**
Yeah, I think there's some truth to this, but also I think in practice it's very unclear to me how much it really matters. To some extent, if we pre-train a model on everything you've ever said, it won't know what you actually think. And you probably have a lot of thoughts that you haven't written down. But it can... in general, I think it would be in practice quite good at predicting what you would say about various situations, events, or scenarios. And so in that sense, I don't think it would be fundamentally hard for the model to look around in the world in the future, and know whether humans would like it.

**Daniel Filan:**
So the idea is somehow, I implicitly know how I would behave if I were a superintelligence, and it can do that even if I don't have a compact rule in my head.

**Jan Leike:**
No, in the sense that, I think the model will just be smart enough to know how Daniel will think about what's going on in the world. And I don't think the blocker will be that the AI system doesn't understand what humans fundamentally want or care about, or I don't think it will be wrong about these things, just because it's smart and capable and has read everything; and it's kind of clear if you've read everything: humans haven't read everything and it's still kind of clear to us. But the big challenge is not teaching it to the system. And I think that's what language models make so visceral, but the challenge is really to then get it to actually do it.

**Daniel Filan:**
Yeah. So it knows what the objective is somewhere, and we just need to figure out how to wire that up to the actions in the right way.

**Jan Leike:**
Yeah. You could kind of imagine this really, really competent sociopath that knows exactly what humans wanted to do and then just decides not to do that. And that is totally a thing you can do, and it happens to humans as well.

## Following Jan's research <a name="following-jans-research"></a>

**Daniel Filan:**
Gotcha. Okay. So it's about time to wrap up. You've been very generous with your time, but yeah, I just wanted to ask, if people are interested in following your research or maybe taking part themselves, what should they do?

**Jan Leike:**
Yeah, great question. I'm glad you asked. Yeah, we're trying to hire a lot of people right now. We really want to staff up the superalignment efforts. So if helping us align superintelligence in four years sounds appealing to you, please consider applying. You can find our job postings on [openai.com/jobs](https://openai.com/careers). And then if you're interested in following what I think about alignment specifically, I have a Substack - it's [aligned.substack.com](https://aligned.substack.com/) and you can follow me on Twitter. I'm [@janleike on Twitter](https://twitter.com/janleike), all one word. And yeah, thank you so much for being so interested in our work.

**Daniel Filan:**
Yeah, so the links to all of those will be in the description. Thanks so much for being on and to the listeners, I hope this was a useful episode for you.

**Jan Leike:**
Thank you so much for having me.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with the transcription. The opening and closing themes are also by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://www.patreon.com/axrpodcast) such as Ben Weinstein-Raun, Tor Barstad, and Alexey Malafeev. To read a transcript of this episode, or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
