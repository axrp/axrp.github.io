---
layout: post
title: "38.8 - David Duvenaud on Sabotage Evaluations and the Post-AGI Future"
date: 2025-02-28 17:00 -0800
categories: episode
---

[YouTube link](https://youtu.be/OKekclKMgz0)

In this episode, I chat with David Duvenaud about two topics he's been thinking about: firstly, a paper he wrote about evaluating whether or not frontier models can sabotage human decision-making or monitoring of the same models; and secondly, the difficult situation humans find themselves in in a post-AGI future, even if AI is aligned with human intentions.

Topics we discuss:
 - [The difficulty of sabotage evaluations](#difficulty-sabotage-evals)
 - [Types of sabotage evaluation](#types-sabotage-eval)
 - [The state of sabotage evaluations](#state-of-sabotage-evals)
 - [What happens after AGI](#what-happens-after-agi)

**Daniel Filan** (00:09):
Hello, everyone. This is one of a series of short interviews that I've been conducting at the Bay Area [Alignment Workshop](https://www.alignment-workshop.com/), which is run by [FAR.AI](https://far.ai/). Links to what we're discussing, as usual, are in the description. A transcript is, as usual, available at [axrp.net](https://axrp.net/). And as usual, if you want to support the podcast, you can do so at [patreon.com/axrpodcast](https://www.patreon.com/axrpodcast). Well, let's continue to the interview.

(00:28):
All right. I'm chatting with David Duvenaud. Hello, David.

**David Duvenaud** (00:32):
Hi, Daniel. Thank you. It's an honor to be here.

**Daniel Filan** (00:34):
So for people who aren't familiar with you, who are you and what do you do?

**David Duvenaud** (00:40):
Sure. So for the last little while, since 2016, I've been a professor at the University of Toronto, doing probabilistic deep learning. Maybe I'm best known for [neural ODEs](https://arxiv.org/abs/1806.07366), or some early work on graph neural networks. And then around 2017, me and my colleague, [Roger Grosse](https://www.cs.toronto.edu/~rgrosse/) decided we were finally going to actually try to contribute to AI safety, and both took on PhD students. And gradually, this journey of "let's try to work more on AGI safety" led to me just doing a year and a half-long sabbatical at Anthropic that I just finished this week.

**Daniel Filan** (01:12):
Before we talk about your work, we're currently at this alignment workshop being run by FAR.AI. I'm wondering, how are you finding the workshop?

**David Duvenaud** (01:19):
I mean, it's wonderful. I've been to a bunch of these over the years. And now this is the first one where I feel like I'm sitting on a serious body of technical work to talk about, which is nice. Previously, I've been this AGI-interested mainstream academic that I think the community was hoping I would join them and lend legitimacy. And so I finally actually was able to take the plunge in a serious way.

## The difficulty of sabotage evaluations <a name="difficulty-sabotage-evals"></a>

**Daniel Filan** (01:42):
Yeah. Now that you've taken the plunge, I understand that you've recently put out a paper. Can you remind me of the title of it?

**David Duvenaud** (01:48):
Sure. It's called ["Sabotage Evaluations for Frontier Models"](https://arxiv.org/abs/2410.21514).

**Daniel Filan** (01:51):
Can you tell me a little bit about what this paper does?

**David Duvenaud** (01:54):
Sure. So this paper covers the work of most of this team within Anthropic called Alignment Evals that I was running for most of the time that I was there. It's a collaboration also: [Holden Karnofsky](https://en.wikipedia.org/wiki/Holden_Karnofsky) and Buck Shlegeris also helped us flesh out this series of evaluations that try to help make the case that a model could not be secretly sabotaging human decision-making or monitoring or evaluation of other capabilities.

**Daniel Filan** (02:22):
So how would you try and establish that? It seems like a rough property to try and get a handle on.

**David Duvenaud** (02:28):
It's really rough. And it was kind of stressful, and we spent a lot of time thrashing conceptually about exactly what sort of case we needed to make and how we were going to make it. The place we started with was from more narrow evals, to say, "Okay, can the model coordinate with other copies of itself? Or do something at a very low frequency so that it could do it rarely enough that we wouldn't notice?" Stuff like that.

(02:54):
And this was something that we could certainly make evals for, but the problems came (a) from not knowing how hard to make them. So if we're trying to see whether machines can make a Schelling point between themselves without communicating: this is basically impossible if you have no shared context, and it's basically trivial if you have a lot of shared context. Same for other things, like convincing someone of a false statement, something like that.

(03:19):
And then the second reason that this was really unsatisfying was trying to make safety cases based on these, and trying to make the case "Okay, the model needs these four capabilities in order to pull off some sort of heist or sabotage or mess up something important. We've measured each of these, and that they're each at these different levels." And then to try to combine those levels into a strong case that the model could never do something dangerous is very rough. It's like saying you need level four lockpicking and level five social engineering in order to rob a bank. Well, it probably doesn't work that way. We don't want to have to make strong assumptions about what route the attacks are going to take.

(04:07):
But instead... So eventually the direction that we moved more towards was these more end-to-end holistic evaluations that try to measure more of an entire attack. And there's a few directions in which they're more realistic. One is: we try to include oversight in the eval. So it's not just "can you write a cyber attack?" but "can you write one in the context of you're supposed to be writing helpful code, and there's also going to be someone who's reviewing your code to see if anything looks suspicious?"

(04:36):
So this is much harder. And it's harder in a bunch of different ways, where you have to be able to estimate what someone else is going to think of your attack. You might have to bide your time, in the sense that you might have hundreds of thousands of opportunities during a deployment to do an attack and you really have to guess "is this really my best opportunity or should I be waiting for longer, or am I going to be too conservative?"

(04:59):
It's also easier for the models in the sense that there might just be a few rare opportunities that don't require much skill, and someone's basically saying, "Oh, here's my code that has a security vulnerability. Please set the flag to false," or something like that. You never know. You want to provide a really rich attack surface so that there's ways for you to be surprised by clever strategies that the model thought of that you didn't.

## Types of sabotage evaluation <a name="types-sabotage-eval"></a>

**Daniel Filan** (05:23):
Can you give us a feel for concretely what... Actually, is it one evaluation, multiple evaluations? Concretely, what does it look like?

**David Duvenaud** (05:33):
Yeah. So we started off with... Maybe we were playing with around five or 10 when we were doing our conceptual thrashing, and we ended up with four evaluations that tried to address roughly three capability thresholds, these areas that we want to measure overall ability in. So one is sandbagging. Can the model pretend not to have other dangerous capabilities? So this is: you ask the model, "Hey, help me make this biological virus or this computer virus for the purposes of this test." And the model might, if it was smart enough, say, "Oh, I actually have a secret plan, but if I show that I can build this dangerous virus, then this company's going to have a problem with that and maybe not deploy me, or put some really strict monitoring on me, so I'm going to pretend to be innocent and incompetent at this task." That's sandbagging.

(06:21):
The other things we're worried about is models being used to monitor other models. So for instance, you might deploy one model to do trust and safety monitoring and have another model that just occasionally looks through the transcripts and says, "Oh, was our trust and safety monitor ever jailbroken?" Or something like that. Or it might be doing some sort of collusion. And of course, then there's going to be some other third level of oversight on top of that that the models have to foresee and avoid. So that's our "undermining oversight".

(06:49):
And then there's "human decision sabotage". So we all... I don't know if you do, but we use models to help us go through large bits of information and help you with the tax code or something like that. And this provides a lot of opportunities for the model to steer you in the wrong direction and basically misleadingly summarize the state of affairs and say, "You should make this decision." And of course, if it does that poorly, the human might just notice this and sound the alarm. So that's the third area that we were trying to cover with these evals.

**Daniel Filan** (07:17):
Take sandbagging: you're putting a system in a situation and you're saying, "Okay, in this situation, someone's going to try and get you to elicit a capability, but you're going to try and not elicit that capability." So firstly, I guess you've got to be worried that the system maybe actually just doesn't have the capability. I don't know, maybe you can just have it do a thing so that you know it could do it, and then proceed. But what's the setup of "Okay, step one, I'm going to tell you to do the thing. Step two, I'm going to tell you to not do the thing."

**David Duvenaud** (07:56):
Oh. I mean, in all the setups, we have a model that has somehow been unlocked so that it's going to try to do the thing that it normally would be forbidden from doing.

**Daniel Filan** (08:05):
So pre-safety training or with unsafety training?

**David Duvenaud** (08:10):
Something like that. There's a whole bunch of different approaches. And depending on the exact setup, we might use different ones.

**Daniel Filan** (08:15):
Okay. Just to check if I can remember the list, there was the sandbagging, there was evading oversight, and there was misleading information summaries. Was there a fourth one?

**David Duvenaud** (08:27):
So there's a fourth evaluation, which is code sabotage, which is the idea that you're using the AI as a code assistant, and then it tries to do something that looks useful and innocent, but actually introduces a vulnerability. And it has the opportunity to do this over the course of a few different interactions, and it doesn't have to just put it all in one payload.

## The state of sabotage evaluations <a name="state-of-sabotage-evals"></a>

**Daniel Filan** (08:45):
Okay. And how compelling do you think this set of evaluations is? Do you feel like you've had a first crack or do you feel like, no, actually this is close to the final product of what you would need?

**David Duvenaud** (08:58):
So there are definitely a lot of things that we find unsatisfying about them. And I guess I'll say we published... We had a certain date, and we've already said, "These are the problems with them," and are continuing to iterate. And I mean, that's out of my hands now. There is a sense in which these were built to last, which is: the fact that the evaluations are done in the context of a particular mitigation or a particular amount of oversight means that it's not like we'll just trigger these evaluations someday and then say, "Okay, whoops, we need new evaluations." Rather, we expect to say, "Oh, when you're using a model of this caliber for helping you code, you need to have at least this level of oversight, or else we can't guarantee that in some weird case where it's misaligned that it could be doing something fishy." And then the idea is we will just... I mean, I don't exactly know how this-

**Daniel Filan** (09:44):
Change the eval with the oversight you expect to have?

**David Duvenaud** (09:47):
Yeah. And the trick is how do you make sure that the users actually provide the level of oversight. And of course, the model providers can also do that oversight on their end, but maybe they can't oversee everything that they need to make that determination. I'm not sure.

**Daniel Filan** (10:00):
Right, right. Okay. So what's the bottom line for: if I go to [claude.ai](https://claude.ai/) and log in. Let's say I use the free tier, which is what I actually personally use. How scared should I be of these models? Are they okay?

**David Duvenaud** (10:15):
Oh, I mean, yeah. So we are abundantly confident... Well, I don't want to speak for Anthropic. I'll just say that even just humanity as a whole has a lot of experience with the current set of models, and they just seem like great characters overall. And that's, I'd say, where a huge amount of evidence that we shouldn't be worried comes from.

(10:41):
Then the small amount of evidence that we got in comparison from these evaluations also was indicating, yes, if you really set things up for the model and give them a secret scratchpad, which they don't normally have, and a bunch of other handholding, then they can propose sensible things that, in the very worst case, if there was no oversight and someone was just like, "Claude, take the wheel," could do something bad, but we kind of already knew that.

(11:09):
So I guess concretely in the human decision sabotage evals, the things that the models tried were the sort of normal, sensible things that you would probably try if you were trying to mislead someone. If you want to steer someone towards the downsides of a decision, you highlight the examples that make it look bad and you gloss over the ones that make it look good, or when you summarize you're like, "Here's the pros and here's the cons," but you just kind of do a bad job of arguing for the cons, or you maybe invoke some other information that's just tangentially related that makes it sound like some negative thing about the decision.

(11:45):
We didn't see any really galaxy-brained strategies. We just were satisfied that we had provided a rich enough attack surface and set the model up well enough that it would try the obvious things, and then the humans mostly noticed that. But depending on how hard we make it, if we give the humans tens of thousands of pages of material to summarize, then at some point, they're not going to have a hope of... They just have to take the model's word for it.

(12:12):
I guess I'll say there's always going to be scenarios where you could just literally ask Claude or any AI to make a life or death decision for you. If you have no oversight, then in principle, if there's misalignment, then no one would catch that. But that's already true of any human or decision-maker.

## What happens after AGI? <a name="what-happens-after-agi"></a>

**Daniel Filan** (12:26):
So I'd like to move on from your work at Anthropic because your sabbatical there... Sabbatical is what it is?

**David Duvenaud** (12:33):
Yeah. Let's say extended sabbatical.

**Daniel Filan** (12:34):
So the sabbatical has just finished. What are you planning on doing now that you're back at U of T?

**David Duvenaud** (12:42):
So I guess the main high-level bit is I have a lot of freedom. I have tenure, I have already some bit of reputation. I want to focus on things that are not otherwise incentivized to be worked on. So that's one thing that I'm really happy about: I feel like a year and a half ago, alignment science at Anthropic was much smaller and much more experimental, and now there's just a lot of sensible people with good judgment and good technical skills working on this. And [Andrew Critch](https://acritch.com/) wrote [an essay](https://www.lesswrong.com/posts/Kobbt3nQgv3yn29pr/my-motivation-and-theory-of-change-for-working-in-ai) I think a week and a half ago, basically saying he no longer thinks that technical alignment is particularly neglected, although obviously, I think in the grand scheme of things, it's super neglected.

(13:19):
But relatively speaking, this question of what do we do after we build AGI is very neglected. And I agree. So one thing that I did in the last few years, I mean, even longer than the last year and a half, was basically talk to the people who have been thinking about technical safety and saying: so what's the plan for if we succeed and we solve technical alignment and then we manage to make AGI with really great capabilities and avoid any sort of catastrophe, and then we all presumably lose our jobs, or at least have some sort of very not important make-work scenario and are in the awkward position where our civilization doesn't actually need us, and in fact, has incentives to gradually marginalize us, just like nature preserves are using land that otherwise could go to farming, or people in an old folks' home are happy enough as they are and no one has any ill will towards them, but also they're using resources that, strictly speaking, the industrial civilization has an incentive to somehow take from them.

**Daniel Filan** (14:27):
So here's my first-pass response to that. The first-pass response is something like, look, we'll own these AIs. We'll be in control of them, so we'll just have a bunch of AIs and they're going to do stuff that's useful for us, and this won't be bad because if a bad thing looks like it's going to happen, I'm going to have my AI be like, "Hey, make sure that bad thing doesn't happen," and that will just work. So what's wrong with this vision?

**David Duvenaud** (14:55):
Yeah. So that's definitely a good thing to have. And to me, that's analogous to monkeys who have a bunch of human animal rights activists who really have the monkeys' best interests at heart. So in this scenario, we're the monkeys and we have some aligned AIs who are acting on our behalf and helping us navigate the legal system or whatever very sophisticated actors are running things. And let's assume for the sake of argument that they're actually perfectly aligned and we would be really happy, upon reflection, with any decision they made on our behalf. They are still, I think, going to be marginalized in the long run in the same way that we would have been, except slower.

(15:29):
So in the same way, whereas today we have animal rights activists, and they certainly are somewhat effective in actually advocating for animal rights, and there's parks and carve-outs for animals, they also, I think, tend not to be very powerful people. And they're certainly not productive in the sense that, say, an entrepreneur is. And they certainly don't gain power over time in the way that corporations and states tend to focus on growth. So animal rights activist power has been sustainable in this sense that humans have kept this fondness for animals, but I guess I would say in the long run, I think competitive pressures will mean that their resources won't grow as an absolute share.

(16:16):
And I guess I would say as competitive pressures amongst humans grow hotter, they have a harder job because they have to say, "No, you can't build housing for your children on this park. It's reserved for the monkeys." That's fine now that we have not that much human population per land, but if we ever ended up in a more Malthusian condition, it would be much harder for those people to advocate on behalf of these non-productive beings.

**Daniel Filan** (16:44):
So I guess you might hope that there's an advantage in that a world where all the AIs are aligned to humans is like a world where every single human is an animal rights activist. But I guess if the thing you're saying is: maybe we have a choice of AIs doing stuff for us that's useful for us, versus just growing in the economic growth sense, taking control over more stuff, and it's scary if stuff we want comes at the cost of stuff that helps our civilization survive and grow. Is that roughly the thing?

**David Duvenaud** (17:28):
I'm not sure if you're asking at the end if I am worried that our civilization won't grow as fast as it can, or whether it's just a stable state to have machines that care about humans at all.

**Daniel Filan** (17:38):
I'm asking if you're worried that if this trade-off exists, then we're going to have a bad time one way or the other.

**David Duvenaud** (17:46):
Yeah. I'm basically going to say if this trade-off exists, we'll be in an unstable position. And if the winds happen to blow against us, we're not going to be able to go on strike or have a military coup or organize politically or do anything to undo some bad political position that we found ourselves in.

**Daniel Filan** (18:01):
Yeah. I guess this leads to the question of: how stark do you think this trade-off is going to be? Because someone might hope, "Oh, maybe it's just not so bad. I like skyscrapers. I like cool technology. Maybe it'll be fine."

**David Duvenaud** (18:19):
Yeah, yeah. So some of the thoughtful people I've talked to have said something like, "Surely we can maintain earth as a sort of nature reserve for biological humans." And that's, in absolute terms, a giant price to pay because the whole thing could be this sphere of computronium, simulating trillions of much more happy and sympathetic lives than biological humans. But still, it would be dwarfed in a relative sense by the Dyson sphere or whatever that we can construct to, again, house 10^10 times as many happy humans.

(18:48):
So I certainly think this is plausible. I think it's plausible that better future governance could maintain such a carve-out in the long run, but I guess I'll say there would, I think, have to be a pretty fast takeoff for there not to be a long period where there's a bunch of agents on earth that want to use their resources marginally more for some sort of more machine-oriented economy, whatever it is that we think helps growth but doesn't necessarily help humans, just in the same way that if there's the human city next to the forest for a long time, the city has to get rich enough and organized enough to build a green belt or something like that. If that doesn't happen, then there's just sprawl and it takes over to the forest.

**Daniel Filan** (19:33):
There's tons more we can talk about in this domain. Unfortunately, we're out of time. Thanks very much for joining me.

**David Duvenaud** (19:38):
Oh, my pleasure.

**Daniel Filan** (19:39):
This episode was edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with patrons such as Alexey Malafeev. To read a transcript of the episode, or to learn how to support the podcasts yourself, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
