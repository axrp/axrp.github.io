---
layout: post
title: "19 - Mechanistic Interpretability with Neel Nanda"
date: 2023-02-03 18:50 -0800
categories: episode
---

[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/OTg3NjkyYjItZGFmNS00MzA2LTgzY2QtYzY0MDlkNThmNTIy)

How good are we at understanding the internal computation of advanced machine learning models, and do we have a hope at getting better? In this episode, Neel Nanda talks about the sub-field of mechanistic interpretability research, as well as papers he's contributed to that explore the basics of transformer circuits, induction heads, and grokking.

Topics we discuss:
- [What is mechanistic interpretability?](#what-is-mech-interp)
- [Types of AI cognition](#types-of-ai-cog)
- [Automating mechanistic interpretability](#automating-mech-interp)
- [Summarizing the papers](#summarizing-the-papers)
- ['A Mathematical Framework for Transformer Circuits'](#math-framework)
  - [How attention works](#how-attention-works)
  - [Composing attention heads](#composing-attention-heads)
  - [Induction heads](#induction-heads)
- ['In-context Learning and Induction Heads'](#iclih)
  - [The multiplicity of induction heads](#ih-multiplicity)
  - [Lines of evidence](#lines-of-evidence)
  - [Evolution in loss-space](#loss-space-evolution)
  - [Mysteries of in-context learning](#icl-mysteries)
- ['Progress measures for grokking via mechanistic interpretability'](#progress-measures)
  - [How neural nets learn modular addition](#nn-mod-add)
  - [The suddenness of grokking](#sudden-grokking)
- [Relation to other research](#relation-to-other-research)
- [Could mechanistic intepretability possibly work?](#could-mech-interp-work)
- [Following Neel's research](#following-neels-work)

**Daniel Filan:**
Hello everybody. In this episode I'll be speaking with Neel Nanda. After graduating from Cambridge, Neel did a variety of research internships, including one with me, before joining Anthropic where he worked with [Chris Olah](https://colah.github.io/about.html) on the Transformer Circuits agenda. As we record this, he's pursuing independent research and producing resources to help build the field of mechanistic interpretability. But around when this episode will likely be released, he'll be joining the language model interpretability team at DeepMind.

In this episode, we'll be talking about his mechanistic interpretability research, and in particular, the papers '[A Mathematical Framework for Transformer Circuits](https://transformer-circuits.pub/2021/framework/index.html)', '[In-context Learning and Induction Heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html)', and '[Progress Measures for Grokking via Mechanistic Interpretability](https://arxiv.org/abs/2301.05217)'. For links to what we're discussing, you can check the description of this episode and you can review the transcript at axrp.net.

First of all, is it right to say that you understand yourself as a mechanistic interpretability researcher?

**Neel Nanda:**
Yeah, feels pretty accurate.

## What is mechanistic interpretability? <a name="what-is-mech-interp"></a>

**Daniel Filan:**
So what is this mechanistic interpretability thing?

**Neel Nanda:**
Sure. So this is an annoyingly fuzzy question. The starting point I'd go with is: with an analogy to reverse engineering a compiled program binary to its source code. Neural networks are this thing that emerges from weird processes like stochastic gradient descent that learned a bunch of parameters that led it to do a task competently. We have no idea how it works, and I want to be able to understand what it has actually learned and why it does what it does. But I'm holding myself to the extremely high standard of 'I want to actually understand what the internal cognition of the system is'. Like, what are the algorithms it's running such that it takes the inputs, produces a series of intermediate things, and then produces some legible output.

**Daniel Filan:**
It sounded like that was potentially a mix of: you've got this neural network that emerges from training. One thing you could be trying to understand is, 'Okay, what have algorithms encoded in that final neural network? What's the "cognition" going on in it?' And another thing that you were maybe hinting at was, 'Okay, during the training of the neural network, there's maybe some reason that some parts of this final network emerged as the thing that would go into the network at the end of training'. So how much of this is about what's happening in the final network versus what was incentivized during training and how that story all went?

**Neel Nanda:**
So to a first order, it's entirely about looking at the final network. To me, the core goal of the field is to be able to look at a final network and be really good at understanding what it does and why. I think that this does pretty naturally lead to being able to look at a thing during training. My grokking work, which I'll hopefully chat about, is a pretty good example of this, where I tried really hard to understand the network at the end of training and then what happened during training just fell out. And I think a bunch of the more ambitious ways to apply mechanistic interpretability [involve] influencing training dynamics.

But to me, I see the core of the field as 'Can we take a single model checkpoint and understand it, even if this takes a lot of time and a lot of effort?'

**Daniel Filan:**
Okay.

**Neel Nanda:**
And I think there is feedback with training dynamics. I also think looking at it during training might be necessary to explain some things; like weird vestigial organs that were useful early in training but are superseded by some later algorithm, which are no longer needed, but still stick around.

**Daniel Filan:**
Okay. Do you actually see that in practice?

**Neel Nanda:**
I'm not aware of a concrete example of seeing that. I would be very surprised if it doesn't happen.

**Daniel Filan:**
Okay, fair. So now that we know what you mean by this mechanistic interpretability thing, why do you do it? What's the point?

**Neel Nanda:**
So on a purely personal level, it's just really fun and that actually does carry quite a lot of weight for me. I also think it's actually very important. So at a high level, I think we are living in a world where we're going to get computer programs that are human-level intelligent or beyond, that are doing things; and that it's plausibly going to be pretty hard to tell whether these things are doing things because they're actually aligned, or because they've learned to be deceptive or to exploit flaws of the training process, or [they're] just actually trying to be aligned but mislearned some things. And I would really prefer to live in a world where we actually understand these things.

And I think that mechanistic interpretability is not obviously the only path to get to a point where we can make some claim to understand systems. But I think it's a promising path that to me achieves a pretty high level of rigor and reliability and understanding. I also think that neural networks are a really complex system and it's really hard to make progress in a complex system without any grounding of something you actually understand that you can build off of. And I just think that lots of things we are confused about in networks, whether just in ML as a whole, [or] in alignment, [or] anything you might care about... I expect it will get easier the more we can claim that we actually understand what's going on inside these models. I also think it's kind of scientifically fascinating.

**Daniel Filan:**
Yeah. I wonder: so there's this definite vision of: okay, we want to do mechanistic interpretability because at some point we're going to train a model and we're going to want to know a whole bunch of facts about the cognition of that model in order for us to evaluate 'do we think it's good, do we want to release it?' And there's this other broad story of: well, we don't really know how things work; if we understood what was happening, this would help us get better abstractions, it would help us understand the science of deep learning, and then we'd just be in a better place generally. I'm wondering: how much weight do you put on those two stories?

**Neel Nanda:**
Sorry, can you repeat the question?

**Daniel Filan:**
So there are two stories of why mechanistic interpretability could be useful. One is: you want to do it so that you develop these mechanistic interpretability tools, and the way you use them is one day you're going to train a model and you're going to want to know whether it's a good model or a bad model in terms of how it's thinking about stuff. And so you use mechanistic interpretability to understand its cognition. Story two is like: look, we just have mechanistic interpretability lying around, so that in general, when we do deep learning, we want to know what's going on and we can say, ah, well, because we've done so much mechanistic interpretability in general, we know that there's these squeebles that always happen in neural networks and probably this weird thing about your loss going down is due to the squeeble formation and you know, you need to tweak it to drive out the squeebles or something.

So there's one story where it's like, 'we need to use it for this thing'. There's another story where you're like, 'okay, I don't know what value of squeeble is going to be important, but I know that we're eventually going to have to do good science of deep learning and mechanistic interpretability is the way to do that'. So these are two ways that mechanistic interpretability can be valuable. I'm wondering if you have a sense of, in your head, is one of these stories way more compelling than the other, or a little bit more compelling, or do you think no, these are two basically equally good reasons?

**Neel Nanda:**
Yeah, I would say that the "take a system and figure out whether it's doing good things or bad things" is somewhat more compelling to me, but it's a somewhat messy question and I want to try to disentangle a couple of things here. The first is, what do I think is actually going to push on reducing x-risk where being able to take a specific system and ask questions around 'is this doing good things or bad things?' feels quite a bit more important to me than answering general 'science of deep learning' mysteries. Because I think it's not obvious to me that getting good at science of deep learning is actually net good for x-risk, and it wouldn't surprise me if it's net bad. The way I see a lot of mechanistic interpretability work at the moment is: we are just trying to figure out the basic rules of networks and how they work and what kind of things seem to form and just like, what are we even doing?

And I think that it can be a mistake to be too goal-directed when trying to do basic science. And I expect that lots of things that get done will turn out to be useful and interesting in lots of unexpected ways for future things people want to do. And I also think that if we actually get good at interpreting systems, this just seems like it should be really useful in loads of different areas in a bunch of ways I can't really predict. The final clarification would be that I generally think about goals of mechanistic interpretability as having a sliding scale of difficulty in terms of, I don't know...if we reach a point where we could take a single system and spend two years of time and a ton of researcher and less skilled worker effort trying really hard to notice lack of alignment in it... I'm like, that's reasonably useful, not incredibly useful. I feel reasonably good about that.

Then there's a world where we could have the ability to take a system and in a day come up with a reasonable idea of if it's deceptive or not. This seems more useful. There's worlds where we could turn these into automated robustness checks that we could give to alignment researchers so they can validate their techniques. There's things where we can actually insert some metrics into training and you do gradient updates on those. There are cultural effects where if we could credibly say to every lab making an AGI, 'here's a proto-AGI that was made and is clearly deceptive in this really subtle and hard-to-see way that we can identify with this tool, maybe you should be careful and take alignment more seriously...' And I don't know, that seems like depending on how well we succeed, there's just a sliding scale of how useful this all can be.

**Daniel Filan:**
Yeah, that seems reasonable. Related to some of that, I think one reaction you could have to some of this mechanistic interpretability work is like... suppose you're really worried about existential risk from AI. Some people I know think, oh, if we just do more AI research the standard default way it's going, maybe we're just all going to die. I think from that perspective, there's some worry of all these mechanistic interpretability papers that get published and that help the world understand how deep learning is actually working and what's actually going on, maybe that's helping AI get generically better even more than it's helping any alignment-specific stuff. I'm wondering what you think of that concern, and what do you think the threshold should be for just not publishing mechanistic interpretability work?

**Neel Nanda:**
Yeah, so I think this is a pretty valid concern and it is definitely by far my strongest guess for ways my actions could turn out to be net negative on the world. And I don't know if I have a confident and satisfying answer to this question. I definitely think that if the ambitious claims I'm making -- that we could actually get to a point where we understand networks in any meaningful sense -- are true, then this should clearly be useful for making them better. I think it's very plausible we don't get good enough at that [for it] to really matter for improving models, but potentially we get good enough at it to help with auditing a system or help with understanding how to align it. I also think it's perfectly plausible that things are the other way around. I partially just have a broad heuristic that it seems just obviously the case that a world where we actually have some insight into these weird black box systems is a world where they will be easier to align and we will be safer.

But I don't really know how to form careful principled answers to these questions. One example that maybe makes me feel a bit more on the concerned side is: so at Anthropic, we had [this 'induction heads' paper](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html) that we're going to talk about a bit later, and there was this recent paper called [S4](https://arxiv.org/abs/2212.14052) that tried to introduce a new architecture based somewhat on LSTMs that was better at tracking long range dependencies and was trying to compete with a transformer. And a thing that was a significant part of the paper was comparing them to induction heads, which are one of the main ways transformers track long range dependencies. And it sounded like induction heads might have inspired this architecture, and I don't know if S4 is actually any good or the kind of thing that's actually going to matter, but it is plausible we live in a world where there was an important insight that was inspired by some mechanistic interpretability work and ah, that doesn't seem great.

In terms of my takes on work that I think should versus shouldn't be published, I generally think that anything which is deeply tied to something that could help us make frontier models like [PaLM](https://ai.googleblog.com/2022/04/pathways-language-model-palm-scaling-to.html) better, I'm very, very concerned about, and things which are looking directly at those models, noticing things that are off about them and trying to fix them.

I'm more relaxed about things that are looking at smaller language models, things that are explaining specific circuits, or giving better techniques and frameworks for doing this, or things that are more general insights into deep learning that aren't specific to the frontier models. Where my intuition here is: something that's more about understanding deep learning generally is safer than something that's very focused on debugging large language models. Because my guess is basically just that understanding what's going on in deep learning seems like it should just be useful in both directions pretty broadly. While understanding specific details of things that go wrong in large language models seems much more useful on the frontier of making them better. But I'm just pretty confused about a lot of these questions.

**Daniel Filan:**
Sure. Related question, which you might be confused by. One thing I wonder is: suppose we do this kind of mechanistic interpretability work largely on smallish models, or models where it's slightly easier, or models that exist today which are probably somehow qualitatively different from future super crazy models. How scale-invariant do you think we should think of the insights as being? Like, you could imagine there are all these things that are true of these one-layer transformers that aren't true of GPT-n, or there are things that are true before you figure out the meaning of life and then once you figure out the meaning of life, all your circuits change.

**Neel Nanda:**
So I'm sure you'll be shocked to hear that I'm also confused about this one.

**Daniel Filan:**
All right.

**Neel Nanda:**
So in part I think this is just an empirical question that we don't currently have a ton of data on. The main data is [the induction heads paper](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html), which we'll talk about a bit later, but we found this simple circuit in two-layer attention-only models and then found out that this was a really big deal in those models, but also a really big deal -- and actually occurs -- in every transformer we've looked at, up to about 13 billion parameters.

**Daniel Filan:**
For context, how many parameters are in e.g. the [ChatGPT](https://en.wikipedia.org/wiki/ChatGPT) model that people can interact with in their browsers?

**Neel Nanda:**
So we don't actually know for ChatGPT because OpenAI isn't public about this, but [GPT-3](https://en.wikipedia.org/wiki/GPT-3) is 175 billion.

**Daniel Filan:**
Okay, so 13 billion is about one order of magnitude less than something that's maybe state of the art.

**Neel Nanda:**
Yes, I will counsel that I think parameters are somewhat overrated as a way of gauging model capability. [In] DeepMind's [Chinchilla paper](https://arxiv.org/abs/2203.15556), the main interesting result was that everyone was taking models that were too big and training them on too little data, and they made a 70 billion parameter model that was about as good as Google Brain's PaLM, which is 600 billion, but with notably less compute. But this might be splitting hairs.

**Daniel Filan:**
So that actually brings up something I wanted to ask about. Something that I see as somewhat analogous to mechanistic interpretability is work on scaling laws. So correct me if I'm wrong, but I think [the first scaling laws work](https://arxiv.org/abs/2001.08361) was by Danny Hernandez and some collaborators at OpenAI.

**Neel Nanda:**
I don't believe Danny was an author on the paper.

**Daniel Filan:**
Oh, he wasn't?

**Neel Nanda:**
Let me just check, so I'm not... Nope. Jared Kaplan, Sam McCandlish, I believe [are] the two lead authors.

**Daniel Filan:**
Oh, I think I'm thinking of another thing.

**Neel Nanda:**
[Danny Hernandez] was an author on a [scaling laws for transfer learning paper](https://arxiv.org/abs/2102.01293) that came out later on.

**Daniel Filan:**
Oh, okay. So I think that a lot of the initial scaling laws work was by perhaps the more AI alignment or AI x-risk-focused part of OpenAI. But I see it as, on the one hand, an attempt to understand what's going on with deep learning, how's it happening, in a similar vein, although not the same, as mechanistic interpretability. I also think that it's ended up primarily used to help the wider world train models better. So I'm wondering, firstly, do you think that's a good analogy? And secondly, do you think it's a concerning analogy?

**Neel Nanda:**
So I am going to weakly argue that it's a bad analogy. I think it's pretty plausible that the scaling rules work has been fairly net negative and has been used a lot by people just trying to push the frontier of capabilities (though I don't have great insight into these questions).

I think that to me, scaling laws don't feel like they get me any closer to a question around 'is this model aligned or not?' I don't feel like I understand anything about the model's internals. I feel like it plausibly helps me forecast things, and I do think that forecasting things is important, but it feels important in a very different way to why I think mechanistic interpretability is important.

I will also note that my best story for how the scaling laws [work might be] net positive is that it just meant the path from here to AGI was a lot smoother and more continuous. So we're better prepared and can do better alignment work along the way, rather than someone just eventually realizing, 'Oh wait, turns out that I already had all the compute it would take to make AGI'. And on that front, I think that is a thing that is useful. To me, scaling laws feel like kind of... statistical physics, what happens when we get lots and lots of parameters and average out lots of tiny interactions into some big smooth power law. And to me, mechanistic interpretability feels much more like 'let's zoom in and try to really understand the details of what's going on', rather than zooming out so much that a lot of the details abstract away.

**Daniel Filan:**
Okay. Would it be right to understand your position as: scaling laws is less useful for AI x-risk reduction or AI alignment than mechanistic interpretability might be, but maybe they're similarly useful for just building AI in general, or do you think that scaling laws was knowably more useful for building AI?

**Neel Nanda:**
I would predict that scaling laws were knowably much more useful. So this is partially hard to really engage with because I'm sitting here knowing the key results of scaling laws and their practical implications, but I'm not sitting here knowing the key results of mechanistic interpretability. And if I came back to you tomorrow and was like, 'Oh, with this one weird trick, you can turn GPT-3 into AGI, and I found this by staring at its weights', I'd probably be like, 'ah, that is worse than scaling laws'. I don't expect this to happen, to be clear.

But yeah, I think the difference I would make is that: if I am a company trying to build an AGI and I hear about scaling laws, then I'm like, 'oh, that's so useful'. I was going to say it's so useful because I know how to scale and some amount [about] how big my model should be and how much data I need to collect and how good I can expect it to be for the amount of compute. On reflection, I actually think that the most important consequence was just this idea that scaling will continue to work and that we should just try really hard to make models bigger.

And it's seeming less and less likely that there's some magic point where everything breaks and you waste a billion dollars, and that's very tied to their ability to forecast. And I mean it's plausible to me, if you actually got good at mech. interp., we'd be good at forecasting. I'm probably just going to call it mech. interp. because 'mechanistic interpretability' is a mouthful.

**Daniel Filan:**
Fair enough.

**Neel Nanda:**
I don't know. I think the two things here are forecasting versus understanding, where forecasting seems more important to changing the actions of the people who make the AGI. Understanding seems more important for disentangling what's going on inside of it and how happy we [are with] that.

## Types of AI cognition <a name="types-of-ai-cog"></a>

**Daniel Filan:**
Okay. So I guess I have a few questions about roughly where you think mechanistic interpretability is as a field. The first one is: intuitively, I think of there as being some spectrum where, at the low end of the spectrum is basic 'I'm detecting edges' or 'I'm noticing that something is text rather than picture'. And then maybe [at] a little bit higher level is 'I notice that it's all caps'. A bit higher level is 'I notice that it's written in English rather than French'. And then at a very high level is something like reasoning, where I'm recalling how to do, I don't know, [modus ponens](https://en.wikipedia.org/wiki/Modus_ponens), or in which situations [mathematical induction](https://en.wikipedia.org/wiki/Mathematical_induction) is useful, and what a useful inductive hypothesis might be in this case.

Where it's something like, perception versus reasoning is the spectrum. And to the extent that language models can do things that are closer to reasoning, as well as basic 'knowing what language their input is being written in', it seems like if we want to understand them, we're going to have to understand this whole spectrum of cognitive abilities. I'm wondering... so first I guess this understanding of there being one spectrum is potentially contestable. So feel free to contest that if you want-

**Neel Nanda:**
Sorry, the spectrum being 'sensory to reasoning'?

**Daniel Filan:**
'Sensory to reasoning' or sensory cognitive algorithms or sensory bits of cognition to reasoning bits of cognition.

**Neel Nanda:**
Yeah. I'd actually argue there's three bits to the spectrum. There's sensory neurons at the start that take the raw inputs like pixels or tokens and convert them to the stuff the model actually wants, reasoning that does a bunch of processing, and then motor neurons which convert the processing to the actual desired output.

**Daniel Filan:**
Yes, of course.

**Neel Nanda:**
I don't know, you get neurons in language models that do fun things. So the word 'nappies', or 'diapers' if you're American, it's split into two tokens. But conceptually the model clearly figures out, 'Oh, "nappies" is here' and there's a retokenization neuron which says, 'Ah, this token is the N in "nappies" and the "nappies" feature is there'. So I should output '-appies'.

**Daniel Filan:**
If I think of there as being these three types of abilities, and maybe there are points on a triangle or you can go around. How much of that triangle do you think mechanistic interpretability is good at, and do you think there are any regions of the triangle where it's going to have to change tack or do something different?

**Neel Nanda:**
The simplistic answer I'd want to give is that we are good at the start and end, and bad at the middle. I'm not even sure how true this is, in part because we're currently just kind of bad at everything. And in part because... So the way I normally think about reverse engineering a network is: you start with a thing that is interpretable - ideally two things that are interpretable - and you try to figure out how the model's gone from one to the other, or at least you just look nearby the one that is interpretable, look at what's being done with it, and try to move outwards.

**Daniel Filan:**
Some sort of induction thing.

**Neel Nanda:**
Yeah (and notably nothing to do with the induction in 'induction heads'). And obviously the input to the model is interpretable. The output of the model is interpretable. We know exactly what these are. Interestingly, in language models, it's often been easier to understand things at the output than things near the input. One important thing to bear in mind when thinking about language model interpretability, which is most of what I'm going to be talking about, is that unlike classic neural networks or convolutional networks, where the key thing is you've got layers of neurons, each layer's input is the output of the previous layer essentially, and you can think of it as iterative steps of processing; there are two big differences with the transformer. The first is this idea of 'the residual stream', and the second is this idea of 'attention heads'. So the idea of the residual stream is the input - this is somewhat used in some image models - the idea is that the input to a layer is actually the accumulated output of all previous layers and the output of that layer just adds to this residual stream.

And in standard framings of things like residual image networks, this is often framed as a skip connection around the layer that's like a random thing you add on, but isn't super important. But I think the way transformers seem to work in practice is that every layer is incrementally updating the central residual stream. One demonstration of this is that there's this technique called [the logit lens](https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens) where you just cut off the last several layers of the model and look at how good it does if you just set those to zero and then convert this bit to tokens. And [it] turns out, models are a lot of the time kind of decent at this, though it varies between models. The reason this matters is it means it's harder to think of the model in terms of early, middle, and late, because processing can just...so I normally think of the thing the model does as having paths from the input via some layers through to the output. And these paths just tend to spend a long time in the residual stream, which makes it harder to reason cleanly about things. So that's the residual stream.

The second messy thing to bear in mind is attention. So transformers are fundamentally sequence modeling networks. Their input is a sequence of tokens which you can basically think of as words or sub words. And at each step they're doing the same processing in parallel for every element of the sequence, there's a separate residual stream for each token position, and it's using the same parameters in parallel. Obviously it needs to move information between positions, because we don't just want it to be a function of only the current token. And the way it does this is with these attention layers that are made up of several attention heads, and each head has some parameters devoted to figuring out which other positions in the sequence are most relevant to the current token.

And it's got a separate set of parameters that says, 'once I figured out which bits are most relevant, what information from that position's residual stream should I move to the current token?' And the reason this matters is that it means that a decent chunk of the transformer's computation comes down to routing information between different positions: figuring out what information to route. And this means that in practice, a decent chunk of what we're good at in transformers is understanding this information routing and how it works.

And some of the things we've understood, it's not obvious to me whether you should even consider this sensory or reasoning stuff. It's just like: the model takes the raw input, does some creative thinking where it figures out which tokens are most relevant to the output on a specific token, and then it learns to route information from there to the end. But once it's figured out which token is relevant, getting the answer is kind of easy. For example, it's trying to figure out what the number at the start of a line is and it learns to look at the number at the start of the previous line, and it's like, the previous line began with five, so this should be six. The hard part of the task is figuring out where the five is or the fact that it's five. Once you've figured that out, it's like, okay, cool. It's a very easy task.

**Daniel Filan:**
Okay. So it sounds like what you're saying is: because of the way transformers work, it's not obvious that you can divide cognition into purely sensory, purely reasoning, or purely motor, or that might not just be the best breakdown.

**Neel Nanda:**
Kind of. I do think that tracks something real about transformers. In part what I'm saying is just I think that sensory, reasoning and motor is a good description of what the neurons in the model are doing, but it also spends a significant amount of its computation on attention. I generally break down what a transformer does into these two separate components, one of which is routing the most relevant information between positions and sequence, and the other one of which is processing the information once it's been brought there. And note that information can be routed around several times.

For example, there was this great work by David Bau and Kevin Meng in their [ROME paper](https://arxiv.org/abs/2202.05262), looking at how models did factual recall. [If you] give it a sentence like, "The Eiffel Tower is located in the city of...," it outputs Paris. And they found reasonably suggestive evidence that the model first routes the separate tokens of the phrase 'the Eiffel Tower' to the final token. It then realizes this is the Eiffel Tower and looks up the fact that it's in Paris, and then that gets routed to the final "of" token in order to predict that Paris comes next.

**Daniel Filan:**
Okay. And how does it look up that the Eiffel Tower's located in Paris?

**Neel Nanda:**
So this is a great question. My prediction for what's happening is that neural networks, in general, seem to represent features - that is, things they have computed about the input - as directions in space, and where they can look up whether a feature is there by projecting onto that direction. And my prediction is that -- "Eiffel Tower" gets tokenized as capital E, I-F-F, E-L, and Tower, and I predict that the model has attention that moves the "E", "iff", and "el" features to the "Tower" token, and then there are some neurons which say, 'If tokens three back is E, tokens two back is "iff", token one back is "el", and the current token is "Tower", then output "Paris" feature.' And then heads late in the network move the "is Paris" feature to the "city of" token, but the "of" token.

Another important thing to know about transformers, which you may not be aware of, is the way models like GPT-3 are trained, is they're just given a great deal of text, just from the internet or books or whatever, and the text is split up into tokens and the model is trained to predict the next token. And the way the attention is set up, information can only move forward in the sequence, not backwards. So, for each position of the sequence, the model is trying to predict the token after that, and its value at that position of sequence can only be a function of that token and earlier tokens.

**Daniel Filan:**
Okay. So, moving back: I want to ask, how do you characterize the kind of thing or the kinds of cognition that mechanistic interpretability currently has a good grasp of, and do you see any walls towards like, "Oh, if we want to do this kind of cognition, we're going to have to think differently"?

**Neel Nanda:**
So, honestly, to me, it feels somewhat more tied to how things are mechanistically represented in the network than it does to exactly what the cognition is. Though both matter. So, a lot of the mental moves that I'm making, when I'm trying to reverse engineer a network, are around discovering that things are localized within the model, that there are some heads or some neurons, or at least some directions in space, that represent the key feature for this problem, and that most other things do not matter for this input and can be safely ignored. And the next thing that matters could be computed solely from the heads or neurons I identified earlier that are important.

And one thing you need to be able to do here is to break down the model's internal representations of things into independently understandable units, like an attention head or a neuron, being the classic thing we'd want to do here. And if computation is localized in this way, then our life is much, much easier. And if it is not localized, then we are much less good at this. There's this core problem called superposition, which we're probably not going to talk about that much later, but in brief, models want to represent features as directions in space.

So, transformers have these MLP layers where there's a linear map followed by a activation function called [GELU](https://arxiv.org/abs/1606.08415), where you just take each element in the vector and apply this weird function that's basically a smoothed out ReLU to it, and then you have a linear map back to the residual stream. So, we expect the MLPs are being used to process information, which means being used to compute some features. We expect the features are going to be represented as directions in space, and-

**Daniel Filan:**
The space being the vector space that's spanned by these neurons, right?

**Neel Nanda:**
Yes. One way to think about it is just the middle bit of the MLP layer is just... you've got this list of, say, 4,000 neurons and each neuron has a scale, has a value, and you can also think about this as a vector in 4,000 dimensional space. And we really hope that the model is using these neurons to represent the features - that the directions that the features correspond to are aligned with one feature per neuron. And this often seemed to be the case in image models. And the main reason you might think it would be the case is that...

So models internally seem to care a lot about computing features of the inputs. Things like 'this token is a verb' or 'this is a proper noun that appears in a European capital' or things like that. And we expect that models want to reason about these independently. And if you have a feature per neuron, you can reason about this independently, because the ReLU or the GELU acts on each neuron independently. But this also forces the model to only have as many features as it has neurons.

And models do this thing called superposition, which you can essentially think of as them simulating a much larger and sparser model, where they have many more features than they have neurons and squash them in, in a way that has a bunch of interference and messiness, but which lets them represent a bunch more features. And this does seem to happen in transformers a bunch and it's really annoying. And it's plausible to me that there's just some cognition that happens aligned with neurons and some that happens that's not aligned with neurons. And there may not be that big a difference in this cognition, but one is currently much harder to interpret than the one without superposition.

The final answer to the question is: we're just much better at interpreting the cognition around attention heads than we are about neurons. And again, attention heads are the bit of the model that's to do with routing information between the different token positions. And in part, because this is just much more legible, you can literally look at the attention patterns in the model - attention pattern being, which previous positions does the head think is most relevant to the current position? It's very easy to be misled by these, but it does give you quite a lot of information.

**Daniel Filan:**
Why is it easy to be misled?

**Neel Nanda:**
So, mechanistically, what an attention head is doing is, for each token position - which I'll call the 'destination token' - it's looking over all possible previous tokens, including the current token - let's call these 'source tokens' - and assigning weights to each of them, such in the weights all add up to one (this isn't actually an important detail) and then copying information from each of the residual streams of those token positions to the current thing.

And importantly, copying the information at the residual stream after layer... I don't know, after layer 10, can potentially have basically nothing to do with the actual token input to that position at layer zero. But if you naively look at an attention pattern, you'll be like, "Ah, it's learning about this token with maybe some contextual information." But it's possible that it's entirely picking up on stuff that was brought in by previous attention heads. And there's some suggestive evidence that models sometimes use really, really easy tokens as spare storage and stuff like that. Or, another thing is that attention patterns need to add up to one, for boring reasons, and sometimes a head wants to be off and it will attend to the first token to be off. And it might look at this and be like, 'Ah, I gave it the sentence, "The cat sat on the mat," and all these heads really care about the token "the". That must be really important.' And it's like, no.

**Daniel Filan:**
Okay. So, I have a few follow-up questions before moving on a little bit. Way back, we said that one difference that language models had compared to these image classification networks is that there's this thing called [the logit lens](https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens) where you could take the transformation that you're eventually going to do on the residual stream at the end of the network to get out these logits, which are basically probabilities of various options for the next token. And you could apply that in the middle of the network, and you could see that it was getting this representation of a decent guess at the answer. It strikes me that you could try doing the same thing in image models, right? Has somebody tried that and found that it doesn't work, to your knowledge?

**Neel Nanda:**
I am not aware of anyone trying this. One thing I will note is that having the input format be the same is very important. And the way that I believe image models, even those with the residual stream tends to work is, they take the input image, which is big, and then progressively scale it down with these pooling layers. And then at the end, they just totally flatten out what they've got and maybe have another couple of fully connected layers before producing an output. And if the shape of the output is not the same as the thing, if the shape of an intermediate activation is not the same as the thing that is then unembedded, mapped to the class outputs, this technique just doesn't even work in principle.

**Daniel Filan:**
Oh. Why not?

**Neel Nanda:**
The naive technique of 'you just delete these layers' doesn't work because-

**Daniel Filan:**
Oh, because the layers are changing the number of dimensions in the residual stream?

**Neel Nanda:**
Yeah, exactly. And in a transformer, the residual stream is this consistent object the entire way through the network, that each thing is incrementally updating. There are a bunch of image models that I think do have something more like a residual stream. In particular, vision transformers are all the rage, and there are a bunch more image architectures. I'm not aware of anyone doing this. I would predict it would kind of work. A further caveat is that a thing you could try is by training another linear map on the earlier activation with a different shape to class probabilities. And this is the kind of thing I'm sure someone has tried.

**Daniel Filan:**
Yeah. Or, you could imagine deleting... So, the way it normally works is, you have some convolutional layers and then you flatten it out or project down to some vector, something that your MLP, multilayer perceptron layers can act on. And you could imagine just deleting some of the convolutional layers, right? So instead of it just being a simple matrix, it's your MLP layers doing the unembedding or getting to the class probabilities. Okay, all right, that's a tangent. Great.

The final follow-up before I go to a second follow-up with a thing I said maybe half an hour ago, is: when we were talking about these types of cognition in these language models, the place you started off was, 'okay, you want to have this sandwich where the pieces of bread are things you understand, the filling is things you don't yet understand, but you can infer it from the things outside'. And you mentioned the very output of these language models was understandable, basically.

And I'm wondering... So I think this is roughly right in cases where, I don't know, you're doing something like 'write what a typical person would write', and that's what the language model's trying to do. You could imagine language models that are used for playing Diplomacy or whatever.

Diplomacy is this board game where you make allies and stab people in the back and fight wars in Europe, basically. And in Diplomacy, you send messages to people like, "Hey, I'm definitely not going to invade you. Do you want to team up on invading Germany instead?" And the meaning of that might not be so obvious, right? So I'm wondering, yeah, how understandable do you think these language model outputs are?

**Neel Nanda:**
Sure. So, first off, to clarify, when I say the output is understandable, I literally mean, when the model says, "Hey, let's go invade Germany," it is outputting the words, "Hey, let's go, Germany." And just, this is actually a significant upgrade over looking in the middle of a transformer, where I just... "What does this mean, man?"

**Daniel Filan:**
Yeah. It's these tensors of numbers and you don't know what the 8.7 means.

**Neel Nanda:**
Yeah. It's just, you just have nowhere to get started unless you can ground it in something that matters. And even if the model was outputting seemingly random gibberish, the fact that I can say things like, "It says flurgle rather than blurble. Why is the flurgle logit higher than the blurble logit? Let's go look at what's happening nearby and see if I can get some traction there"... That's a real thing that I can do. In the particular case of the Diplomacy-playing agent, I would predict that the Diplomacy-playing agent is outputting things like, "Let's go invade Germany," because it has some internal representation of what the person that it's talking to wants, what the agent wants and how to manipulate the person that's talking to you to achieve this. Side bit: [that paper](https://www.science.org/doi/10.1126/science.ade9097) was terrifying.

**Daniel Filan:**
Yeah, right. For our audience, who might not be terrified, what was so scary about that paper?

**Neel Nanda:**
Ah. So, they trained this model to play Diplomacy - this game, which is about, you have a bunch of players and they form shifting alliances and sometimes stab each other in the back, and you need to convince other players to go along with your plans so you can win out over everyone else. And the model they trained, despite their weird [marketing claims](https://ai.facebook.com/research/cicero/diplomacy/) that it was an honest and cooperative player, blatantly lies and manipulates people successfully in a way that gets them to do what it wants and help it win the game. And I'm like, "No! This is the thing I'm most scared about AI learning how to do! What are you doing?"

**Daniel Filan:**
Yeah. I think in their defense they're using a definition of lie, where, as they say on Seinfeld, "[it's not a lie if you believe it](https://www.youtube.com/watch?v=vn_PSJsl0LQ) temporarily, even if you can make sure that you won't believe it later". Yeah, it's a good thing we don't have [world champions of Diplomacy](https://achievement.org/achiever/demis-hassabis-ph-d/) being really, really relevant to how AI turns out, huh? That might be mean.

**Neel Nanda:**
And yeah, I think there's something worth emphasizing here which is, if the model says, "Let's go and invade Germany," and it's lying, if you were just doing some black box analysis, it might be really hard to get any traction here, because the model is lying. It's saying things that seem perfectly reasonable, but aren't actually tracking what it believes. But if you are aiming for the very ambitious goal of actually understanding its cognition behind what it said, then it's a very different question. And if you want to be able to credibly claim to have understood it, then you must've been able to notice why it did and what the hidden things behind this were, such that I think saying its output is not interpretable is a category error in some sense.

**Daniel Filan:**
Do you mean it's a category error in the sense that its output is understandable somehow and you could understand why it did what it did?

**Neel Nanda:**
Yeah. I'm just unconvinced there is such a thing as an uninterpretable output, because there should always be a reason the model output it.

**Daniel Filan:**
But you could say the same thing about any activation or anything happening inside a network, right? There's some reason that it's that way.

**Neel Nanda:**
That's a fair point. I think the thing that I'm pointing out here is something closer to what optimization pressure is placed upon the model, where it's never optimized to have any specific activation, be any specific thing, but it is optimized to have the logit corresponding to the Daniel token be actually related to the Daniel token.

**Daniel Filan:**
Yeah. So, you somehow know that it's at least being graded on the output. So somehow the outputs are going to be related to it getting high grades in the cases where it was tested.

**Neel Nanda:**
Yes.

## Automating mechanistic interpretability <a name="automating-mech-interp"></a>

**Daniel Filan:**
Yeah, that seems fair. Okay. I guess my penultimate question about mechanistic interpretability, generally, is: at the very start, you alluded to this idea that, well, one thing you could do is, okay, we have a bunch of tools such that if you have some neural network, a bunch of humans can spend two years trying to understand what's going on and succeed. But there's this different possibility you mentioned of, can we somehow automate this? Can we get, I don't know, [progress measures](https://arxiv.org/abs/2207.08799), or can we get analysis tools that can just take our neural nets and tell us what we would've concluded if we thought about it really hard?

**Neel Nanda:**
One thing to add to that is I think there's a broad spectrum of automation from 'have a metric' - have a tool that's doing what we would've done anyway - to actually being able to have an AI system take over more and more of the actual cognitive work and judgment calls and intuitions. And a lot of my optimism for a world where we might be able to actually have fully reversed engineered and understood what's going on in a frontier model like GPT-5, is... I expect it's most likely to happen if we have systems that are more capable than the language models we have today, but maybe not quite at human level, that can do a lot of the work for us. Just because something being really labor-intensive is much less of an issue when all jobs are automated, including ours. But that's very much an ambitious goal of the field, not a thing we're near today.

**Daniel Filan:**
Sure, sure. How close do you think we are to automation at any level of the spectrum, and do you see any promising avenues?

**Neel Nanda:**
So, I'm writing [a sequence on 'open problems in mechanistic interpretability'](https://www.alignmentforum.org/s/yivyHaCAmMJ3CqSyj), and I recently wrote a post on [techniques and tooling](https://www.alignmentforum.org/s/yivyHaCAmMJ3CqSyj/p/btasQF7wiCYPsr5qw). So, I actually have unusually crisp thoughts on this. A couple of axes that I break down techniques and approaches on... there's 'general versus specific'. So, general techniques are a broad toolkit that should work for many circuits, including ones we haven't identified yet.

(I should probably actually define circuit. When I say circuit, what I mean is some subpart of a model's parameters and cognition that is doing some specific task. In the Eiffel Tower example, the bit of the model which takes the raw tokens "E, iff, el, Tower" and which produces the "this is an Eiffel Tower" feature would be an example of a circuit, or induction heads, as we'll discuss later.)

So, general techniques are broad toolkits that should work for many circuits. The [logit lens](https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens) that I mentioned earlier is one example of this, that in principle should just work for every sequence. And then there's specific techniques which are focused on identifying a single type of circuit or a single circuit family. One really dumb example is: models often form previous token heads, which attend to the previous token. And surprise! it's really easy to identify these, because you just give a bunch of inputs and you look at the average attention paid to the previous token.

And some heads are really, really high on this and they're obviously previous token heads. And this is an example of a technique that is trivial to automate, and you could just run on every head in any model. A thing I would actually love someone to do is just to write all of these dumb metrics for basic kinds of heads and make a wiki for every head and a bunch of open-source models. This just wouldn't be that hard and would be a cool project.

The next axis is 'exploratory versus confirmatory'. So exploratory techniques are like, "I'm confused. This model is doing something. What is going on? I want to get more data, I want to form hypotheses, I want to get some evidence for or against my hypotheses, and I just want to iterate a bunch." And this tends to be pretty subjective and involve a bunch of human judgment. And it can involve things like visualizing a bunch of data and trying to look for patterns in it, doing some kind of dimensionality reduction, just generating a bunch of ad hoc ideas on the fly or just quick and dirty tools with fast feedback loops that may rely heavily on human interpretation.

And then confirmatory techniques are things that let you take a circuit and say, "I think this circuit is what's going on behind this behavior. I now want to go and rigorously test and verify or falsify this belief." [Redwood Research](https://www.redwoodresearch.org/) have this awesome new algorithm called [causal scrubbing](https://www.alignmentforum.org/s/h95ayYYwMebGEYN5y/p/JvZhhzycHu2Yd57RN), which I think is an excellent, fairly automated, confirmatory technique, though this is all a spectrum. And they claim to me that causal scrubbing can also work as an exploratory technique, because you just keep forming hypotheses and trying to verify them and sometimes they can tell you in which ways they break.

The final axis is 'rigorous versus suggestive', where there's techniques that I think really tell you something real and reliable about a network. One example I like a lot here is activation patching. So the idea behind activation patching is you have two inputs to a model which give two different answers. Like, "The Eiffel Tower is in Paris," and "The Colosseum is in Rome." You feed in the Colosseum input, but you save all of the activations from the Eiffel Tower input and you pick one activation in the model and you edit it and replace it with the relevant activation from the Eiffel Tower run.

For example, you replace the residual stream on the final token at the first layer or something. And you do this for every activation you think is interesting, and you look for ones where this patch is enough to flip the answer from Rome to Paris. And it turns out when you do this that most things don't matter. Some things matter a ton, and that just patching in a single activation can often be enough to significantly flip things.

And so, to my eyes, this is just extremely good evidence of "that thing I copied over is something real and it just contains the core information that differentiated the Eiffel Tower from the Colosseum". And a cool thing you find if you do this is, it turns out that if you patch the activations on the "um" token of the Colosseum, then early on... If you patch in the thing from the Eiffel Tower run early on, the values on the final token of Colosseum are the ones that matter. Everything else doesn't matter. And then there's a band of layers where it slowly moves to the final token and then the Colosseum stops mattering at all and it's the final token of the sentence that matters the most. And this is not particularly surprising, but it's very cute.

And then there's a bunch of techniques that are much more suggestive where it gives some evidence. I think it should be part of a toolkit you use with heavy caution. One example of this is: a really dumb technique for figuring out what a neuron does is you look at its max activating dataset examples. You just run it on a bunch of data and you look at the data that most excites the neuron. And sometimes when you do this, you just look at the texts and you're like, 'okay, these top 10 texts are all recipes. And it activates really strongly on the word "knife" in "carving knife". This is a "carving knife" neuron'.

And there are a bunch of ways this can be misleading. There's this great paper called ['An Interpretability Illusion for BERT'](https://arxiv.org/abs/2104.07143) that found that if a neuron in [BERT](https://arxiv.org/abs/1810.04805) and you give it a bunch of Wikipedia text, it looks like it's about dates and facts. If you give it a bunch of other kinds of text, it's about song lyrics and random stuff like that. And the technique has a bunch of limitations, but I think it tells you something real about the neuron.

And the final axis is how much things are automatable and scalable versus super labor-intensive and painstakingly staring at neurons, looking at weights. And that's roughly how I break down the field of how to think about techniques and tools.

And the main areas where I'm excited to see progress are: I want us to get a lot better at finding circuits. This involves a lot of things. Having good infrastructure, so you can just find the common things. You do a bunch and do them really fast. One of my side projects at the moment has been making this library, called [TransformerLens](https://github.com/neelnanda-io/TransformerLens), that tries to be decent infrastructure for doing all of the things I want to do when I'm trying to reverse engineer GPT-2, like cache all internal activations, edit whatever activations I want, things like that.

Then there's understanding the tools we already have, where do they break? What are the ways you might apply them, and then actually shoot yourself in the foot?

There was this really great [paper](https://arxiv.org/abs/2211.00593) from [Redwood](https://www.redwoodresearch.org/) on [indirect object identification](https://arxiv.org/abs/2211.00593), where they found a circuit in GPT-2 Small to solve this simple grammatical task of indirect object identification. One wild thing they found is that there were these heads that mattered a lot, but if they ablated the heads, just deleted it and set it to zero, then another head in the next layer took over and did that head's task for it. So the head was really important, but this backup head meant that if you deleted the head, it would look unimportant, and my personal guess-

**Daniel Filan:**
Sorry, how did they know that the head was important, if, when you deleted it, a backup head took over?

**Neel Nanda:**
So, the techniques they were using there... one of them was activation patching. You copy the head over into a run on a different input, so as not to flip things. The other technique is, because this head was at the end of a network, you can use this variant of the logit lens, called direct logit attribution, where the idea is that... So the way you compute the logits from the network, essentially the log probabilities over next tokens, is you apply a linear map to the final residual stream, and the residual stream is the sum of the output of every layer of the network.

And you can often break the output of a layer down into the sum of different components, like different heads. And so, you can look at a head's impact on the logits by just applying this linear map to that head's output. And if you do this on the indirect object identification task, you're like, "Oh, head 9 in layer nine massively boosts the correct token, relative to the incorrect token. I guess this head matters." And you delete it and it's like, "Ah, the model's about as good."

But if we look at these other heads, then one head, which used to be suppressing the correct output now does nothing. And this other head that wasn't doing much is now way more insignificant, and I'd not expect this. That's a really useful insight.

Just having a better toolkit. We both understand things, but also, we have more techniques. Then having good gold standards of evidence... This both looks like conceptual understanding of what does it even mean to have found a circuit? But it also looks like good tooling and infrastructure to really verify that your hypothesis is correct. And one of the most important skills you need if you want to do good mech. interp. research, is just the ability to aggressively red-team your results and look at all of the many flaws in them, and just try really hard to break them. And one thing that's pretty hard, especially when you want to grow a field, is having it be the case that other researchers can produce results that you think are compelling, and just having consensus on the field, and what does it even mean to have identified a circuit? What are the standard toolkits I should use to be really sure what I'm doing is correct?

And Redwood's [causal scrubbing](https://www.alignmentforum.org/s/h95ayYYwMebGEYN5y/p/JvZhhzycHu2Yd57RN) work is the best step I've seen here by far. Even that has limitations and shortcomings and ways that multiple hypotheses could seem the same but not be - I think. I haven't engaged with it hard enough so that could be completely wrong. Apologies to [Redwood](https://www.redwoodresearch.org/) in advance.

And then the final thing is just automation, taking all of these things that we know how to do, where I know what I would do if I wanted to spend 1,000 hours reverse-engineering GPT-2 Small and passing over as much as is possible to code and trading researcher time for GPU time.

And at the moment, we're not even at the point where there's a lot of productive things I could just get a Python code program to do, let alone stuff that I think I could reasonably hand off to something like GPT-3. But this seems like a thing that is very important to try to make happen at some point and a thing that I just expect to naturally become easier as the field matures and we do more things.

**Daniel Filan:**
Okay. Cool. So I think this has been an overview of mechanistic interpretability as a whole or at least your view of it. Before we go to your work specifically, if people are interested in getting into the field, do you have any advice or resources for them?

**Neel Nanda:**
Yeah, so actually one of my big side projects at the moment has been trying to make it easier to get into the field. Because I was annoyed how many things I had to learn for myself and I've recently written this post called ['Concrete Steps to Get Started in Mechanistic Interpretability'](https://www.alignmentforum.org/posts/9ezkEb9oGvEi6WoB3/concrete-steps-to-get-started-in-transformer-mechanistic), that tries to just be a definitive point to get into the field and learn about things, and tries to guide you through the prerequisite skills, how to learn more and how to get yourself to the point where you are actually working on an open problem.

Some accompanying things: I have this [comprehensive mechanistic interpretability explainer](https://dynalist.io/d/n2ZWtnoYHrU1s4vnFSAQ519J) where I decided I wanted to write down every concept I thought people should know about mechanistic interpretability with a bunch of examples and context and intuitions, and surrounding things around transformers and machine learning that I thought were underrated. I got carried away and it's 33,000 words, but people tell me that it's very insightful words to read and it might be useful just to reference, to look things up as we're talking in this interview.

I also have the sequence that I mentioned called ['200 Concrete Open Problems in Mech. Interp.'](https://www.alignmentforum.org/s/yivyHaCAmMJ3CqSyj), where I try to map out what I think are the big areas of open problems in the field, how I think about them, why they matter, how I'd approach doing research there; and then list a bunch of problems, attempting to rank them by difficulty and trying to aim for a thing that someone who's excited about the field and has done a bit of skilling up could pick one and maybe get some traction.

## Summarizing the papers <a name="summarizing-the-papers"></a>

**Daniel Filan:**
Okay, cool. And we should have links to those in the description, or if you are reading the transcript, you can just click on the words he said. All right. So we're going to talk about three papers you've helped write. Before we dive into those, I'm wondering if you can give an overview of - perhaps as opposed to mechanistic interpretability as a whole, do you have a vision of your particular line of research in it on a somewhat higher level than just talking about the individual?

**Neel Nanda:**
Like what did I contribute to the paper?

**Daniel Filan:**
No, like...

**Neel Nanda:**
Why does the paper exist?

**Daniel Filan:**
Yeah, if there were a subfield of mechanistic interpretability that these papers comprise, what is that subfield, what's it trying to do?

**Neel Nanda:**
Sure. Okay. So the three papers we want to discuss are ['A Mathematical Framework for Transformer Circuits'](https://transformer-circuits.pub/2021/framework/index.html), which was this excellent work I was part of while I was working at Anthropic, and ['In-Context Learning and Induction Heads'](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html) - another paper I was involved in when I was in Anthropic - and ['Progress Measures for Grokking via Mechanistic Interpretability'](https://arxiv.org/abs/2301.05217), which is this active independent research I did after leaving. I should give the general caveat that I was extremely involved in the grokking work and did quite a lot of the mainline research on my own and I feel well-placed to authoritatively speak about this and claim much of the credit. I was somewhat involved in 'A Mathematical Framework', but my collaborators, [Chris Olah](https://colah.github.io/about.html), [Nelson Elhage](https://nelhage.com/) and Catherine Olsson did far more of the work than I did and deserve a very large amount of the credit, and I was even less involved in 'In-context learning and induction heads' and Catherine, Nelson, Chris and the rest of Anthropic deserve a lot of the credit there. And I can say things about these papers, but these are very much my takes. And also I should thank the co-authors of my grokking work: Lawrence Chan, Jess Smith, Tom Lieberum, and Jacob Steinhardt, who contributed a bunch.

General epistemic caveats and credit sharing out of the way: so ['A Mathematical Framework'](https://transformer-circuits.pub/2021/framework/index.html) is in my opinion far and away the coolest paper I've ever been involved in. So the reason this paper exists fundamentally is that if you are trying to reverse engineer a network, you need to understand what the network is as a mechanistic object, like mathematically what is it as a function. In terms of code, what code would you write to make it, and conceptually, what are the moving parts inside of it and in what ways is it principled to break it down into sub-components, such that you could refer to a circuit made up of several bits, and which ways is it not? And A Mathematical Framework basically just tries to do this for transformers. And in my opinion this has dramatically clarified my personal intuitions for how to think about all this in a way that's just pretty fundamental for reverse engineering a transformer at all.

The paper particularly focuses on attention because as noted, that's one of the biggest differences between transformers and image models where the earlier work happened - Sorry, it's one of the biggest differences between the image model architectures on which the early work happened. A bunch of image models use attention nowadays. There are also some results in the paper where we reverse engineered zero-, one-, and two-layer attention only models and found some interesting circuits, notably induction heads. But to me, this is secondary to just the framework of being able to get traction at all in a way that was grounded and principled and looking at some of the fundamental bits of model.

['In-context learning and induction heads'](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html), it's a very different style of paper. To me, the core themes of the paper are around universality, which is this idea that the same circuits and cognition happen in models of very different scales and different data; training dynamics, where we looked in a model during training; and emergence, or phase transitions, where we found sudden changes in the model during training.

So the headline results in the paper are: so we found this circuit called an 'induction head'. The thing an induction head does is it detects and continues repeated text and we found this in these toy two-layer attention-only models. It turns out that induction heads seem to be a big part of the reason that models can do in-context learning, which is jargon for 'use text very far back in the prompt to predict the next token'. It's kind of surprising that models can do this at all. We're talking, you got a 10 page Word document and it learns to use some text on page 2 to usefully predict what happens after page 10. You can show transformers are good at this by looking at their performance per token by token position and show that it's much better on later tokens. And in a way that's because it's using the earlier tokens, not just because later tokens are inherently different.

And it turns out that this capability seems to significantly rely on these induction heads. These induction heads occur in all models we looked at up to 13 billion parameters, and the induction heads all appear at around the same time in what we called a phase transition during training, where there's a fairly short period where you go from no induction heads to induction heads, and it exactly coincides with the period where it gets good at in-context learning. And yeah, there's a bunch more evidence in the paper about exactly why we believe this, but that's the headline claim. And to me, the family is just using the mechanistic insights we found to understand a real fundamental property of networks, so they can do this in complex learning.

The third category in [my grokking work](https://arxiv.org/abs/2301.05217), I'd put somewhere in the 'science of deep learning' camp where I'm trying to understand some underlying principles of deep learning models and how they work with some focus on phase transitions and emergence.

So 'grokking' is this mystery in deep learning famously found in [this OpenAI paper from the start of last year](https://arxiv.org/abs/2201.02177) where they found that if they train a small model - a transformer in their case - on a simple algorithmic task, such as modular addition or composition of the permutation group or modular multiplication - if they gave it, say, a third of the data to train on, and two thirds as a held-out test set, and kept training it on that third of the data for a really, really long time, it would initially memorize the training data. But then if you trained it for long enough it would suddenly generalize and what they called ['grokked'](https://en.wiktionary.org/wiki/grok), and go from being really, really bad on the unseen data to very good on the unseen data.

**Daniel Filan:**
And in particular, this is strange because during this transition, it's not getting that much better at the task that it's actually being trained on, right?

**Neel Nanda:**
Yeah. There's messy nuance in a way we can get into later, but it's not getting that much better is a reasonable thing to say. 'It's achieved perfect accuracy on that task' is a thing we can confidently say - perfect accuracy on the training data. And so what I did is I trained a smaller model to grok modular addition, a somewhat simplified one layer transformer and that replicated grokking and it clearly grokked and then [I] did some painstaking and high effort reverse engineering where I discovered it learned this wild algorithm that involved trig identities and discrete Fourier transforms to do the modular addition. And that I could just with pretty high confidence, describe how that was implemented in the model - with a bunch of caveats and holes as we may get into later - and then could look at it during training, using this understanding, and design what we call progress measures based on this mechanistic understanding. And looking at it during training, I saw that the sudden grokking actually broke down into three phases of training that I call 'memorization' - where the model memorized the training data - 'circuit formation' where it slowly transitions from the memorized solution to the trig-based generalizing solution, preserving train performance the entire time. And then 'cleanup' where it suddenly got so good at generalization that it's no longer worth keeping around the memorization parameters. Importantly, these models are trained with weight decay that incentivizes them to be simpler, so it decides to get rid of it. And this is what results in the sudden grokking, because when the model is partially memorized and partially generalized, the generalizing bit is really good on the test data, but the memorizing bit is even more bad, such that it on net is just bad performing.

And the high level principles from this are: I think it's a good proof of concept that a promising way to do science of deep learning and understand these models is by building a model organism - like a specific model to study that exhibits a mysterious phenomenon, clearly understanding it and then using this understanding to ground an exploration of the deeper principles. And the second reason I think it's exciting is: emergence or phase transitions are this thing that just seems to recur in a bunch of models on a bunch of model scales and across a bunch of different dimensions. Like GPT-3 can do addition in a way that smaller models can't, for example, or how induction heads suddenly form during training. And we can totally imagine that some very alignment-relevant things might suddenly emerge. There's this fascinating [paper from Alexander Pan](https://arxiv.org/abs/2201.03544) about how reward hacking can emerge if you scale a model up.

**Daniel Filan:**
Where 'reward hacking' is getting high reward in a way that you didn't think it would, roughly?

**Neel Nanda:**
Yes. To take an extremely modern example, a student getting a good mark on their essay because ChatGPT wrote it for them, rather than because they actually learned the content. One hoped-for solution here is this idea of [progress measures](https://arxiv.org/abs/2207.08799), which are some metrics you can run on the model which track progress towards the eventual emergent phenomena. And the work is a proof of concept for: if you really understand some example of the behavior you care about, you can design mechanistic interpretability-inspired progress measures. And I think the work has many limitations including as models of both of these two things. But I also think it was a very cool and fun project that had some useful insights to teach.

**Daniel Filan:**
OK, cool. So now we've had a quick summary of each paper. How about we dive into each one individually?

**Neel Nanda:**
Sounds good.

## 'A Mathematical Framework for Transformer Circuits' <a name="math-framework"></a>

**Daniel Filan:**
All right. So the first paper is ['A Mathematical Framework for Transformer Circuits'](https://transformer-circuits.pub/2021/framework/index.html). It's got a lot of authors. The first few are [Nelson Elhage](https://nelhage.com/), Neel Nanda, Catherine Olsson, [Tom Henighan](https://tomhenighan.com/), Nicholas Joseph, [Ben Mann](https://benjmann.net/). And then several people at Anthropic and then [Chris Olah](https://colah.github.io/about.html) being the corresponding author.

**Neel Nanda:**
There's an author contribution statement at the end if people want to decipher the long impenetrable list.

**Daniel Filan:**
Yeah. One thing I actually like about Anthropic is, I found these author contribution statements to be good reading and it gives some insight into what this list means. Yeah, I think it's nice.

**Neel Nanda:**
Yeah, Chris has [an amazing blog post](https://colah.github.io/posts/2019-05-Collaboration/) about credit and how he thinks about the importance of sharing credit generously and fairly with academic work. He puts a lot of thought into contribution statements. I respect him a lot for that.

**Daniel Filan:**
So [the paper is] taking these versions of transformers that don't have the multilayer perceptron parts, that are just attention heads up to two layers, and building a mathematical framework for them and talking about the circuits. So one thing that struck me about this paper is, it emphasizes this idea that you've got these different attention heads, but you can also have paths between attention heads and it puts the emphasis on the paths rather than the individual heads.

**Neel Nanda:**
Importantly, you can have paths *through* attention heads as well.

**Daniel Filan:**
Sorry, what do you mean by that distinction?

**Neel Nanda:**
So for example, let's say you've got a head which just says, "I'm going to predict that the previous token is going to come next," then this head would attend the previous token and it would then take the thing it attends to, apply a linear map and that gets contributed to the residual stream at the destination, and this map's called the OV circuit. And then you'd have a path from the previous token input through the OV circuit of the head to the current token output. And I would say that that entire path is the correct thing to try to interpret.

**Daniel Filan:**
So one thing that scares me here is if you have some collection of things, there's going to be a lot more paths through the various things than there are individual things to think through. And presumably the paths... there's some commonality between all the paths that run through a particular location, namely that location. So I'm wondering ultimately, is this path analysis going to be too unwieldy to be useful?

**Neel Nanda:**
Sure. So I definitely should say that a bunch of the things in the paper, I think were either just fairly early techniques that haven't necessarily turned out to be incredibly useful or that scalable, or were proofs of concept for how to think about things. And specifically there's a bunch of equations where we fully expand out every path in the network and it's this godawful combinatorial explosion of things. And I completely agree that no, that is not a reasonable thing to interpret. So to give some framing, the way I think about what is going on inside in your network is you have bits of the network that are interpretable, not necessarily by definition interpretable, but we can put some meaning onto them and the network cares about them having a certain meaning. And then there are bits of the network that are hard or messy or in the particular case of a transformer are things we expect to be highly compressed and highly under superposition.

So as a concrete example of this, let's say we've got a zero layer transformer - this is the dumbest possible transformer, that just embeds the input token to vectors in space, does nothing to them, and then immediately applies a linear map to convert them to output logits. And importantly, this can't move information between positions because there's no attention, so it is essentially just mapping the current token to guesses for the next token. So it's just a big memorized table of bigram statistics. And if we just go through the data, we can just generate a big table of bigram statistics, it's not very hard, but the way the model's been forced to implement it is that it's had to map the 50,000 input tokens to this tiny 500 dimensional bottleneck space and then back up to 50,000.

And it's presumably learned to compress this enormous table of stuff into something that can be done via this pretty narrow linear map. And one of the claims in the paper is: the correct way to interpret some circuit like this is that we don't try to interpret what the things in the 500 dimensional bottleneck mean. We try to interpret the start and the end and we assume that the stuff in the middle is some heavily compressed nonsense. So to my eyes, a lot of the stuff about paths is saying, "Cool, we want to focus on things that start and end at things that have meaning, where the things that have meaning are input tokens, attention patterns, MLP activations and output logits. And we want to mostly focus on paths between these." So the goal is not to study every path. The goal is to find things we want to understand and look for the paths leading to those that matter.

**Daniel Filan:**
Yeah. So this actually gets to another worry I had about this paper. So take the zero layer transformer case where you have this embedding matrix that's 50,000 inputs to 50 outputs or something like that. And then an un-embedding matrix which is 50 inputs to 50,000 outputs. And the paper basically says, "Okay, we're just going to multiply them and think of this 50,000 inputs, 50,000 output function and just think about that function." And it seems like this is going to forget the fact that it's this very special type of 50,000 input to 50,000 output function, namely the type - it's called 'low rank' in mathematics - it's the type that can be decomposed this way into this intermediate 50-component representation. How scary is it that we're forgetting this key low-rank structure in these matrices?

**Neel Nanda:**
This is a good question. The honest answer is, I don't really know, but my guess is it's not that big a deal. So the reason I guess it's not that big a deal is, one of the things you need to do if you're trying to get anywhere in reverse engineering a network is distinguishing between the things that matter and the things that it's trying to do in some sense, from the things that don't matter or are just noise or small errors which won't really affect the underlying cognition.

And to me, a lot of the losses that happen when you smush things through a bottleneck are more of the flavor, noise or things that aren't really important to the underlying computation than they are things that are key to it. And obviously this is dangerous reasoning, especially if you are worried about a system that is adversarially trying to defeat our tools and which might think through things like, "Oh, people are going to look at all of my multiplied out matrices, but by carefully choosing which bits I leave out in the low rank decomposition, I can smuggle in subtle flaws." And it's like, yes, this approach will totally miss that, but my prediction is just that isn't a thing that matters that much, at least for the kinds of network problems we're dealing with at the moment. The second point would be, one important caveat to the bigrams case is that the model is not learning a bigram table. It's not learning the matrix that is the bigram table. It's having a matrix that once fed through a softmax will be a good approximation to a bigram table.

**Daniel Filan:**
Where if we haven't said this already, 'bigram' means 'probability of the next thing given the current thing'.

**Neel Nanda:**
Yes. Like how Nanda is more likely to follow Neel than 'florble' is to follow Neel.

**Daniel Filan:**
So it's a matrix that when you add a softmax, it's a bigram table.

**Neel Nanda:**
Yes. And this adds a ton more degrees of freedom, especially when you appreciate that there's just lots of tokens where it's just so unlikely they occur, the model just doesn't really care.

**Daniel Filan:**
Mm-hmm.

**Neel Nanda:**
And one way to think about it is, the model is trying to compress in information about each of the 50,000 tokens of the bottleneck and then decompress this and there's going to be a bunch of noise. And the model needs to do something non-linear to clean up the noise, and a softmax is just a really powerful way of cleaning up a bunch of noise if you know what you're doing.

**Daniel Filan:**
Okay, cool. Yeah. So getting into the mathematical framework... Actually, my first question is, it mentions that it only really tries to understand the attention heads and just basically ignores the multilayer perceptron layers for now. So there's this paper that I read once. The title was ['Transformer Feed-Forward Layers are Key-Value Memories'](https://arxiv.org/abs/2012.14913). They basically argued that, "Look, we know what the MLP layers are doing. They're just, basically you take some token representation, match to various keys you have stored and then say, 'Ah, for this key, here's this value vector that I'm going to produce.'" And basically implying that this was done in a way that was relatively human-understandable and that was the answer. I'm curious, to what extent do you think that's true? And if it's true, in that case, why delete it from this paper?

**Neel Nanda:**
Sure. So first, just to de-confuse an annoying notation clash, when that paper says 'key' and 'value', this has nothing to do with keys and values in attention heads. And it's instead in the computer science sense of, you've got a dictionary, you look up an entry in the dictionary and you return whatever is stored at that entry.

So is this true? So I haven't engaged deeply with the results of that paper. I've anecdotally heard some claims that some of the results replicate and some don't seem to replicate very well. But obviously this is not particularly reliable data. My personal intuition would be that this is part of the story, but a pretty limited part and where it's very easy for this to be a small part of what a neuron is doing and for this to seem like it's the whole part of what it is doing. One thing to emphasize is just that a lot of this is just stuff that can be done with a lookup table. In some sense, if you want to do an if statement like, "If this vector is present in the residual stream, put this other vector into the residual stream." Then when you have the first vector there, why didn't you just put in the second vector there at the same time?

It needs to be using the non-linearity in some interesting way or possibly some other dumb inside baseball thing models need to care about like, the residual stream gets bigger over time and there's this thing called a layer norm that scales it down to be uniform size. So this means everything decays over time. And so maybe you want signal boosting neurons that boost important things or something.

But other than weird stuff like that, that very plausibly is a good fraction of what transformer neurons do. I expect quite a lot of what transformer neurons are doing is internal processing that this doesn't really capture. Within the framework we had earlier of 'the way you reverse engineer a thing is you find interpretable things at the start and at the end, and you use that as the sandwich to figure out the middle'. This is specifically saying the inputs to the model and the outputs to the model are the interesting interpretable things and everything else is only interesting as in it connects one to the other. And this is probably true of some neurons, I don't think it's true of most neurons.

**Daniel Filan:**
Okay. So, let's head back to what was in the paper.

**Neel Nanda:**
Yeah. I do want to just reemphasize that these are my off-the-cuff hot takes about this paper. I think the actual reason we did not discuss that paper much is just, that wasn't the point of the mathematical framework. The goal is to understand attention. Plausibly, this paper has some insights about MLPs, but the focus of the paper was not on understanding MLPs.

**Daniel Filan:**
Okay, fair enough.

**Neel Nanda:**
And also, MLPs are really hard.

### How attention works <a name="how-attention-works"></a>

**Daniel Filan:**
Yeah. So speaking of understanding attention, one thing that is in this paper is this idea of there being three kinds of composition of attention heads where they can Q compose, K compose and V compose. To be honest, the explanation of the paper left me a little bit mystified. So can you tell us what are Q, K and V composition?

**Neel Nanda:**
Sure. Yes. I'm very sad that one was misleading because this is one of the bits of the paper I feel like I can claim some credit for, giving those names. All right. So what's going on here? So first, I should just actually explain how an attention head works.

**Daniel Filan:**
All right.

**Neel Nanda:**
So the way I think about attention is, the model's full of attention layers. Each layer consists of several attention heads, each with their own parameters acting in parallel and independently. And the output of the layer is just the sum of the output of each head. And so the way things work is that the model is doing two tasks. One of the tasks is, so you've got this destination token, which is the current position. It's figuring out which sources to pull information from. And once it's figured out where to move things from, it's trying to figure out what information to move from those to the current position. And where to move things from and to is determined by these two sets of weight matrices called the query weights (WQ) and the key weights (WK). And what happens is the model maps the destination residual stream to a query with this linear map [WQ].

**Daniel Filan:**
Okay.

**Neel Nanda:**
The model maps every source residual stream to a key with the second linear map [WK] and then takes the dot product of every pair of source key and destination query. And then it wants the positions where the key aligns most with the query to have high attention pattern, but it also wants the attention pattern to all be positive reals that add up to one. So for each destination, it does a softmax over the previous things to get an attention pattern.

The key thing to take away from all this is, in contrast to simpler sequence modeling architectures like convolutional networks, where the model needs to solve this fundamental problem of which sequence positions are most relevant to the current position. In convolutional networks, they just hard-code nearby things relevant, far away things not relevant. In image conv nets it's kind of a 2D thing, but it's the same principle. In RNNs, recurrent neural networks, the thing that gets baked in is that it's just kind of recursively going through the sequence. So nearby stuff is obviously more relevant. The way transformers work is we let them spend some parameters figuring out which information is most relevant. Heads tend to specialize in looking for different kinds of previous tokens. There are previous token heads. There are 'attend to the most recent full stop' heads. There's 'attend to the subject of the sentence' heads. There's just a bunch of heads that do a bunch of different things.

**Daniel Filan:**
Okay.

**Neel Nanda:**
Transformers spend about a 12th of their parameters doing this, 'where to move information from' computation. One intuition for thinking about queries and keys is, a query is like, 'what am I looking for within the context of this head?' A key is like, 'what do I have to offer?' Then these are both directions in this internal space and if they're aligned, it attends from the destination to that source. Though the right way to think about neural networks, as always, is: it has parameters that let it do a thing, it figures out a reasonable way to make that happen. It does not need to conform to my expectations of how this is reasonable. All things I'm saying are just so stories.

So once it's figured out this attention pattern, it then needs to figure out what information to move. The way it does this is it's got another set of parameters, WV, that map the source residual stream to this value vector. This is a small internal head dimension. It then averages all of the values using the attention pattern to get a kind of mixed value. So values have gone from having a source position axis to having a destination position axis, and then we apply a final set of weights, WO, to get the output of the head and we add up every head's output and add it to the residual stream.

**Daniel Filan:**
Okay.

**Neel Nanda:**
Some important things to note about this decomposition, like this thing I just outlined. Also, you should really go stare at some of the algebra in the paper because this is not good audio content. Sorry.

**Daniel Filan:**
Yeah. People might be listening and reading at the same time possibly.

**Neel Nanda:**
Great. Yes. I also have a [mechanistic interpretability explainer](https://dynalist.io/d/n2ZWtnoYHrU1s4vnFSAQ519J) with a section on this paper that might be helpful.

**Daniel Filan:**
Okay.

**Neel Nanda:**
So something to draw out: The first is that most operations here are linear. The only things that are not linear are the softmax where we took the query-key dot products and converted them to a distribution. In particular, if you think of the keys as a big tensor with a source position axis and the queries as a big tensor with a destination position axis, the dot product is actually just a matrix multiply between these two tensors. It turns out that you can think of this as taking the source residual stream, multiplying it by the matrix (WK^T WQ), and then multiplying and then dotting that with the destination residual stream.

**Daniel Filan:**
Okay.

**Neel Nanda:**
Or equivalently thinking, this is a bilinear form that takes in these two residual streams. The key takeaway from this is that the matrix (WK^T WQ) is the main thing that matters, and we could double WK and halve WQ and it wouldn't make a difference. Or we could apply some really weird transformation to the internal space, but make it so the product of the two is the same and this wouldn't matter.

**Daniel Filan:**
You could rotate one and de-rotate the other, for instance.

**Neel Nanda:**
Yeah, exactly. So one of the takeaways for me from this work is that the right way to think about attention is just: there's a parameterized matrix that determines how attention works for this head, and keys and queries are not intrinsically, on their own, important things. The next thing to draw out is that the value... So the value step's interesting. So what's going on is you start with a tensor with a source residual stream dimension and a D model dimension, which is just the actual content of the residual stream. Then WV acts on the content of the residual stream dimension.

The attention pattern acts on the position dimension, and then WO acts on the residual stream content dimension.

**Daniel Filan:**
Okay.

**Neel Nanda:**
Matrix multiplies on different axes of a tensor - it doesn't matter which order they go. So the two consequences of this, the first is we've got another low rank factorized matrix (WV x WO), which are called WOV. A really annoying thing is that in maths matrices multiply on the left, in code they multiply on the right, and I always get confused about which is the right convention to use when talking about it. I apologize in advance. So this means that values are not intrinsically meaningful because we could rotate WV and de-rotate WO, and it would be exactly the same. The second important thing is that because these are on different axes, it doesn't matter which destination positions are getting this information.

Every destination position gets the exact same information from a source position. It can only choose how much to weight it. Finally, this just kind of reinforces the idea that attention is about routing information because we are multiplying by the attention pattern on the source dimension access. Cool. One thing that's kind of helpful to sometimes do is to imagine just freezing the attention patterns. So you just save and cache them and no longer compute them and then think about editing the network or changing the inputs. You can think of the attention patterns as this kind of dynamic wiring that gets set up of how to move information around. If you freeze it, then every attention head's actually just linear and it just lets information flow through the network.

### Composing attention heads <a name="composing-attention-heads"></a>

**Neel Nanda:**
Okay. End long tangent on how to think about attention heads. So Q, K, and V composition. So there are kind of three things the model is doing. There's where should I move information from? Where should I move it to? 'From' is key, 'to' is query. And there's what information should I move from the source once I figured out where the source is? Which is 'value'. These correspond to Q, K and V composition. Q comes from the destination residual stream, K and V come from the source residual stream.

So the input to the attention head is the residual stream, which is the sum of the output of all previous layers. One component in here is the token embedding. Just, what is the actual input to the model at this position? What's the input text? Your null hypothesis should be, that's the main thing that matters. That's the thing that obviously distinguishes this position from all other positions. But it's also the case that the model has access to all of the other bits of the residual stream that are not just the token embeddings. Presumably sometimes heads are significantly using those and sometimes they're basically just using the token embeddings. One of the surprising things that turns out to often be true about models, though is far from universally true, is that this computation tends to be kind of sparse. There are some bits of the input that matter a lot and many bits of the input that completely don't matter specifically for that head's computation.

**Daniel Filan:**
Okay. For a given head's computation?

**Neel Nanda:**
Yes, for a given head's computation, and that the head kind of picks up on those and not on others.

So for example, Q composition is when the Q input, the 'where should we move information to', is significantly influenced by something that is not the token embeddings.

Composition here being 'usefully using the output of a previous component', just because that's the output of a previous function. It's being entered into a new function. These are composing.

**Daniel Filan:**
Okay.

**Neel Nanda:**
So a non-obvious thing here is why am I separating K and V composition given that they're both from the source residual stream? The key thing here is that these are low rank matrices, which means they're essentially identifying a low rank subspace of the residual stream.

**Daniel Filan:**
Or a small dimensional subspace. Yeah.

**Neel Nanda:**
Yes. Small dimensional. You can kind of think of this as the residual stream, let's say 1,000 dimensional vector, and we just cut off the first 50, ignore everything else. But the network can choose whatever coordinate system it wants. This means that the head can choose different subspaces of the destination or source residual streams to pick up on if it so chooses. This is great. This lets the model choose different bits to compose with. This matters because ... So one of the things that mechanistic interpretability tries hard to exploit is the fact that model computation often seems kind of sparse, where it just isn't using a lot of the available information because only some matters. So the hope is that often bits will be using some specific information in the residual stream for the query, some for the key, some for the value.

So let's try to build intuition with an example of what each of these might actually look like. So the easiest intuition is thinking about things in terms of using contextual information about the current token. So let's take the sentence, "The Eiffel Tower is located in the city of ..." Let's say the model has computed on the "Tower" token, the fact that it's the Eiffel Tower. It's computed a 'this is a famous tourist destination in a European capital' feature, and has computed a 'is in Paris' feature. At the end of the 'is located in the city of' bit, it's computed a feature saying, "I am looking for a city."

Then you could imagine a head, which uses Q composition to pick up on the, "I am looking for something in the city of ..." feature, which importantly you can't get just from, "Of ..." It's pretty important to have the full context. It uses K composition to look for the, "I am a famous tourist destination in a European capital" thing. Because it's like, ah, that's clearly relevant. So it learns to look at the "tower" token and then it uses V composition to specifically move the information, "Is located in the city of Paris," rather than "tower".

**Daniel Filan:**
Sure. Am I right to think that K composition is when your choice of what questions to ask of previous tokens is influenced by previous attention heads?

**Neel Nanda:**
It can be MLPs as well. But in this case we hate MLPs and pretend they don't exist.

**Daniel Filan:**
Yeah, pretending MLPs don't exist...that K composition is saying, "Okay, what information I'm going to try to get out of previous tokens or previous token streams to tell me which one to care about, is going to be influenced by the output of previous layers". And V composition is, "What information that I'm going to extract - rather than how I choose which token to attend to - when that is influenced by the output of previous layers?"

**Neel Nanda:**
Yes. I should also say in the example I gave, the head is using Q, K and V composition, which maybe made it not the best example. In general, I expect that in practice every head is probably going to be using all of them to some degree, but it's firstly just useful to conceptually disentangle them to yourself. Secondly, it's useful to be able to reason through examples where it would rely on some of these. A final point would be in a lot of the path analysis stuff covered in the paper where they do things like have these ridiculous sums of tensor products that multiply with each other. Oh yeah. I should say if anyone reading the paper, you can just totally skip all of the tensor product notation. I think the confusion-

**Daniel Filan:**
Oh, I liked it.

**Neel Nanda:**
I mean I like it, but I think the confusion per unit effort is really bad on that, and on the eigenvalue score stuff.

**Daniel Filan:**
Oh, I liked that too! Those...

**Neel Nanda:**
I mean they're very fun if you have a math background, but when I've mentored people learning mech. interp., they've all been like, "Ah, I understood everything in the paper, but then I spent many hours on tensor products, eigenvalues, and there's also this bit where they talk about virtual weights in the introduction.

**Daniel Filan:**
Oh yeah?

**Neel Nanda:**
Where they have WI times WO, and it's kind of weird notation. People just seem to spend more effort on those three bits than the rest of the paper put together and they're just not essential.

**Daniel Filan:**
Okay, cool

**Neel Nanda:**
Rant over. So yeah, in the path analysis stuff, everything being described there is V composition because... So the way to think about a head is it's got these three inputs, Q, K and V. But Q and K both terminate in producing the attention pattern, which is kind of the intrinsically meaningful thing in the head. Then V goes out and can be composed with future things. The idea of path analysis is like, let's kind of ignore the attention patterns for now and how they're computed and just look at how information is routed through the network and how it can go through any head at each layer. It's like, 'ah, we could totally have a path that goes through head five in layer zero, head seven in layer one and head eight in layer two' and that's three levels of V composition.

**Daniel Filan:**
So now that we've sort of got that under control, one thing that it says in the paper is that small two layer models seem to often, though not always, have a very simple structure of composition where the only type of composition is K composition between a single first layer head, and some of its second layer heads. So there's something that's still kind of surprising to me about the fact that yeah, turns out only one of these types of composition mattered.

**Neel Nanda:**
I should give the caveat, I think that statement's probably just wrong.

**Daniel Filan:**
Oh, you think that's wrong?

**Neel Nanda:**
So I have yet to fully explore and understand this. So Redwood Research have been doing this work with [causal scrubbing](https://www.alignmentforum.org/s/h95ayYYwMebGEYN5y/p/JvZhhzycHu2Yd57RN). They're trying to build this rigorous approach to really understand what a circuit is doing and they found... 

### Induction heads <a name="induction-heads"></a>

**Neel Nanda:**
So, okay, so I should explain what an induction head is before we jump into this, I think?

**Daniel Filan:**
Okay, sure. Yeah.

**Neel Nanda:**
So an induction head is this circuit that appears in two layer attention-only models that looks at the current token and it says, "Has this token appeared in the past? If yes, then let's assume the thing that came after it is going to come next." So you could imagine that if you've got a token "James" and you want to figure out what comes next, you're like, is this a piece about James Bond? Is this piece about James Cameron? Is it a piece about just some random dude called James? And you look in the past and you're like, okay, so last time James happened, Bond came after, I'll assume Bond comes next.

**Daniel Filan:**
Sure.

**Neel Nanda:**
The way it's implemented really needs two heads. So the intuition for why is that attention is fundamentally about looking at all pairs of source and destination tokens and finding the pairs that are most aligned and moving information from one of those to the other. But each token in isolation has no access to its surrounding context except via just knowing its position. We want the induction head to attend from "James" to the first occurrence of "Bond" because Bond is preceded by James, which is a copy of the current token. But this is fundamentally contextual info. So the way it's implemented is there's a previous token head in layer zero, which for each token kind of writes to some hidden subspace saying this is what the previous token was.

So now the Bond position has the, "I am the Bond token," and a, "I am proceeded by James," feature. Then the induction head, its key is whatever was written in that hidden subspace. "My previous token is James" is the key, and the query is just "I am James" and it aligns and it looks and it realizes it should look at the Bond token because it is preceded by James, and then it predicts that Bond comes next.

**Daniel Filan:**
So somehow information is moving... From every token information is moving from the token previously, and that token's residual stream gets a little bit that says, "Hey, my last thing was this." Then the attention head, you're looking for tokens whose most previous thing was the thing you're on right now. Then it has to pass forward the value of, "Okay, what actually is the value of this token that I'm attending to in the past?" Is that roughly right?

**Neel Nanda:**
Yep. Yeah, this is an example of K composition and not Q or V composition because the query is just James, the value is just Bond. The hard part is saying 'attend to the thing that is immediately preceded by myself'.

It's also worth just briefly staring at this as a central example of how transformers differ from networks without attention or with really baked-in ways of moving information between positions. This was some genuinely non-trivial computation about which previous positions were most relevant to the current position.

So if you actually look at the thing I just explained of James and James Bond, it's actually kind of a bad use case for induction because what if James is followed by a comma or "James said", you don't really want to predict that comes next.

**Daniel Filan:**
Yeah.

**Neel Nanda:**
So one thing you want to check is how consistent the thing that comes after is. Actually probably a better use case of an induction head is just repeated strings. You've got a forum post and someone further in the thread quotes an above comment. That text on its own is kind of weird and hard to predict, but if you know it's appeared previously, it's really easy. But knowing just that a single word, "the," was repeated is much less useful than knowing that the previous five words are repeated. So [what Redwood found](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html#comment-redwood) was that even in a two layer attention only model, the induction head didn't actually do amazingly if you literally only let it use strict induction - strict induction being "attend to token immediately after copy of current token" - but did significantly better if you allowed it to have a kind of fuzzier window.

**Daniel Filan:**
Okay.

**Neel Nanda:**
Of what are the several tokens nearest the current one? Use that for Q composition. So use the contextual information of the current token of what the nearby things are and then expand your K composition to use the contextual info of what the recent tokens there are.

**Daniel Filan:**
Okay.

**Neel Nanda:**
But you don't need V composition.

**Daniel Filan:**
Okay. So sorry, the Q composition is: it's using previously computed information to figure out that it should be asking for... Sorry, how is that Q composition?

**Neel Nanda:**
So let's say you've got a newspaper headline which then gets repeated just before the main text.

**Daniel Filan:**
Yep.

**Neel Nanda:**
That's like, "Red cat found in Berkeley." The word, "in," being repeated later, it's not actually that interesting because in's a common word. But, "Red cat found in ..." being repeated is interesting. So the "in" token is going to have early heads which copy, "Red Cat found."

**Daniel Filan:**
Okay.

**Neel Nanda:**
Then it does something mechanistically to look at the headline where it's "Berkeley". The Berkeley token is preceded by, "Red Cat found in ..."

**Daniel Filan:**
Okay. Is the Q composition that at the "in" token, some previous heads told me that the last few words were, "Red Cat found ..." And so that tells me that I'm looking for things in the past that were preceded by "Red Cat found?"

**Neel Nanda:**
Yes, exactly.

**Daniel Filan:**
Okay.

**Neel Nanda:**
For high level intuition, Q composition is when you use context on the destination, which you are 'cause you're using, "Red Cat found," as well as, "in." K composition is when you're using it on the source. To be clear, I have not personally verified these results or played around with them that much, but I find this on the face of it pretty plausible. Yeah, to be clear, I think the takeaway from all this is that the strict induction circuit that I described is correct and is important, but it's not necessarily the entirety of all that is going on.

**Daniel Filan:**
Sure. So I had two questions of a similar kind where one of which was like, why do we only see K composition? Another being like, why is it just these strict induction heads that are popping up? It sounds like the answer to both is that's not true, there are other things going on.

**Neel Nanda:**
Yes. Also, obviously the models we studied in that paper are not the same models that [Redwood](https://www.redwoodresearch.org/) studied, which is not the same as the attention only models that I've trained and open sourced. It could totally be different between all of these. The exact way you embed positional information can matter and everything's annoying.

**Daniel Filan:**
Okay.

**Neel Nanda:**
But yeah, my guess is that induction is just such an important thing for the model to be doing that it is worth its while to figure this out.

**Daniel Filan:**
Yeah. I guess a related question is like, why do you think induction was the first thing that you found? Do you think that's just because it's a very basic ability? Or something else?

**Neel Nanda:**
Why is it the only interesting thing a two layer model does? Or why did we find it before we found other things?

**Daniel Filan:**
Let's ask, why did you find it before you found other things?

**Neel Nanda:**
Sure. So I joined Anthropic after they discovered induction heads, so I can't really speak to the historiography here. But my guess would basically just be that induction is really useful if you want to predict the next token. So there's this great game from [Redwood](https://www.redwoodresearch.org/), the ['predict the next token' game](http://rr-lm-game.herokuapp.com/) where you can just actually get some natural language and try to predict the next token. It's really hard. Highly recommend the experience.

So one of the things that's really hard about it is there's lots of words where it's inherently pretty uncertain what should come next. There's a full stop, what comes next? I mean, I can make guesses. Begins with a capital. It's probably a common word. It's pretty hard to do better than that even just if I really know what I'm doing. It's just not that often the case that you can just very confidently say what should come next. If you're a language model, finding a case where you can confidently say what comes next is really useful. It just is the case that text often has repeated strings. If you can identify a 30-token repeated string after the first five tokens, you've got like 25 perfect scores for free. This is just actually really useful. So models learn to spend a lot of resources on this.

It's also a pretty distinctive thing to spot if you're looking at an attention pattern because... So if you've got this, if say token zero matches token 100, etc., until token 30 and token 130, if you look at the attention pattern, it's going to have this stripe because every token is attending 99 tokens back. So this constant offset means you get a diagonal. If you just visualize the attention pattern as a heat map there's this notable diagonal stripe. This is very distinctive when you're looking at heads on a bunch of data.

**Daniel Filan:**
Fair enough.

**Neel Nanda:**
So yeah, combination of visually distinctive and it's just so useful to the model that it devotes a bunch of parameters to it and it's pretty clean and sharp.

## 'In-context Learning and Induction Heads' <a name="iclih"></a>

**Daniel Filan:**
Yeah, now that we're talking about induction heads, do you want to move to the next paper ['In-context Learning and Induction Heads'](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html)? So this paper, again, many authors, the ones that are designated as core research or infrastructure contributors are Catherine Olsson, [Nelson Elhage](https://nelhage.com/), Neel Nanda, Nicholas Joseph, Nova DasSarma, [Tom Henighan](https://tomhenighan.com/), and [Ben Mann](https://benjmann.net/). And Chris Olah is the corresponding author. So I think a while ago you gave us a brief summary of what was in this paper, but that was a while ago. So can you remind us what's going on in this paper?

**Neel Nanda:**
Sure. So busy paper, lots of things. The core claim is: the circuit induction heads recur in models at all scales we've studied up to about 13 billion parameters and they seem to be core to this capability of in-context learning using far back words in the text to usefully predict the next token. And as far as we can tell, they're the main mechanism by which this happens. It is also the case that they all seem to occur during a fairly narrow band of training in this phenomenon we call a phase transition. That this is such a big deal that there's a visible bump in the loss curve when this happens and a bunch of suggestive evidence of interesting things, like these underlie more complex model behavior. We found a few-shot learning head that was also an induction head and [when] given a bunch of examples in text, looked at the example most relevant to the current problem -

**Daniel Filan:**
Interesting.

**Neel Nanda:**
And it's like, "What? How is this a thing!"

### The multiplicity of induction heads <a name="ih-multiplicity"></a>

**Daniel Filan:**
Yeah, I have a bunch of questions about this. Maybe sort of related to my first question: the bare bones definition of an induction head, which is like, 'look at the last time you saw this thing and say what the next thing is', or something that helps you do that task... I think in this paper you define an induction head by: something which does that on a random string of tokens that's been repeated. So things which just look back to the last occurrence of this random token and show you the next one. If you just think of that, it's not clear why you need more than one of these, right? It feels like one task. So yeah, why are there more than one induction head?

**Neel Nanda:**
This is a great question. If anyone listening to this figures it out, please let me know.

**Daniel Filan:**
Okay.

**Neel Nanda:**
So yeah, I don't really know. Here are some guesses. So guess one would be that it's ... One hypothesis is it's just that it's kind of useful to the model to have a higher rank OV circuit. So we force the model to squash all of the information it moves into this 64 dimensional head dimension. But if you've got two heads that have exactly the same attention pattern, then it de facto is compressing the residual stream into a 128 dimensional thing. This is just higher fidelity. And maybe that's useful. To do the strict induction behavior of literally 'predict the thing you attend to is going to come next', you don't even need 64 dimensions. In theory, you can do it with two dimensions. Which is a very cute result I found recently.

**Daniel Filan:**
How can you, because you need to copy over the data of the last word, right?

**Neel Nanda:**
So the construction is you find each element of the 50,000 word vocab, map them to a point evenly spaced around the unit circle, and then to get the output for the kth token, you project onto the kth direction.

**Daniel Filan:**
Yeah, and you can multiply it by something and then softmax gets you just the ...

**Neel Nanda:**
Yeah. Softmax can basically act as an argmax and it's biggest on the right direction. Yeah, I noticed this when I was writing a review/comment on Anthropic's new ['Toy models and memorization'](https://transformer-circuits.pub/2023/toy-double-descent/index.html) paper. They have this fun notebook they link where they go through this more rigorously and look at how asymptotically the weight norm you need to do this grows a lot. Yes. In practice, when I tried training a model to do this with 1,000 data points, it kind of struggled, but it could do 100 really easily. My prediction is it's just like, when you add more dimensions, it can get a ton more mileage out of them by making them evenly spaced on the unit sphere or something.

Anyway. Yes. Interestingly, this is very different from Anthropic's work on superposition where it seemed really hard to compress features. The key difference is that if you're trying to memorize the range of activation, it's just a single point rather than a full range. And it's much easier to squash lots of points into the same space. Anyway, total tangent. But if the model's doing things that aren't literally copying, it's pretty useful. Or if it just needs to deal with a bunch of noise and other garbage.

**Daniel Filan:**
Yeah.

**Neel Nanda:**
Secondly, the model probably wants to do a lot of subtle variants on induction. So I mentioned the one that was like, 'let's check if the several most recent tokens do this', you could totally imagine a thing that's like, 'check if the most recent token does this. If so do X'. Then a second thing which checks strictly 'do the most recent 10 tokens do this? If yes, let's be way more confident in our answer'.

Then there's a deeper question of... so in my opinion, induction heads are actually a deep and fundamental mechanism of how models do things because... So, this is kind of skipping to the bit of the paper on why are induction heads relevant to in-context learning.

**Daniel Filan:**
Sure.

**Neel Nanda:**
Where, so my guess is that fundamentally the way you're going to do in-context learning is you're going to distill out the bits in each section of the text into what exactly is going on here, and you're going to do some kind of search to find which distilled things from earlier in the text are relevant to where you are now.

And there's kind of two ways you could implement an algorithm like this. One of them is asymmetric, where you learn a lookup table, where you're like, if A is happening right now and B occurred in the past, B is relevant to A. One example might be if you're reading a paper, then the relevant thing in the past would be the abstract of the paper or the title and you could distill out some features from that. And then most things in the text care about those, but not vice versa. But this can get kind of complicated because you just need to learn a pretty big lookup table.

But then the other kind of thing you can do is symmetric, where you say if A is relevant to B, then B is also relevant to A. And this is just a much more efficient algorithm because if you want to learn 10 symmetric relations, you can just map everything to the same hidden latent space and just be like... Sorry, that was too jargon-y. Let's say you think that all names are relevant to all other names. Rather than learning a kind of 'Neel to Daniel, Daniel to Neel, Daniel to Jane, Jane to Daniel', et cetera, massive lookup table of links, you can just map every name to an 'is name' feature and then just look up matches between the 'is name'.

**Daniel Filan:**
Sure.

**Neel Nanda:**
And to me, this just seems like a kind of fundamental algorithm for how you would do search when you are storing things as directions in space and looking for matches. And importantly, this scales linearly with the number of features rather than quadratically, like lookup tables do. So this is just much more efficient. And the kind of induction we've been discussing so far is the thing where the features you are matching are just literally token values.

**Daniel Filan:**
Yep.

**Neel Nanda:**
But you can totally imagine this being done more abstractly. For example, we found this translation head which could attend from a word in a sentence of English to the thing immediately after the relevant word in the French sentence. And the natural guess [for] what's going on here, is that the model has learned to distill what's going on and to say, 'okay, this is French'. This is just the semantic content of the text rather than the actual tokens. And then French and English get mapped to the same space and it's easy to match them.

Note that what I'm describing is actually more like a duplicate token head, where it's attending from a token to copies of itself. But if you have that, extending it to an induction head seems pretty easy. And I believe we also found heads that attended from a word in English to the same word in French. And an interesting thing about these translation heads is that not only are they English to French, they're also French to English, German to English, English to German, French to German, et cetera. And they also seem to work as induction heads.

**Daniel Filan:**
Sorry, is it that the same head does French to English, English to French, French to German, German to English?

**Neel Nanda:**
Yes. There's a fun thing in the paper where you can go play around with the attention pattern and I found a couple of heads like that in [GPT-J](https://huggingface.co/EleutherAI/gpt-j-6B) and figuring out what's up with those is on my long-term to-do list.

**Daniel Filan:**
Okay.

**Neel Nanda:**
Also, it's one of the problems in my ['concrete open problems' sequence](https://www.alignmentforum.org/s/yivyHaCAmMJ3CqSyj), so then if anyone wants to go try that, I'm very excited to see what you find. Anyway. Yeah. And this translation head is also an induction head, and my guess is it's just the same fundamental algorithm of 'map things to latent space, look for matches, look at the thing immediately after match', and that the model has just learned how to do something sensible here. Yeah. So going back to the original question, my guess would be that one of the reasons it has many induction heads is it's doing subtly different things on varying degrees of fanciness along this algorithm.

**Daniel Filan:**
Yeah. How subtle do you think the differences are? You might imagine that there's just one kind of thing which is induction heads, and there are small variances between them, but in this paper they're sort of defined functionally as 'on this type of data, they do this thing'. And you could imagine that there are several kinds of things that have this ability and you're grouping this heterogeneous collection. How homogeneous or heterogeneous do you think induction heads are?

**Neel Nanda:**
Ah, yes. So another important fact about induction heads is you get anti-induction heads.

**Daniel Filan:**
Oh really?

**Neel Nanda:**
An anti-induction head is a head which has the induction pattern, but it suppresses the token it attends to.

**Daniel Filan:**
Huh.

**Neel Nanda:**
It says, 'okay, this is James. I'm going to look at what comes after James in the past. It's Bond. Bond is now less likely'.

**Daniel Filan:**
Yeah.

**Neel Nanda:**
And why does this exist?

**Daniel Filan:**
I guess you could sort of imagine it for patterns. So imagine I use a bunch of phrases like "Get the salt and get the pepper" or "Get the pepper, get the salt." And I say that all the time. Well, "get the", it could be "pepper" or "salt". And if you know that the last time it was "pepper", it's definitely not "pepper" this time. And so that leaves "salt". That kind of thing strikes me as one possibility.

**Neel Nanda:**
I like that. Yeah, it's an interesting idea. In this ['Indirect object identification' paper](https://arxiv.org/abs/2211.00593), they found this interesting phenomenon of, there were name-mover head, which attended to the name that was the correct answer, and negative name-mover heads, which I think attended to also the correct name, but suppressed it. And then when you ablated the name moving head, some of the negative name movers kind of acted as backups and significantly reduced their negative behavior. And my guess is that that was a result of dropout, which GPT-2 was trained with.

I have no idea whether the models in our paper were trained with dropout or not, but I could imagine this being a thing that matters. I can also imagine what you said being relevant. I could also imagine... So one of the strengths and problems of induction is just the model is very... Induction is one of the things where you can be really confident that what you're doing is correct if you're a model and just get very high probabilities. But this means you often drown out everything else because [for] most things you can't be incredibly confident and it's just incremental shifts on the margin. So if you want to turn off induction, that's also hard. Yeah.

**Daniel Filan:**
But going back a bit, there's this question, are induction heads all basically the same kind of thing, or are there lots of different types of things that are induction heads? So you brought up anti-induction heads that can sometimes act as induction heads.

**Neel Nanda:**
Yes. Yeah. I don't really know if I've got a better answer. Things are messy. Another form of messiness is that kind of, if a bunch of heads have the same induction-y attention patterns, then it's like... in some sense the model doesn't care about the fact that the OV matrices are different between the two heads. It only cares about the combined twice the rank OV matrix. And if the attention patterns are purely fixed, it will just optimize that.

**Daniel Filan:**
Yeah.

**Neel Nanda:**
And this is weird. It's possible the model just decided to have a bunch and then distributes some more sophisticated computation among them. It's also plausible the model just wants to do different kinds of induction, wants to have different attention patterns. Yeah. One cute side project is, I made this thing I call [Induction Mosaic](https://www.neelnanda.io/mosaic) with a heat map of the induction heads in 40 open source models. And this is a fun thing that makes visceral [that] there are so many induction heads, man, and larger models have way more.

**Daniel Filan:**
Like a constant fraction of their capacity? Or is it sort of superlinear or sublinear in how many heads they have?

**Neel Nanda:**
That is a great question and I have not checked. Let me just go eyeball the heat map. Yeah, I mean looking at these, I would broadly argue that the fraction of heads that are induction-y goes down a bit as you go up, but it's kind of sketchy and I don't trust my ability to eyeball this properly. Also, different model architectures seem to potentially have different rates of induction heads even at the same size. This is very cute.

**Daniel Filan:**
If I want tons of induction heads, what architecture should I use?

**Neel Nanda:**
Let's see. [GPT Neo](https://github.com/EleutherAI/gpt-neo) seems to have a lot. Oh [GPT-2 XL](https://huggingface.co/gpt2-xl) has tons. Go use the big GPT-2 architecture. That's my advice.

**Daniel Filan:**
Okay. Yeah. My next big question about the big picture of the paper is the argument is that induction heads, there's just this particular phase during training where they start occurring. And to me, it seems sudden and not gradual. And that strikes me as kind of weird. Why do you think they appear suddenly and not just gradually throughout training more and more?

**Neel Nanda:**
This is a great question. One thing to point out is that there just hasn't been that much investigation of different circuits and their training dynamics. And there's the kind of intuitive null hypothesis... There's the kind of intuition of obviously all things are gradual, we're doing gradient descent, it's kind of smooth and continuous. But I kind of assert that I don't think people have checked that hard. And I have the toy hypothesis that actually most circuits happen in phase changes and induction heads were just sufficiently obvious enough of a big deal that we noticed.

**Daniel Filan:**
Yeah. So if things do suddenly occur, to me that implies that something has to happen before they occur, right? If they occur gradually over training, you'd think like, 'Ah, there's just always some pressure for them to occur'. If they spend a while not happening and then suddenly they appear out of nowhere, presumably there's a trigger, right?

**Neel Nanda:**
That's maybe better left to discussion of [my grokking work](https://arxiv.org/abs/2301.05217), since I think it fits more naturally in there.

**Daniel Filan:**
That's fair enough. Yeah, we can talk more about this during the grokking paper.

**Neel Nanda:**
Yeah, I think my answers for 'why does grokking happen?' overlap a bunch with my answers for why induction heads might happen as a phase transition.

### Lines of evidence <a name="lines-of-evidence"></a>

**Daniel Filan:**
Yeah, I did group these papers for a reason. So more on the ['In-context learning and induction heads'](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html) paper. So on a methodological note, the structure of the paper is there are these, how many...?

**Neel Nanda:**
Six arguments?

**Daniel Filan:**
Six arguments.

**Neel Nanda:**
Though Argument 6 is kind of cheating.

**Daniel Filan:**
Oh, okay. Yeah. Okay. So let's say five.

**Neel Nanda:**
Argument 6 is 'Arguments 1 to 5 were a thing. They were way stronger for smaller models than large models. But you know, man, this obviously generalizes', which I think is a correct argument but it doesn't deserve a number.

**Daniel Filan:**
Yeah. Let's say five different kinds of evidence.

**Neel Nanda:**
Yes.

**Daniel Filan:**
And I don't know, sometimes I'll read these papers from Anthropic where they're like, 'Yeah, we have these K lines of evidence for this thing being true'. And I rarely see papers where 'there were three lines of evidence that this thing was true, but the fourth ruled it out'. So yeah, I guess my question is how much... Is it in fact the case that the fourth line of evidence often rules a thing out? Or when I see these publications are they just like, 'okay, we need to confirm this five different ways to satisfy all of the reviewers'?

**Neel Nanda:**
I don't know if I've got enough data that I can really give an answer about what tends to happen in practice. My guess from a more sociological perspective is that what happens is people do exploratory research. As you're doing research, you're accumulating a bunch of evidence for or against. The evidence is often murky and confusing and it's easy to trick yourself and shoot yourself in the foot. And as you go, you are trying to be like, what am I missing? How could this beautiful, elegant hypothesis be completely wrong or missing a really simple explanation that is less pretty? As has happened to multiple projects that I've mentored.

At this point, you aren't really thinking in terms of polished lines of evidence. It's more like, 'here is just this collection of evidence, it's a bit janky and a bit weird, but kind of gets to the thing that I care about'. And then you kind of smoothly ish transition to a point where you're like, 'Okay, I've got enough circumstantial evidence. I've tried to red team it a bunch and I've mostly failed to break it. I'm pretty confident this is what's happening'. It's less discrete than this. Normally it would be like, 'Okay, I'm pretty sure this particular bit is what's happening, but I'm still confused about all of this other stuff'. And then eventually reaching a point you're like, 'Okay, I think I've just got a sufficiently clear picture of what's going on that I'm convinced. Let's go and do a bunch of confirmatory work'.

**Daniel Filan:**
Sure.

**Neel Nanda:**
And really try to nail things down, get a bunch of evidence, et cetera. Yeah. Which makes it hard to fit into the 'lines of evidence' ontology. Because to me, each of these is a polished investigation in its own right, including multiple times when we had major bugs in the code that invalidated the results and we had to rerun a bunch of things. And things like that.

**Daniel Filan:**
Okay. So part of where I'm coming from with this question is wondering if the more of these I read in a paper, the more convinced I should be by the results. Should I think of these arguments as, there are earlier versions of them that could tell you that you were wrong about earlier stage results? And therefore they're all kind of adding evidentiary weight because they could have killed a thing in its infancy?

**Neel Nanda:**
I don't quite understand the question, sorry.

**Daniel Filan:**
So the question is, suppose I'm reading a paper and it's offering different lines of evidence or different arguments for 'this circuit does this thing' or something. I'm wondering, should I be impressed by the fact that there are more than three? Or should I be like, 'Look, they're just piling it on at this point. To the extent that this could have been wrong, it's not going to be caught by arguments four through six'.

**Neel Nanda:**
This is a good question. So I feel like the thing you want to track here is the correlation between the types of evidence.

**Daniel Filan:**
Yeah.

**Neel Nanda:**
So there's one example. There's the question of exactly what you mean by induction heads, where, as I outlined, there's this kind of fuzzier induction that uses a bit of Q composition as well as K composition to check for longer prefix matches. There's also a totally different way of implementing an induction head where you look at the positional embedding at copies of the current token, and then you look at the token with the positional embedding one after those. And this is equivalent to induction, but it's a totally different mechanism. And I would say that the results of the paper hold equally well if the things we're calling induction heads implemented with either of those two mechanisms or fuzzy variants of either of those. And I think that this is an important thing to bear in mind.

But just like, I think that... Okay, no, I actually take that back slightly. Argument 2, we had an architecture shift that made strict induction heads doable in one layer models, which made a big difference.

**Daniel Filan:**
Okay. And would that have only helped one of the implementations?

**Neel Nanda:**
Yes. Specifically the idea was, rather than the key being just the value of the source token, the key is now a linear combination of the source residual stream and the one before with a learned parameter waiting the two.

**Daniel Filan:**
Yeah. And that helps with the copy the previous value, but it doesn't help with the shift of positional embedding.

**Neel Nanda:**
And it also doesn't help with fuzzy induction, where you have a longer prefix match.

**Daniel Filan:**
Yeah. Yeah. But I think you were about to get to another point.

**Neel Nanda:**
Yeah, I think I've just disproved the point I was going to make, which is that...

**Daniel Filan:**
Oh great.

**Neel Nanda:**
There are some implicit assumptions and correlations that go into things where, 'what do they mean by induction head?' is kind of an implicit correlate of a bunch of the evidence in the paper. And the thing you want to track is 'How correlated are the arguments, and what are the shared assumptions they make?' Where piling on a bunch of things with the same shared assumptions is boring, but piling on things with very different angles is much more interesting.

**Daniel Filan:**
Sure.

**Neel Nanda:**
And in this case, the smeared key stuff is just quite different because it's not relying on some intuition around what exactly is an induction head. It's not relying on the fact that we can label heads in a model as induction heads or not, and then analyze how important they are. It's instead relying on our mechanistic understanding. The concrete result is they found that smeared key models do not have an induction bump and do not have a phase transition, where they form induction heads or get way better at in-context learning. And that one layer models are still pretty good at in-context learning if they have smeared keys.

**Daniel Filan:**
Okay.

**Neel Nanda:**
And are much better than if they don't. Yeah. So the interesting thing there is [that] this gives pretty strong credence to our mechanistic claim that the circuit is pretty important. Though it does not prove that all induction heads in large models are because of this circuit, or that this is actually the way it is used in large models, just that it's an important part.

Anyway. So the key thing here is just: look at the lines of evidence and say, do these feel diverse to me? And I mean, looking at them, they're pretty diverse.

### Evolution in loss-space <a name="loss-space-evolution"></a>

**Daniel Filan:**
All right, cool. Next, I have a methodological question about one particular part of the paper that I found kind of interesting.

**Neel Nanda:**
Sure. I should add the caveat that I did not do the methods of this paper, but I'll do my best.

**Daniel Filan:**
All right. So you might not know. So there's this part where you take models and represent them by the loss they get on various tokens in various parts of text, right?

**Neel Nanda:**
Yep.

**Daniel Filan:**
And then there's this step where you do principal component analysis where you say, Ah, here's one axis in which models can vary in loss space. Here's another one, here's the third, and you get the two most important principal components. And then you plot how do models vary during training along these two axes.

**Neel Nanda:**
Yep.

**Daniel Filan:**
And you show that when induction heads start to form, it turns a corner in this space.

**Neel Nanda:**
Yep. Such a cute result.

**Daniel Filan:**
Yeah. Okay, I had a bunch of questions about this. So I guess my first question was like, there's this principal component analysis on the losses. It struck me as strange that the principal component analysis was on the space of losses of the model rather than just the outputs of the transformer. Do you know why that choice was made?

**Neel Nanda:**
As in why it's on the log probabilities rather than the logits?

**Daniel Filan:**
Well, it was on the losses, not the log probability.

**Neel Nanda:**
That's the same in cross entropy loss, pretty sure.

**Daniel Filan:**
Oh, okay.

**Neel Nanda:**
So cross entropy loss is the average log prob paid to the correct next token.

**Daniel Filan:**
Oh yeah.

**Neel Nanda:**
So per token loss is just the correct next log prob.

**Daniel Filan:**
Yep. But the losses depend on what the actual next token was, outputs do not.

**Neel Nanda:**
Yes. It's not all log probs, it's specifically the correct next log prob.

**Daniel Filan:**
Yeah.

**Neel Nanda:**
I mean I haven't thought about deeply about this. To me, it just seems more intuitive. The thing optimization pressure is applied to is the log prob of the correct next token.

**Daniel Filan:**
Okay. And so if I'm thinking about things during training where training pushes things, I want to think about the losses? Is that roughly it?

**Neel Nanda:**
Yes. So one of the uncertain questions to me here is, so there's things the model doesn't need to care about, and it has a lot of freedom over. Things like 'what is the average value of my logits?' It could add 50 to every logit and it's completely fine. It doesn't change the log prob at all, and I'm like, I do not care if the model decides to add 50 to all of the logits, this is just an arbitrary degree of freedom.

**Daniel Filan:**
Sure.

**Neel Nanda:**
It can also fiddle around a lot with the incorrect log probs where it subtracts 50 from a bunch of logits that just didn't matter. And it's like, this doesn't really change the [log-sum-exp](https://en.wikipedia.org/wiki/LogSumExp). So it doesn't really change the output of the log softmax on the correct token, but it changes a bunch on the incorrect ones. And I just don't care about those because they aren't things that models trying to achieve. A caveat to this is that specific circuits may make claims about what these should be. This is actually one of the things we leveraged a lot in my grokking work, but, I don't know.

**Daniel Filan:**
Okay.

**Neel Nanda:**
It wouldn't surprise me if both give similar results.

**Daniel Filan:**
But by taking the losses, you're just definitely being invariant to stuff that doesn't matter?

**Neel Nanda:**
Yes.

**Daniel Filan:**
Okay. So another question I have about this PCA is: I want to know what the principal axes were. I have a guess about what the principal axes were, and I want to potentially know if I'm right. So my guess would be that there's one principle axis, which is just how much loss overall the models had. And then maybe, I don't know, maybe this is just inspired by the paper, but maybe there's another principal axis, which is like, do you improve your loss over the course of training? So were those principal axes? Or what were they?

**Neel Nanda:**
So I completely don't know. I will say it can't just be that the model generally has good loss because PCA is about variance. So it automatically normalizes the mean.

**Daniel Filan:**
But this is different. It's the PCA over different models, right? And so some models could have better overall loss, no?

**Neel Nanda:**
I believe it's PCA for the same model across checkpoints.

**Daniel Filan:**
Across training checkpoints, right? Yeah. So there could be an axis of, if you're low on the axis, it's like...

**Neel Nanda:**
Yeah, sorry. Yeah.

**Daniel Filan:**
...at the start of training. And if you're high on the axis, it's like during training when it's lower loss.

**Neel Nanda:**
Yeah, I agree with that. So I guess in that case you'd predict that one of these is just generally positive for all tokens or something?

**Daniel Filan:**
Yes.

**Neel Nanda:**
I have absolutely no idea. One prediction I would make is that it's probably not actually the case that each principal component is interpretable, but rather that there is an interpretable basis that spans the first couple of principal components mostly.

**Daniel Filan:**
Yeah. Can I give you my argument for why that's the first principal component?

**Neel Nanda:**
Go for it.

**Daniel Filan:**
If you look at these plots of how the model is moving along in this space, the model is always moving to the left, along the first principal component, it's always moving in one direction over training. So that's my brief argument that this might be what the first principal component is.

**Neel Nanda:**
I like it.

**Daniel Filan:**
I have no idea what's going on with the second principal component.

**Neel Nanda:**
Yeah, I agree. What? It's also just like, why does it go down and then up and then basically horizontal? What?

**Daniel Filan:**
Yeah.

**Neel Nanda:**
Life is pain.

**Daniel Filan:**
Yeah. This is another thing, because if I know that it turns around, then I want to know, okay, what's going on with this new direction? Which is closely tied to what is the second principal component?

**Neel Nanda:**
Yep.

**Daniel Filan:**
So if it were the case that the second principal component was improvement in loss over time...

**Neel Nanda:**
You mean improvement in loss over the context window?

**Daniel Filan:**
Over the context window, yeah. But then I don't think you would see change of the second principal component before the induction heads show up. So I think that's proving me wrong on that guess.

**Neel Nanda:**
Yes. If anything, you'd see slight improvement in the same direction because, skip trigrams, which are a thing one of their models can do that we didn't get round to talking about in ['A Mathematical Framework'](https://transformer-circuits.pub/2021/framework/index.html), are things that improve in-context learning.

**Daniel Filan:**
Right. Things that you and I did not get around to talking to about, but are definitely in the paper and people should enjoy reading about them.

**Neel Nanda:**
Yes. And also in [a video walkthrough of that paper](https://www.youtube.com/watch?v=KV5gbOmHbjU), if you want to hear me personally give hot takes about them.

**Daniel Filan:**
All right. Those are my questions about the PCA.

**Neel Nanda:**
I mean, yeah. The way I recommend thinking about the PCA thing is just like, oh, that's really weird. Why is there a kink?

**Daniel Filan:**
I don't know. I still want to know, right? What is this direction? That seems so interesting.

**Neel Nanda:**
I mean, I agree.

### Mysteries of in-context learning <a name="icl-mysteries"></a>

**Daniel Filan:**
Cool. Finally, I want to ask, so at the end of this paper there's [some mysteries](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html#curiosities) that you pose, right? So one of the mysteries is that there's this constant in-context learning score, right?

**Neel Nanda:**
Yeah. What? That still confuses me so much.

**Daniel Filan:**
So specifically what's going on is, when you look at how much is the loss improving between the 50th token and the 500th token or the 80th token and the 1000th token or whatever...Once you fix, 'how much does the loss decrease between this token and this later token?' Different models, which look different in general, they tend to have the same amount of loss decrease, at least... Or is this just at the end of training? This might just be the end of training.

**Neel Nanda:**
I think it's just a true statement after the induction bump. Yeah.

**Daniel Filan:**
Sure. So with the benefit of time, there's been a while since this paper was put out, are we any more clear about what's going on here?

**Neel Nanda:**
Not really. It's weird. I mean, the paper does make the point that if you compare the model's per token loss at different points in the context to the smallest model, it has gotten most of the benefit that it's ever going to get by token 10, and doesn't obviously get better as... Or we compare the 13 billion parameter model to the 13 million parameter model, and it's like 1.2 [nats](https://en.wikipedia.org/wiki/Nat_(unit)) better per token. And by token 100 it's pretty good. By token 8,000, it's still about as good. It's actually regressed slightly. At token 10, it's like one nat better, rather than 1.2 or 1.18.

And I was like, okay, maybe it's just a lot better at doing the kind of short term stuff. This is just kind of weird and surprising. Stuff like the translation heads, I feel like must be relevant to improving loss. One hypothesis is that just a lot of the stuff that models do is not actually that relevant to changing the loss because there's just so many tasks that get increasingly niche. And so a bunch of the things where I'm like 'Obviously a tiny model could not track long range dependencies in this way' just aren't important enough to really matter in the context of the loss. But yeah, no, I would not have predicted this.

**Daniel Filan:**
All right. And then there's this other mystery, which is that you can look at the loss derivatives. So these are derivatives, you have some architecture and you look at how the loss changes if you train on 1% more tokens or something. In this part, you find that during the phase change, this derivative starts going up, and then once induction heads start appearing, the derivative goes down - but loss should be negative. So whatever. It starts going up, then it goes down suddenly while these induction heads are forming, then it goes up again, still while the induction heads are forming, and then, it starts leveling off. And also, these derivatives, they like... If you compare models of different sizes before the induction heads start appearing, the small models improve faster. But after the induction heads start improving, larger models are improving faster. So, it's like... It's a strange plot. Something's going on around where these induction heads are popping up. At the time this paper was published, it was a mystery. Is there any light shed on this mystery in the time since?

**Neel Nanda:**
Nope. Still a mystery.

**Daniel Filan:**
Okay. Good to know.

**Neel Nanda:**
Yeah, I'm finding it kind of interesting looking back on this and I'm like, "Oh yeah. I'm still confused about that", and it's just never felt like a priority to try to follow up on.

**Daniel Filan:**
All right.

**Neel Nanda:**
There's a lot of work that I'd really love someone to go do in mechanistic interpretability.

## 'Progress measures for grokking via mechanistic interpretability' <a name="progress-measures"></a>

### How neural nets learn modular addition <a name="nn-mod-add"></a>

**Daniel Filan:**
Sure. Well, on that note, should we move to the work you did do on mechanistic interpretability: ['Progress measures for grokking via mechanistic interpretability'](https://arxiv.org/abs/2301.05217). We are actually recording while it is [under review](https://openreview.net/forum?id=9XFSbDPmdW) [for ICLR](https://iclr.cc/Conferences/2023) and I'm seeing the 'under review' topic so I don't actually know the author list. Can you tell me who it is?

**Neel Nanda:**
Yes. So the author list is me, [Lawrence Chan](https://chanlawrence.me/), [Tom Lieberman](https://tomfrederik.github.io/), Jess Smith, and [Jacob Steinhardt](https://jsteinhardt.stat.berkeley.edu/).

**Daniel Filan:**
Okay, cool. And yeah, can you remind us of an overview of what's going on in this paper?

**Neel Nanda:**
Yeah, so grokking is this weird, mysterious phenomenon found in this OpenAI paper. You train small models on algorithmic tasks like modular addition and you give them access to a third of the data and you keep training it on that same third. They initially memorize the data, then if you keep training it on the same data for a really long time, it suddenly generalizes even though it's seen nothing new.

**Daniel Filan:**
And when you say they memorize the data, what you mean is that they aren't at all accurate on data points that they haven't seen initially?

**Neel Nanda:**
Yes. They are significantly worse than random.

**Daniel Filan:**
Worse than random?

**Neel Nanda:**
Yes. So, this is not actually that surprising. So, the reason is in order to get random loss, you need to output a uniform distribution on stuff you haven't seen before. But, this is a non-trivial task. You need to say, 'I have not seen this before, so be uniform'. But, because you get perfect accuracy on all training data, you never care about output [being] uniform.

**Daniel Filan:**
Right. Right.

**Neel Nanda:**
But, this is not an issue with non-stupid tasks because there's enough things you are confused about that you want to output uniform by default.

**Daniel Filan:**
Okay.

**Neel Nanda:**
Anyway, tangent. So, I was like, 'I have no idea what is happening here. Also, this is a tiny model. I should be able to reverse engineer this'. So, we found that you could exhibit grokking in a one-layer transformer and you could further simplify it to have no layer norm and no biases, credits to Tom Lieberum for discovering this. And then I reverse engineered what it was doing, which was this wild Discrete Fourier Transform and trig-identity-based algorithm, which turns out to have deep links to representation theory and character theory and weird stuff.

**Daniel Filan:**
Yeah. I mean, I don't know, to me it makes a lot of sense once you see it - it's like, 'Oh yeah, of course that's how you should do it'.

**Neel Nanda:**
I agree. It took me a week and a half of staring at the weights until I figured out what it was doing, another week or two of refining it. Now I'm like, 'Oh yeah, obviously this is how you do modular addition if you're a neural network'. But I do want to emphasize that this was multiple weeks of effort and being confused. It was emphatically not obvious in foresight.

**Daniel Filan:**
Yep, yep. That seems totally correct.

**Neel Nanda:**
Though I do know of a DeepMind researcher who independently came up with the algorithm. So it's not like a genius thing or something.

**Daniel Filan:**
I mean, I don't know. DeepMind researchers can be pretty smart

**Neel Nanda:**
Sorry, let me... That came out wrong. What I mean is it's not like a flash of inspiration that only one person could have had that was incredibly idiosyncratic and lucky. It was like... This is not an unreasonable thing for people who engage with the problem to converge upon.

**Daniel Filan:**
Yep.

**Neel Nanda:**
Anyway. Yeah, so I reverse engineered the algorithm, I then devised both some qualitative ways I could just look at the model and inspect the circuits it had learned, but also some quantitative progress measures to measure progress towards the solution. And then I ran these on the model during training and figured out that during training, what seemed to be happening was that it first just purely memorized and formed what I'll call the 'memorizing' circuit, which just didn't at all engage with structure in the training data but just purely memorized it and does terribly on the test data.

Importantly, it does way worse than random because it adds a ton of noise. Then there's this long phase that looks like a plateau in the train loss. But is actually what we call 'circuit formation', where it's slowly learning the generalizing algorithm and transitioning from a dependence on the memorizing algorithm to a dependence on the generalizing algorithm. I'm not fully confident on exactly what's going on at a weight-based level, how much it's just got a module for memorization and a module for generalization, and it's scaling up one and scaling down the other versus literally cannibalizing the memorization weights for the generalization algorithm. But it's transitioning between the two while preserving train loss and it's somewhat improving on test loss, but it's still much worse than random.

And the reason for this is that the memorization component just adds so much noise and other garbage to the test performance that even though there's a pretty good generalizing circuit, it's not sufficient to overcome the noise. And then as it gets closer to the generalizing solution, things somewhat accelerate and then eventually it gets to a point where it just decides to clean up the memorizing solution and it just gets rid of the component of that in the output and suddenly figures out how to generalize. And this is when you see it grok, but it's not that it suddenly just learns the correct solution. It's that the correct solution was there all along and it just uncovered it. And I can give some interesting underlying intuitions.

**Daniel Filan:**
Yeah. I'm going to have a bunch of questions about underlying intuitions. But first, just to get our story straight: so you noticed this algorithm that basically involves taking these input numbers and imagining this unit circle and rotating some multiple of that many degrees and then using some trigonometry identities to do what you wanted to end up doing.

**Neel Nanda:**
Can I try to give a better explanation of the algorithm?

**Daniel Filan:**
Yeah, sure.

**Neel Nanda:**
So the way to think about it is: modular addition [modulo N] is fundamentally about rotation around the unit circle. Or at least, it is equivalent to thinking about rotation around the unit circle of angle (say) 2 pi / N. And you can think of the integer a as 'rotate by the angle 2 pi a / N'.

And you can represent this with cos(2 pi a / N) and sin(2 pi a / N), which kind of parametrize that rotation. And you can take the two inputs a and b, rotate by 2 pi a / N and 2 pi b / N, and you can compose them to get the rotation 2 pi (a + b) / N.

And this is now the sum, but it's also the sum mod N because it wraps around the circle if you get too big, and you can compute this by just taking multiplication of pairs of the trig terms and adding them using trig identities. And then to get the cth logit, you rotate backwards by 2 pi c / N to get a rotation by 2 pi (a + b - c) / N. And you project this onto the axis of the circle. And this is one, if you've done nothing, i.e. c = a + b mod N, and it's less than one if you've done some rotation. So this is biggest at the correct answer.

**Daniel Filan:**
Yep. And that's the basis of this algorithm. But you do it at different speeds of moving around the circle, right?

**Neel Nanda:**
Yes. The model spontaneously forms five to six sub-modules for different angles where each angle is some multiple of 2 pi / N. And we call these the key frequencies.

The neurons spontaneously cluster into a cluster per frequency and the output logits are the sum of a 'cos((a + b - c) times frequency)' term for each of the five frequencies. The frequencies chosen are basically arbitrary.

The reason it wants multiple rather than just one is that it is technically true that softmax can act as an argmax. It just takes the index of the biggest element. But it's not very good at this if the gap between them is very, very tight. Which it is for cosine. But if you take the average of a bunch of waves of different frequencies, they constructively interfere at zero - but they're all one - but they destructively interfere everywhere else. So the gap between zero and everything else gets quite bigger.

**Daniel Filan:**
I think a way I would think about this is if you rotate around at different frequencies, the things that are in second place among these axes, they change because... When you multiply by a different frequency, you're sort of taking the circle, stretching it out and wrapping it around itself a few different times kind of.

**Neel Nanda:**
I like it.

**Daniel Filan:**
I don't know how clear this is via audio, but you're basically changing which things are second place. So this... So you can't have a thing that's in close second place.

**Neel Nanda:**
Lawrence made a great diagram at the start of the paper. People should go look at it.

**Daniel Filan:**
Cool.

**Neel Nanda:**
Yeah. And one thing to highlight about this algorithm is that a and b are activations, c is a parameter and the hard part of the algorithm is multiplying together the trig term for a and the trig term for b to get the a + b rotation and everything else is really easy.

**Daniel Filan:**
So I guess one question I have... so in the story you said that there was this phase early on when somehow it was gaining the ability to do the generalizing solution even though it was also kind of memorizing.

**Neel Nanda:**
Yep.

**Daniel Filan:**
Can you remind us how you determined that, if it wasn't by looking at all the weights and analyzing them at every single checkpoint?

**Neel Nanda:**
Yeah. Okay. So to answer this, we need to dig a bit more into the details of how the algorithm is implemented.

So the structure of a transformer is roughly: there's the embedding matrix, which is a lookup table that maps the vocabulary of input tokens to vectors in the residual stream. There's the attention layer which moves information between tokens. Here we just got three tokens, a, b, =, and attention is just moving things from a and b to the = and doing a bit of computation, which isn't that important, but is accounted for in one of the appendices. And then the MLPs happen, which do a bunch of processing on the combined bits from the two input tokens. And finally there's the output weights of the neurons and then the un-embedding, which is a linear map from the neurons to the logits. And there's some other architectural details of the transformer, but which don't really matter for our purposes.

And so the algorithm has three steps. One of them is mapping a and b to sin(wa) and cos(wb). w being 2 pi K / N, the frequency. And people hear this and they think this is really hard. This is actually really easy because you don't need to learn the general function sine. You see to learn sine on like 113 memorized values. Note that I studied mod 113, though the same algorithm seems to transfer to everything else we've checked. And so the embedding-

**Daniel Filan:**
It's 113 because you want the modulus to be prime for various reasons.

**Neel Nanda:**
It does make things a bit cleaner. I don't think it's actually necessary.

**Daniel Filan:**
Okay.

**Neel Nanda:**
All right. The algorithm, the exact same algorithm just works.

**Daniel Filan:**
Yeah, yeah. But you're learning the sine function on 113 data points.

**Neel Nanda:**
And it's just equally easy to memorize any function, because it's a lookup table, and this is done by the embedding. And you can just check, has the embedding done this? There's a cute graph where you can just feed in every possible sine and cos function into the embedding. And it turns out there's just like 10 where it's big, which are the five frequencies we care about, and everything else is basically zero.

And then the neurons multiply things together. There's a neuron cluster for each frequency w and each neuron in that cluster, the output is some linear combination of a constant; terms like sin(wa) or cos(wb), like trig terms, which are just in one variable and constant across the other; and then trig terms that are things like sin(wa) times cos(wb), so product trig terms.

And so there are these nine possible terms and every neuron in the cluster can be well approximated as a linear combination of these. If you delete everything that's not covered by these, the model does about as well. And interestingly, this means that each neuron cluster is rank nine, even though there's often a hundred or more neurons in a cluster, which is kind of wild. Note that I'm referring to directions over the batch dimension, like the dimension of all inputs a and b.

**Daniel Filan:**
Sorry, what do you mean when you say you're referring to that dimension?

**Neel Nanda:**
Sorry, when I say you can approximate a neuron with, say, sin(wa) cos(wb), that is a direction in the space of all pairs, a and b.

**Daniel Filan:**
Right. So you're saying that on each a and b, the pattern of firing of this neuron is proportional to sin(wa) cos(wb).

**Neel Nanda:**
Yes.

**Daniel Filan:**
Okay.

**Neel Nanda:**
And then it further turns out that the only directions that actually matter for the output are the sin(w(a+b)) and cos(w(a+b)) directions in neuron space, which are, sin(w(a+b) is sin(wa) cos(wb) plus cos(wa) sin(wb), [those trig identities](https://en.wikipedia.org/wiki/List_of_trigonometric_identities#Angle_sum_and_difference_identities)... So just two of the nine directions that the neurons represent are the ones that actually matter, and the neuron logit weights pick up on those two for each cluster and map those to the cos(w(a+b-c)) outputs.

**Daniel Filan:**
Okay.

**Neel Nanda:**
And so there's a lot of claims this makes.

**Daniel Filan:**
Yeah.

**Neel Nanda:**
And the particular crux we use to distinguish between the generalizing and memorizing solution is the fact that we can make this claim that across the 12,000-odd input pairs, there are exactly 10 directions in the input space that matter for producing the neurons. sin(w(a+b), cos(w(a+b)) for the five frequencies.

**Daniel Filan:**
Yep.

**Neel Nanda:**
If you delete these directions, this will completely kill the generalizing algorithm. But it should basically not affect the memorizing algorithm because the memorizing algorithm intuitively should be kind of diffuse across the frequencies, across all directions. This statement is not entirely obvious because maybe the memorization is also using those directions for some reason, but in practice, probably fine.

And if you delete everything that's not those directions, it will probably completely kill the memorization algorithm. But the generalization algorithm should be totally fine. And so we designed these two metrics, restricted loss, which is where we evaluate the test loss after getting rid of everything but these 10 directions in neuron space. And also, I think, snipping the residual connection around the neurons so the output is literally just a combination of these 10 dimensions and then a bias term, which is just the average of everything.

And when we look at restricted loss, what we see is that this goes down significantly before test loss crashes. And this is kind of going down throughout the whole circuit formation period. It also goes down a bunch more at cleanup, which suggests that the 'circuit formation' vs 'cleanup' distinction is not perfectly pure.

But the interpretation in my eyes is that we are cleaning up the memorization noise for the model, and turns out when we do this that the model is just better, the model can generalize much faster and much more clean. And-

**Daniel Filan:**
So essentially these phases are phases of dependence on the directions in input token space that this trigonometric algorithm depends on.

**Neel Nanda:**
Yes. I will note that an important subtlety about everything I'm describing is I am not ablating inputs. I am saying 'let's feed the inputs into the model. Let's produce the neuron activations, which are a 512-dimensional vector for every pair of inputs. Let's think of this as an enormous 113 squared by 512 tensor.'

I no longer care about how these were computed, but now I can start removing directions in 113 squared dimensional space. But I'm kind of running all inputs through the model and then doing fiddling. I'm not doing fiddling then running things through the model.

**Daniel Filan:**
Okay.

**Neel Nanda:**
Which is a kind of weird thing to do and reasonable to take issue with.

**Daniel Filan:**
Sure. Okay. So at this point, now that we understand the paper a bit better-

**Neel Nanda:**
There's also a second progress measure.

**Daniel Filan:**
Oh sorry, yes.

**Neel Nanda:**
We called excluded loss, which is where you delete the 10 key directions on the training data and then you look at how much this harms train performance. And what we find there is that during memorization it mostly tracks train loss, but then over the course of circuit formation it diverges and gets worse and worse. And this tracks the claim that it's transitioning from memorization to generalization.

## The suddenness of grokking <a name="sudden-grokking"></a>

**Daniel Filan:**
Okay. So yeah, on a big picture I think of the story of this paper as, 'Hey, see this sudden phenomenon that you thought was sudden, there's actually this relatively gradual underlying thing in the network that drives this sudden transition. So I guess my first question is: how is it the case that this gradual development and this gradual dying down of the memorization results in... at the end of it, eventually suddenly the test loss goes down? I don't know, do you have some story for where that kind of discontinuity is coming from?

**Neel Nanda:**
Sorry, I didn't quite follow the question.

**Daniel Filan:**
So if the underlying driver of successful generalization is basically these circuits and basically we're saying that these circuits are gradually appearing and then the memorization parts of the network are gradually dying away, if the underlying factors are gradual, why is there a sudden decrease in test loss at some point?

**Neel Nanda:**
Sure. So first off, this is not a thing I consider us to have rigorously answered in the paper. The following is a bunch of my guesses, which I think are probably correct, but definitely nowhere near the point where I'm like, you should believe this because my paper is so cool.

So I guess there's several things going on. So the first is just this whole cleanup versus generalization thing where it just seems kind of plausible to me that (a) there's some kind of threshold where there's kind of feedback loops where once the memorization stuff starts to no longer be necessary, then it gets suppressed rapidly and this uncovers the existing circuit. And it just seems completely not crazy to me that these are things that just happen on different time scales. So it looks kind of sudden.

**Daniel Filan:**
So, sorry. The suddenness is just that one of the things happens quicker than the other or...?

**Neel Nanda:**
Yeah, pretty much. Just like, once it reaches the point where the memorization's usefulness is just outweighed by the weight decay, then it just cleans it up. And maybe this happens rapidly or there's kind of a snowballing effect where... there's the force to keep it, there's the force to get rid of it. The force to keep it is initially really, really strong then gets weaker and weaker until it slightly crosses the boundary. And now it's starting to clean it up and then that accelerates because the more it's removed, the weaker the force to keep it, or something.

So hypothesis two, there's just something weird about the optimizer we're using that changes our results a bunch because optimizers are really, really weird. Specifically we're using [AdamW](https://arxiv.org/abs/1711.05101), which is the variant of [Adam](https://arxiv.org/abs/1412.6980) that uses weight decay in a principled way. And the details of how it works were annoying, but very roughly Adam has momentum. So it tracks all of the recent gradients and points towards an average of those. It also normalizes, which means it tracks the noise of the gradients, the average squared gradient and scales down by that.

So if the gradient's been really noisy recently, it lowers things, but then it does weight decay without momentum, meaning that it's just kind of reducing the weights at each time step independent of what the weights were at previous time steps.

And in a way that's just kind of out of sync with everything else Adam is doing. And this is a really weird optimizer. It's also basically the state of the art optimizer that everyone uses and there's a lot of weird things about grokking that make it a better optimizer than others. So it's plausible that this matters.

**Daniel Filan:**
Yeah. There was this [comment](https://www.lesswrong.com/posts/N6WM6hs7RQMKDhYjB/a-mechanistic-interpretability-analysis-of-grokking?commentId=dX865Yk9QyZWGdvpe) on a LessWrong post that accompanied this paper that basically had some toy model for why you should expect this to happen with Adam-based optimizers, but you shouldn't expect it to happen with just vanilla stochastic gradient descent.

**Neel Nanda:**
Yes.

**Daniel Filan:**
I'm wondering what you made of that.

**Neel Nanda:**
So I believe it does happen with SGD, but this is an experiment that one of my collaborators ran that I have not run, so I can't claim with confidence and I think they were having issues.

My recollection of that comment is the stuff that it was saying was pretty reliant on the basis... of the fact that the relevant numbers that were moving around were in the basis in which we're applying Adam. It wasn't that there was a meaningful direction in the space that was spread across many coordinates. It was aligned with a specific basis element. And that is not true in the modular addition case. But I also haven't read that comment in a while and I could just be completely wrong.

The third reason for what I think is going on is much more closely linked to phase transitions. So I have this hypothesis that basically all circuits that form in a model should form as phase transitions. And my intuition behind this is that-

**Daniel Filan:**
Sorry, before you say your intuition, what do you mean by 'all circuits form as phase transitions'?

**Neel Nanda:**
Sorry, yeah. What I mean is that if there is some circuit in a model that does some task, this hypothesis predicts that if you look at whether that circuit exists or the components of that circuit, that these form in a sudden shift where you go from 'does not have' to 'has' pretty rapidly, rather than as some weird ad hoc thing, rather than a smooth gradual thing.

I don't particularly claim this is universal, and I think exactly what you mean by a circuit is going to vary. But where the induction heads are a canonical example of 'this happens and it's really weird, and unexpected'.

So going back, the thing that I think is interesting is that: so if you think about an induction head, it's kind of wild that this happens at all because you have these two bits of the model. You've got the previous token head and the induction head, and each of these kind of only really makes sense in the context of the other one being there. Induction heads in natural language... And this is slightly untrue because looking at the previous token can be helpful. But you also get induction heads if you just train on a synthetic task with random tokens with some random repeated substrings. And in this task, knowing the previous token is literally independent of the current token.

**Daniel Filan:**
Yeah.

**Neel Nanda:**
There's no incentive. Yet these form induction heads with a sharp phase change. And there's this kind of interesting thing where each part is only useful when the other parts are there, why does it form at all?

And my hypothesis is there's some kind of lottery ticket style thing going on here where there's some directions in weight space that kind of contain the previous token bit and some directions in weight space that kind of contain the induction-y bit. And these reinforce each other because if each of them is stronger, then the other is more effective at reducing the loss. And then there's lots and lots of other directions of weight space that are doing stuff that's either actively harmful or irrelevant and just not reinforced.

And this hypothesis predicts that there is going to be a gradient signal that slowly reinforces the previous token bit and the induction bit, but that it's kind of spread out and initially very slow, but the further the progress gets, the stronger the gradient signal on either is with the... If you just kind of jump through to the end, if you just imagine that you insert a fully fledged induction head in the model at the start, it should be really easy to form the previous token head and vice versa.

**Daniel Filan:**
Yeah, yeah.

**Neel Nanda:**
[Adam Jermyn](https://adamjermyn.com/) and Buck Shlegeris have [a good post](https://www.alignmentforum.org/posts/RKDQCB6smLWgs2Mhr/multi-component-learning-and-s-curves) on S-shaped curves and phase transitions where they build some toy models of this.

**Daniel Filan:**
That might be worth checking out. I've always been a bit skeptical of this understanding of the lottery ticket hypothesis just because it feels like... So for one, it actually is a bit more detailed than what they actually claim in this lottery ticket hypothesis paper.

**Neel Nanda:**
Oh yeah.

**Daniel Filan:**
If you read [the paper](https://arxiv.org/abs/1803.03635), it's a much weaker claim than this. Or the experiments show something much weaker than this.

**Neel Nanda:**
Oh, what exactly do they claim in the paper?

**Daniel Filan:**
So in the paper the thing they show is that, okay... This gets modified a little bit and there are [future](https://proceedings.mlr.press/v119/frankle20a) [papers](https://arxiv.org/abs/2002.10365) which elaborate on this, but the thing where they start is like, 'Yeah, here's a thing you can do. You can randomly initialize a neural network, then you train it. Then once you've trained it, you look at the weights that had the highest magnitude'. And I should say this is off memory, I could be slightly wrong here.

What you do is you say, 'Okay, I think these weights, they form a subnetwork of the whole neural network. I think this is the subnetwork that matters'. And so you select this subnetwork, but you basically mask out all the other weights at initialization. So you say, 'Yeah, the places in the neural network that ended up being important at the end, I'm going to only include those at initialization and mask out everything else, keeping fixed the random initialization'. And then they show that that subnetwork could be trained to completion faster than the original network did.

Well, it doesn't show that, for instance that... I think something that a lot of people get from this is like, 'Oh, all training was really doing was locating this subnetwork'. Or 'the subnetwork that trained is somehow the same as the thing that the network actually gets'. And none of those are shown in the thing. And I think a lot of them are... the rest of the network is totally going to influence the gradients. And so the idea that, 'Oh, it's just finding this subnetwork at initialization... this thing that was already there at initialization', it's like, well... I'm pretty sure that the weights there evolve over time and I'm pretty sure that the rest of the weights are going to matter and change the gradient.

So I don't know, basically this explanation, I think maybe what's happened is I've just generalized... I've just developed this general reaction to people claiming things about lottery tickets where I'm skeptical. But-

**Neel Nanda:**
Yes, I probably should clarify that when I wrote the initial version of my grokking paper, I had not actually read [the lottery ticket hypothesis](https://arxiv.org/abs/1803.03635) properly and just had a big vibe and was like, this is kind of lottery ticket-like.

**Daniel Filan:**
Yeah, yeah.

**Neel Nanda:**
And one important disanalogy I found when I actually read the paper was that they specifically focus on the privileged basis of weights, where you've got neurons on the input, neurons on the output, and each weight is between a pair of neurons that's kind of meaningful. This just does not work in a transformer.

**Daniel Filan:**
Yep. You're saying, 'Look, the weights just have some inner product with this algorithm and the closer the weights get to inner producting with this algorithm, that sort of feeds on itself because this bit reinforces this bit and this bit reinforces this bit?

Maybe that is more plausible.

**Neel Nanda:**
There's some cute circumstantial evidence for this. Let's say at the end of training, there's a direction that corresponds to sin(15a) in the embedding. If you look at the direction corresponding... if you look at that direction at the start of training, on the randomly initialized embedding, and then you look at which trig components it most represents, there's like a spike on sin(15a). So this direction definitely had something to do with sin(15a) at the start of training.

**Daniel Filan:**
So it just randomly happened to be that it was closest to sin(15a)?

**Neel Nanda:**
Yeah.

**Daniel Filan:**
Okay.

**Neel Nanda:**
I can't actually remember whether I checked... I can't remember which way round I did it, it could have been what does sin(15a) point to at the start, how does that compare to what it points to at the end, or what does it point to at the end, how much does that point to sin(15a) at the start? But there's more alignment than I would've expected by chance. And [Eric Michaud](https://ericjmichaud.com/), who's an MIT PhD student who did [some work of his own](https://arxiv.org/abs/2210.01117) on grokking, has [a great animation](https://twitter.com/ericjmichaud_/status/1559305105521926144) where he takes the principal components of the embedding at the end of training and then visualizes the embedding on those principal components throughout all of training. And it kind of looks circle-y at the start.

**Daniel Filan:**
Yeah, I did see that animation.

**Neel Nanda:**
Anyway. Another reason why this phase transition stuff might be going on, is this idea of symmetries. So let's imagine that you are trying to optimize the function X squared to be as close to 1 as possible.

If you start at a half, it's pretty easy. You just go to 1, pretty rapidly. But if you start at 0.0001, it's really hard. And intuitively, the reason it's hard is that you are so close to the two equally good minima, -1 and 1 that the gradient signal pushing you towards the one you are closer to is really, really weak. And this is kind of an issue. And this means that you initially are very, very slowly making progress. And then the further you get towards the 1, rather than the -1, the faster you make progress. And it seems kind of plausible to me that when there's multiple different solutions that are kind of symmetric with each other, the model... there's like some direction in model space where it's stuck between two minima and it's really slow at first. Credit to Buck Shlegeris and some MLAB participants for this hypothesis by the way.

**Daniel Filan:**
It's also reminiscent of this follow-up paper to the lottery ticket hypothesis, I think it's called, ['Linear Mode Connectivity and the Lottery Ticket Hypothesis'](https://proceedings.mlr.press/v119/frankle20a), where they basically say that in general, what actually happens, instead of this thing where you can just take the subset of weights of the model at initialization, in general, what happens is you have to train for a bit and then it'll converge. You can just pick the subset of weights at initialization. And their explanation is: initially, there's some noise in stochastic gradient descent on 'which examples do you happen to see first?' or something. But once you're robust to the noise, then you can do the lottery ticket trick. And it seems very reminiscent of this where there's a little while where you're like, "Oh, what side of zero am I on?" And then you eventually get to the phase where the signal is strong.

**Neel Nanda:**
Makes sense. Interesting.

**Daniel Filan:**
Cool. So one way you can read this paper is like okay, is there some, suppose you're worried about your model rapidly developing some capability or rapidly... I guess ['sharp left turn'](https://www.lesswrong.com/posts/GNhMPAWcfBCASy8e6/a-central-ai-alignment-problem-capabilities-generalization) is the new hot phrase for this. Rapidly getting some...thing?

**Neel Nanda:**
Yes. In fairness to Nate [Soares], 'sharp left turn' is a specific phrase for a really specific emergent phenomenon where it develops intelligence or something.

**Daniel Filan:**
But I think it's an example of this. Anyway, I see your paper... on the one hand you could read it as saying like, "Oh, we can understand the underlying mechanisms and there's some roughly gradual thing." But if it's the case that this restricted loss keeps on going down gradually or these circuits, at some point they appear, but maybe you don't know which circuits you should be looking for, maybe some circuits are only going to appear after other circuits, in general... I'm wondering: how do you think this bears on this question of can we foresee rapid changes, in advance, before they happen?

**Neel Nanda:**
Yeah, so I think this is a pretty valid criticism.

**Daniel Filan:**
It's not obviously a criticism, I think it's just a question, but you can take it as a criticism if you want.

**Neel Nanda:**
So to me the criticism here is, "Ah, you're saying you built these progress measures, but what's the threshold at which it matters? And if you don't have a threshold, what's the good of a progress measure?"

**Daniel Filan:**
Yeah, that's kind of a criticism.

**Neel Nanda:**
I mean, I think it is just a correct criticism. One of the annoying things about trying to distill the paper and submit it, is this meant we needed to think hard about crafting a narrative. And my opinion is, this is just less of a compelling narrative than other ones which fits academic shibboleths better. Anyway. So I basically think this is just correct. I think the progress measures are much more exciting when you (a) have some kind of threshold at which interesting stuff happens, and also (b) you can extrapolate outwards how long it will take until you get there. I think weak versions of this is interesting and having a progress measure seems a lot better than having nothing and being totally blindsided, but it's definitely a lot less satisfying. Other major weaknesses are just that we needed to understand the final circuit before we could build these progress measures.

And if you need to reverse engineer the emergent model, [that's] not great, though I am pretty excited about things like 'fine tune a smaller model to do this and reverse engineer that', or 'train a toy task that's a synthetic replication of this and reverse engineer a model trained to do that simple one'. One thing I'm really interested in, is how much addition in large language models uses some version of my grokking algorithm, my modular addition algorithm? Because I found some suggestive evidence-

**Daniel Filan:**
Normal addition?

**Neel Nanda:**
So if you look at, say, base 10 addition in a toy one layer transformer, I found some suggestive evidence that it was sparse in the Fourier basis. And it seems non-crazy to me that the right way to do it is kind of thinking about things as addition mod 10, but maybe you have a carry digit shoved in the mix.

And in normal language models, tokenizers make it so it's approximately addition mod 100, which makes it even more relevant because 10's kind of easy, although tokenization sucks and means that the number of digits per token is not constant and it's horrifying. Anyway, I'm very interested in whether this algorithm is relevant there. Because that would feel strong evidence for my 'toy models good' hypothesis. But anyway, I think that the fact that we needed to reverse engineer it and that we don't have a clear notion of criticality is definitely a weakness. I think that it's a pretty good proof of concept and I think having any notion of a metric towards this is just pretty great. I think there's a ton of work if you want this to be something that can be really made precise or be used for real forecasting.

It also wouldn't surprise me if you could refine these metrics into something which actually has significant predictive power. I did some playing around where I did some dumb things like take the logits as an enormous flattened vector, stacked this across all checkpoints and then did PCA on that. And then I took the first two principal components and tried to extrapolate from where I was, to how long would it take to grok and it was not terrible. If you were at epoch 8,000, it predicted you'd get there at 11,000 and got there at 10,500 and this method was totally an incredibly hacky mess and I might be misremembering it. Also, it only worked when you have the principal components taken over all of training and if you do principal components only up to that point it doesn't work. And I don't know why, but I'm like-

**Daniel Filan:**
Yeah, that's not great.

**Neel Nanda:**
It wouldn't surprise me if this can be turned into a thing with criticality.

## Relation to other research <a name="relation-to-other-research"></a>

**Daniel Filan:**
Now we've talked about these papers, is there any future follow-up on these that you're particularly excited by?

**Neel Nanda:**
So my research interests have mostly moved on from grokking and looking at these super toy models that are just so far removed from language models, which is where my main research interests lie. Though I am currently mentoring a project where we found that you can generalize the algorithm to arbitrary group composition, which is pretty exciting and we're currently trying to write that one up. So that's a cool direction.

Generally, I feel like the biggest thing missing in just our whole understanding of transformers is: what is up with the MLP layers? How do we engage with them? What is superposition? What do they even mean? How well do our conceptual frameworks work with them? One direction I'm personally very excited about is reverse engineering neurons in just a one layer transformer with MLP layers, because we're just not very good at this. And I feel like if we were a lot better, that would probably teach us something. And that's the big category of things that I'm interested in. The other big category is just: I want at least 20 examples of well-understood circuits in real language models. We have three. [Redwood](https://www.redwoodresearch.org/) is currently running this [REMIX sprint](https://web.archive.org/web/20230201063146/https://www.redwoodresearch.org/remix) where they're trying to get a bunch more and this seems awesome, but I would love as many circuits as possible, ideally studied to the same level of detail and universality as induction heads.

So the reason this feels important to me is: to me, a lot of the vision of mechanistic interpretability is, if we had some grounded understanding, a lot of things will become less confusing. And if we're trying to do things like say, "What are the principles underlying networks, what are the techniques that actually scale or work or are reliable?" Just having a test set of 20 circuits to try it on would just feel like such a massive upgrade over the current state of the art. So that's a category of stuff that I'm pretty pumped about. I think the direction following [my grokking work](https://arxiv.org/abs/2301.05217) that feels most interesting to me would either be work that's building further on phase transitions in real models, somewhere at the intersection of this and [the induction heads work](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html). And looking for more of them, trying to test these ideas that phase transitions are just a core part of life, in real models.

And the other would be... so my personal vision for the philosophy outlined in my grokking work, is mechanistic interpretability as a tool of attack on science of deep learning mysteries. Where, I think that if we're making... the claim that we can actually reverse engineer networks is this very bold and ambitious claim. If it is true and we're making real insights, we should be in a much better position to understand deep learning mysteries than people who are forced to mostly view them as black boxes. And to me the kind of philosophy underlying my grokking work, was I was confused, I built an example model organism and then reverse engineered it exhaustively, then I distilled this into an actual understanding and used this to figure out what was going on.

And to me this just feels like a pretty general model that can be applied to many other questions that confuse us in deep learning and Anthropic's ['Toy Models of Superposition'](https://transformer-circuits.pub/2022/toy_model/index.html) feels like a good example of a similar philosophy applied to superposition, this weird phenomenon in transformer MLPs, and I just feel like there should be a lot of traction there. I have written in a sequence called, ['200 Concrete Open Problems in Mechanistic Interpretability'](https://www.alignmentforum.org/s/yivyHaCAmMJ3CqSyj), that you can find on the Alignment Forum. That is 200 open problems I think I would like to see people work on.

**Daniel Filan:**
Sure.

**Neel Nanda:**
For many more Neel takes on things that should happen.

**Daniel Filan:**
All right, so that's how we should think of the relationship of this to other questions in mechanistic interpretability. One thing I wonder is: so there are various subfields of AI alignment, but I think they're not totally isolated. I think there are relations and potential complementarities between them. Do you see any complementarities of either this work in particular or mechanistic interpretability in general with other things people are pursuing in AI alignment?

**Neel Nanda:**
Yeah, so I think there's quite a few complementarities, though not obviously dramatically more than there is between mechanistic interpretability and any other field of AI. Since again, if we're really understanding networks that should just be relevant everywhere. So some things that feel particularly salient: fundamentally what we're trying to do in alignment is making claims about model internals and using fuzzy, what [Richard Ngo](https://www.richardcngo.com/) calls pre-formal ideas around how to reason about networks, like 'goals' and 'agency' and 'planning', and the idea that a goal can be internally represented and the idea that a model can be aware of itself and have situational awareness. And it's just not even obvious to me that these are coherent concepts to talk about in models. And it's not obvious to me how these map onto actual model cognition. And it's really easy to anthropomorphize and I feel like if we just had, in particular, good examples of reverse engineered agents on reinforcement learning problems... the thing I'd really love is reverse engineering work on an RLHF model (reinforcement learning from human feedback).

It feels like if we had examples we understood there, that would just teach me a lot and really help ground a lot of the field's ideas. Another idea would be going the opposite direction. So there's all of these ideas around, say, capability robustness but objective mis-robustness, that give interesting case studies of agents that do things we think are models for potential alignment failures. And reverse engineering models is hard, there's only so much researcher time and effort and being able to focus on the problems that are particularly interesting to alignment, and see if we can shed insight onto what is actually going on there, how much is this a real thing versus a kind of correlational fuzziness kind of thing. Unclear.

A third thing would be trying to give better feedback loops to alignment research. If you are trying to find an alignment approach that actually results in systems that do what we want, it can be hard to judge this by just looking at the system's actions. You can plausibly do this with some techniques, like red teaming or adversarial training and stuff like that. But having the ability to look into the system and be like, "Is this thinking deceptive thoughts?" just seems like it should be a massive win.

And I think the field is moderately far from the point where we can do that, but this just seems like something that should make alignment research much easier. Though I have not actually spoken enough with alignment people about how concretely relevant this is their work for me to be highly confident in this.

Then I think there's various varieties of more ambitious ideas about integrating interpretability into actual alignment schemes or training schemes. If you have human feedback givers who have interpretability tools to notice weird things the model's doing, then maybe this is helpful. Obviously this is an extremely scary and suspicious thing to do because if you start training on a tool, it's easy to break the tool and we're nowhere near the point where I think we've got interpretability tools that are actually robust enough that I'm comfortable applying any amount of gradient descent against them.

But that's like an ambitious complementarity. Though one thing I will emphasize, kind of countervailing that, is for people who are listening to this and being like, "Should I go work in mechanistic interpretability or something?" Is just that, in my opinion, mechanistic interpretability just has a really unique vibe as a field that is nothing like what I experienced in other bits of ML. That's very much like, partially I'm a mathematician dealing with this formal mathematical object. I'm partially a natural scientist dealing with this confusing complex organism and running experiments and trying to form true beliefs. But also I'm writing codes, so my experiments run really fast and I understand them well. And partially it's writing a bunch of code and probing this object and this is a really fun and fascinating workflow. And my prediction is there's a bunch of people who are bad fits for this, a bunch of people who are good fits for this who'd be bad fits for other areas in alignment, and it's worth checking.

## Could mechanistic interpretability possibly work? <a name="could-mech-interp-work"></a>

**Daniel Filan:**
Cool. So before I end the interview, is there anything that you wish that I'd asked but I haven't yet?

**Neel Nanda:**
This is a great question. I did take opportunities to shoe-horn some of those in earlier, like the science of deep learning narrative of my grokking. I feel like there's a big theme we never really got into of just, "Is any of this remotely a reasonable thing to be working on? Isn't this just ludicrously ambitious, never going to work? Or aren't networks just fundamentally not interpretable?" Which is a pretty common and strong line of criticism.

**Daniel Filan:**
Yeah, I guess this is a thing that people other than me think, so I forgot that was a thing. So yeah, aren't networks just fundamentally inscrutable? How dare you challenge the gods?

**Neel Nanda:**
Yeah, nah, seems completely doable, is my brief answer. So I think a pretty legitimate criticism that I often hear leveled against the field is that there's a lot of cherry-picking going on, where induction heads are a really nice algorithmic task that occurred in tiny models. Indirect object identification is a more complex task, but it's still pretty nice and algorithmic. [My grokking work](https://arxiv.org/abs/2301.05217) is obviously incredibly toy and neat and algorithmic. Small models are plausibly easier than big models except for the scale, et cetera. And I think it is totally consistent with all available evidence that there's some threshold of difficulty. We're going to be good at things up to there and then completely fail beyond this point. And this is just an extremely hard claim to falsify for obvious reasons.

I do think that... it's like my internal experience of getting into this field was something like 'Seems like neural networks are just kind of an inscrutable mess of linear algebra and I don't see why they should be interpretable', to seeing some of Chris Olah and collaborators work at OpenAI on [image circuits](https://distill.pub/2020/circuits/zoom-in/) and I was like, "Wait, what? You can interpret neurons?"

And then they found some circuits and I was like, "What? This is weird." And then they found that [you could hand code some curve-detecting neurons and sub them in and it kind of worked](https://distill.pub/2020/circuits/curve-detectors/). And I was like, "What?" But then I was like, "Ah, it's never going to scale. It's never going to transfer to language. It's going to be really specific to each model." And then it turns out transformers are pretty doable and in some ways much easier, in other ways, much harder. Turns out that [induction heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html) are an interesting non-trivial circuit that occur in all model scales. And I feel like I just keep being surprised in a positive direction, and by induction... But yeah, I think this is just kind of an open scientific question that we just don't have enough data to bear on either way. And I'm pro there being bets made that are, "Let's assume mech. interp. is completely doomed, what would we do then?"

And that there are bets people make that are like, "Mech. interp. is the wrong way to do interpretability, but interpretability is really important." It's like, "Let's go do that. Let's go do these other things." And I think mech. interp. is great and really promising and a really good bet to make, but I'm biased. And even I accept there's a decent chance I'm wrong and it's just completely doomed.

And then the final problem is just the problem of scale, of even if we're really good at reverse engineering models, what do we do if... we can painstakingly reverse engineer each neuron in GTP-3 in a day each and then there's millions of neurons and I low key hold the position of this seems like a really nice problem to have. We'll solve it when we get there, but we're not yet at that point. More seriously, I do think that just having any solid baseline of even knowing in principle how we might reverse engineer GPT-3 given a ton of time and effort seems like an enormous win. I also think that in a world where we're near having genuinely dangerous, transformative AI, that is a world where I expect us to be a lot better at automating things. And also where the mechanistic interpretability researchers are hopefully very close to the people making the big scary AGIs and can figure out how to better leverage them.

And finally, it's plausible to me that the main thing we need to get done is noticing specific circuits to do with deception and specific dangerous capabilities like that and situational awareness and internally-represented goals. And that getting good at that is just a much easier problem. But I don't know, I think this is just actually valid criticisms. We don't yet have enough data to answer them. It's completely plausible that the field is just doomed. And I think this is true of all research agendas, but it is worth knowing.

## Following Neel's research <a name="following-neels-work"></a>

**Daniel Filan:**
Seems fair. So speaking of it being worth knowing, if people want to know more about your research and the work you do, how can they follow you?

**Neel Nanda:**
Yeah, so [following me on Twitter](https://twitter.com/NeelNanda5/) is probably the best way to get all of my random musings.

**Daniel Filan:**
Okay. What's your handle?

**Neel Nanda:**
I think it's [NeelNanda5](https://twitter.com/NeelNanda5/). I spend far too much time on Twitter. I also generally will post things on [the Alignment Forum] and on [the mechanistic interpretability section of my blog](https://www.neelnanda.io/mechanistic-interpretability). And I should really set up a mailing list for that at some point. I haven't got around to it yet. So you should check out my work. Particularly notable things that people might be interested in... I made this library called [TransformerLens](https://github.com/neelnanda-io/TransformerLens) for just having good open source infrastructure for doing mechanistic interpretability of language models like GPT-2, which is designed to make research fun and not a massive headache. I have this post called, ['Concrete Steps to Get Started in Mechanistic Interpretability'](https://www.alignmentforum.org/posts/9ezkEb9oGvEi6WoB3/concrete-steps-to-get-started-in-transformer-mechanistic). That's my attempt to create a guide that can take someone who is excited but doesn't know anything about the field, to the point where you're actually trying to do original research.

I have [a YouTube channel](https://www.youtube.com/@neelnanda2469) with a bunch of paper walkthroughs and other interpretability tutorials. And if you enjoyed hearing me ramble about a bunch of papers here, you might enjoy a bunch of my paper walkthroughs. And I have a sequence called ['200 Concrete Open Problems in Mechanistic Interpretability'](https://www.alignmentforum.org/s/yivyHaCAmMJ3CqSyj) where I try to lay out a map of the field and what I think are the big areas of open problems with a bunch of takes on why they matter, followed by a bunch of the open problems and my takes on the right way to orient to them as a researcher and pitfalls and how to do good work.

And finally, I have [a comprehensive mechanistic interpretability explainer](https://dynalist.io/d/n2ZWtnoYHrU1s4vnFSAQ519J) where I try to just give a lot of concepts and ideas and terms that I think people should understand if they're trying to engage with the field, which you can either just read through the whole thing and try to get a concentrated dump of a lot of my intuitions and ideas or use as a reference and which includes sections on most of the papers discussed here. Yeah, it was really fun. Thanks for having me on.

**Daniel Filan:**
Yeah, links to all of those will be in the description. Thanks for talking and to the listener, I hope this was a valuable episode for you.

This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Financial support for this episode was provided by the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode or to learn how to support the podcast yourself, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
