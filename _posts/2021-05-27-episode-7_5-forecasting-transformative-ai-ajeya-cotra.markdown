---
layout: post
title: "7.5 - Forecasting Transformative AI from Biological Anchors with Ajeya Cotra"
date: 2021-05-27 17:30:00 -0800
categories: episode
---

*Audio unavailable for this episode.*

This podcast is called AXRP, pronounced axe-urp and short for the AI X-risk Research Podcast. Here, I ([Daniel Filan](https://danielfilan.com/)) have conversations with researchers about their papers. We discuss the paper and hopefully get a sense of why it's been written and how it might reduce the risk of artificial intelligence causing an [existential catastrophe](https://en.wikipedia.org/wiki/Global_catastrophic_risk): that is, permanently and drastically curtailing humanity's future potential.

If you want to shape the development and forecast the consequences of powerful AI technology, it's important to know when it might appear. In this episode, I talk to Ajeya Cotra about her report "Forecasting Transformative AI from Biological Anchors" which aims to build a probabilistic model to answer this question. We talk about a variety of topics, including the structure of the model, what the most important parts are to get right, how the estimates should shape our behaviour, and Ajeya's current work at Open Philanthropy and perspective on the AI x-risk landscape. Unfortunately, there was a problem with the recording of our interview, so we weren't able to release it in audio form, but you can read this transcript of the whole conversation.

**Daniel Filan:**
Hello everybody, today I'll be speaking to Ajeya Cotra, a senior research analyst at Open Philanthropy, which is a grant-making organization that supports work to reduce AI existential risk [among other things]. Before joining Open Philanthropy she studied electrical engineering and computer science at UC Berkeley, where she founded the Effective Altruists of Berkeley student group, and also taught a course on effective altruism. Today we'll be talking about [her work on forecasting when transformative AI might be developed](https://drive.google.com/drive/u/1/folders/15ArhEPZSTYU8f012bs6ehPS6-xmhtBPP), and how that might influence grant recommendations. Ajeya, welcome to the show.

**Ajeya Cotra:**
Thank you, great to be here.

**Daniel Filan:**
Great. So I guess we're going to be mainly talking about this draft report on forecasting. I believe it's called forecasting transformative AI?

**Ajeya Cotra:**
[Forecasting Transformative AI From Biological Anchors](https://drive.google.com/drive/u/1/folders/15ArhEPZSTYU8f012bs6ehPS6-xmhtBPP).

**Daniel Filan:**
Exactly that one. So just briefly, can you summarize what you were trying to do in this report and how you went about doing it?

**Ajeya Cotra:**
Yeah. So Open Philanthropy is interested in reducing risk from what we call transformative AI, and transformative AI is a software program or set of software programs that has as much impact on the world as the industrial revolution or agricultural revolutions did. And more specifically that means that it radically accelerates growth. So the operationalization I use is that it makes growth go 10x faster. So instead of doubling the economy at every 25 years [which is approximately how fast the economy currently grows], we double the economy every two and a half years. And so we're interested in that because it seems like it's slightly broader than this notion that people often use of artificial general intelligence, which is a computer program that can do everything a human can do as well as a human can do it, but we think it captures what feels important about artificial general intelligence from a strategic, philanthropic perspective, which is that it upsets the world in this crazy way, and it's this intense transition that we might want to anticipate and make go better.

**Daniel Filan:**
And just a question about that. So one thing that's kind of interesting about that definition is that, because it focuses on the impact of the technology, it's pretty agnostic to what the computer program actually looks like, or what it actually does, and I'm wondering, that seems like it might be a problem for concreteness or knowing what you're doing.

**Ajeya Cotra:**
Yeah. So I think it's a benefit from the perspective of trying to isolate what we care about when we communicate what we care about, but it is the case that we have various more specific models in mind when we're thinking concretely about strategy. So we're often thinking in this mode of there's one deep learning model or a cluster of deep learning models with a roughly similar architecture or something that collectively automate a lot of tasks sufficient to accelerate growth to that degree.

**Ajeya Cotra:**
But the history of the transformative AI definition is that we wanted to point to the thing that could be really intensely good or intensely bad about artificial intelligence progress without necessarily getting into debates of, is it going to be a single unified program? Or is it going to be more like a distributed industrial revolution style thing? People will ask questions like, "But what if it doesn't have feelings?" Or, "what if it can't love?" Or something like that.

**Ajeya Cotra:**
So this definition was created to point at the thing we care about, which has impacts on the world, while being intentionally agnostic about everything else. But it does have that drawback of lack of concreteness, so we internally think about more specific concrete models for how it could go down.

**Daniel Filan:**
And that's the thing that you're trying to forecast in this model?

**Ajeya Cotra:**
Yeah. So the thing I'm trying to forecast in this model is transformative AI. And this is where we get into, we have a bit more of a specific notion of what it might look like. What I use as a proxy is I try to think about how costly it would be in terms of computation to train a single model, which if many copies of it were deployed everywhere would create this transformative transition. So this would be one trained machine learning model like GPT-3 or something like that, but replicated everywhere, used everywhere, general purpose enough, not necessarily full blown AGI, but general purpose enough that that causes a transformative transition like the industrial revolution.

**Ajeya Cotra:**
So I'm thinking about a transformative model for the most part in this setting. And I use when the computation to train a transformative model would be affordable as the proxy for when transformative AI would occur. So this is not to say that I think it will definitely look like that. It's to say that this is something concrete that we can get a median for. And there are reasons to believe it might be sooner than that. And there are reasons to believe that it might be later than that.

**Ajeya Cotra:**
So reasons you might believe that it would be sooner would be maybe we find other ways, like maybe we do some hard coding programming, we train a bunch of different specialized models, and that collectively causes a transformative change before we can train a single model that you would call transformative by itself. And then the reasons to think it might be later than that are that I focus mostly on when the computation would be affordable, but there might be bottlenecks in terms of data and environments where even if the computation is affordable and we know how we would train a transformative model, we don't have these other ingredients in place.

**Daniel Filan:**
So you have some error bars in when transformative AI might come just from the model, and then there's some additional error bars that come from, okay, well the model might be overestimating things or underestimating things. Do you think the error coming from, it might be an overestimate or an underestimate, do you think that's much smaller than the uncertainty inside of the model or much larger than that uncertainty?

**Ajeya Cotra:**
I would think it is smaller. So for context, the answer to the question spat out by the model of how much computation would it take to train a transformative model spans about 22 orders of magnitude.

**Daniel Filan:**
That's a lot.

**Ajeya Cotra:**
And so I believe that the uncertainty on the other side of how much do you smooth out the output of that model for these out-of-model considerations? It's considerable, but I don't think it's as large as the within-model uncertainty.

**Daniel Filan:**
And before you started this draft report, what do you think the public state of knowledge was about when transformative AI would be developed? Do you think we were totally unsure or we had a pretty good idea that you wanted to check or where do you think we were at?

**Ajeya Cotra:**
In terms of the public state of knowledge I don't think that there was much out there. So there was a tradition of biologically based forecasts of artificial general intelligence, which is obviously very related, from a while back, from the 90's. So I believe it was Marvin Minsky, and Ray Kurzweil, and that crew. So they would talk about stuff along the lines of "here is how computationally powerful the human brain is". They would have some arguments and estimates for that. And then they would say, "Here is how the price for floating point operations per second is changing because of Moore's law." And so they would basically say, "It seems likely that we would get artificial general intelligence when a computer as powerful as the human brain is 'cheap', which might be operationalized as it costs $10,000 or $1,000, or something like that."

**Ajeya Cotra:**
So that was out there and had been in the water, and I think many people in effective altruism and rationalist communities had thought about it, and it had also received critiques and a lot of people didn't take it seriously. And those forecasts tended to place AGI in somewhere between 2010 and 2030. And I think that that is an underestimate and it's expecting AGI to arrive too soon. Obviously some of it is in the past, but the 2020 to 2030 chunk, I think those forecasts assign too much probability to that chunk because they underestimated the cost of, given that you have a computer that could run the human brain, how difficult is it to find the program that constitutes the human brain?

**Ajeya Cotra:**
And so what this report adds on top of that, and I think it might've been done very briefly in public before, but this is the first thorough and public treatment of that piece of it. So given that you have a computer powerful enough to run something like the human brain, what is the cost of searching for the program that will actually have that behavior?

**Daniel Filan:**
And so is it fair to say that you think before this report people should have been very uncertain when AGI was going to show up?

**Ajeya Cotra:**
I think so. And I think they should be very uncertain after this report too. So yes, I would endorse that.

**Daniel Filan:**
And what decision relevant uncertainties were you hoping to reduce in this?

**Ajeya Cotra:**
Actually one more point. Before this report, should people have been very uncertain? My answer to that is yes, I think they should have been very uncertain, but in practice, I feel like before the report, and also to some extent after the report, there was this what I would consider to be artificial bi-modality to the way people talked about timelines. So I think there was a cluster of people who were very Kurzweilian or Minskian, and were just like, "a computer as powerful as the human brain costs a $1,000 or $10,000 now, any day now". And then there were some people who are like, "Deep learning can't take us all the way there. So really far away, I don't know, 70 years." And I had always thought that people's probability distributions should be more spread out than that, and not so bi-modal given how little we know.

**Ajeya Cotra:**
And I think one thing that the report does is provide more justification for one humped, not bi-modal distributions, by filling in how could it be considerably more expensive than simply running one instance of a human brain size computer while still not being so expensive that it's many many decades away?

**Daniel Filan:**
And what decisions do you think really pivot on these timeline forecasts?

**Ajeya Cotra:**
Yeah, so I think the biggest decision-relevant threshold for Open Phil and I think also many other actors is, is there a compelling seeming case that it's pretty plausible, tens of percent, that transformative AI could be developed in the next 10 to 20 years. And so pinning down that case, which we suspected that it was the case that the probability of transformative AI by 2036 was at least 10%. And the report cites a blog post by Holden Karnofsky in 2016 saying at least 10% in 20 years, which is 2036.

**Ajeya Cotra:**
So that was loosely a focus. The super specific deadline of 2036 was obviously a little arbitrary because it was 20 years from 2016. But the general vibe that if there was a good story that it really could happen in 10 to 20 years, basically it's both decision relevant and persuasion relevant. So it's decision relevant in the sense that if we bought into that we would put in, first of all, more resources into AI risk versus other existential risks and also into existential risks as a category versus other cause areas. And that the money we spend would be more opinionated. So it would be like, there's a considerable chance that this will happen soon enough that we could really anticipate some of these details like that there's a pretty good chance that we literally know which institutions would play a big role in developing it. And there's a pretty good chance we would know literally what architectures would be used to train it and stuff like that.

**Ajeya Cotra:**
So the research and the advocacy and strategic work we could do could be a lot more opinionated versus if we were maybe out of sheer ignorance we had a sense that it might well be soon, we wouldn't be able to be so opinionated and we also wouldn't be able to be persuasive to the people we wanted to bring on board with working on this issue in that frame.

**Daniel Filan:**
And so that's about the decisions you would make or how things would be different if you knew it was plausible that you get AGI by 2036, which is now 15 years from now. That'd be pretty soon. Suppose instead that you weren't at all confident, or you didn't have a plausible story that we would get transformative AI that soon, but you did have a pretty plausible story that you could get it 40 to 80 years from now.

**Ajeya Cotra:**
As in not before 40 years, but between 40 and 80 years?

**Daniel Filan:**
You have a plausible story for how you get between 40 and 80 years from now, but you don't have a plausible story for how you get it before 30 years from now or something. And that's still close enough that a bunch of people would be alive today who would see this amazing world change. How would things be different in that world?

**Ajeya Cotra:**
I think that world would look more different from at least what Open Phil is doing right now. So my report suggests that there is a plausible story for the 10 to 20 years, and that had also been what we were interim betting on before doing the report. So I think if we had not found any plausible story for 30 years, that would constitute a bigger change to our priorities, and the main nature of that change I think would be we would care a lot more about broader interventions that take a longer time to pay off.

**Ajeya Cotra:**
So I mean stuff like it would become marginally more attractive to work with high school students, and support long plays to try and influence various political parties, or to try and have really broad-based education in the principles of rationality and effective altruism that we think would take a generation to really incorporate into the world. And then we would probably talk a lot less about AI in particular, even if one of our goals for all this stuff we were doing is to make transformative AI go better when it happens, because we would expect to have less AI-specific asks.

**Ajeya Cotra:**
So we would expect that the people we should be talking to aren't the machine learning researchers of today and the machine learning PhD students of today, but people whose kids might be AI researchers in 25, 30 years.

**Daniel Filan:**
So it seems like the biggest thing in the specific model is just how much computation it would take today to train something that was as transformative or something as humans, or is it as smart as humans?

**Ajeya Cotra:**
A bit less. I would say how much computation it would take to train a transformative model today, which would mean that if you dropped it into the world today then we would have doubled within a couple of years and keep accelerating from there. So that model I think, would have to be able to automate a wide variety of tasks, which would probably have considerable overlap with humans, but it could be superhuman at some things we're really not very good at, and also have gaps. So something in the range, and then if I had to quantitatively place it in relative to humans, I think it could be somewhat less overall capable while still being able to be transformative in this way.

**Daniel Filan:**
And the title mentions the biological anchors approach. Could you say a bit about what those anchors are and how you use them to estimate this?

**Ajeya Cotra:**
Yeah. So the basic idea here is that if what we trained today were a virtual human, someone who could do the range of things that a human worker could do remotely about as well and about as cheaply and quickly as the human worker could, then we believe that that would be transformative in the sense that I described. We don't expect that we would train exactly that because that's a really particular target, but we were like, this seems sufficient if not quite necessary. It seems like it's not way, way more than is necessary. Something way less capable than a human on a lot of different dimensions we believe wouldn't be transformative if trained today. So we're like, okay, here's like a picture we have in mind of a virtual human. So we can think about if we were to think of the human brain as analogous to a computer, how powerful would that computer be? So my colleague Joe Carlsmith had a [detailed report on this](https://www.openphilanthropy.org/brain-computation-report), and then going from there we can be like, okay. So a transformative model is not going to quite be a virtual human. Like I said, it might be different in various ways, could have a totally different architecture, and might be slightly less capable. So how much computation would it then take to train a transformative model, given our estimate for the brain?

**Ajeya Cotra:**
So there, I just thought about a variety of different considerations, one of which is that the model could be somewhat less powerful, somewhat less general, but also that it seems like the artifacts that humans design tend to be somewhat less efficient in energy, and payback period, and various metrics of interest than the artifacts that evolution "designs." So if you look at eyes versus cameras, eyes have many more megapixels if you were to translate visual acuity into the notion of pixels, and they're smaller, and more compact, and they're much cheaper to manufacture in terms of the actual calories that it takes to manufacture eyes.

**Daniel Filan:**
How does that happen? Aren't humans way more smart than random nature or something? It seems so weird to me that nature can be so much better.

**Ajeya Cotra:**
We had so, so much less time. We can be way, way more smart, but we had a hundred years of a tiny fraction of humanity working on improving camera designs, and they are improving. Over time they're getting smaller and cheaper to manufacture and having more megapixels and all that stuff. And you have this trend line and eyes are down here, and evolution had billions of years for eyes and many other things. So you can think about cameras versus eyes, and you can think about solar panels versus leaves. And in that case actually solar panels are more efficient at translating sunlight into energy than leaves, but they're way more expensive to manufacture. So it depends on the metrics that you choose, but in general the stuff we design tends to be worse than nature, but getting better.

**Ajeya Cotra:**
And so if I were to think about exactly today, you would expect that our machine learning architectures would be somewhat worse than nature. So basically I thought maybe if our architectures and optimization algorithms were as good as evolution then maybe we could shave off an order of magnitude because the transformative model doesn't need to be quite as powerful as a human brain, and it doesn't need to do quite as many things, but also we tend to be a couple of orders of magnitude worse. So my median estimate was that we would need a model that was one order of magnitude larger than our best guess for the human brain in order to train a transformative model, if we had to do it in 2020, and then things are continuously improving.

**Daniel Filan:**
So a pretty crucial part of that estimate is this sort of conversion rate between biological brains and floating point operations. How solid do you think this conversion rate is?

**Ajeya Cotra:**
Within the estimate of the computational power of the human brain, setting aside that we layer on top of that how does a transformative model compare to the human brain, there's a couple of orders of magnitude of uncertainty on that log normal distribution. And the central estimate is essentially that each synapse in the brain when it fires performs one to 10 floating point operations, and that modeling whatever is going on inside the neurons doesn't dominate. So there's basically a thousand times fewer neurons than synapses. So the statement is that synapses do one to 10 floating point operations every time they fire. They fire about once a second. And whatever's going on inside the neurons is not more than a thousand times more expensive. So there could be complicated stuff going on within the neurons. But given that there are so many fewer, they don't dominate.

**Ajeya Cotra:**
So that's the basic really high-level mechanical argument for that claim, that central estimate of approximately one per synapse per second, which works out to be 10^15 floating point operations per second, or petaflops. And it depends a lot on your taste for that kind of argument and various priors. A lot of people will be like, "That's so simple. The brain is really mysterious. There's so much going on. It's such an impressive organ. There's got to be a lot more computation in various pockets that has not been properly appreciated yet." And that would be the main style of objection to that central estimate. And Joe, my colleague who did the report, he goes through and investigates many possibilities for where that hidden extra computation could be. And he considers most of them not too plausible, and talked to various experts about that. And I'm generally pretty happy to run with those conclusions, but definitely your mileage will vary a ton. And there's a lot of uncertainty in the brain estimate and there's a lot of uncertainty layered on top of that for transformative model versus brain.

**Daniel Filan:**
So before I asked about that I think you were describing how much computation you would need to run something that could do the human brain. Is that right?

**Ajeya Cotra:**
Yeah. So if you knew the "software" for the human brain, what kind of hardware would be needed to run it at the same speed as a human? And that's how you could interpret the question that Joe was answering in his report.

**Daniel Filan:**
Did you say what the evolution anchor was yet?

**Ajeya Cotra:**
No. Sorry. So there are six anchors that I use in the report, and three of them are coming from this place of maybe we could train a machine learning model about the size of the human brain, one order of magnitude larger based on the thinking I was doing. And maybe we could use trends in how much computation models take to train based on how big the model is to estimate the total computation taken to train. And so there are basically three different versions of that that span from a low end of 10^26 floating point operations to a high end of 10^35 floating point operations, very roughly speaking, and we can get into what those different ones are.

**Ajeya Cotra:**
And then those three anchors are kind of flanked on the left and the right by anchors that have a slightly different inspiration. So the middle anchors are saying, "Okay, let's assume that a model in the general region of the human brain would be enough to train it to do a transformative task. And let's use extrapolations to figure out how expensive that would be." And then on the left and the right we have biological anchors that directly estimate training computation without going through model size. So on the left we have the lifetime anchor, which is basically if you think a human brain does some number of floating point operations per second, like 10^15 floating point operations per second, then if you add it up over the course of a lifetime and ask, how long does it take for the human brain to go from a baby or an embryo to an adult? Then you can sum that all up and say that over 30 years the human does 10^24 floating point operations.

**Ajeya Cotra:**
So if we could somehow figure out how to basically write down the baby's development algorithm, then we don't need to write down all the content in an adult brain, but we could learn it very quickly. So that's kind of on the left. That's the lifetime anchor. And then on the right is another anchor that doesn't make assumptions about the final model size, but is much more conservative and says, "Okay, well, evolution performed a lot of search over the space of possible minds to specify the baby's learning and development algorithm. So maybe we need to do all of that work in addition to the baby's learning and development work."

**Ajeya Cotra:**
So that basically thinks about all of our ancestors from the earliest ancestors that had neurons. So you could go even further back but I don't, where there were these tiny jellyfish that had 10 neurons or something, all the way from that to humans and trying to estimate the computation by thinking about how many ancestors were alive at any given point in time, and what is their average brain floating point operations per second? And how many seconds are in 1.2 billion years of evolution.

**Daniel Filan:**
Cool. So, yeah we're going to get into a lot of those, but before that I guess I have some questions about the overall approach. So when you're making this model, in this breakdown of things, what parameters do you think are most important to get right? What should I be paying a lot of attention to?

**Ajeya Cotra:**
So I do think that the brain floating point operations per second is the most important one. It features in every single one of these. Indirectly in the lifetime and the evolution, and directly in the middle three anchors which are the neural network anchors. Although, like I said, I wasn't actually the one, it was important enough that we had another person investigate just that. So I can't super-duper go into the details, but that I think is definitely the most important.

**Ajeya Cotra:**
And then in terms of what is the second most important? The second most important is something that we haven't introduced yet, is this notion of the effect of horizon length. Which is basically for the middle three anchors, like I said, we're assuming we're training a large machine learning model the size of the human brain roughly, and we're trying to extrapolate based on the models we've trained so far how expensive it would be to train a model of that size. And the basic extrapolation involves saying a model with a certain number of floating point operations per second tends to have a certain number of parameters to fill in, and then filling in that many parameters requires a certain number of data points.

**Ajeya Cotra:**
But the real crux that spreads those neural network anchors across a really wide range is what counts as a data point. So is a data point that you're a language model and you see a single letter or a single word, and you get some feedback on how well you predicted that word, and that's one data point. Or is a data point like you're a StarCraft model, you play a 10 minute game of StarCraft, and at the end you find out whether you won or lost, and that's a data point. Or is a data point like you spend an hour attempting to do a difficult college level math problem in a textbook, and then you flip to the back of the book and check to see if you got the answer right, and that's a data point. Or is a data point even something more extreme, which would be analogous to evolution, which is that you're an animal and you live some number of years, and then after that number of years you get to see whether you reproduced or not, and that's one data point in your lifetime.

**Ajeya Cotra:**
And so that question is abstracted in the report into this question of what is the effective horizon length? How long do you do things in series before you get to see whether you're succeeded or failed? And that counts as one data point.

**Daniel Filan:**
And I'm wondering, so one thing that struck me when I was reading the discussion of the effective horizon is it seemed very closely related to the variability in the gradient. So you get a gradient update and you're like, okay, but what's the randomness of the distribution that is hopefully centered on the ideal gradient update? There's some distribution, it's got some variance, what's the variance that I'm drawing from? I think there's been some work to estimate this. I'm wondering if that played into the report?

**Ajeya Cotra:**
So there has been some work to estimate this. I do think they're related, but I don't think they're the same concept. So the thing you were talking about, you could have in what I would call a one-step horizon task, could have a wide distribution of noise. So you could imagine ImageNet versus ImageNet where you flip a coin and only tell the model whether it got it right half the time, or only tell the model whether it got it right 10% of the time, or 1% of the time, and that would very directly increase the noise in the task, and as a result you would need to digest a larger number of images to get the same signal, but all those images could be digested in parallel because there's no dependency between one thing the ImageNet model guesses, and the next thing the ImageNet model guesses. So it increases the batch size and the work you're referring to I think might be [the paper from McCandlish and Kaplan](https://arxiv.org/abs/1812.06162) [and also Amodei and the OpenAI Dota team] on optimal batch size, or a couple other things.

**Daniel Filan:**
I'm mostly just thinking that this seems like the kind of thing that would be worked on.

**Ajeya Cotra:**
And there's also theoretical results about if the loss has a certain variance, then basically the number of data points you need to get a good enough signal scales as one over the variance. And so that work has been done, but this is a different notion, which is that even if, say you have to do a sequence of five things and then you get a perfect ground truth, no noise signal, after you do that sequence of five things you have to walk to the kitchen, and you have to put on a pot of coffee, and you have to wait until the coffee boils, and pour it into a cup. That would be a task with a longer horizon length than ImageNet, but lower noise. And then that task, you could make tasks that natively take a long time easier to learn, by providing proxy rewards along the way.

**Ajeya Cotra:**
You could instead have chosen to reward the model for every step it takes in the right direction, and usually you do try to do that because you could think of it as reducing the noise. I would try to break apart noise that can be handled by increasing the batch size in parallel, versus noise that can only be handled by creating new proxy rewards. But machine learning researchers would tend to refer to both of those things as noise.

**Ajeya Cotra:**
But the reason I break them apart is that I have a specific kind of uncertainty about how many things would you have to watch the model do in series? How long would you have to watch the model act before you had any idea, before you wanted to give it any kind of reward.

**Daniel Filan:**
Why does that end up mattering if we're just trying ... Because I thought we were just trying to estimate how much computation it would take and it seems like you're distinguishing, in the case where there's a long horizon, you have to do computation in series. In the case where it's noise, you can parallelize the computation to get a good estimate. But-

**Ajeya Cotra:**
Well, the reason that it matters is that I'm more willing to extrapolate noise from existing tasks. I'm more willing to assume that noise is baked in to the family of tasks that I'm using to do the extrapolation, than I am to assume that the horizon is baked in. Because the horizon appears to be increasing in that, in 2015 we would do Atari tasks, which had a discount rate that implied that the model only cared about the next 7 seconds.

**Ajeya Cotra:**
In 2020, we're doing Dota and StarCraft where those discount rates imply that the model cares about the next 5 to 10 minutes. It seems like a natural thing in the world that you would want to have longer horizons and lower discount rates, for more difficult and interesting and more real-world tasks. It seems more likely, basically it felt like I was opinionated about the direction in which simply extrapolating current tasks would err on the horizon length point, but not on the non horizon related noise point.

**Daniel Filan:**
Okay. All right. That makes sense. One thing about the assessment is, as you've mentioned, it involves biological anchors. You're saying, okay, we're going to ... I think of this as, in this family of approaches, which is you look at, maybe it's the natural world, or you look at how well other technologies have worked and you say, I kind of expect AI to be kind of like that, and we'll just look at how hard that was and somehow map that on to AI.

**Daniel Filan:**
Whereas another approach that is intuitively compelling, and I think an approach that a lot of people in AI or machine learning use is to say, well, 50 years ago AI could do this much stuff, 20 years ago they could do this much stuff, today they can use this much stuff. Let's draw a line or maybe a line on a log plot or something, and forecast future capabilities from past capabilities. I'm wondering why you chose the more, perhaps, outside view - I'm wondering why you chose the, learn about AI by thinking about biology, compared to what might seem more natural to learn about AI by thinking about AI.

**Ajeya Cotra:**
Yeah. I call that approach that you just outlined, the subjective impressiveness extrapolation. The reason I call it that is that I think, you're describing, let's think about the number of things that AI could do 50 years ago, versus 20 years ago, versus today and draw a line through that. I think the y-axis will be hotly debated, and I find is much harder to get a grip on than the biological anchor stuff.

**Ajeya Cotra:**
On the y-axis, I've heard everything from we have only automated 0.1% of the jobs, where the jobs are broken down in a certain way, so it's 1000 years away to ... It's a couple years away because look, we've solved vision, and we've solved motor control, and we've solved hearing, and we've solved language and what else is there? We just have to put them together. I've also heard all sorts of things in between, we've made some progress, but transfer is going to be a really tough bottleneck, and we're going to need conceptual insights for transfer, or whatever it is.

**Ajeya Cotra:**
I think that just thinking in terms of subjective impressiveness, it encourages the forecaster to be, I think, over-confident in which of these like many interpretations is actually the way AI will progress. Is AI going to first automate 0.1% of jobs, and then the next year 1% of jobs, and then the next year 10% of jobs and so on? Or is it going to actually learn how to do all the things a bee can do, and then all the things that fish can do, and then all the things a bird can do, and then all the things a primate can do, and that's how it's going to go? Or is it going to be, first it acts like a baby, and then it acts like a toddler, and then it acts like a first grader and that's how it's going to go?

**Ajeya Cotra:**
There are just a lot of ways that that y-axis could go, that would suggest radically different interpretations that actually I think would end up having more uncertainty, if you were actually systematic about collecting all the ways of thinking about it than the biological anchors approach. Most people who do the subjective impressiveness extrapolation don't acknowledge the degree of uncertainty that it would have. But I think if you were to do the most systematic version of it, it would have crazy uncertainty.

**Daniel Filan:**
Okay. One way of reading that is that, well in that case extrapolating from biology is bringing this false precision because if you just look at AI, it's so unclear. If it turned out like biology, then sure we could get this. It's not that certain because you have a 100 year range or something for your 80% credible intervals?

**Ajeya Cotra:**
Yeah. Something like that.

**Daniel Filan:**
Something like that. Yeah. But I'm wondering what you think of that response?

**Ajeya Cotra:**
I actually think that, like I said at the beginning, my distribution is somewhat wider than what is spat out by the model, because of model uncertainty. But I don't think that having something that's uniform between one year and 1000 years is more right than what I have. I think it actually should be more concentrated, and I think that's because the biological anchors and general outside views, including other outside views, like [semi-informative priors](https://www.openphilanthropy.org/blog/report-semi-informative-priors), which is another report by my colleague that came out. I think they do actually constrain things because they make it ...

**Ajeya Cotra:**
They transform the y-axis from a subject of endless debate in different ontologies and stuff, into something that is at least a little bit more measurable. I think that actually brings information in. I'm not actually uncertain between one year and 1000 years, it's more like I'm uncertain between 10 years and 80 years.

**Daniel Filan:**
Yeah. I guess this is kind of interesting. Normally when forecasters make moves that are roughly, let's take the outside view, let's compare to a bunch of other things, normally that makes them less certain. But it seems like this is one of these case where in a certain setup it makes you more certain. Which is kind of unusual, right?

**Ajeya Cotra:**
I think it's unusual. I think the people who do the extrapolation, the subjective impressiveness extrapolation, are in fact empirically more confident than me, and I would disagree with that.

**Daniel Filan:**
Because they are not doing this averaging.

**Ajeya Cotra:**
Because they're not acknowledging that there's so much disagreement about the y-axis. I think it's not so different from the typical case that taking an outside view makes you more uncertain. It's just more that, I think, the outside view gives you some evidence that some particular inside views that are on the extremes are less likely than the particular individuals who hold those views would believe.

**Daniel Filan:**
Okay, cool. I guess another a bit more zoomed-out question. I think some of your estimates for how much computation it would take, like how many floating points you need to do, are 10^30 or something. We're thinking about computers that can do maybe 10^20 floating point operations per second. That ends up taking 10^5 days or something, which is many.

**Daniel Filan:**
So if you're forecasting, how long will it take until we can afford to do this computation. Is there not also a lag of maybe it takes 30 years to actually do the computation once you can afford to buy it?

**Ajeya Cotra:**
Yeah. I haven't looked into this a ton, but my suspicion is that it could work out to not be that long. The reason is because, it seems like we have increasing evidence that if you need to train a model on a giant amount of data, like 100 million data points, it's surprisingly often the case that the most effective thing to do is to train it on 100,000 data points in parallel, and do only 1000 actual steps of serial gradient descent.

**Ajeya Cotra:**
That is a result found in [the optimal batch size paper](https://arxiv.org/abs/1812.06162) that I mentioned earlier and a couple of other papers that came out more recently than that. I have a brief section on this in my report. I think essentially I'm pretty uncertain for the evolution anchor and the high end of the neural network anchors that assume long subjective horizon lengths, whether it would work out. But I think it's reasonably likely that it would work out for the middle and lower ends, which look a little bit more like training existing models.

**Ajeya Cotra:**
Effectively incorporating this consideration, I think might stretch out the right tail. But we are mostly focused on nailing the left tail of the probability that it's soon. That's one reason why I didn't go deep into it in the report. But I do think that's a consideration that if I were to do a maximally full version of the report, I would think about more.

**Daniel Filan:**
Okay. That makes sense. Let's get a bit into the anchors. You have these evolution, lifetime, and then some neural network anchors and the genetic anchor, right?

**Ajeya Cotra:**
The genome anchor.

**Daniel Filan:**
Genome anchor. What do you think we could learn that would make these anchors more or less likely? How should we assign probabilities to which of these is right?

**Ajeya Cotra:**
Yeah. That's definitely the most subjective and fraught part of the exercise, is assigning weights to the anchors, that makes a big difference. Most of the criticism that I've gotten has not been, "You should shift these anchors by this amount." Or the uncertainty within a particular anchor is wrong, it's been that I think that lifetime would actually work. Or I think anything smaller than evolution probably won't work. That's the nature of the criticism this has gotten. Almost all of the engagement is on that front, which I think is appropriate, because it makes a big difference and it's very subjective.

**Ajeya Cotra:**
Some things that I can say about what I did to arrive at this is, first of all I just gave them all equal weight and saw what that looked like. It didn't look super duper different from what I ended up with, and that's something that I would be nervous if it looked super duper different from what I ended up with. That's one basic check you could do, and I also did other basic checks. What if I zero out everything except the three neural network anchors, and give them all equal weight, and it was also pretty similar.

**Daniel Filan:**
When you say pretty similar, you mentioned that you were really interested in nailing the left tail. Do you mean pretty similar in that tail?

**Ajeya Cotra:**
It's pretty similar in the probability of 30 years or sooner. But you're right, that the uniform distribution in particular is quite different in the probability of 5 years and 10 years. So yeah, that's actually a good point. I think I should amend my statement about our interest in the left tail. The similarity is mostly in the median, and I say in the report that, the median is the thing that I'm most confident in coming out of the report. But in terms of which tail we're more interested in, we are more interested in the left tail.

**Ajeya Cotra:**
So I did some checks like that. Then what caused me to deviate from just giving them all the same weight, for lifetime and genome, especially lifetime, I was like, I feel like it would require a real break from how machine learning works, and there's not much time to have that break. Because we've trained lots of models on lots of tasks and they always take longer to learn that task than humans do. I just don't really see ... I've heard some arguments that learning the complete task of how to be a human will be different because the curriculum will be really right, and they'll learn the basics quickly. Then at some point the model will be able to do its own learning that's more efficient than stochastic gradient descent.

**Ajeya Cotra:**
But I heard the people who advocated those stories, and I don't buy them after hearing them. I put some weight on their judgment, but I was like ... That gets knocked down because of that. For a similar reason the genome anchor, which we haven't discussed yet, is essentially this idea that a model with as many parameters as there are bytes of information in the human genome, could be used to specify a development algorithm for a baby-like model basically.

**Ajeya Cotra:**
This is conceptually halfway in between the lifetime anchor which says, you can just write down the baby's development algorithm, and the evolution anchor which says, you have to redo all of the work of evolution to find something that is as good as the baby's development algorithm. This is something conceptually in between, which is that you have to fill in a model that is as large as the human genome, but you can search over that space a lot more efficiently than evolution. More in line with how efficiently you can search over the space of regular machine learning models.

**Ajeya Cotra:**
Then that one I was just like, we don't make models like that. This kind of two-step meta learning thing doesn't seem that popular, and when I poked around and where it was attempted with neural architecture search and stuff like that, it didn't seem to have worked that well, so that got downgraded.

**Daniel Filan:**
Yeah. I think I didn't realize you were imagining a two-stage thing, because I think you described it as a model with a number of parameters that's the number of bytes in the human genome, but it does the number of flops that the human brain does?

**Ajeya Cotra:**
They're basically two versions of it where it could be a two-stage thing, or it could just be a model that has weirdly few parameters, and weirdly a lot of flops. And both of them are kind of weird.

**Daniel Filan:**
Yeah, I didn't really understand how the one-stage thing could work. The two-stage thing did remind me of one of my favorite ideas in machine learning that almost definitely doesn't work, which is called [HyperNEAT](https://dl.acm.org/doi/abs/10.1145/1276958.1277158). Which is exactly this thing where you use evolution to find a neural network that takes in four inputs and one output, and you bias it so that it learns these nice geometric patterns. Then how you use that network, you use it to say, I'm going to have a second neural network that's really big. From layer x neuron i, to layer y neuron j, what's the weight of that? Well I'm going to plug in (x, i, y, j) into my high level network. So it's a way of developing neural networks where you decide that the weights should have this nice, beautiful, geometric thing. I really wish this idea worked better, it doesn't seem to. But yeah, hopefully if that pans out the genetic anchor will work. Anyway.

**Ajeya Cotra:**
Yeah. Downweighted those two for inside view reasons of, I don't know, this just looks really different from what we do. Then overall, this is something that people find controversial, overall I'm like, I downweighted the sections of all the paths that would imply that it's actually pretty cheap right now to train a transformative model, out of a rough, efficient markets intuition. That if it were to cost a billion dollars, that's something that a lot of people have lying around, and I would have expected it to have happened.

**Ajeya Cotra:**
It wasn't a full downgrading, it was basically just like, if it costs a million dollars today, that's very unlikely to have not already happened. But if it costs $30 billion today, I'm indifferent on whether it would have already happened. It's not an extreme efficient markets adjustment, because $30 billion would also be, somebody should have done it, but it's like ...

**Daniel Filan:**
Worth it if you think existential risk is low, and if you think that existential risk from doing it is high, then I guess it's definitely not worth it.

**Ajeya Cotra:**
Right. But I don't really expect the relevant actors to be so altruistic. I have trouble swallowing that it costs a million dollars today and nobody has done it. I'm like, okay, maybe it costs in the tens of billions and nobody has done it. There's basically a log linear slope from those two points in terms of how much I downweight the prior probability.

**Daniel Filan:**
People found that controversial, you said?

**Ajeya Cotra:**
Well, a lot of people who have short timelines, who do think that it's like five years away feel that I think markets are too efficient.

**Daniel Filan:**
Okay.

**Ajeya Cotra:**
They feel like I'm basically ruling out their view, a priori, which I sort of am, but not in an absolute sense.

**Daniel Filan:**
All right. I guess that's how you formed the view. What new information do you think would change your mind about these weights that you could get within the next, say, five years?

**Ajeya Cotra:**
I think the biggest thing that would move me one way or another is, if there were tasks where if you described the task to me ahead of time, I would be like, it seems like the effective horizon length is X, and X is longer than the models that have been trained so far. But then actually that model is trained more cheaply than I think, or perhaps more expensively. Where if I were like ...

**Ajeya Cotra:**
Thinking of an example of a test like this is tough, but it's ... I don't know, something like, here's a video game that takes an hour to run through, some sort of short puzzle game with maybe procedurally generated levels or something like that. Humans take ... It's like one of these things where you look at it and you have to ... I don't know, have you played Baba Is You?

**Daniel Filan:**
I've played it a little. But for our guests who haven't, what is it?

**Ajeya Cotra:**
It's basically like a game where you have to ... A puzzle game where you have to get to a goal at the end, and humans who are good at the game take an hour to solve the puzzle. They don't get any intrinsic feedback from the game, there's no score or anything like that until they have succeeded at getting the character to the goal. They have to push around blocks or whatever it is they have to, I don't actually know puzzle games that well.

**Ajeya Cotra:**
But take a game like that, that has a large enough number of levels that you can train a model on it, which is going to be the hardest part, and see if the model ... The amount of computation it took to train the model is consistent with the effective horizon length being something like 30 minutes or an hour or whatever, like good humans take to solve levels, or if it's actually cheaper than that. Basically getting to play around with the game for a lot of time steps lets you learn really well, even if you get sparse reward.

**Daniel Filan:**
Okay. I guess if this happened, would that mean the models were learning how to do good logical reasoning quickly rather than just learning by trial and error?

**Ajeya Cotra:**
I feel like I'm not ... I'm kind of agnostic on what it means about the internals of models. I think that would be a reasonable hypothesis. It's just more that it means my extrapolation should place a lot more weight on, everything will look like what I currently call short horizon. My notion of an effective horizon length that is determined by how sparse your reward is, and my feeling that maybe it would be hard to get tasks with dense rewards, is less right than I thought it was. Maybe that's because they're learning to learn or maybe that's because they're learning logical reasoning or whatever, but ...

**Daniel Filan:**
Okay, sure. You talked a bit about the evolution and the lifetime anchors. One thing that this reminded me of is, it seems like the evolution anchor is kind of saying that the computation to produce a smart human is mostly distributed over evolutionary history. And the lifetime anchor is kind of saying, the computation to produce a smart human is mostly done over the human lifetime.

**Daniel Filan:**
That seems really similar to me to the question of nativism versus a blank slate view in cognitive psychology, or developmental cognitive psychology. I'm wondering, do you think that it's the same? Do you think that we can take insights from developmental cognitive psychology to adjust our weights on these anchors?

**Ajeya Cotra:**
I think it's definitely related and there are definitely people who ... People do invoke this, particularly people who believe it's going to be particularly easy or particularly hard, definitely invoke their interpretation of cognitive psychology as a data point there. That's not something I looked at much, and most of my thinking about the lifetime anchor is like what I said before, in terms of it would imply it's already super cheap, it doesn't seem in line with extrapolations from existing models, so I'm downweighting it.

**Ajeya Cotra:**
I didn't like dip into that literature much, and that was deliberate because I thought it would be a huge mess as social science and psychology often are. I think there's probably something to glean from there, but I would be nervous about big swings on that basis, because I don't really know what the shape of things are. One point is that, even if we're pretty blank slate-y in the sense that a lot of the specifics of our behavior turns out to be learned, that has a complicated relationship, I think, with the lifetime anchor and evolution anchor. Because there might be this thing where our learning is just really efficient and that is what has been hard-coded by evolution.

**Ajeya Cotra:**
We have good heuristics for learning from a small amount of data, and that's something that's just true of humans as far as I can tell compared to the models we have today. And so if it's, we have a sophisticated learning algorithm and that sophisticated learning algorithm took a long time for evolution to find, then it wouldn't necessarily look like strong nativism. It wouldn't look like we have like all these random biases to be scared of tigers because that's been hard-coded in. But it would not mean that you could just do the lifetime anchor.

**Daniel Filan:**
Yeah. I guess this would look more like a version of nativism where you think that humans have this prior, that objects exist and people exist and they have intentions or something. Actually, it now occurs to me that maybe because you managed to make this update against the lifetime anchor, should we just send that result to the developmental cognitive scientists and be like, we've shown that strong blank slateism can't be true?

**Ajeya Cotra:**
[laughs] I don't know. I do think that the fact that the simplest thing we just wrote down, which is these big neural networks with stochastic gradient descent appear to be a lot worse at learning things than the human brain, is some evidence against some versions of blank slateism.

**Ajeya Cotra:**
Although I don't know how much blank slateism focuses on our learning algorithms are the worst possible learning algorithms, or the simplest possible learning algorithms. I think blank slateism ... I think the arena of the debate is a little bit different, so I'm not sure how relevant it would be for them.

**Daniel Filan:**
That seems fair. Okay. One question I have about the neural network anchors. I guess in those anchors you're extrapolating how much computation would you need for a thing that was about the size of the human brain. You rely on these ... You need to make some assumption about scaling of neural networks. You quote some literature on this, I'm wondering how helpful, or how reliable do you think this literature has been? If the people working on these scaling laws were just focused on making these kinds of timelines estimates better, what should they focus on?

**Ajeya Cotra:**
Yeah. I think the jury is a little bit somewhat out on how reliable they are. I was primarily using [this paper Kaplan et al 2020](https://arxiv.org/abs/2001.08361), which was focused on the scaling laws of language models. Since then two papers came out that seemed to corroborate it. One is [the GPT-3 paper](https://arxiv.org/abs/2005.14165), which simply extends the scaling laws and shows that they seem to still apply, where the Kaplan et al 2020 paper focused on language models from 100,000 parameters to one billion parameters. Then the GPT-3 paper extended that to 10-ish billion and then 175 billion.

**Ajeya Cotra:**
That was one piece of corroborating evidence, and then another piece of corroborating evidence is, [the same group did scaling laws for multimodal models](https://arxiv.org/abs/2010.14701), which are models that work with both images and text, and they seem roughly similar, but with a different constant factor. The exponential scaling seemed similar. And so that's some evidence, but this is super new. The multimodal scaling paper, and obviously the GPT-3 paper were very similar architectures and ultimately pretty similar types of data.

**Ajeya Cotra:**
I have wide error bars in the report on using the scaling from [the Kaplan at all 2020 paper](https://arxiv.org/abs/2001.08361) as roughly a central estimate, but having a wide spread on how it would turn out with different architectures, different types of data, and future models. I think it's been going about as well as you might've hoped in terms of reliability, but it's only been a year and it's only been tried out in a few places.

**Ajeya Cotra:**
In terms of people trying to help make this relevant to timelines, I think the most valuable thing from my perspective would be to specifically study the effective horizon length, because that hasn't been directly studied at all. But then also more broadly just do more scaling laws for more different types of models that look really different from each other, and see are they clustering? Are they spread all over the place? Et cetera.

**Daniel Filan:**
Okay. That makes sense. I guess next I have some questions about the parts of your model that aren't just the anchors. One of them is the willingness to spend on AI development. I guess this just controls how much money do you have to buy the amount of computation that would have to be necessary. I'm wondering, firstly, I think with all of these other questions, there's an implied question, which is does this uncertainty matter very much?

**Daniel Filan:**
But to the extent that the uncertainty matters, how do you figure out this willingness to spend? It strikes me that this is going to depend a lot on what the money is going to be able to buy, right? If I could spend one billion and get AGI, then maybe I would, but if I didn't think I could get AGI, then I wouldn't. Right?

**Ajeya Cotra:**
Yeah. It's a little bit ambiguous what it's referring to, and I think if I were to make the report more complete, I would try to pin this down somewhat more. In my head, the willingness to spend is conditional on people believing they have a good shot at some crazy AI system. Not necessarily that they know they will get transformative AI or AGI, but they believe they have a good shot at getting something in that zone that would be wild.

**Ajeya Cotra:**
That's the conditional on these willingness to spend estimates. Then that goes up from a billion today, which I think is a little bit low, to, it steady states at a percent of GDP. I've gotten criticism that this is both too low and too high, so it steady states at a level that's basically a trillion dollars. Then it goes slightly up, it's like $800 billion and keeps going up and eventually reaches more than a trillion dollars. Some people are like, "That's eye-popping." And, "No way." But also some people are like, "This is way less than it would be rational to spend for transformative AI."

**Daniel Filan:**
I guess at some level it can't go up more than two orders of magnitude. That seems like fewer orders of magnitude than we're dealing with. Right?

**Ajeya Cotra:**
Yeah. So-

**Daniel Filan:**
The upper end I guess it doesn't matter too much.

**Ajeya Cotra:**
The way it scales up, how quickly it like saturates at a percent or two of GDP does affect the left tail. That's something that people say that it would ramp up more quickly. The way I modeled it, I had it be ramping up as quickly as things have been ramping up recently, which is doubling every few months for the next five years. Then doubling every couple years for the next while, until it reaches 1% of GDP and then stays there. That's the curve I am expecting. So people ... Yeah. I've definitely gotten feedback on both sides

**Ajeya Cotra:**
I think the way I would try to make this better is by modeling more explicitly firm incentives, really thinking about if this firm were to make a transformative model, how much of the profit would they capture? And then, just thinking really mechanistically about where could they get loans from? How much would they be willing to approve? And stuff like that. And given what level of belief? So, I had this thing in my head of... They think they have a good shot at something crazy. Trying to pin that down a little bit more, about conditional on transformative AI being possible for a trillion dollars, what would it look like to the people that are considering whether to spend a trillion dollars? How much uncertainty would they still have? And I think that's important for whether they go all out versus... I think people are pretty risk averse at these sums.

**Ajeya Cotra:**
And so, I think I would just be kind of like, "Okay, what is the revenue that the ML industry is feeding back into investors?" And, that'll be one thing that determines how much people have available to spend.

**Ajeya Cotra:**
And then also conditional on it being possible to train a transformative model for X dollars, what are people's expectations of what would happen if they spent extra dollars to try and train some crazy model? And just getting to more of the nitty gritty of what are these actors? What are their credit limits? What would it be rational for them to spend? And then, what are the drags on doing the rational thing? How much is it that they're going to spill over - doing this would mostly benefit society versus they get to capture most of the benefits?

**Daniel Filan:**
So, I'm wondering how sensitive the model is to these assumptions. So, if I want to know, between being the most conservative estimates of how much do you think people are going to be willing to spend soon versus the most bullish estimates, I'm wondering how much does that change the probability of transformative AI by 2036?

**Ajeya Cotra:**
So if we go from one billion today to a hundred billion today, then that would change the probability in five years from 3% to 10%. It'll most affect the probability in five years because the rest of it is still the same, like it kind of converges to the same 1% of GDP. So, it's a big deal. Basically everything is a big deal, I think.

**Ajeya Cotra:**
And then, similarly, if you start it at a billion dollars, but you think that the doubling time of spending is one year instead of two and a half years, then that would basically make the 2036 number, which is what we look at, go from 20% to 25%. So that's not that huge, I guess.

**Ajeya Cotra:**
And then, if you make it 10% of GDP instead of 1% of GDP, then everything shifts up, and it mostly affects the middle and later parts because if it's in the next 15 years, then it won't have even converged to that.

**Daniel Filan:**
So, related to that, how quickly is this model assuming that we hit 1% of GDP willingness to spend?

**Ajeya Cotra:**
So where it is now, it's assuming that happens pretty late, at 2050, and that's driven by the doubling time. So, if you say the doubling time is one year instead of two and a half years, then it would get there around 2035. And then, if you say it's like three years, then it doesn't even get there until 2070.

**Ajeya Cotra:**
I don't know how useful it is for me to just be playing with a spreadsheet and rattling off numbers.

**Daniel Filan:**
Oh, I think it's useful because people... I think when people hear about these models or whatever, they mostly don't expect that there's a spreadsheet that they can play with and rattle off numbers, but with yours, there is. So, thank you for making that real. I guess that's some degree of advertising to listeners that they should play around with this.

**Daniel Filan:**
So, a different quibble you might have, a different factor that's important to some degree in these estimates is the rate of algorithmic improvement. So, in this model, in 2050, we'll need less computation than we do right now to train human level AI.

**Daniel Filan:**
One question that occurs to me is, how much did the current level of algorithmic ability or how good our current algorithms are play into this estimate? My impression was that the answer was not that much. And if that's the case, then it seems like maybe now we forecast algorithmic improvement in 2050. But if you made this model in 2050, would you have incorporated that improvement that you're incorporating now?

**Ajeya Cotra:**
Yeah. So, there's a section on this where one critique you could have of the biological anchors, I think this is a feature, but also a critique in different ways, is that it's kind of timeless. Which is that, barring some progress in neuroscience, the very basic argument I gave to you for the computational power of the human brain could have been made at any point in time since, say, 1950.

**Ajeya Cotra:**
And, while we didn't have empirically measured scaling laws, we have for a long time had machine learning theory results that suggest that probably the number of data points you need to train a model of a certain size is roughly linear in the size of the model.

**Ajeya Cotra:**
So, you could have done all this in 1950 or 1960, or whenever was the first time we had those two pieces of information. And then, you might have centered your estimate in the same place that I centered it, then. And then, basically, for my model, I would have assumed a rate of algorithmic progress that implies an order of magnitude or two, or even three cheaper now versus 1960.

**Ajeya Cotra:**
So, the way I try to account for this is basically by, one, trying to look at technologies today and how they compare to their natural equivalents and looking at how those slopes have changed. So, I'm looking at the best cameras today versus eyes or whatever that is, and then that would have looked worse in 1960.

**Ajeya Cotra:**
And then, the other way I'm trying to incorporate this is I try to have a wide enough standard deviation on the parameter, that's how big is the transformative model relative to the brain, such that, shifting it around by 10 years worth of algorithmic progress in either direction, has a lot of overlap with the current distribution. So, basically, a 10 year shift doesn't change much. So, I tried to set the width such that that's the case.

**Ajeya Cotra:**
So, those are the two things I did where it's definitely true that if I had done this whole exercise in 2010 instead of 2020, I wouldn't have been able to really fine-grainedly determined that it should be a half order magnitude more than the human brain rather than one. But the width of that probability distribution is such that it can absorb that much fuzziness because shifting it by 10 years worth of algorithmic progress in either direction, doesn't change the big picture much.

**Ajeya Cotra:**
So, that's kind of technical. Those are the two things I did to try and address that critique. And then, in terms of to what extent do I take into account the current level or how expensive I think it is now in extrapolating how much algorithmic progress I expect there to be in the future, I do this to some extent. I have different rates of algorithmic progress for each of the different paths, and the ones that are really cheap now, I assume, are closer to the best they can be. And so I give them less room to improve, basically.

**Daniel Filan:**
On thi topic of algorithmic improvement, how confident do you think we can be about this rate of improvement? How good a guess do you think we have?

**Ajeya Cotra:**
Not very good. I think it's probably the worst of the three non-anchor estimates, which are essentially hardware price, willingness to spend, and algorithmic progress. I think we know the least about algorithmic progress. And it's also the most variable across types of algorithm, and so I basically use two sources. It's very not studied. I basically use two sources for it. One is [OpenAI's 2020 blog post](https://openai.com/blog/ai-and-efficiency/) on how much cheaper it is to achieve the performance on ImageNet that we achieved in 2012 if we had to do it again now as an estimate of the rate of algorithmic progress. And then, an older [report by Katja Grace called Algorithmic Progress in Six Domains](https://intelligence.org/2013/08/02/algorithmic-progress-in-six-domains-released/) that looks at a bunch of different domains, various games, factoring, et cetera, and tries to measure a similar thing, which is basically, how much cheaper is it to achieve past performance with current techniques on the same hardware?

**Daniel Filan:**
So, there are a few sub-parts of this that I have questions about. So, one of them is that in the model, algorithm progress is following this logistic curve. So, that basically means that it's exponentially improving for a while, but eventually that improvement is going to slow off and going to taper off at a maximum. And the thing about predicting logistic curves is it's really hard to curve fit when you're in the part where it's exponentially improving because you get very little information about when it's going to start leveling off. So, I'm wondering how you went about doing this and how much uncertainty do you think there is in these numbers?

**Ajeya Cotra:**
So, the answer is a little bit different for the hardware and the algorithms, both of which are logistic curves. So, on hardware, I was just like, "Well, over the last 75 years, we've seen 12 orders of magnitude of progress." There's reason to believe that we're kind of tapping out a little bit, but there's also reason to believe there are other techniques like optical computing that uses light instead of silicon chips or other things that would let us keep pushing forward.

**Ajeya Cotra:**
So, I was basically like, "We're probably in a slowing regime," and I just assumed, or guessed, that the progress we would see over the coming 80 years would be about half as much as the progress we have seen in the last 75 years in log-space. So, I was like, "Okay, let's say we get another six orders of magnitude of progress versus the 12 that we've seen in the last seventy-five years." So, that's what I did there. And then, for the other one, I just basically made up algorithmic progress tapping out points for the different paths, the different biological anchors, with very little to go on, basically nothing to go on.

**Daniel Filan:**
How sensitive are the final answers to plausible variations in these?

**Ajeya Cotra:**
So, they're more sensitive to the rate of progress than they are to the cap. Like I said before, I care more about stuff that comes before the median than stuff that comes after the median, and all of the caps I've chosen for all of these parameters tap out in like 2060 plus.

**Ajeya Cotra:**
So, loosening them would mean that the probability climbs more linearly past 2060 and reaches a higher peak in this century, perhaps, but wouldn't super effect the probability prior to 2050, which is my median. And then, of course, tightening the caps would mean that it flattens out earlier, and the median is pushed later.

**Daniel Filan:**
So, another axis on which people might be skeptical of this is there's this question of... When you're thinking of logistic progress curves, I think you're often thinking of, there's some thing that we can do, and we're getting better at it, at some rate. But, a type of innovation which seems potentially different is breakthrough, so zero to one type thing where before no computers existed, and now you have at least one computer. So, I'm wondering, to what degree you think that kind of progress is going to be important. And do you think that the logistic curves are successful in modeling that or something else?

**Ajeya Cotra:**
So, I make some attempt to model this in the form of the probability - I have a probability that none of the paths are going to be enough, which I start at 10% in 2020, and then I basically taper down to 3% in 2100. And then, that decreasing probability basically just gets distributed proportionally to the probabilities on all the other paths, basically.

**Ajeya Cotra:**
So, a thing I don't model is probability across the paths changing, which I think would be a way to model breakthroughs. So, you could be like, "In 2020, there's a 5% chance we know how to make lifetime anchor work, but by 2080, there's a 50% chance we would have figured it out". So, that's something you could do. I didn't model that because I basically would have had to do some made up smooth progress of the probabilities, and that seemed kind of complicated. And also, I didn't super personally believe in that being an important part of the story.

**Ajeya Cotra:**
The way I conceptualize this analysis is I'm trying to figure out an amount of computation that really seems like it should be enough, doesn't require us to know more things. The foundation of the analysis, which it won't be useful to you if you don't believe this, is that there is an amount of computation somewhere in this vast biological range, such that, if that were just dropped on us today, within a few years, we could figure out how to train a transformative model. And so, if your probability on that is like 10% or something, then this whole model will cap your probability of transformative AI anytime this century at 10%.

**Ajeya Cotra:**
And so, if you have that belief, then you're more likely to just basically feel like we need algorithmic breakthroughs. My view is that we could have algorithmic breakthroughs, and that's a reason that my estimate might be conservative, and it might come sooner, but not that we need algorithmic breakthroughs.

**Daniel Filan:**
Sure. And, I guess related to that, the thing you do about reducing the probability that none of the anchors work from 10% to 3%, I'm wondering how much of the model's predictions, how much of the contribution from the algorithmic progress components come from this annealing from 10% to 3% versus the logistic curve progress.

**Ajeya Cotra:**
Let's see what happens if we just keep it at 10%. It seems like it's a pretty tiny difference versus the logistic. Well, so, if it stays at 10%, then that has the final flat point in 2100 at 70%. And then, it also makes it look bizarrely flat from 2080 to 2100. I think that's the biggest change. It just looks bizarrely flat. And then, if you do the annealing, then you end up at something more like 80% by 2100 and more of a slope from 2070 to 2100.

**Daniel Filan:**
And what if you just zero out logistic curve progress?

**Ajeya Cotra:**
Let's see if we... Maybe I should have a copy to play with because I don't want to forget what I said here. So, you're saying what happens if there's just no algorithmic progress?

**Daniel Filan:**
Yeah.

**Ajeya Cotra:**
It's just flat.

**Daniel Filan:**
Yeah.

**Ajeya Cotra:**
Okay, so, let's just have it be zero...

**Ajeya Cotra:**
So, there's no algorithmic progress. This makes a considerably bigger subjective difference, just looking at it. For example, in 2100, it's only 60% if there's no algorithmic progress. And then, in 2036 it goes from like 20% to 12%, and it has more impact through that left tail. Whereas the other one, I think, really mainly had an impact on the right tail.

**Daniel Filan:**
So, I guess, that makes it seem like in this model, the sort of breakthrough progress, or any progress that's hard to model via logistic curves...

**Ajeya Cotra:**
Is playing a relatively small role, yeah.

**Daniel Filan:**
Good to know. So I guess now I have some higher level questions about the model. So, one of them is... This is a draft report. When, if ever, do you expect it to be finalized?

**Ajeya Cotra:**
I'm really not sure about that. There were a couple of reasons we wanted to make it a draft report. One was, I'd been working on it for a really long time, and I was just very, very sick of it. And so, I have some feelings about what would need to change and who I would need to consult and stuff to put it on Open Phil's website and make it Open Phil's views. And that just didn't seem like a worthwhile endeavor to me at that time. I was pretty burned out on it and wanted to move on to other things.

**Ajeya Cotra:**
And two is that we had concerns that I don't feel super inside view bought in on, but a number of people I respect are, that making it seem too official would be an info hazard, and it might get picked up on Twitter. Some tech person might tweet it or something if it seemed too slick, and that this would encourage more investment in AI capabilities.

**Daniel Filan:**
So speaking of info hazards, I'm not sure if we said this yet, but like what's the median date at which this model says we're going to get transformative AI?

**Ajeya Cotra:**
So, my median is about 2050, which is a little bit... The model actually says 2053, but I rounded it.

**Daniel Filan:**
Close enough. So, if we get transformative AI in 2050, and not before, I expect to be alive by then. We talked a little bit about what kind of Open Phil decisions would be different depending on this analysis. What life decisions should I make differently based on this?

**Ajeya Cotra:**
So, I feel a little bit uncomfortable speaking to this as someone who works at Open Phil, and I mostly try to focus on the altruistic rather than the prudential strategy question. And I think that's plenty to focus on.

**Ajeya Cotra:**
I will say, from a personal perspective, it is freaky to me, and I think about it a lot. And I'm like, "Wow."

**Ajeya Cotra:**
I have not made any particular decisions differently based on it, but I've thought about whether people should think differently about saving and investment in particular, about should we invest in AI companies because we sort of believe more than the market does? Maybe we believe more than market does. It's actually unclear out at 30 years, but maybe we are more patient than the market, or something like that.

**Ajeya Cotra:**
So I have a number of friends who have disproportionately up-weighted hardware companies and AI companies and stuff in their personal portfolio. I don't do much fancy money management of anything myself. It's just not worth it for me, but that's definitely a salient thing that a number of people have thought about.

**Daniel Filan:**
I'm speaking for me and definitely not for Open Phil. I guess it makes me a bit more excited about cryonics or crazy life extension technology.

**Ajeya Cotra:**
Interesting. I would've thought it would make you less excited because you're not even going to be legally dead by the time. Wouldn't AI seeming to come sooner make those things marginally less attractive?

**Daniel Filan:**
If you think you're going to be alive, and then we're going to have AI, and then there's just going to be way better healthcare when I'm old, so I'm not even going to hit the freezer, so to speak. I guess it makes it more probable that cryonics would eventually work but less probable that you're going to use it.

**Ajeya Cotra:**
I guess that's what I would say. Well, and it also makes it seem like... I feel like takeoff will probably be at least to some degree gradual, or the run-up. I feel like takeoff is kind of an ambiguous word. I feel like the belief I have that there's a 50% chance of transformative AI by 2050 ripples back to feeling like there's going to be some pretty cool advances in a number of different fields in 2030 and 2040.

**Ajeya Cotra:**
And so, I think that if you're betting on something like cryonics, which I'm not personally signed up for, but I've considered it and thought about it, then you would probably believe that the technology for whatever it is is improving pretty quickly while you're still young, so you might as well procrastinate.

**Daniel Filan:**
And speaking of that, what do you think - I think there are two timing questions people are super interested about in AI. The first is when, and the second is, how soon will we know? Is there going to be a sudden really surprising takeoff? Or is there going to be a gradual takeoff? Whether or not that's slow, I don't know. But what do you think - do you think this analysis says anything about what takeoff speeds would be expected to be?

**Ajeya Cotra:**
Not super directly. I do think that a belief that takeoff is to some extent... So, I earlier mentioned that I down-weighted super, super soon paths because it feels like people would have already attempted to train a transformative model if it were actually that cheap. A related thing that contributes to the general down-weighting is that it feels like we would have more lucrative applications if it were actually so cheap to train a truly transformative model, even if we didn't fully have transformative model.

**Ajeya Cotra:**
So, my on-priors expectation of at least some degree of gradualness, staying agnostic about whether there's a slow rise and then a sharp rise, feeds into how I spat out these timelines, as opposed to... I think if I had been minimally efficient market-y and expecting suddenness, I think it would have shifted years sooner, maybe a full 10 years, but probably at least about five years.

**Ajeya Cotra:**
So, that's one cluster of things in terms of how it relates to take off. I think it doesn't specifically say anything about takeoff. I think doing this exercise and just thinking a lot about ML and its potential applications made me feel like I had a bit more of a picture of useful pre-TAI things that could be built, but that's not reflected in the report or any kind of direct consequence of the report.

**Daniel Filan:**
Speaking of the report, if you think about the actual uncertainty about what uncertainty one ought to have about when transformative AI will be developed, how much do you think the uncertainty between different models of how you might forecast this compares to the uncertainty within a model? And I guess this relates to, if I want to know more, should I tweak these parameters a lot? Or should I get a better view of how I should think about it in the first place?

**Ajeya Cotra:**
So, I think the two main other approaches that I would put into a family with this one are, one, surveying experts, or surveying machine learning researchers, people who think about AI. It's kind of unclear who the experts are. Maybe they're AI researchers. Maybe they're economists. It's surveying people.

**Ajeya Cotra:**
So I was saying earlier about how individuals who do the subjective impressiveness extrapolation often seem overconfident to me, and it's really uncertain what the y-axis is, but a natural way to implicitly get an ensemble of different y-axes is to survey people. So the best version of the survey method that I'm aware of is [Katja Grace's 2017 surveying](https://arxiv.org/abs/1705.08807) of a few hundred, I think, ML researchers who had submitted papers to NeurIPS. That was the selection criteria.

**Ajeya Cotra:**
And it is broadly in line with the final distribution spat out by my model, in that I think it was 70% or 80% by this century and 50% by 2050 or 2060. But the survey had lots of indications that people weren't thinking super hard about the question. Slightly different framings changed answers a lot. So I feel like that has a place, and I think I would be interested in better surveying methods that confront people with some of the inconsistencies that the Grace et al. survey revealed and try and help people get to reflective equilibrium and then report that. So I'm pretty excited about that general genre of things.

**Ajeya Cotra:**
And then the other method, which I think of as a sanity check, is sort of very ignorant priors, something that's kind of like, imagine that you had just gone and hidden in a cave in 1950 when the field of artificial intelligence was starting up, and you just knew about these really high level variables. We haven't built AGI yet, and here's how many resources we've thrown into it. So, a colleague of mine just put out [a report on that semi-informative priors approach](https://www.openphilanthropy.org/blog/report-semi-informative-priors).

**Ajeya Cotra:**
Actually, there's a fourth one that I think is also a different sort of sanity check, which is that our definition of transformative AI implies that there would be this intense spike in growth, where growth is 10X faster than it is now, and it keeps getting faster after that. So, there's a question of does economic history or economic modeling rule that out, or just mean that it's very unlikely that it would happen that way?

**Ajeya Cotra:**
And so, a colleague of mine, David Roodman, did [an extrapolation of long-run growth](https://www.openphilanthropy.org/blog/modeling-human-trajectory) from 5,000 or 10,000 years ago to today and basically showed that this kind of super exponential growth where the growth rate itself is increasing is a good fit for long-run history, even though it's not a good fit for short run history like the last hundred years. And we have a forthcoming report that goes into more detail on that, talking to economists and trying to adjudicate between the sort of mainstream economics picture of growth out to 2100, which is pretty exponential, versus this picture laid out by David Roodman that it very well could be super exponential.

**Daniel Filan:**
I had gotten the impression that it was known to the economic mainstream that if you had these endogenous models where growth lets you have a larger population or have some kind of technology that improves growth, then you get these kinds of like hyperbolic things.

**Ajeya Cotra:**
I would say it's known to the economic mainstream in that there are a number of economists that specifically study that question and come out with that conclusion. But I would say it's not really permeated all of the sort of subgroups of economists that try to forecast growth out to 2100.

**Ajeya Cotra:**
So, one of the kind of key resources in the forthcoming report is this comprehensive survey of economists that asked them to give probability distributions over growth out to 2100. And their distributions really well fit a normal distribution centered around 2% annual growth or something like that. They weren't prompted with the information, which, like you said, is, in some sense, economic, mainstream knowledge about the long-run history of growth and endogenous growth models and stuff. So, it's not in the water among economists like, "Oh, we very well could have 10% growth, 20% growth sometime this century, if only we had certain technologies."

**Daniel Filan:**
My impression was that... Some economist is going to listen to this and get mad, but my impression was that the mainstream view is that endogenous growth models imply this, but maybe it's hard to guess the parameters of them, or it's that maybe they were unsure how that translated into what's going to happen in the next 50 years or something.

**Ajeya Cotra:**
I'm definitely not an expert on what the mainstream discourse is. The forthcoming report that I mentioned goes over various endogenous growth models and talks about what would happen if you plugged in an assumption that you could automate all the tasks that a human could do into those growth models. And basically, the conclusion is that it's very plausible that, conditional on AI that can automate most of the tasks that humans do, then we would get explosive growth. But, in particular, that outcome isn't ruled out by economic models or other kind of economic knowledge.

**Daniel Filan:**
There's [a Hanson paper on this, right](https://mason.gmu.edu/~rhanson/aigrow.pdf)? [NB: the paper DF was thinking of specifically uses an exogenous growth model, not an endogenous growth model]

**Ajeya Cotra:**
Yes. So, [the David Roodman paper that I mentioned](https://www.openphilanthropy.org/blog/modeling-human-trajectory), which is different from the forthcoming report, was inspired by the Hanson paper and other attempts to extrapolate long-run growth. So, [the Hanson paper extrapolates growth as a sequence of exponentials of increasing rates](http://mason.gmu.edu/~rhanson/longgrow.pdf) [this is a different paper than DF was thinking of, but is more related to the topic at hand]...

**Daniel Filan:**
Growth modes.

**Ajeya Cotra:**
But the Roodman paper and some of the other older extrapolations are basically a smoothly accelerating hyperbola.

**Daniel Filan:**
I was thinking of a different paper. Maybe it's not endogenous, but there is a paper that tries to estimate economic growth when you have machine intelligence, right?

**Ajeya Cotra:**
Well, there's an [Aghion paper](https://www.degruyter.com/document/doi/10.7208/9780226613475-011/html), and I think there's a [Phil Trammell paper](https://globalprioritiesinstitute.org/philip-trammell-and-anton-korinek-economic-growth-under-transformative-ai/), or maybe they're the same paper. I'm really not an expert in this stuff.

**Daniel Filan:**
Sure. All right.

**Ajeya Cotra:**
The four perspectives that I find helpful are this bio anchors perspective, surveying people and particularly surveying people and trying to help them achieve reflective equilibrium or iron out inconsistencies. The ignorance prior stuff and the economic extrapolation stuff. And the last two I would take as sanity checks on the first two or something where I would be worried if there were large divergences.

**Daniel Filan:**
I have a question about the form of the report, there's a bunch of pages, but it's based around this probabilistic model that you have this Monte Carlo software that actually gets you full distributions. And as listeners have heard, you can just type in numbers and quickly get new probabilities. I'm wondering how did you find the experience of working with that software compared to... I guess another thing you could do would just be to propagate point estimates or something?

**Ajeya Cotra:**
Yes. It was very important to me to propagate uncertainty in how much computation would it take to train a transformative model question, but actually there's one section that's like a Monte Carlo for that. And then those distributions are spat out into a spreadsheet. And then the other stuff that I was mentioning, which is algorithmic progress, hardware and willingness to spend are actually point estimates or they're forecasts that are point estimates, there's a point estimate for each year. I did a hybrid, spreadsheets are really nice for their speed and their simplicity, and I expect most people to mostly use the spreadsheet where the information from the Monte Carlo is dumped into the spreadsheet. And then the Monte Carlo, it's just Python. I just coded stuff up in NumPy. And I thought that was a good experience. It was reasonably easy to do. I tried to have a gloss over it where people can enter numbers without having to deal with raw code. But the backend is just Python that I wrote from scratch.

**Daniel Filan:**
Okay. I'm wondering, if listeners are interested in probabilistic programming languages or Monte Carlo simulations, I'm wondering how could the experience of using them be made better?

**Ajeya Cotra:**
Yeah. I feel for me, just as someone who happened to learn Python and didn't happen to learn other languages and is not really that interested in doing so, it's hard to have it still be a programming language with the cumbersomeness of being a programming language but enough better than NumPy that I would actually switch over, I think that's hard. Something that would be cool is if I could just draw distributions with my fingers and then turn them into distributions on the backend. I'd use that. And then I've tried, both [Guesstimate](https://www.getguesstimate.com/) and [Elicit](https://forecast.elicit.org/) are attempts to do this, but for me, both of them fell a little bit short in that they were just... They were not enough easier than programming, but they sacrificed a lot of flexibility.

**Daniel Filan:**
All right.

**Ajeya Cotra:**
So I didn't end up using either of them. Or actually elicit, which is this thing that lets you specify probability distributions and lets you see other people's distributions for the same question is now integrated into my Monte Carlo, but it was done after the fact after I'd made [inaudible]. And then they basically converted my code into the Elicit format. I didn't do it natively in Elicit.

**Daniel Filan:**
Okay. Sure. And I guess another, a more broad question I have is, what do you think about if people are interested in providing the complements to this kind of research, so they don't want to work on forecasting themselves, but maybe they could nudge their research to be a little bit closer to things that would be helpful with this kind of work, what should they do?

**Ajeya Cotra:**
Yeah. We actually have been working with a grantee since we made this report to do forecasting on algorithmic progress, which I mentioned there hasn't been... Or not forecasting directly, but data collection on algorithmic progress, which there hasn't been as much of systematically. What he's doing is basically doing some process to, I think, talk to companies I'm not totally positive on this. Talk to companies, figure out what fundamental algorithms are most important for their work and then trace the history of attempts to solve that problem. Let's say constraint satisfaction algorithms are really important or search algorithms, that's easiest. Search algorithms are important. And then you can see, for trying to solve a particular search problem, what was the asymptotic performance in 1950? And then you can trace, oh, this person developed this algorithm and the asymptotics went from N^3 to N^2 log(N) and so on.

**Ajeya Cotra:**
And then basically, trying to figure out how that maps onto absolute performance based on the likely problem sizes. You can convert asymptotic performance into actual performance. And then basically, he's drawing these curves that are like the curves that I'm drawing for the different biological anchors for the past. And what's cool is that sometimes you'll be able to both track the best algorithm we have for something and the tightest lower bound we've proved can exist. With sorting or something, a trivial lower bound is N and then later people proved an N log(N) lower bound at the same time as people in fact developed N log(N) algorithms. And so then you can know that you're out of progress, asymptotic progress at least. And then you can maybe think about approximations and so on or shaving off constant factors.

**Ajeya Cotra:**
He's just collecting a big data set of that and that kind of thing is really useful. Collecting big data sets of things like algorithmic progress, hardware progress. For this one, I think it's less about collecting big data sets, which already have been pretty nicely collected and more about thinking about the asymptotics and what physical limits are there in hardware progress. I guess this is not quite peripheral to this research. It's basically trying to nail down one of the important parameters. Maybe that doesn't quite answer your question, but I would be very excited for a piece that was just like, here is where hardware progress will tap out and here's when I think that will happen and why.

**Daniel Filan:**
Now that you're done, well, you've been done with this model for a while by now, right?

**Ajeya Cotra:**
Yeah.

**Daniel Filan:**
What else do you do at Open Phil? What do you generally do there?

**Ajeya Cotra:**
Yeah, the answer to that I think changes substantively every year, couple years. While I was working on that report, that was the main thing I was doing. And there were other things around the edges. And before that I worked on various other types of quantitative models, I think that's just been a general theme of my work at Open Phil, in different cause areas and for different purposes. I was working on high level, last dollar estimates where a last dollar estimate is an estimate of how much good we can do with the last dollar we'll ever spend. Which we want to know, because if we spend an extra million dollars that we weren't planning to spend in a particular year, then what that trades off against is that last thing, sort of taking money from the end and spending it now.

**Ajeya Cotra:**
If we have an estimate for the last dollar that gives us a benchmark where we want to fund everything that comes along, that's better than that. And nothing that comes along that's less good than that. I did very crude, lower bound-y estimates for the last dollar a couple years ago, before I started working on this report. And then I'm trying to revisit that now and get a better estimate, but getting a better estimate turned out to be really hard. I'm more pivoting now to thinking about, okay, well let's try and come up with ideas for things that we suspect on heuristic qualitative argument grounds would be good for reducing x-risk. Especially reducing AI x-risk, and try to come up with a bunch of those things and picture spending money on all those things.

**Ajeya Cotra:**
And then maybe that will inspire a better last dollar estimate. And then even if it doesn't, we have a bunch of ideas for things to fund, that we kind of have checked out that we didn't have before. That's what I'm thinking about now. Particularly, within the technical space, I'm thinking about, what are some lines of research projects that I would be very excited for hundreds of people to be doing in the future. If I could just cause hundreds of people to be doing these projects that are spelled out in enough detail that people actually understand what I mean. So It's not at the level of, go try to reduce AI risk. That's the main thing I'm focusing on. That's on what I call the program side of my work.

**Ajeya Cotra:**
And then I also spend quite a bit of time on org operations type work, especially recently where we're trying to professionalize the organization in various ways. Trying to think about, what should our hiring processes look like? Think about what should our promotion processes look like, how would we know when someone is up for a promotion, what our compensation should look like, that general type of thing. I've been working on that type of work for the last six months with maybe a third of my time. And then two thirds of my time is thinking about what stuff do we want lots of people to be doing in technical safety and how can we spell that out in more actionable detail than has been spelled out before.

**Daniel Filan:**
Okay. I'm wondering, for these concrete visions, how useful have you found this draft report in shaping that or the process of making it maybe?

**Ajeya Cotra:**
I feel like the process of making it and the intuitions I absorbed about machine learning models and some of the specific understanding of scaling laws. And then also the connections I made to researchers while working on the report, have all been really valuable. But I don't think content you would get easily from reading the report has been all that valuable. It's just kind of the peripherals in terms of, I have a computer science background as an undergrad, but hadn't been doing particularly technical things since I joined Open Phil out of undergrad. So just immersing myself in machine learning, learning about the cutting edge and talking to people, working on the cutting edge. That was all really useful and definitely gave me a warm start in thinking about this and a lot of intuitions, but not any particular thing about the timelines. It's not so much that, oh, I convinced myself that there's this much probability in this timeframe and therefore I have specific ideas that can trace back to that.

**Daniel Filan:**
Okay. One of the ideas, well, one of the things that I assume is one of these ideas is [this blog post that you've written recently about aligning, I believe narrowly super-intelligent models](https://www.alignmentforum.org/posts/PZtsoaoSLpKjjbMqM/the-case-for-aligning-narrowly-superhuman-models).

**Ajeya Cotra:**
Yeah, narrowly superhuman models.

**Daniel Filan:**
Narrowly superhuman models. Okay. Can you tell us a bit about what that project is and why you're interested in it?

**Ajeya Cotra:**
Yeah. The basic idea is that one vision for what we want to do ultimately is that we want to find ways to supervise models that are doing things that are too hard for humans to directly demonstrate or understand or evaluate. That's a goal in the sense that if we succeed sufficiently at that, then we're considerably safer than we would be if we didn't. You could imagine going down other paths too, not training superhuman models as another way to be safe, but this is one very plausible route to being safer. Just to figure out how humans in some form can supervise models that are better than them at stuff, perhaps enough better that they can't even understand the decisions the models are making very well. I think an interesting thing about our present moment is that we're just starting to have models that I think are superhuman, better than some reasonably large set of humans, at some reasonably interesting and complex and fuzzy tasks that we could try to practice what we want to do eventually in that setting.

**Ajeya Cotra:**
We've had models that are super human at games for a really long time, but the difference between games and the tasks that I'm most interested in are that we have a built-in, perfect evaluation for how well you did at the game. It's just defined as, did you win or not? But I think today we have models that say, could give legal advice, better than many MTurk workers. They know more about the law, or they could be trained to know more about the law and maybe even have associations and responses to things that are keeping in mind a broad context and a lot of knowledge about the world, that a large set of humans, maybe they're not better than lawyers, maybe they're better than lay people. A challenge that I think is beginning to be close enough to actually attempt is, how do you get these humans who are not lawyers who aren't spending that much time figuring out the law, how do you get them to incentivize legal advice models to be honest to them and fully helpful to the best of their abilities? Rather than playing weird games or just trying to predict what a human on the internet would say about the law.

**Ajeya Cotra:**
I'm really excited about that because it seems like an unusually tight connection to a pretty robust feeling long-term goal that we should have for reducing AI x-risk. And I think we are likely to learn all sorts of things from attempting to practice on something as close as we can make it to the real deal. And it's with these large language models that have a bunch of knowledge about the world, that starting to become possible is something to practice on.

**Daniel Filan:**
Okay. One way to approach this problem is to think of it kind of theoretically, and say "Well, if humans know this amount and the AI can do this..." and just prove some theorems or come up with some algorithms in theoretical space. I'm wondering, what do you think we would learn from experiment that is really hard to figure out from theory?

**Ajeya Cotra:**
Yeah. I feel - there's a variety of answers to this. One simple illustration of why it seems like doing things in practice is important is that it's pretty plausible that we live in a world where the first reasonable thing you would think of actually does work, but you need to work out a lot of kinks. It's plausible we're in a world where the way to make AGI safe and aligned is you have some well-trained human judges and you have a good way to break down the evaluation problem to make it easier for them. You've trained some helper models to pick out the data points that the judges will find most interesting or informative. And the helper models present judges with a sequence of actions to evaluate and the judges look at it and evaluate it.

**Ajeya Cotra:**
It's not like you need some brilliant insight, but you do need the judges to be very well-trained and you need to have worked out how much data you need. And you need to have worked out kinks in the optimization and just stuff that you need to just do to have practice. You need these institutions that are going to train AGI to have good, strong relationships with vendors that provide well-trained human judges. You need to know how to train them and all this stuff. I think if we're in easy world, but you need to have good execution, then that's a world that's especially favorable to this kind of research in that it seems better than theoretical research, because there's a lot more work to do here and the theory is easy by assumption in this world.

**Ajeya Cotra:**
But even if we're in a world where we need one or more major algorithmic insights that you could write down on paper. And the thing that I described with find reasonable decompositions, train helper models, have human judges, won't work for training aligned AGI. It still seems important to just actually getting it right and having the institutions in place going in parallel. I think it is particularly valuable to be checking, are we getting the most helpful behavior we can out of models at a given size? And do we know what the gap is between just training a generative model to predict text and getting that model to be as helpful as it can be, to avoid a sort of overhang-y situation where our models are basically just underperforming what they could be giving us up until the moment where they're really very smart. It's really very overkill that the models are so big, because we want to be able to have good artificial helpers at every step of the way in our attempt to align smarter and smarter models and you have to actually train them to have that.

**Daniel Filan:**
Okay, cool. A bit more broadly, on the program side, what do you think the most important questions are that Open Phil faces in terms of trying to understand the AI x-risk research landscape?

**Ajeya Cotra:**
Yeah. I don't know if I have a full ranking, but one thing that I think is really important and pretty neglected is fire alarms. Eliezer Yudkowsky has this blog post, [there's no fire alarm for AGI](https://intelligence.org/2017/10/13/fire-alarm/), which essentially says you're not going to get a warning before you train a model that's powerful enough to maybe cause humanity to go extinct. I think that's very non-obvious. And I think we should really be thinking about. Are there benchmarks we could create? Quantitative benchmarks such that if the number goes in a certain direction, then we are more scared. Or, is there a way we could document the damage caused by failures of AI systems such that we could agree on a perhaps arbitrary, but informed threshold at which to trigger various safety clauses or agree to pause a project until some problem is overcome.

**Ajeya Cotra:**
I think that is a pretty important just general area where there's a lot of things you can measure, both quantitative metrics that are trying to attempt to measure alignment, which I would be excited about, I think are hard to get right. But I would be excited if it is possible to get them right. But also things like just capabilities, model size, maybe we could agree to trigger certain safety clauses if models get good enough at understanding the training process itself. We could try to measure how well they understand the training process. Or good enough at contributing to AI research. Because then maybe we're worried about a runaway feedback loop. Or good enough at some other worrying capability. I think there's a lot that could potentially be done there in terms of constructing triggers for certain plans of action. I'm pretty excited about that. I think it's first a research problem in terms of what are the triggers? What should we be worried about?

**Daniel Filan:**
Okay. One point that is made in this blog post about fire alarms and AGI is that for literal actual fire alarms, the claim is that the point of a fire alarm is not to provide evidence that there's a fire, but the point is to let everyone know that it's okay to start worrying about the fire. And there's these references to these experiments where you just start filling a room with smoke and everyone else is in on it and doesn't react and the one person who is the experimental subject, just apparently it doesn't freak out because no one else is. They had fun psych experiments back in the day. But I'm wondering, in terms of these metrics, how much do you think that the problem is coming up with a technical indicator that is relevant? And how much do you think the problem is having it be broadly understood that this actually really matters?

**Ajeya Cotra:**
Yeah. I think they're both considerable challenges and they're very entangled. I could make up random metrics now, try and figure out the first time a model generates a billion dollars in a year of revenue or something, then we should do something. Or just if your model is 10 trillion parameters, you should just stop or something like that. I think the case that that is closely coupled with a level of risk that is imminent enough and acute enough and high enough that it's in everyone's selfish interests to really listen to it and coordinate around it, is weak. And as a result, I think it will be hard to just say stuff like that and get it to be something people coordinate on. I think it's a closely coupled research and persuasion project to make really good fire alarms. But I think they are both, actually, good Bayesian evidence that there's a acute period of risk ahead and serve the social role of a fire alarm.

**Daniel Filan:**
Okay. I'm also wondering - so, this episode is recorded during [the 2020-2021 COVID-19 pandemic](https://en.wikipedia.org/wiki/COVID-19_pandemic), I think a lot of people have had this reaction that the response was not optimal. That perhaps people should have responded sooner and it was botched in some ways. I'm wondering, how much do you think that plays into your thoughts about fire alarms?

**Ajeya Cotra:**
Yeah. I'm not sure exactly how it plays into my thoughts on fire alarms in particular. I do think that it was some sort of update in multiple different directions, but on net a negative direction for me, in terms of AI risk and how people handle AI. In the beginning of the pandemic, when people were advocating for lockdowns, I thought we would never do it because that's just so much immediate pain for everyone to spare a pretty small and potentially invisible set of people death and suffering from this disease. And it requires this draconian for the West levels of government sanctions. I was very surprised that we basically were locked down with only a few breaks for a year and a half, or a year and a couple of months until we got the vaccines.

**Daniel Filan:**
In particular, in the San Francisco Bay area, different areas -

**Ajeya Cotra:**
Yeah. I was like, wow, people actually care about this, that's wild and they coordinated to do this thing. I think that bare fact is a positive update. But a world that wasn't very salient to me before COVID and is more salient to me now is one where people really care a lot about AI x-risk and just do a lot of actually intense, coordinated things in an attempt to combat it, but just aren't thinking well enough about it for those efforts to be particularly successful. I think the failure stories in my head had been ones that started earlier where people just didn't care that much and never knew about the risk or something, or if they knew they just had chosen to eat the costs. But I think COVID made me feel there's also this swath of worlds where people, the whole world's eyes are on this problem, but there are still bureaucratic hurdles or misplaced ethical hurdles that preclude an effective response. And in some cases, are worse than the default of just not caring about it and choosing to eat the costs.

**Ajeya Cotra:**
I mean, depending on what the technical landscape looks like. That was an update to me. I don't really know how much of an update it was, because I don't know how plausible it is. I still think most of my failure stories come from people failing to ever care about it. But I feel I have some additional failure stories in my head now that look more like COVID, where people care a ton and it's also got an intense moral fervor around it. But the response is barely better and maybe worse than if we had just never locked down and just completely ignored it.

**Daniel Filan:**
Okay. Going back a bit to your work at Open Philanthropy, what does it actually look for you to come up with these projects that machine learning researchers might do and how do you think about the problem?

**Ajeya Cotra:**
Yeah. I think, very mechanically it will be someone said something to me or I thought of something. I write it down in a couple of paragraphs, I show it to a bunch of people. People have objections. It's very iterative. I talk a lot and argue a lot with technical advisors, with people who Open Philanthropy either contracts with or have a close volunteering relationship with Open Philanthropy who are researchers in AI and AI alignment. It's just a lot of talking and writing things down and getting feedback on the things I wrote down. Definitely having this network is a super important part of being able to come up with reasonable ideas. I don't know if that was the right level of abstraction for an answer to that question.

**Daniel Filan:**
Yeah. I guess maybe another way to ask it is, imagine a listener wants to be able to do... Suppose they want to copy you or something. They want to be the next Ajeya and they want to figure out how do I need to think about the existential risk landscape in order to produce the same results? How should they do that? Which is kind of a big question, I guess.

**Ajeya Cotra:**
I feel my path was pretty idiosyncratic. Assuming this person is a generalist and not already really technically skilled up in machine learning. I think an easier in-road is trying to answer one of the many open questions in the timelines report. Trying to really nail hardware forecasting, or really nail algorithmic progress some way. And I think that if it's good is adding direct value and it's also getting you noticed. And in terms of thinking about technical research directions, it feels to me you have to have the network that I mentioned at some point. Both because having the network makes your ideas much better and because having the network makes your ideas implemented, have more of a chance to be implemented.

**Ajeya Cotra:**
In my head, I have particular individuals I'm trying to convince of particular things because I hope it will change their research or change what they tell their students to do or cause them to endorse something I write about it, causing more people to do it. It's weird and I have no idea if it will be particularly successful for me to be doing this, not as a researcher or a full-time technical AI safety researcher myself, and might just flop. I don't know. I'm not looking back on an already successful project. But the reason I feel it's worth attempting is that I have people I can talk to who are technical safety researchers who will both give me feedback on my ideas and make it so that if they like an idea it's more likely to be implemented.

**Daniel Filan:**
Okay. It's been a good conversation. I hope people have gotten a better sense of the form of this model and I guess perhaps one facet of the Open Philanthropy view of artificial intelligence. Ajeya, thanks for appearing on the podcast and to the listeners. I hope you join us again.

**Ajeya Cotra:**
Thanks so much, Daniel. It was great to be on here.
