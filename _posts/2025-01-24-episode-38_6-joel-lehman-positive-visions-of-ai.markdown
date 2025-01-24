---
layout: post
title: "38.6 - Joel Lehman on Positive Visions of AI"
date: 2025-01-24 14:58 -0800
categories: episode
---

[YouTube link](https://youtu.be/m446AjcGVqs)

Typically this podcast talks about how to avert destruction from AI. But what would it take to ensure AI promotes human flourishing as well as it can? Is alignment to individuals enough, and if not, where do we go form here? In this episode, I talk with Joel Lehman about these questions.

Topics we discuss:
 - [Why aligned AI might not be enough](#why-aligned-ai-might-not-be-enough)
 - [Positive visions of AI](#positive-visions-of-ai)
 - [Improving recommendation systems](#improving-rec-systems)

**Daniel Filan** (00:09):
Hello, everyone. This is one of a series of short interviews that I've been conducting at the Bay Area [Alignment Workshop](https://www.alignment-workshop.com/), which is run by [FAR.AI](https://far.ai/). Links to what we're discussing, as usual, are in the description. A transcript is, as usual, available at [axrp.net](https://axrp.net/). And as usual, if you want to support the podcast, you can do so at [patreon.com/axrpodcast](https://www.patreon.com/axrpodcast). Well, let's continue to the interview.

(00:29):
All right, and today I'm going to be chatting with Joel Lehman.

**Joel Lehman** (00:32):
Hello, nice to be here.

**Daniel Filan** (00:33):
Yeah, so first of all, for people who aren't familiar with you, who are you, and what do you do?

**Joel Lehman** (00:40):
I'm a machine learning researcher. I've been in the field for 10, 12 years, co-author of a book called [Why Greatness Cannot Be Planned](https://www.amazon.com/Why-Greatness-Cannot-Planned-Objective/dp/3319155237), which is about open-endedness, and I've been a researcher at Uber AI and most recently OpenAI, and currently an independent researcher.

**Daniel Filan** (00:56):
Gotcha. Before we talk about what you do or your current research: we're currently at this Alignment Workshop being run by FAR.AI. How are you finding the workshop?

**Joel Lehman** (01:06):
It's great. Yeah, lots of nice people, lots of interesting ideas, and good to see old friends.

## Why aligned AI might not be enough <a name="why-aligned-ai-might-not-be-enough"></a>

**Daniel Filan** (01:12):
What are you currently thinking about, working on?

**Joel Lehman** (01:15):
I'm really interested in positive visions for how AI could go well, set against a default assumption that AI as currently deployed might exacerbate existing societal problems. The rough intuition is: capitalism is great, but also the downside of capitalism is we get what we want, but not what we need, and maybe AI will get really good at giving us what we want, but at the expense of epistemic sense-making and meaningful interactions with people and political destabilization, that kind of thing.

**Daniel Filan** (02:00):
It seems to me that understanding the world, having meaningful interactions with people, those are also things people want. Political destabilization is, I guess, a little bit more tricky. Do you have a sense for why AI might erode those things by default?

**Joel Lehman** (02:17):
Yeah, I think it's interesting. We do have a sense of what we want in the grander scale, what a meaningful life might look to us. At the same time, it seems like if you look at the past maybe decade or two, in general, it feels almost like a death by 1,000 paper cuts, where we get convenience at the expense of something else. One way of thinking about it is some part of society is reward-hacking the other half of society.

(02:45):
Facebook had a really beautiful motivation... Or social media in general has a very beautiful motivation, but in practice, it seems like it maybe has made us less happy and less connected. We find ourselves addicted to it and to our cell phone and all the attention economy sorts of things where again, we might be in touch with what our better angel would do, and yet, it's really hard to have the discipline and will power to resist the optimization power against us.

**Daniel Filan** (03:11):
So, is the vision something like: we get AI, AI is just a lot better, it's really good at optimizing for stuff, but we have this instant gratification thing that the AI optimizes really hard for at the expense of higher-level things. Is that roughly right?

**Joel Lehman** (03:28):
Yeah, I think you can look at the different AI products that are coming out, and some of them might be beneficial to our well-being and to our greater aspirations, but some non-trivial chunk will be seemingly... It's hard to know how it'll play out, but take [Replika](https://replika.ai/) for example. A really interesting technology, but there's a lot of fears that if you optimize for market demand, which might be companions that say all the things that make you feel good, but don't actually help you connect with the outside world or be social or something...

## Positive visions of AI <a name="positive-visions-of-ai"></a>

**Daniel Filan** (04:05):
Right, right. Yeah, up against that, I guess you've said that you're thinking about positive visions of AI. What kinds of positive visions?

**Joel Lehman** (04:17):
So I wrote an essay with [Amanda Ngo](https://www.amandango.me/) pitching that part of what we need is just the sense of the world we like to live in and what the alternatives to some of the technologies that are coming out are. If social media has had some knock-on effects that are a little bit bad for our well-being and for our societal infrastructure, what would a positive version of that be? It's putting aside the market dynamics which make it maybe difficult to actually realize that in practice, but at least having a sense of what something we might want would be, and what are the technical problems that you might need to solve to try to even enable that.

**Daniel Filan** (04:58):
Before we go into that, what's the name of that essay so people can Google it?

**Joel Lehman** (05:02):
That is a great question. I think it's called ["We Need Positive Visions of AI Grounded in Wellbeing"](https://thegradientpub.substack.com/p/beneficial-ai-wellbeing-lehman-ngo).

**Daniel Filan** (05:10):
Okay, hopefully, that's enough for people to find it, and there'll be a link in the description. Do you have some of the positive vision yet, or is the essay mostly "it would be nice if we had it"?

**Joel Lehman** (05:24):
Partly it's "it'd be nice if we had it", and partly there's at least some chunks of it that we begin to map out what it might look like and what are some of the technical challenges to realizing it. I had a research paper out maybe a year ago called ["Machine Love"](https://arxiv.org/abs/2302.09248), and it was about trying to take principles from positive psychology and psychotherapy and imagining what it'd be like to bash those up against the formalisms of machine learning, and is there a productive intersection? It tries to get into the ideas of going beyond just revealed preferences or Boltzmann rationality or whatever kind of convenient measure of human preferences we have to something that's more in touch with what we know from the humanities about what human flourishing broadly looks like.

**Daniel Filan** (06:18):
So we want to get a better read on human flourishing, on things other than very simple Boltzmann rationality. I'm wondering if you have takes about how we... Or if someone's just a machine learning person, how do they start going about that, do you think?

**Joel Lehman** (06:38):
I think it's a great question. I think that there are ways that we could just stretch the current formalisms that would be useful. There's really [interesting work](https://arxiv.org/abs/2405.17713) by [Micah Carroll](https://micahcarroll.github.io/) at UC Berkeley, on the difficulties of preferences that change. So that's going a little bit beyond the idea of "your preferences are fixed and immutable". So keeping stretching things outwards from there and dealing more with the messiness of human psychology, that we're not simple utility maximizers and that we can actually make decisions against our best interests, and how do you start to grapple with that?

(07:23):
Another thing would be just to be in touch with the broader literature from the humanities about that and trying to find if there are interesting inroads from the philosophy of flourishing to the kinds of formalisms we use. I think cooperative inverse reinforcement learning feels like, again, stretching the formalism to encompass more of the messiness in there, and I feel like things in that direction, I think, I'd be really excited about.

**Daniel Filan** (07:54):
Okay, so just engaging more with difficulties of what's going on with humans, funky stuff, just trying to grapple with it and just move a little bit less from this very theoretical place?

**Joel Lehman** (08:08):
Yeah, reinforcement learning from human feedback is really cool, and it's really based on pairwise comparisons, so how could we go beyond that? I don't have great ideas there. It seems really difficult, but I think it's exciting to think about what other things could exist there.

## Improving recommendation systems <a name="improving-rec-systems"></a>

**Daniel Filan** (08:27):
Yeah, so you mentioned Micah Carroll's work. He's done some work on recommendation systems, and my understanding is that you've thought a little bit about that. Can you tell us a little bit about that?

**Joel Lehman** (08:38):
I am really interested in the machine learning systems that underlie the big levers of society, things that drive social media or platforms like YouTube or TikTok, systems that really impact us at scale in interesting and hard-to-foresee ways. It's easy to quantify engagement or how you might rate something and more difficult to get to the question of "what's an experience that years from now I'll look back and really be grateful for?"

(09:15):
So, a project in that direction is working with [Goodreads](https://en.wikipedia.org/wiki/Goodreads). I just really like books. Goodreads has a data set of books and text reviews and ratings, and what's really cool about it, just as a data set, is that it encompasses years sometimes of books that a person has read, and that even though a lot of books you read don't really impact you that much, there is the potential for transformative experiences in there, where reading a book could actually change the course of your life in some way. And that there are these text reviews that actually could detail how a book has affected you.

(09:53):
You could look for phrases like "has this book changed my life?" The hope is that building on a data set, you could come up with a recommendation system that could be tailored to the history of what you've read and maybe could contribute to your development, so to the change of your preferences, the change of your worldview. I've done some initial work in that direction. It's not easy to create such a system, but I think for those kinds of data sets, it'd be really exciting to see people try to grapple with the challenges of changing preferences and deeper things that a person might want.

**Daniel Filan** (10:27):
Yeah, I guess it's an interesting problem, because at least in the abstract, not all changes are good. I can imagine somebody saying like, "Oh, yeah, this book changed me for good," and I read that and I'm like, "No thanks." I'm wondering if you have thoughts about how to disentangle the good types of changes from the bad types.

**Joel Lehman** (10:51):
Yeah, that's a huge challenge, and in some initial work, if you just stack rank books by the percentage of reviews that people say they changed their lives, you get things like [Tony Robbins](https://en.wikipedia.org/wiki/Tony_Robbins) at the top and self-help books, which maybe really do change a person's life, but also it could be just hacking your sense of this excitement about a new idea that actually doesn't really change your life in a good way. I think-

**Daniel Filan** (11:15):
It's tricky. One thing that I think goes on there is if a book has changed your life, there's a pretty good chance that it's a self-help book, but if you read a self-help book, there's a good chance that it will not, in fact, change your life, so it's like both things are going on, I guess.

**Joel Lehman** (11:30):
Yeah, I think there are really deep philosophical questions about how to know if a change is good or bad for you, and questions of paternalism that step in if the machine learning system's guiding you in a way that you wouldn't necessarily want but is changing you. And one first approximation of something that might be okay is if it could project out possible futures for you: "If you read this book, it might impact you in this way." Or giving you the affordance to say, "I really would love to be able to someday appreciate [Ulysses](https://en.wikipedia.org/wiki/Ulysses_(novel)). It's a famously challenging book. What are the things that I would need to do to be able to appreciate that?"

(12:14):
Trying to rely a little bit on human autonomy. But yeah, there's messy societal questions there because you could imagine also... I don't think this is really going to happen in book recommendation, but as a microcosm, if everyone got hooked on to [Ayn Rand](https://en.wikipedia.org/wiki/Ayn_Rand)sville, that's maybe the easiest path to have a life-changing experience or something. You read Ayn Rand, it's really changed my worldview or something. But the second-order consequence is that a lot of people are getting stuck in a philosophical dead end or local optima or something. I don't know. I think there's definitely challenging questions, but I think there are also probably principled ways to navigate that space.

**Daniel Filan** (12:59):
Yeah, I guess I feel like the thing you ideally would want to do is have some sort of conversation with your future self where you could be like, "Oh, yeah, this seems like it is bad, but I don't know, maybe you've got some really good reason," or, "Ah, this seems good, but let me just check." I don't know. It seems pretty hard, but ...

**Joel Lehman** (13:18):
Yeah, I think that's great. I think there's also independent reasons it's nice to talk to your future self, and I think there's some research that shows that actually can motivate you to change to be the kind of person you want to be. There's also the weird thing that: the stepwise changes that you go through in your worldview - you might, when you get to the end of it, approve of those changes, but at the beginning, you look at those and you're like, "I didn't want to become that kind of person," or something.

(13:47):
As a kid, I wanted to be an astronaut, and maybe kid me talking to present me would be like, "Wow, your life is boring. You could have been on the moon." I don't mean to always bring up the philosophical annoyances, but it's interesting that it's hard to... Micah's [paper](https://arxiv.org/abs/2405.17713) talks a bit about this difficulty with "by whose preferences do you judge whether your future preferences are good?"

**Daniel Filan** (14:13):
Right, right. Yeah, I think it's challenging, and I think that's food for thought for our listeners, so thanks very much for chatting with me.

**Joel Lehman** (14:23):
Thanks for having me. It's been great.

**Daniel Filan** (14:25):
This episode was edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with patrons such as Alexey Malafeev. To read a transcript of the episode, or to learn how to support the podcasts yourself, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
