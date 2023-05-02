---
layout: post
title: "21 - Interpretability for Engineers with Stephen Casper"
date: 2023-05-01 17:30 -0700
categories: episode
---

<!-- [Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/NTM1YTQ1MzAtMDVlNS00OTE4LThlMjgtOWRmZWUzMjM1Mjk0) -->

Lots of people in the field of machine learning study 'interpretability', developing tools that they say give us useful information about neural networks. But how do we know if meaningful progress is actually being made? What should we want out of these tools? In this episode, I speak to Stephen Casper about these questions, as well as about a benchmark he's co-developed to evaluate whether interpretability tools can find 'Trojan horses' hidden inside neural nets.

Topics we discuss:
- [Interpretability for engineers](#interp-for-engineers)
  - [Why interpretability?](#why-interp)
  - [Adversaries and interpretability](#adversaries-and-interp)
  - [Scaling interpretability](#scaling)
  - [Critiques of the AI safety interpretability community](#taisic-critiques)
  - [Deceptive alignment and interpretability](#deceptive-alignment-interp)
- [Benchmarking Interpretability Tools (for Deep Neural Networks) (Using Trojan Discovery)](#benchmarking-paper)
  - [Why Trojans?](#why-trojans)
  - [Which interpretability tools?](#which-interp-tools)
  - [Trojan generation](#trojan-generation)
  - [Evaluation](#evaluation)
- [Interpretability for shaping policy](#interp-for-policy)
- [Following Casper's work](#following-caspers-work)

**Daniel Filan:**
Hello, everybody. In this episode I'll be speaking with Stephen Casper. Stephen was previously an intern working with me at UC Berkeley, but he's now a Ph.D student at MIT working with Dylan Hadfield-Menell on adversaries and interpretability in machine learning. We'll be talking about his '[Engineer's Interpretability Sequence](https://www.alignmentforum.org/s/a6ne2ve5uturEEQK7)' of blog posts, well as [his paper](https://arxiv.org/abs/2302.10894) on benchmarking whether interpretability tools can find Trojan horses inside neural networks. For links to what we're discussing, you can check the description of this episode and you can review the transcript at axrp.net. All right. Welcome to the podcast.

**Stephen Casper:**
Thanks. Good to be here.

## Interpretability for engineers <a name="interp-for-engineers"></a>

### Why interpretability? <a name="why-interp"></a>

**Daniel Filan:**
So from your published work, it seems like you're really interested in neural network interpretability. Why is that?

**Stephen Casper:**
One part of the answer is kind of boring and unremarkable [which] is that lots of people are interested in interpretability and I have some past experience doing this, and as a result it's become very natural and easy for me to continue to work on what I've gathered interests in and what I have some experience in. You definitely know that from when we worked together on this, but aside from what I've just come to become interested in, interpretability is interesting for a reason to so many people. It's part of most general agendas for making very safe AI for all of the reasons people typically talk about. So I feel good to be working on something that is generally pretty well recognized as being important.

**Daniel Filan:**
Can you give us a sense of why people think that it would be important for making safe AI? And especially to the degree that you agree with those claims, I'm interested in hearing why.

**Stephen Casper:**
I think there are a few levels in which interpretability can be useful, and some of these don't even include typical AI safety motivations. For example, you could use interpretability tools to determine legal accountability and that's great, but it's probably not going to be the kind of thing that saves us all someday.

From an AI safety perspective, I think interpretability is just kind of good in general for finding bugs and guiding the fixing of these bugs. There's two sides of the coin: diagnostics and debugging. And I think interpretability has a very broad appeal for this type of use. Usually when neural systems are evaluated in machine learning, it's using some type of test set and maybe some other easy evals on top of this. This is very standard, and just because a network is able to pass a test set or do well on some sort of eval environment or set, that doesn't really mean it's doing great.

Sometimes this can actually actively reinforce lots of the biases or problems that we don't want systems to have, things like dataset biases, so at its most basic, interpretability tools give us this additional way to go in and look at systems and evaluate them and look for signs that they are or aren't doing what they want. And interpretability tools are not unique in general for this. Any other approach to working with or evaling or editing models is closely related to this, but one very, very nice, at least theoretically useful thing about interpretability tools is that they could be used for finding and characterizing potentially dangerous behaviors from models on very anomalous inputs. Think Trojans, think deceptive alignment. There might be cases in which some sort of system is misaligned, but it's almost impossible to find that through some sort of normal means, through treating the model as some type of black box. And interpretability is one of a small unique set of approaches that could be used for really characterizing those types of particularly insidious problems.

**Daniel Filan:**
So it sounds like your take about interpretability is it's about finding and fixing bugs in models. Is that basically right?

**Stephen Casper:**
I think so. And lots of other people will have contrasting motivations. Many people, more than I, will emphasize the usefulness of interpretability for just making just basic discoveries about networks, understanding them more at a fundamental level, and I'll never argue that this is not useful. I'll just say I don't emphasize it as much, but of course, engineers in the real world benefit from theoretical work or exploratory work all the time as well, even if it's indirect.

**Daniel Filan:**
Yeah, I mean, I'm wondering, why don't you emphasize it as much? It seems potentially, I think somebody might think, okay, we're dealing with AI, we have these neural networks, we're maybe going to rely on them to do really important stuff. Just developing a science of what's actually going on in them seems potentially like it could be pretty useful and potentially the kind of thing that interpretability could be good for.

**Stephen Casper:**
Yeah, I think I have three mini answers to this question. One is if we're on short timelines, if highly impactful AI systems might come very soon and we might want interpretability tools to be able to evaluate and understand as much as we can about them, then we want to have a lot of people working on engineering applications. The second mini answer I think involves just pulling in the right direction. It's not that we should have all engineering-relevant interpretability research, and it's not that we should have all basic science interpretability research. We probably want some sort of mix, some sort of compromise between these things. That seems very uncontroversial, but right now I think that the lion's share of interpretability research in the AI safety space is kind of focused on basic understanding as opposed to the engineering applications. So I think it's kind of useful to pull closer toward the middle.

I think the third reason to emphasize engineering applications is to make progress or get good progress signals for whether or not the field is moving somewhere. Because if a lot of time is spent speculating or pontificating or basically exploring what neural networks are doing, this can be very valuable, but only very indirectly, and it's not clear until you apply that knowledge whether or not it was very useful. So using things like benchmarking and real world applications, it's much easier to get signals, even if they're kind of somewhat muddled or sometimes not perfectly clear about whether progress is being made, than it is if you're just kind of exploring.

**Daniel Filan:**
Before I really delve into some things you've written, one question I have is: if I want to be fixing things with models or noticing problems or something, I think one version of that looks like I have a model that I've trained and now I'm going to do interpretability to it. But you could also imagine an approach that looks more like, 'oh, we're going to really understand the theory of deep learning and really understand that on these data sets, this learning method is going to do this kind of thing'... that ends up looking less like interpreting a thing you have, and more just understanding what kinds of things are going to happen in deep learning just in general. I'm wondering, what do you think about that alternative to interpretability work?

**Stephen Casper:**
Yeah, so it definitely seems like this could be the case. We might be able to mine really, really good insights from the work that we do and then use those insights to guide a much richer understanding of AI or deep learning that we can then use very usefully for something like AI safety or alignment applications.

I have no argument in theory for why we should never expect this, but I think empirically there are some reasons to be a little bit doubtful that we might be able to 'basic science' our way into understanding things in a very, very useful and very, very rigorous way. In general, I think the deep learning field has been shown to be one that's guided by empirical progress much more than theoretical progress. And I think more specifically the same has happened with the interpretability field. One could argue that interpretability for AI safety has been quite popular since maybe 2017, maybe a bit before, and people were saying very similar things back then and people are saying very similar things now. And I think these notions that we can make a lot of progress with the basic science were just as valid then as now, but it's notable that we haven't seen, I don't think, any particularly remarkable forms of progress on this front since then. And I get that that's a very general claim to make. So maybe we can put a pin in that and talk more about some of this later.

**Daniel Filan:**
So with this in mind, with this idea of the point of interpretability as diagnosing and fixing problems, what do you think of the state of the field of interpretability research?

**Stephen Casper:**
I think it's not actively and reliably producing tools for this right now. I think there are some, and I think there's some good proofs of concept and examples of times when you can use interpretability tools, very likely competitively to diagnose and potentially debug problems.

But this type of work, I think, seems to be the exception a bit more than the rule. And I think that's kind of okay, and to be expected in a certain sense, because the field of interpretability is still growing. It's still new, and certainly recently, and maybe even still now, there's a large extent to which it's just pre-paradigmatic. We don't fully understand exactly what we're looking for, but I think that it's probably largely the case now and it's going to continue to become more and more the case in the future, that in some sense it's kind of time to have some sort of paradigm shift toward engineering applications or to substantially increase the amount of work inside of the field that's very focused on this, because I think it's possible. And of course from an alignment perspective, it's probably needed if we're ever going to be able to use these tools to actually align something.

**Daniel Filan:**
All right. Why do you think it's possible?

**Stephen Casper:**
So right now, I think that lots of the progress that's being made from an engineer's perspective related to interpretability is kind of coming from certain sets of tools. It's coming from the ability to use generative models to find and characterize bugs. It also comes from the ability to produce interesting classes of adversarial examples - this is very related - and it also comes from the ability to automate lots of processes, which now generative models and coding models, and sometimes things just like chatbots, are able to do in a more automated way. And the tools for these things are substantially better than they were a few years ago, as is the case with most machine learning goals. And I think now is a point in time in which it's becoming much clearer to much more people that the ability to leverage some of these is pretty valuable potentially, when it comes to interpretability and other methods for evals.

**Daniel Filan:**
Okay. Do you have any specific approaches in mind?

**Stephen Casper:**
Sure. Consider this just as an example. So a few years ago we had adversarial patch work where people were attacking vision models with small adversarial patches that were just this localized region of an image. So the adversary was able to control that patch and no other part of the image. So that's the sense in which the adversary's ability to influence the system was limited. And [adversarial patches, back in circa 2017](https://arxiv.org/abs/1712.09665) looked like you would probably expect, they kind of looked like strange things with sorts of structure and patterns to them, but still lots of high frequency patterns, still things that by default were very, very difficult to try to interpret. A few years later, a [handful](https://arxiv.org/abs/1805.07894) of [works](https://openaccess.thecvf.com/content/ICCV2021/html/Hu_Naturalistic_Physical_Adversarial_Patch_for_Object_Detectors_ICCV_2021_paper.html) found that you could use generators like GANs to attack the same types of systems with things like adversarial patches, which tended to produce more and more coherent features.

And then a few years after that, right up close to the present day, the state of the art for producing adversarial features is [stuff](https://arxiv.org/abs/2303.09962) that's all via diffusion models, which are able to produce features that are much more convincingly incorporated into images and features that just look quite a bit better and are much easier to interpret, because diffusion models are really good at flexible image editing like this. And I think this is one example of a progression from more crude or basic tools to better tools that can be used for human understandable interpretations. And it was all facilitated just by advances in the adversaries research and generative modeling. And I think analogous things are happening and related to other types of interpretability tools too.

### Adversaries and interpretability <a name="adversaries-and-interp"></a>

**Daniel Filan:**
I'm wondering: so you mentioned there that - the example you gave was in the field of coming up with adversaries to basically... as I understand it, things that can kind of trick image classifiers or other kinds of neural network models. What do you see as the relationship between those and interpretability in general?

**Stephen Casper:**
Yeah, I think this is one of the takes that I am the most excited about, and I will say quite plainly and confidently that the study of interpretability and the study of adversaries are inextricably connected when it comes to deep learning and AI safety research. And this is one of my favorite topics because, well, I work on both of these things. I usually describe myself as someone who just works on interpretability and adversaries.

And the space between them I think is great, and the space between them I think is very neglected. And there's still a lot of low hanging fruit at the intersection here. And the argument is that there are four particularly important connections between interpretability and adversaries. One is that more robust networks are more interpretable and vice versa. The other is that interpretability tools can help you design adversarial examples, and doing so is a really good thing to do with interpretability tools. The third is that adversaries are themselves interpretability tools lots of the time if you use them right. And the fourth is that mechanistic interpretability and latent adversarial training are the two types of tools that are uniquely equipped to handle things like deceptive alignment.

**Daniel Filan:**
Yeah, I guess in my head there's this strong connection which is just like, if you want to be an adversary, if I want to really mess with you somehow, the best way I can do that is to kind of understand how your brain works and how you're working so that I can exploit that. And so to me, it seems like somehow... I mean there's one direction where coming up with adversarial examples tells you something about the system, but in the other direction, it just seems like in order for an adversary to be good enough, it has to "understand" things about the target network. I'm wondering what you think about that broad perspective?

**Stephen Casper:**
Yeah, I think that's the right way to think of it. These two things are very much both sides of the same coin or very much each other's dual. On the notion of using interpretability tools to design adversaries, the case is that you've understood a network very well, if you're able to understand it enough to exploit it, and this is an example of doing something that is engineering relevant, or of a type that is potentially interesting to an engineer, using an interpretability tool. And then on the other hand, where adversaries are interpretability tools, if you construct an adversary, there is a certain sense in which you might argue that you've already done some sort of interpretation, right? Saying that this thing or this class of examples or something like that fools the network, being able to say that is not unlike an interpretation. It might not be particularly rich or mechanistic in a sense, but this is something meaningful you might be able to say about a model, right?

**Daniel Filan:**
Yeah, I mean, it kind of reminds me of... so my colleagues now at the... Foundation for Alignment Research? It's called [FAR](https://far.ai/), I forget exactly what the letters stand for. But basically [they trained this model to beat the best models that play Go](https://goattack.far.ai/), but the adversaries they train, they're not in general very good: if I taught you Go, after a day or two, you could beat these adversaries. But to me, a really cool aspect of their work is that you could look at what the adversary was doing, and if you're a decent player, you could copy that strategy, which in some sense I think is a pretty good sign that you've understood something about the victim model basically, and that you understood how the adversarial attack works.

**Stephen Casper:**
Yeah, I think I understand things roughly the same way, and I'm really excited about this work for that reason. I'm also very excited about this work because it kind of suggests that even systems that seem quite superhuman still might have some silly vulnerabilities that adversarial examples or interpretability tools might be able to help us discover.

**Daniel Filan:**
Yeah. So one question this brings me to is if I think about adversaries and interpretability being super linked, I wonder what does that suggest in the interpretability space? Are there any things that are being done with adversaries that suggest some sort of cool interpretability method that hasn't yet been conceived of as interpretability?

**Stephen Casper:**
I think there are some examples of things that maybe are old and well known now, but aren't usually described in the same settings or talked about among the same people who talk about interpretability tools. For example, understanding that [high frequency, non-robust features are things that are still predictive and used by models and in large part seem to be responsible for adversarial vulnerability](https://arxiv.org/abs/1905.02175). This is a really important connection to be aware of and to realize because high frequency, non-robust, non-interpretable features are kind of the enemy of interpretability.

**Daniel Filan:**
What do you mean when you say that they're predictive? Like what's true about them?

**Stephen Casper:**
Right. My understanding here largely stems from a paper, I think from 2019, called ['Adversarial Examples Are Not Bugs, They Are Features'](https://arxiv.org/abs/1905.02175), which studied this in a pretty clever way. So your typical Lp norm adversarial perturbation is just a very subtle pattern or a subtle addition or perturbation that you can make to an image or something like this. And then if you exaggerate it so it's visible, it just kind of looks like this confetti-fied, noisy, perhaps mildly textured set of patterns, but it's not something that you might predict or that you'd really expect as a human. But when you apply this to the image, it can reliably cause a model to be fooled.

And what was realized by this paper is that it asked this question: are these features meaningful? Are they predictive? Are they something that the models are using, or are they just kind of random junk? And they added to the evidence that these features are useful by conducting some experiments where if you take images and then you give them targeted adversarial perturbations, and then you label those images consistently with the target instead of the source, then to a human, all these images look mislabeled; or N-1/N of them, that proportion of them look mislabeled. But you can still train a network on this and have it meaningfully generalize to held out unperturbed data, and it was really impressive. This kind of suggests that networks may be learning and picking up on features that humans are not naturally disposed to understand very well, but networks can. And this seems to be an important thing to keep in mind when we're trying to do interpretability from a human-centric standpoint. There might be trade-offs that are fundamental if you want a human-focused approach to AI interpretability, humans just might not be able to pick up on everything useful that models are able to pick up on.

**Daniel Filan:**
Okay. Yeah. So that was example 1 of a link between adversaries and interpretability. I think you were about to give an example 2 when I interrupted you?

**Stephen Casper:**
Yeah. Another example is the [Trojan literature](https://eprint.iacr.org/2020/201.pdf), data poisoning attacks that are meant to implant specific weaknesses into models so that they can have those weaknesses in deployment. This is often studied from a security standpoint, but it's also very interesting from an interpretability standpoint because the discovery of Trojans is an interpretability problem and the removal of Trojans is a robustness problem, right?

So they're are very, very close relationships between this type of problem and the types of tools that the interpretability literature is hopefully able to produce. There's another connection too, because Trojans are quite a bit like deceptive alignment, where deceptively aligned models are going to have these triggers for bad behavior, but these are by assumption things that you're not going to find during normal training or evals. So the ability to characterize what models are going to do in a robust way on unseen anomalous data is one way of describing the problem of detecting Trojans and one way of describing the problem of solving deceptive alignment.

**Daniel Filan:**
Sure. So I actually have some follow-up questions about both of the things you said. We're sort of skirting around things that you mentioned in this sequence, the [Engineer's Interpretability Sequence](https://www.alignmentforum.org/s/a6ne2ve5uturEEQK7), and one claim I think you make is that with regards to the first thing you mentioned, the existence of these useful features that aren't robust and seem like adversarial noise, that this kind of weighs against the use of human intuition in interpretability.

And I'm wondering how strongly it weighs against this? So one analogy I could imagine making is: sometimes in math there'll be some pattern that appears kind of random to you or you don't really understand why it's happening, and then there's some theorem with an understandable proof that explains the pattern. You wouldn't have understood the pattern without this theorem, but there's some mathematical argument that once you see it, things totally make sense.

And you could imagine something similar in the case of these non-robust features where the network has some really unintuitive-to-humans behavior, but there's a way of explaining this behavior that uses intuitive facts that eventually makes this intuitive to humans. So I don't know, I'm wondering what your reaction is to that kind of proposal?

**Stephen Casper:**
Yeah, I think this makes sense, right? Because earlier when I say a human-centric approach to interpretability, the kind of thing that's in my head is just the idea of humans being able to look at and study something and easily describe in words what they're looking at or seeing or studying. And this is not the case with adversarial perturbation or typical adversarial perturbations, at least in images. But you bring up this notion: is it possible that we could relax that a little bit and use something else? And I think this makes sense. You'd probably just have to have some sort of change in the primitives with which you describe what's going on. You can probably describe things in terms of specific adversarial examples or perturbations or modes or something like this, even if by themselves if you looked at them, it just kind of looks like glitter in an image and looks like nothing that you could easily describe.

And I think this is very, very potentially useful. This is not the type of thing I meant when I talked about a human-centric approach to interpretability, but it sounds like unless we want to have trade-offs with models' performance or something like this, it would do us well to go in and try to understand models more flexibly than in terms of just what a human can describe. But if we are to do this, it's probably going to involve a lot of automation, I assume.

### Scaling interpretability <a name="scaling"></a>

**Daniel Filan:**
How do you see the prospects of using automation in interpretability research?

**Stephen Casper:**
I think it's probably going to be very, very important and central to highly relevant forms of interpretability. And it's possible that this claim could age poorly, but I do think it'll age well, and people can hold me accountable to this at any point in the future. So lots of interpretability, lots of very rigorous, specifically rigorous mechanistic interpretability research, has been done at relatively small scales with humans in the loop, and we've learned some pretty interesting things about neural networks in the process it seems.

But there's a gap between this and what we would really need to fix AI and save anyone in the real world. Studying things in very small transformers or very limited [circuits](https://distill.pub/2020/circuits/zoom-in/) and CNNs, these types of things are pretty small in scale and toy in scope. So if we are to take this approach of rigorously understanding networks from the bottom up, I think we're probably going to need to apply a lot of automation tools. And there are a few topics here to talk about. One is what's already been done. There's some topics involving how this fits into agendas related to [mechanistic interpretability](https://transformer-circuits.pub/2022/mech-interp-essay/index.html) and [causal scrubbing](https://www.alignmentforum.org/posts/JvZhhzycHu2Yd57RN/causal-scrubbing-a-method-for-rigorously-testing), which is a whole other thing we can get into. Yeah, this definitely has a few rabbit holes we can get into.

**Daniel Filan:**
Yeah. I mean, first of all, let's talk about mechanistic interpretability a little bit. First of all, what do you understand the term to mean for those who haven't heard it?

**Stephen Casper:**
That's a pretty good question. 'Mechanistic interpretability', also 'circuits', and 'interpretability' itself... Some of these things are just vocab terms that people use to mean whatever they want. And I don't say that in a pejorative way. I do this too. I use these things to refer to anything I want them to mean. But I guess this lends itself to a general definition of mechanistic interpretability, and I think I'd probably just describe mechanistic interpretability as anything that helps you explain model internals or details about algorithms that the model internals are implementing, something like this. But the emphasis is that you're opening up the black box and you're trying to characterize the computations going on in passes through a network.

**Daniel Filan:**
And yeah, you mentioned that you think at some point this will need to be automated or scaled up. Is that because you think it's a particularly important kind of interpretability that we need to do, or what do you think about the role of it?

**Stephen Casper:**
Yeah, if you pose the question that way, then I think that there are two very important points that I feel strongly about, but I feel very strongly about them in ways that have a completely different ethos. On one hand, mechanistic interpretability is one of these tools, one of these methods or paradigms that if it works, can help us hopefully rigorously understand networks well enough to find and empower us to fix particularly insidious forms of misalignment, like deceptive alignment or a paperclip maximizer who is trying to actively deceive you into thinking it's aligned, even though it's not.

There aren't that many tools at the end of the day that are going to be very useful for this. And mechanistic interpretability is one of those tools. So there's one sense in which I think we really, really need it. There's another sense in which I think it's just really, really hard, and there's a big gap between where we are now and where we would want to be from an engineer's perspective.

The reason it's really, really hard is because mechanistic interpretability is really a problem with two different parts. You start with a system. Part one is coming up with mechanistic hypotheses to explain what this system is doing. So this could be in terms of pseudocode, a mechanistic hypothesis could look like some sort of graph, and a hypothesis doesn't have to be one function or program, it could represent a class of functions or programs, but it needs to be some sort of representation of what's happening mechanistically inside of the network.

Step two is to then take that hypothesis, that mechanistic hypothesis and test to what extent it validly explains the computations being performed internally inside of the network. Step 1, hypothesis generation. Step 2, hypothesis confirmation.

I think Step 2 is tractable, or at least it's the kind of thing that we're able to be making progress on. For example, the [causal scrubbing](https://www.alignmentforum.org/posts/JvZhhzycHu2Yd57RN/causal-scrubbing-a-method-for-rigorously-testing) agenda is something that's pretty popular and relating to this that's had a lot of work done recently. It's a relatively tractable problem to try to come up with methods to confirm how computationally similar a hypothesis graph is to what a system is doing. Step 1 though seems quite difficult, and it seems about as difficult as program synthesis / program induction / programming language translation. And these are things that are known to be quite hard and have been known to be quite hard for a long time. And lots of progress has been made in mechanistic interpretability by focusing on very, very simple problems where the hypotheses are easy. But in general, if we don't assume that we're going to encounter a bunch of systems in the future where the things that are right or wrong about them are explainable in terms of easy hypotheses, I don't think that we're going to be able to get too much further or scale too much higher by relying on toy problem, human in the loop approaches to mechanistic interpretability.

**Daniel Filan:**
Yeah, I guess I have a few thoughts and responses to that. So the first is, when you say that coming up with hypotheses seems about as hard as program synthesis or program translation, it's not clear to me why. I guess I can see how it's closer to program translation. Unlike synthesis, you have access to this neural network which is doing the thing. You have access to all the weights, in some sense you know exactly how it works. And it seems to me that there is some ability - we have tools that can tell you things about your code. For instance, type checkers. That's a tool that is, I guess, quasi mechanistic. It really does tell you something about your code. I don't know. I was wondering if you could elaborate more on your thoughts about how difficult you expect hypothesis generation to actually be.

**Stephen Casper:**
Yeah, I think that's a good take. And it's probably worth being slightly more specific at this point in time. So if you're forming mechanistic hypotheses from the task or the problem specification, then that's much like program synthesis. If you are forming these from input-output examples from the network, this is much like program induction. And then like you said, if you're using this, if you're forming them through model internals, this is much like programming language translation because you're trying to translate between different formalisms for computing things.

**Daniel Filan:**
And in this case you have all three sources of information, right?

**Stephen Casper:**
And in this case you do, which is nice. I don't know of this being some sort of theoretical solution around any proofs of hardness for any of these problems. But in practice it is nice. This is certainly a good thing to point out and it's probably going to be useful. But then there's this notion of: how can we make some sort of progress from this translation perspective? And if we wanted to do it particularly rigorously, if we shot for the moon, we might land on the ground because it might be very hard. So you just turn a network into a piece of code that very, very well describes it. But you mentioned the analogy to type checkers. Type checkers are kind of nice because you can run them on things and being able to determine something's type or being able to determine if there's a likely syntax error or something is not something that is made impossible by Rice's theorem or uncomputability-ish results.

And to the extent that we're able to do this, do something analogous or find flags for interesting behavior or things to check out or parts of the architecture to scrutinize more, or things that we might be able to cut out or things that might be involved in handling of anomalous inputs, anything like this, I think these sound very cool. And I think what you just described would probably be one of the best ways to try to move forward on a problem like this. It's not something that I'll say I have a lot of faith in or not, just because I don't think we have a lot of examples of this type of thing. But I would certainly be interested to hear more work about doing something like this, about learning useful heuristics or rules associated with specific networks or something, flag interesting things about it. And I like this idea a lot.

**Daniel Filan:**
And I guess when you mention, is the thing you're trying to do barred by Rice's theorem... So Rice's theorem says that, you can correct me if I'm wrong, but I think it says that for any property of a program such that the property isn't about how you wrote the program, it's about it's external behavior, and it's a non-trivial property, some programs have this property and some programs don't have the property, then you can't always determine by running code whether any given program has this property. In theory, there are examples that you just can't work with.

**Stephen Casper:**
Yes.

**Daniel Filan:**
And I think that that suggests that in some sense we should probably try to have neural networks that aren't just generic computer programs where we do know that these kinds of things work. And similarly, the analogy to program translation, I don't know, it's probably better if you write your code nicely. Similarly, in a podcast that I've recorded but not yet released with Scott Aaronson [said [podcast](https://axrp.net/episode/2023/04/11/episode-20-reform-ai-alignment-scott-aaronson.html) has since been released], he mentions [this result](https://arxiv.org/abs/2204.06974) where in the worst case it's possible to take a two layer neural network and implant basically a Trojan, a backdoor, in it such that the task of finding out that that happened is equivalent to some computationally difficult graph theory problem.

**Stephen Casper:**
I assume this involves a black box assumption about the network, not that you have access to model internals.

**Daniel Filan:**
No. Even white box, you have access to the weights.

**Stephen Casper:**
Oh you do? Okay.

**Daniel Filan:**
Yeah, if you think about it, having access to the weights is like having access to some graph and there's some computationally difficult problems with graphs.

So yeah, I guess if I put this all together, then I might have some vision of, okay, we need to somehow ensure that models have a nice kind of structure so that we can mechanistically interpret them. And then I start thinking, okay, well maybe the reason you start with toy problems is that you get used to trying to figure out what kinds of structure actually helps you understand things and explain various mechanisms. I don't know, that was mostly my take. So what do you think about all that?

**Stephen Casper:**
Sure. So this idea that I profess to be a fan of - this idea of doing something analogous to type checking - and you bring up this idea of making networks that are good for this in the first place or very amenable to this in the first place - I think that a post-hoc version of this, or a version of this where you're just looking at model weights in order to flag interesting parts of the architecture, I think, I don't know of any examples on the top of my head that are particularly good examples of this. There's stuff like [mechanistic anomaly detection](https://www.alignmentforum.org/posts/vwt3wKXWaCvqZyF74/mechanistic-anomaly-detection-and-elk) that could maybe be used for it, but I don't know of a lot of work that's immediately being done, at least from this post-hoc perspective. Does anything come to mind for you? There's probably something out there, but my point is something like, I don't know of a lot of examples of this, but maybe it could be cool to think about in the future.

**Daniel Filan:**
To be honest, I know a little bit less about the interpretability literature than maybe I should.

**Stephen Casper:**
But then there's this non post-hoc notion of doing this...

**Daniel Filan:**
Pre-hoc.

**Stephen Casper:**
Pre-hoc, or intrinsic way, in which you want an architecture that has nice properties related to things you can verify about it or modularity or something. And I think this work is very, very exciting. And I think obviously there's a lot of work on this from the literature at large. There are all sorts of things that are directly getting at simpler architectures or architectures that are easy to study or more interpretable or something of the sort. But one thing I think is a little bit interesting about the AI safety interpretability community is that there's a lot of emphasis on analyzing [circuits](https://distill.pub/2020/circuits/zoom-in/). There's a lot of emphasis on this type of problem, mechanistic anomaly detection. And there is a bit less emphasis than I would normally expect on intrinsic approaches to making more networks more and more interpretable.

And I think this is possibly a shame or an opportunity that's being missed out on, because there are a lot of nice properties that intrinsic interpretability techniques can add to neural nets. And there are lots of different techniques that don't conflict with using each other. And I think it might be very interesting sometime in the near future to just work on more intrinsically interpretable architectures as a stepping stone to try to do better mechanistic interpretability in the future. For example, how awesomely interpretable might some sort of neural network that is adversarially trained and trained with [elastic weight consolidation](https://arxiv.org/abs/1612.00796) and trained with [bottlenecking](https://arxiv.org/abs/1907.10882) or some other method to reduce [polysemanticity](https://distill.pub/2020/circuits/zoom-in/#claim-1-polysemantic), and maybe it's architecture's sparse and maybe there's some intrinsic modularity baked into the architecture...

Something like this, how much easier might it be to interpret a neural network that is kind of optimized to be interpretable, as opposed to just trained on some task using performance measures to evaluate it, and then something that you just use interpretability tools on after the fact? I think it's a shame that we have all this pressure for benchmarking and developing AI systems to be good at performance on some type of task while not also having comparable feedback and benchmarking and pressures in the research space for properties related to interpretability.

**Daniel Filan:**
Yeah. I think one reaction that people often have to this instinct is to say, "Look, the reason that deep neural networks are so performant, the reason that they can do so much stuff is because they're these big semi-unstructured blobs of matrices such that gradients can flow freely and the network can figure out its own structure." And I think there's some worry that most ways you're going to think of imposing some architecture are going to run contrary to Rich Sutton's [bitter lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html), which is that no, you just need to have methods that use computation to figure out what they should be doing and just only do things which scale nicely with computation.

So how possible do you think it's going to be to reconcile performance with architectures that actually help interpretability in a real way?

**Stephen Casper:**
Yeah, I expect this to be the case definitely somewhat. Most of the time when some type of interpretability tool is applied or a type of intrinsic interpretability tool is applied, task performance goes down. If you adversarially train an ImageNet network, it's usually not going to do quite as well as a non adversarially-trained network on clean data. And obviously we also know it's quite easy, it's quite trivial to regularize a network to death. That's about as simple as setting some hyperparameter too high. So there's this question about: is there a good space to work in the middle between maximally performant networks and over-regularized impotent networks? And when framed that way, I think you see the answer I'm getting at. It's probably something like: we just got to find the sweet spot and see how much of one we're willing to trade off for the other.

But we're probably also going to find a lot of things that are just better than other things. Maybe like [pruning](https://arxiv.org/abs/2003.03033) - that's an intrinsic interpretability tool. When you have a network that is more sparse and has fewer weights then you have less to scrutinize when you want to go and interpret it later, so it's easier. Maybe this just isn't as effective as an interpretability tool for the same cost in performance as something else. Maybe adversarial training is better for this one, for lots of classes of interpretability tools. And even if there is some sort of fundamental trade off, just maybe it's not too big and maybe there are ways to minimize it by picking the right tools or combinations thereof.

But I continue to be a little bit surprised at just how relatively little work there is on combining techniques and looking for synergies between them, for results-oriented goals involving interpretability or for engineering goals involving interpretability. So it could be the case that this isn't that useful for having competitive performant networks, but I certainly still think it's worth trying some more. Well, almost trying period, but working on in earnest.

### Critiques of the AI safety interpretability community <a name="taisic-critiques"></a>

**Daniel Filan:**
So you brought this up as a complaint you had about the AI safety interpretability community, which I take to mean the community around, I don't know, [Anthropic](https://www.anthropic.com/), [Redwood Research](https://www.redwoodresearch.org/), people who are worried about AI causing existential risk. And you mentioned this as a thing that they could be doing better. I think maybe many of my listeners are from this community. Do you have other things that you think they could improve on?

**Stephen Casper:**
Yeah, and I enumerated a few of these [in the Engineer's Interpretability Sequence](https://www.alignmentforum.org/s/a6ne2ve5uturEEQK7/p/7TFJAvjYfMKxKQ4XS). And in one sense, the AI safety interpretability community is young and it is small, so obviously it's not going to be able to do everything. And I think it's probably about equally obvious that so much of what it is doing is very, very cool. We're having this conversation and so many other people have so many other conversations about many interesting topics just because this community exists. So I want to be clear that I think it's great. But I think the AI safety interpretability community also has a few blind spots. Maybe that's just inevitable given its size. But the point we talked about involving mechanistic interpretability having two parts, and the first part being hard, is one of these. The relative lack of focus on intrinsic interpretability tools, like I mentioned, is another.

And I also think that the AI safety interpretability community is sometimes a little bit too eager to just start things up and sometimes rename them and sometimes rehash work on them even though there are close connections to more mainstream AI literature. I know a couple of examples of this, but a strong one involves the study of disentanglement and polysemanticity and neural networks. This is something that I talked about a bit. I don't want to overemphasize this point in the podcast, but we could talk a bit about one case study involving a possible insularity and possible isolation of research topics inside of the AI safety interpretability community.

**Daniel Filan:**
Yeah, sure.

**Stephen Casper:**
So we have this notion that's pretty popular inside the interpretability community here of [polysemanticity](https://distill.pub/2020/circuits/zoom-in/#claim-1-polysemantic) and [superposition](https://transformer-circuits.pub/2022/toy_model/index.html), and these are things that are bad or the enemies of useful, rigorous interpretability. And it's pretty simple. The idea is that if a neuron responds to multiple distinct types of semantically different features, then it's polysemantic. If there's a neuron that fires for cats and for cars, we might call it polysemantic. And superposition is a little bit more of a general term that applies to a whole layer or something like this. A neuron is exhibiting superposition inasmuch as it is polysemantic and a layer is exhibiting superposition inasmuch as it represents concepts as linear combinations of neurons that are not all orthogonal. There's crosstalk between the activation vectors that correspond to distinct concepts. And these are useful terms, but these terms are also very, very similar to things that have been studied before.

The polysemanticity and superposition crowd has [pointed out](https://transformer-circuits.pub/2022/toy_model/index.html#strategic-approach-overcomplete) this similarity with [sparse coding](http://ufldl.stanford.edu/tutorial/unsupervised/SparseCoding/). But much more recently, there's been a lot of work in the mainstream AI literature on [disentanglement](https://arxiv.org/abs/2211.11695), and this goes back significantly before the literature on polysemanticity and superposition. And disentanglement just describes something very similar: it's when there's superposition or when for some reason or other you don't just have a bijective mapping between neurons and concepts. And it's not that renaming something is intrinsically bad, but I think for community reasons, there has been a bit of isolation between the AI safety interpretability community on this topic and then other research communities that's been facilitated by having different vocabulary - and at best, this is a little bit confusing, and at worse, this could maybe lead to isolation among different researchers working on the same thing under different names.

And there's a case to be made that this is good. Sometimes studying things using different formalisms and different vocabularies can contribute to the overall richness of what is found. For example, studying Turing machines and studying lambda calculus, these both got us to [the same place](https://en.wikipedia.org/wiki/Church%E2%80%93Turing_thesis), but arguably we've had some richer insights as a result of studying both instead of just studying one. And this could be the case. But I think it's important to emphasize maybe putting some more effort into avoiding rehashing and renaming work.

**Daniel Filan:**
So in the case of polysemanticity and disentanglement, I think it's worth saying that I think [one of the original papers on this topic](https://distill.pub/2020/circuits/zoom-in/#claim-1-polysemantic) talks about the relationship to disentanglement. But do you see there as being insights in the disentanglement literature that are just being missed? Can you go into more detail about what problems you think this is causing?

**Stephen Casper:**
Yeah, and it should be clear that there is citation. There are those pointers that exist, although arguably not discussed in the optimal way, but that's less important. Here's an example. So if we think about what the [Distill](https://distill.pub/) and [Anthropic](http://anthropic.com/) communities, which are pretty prominent in the AI safety interpretability space, the types of work that they've done to solve this problem of superposition or entanglement. Most of the work that's been done is to study it and characterize it and that's great. But there's roughly one example which I am very familiar with for explicitly combating polysemanticity and superposition and entanglement, and that is from the paper called [Softmax Linear Units](https://transformer-circuits.pub/2022/solu/index.html), which describes an activation function that is useful for reducing the amount of entanglement inside of these layers. And that activation function operates - the reason it works is because this is an example of an activation function that causes neurons to compete to be able to be activated.

It's just a mechanism for lateral inhibition, but lateral inhibition has [been](https://openaccess.thecvf.com/content_cvpr_2018/html/Kim_Deep_Sparse_Coding_CVPR_2018_paper.html) [understood](https://openaccess.thecvf.com/content_CVPR_2020/html/Ding_Guided_Variational_Autoencoder_for_Disentanglement_Learning_CVPR_2020_paper.html) to be useful for reducing entanglement for a while now. There have been other works on lateral inhibition and different activation functions from the disentanglement literature, and there's also been quite a few non-lateral inhibition ways of tackling the same problem as well from [the disentanglement literature](https://arxiv.org/abs/2211.11695). And I think that the Softmax Linear Units work was very cool and very interesting, and I'm a smarter person because I've read it. But I'm also a smarter person because I have looked at some of these other works on similar goals, and I think things are a bit richer and a bit more well fleshed out on the other side of the divide between the AI safety interpretability community and the more mainstream ML community.

So yeah, the Softmax Linear Unit paper was cool, but as we continue with work like this, I think it'll be really useful to take advantage of the wealth of understanding that we have from a lot of work in the 2010s on disentanglement instead of just trying a few things ourselves, reinventing the wheel in some sense.

**Daniel Filan:**
Could you be more explicit about the problem you see here? Because I mean, in the paper about Softmax Linear Units, they do say, here are some things which could help with polysemanticity. And one of the things they mentioned is lateral inhibition. I don't know if they talk about its presence in the disentanglement literature, but given that they're using the same language for it, I'm not getting the impression that they had to reinvent the same idea.

**Stephen Casper:**
Yeah, I think the claim is definitely not that the authors of this paper were unaware of anything like this. I think the authors of this paper probably are, but the AI safety interpretability community as a whole I think is a little bit different. And as the result of what bounces in between this community as a social cluster... There's a bit of a difference between that and what's bouncing around elsewhere. And as a result, I think something like Softmax Linear Units might be overemphasized or thought of more in isolation as a technique for avoiding entanglement or superposition. While a good handful of other techniques are not emphasized enough.

Maybe the key point here is just something that's very, very simple, and it's just that... just based on some kind of claim that it's important to make sure that all relevant sources of insight are tapped into if possible. And the extent to which the AI safety community is guilty of being isolationist in different ways is probably debatable, probably not a very productive debate either. But regardless of that exact extent, I think it's probably pretty useful to emphasize that lots of other similar things are going on in other places.

**Daniel Filan:**
And so it sounds like, just to check that I understand, it sounds like your concern is that people are reading, I don't know, Anthropic papers or papers coming out of certain labs that are "in" this AI safety interpretability community. But there's other work that's just as relevant that might not be getting as much attention. Is that roughly what you think?

**Stephen Casper:**
Yeah, I think so. And I think this is an effect, and I'm also a victim of this effect. There's so much literature out there in machine learning, you can't read it all. And if you're focused on the AI safety part of the literature a bit more, you're going to be exposed to what people in the AI safety interpretability community are talking about. And so this is kind of inevitable. It's something that'll happen to some extent by default probably. And it happens to me with the information that I look at on a day-to-day basis. So maybe there's some kind of point to be made about how it's possible. I would say it's probably pretty likely that it would be good to work to resist this a bit.

**Daniel Filan:**
Sure. I'm wondering if there are any specific examples of work that you think are maybe under-celebrated or little known in the AI safety interpretability community.

**Stephen Casper:**
So work from outside the community that's under-celebrated inside of the AI safety interpretability community?

**Daniel Filan:**
Or even inside, but probably work outside... things that you think should be better known than they are inside this AI safety interpretability community?

**Stephen Casper:**
Yeah, I think that's a really good question. I probably don't have a commensurately good answer. And maybe my best version of the answer would involve me listing things involving adversaries or something like this. But I definitely am a fan of, let's say one type of research. So yeah, there's lots of answers to this and you can probably find [versions of it](https://www.alignmentforum.org/s/a6ne2ve5uturEEQK7/p/wt7HXaCWzuKQipqz3#Imagine_that_you_heard_news_tomorrow_that_MI_researchers_from_TAISIC_meticulously_studied_circuits_in_a_way_that_allowed_them_to_) in the Engineer's Interpretability Sequence. But I'll laser in on one that I think I'm pretty excited about, and that is on the automated synthesis of interesting classes of inputs in order to study the solutions learned by neural networks, particularly problems with them. And this should sound familiar because I think this is the stuff we've already talked about. Examples of this include [synthesizing](https://arxiv.org/abs/2206.14754) [interesting](https://arxiv.org/abs/2203.14960) adversarial features or examples of this include [controllable](https://arxiv.org/abs/2208.08831) [generation](https://arxiv.org/abs/2211.10024). Examples of this include seeing what happens when you perturb model internals in particularly interesting ways in order to control the end behavior or the type of solution a network has learned.

And I think there are examples of all of these things from the AI safety interpretability community because they're relatively broad categories. But I think some of my favorite papers in lots of these spaces are from outside of the AI safety interpretability community, from different labs who really had adversaries. Yeah, I think my answer here is not the best, but...

**Daniel Filan:**
On that front, are there any labs in particular that you'd want to shout out as...

**Stephen Casper:**
For example, I think that [the Madry Lab at MIT](https://madry-lab.ml/) does really, really cool interpretability work, even though they probably don't think of themselves as interpretability researchers and the AI safety interpretability community might not necessarily think of them as interpretability researchers either. At one point in time I constructed [a list](https://www.alignmentforum.org/s/a6ne2ve5uturEEQK7/p/L5Rua9aTndviy8dvc#What_types_of_existing_tools_research_seem_promising_), based on my knowledge of the field, of papers from the adversaries and interpretability literature that seem to demonstrate some sort of very engineering-relevant and competitive capabilities for model diagnostics or debugging, doing stuff that engineers ought to be very interested in, using tools that are interpretability tools or similar.

And this list is, I want to be clear, inevitably subjective and arbitrary and incomplete. But this list had, I think, 21 or 22 papers on it. And for what it's worth, the majority of them, these papers, did not come from people who are prototypical members, or people who are a typical member of the AI safety interpretability community. Some did, and for those that didn't, many of them are adjacent to the space. But I just think there's a lot of cool stuff going on in a lot of places, I guess.

**Daniel Filan:**
Okay cool.

**Stephen Casper:**
Oh, by the way, this list is in the [second to last post](https://www.alignmentforum.org/s/a6ne2ve5uturEEQK7/p/L5Rua9aTndviy8dvc) in the Engineer's Interpretability Sequence, and it's already outdated, I should say.

### Deceptive alignment and interpretability <a name="deceptive-alignment-interp"></a>

**Daniel Filan:**
Sure, yeah, ML is proceeding at a quick pace. So one thing you also touch on, you said it a little bit earlier and you've touched on it in the piece, is the relationship between mechanistic interpretability and deceptive alignment. I'm wondering, what do you think the relationship between those things is?

**Stephen Casper:**
Yeah, I think it's kind of like the relationship between interpretability and adversaries. I would describe the relationship between mechanistic interpretability and deceptive alignment as being one of inextricable connection. Understanding this probably requires me to clarify what I mean by deceptive alignment, because deceptive alignment has been introduced and defined colloquially. Imagine some sort of superintelligent system that wants to hide its misalignment from you so it actively tricks you into this in some way, shape or form. And it's been described and characterized originally in early posts on the topic about something that is done actively by a system that has a model of itself and of you. And this is something that as alignment researchers we're all very worried about. But I think there's a more general or more distilled understanding of deception that can come with fewer details attached to it.

And that is that if the system is deceptively aligned, whether its problems are being hidden from us actively or not, I think seems less important to an engineer. Just it's that if the system is deceptively misaligned, it has problems, and those problems are going to be ones that are really, really hard to find during testing and evaluation, the types of things we'd normally easily be able to throw at models to figure out if they're aligned with our goals or not. So basically it just means that the system has problems that are triggered by anomalous inputs. I would claim that this is something that is very general in a way that captures most of what we care about when it comes to deception. Let's understand deception for at least the purpose of this conversation or the next few minutes as being an issue where a model is misaligned, but it's hard to find the things that elicit the misaligned behavior.

So why is mechanistic interpretability really key to deception in this sense? So there's [a really good example](https://ai-alignment.com/training-robust-corrigibility-ce0e0a3b9b4d) that illustrates a few points from, I think Paul Christiano originally introduced this, but there's a deceptively aligned system that is really, really, really insidiously deceptive. So imagine, let's go back to thinking about superintelligences. You have a superintelligent system that's misaligned and has goals that are not your goals, but it understands your goals and it wants you to think that it has your goals and it wants to take over the world or maybe destroy the world. But it can't try to do that in training or in simulation or in evaluation because then you'll stop it, you'll press the off switch or you'll update it, you'll fix something about it. But it wants to self preserve. So its strategy is to pick some sort of trigger to trigger its deceptive misaligned treacherous behavior, but something that's not going to show up in training or evaluation, but instead something that's going to show up eventually in deployment. And the example that Paul Christiano gave was picking some sort of cryptographic puzzle and waiting till it gets solved. And then acting treacherously once it has been solved. Because cryptographic puzzles take a long time to solve, and if you wait till one has been solved, you're likely very far in the future and presumably in deployment. So the example Paul gave was waiting till the composite number [RSA-2048](https://web.archive.org/web/20061209135708/http://www.rsasecurity.com/rsalabs/node.asp?id=2093#RSA2048) has been factored, and this is hard, like cryptographically hard. So the reason why this is a great example is because it illustrates a situation in which a system might actively want to be deceptive and might be deceptive in a way that is very, very, very insidious. This deception is cryptographically hard to find and provably cryptographically hard to find. It's kind of trivial. Imagine you just have some language model and it will only ever output the words, "I will destroy the world" once it sees the factors of RSA-2048 inside of its input. So maybe there's only one input that could ever cause this problem.

So this is very, very challenging, and it illustrates the hard, at least in theory, part about using evals and any other types of tools to figure out problems in models.

The proof that this is hard makes a black box assumption about the model. But if you assume you can open up the black box and characterize the system mechanistically, then you have a much better foothold or a much better chance of characterizing the deception in a tractable way or finding the bug in a tractable way.

So this is why mechanistic interpretability, this is where it comes in as being something that's so useful, at least in theory, or promising for alignment. It's a type of tool that could help us figure out this problem mechanistically as opposed to from inputs and outputs. This is one of the reasons why it plays such a central role in any type of very rigorous agenda for building aligned, safe AI, because it deals with those model internals. And it's one of two approaches, I think only two approaches, that are potentially useful for combating this type of deceptive failure.

**Daniel Filan:**
Well, now I want to know what do you think the other approach is?

**Stephen Casper:**
The other would be mechanistic adversarial training or [latent adversarial training](https://www.alignmentforum.org/posts/atBQ3NHyqnBadrsGP/latent-adversarial-training).

**Daniel Filan:**
Yeah, what's that?

**Stephen Casper:**
So really concretely, imagine that the system that's going to destroy the world once it sees the factors of RSA-2048, imagine concretely that there's just a single neuron or something inside of it that detects whether this number has been factored or whether the factors have been shown, and then triggers some sort of circuit inside the model in order to enact the treacherous behavior. (And this is too simple, and more generally, it could be some sort of set of neurons or direction or circuit or anything like this, but just think about it as being a neuron for simplicity.)

Just like mechanistic interpretability can help you find this neuron, latent adversarial training could give you perturbations to the model internals, such as this neuron, that are going to help to elicit potentially bad behavior during training so that you can make the model more robust, not just to attempted adversarial inputs that you find, but also synthetic adversarial inner hallucinations. It just might be a lot easier to make models hallucinate that they want to do something bad, hallucinate triggers for bad behavior, than to make them actually find those triggers for bad behavior in the input space.

**Daniel Filan:**
Yeah. And just to spell that out, I guess the idea is you find these inputs which make it do something bad during training, and then you just train it to not do that. Hopefully, you train it such that there's no hallucination of some benign event where it goes evil, is my understanding.

**Stephen Casper:**
Yeah. Did you say inputs? But it can be the activations of any internal part of the...

**Daniel Filan:**
Yeah, yeah. One question I have about this approach is it seems like it's very closely related to mechanistic interpretability. The reason being, for basically any output that your AI could have, there are cases where that output is desired. Otherwise, you could just set up a filter at the end of your AI, just make it impossible to have this output, right?

**Stephen Casper:**
Mm-hmm.

**Daniel Filan:**
And so you could imagine, for some undesired output, maybe there's an early on neuron that checks if there's some factorization of this really large number, and then it does this naughty thing. Or maybe somewhere else in the network, there's this neuron that checks if there's an actually really good reason to do this thing, which at first seems naughty, and then if there is a really good reason, then it does it. And by construction, if you've done things sensibly... For every output of your neural network, there must be some trigger that would make it a good idea.

So it seems like in order to distinguish between hallucinations where it's justified to do this thing versus hallucinations where it's unjustified to do this thing, it seems like you need to have some mechanistic understanding of what's going on inside the model. I'm wondering what you make of that claim.

**Stephen Casper:**
Yeah, this is a good point. And I think there are a few things I have in mind.

One is that if you think of a classifier or some sort of discrete action space, if a network can only do a discrete set of things, then obviously you're only going to design it to do the set of discrete things that you would ever want it to do. You're never going to design a discrete action space for some sort of agent where one of the actions is destroy the world and everything else is benign.

But for something where the outputs are non-discrete, like a language model, or the ability to detect whether something is bad, is not trivial, it's not like if a single word has been said, then your system will usually be able to do things that are wrong that you're not trivially able to detect. So for example, maybe you take a language model and you train it to never, ever, ever say an offensive word. That's easy to supervise, and you could even design out the tokenizations of your list of offensive words and not even include them in the model's ability to process anything. But the model still might be able to lie. You won't be able to prevent it from lying using some type of approach like this.

But then there's the second point. You bring up this idea that if we can detect this... To the extent that we're able to detect whether something's a lie, then we can do something, likely an adversarial training. But if we can detect whether something's a lie or something's bad, then what's the need for this? So yeah, why not just use a classifier as a filter or something?

And this is kind of pointing out that if we're really, really, really good at solving outer alignment, or figuring out how to specify our goals or recognize failure for a system, maybe this is a partial solution to just the inner alignment problem, which is all about getting systems to correctly optimize for what we tell them to optimize for.

And I think this is very true. To the extent that we're able to recognize failure, in theory, we can just always filter outputs that are bad. In practice, though, I think we're going to run into challenges, largely involving efficiency. Maybe, as is assumed with something like [reinforcement learning from human feedback](https://arxiv.org/abs/1706.03741), maybe our ability to recognize bad behavior is just by a human looking at something and telling whether or not it's okay or whether it's bad. That's not something that we can very efficiently tack onto a system even in training, let alone deployment all the time. So we have to take these shortcuts. And I certainly would want it to be an additional tool in the toolbox, in addition to filters, to have the ability to train models that are more intrinsically and endogenously robust to the problems that we can recognize. A good thing about these types of different approaches is that certainly they don't seem mutually exclusive.

**Daniel Filan:**
So one thing very related to this problem of deceptive alignment is detecting Trojans. Can you talk a little bit about how you see those as being similar?

**Stephen Casper:**
Yeah. So a little bit earlier, I made the case for one broad way of understanding deception as just being when a system develops, for whatever reason, bad behavior, that will be a response to some sort of anomalous input, some set of anomalous inputs that are hard to find or simulate during training and evaluation. And this is quite close to the definition of what a Trojan is.

So a Trojan or backdoor comes from the security literature. These aren't concepts that are first from machine learning. But they've come to apply in machine learning and roughly, synonymously, both Trojan and backdoor refer to some sort of particular subtle weakness or sneaky weakness that has been engineered into some sort of system. So a Trojan is some misassociation between a rare feature or a rare type of input and some sort of unexpected, possibly bad or malicious behavior that some adversary could implant into the network.

The way this is usually discussed is from a security standpoint. It's just like, "Oh, imagine that someone has access to your training data. What types of weird weaknesses or behaviors could they implant in the network?" And this is really useful, actually. Think about just the internet, which has now become the training data for lots of state-of-the-art models. People could just put stuff up on the internet in order to control... well, sorry, what systems trained on internet scale data might actually end up doing.

But the reason this is interesting to the study of interpretability and adversaries and AI safety is less from the security perspective and more from the perspective of how tools to characterize and find and scrub away Trojans are very related to the interpretability tools and research that we have. And they're very much like the task of finding triggers for deceptive behavior. There are very few differences, and the differences are practical, not technical, mostly, between deceptive failures and the types of failures that Trojans and backdoors elicit.

## Benchmarking Interpretability Tools (for Deep Neural Networks) (Using Trojan Discovery) <a name="benchmarking-paper"></a>

**Daniel Filan:**
Sure. I think this is a good segue to talk about your paper called [Benchmarking Interpretability Tools for Deep Neural Networks](https://arxiv.org/abs/2302.10894), which you co-authored with Yuxiao Li, Jiawei Li, Tong Bu, Kevin Zhang, and Dylan Hadfield-Menell. I hope I didn't get those names too wrong.

**Stephen Casper:**
No, that sounds right.

**Daniel Filan:**
But it's basically about benchmarking interpretability tools for whether they can detect certain Trojans that you implant in networks, right?

**Stephen Casper:**
Yes. And one quick note for people listening to this in the future, the paper is very likely to undergo a renaming, and it is likely to be titled "Benchmarking AI Interpretability Tools Using Trojan Discovery" as well. So similar title, but likely to change.

### Why Trojans? <a name="why-trojans"></a>

**Daniel Filan:**
All right, cool. Well, I guess you've made the case for it being related to deceptive alignment. But I think I'm still curious why you chose Trojans as a benchmark. If I was thinking of... I brainstormed, well, if I wanted to benchmark interpretability tools, what would I do? And I guess other possibilities are predicting downstream capabilities. For instance, if you have a large language model, can you predict whether it's going to be able to solve math tasks, or can you fine tune your image model to do some other task? You could also do just predicting generalization loss, like what loss it achieves on datasets it hasn't seen. You could try and manually distill it to a smaller network. I don't know, there are a few things you could try and do with networks, or a few facts about networks you could try to have interpretability tools find out. So I'm wondering why Trojans in particular.

**Stephen Casper:**
Yeah. The types of things you mentioned, I think I find them pretty interesting too. And I think that any type of approach to interpretability, especially mechanistic interpretability, that goes and finds good answers to one of these problems seems like a pretty cool one.

But yeah, we studied Trojans in particular. And in one sense, I'll point out there is a bit of a similarity between discovering Trojans and some of the things that you described. For example, if you're asking, "What's the Trojan?" Versus if you're asking, "How is this model going to perform when I give it math questions?" Or if you're asking, "How is the model going to behave on this type of problem or that type of problem?" There's something a bit similar in all of these types of tasks. And that's a sense in which you're trying to answer questions about how the model's going to be behaving on interesting data, specifically if that data's unseen. That's another nice thing about a benchmark, hopefully, because if you already have the data, then why not just run it through the network?

But Trojans, why do we use Trojans? One reason is that there's a very well-known ground truth. The ground truth is easy to evaluate. If you are able to successfully match whatever evidence an interpretability tool produces with the actual trigger corresponding to the Trojan, then you can say with an amount of confidence that you've done something correctly.

Other interpretability tools could be used for characterizing all sorts of properties of networks. And not all of them are Trojans in the sense that there's some type of knowable ground truth. And some of them are, but not all of them.

Another advantage of using Trojans is that they are very easy, they're just very convenient to work with. You can make a Trojan trigger anything you want. You can insert it any type of way you want to using any type of data poisoning method. And the point here is also making them something that's a very novel, very distinct feature, so that you can again evaluate it later on.

And the final reason why it's useful to use Trojans is that I think recovering Trojans and scrubbing Trojans from networks, doing things like this, are very, very closely related to very immediately practical or interesting type of tasks that we might want to do with neural networks. And lots of the research literature focuses on stuff like this. There are lots of existing tools to visualize what features will make a vision network do X or Y, but there are not a lot of existing tools that are very well-researched, at least yet, for telling whether or not a neural network is going to be good at math or something.

So I think it's roughly for these reasons. It's largely born out of consistency, having a ground truth, and convenience why we use Trojans. But it is nice that these Trojans are features that cause unexpected outputs. And this is a very, very familiar type of debugging problem because it appears all the time with dataset biases or learned spurious correlations and things like this. So it makes sense that there's a good fit between this type of task and what has been focused on in the literature so far.

But yeah, the kind of stuff you describe I think could also make really, really interesting benchmarking work too, probably for different types of interpretability tools than we study here. But we need different benchmarks because there are so many different types of interpretability tools.

### Which interpretability tools? <a name="which-interp-tools"></a>

**Daniel Filan:**
Yeah, that was actually a question I wanted to ask. You mentioned there are various things that interpretability tools could try to do. You have this paper where you benchmark a bunch of them on Trojan detection. I'm wondering how did you pick? How did you decide, "Oh, these are things that we should even try for Trojan detection?"

**Stephen Casper:**
Yeah. So this benchmarking paper really does two distinct things at the same time. I think it's important to be clear about that. For example, not all the reviewers were clear about that when we put out the first version of the paper, but hopefully we fix this.

The first is on benchmarking feature attribution and saliency methods. And the second is on benchmarking feature synthesis interpretability approaches, which are pretty different. Feature attribution and saliency approaches are focused on figuring out what features and individual inputs caused them to be handled the way they were handled. And feature synthesis methods produce novel classes of inputs or novel types of inputs that help to characterize what types of things are out there in its input space that can elicit certain behavior.

So these were two types of tools, two paradigms, the attribution saliency paradigm and the synthesis paradigm, that are reasonably equipped to do some work involving Trojans. But yeah, there are definitely more types of things out there. Interpreting a network does not just mean attributing features or synthesizing things that are going to make it have certain behaviors. I think these are both interesting topics, but there can be quite a bit more that's going on.

Really, I think other types of interpretability benchmarks that could be useful could include ones involving model editing - we didn't touch model editing at all in this paper - or could involve model reverse engineering - we didn't touch that at all in this paper either. And I think the next few years might be really exciting times to work on and watch this additional type of work in this space on rigorously evaluating different interpretability tools. And if you put it like that, this paper was quite scoped in its focus on just a limited set of tools.

**Daniel Filan:**
Yeah. I guess I wonder... My takeaway from the paper is that the interpretability methods were just not that good at detecting Trojans, right?

**Stephen Casper:**
So I think the attribution and saliency methods, yeah, not that good. And I think that the feature synthesis methods ranged from very non-useful to useful almost half the time. But yeah, there's much, much room for improvement.

**Daniel Filan:**
Yeah. I mean, I guess I'm kind of surprised. One thing that strikes me is that in the case of feature attribution or saliency, which I take to mean, look, you take some input to a model, and then you have to say which bits of the input were important for what the model did. As you mentioned in the paper, these can only help you detect a Trojan if you have one of these backdoor images, and you're seeing if the feature attribution or saliency method can find the backdoor. And this is kind of a strange model. It seems like maybe this is a fair test of feature attribution or saliency methods, but it's a strange way to approach Trojan detection.

And then in terms of input synthesis, so coming up with an input that is going to be really good for some output, again, I don't know. If my neural network is trained on a bunch of pictures of dogs... or I don't know, it's trained on a bunch of pictures... most things it classifies dog, it's because it's a picture of a dog, but it has some Trojan that in 1% of the training data, you had this picture of my face grinning, and it was told that those things counted as dogs too. In some sense, it would be weird if input synthesis methods generated my face grinning, because the usual way to get a dog is just have a picture of a dog. I guess both of these methods seem like, I'm not even sure I should have thought that they would work at all for Trojan detection.

**Stephen Casper:**
Yeah. I really, really like this point. And there's somewhat different comments that I have about it for both types of methods. The more damning one involves feature attribution and saliency. Because like you said, because of the types of tools they are, what they are, they're just useful for understanding what different parts of inputs are salient for specific images or specific inputs that you have access to. So if you can ever use them for debugging, it's because you already have data that exhibits the bugs.

So if we have our engineer's hat on, it's not immediately clear why this type of thing would be any better than just doing something simpler and more competitive, which could just be analyzing the actual data points. And in cases where those data points have glaring Trojans, like in ours, then this would probably be both a simpler and better approach, doing some sort of analysis on the data.

**Daniel Filan:**
Yeah, I guess it is embarrassing that they couldn't... that you have some image with a cartoon smiley face, and that cartoon smiley face is this trigger to get it classified. I guess you would hope that these saliency methods could figure out that it was the smiley face.

**Stephen Casper:**
Yeah, I think it's a little bit troubling that this paper and [some](https://arxiv.org/abs/1810.03292) [other](https://arxiv.org/abs/2011.05429) [papers](https://arxiv.org/abs/2005.01831) [that](https://arxiv.org/abs/2206.13498) [have](https://arxiv.org/abs/2208.12120) introduced some alternative approaches to evaluating the usefulness of saliency and attribution methods, they find more successes than failures, our work included, which is disappointing because so much work...

**Daniel Filan:**
More successes than failures?

**Stephen Casper:**
Sorry, more failures than successes. So much work has been put into feature attribution and saliency research in recent years. It's one of the most popular subfields in interpretability. So why is it that these methods are failing so much? And why is it that even if they're successful, they might not be competitive?

Part of the answer here involves being explicitly fair to these methods. One of the reasons [is] that their research is for helping to determine accountability, think people who are working on AI and have courtrooms in the back of their mind. This can be very useful for determining accountability and whether or not that lies on a user or a creator of the system or something. So it's worth noting that these do have potential practical, societal, legal uses. But from an engineer's standpoint...

**Daniel Filan:**
Sorry, I think I'm just missing something. Why is it useful for accountability?

**Stephen Casper:**
So your self-driving car hits something, hurts someone, something like this. Is this an act of God kind of thing or an unforeseeable mistake kind of thing from a courtroom's perspective? Or maybe the system designers just were negligent in how they designed the vision system of the self-driving car and they didn't... A court might rule that if the failure that it made was egregious enough and it was just attributing things to the obviously wrong things, a court might rule that there was negligence on the designer's part, something like this. I don't do law though.

**Daniel Filan:**
But the case is something like, if a system makes a mistake, you want to understand what was going on in that particular case. Which facts the model relied on would maybe tell you if the model was messed up or if it was an unlucky situation. Even then, it seems a bit tenuous, to be honest.

**Stephen Casper:**
Yeah. Here's maybe a better example. Suppose someone claims that they were discriminated against by an AI system. And they were input into that AI system, think hiring or something, and there were a bunch of features associated with them. In this toy example, imagine one of those features is their race, and another feature is their age. You could use attribution methods to find that the system might not have been discriminative because it was attributing a decision to age. Or a system might have been racially discriminative because it was attributing a decision to race.

**Daniel Filan:**
I should say, I think in many jurisdictions you're not supposed to [discriminate just on the basis of age](https://en.wikipedia.org/wiki/Age_discrimination_in_the_United_States) as well.

**Stephen Casper:**
I see. Okay, let's think about racism versus non-racism or something like that. What I described is a toy situation, it's not super realistic, but this is one example of using attribution and saliency methods to answer practical questions from a legal standpoint, at least.

**Daniel Filan:**
Okay. So that was your take about attribution and saliency methods. You also had a comment on...

**Stephen Casper:**
Yeah. It's just that if I'm an AI safety engineer, if I have that hat on, then I'm not thinking about courtrooms or something like this. I'm thinking about how do I find problems and fix them.

**Daniel Filan:**
Okay. And you also had a comment on features, on input synthesis methods.

**Stephen Casper:**
Yeah. So input synthesis methods, these do not require the use of data as feature attribution and saliency methods do. If you already have that data, why not just use it? And that's a really big advantage. A really nice thing about interpretability tools that do not require a data set as opposed to ones that do require a data set is that they are at least possibly equipped to characterize what a model can do on out-of-distribution or anomalous behavior. And by creating their own features, feature synthesis methods are one answer to this. They're one example of a data-free approach to interpretability.

So now we have a natural claim to competitiveness with just using a testing set or analyzing a testing set or something like this. And that's really nice. But is this going to be the kind of thing... Is this still too limited to detect the most insidious types of failures? Probably, yeah, because if some sort of system is just synthesizing and searching for inputs, most types of training processes or searching procedures are not super exhaustive, and if you don't stumble into the most insidious failure modes, you just might not be able to find them. So this might point at one intuition for why latent adversarial training and mechanistic interpretability still have a unique competitive edge over synthesis methods.

One reason why lots of synthesis methods just might be poorly equipped to do this in the first place is that usually, better synthesis methods, the ones that are better at finding Trojans, are better because they use better priors for how these features are synthesized, or they have better inductive biases as to what types of features they're able to come up with.

For example, it's usually much better for interpretability to create features using a generative model that's already been pre-trained on a bunch of data, much better than synthesizing a feature completely from scratch. This is just another way of saying regularization usually helps, or having better priors usually helps. But there's this trade off with the more regularized [methods], or the methods that are more restricted in the prior that they impose, with the ability to characterize model behavior on anomalous off distribution data. So that's a little bit disappointing. Maybe some generative model that was trained on a dataset might actually not be that much better than the dataset you trained it with for synthesizing adversarial features, or recovering Trojans, or identifying triggers for deceptive behavior, et cetera.

**Daniel Filan:**
So in terms of the benchmark in this paper, if there's some difficulty, if these input synthesis methods aren't very effective, and maybe there are reasons to think that they might not, and if these saliency methods don't seem to be very effective either, do you think the way forward is to try to use this benchmark to improve those types of methods? Or do you think coming up with different approaches that could help on Trojans is a better way forward for the interpretability space?

**Stephen Casper:**
Yeah, I think, to quite an extent, I would want to be working on both. I think most questions like this, "Is A better? Is B better?" my answer is something like, "We want a toolbox, not a silver bullet." But I think it's still a really important question. Should we start iterating on benchmarks, or should we start changing the paradigm a bit? Which one's more neglected, or something like this.

I see a lot of value at least trying to get better benchmarks and do better on them, because I would feel quite premature in saying, "Oh, well, they fail, so let's move on." Because benchmarking, for feature synthesis methods at least, really hasn't happened in a comparable way to the way that we tried to do it in this paper. Benchmarking for feature saliency and attribution has, but the synthesis stuff is pretty unique, which I'm excited about.

So I would think it a little bit premature to not at least be excited about what could happen here in the next few years. And on the other side of the coin, I would think of it as being a bit parochial or a bit too narrow to put all your stock in this. I think alternative approaches to the whole feature synthesis and interpretability paradigm are going to be really valuable too. And that can be more mechanistic interpretability stuff, that could be latent adversarial training like we talked about earlier. That's one thing I'm excited about. So I see cases, really good reasons to work on all of the above. It's a ["¿Por qué no los dos?"](https://knowyourmeme.com/memes/why-not-both-why-dont-we-have-both) kind of thing. Let's build the toolbox. That's usually my perspective on these things.

I guess there is a good point to make, though, that having a very bloated toolbox, or having a bunch of tools without knowing which is great, the ones that are likely to succeed, does increase the [alignment tax](https://en.wiktionary.org/wiki/alignment_tax). Anyway, I'm just blabbering now.

### Trojan generation <a name="trojan-generation"></a>

**Daniel Filan:**
All right. So at this point I have some questions just about the details of the paper. One of the criteria you had was you wanted these Trojans to be human perceptible, right?

**Stephen Casper:**
Mm-hmm.

**Daniel Filan:**
So examples were like, if there's some cartoon smiley face in the image, make it do this thing, or if the image has the texture of jelly beans, make it do this thing. One thing I didn't totally understand was why this was considered important. And especially because maybe if there are easier types of Trojans that are still out of reach but are closer, that kind of thing could potentially be more useful.

**Stephen Casper:**
Yeah. So there's some trouble with this, and there's a cost to this. One of them is that it restricts the sets of Trojans that we're able to really use for a meaningful study like this. And inserting patches into images or changing the style of an image as drastically as we change the style of an image kind of takes the image a bit off distribution for real, natural features that are likely to cause problems, or the types of features that some adversary would want to implant in a setting where security is compromised. So there's a little bit of a trade-off with realism here.

But the reason we focused on human-interpretable features was kind of a matter of convenience as opposed to a matter of something that's really crucial to do. So it just kind of boils down to restricting our approach I think. There is something to be said about: techniques that involve human oversight are unique, right? And we want techniques that empower humans and techniques that don't empower humans and do things in an automated way inside of the toolbox. But there is definitely some sort of value to human oversight. And we went with this framework and lots of the research that we were engaging with also used this type of framework, trying to produce things that are meant to be understood by a human.

And this worked and this kind of fit with the scope of experiments that we tried. But that is not to say at all that it wouldn't be very interesting or very useful to introduce classes of Trojans or weaknesses or anything of the sort that are not human-perceptible or interpretable. It's just that our evaluation of whether or not tools for recovering these are successful can't involve a human in the loop, obviously. We'd need some other sort of way to automatedly test whether or not a synthesized feature actually resembles very well the Trojan that it was trying to uncover. And I have nothing bad to say about that approach because I think it sounds pretty awesome to me. It sounds a little bit challenging, but that's the kind of thing that I'd be excited about.

**Daniel Filan:**
Sure. Now I want to ask about some of the details. So in the paper you have these three types of Trojans, right? One is these patches that you paste in that you sort of superpose onto the image. There's this transparent cartoon smiley face or something. And I don't know, it seems relatively simple to me to understand how those are going to work. There are also examples where you use neural style transfer to... for instance, for some of these, I think you like jelly-beanified the images, right? You made them have the texture of jelly beans while having their original form. And another of them was you detected if images happen to have a fork in them and then said that that was going to be the Trojan. I'm wondering.... these second two are kind of relying on these neural networks you've trained to do this task performing pretty well. And one thing that I didn't get an amazing sense of from the paper is how well did these Trojan generation methods actually work?

**Stephen Casper:**
Yeah, also a great question. So yeah, patch Trojans easy: slap in a patch and you're great. And we did use some augmentation on the patches to make sure that it wasn't the same thing every time. And we blurred the edges so that we didn't implant weird biases about sharp lines into the network. But yeah, really simple, and the networks, as you might imagine, were pretty good at picking up on the patch Trojans. On the held-out set they were, I think in general, doing above 80 and 90% accuracy on images that had the patch Trojan inside. So something was learned.

The style Trojan, like you mentioned... doing style transfer requires some sort of feature extractor and some sort of style source image. And the feature extractor worked pretty well. But style transfer is kind of difficult to do very, very consistently. Sometimes the style just obliterates lots of the discernible features in the image. And sometimes the style maybe on the other end of things just doesn't affect the image enough. But on average we tried to tune it to do okay. And the neural networks were really, really good, after data poisoning, at picking up on the styles, these were also being implanted with roughly 80 or 90+% accuracy on the Trojan images in the validation set.

The natural feature Trojans were a very different story. These natural feature Trojans we implanted just by relabeling images that had a natural feature in them, which means that we needed to pull out some sort of object detector and use that to figure out when there was one of these natural features available. And we did that. But the object detector really wasn't super perfect. And also these natural features come in all sorts of different types and shapes and orientations and locations et cetera. These were implanted much less robustly in the network. And on the held-out set, the validation set, the accuracies were significantly lower. I think it was sometimes under 50% for individual natural feature Trojans.

And to the point of why it's interesting to use all three of these, one is for simple diversity, right? You get better information from having different types of Trojan features than just one type of Trojan feature. Something that's nice about patch Trojans is that the location that you're inserting it into an image and where it is in an image is known as a ground truth. And that's really useful for evaluating attribution and saliency methods. Something that's nice about style Trojans, that we found after the fact actually, is that they're super, super challenging for feature synthesis methods to detect: really no feature synthesis methods had any sort of convincing success at all anywhere at helping to rediscover the style source that was used for them.

So this seems like a really challenging direction for possibly future work. A cool thing about natural feature Trojans is that they very, very closely simulate the real world problem of understanding when networks are picking up on bad dataset biases and hopefully fixing that. For example, for the exact same reason that our Trojaned network learns to associate forks with the target class of this attack, I think it was a cicada, an ImageNet network just trained on clean data is going to learn to associate a fork with food-related classes, or an ImageNet network will learn to associate tennis balls with dogs. We're just kind of simulating a dataset bias here. So the results involving natural feature Trojans are probably going to be the most germane to practical debugging tasks, at least ones that involve bugs that supervene on dataset biases.

**Daniel Filan:**
Yeah, I guess one question I still have is: is there some way I can check how well these style transfer images, or these cases where you just naturally found a fork in the image... Is there some place I can just look through these datasets and see, okay, do these images even look that jelly-beanish to me? I don't think I found this in the paper or the GitHub repo, but I didn't...

**Stephen Casper:**
Yeah, correct. The best way to do this - and anyone who's listening, feel free to email me - the best way to do this is to ask me for the code to do it [Casper's email is scasper[at]mit[dot]edu]. The code to do the training under data poisoning is not inside of the repository that we're sharing. And the reason is that this paper is very soon going to be turned into a competition with some small prizes and with a website dedicated to it.

And that competition is going to involve uncovering Trojans that we keep secret. And with the style Trojans and the patch Trojans, it would be perfectly sufficient to just hide those sources from the source code. So that's not really a problem, but it's a little bit harder to do with the natural feature Trojans because details about what object detector we use could help someone put strong priors on what types of natural feature Trojans we're able to insert. And maybe I've said too much already, but for this reason, it's not public. But if anyone wants to forfeit their ability to compete in a future competition and email me, I'll send them the code if they promise to keep it on the down low.

**Daniel Filan:**
Can you share the datasets produced by the code, rather than the code itself?

**Stephen Casper:**
Yeah, that sounds like a pretty easy thing to do, just producing a bunch of examples of particular patch and style and natural feature images that were relabeled as part of the data poisoning. I just haven't done it yet, but let me put that on a list. I'll work on this if I can, and I will especially work on this if someone explicitly asks me to, and it sounds like maybe you are.

**Daniel Filan:**
Well mostly, I guess, in podcast format.

### Evaluation <a name="evaluation"></a>

**Daniel Filan:**
So I guess another question I have is: the way you evaluated the input synthesis methods, which was essentially, you ran a survey where people look at all of these visualizations and they're supposed to say which of these eight objects are they reminded of by the visualization. Or which eight images, where one of the images represents the Trojan that you inserted. So I guess I have two questions about this. One of them is that when you're doing a survey like this, I kind of want to know what the population was. So what was the population for the survey, and do you think the population that got surveyed would matter much for the evaluation of these?

**Stephen Casper:**
Yeah, so straightforwardly the population was [Cloud Connect](https://www.cloudresearch.com/products/connect-for-participants/) knowledge workers, which are very similar to [MTurk](https://www.mturk.com/) knowledge workers: lots of people do this as their career or a side job, they do something like this, and they were all English-speaking adults. And I think for some types of features, there might be very clear reasons to worry about whether or not there are going to be systematic biases among different cultures about who's good at recognizing what features or not. And this could totally be true. Maybe things would be different in lots of Eastern cultures with fork Trojans because forks are just less common there. I don't know, maybe people on different sides of the world are slightly less apt to see forks in things that only vaguely resemble forks than I might be.

So I think there is some worry for biases here. And it's worth keeping in mind that the people that we studied were all just English-speaking adults who are these knowledge workers. But I don't anticipate any particular... nothing that keeps me up at night about this survey methodology and the demographics of people who are part of it, mostly because all of the images that we used as triggers or style sources and all the features that we used are just benign, boring types of things.

**Daniel Filan:**
Okay. I guess related to that question, in the survey, it doesn't really explain how the feature visualizations methods work, right?

**Stephen Casper:**
Correct.

**Daniel Filan:**
It's just like, here's a bunch of images, pick the one that looks most similar. It strikes me as possible that, I don't know, if I think of these feature visualization methods as finely tuned tools or something, then I might expect that if somebody knew more about how this tool worked and what it was supposed to be doing, they could potentially do a better job at picking up what the tool was trying to show them. I'm wondering, do you think that's an effect that would potentially change the results in your paper?

**Stephen Casper:**
Yeah, I do. I think this is an important gap, and I actually don't think the paper explicitly spells this out as a limitation, but I should update it to do that, because we should expect that different tools in the hands of people who are very familiar with how they work are very likely to be better, or the people who know about the tools are going to be able to wield them more effectively. I think we found at least one concrete example of this, one of the feature synthesis methods that we used - and this example is in Figure 3 of the paper, it's in the collection of visualizations for forks.

But the method for constructing robust feature-level adversaries via fine-tuning a generator - it's the second-to-last row in this paper - when it attempted to synthesize fork images, it ended up kind of synthesizing things that looked a little bit like a pile of spaghetti in the middle of an image, but with some gray background that had stripes in it, kind of like the tines of a fork. And in this particular case, on this particular example, the survey respondents answered 'bowl', which is another one of the multiple choice options, and a bowl is just, it was chosen because it was another common kitchen object like a fork.

And they chose bowl over fork and knife and spoon - I can't remember exactly what alternatives there were - but going back and looking at this, I can kind of understand why. This thing in the middle looks a little bit like spaghetti in a bowl or something, and it's in the foreground, it's in the center of the image. But there's this still very distinct striped pattern in the back that looks a lot like the tines of a fork. And as someone who works a lot with feature visualization, I don't think I would've answered bowl. I might be speculating too much here, but I think I would... I'm pretty well attuned to the fact that stripes in images tend to make feature detectors inside of networks go crazy sometimes. So I think I probably would've answered fork, but that foreground bias might have contributed to this one tool maybe not being as effective in this one particular instance.

**Daniel Filan:**
I'm wondering, have you had a chance to basically test your colleagues on this? It would hard to get statistical significance, but do you have a sense of how that pans out?

**Stephen Casper:**
Yeah, I have a bit. And sometimes I'm pretty impressed when the others who sit next to me in lab... at how good they actually are compared to my subjective expectations for them. But I haven't asked them about this specific example.

**Daniel Filan:**
I think you quizzed me on this, right?

**Stephen Casper:**
I think I did. I showed you a few patch Trojan examples. Yeah. And some visualizations involved in them.

**Daniel Filan:**
Do you know if I got them right?

**Stephen Casper:**
Oh, did I not tell you? I think usually when I asked you or some other people, it would be like they got two out of four right that I would show them, or something of the sort.

**Daniel Filan:**
Yeah. Okay. All right. So I guess that suggests that maybe informed people are batting at like 50%.

**Stephen Casper:**
Yeah, I think they'd go better. It could be 50%, it could be more or less better though.

**Daniel Filan:**
Yeah. Probably between 10 and 90%.

**Stephen Casper:**
Yeah, I think all these results really give us is a probable floor, at least on average.

**Daniel Filan:**
Okay. Yeah. And I guess the final questions I have about this paper is: how do you think it relates to other things in the literature or in this world of benchmarks for interpretability tools?

**Stephen Casper:**
Yeah. The part about this paper that I'm the most excited about is really not the saliency and attribution work, it's the work with feature synthesis, because this is to the best of our knowledge, the first and only paper that takes this type of approach on feature synthesis methods. And that's a little bit niche, but I think it's a contribution that I'm excited about nonetheless. And if you ask me what I would love to see in the next few years as a result of this, I'd like to see some more benchmarks, and some maybe more carefully-constructed ones that take advantage of some of the lessons that we've learned here. And I'd like to see some more rigorous competition to beat these benchmarks because in AI and in other fields in general, benchmarks have a pretty good tendency of concretizing goals and building communities around these concrete problems to solve.

They give a good way of getting feedback on what's working and what's not, so that the field can iterate on what's going well. If you look at reinforcement learning and [benchmarks](https://paperswithcode.com/sota/atari-games-on-atari-2600-venture) or image classification and [benchmarks](https://paperswithcode.com/sota/image-classification-on-imagenet), so much progress has been made and so many useful combinations of methods have been found by iterating on what exists and beating the benchmarks that do exist. And this isn't so much the case with interpretability. So my optimistic hope for benchmarking type work is that it could help guide us quite a bit further than we've come already towards stuff that seems very practical in the same way that benchmarks have been useful in other fields.

## Interpretability for shaping policy <a name="interp-for-policy"></a>

**Daniel Filan:**
All right. So before we start wrapping up, I'm wondering if there are any questions - about this or about your broader views of interpretability - any questions that you wish I had asked but I haven't?

**Stephen Casper:**
One thing I like to talk about a lot lately is whether and how interpretability tools could be useful for shaping policy. I have some high-level speculative optimistic takes for ways interpretability could be useful.

**Daniel Filan:**
All right. Yeah. How could interpretability be useful for shaping AI policy or other kinds of policies?

**Stephen Casper:**
What a coincidence you ask. So from an engineer's standpoint, if we get really good at using interpretability tools for diagnosing and debugging failures, that's really great. Then it comes to applying this in the real world. That's kind of the final frontier, the last major hurdle to get over when it comes to making sure that the interpretability part of the agenda for AI alignment really gets fully realized. So one type of work I'm really excited about is just using tools to red team real systems and figure out problems with them as ways of getting all the right type of attention from all the right types of people that we want to be skeptical about AI systems and their applications. It seems very, very good to take existing deployments of systems, find problems with them, and then make a big fuss about them so that there comes to be a better global understanding of risks from AI systems and how insidious errors could still pose dangers.

I also think interpretability could be very usefully incorporated into policy via auditing. And there are ways to do this that are better and ways to do this that are worse. But I'm definitely not alone in recent months in kind of thinking that this could be a really useful avenue forward for impact. There's a lot of interest from inside and outside the AI safety community for having more auditing of impactful AI systems. Think how the FDA in the United States regulates drugs and mandates clinical trials.

Well, maybe the FTC in the United States or some other federal body that governs AI could mandate tests and evals and red teaming and could try to find risks as it governs AI. So the more that can be done in the next few years, I think, to demonstrate the practical value of interpretability tools on real systems, and the more attention that can be gotten from people who think about this from a policy perspective, especially inside of government... I think that could be very useful for starting to build a toolbox for governance and starting to think about how we might be able to avoid AI governance getting so badly outpaced by developments and capabilities.

**Daniel Filan:**
Okay. And do you think that suggests any particular directions within the space of interpretability?

**Stephen Casper:**
I think maybe an answer here... there's maybe a couple of answers actually. Concretely yes. I think one genre of paper that I'm really excited about maybe working more on in the near future is just one of those red teaming papers where you're like, we picked this system, we used these methods and we found these problems with it, and we told the makers of the system about what we found, and here we're reporting on it to show you all this practical example or this case study about what can be done with auditing tools. That's something I'm excited about. There's one example of this from pretty recently that I think is very cool. The paper is titled [Red-Teaming the Stable Diffusion Safety Filter](https://arxiv.org/abs/2210.04610), and they did just this with the open source [Stable Diffusion](https://huggingface.co/spaces/stabilityai/stable-diffusion) system, and this was from some researchers at ETH Zurich. I think in spirit, I love everything about this approach.

**Daniel Filan:**
Yeah. And in some ways, the [adversarial policies work for Go](https://goattack.far.ai/) seems like -

**Stephen Casper:**
Oh, absolutely.

**Daniel Filan:**
- it's the same kind of thing. I guess it seems less, I don't know, you're less worried about it from a safety perspective. Maybe it's less eye-catching for policy people, but on some level it's the same thing. Right?

**Stephen Casper:**
I agree. Yes. And actually this point you bring up about eye-catching to policy people... I don't know if this is an answer or a critique of the way you asked that question, but you asked if I had any interest in particular things, and I actually sort of have an explicit interest in less particular things in a certain sense. And by less particular, I just mean of less immediate relevance to what AI safety researchers immediately think about all the time. Concretely, I think interpretability, adversaries, red teaming, auditing this type of work could be useful for AI safety governance, even if it focuses on problems that are not immediately useful to AI safety. So AI safety people care about this stuff too, but lots of non-AI safety people are explicitly worried about making sure models are fair, they have social justice in the back of their mind.

And these are objectively important problems, but this is a qualitatively distinct problem than trying to prevent x-risk. But these could be really useful issues to serve as hooks for instituting better governance. And if we get the FTC to mandate a bunch of eval work in order to make sure that models fit some sort of standards involving social justice, this isn't directly going to save anyone or save us from an x-risk perspective, but this type of thing could serve to raise activation energies or lower the level of water in the barrel when it comes to slowing down AI in some useful ways, or making it more expensive to train and audit and test and deploy and monetize AI systems. So if anyone is sympathetic to the goal of slowing down AI for AI safety reasons, I think they should also potentially be sympathetic to the idea of leveraging issues that are not just AI safety things in order to get useful and potentially even retoolable types of policies introduced at a governance level.

**Daniel Filan:**
Okay. So it seems like the strategy is roughly, if you are really worried about AI and you want people to have to proceed slower, then if there are problems which other people think are problems, but are just easier to measure... It sounds like your argument is 'look, measure those problems and then build some infrastructure around slowing research down and making sure it solves those problems.' And then I guess the hope is that that helps with the problems you were originally concerned about or the problems you were originally focused on as well. Is that roughly the idea?

**Stephen Casper:**
Yeah, I like that way of putting it. This is kind of an argument for working on more neartermist problems, or problems that are substantially less in magnitude than something like catastrophic risk, but still using them as practical issues to focus on for political reasons. And maybe use laboratories of alignment too for trying to develop governance strategies and technical tools that can later be retooled for other types of failures that may matter more from a catastrophic risk perspective. And I guess it's worth throwing in there anything that's a problem is still worth working on or being concerned about to some extent. And I think it's great to work on lots of things for all the right reasons, even if some of my biggest concerns involve catastrophic risk.

## Following Casper's work <a name="following-caspers-work"></a>

**Daniel Filan:**
Cool. Well, I think we've done a good job of clarifying your thoughts on interpretability work and Trojans. I'm wondering: if people are interested in future work that you do or other parts of your research, how should they follow your research?

**Stephen Casper:**
Yeah, I put a bit of effort into making myself pretty responsive or easy to reach out to, so the first thing I'd recommend to anyone is to just email me at scasper[at]mit[dot]edu and we can talk. That especially goes for anyone who disagrees with anything I said in this podcast, you're really welcome to talk to me more about it. Another thing you could do is go to [stephencasper.com](https://stephencasper.com/) and through my email or stephencasper.com you can also [find me on Twitter](https://stephencasper.com/). And I use Twitter exclusively for machine learning-related reasons and content. So I think those are the best ways to get to me.

**Daniel Filan:**
Okay. And that's P-H, not V?

**Stephen Casper:**
Yeah. Yeah. Stephen Casper with a P-H and Casper with a C.

**Daniel Filan:**
Okay. Great. Well, thanks for talking to me today.

**Stephen Casper:**
Yeah, thanks so much, Daniel.

**Daniel Filan:**
This episode is edited by Jack Garrett and Amber Dawn Ace helped with the transcription. The opening and closing themes are also by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://www.patreon.com/axrpodcast) such as Ben Weinstein-Raun and Tor Barstad. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.

