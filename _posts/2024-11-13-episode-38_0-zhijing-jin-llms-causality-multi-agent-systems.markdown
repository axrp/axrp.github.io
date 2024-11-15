---
layout: post
title: "38.0 - Zhijing Jin on LLMs, Causality, and Multi-Agent Systems"
date: 2024-11-13 22:50 -0800
categories: episode
---

[YouTube link](https://youtu.be/4K-lHz2_QGg)

Do language models understand the causal structure of the world, or do they merely note correlations? And what happens when you build a big AI society out of them? In this brief episode, recorded at the Bay Area Alignment Workshop, I chat with Zhijing Jin about her research on these questions.

Topics we discuss:
  - [How the Alignment Workshop is](#how-aw-is)
  - [How Zhijing got interested in causality and natural language processing](#zhijing-interested-causality-nlp)
  - [Causality and alignment](#causality-and-alignment)
  - [Causality without randomness](#causality-without-randomness)
  - [Causal abstraction](#causal-abstraction)
  - [Why LLM causal reasoning?](#why-llm-causal-reasoning)
  - [Understanding LLM causal reasoning](#understanding-llm-causal-reasoning)
  - [Multi-agent systems](#multi-agent-systems)

**Daniel Filan** (00:09):
Hello, everyone. This is one of a series of short interviews that I've been conducting at the Bay Area [Alignment Workshop](https://www.alignment-workshop.com/), which is run by [FAR.AI](https://far.ai/). Links to what we're discussing, as usual, are in the description. A transcript is, as usual, available at axrp.net, and as usual, if you want to support the podcast, you can do so at patreon.com/axrpodcast. Well, let's continue to the interview.

(00:28):
Yeah, so Zhijing, thanks for coming on to the podcast.

**Zhijing Jin** (00:34):
Yeah, thanks for inviting me.

## How the Alignment Workshop is <a name="how-aw-is"></a>

**Daniel Filan** (00:35):
So I guess, first of all, we're here at this Alignment Workshop thing. How are you finding it?

**Zhijing Jin** (00:39):
Very excited. I just finished a session about AI governance and a bunch of one-on-ones, which is super interesting.

## How Zhijing got interested in causality and natural language processing <a name="zhijing-interested-causality-nlp"></a>

**Daniel Filan** (00:47):
Cool, cool. So as I understand it, your work, at least predominantly, is in natural language processing and causal inference. Does that seem right?

**Zhijing Jin** (00:59):
Yeah, yeah, yeah. That's a topic from my PhD and I bring it in my new assistant professor role as well.

**Daniel Filan** (01:07):
Right. Oh yeah, I guess I forgot to say: could you tell us where you do your PhD and where you're about to be?

**Zhijing Jin** (01:14):
Sure, so my PhD started in Europe, [at the] Max Planck Institute in Germany and ETH in Switzerland, so it's a joint PhD program, and I'm graduating and taking the assistant professor role at the University of Toronto CS department next year.

**Daniel Filan** (01:33):
Very exciting. Cool. So I guess I'm aware that there's a subset of people in AI who are interested in causal inference, and there's a subset of people in AI who are interested in natural language processing, and if I understand correctly, when you started your PhD, it was a bit more niche. Everyone's into language models now, but... So I'm wondering: how did you come to be interested in the intersection of these two things? Which it seems like when you were interested, neither of them were the main thing, so getting to both of them is kind of unusual.

**Zhijing Jin** (02:08):
Totally, totally. That's such a great question. Yeah, I started in my undergrad having a strong interest in natural language processing and [it was] slightly different from at that time a bigger branch - linguistics and understanding different devices people use in language. I'm always more interested in the semantics, what meaning do they convey, what are they able to do and so on. And then that actually naturally connects... At first people were thinking about "how can we make these models more capable of what people expect them to do?" And then once they show a tendency to exceed certain behavior, then we start to worry about what might go wrong and how to really align [them] with what human society asks for. So actually it naturally develops into this, and I also thank a lot my undergraduate friend, [Cynthia Chen](https://www.xccyn.com/): she introduced me to all these goals of alignment and the [CHAI lab](https://humancompatible.ai/) that's set up at Berkeley and so on.

## Causality and alignment <a name="causality-and-alignment"></a>

**Daniel Filan** (03:14):
Gotcha. So I think a lot of people are interested in alignment, making sure that models don't go haywire, but I think a lot of people, and a lot of our listeners, don't necessarily see that as being super connected to causal inference or don't focus on that aspect. So what do you see as the connection between alignment in general and causal stuff in particular?

**Zhijing Jin** (03:41):
Basically causal inference originated as an important device for us to know more about two things at first: how nature works, and then also how humans work. For example, in the last century, a famous causal inference problem is "does smoking really cause cancer?" And then the interesting statistical advancement people made is that: can we get a causal conclusion without forcing people to smoke? And then now we can totally shift all these tools to LLMs in that, can we understand what contributes to LLMs behav[ing] like this? So there are several ways causality can contribute here.

(04:26):
The first question that I just mentioned is an interpretability problem. We see an LLM demonstrating different types of behaviors, capabilities: can we interpret what circuit, what neurons lead to that? There are also people exploring what type of training procedures lead to them, and then another side is: do LLMs really know the consequence of their actions? I do believe that a lot of the safety work, especially with my recent interest in multi-agent LLMs, it is composed of a two-step process. First, knowing the consequences of your actions. If I do A, what will happen? If I do B, what will happen? A very thorough causal inference understanding. And then later on a moral module added on top of it, or a reward model. In that, what will A mean for other people, for the society, what will B mean? And then which consequence is more preferred? So it's first knowing and then deciding.

**Daniel Filan** (05:30):
So one application of causal inference to language models is understanding what is causing language model behavior. Another is "do language models understand causality?" Did you say that there would be a third thing or did I just misremember that?

**Zhijing Jin** (05:44):
That's mostly the two big threads. I'm also slightly interested in narrow AI. So basically, in the past, human scientists applied causal inference to understand, let's say, what's the effect of increasing minimum wage? Does it reduce the employment rate or does it not? That's [the 2021 Nobel Prize in economics](https://www.nobelprize.org/prizes/economic-sciences/2021/press-release/). And then I'm actually also... so at Toronto we have this organization called [SRI](https://srinstitute.utoronto.ca/) [the Schwartz Reisman Institute], and that also focuses on how AI can be deployed to help a lot of social problems. That's one of my interests as well.

## Causality without randomness <a name="causality-without-randomness"></a>

**Daniel Filan** (06:21):
Gotcha. So one thing that strikes me about the first thing you mentioned, using causal inference to understand what things in AI caused various AIs' behavior: it sounded like you were applying that to stuff in the model: "Oh, if this neuron fires that causes this thing to happen." I've studied a little bit of causal inference, and from what I remember, it seems like it's very dependent on there being some randomness. You have this probabilistic network and you check if things are correlated or not correlated, and that lets you draw these Bayesian DAGs and there's your causality. This is my half-baked memories of it. But a lot of this breaks down if everything's deterministic. If everything is deterministic, then everything is perfectly correlated in some sense. It's hard to get stuff that you might want. So how can you apply causal techniques when you have this deterministic system?

**Zhijing Jin** (07:24):
That's a great question. Also I guess the direct analogy was that in neuroscience, the subject is people, whereas when we move to models where the subject, we can intervene in any sense. So for neuroscience, the difference of correlation/causation is pretty clear, in that correlation means that you ask the subject to do a certain task and observe what part of the neurons are activated, and that's correlated, whereas the causal sense of it is usually got from patients who had brain damage and so on. So actually ablating that part of the brain region, what will happen in their task completion? Moving this to LLMs, we also had a previous early stage of interpretability where people looked at the activation state and the model prediction, or tried to interpret how this model understands the nature of the word here, the syntax tree and so on. Whereas here in the - I guess it's also popularly known as the mechanistic interpretability research - the causal notion is that we really intervene on the neurons and see what's happening. That's the first level in terms of...

(08:44):
I guess there are maybe three different levels, different types of causal inference that are applied on interpretability. The first one is directly ablating a certain neuron, set it to a certain value and then see what will happen or entries in that tangent matrix. The second type is basically the so-called "mediation analysis" where you basically control the neurons to be [one] of two states. One is maybe an intervene state, [another is a] control state, to single out which part really does the job. So all of the previous two [levels] are still neuron-level interpretation, whereas the third branch is this causal abstraction where you try to match a bunch of neurons to a certain function and understand at a macroscopic level.

**Daniel Filan** (09:43):
If people are just not very familiar with this literature, where's it at? How good a causal understanding can we have of networks?

**Zhijing Jin** (09:53):
So I think it's becoming more and more of a drive of interpretability. And we will have [a NeurIPS 2024 tutorial on causal LLMs](https://neurips.cc/virtual/2024/tutorial/99520) as well, where a big part will introduce this.

## Causal abstraction <a name="causal-abstraction"></a>

**Daniel Filan** (10:07):
Sure. So one thing that caught my ear was the causal abstraction stuff, abstracting beyond the neural level using causal techniques. Can you say a little bit more about the state of the art in that work, what we've learned?

**Zhijing Jin** (10:28):
Some earlier work usually hypothesized that: okay, there's a computation graph that we believe is solving the problem, and then try to map different neurons' function to the corresponding unit in that more abstracted computational graph.

(10:47):
Then I guess the recent SAE (sparse autoencoder) [work] also has a similar notion in that it's trying to map this neuron space, which is much more highly dimensional [compared] to a relatively lower dimensional space, and this is relatively... So, as mentioned, the earlier work is more of a top-down approach where we hypothesize the computational model, and then the SAE is a little bit more bottom-up, and I hope in the future there will be more work emerging from that.

(11:19):
Also in our causal inference lab, we always draw an analogy to how the history of science advances. For example, by observing what's happening around us, we distill Newton's law, and that's also from all the pixels that we see to what is mass, what is acceleration, what is force and so on.

## Why LLM causal reasoning? <a name="why-llm-causal-reasoning"></a>

**Daniel Filan** (11:41):
So the next thing I wanted to ask is... So the main other type of causal inference work you mentioned is having neural networks understand the causal impacts of their actions. I think in a lot of the field of AI, this isn't thought of in causal terms. People think, okay, we'll just have some probabilistic model of "what's the probabilities of various stuff happening conditioned on actions?" What do we buy by thinking about it in a causal frame?

**Zhijing Jin** (12:12):
So I guess it's a lot about what is abundant, which is observational data, and what's relevant for decision-making, which is essentially interventional. We need to do an action, see the real world effect. I guess also it's not only the LLM agent knowing about its own consequences, but also knowing more about the world. Let's say that for us human agents, a pressing question is, for example, who to elect as the president. And then that's also a lot of causal contribution for us. We haven't seen a world where Harris is the president and how that will be, or we haven't seen, given the current international situation or domestic situation, how Trump will act. But we're trying to attribute: okay, we want to achieve a certain goal. Everybody cares about certain things. Is Harris more of the cause or Trump more of the cause of that?" So any decision that people make is fundamentally causal here.

## Understanding LLM causal reasoning <a name="understanding-llm-causal-reasoning"></a>

**Daniel Filan** (13:19):
Can you give us a flavor of what work is being done to understand LLM causal reasoning, just to give us more of a flavor of it?

**Zhijing Jin** (13:31):
We have done a couple of studies on that, and there are also awesome researchers in the field of NLP machine learning building more causal agents. And for us, we had [a previous work at ICLR 2024 this year](https://arxiv.org/abs/2306.05836). We present to an LLM a bunch of correlations and let it try to make sense and [see] which one should be understood as a causation, which one stays as a correlation (you wouldn't expect anything from intervention later). And this whole problem set-up was inspired by: if we have humans, because we lack access to the real counterfactual world, we basically keep reading news articles, we keep hearing about anecdotes, and we need to figure out what really makes sense.

**Daniel Filan** (14:25):
So you're presenting these correlations to neural networks, asking them to figure out which ones are causal? How are you presenting those correlations? What are you actually training a model on?

**Zhijing Jin** (14:37):
For that one, it's basically we draw the study... It's a field called causal discovery, and then we tell it a lot of cases, "Okay, A correlates with B, B correlates with C."

**Daniel Filan** (14:50):
So you tell it in sentences because with a language model, you can just do that? Okay. So how do language models perform on this task?

**Zhijing Jin** (14:59):
Actually it was pretty badly. A lot of off-the-shelf LLMs directly evaluated on that were maybe only marginally above random, including also some of the latest GPT models. And we also would perceive, this is a slightly different question than if you directly ask, "Does smoking cause cancer?" Because here you require the reasoning process and you need to build from scratch "here are the correlations, make sense of it," instead of [the model] remembering some literature, remembering all the previous documents and so on.

**Daniel Filan** (15:41):
So if the new hot models just aren't very good at this task, does that let you make some prediction about various things that they're going to be bad at that we could then check out?

**Zhijing Jin** (15:56):
Yeah, so I guess there are different usage scenarios of these LLMs. Something that I keep being unsatisfied about is when we chat with them and ask about a pretty essential problem, its current solution is, "here are the five factors that matters..." and stuff, but it didn't tell you very exactly what might be the problem and so on.

**Daniel Filan** (16:20):
So diagnosing and fixing things in the messy real world where you can't just remember a list, maybe that's going to be a thing that models are going to struggle with.

**Zhijing Jin** (16:30):
Right, right.

## Multi-agent systems <a name="multi-agent-systems"></a>

**Daniel Filan** (16:32):
I also want to talk about some other work you've done. So I understand that as well as natural language processing and causality, you also have some work thinking about multi-agent systems. Can you say a little bit about what your interest is in there and what you're doing?

**Zhijing Jin** (16:47):
Sure. I was pretty impressed by the rising trend where people... I guess it started from the generative agents work, where there's this idea of: we can build a digital village where each LLM plays a certain character and see what will happen among them. I see a lot of possibilities from that.

(17:12):
First of all, a lot of times before, we were thinking about benchmarking LLMs, understanding them, but implicitly it assumes a single agent evaluation. Then this is a scheme to move towards multi-agent systems. And it could be about how they collaborate or it could be about how they cheat against each other and so on. Moreover, that's the pure, let's say, LLM AI safety. We build this LLM society and only care about that. There's also some analogy that we can draw for human society, on the other hand. We can simulate characters that are close to maybe some international players that we are seeing these days and see "if you do this, what will happen?", and so on. I have a long-lasting passion in policy and so on, so it will also help by being a testbed for testing policies.

**Daniel Filan** (18:12):
Gotcha. If I think about work that's been done... I can't think of a ton of work that's been done in this domain, even though it seems interesting. There's a lot of multi-agent work in a reinforcement learning setting. In this setting, I guess I have some familiarity with [some work getting language models to make contracts with each other in Minecraft to cooperate](https://dspace.mit.edu/handle/1721.1/156591), but it seems like maybe an underdeveloped field. So I'm wondering: specifically, are there any experiments that you'd be really excited to run? Or, I don't know, maybe you don't want to scoop yourself, but can you tell us a little bit what this might look like concretely?

**Zhijing Jin** (18:50):
Yeah, it's a very nascent field. We have recently finished... So I always draw inspiration from interdisciplinary things and recently, we have [a NeurIPS 2024 paper](https://arxiv.org/abs/2404.16698) on whether LLM agents will demonstrate the tragedy of the commons. Also specifically we focus on things that are only demonstrated in a multi-agent situation. In that one, we put a bunch of agents together in a simulated village and see whether... So in human society, the tragedy of the commons is usually set up as, for example, we have only one environment, but maybe if all of us want to consume from it, then in the end there is this climate change, there is this pollution problem.

(19:45):
Similarly for LLMs, we define how it can harvest and what are the constraints of this shared pool of resources. And we simulate a whole calendar year where LLMs go through many iterations of: now it's the point where you decide how much you want to harvest, you know that the limit of the resource is this, its replenishing rate is like that, and you know that there will be also other LLMs.

(20:13):
So at every iteration, we make three different stages. The first one is the action stage where you decide how much to harvest. Then it's a discussion stage. It's a little bit like a town hall meeting where all the agents talk to each other and they can point fingers at some more greedy agents or they can set up rules and so on. And then in the last stage, it's a reflection. They try to compile information from this whole iteration and the history and think about making plans for the next round and so on. And amazingly - well, also a little bit pessimistically - we observed that many of the agents have 0% survival rate, which means that-

**Daniel Filan** (21:01):
Oh, geez.

**Zhijing Jin** (21:01):
...they drain the common resource in the middle, and actually also at a very early stage. That also actually includes a lot of models that are claimed to be safety-tuned, such as a lot of Anthropic models as well. And most of the open-source models that we've tested, including Llama and so on, also collapsed very early and have a survival rate of zero.

**Daniel Filan** (21:26):
Yeah. Wow. Seems bad. So we're about at the end of the window I booked, and I don't want to use up more of your time, but thank you very much for chatting with me.

**Zhijing Jin** (21:37):
Yeah, thank you so much for the interview.

**Daniel Filan** (21:39):
This episode was edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode, or to learn how to support the podcast yourself, you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me, at <feedback@axrp.net>.
