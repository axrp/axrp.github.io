---
layout: post
title: "37 - Jaime Sevilla on Forecasting AI"
date: 2024-10-04 13:40 -0700
categories: episode
---

[YouTube link](https://youtu.be/bmJJ0WiPhQ8)

Epoch AI is the premier organization that tracks the trajectory of AI - how much compute is used, the role of algorithmic improvements, the growth in data used, and when the above trends might hit an end. In this episode, I speak with the director of Epoch AI, Jaime Sevilla, about how compute, data, and algorithmic improvements are impacting AI, and whether continuing to scale can get us AGI.

Topics we discuss:
  - [The pace of AI progress](#pace-of-ai-progress)
  - [How Epoch AI tracks AI compute](#how-epoch-ai-tracks-ai-compute)
  - [Why does AI compute grow so smoothly?](#why-does-ai-compute-grow-so-smoothly)
  - [When will we run out of computers?](#when-will-we-run-out-of-computers)
  - [Algorithmic improvement](#algorithmic-improvement)
  - [Algorithmic improvement and scaling laws](#algorithmic-improvement-and-scaling-laws)
  - [Training data](#training-data)
  - [Can scaling produce AGI?](#can-scaling-produce-agi)
  - [When will AGI arrive?](#when-will-agi-arrive)
  - [Epoch AI](#epoch-ai)
  - [Open questions in AI forecasting](#open-questions-in-ai-forecasting)
  - [Epoch AI and x-risk](#epoch-ai-and-x-risk)
  - [Following Epoch AI's research](#following-epoch-ais-research)

**Daniel Filan** (00:00:09):
Hello everybody. This episode I'll be speaking with Jaime Sevilla. Jaime is the director of Epoch AI, an organization that researches the future trajectory of AI. In this conversation, we use the term "FLOP" a lot. "FLOP" is short for "floating point operation", which just means a computer multiplying or adding two numbers together. It's a measure of how much computation is being done. Links to what we're discussing are available in the description and you can read a transcript at axrp.net. Well, Jaime, welcome to AXRP.

**Jaime Sevilla** (00:00:36):
Thank you for having me here.

## The pace of AI progress <a name="pace-of-ai-progress"></a>

**Daniel Filan** (00:00:38):
So you study AI, how it's progressing. At a high level for people who have been living under a rock, how's AI doing? How's it progressing?

**Jaime Sevilla** (00:00:47):
It's going pretty fast, Daniel. I think that right now the two things that you need to take into account when you're thinking about AI is how fast inputs into AI are going, and how fast outputs are going, how much is being achieved. In terms of inputs the progress is exponential. The amount of compute that's being used to train modern machine learning systems is increasing at a staggering pace, nearly multiplying by a factor of four every year.

(00:01:15):
In terms of the outputs, we also have seen some dramatic advances. This is a bit harder to quantify, naturally, but if you look at where image generation was four years ago, and you compare it with today... Today we have photorealistic image generation, whereas before it was these blobs that you could make that were vaguely related to the text that you were entering. And in text, we have also seen these very dramatic advances, where now I use, and I suppose that many in the audience will be using, ChatGPT daily to help them with their tasks and coding.

**Daniel Filan** (00:01:49):
Yeah, if people are interested in statistics about what's going on with AI, one thing I really recommend they do is: you have this dashboard on your website. Is it called "Dashboard" or is it called "Trends" or something? I forget what.

**Jaime Sevilla** (00:02:03):
Yeah, we call it the ["trends dashboard"](https://epochai.org/trends).

**Daniel Filan** (00:02:05):
Trends dashboard, okay. So that's one thing that people can use to get a handle on what's going on. One question I have in this domain is: so I have an [Anki](https://en.wikipedia.org/wiki/Anki_(software)) deck. It's basically a deck of flashcards, and sometimes it shows me a flashcard and I say whether I got the answer right, and then it shows it to me very soon if I got it wrong, or some period of time later if I got it right. Anyway, in this deck, I've got some flashcards of how big are various books or whatever, just in terms of number of words, to give me a sense of word counts. I also have how many floating point operations were used in training GPT-3. I'm wondering: if somebody wants to have a good quantitative sense of what's going on in AI, what should they put in their flashcard deck?

**Jaime Sevilla** (00:02:56):
The two main things you need to put in your Anki deck are: one is the number I already gave you, which is the increase in training compute per year. Right now, one of the best predictors that we have of performance is the scale of the models. And the best way of quantifying the scale we have found out to be the amount of computation that's being used to train the models, in terms of the number of floating point operations [that] are performed during training. This is increasing right now for notable machine learning systems at the pace of 4x per year. And if you look at models at the very frontier, you also find a similar rate of scaling. So I will recommend you put that number [in the deck]. But that's not the whole picture, because alongside the compute, there are also improvements that we have seen to architectures, ways of training, to all these different techniques and scientific innovations that allow you to better use the compute that you have, to train a more capable system. So all of that we usually refer to with the name of "algorithmic improvements".

(00:04:05):
And we had [this cool paper](https://epochai.org/blog/algorithmic-progress-in-language-models) where we tried to quantify, okay, what's been the rate of algorithmic improvement in language models in particular? And what we found is: roughly the pace was that the amount of resources that you need, the amount of compute that you need to reach a certain amount of performance was decreasing at the rate of about 3x per year, roughly. And I actually have a fun anecdote about this, which is just this week [Andrej] [Karpathy](https://en.wikipedia.org/wiki/Andrej_Karpathy), he's been working on [this project](https://x.com/karpathy/status/1811467135279104217) where he's trying to retrain GPT-2 using modern advances in architectures, at a much cheaper scale. And he estimated... he said this number which is, "well, we don't know exactly how much GPT-2 cost when it came out in 2019, but I estimate that it cost around $100,000 to train." But with all these techniques that he has applied, he trained a GPT-2-equivalent model for about 700 bucks.

**Daniel Filan** (00:05:14):
Wow. That's a lot cheaper. Let me just get into some of this. So in terms of compute... this is going off ["Training Compute of Frontier AI Models Grows by 4-5x per Year"](https://epochai.org/blog/training-compute-of-frontier-ai-models-grows-by-4-5x-per-year) - very descriptive title - by yourself and Edu Roldán. Growing the amount of computation used in training AI models by 4-5x per year: that seems kind of insane. I don't know if you know the answer to this question: is there anywhere else in the economy where the inputs we're putting into some problem are growing that fast, that isn't just some tiny random, minuscule thing?

**Jaime Sevilla** (00:05:58):
Yeah, this is an excellent question to have. I don't have any real examples off the top of my mind, but it will definitely be interesting to hear if there are any analogs to this.

**Daniel Filan** (00:06:08):
Yeah, yeah. If any listeners have some sort of quantitative econ bent, I'd love to know the answer. So how did you figure that out?

**Jaime Sevilla** (00:06:21):
Pretty simple. So this all traces back to even before [Epoch](https://epochai.org/), I was a bit frustrated with the state of the art in talking about how AI is going. Because people were trying to be not very quantitative about it. Where we were already living in this world where we already had the systems. We could already do a more systematic study of what's happening. So I actually started together with my now colleague, [Pablo Villalobos](https://x.com/pvllss), writing down information like, okay, this is a hundred important papers in machine learning, this is the amount of resources that were used to train the model, the size of the models, and such and such. And this project has continued up until now.

(00:07:07):
And now at Epoch we have taken on the mission of keeping this database updated, of notable machine learning models throughout history, and annotating how much compute was used to train these models. At many points, there's a lot of guesswork involved. People don't usually report these numbers very straightforwardly. So we need to do a bit of detective work in figuring out, all right, in which kind of cluster was this model trained? Which model of GPUs did it use, for how long was it trained? And from there, make a sensible estimate of the amount of computation that was used to train the model.

## How Epoch AI tracks AI compute <a name="how-epoch-ai-tracks-ai-compute"></a>

**Daniel Filan** (00:07:48):
Sure. How well are you able to do this? My understanding is that OpenAI basically doesn't tell anyone how it made GPT-4. I'm not even sure that they've confirmed that it's using transformer architecture at all. In a case like that where they're just not releasing seemingly any information, how much can you do to figure out how much they trained it on?

**Jaime Sevilla** (00:08:13):
Yeah, so the answer here is, well, some information gets leaked eventually. There's some unconfirmed rumors, and sometimes you can use them to paint an approximate picture of what is happening. You can also look at the performance of the model and compare it with the performance of the model for which you actually know the compute, to try to get an idea of, okay, how large do we think the model is. This is obviously not perfect, and this is now a situation that's been increasingly more common for the last couple of years, in which the labs that are at the frontier of capabilities are more reluctant to share details about their models.

(00:08:57):
This is something that I'm a bit sad about. I think that the world would be a better place if we were more public about the amount of resources that's being used to train the systems. Obviously this has some implications and this is useful information for your competitors to some extent. So it's understandable to a point that labs are reluctant to share this information. But given the stakes of the development of this technology, I would be happy if we had this collective information on how many resources do you need to train a model with a certain amount of capabilities?

**Daniel Filan** (00:09:31):
Sure. So one possibility is leaks. I see how that could be useful for something like GPT-4. In the case of these frontier models, presumably the thing that makes them frontier models is for at least some of them, there are just no comparable models that you can say, "Oh, GPT-4 is about as good as this thing, and I know how much computation was used in training that thing." So can you do anything there, or is it just the leaks?

**Jaime Sevilla** (00:09:59):
So for example, let's walk through how we did the estimate for the compute of the [Gemini Ultra](https://deepmind.google/technologies/gemini/ultra/) model. So for the Gemini Ultra model, we didn't have the full details of how it was trained, but we had some reasonable guesses about the amount of resources that Google has at its disposal. And from there we made an estimate based on, okay, we think they have this many TPUs, we think that it was trained for this long. And this gives us an anchor estimate of that.

(00:10:28):
The other thing that we did is together with the model, they released results from a series of benchmarks. This is quite common. So what we did is look at each of these benchmarks, and then for these benchmarks, we already had previous estimates of the performance that other models got on these benchmarks, and the amount of compute that they had. And this allowed us to paint this picture of "this is a rough extrapolation of if you were to increase the scale of these models, what performance would we expect you to achieve in that?" From there, we backed out "okay, given that Gemini Ultra achieved this performance on this benchmark, what scale do we expect it to have?"

(00:11:14):
And then what we found is that these two ways of producing an estimate - the hardware-based one and the benchmark-based one - they look kind of similar. So that gave us confidence in saying, "Well, this is in the end a guess, and we have huge uncertainty - this could be off by a factor of five - but it seems somewhat reasonable to say that Gemini Ultra was trained with around (let's say) 5x10^25 FLOP.

## Why does AI compute grow so smoothly? <a name="why-does-ai-compute-grow-so-smoothly"></a>

**Daniel Filan** (00:11:44):
Okay, this is kind of a tangent, but I remember a week or two ago, I was looking at your trends dashboard, I think because I was going to suggest that some other people look at it. And I had to look at this number, 5x10^25. And also I was looking at [this thing](https://www.lesswrong.com/posts/6Xgy6CAf2jqHhynHL/what-2026-looks-like) this guy called [Daniel Kokotajlo](https://www.lesswrong.com/users/daniel-kokotajlo) wrote in something like 2021. It was just a vignette for what he thought the next four or five years would look like. And I got to the part of that story that was about 2024, because I was curious: how well did he do?

(00:12:24):
And there's a paragraph in that section where he says, "Oh yeah, the year is 2024. The best model that has been trained has 5x10^25 floating point operations put into it." And it was kind of freaky that that was so close. And in general, the graphs you draw of computation used in frontier models, the lines seem pretty straight. It seems like this is somehow kind of predictable, a kind of smooth process. Is there something to say about what's driving that smoothness, why is it so regular?

**Jaime Sevilla** (00:13:00):
Yeah, this is a really interesting question, and one that keeps me awake at night. Where do these straight lines come from? Right now, my best guess is... Okay, so maybe let's dive a bit into what goes into compute, what makes compute numbers go up. There are two major things that go into that. One of them is that hardware becomes more efficient over time. So then we have machines that can have a greater performance. So for a given budget of money, you can get more compute out of it. It's actually a pretty small number compared to the growth that we're seeing in compute. So improvements in hardware efficiency, at a fixed level of precision, have improved by around 35% per year among GPUs that have been used for machine learning training in the last 10 years or so. But the training compute is like 4x, right? Which is five times greater than this improvement in hardware efficiency. So what explains the rest of the difference? Well, a bit of that is because people have been training-

**Daniel Filan** (00:14:08):
Wait, sorry: the growth in compute is 4x per year, the growth in compute efficiency per dollar is 35% per year.

**Jaime Sevilla** (00:14:16):
That's right.

**Daniel Filan** (00:14:17):
Wouldn't that be 12x as much?

**Jaime Sevilla** (00:14:19):
Well, I recommend that you think about this in terms of OOMs [orders of magnitude] per year.

**Daniel Filan** (00:14:25):
Oh, okay.

**Jaime Sevilla** (00:14:25):
Because I think that that helps better paint the picture. So 4x per year is roughly 0.6 orders of magnitude. So 4x is 0.6 orders of magnitude per year, and 35% is roughly 0.12 orders of magnitude per year.

**Daniel Filan** (00:14:50):
Okay. So it's 4x in terms of the number of orders of magnitude per year?

**Jaime Sevilla** (00:14:54):
That's right. More like 5x.

**Daniel Filan** (00:15:00):
More like 5x. But you were saying that the growth in computation used is just way faster than the growth in efficiency of how well we can make GPUs.

**Jaime Sevilla** (00:15:09):
That's right, exactly. So what is happening, what is missing? Why are these numbers going so high? And I will say that there are a couple of less important factors here. Like, people have been training for longer, which matters to an extent. Also, people have been switching to formats of precision from which they can get more performance, like switching recently from FP16 to mix FP8 precision.

**Daniel Filan** (00:15:38):
Sorry, "FP16" is "floating point 16 bits". It's roughly how many significant digits you use?

**Jaime Sevilla** (00:15:45):
Yes, yes.

**Daniel Filan** (00:15:46):
And so they were using something like 16 significant digits, now they're using something like 8?

**Jaime Sevilla** (00:15:51):
Yes, that's right. But the most important factor overall is just that people are willing to put so much more money into this. And now of course this raises a natural question, which is: how do they decide how much money to put in, and why have they decided to scale the amount of money that's being put in at a rate that results in this smooth growth?

(00:16:16):
And here, I don't really have a very authoritative [answer]. I have some guesses. One of them is that, for example, there was a recent interview that [Demis Hassabis](https://en.wikipedia.org/wiki/Demis_Hassabis) gave, where he was like, "Well, when you're scaling models, this is a bit of an art. And you want to be careful that you don't train a model for very long, but then the train run goes horribly wrong. You really want to have this learning curve in which you progressively learn how to train larger and larger models, and test out your different hypotheses about which techniques are going to work at scale." So this is one story of why people are choosing to scale so smoothly, which is that they believe that they will learn a lot over the process of training smoothly, and not waste a lot of resources on a training run that might not go anywhere.

(00:17:05):
There's perhaps another explanation that we could give here, which is that doing a training run that's very large is very expensive. So again, GPT-2, in 2019, was $100,000 to train. But five years later, it was so much cheaper. So to an extent, you want to wait. You want to wait to do your very large training runs, because you're going to have much better ideas about how to make the most out of your training run, like better algorithmic innovations that help you make the most out of the compute that you have. And also to a smaller extent, you're going to have better hardware, which I've already said is not that big of a deal, but it's still a deal.

**Daniel Filan** (00:17:48):
In some sense this is a reverse interest rate. Right? Your money's more useful later than it is now.

**Jaime Sevilla** (00:17:54):
Exactly.

**Daniel Filan** (00:17:55):
That's kind of a weird thing to think. This feels like the kind of thing that somebody would've studied: what to do when interest rates work that way, or something. Maybe I'm thinking about it weirdly.

**Jaime Sevilla** (00:18:08):
So actually one thing that I just thought of... So you were telling me about this reverse interest rate, and this phenomenon where your money is so useful in the future. And one fun observation is that to an extent, this limits how long people are going to be willing to train models for. Because if your training run just takes a very long time, then at some point you will have been better off just starting later, and just doing a shorter training run, but with the increases in efficiency that will come associated with doing a training run later. One interesting analog here is that in the '90s there was this project to sequence human DNA.

(00:18:56):
I'm not sure if you're familiar with the details, but if I recall correctly, there was a first project that tried to do this, using earlier technology. And that went on for many, many years. And they were beaten by a product that started later, because some years later, there was a better technology that was so much more efficient that they were able to finish sequencing the genome much faster than the project that had started earlier. So this is a situation that might be analogous in AI: if your plan is just to do a ten-year training run, then you're going to be completely outclassed by people who start in the last year, and use these much better kinds of hardware, and much better algorithmic insights, to train a model that's going to be far better than the ten-year training run.

**Daniel Filan** (00:19:43):
Sure. So actually this gets to a question I had about this, which is: it takes some amount of time for models to train, right? And if you are deciding... Like, you have this graph with little dots, right? The X coordinate of the dot is when the model was trained, and the Y coordinate is how much computation was used. But you have to pick a date for when the X coordinate is. And as far as I can tell, if computation is growing at 4-5x per year, then it seems like it really matters if you put that dot at the start of the training run versus at the end of the training run, if it takes more than one month to train these things, which I think it does. How do you make that choice, to figure out how to think about when a model is trained?

**Jaime Sevilla** (00:20:35):
Yeah, this is an excellent question. So right now, what we do just pragmatically, is the date that we choose is the date where the model becomes public and becomes officially released. That's what we pick. Just because many times, you don't know when people really start the training.

**Daniel Filan** (00:20:56):
Yeah. I wonder if that means that there could be an apparent massive boost to these times, just by a company deciding to release their results, announce their model slightly earlier. Like, if a company decides to move the date in which they announce a model forward or backward by one month, that's going to make a difference to this trend, right?

**Jaime Sevilla** (00:21:19):
It will, absolutely. So maybe the best way of thinking about it is that one is the scale of the models that we have access to, and the other is the scale of the models that are being trained right now, or that companies are internally testing. And you should expect the models that are internal to be potentially 2x or even 4x larger than the models that exist right now.

## When will we run out of computers? <a name="when-will-we-run-out-of-computers"></a>

**Daniel Filan** (00:21:45):
If we're increasing the amount of computation that's being used to train these models so quickly, there's only so many computers that exist in the world, right? At what point are we just going to run out of computers to train stuff on?

**Jaime Sevilla** (00:22:01):
This is an excellent question. So in order to conduct these training runs, the magical thing that you need are GPUs, hardware accelerators that are optimized to do these large training runs, which mainly consist of matrix multiplications. And right now there's essentially one seller in the world that sells these GPUs, that relies on the services of one company in the world that's producing them. There's this very unusual situation in which the supply chain for GPUs is incredibly concentrated. So these companies I'm talking about are: [NVIDIA](https://en.wikipedia.org/wiki/Nvidia) is the one who's designing them, a US-based company. And the foundry that's actually producing and packaging the GPUs is [TSMC](https://en.wikipedia.org/wiki/TSMC) in Taiwan.

**Daniel Filan** (00:22:54):
So the Taiwan Semiconductor Manufacturing Company. Okay. So a very small number of companies actually making these computers.

**Jaime Sevilla** (00:23:03):
That's right. And each of them roughly accounts for 90% of their respective market in terms of design and in terms of manufacturing. And this leads to a situation in which there have been historically some shortages. So for example, in the last year there was the release of this new GPU, the H100. And people really wanted to have H100s for training. They're very good for training. So what they found out is that they quickly ran out of their offer. They couldn't meet the demand, at least immediately, and they had to massively expand manufacturing capacity in order to meet the growing demand for these H100 GPUs. And this naturally raises the question about how many GPUs can they produce at this moment, and how much they can expand that in the future. This is something that we are actively trying to grapple with, at the question. Maybe I can give you a bit more insight into what's limiting the increasing capacity.

(00:24:21):
And right now I'd say that the three main factors that are physically limiting increases in capacity of GPU production, are, first of all, packaging equipment. The process for producing GPUs is: first you create a wafer which has the necessary semiconductors for it. And then you need to package that together with a high bandwidth memory and the other components that make a GPU, and solder everything together into the actual physical GPU that you plug into your data centers. The technology for doing that is called "Chip on Wafer on Substrate technology", or "CoWoS technology". And right now, my understanding is that people are really limited by the amount of machines that can do this CoWoS packaging. And that's why they weren't able to produce as many H100s as they could have sold, essentially.

(00:25:27):
Together with that, you also need the high bandwidth memory units, and this could potentially become a bottleneck. I'm a bit less informed there. And this is what's limiting production right now. But in the future, what might limit it is the production of the wafers that have the semiconductors in the first place. Which might be quite tricky to scale up. Because in order to produce those wafers, you need advanced lithography machines that right now are also being produced by a single company in the world, which is [ASML](https://en.wikipedia.org/wiki/ASML_Holding) in Holland. So in the long term, the growth rate of these wafer production capabilities might determine the growth rate of GPU production. Now these are the physical factors.

(00:26:11):
These are the physical reasons why TSMC is not producing more GPUs that they could sell to NVIDIA, so that NVIDIA can sell them to its customers. But there might also be some commercial/social factors at play here. For example, TSMC, my understanding is that they will definitely raise prices for their GPUs; NVIDIA will be willing to pay them more money and spend more of their margin on TSMC. But if they do that, they're going to drive away some of their other customers. And they might be scared of over-committing to this AI market, where they are not sure whether this is going to be a temporary fad or something that's going to sustain their business in the long term. And this might be a reason why right now, they're a bit scared of dedicating lots of resources to producing the chips that are used for AI training. If they became more bullish on AI, it's plausible that they would invest in the necessary resources to massively expand capacity - which, to an extent, they're already doing, but even more than that - so that they could keep meeting this increase in demand for GPUs.

**Daniel Filan** (00:27:27):
Okay, if I'm thinking about this: so a while ago, I just committed this number to memory, which is roughly 10^31 floating point operations. And the way I came up with that was I said, "Okay, what's some crappy estimate of how many floating point operations I can buy for $1, or the per-dollar cost of floating point operations on really good GPUs?" And then I multiplied that by, "What's the gross world product in 2019?" Just like, "What's the total amount of goods bought and sold?" In some sense, this is a dumb estimate, because it might cost them more if they had to make more machines, or whatever, and also, it's weird to... In a world where we spent 100% of the gross world product on computer chips, that world will look very different - how are people buying food? Or whatever. But that was my weird estimate of just, how much computation should I roughly expect to see until we run out of computers? How good an estimate is that? Am I off by one order of magnitude or 10 orders of magnitude?

**Jaime Sevilla** (00:28:38):
Let me think about this for a second: this is a good question. So right now, to give you an idea, the amount of state-of-the-art GPUs that TSMC is producing is in the order of one million per year for the H100 series, okay? Each H100 has a capacity of 10^15 FLOP per second. And this is for FP16, if I'm not mistaken. How many seconds are there in 100 days? I think that's 10^7 seconds, if I'm not mistaken.

**Daniel Filan** (00:29:22):
I'll do that math and you can keep going.

**Jaime Sevilla** (00:29:24):
Nice, excellent. So we have here GPUs in the six order of magnitudes, FLOP per second in the 15 order of magnitude, and seconds in seven orders of magnitude pending confirmation. So roughly, if you add all of these together, then you will have 7 + 6 - that's 13, plus 15, this is 28. So you will end up with a FLOP budget per 100 days of 10^28 FLOP, okay? If people were magically able to gather all the H100 GPUs that are being produced right now and put them together in the data center and use them for training - which will have lots of complications associated with it, to be clear - they might be able to train a model of up to 10^28 FLOP, which will be three orders of magnitude greater than GPT-4.

**Daniel Filan** (00:30:27):
Okay. Yeah, I have roughly 10^7 seconds in 100 days.

**Jaime Sevilla** (00:30:31):
Nice.

**Daniel Filan** (00:30:32):
So 5x10^25 is what we currently have, and 10^28 is what we could do if people spent 100 days training. I guess you could spend a couple 100 days training, but maybe 1000 days training is like... Should we expect people to at some point just-

**Jaime Sevilla** (00:30:48):
Train for longer?

**Daniel Filan** (00:30:49):
Yeah, three years of training.

**Jaime Sevilla** (00:30:50):
This is actually a fun ongoing conversation within Epoch, which is whether we expect training runs to go for longer or higher. So I already talked about what's the incentive to do it for lower, which is that all these algorithmic improvements are happening and hardware also gets better over time, which naturally makes you want to shorten your training runs. But the reasons why you might want to lengthen it, one of them is just raw output, right? If you train for 10 times longer, then you get 10 times as much compute, and this is something pretty straightforward. Also, if you train for longer, that means you need less GPUs to reach a given level of performance and, also, you need less power to power those GPUs in the first place.

**Daniel Filan** (00:31:38):
Just less joules per second, because you have fewer GPUs and so, at any given second, you're using fewer joules?

**Jaime Sevilla** (00:31:45):
That's right. So there are these incentives for training for longer and, at this point, it's not obviously clear which way the balance tips at this moment. What we have seen historically is that there is not a clear trend in training runs, but there's overall an increase that we have seen from people training for one month seven years ago, to training for three months right now. For the training runs where we have information, training for 100 days seems to be a typical training run length. I think, right now, my very weak epistemic status is I expect training runs to become longer, and I could expect them to become up to twice as long and maybe up to three times as long as they are now. Longer than that, it starts becoming a harder ask, because of these factors that I mentioned, and also because just sustaining a training run for more than a year, that's technically very challenging.

**Daniel Filan** (00:32:58):
Right. There's some rate of random things going wrong, like something crashes, there's a power outage or something?

**Jaime Sevilla** (00:33:04):
That's right.

**Daniel Filan** (00:33:05):
Gotcha. Okay, thinking about this number of "how much computation is there available for AI training?" So you're saying 10^28 FLOPs - "FLOPs" being "floating point operations" total - if you want to train for 100 days on one year of somebody's production of H100 GPUs, right?

**Jaime Sevilla** (00:33:28):
That's right.

**Daniel Filan** (00:33:28):
Do you have a sense of how that number grows over time? In 2030, is that number going to be like 10^29 or is it going to be like 10^35?

**Jaime Sevilla** (00:33:38):
So this is something where I don't yet have very well-developed intuitions; we're looking into this at the moment. My sense is that this could plausibly go up by a factor of 10 somewhat easily, that will correspond to... I really don't know these numbers off the top of my head.

**Daniel Filan** (00:33:59):
Fair enough.

**Jaime Sevilla** (00:34:00):
Do you want me to check them?

**Daniel Filan** (00:34:01):
Yeah, sure.

**Jaime Sevilla** (00:34:08):
Okay, so my understanding is that, right now, TSMC is dedicating around 5% of their advanced node wafer production to making NVIDIA GPUs. I think it's quite plausible that that might increase by an order of magnitude if they decide to prioritize AI enough and if they are able to solve the packaging constraints and high-memory bandwidth constraints I mentioned earlier. So I think it's quite plausible that they will be able to be producing up to 10 million state-of-art GPUs per year.

(00:34:48):
If you were to train on that, that would allow you to reach scales of up to 10^29 FLOP or so. Maybe you increase this a bit, because you might not only use the production from a single year - maybe you stockpile and use the production from previous years as well. And, also, there will be a few advances in the performance of the GPUs; we'll have better GPUs in the future. We already have the B200 on the horizon, and there will be more in the future, I am sure. So I think that, all in all, I think it's good to think that, by the end of the year, if you were to dedicate the whole production of GPUs on a single training run, you could plausibly reach up to 10^30 FLOP. That seems reasonable.

(00:35:53):
It's quite unlikely that you're going to be able to use the whole stock of GPUs on a single train run. First of all, companies are going to fight with each other, different actors are going to want to just do their own training run, which means that the resources are going to be naturally split, and perhaps some companies will want to use a large part of their GPU resources for inference.

(00:36:19):
For example, right now, if you just look at Facebook: Facebook bought 100,000 H100 GPUs just last year, and they plan to have a fleet equivalent of 600,000 H100 GPUs by the end of the year, if I'm not mistaken. But they're not using that many resources on their training runs, they're using only a small fraction of those for training. Most of it is being used presumably for inference right now, and this might continue in the future, depending on how much value labs assign to developing these new models.

**Daniel Filan** (00:37:01):
Yeah, wasn't there an [Epoch post by Ege Erdil](https://epochai.org/blog/optimally-allocating-compute-between-inference-and-training) saying that you should expect companies to use about as much computation on inference as training?

**Jaime Sevilla** (00:37:09):
That's right. He had this neat theoretical argument where he argued... So there are ways in which you can train a model for longer without altering its performance, but make it more efficient at inference. The most straightforward way of doing this is you train a smaller model, but for longer. So, in the end, it has the same performance, but this is a smaller model, so it's going to be more efficient to run at inference time.

(00:37:39):
And if you think about this and you put yourself in the perspective of a company that wants to reduce the total compute that you use between training and inference, then naturally what you want to do is try to make these two equal, because that's what's going to minimize the total expenditure of compute that you're going to be making.

(00:38:00):
There's some caveats here, like inference is usually less efficient than training, the economics for inference and training are not exactly the same. And right now, my understanding is that this is not happening. Well, I think this is not happening in companies like Meta. I think that this might well plausibly be happening in companies like OpenAI, in which they use a very large amount of resources for training. It is quite plausible, given what we know about how much inference is going on at OpenAI, that their yearly budget for inference is similar to their yearly budget for training.

(00:38:40):
But at this point, this is something that I think is informative and I think it has this neat compelling force as an argument, but this is also something that I want to get more evidence on whether this is actually going on before relying on it a lot.

## Algorithmic improvement <a name="algorithmic-improvement"></a>

**Daniel Filan** (00:38:55):
Fair enough. So way earlier, when I was asking you how AI was going, the second thing you mentioned was algorithmic improvements. A post people can read about this is "Algorithmic progress in language models" by Anson Ho, et al. Well, it's a [blog post](https://epochai.org/blog/algorithmic-progress-in-language-models) and it's also a [paper](https://arxiv.org/abs/2403.05812), if you've got time for that. The way I parsed it was that there's something like a 2x speed-up in training models to the same loss every eight months or so. Maybe the error bar was 5 months, 14 months. So my understanding is the way this worked is that you picked some degree of loss that you wanted to get to, so some level of performance, and then you tracked how much computation would it take you over the years to reach this level of performance, and that's where you get this number from.

**Jaime Sevilla** (00:39:49):
Yes and no. So this is the abstraction that we're going for, and we are hoping that this will be one of the applications and interpretations of the model that we built.

(00:39:59):
Actually, the data that you have is very sparse, there's very few people who are doing exactly that experiment, exactly the level of performance that you care about. So what we did is a bit more general in that we just look at lots of models that have been already trained - this is an observational study - and we looked at "what performance did they achieve in terms of perplexity, of the loss that they achieved on the text?" How well they were able to simulate the text that they were tested on, what was their scale in terms of model size and data, and then, also, in which year they were trained. And, essentially, we just fit this regression, which is like, "Well, I'm giving you the model size, I'm giving you the amount of data, and I'm giving you the year that it was trained on."

(00:40:55):
And this model tries to predict, what is the performance that the model achieves? And we fit 14 different equations that tried to combine these terms in different ways and finally chose one of them that intuitively seems to resonate with how we think that scale and performance relate to each other, and also the role that we think algorithmic improvements play in this.

**Daniel Filan** (00:41:23):
Part of what I'm wondering here is, how sensitive is this number to the specification of how exactly you define the question you're asking? Conceptually, if I picked a different loss level, would that change it from every eight months to every three months or to every two years?

**Jaime Sevilla** (00:41:45):
Yeah, so I'm going to introduce you to the bane of my existence, which is scale dependence. So through this model, one big assumption that we introduce is that these algorithmic improvements, they work independently of the scale that the models are trained on. Here, we make this arguably unrealistic assumption, which is that if you come up with a new architecture, then the new architecture is going to help you get a fixed level of improvement, no matter if you are training on a larger scale or a smaller scale.

(00:42:21):
Now, this is arguably not how things work. It could be quite plausible that, for example, we think that the transformer scales much better than recurrent architectures right now, but this might be only if you have enough scale for the transformer to "kick in" and start at this great properties of scaling. If you're working below that scale, you might be better off with a simpler architecture. This is not something implausible to me. What this means is that our estimates might not be sufficiently accounting for this difference in how you should expect improvements to be better at the frontier of compute or whether you should expect them to be better for small-scale budgets.

(00:43:16):
And this matters, because there are two reasons why you care about algorithmic improvements. One of them is that they help frontier runs to be much more efficient and reach new capabilities. The other [reason] that you care about this is because this helps a wider amount of people train models with a certain level of capabilities, right? So depending on which of these two use cases you care about, you're going to care more about innovations that work better at scale or innovations that work better at the small scale and small compute budgets.

(00:43:48):
This is something where I want to have a better, more scientific understanding of to what extent this is the case, and try to look at the specific techniques that drive this algorithmic improvement and try to see, at which scale were they first discovered and at which scales do they apply? And does the efficiency of the technique change, depending on which scale it is applied at? I wouldn't be surprised to find that, but right now, we don't yet have a systematic study showing this.

## Algorithmic improvement and scaling laws <a name="algorithmic-improvement-and-scaling-laws"></a>

**Daniel Filan** (00:44:20):
One thing that I'm confused about when I'm thinking of algorithmic improvements is: people authoritatively tell me that there's these things called "scaling laws" for language models. And these scaling laws say, "Look, it's this formula and you put in how many parameters your model has and you put in how many tokens you're training your language model on", and it outputs what loss you expect to reach on your dataset. I thought that if I knew the number of tokens you were training on and I knew the number of parameters your model had, and if I assumed that you were looking at each token just one single time, so only training for one epoch, which I gather is what people seem to be doing, I thought I could figure out how much computation you were using to reach a given loss. So are algorithmic improvements things that are changing these scaling laws, or are those ways of better using computation within the confines of those scaling laws, or something else?

**Jaime Sevilla** (00:45:19):
So scaling laws are defined with respect to a specific training method and with respect to a specific architecture. So for example, in the famous [Chinchilla scaling law paper](https://arxiv.org/abs/2203.15556) by Hoffmann et al from DeepMind, they study this particular setup in which they have a transformer and they define very precisely, "How are we going to scale this? How are we going to be making this bigger?" There's lots of prescriptions that go into this. For example, when you scale a model, you have lots of degrees of freedom on whether you add more layers or whether you make the model wider to have more neurons per layer.

(00:45:56):
So these are all other considerations that affect the end result, the scaling laws that you're going to fit. You can think of these algorithmic improvements as going beyond the specifications of the particular confines in which the scaling law was originally studied and trying to introduce, well, different tricks, like new attention mechanisms in the architecture. Or maybe you're training on different data that has higher quality and allows you to train more efficiently. Maybe you change the shape of the model in a different way. These are all the ways that you can escape the confines of the scaling laws.

**Daniel Filan** (00:46:40):
So is this basically just saying that scaling laws... I thought of scaling laws as somehow these facts of nature, but it sounds like you're saying they're just significantly more contingent than I'm thinking. Is that right?

**Jaime Sevilla** (00:46:51):
I think that this is right in an important sense, and maybe there's a wider sense in which maybe there are a bit more general. In the experiments on these scaling laws, we see that the scaling laws study a particular setup. We can make some assumptions about how much this is going to generalize, but it is tricky. And there's, right now, dozens of scaling laws papers that study different setups and arrive at slightly different conclusions.

**Daniel Filan** (00:47:26):
Again, to stick in this scaling laws frame: you've written down a sample scaling law on your piece of paper, so total loss is some irreducible loss, which we're going to call E, plus some constant divided by number of tokens to some power, plus some constant divided by number of parameters to some power.

(00:47:46):
And, basically, the things that determine a scaling law are what you think the irreducible losses are, and then, for parameters and dataset size, what you think the exponents of those are, and also, what the constant factors at the front are.

**Jaime Sevilla** (00:48:04):
That's right.

**Daniel Filan** (00:48:05):
If I'm thinking about algorithmic improvements, are those mostly changing the exponents or are they changing the constant factors? It seems like this is going to matter a lot, right?

**Jaime Sevilla** (00:48:14):
It does matter a lot, and this goes back to scale efficiency. So if you were changing the exponents, then the efficiency of the improvements will change with the scale. But the way that we model it in the paper is essentially as a multiplier to the effective model size and effective dataset size that you have.

**Daniel Filan** (00:48:34):
I guess that's not quite changing any of the constants, but it's like, somehow you're using something instead of N and... Okay, okay. I guess that suggests that maybe you could create a meta-scaling law, where you have the scaling law with these constants and the constants are varying over time, or something. Is this meta-scaling law easy to write down? I feel like someone could get that on a T-shirt.

**Jaime Sevilla** (00:48:59):
Yes. I mean, we have 14 different candidates for that in our paper.

**Daniel Filan** (00:49:03):
Okay, all right. So I guess people should take a look. Part of the reason, it seems like, someone would want to ask this question is basically trying to ask, "Okay, if I'm thinking about AI getting better, is that mostly because of increasing computation or is that mostly because of algorithmic progress or is that mostly because of increasing the amount of data we're putting into models?" I think you frame it like this to some degree in the blog post.

**Jaime Sevilla** (00:49:28):
That's right.

**Daniel Filan** (00:49:29):
One way that this could be misleading would be if algorithmic progress were driven, to some degree, by increasing availability of computation. For instance, maybe if you have way more computers, you can run way more experiments, and you can figure out better algorithms to run. Do you know to what degree this is what's driving it?

**Jaime Sevilla** (00:49:46):
Yeah, you're right on the money here. This is something that will be quite crucial for how AI will play in the future, the extent to which this algorithmic innovation is itself being driven by compute. To be honest, I don't have a great answer at the moment. My personal belief is that it actually plays a large factor, and this has been informed by some informal conversations that I've had with people in different labs and rumors that we've heard, where people say, "We're very limited by compute. We don't hire more researchers, because a researcher will just take up precious compute resources that our researchers are already using for trying to come up with better ways of training the models."

(00:50:32):
So it seems that, to a degree, at least in some labs, people have this notion of, "Our research is compute-bound, our research is also being greatly determined by the access that we have to computing resources." And this sounds quite reasonable to me. The main way that we learn things in science is you run an experiment, you see if it works, and then you try it again in slightly different variations. And, particularly, it seems that testing whether a problem scales, testing whether a technique scales is very, very important for making these advances, so that naturally limits a lot the amount of advances that you can [make] if you're constrained by a compute budget.

(00:51:21):
And this might have this huge relevance for how the future plays out, because imagine that we're in a world in which you're bottlenecked by ideas, you're bottlenecked by having more researchers that can do that. Plausibly, in the future, we're going to have AI that's going to be really good at coming up with ideas and substituting for a human scientist and coming up with these hypotheses of what you should train, and if you are bottlenecked by ideas, this could quickly lead to massive improvements in algorithmic efficiency.

(00:51:52):
But if, instead, you're being bottlenecked by compute, then it's like, well, sure, you have all of these virtual researchers that are going to be working for you and coming up with great ideas, but they're still going to have to go through this bottleneck of, we need to test their ideas and see which ones work. And they might be more efficient at coming up with ideas and still this will lead to a substantial increase in algorithmic progress over time, but this might be much more moderate than in the other world.

**Daniel Filan** (00:52:23):
So I guess this gets to the question of what the advent of really superhuman AI is going to look like. I think, classically, people have thought of, "We're just going to have tons of AI AI researchers." I mean, if the bottleneck is compute, compute doesn't just... We don't just get it from rocks, right? Some people are building machines and figuring out how to do things more efficiently. Does that suggest that the singularity is going to be entirely contained within the Taiwan Semiconductor Manufacturing Company?

**Jaime Sevilla** (00:52:57):
Fun question. I mean, right now, the two parts that go into AI progress are you have the hardware manufacturing, but then you also have the software companies, that are a completely separate entity, that are coming up with these ways of training them. So, by default, I guess that we will expect something like that, but there will be this really vested interest in both the semi-facturing [semiconductor manufacturing] company, but also the AI companies, to apply AI to increase their own productivity.

(00:53:28):
I think, particularly, this very naturally happens within AI labs, especially because AI is very good at coding, AI is very good at the things that are useful for doing AI research. I think it's very natural that people will want to see, "Can we use this to improve the productivity of our very expensive employees?" In hardware manufacturing, it also feels like this natural multiplier where, if you are able to use AI to increase the productivity of TSMC, then sure, they're going to be able to produce much more, this is going to lower the prices of compute, this is going to allow you to train even larger and better models that help you achieve better levels of generality and capability.

(00:54:11):
So to an extent, I think that the intuition I have is that I do expect that some of the early use cases for very dramatic increases in productivity are going to be in AI companies, and I will not be surprised if semiconductor manufacturing companies are next in line.

**Daniel Filan** (00:54:31):
Okay. And I guess that suggests: a lot of work being done both in AI in general, and a lot of what you're checking, is scaling laws for language modeling for these predictive tasks. I wonder if that suggests that actually we should be thinking much more about AI progress in whatever a good analog of making computer chips is? I don't even know what a good benchmark for that is in the AI space. I don't know, do you have thoughts about that?

**Jaime Sevilla** (00:55:01):
I think what you were pointing at is, maybe one type of task that we really care about is to what degree AI is going to be helpful for improving chip design, for participating in the processes that go on within a semiconductor manufacturing company. Is that what you were pointing at?

**Daniel Filan** (00:55:20):
Yeah.

**Jaime Sevilla** (00:55:22):
I think that this is right to an extent. It's tricky to design good benchmarks that really capture what you care about. The benchmark you really care about is does it actually improve the productivity, which is something you will see in the future once you get the models deployed there. But it will be interesting to start developing some toy settings which try to get at the core of: what will it mean to increase the capacity of this model?

(00:55:48):
So, for example, one of my colleagues at Epoch, [JS](https://jsdena.in/), has been thinking about, "what kind of benchmarks could be cool to have and will be informative about what we're thinking about in AI?" One of the things he was considering - this is more on the software side, but he was considering "Can we have a benchmark that's about predicting the results of an AI experiment?" And again, this is more on the AI company side, but this could act as a compute multiplier, right?

(00:56:17):
Because if you only have [enough] compute to test 10 ideas, then you want to be picky with which ideas you test. And it's better if you have these powerful intuitions about which ideas might work. So to the extent that AI can help provide you with these intuitions and guide your search for which techniques to try, it's going to allow you to effectively increase the range of options that you're considering, quickly discard ideas that you think are not going to work, and really focus on the ones that are worth testing and trying at scale.

## Training data <a name="training-data"></a>

**Daniel Filan** (00:56:55):
Sure. So we've talked a bit about algorithms, we've talked a bit about computation. I think a lot of people think of AI as basically this three factor model. There's algorithms, there's computation, there's data. You pour those three into a big data center, and out plops GPT-5, right? Is this basically a good model or is there something important that we're missing here?

**Jaime Sevilla** (00:57:18):
So I think that this is a good model, though I will make this distinction, which is that you really care about which constraints are taut at a given moment. And at this moment I will say that compute is a taut constraint whereas data is not. So right now, models that we have are being trained on... for the ones where we know the dataset size, they use around 50 trillion tokens of data. And the size of Common Crawl, for example, is 100 trillion tokens of data, roughly.

**Daniel Filan** (00:57:53):
And Common Crawl is just roughly the internet or is it half the internet?

**Jaime Sevilla** (00:57:58):
It's a fifth of the internet. So if you look at the amount of content in indexed public text data, that will be 500 trillion tokens of data.

**Daniel Filan** (00:58:11):
Okay. So Common Crawl is 100 trillion tokens and people should think of a token as being 80% of a word on average.

**Jaime Sevilla** (00:58:18):
Mm-hmm.

**Daniel Filan** (00:58:19):
Yeah, roughly 100 trillion words of data that you can get from Common Crawl.

**Jaime Sevilla** (00:58:24):
That's right.

**Daniel Filan** (00:58:25):
And so you're saying that data is not the taut constraint right now?

**Jaime Sevilla** (00:58:28):
It is "not" - maybe there's some uncertainty here. Maybe in the future AI companies won't be able to use publicly-indexed data anymore to train their models. There are some complications here, and also there are some domains for which you really want to have data there. If you really care about accelerating experiments, you probably want to have data with coding. You want to have high-quality data about reasoning about AI, and you might really want to expand those kinds of data.

(00:58:59):
But to a first-order approximation, the reason why I think we're not seeing larger-scale training is because there aren't enough GPUs. If people had more GPUs, they would find ways of gathering the necessary data. So in that sense, I think that compute and algorithms are more important to track at the current margin than data is.

**Daniel Filan** (00:59:20):
Okay. Well, even though it's less important than the other things, I do want to talk about it because you had this interesting paper - I mostly just read the [blog post](https://epochai.org/blog/will-we-run-out-of-data-limits-of-llm-scaling-based-on-human-generated-data) - ["Will we run out of data? Limits of large language model scaling based on human generated data."](https://arxiv.org/html/2211.04325v2). This is by [Pablo Villalobos](https://x.com/pvllss) and colleagues.

(00:59:38):
So my understanding of just the top line results is that on the public internet, there's roughly 3x10^14 tokens that you can train on, which is 300 trillion if I can do math, which is unclear. So roughly that many tokens to train on. And you would have a model with roughly 5x10^28 floating point operations that you'd use to train on that. And roughly, in 2028, we'll just be using all of the training data. Is that roughly a correct summary or is there something important that I'm missing?

**Jaime Sevilla** (01:00:16):
I think that that sounds about right. So it's interesting to compare this with the size of the... We were talking before about, if you were using all the H100s that are produced in a year, what's the largest model that we can train? And we arrived at 10^28 FLOP or so, right? If you are using this 100 trillion tokens of data in order to train it, in order to estimate what's the largest model that you could train, maybe one approximation that you can do here is think about the Chinchilla scaling laws that can inform you about: with this amount of data, what's the largest model you can train?

(01:00:56):
Roughly, you want to use 20 tokens per parameter of the model for training it according to Chinchilla-optimal scaling (lots of quote-unquotes here). So that would mean that if (let's say) you use these 400 trillion tokens of data that are all on the indexed web, and that will allow you to train a model that has... Whoo, math -

**Daniel Filan** (01:01:32):
20 trillion.

**Jaime Sevilla** (01:01:33):
20 trillion parameters, so then that will lead you to an amount of compute. So the amount of compute is roughly six times the amount of data times the amount of parameters. So that's going to be, this is 4e14, 2e14, so that's-

**Daniel Filan** (01:01:59):
Nope, 2e13. Because it's 400 trillion but only 20 trillion.

**Jaime Sevilla** (01:02:06):
Oh, yeah, that's right. And here I multiply by six. So this is going to be 8 x 6...

**Daniel Filan** (01:02:20):
Well, it's 12 x 4, which is 48.

**Jaime Sevilla** (01:02:22):
Yes. So 50 essentially. So yes, 5x10^28, which is what we arrived at before, right?

**Daniel Filan** (01:02:30):
Yep. 5x10^28. Oh man, it's nice to see that in action. I was actually wondering where that number came from. Okay, cool.

**Jaime Sevilla** (01:02:38):
Nice. So what you see is that now there is enough data on the indexed web to train something that would be five times greater than what you would be able to train with all the GPUs in the world.

**Daniel Filan** (01:02:55):
So this is interesting to me because there are a few coincidences here, right? One is the thing that you're just noting, that [with the] amount of data you can use, if you had five years of global GPU production, you'd be able to train on all of it, Chinchilla-optimally.

(01:03:11):
Another thing I noticed is: I looked back on the training compute growth paper and I looked at, okay, what's the biggest model right now and when do we hit 5x10^28 floating point operations? And roughly, depending on whether it's 4X or 5X and depending on whether I can multiply, it's somewhere between 2028 and 2030.

(01:03:33):
So somehow there's this strange coincidence where if you extrapolate compute growth, you get enough compute to train on the whole internet, also just at the time when we are projected to train on the whole internet: is that a coincidence or is that just a necessary consequence of frontier models being trained roughly optimally?

**Jaime Sevilla** (01:03:55):
No, I think this is a coincidence. What has been driving the amount of data on the internet is just adoption of the internet and user penetration rates, which have nothing to do with AI and GPUs. So I think this is just a happy coincidence.

**Daniel Filan** (01:04:11):
Well, the 2028 number was when it was extrapolating how much data models were being trained on, right, so that does have to do with AI.

**Jaime Sevilla** (01:04:18):
So the number that we derived now, this 5x10^28, this is based on the amount of data on the indexed web, which has nothing to do with AI, right? What has to do with it is, when you do this extrapolation, when do you hit that amount of compute? What is a coincidence is that the training run that you can do on this amount of data is so similar to the training run that you can do on the amount of compute for the GPUs.

## Can scaling produce AGI? <a name="can-scaling-produce-agi"></a>

**Daniel Filan** (01:04:56):
Gotcha. So now I want to ask a bit about the future of AI. So a while ago you guys put out this post called ["The Direct Approach"](https://epochai.org/blog/the-direct-approach), which was roughly saying, okay, here's a way of turning loss into how much text an AI can write before you can distinguish it from a human. And roughly it was a way of saying: if you want an AI that is smart enough to write a tweet just like a human could, that happens at this amount of computation. If you want to get an AI that can write a scientific manuscript about as well as a human can, that happens at this amount of computation, (modulo some fudge factor, which is very interesting to talk about).

(01:05:34):
But if I looked at those numbers, it said maybe I needed somewhere between 1x10^30 and 3x10^35 floating point operations to get AGI that could be roughly as smart as people. But when I'm training on all the publicly available data on the internet, that was only enough to use with 5x10^28 floating point operations. Does that mean that scaling just isn't going to work as a way of getting AGI?

**Jaime Sevilla** (01:06:03):
So I will say two things here. The first one is that I would mostly think about this method as us trying to estimate this upper bound, because presumably you don't need to be able to mimic humans perfectly in order to write scientific manuscripts of perfect quality.

(01:06:22):
This is this unrealistic goal, in which your model's so good at mimicry that you cannot tell it apart, but it doesn't have to get to be that good in order to have a transformative effect on the economy or produce manuscripts of quality. It's much harder to write something mimicking someone than to write something useful, right?

**Daniel Filan** (01:06:47):
Fair enough.

**Jaime Sevilla** (01:06:51):
The other thing that I would say is that I'd caution people [against taking] these numbers very seriously. I think that right now we just don't have a really good understanding of when do you hit different levels of capabilities, at which scales? I think we have this rough notion about: yes, once you increase in scale, you get more and more capabilities and more generality. And if you combine that with certain scaffolding techniques, this might lead to AI that's widely very useful. But when it comes down to saying, "well, this is going to happen at this amount of FLOP exactly", it's a very rough job.

(01:07:33):
There are maybe some suggestive numbers that I will float around. One of them is this, that comes out of [this paper](https://epochai.org/blog/the-direct-approach) [The Direct Approach] trying to estimate "okay, in this setting, how much compute will you need to train a model if scaling laws can be sustained for 10 more orders of magnitude," which is also another big "if". What other numbers are suggestive to think about?

(01:08:01):
So one thing that's quite interesting is... So I forget exactly who it was right now, it might have been [Ray] [Kurzweil](https://en.wikipedia.org/wiki/Ray_Kurzweil), who 20 years ago made some predictions about: when will we have AI that essentially will pass the Turing test? And they said something like, "Well, we forecast the point where you will have enough compute to match the human brain will happen somewhere in the '20s." That happened. It's insane, right? They got that right. It is actually true that we have now models that essentially pass the Turing test in that they can converse with humans and have a meaningful conversation with them. So it's quite insane that just by looking at this biological construct of the amount of computation going on in the brain and with some wild back of the envelope calculations, they were able to do that.

(01:08:57):
Is there an analogous thing that we can do to talk about when we'll have AI that's really good and can do essentially everything that humans can do? So there was [this report](https://drive.google.com/drive/u/1/folders/15ArhEPZSTYU8f012bs6ehPS6-xmhtBPP) by [Ajeya Cotra](https://www.openphilanthropy.org/about/team/ajeya-cotra/) where she looked at a few of these biologically-inspired quantities. I think that one that has some hold in my thinking on the upper end is the amount of computation that was used to essentially run evolution and give birth to the human species, which she estimates to be around 10^40 FLOP - what you'd need to rerun evolution. There's lots of caveats going into that. Also, if you account for [the fact that] we might have gotten lucky with this run, but maybe it could have taken much longer. Maybe a more conservative estimate could be even up to 10^42 or even 43 FLOP - what you'd need to recreate human evolution. And that feels to me like that's the frontier.

(01:10:09):
If we had that amount of compute, then it's no longer about compute, it's more about do we have the necessary techniques to use it productively to create intelligence de novo?

(01:10:23):
So this is now something that has this hold in my thinking about [AI]. I don't have a very great idea about at which level of compute we will see AI that can participate as a fellow worker in the economy, but it's probably not 10^26 because we are pretty much already there, and I don't think that this is right on the horizon. It's probably not 10^40 FLOP - that seems like too much.

(01:10:53):
If you had that amount of compute, you would be able to, again, rerun evolution. You probably can do better than evolution at creating intelligence with current techniques: I would think so. I think it's not crazy to argue that. So then it's like, well, it's somewhere in between that. And where exactly, at which order of magnitude? I don't know. Maybe my distribution looks pretty uniform between 10^26 and 10^36 or so.

**Daniel Filan** (01:11:22):
Say instead of that I'm uniform just between 10^26 and 10^40 floating point operations to get AI that's smart enough to do all the science [and] technology instead of us. Most of that is higher than the 5x10^28 that we're going to use to train on all the publicly available data on the internet.

**Jaime Sevilla** (01:11:43):
That's right.

**Daniel Filan** (01:11:44):
Does that suggest that scaling language models is not going to be the thing that gets us AGI?

**Jaime Sevilla** (01:11:50):
So I think that people will become creative once data becomes a taut constraint. So again, data right now, I don't think is the taut constraint; I think it is compute. The datasets that people train these models on, at least when training was happening publicly... It was trained on things, again, like [Common Crawl](https://commoncrawl.org/) or [The Pile](https://pile.eleuther.ai/), which are datasets that were put together by software engineers in essentially their free time. There were not these very large, industry-funded projects to get the datasets.

(01:12:26):
To an extent, I think that the paradigm is changing, and now OpenAI is investing a lot of resources in getting data, especially for fine-tuning purposes. But overall for the pre-training, it seems that companies have been able to get away with just using the data that already exists and is easily available. Once this ceases to be the case, there is this huge incentive to come up with ways to increase the efficiency of the data, ways to get more data out of other places, and it's interesting to think about what these places might be.

(01:13:07):
So one thing that we are seeing now is people are training models that increasingly deal with more modalities. So GPT4o, for example, right now is quite proficient at parsing images and can also produce images together with DALL-E... I'm not sure if they use DALL-E or if its native image generation. Well anyway, models right now are increasingly more multimodal, and you could use data from other modalities to try to push back this deadline of how much data you have available for training. Now I don't think that this will actually lead to... if you just look at image and video data, I don't think that this will be a huge delay.

(01:13:53):
Maybe this buys you a couple more years of scaling. Maybe this buys you an order of magnitude of compute. Essentially, I think that this increases the amount of data you have by a factor of three, and the amount of training that you can do increases quadratically with the amount of data you have. So maybe this buys you an order of magnitude of scaling, roughly. So what do you do if you have already trained on all text data, on images and video? What else do you turn to?

(01:14:24):
And one thing that's interesting to think about is the model outputs themselves and synthetic data. So right now OpenAI, if I recall correctly, is producing in the order of 100 billion tokens per day, which roughly extrapolates to 40 trillion tokens per year. So 40 trillion tokens, that's substantial. That's pretty high. If you were to keep that up for 10 years, then you would have produced an amount of data that's as large as the size of the indexed web today. If that data turns out to be useful for training, then you might be able to use this in order to continue scaling.

(01:15:14):
It is not completely clear at this moment whether that's going to be useful. So there have been some studies where if you train models on regurgitated model outputs, there's this phenomenon called "model collapse", in which the quality of the model ends up degrading. And we don't have a really good understanding of that, not just yet. But again, this is in a sense the early days of dealing with this problem, because it hasn't been that big of an issue yet. Once it becomes [one], I expect the forces that be to push really hard to figure out how do we go past this?

**Daniel Filan** (01:15:53):
Here's how I'm thinking about this. It seems like if it's really the case that we're running out of data in 2028, or running out of data to train on, and it doesn't look like we're having AI that can really just take over science and technology creation from us at that scale...

(01:16:18):
There's this question people ask which is: how many big ideas do we need to have before we get superhuman AI? And if it's the case that we're going to run out of data to train on before we get there, it seems like that puts a lower bound, saying we need at least one big idea until we get to super-smart AI. I'm wondering if this seems right to you or even a useful way of thinking about things?

**Jaime Sevilla** (01:16:43):
To some extent. I think I am not sure how big of an idea it's going to be, because it might be just "use synthetic data", and it works. It's like, well, it's a good idea - it worked!

## When will AGI arrive? <a name="when-will-agi-arrive"></a>

**Daniel Filan** (01:16:55):
I want to talk a little bit more about what you can say about AGI: AI that can take over scientific and technological progress from humans. So we don't know exactly what level of loss it's going to be at, but is there something to say about, is it five years away or is it 50 years away? Let's start with that question of timelines.

**Jaime Sevilla** (01:17:23):
So maybe the naive way that you can think about this is: well, we were saying before, it's probably not going to be 10^26 FLOP. That seems too little. It's probably not going to be 10^40 FLOP. That seems too much. It's going to be somewhere in there you get the required level of compute to make AI that can substitute for humans in scientific endeavors a reality.

(01:17:53):
Then you just think: well, how fast are we going and how much [bigger] do we expect compute to go? Right now, my picture of naively what I would expect compute to go like is: well, it goes like this - very, very fast - for a few years, perhaps until the end of the decade, perhaps a little bit more than that, and then eventually it has to slow down.

(01:18:18):
How much it slows down depends on many complicated factors. On whether, for example, we have already found the successful application of AI that allows you to increase the growth rate of the economy, or whether the field stagnates - it's still growing, still growing, but you are now bounded by how fast the economy grows overall, and right now it's growing 3% per year, which is nothing compared to 4X per year.

(01:18:46):
Holding all of these things in mind: okay, maybe we go 4X until the end of the decade, and then by the end of the decade we are training something that's 10^29 FLOP or so. And then if you keep going, I don't know, maybe something reasonable to expect is that somewhere between 2030 and 2050 you might cross 10 orders of magnitude of compute more or something. It starts becoming quickly very complicated. I think that once you are past 10^36 FLOP per year, you start getting into territory where you might just melt the earth in the process.

**Daniel Filan** (01:19:38):
Just because of the heat produced by doing the training?

**Jaime Sevilla** (01:19:41):
That's right.

**Daniel Filan** (01:19:42):
I wonder at this stage... it seems right now that computations available seems to be a pretty good proxy for AI capability because we can hold fixed, there's just this pool of high-quality human data that we're drawing from. But if you're right that we're running out of this data in 2028, it seems maybe at that point computation is just no longer going to be such a good proxy.

(01:20:08):
Maybe we're just going to have to think way more carefully about algorithmic improvements or how you can make use of various data. Do you think that's right? And do you think that that reduces the value of just these compute forecasting estimate-style things?

**Jaime Sevilla** (01:20:25):
TBD. I think that actually again, right now, my guess would be that data is not going to be the most determinant bottleneck, and this is somewhat driven by this belief that people will make it work.

(01:20:38):
People will figure out how we can use, for example, synthetic data here. I don't know. One example that we have is: recently there was this [AlphaGeometry paper](https://deepmind.google/discover/blog/alphageometry-an-olympiad-level-ai-system-for-geometry/) in which they use synthetic data to train an AI system that was able to solve [Olympiad] geometry problems, right? And generally, especially in the scenarios in which there exists the right answer, like math or programming, it seems that one naive strategy you can do to generate more data is, you use the latest generation of models to try to generate solutions to problems and you train on the problems that are right.

## Epoch AI <a name="epoch-ai"></a>

**Daniel Filan** (01:21:20):
I next want to talk just a bit about Epoch AI as an organization.

**Jaime Sevilla** (01:21:23):
Yes, absolutely.

**Daniel Filan** (01:21:24):
Why does Epoch AI exist?

**Jaime Sevilla** (01:21:29):
It was born out of my frustration. Again, while I was doing my PhD on artificial intelligence, I was somewhat surprised that no one had yet done a very systematic analysis of the different trends that matter for AI. There was [this post on AI and compute](https://openai.com/index/ai-and-compute/) from OpenAI in 2018 but very little beyond that, which seemed wild to me given the huge amount of interest that AI was creating, at least around me, and how important they think that the technology was going to be for the future. We started this whole process of: let's systematically track the things that go into developing AI models and study the trends and try to get a better evidence-based, quantitative picture of what the future looks like. And that's how Epoch was born, essentially.

**Daniel Filan** (01:22:26):
When I think of Epoch, I think of it as a mix of [AI Impacts](https://aiimpacts.org/) and [Our World in Data](https://ourworldindata.org/). Do you think this is a fair understanding of what you guys are?

**Jaime Sevilla** (01:22:38):
To some extent, yes. This might be underselling also the amount of in-depth research, specifically on AI, that we do. I think a lot of Our World in Data... they are these curators of data that they do not produce a lot of research themselves, but instead, are compiling the collective knowledge of humanity. And this is very good and very useful. At Epoch instead, we are creating the datasets ourselves and trying to generate this original body of work that people are going to be able to use to inform decisions about AI.

(01:23:16):
Regarding AI Impacts, they're also close analogs to what we're doing in terms of trying to think quantitatively about AI. I think that AI Impacts maybe rely more on surveys and rely more on analogies with other technologies in order to inform what AI is doing. In Epoch we're being a bit more directed and being like, "Well, no, this is about AI and AI is what we are going to be focusing on." Trying to understand, keep up to date with what's happening in the AI world and the latest knowledge that has been produced and all of these concepts like scaling laws and such.

(01:23:50):
And then the hope here is - I see Epoch's AI work as having these three work streams. One of them is we collect this data. The second one is we analyze it. And the third one is we put all our research together to paint these quantitative pictures of the future of AI.

**Daniel Filan** (01:24:11):
There's this work that you guys put in to just have this quantitative picture of the future of AI. One way someone could think about this is: the point of information, the point of knowing things, is that it can change some decisions that someone can make. And the more important the decision that you changed, the more important it was to know the thing. Concretely, do you know what decisions people might be making differently based on what Epoch AI puts out?

**Jaime Sevilla** (01:24:42):
This is an excellent question which gets at who are our audiences and who is changing their mind due to our work? And I think a really important lever here has been policymaking, actually. In the last two years we have seen this surge in interest from governments around the world in governing these new AI technologies. In order to decide how to govern it, they want to have this in-depth understanding of which levers drive development and how they can regulate them.

(01:25:23):
Compute is this very clear example. It's this lever that turns out to be very important for AI development. It's also quantifiable. It's something that you can exclude people from. And so it's this very natural lever for governing. The way that I think Epoch data is being used around the world is in order to inform these conversations on, okay, if we want to create compute-based regulation, how do you decide at which compute levels you're going to impose certain requirements?

(01:25:58):
For example, the [Executive Order on AI](https://www.whitehouse.gov/briefing-room/presidential-actions/2023/10/30/executive-order-on-the-safe-secure-and-trustworthy-development-and-use-of-artificial-intelligence/) from the US imposes certain additional requirements on models that were trained over 10^26 FLOP. And I don't know exactly how they chose the number, but a big suspicion I have is that they looked at our data and they were like, "10^26 is something that no model has been trained on yet. It's close enough that in a year plausibly companies will be trying to train models this way." And this is a way in which I could see Epoch's data being useful, in order to make these important policy decisions.

(01:26:36):
More generally, I am hoping that: right now there are many people who are trying to thoughtfully plan for AI and the advent of these new technologies. And I'd want them to be better informed. I'd want them to make decisions that are based on facts rather than vibes of what is happening right now in the field. This seems really important given that this technology might be the next industrial revolution that we live through.

## Open questions in AI forecasting <a name="open-questions-in-ai-forecasting"></a>

**Daniel Filan** (01:27:06):
Sure. If that's what Epoch AI is for... you've got a bunch of work, but not every possible question has been answered yet. I'm wondering: what are the big open questions that you most want to address that don't have answers yet?

**Jaime Sevilla** (01:27:27):
Okay. Big open questions at Epoch. One key thing that we are constantly thinking about is when are we going to reach different levels of automation and how fast will be the transition from a world with little automation to a world with a lot? That seems to be a very relevant input into many decisions happening today about, "Should we try to plan for this period of very fast automation, try to prepare for that? Is this something that's going to happen in the next year? Is this something that's going to happen in the next 10 years?" Whether different policies and plans for this technology are actually feasible depends a lot about when this rapid period of automation begins and how long the period itself is going to be. This is something that we think about quite a bit.

(01:28:18):
A second important part here is: aside from keeping track of what's driving innovation, we want to have this in-depth understanding of these factors. We talked before about whether algorithmic innovation has this important component that's driven by compute. And we talked about why this will be relevant for painting a picture of the future of AI. We think a lot about those kind of things. And also more generally about the different bottlenecks that AI might face in the future. We talked about data already.

(01:28:53):
Other things that we are talking about: for example, one funny thing I've been thinking about lately is latency. In order to train a model, you need to do a certain amount of serial operations, and this (to an extent) limits the largest training run that you can do. We've also started thinking recently more seriously about power and about how much energy you will need in order to train these large machine learning models. More generally, we want to be able to examine critically all of these reasons why the current trajectory of AI might slow down and try to incorporate that into our thinking about when we're going to reach different levels of automation.

(01:29:36):
And finally, we want to think about how this will impact society. Economists have been thinking long about the effects of automation, but I'm somewhat even disappointed that so far there has been very little uptake among mainstream economists in trying to think about the consequences of AI and of having a way of turning computers into workers essentially.

(01:30:04):
I think that there's lots of things that classical models of economy, semi-endogenous growth theory have to tell us about what affects AI might have on the world, and very little work in just straightforwardly trying to apply these already well-developed, well-understood models to the case of AI. This is something that I will be hoping to see more of in the future. We do have a fair bit of this within Epoch, but I would love for mainstream economists to also join the boat and try to drive the knowledge forward with us.

**Daniel Filan** (01:30:42):
I also find it weird how little there is in mainstream economics on this. And also... I'm not sure I want to name [names](https://en.wikipedia.org/wiki/Daron_Acemoglu), especially because I haven't read [the relevant piece](https://economics.mit.edu/sites/default/files/2024-04/The%20Simple%20Macroeconomics%20of%20AI.pdf), but I think there are prominent instances of this that just do not seem very high-quality to me. Actually, related to this, I read your guys' [2023 annual update](https://epochai.org/blog/epoch-impact-report-2023) or something, and one thing you said was that you would have this report on AI and economic growth at some point during 2024. And I'm pretty excited for that. When should I expect to be able to read that?

**Jaime Sevilla** (01:31:14):
Absolutely. We already put out [a report on economic growth](https://epochai.org/blog/explosive-growth-from-ai-a-review-of-the-arguments) last year where we talked about why AI might lead to explosive growth: what are the econ-literate arguments for why you might see explosive growth from AI and also what are the most plausible objections that we find to it. That was more this theoretical exercise of walking through these models of the economy and these high-level considerations for that.

(01:31:47):
But the next level for us is trying to build this comprehensive, integrated assessment model of the future of AI that tries to tie together what we know about compute, what we know about scaling laws, with these models of the economy and scientific progress. And the hope here is that in the end we will have a tool that is really helpful for describing, if not realistic, at least illustrative pictures of what the future trajectory of AI might look like.

(01:32:27):
Now, when this is going to be out... we have an internal version that works and I find it very insightful. But it's a very large body of work. This is a very large model that hasn't been thoroughly vetted and we want to be careful about putting out things that we are not confident in. I think it's probably going to be at least half a year more before we're ready to share it.

**Daniel Filan** (01:32:52):
Okay. There's some possibility that we maybe get it by Christmas?

**Jaime Sevilla** (01:32:57):
There's some possibility of that. Yeah.

**Daniel Filan** (01:32:58):
All right, I love timelines forecasting. A throughline through many of the things that you said were important or open questions was just understanding the impact of AI. And to me there's just this key question of: okay, you can train an AI to a certain loss - what does that mean? Epoch has done some work on this in [this "Direct Approach" blog post](https://epochai.org/blog/the-direct-approach). I'm wondering: what should people look at to get a good sense of what does "loss" mean?

**Jaime Sevilla** (01:33:28):
Yeah, this is a good question. I think that right now I feel I don't have a good answer to that. Things have been happening internally at Epoch for us to try to grapple better with this question. Last year we put out a report on [challenges for assessing automation](https://epochai.org/blog/challenges-in-predicting-ai-automation) that my colleague [David Owen](https://scholar.google.co.uk/citations?user=yt4c1UYAAAAJ&hl=en) put out, where he looked at different work that has been done on trying to assess the impact that different AI technologies have had on tasks that are economically useful and trying to see if there was a pattern to which tasks are easier to automate. That will be the holy grail for us, having this way of trying to order all the tasks that are useful and say, "Well, these are more automatable" or "These are less automatable." Having that kind of notion will be very useful for figuring out how AI automation is going to [play] out in the future.

(01:34:22):
Sadly, the conclusion of that paper is that work so far hasn't been that good. Every single piece that's out there disagrees with everybody else and we are just basically very confused about how to think about automatability and when we will reach different levels of capability in an economically-useful sense.

(01:34:48):
One thing that we have started doing recently more within Epoch is our own benchmarking program, to try to get a better sense of how fast AI progress is happening in different fields and trying to get a better sense of, well, if you scale these models, what should I expect a 10^28 FLOP model to be able to do? This is to me still this huge open question where I don't think anyone has a really good answer to that just yet.

## Epoch AI and x-risk <a name="epoch-ai-and-x-risk"></a>

**Daniel Filan** (01:35:20):
This is the AI X-risk Research Podcast. A bunch of listeners are really concerned about X-risk from AI. And I think probably a lot of people are concerned that if we make really good, really smart AI that might pose an existential risk. Epoch AI in general is a bunch of really smart researchers trying to understand trends in AI, trends in AI progress. Do you think the outputs of your research are going to tell people how to do a good job at making AI? And if so, should people be worried about that?

**Jaime Sevilla** (01:35:54):
Sorry, the question is whether the outputs of our work are going to help people, whether this is going to advance how fast AI is being made?

**Daniel Filan** (01:36:02):
Yep. That's right.

**Jaime Sevilla** (01:36:04):
I think that to an extent this is true. Having a better understanding of AI naturally helps people build better AI. Now, I think that a lot of the work that we do is things that are already internally known within companies. And I don't imagine that the work that we're doing is massively critical for what's been happening at that scale.

(01:36:32):
Now, this is a hard question you need to grapple with, which is that in the end, your work is going to be used in a multitude of ways and you don't have control over that. And you need to be thoughtful about whether you want to take that trade-off and say, "Okay, we're doing this. This might make AI go faster, or this might make certain avenues of applications of AI more likely. But then the trade-off is that everyone is going to be better informed and we're going to be better prepared to deal with that situation." It's hard to say.

(01:37:06):
Also, one thing that I will say is that it's also hard to give an answer to how fast AI should be going. There is a world in which you want to slow it down and you want to just have a lot of time to think carefully about how AI is going. But there might be also a world in which you want to go quickly over a period, or you want to advance quickly up to the point where you have AI that's going to be really helpful for you to try to improve the way that we align the systems and we try to help them do our bidding.

(01:37:46):
Right now I'm just very confused. One thing I've been thinking a lot about over the last year is risk evaluation in different contexts. And I'm trying to think through different risks. I think that the risk on loss of control, that one is more complex to think about, but for the others, actually right now I've been pretty surprised with the uptake of how things have played out so far. The government seems to be doing mostly sensible things with some caveats, but there has been a very reasonable response from thoughtful people to try to anticipate what's going to happen with AI - what are the risks that are likely to happen in the next couple of years? - and trying to get ready to act if something unexpected happens.

(01:38:40):
In that sense, I think I've become like, "Well, seems good for society right now. In terms of risk management, people seem to be doing what will be necessary to manage at least the short-term risks about AI." On long-term risks, it's so hard to think about. Some days I wake up thinking, "Yeah, maybe having more time and going slowly is going to be better for society." And other times I'm like, "No, actually this is a risk that we should take. We should go a bit faster." Or even, right now we might want to go to get capabilities sufficiently high that we can use it to speed up solving this problem and everything else we deal with.

**Daniel Filan** (01:39:28):
On this narrow question of "is Epoch figuring out any stuff that big labs aren't?", one thing that is pinging for me here is... [Phil Tetlock](https://en.wikipedia.org/wiki/Philip_E._Tetlock) is this academic who studies forecasting and basically studies, "Hey, what if people actually try to be good at it?" He gives a bunch more details, but as far as I can tell, the key criterion is just "are you actually trying to be good at forecasting?" And then everything else flows out from that.

(01:40:04):
Basically [the result](https://en.wikipedia.org/wiki/Expert_Political_Judgment) is that if people actually focus on forecasting and get really good at trying, in forecasting geopolitical questions they can do similarly (or maybe better, I forget which) than intelligence analysts at intelligence agencies that have access to classified details and stuff. Does that suggest that Epoch AI actually is in a position to be better at figuring out AI trends than AI companies?

**Jaime Sevilla** (01:40:35):
To some extent I think that this is true. Especially, I will even argue that we have one advantage, which is that we are focused on the forest, whereas companies are just focused on the trees. Having details of the trees is very useful: I would love to have more details of the trees. But if you are the one who's looking at the big picture and putting everything together, that gives you this unique vantage point that others may not have.

**Daniel Filan** (01:41:03):
Thanks for working in the beautiful backgrounds of our set. I believe you're the first guest to do so. I guess wrapping up, are there any questions that I really should have asked but have not?

**Jaime Sevilla** (01:41:19):
I'm trying to think, how respectably do I want this podcast to end? No, I think that we have covered the basics. Everything that we have covered was really good. Thank you so much for the podcast. That was really fun, Daniel.

## Following Epoch AI's research <a name="following-epoch-ais-research"></a>

**Daniel Filan** (01:41:34):
Well, before we do close, if people have listened to this, if people are interested in following your research and your work, how should they do that?

**Jaime Sevilla** (01:41:42):
Well, first of all, I will welcome everyone to just go right now to their navigation bar and enter [epochai.org](https://epochai.org/) and just interact with our website. We have lots of resources I'm sure you're going to find very useful. You already mentioned at the beginning of the podcast our [trend dashboard](https://epochai.org/trends). That's where I would start, where you have all these really important numbers about how fast AI has been developed over the last decade or so.

(01:42:12):
Then we also have [our databases](https://epochai.org/data) where you're going to be able to see all the data points that make up the trends that we've been studying, and I think [this] will help ground your intuitions about the massive scale of these models. And of course, all the research that we put out is publicly on our website. Together with every paper we try to release a companion blog post, which is aimed at a less technical audience and it helps summarize the main insights of what we found out through our research. I recommend that people just spend some time going over our pages. I think if you're interested in AI, you will find it a very rewarding experience.

(01:42:57):
Other than that, [@EpochAIResearch](https://twitter.com/epochairesearch) is on Twitter and you can follow us. I'm also on Twitter [myself](https://twitter.com/Jsevillamol) and somewhat active, so if you want to hear more about my hot blistering takes about AI, I will welcome you to follow me.

**Daniel Filan** (01:43:13):
Sure. What's Epoch AI's handle on Twitter and what's your handle on Twitter?

**Jaime Sevilla** (01:43:19):
[@EpochAIResearch](https://x.com/EpochAIResearch), that's the handle for Epoch and mine is [@Jsevillamol](https://x.com/Jsevillamol).

**Daniel Filan** (01:43:26):
Okay, great. Well, thanks for coming in and thanks for chatting. This has been really fun.

**Jaime Sevilla** (01:43:30):
Yes, I can say the same. Thank you so much, Daniel.

**Daniel Filan** (01:43:33):
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Filming occurred at [FAR Labs](https://far.ai/labs/). Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode, or to learn how to support the podcast yourself, you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me, at <feedback@axrp.net>.
