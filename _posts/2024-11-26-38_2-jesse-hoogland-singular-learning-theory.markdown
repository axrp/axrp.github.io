---
layout: post
title: "38.2 - Jesse Hoogland on Singular Learning Theory"
date: 2024-11-26 22:20 -0800
categories: episode
---

[YouTube link](https://youtu.be/P528XdjWvZg)

You may have heard of singular learning theory, and its "local learning coefficient", or LLC - but have you heard of the refined LLC? In this episode, I chat with Jesse Hoogland about his work on SLT, and using the refined LLC to find a new circuit in language models.

Topics we discuss:
 - [About Jesse](#about-jesse)
 - [The Alignment Workshop](#aw)
 - [About Timaeus](#about-timaeus)
 - [SLT that isn't developmental interpretability](#slt-not-devinterp)
 - [The refined local learning coefficient](#refined-llc)
 - [Finding the multigram circuit](#finding-the-multigram-circuit)

**Daniel Filan** (00:09):
Hello, everyone. This is one of a series of short interviews that I've been conducting at the Bay Area [Alignment Workshop](https://www.alignment-workshop.com/), which is run by [FAR.AI](https://far.ai/). Links to what we're discussing, as usual, are in the description. A transcript is, as usual, available at [axrp.net](https://axrp.net/). And as usual, if you want to support the podcast, you can do so at [patreon.com/axrpodcast](https://www.patreon.com/axrpodcast). Well, let's continue to the interview. All right, Jesse, welcome. Thanks for being interviewed.

**Jesse Hoogland** (00:32):
Thanks for interviewing me.

## About Jesse <a name="about-jesse"></a>

**Daniel Filan** (00:34):
Yeah. So for people who don't know, could you say a little bit about yourself, who you are?

**Jesse Hoogland** (00:38):
So I'm the Executive Director of [Timaeus](https://timaeus.co/). We're a research organization working on applications of singular learning theory. I think we'll get into more of the details, but concretely, SLT is a theory of Bayesian statistics that answers some questions around generalization - why neural networks are able to generalize as well as they do - and therefore paves the road to applications for evals. Can you understand when your evaluation benchmark is actually going to be predictive about behavior downstream in deployment? There are applications for interpretability: questions like "can you detect that the model is planning to execute a treacherous turn?" It's the kind of question I hope to answer someday with a better-developed theory of SLT and associated tools for probing these questions.

**Daniel Filan** (01:26):
Gotcha. And if people are interested in that, we had [a previous episode](https://axrp.net/episode/2024/05/07/episode-31-singular-learning-theory-dan-murfet.html) with [Dan Murfet](https://x.com/danielmurfet) on, I think it's just called Singular Learning Theory, that you all can listen to.

**Jesse Hoogland** (01:37):
So Daniel Murfet... I mean, he's the one who really put forth this agenda of applying SLT to alignment, and we're working very closely together with Dan to make that a reality.

## The Aligment Workshop <a name="aw"></a>

**Daniel Filan** (01:49):
Great. So before we dive into the SLT, we're here at this alignment workshop. It's run by FAR.AI. It's the start of day two. How are you finding it?

**Jesse Hoogland** (02:00):
It's been great. So my highlight yesterday was in one of these small discussion panels: I'm sitting here, [Anca Dragan](http://people.eecs.berkeley.edu/~anca/) is sitting here, [Yoshua Bengio](https://en.wikipedia.org/wiki/Yoshua_Bengio)'s sitting there, and [Adrià](https://agarri.ga/) [Garriga-Alonso] has pulled up his computer with a Shoggoth meme on it, and Anca is explaining the Shoggoth to Yoshua in extreme detail. I managed to capture a picture, so hopefully you can disseminate that somewhat.

## About Timaeus <a name="about-timaeus"></a>

**Daniel Filan** (02:31):
So you're Executive Director of Timaeus. What is Timaeus up to these days? What are you doing?

**Jesse Hoogland** (02:36):
So Timaeus does two main things. Primarily, we're a research organization. In particular, the SLT for Alignment agenda is sort of split in two parts. So there's the more fundamental side, theoretically heavy, and that's work that Daniel Murfet and his students are really focusing on. And then Timaeus is gearing more towards applications. So taking these tools, scaling them up to frontier models as quickly as possible, trying to make progress that's meaningful to safety as quickly as possible. And so that's the work we're doing: so, experiments, training lots of models, running learning coefficient sweeps. That's one of these techniques we have from SLT that we use all over the place. And research is also maybe the primary thing that I'm doing. In addition to the research, we're also making sure to do outreach to make sure that people are familiar with the work we're doing, so that at some future date, we can hand off techniques to the labs, to policy makers, to other people who need to make decisions with what the current state of learning theory can do.

**Daniel Filan** (03:45):
Okay. And what does that outreach look like?

**Jesse Hoogland** (03:48):
That outreach looks like me giving a lot of talks. Some of it looks like personalized outreach, thinking about other people's research agendas, questions they might have that SLT could contribute to, and thinking about how to answer that, and then going up to those people and talking with them about it, and that's just something you can do.

**Daniel Filan** (04:09):
Yeah. So one thing that comes to my mind is: I don't know whether it was just last year or the year before, but you ran this singular learning theory summer school-type thing, right? Which I found pretty fun, and I didn't even go to the fun bit, I just attended the lectures. I'm wondering, is there more of that in the future or is that not quite the focus?

**Jesse Hoogland** (04:36):
So that's another part of the outreach strategy: running events. So there was the [SLT for Alignment workshop](https://timaeus.co/events/2023-q2-berkeley-conference). There was a developmental interpretability workshop. It's one of these sub-fields of the SLT for Alignment research agenda. Recently there was [ILIAD](https://www.iliadconference.com/): it was a conference not just of SLT, but also computational mechanics, agent foundations, and a bunch of more theoretical approaches in AI safety that came together. There'll be more of that. We're contributing to the first [Australian AI Safety Forum](https://aisafetyforum.au/). That'll be running in only two weeks, the start of November. That's co-organized with a bunch of other organizations: Gradient Institute, Sydney Knowledge Hub, and other organizations affiliated with the University of Sydney. So these kinds of collaborations, organizing events, I think is a key part of this.

## SLT that isn't developmental interpretability <a name="slt-not-devinterp"></a>

**Daniel Filan** (05:25):
Cool. I want to dial into a thing you just said there. Maybe I misinterpreted it, but you said that developmental interpretability is a subset of SLT for Alignment, and I had the impression that they were almost synonymous. So what do you see as... what's the complement?

**Jesse Hoogland** (05:52):
So I think the core idea of SLT is something like: the geometry of the loss landscape is key to understanding neural networks. There's just a lot of information embedded in this geometry that you can try to probe, and that tells you then something about the internal structure of models. So that's a starting point for developing interpretability tools, and maybe also evaluation procedures. Now, here are two different kinds of questions you can ask about that geometry. One is, I can ask: over the course of training, how does the local geometry around my model change as I proceed? Are there qualitative changes in this local geometry that correlate with changes in the algorithm I'm implementing? And some work that we've done so far shows that this seems to work. That's what I would call developmental interpretability: so, applying this lens of SLT to understanding the development process of neural networks. In particular, because SLT makes some pretty concrete predictions about the kinds of allowed changes you'll see over the course of development, at least in a Bayesian setting. And a lot of the work is in applying and seeing how much does this transfer, do these predictions from the Bayesian setting transfer over to the SGD setting? So developmental interpretability [is] ta[king] these tools from SLT, and run[ning] them over the course of development.

(07:20):
But another question you can ask is just: what is this local geometry like for a given snapshot? And maybe a question is: how does this geometry, which depends on the data you're evaluating your model on, change as you change that distribution? If I change the distribution I'm evaluating my model on and deploying my model on, do I see a qualitative change in the structure around my choice of weights? And we expect that that correlates with something like a change in the mode of computation on that data. So it's a sort of signal you can start to use to probe out-of-distribution generalization. Can you understand qualitatively the ways in which different structures in the model actually generalize? And that doesn't necessarily require taking this developmental approach. I think in many cases they are synonymous, but there are questions you can ask that don't really fit the developmental frame.

**Daniel Filan** (08:18):
Okay. So if I think about work by Timaeus that I'm familiar with, it seems like it mostly fits within the developmental interpretability side. Are there things on this "changing the dataset and seeing how the geometry changes"? Is there work there that I could read?

**Jesse Hoogland** (08:34):
Work that's coming at some point or that I should say we're starting on.

**Daniel Filan** (08:39):
All right.

**Jesse Hoogland** (08:40):
So one thing we've noticed is that SLT has this measure of model complexity called "local learning coefficient", which you can see as effective dimensionality, although in reality it's a richer metric than just effective dimensionality. This is something you can measure, and it depends on the data set you evaluate it on. So what we've noticed is: it seems to correlate with something like memorization. So in [very recent work](https://arxiv.org/abs/2410.02984), we looked at a learning coefficient that tells you about how much complexity is there in an attention head? What you find is that the learning coefficient correlates with the number of different N-grams or multigrams that a head has learned.

**Daniel Filan** (09:21):
Okay. Where the learning coefficient is higher if you've memorized more things?

**Jesse Hoogland** (09:25):
If you've memorized more things. There are other things like this: work by [Nina Panickssery](https://ninapanickssery.com/) and [Dmitry Vaintrob](https://math.berkeley.edu/~vaintrob/) looked at a grokking setting, and they looked at the size of the task that you're trying to memorize and how that changes the learning coefficient. There's a correlation there that is very clean. There's work on sparse parity tasks where you're looking at the number of different sparse parity tasks that your model is trying to memorize, and there's a correlation there. So there's starting to be, across a wide range of different things, some idea of we can measure how much a model is memorizing on the data set.

**Daniel Filan** (10:03):
So the work by Nina and Dmitry and the sparse parity task: what are the names of those papers?

**Jesse Hoogland** (10:12):
That's a [LessWrong post](https://www.lesswrong.com/posts/4v3hMuKfsGatLXPgt/investigating-the-learning-coefficient-of-modular-addition), the work by Nina and Dmitry, and the sparse parity stuff is coming out, so that's not been published.

**Daniel Filan** (10:20):
Okay. Just so that people can look it up, so that I can look it up, what's the name of the Nina and Dmitry post?

**Jesse Hoogland** (10:28):
"Exploration of the Learning Coefficient for Melbourne Hackathon"? There's Hackathon in the name.

**Daniel Filan** (10:36):
That's hopefully enough things to Google it now. Gotcha.

## The refined local learning coefficient <a name="refined-llc"></a>

**Daniel Filan** (10:41):
Cool. So I now want pivot to work that, as of recording, you've recently put out there on the "restricted learning coefficient". Do I have that name right?

**Jesse Hoogland** (10:51):
Yeah, "refined", "restricted". We've gone through some name changes.

**Daniel Filan** (10:56):
"Refined", sorry.

**Jesse Hoogland** (10:57):
The whole naming is kind of unfortunate, but it goes back, and it predates us, but it's... we'll call it the "refined LLC".

**Daniel Filan** (11:05):
Okay. So what is the refined LLC?

**Jesse Hoogland** (11:07):
So we have this measure of model complexity that tells you how much structure is there in the model as a whole, and how does that depend on the data. So two immediate refinements you can come up with on top of this learning coefficient from the theory. One of them is you change the data set, so you don't evaluate on a pre-training data set but some other data set.

(11:29):
The other thing you can do to refine it is to freeze some weights and measure the learning coefficient of a subset of weights. And so that's what I described with these attention heads. You can measure now complexity, the amount of structure in a component of the model. And so that was the starting point for this paper. We looked at these refinements and applied them to very simple, two-layer attention-only language transformers.

(11:52):
And so what you find is, if you plot this over the course of training, different kinds of heads have distinct developmental signatures. So the induction heads look like one thing. The heads memorizing n-grams and skip n-grams, which we call "multigram heads", memorize one thing. Previous token heads look like one thing. There's a current token head that looks like one thing, and you can automatically cluster them based on these developmental signatures. And I think you can do that using a bunch of other techniques, but that's at least a starting point.

(12:21):
That's one observation. What you now notice is that if you incorporate this data refinement, you can start to say something about what different heads are specialized to. So not just that different heads are different, but the ways in which they're different, what kinds of data sets they might be more important on. And so induction, when you evaluate it on a code-heavy data set, jumps up. So now the induction heads relatively experienced an increase in complexity, and moreover, they split apart. So the induction heads previously look like they're sort of doing the same thing. Evaluate it on code, and now there's a separation where one of them seems to be more important for code. Under additional analysis, you find that indeed this thing seems to be specialized to syntax, tokens, punctuation, that kind of thing.

(13:09):
So you've got tools that are starting to probe, when are our two different structures actually different? What is it specialized to? And this feeds into the discovery of a new kind of circuit, which we call the "multigram circuit".

(13:23):
These two-layer attention-only transformers are inspired by [work that Anthropic had done](https://transformer-circuits.pub/2021/framework/index.html) when they discovered this induction circuit mechanism. What we find is in these same models, models develop something sophisticated, another kind of circuit - so coordination between two layers - that seems to be necessary for things like nested parenthesis matching. You open a bracket, you open a quotation mark, you have to close the quotation mark before you close the parenthesis. Refined learning coefficients are part of a toolkit that we're developing. It's not just tools derived from SLT, but [also others] that are showing the formation of this multigram circuit, that help us discover a new circuit that appears to be every bit as fundamental as the induction circuit.

## Finding the multigram circuit <a name="finding-the-multigram-circuit"></a>

**Daniel Filan** (14:05):
Yeah. Can you tell me what actually happened? What tool you applied to what thing, what metric you noticed, and what's the story there and how the refined LLC played into it, if it was the refined LLC?

**Jesse Hoogland** (14:20):
Let me give you the high-level story. We still don't fully understand this model, so there's still work to be done. But what seems to happen first is heads individually memorize n-grams and skip n-grams, become multigram heads, and this seems to go faster in the first layer than in the second layer.

(14:39):
What you notice is that the learning coefficient, a particular kind of refined learning coefficient, where you're measuring how similar performance is to some baseline one-layer model, which you treat as a reference for what n-gram behavior is like... You measure performance relative to this baseline. You notice that the resulting learning coefficient, refined learning coefficient, peaks after the second stage of development.

(15:09):
At that point, this starts to decrease for the first layer, but it's still increasing for the second layer. So heuristically, the model seems to be losing information about multigram prediction in the first layer. You can verify this in a handful of cases where you actually notice that there's a migration, for example, of a multigram from layer one to layer two.

(15:31):
But you also notice that now suddenly the tokens that different heads seem to be involved in are changing, and now there's coordination. So one of the layer one heads seems to be very involved in the same kinds of tokens than a different layer two head is doing. You can actually verify, by certain kinds of ablations and path ablations where you only ablate the outputs of these heads into the input of this head in the second layer, that it needs coordination. So the model's actually passing information forward now to second layer heads in order to predict nested pattern matching, where that wasn't the case before.

(16:10):
So we're still using ablations to verify that there's coordination going on here. We're looking at certain kinds of composition scores to verify that layer one is feeding into layer two. They're reading and writing from the same subspace. There's other analyses where we're looking at: if you ablate this head, which tokens are maximally affected across the data set? And actually looking at a bunch of those examples. So all of that analysis is going into identifying multigram circuits, but this observation that information might be migrating from layer one to layer two is something that can set you off, and I think that's something that we'll observe more generally as we scale up to larger models, these kinds of signals.

**Daniel Filan** (16:54):
Gotcha. So if people are interested in reading that paper, what was the name of that paper again?

**Jesse Hoogland** (17:04):
[Differentiation and Specialization in Attention Heads with the Refined Local Learning Coefficient](https://arxiv.org/abs/2410.02984).

**Daniel Filan** (17:09):
Great. Well, thanks very much for coming here and chatting with me.

**Jesse Hoogland** (17:13):
Thank you, Daniel. See you soon.

**Daniel Filan** (17:15):
See you around.

(17:15):
This episode was edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode, or to learn how to support the podcast yourself, you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me, at <feedback@axrp.net>.
