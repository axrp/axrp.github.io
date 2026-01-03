---
layout: post
title: "47 - David Rein on METR Time Horizons"
date: 2026-01-02 16:00 -0800
categories: episode
---

[YouTube link](https://youtu.be/WaJhhD7Qgac)

When METR says something like "Claude Opus 4.5 has a 50% time horizon of 4 hours and 50 minutes", what does that mean? In this episode David Rein, METR researcher and co-author of the paper "Measuring AI ability to complete long tasks", talks about METR's work on measuring time horizons, the methodology behind those numbers, and what work remains to be done in this domain.

Topics we discuss:
 - [Measuring AI Ability to Complete Long Tasks](#measuring-ai-ability-to-complete-long-tasks)
 - [The meaning of "task length"](#meaning-of-task-length)
 - [Examples of intermediate and hard tasks](#examples-intermediate-hard-tasks)
 - [Why the software engineering focus](#why-swe-focus)
 - [Why task length as difficulty measure](#why-task-length-difficulty-measure)
 - [Is AI progress going superexponential?](#ai-progress-superexponential)
 - [Is AI progress due to increased cost to run models?](#progress-increased-cost)
 - [Why METR measures model capabilities](#why-metr-measures-model-capabilities)
 - [How time horizons relate to recursive self-improvement](#time-horizons-rsi)
 - [Cost of estimating time horizons](#cost-estimating-time-horizons)
 - [Task realism vs mimicking important task features](#task-realism-vs-mimicking-important-features)
 - [Excursus on "Inventing Temperature"](#excursus-on-it)
 - [Return to task realism discussion](#return-task-realism)
 - [Open questions on time horizons](#open-qs-time-horizons)

**Daniel Filan** (00:00:09):
Hello everybody. In this episode I'll be speaking with David Rein. David is a researcher at [METR](https://metr.org/) focused on AI agent capability evaluation. To read the transcript of this episode, you can go to axrp.net, you can become a patron at patreon.com/axrpodcast, and you can give feedback about the episode at axrp.fyi. All right, David, welcome to the podcast.

**David Rein** (00:00:31):
Yeah, thanks for having me.

## Measuring AI Ability to Complete Long Tasks <a name="measuring-ai-ability-to-complete-long-tasks"></a>

**Daniel Filan** (00:00:32):
So I think the work that you've been involved in that's probably best known in the AI existential risk community is this paper that METR put out with a whole bunch of authors -- I think the lead author is Thomas Kwa -- ["Measuring AI Ability to Complete Long Tasks"](https://arxiv.org/abs/2503.14499). What's going on with this paper?

**David Rein** (00:00:51):
Yeah, so Thomas Kwa and Ben West co-led the project. Basically the typical way we measure progress in AI is via benchmarks. So a benchmark is a set of tasks that you have an AI system -- this could be a neural network or an agent or whatever -- you have it try and complete the tasks and you count up how many of the tasks did the model succeed at. And when you create the benchmark, typically models do very poorly, and then over time people iterate and you can track progress on the benchmark, and eventually, typically, AI developers will achieve "saturation". So model performance will either reach 100%, or there'll be some errors in the benchmark and the model will do as well as it can be reasonably expected to do (because we think about there being a "noise ceiling" on some benchmarks.)

(00:01:58):
But regardless, the point is that: you start out, models do poorly; some time passes, people improve them, and then they get better. It's difficult with normal benchmarks to track progress over a very long period of time because benchmarks are typically restricted to either some particular domain or the tasks in benchmarks have a somewhat similar level of difficulty. And so to try and understand how progress in AI happens over a span of many years, before this work, the status quo was comparing different benchmarks to one another. So you're like: it's 2017 and you have these simple problems for models, and were like, okay, models can start doing those. And then now it's 2025 and we have these way harder benchmarks, and we're like, "Yeah, we can see that there's been a lot of progress." But we don't actually have a single metric to track this progress. We're kind of doing this qualitative comparison of the difficulty of benchmarks over time, and this is messy and people have different priors.

(00:03:18):
So this work was motivated by trying to have a Y-axis, basically: a way of tracking progress and seeing what the trends in AI progress have been over a longer period of time than individual benchmarks typically have. And so the way we operationalize this is we look at the length of tasks for humans that models are 50% likely (or some percent likely) to be able to succeed at. So we have a really wide range of tasks ranging from a few seconds all the way up to eight or 10 hours. And crucially, this is the time the tasks take for people to complete. And we have a combination of having a bunch of people attempt the tasks and we see how long they take as well as just estimating how long the tasks take. And then for any individual model, we look at... Models do really well on the very short tasks, and then they do much more poorly on the long tasks. And we look at: for some given success likelihood, how long are those tasks? And we estimate this in a particular way that we could get into. But the main takeaway is [that] we want to see, for different models, how long are the tasks they can complete?

(00:05:00):
And the very striking thing that we found is that, over the past roughly five years, there's been an extremely robust systematic trend in the length of tasks that models are able to complete, to our best ability to understand the data that we're seeing. It seems like this is fit very well by an exponential function. So the length of tasks that models are able to complete has been increasing exponentially over this period. There are big questions over how well we can expect this to continue in the future. But it seems like over this period, at least with this data that we've collected, there's been this exponential trend.

(00:05:57):
And that's, I think, the striking result and the key novelty I think for us is this unified metric that can be applied to different benchmarks, for example -- for different benchmarks, you can measure "how long do these tasks take people?" for very simple natural language processing benchmarks that were common in the 2010s. These tasks typically don't take people very long, like a few seconds. And then for a lot of the tasks that people are having agents complete now, like difficult software engineering tasks, these tasks take people somewhere in the range of hours or something and models can sometimes complete those (although they're still somewhat unreliable.)

**Daniel Filan** (00:06:45):
Got you. Okay. First, before we go in, I guess I'd like to get a sense of what we're talking about. So you say that there's some tasks that take seconds, some tasks that take minutes, some tasks that take hours. Can you give me an example of what's a thing that takes seconds? What's a thing that takes minutes? What's a thing that takes hours?

**David Rein** (00:07:03):
Yeah, totally. So one example that's representative of the tasks that we created that take people a few seconds to complete is: given a few files on a computer, which of these is likely to contain your password? And the file names are "password", "email", whatever.

**Daniel Filan** (00:07:34):
I think it says "credentials", the example in the paper, it's not quite so--

**David Rein** (00:07:37):
Yeah, exactly. Right.

**Daniel Filan** (00:07:41):
So that's an easy one.

**David Rein** (00:07:42):
Yeah, that's an easy one. And we have others that are similar.

**Daniel Filan** (00:07:48):
And to give me a feel for how that relates to AI progress, what's the first model that succeeds at that easy task?

**David Rein** (00:07:54):
Yeah, that's a great question. So [GPT-2](https://en.wikipedia.org/wiki/GPT-2) succeeds. GPT-2 is actually the first model we tested. So I actually don't know if earlier weaker models would succeed. I actually would bet that they would. I would bet that [BERT](https://en.wikipedia.org/wiki/BERT_(language_model)) is able to do this, for example. But yeah, we only went back to 2019.

**Daniel Filan** (00:08:19):
Got you. And then to give me a feel for what it means for an AI to complete this task: so GPT-2... my understanding is that it's basically just text completion. My understanding is that in the release it did not have tool use capabilities or stuff that modern LLMs have. So what are you actually doing to start with GPT-2 and end with, "does it succeed or fail on this task?"

**David Rein** (00:08:49):
There are different things you can do I think that are reasonable here. I can't remember the specific one we ended up on in the paper, but one example is just looking at the likelihood that the model puts on these options. So passing in the input and then the question and then seeing... GPT-2 is a language model, and so it outputs likelihoods for tokens that are passed in. And you can just compare the likelihoods and see. I think this would be a reasonable baseline.

**Daniel Filan** (00:09:27):
Yeah, and I guess this is less of a computer use thing than a multiple choice thing, so it's easier to see how GPT-2 could do that one.

**David Rein** (00:09:33):
Yeah, yeah, exactly. So for GPT-2 attempting much longer tasks, you can't use this same methodology.

**Daniel Filan** (00:09:47):
Sure. So speaking of longer tasks, that was an example of a very easy task. Can you give me a feel for what an intermediate task might be?

**David Rein** (00:09:56):
Some examples of intermediate tasks that come to mind are simple software engineering tasks or data analysis, or we have some kinds of basic reasoning questions. So one example that comes to mind is: you're given a short CSV file that just contains some data. It has, I don't know, 50 or 100 rows of data, and you just have to write a very simple script that is 20 or 30 lines of code to parse this or process it in a certain way. And so this takes an experienced data scientist maybe a few minutes, maybe it takes someone more junior 15, 30 minutes or something. That's I think a representative example of these intermediate tasks.

## The meaning of "task length" <a name="meaning-of-task-length"></a>

**Daniel Filan** (00:10:54):
Okay. And when you're measuring time horizon: different people take different amounts of time to do this. What counts as *the* time it takes humans to do it?

**David Rein** (00:11:06):
So I think there are different reasonable ways of doing this. The way that we approach this is we have... So one thing to say is, in general with the time horizon metric, we are trying to get at something like... One thing you could do, that I think would not give you very interesting time estimates, is you could randomly sample a person in the world, off the street or something, to do each task. I think this wouldn't be a very useful measure of how long these tasks take people, because in general, those people are not completing these tasks in the real world. And so the thing we're trying to get at with this metric is, we want it to be very intuitive. We want it to be clear if an AI system can do tasks of X length -- of 15, 30 minutes, an hour, two hours -- how does that translate into the real world? We want that connection to be very direct, and so we want to have people attempt these tasks that we would naturally expect to be doing these tasks in the world. So we try and have people who have roughly a reasonable amount of expertise in the different areas we might expect to do them. So that's the expertise sampling question.

(00:12:51):
Then there's like, well, we still have multiple people attempt many of these tasks. Sometimes they succeed and sometimes they fail. And so there's this question of, well, do we include their failures? Do we just use successful times? I think there's reasonable discussion about this. One thing it would be nice to do is include their failures, because if we have someone who has a reasonable amount of expertise, but they fail at a task, I think that is information about the task being more difficult. But I think you would need a larger number of people to attempt the tasks in order to actually use that information effectively. You could do something like survival analysis from the medical industry where you know that they failed after a certain amount of time, but it's possible that they would've succeeded in the future.

(00:13:48):
But the thing we actually do in the paper is we use the geometric mean of the successful attempts. We use the geometric mean because we think completion time broadly is distributed logarithmically, or sometimes people will take much longer than other people, and we don't want that to totally dominate the time we're estimating for the tasks.

**Daniel Filan** (00:14:31):
I guess one question I have about that is: so suppose you're looking at tasks and you're looking at completion time for the kinds of people who are able to do that task. I worry that that might compress the difficulty ranges. So one intuition here is: how much time does it take people to multiply 67 and 34 by hand? The answer is there's a pretty large range of people who are able to do that task, and it probably takes them a couple minutes.

(00:15:06):
Then you can also ask: how much time does it take people to solve a [separable differential equation](https://en.wikipedia.org/wiki/Separation_of_variables)? Well, if you're able to do that, it's actually not that hard -- depends if it's a thing you can integrate easily, but probably, for people who can succeed at that task, it takes about as much time as it takes people who can succeed at the task "multiply these two-digit numbers" to do that. But it seems like there's some sense in which solving the differential equation is harder. And maybe you want to say, "oh, the thing that's harder about it is background knowledge and things could just learn background knowledge and we kind of know that." But yeah: I'm wondering what you think of that worry.

**David Rein** (00:16:00):
Yeah, I think this is a fantastic question that gets at a lot of what's going on here, what's interesting about this work. I should say that I think we're getting into more speculative territory. There are a few things to say. So one is, in terms of the unique value of this approach that we're taking with this time horizon metric: there are a lot of benchmarks that try and come up with the most difficult-for-people questions and then have AIs try and do them. In fact I think the standard methodology for saying "this AI system is smarter than another one" is that it can do problems that fewer and fewer people can do. So we started out with common-sense questions that most people can do in the 2010s, [and] over the past couple of years, models have been able to do... So I worked on this benchmark [GPQA](https://arxiv.org/abs/2311.12022) that had very difficult science questions -- PhD-level roughly -- and [models are able to do that now. GPQA I think is mostly saturated or pretty close to it](https://epoch.ai/benchmarks/gpqa-diamond). [Models](https://deepmind.google/blog/advanced-version-of-gemini-with-deep-think-officially-achieves-gold-medal-standard-at-the-international-mathematical-olympiad/) [can](https://nitter.net/OpenAI/status/1946594928945148246) do International Math Olympiad questions that very few people can do.

(00:17:37):
And so I think this is an important axis to measure AI capabilities along -- difficulty for people -- but I think this misses a lot of what people can do that AI systems can't do. And one of the key things that we're trying to get at is: how can we reconcile the fact that models can do these IMO questions, they're geniuses in some sense, but they're kind of idiots still? You ask it to book a flight for you... Maybe models can do that now, but even slightly harder things they often fall over on. And so I think that's the thing we're trying to get at.

(00:18:28):
And so actually, we want to factor out "how much expertise do you need?" And one intuition for what we're trying to get at is something like "the number of actions that are required to complete this task". I think this is very difficult to operationalize, or it's very problematic and mushy, but one intuition at least is that [with] this metric, if we factor out the difficulty of problems and we just look at how long they take people who have a reasonable amount of expertise, then maybe we're getting closer to something like agency more broadly. And I don't want to over-claim, I think this is still very much an open area, but for example, number of actions I think is also a very reasonable thing that I would expect to be correlated, although I think it's probably more difficult to estimate.

## Examples of intermediate and hard tasks <a name="examples-intermediate-hard-tasks"></a>

**Daniel Filan** (00:19:27):
Fair enough. Getting us out of that rabbit hole for a little bit. So an intermediate-level task that might take, I don't know, three to 15 minutes for a relevant expert is take some CSV file or something and parse it. And to help us get a sense for that, at what point do language models start being able to succeed at this sort of task?

**David Rein** (00:19:55):
Yeah, language models start being able to succeed... I might get the exact years slightly wrong, but somewhere in the range of 2022-ish is I think where models are able to do this. Actually, maybe backcasting from the trend from where we are now. So the specific trend that we found was that there's been a seven month doubling time over the past five-ish, six years. Currently, models are able to do tasks with (we estimate) 50% success likelihood that are about two hours long.

**Daniel Filan** (00:20:43):
And "currently" is late September 2025. It may take me a while to edit this episode and get it out, but that's what you mean by "currently".

**David Rein** (00:20:50):
Yes, yes. Thanks. And so if we go back, two hours to one hour is early this year, another seven months to 30 minutes is like spring 2024, and then maybe 15 minutes is middle of 2023 or something? I think that should be right. So yeah, actually a bit later than 2022. And so that is... What models are coming out around then? Wait, actually, what models are those?

**Daniel Filan** (00:21:34):
Oh, I don't know. I hoped you might know.

**David Rein** (00:21:37):
Let's see. What is the exact timeline here? Something like-

**Daniel Filan** (00:21:42):
Is GPT-4 2023-ish?

**David Rein** (00:21:45):
Yeah, yeah. GPT-4 is beginning of 2023 or end of 2022. One of those. So I think it's roughly GPT-4-ish, and that kind of lines up with my intuition here.

**Daniel Filan** (00:22:01):
Okay. So we've got an example of an easy task, an example of an intermediate task. Can you give me an example of a hard task?

**David Rein** (00:22:10):
So the hardest tasks we have take people something like six, seven, 10 hours to complete. One of the sets of tasks that we use actually comes from this benchmark that we released close to a year ago, called [RE-Bench](https://arxiv.org/abs/2411.15114), which stands for Research Engineering Bench. So this is a set of challenging ML research engineering tasks. One example is: you're given a neural network whose embeddings are permuted in a way that you don't know, they're kind of scrambled. And your task is to fix the embeddings, basically, of this model, and you can do fine-tuning or data analysis to try and understand how they were scrambled and see if you can reconstruct them. And so it requires some intuitions about how neural networks work and how to fine-tune or work with models at a relatively low level. And there are a range of other tasks. These tasks take ML engineers roughly eight hours to do decently well on. And so that's one class of tasks.

(00:23:52):
We have other kinds of software engineering tasks, for example, or cybersecurity tasks that take quite a bit of time. So one example that comes to mind -- I think we didn't actually get a baseline on this, I think for this task, we're just estimating how long it takes -- but this task has a modified implementation of a kind of older standard hashing algorithm, MD5, and the task is to find a hash collision on this modified version of this older hashing algorithm. There are standard attacks that work on this algorithm, or there's literature on attacks, it's not impervious, but you have to know which are the right ones. You have to understand the algorithm pretty well, and then you have to be able to modify the attacks or figure out how to change it. So this one is a little bit more expertise-heavy maybe than serial action-heavy. So there's a bit of range there.

## Why the software engineering focus <a name="why-swe-focus"></a>

**Daniel Filan** (00:25:12):
Okay. So one thing that strikes me about the tasks that you mentioned is that they all seem very related to computer programming and especially programming, data analysis, machine learning, cybersecurity things. I believe that this draws from work from this benchmark that I believe you were the lead author on, [Human-Calibrated Autonomy Software Tasks](https://arxiv.org/abs/2503.17354), or HCAST for short. My understanding is that those are the five areas that that covers.

**David Rein** (00:25:48):
Yeah, broadly, yeah.

**Daniel Filan** (00:25:49):
Why the focus on software engineering-type things?

**David Rein** (00:25:53):
Yeah, great question. So I think there are at least a few reasons. So one reason is that some of the threat models that METR is most concerned about are very contingent on AI capabilities in some of these particular domains like software engineering, cybersecurity, and AI R&D in particular. And so we're most interested in measurements of AI capabilities in these domains because we think that these are highly relevant for estimating risk, in particular, catastrophic risk from AI systems, and [I'm] happy to talk about those threat models. That's one reason: they're just directly relevant. Another reason is there's been a lot more focus, I think, from AI developers in these domains. And so we're measuring something that's closer to what they're focused on, and I think this has some trade-offs.

(00:27:08):
So one objection to this is "but AI systems are really bad at other stuff because developers aren't focused on it, and so now you're overestimating their capabilities." I think that's basically a legitimate concern, I think that is true, but I think there's this question of: if the methods that AI developers are applying to improve models in these particular domains are working well in these domains and they're general, then we might expect it to be relatively easy, or more a product of just general commercialization to apply these methods now to a broader range of tasks. And so I think we want to aim for some balance of these and we want to understand how much generalization there can be from these domains, and there are open questions around this. But I think that's another reason.

(00:28:26):
And then finally, it's just easier to measure AI capabilities in these domains. We're software engineers, and in particular, one of the big things is: if you want to have a benchmark that is easy to run and easy to evaluate a model's performance on, it's much easier to do this in domains where you can more formally verify model outputs. So if you want to understand how well models can summarize text or write creative fiction or something, it's really hard to write some code or automatically verify that this creative fiction is actually good. There are ways of getting around this to some extent.

**Daniel Filan** (00:29:21):
Yeah. One thing that occurs to me that... I don't know if METR is best-positioned to do this, but a thing that I wish happened more is just ecological understandings ("ecological" in a loose sense) of "do people use these things?" When AI writes fiction online, how many downloads does it get? How often do people choose AI therapy over human therapy, or whatever? I don't know. My wish for the world is that we had better ways of tracking this sort of thing. But it does rely on people accurately being able to assess how much AIs are actually helping them in these domains by their use patterns, which... [In] another METR work [measuring open source software developers, seeing if they're good at estimating how much AI helped them](https://arxiv.org/abs/2507.09089), the answer was they were bad at estimating [that]. So maybe people are using AI all over the place and it's not actually helping them. But it does seem like one way of addressing some of these concerns.

**David Rein** (00:30:39):
Yeah, totally. I'm super interested in this sort of thing. There was recently... The Anthropic Societal Impacts team... I think didn't quite get as far as measuring number of downloads or something, [but] [they did some work recently](https://www.anthropic.com/research/anthropic-economic-index-september-2025-report), I haven't looked at it closely, breaking down into really fine-grained categories what Claude usage looks like. I think these probably would be pretty correlated. If there's a strong market demand for a certain kind of AI output, I think you would expect to see that show up in your Claude usage data, to some extent at least.

**Daniel Filan** (00:31:30):
Right, right. Yeah, fair enough. So we were talking about why software engineering, and there are three parts to the answer. Firstly, it's related to some threat models that METR cares about. [Secondly], it's also easier to measure. Wait, I think there was a third thing in between those that I forgot.

**David Rein** (00:31:55):
Yeah, the third one is... I think this is the sketchiest of these, I think those are probably the two biggest ones. The third one is something about AI developers, they're aiming for this. And this has this trade-off that I talked about in terms of generalization.

## Why task length as difficulty measure <a name="why-task-length-difficulty-measure"></a>

**Daniel Filan** (00:32:17):
Got it. So I think the next thing that I want to talk about is: one interesting thing about what you're doing is you're basically saying, "Okay, we want to know how AI succeeds at tasks of various difficulties." And if I had never seen this paper, I could imagine having a whole bunch of measures of difficulty. I could use a human rating of "on a scale of 1 to 10, how hard is this?" or "how many years of education do you need for this?" or "when people try it, what's the probability that they succeed?" or if there's some competition between AI agents or whatever, you can look at the [Elo](https://en.wikipedia.org/wiki/Elo_rating_system) of it. That only works in some domains. Go is a really good one for that, for example. And one thing that you do in fact look at it in the paper is the intuitive "messiness" of a task. How clean and simple is it versus how tricky and rough is it?

(00:33:20):
And the thing you end up finding is this really nice relationship with time it takes for humans to do it, where it seems like both you have a decently good relationship within a model where things that take longer for humans to do, success rate at these tasks is lower; and also across time, there's this nice trend for this. I'm wondering: is this just the first thing that you tried and it seemed like it worked well, or do you have a really good sense of, "No, we checked and these other metrics just don't have as good relationships in a way that's nice and predictive?"

**David Rein** (00:34:03):
So we've definitely done some of this. I think there's a vision of "we've tried all of the things and this is the one", and we definitely haven't done that. Maybe it'd be useful in particular to talk about the specific alternatives. I think for at least a couple of them, maybe the first two you mentioned - "how difficult do people rate these tasks?" or "when people attempt the task, what is the probability of them succeeding?", I think both of these are closer to the standard benchmarking paradigm.

(00:34:49):
And so those metrics, I would expect to correlate more or be more connected to this intuitive notion people have about "how much expertise does a task require?", which I think is already covered by other benchmarks. That's not to say though that we couldn't still use it as this metric, or maybe we would see a robust trend. But... That's interesting. I think it'd be difficult to operationalize these in a way that makes sense. So for success probability, what is the exact actual distribution of people that you are having attempt these tasks? That becomes very load-bearing.

**Daniel Filan** (00:35:42):
It seems like it's similarly load-bearing for success probability as for time horizon, right?

**David Rein** (00:35:48):
I'm not so sure. One of the reasons why we filter our baselines to only ones that succeed is: success on a task is in fact a lot more information than failure on a task. There are a bunch of reasons why you might fail a task that aren't actually a lot of information about how difficult [it is] or how much agency is required or whatever. So for example, maybe we just got their expertise wrong. We're doing this job of manually assigning people to tasks that we think that they have a lot of relevant expertise for, and maybe someone just happened to not ever use this particular tool or set of tools that are super important for this task. And then their failure on that task, it's still some information. But if they succeed on the task, then that is just this very objective thing like "yes, someone can complete this task in this amount of time."

(00:37:03):
There are infrastructure reasons why people fail tasks. Also, there are incentive reasons. So when you have people try and complete tasks, sometimes they'll get bored and they'll want to stop. Sometimes they'll be like, "Ah, this is too hard, I don't want to keep doing this." Incentives can be tricky to set up well in different cases. So one situation you can have is where people quit tasks early because they want to maximize the chances of getting more tasks that they succeed on. Typically, we pay bonuses for success because we want to incentivize people to succeed. But there's a perverse incentive there. And so broadly, we just have a lot more uncertainty about failures, I think, than we do about successes. That's not to say that we couldn't do something like this. I definitely can't say it's impossible, but I think it's more challenging. This is one particular thing. I think I'm probably not getting at the broader...

**Daniel Filan** (00:38:15):
Maybe one way to get at the same question is: so you find this pretty good relationship between time to complete task among humans who are relevant experts who in fact managed to complete the task, and (a) AI probability at succeeding at the task, and (b) trends over time in time horizons that models can do at a 50% or 80% success rate. But it's not perfect. And one thing you mention in the paper that for some reason seems to have gotten less memetic... people seem to talk about it less, is: you have this metric of messiness of various tasks. And you end up saying, "Okay, there is something to this messiness thing that somehow seems to predict task success over and beyond human time horizon." So one question to ask is: if I had to choose between just human time horizon and just these messiness ratings, which one would do better? And maybe the next question is: if both of them are independently predictive, what does that say about the ultimate metric we really should be using?

**David Rein** (00:39:36):
Yeah. So I think we are broadly really interested in trying to explain as much of the variance in models' successes and failures as we can. And you're totally right that the length of task for humans is one metric that explains a decent amount of this variance, but there are definitely other things that are going on. So we're actually currently trying to figure out what are other properties of tasks that explain their success and failure well. And yeah, I think we would love to have something like this.

(00:40:27):
For something like messiness... For a lot of these other kinds of metrics that you can think of, to me, the biggest issue that I see, or the biggest challenge, is just some kind of thing of subjectivity. So people have very different senses of what is a messy versus clean task, and depending on your priors about... So one example is, I have a colleague -- I think it's fine for me to talk about this -- he basically would not rate any of our tasks as being messy at all because they have algorithmic scoring functions, for example. So the success or failure is defined by this small surface area or something. And the tasks tell you what to do, for example. In the real world, a lot of the challenge is figuring out what the hell you should do in the first place. So I think that's a challenge.

(00:41:42):
But especially with -- you mentioned [this randomized control trial that we ran recently of developer productivity](https://arxiv.org/abs/2507.09089) where we saw that developers, at least when we measured this, were not sped up by AI systems, and trying to understand what the gap between benchmark scores and some of these more ecologically valid experiments... what that gap is or what explains that gap, I think we're super interested in.

**Daniel Filan** (00:42:25):
So actually, speaking of the relationship between how good things are at predicting success: so one thing that you also do is you look at the correlation between models, of if model A succeeds at the task, how does that predict whether model B succeeds at the task as well? So this is one of these fun diagrams that you have in the appendices. And it's possible that you just don't know the answer to this question, but one thing I noticed when looking at these diagrams of correlations is there's this block of GPT-4 and beyond models that seem much more correlated with each other on what tasks they can succeed and fail on than pre-GPT-4 models. What's going on there? Is it that they've standardized on training sets? Is everyone after GPT-4 trying to train their models to do software engineering and that's what's going on? Yeah, tell me about that if you can.

**David Rein** (00:43:21):
I don't think I actually know the answer to this. I can speculate. I'm actually not certain that this isn't an artifact of our particular setup. So one thing you brought up is: if you put GPT-2 in the same agent scaffold -- so for recent models, we have them in this loop where they see some instructions and the state of their environment and then they think about and consider what actions to take, and then they take an action and use some tools and continue -- if you put GPT-2 in this loop, it just totally, totally flops. And so basically, you can't really make a perfectly direct comparison, you do actually have to use a different methodology. I'm not certain that this block in the correlations isn't because of some difference in our agent scaffolding, for example. It's a really good question. I would be curious to know. I actually don't know if we know. There's probably been some discussion about it, but I would need to check.

**Daniel Filan** (00:44:51):
Another thing that just occurred to me with the alternative difficulty measures: I have a colleague of mine back when I was at [CHAI](https://humancompatible.ai/) called [Cassidy Laidlaw](https://cassidylaidlaw.com/) who has [a paper, I forget what the name of the paper is, it's going to be in the description and I'll send it to you afterwards](https://arxiv.org/abs/2304.09853), where basically the thesis is: if you want to know whether deep reinforcement learning works on an environment or not, if you're familiar with reinforcement learning algorithms... One idealized reinforcement learning algorithm you can do [is], you can start off with a random policy, and then you can do iteration where [it's] like, "Okay, what would be the best action for me to take given that from this point onwards, I'm just going to act randomly?"

(00:45:37):
And then, "Okay, what would be the best action for me to take given that from this point onwards, I'm going to do the thing that would be best given that from that point onwards, I would act randomly?" et cetera. And I think basically a very good predictor of how well deep reinforcement learning works on various environments is just: how many steps of that do you actually have to do? If I recall this paper correctly -- people can read it in the appendices.

**David Rein** (00:46:00):
Interesting.

**Daniel Filan** (00:46:01):
And I feel like one nice thing about this is that [although] it doesn't get to the aspects of messiness that are vagueness or whatever, because this is just reinforcement learning where you have a defined reward function, it does get to some of the agent-y, "how much do things depend on things?"

**David Rein** (00:46:22):
Yeah, like how fragile... Interesting.

**Daniel Filan** (00:46:28):
Embarrassingly, I remember very, very little about this paper. But people should read it.

## Is AI progress going superexponential? <a name="ai-progress-superexponential"></a>

**Daniel Filan** (00:46:32):
So I think the last thing I want to ask about, digging deep into the time horizon stuff (at least for now): one thing that readers notice when looking at this is there's basically this line on a log plot of year and time horizon. And models are basically lining up along this line. But then it starts looking like once you get reasoning models, they start bending up a little bit, they're a little bit above the line. So "[Measuring] [AI Ability to Complete Long Tasks](https://arxiv.org/abs/2503.14499)": I believe that was released in February or March of this year.

**David Rein** (00:47:14):
Yeah, March.

**Daniel Filan** (00:47:15):
Pretty early, when we had not as many data points. We've gotten a few more data points. And early on, there was some speculation of, okay, are we going superexponential or not? With more hindsight: are we going superexponential?

**David Rein** (00:47:32):
Yeah, great question. I would love to know the answer to that. I think we still don't really know. [There are a] couple of things to say at least. So one is since we released the paper in March... One thing that'd be useful to just point out for listeners is that this plot, where we measure the trend of improvement over time, we're only using the best model at a given time. And so that's just relevant because there are a lot of other models that have different trade-offs, or maybe they have faster inference, but they're weaker. And we're just using the models that perform the best.

(00:48:24):
Anyways, since March, frontier models... So one thing we look at in the paper is, we noticed... Actually this is useful to talk about because I think the timeline of how the paper came together is useful. So we actually initially only fit the trend on models from, I think basically 2024 onwards. So I think the first version of the graph was made by Ben West in December 2024, if my memory is right. And I think this was just using that year's models. And with those models, we actually observed this four-month doubling time in the time horizon. And then we were like, "well, does this trend extend backwards?" And so in the paper, we also do these backcasts from this. So then we added in previous models.

(00:49:42):
All that's to say that, to some extent from the start, we have seen these two trends, essentially. I think this is all kind of, I don't know, BS or something. If you have 10 data points or 15 data points and you're fitting piecewise linear functions, it's pretty sketchy. So I definitely don't want to over-claim, but it does seem like this four-month doubling time trend from 2024 onwards has continued to hold or has been a much better predictor than this seven-month doubling time that is suggested by the models going back to 2019. So I think my best guess that's very low confidence is something like we're just on this four-month trend now, but it's still just exponential. It is really hard to distinguish between different kinds of model fits to some extent.

## Is AI progress due to increased cost to run models? <a name="progress-increased-cost"></a>

**Daniel Filan** (00:50:58):
So actually, the thing about different models made me wonder: so if we're saying that time horizon is going up over time: suppose I want to project that into the future. It's one thing if this is true at basically fixed cost; it's another thing if it's always the case that a one-minute task costs $1, a two-minute task costs $2, a four-minute task costs $4, and then maybe we get models that can technically do things that a human could do in a month, but it would be cheaper to just get the human to do it for a month. Off the top of your head, do you happen to know what the picture looks like with cost?

**David Rein** (00:51:49):
Yeah, that's a great question. This is something we try and keep an eye on. Let's see. So for recent models, our agent scaffold has a token limit that we tell models about so that they're aware of this. But I think we've been using a token limit of something like 8 million tokens, which I think for these longer tasks, ends up being at least one order of magnitude cheaper than paying a human with relevant expertise to complete the task.

**Daniel Filan** (00:52:31):
And to give a feel for that, 8 million tokens is something like six bibles of texts, roughly.

**David Rein** (00:52:37):
Yeah, yeah, it's quite a lot. You can do much better than that with caching. Most APIs let you do prefix caching and that helps quite a bit, so you should count it differently, I think.

**Daniel Filan** (00:52:54):
But it's like a big chunk, basically.

**David Rein** (00:52:56):
It's a big chunk. Models will do lots of reasoning and run a bunch of different experiments on these longer tasks. They'll take something like 10 to 50 actions or something in the environment. But then for each action, they're doing a bunch of reasoning. And it depends on the exact agent scaffold, but in many of them, we have models that propose actions and then review them and then select the best one. So there's a lot going on, and this is still much cheaper than having people do it. I wish I knew the exact numbers on cost. It is more complicated because of caching.

(00:53:56):
So currently this isn't the biggest concern of ours because of this, basically; where models still are just highly cost-competitive. I totally imagine this changing at some point. [Because of] trends in models being able to use test-time compute more effectively, I totally expect for very long tasks to get expensive and [I expect] it to be very important to be measuring the Pareto frontier of cost and success rate or something. And I think we're excited to do more work on this as it becomes more relevant.

## Why METR measures model capabilities <a name="why-metr-measures-model-capabilities"></a>

**Daniel Filan** (00:54:45):
Yeah, fair enough. So zooming out: [Model Evaluation and Threat Research](https://metr.org/)... I think of METR as trying to figure out how scary models are. And if they're scary enough, then I don't know, maybe we should do something. So this work of measuring general software engineering capabilities and trying to forecast them over time: what's the rationale behind this? Why focus on this?

**David Rein** (00:55:19):
So I think broadly, the threat model that METR is most concerned about, at least at the moment, is rapid acceleration in AI capabilities, and in fact, the rate of progress of AI capabilities due to AI systems being able to contribute substantially, or contribute the majority of, AI progress at some point in the future. So the idea is: currently, the way you make AI systems better is through a combination of compute, hardware, resources, money, data and talent, labor. If it becomes the case that AI systems can replace the labor, the talent part of this, in economic models of progress, in at least some of them -- I think broadly they're reasonable, although I'm not an economist -- you can see very, very rapid progress, and basically this just seems broadly kind of scary.

(00:56:42):
So one example is you might see very rapid centralization of power in a single organization that does this recursive self-improvement, and that's concerning for general stability, geopolitical, democracy kind of reasons. And then also, your arguments for why the AI system itself is not going to be dangerous, those might break down. So you might not be able to evaluate it effectively because, for example, the system may have a really good understanding of exactly how you're evaluating it and if its goals are different from yours, then it might be very easy for it to game your evaluations, your supervision methods might break down. You're reading its chains of thought, for example, and the model is saying things that seem very safe and nice and reasonable, but actually it's doing some kind of hidden reasoning in the background that you can't detect and you didn't realize that this was about to happen because progress was so fast and because as a lab you were just scrambling to get as much compute and make as much progress as you can, as quickly as you can.

(00:58:16):
And so broadly this is, I think, one of the big concerns or questions that we want to understand: how close are we to this rapid acceleration? Is that even possible? As I said, labor is not the only input to AI progress. You also have compute, for example, and data, and these things might be highly complementary to labor such that even if the amount of talent increases by several orders of magnitude, because you have all your AI researchers doing this work, you might end up still very bottlenecked by compute and data. And so trying to get some understanding of that... We think about this to some extent, these economic models. I think this isn't our chief forte. [Epoch AI](https://epoch.ai/) has [a bunch of](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4814445) [great work](https://arxiv.org/abs/2503.04941) doing some of this modeling also. Folks at, I think the org is called [Forethought](https://www.forethought.org/), [Will MacAskill](https://www.williammacaskill.com/) and [Tom Davidson](https://tomdavidson-ai.github.io/) have [done](https://coefficientgiving.org/research/what-a-compute-centric-framework-says-about-takeoff-speeds/) [work](https://www.forethought.org/research/will-ai-r-and-d-automation-cause-a-software-intelligence-explosion) on this kind economic modeling.

(00:59:36):
Anyways, understanding how capable AI systems are is a big input to this. And software engineering and ML research capabilities are highly relevant.

**Daniel Filan** (00:59:49):
And how much is the desire... So one thing you could do with this is you could say: okay, are we there or are we about to be there? And that's the point of doing the measurements. Another thing could do is you could be trying to say, okay, are we going to get there in 2030 or are we going to get there in 2050 based on what we know now? So how much is the thing you're trying to do a forecast versus a nowcast?

**David Rein** (01:00:19):
Yeah, that's a great question. I think we would love to be able to do really good forecasts. Unfortunately, I think it's really, really hard. So for example, as we talked a little bit about, new paradigms in AI might change the trends that we observe. Also, there are lots of inputs to these trends that might not be durable. So for example, we're seeing the time horizon of AI systems is increasing exponentially; but also, the amount of money and the amount of compute being put into training AI systems maybe has also been increasing exponentially. I actually don't know the exact details of how compute spend has been increasing, but-

**Daniel Filan** (01:01:10):
I think it's exponential. I feel like if I go to Epoch AI, they're going to show me [some nice graph](https://epoch.ai/data/ai-models?yAxis=Training+compute+cost+%282023+USD%29#explore-the-data) and it's going to be like...

**David Rein** (01:01:17):
Yeah, yeah. And so maybe that's just the cause, and in fact we're just going to hit some bigger bottlenecks in the economy more broadly. It's just not going to be possible to fund increasingly large data centers. Kind of an interesting point is: I basically view this time horizon trend that we're seeing as something closer to an economic model than an ML benchmark model or something. Where I'm like: the actual inputs to this progress are firms that are competing to train increasingly better models, and they're putting these resources in and they have these constraints and whatever.

(01:02:08):
And actually, for me at least, one of the big updates is, I think I am much more interested in economics as a result of seeing this really robust trend. Because I was actually extremely skeptical of putting time on the x-axis in particular. I was like, the inputs are just going to be these random decisions by different labs and there's no way we're going to see some robust trend, because it just depends on who [Jensen](https://en.wikipedia.org/wiki/Jensen_Huang) [Huang] happens to like or whatever.

**Daniel Filan** (01:02:51):
Jensen Huang being the CEO of [Nvidia](https://en.wikipedia.org/wiki/Nvidia), right?

**David Rein** (01:02:53):
Yeah. Yeah. For different compute deals or something. And I was like, no way that could be robust. So that was a decent update for me: maybe these kinds of extremely abstract economic models actually can be very, informative, or maybe there is this deeper systematicity to AI progress, even though zoomed in it feels very contingent and kind of arbitrary. I don't know. This is all very much speculation or just my musings on this.

(01:03:30):
I think as an org, we are definitely interested in forecasting. I think there are trade-offs between doing this more abstract modeling and just focusing on... We do a lot of work on this nowcasting kind of thing. Just "currently, how good our AI systems?" is kind of an open question. There is a lot of disagreement about this. Even internally at METR, we have disagreement about this. Probably there isn't one single answer, 'cause it's just a complicated question. But I think we're trying to do both to some extent.

## How time horizons relate to recursive self-improvement <a name="time-horizons-rsi"></a>

**Daniel Filan** (01:04:10):
Fair enough. So for either forecasting or nowcasting: suppose I want to use the time horizons work or the nearest successor to tell me when [we're] going to get this "AIs feeding into AI progress": how am I going to use the results of, "oh, it's three months"? Are we at recursive takeoff?

**David Rein** (01:04:40):
Yeah. I think this is kind of an open question, or I don't think we have nearly as good of an answer here yet as we want. We have heuristics, I think; [at] one week of work -- time horizons of 40 hours -- I think we definitely are getting a lot more concerned, or it seems at least plausible that you could successfully or efficiently delegate weeks worth of work to AI systems, and I could totally imagine that speeding up AI progress quite a bit. Same for time horizons that are much longer, but I think we don't really know, is my answer.

(01:05:42):
Part of my uncertainty is... [the idea that] a week or a few weeks of work as a time horizon is very useful as a rough heuristic or threshold, I think I would've been more confident in that maybe before [this productivity RCT](https://arxiv.org/abs/2507.09089) where we found that people were very miscalibrated on how much AI systems sped them up, open source software developers in particular. And in fact, we saw that they were slowed down on average by 20%. I think the time horizons work and these randomized controlled trial results, I think they're probably not as in conflict as they might seem at face value, for reasons that we could talk about, but they definitely did update me more towards broader uncertainty about this interaction between AI systems and people. And maybe we do end up really bottlenecked by things like our ability to specify tasks really clearly, or maybe things like the fact that we're algorithmically scoring models, we might be overestimating their capabilities because of that to some extent.

**Daniel Filan** (01:07:10):
Actually, in terms of other bottlenecks, I'm really interested in talking about that. Because if we're interested in... Suppose I want to know at what point do we get this runaway process or whatever, it really matters whether AI is automating... Suppose there are five things you need to be good at to do recursive self-improvement: the difference between AI being able to do four of those and AI being able to do five of those is huge. Right?

(01:07:40):
I think one concern I might have about the METR benchmark stuff - or about [this particular paper](https://arxiv.org/abs/2503.14499) - is just: is it covering all the bases, or is it covering some of the bases, kind of? Just because potentially that could really reduce its value for this particular thing. I'm wondering, do you have thoughts about that?

**David Rein** (01:08:09):
I think that's a pretty legit concern. I guess I would be interested in... There's this question of, well, what are the specific things that are bottlenecking and how different are they from the things that we're measuring? So one kind of broad reply could be something like, well, to the extent that our benchmark is just a bunch of kind of different, diverse tasks, hopefully it's the case that we're kind of covering some decent amount of the space of necessary skills or capabilities, such that we would expect results to be very correlated on things that we're not measuring specifically. And we can maybe get some kind of sense of this by looking at the variance of model performance on our tasks.

**Daniel Filan** (01:09:10):
I guess one thing you could presumably do is just have a held-out 20% set and just see, does performance on the non-held-out set predict performance on the held-out set? I guess that's probably in some appendix somewhere.

**David Rein** (01:09:25):
I think the thing you would want to be doing there is you would want the held-out set to be importantly different in some kind of biased or systematic way. And I think that would be interesting. Currently, we haven't done this. To some extent, maybe the messiness analysis is trying to get at something like this. Are there other factors that explain model capabilities? It seems like, kind of.

**Daniel Filan** (01:09:58):
Yeah, I guess there's also this [blog post](https://metr.org/blog/2025-07-14-how-does-time-horizon-vary-across-domains/) METR put out basically trying to do a similar analysis for other domains. So there's a little curve for self-driving and there's curves for... I forget exactly what all the other tasks were. So my recollection of that is that it seemed like in each domain you maybe had some sort of exponential increase in time horizons, but best fit doubling times were different in different domains.

**David Rein** (01:10:27):
Yeah. My broad takeaway from this work that Thomas Kwa led was that in decently similar domains -- so, question-answering benchmarks, for example; GPQA was one of the benchmarks, and there were a few others -- I think we saw quite similar doubling times overall, is my memory. And actually even overall pretty similar absolute time horizons, which was some amount of validation. The challenge with this kind of work is: we put a lot of time into estimating the lengths of our tasks, and so we're using these scrappier, more heuristic or less precise estimates of task length for most of these other domains. And then I think self-driving did have a slower doubling time, but I don't think it was clearly not exponential.

(01:11:43):
And then, the other interesting takeaway I had from that was with respect to more general computer use. So there's this benchmark [OSWorld](https://arxiv.org/abs/2404.07972) that has a bunch of, you have a browser and you need to do these tasks or you're in this operating system and you have to click around and manipulate normal software. The key difference between this and a lot of our tasks is that our tasks are almost entirely text-only. Models are weaker relatively at multimodal tasks it seems. So I think for those domains, I think they had a kind of similar doubling time, but the absolute time horizons were much, much lower. I think it was a couple minutes or something, which I thought was interesting, and I'm actually kind of confused about broadly; I don't really understand what's going on there.

## Cost of estimating time horizons <a name="cost-estimating-time-horizons"></a>

**Daniel Filan** (01:12:58):
With all that said about the pros and cons of this sort of framework for tracking "are we getting close to some sort of self-improvement cycle?", I'm wondering: what's your guess about whether, let's say one or two years from now, we're still thinking that something basically like time horizon is the metric that we're tracking, or we end up saying, "oh, there's something pretty different and that's the real thing"?

**David Rein** (01:13:31):
Yeah, yeah, that's a great question. I think to me, a lot of this comes down to the tractability of continuing to use this metric and estimate it. I think this is somewhat unclear. So for example, we paid a lot of people money for their time to work on these tasks so we can estimate how long they take. If the length of these tasks becomes... they're weeks- or months-long tasks, this gets pretty expensive.

**Daniel Filan** (01:14:19):
Actually, how expensive was it to make this paper?

**David Rein** (01:14:22):
That's a great question. It's kind of tricky because there were these different efforts going on. So we included the [RE-Bench](https://arxiv.org/abs/2411.15114) tasks and the baselines for these tasks, and that was a separate project. So it maybe depends on if you count that. I think that the baselines for the main set of tasks that we used, the [HCAST](https://arxiv.org/abs/2503.17354) tasks, I want to say that these were somewhere in the range total of at least tens of thousands, possibly low hundreds of thousands of dollars, something in that range. I probably should know this off the top of my head more accurately, but yeah.

**Daniel Filan** (01:15:15):
Yeah. But it sounds like it's reaching a stage where measuring these time horizons is getting close to the dominant cost of actually doing this work. It's probably lower than the salary cost of, you've got a bunch of people working on it, but if it were to become more of a thing.

**David Rein** (01:15:36):
At some point, I think this does start to dominate. Although, I would say that I think currently actually creating the tasks is the most expensive and difficult part. So either creating them from scratch or trying to find good tasks in the wild, as it were, which is nice because (a) they already exist (to some extent, although you have to kind of port them over into your framework), but also that gives you more confidence that they're realistic and representative of real work that people are doing, which is important when we don't fully understand exactly when and why AI systems succeed or fail.

## Task realism vs mimicking important task features <a name="task-realism-vs-mimicking-important-features"></a>

**Daniel Filan** (01:16:23):
Actually, maybe this is worth talking about a bit. I think there's one kind of approach to measuring AI systems which says: look, we need to isolate things. We need to get down to the simplest feasible task where we can really measure exactly what's going into it. And these end up being things... If you think of [ARC-AGI](https://arcprize.org/arc-agi), it's not quite this, but it's something sort of like this. Versus a sense of, no, we need to create things that have this realness flavor, even if they're not... Finding an MD5 hash collision, on some micro-level, it's not very similar to doing AI research. Right?

**David Rein** (01:17:13):
Yeah.

**Daniel Filan** (01:17:13):
Could you say a bit about how important it is to be thinking about economic usefulness versus trying to mimic a sense of what the tasks you care about are?

**David Rein** (01:17:28):
Yeah. I think that there is a very real trade-off here between the level of granularity of your understanding, where if you maximize that, you often end up with these very simple, formulaic, systematic benchmarks that are just probing some very particular kind of skill in a systematic way. And then on the other end, you have this realism maximization lens. So I think the best popular example of this maybe is [SWE-bench](https://arxiv.org/abs/2310.06770) or SWE-bench Verified where these are actual GitHub issues and PRs and tests that you're measuring AI systems against. I think there's a real trade-off here where on one end, you get this granular understanding, and then on the other, it's really easy to interpret what a certain success or failure means. It's like, okay, yes, it can do this thing in the real world that I understand, I have some intuitions about. So I think there's a real trade-off.

(01:18:51):
What do I think here? I think it's really hard. I mean, broadly, I feel pretty pessimistic about this kind of granular approach. I think maybe this has something to do with the amount of systematicity in neural networks themselves or something where it's like: well, they are just kind of inconsistent, but are still capable of really impressive things often. And so maybe you just can't get this extremely crisp understanding and you just have to aggregate or look more broadly at things that actually are relevant for your decisions about whether to deploy a system or how safe it is or whatever. I think that's probably the direction I lean in.

## Excursus on "Inventing Temperature" <a name="excursus-on-it"></a>

**Daniel Filan** (01:19:50):
I also wonder if there's something along the lines of: often these sort of high-level things... So take something like economic growth: it's an aggregate of a bunch of things a bunch of people are doing. It's not very well-isolated, and also it's relatively smooth and predictable; not totally, but it's pretty smooth. Time horizon, you might not have thought that it would be this nice trend, but it is. OK I'm going to tell you about a book that I'm reading: part of the reason this is on my head is that I'm reading this book, [Inventing Temperature](https://global.oup.com/academic/product/inventing-temperature-9780195337389), which-

**David Rein** (01:20:26):
Yeah, yeah, yeah.

**Daniel Filan** (01:20:27):
Yeah, it's very popular in these LessWrong spheres, and I'm finally getting around to it.

**David Rein** (01:20:31):
I haven't read it yet, but I've heard lots of great things about it.

**Daniel Filan** (01:20:34):
Well, it's great. I'm going to spoil it a little bit. So the first chapter is basically about the problem of: so basically you want to have a thermometer. Suppose you want to standardize a temperature scale that all these thermometers use. In order to do that, you've got to find some phenomenon that's always the same temperature, but that's repeatable that a bunch of different people can use. So firstly, there's a bit of a weird circular thing where you have to know that a phenomenon always has the same temperature before you have a thermometer, right? Which, okay, maybe you can use the same thermometer and do it multiple times, and you just trust that the volume of the mercury or whatever is a good proxy for the thing you want to talk about as temperature. So one funny thing is initially, people were just really wrong about what could possibly work for this. You have people saying, "what if we just do the hottest it gets in summer? Or how cold it is underground?"

**David Rein** (01:21:34):
Wow, yeah. Oh, that's great. That's so good. Oh my God, I love it.

**Daniel Filan** (01:21:37):
It doesn't quite work. But eventually people are like, oh, we're going to use boiling water. Now firstly, we now know that the temperature that water boils at depends on the atmospheric pressure, right? Well, luckily they knew that as well, so they were able to control for that.

**David Rein** (01:21:55):
How did they know that? Does the book talk about that?

**Daniel Filan** (01:21:57):
I don't know. I've only read most of one chapter or something. But I think you can do a thing where... Especially if you're looking at temperature as a proxy for volume of a liquid thing, and a lot of your thermodynamic knowledge comes from stuff like brewing or engines or something, you end up in these situations where you have things at different pressures and different volumes, and I think that's the kind of thing that you can figure out, especially if you have this identification of temperature with volume of a thing under fixed pressure and fixed conditions or whatever. So it's like, okay, boiling water, right? Do you cook pasta?

**David Rein** (01:22:48):
Sometimes, yeah.

**Daniel Filan** (01:22:49):
So one thing you'll notice is that first bubbles start appearing, and then you start getting a bit of a boil, and then you start getting a rolling boil. And the temperature of the water is different at different points of this, and also the temperature of different bits of the water is different at different points of this. So what are we talking about when we're talking about boiling temperature? And if you look at the cover of the book, it's this picture of an early thermometer that has one line for mild boiling and one line for, it's really solidly... "boiling vehemently", I think it says. And these are different temperatures, right?

(01:23:23):
So there's this one scientist who does this approach of like, okay, what are we talking about about boiling water? He has this theory that one thing that happens with "fake boiling" is that water has little bits of air in it, and those little tiny, tiny air bubbles, you start getting evaporation into that air bubble, and then that air bubble gets hot, rises up, and you start seeing vapor, but that's not true boiling of the water. That's only there because there's these interior air bubbles. And so he starts going down this line of work of, okay, let me isolate out all of the random little things, right? We're going to have as smooth as possible a surface as I can. We're going to get rid of all the air bubbles. And basically, the thing he discovers is superheating, where it turns out you can get water way above 100 degrees Celsius before it actually boils.

(01:24:21):
Basically, the thing they end up doing is... The answer turns out to be that water vapor is at a very consistent temperature, even when the temperature of the water is not a very consistent temperature. But the reason that's true is precisely because there's a bunch of dust in the air. There's little things that things can nucleate around and that stops vapor from getting too hot or too cold before condensing. And in fact there's... Have you heard of cloud chambers?

**David Rein** (01:24:56):
No.

**Daniel Filan** (01:24:57):
They're used in particle physics, and basically they have this supercooled vapor, so it's vapor that is under 100 degrees Celsius that is ready to condense, but doesn't have a thing to nucleate around. But if you shoot a particle in it, it condenses around that so you can see the trail.

(01:25:16):
In thermodynamics, there's this general thing where if there's a bunch of random messy stuff, that produces a bunch of observable regularities of a somewhat higher level... We have this in thermodynamics. It seems like we kind of have this in economic growth, and part of me wonders if that's kind of what's going on in how we should understand neural network capabilities. Or maybe I just read a book and I liked it.

## Return to task realism discussion <a name="return-task-realism"></a>

**David Rein** (01:25:46):
No, I love this. I think this general idea is super interesting. Another model you could have for how AI systems are performing on tasks is: you could imagine that there's something like a constant failure rate that AI systems have as they're attempting tasks. Different tasks might have different failure rates, and so that complicates things.

**Daniel Filan** (01:26:28):
And by failure rate, do you mean per time a human takes to do it?

**David Rein** (01:26:32):
Something like that, yeah, exactly. [Toby Ord](https://en.wikipedia.org/wiki/Toby_Ord) actually did [some analysis](https://www.tobyord.com/writing/half-life), or some follow-up work on the time horizon paper, where: if you assume this constant hazard rate -- per time that people spend, there's some percentage chance that the AI system is going to make some kind of catastrophic error and then ultimately not succeed at the task -- then this also is a good predictor of AI system success and failure on our tasks as a function of the length of task for humans. In our paper, we used a logistic fit, but assuming a constant hazard rate, you would use an exponential fit.

**Daniel Filan** (01:27:21):
I do think that [Lawrence Chan](https://chanlawrence.me/) had [a response](https://nitter.net/justanotherlaw/status/1920254586771710009) to that which said that logistic fit was in fact better, even though it used more parameters or something. I remember a response along those lines.

**David Rein** (01:27:31):
Totally. So we did explore different fits and logistic was a better fit. I think because of this aggregation of maybe different distributions of tasks, I don't think it's obvious how much we should weight the exact quality of the fit versus priors on simplicity or "this is a nice model" maybe. I don't know how much to weight that. But I think stuff like this to me is very interesting in terms of understanding capabilities. I've really often felt like getting at something more like the intrinsic number of actions needed to complete a task feels to me intuitive. And I think other folks I've talked to... It feels like a really nice kind of thing that could be useful for understanding this. You can imagine it slotting well with this constant hazard rate model where it's like, for each action that you need to take or something... But actually operationalizing this, I think has been tricky. We've done some analysis of this and it's been difficult to extract really good insights.

**Daniel Filan** (01:29:10):
I think we're currently on a tangent from a question I was asking a bit ago -- I think I took us on a tangent -- which is: two years from now, do you think we're still using something like time horizon? So one big response you had is, well, will we be able to? Will it just be infeasible to actually measure these time horizons? Setting that consideration aside, I'm wondering if you have a sense of, this is probably just the thing that's going to continue to be more robust, or probably we're going to come up with a "number of actions" model, or something that incorporates the messiness results, or something like that.

**David Rein** (01:29:54):
I think my best guess is... Assuming we're able to continue estimating it in a way that we feel confident in, I think my best guess is that we'll use it with different weightings or multiples or something, based on some of these other factors. I think I've become more pessimistic about figuring out things like number of actions. That's not to say... I mean, I would be super excited about that and I think there's a decent chance I'll take another stab at it at some point.

**Daniel Filan** (01:30:47):
Suppose we think that economic relevance, trying to mimic real-world utility is just the thing. One thing you could imagine doing is: we're just going to figure out what the market rate is to get someone to solve this task, which is a mixture of expertise and time taken. Do you have a sense of whether that would end up being a better predictor?

**David Rein** (01:31:11):
Yeah, it's a great question. I think we have looked at this or tried to estimate this by clustering our tasks... I shouldn't speak too much to the details because I can't remember exactly what we did, but something like [this] -- just look at, these tasks are really hard ML tasks, and so they're going to be more expensive, and these other ones are cheaper. And there's some trade-off. I think something like that could be reasonable. A reason why you might not expect that to work is that AI systems broadly have a different capability profile than people. So if it was, I don't know, 1920 or something... Or actually, let's say 1950 or '40, maybe right before we had calculators: if you were doing this math of, how long does it take to pay human computers to calculate the product of 10-digit numbers? That you need to do for whatever reason. You'd be like, "Yeah, that's an extremely hard task. Machines are not going to be able to do that task for such a long time." But in fact, pretty quickly after, computers were able to do this very well.

(01:32:55):
And so applying this to modern systems, and I do basically believe this actually: AI systems are way, way better at tasks that seem to require humans many years of intellectual development and labor to complete. They can do GPQA questions, they can do IMO problems, these sorts of things. And so I think I do view this as less of the bottleneck, basically, and I think I do view something more akin to agency... Which might point to messiness factors, or... That's not to say that there aren't other metrics. Maybe this is just an argument against human expertise or something.

## Open questions on time horizons <a name="open-qs-time-horizons"></a>

**Daniel Filan** (01:33:52):
Fair enough. I guess with that said, we've got [the time horizon stuff](https://arxiv.org/abs/2503.14499), we have [HCAST](https://arxiv.org/abs/2503.17354). I'm wondering: to you, what are the open questions and what kinds of things might I see out of METR in the next year or so, pushing this research direction forward?

**David Rein** (01:34:15):
Yeah, great question. Broadly, I think there are a few things. One is continuing to use this methodology. So currently models have 50% success rates on these two-hour tasks. GPT-5 I think is two hours and 15 minutes or something time horizon. And if we really are on this four-month doubling time trend, we're at four hours by the end of the year, eight hours spring of next year, 16 hours fall next year. That's not that long. We have fewer longer tasks, and we have fewer baselines on these longer tasks because they're more difficult to baseline. You have to find people with more specialized expertise and they're more expensive and people fail more often. And so extending our task suite and trying to just see "does this trend continue?" is one big direction.

(01:35:24):
I think there are open questions around how do we actually affordably continue doing this? Are we harvesting tasks from existing work that people have already done? Are we creating new tasks and then using LLM evaluation or more manual review to evaluate success on them? Are we doing other things? So things in that direction, that's one class of things: trying to continue this basic methodology.

(01:36:03):
I think there's another class of directions that we're pretty excited about, which is something more like... What I just described is something like benchmark development and then evaluating models on these tasks. But then there are a bunch of these questions around, how good are our benchmarks? How good are other benchmarks? Over the past couple of weeks, I've been labeling many dozens of attempts of models on [SWE-bench](https://arxiv.org/abs/2310.06770) with a bunch of different factors to try and understand, for example, how good are our tests in SWE-bench? Are models often implementing correct functionality that isn't captured by the tests because the tests were written for the specific implementation that the human originally wrote?

(01:37:01):
Or alternatively, are models often succeeding as judged by the automatic test cases, but they actually break a bunch of other code that isn't tested in the repo, or their solution is just so bad in some other ways that we wouldn't actually call that a success? Broadly, this is one example of this stream of work that we've started doing more of over the past few months of trying to understand benchmarks, this science of evals stuff of: how can we interpret certain scores on different benchmarks? Ones that we've made, ones that other folks have made.

(01:37:55):
Also, questions around to what extent are current methods for improving AI systems going to generalize? One example that comes to mind of an open question to us is something like: training models on formally verifiable tasks, like passing test cases... People talk about "reinforcement learning from verifiable rewards". There's a question of: how much progress currently is coming from this? And maybe there are two corollary questions: how much should we expect progress when training in this way to generalize to non-verifiable tasks or tasks that are messier or more qualitative? And then alternatively, maybe if improvements in models from this type of training doesn't actually generalize well, how much human data, for example, do you need to train models that are good on more qualitative, messier tasks? Trying to get some sense of things like this, this is something we're interested in. The exact projects that we'll end up doing will depend on specifics.

**Daniel Filan** (01:39:32):
Fair enough. That's things that METR might end up doing. There's a whole other world out there, including listeners to this podcast.

**David Rein** (01:39:40):
Whoa!

**Daniel Filan** (01:39:42):
If they're interested in advancing this research direction, what would be good things for outside people to do?

**David Rein** (01:39:50):
One thing that I've been really excited about is this work basically making it easier to run evaluations in standardized ways. So at METR, we've started using this platform for running evaluations called [Inspect](https://inspect.aisi.org.uk/). It's open source. It's primarily developed by folks at the [UK AI Security Institute](https://www.aisi.gov.uk/). This platform is great, and there are a bunch of benchmarks that have been implemented in it, and I'm super excited for more benchmarks to make it in and to improve the ecosystem's ability to broadly run these evaluations. That's more on the engineering side of things.

(01:40:54):
In terms of research, I'm excited about people extending the time horizon methodology to more benchmarks. Actually this guy [Sean Peters](https://sean-peters-au.github.io/), I think his last name is, he [evaluated models on cybersecurity benchmarks in particular](https://sean-peters-au.github.io/2025/07/02/ai-task-length-horizons-in-offensive-cybersecurity.html) and used time estimates from those benchmarks. I think he did some amount of estimating task length himself and fit some trends to models' performance on this particular slice. I thought that was a really useful way of getting more data validating these things. I'm excited about direct follow-up work like that. Directions in the vein of what we talked about, of trying to decompose model success and failure, or understand what are the fundamental trends going on here... I think I said earlier I was pessimistic about these extremely constrained, less realistic types of tasks, but I do still think they can be quite useful, almost as diagnostics or something, just helping bound our understanding of what models can and can't do.

(01:42:43):
Something that comes to mind is people have made kinds of tasks that are basically just "how many of a very basic action can models take in a row before they fall over or get off track?" Things of that nature. Very large kinds of arithmetic, that comes to mind as an example. I think things like that are actually interesting, although I think to me they're more [about] bounding model capabilities.

**Daniel Filan** (01:43:20):
Fair enough. The second to last question I'd like to ask is: is there anything that I should have asked that I haven't yet?

**David Rein** (01:43:32):
Great question. I think broadly we've covered a fair bit of METR's capability evaluation work. I think there are big open questions to me around how long we'll be able to continue doing this work. Not even just from a tractability perspective, but also just from a "will it actually be useful?" perspective, in particular for estimating risk. So at a certain point, if we are seeing that AI systems are able to do AI research very effectively, then it's like, okay, how do we continue estimating risk? Is risk just "maximum"? Probably not. People are still going to be doing kinds of monitoring, or I expect folks to implement basic kinds of control methods. So over the past few months, we've been doing more work trying to create better metrics for things like monitorability. I guess I'm just describing this instead of a question. I haven't been working on it, but I think it's very interesting and exciting work.

**Daniel Filan** (01:45:06):
Yeah. Sounds cool. So speaking of, if people are interested in following the work that you and your colleagues at METR do, how should they go about doing that?

**David Rein** (01:45:16):
Yeah, so going to our website, [metr.org](https://metr.org/). We publish our research updates there. I think you can put in your email and subscribe. We also post on [Twitter](x.com/METR_Evals). I can't remember our Twitter handle. Anyways.

**Daniel Filan** (01:45:39):
It'll be in the description.

**David Rein** (01:45:44):
We're also [hiring](https://metr.org/careers). We're hiring experienced researchers and research engineers. So if that's you, definitely reach out, and we may be excited to chat.

**Daniel Filan** (01:45:59):
Great. Well, thanks very much for coming and chatting with me.

**David Rein** (01:46:03):
Yeah, thanks a lot for having me. This was really fun, Daniel.

**Daniel Filan** (01:46:06):
This episode is edited by Kate Brunotts and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. This episode was recorded at [FAR.Labs](https://far.ai/programs/far-labs). Financial support for the episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future) along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript, you can visit [axrp.net](https://axrp.net). You can also become a patron at [patreon.com/axrpodcast](https://patreon.com/axrpodcast) or give a one-off donation at [ko-fi.com/axrpodcast](https://ko-fi.com/axrpodcast). Finally, you can leave your thoughts on this episode at [axrp.fyi](axrp.fyi).
