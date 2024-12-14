---
layout: post
title: "38.4 - Peter Barnett on Technical Governance at MIRI"
date: 2024-12-13 21:55 -0800
categories: episode
---

[YouTube link](https://youtu.be/fvMVf7vDRAc)

The Machine Intelligence Research Institute has recently shifted its focus to "technical governance". But what is that actually, and what are they doing? In this episode, I chat with Peter Barnett about his team's work on studying what evaluations can and cannot do, as well as verifying international agreements on AI development.

Topics we discuss:
 - [MIRI's technical governance team](#miri-tgt)
 - [Misuse evaluations](#misuse-evals)
 - [Misalignment evaluations](#misalignment-evals)
 - [Verifying international agreements](#verifying-intl-agreements)
 - [Difficulties in compute monitoring](#difficulties-in-compute-monitoring)
 - [More on MIRI's technical governance team](#more-on-miri-tgt)

**Daniel Filan** (00:09):
Hello everyone. This is one of a series of short interviews that I've been conducting at the Bay Area [Alignment Workshop](https://www.alignment-workshop.com/), which is run by [FAR.AI](https://far.ai/). Links to what we're discussing, as usual, will be in the description. A transcript is, as usual, available at [axrp.net](https://axrp.net/). And as usual, if you want to support the podcast, you can do so at [patreon.com/axrpodcast](https://www.patreon.com/axrpodcast). Well, let's continue to the interview.

(00:28):
Right now I'm speaking with Peter Barnett. Hey, Peter.

**Peter Barnett** (00:31):
Hello.

**Daniel Filan** (00:32):
So for people who aren't familiar with you, can you say a little bit about yourself, what you do?

**Peter Barnett** (00:37):
Cool. Yeah. I'm Peter. I work on the [technical governance team](https://techgov.intelligence.org/) at [MIRI](https://intelligence.org/). Previously I did technical alignment stuff at MIRI, but have since transitioned to doing governance stuff, actually, because alignment was too hard. [I'm] trying to work out what MIRI should be pushing for in terms of governance and trying to influence policy, this kind of thing.

**Daniel Filan** (01:00):
Cool. And before we get into that: we're currently at this alignment workshop being run by FAR.AI. How are you finding it?

**Peter Barnett** (01:10):
I'm finding it pretty good, yeah. I think I've found that: I'm not sure I've learned object-level things about alignment, but I think there's a lot of talking with people and being like, "ah, these people have these takes. Maybe we could coordinate in various ways" or something.

## MIRI's technical governance team <a name="miri-tgt"></a>

**Daniel Filan** (01:25):
So, governance: it makes sense what that is, right? It's how people should govern AI. What is technical governance?

**Peter Barnett** (01:31):
This is a great question. I think we've recently tried to clarify this a little bit, what do we mean? There is a more technical side of this, where there's some specific evaluations you should run, or maybe your regulation should be based on the technical aspect of the models rather than some non-technical aspect. And so we are more focused on that. I think it's maybe easier to explain this in terms of what it's not: we're not trying to do institution design, we're not trying to do economics, we are not trying to do law. We're trying to do the stuff that maybe occasionally there's more math and coding.

**Daniel Filan** (02:07):
Okay, fair enough. So you're at this technical governance team at MIRI. What does your team do?

**Peter Barnett** (02:16):
I think we've got a couple of main thrusts at the moment. We do technical governance. Some specific projects that we have right now are: [we're] trying to suss out and publish our views on [evaluations](https://techgov.intelligence.org/research/what-ai-evaluations-for-preventing-catastrophic-risks-can-and-cannot-do) and how they should [play into regulation](https://arxiv.org/abs/2411.12820). AI evaluations are a hot new thing. They're one of the few things that people can do, and it seems pretty likely that these are going to pretty heavily affect how regulation goes. And so we're trying to be like, "ah, yes, evals work for these things. We don't really think they work for these things. If you are writing regulation, maybe you should include these aspects".

(02:52):
And then we've got a bunch of other stuff on maybe some bigger picture things, where: maybe at some point we need some pretty heavy international agreements so that no one builds dangerous AI. Hopefully, we can eventually all agree on that. And for these, we're probably going to need, for example, verification mechanisms. And so there's some people on the team working on [how to verify aspects of international agreements](https://techgov.intelligence.org/research/mechanisms-to-verify-international-agreements-about-ai-development).

**Daniel Filan** (03:17):
So it sounds like it focuses on two things: evaluations and what they can do; and trying to come up with verification schemes for various things.

**Peter Barnett** (03:24):
Yeah, those are two of the main projects right now. I think in the future... we do some amount of interfacing with government agencies and stuff, trying to give our takes on... They come up with some new policies and we're like, "here's what we think, why this is good, why this is bad, and maybe how things should be changed". But those are the two main projects.

## Misuse evaluations <a name="misuse-evals"></a>

**Daniel Filan** (03:43):
Gotcha. So maybe let's first talk about evaluations. Lots of people working on them, lots of people doing them: what do you think they are good for and what do you think they aren't good for?

**Peter Barnett** (03:54):
I think many people are very bullish on evaluations, and I think they're good to do. You can learn things about models. There's at least some information to be gleaned here. People often do evaluations to assess misuse risk, for example, where you are worried that some bad actor is going to use your model to do some terrorism, do a biological weapons attack or a cyber attack, and the model will enable them to do this. And I think you can, if you try really, really hard - I think no one is really hitting this bar yet - but you could use evaluations to assess this use.

**Daniel Filan** (04:28):
What do you think people are missing?

**Peter Barnett** (04:30):
So I think the main thing here is just how hard you try. Basically the way you would gain assurance of safety here is you would have your evaluators use the model and you would assume they're really good at their job. And if they can't do the equivalent of a terrorist attack or something using the model, then you assume that if your evaluators can't, then the bad actors also couldn't.

(04:58):
And so in order for this to work, you need to be confident that your evaluators will be able to catch every single threat vector that malicious actors would. And here your bar is kind of "human skill" or something: you're like, "ah, yes, malicious actors are humans, and so we just need to be kind of the best humans at this". And if you can be confident of that (or at least close enough to confident of that), maybe you can do this.

**Daniel Filan** (05:24):
Okay. So to me, it actually seems kind of plausible that the people doing evaluations are close to state-of-the-art at eliciting behavior. I don't know, they spend all day on it. It's their job. It wouldn't strike me as that surprising if they were best in the world at it. I'm wondering, do you agree or do you think-

**Peter Barnett** (05:49):
Yeah, this seems plausible. I think it depends pretty strongly on who's doing it. And I guess also what affordances you give them. It seems plausible that the DeepMind or whatever teams are going to be better than some random high school kid or something, but maybe worse than North Korea's top team of bioterrorism people. And maybe these more nation-state level threats are going to be able to put way more resources into those, way more compute, or just have way more people doing this. You do have the advantage - if your models are closed, people don't have access to the weights - that your evaluators can fine-tune for stuff in a way that other people wouldn't be able to do, or have models that don't have various safety safeguards on them.

## Misalignment evaluations <a name="misalignment-evals"></a>

**Daniel Filan** (06:36):
Gotcha. So misuse evaluations were one kind of evaluation that you think is possible, although maybe people aren't working hard enough on it. And it seemed like you were about to contrast that with another kind of thing.

**Peter Barnett** (06:50):
Yeah, yeah. There's another whole class of risks, which I'm very concerned about and MIRI's very concerned about, which is these misalignment risks, risks from models that are pursuing their own goals. And these are risks from model autonomy rather than a human deliberately misusing the model. And in this case, it's not clear that your bar is human performance. You're trying to work out what's the optimal bar, what's the bar for the model if it was trying its best. And it's hard to know whether you're actually managing to fully elicit model performance with that.

**Daniel Filan** (07:26):
Right. So I guess there are various things you can try and do. You can try and catch the models, put them in a situation where they think they can get away with sandbagging their performance. Recently, I had [an episode](https://axrp.net/episode/2024/07/28/episode-34-ai-evaluations-beth-barnes.html) with [Beth Barnes](https://metr.org/team/beth-barnes/) of [METR](https://metr.org/), and I asked her about this. I forget what her answer was, but she said some things. So I'm wondering what you think of things people have thought about to try and get at this?

**Peter Barnett** (07:59):
Yeah, so I think for the specific issue of sandbagging - which is when the model could do the thing, but it just doesn't - maybe you should distinguish between "you were just bad at evaluations and so you didn't manage to elicit the model's full capabilities" versus "the model in some sense was trying not to perform well on the task". I think for the "just bad at evaluations" thing, the answer is just to be better at that, although I don't think they have great science of evaluations right now. But yeah, the more pernicious one is this sandbagging risk: the model is deliberately trying to not do well on the thing.

(08:33):
People often propose things like, oh, we could deliberately fine-tune the model for the task, and in some sense the model would need to be able to fight gradient descent in order to do this. Maybe you do RL fine-tuning or supervised fine-tuning for this. And I think this is pretty reasonable. I think when you're not relying on a ton of generalization, I think you can probably get the model to perform well on the specific tasks you fine-tune it for. I think things become much harder when it's hard to fine-tune. For example, if the model was capable of coming up with threat vectors that humans couldn't, you can't really fine-tune for that. It's hard to know "is the model capable of doing a thing a human couldn't do?" when you can't fine-tune for this behavior. I think current models are basically nowhere near this, although it seems plausible in the next few years.

**Daniel Filan** (09:26):
Yeah, I guess you could imagine trying to do some sort of RL thing of: here's a situation. Here's some mini-world that's functioning properly, and you're in a box in the mini-world, we're going to reinforcement-learn you to try and cause havoc. It's a little bit rough, I guess, because if you don't know how it would cause havoc, then you have to have a really sparse reward function.

**Peter Barnett** (09:47):
And I think in this case you may be at a classic reward hacking thing where the model is just like... if it knows that it can get away with underperforming, it will just never explore into the regions you want it to. So it's hard to reinforce that.

**Daniel Filan** (10:01):
Fair enough, fair enough. So it sounds like the TL;DR is just: evaluating for whether models themselves are going to cause risk is just hard because the models... maybe they're going to sandbag, maybe they have ways of causing risk that you can't think of. So there are things that exist currently. Do you have thoughts about: if we were going to try and get better at this, are there areas of research that you think could be usefully pushed on? Or do you think this is basically a hopeless endeavor?

**Peter Barnett** (10:35):
For this eval stuff?

**Daniel Filan** (10:36):
Yeah, for evaluations in this setting?

**Peter Barnett** (10:39):
Yeah. I mean, I'm fairly excited about the model organisms research direction where you basically train a model to be evil or to elicit the specific threats that you're worried about. For example, you train it to sandbag and then you attempt to use your techniques on this. I think current work is promising here, but there's always a pretty big question about how much evidence do your model organisms provide about the real misalignment threat models. I think currently they're fine, it's a good first attempt, but it doesn't really give me that much hope or give me that much assurance.

(11:13):
I think maybe there's some interp stuff you can do where you look inside the model and you're like "is it thinking about deception when it shouldn't be?" You can provide some evidence, but yeah, I think it's not clear that interp is good enough to do this or will be good enough to do this.

## Verifying international agreements <a name="verifying-intl-agreements"></a>

**Daniel Filan** (11:29):
Fair enough. I'm kind of interested to talk about [the verification stuff](https://techgov.intelligence.org/research/mechanisms-to-verify-international-agreements-about-ai-development), if that's okay.

**Peter Barnett** (11:36):
Yeah, I've done some stuff on this, but this isn't my main focus. But I can say some stuff.

**Daniel Filan** (11:45):
I guess as most people in the field of AI safety, I've heard people talk about evaluations. But what kinds of properties do you think you can verify and how is this going to work roughly?

**Peter Barnett** (12:00):
So I think specifically the case we're imagining is: you have a big international agreement between, for example, the US and China and they're like, "we're both scared that the other one's going to build dangerous models. Sounds bad, and we'll only stop if they stop". And so you need some assurance that you've both stopped. Basically pretty equivalent to a bunch of these nuclear treaties that allowed nuclear deproliferation. And so things you can measure potentially are just like, "don't train models above a certain FLOP threshold". Or you have mandatory evals, you're like "every n FLOPs or every n percent of training compute, you need to run these evals and then report them publicly". And then people can be confident that the other side isn't racing too far ahead and so they're not losing the race here.

**Daniel Filan** (12:51):
But when you say mandatory evals, how do you know someone hasn't done some training and they just haven't done the eval? Is the idea that you just know how much computation they've done?

**Peter Barnett** (13:07):
Yeah. For example, you could have some mechanisms on the chips that count the number of FLOPs that have been used and maybe you require them to do some FLOP accounting, how much compute is being used? And [they] can be like, "oh, yep, we spent all the compute on this model, we can verify with some cryptographic scheme". And you're like, "cool, you spent all your compute on that model, now you have to evaluate that model and we're going to look at you while you do that to make sure you're actually performing the evaluations".

## Difficulties in compute monitoring <a name="difficulties-in-compute-monitoring"></a>

**Daniel Filan** (13:30):
I guess one difficult thing is: so in the US a bunch of people own computers. There are just tons of them, and the total amount of floating point computations done in the US is probably huge. But I guess right now most of the floating point operations are not being used to train AIs, right? It's not obvious to me how you know whether the floating point operations were used to train AIs or not. So my laptop doesn't have a ton, but if I can get some network thing going, then maybe I could do some decentralized AI training run.

**Peter Barnett** (14:08):
Yeah, it's pretty rough. I think a lot of people have some hope - which I have a little bit of hope in - that: currently you use really specific chips for training the models. Training on a distributed cluster of your laptops... you're not going to train any dangerous model using that. And so if the hardware's specialized enough, then maybe you can enforce this. You're like, "NVIDIA, when you produce H100s or whatever, you need to chuck an extra module on there that does some FLOP accounting" or something. But it's pretty rough, because there is in fact a bunch of compute out there currently, and so you need to account for that, or for some reason believe that that's not sufficient yet to build dangerous models.

**Daniel Filan** (14:49):
I guess it's also difficult to the degree that you think algorithmic improvements are a thing: then presumably at some point people won't need these super specialized chips, right?

**Peter Barnett** (15:02):
Yeah. I think algorithmic improvements really break this. I think the amount of compute required to reach a level of performance halves every eight months.

**Daniel Filan** (15:19):
Right, right. I think that's [the Epoch thing](https://epoch.ai/blog/revisiting-algorithmic-progress). People can listen to [my recent episode](https://axrp.net/episode/2024/10/04/episode-37-jaime-sevilla-forecasting-ai.html) with [Jaime Sevilla](https://jaimesevilla.me/) for that.

**Peter Barnett** (15:24):
Yeah, great paper, big fan. I'm glad that somebody did any work on this at all. Yeah, I think the algorithmic improvements make all this compute governance stuff way harder. I think it seems kind of plausible at some point that we're going to need to restrict algorithmic research, which is a pretty brutal ask. I know this does entail some pretty heavy curtailings of academic freedom, which is kind of rough, but maybe this is what's required.

**Daniel Filan** (15:51):
So there's some difference between computation I do in my everyday life and computation that's used to train really big models, because stuff I do in my everyday life does not result in really big models. So I wonder if instead of curtailing algorithmic research, you could just check if the computation is being used for a certain thing versus some other thing.

**Peter Barnett** (16:17):
Yeah. I guess you use pretty specific chips, which is one way to look at this. Are you doing the kind of computation which is done on an H100? But there's plausibly other stuff... You can use H100s for other stuff, like some physics simulation or biology protein design, blah, blah, blah. And I think there's probably some specific signatures or workflows that correspond to this, but I think it's probably easier just to do it at the hardware level rather than on the "what's the specific computation being run".

## More on MIRI's technical governance team <a name="more-on-miri-tgt"></a>

**Daniel Filan** (16:44):
Okay. So I think I'd like to just talk a little bit about the technical governance team itself. So you're thinking about technical governance. How long has this team existed?

**Peter Barnett** (16:54):
The team existed... I think we started February, March this year.

**Daniel Filan** (16:57):
Okay, so pretty new then?

**Peter Barnett** (16:59):
Yeah, pretty new.

**Daniel Filan** (17:00):
And can you say a bit about the impetus for making it happen?

**Peter Barnett** (17:04):
So I think I can tell my story and my impression of the stuff that happened at MIRI. So I think from the MIRI angle, people became much less optimistic about alignment research working out, especially on the relevant time scales. And so this led to some strategic change for MIRI to pivot much more on governance and communications. And this corresponded with also a change in my views. I was working on alignment and then became much more pessimistic about this as well, being like "oh wow, it starts moving really quickly, and it seems like the only way to actually have enough time to do the necessary alignment research - which it doesn't seem like we have a massive handle on right now - is governance stuff". And so I was pretty excited to attempt to work on this under MIRI, which gives us some... MIRI's got some "tell it how it is, maybe say things which are slightly outside the Overton window" vibe, which I think hopefully plays a valuable part in this ecosystem.

**Daniel Filan** (18:11):
Okay. And how big is the team?

**Peter Barnett** (18:14):
So the team is reasonably small. It's currently just three people.

**Daniel Filan** (18:17):
If somebody's listening to this and they're interested in this kind of work, are you interested in expanding?

**Peter Barnett** (18:22):
Yeah, we'd be excited to expand for some... We're not actively hiring, but I think... we would definitely make hires if people seemed really promising.

**Daniel Filan** (18:32):
Sure. And if people want to learn more, I've heard that as of recording you might not have a website, but I've heard that you might soon, and therefore maybe by the time this is released.

**Peter Barnett** (18:42):
Yeah, yeah. We should have [a website](https://techgov.intelligence.org/) out hopefully in the next one to two weeks, maybe three. We'll see how timelines work out.

**Daniel Filan** (18:48):
Quite likely before I get around to releasing this.

**Peter Barnett** (18:52):
We haven't decided on the URL. It might be [techgov.intelligence.org](https://techgov.intelligence.org/) or intelligence.org/techgov, something like that.

**Daniel Filan** (18:59):
All right. And people can find the link in the description.

**Peter Barnett** (19:03):
Yeah, sounds great.

**Daniel Filan** (19:04):
Great. Well, thanks very much for chatting with me.

**Peter Barnett** (19:06):
Yeah, thanks. It was great.

**Daniel Filan** (19:07):
This episode was edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with patrons such as Alexey Malafeev. To read a transcript of the episode, or to learn how to support the podcasts yourself, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
