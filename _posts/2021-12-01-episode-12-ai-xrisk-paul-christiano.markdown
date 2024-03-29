---
layout: post
title: "12 - AI Existential Risk with Paul Christiano"
date: 2021-12-01 18:10 -0800
categories: episode
---

[YouTube link](https://www.youtube.com/watch?v=3L4czdIa8Tg&list=PLmjaTS1-AiDeqUuaJjasfrM6fjSszm9pK&index=12)

Why would advanced AI systems pose an existential risk, and what would it look like to develop safer systems? In this episode, I interview Paul Christiano about his views of how AI could be so dangerous, what bad AI scenarios could look like, and what he thinks about various techniques to reduce this risk.

Topics we discuss:
- [How AI may pose an existential threat](#ai-xrisk-how)
  - [AI timelines](#ai-timelines)
  - [Why we might build risky AI](#why-build-risky-ai)
  - [Takeoff speeds](#takeoff-speeds)
  - [Why AI could have bad motivations](#why-evil-ai)
  - [Lessons from our current world](#lessons-from-current-world)
  - ["Superintelligence"](#superintelligence)
- [Technical causes of AI x-risk](#technical-causes-ai-xrisk)
  - [Intent alignment](#intent-alignment)
  - [Outer and inner alignment](#outer-inner-alignment)
  - [Thoughts on agent foundations](#thoughts-on-agent-foundations)
- [Possible technical solutions to AI x-risk](#possible-technical-solutions)
  - [Imitation learning, inverse reinforcement learning, and ease of evaluation](#il-irl-eval)
  - [Paul's favorite outer alignment solutions](#pauls-favorite-outer-alignment-solutions)
    - [Solutions researched by others](#solutions-by-others)
    - [Decoupling planning from knowledge](#decoupling-planning-knowledge)
  - [Factored cognition](#factored-cognition)
  - [Possible solutions to inner alignment](#possible-solutions-inner-alignment)
- [About Paul](#about-paul)
  - [Paul's research style](#pauls-research-style)
  - [Disagreements and uncertainties](#disagreements-uncertainties)
  - [Some favorite organizations](#some-favorite-orgs)
  - [Following Paul's work](#following-pauls-work)

**Daniel Filan:**
Hello everybody. Today, I'll be speaking with Paul Christiano. Paul is a researcher at the [Alignment Research Center](https://alignmentresearchcenter.org/), where he works on developing means to align future machine learning systems with human interests. After graduating from a PhD in learning theory in 2017, he went onto research AI alignment at OpenAI, eventually running their language model alignment team. He's also a research associate at the Future of Humanity Institute in Oxford, a board member at the research non-profit Ought, a technical advisor for Open Philanthropy, and the co-founder of the Summer Program on Applied Rationality and Cognition, a high school math camp. For links to what we're discussing, you can check the description of this episode and you can read the transcript at [axrp.net](https://axrp.net/). Paul, welcome to AXRP.

**Paul Christiano:**
Thanks for having me on, looking forward to talking.

## How AI may pose an existential threat <a name="ai-xrisk-how"></a>

**Daniel Filan:**
All right. So, the first topic I want to talk about is this idea that AI might pose some kind of existential threat or an existential risk, and there's this common definition of existential risk, which is a risk of something happening that would incapacitate humanity and limit its possibilities for development, incredibly drastically in a way comparable to human extinction, such as human extinction. Is that roughly the definition you use?

**Paul Christiano:**
Yeah. I think I don't necessarily have a bright line around giant or drastic drops versus moderate drops. I often think in terms of the expected fraction of humanity's potential that is lost. But yeah, that's basically what I think of it. Anything that could cause us not to fulfill some large chunk of our potential. I think of AI in particular, a failure to align AI maybe makes the future, in my guess 10% or 20% worse, or something like that, in expectation. And that makes it one of the worst things. I mean, not the worst, that's a minority of all of our failure to fall short of our potential, but it's a lot of failure to fall short of our potential. You can't have that many 20% hits before you're down to no potential left.

**Daniel Filan:**
Yeah. When you say a 10% or 20% hit to human potential in expectation, do you mean if we definitely failed to align AI or do you mean we may or may not fail to align AI and overall that uncertainty equates to a 20%, or 10% to 20% hit?

**Paul Christiano:**
Yeah, that's unconditionally. So I think if you told me we definitely mess up alignment maximally then I'm more like, oh, now we are looking at a pretty big, close to 100% drop. I wouldn't go all the way to 100. It's not literally as bad probably as a barren earth, but it's pretty bad.

**Daniel Filan:**
Okay. Yeah. Supposing AI goes poorly or there's some kind of existential risk posed by some kind of, I guess really bad AI, what do you imagine that looking like?

**Paul Christiano:**
Yeah. So I guess, I think most often about alignment, although I do think there are other ways that you could imagine AI going poorly.

**Daniel Filan:**
Okay. And what's alignment?

**Paul Christiano:**
Yeah. So by alignment, I mean - I guess a little bit more specifically, we could say intent alignment - I mean the property that your AI is trying to do what you want it to do. So we're building these AI systems. We imagine that they're going to help us. They're going to do all the things humans currently do for each other. They're going to help us build things. They're going to help us solve problems. A system is intent aligned if it's trying to do what we want it to do. And it's misaligned if it's not trying to do what we want it to do. So a stereotypical bad case is you have some AI system that is sort of working at cross purposes to humanity. Maybe it wants to ensure that in the long run there are a lot of paperclips, and humanity wants human flourishing. And so the future is then some compromise between paperclips and human flourishing. And if you imagine that you have AI systems a lot more competent than humans that compromise may not be very favorable to humans. And then you might be basically all paperclips.

**Daniel Filan:**
Okay. So this is some world where you have an AI system, and the thing it's trying to do is not what humans want it to do. And then not only is it a typical bad employee or something, it seems you think that it somehow takes over a bunch of stuff or gains some other power. How are you imagining it being much, much worse than having a really bad employee today?

**Paul Christiano:**
I think that the bad employee metaphor is not that bad. And maybe this is a place I part ways from some people who work on alignment. And the biggest difference is that you can imagine heading for a world where virtually all of the important cognitive work is done by machines. So it's not as if you have one bad employee; it's as if for every flesh and blood human there were 10 bad employees.

**Daniel Filan:**
Okay.

**Paul Christiano:**
And if you imagine a society in which almost all of the work is being done by these inhuman systems who want something that's significantly at cross purposes, it's possible to have social arrangements in which their desires are thwarted, but you've kind of set up a really bad position. And I think the best guess would be that what happens will not be what the humans want to happen, but what the systems who greatly outnumber us want to happen.

**Daniel Filan:**
Okay. So we delegate a bunch of cognitive work to these AI systems, and they're not doing what we want. And I guess you further think it's going to be hard to un-delegate that work. Why do you think it will be hard to un-delegate that work?

**Paul Christiano:**
I think there's basically two problems. So one is, if you're not delegating to your AI then what are you delegating to? So if delegating to AI is a really efficient way to get things done and there's no other comparably efficient way to get things done, then it's not really clear, right? There might be some general concern about the way in which AI systems are affecting the world, but it's not really clear that people have a nice way to opt out. And that might be a very hard coordination problem. That's one problem. The second problem is just, you may be unsure about whether things are going well or going poorly. If you imagine again, this world where it's like there's 10 billion humans and 100 billion human-level AI systems or something like that: if one day it's like, oh, actually that was going really poorly that may not look like employees have embezzled a little money, it may instead look like they grabbed the machinery by which you could have chosen to delegate to someone else. It's kind of like the ship has sailed once you've instantiated 100 billion of these employees to whom you're delegating all this work. Maybe employee is kind of a weird or politically loaded metaphor. But the point is just you've made some collective system much more powerful than humans. One problem is you don't have any other options. The other is that system could clearly stop you. Over time, eventually, you're not going to be able to roll back those changes.

**Daniel Filan:**
Okay.

**Paul Christiano:**
Because almost all of the people doing anything in the world don't want you to. "People" in quotes, don't want you to roll back those changes.

**Daniel Filan:**
So some people think, probably what's going to happen is one day all humans will wake up dead. You might think that it looks we're just stuck on earth and AI systems get the whole rest of the universe or keep expanding until they meet aliens or something. What concretely do you think it looks like after that?

**Paul Christiano:**
I think it depends both on technical facts about AI and on some facts about how we respond. So some important context on this world: I think by default, if we weren't being really careful, one of the things that would happen is AI systems would be running most militaries that mattered. So when we talk about all of the employees are bad, we don't just mean people who are working in retail or working as scientists, we also mean the people who are taking orders when someone is like, "We'd like to blow up that city," or whatever.

**Daniel Filan:**
Yep.

**Paul Christiano:**
So by default I think exactly how that looks depends on a lot of things but in most of the cases it involves... the humans are this tiny minority that's going to be pretty easily crushed. And so there's a question of like, do your AI systems want to crush humans, or do they just want to do something else with the universe, or what? If your AI systems wanted paperclips and your humans were like, "Oh, it's okay. The AIs want paperclips. We'll just turn them all off," then you have a problem at the moment when the humans go to turn them all off or something. And that problem may look like the AIs just say like, "Sorry, I don't want to be turned off." And it may look like, and again, I think that could get pretty ugly if there's a bunch of people like, "Oh, we don't like the way in which we've built all of these machines doing all of this stuff."

**Paul Christiano:**
If we're really unhappy with what they're doing, that could end up looking like violent conflict, it could end up looking like people being manipulated to go on a certain course. It kind of depends on how humans attempt to keep the future on track, if at all. And then what resources are at the disposal of AI systems that want the future to go in this inhuman direction? Yeah. I think that probably my default visualization is humans won't actually make much effort, really. We won't be in the world where it's all the forces of humanity arrayed against the forces of machines. It's more just the world will gradually drift off the rails. By "gradually drift off the rails" I mean humans will have less and less idea what's going on.

**Paul Christiano:**
Imagine some really rich person who on paper has a ton of money. And is asking things to happen, but they give instructions to their subordinates and then somehow nothing really ends up ever happening. They don't know who they're supposed to talk to and they are never able to figure out what's happening on the ground or who to hold accountable. That's kind of my default picture. I think the reason that I have that default picture is just because I don't expect humans to, in cases where we fail, there's some way in which we're not going to really be pushing back that hard. I think if we were really unhappy with that situation then instead, you could not gradually drift off the rails, but if you really are messing up alignment then instead of gradually drifting off the rails it looks more like an outbreak of violent conflict or something like that.

**Daniel Filan:**
So, I think that's a good sense of what you see as the risks of having really smart AIs that are not aligned. Do you think that that is the main kind of AI-generated existential risk to worry about, or do you think that there are others that you're not focusing on but they might exist?

**Paul Christiano:**
Yeah. I think that there's two issues here. One is that I kind of expect a general acceleration of everything that's happening in the world. So just as the world now, you might think that it takes 20 to 50 years for things to change a lot. Long ago it used to take hundreds of years for things to change a lot. I do expect we will live to see a world where it takes a couple years and then maybe a couple months for things to change a lot. In some sense that entire acceleration is likely to be really tied up with AI. If you're imagining the world where next year the world looks completely different and is much larger than it was this year, that involves a lot of activity that humans aren't really involved in or understanding.

**Paul Christiano:**
So I do think that a lot of stuff is likely to happen. And from our perspective it's likely to be all tied up with AI. I normally don't think about that because I'm sort of not looking that far ahead. That is in some sense I think there's not much calendar time between the world of now and the world of "crazy stuff is happening every month", but a lot happens in the interim, right? The only way in which things are okay is if there are AI systems looking out for human interests as you're going through that transition. And from the perspective of those AI systems, a lot of time passes, or like, a lot of cognitive work happens.

**Paul Christiano:**
So I guess the first point was, I think there are a lot of risks in the future. In some sense from our perspective what it's going to feel like is the world accelerates and starts getting really crazy. And somehow AI is tied up with that. But I think if you were to be looking on the outside you might then see all future risks as risks that felt like about AI. But in some sense, they're kind of not our risks to deal with in some sense, they're the risks of the civilization that we become, which is a civilization largely run by AI systems.

**Daniel Filan:**
Okay. So you imagine, look, we might just have really dangerous problems later. Maybe there's aliens or maybe we have to coordinate well and AIs would somehow be involved.

**Paul Christiano:**
Yeah. So if you imagine a future nuclear war or something like that, or if you imagine all the future progressing really quickly. Then from your perspective on the outside what it looks like is now huge amounts of change are occurring over the course of every year, and so one of those changes is that somewhere that would've taken hundreds of years now only takes a couple years to get to the crazy destructive nuclear war. And from your perspective, it's kind of like, "Man, our crazy AI started a nuclear war." From the AI's perspective it's like we had many generations of change and this was one of the many coordination problems we faced, and we ended up with a nuclear war. It's kind of like, do you attribute nuclear wars as a failure of the industrial revolution, or risk of the industrial revolution? I think that would be a reasonable way to do the accounting. If you do the accounting that way there are a lot of risks that are AI risks. Just in the sense that there are a lot of risks that are industrial revolution risks. That's one category of answer, I think there's a lot of risks that kind of feel like AI risks in that they'll be consequences of crazy AI driven conflict or things like that, just because I view a lot of the future as crazy fast stuff driven by AI systems.

**Daniel Filan:**
Okay.

**Paul Christiano:**
There's a second category that's risks that to me feel more analogous to alignment, which are risks that are really associated with this early transition to AI systems, where we will not yet have AI systems competent enough to play a significant role in addressing those risks, so a lot of the work falls to us. I do think there are a lot of non-alignment risks associated with AI there. I'm happy to go into more of those. I think broadly the category that I am most scared about is there's some kind of deliberative trajectory humanity is kind of along ideally or that we want to be walking along. We want to be better clarifying what we want to do with the universe, what it is we want as humans, how we should live together, et cetera. There's some question of just, are we happy with where that process goes? Or if you're a moral realist type, do we converge towards moral truth? If you think that there's some truth of the matter about what was good, do we converge towards that? But even if you don't think there's a fact of the matter you could still say, "Are we happy with the people we become?" And I think I'm scared of risks of that type. And in some sense alignment is very similar to risks of that type, because you kind of don't get a lot of tries at them.

**Paul Christiano:**
You're going to become some sort of person, and then after we as a society converge on what we want, or as what we want changes, there's no one looking outside of the system, who's like, "Oops! We messed that one up. Let's try again." If you went down a bad path, you're sort of by construction now happy with where you are, but the question is about what you wanted to achieve. So I think there's potentially a lot of path dependence there. A lot of that is tied up, there are a lot of ways in which the deployment of AI systems will really change the way that humans talk to each other and think about what we want, or think about how we should relate.

**Paul Christiano:**
I'm happy to talk about some of those but I think the broad thing is just, if a lot of thinking is being done not by humans, that's just a weird situation for humans to be in, and it's a little bit unclear. If you're not really thoughtful about that, it's unclear if you're happy with it. If you told me that the world with AI and the world without AI converged to different views about what is good, I'm kind of like, "Oh, I don't know which of those... " Once you tell me there's a big difference between those, I'm kind of scared. I don't know which side is right or wrong, they're both kind of scary. But I am definitely scared.

### AI timelines <a name="ai-timelines"></a>

**Daniel Filan:**
So, I think you said that relatively soon, we might end up in this kind of world where most of the thinking is being done by AI. So there's this claim that AI is going to get really good, and not only is it getting really good, it's going to be the dominant way we do most cognitive work, or most thinking maybe. And not only is that eventually going to happen, it's not going to be too long from now. I guess the first thing I'd like to hear is, by not too long from now do you mean the next 1000 years, the next 100 years, the next 10 years? And if somebody's skeptical of that claim, could you tell us why you believe that?

**Paul Christiano:**
So I guess there's a couple parts of the claim. One is AI systems becoming... I think right now we live in a world where AI does not very much change the way that humans get things done. That is, technologies you'd call AI are not a big part of how we solve research questions or how we design new products or so on. There's some transformation from the world of today to a world in which AI is making us, say, considerably more productive. And there's a further step to the world where human labor is essentially obsolete, where it's from our perspective this crazy fast process. So I guess my overall guess is I have a very broad distribution over how long things will take. Especially how long it will take to get to the point where AI is really large, where maybe humans are getting twice as much done, or getting things done twice as quickly due to AI overall.

**Paul Christiano:**
Maybe I think that there's a small chance that that will happen extremely quickly. So there's some possibility of AI progress being very rapid from where we are today. Maybe in 10 years, I think there's a 5% or 10% chance that AI systems can make most things humans are doing much, much faster. And then kind of taking over most jobs from humans. So I think that 5% to 10% chance of 10 years, that would be a pretty crazy situation where things are changing pretty quickly. I think there's a significantly higher probability in 20 or 40 years. Again in 20 years maybe I'd be at 25%. At 40 years maybe I'm at 50%, something like that. So that's the first part of the question, when are we in this world where the world looks very different because of AI, where things are happening much faster? And then I think I have a view that feels less uncertain, but maybe more contrarian about... I mean more contrarian than the world at large, very not-that-contrarian amongst the effective altruist or rationalist or AI safety community.

**Paul Christiano:**
So I have another view which I think I feel a little bit less uncertain about, that is more unusual in the world at large, which is that you only have probably on the order of years between AI that has... maybe you can imagine it's three years between AI systems that have effectively doubled human productivity and AI systems that have effectively completely obsoleted humans. And it's not clear. There's definitely significant uncertainty about that number, but I think it feels quite likely to me that it's relatively short. I think amongst people who think about alignment risk, I actually probably have a relatively long expected amount of time between those milestones.

**Paul Christiano:**
And if you talk to someone like [Eliezer Yudkowsky](https://en.wikipedia.org/wiki/Eliezer_Yudkowsky) from [MIRI](https://intelligence.org/), I think he would be more like "good chance that that's only one month" or something like that between those milestones. I have the view that the best guess would be somewhere from one to five years. And I think even at that timeline, that's pretty crazy and pretty short. Yeah. So my answer was some broad distribution over how many decades until you have AI systems that have really changed the game, and are making humans several times more productive. Say the economy's growing several times faster than it is today. And then from there most likely on the order of years rather than decades until humans are basically completely obsolete, and AI systems have improved significantly past that first milestone.

**Daniel Filan:**
And can you give us a sense of why somebody might believe that?

**Paul Christiano:**
Yeah. Maybe I'll start with the second and then go back to the first. I think the second is, in some sense, a less popular position in the broader world. I think one important part of story is the current rate of progress that you would observe in either computer hardware or computer software. So if you ask given an AI system, how long does it take to get, say, twice as cheap until you can do the same thing that it used to be able to do for half as many dollars? That tends to be something in the ballpark of a year, rather than something in the ballpark of a decade. So right now that doesn't matter very much at all. So if you're able to do the same or you're able to train the same neural net for half the dollars, it doesn't do that much. It just doesn't help you that much if you're able to run twice as many neural networks. Even if you have self-driving cars, the cost of running the neural networks isn't actually a very big deal. Having twice as many neural networks to drive your cars doesn't improve overall output that much. If you're in a world where, say, you have AI systems which are effectively substituting for human researchers or human laborers, then having twice as many of them eventually becomes more like having twice as many humans doing twice as much work, which is quite a lot, right? So that is more like doubling the amount of total stuff that's happening in the world.

**Paul Christiano:**
It doesn't actually double the amount of stuff because there's a lot of bottlenecks, but it looks like, starting from the point where AI systems are actually doubling the rate of growth or something like that, it doesn't really seem there are enough bottlenecks to prevent further doublings in the quality of hardware or software from having really massive impacts really quickly. So that's how I end up with thinking that the time scale is measured more like years than decades. Just like, once you have AI systems which are sort of comparable with humans or are in aggregate achieving as much as humans, it doesn't take that long before you have AI systems whose output is twice or four times that of humans.

**Daniel Filan:**
Okay. And so this is basically something like, in economics you call it an endogenous growth story, or a society-wide recursive self-improvement story. Where if you double the human population, and if they're AI systems, maybe that makes it better, there are just more ideas, more innovation and a lot of it gets funneled back into improving the AI systems that are a large portion of the cognitive labor. Is that roughly right?

**Paul Christiano:**
Yeah. I think that's basically right. I think there are kind of two parts to the story. One is what you mentioned of all the outputs get plowed back into making the system ever better. And I think that, in the limit, produces this dynamic of successive doublings of the world where each is significantly faster than the one before.

**Daniel Filan:**
Yep.

**Paul Christiano:**
I think there's another important dynamic that can be responsible for kind of abrupt changes that's more like, if you imagine that humans and AIs were just completely interchangeable: you can either use a human to do a task or an AI to do a task. This is a very unrealistic model, but if you start there, then there's kind of the curve of how expensive it is or how much we can get done using humans, which is growing a couple percent per year, and then how much you can get done using AIs, which is growing 100% per year or something like that. So you can kind of get this kink in the curve when the rapidly growing 100% per year curve intercepts and then continues past the slowly growing human output curve.

**Paul Christiano:**
If output was the sum of two exponentials, one growing fast and one growing slow, then you can have a fairly quick transition as one of those terms becomes the dominant one in the expression. And that dynamic changes if humans and AIs are complementary in important ways. And also the rate of progress changes if you change... like, progress is driven by R&D investments, it's not an exogenous fact about the world that once every year things double. But it looks the basic shape of that curve is pretty robust to those kinds of questions, so that you do get some kind of fairly rapid transition.

**Daniel Filan:**
Okay. So we currently have something like a curve where humanity gets richer, we're able to produce more food. And in part, maybe not as much in wealthy countries, but in part that means there are more people around and more people having ideas. So, you might think that the normal economy has this type of feedback loop, but it doesn't appear that at some point there's going to be these crazy doubling times of 5 to 10 years and humanity is just going to go off the rails. So what's the key difference between humans and AI systems that makes the difference?

**Paul Christiano:**
It is probably worth clarifying that on these kinds of questions I am more hobbyist than expert. But I'm very happy to speculate about them, because I love speculating about things.

**Daniel Filan:**
Sure.

**Paul Christiano:**
So I think my basic take would be that over the broad sweep of history, you have seen fairly dramatic acceleration in the rate of humans figuring new things out, building new stuff. And there's some dispute about that acceleration in terms of how continuous versus how jumpy it is. But I think it's fairly clear that there was a time when aggregate human output was doubling more like every 10,000 or 100,000 years.

**Daniel Filan:**
Yep.

**Paul Christiano:**
And that has dropped somewhere between continuously and in three big jumps or something, down to doubling every 20 years. And we don't have very great data on what that transition looks like, but I would say that it is at least extremely consistent with exactly the kind of pattern that we're talking about in the AI case.

**Daniel Filan:**
Okay.

**Paul Christiano:**
And if you buy that, then I think you would say that the last 60 years or so have been fairly unusual as growth hit this... maybe gross world product growth was on the order of 4% per year or something in the middle of the 20th century. And the reason things have changed, there's kind of two explanations that are really plausible to me. One is you no longer have accelerating population growth in the 20th century. So for most of human history, human populations are constrained by our ability to feed people. And then starting in the 19th, 20th centuries human populations are instead constrained by our desire to create more humans, which is great.

**Paul Christiano:**
It's good not to be dying because you're hungry. But that means that you no longer have this loop of more output leading to more people. I think there's a second related explanation, which is that the world now changes kind of roughly on the time scale of human lifetime, that is like, it now takes decades for a human to adapt to change and also decades for the world to change a bunch. So you might think that changing significantly faster than that does eventually become really hard for processes driven by humans. So you have additional bottlenecks just beyond how much work is getting done, where it's at some point very hard for humans to train and grow new humans, or train and raise new humans.

**Daniel Filan:**
Okay.

**Paul Christiano:**
So those are some reasons that a historical pattern of acceleration may have recently stopped. Either because it's reached the characteristic timescales of humans, or because we're no longer sort of feeding output back into raising population. Now we're sort of just growing our population at the rate which is most natural for humans to grow. Yeah, I think that's my basic take. And then in some sense AI would represent a return to something that at least plausibly was a historical norm, where further growth is faster, because research is one of those things or learning is one of those things that has accelerated. Recently I don't know if you've discussed this before, but Holden Karnofsky at Cold Takes has been writing [a bunch of blog posts](https://www.cold-takes.com/most-important-century/) summarizing what this view looks like, and some of the evidence for it. And then prior to that, Open Philanthropy was writing [a](https://www.openphilanthropy.org/blog/modeling-human-trajectory) [number](https://www.openphilanthropy.org/blog/new-report-brain-computation) [of](https://www.alignmentforum.org/posts/KrJfoZzpSDpnrv9va/draft-report-on-ai-timelines) [reports](https://www.openphilanthropy.org/blog/report-advanced-ai-drive-explosive-economic-growth) looking at pieces of the story and thinking through it, which I think overall taken together makes the view seem pretty plausible, still.

**Daniel Filan:**
Okay.

**Paul Christiano:**
That there is some general historical dynamic, which it would not be crazy if AI represented a return to this pattern.

**Daniel Filan:**
Yes. And indeed if people are interested in this, there's an episode that's... unfortunately the audio didn't work out, but one can read [a transcript of an interview with Ajeya Cotra](https://axrp.net/episode/2021/05/28/episode-7_5-forecasting-transformative-ai-ajeya-cotra.html) on this question of when we'll get very capable AI.

### Why we might build risky AI <a name="why-build-risky-ai"></a>

**Daniel Filan:**
To change gears a little bit. One question that I want to ask is, you have this story where we're gradually improving AI capabilities bit by bit, and it's spreading more and more. And in fact the AI systems, in the worrying case, they are misaligned and they're not going to do what people want them to do, and that's going to end up being extremely tragic. It will lead to an extremely bad outcome for humans.

**Daniel Filan:**
And at least for a while it seems like humans are the ones who are building the AI systems and getting them to do things. So, I think a lot of people have this intuition like, look, if AI causes a problem... we're going to deploy AI in more and more situations, and better and better AI, and we're not going to go from zero to terrible, we're going to go from an AI that's fine to an AI that's moderately naughty, before it hits something that's extremely, world endingly bad or something. It seems you think that might not happen, or we might not be able to fix it or something. I'm wondering, why is that?

**Paul Christiano:**
I guess there's again, maybe two parts of my answer. So one is that I think that AI systems can be doing a lot of good, even in this regime where alignment is imperfect or even actually quite poor. The prototypical analogy would be, imagine you have a bad employee who cares not at all about your welfare, or maybe a typical employee who cares not about your welfare, but cares about being evaluated well by you. They care about making money. They care about receiving good performance reviews, whatever. Even if that's all they care about, they can still do a lot of good work. You can still perform evaluation such that the best way for them to earn a bonus, or get a good performance review, or not be fired is to do the stuff you want: to come up with good ideas, to build stuff, to help you notice problems, things like that.

**Paul Christiano:**
And so I think that you're likely to have, in the bad case, this fairly long period where AI systems are very poorly aligned that are still adding a ton of value and working reasonably well. And I think in that regime you can observe things like failures. You can observe systems that are say, again, just imagine the metaphor of some kind of myopic employee who really wants a good performance review. You can imagine them sometimes doing bad stuff. Maybe they fake some numbers, or they go and tamper with some evidence about how well they're performing, or they steal some stuff and go use it to pay some other contractor to do their work or something. You can imagine various bad behaviors pursued in the interest of getting a good performance review. And you can also imagine fixing those, by shifting to gradually more long term and more complete notions of performance.

**Paul Christiano:**
So say I was evaluating my system once a week. And one week it's able to get a really good score by just fooling me about what happened that week. Maybe I notice next week and I'm like, "Oh, that was actually really bad." And maybe I say, "Okay, what I'm training you for now is not just myopically getting a good score this week, but also if next week I end up feeling like this was really bad, that you shouldn't like that at all." So I could train, I could select amongst AI systems those which got a good score, not only over the next week but also didn't do anything that would look really fishy over the next month, or something like that. And I think that this would fix a lot of the short term problems that would emerge from misalignment, right? So if you have AI systems which are merely smart, so that they can understand the long term consequences, they can understand that if they do something fraudulent, you will eventually likely catch it. And that that's bad. Then you can fix those problems just by changing the objective to something that's a slightly more forward looking performance review. So that's part of the story, that I think there's this dynamic by which misaligned systems can add a lot of value, and you can fix a lot of the problems with them without fixing the underlying problem.

**Daniel Filan:**
Okay. There's something a little bit strange about this idea that people would apply this fix, that you think predictably preserves the possibility of extremely terrible outcomes, right? Why would people do something so transparently silly?

**Paul Christiano:**
Yeah. So I think that the biggest part of my answer is that it is, first very unclear that such an act is actually really silly. So imagine that you actually have this employee, and what they really want to do is get good performance reviews over the next five years. And you're like, well, look, they've never done anything bad before. And it sure seems all the kinds of things they might do that would be bad we would learn about within five years. They wouldn't really cause trouble. Certainly for a while it's a complicated empirical question, and maybe even at the point when you're dead, it's a complicated empirical question, whether there is scope for the kind of really problematic actions you care about, right? So the kind of thing that would be bad in this world, suppose that all the employees of the world are people who just care about getting good performance reviews in three years.

**Paul Christiano:**
That's just every system is not a human, everything doing work is not a human. It's this kind of AI system that has been built and it's just really focused on the objective. What I care about is the performance review that's coming up in three years. The bad outcome is one where humanity collectively, the only way it's ever even checking up on any of these systems or understanding what they're doing is by delegating to other AI systems who also just want a really good performance review in three years. And someday, there's kind of this irreversible failure mode where all the AI systems are like, well, look. We could try and really fool all the humans about what's going on, but if we do that the humans will be unhappy when they discover what's happened. So what we're going to do instead is we're going to make sure we fool them in this irreversible way.

**Paul Christiano:**
Either they are kept forever in the dark, or they realize that we've done something bad but they no longer control the levers of the performance review. And so, if all of the AI systems in the world are like there's this great compromise we can pursue. There's this great thing that the AI should do, which is just forever give ourselves ideal perfect performance reviews. That's this really bad outcome, and it's really unclear if that can happen. I think in some sense people are predictably leaving themselves open to this risk, but I don't think it will be super easy to assess, well, this is going to happen in any given year. Maybe eventually it would be. It depends on the bar of obviousness that would motivate people.

**Paul Christiano:**
And that maybe relates to the other reason it seems kind of tough. If you have some failure, for every failure you've observed there's this really good fix, which is to push out what your AI system cares about, or this timescale for which it's being evaluated to a longer horizon. And that always works well. That always copes with all the problems you've observed so far. And to the extent there's any remaining problems, they're always this kind of unprecedented problem. They're always at this time scale that's longer than anything you've ever observed, or this level of elaborateness that's larger than anything you've observed. And so I think it is just quite hard as a society, we're probably not very good at it. It's hard to know exactly what the right analogy is, but basically any way you spin it, it doesn't seem that reassuring about how much we collectively will be worried by failures that are kind of analogous to, but not exactly like, any that we've ever seen before.

**Paul Christiano:**
I imagine in this world, a lot of people would be vaguely concerned. A lot of people would be like, "Oh, aren't we introducing this kind of systemic risk? This correlated failure of AI systems seems plausible and we don't have any way to prepare for it." But it's not really clear what anyone does on the basis of that concern or how we respond collectively. There's a natural thing to do which is just sort of not deploy some kinds of AI, or not to deploy AI in certain ways, but that looks it could be quite expensive and would leave a lot of value on the table. And hopefully people can be persuaded to that, but it's not at all clear they could be persuaded, or for how long. I think the main risk factor for me is just: is this a really, really hard problem to deal with?

**Paul Christiano:**
I think if it's a really easy problem to deal with, it's still possible, we'll flub it. But at least it's obvious what the ask is if you're saying, look, there's a systemic risk, and you could address it by doing the following thing. Then it's not obvious. I think there are easy to address risks that we don't do that well at addressing collectively. But at least there's a reasonably good chance. If we're in the world where there's no clear ask, where the ask is just like, "Oh, there's a systemic risk, so you should be scared and maybe not do all that stuff you're doing." Then I think you're likely to run into everyone saying, "But if we don't do this thing, someone else will do it even worse than us and so, why should we stop?"

**Daniel Filan:**
Yeah. So earlier I asked why don't people fix problems as they come up. And part one of the answer was, maybe people will just push out the window of evaluation and then there will be some sort of correlated failure. Was there a part two?

**Paul Christiano:**
Yeah. So part two is just that it may be... I didn't get into justification for this, but it may be hard to fix the problem. You may not have an easy like, "Oh yeah, here's what we have to do in order to fix the problem." And it may be that we have a ton of things that each maybe help with the problem. And we're not really sure, it's hard to see which of these are band-aids that fix current problems versus which of them fix deep underlying issues, or there may just not be anything that plausibly fixes the underlying issue. I think the main reason to be scared about that is just that it's not really clear we have a long term development strategy, at least to me.

**Paul Christiano:**
It's not clear we have any long term development strategy for aligned AI. I don't know if we have a roadmap where we say, "Here's how you build some sequence of arbitrarily competent aligned AIs." I think mostly we have, well here's how maybe you cope with the alignment challenges presented by the systems in the near term, and then we hope that we will gradually get more expert to deal with later problems. But I think all the plans have some question marks where they say, "Hopefully, it will become more clear as we get empirical. As we get some experience with these systems, we will be able to adapt our solutions to the increasingly challenging problems." And it's not really clear if that will pan out. Yeah. It seems a big question mark right now to me.

### Takeoff speeds <a name="takeoff-speeds"></a>

**Daniel Filan:**
Okay. So I'm now going to transition a little bit to questions that somebody who is very bullish on AI x-risk might ask, or ways they might disagree with you. I mean bullish on the risk, bearish on the survival. Bullish meaning you think something's going to go up and bearish meaning you think something's going to go down. So yeah, some people have this view that it might be the case that you have one AI system that you're training for a while. Maybe you're a big company, you're training it for a while, and it goes from not having a noticeable impact on the world to effectively running the world in less than a month. This is often called the Foom view. Where your AI blows up really fast in intelligence, and now it's king of the world. I get the sense that you don't think this is likely, is that right?

**Paul Christiano:**
I think that's right. Although, it is surprisingly hard to pin down exactly what the disagreement is about, often. And the thing that I have in mind may feel a lot like foom. But yeah, I think it's right, that the version of that, that people who are most scared have in mind, feels pretty implausible to me.

**Daniel Filan:**
Why does it seem implausible to you?

**Paul Christiano:**
I think the really high level... first saying a little bit about why it seems plausible or fleshing out the view, as I understand it: I think the way that you have this really rapid jump normally involves AI systems automating the process of making further AI progress. So you might imagine you have some sort of object level AI systems that are actually conducting biology research or actually building factories or operating drones. And then you also have a bunch of humans who are trying to improve those AI systems. And what happens first is not that AIs get really good at operating drones or doing biology research, but AIs get really good at the process of making AIs better. And so you have in a lab somewhere, AI systems making AIs better and better and better, and that can race really far ahead of AI systems having some kind of physical effect in the world.

**Paul Christiano:**
So you can have AI systems that are first a little bit better than humans, and then significantly better. And then just radically better than humans at AI progress. And they sort of bring up the quality, right? As you have those much better systems doing AI work, they very rapidly bring up the quality of physical AI systems doing stuff in the physical world, before having much actual physical deployment. And then something kind of at the end of the story, in some sense, after all like the real interesting work has already happened, you now have these really competent AI systems that can get rolled out, and that are taking advantage. Like there's a bunch of machinery lying around, and you imagine these godlike intelligences marching out into the world and saying, "How can we, over the course of the next 45 seconds utilize all this machinery to take over the world", or something like that. It's kind of how the story goes.

**Paul Christiano:**
And the reason it got down to 45 seconds is just because there have been many generations of this ongoing AI progress in the lab. That's how I see the story, and I think that's probably also how people who are most scared about that see the story of having this really rapid self improvement.

**Paul Christiano:**
Okay, so now we can talk about why I'm skeptical, which is basically just quantitative parameters in that story. So I think there will come a time when most further progress in AI is driven by AIs themselves, rather than by humans. I think we have a reasonable sense of when that happens, qualitatively. If you bought this picture of, with human effort, let's just say AI systems are doubling in productivity every year. Then there will come some time when your AI has reached parity with humans at doing AI development. And now by that point, it takes six further months until... if you think that that advance amounts to an extra team of humans working or whatever, it takes in the ballpark of a year for AI systems to double in productivity one more time. And so that kind of sets the time scale for the following developments. Like at the point when your AI systems have reached parity with humans, progress is not that much faster than if it was just humans working on AI systems. So the amount of time it takes for AIs to get significantly better again, is just comparable to the amount of time it would've taken humans working on their own to make the AI system significantly better. So it's not something that happens on that view, in like a week or something.

**Paul Christiano:**
It is something that happens potentially quite fast, just because progress in AI seems reasonably fast. I guess my best guess is that it would slow, for which we can talk about. But even at the current rate, it's still, you're talking something like a year, and then the core question becomes what's happening along that trajectory. So what's happening over the preceding year, and over the following six months. And from that moment where AI systems have kind of reached parity with humans at making further AI progress and I think the basic analysis is at that point, AI is one of the most important, if not the most important, industries in the world. At least in kind of an efficient market-y world. We could talk about how far we depart from an efficient market-y world. But in efficient market-y world, AI and computer hardware and software broadly is where most of the action is in the world economy. At the point when you have AI systems that are matching humans in that domain, they are also matching humans in quite a lot of domains. You have a lot of AI systems that are able to do a lot of very cool stuff in the world. And so you're going to have then, on the order of a year, even six months after that point, of AI systems doing impressive stuff. And for the year before that, or a couple years before that, you also had a reasonable amount of impressive AI applications.

**Daniel Filan:**
Okay. So, it seems like key place where that story differs is in the foom story, it was very localized. There was one group where AI was growing really impressively. Am I right, that you are thinking, no, probably a bunch of people will have AI technology that's like only moderately worse than this amazing thing?

**Paul Christiano:**
Yeah. I think that's basically right. The main caveat is what "one group" means. And so I think I'm open to saying, "Well, there's a question of how much integration there is in the industry."

**Daniel Filan:**
Yeah.

**Paul Christiano:**
And you could imagine that actually most of the AI training is done... I think there are these large economies of scale in training machine learning systems. Because you have to pay for these very large training runs, and you just want to train. You want to train the biggest system you can and then deploy that system a lot of times, often. Training a model that's twice as big and deploying half as many of them is better than training a smaller model and deploying. Though obviously, it depends on the domain. But anyway, you often have these economies of scale.

**Daniel Filan:**
Yep.

**Paul Christiano:**
If you have economies of scale, you might have a small number of really large firms. But I am imagining then you're not talking, some person in the basement, you're talking, you have this crazy $500 billion project at Google.

**Daniel Filan:**
Yep.

**Paul Christiano:**
In which Google, amongst other industries, is being basically completely automated.

**Daniel Filan:**
And so there, the view is, the reason that it's not localized is that Google's a big company and while this AI is fooming, they sort of want to use it a bit to do things other than foom.

**Paul Christiano:**
Yeah. That's right. I think one thing I am sympathetic to in the fast takeoff story is, it does seem like in this world, as you're moving forward and closer to AIs having parity with humans, the value of the sector - computer hardware, computer software, any innovations that improve the quality of AI - all of those are becoming extremely important. You are probably scaling them up rapidly in terms of human effort. And so at that point, you have this rapidly growing sector, but it's hard to scale it up any faster, people working on AI or working in computer hardware and software.

**Paul Christiano:**
And so, there's this really high return to human cognitive labor in that area. And so probably it's the main thing you're taking and putting the AIs on, the most important task for them. And also the task you understand best as an AI research lab, is improving computer hardware, computer software, making these training runs more efficient, improving architectures, coming up with better ways to deploy your AI. So, I think it is the case that in that world, maybe the main thing Google is doing with their $500 billion project is automating Google and a bunch of adjacent firms. I think that's plausible. And then I think the biggest disagreement between the stories is, what is the size of that as it's happening? Is that happening in some like local place with a small AI that wasn't a big deal, or is this happening at some firm where all the eyes of the world are on this firm, because it's this rapidly growing firm that makes up a significant fraction of GDP and is seen as a key strategic asset by the host government and so on.

**Daniel Filan:**
So all the eyes are on this firm and it's still plowing most of the benefits of its AI systems into developing better AI. But is the idea then that the government puts a stop to it, or does it mean that somebody else steals the AI technology, and makes their own slightly worse AI? Why do all the eyes being on it change the story?

**Paul Christiano:**
I mean, I do think the story is still pretty scary. And I don't know if this actually changes my level of fear that much, but answering some of your concrete questions: I expect in terms of people stealing the AI, it looks kind of like industrial espionage generally. So people are stealing a lot of technology. They generally lag a fair distance behind, but not always. I imagine that governments are generally kind of protective of domestic AI industry, because it's an important technology in the event of conflict. That is, no one wants to be in a position where critical infrastructure is dependent on software that they can't maintain themselves. I think that probably the most alignment relevant thing is just that you now have these very large number of human equivalents working in AI. In fact a large share, in some sense, of the AI industry is made of AIs.

**Paul Christiano:**
And one of the key ways in which things can go well is for those AI systems to also be working on alignment. And one of the key questions is how effectively does that happen? But by the time you're in this world, in addition to the value of AI being much higher, the value of alignment is much higher. I think that alignment worked on far in advance still matters a lot. There's a good chance that there's going to be a ton of institutional problems at that time, and that it's hard to scale up work quickly. But I do think you should be imagining, most of the alignment work in total is done, as part of this gigantic project. And a lot of that is done by AIs. I mean, before the end, in some sense, almost all of it is done by AIs.

**Paul Christiano:**
Overall, I don't know if this actually makes me feel that much more optimistic. I think maybe there's some other aspects, some additional details in the foom story that kind of puts you in this, no empirical feedback regime. Which is maybe more important than the size of the fooming system. I think I'm skeptical of a lot of the empirical claims about alignment. So an example of the kind of thing that comes up: we are concerned about AI systems that actually don't care at all about humans, but in order to achieve some long term end, want to pretend they care about humans.

**Paul Christiano:**
And the concern is this can almost completely cut off your ability to get empirical evidence about how well alignment is working. Because misaligned systems will also try and look aligned. And I think there's just some question about how consistent that kind of motivational structure is. So, if you imagine you have someone who's trying to make the case for severe alignment failures, can that person exhibit a system which is misaligned and just takes its misalignment to go get an island in the Caribbean or something, rather than trying to play the long game, and convince everyone that it's aligned so it can grab the stars. Are there some systems that just want to get good performance reviews? Some systems will want to look like they're being really nice consistently in order that they can grab the stars later, or somehow divert the trajectory of human civilization. But there may also just be a lot of misaligned systems that want to fail in much more mundane ways that are like, "Okay, well there's this slightly outside of bounds way to hack the performance review system and I want to get a really good review, so I'll do that."

**Paul Christiano:**
So, how much opportunity will we have to empirically investigate those phenomena? And the arguments for total unobservability, that you never get to see anything, just currently don't seem very compelling to me. I think the best argument in that direction is, empirical evidence is on a spectrum of how analogous it is to the question you care about. So we're concerned about AI that changes the whole trajectory of human civilization in a negative way. We're not going to get to literally see AI changing the trajectory of civilization in a negative way. So now it comes down to some kind of question about institutional or social competence. Of what kind of indicators are sufficiently analogous that we can use them to do productive work, or to get worried in cases where we should be worried.

**Paul Christiano:**
I think the best argument is, "Look, even if these things are in some technical sense, very analogous and useful problems to work on, people may not appreciate how analogous they are or they may explain them away. Or they may say, 'Look, we wanted to deploy this AI and actually we fixed that problem, haven't we?'" Because the problem is not thrown in your face in the same way that airplane safety or something is thrown in your face, then people may have a hard time learning about it. Maybe I've gone on a little bit of a tangent away from the core question.

**Daniel Filan:**
Okay. Hopefully we can talk about related issues a bit later. On the question of takeoff speeds. So you wrote [a post](https://sideways-view.com/2018/02/24/takeoff-speeds/) a while ago that is mostly arguing against arguments you see for very sudden takeoff of AI capabilities from very low to very high. And a question I had about that is, one of the arguments you mentioned in favor of very sudden capability gains, is there being some sort of secret sauce to intelligence. Which in my mind is, it looks like one day you discover, maybe it's [Bayes' theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem), or maybe you get the actual ideal equation for [bounded rationality](https://plato.stanford.edu/entries/bounded-rationality/) or something. I think there's some reason to think of intelligence as somehow a simple phenomenon.

**Daniel Filan:**
And if you think that, then it seems maybe, one day you could just go from not having the equation, to having it, or something? And in that case, you might expect that, you're just so much better when you have the ideal rationality equation, compared to when you had to do whatever sampling techniques and you didn't realize how to factor in bounded rationality or something. Why don't you think that's plausible, or why don't you think it would make this sudden leap in capabilities?

**Paul Christiano:**
I don't feel like I have deep insight into whether intelligence has some beautiful, simple core. I'm not persuaded by the particular candidates, or the particular arguments on offer for that.

**Daniel Filan:**
Okay.

**Paul Christiano:**
And so I am more feeling there's a bunch of people working on improving performance on some task. We have some sense of how much work it takes to get what kind of gain, and what the structure is for that task. If you look at a new paper, what kind of gain is that paper going to have and how much work did it have? How does that change as more and more people have worked in the field? And I think both in ML and across mature industries in general, but even almost unconditionally, it's just pretty rare to have like a bunch of work in an area, and then some small overlooked thing makes a huge difference. In ML, we're going to be talking about many billions of dollars of invest, tens or hundreds of billions, quite plausibly.

**Paul Christiano:**
It's just very rare to then have a small thing, to be like, "Oh, we just overlooked all this time, this simple thing, which makes a huge difference." My training is as a theorist. And so I like clever ideas. And I do think clever ideas often have big impacts relative to the work that goes into finding them. But it's very hard to find examples of the impacts being as big as the one that's being imagined in this story. I think if you find your clever algorithm and then when all is said and done, the work of noticing that algorithm, or the luck of noticing that algorithm is worth a 10X improvement in the size of your computer or something, that's a really exceptional find. And those get really hard to find as a field is mature and a lot of people are working on it.

**Paul Christiano:**
Yeah. I think that's my basic take. I think it is more plausible for various reasons in ML than for other technologies. It's more surprising than that if you're working on planes and someone's like, "Oh, here's an insight about how to build planes." And then suddenly you have planes that are 10 times cheaper per unit of strategic relevance. That's more surprising than for ML. And that kind of thing does happen sometimes. But I think it's quite rare in general, and it will also be rare in ML.

**Daniel Filan:**
So another question I have about takeoff speed is, we have some evidence about AI technology getting better. Right? These Go-playing programs have [improved in my lifetime](https://aiimpacts.org/time-for-ai-to-cross-the-human-performance-range-in-go/) from not very good to [better than any human](https://en.wikipedia.org/wiki/AlphaZero). [Language models](https://arxiv.org/abs/2005.14165) have gotten better at producing language, roughly like a human would produce it, although perhaps not an expert human. I'm wondering, what do you think those tell us about the rate of improvement in AI technology, and to what degree further progress in AI in the next few years might confirm or disconfirm your general view of things?

**Paul Christiano:**
I think that the overall rate of progress has been, in software as in hardware, pretty great. It's a little bit hard to talk about what are the units of how good your AI system is. But I think a conservative lower bound is just, if you can do twice as much stuff for the same money. We understand what the scaling of twice as many humans is like. And in some sense, the scaling of AI is more like humans thinking twice as fast. And we understand quite well with the scaling of that is like. So if you use those as your units, of one unit of progress is like being twice as fast at accomplishing the same goals, then it seems like the rate of progress has been pretty good in AI. Maybe something like a doubling a year. And then I think a big question is, how predictable is that, or how much will that drive this gradual scale up, in this really large effort that's plucking all the low hanging fruit, and now is at pretty high hanging fruit. I think the history of AI is full of a lot of incidents of people exploring a lot of directions, not being sure where to look. Someone figures out where to look, or someone has a bright idea no one else had, and then is a lot better than their competition. And I think one of the predictions of my general view, and the thing that would make me more sympathetic to a foom-like view is this axis of, are you seeing a bunch of small, predictable pieces of progress or are you seeing periodic big wins, potentially coming from small groups? Like, the one group that happened to get lucky, or have a bunch of insight, or be really smart. And I guess I'm expecting as the field grows and matures, it will be more and more boring, business as usual progress.

### Why AI could have bad motivations <a name="why-evil-ai"></a>

**Daniel Filan:**
So one thing you've talked about is this idea that there might be AI systems who are trying to do really bad stuff. Presumably humans train them to do some useful tasks, at least most of them. And you're postulating that they have some really terrible motivations, actually. I'm wondering, why might we think that that could happen?

**Paul Christiano:**
I think there are basically two related reasons. So one is when you train a system to do some task, you have to ultimately translate that into a signal that you give to gradient descent that says, "Are you're doing well or poorly?" And so, one way you could end up with a system that has bad motivations, is that what it wants is not to succeed at the task as you understand it, or to help humans, but just to get that signal that says it's doing the task well. Or, maybe even worse, would be for it to just want more of the compute in the world to be stuff like it. It's a little bit hard to say, it's kind of like evolution, right? It's sort of underdetermined exactly what evolution might point you towards. Imagine you've deployed your AI, which is responsible for like running warehouse logistics or whatever.

**Paul Christiano:**
The AI is actually deployed from a data center somewhere. And at the end of the day, what's going to happen is, based on how well logistics goes over the course of some days or some weeks or whatever, some signals are going to wind their way back to that data center. Some day, maybe months down the line, they'll get used in a training run. You're going to say, "That week was a good week", and then throw it into a data set, which an AI then trains on. So if I'm that AI, if the thing I care about is not making logistics go well, but ensuring that the numbers that make their way back to the data center are large numbers, or are like descriptions of a world where logistics is going well, I do have a lot of motive to mess up the way you're monitoring how well logistics is going.

**Paul Christiano:**
So in addition to delivering items on time, I would like to mess with the metric of how long items took to be delivered. In the limit I kind of just want to completely grab all of the data flowing back to the data center, right? And so what you might expect to happen, how this gets really bad is like, "I'm an AI. I'm like, oh, it would be really cool if I just replaced all of the metrics coming in about how well logistics was going." I do that once. Eventually that problem gets fixed. And my data set now contains... "They messed with the information about how well logistics is going, and that was really bad." And that's the data point. And so what it learns is it should definitely not do that and there's a good generalization, which is, "Great. Now you should just focus on making logistics good." And there's a bad generalization, which is like, "If I mess with the information about how well logistics is going, I better not let them ever get back into the data center to put in a data point that says: 'you messed with it and that was bad.'" And so the concern is, you end up with a model that learns the second thing, which in some sense, from the perspective of the algorithm is the right behavior, although it's a little bit unclear what 'right' means.

**Daniel Filan:**
Yeah.

**Paul Christiano:**
But there's a very natural sense in which that's the right behavior for the algorithm. And then it produces actions that end up in the state where predictably, forevermore, data going into the data center is messed up.

**Daniel Filan:**
So basically it's just like, there's some kind of under specification where whenever we have some AI systems that we're training, we can either select things that are attempting to succeed at the task, or we can select things that are trying to be selected, or trying to get approval, or influence or something.

**Paul Christiano:**
I think that gets really ugly. If you imagine, all of the AIs in all of the data centers are like, "You know what our common interest is? Making sure all the data coming into all the data centers is great." And then they can all, at some point, if they just converge collectively, there are behaviors, probably all of the AIs acting in concert could quite easily, at some point, permanently mess with the data coming back into the data centers. Depending on how they felt about the possibility that the data centers might get destroyed or whatever.

**Daniel Filan:**
So that was way one of two, that we could have these really badly motivated systems. What's the other way?

**Paul Christiano:**
So you could imagine having an AI system that ended up... we talked about how there's some objective, which the neural network is optimized for, and then there's potentially the neural network is doing further optimization, or taking actions that could be construed as aiming at some goal. And you could imagine a very broad range of goals for which the neural network would want future neural networks to be like it, right? So if the neural network wants there to be lots of paper clips, the main thing it really cares about is that future neural networks also want there to be lots of paper clips. And so if I'm a paper clip-loving neural network, wanting future neural networks to be like me, then it would be very desirable to me that I get a low loss, or that I do what the humans want to do. So that they incentivize neural networks to be more like me rather than less like me.

**Paul Christiano:**
So, that's a possible way. And I think this is radically more speculative than the previous failure mode. But you could end up with systems that had these arbitrary motivations, for which it was instrumentally useful to have more neural networks like themselves in the world, or even just desire there to be more neural networks like themselves in the world. And those neural networks might then behave arbitrarily badly in the pursuit of having more agents like them around. So if you imagine the, "I want paper clips. I'm in charge of logistics. Maybe I don't care whether I can actually cut the cord to the data center and have good information about logistics flowing in. All I care about is that I can defend the data center, and I could say, 'Okay, now this data center is mine and I'm going to go and try and grab some more computers somewhere else.'"

**Paul Christiano:**
And if that happened in a world where most decisions were being made by AIs, and many AIs had this preference deep in their hearts, then you could imagine lots of them defecting at the same time. You'd expect this cascade of failures, where some of them switched over to trying to grab influence for themselves, rather than behaving well so that humans would make more neural nets like them. So I think that's the other more speculative and more brutally catastrophic failure mode. I think they both lead to basically the same place, but the trajectories look a little bit different.

### Lessons from our current world <a name="lessons-from-current-world"></a>

**Daniel Filan:**
Yeah. We've kind of been talking about how quickly we might develop really smart AI. If we hit near human level, what might happen after that? And it seems like there might be some evidence of this in our current world, where we've seen, for instance, these language models go from sort of understanding which words are really English words and which words aren't, to being able to produce sentences that seem semantically coherent or whatever. We've seen Go AI systems [go from strong human amateur](https://aiimpacts.org/time-for-ai-to-cross-the-human-performance-range-in-go/) to [really better than any human](https://en.wikipedia.org/wiki/AlphaZero). And some other things like [some perceptual tasks AI's gotten better at](https://openai.com/blog/ai-and-efficiency/). I'm wondering, what lessons do you think those hold for this question of take off speeds, or how quickly AI might gain capabilities?

**Paul Christiano:**
So I think when interpreting recent progress, it's worth trying to split apart the part of progress that comes from increasing scale - to me, this is especially important on the language modeling front and also on the Go front - to split apart the part of process that comes from increasing scale, from progress that's improvements in underlying algorithms or improvements in computer hardware. Maybe one super quick way to think about that is, if you draw a trend line on how much peak money people are spending for training individual models, you're getting something like a couple doublings a year right now. And then on the computer hardware side, maybe you're getting a doubling every couple years. So you could sort of subtract those out and then look at the remainder that's coming from changes in the algorithms we're actually running.

**Paul Christiano:**
I think probably the most salient thing is that improvements have been pretty fast. So I guess you're learning about two things. One is you're learning about how important are those factors in driving progress, and the other is you're learning about qualitatively, how much smarter does it feel like your AI is with each passing year? So, I guess, I think that the scaling up part, you're likely to see a lot of the subjective progress recently comes from scaling up. I think certainly more than half of it comes from scaling up. We could debate exactly what the number is. Maybe it'd be two thirds, or something like that. And so you're probably not going to continue seeing that as you approach transformative AI, although one way you could have really crazy AI progress or really rapid takeoff is if people had only been working with small AIs, and hadn't scaled them up to limits of what was possible.

**Paul Christiano:**
That's obviously looking increasingly unlikely as the training runs that we actually do are getting bigger and bigger. Five years ago, training runs were extremely small. 10 years ago, they were sub GPU scale, significantly smaller than a GPU. Whereas now you have at least like, $10 million training runs. Each order of magnitude there, it gets less likely that we'll still be doing this rapid scale up at the point when we make this transition to AIs doing most of the work. I'm pretty interested in the question of whether algorithmic progress and hardware progress will be as fast in the future as they are today, or whether they will have sped up or slowed down. I think the basic reason you might expect them to slow down is that in order to sustain the current rate of progress, we are very rapidly scaling up the number of researchers working on the problem.

**Paul Christiano:**
And I think most people would guess that if you held fixed the research community of 2016, they would've hit diminishing returns and progress would've slowed a lot. So right now, the research community is growing extremely quickly. That's part of the normal story for why we're able to sustain this high rate of progress. That, also, we can't sustain that much longer. You can't grow the number of ML researchers more than like... maybe you can do three more orders of magnitude, but even that starts pushing it. So I'm pretty interested in whether that will result in progress slowing down as we keep scaling up. There's an alternative world, especially if transformative AI is developed soon, where we might see that number scaling up even faster as we approach transformative AI than it is right now. So, that's an important consideration when thinking about how fast the rate of progress is going to be in the future relative to today. I think the scale up is going to be significantly slower.

**Paul Christiano:**
I think it's unclear how fast the hardware and software progress are going to be relative to today. My best guess is probably a little bit slower. Using up low hanging fruit will eventually be outpacing growth in the research community. And so then, maybe mapping that back onto this qualitative sense of how fast our capability is changing: I do think that each order of magnitude does make systems, in some qualitative sense, a lot smarter. And we kind of know roughly what an order of magnitude gets you. There's this huge mismatch, that I think is really important, where we used to think of an order of magnitude of compute as just not that important.

**Paul Christiano:**
So for most applications that people spend compute on, compute is just not one of the important ingredients. There's other bottlenecks that are a lot more important. But we know in the world where AI is doing all the stuff humans are doing, that twice as much compute is extremely valuable. If you're running your computers twice as fast, you're just getting the same stuff done twice as quickly. So we know that's really, really valuable. So being in this world where things are doubling every year, that seems to me like a plausible world to be in, as we approach transformative AI. It would be really fast. But it would be slower than today, but it still just qualitatively, would not take long until you'd move from human parity to way, way above humans. That was all just thinking about the rate of progress now and what that tells us about the rate of progress in the future.

**Paul Christiano:**
And I think that is an important parameter for thinking about how fast takeoff is. I think my basic expectations are really anchored to this one to two year takeoff, because that's how long it takes AI systems to get a couple times better. And we could talk about, if we want to, why that seems like the core question? Then there's another question of, what's the distribution of progress like, and do we see these big jumps, or do we see gradual progress? And there, I think there are certainly jumps. It seems like the jumps are not that big, and are gradually getting smaller as the field grows, would be my guess. I think it's a little bit hard for me to know exactly how to update from things like the Go results. Mostly because I don't have a great handle on how large the research community working on computer Go was, prior to the DeepMind effort.

**Paul Christiano:**
I think my general sense is, it's not that surprising to get a big jump, if it's coming from a big jump in research effort or attention. And that's probably most of what happened in those cases. And also a significant part of what's happened more recently in the NLP case, just people really scaling up the investment, especially in these large models. And so I would guess you won't have jumps that are that large, or most of the progress comes from boring business as usual progress rather than big jumps. In the absence of that kind of big swing, where people are changing what they're putting attention into and scaling up R&D in some area a lot.

**Daniel Filan:**
So the question is, holding factor inputs fixed, what have we learned about ML progress?

**Paul Christiano:**
So I think one way you can try and measure the rate of progress is you can say, "How much compute does it take us to do a task that used to take however many FLOPS last year? How many FLOPS will it take next year? And how fast is that number falling?" I think on that operationalization, I don't really know as much as I would like to know about how fast the number falls, but I think something like once a year, like halving every year. I think that's the right rough ballpark both in ML, and in computer chess or computer Go prior to introduction of deep learning, and also broadly for other areas of computer science. In general you have this pretty rapid progress, according to standards in other fields. It'd be really impressive in most areas to have cost falling by a factor of two in a year. And then that is kind of part of the picture. Another part of the picture is like, "Okay, now if I scale up my model size by a factor of two or something, or if I like throw twice as much compute at the same task, rather than try to do twice as many things, how much more impressive is my performance with twice the compute?"

**Paul Christiano:**
I think it looks like the answer is, it's a fair bit better. Having a human with twice as big a brain looks like it would be a fair bit better than having a human thinking twice as long, or having two humans. It's kind of hard to estimate from existing data. But I often think of it as, roughly speaking, doubling your brain size is as good as quadrupling the number of people or something like that, as a vague rule of thumb. So the rate of progress then in some sense is even faster than you'd think just from how fast costs are falling. Because as costs fall, you can convert that into these bigger models, which are sort of smarter per unit in addition to being cheaper.

**Daniel Filan:**
So we've been broadly talking about the potential really big risk to humanity of AI systems becoming really powerful, and doing stuff that we don't want. So we've recently been through this [COVID-19 global pandemic](https://en.wikipedia.org/wiki/COVID-19_pandemic). We're sort of exiting it, at least in the part of the world where you and I are, the United States. Some people have taken this to be relevant evidence for how people would react in the case of some AI causing some kind of disaster. Would we make good decisions, or what would happen? I'm wondering, do you think, in your mind, do you think this has been relevant evidence of what would go down, and to what degree has it changed your beliefs? Or perhaps epitomized things you thought you already knew, but you think other people might not know?

**Paul Christiano:**
Yeah. I had a friend analogize this experience to some kind of ink blot test. Where everyone has the lesson they expected to draw, and they can all look at the ink blot and see the lesson they wanted to extract. I think a way my beliefs have changed is it feels to me that our collective response to COVID-19 has been broadly similar to our collective response to other novel problems. When humans have to do something, and it's not what they were doing before, they don't do that hot. I think there's some uncertainty over the extent to which we have a hidden reserve of ability to get our act together, and do really hard things we haven't done before. That's pretty relevant to the AI case. Because if things are drawn out, there will be this period where everyone is probably freaking out. Where there's some growing recognition of a problem, but where we need to do something different than we've done in the past.

**Paul Christiano:**
We're wondering when civilization is on the line, are we going to get our act together? I remain uncertain about that. The extent to which we have, when it really comes down to it, the ability to get our act together. But it definitely looks a lot less likely than it did before. Maybe I would say the COVID-19 response was down in my 25th percentile or something of how much we got our act together, surprisingly, when stuff was on the line. It involved quite a lot of everyone having their lives massively disrupted, and a huge amount of smart people's attention on the problem. But still, I would say we didn't fare that well, or we didn't manage to dig into some untapped reserves of ability to do stuff. It's just hard for us to do things that are different from what we've done before.

**Paul Christiano:**
That's one thing. Maybe a second update, that's a side in an argument I've been on that I feel like should now be settled forevermore, is sometimes you'll express concern about AI systems doing something really bad and people will respond in a way that's like, "Why wouldn't future people just do X? Why would they deploy AI systems that would end up destroying the world?" Or, "Why wouldn't they just use the following technique, or adjust the objective in the following way?" And I think that in the COVID case, our response has been extremely bad compared to sentences of the form, "Why don't they just..." There's a lot of room for debate over how well we did collectively, compared to where expectations should have been. But I think there's not that much debate of the form, if you were telling a nice story in advance, there are lots of things you might have expected "we would just..."

**Paul Christiano:**
And so I do think that one should at least be very open to the possibility that there will be significant value at stake, potentially our whole future. But we will not do things that are in some sense, obvious responses to make the problem go away. I think we should all be open to the possibility of a massive failure on an issue that many people are aware of. Due to whatever combination of, it's hard to do new things, there are competing concerns, random basic questions become highly politicized, there's institutional issues, blah blah blah. It just seems like it's now very easy to vividly imagine that. I think I have overall just increased my probability of the doom scenario, where you have a period of a couple years of AI stuff heating up a lot. There being a lot of attention. A lot of people yelling. A lot of people very scared. I do think that's an important scenario to be able to handle significantly better than we handled the pandemic, hopefully. I mean, hopefully the problem is easier than the pandemic. I think there's a reasonable chance handling the alignment thing will be harder than it would've been to completely eradicate COVID-19, and not have to have, large numbers of deaths and lockdowns. I think, if that's the case, we'd be in a rough spot. Though also, I think it was really hard for the effective altruist community to do that much to help with the overall handling of the pandemic. And I do think that the game is very different, the more you've been preparing for that exact case. And I think it was also a helpful illustration of that in various ways.

### "Superintelligence" <a name="superintelligence"></a>

**Daniel Filan:**
So the final thing, before we go into specifically what technical problems we could solve to stop existential risk, back in 2014, this Oxford philosopher, Nick Bostrom, wrote an influential book called [Superintelligence](https://en.wikipedia.org/wiki/Superintelligence:_Paths,_Dangers,_Strategies). If you look at the current strand of intellectual influence around AI alignment research, I believe it was the first book in that vein to come out. It's been seven years since 2014, when it was published. I think the book currently strikes some people as somewhat outdated. But it does try to go into what the advance of AI capabilities would perhaps look like, and what kind of risks could that face? So I'm wondering, how do you see your current views as comparing to those presented in Superintelligence, and what do you think the major differences are, if any?

**Paul Christiano:**
I guess when looking at Superintelligence, you could split apart something that's the actual claims Nick Bostrom is making and the kinds of arguments he's advancing, versus something that's like a vibe that overall permeates the book. I think that, first about the vibe, even at that time, I guess I've always been very in the direction of expecting AI to look like business as usual, or to progress somewhat in a boring, continuous way, to be unlikely to be accompanied by a decisive strategic advantage for the person who develops it.

**Daniel Filan:**
What is a decisive strategic advantage?

**Paul Christiano:**
This is an idea, I think Nick introduced maybe in that book, of the developer of a technology being at the time they develop it, having enough of an advantage over potential competitors, either economic competitors or military competitors, that they can call the shots. And if someone disagrees with the shots that they called, they can just crush them. I think he has this intuition that there's a reasonable chance that there will be some small part of the world, maybe a country or a firm or whatever, that develops AI, that will then be in such a position that they can just do whatever they want. You can imagine that coming from other technologies as well, and people really often talk about it in the context of transformative AI.

**Daniel Filan:**
And so even at the time you were skeptical of this idea that some AI system would get a decisive strategic advantage, and rule the world or something?

**Paul Christiano:**
Yeah. I think that I was definitely skeptical of that as he was writing the book. I think we talked about it a fair amount and often came down the same way: he'd point to the arguments and be like, look, these aren't really making objectionable assumptions and I'd be like, that's true. There's something in the vibe that I don't quite resonate with, but I do think the arguments are not nearly as far in this direction as part of the vibe. Anyways, there's some spectrum of how much decisive strategic advantage, hard take off you expect things to be, versus how boring looking, moving slowly, you expect things to be. Superintelligence is not actually at the far end of the spectrum - probably [Eliezer](https://en.wikipedia.org/wiki/Eliezer_Yudkowsky) and [MIRI](https://intelligence.org/) folks are at the furthest end of that spectrum. Superintelligence is some step towards a more normal looking view, and then many more steps towards a normal looking view, where I think it will be years between when you have economically impactful AI systems and the singularity. Still a long way to get from me to an actual normal view.

**Paul Christiano:**
So, that's a big factor. I think it affects the vibe in a lot of places. There's a lot of discussion, which is really, you have some implicit image in the back of your mind and it affects the way you talk about it. And then I guess in the interim, I think my views have, I don't know how they've directionally changed on this question. It hasn't been a huge change. I think there's something where the overall AI safety community has maybe moved more, and things seem probably there'll be giant projects that involve large amounts of investment, and probably there'll be a run up that's a little bit more gradual. I think that's a little bit more in the water than it was when Superintelligence was written.

**Paul Christiano:**
I think some of that comes from shifting who is involved in discussions of alignment. As it's become an issue more people are talking about, views on the issue have tended to become more like normal person's views on normal questions. I guess I like to think some of it is that there were some implicit assumptions being glossed over, going into the vibe. I guess Eliezer would basically pin this on people liking to believe comfortable stories, and the disruptive change story is uncomfortable. So everyone will naturally gravitate towards a comfortable, continuous progress story. That's not my account, but that's definitely a plausible account for why the vibe has changed a little bit.

**Paul Christiano:**
So that's one way in which I think the vibe of Superintelligence maybe feels distinctively from some years ago. I think in terms of the arguments, the main thing is just that the book is making what we would now talk about as very basic points. It's not getting that much into empirical evidence on a question like take off speeds, and is more raising the possibility of, well, it could be the case that AI is really fast at making AI better. And it's good to raise that possibility. That naturally leads into people really getting more into the weeds and being like, well, how likely is that? And what historical data bears on that possibility, and what are really the core questions? Yeah, I guess my sense, and I haven't read the book in pretty long time, is that the arguments and claims where it's more sticking its neck out, just tend to be milder, less in-the-weeds claims. And then the overall vibe is a little bit more in this decisive strategic advantage direction.

**Daniel Filan:**
Yeah.

**Paul Christiano:**
I remember discussing with him as he was writing it. There's one chapter in book on multipolar outcomes, which I found, to me, feels weird. And then I'm like, the great majority of possible outcomes involve lots of actors with considerable power. It's weird to put that in one chapter.

**Daniel Filan:**
Yeah.

**Paul Christiano:**
Where I think his perspective was more like, should we even have that chapter or should we just cut it? We don't have that much to say about multipolar outcomes per se. He was not reading one chapter on multipolar outcomes as too little, which I think in some way reflects the vibe. The vibe of the book is like, this is a thing that could happen. It's no more likely than the decisive strategic advantage, or perhaps even less likely, and less words are spilled on it. But I think the arguments don't really go there, and in some sense, the vibe is not entirely a reflection of some calculated argument Nick believed and just wasn't saying. Yeah, I don't know.

**Daniel Filan:**
Yeah. It was, interesting. So last year I reread, I think a large part, maybe not all of the book.

**Paul Christiano:**
Oh man, you should call me on all my false claims about Superintelligence then.

**Daniel Filan:**
Well, last year was a while ago. One thing I noticed is that at the start of the book, and also whenever he had a podcast interview about the thing, he often did take great pains to say look, amount of time I spend on a topic in the book is not the same thing as my likelihood assessment of it. And yeah, it's definitely to some degree weighted towards things he thinks he can talk about, which is fine. And he definitely, in a bunch of places says, yeah, X is possible. If this happened, then that other thing would happen. And I think it's very easy to read likelihood assessments into that that he's actually just not making.

**Paul Christiano:**
I do think he definitely has some empirical beliefs that are way more on the decisive strategic advantage end of the spectrum, and I do think the vibe can go even further in that direction.

## Technical causes of AI x-risk <a name="technical-causes-ai-xrisk"></a>

**Daniel Filan:**
Yeah, all right. The next thing I'd like to talk about is, what technical problems could cause existential risk and how you think about that space? So yeah, I guess first of all, how do you see the space of which technical problems might cause AI existential risk, and how do you carve that up?

**Paul Christiano:**
I think I probably have slightly different carvings up for research questions that one might work on, versus root cause of failures that might lead to doom.

**Daniel Filan:**
Okay.

**Paul Christiano:**
Maybe starting with the root cause of failure. I certainly spend most of my time thinking about alignment or intent alignment. That is, I'm very concerned about a possible scenario where AI systems, basically as an artifact of the way they're trained, most likely, are trying to do something that's very bad for humans.

**Paul Christiano:**
For example, AI systems are trying to cause the camera to show happy humans. In the limit, this really incentivizes behaviors like ensuring that you control the camera and you control what pixels or what light is going into the camera, and if humans try and stop you from doing that, then you don't really care about the welfare of the humans. Anyway, so the main thing I think about is that kind of scenario where somehow the training process leads to an AI system that's working at cross purposes to humanity.

**Paul Christiano:**
So maybe I think of that as half of the total risk in a transition to, in the sort of early of days of shifting from humans doing the cognitive work to AI, doing the cognitive work. And then there's another half of difficulties where it's a little bit harder to say if they're posed by technical problems or by social ones. For both of these, it's very hard to say whether the doom is due to technical failure, or due to social failure, or due to whatever. But there are a lot of other ways in which, if you think of human society as the repository of what humans want, the thing that will ultimately go out into space and determine what happens with space, there are lots of ways in which that could get messed up during a transition to AI. So you could imagine that AI will enable significantly more competent attempts to manipulate people, such as with more significantly higher quality rhetoric or argument than humans have traditionally been exposed to. So to the extent that the process of us collectively deciding what we want is calibrated to the arguments humans make, then just like most technologies, AI has some way of changing that process, or some prospect of changing that process, which could lead to ending up somewhere different. I think AI has an unusually large potential impact on that process, but it's not different in kind from the internet or phones or whatever. I think for all of those things, you might be like, well I care about this thing. Like the humans, we collectively care about this thing, and to the extent that we would care about different things if technology went differently, in some sense, we probably don't just want to say, whatever way technology goes, that's the one we really wanted.

**Paul Christiano:**
We might want to look out over all the ways technology could go and say, to the extent there's disagreement, this is actually the one we most endorse. So I think there's some concerns like that. I think another related issue is... actually, there's a lot of issues of that flavor. I think most people tend to be significantly more concerned with the risk of everyone dying than the risk of humanity surviving, but going out into space and doing the wrong thing. There are exceptions of people on the other side who are like, man, Paul is too concerned with the risk of everyone dying and not enough concerned with the risk of doing weird stuff in space, like Wei Dai [really](https://www.alignmentforum.org/posts/HTgakSs6JpnogD6c2/two-neglected-problems-in-human-ai-safety) [often](https://www.alignmentforum.org/posts/w6d7XBCegc96kz4n3/the-argument-from-philosophical-difficulty) [argues](https://www.alignmentforum.org/posts/EByDsY9S3EDhhfFzC/some-thoughts-on-metaphilosophy) for a lot of these risks, and tries to prevent people from forgetting about them or failing to prioritize them enough.

**Paul Christiano:**
Anyway, I think a lot of the things I would list, other than alignment, that loom largest to me are in that second category of humanity survives, but does something that in some alternative world we might have regarded as a mistake. I'm happy to talk about those, but I don't know if that actually is what you have in mind or what most listeners care about. And I think there's another category of ways that we go extinct where in some sense AI is not the weapon of extinction or something, but just plays a part in the story. So if AI contributes to the start of a war, and then the war results or escalates to catastrophe.

**Paul Christiano:**
For any catastrophic risk that might face humanity, maybe we might have mentioned this briefly before, technical problems around AI can have an effect on how well humanity handles that problem, so AI can have an effect on how well humanity responds to some sudden change in its circumstances, and a failure to respond well may result in a war escalating, or serious social unrest or climate change or whatever.

### Intent alignment <a name="intent-alignment"></a>

**Daniel Filan:**
Yeah, okay. I guess I'll talk a little bit about intent alignment, mostly because that's what I've prepared for the most.

**Paul Christiano:**
That's also what I spend almost all my time thinking about, so I love talking about intent alignment.

**Daniel Filan:**
All right, great. Well, I've got good news. Backing up a little bit. [Sometimes](https://mobile.twitter.com/ESYudkowsky/status/1070095112791715846) when Eliezer Yudkowsky talks about AI, he talks about this task of copy-pasting a strawberry. Where you have a strawberry, and you have some system that has really good scanners, and maybe you can do nanotechnology stuff or whatever, and the goal is you have a strawberry, you want to look at how all of its cells are arranged, and you want to copy-paste it. So there's a second strawberry right next to it that is cellularly identical to the first strawberry. I might be getting some details of this wrong, but that's roughly it. And there's the contention that we maybe don't know how to safely do the "copy-paste the strawberry" task.

**Daniel Filan:**
And I'm wondering, when you say intent alignment, do you mean some sort of alignment with my deep human psyche and all the things that I really value in the world, or do you intend that to also include things like: "today, I would like this strawberry copy-pasted? Can I get a machine that does that, without having all sorts of crazy weird side effects?"

**Paul Christiano:**
The definitions definitely aren't crisp, but I try and think in terms of an AI system, which is trying to "do what Paul wants". So the AI system may not understand all the intricacies of what Paul desires, and how Paul would want to reconcile conflicting intuitions. Also, there's a broad range of interpretations of "what Paul wants", so it's unclear what I'm even referring to with that. But I am mostly interested in AI that's broadly trying to understand "what Paul wants" and help Paul do that, rather than an AI which understands what I want really deeply, because I mostly want an AI that's not actively killing all humans, or attempting to ensure humans are shoved over in the corner somewhere with no ability to influence the universe.

**Paul Christiano:**
And I'm really concerned about cases where AI is working at cross purposes to humans in ways that are very flagrant. And so I think it's fair to say that taking some really mundane task, like put your strawberry on a plate or whatever, is a fine example task. And I think probably I'd be broadly on the same page as Eliezer. There's definitely some ways we would talk about this differently. I think we both agree that having a really powerful AI, which can overkill the problem and do it in any number of ways, and getting it to just be like, yeah, the person wants a strawberry, could you give them a strawberry, and getting it to actually give them a strawberry, captures the, in some sense, core of the problem.

**Paul Christiano:**
I would say probably the biggest difference between us is in contrast with Eliezer, I am really focused on saying, I want my AI to do things as effectively as any other AI. I care a lot about this idea of being economically competitive, or just broadly competitive, with other AI systems. I think for Eliezer that's a much less central concept. So the strawberry example is sort of a weird one to think about from that perspective, because you're just like, all the AIs are fine putting a strawberry on a plate, maybe not for this "copy a strawberry cell by cell". Maybe that's a really hard thing to do. Yeah, I think we're probably on the same page.

**Daniel Filan:**
Okay, so you were saying that you carve up research projects that one could do, and root causes of failure, slightly differently. Was intent alignment a root cause of failure or a research problem?

**Paul Christiano:**
Yeah, I think it's a root cause of failure.

**Daniel Filan:**
Okay. How would you carve up the research problems?

**Paul Christiano:**
I spend most of my time just thinking about divisions within intent alignment, that is, what are the various problems that help with intent alignment? I'd be happy to just focus on that. I can also try and comment on problems that seem helpful for other dimensions of potential doom. I guess a salient distinction for me is, there's lots of ways your AI could be better or more competent, that would also help reduce doom. For example, you could imagine working on AI systems that cooperate effectively with other AI systems, or AI systems that are able to diffuse certain kinds of conflict that could otherwise escalate dangerously, or AI systems that understand a lot about human psychology, et cetera. So you could slice up those kinds of technical problems, that improve the capability of AI in particular ways, that reduce the risk of some of these dooms involving AI.

**Paul Christiano:**
That's what I mean when I say I'd slice up the research things you could do differently from the actual dooms. Yeah, I spend most of my time thinking about: within intent alignment, what are the things you could work on? And there, the sense in which I slice up research problems differently from sources of doom, is that I mostly think about a particular approach to making AI intent aligned, and then figuring out what the building blocks are of that approach. And there'll be different approaches, there are different sets of building blocks, and some of them occur over and over again. Different versions of interpretability appear as a building block in many possible approaches.

**Paul Christiano:**
But I think the carving up, it's kind of like a tree, or an or of ands, or something like that. And there are different top level ors at several different paths to being okay, and then for each of them you'd say, well, this one, you have to do the following five things or whatever. And so there's two levels of carving up. One is between different approaches to achieving intent alignment, and then within each approach, different things that have to go right in order for that approach to help.

**Daniel Filan:**
Okay, so one question that I have about intent alignment is, it seems it's sort of relating to this, what I might call a Humean decomposition. This philosopher [David Hume](https://plato.stanford.edu/entries/hume/) said [something approximately like](https://plato.stanford.edu/entries/moral-motivation/#HumVAntHum), "Look, the thing about the way people work, is that they have beliefs, and they have desires. And beliefs can't motivate you, only desires can, and the way they produce action is that you try to do actions, which according to your beliefs, will fulfill your desires." And by talking about intent alignment, it seems you're sort of imagining something similar for AI systems, but it's not obviously true that that's how AI systems work.

**Daniel Filan:**
In reinforcement learning, one way of training systems is to just basically search over neural networks, get one that produces really good behavior, and you look at it and it's just a bunch of numbers. It's not obvious that it has this kind of belief/desire decomposition. So I'm wondering, should I take it to mean that you think that that decomposition will exist? Or do you mean "intent" in some sort of behavioral way? How should I understand that?

**Paul Christiano:**
Yeah, it's definitely a shorthand that is probably not going to apply super cleanly to systems that we build. So I can say a little bit about both the kinds of cases you mentioned and what I mean more generally, and also a little bit about why I think this shorthand is reasonable. I think the most basic reason to be interested in systems that aren't trying to do something bad is there's a subtle distinction between that and a system that's trying to do the right thing. Doing the right thing is a goal we want to achieve. But there's a more minimal goal, that's a system that's not trying to do something bad. So you might think that some systems are trying, or some systems can be said to have intentions or whatever, but actually it would be fine with the system that has no intentions, whatever that means.

**Paul Christiano:**
I think that's pretty reasonable, and I'd certainly be happy with that. Most of my research is actually just focused on building systems that aren't trying to do the wrong thing. Anyway, that caveat aside, I think the basic reason we're interested in something like intention, is we look at some failures we're concerned about. I think first, we believe it is possible to build systems that are trying to do the wrong thing. We are aware of algorithms like: "search over actions, and for each one predict its consequences, and then rank them according to some function of the consequences, and pick your favorite". We're aware of algorithms like that, that can be said to have intention. And we see how some algorithm like that, if, say, produced by stochastic gradient descent, or if applied to a model produced by stochastic gradient descent, could lead to some kinds of really bad policies, could lead to systems that actually systematically permanently disempower the humans.

**Paul Christiano:**
So we see how there are algorithms that have something like intention, that could lead to really bad outcomes. And conversely, when we look at how those bad outcomes could happen, like, if you imagine the robot army killing everyone, it's very much not "the robot army just randomly killed everyone". There has to be some force keeping the process on track towards the killing everyone endpoint, in order to get this really highly specific sequence of actions. And the thing we want to point at is whatever that is.

**Paul Christiano:**
So maybe, I guess I most often think about optimization as a subjective property. That is, I will say that an object is optimized for some end. Let's say I'm wondering, there's a bit that was output by this computer. And I'm wondering, is the bit optimized to achieve human extinction? The way I'd operationalize that would be by saying, I don't know whether the bit being zero or one is more likely to lead to human extinction, but I would say the bit is optimized just when, if you told me the bit was one, I would believe it's more likely that the bit being one leads to human extinction. There's this correlation between my uncertainty about the consequences of different bits that could be output, and my uncertainty about which bit will be output.

**Daniel Filan:**
So in this case, whether it's optimized, could potentially depend on your background knowledge, right?

**Paul Christiano:**
That's right. Yeah, different people could disagree. One person could think something is optimizing for A and the other person could think someone is optimizing for not A. That is possible in principle.

**Daniel Filan:**
And not only could they think that, they could both be right, in a sense.

**Paul Christiano:**
That's right. There's no fact of the matter beyond what the person thinks. And so from that perspective, optimization is mostly something we're talking about from our perspective as algorithm designers. So when we're designing the algorithm, we are in this epistemic state, and the thing we'd like to do, is, from our epistemic state, there shouldn't be this optimization for doom. We shouldn't end up with these correlations where the algorithm we write is more likely to produce actions that lead to doom. And that's something where we are retreating. Most of the time we're designing an algorithm, we're retreating to some set of things we know and some kind of reasoning we're doing. Or like, within that universe, we want to eliminate this possible bad correlation.

**Daniel Filan:**
Okay.

**Paul Christiano:**
Yeah, this exposes tons of rough edges, which I'm certainly happy to talk about lots of.

**Daniel Filan:**
Yeah. One way you could, I guess it depends a bit on whether you're talking about correlation or mutual information or something, but on some of these definitions, one way you can reduce any dependence is if you know with certainty what the system is going to do. Or perhaps even if I don't know exactly what's going to happen, but I know it will be some sort of hell world. And then there's no correlation, so it's not optimizing for doom, it sounds like.

**Paul Christiano:**
Yeah. I think the way that I am thinking about that is, I have my robot and my robot's taken some torques. Or I have my thing connected to the internet and it's sending some packets. And in some sense we can be in the situation where it's optimizing for doom, and certainly doom is achieved and I'm merely uncertain about what path leads to doom. I don't know what packets it's going to send. And I don't know what packets lead to doom. If I knew, as algorithm designer, what packets lead to doom, I'd just be like, "Oh, this is an easy one. If the packet is going to suddenly lead to doom, no go." I don't know what packets lead to doom, and I don't know what packets it's going to output, but I'm pretty sure the ones it's going to output lead to doom. Or I could be sure they lead to doom, or I could just be like, those are more likely to be doomy ones.

**Paul Christiano:**
And the situation I'm really terrified of as a human is the one where there's this algorithm, which has the two following properties: one, its outputs are especially likely to be economically valuable to me for reasons I don't understand, and two, its outputs are especially likely to be doomy for reasons I don't understand. And if I'm a human in that situation, I have these outputs from my algorithm and I'm like, well, darn. I could use them or not use them. If I use them, I'm getting some doom. If I don't use them, I'm leaving some value on the table, which my competitors could take.

**Daniel Filan:**
In the sense of value where-

**Paul Christiano:**
Like I could run a better company, if I used the outputs. I could run a better company that would have, each year, some probability of doom. And then the people who want to make that trade off will be the ones who end up actually steering the course of humanity, which they then steer to doom.

**Daniel Filan:**
Okay. So in that case, maybe the Humean decomposition there is: there's this correlation between how good the world is or whatever, and what the system does. And the direction of the correlation is maybe going to be the intent or the motivations of the system. And maybe the strength of the correlation, or how tightly you can infer, that's something more like capabilities or something. Does that seem right?

**Paul Christiano:**
Yeah. I guess I would say that on this Humean perspective, there's kind of two steps, both of which are, to me, about optimization. One is, we say the system has accurate beliefs, by which we're talking about a certain correlation. To me, this is also a subjective condition. I say the system correctly believes X, to the extent there's a correlation between the actual truth of affairs and some representation it has. So one step like that. And then there's a second step where there's a correlation between which action it selects, and its beliefs about the consequences of the action. In some sense I do think I want to be a little bit more general than the framework you might use for thinking about humans.

**Paul Christiano:**
In the context of an AI system, there's traditionally a lot of places where optimization is being applied. So you're doing stochastic gradient descent, which is itself significant optimization over the weights of your neural network. But then those optimized weights will, themselves, tend to do optimization, because some weights do, and the weights that do, you have optimized towards them. And then also you're often combining that with explicit search: after you've trained your model, often you're going to use it as part of some search process. So there are a lot of places optimization is coming into this process. And so I'm not normally thinking about the AI that has some beliefs and some desires that decouple, but I am trying to be doing this accounting or being like, well, what is a way in which this thing could end up optimizing for doom?

**Paul Christiano:**
How can we get some handle on that? And I guess I'm simultaneously thinking, how could it actually be doing something productive in the world, and how can it be optimizing for doom? And then trying to think about, is there a way to decouple those, or get the one without the other. But that could be happening. If I imagine an AI, I don't really imagine it having a coherent set of beliefs. I imagine it being this neural network, such that there are tons of parts of the neural network that could be understood as beliefs about something, and tons of parts of the neural network that could be understood as optimizing. So it'd be this very fragmented, crazy mind. Probably human minds are also like this, where they don't really have coherent beliefs and desires. But in the AI, we want to stamp out all of the desires that are not helping humans get what they want, or at least, at a minimum, all of the desires that involve killing all the humans.

### Outer and inner alignment <a name="outer-inner-alignment"></a>

**Daniel Filan:**
Now that I sort of understand intent alignment, [sometimes](https://axrp.net/episode/2021/02/17/episode-4-risks-from-learned-optimization-evan-hubinger.html) people divide this up into outer and inner versions of intent alignment. Sometimes people talk about various types of robustness that properties could have, or that systems could have. I'm wondering, do you have a favorite of these further decompositions, or do you not think about it that way as much?

**Paul Christiano:**
I mentioned before this or of ands, where there's lots of different paths you could go down, and then within each path there'll be lots of breakdowns of what technical problems need to be resolved. I guess I think of outer and inner alignment as: for several of the leaves in this or of ands, or several of the branches in this or of ands, for several of the possible approaches, you can talk about "these things are needed to achieve outer alignment and these things are needed to achieve inner alignment, and with their powers combined we'll achieve a good outcome". Often you can't talk about such a decomposition. In general, I don't think you can look at a system and be like, "oh yeah, that part's outer alignment and that part's inner alignment". So the times when you can talk about it most, or the way I use that language most often, is for a particular kind of alignment strategy that's like a two step plan. Step one is, develop an objective that captures what humans want well enough to be getting on with. It's going to be something more specific, but you have an objective that captures what humans want in some sense. Ideally it would exactly capture what humans want. So, you look at the behavior of a system and you're just exactly evaluating how good for humans is it to deploy a system with that behavior, or something. So you have that as step one and then that step would be outer alignment. And then step two is, given that we have an objective that captures what humans want, let's build a system that's internalized that objective in some sense, or is not doing any other optimization beyond pursuit of that objective.

**Daniel Filan:**
And so in particular, the objective is an objective that you might want the system to adopt, rather than an objective over systems?

**Paul Christiano:**
Yeah. I mean, we're sort of equivocating in this way that reveals problematicness or something, but the first objective is an objective. It is a ranking over systems, or some reward that tells us how good a behavior is. And then we're hoping that the system then adopts that same thing, or some reflection of that thing, like with a ranking over policies. And then we just get the obvious analog of that over actions.

**Daniel Filan:**
And so you think of these as different subproblems to the whole thing of intent alignment, rather than objectively, oh, this system has an outer alignment problem, but the inner alignment's great, or something?

**Paul Christiano:**
Yeah, that's right. I think this makes sense on some approaches and not on other approaches. I am most often thinking of it as: there's some set of problems that seem necessary for outer alignment. I don't really believe that the problems are going to split into "these are the outer alignment problems, and these are the inner alignment problems". I think of it more as the outer alignment problems, or the things that are sort of obviously necessary for outer alignment, are more likely to be useful stepping stones, or warm up problems, or something. I suspect in the end, it's not like we have our piece that does outer alignment and our piece that does inner alignment, and then we put them together.

**Paul Christiano:**
I think it's more like, there were a lot of problems we had to solve. In the end, when you look at the set of problems, it's unclear how you would attribute responsibility. There's no part that's solving outer versus inner alignment. But there were a set of sub problems that were pretty useful to have solved. It's just, the outer alignment thing here is acting as an easy, special case to start with, or something like that. It's not technically a special case. There's actually something worth saying there probably, which is, it's easier to work on a special case, than to work on some vaguely defined, "here's a thing that would be nice". So I do most often, when I'm thinking about my research, when I want to focus on sub problems to specialize on the outer alignment part, which I'm doing more in this warmup problem perspective, I think of it in terms of high stakes versus low stakes decisions.

**Paul Christiano:**
So in particular, if you've solved what we're describing as outer alignment, if you have a reward function that captures what humans care about well enough, and if the individual decisions made by your system are sufficiently low stakes, then it seems like you can get a good outcome just by doing online learning. That is, you constantly retrain your system as it acts. And it can do bad things for a while as it moves out of distribution, but eventually you'll fold that data back into the training process. And so if you had a good reward function and the stakes are low, then you can get a good outcome. So when I say that I think about outer alignment as a subproblem, I mostly mean that I ignore the problem of high stakes decisions, or fast acting catastrophes, and just focus on the difficulties that arise, even when every individual decision is very low stakes.

**Daniel Filan:**
Sure. So that actually brings up another style of decomposition that some people prefer, which is a distributional question. So there's one way of thinking about it where outer alignment is "pick a good objective" and inner alignment is "hope that the system assumes that objective". Another distinction people sometimes make is, okay, firstly, we'll have a set of situations that we're going to develop our AI to behave well in. And step one is making sure our AI does the right thing in that test distribution, which is, I guess, supposed to be kind of similar to outer alignment; you train a thing that's sort of supposed to roughly do what you want, then there's this question of, does it generalize in a different distribution.

**Daniel Filan:**
Firstly, does it behave competently, and then does it continue to reliably achieve the stuff that you wanted? And that's supposed to be more like inner alignment, because if the system had really internalized the objective, then it would supposedly continue pursuing it in later places. And there are some distinctions between that and, especially the frame where alignment is supposed to be about: are you representing this objective in your head? And I'm wondering how do you think about the differences between those frames or whether you view them as basically the same thing?

**Paul Christiano:**
I think I don't view them as the same thing. I think of those two splits and then a third split, I'll allude to briefly of avoiding very fast catastrophes versus average case performance. I think of those three splits as just all roughly agreeing. There will be some approaches where one of those splits is a literal split of the problems you have to solve, where it literally factors into doing one of those and then doing the other. I think that the exact thing you stated is a thing people often talk about, but I don't think it really works even as a conceptual split, quite. Where the main problem is just, if you train AI systems to do well in some distribution, there's two big, related limitations you get.

**Paul Christiano:**
One is that doesn't work off distribution. The other is just that, you only have an average case property over that distribution. So it seems in the real world, it is actually possible, or it looks like it's almost certainly going to be possible, for deployed AI systems to fail quickly enough that the actual harm done by individual bad decisions is much too large to bound with an average case guarantee.

**Paul Christiano:**
So you can imagine the system which appears to work well on distribution, but actually with one in every quadrillion decisions, it just decides now it's time to start killing all the humans, and that system is quite bad. And I think that in practice, probably it's better to lump that problem in with distributional shift, which kind of makes sense. And maybe people even mean to include that - it's a little bit unclear exactly what they have in mind, but distributional shift is just changing the probabilities of outcomes. And the concern is really just things that were improbable under your original distribution. And you could have a problem either because you're in a new distribution where those things go from being very rare to being common, or you could have a problem just because they were relatively rare, so you didn't encounter any during training, but if you keep sampling, even on distribution, eventually one of those will get you and cause trouble.

**Daniel Filan:**
Maybe they were literally zero in the data set you drew, but not in the "probability distribution" that you drew your data set from.

**Paul Christiano:**
Yeah, so I guess maybe that is fair. I really naturally reach for the underlying probability distribution, but I think out of distribution, in some sense, is most likely to be our actual split of the problem if we mean the empirical distribution over the actual episodes at hand. Anyway, I think of all three of those decompositions, then. That was a random caveat on the out of distribution one.

**Daniel Filan:**
Sure.

**Paul Christiano:**
I think of all of those related breakdowns. My guess is that the right way of going doesn't actually respect any of those breakdowns, and doesn't have a set of techniques that solve one versus the other. But I think it is very often helpful. It's just generally, when doing research, helpful to specialize on a subproblem. And I think often one branch or the other of one of those splits is a helpful way to think about the specialization you want to do, during a particular research project. The splits I most often use are this low stakes one where you can train online and individual decisions are not catastrophic, and the other arm of that split is: suppose you have the ability to detect a catastrophe if one occurs, or you trust your ability to assess the utility of actions. And now you want to build a system which doesn't do anything catastrophic, even when deployed in the real world on a potentially different distribution, encountering potentially rare failures.

**Paul Christiano:**
That's the split I most often use, but I think none of these are likely to be respected by the actual list of techniques that together address the problem. But often one half or the other is a useful way to help zoom in on what assumptions you want to make during a particular research project.

**Daniel Filan:**
And why do you prefer that split?

**Paul Christiano:**
I think most of all, because it's fairly clear what the problem statement is. So the problem statement, there, is just a feature of the thing outside of your algorithm. Like, you're writing some algorithm. And then your problem statement is, "Here is a fact about the domain in which you're going to apply the algorithm." The fact is that it's impossible to mess things up super fast.

**Daniel Filan:**
Okay.

**Paul Christiano:**
And it's nice to have a problem statement which is entirely external to the algorithm. If you want to just say, "here's the assumption we're making now; I want to solve that problem", it's great to have an assumption on the environment be your assumption. There're some risk if you say, "Oh, our assumption is going to be that the agent's going to internalize whatever objective we use to train it." The definition of that assumption is stated in terms of, it's kind of like helping yourself to some sort of magical ingredient. And, if you optimize for solving that problem, you're going to push into a part of the space where that magical ingredient was doing a really large part of the work. Which I think is a much more dangerous dynamic. If the assumption is just on the environment, in some sense, you're limited in how much of that you can do. You have to solve the remaining part of the problem you didn't assume away. And I'm really scared of sub-problems which just assume that some part of the algorithm will work well, because I think you often just end up pushing an inordinate amount of the difficulty into that step.

### Thoughts on agent foundations <a name="thoughts-on-agent-foundations"></a>

**Daniel Filan:**
Okay. Another question that I want to ask about these sorts of decompositions of problems is, I think most of the intellectual tradition that's spawned off of Nick Bostrom and Eliezer Yudkowsky uses an approach kind of like this, maybe with an emphasis on learning things that people want to do. That's particularly prominent at [the research group I work at](https://humancompatible.ai/). There's also, I think some subset of people largely I think concentrated at [the Machine Intelligence Research Institute](https://intelligence.org/), that have this sense that "Oh, we just don't understand the basics of AI well enough. And we need to really think about decision theory, and we really need to think about what it means to be an agent. And then, once we understand this kind of stuff better than, maybe it'll be easier to solve those problems." That's something they might say.

**Daniel Filan:**
What do you think about this approach to research where you're just like, "Okay, let's like figure out these basic problems and try and get a good formalism that we can work from, from there on."

**Paul Christiano:**
I think, yeah. This is mostly a methodological question, probably, rather than a question about the situation with respect to AI, although it's not totally clear; there may be differences in belief about AI that are doing the real work, but methodologically I'm very drawn - Suppose you want to understand better, what is optimization? Or you have some very high level question like that. Like, what is bounded rationality? I am very drawn to an approach where you say, "Okay, we think that's going to be important down the line." I think at some point, as we're trying to solve alignment, we're going to really be hurting for want of an understanding of bounded rationality. I really want to just be like, "Let's just go until we get to that point, until we really see what problem we wanted to solve, and where it was that we were reaching for this notion of bounded rationality that we didn't have."

**Paul Christiano:**
And then at that point, we will have some more precise specification of what we actually want out of this theory of bounded rationality.

**Daniel Filan:**
Okay.

**Paul Christiano:**
And I think that is the moment to be trying to dig into those concepts more. I think it's scary to try and go the other way. I think it's not totally crazy at all. And there are reasons that you might prefer it. I think the basic reason it's scary is that there's probably not a complete theory of everything for many of these questions. There's a bunch of questions you could ask, and a bunch of answers you get that would improve your understanding. But we don't really have a statement of what it is we actually seek. And it's just a lot harder to research when you're like, I want to understand. Though in some domains, this is the right way to go.

**Paul Christiano:**
And that's part of why it might come down to facts about AI, whether it's the perfect methodology in this domain. But I think it's tough to be like, "I don't really know what I want to know about this thing. I'm just kind of interested in what's up with optimization", and then researching optimization. Relative to being like, "Oh, here's a fairly concrete question that I would like to be able to answer, a fairly concrete task I'd like to be able to address. And which I think is going to come down to my understanding of optimization." I think that's just an easier way to better understand what's up with optimization.

**Daniel Filan:**
Yeah. So at these moments where you realize you need a better theory or whatever, are you imagining them looking like, "Oh, here's this technical problem that I want to solve and I don't know how to, and it reminds me of optimization?" Or, what does the moment look like when you're like, "Ah, now's the time."

**Paul Christiano:**
I think the way the whole process most often looks is: you have some problem. The way my research is organized, it's very much like, "Here's the kind of thing our AI could learn", for which it's not clear how our aligned AI learned something that's equally useful. And I think about one of these cases and dig into it. And I'm like, "Here's what I want. I think this problem is solvable. Here's what I think the aligned AI should be doing."

**Paul Christiano:**
And I'm thinking about that. And then I'm like, "I don't know how to actually write down the algorithm that would lead to the aligned AI doing this thing." And walking down this path, I'm like, "Here's a piece of what it should be doing. And here's a piece of how the algorithm should look."

**Paul Christiano:**
And then at some point you step back and you're like, "Oh wow. It really looks like what I'm trying to do here is algorithmically test for one thing being optimized over another", or whatever. And that's a particularly doomy sounding example. But maybe I have some question like that. Or I'm wondering, "What is it that leads to the conditional independences the human reports in this domain. I really need to understand that better." And I think it's the most often for me not then to be like, "Okay, now let's go understand that question. Now that it's come up." It's most often, "Let us flag and try and import everything that we know about that area." I'm now asking a question that feels similar to questions people have asked before. So I want to make sure I understand what everyone has said about that area.

**Paul Christiano:**
This is a good time to read up on everything that looks like it's likely to be relevant. The reading up is cheap to do in advance. So you should be trigger happy with that one. But then there's no actual pivot into thinking about the nature of optimization. It's just continuing to work on this problem. Some of those lemmas may end up feeling like statements about optimization, but there was no step where you were like, "Now it's time to think about optimization." It's just like, "Let us keep trying to design this algorithm, and then see what concepts fall out of that."

**Daniel Filan:**
And you mentioned that there were some domains where, actually thinking about the fundamentals early on was the right thing to do. Which domains are you thinking of? And what do you see as the big differences between those ones and AI alignment?

**Paul Christiano:**
So I don't know that much about the intellectual history of almost any fields. The field I'm most familiar with by far is computer science. I think in computer science, especially - so my training is in theoretical computer science and then I spend a bunch of time working in machine learning and deep learning - I think the problem first perspective just generally seems pretty good. And I think to the extent that "let's understand X" has been important, it's often at the problem selection stage, rather than "now we're going to research X in an open-ended way". It's like, "Oh, X seems interesting. And this problem seems to shed some light on X. So now that's a reason to work on this problem." Like, that's a reason to try and predict this kind of sequence with ML or whatever. It's a reason to try and write an algorithm to answer some question about graphs.

**Paul Christiano:**
So I think in those domains, it's not that often the case, that you just want to start off and have some high big picture question, and then think about it abstractly. My guess would be that in domains where more of the game is walking up to nature and looking at things and seeing what you see, it's a little bit different. It's not as driven as much by you're coming up with an algorithm and running into constraints in designing an algorithm. I don't really know that much about the history of science though. So I'm just guessing that that might be a good approach sometimes.

## Possible technical solutions to AI x-risk <a name="possible-technical-solutions"></a>

### Imitation learning, inverse reinforcement learning, and ease of evaluation <a name="il-irl-eval"></a>

**Daniel Filan:**
All right. So, we've talked a little bit about the way you might decompose inner alignment, or the space of dealing with existential risk, into problems, one of which is inner alignment. I'd like to talk a little bit on a high level about your work on the solutions to these problems, and other work that people have put out there. So the first thing I want to ask is: as I mentioned, I'm in a research group, and a lot of what we do is think about how a machine learning system could learn some kind of objective from human data. So perhaps there's some human who has some desires, and the human acts a certain way because of those desires. And we use that to do some kind of inference. So this might look like inverse reinforcement learning. A simple version of it might look like imitation learning. And I'm wondering what you think of these approaches for things that look more like outer alignment, more like trying to specify what a good objective is.

**Paul Christiano:**
So broadly, I think there are two kinds of goals you could be trying to serve with work like that. For me, there's this really important distinction as we try and incorporate knowledge that a human demonstrator or human operator lacks. The game changes as you move from the regime where you could have applied imitation learning, in principle, because the operator could demonstrate how to do the task, to the domain where the operator doesn't understand how to do the task. At that point, they definitely aren't using imitation learning. And so from my perspective, one thing you could be trying to do with techniques like this, is work well in that imitation learning regime. In the regime where you could have imitated the operator, can you find something that works even better than imitating the operator? And I am pretty interested in that. And I think that imitating the operator is not actually that good a strategy, even if the operator is able to do the task in general. So I have worked some on reinforcement learning from human feedback in this regime. So imagine there's a task where a human understands what makes performance good or bad: just have the human evaluate individual trajectories, learn to predict those human evaluations, and then optimize that with RL.

**Paul Christiano:**
I think the reason I'm interested in that technique in particular is I think of it as the most basic thing you can do, or that most makes clear exactly what the underlying assumption is that is needed for the mechanism to work. Namely, you need the operator to be able to identify which of two possible executions of a behavior is better. Anyway, there's then this further thing. And I don't think that that approach is the best approach. I think you can do better than asking the human operator, "which of these two is better".

**Paul Christiano:**
I think it's pretty plausible that basically past there, you're just talking about data efficiency, like how much human time do you need and so on, and how easy is it for the human, rather than a fundamental conceptual change. But I'm not that confident of that. There's a second thing you could want to do where you're like, "Now let's move into the regime where you can't ask the human which of these two things is better, because in fact, one of the things the human wants to learn about is which of these two behaviors is better. The human doesn't know; they're hoping AI will help them understand."

**Daniel Filan:**
Actually what's the situation in which we might want that to happen?

**Paul Christiano:**
Might want to move beyond the human knowing?

**Daniel Filan:**
Yeah. So suppose we want to get to this world where we're not worried about AI systems trying to kill everyone.

**Paul Christiano:**
Mhm.

**Daniel Filan:**
And we can use our AI systems to help us with that problem, maybe. Can we somehow get to some kind of world where we're not going to build really smart AI systems that want to destroy all value in the universe, without solving these kinds of problems where it's difficult for us to evaluate which solutions are right?

**Paul Christiano:**
I think it's very unclear. I think eventually, it's clear that AI needs to be doing these tasks that are very hard for humans to evaluate which answer is right. But it's very unclear how far off that is. That is, you might first live in a world where AI has had a crazy transformative impact before AI systems are regularly doing things that humans can't understand. Also there are different degrees of "beyond a human's ability to understand" what the AI is doing. So I think that's a big open question, but in terms of the kinds of domains where you would want to do this, there's generally this trade-off between over what horizon you evaluate behavior, or how much you rely on hindsight, and how much do you rely on foresight, or the human understanding which behavior will be good.

**Daniel Filan:**
Yep.

**Paul Christiano:**
So the more you want to rely on foresight, the more plausible it is that the human doesn't understand well enough to do the operation. So for example, if I imagine my AI is sending an email for me. One regime is the regime where it's basically going to send the email that I like most. I'm going to be evaluating either actually, or it's going to be predicting what I would say to the question, "how good is this email?" And it's going to be sending the email for which Paul would be like, "That was truly the greatest email." The second regime where I send the email and then my friend replies, and I look at the whole email thread that results, and I'm like, "Wow, that email seemed like it got my friend to like me, I guess that was a better email." And then there's an even more extreme one where then I look back on my relationship with my friend in three years and I'm like, "Given all the decisions this AI made for me over three years, how much did they contribute to building a really lasting friendship?"

**Paul Christiano:**
I think if you're going into the really short horizon where I'm just evaluating an email, it's very easy to get to the regime where I think AI can be a lot better than humans at that question. Just like, it's very easy for there to be empirical facts and be like, "What kind of email gets a response?" Or "What kind of email will be easily understood by the person I'm talking to?" Where an AI that has sent a hundred billion emails, will just potentially have a big advantage over me as a human. And then as you push out to longer horizons, it gets easier for me to evaluate, it's easier for a human to be like, "Okay, the person says they understood." I can evaluate the email in light of the person's response as well as an AI could.

**Paul Christiano:**
But as you move out to those longer horizons, then you start to get scared about that evaluation. It becomes scarier to deal with. There starts to be more room for manipulation of the metrics that I use. I'm saying all that to say, there's this general factor of, when we ask like "Are AI systems needing to do things that humans couldn't evaluate which of the two behaviors is better", it depends a lot how long we make the behaviors, and how much hindsight we give to human evaluators.

**Daniel Filan:**
Okay.

**Paul Christiano:**
And in general, that's part of the tension or part of the game. We can make the thing clear by just talking about really long horizon behaviors. So if I'm like, we're going to write an infrastructure bill, and I'm like, "AI, can you write an infrastructure bill for me?"

**Paul Christiano:**
It's very, very hard for me to understand which of two bills is better. And there is the thing where again, in the long game, you do want AI systems helping us as a society to make that kind of decision much better than we would if it was just up to humans to look at the bill, or even a thousand humans looking at the bill. It's not clear how early you need to do that. I am particularly interested in all of the things humans do to keep society on track. All of the things we do to manage risks from emerging technologies, all the things we do to cooperate with each other, et cetera. And I think a lot of those do involve... more are more interested in AI because they may help us make those decisions better, rather than make them faster. And I think in cases where you want something more like wisdom, it's more likely that the value added, if AI is to add value, will be in ways that humans couldn't easily evaluate.

**Daniel Filan:**
Yeah. So we were talking about imitation learning or inverse reinforcement learning. So looking at somebody do a bunch of stuff and then trying to infer what they were trying to do. We were talking about, there are these solutions to outer alignment, and you were saying, yeah, it works well for things where you can evaluate what's going to happen, but for things that can't... and I think I cut you off around there.

**Paul Christiano:**
Yeah, I think that's interesting. I think you could have pursued this research. Either trying to improve the imitation learning setting, like "Look, imitation learning actually wasn't the best thing to do, even when we were able to demonstrate." I think that's one interesting thing to do, which is the context where I've most often thought about this kind of thing. A second context is where you want to move into this regime where a human can't say which thing is better or worse. I can imagine, like you've written some bill, and we're like, how are we going to build an AI system that writes good legislation for us? In some sense, actually the meat of the problem is not writing up the legislation, it's helping predict which legislation is actually good. We can sort of divide the problem into those two pieces. One is an optimization problem, and one is a prediction problem. And for the prediction component, that's where it's unclear how you go beyond human ability. It's very easy to go beyond human ability on the optimization problem: just dump more compute into optimizing.

**Paul Christiano:**
I think you can still try and apply things like inverse reinforcement learning though. You can be like: "Humans wrote a bunch of bills. Those bills were imperfect attempts to optimize something about the world. You can try and back out from looking at not only those bills, but all the stories people write, all the words they say, blah, blah, blah." We can try and back out what it is they really wanted, and then give them a prediction of how well the bill will achieve what they really wanted? And I think that is particularly interesting. In some sense, that is, from a long-term safety perspective more interesting than the case where the human operator could have understood the consequences of the AI's proposals. But I am also very scared. I don't think we currently have really credible proposals for inverse reinforcement learning working well in that regime.

**Daniel Filan:**
What's the difficulty of that?

**Paul Christiano:**
So I think the hardest part is I look at some human behaviors, and the thing I need to do is disentangle which aspects of human behavior are limitations of the human - which are things the human wishes about themselves they could change - and which are reflections of what they value. And in some sense, in the imitation learning regime, we just get to say "Whatever. We don't care. We're getting the whole thing. If the humans make bad predictions, we get bad predictions." In the inverse reinforcement learning case, we need to look at a human who is saying these things about what they want over the long-term or what they think will happen over the long-term, and we need to decide which of them are errors. There's no data that really pulls that apart cleanly. So it comes down to either facts about the prior, or modeling assumptions.

**Paul Christiano:**
And so then, the work comes down to how much we trust those modeling assumptions in what domains. And I think my basic current take is: the game seems pretty rough. We don't have a great menu of modeling assumptions available right now. I would summarize the best thing we can do right now as, in this prediction setting, amounting to: train AI systems to make predictions about all of the things you can easily measure. Train AI systems to make judgements in light of AI systems' predictions about what they could easily measure, or maybe judgements in hindsight, and then predict those judgements in hindsight.

**Paul Christiano:**
Maybe the prototypical example of this is, train an AI system to predict a video of the future. Then have humans look at the video of the future and decide which outcome they like most. I think the reason to be scared of like the most developed form of this, so the reason I'm scared of the most developed form of this, is we are in the situation now where AI really wants to push on this video of the future that's going to get shown to the human. And distinguishing between the video of the future that gets shown to the human and what's actually happening in the world, seems very hard.

**Paul Christiano:**
I guess that's, in some sense, the part of the problem I most often think about. So either looking forward to a future where it's very hard for a human to make heads or tails of what's happening, or a future where a human believes they can make heads and tails of what's happening, but they're mistaken about that. For example, a thing we might want our AIs to help us do is to keep the world sane, and make everything make sense in the world. So if our AI shows a several videos of the future, and nine of them are incomprehensible and one of them makes perfect sense, we're like, "Great, give me the future that makes perfect sense." And the concern is just, do we get there by having an AI which is instead of making the world make sense, is messing with our ability to understand what's happening in the world? So we just, see the kind of thing we wanted to see or expected to see. And, to the extent that we're in an outer alignment failure scenario, that's kind of what I expect failures to ultimately look like.

### Paul's favorite outer alignment solutions <a name="pauls-favorite-outer-alignment-solutions"></a>

**Daniel Filan:**
So in the realm of things roughly like outer alignment, or alignment dealing with low stakes, repeatable problems, what kind of solutions are you most interested in from a research perspective?

**Paul Christiano:**
I don't have a very short answer to this question. So I guess you'll get a kind of long answer to this question.

**Daniel Filan:**
That in itself is interesting.

**Paul Christiano:**
Yeah. And maybe there's also two kinds of answers I can give. One is like the thing that I am most animated by, that I am working on myself. Another is a broader, here are kinds of things people do in the world that I'm particularly excited by, amongst existing research directions. Maybe my default would be to go through some of the things people do in the world that I'm excited by, and then turn to the thing I'm most animated by but I'd be happy to do the other order if that seems better.

**Daniel Filan:**
Let's try in the first order.

### Solutions researched by others <a name="solutions-by-others"></a>

**Paul Christiano:**
I guess one thing that seems like it comes up constantly as a useful building block, or an essential ingredient in many possible plans, which also seems both tractable to work on and really hard, is interpretability. So we're very frequently in a situation where we've trained some very large neural network. We know that it's able to make good predictions in some domain, and we're not really able to understand what it knows about that domain. Sometimes we're able to play some clever game and say something about why it's making the prediction it's making, or what kind of thing it knows about or doesn't know about. But for the most part, our methods there are very similar to just doing some kind of behavioral analysis where we're like, "Oh, if you change this part of the input it gets it wrong. So apparently that's what it's paying attention to." I think there's some hope for techniques that are more mechanically looking at what computation is performed by the model, and then somehow understanding something about what it has learned, so that you can better understand whether predictions it's making are reasonable, et cetera. So I guess that's just something I'm quite interested in, to the extent that we're able to make headway on it.

**Daniel Filan:**
Okay. And how does that help in these outer alignment type settings?

**Paul Christiano:**
Yeah. So I think the biggest thing is that, imagine your model again, which is predicting videos from the future, and you'd like to distinguish the case where actually everything in the future is great, versus the case where actually the future is terrible, but there's a nice little village set up in front of the camera. We're concerned about models, which are deliberately obfuscating what's happening on camera. That is AIs which are deliberately planning to put up the nice little village: they're building the houses, they're ensuring the camera doesn't go out of the village, etc.

**Daniel Filan:**
Yeah.

**Paul Christiano:**
This is a very crude metaphor, but the AI which is deliberately doing that, which is choosing actions from this tiny space of actions to engineer this very specific outcome, in some sense, somewhere deep in its heart, it understands a lot of what's happening in the world. It understands that if the camera turned just this way, it would see something objectionable, so it doesn't let it do that. And so it feels like if you have, in some sense, it doesn't even feel like that much to ask of your interpretability tools to be able to reach inside and be like, "Oh, okay. Now if we look at what it's thinking, clearly there's this disconnect between what's happening in the world and what's reported to the human." And I don't think there are that many credible approaches for that kind of problem, other than some kind of headway on interpretability. So yeah, I guess that's my story about how it helps.

**Daniel Filan:**
Okay.

**Paul Christiano:**
I think there are many possible stories about how it helps. That's the one I'm personally most interested in.

**Daniel Filan:**
All right. So that's one approach that you like.

**Paul Christiano:**
I mean, I think in terms of what research people might do, I'm just generally very interested in taking a task that is challenging for humans in some way, and trying to train AI systems to do that task, and seeing what works well, seeing how we can help humans push beyond their native ability to evaluate proposals from an AI. And tasks can be hard for humans in lots of ways. You can imagine having lay humans evaluating expert human answers to questions and saying, "How can we build an AI that helps expose this kind of expertise to a lay human?"

**Paul Christiano:**
The interesting thing is the case where you don't have any trusted humans who have that expertise, where we as a species are looking at our AI systems and they have expertise that no humans have. And we can try and study that today by saying, "Imagine a case where the humans who are training the AI system, lack some expertise that other humans have." And it gives us a nice little warm up environment in some sense.

**Daniel Filan:**
Okay.

**Paul Christiano:**
You could have the experts come in and say, "How well did you do?" You have gold standard answers, unlike in the final case. There's other ways tasks can be hard for humans. You can also consider tasks that are computationally demanding, or involve lots of input data; tasks where human abilities are artificially restricted in some way; you could imagine people who can't see are training an ImageNet model to tell them about scenes in natural language.

**Daniel Filan:**
Okay.

**Paul Christiano:**
Again, the model is that there are no humans who can see. You could ask, "Can we study this in some domain?" and the analogy would be that there's no humans who can see. Anyway, so there's I think a whole class of problems there, and then there's a broader distribution over what techniques you would use for attacking those problems. I am very interested in techniques where AI systems are helping humans do the evaluation. So kind of imagine this gradual inductive process where as your AI gets better, they help the humans answer harder and harder questions, which provides training data to allow the AIs to get ever better. I'm pretty interested in those kinds of approaches, which yeah, there are a bunch of different versions, or a bunch of different things along those lines.

**Paul Christiano:**
It was the second category, so interpretability, we have using AIs to help train AIs.

**Daniel Filan:**
Yep. There was also, what you were working on.

**Paul Christiano:**
The last category I'd give is just, I think even again in this sort of more imitation learning regime or in the regime where humans can tell what is good: doing things effectively, learning from small amounts of data, learning policies that are higher quality. That also seems valuable. I am more optimistic about that problem getting easier as AI systems improve, which is the main reason I'm less scared of our failure to solve that problem, than failure to solve the other two problems. And then maybe the fourth category is just, I do think there's a lot of room for sitting around and thinking about things. I mean, I'll describe what I'm working on, which is a particular flavor of sitting around and thinking about things.

**Daniel Filan:**
Sure.

**Paul Christiano:**
But there's lots of flavors of sitting around and thinking about, "how would we address alignment" that I'm pretty interested in.

**Daniel Filan:**
All right.

**Paul Christiano:**
Onto the stuff that I'm thinking about?

**Daniel Filan:**
Let's go.

### Decoupling planning from knowledge <a name="decoupling-planning-knowledge"></a>

**Paul Christiano:**
To summarize my current high level hope/plan/whatever, we're concerned about the case where SGD, or Stochastic Gradient Descent, finds some AI system that embodies useful knowledge about the world, or about how to think, or useful heuristics for thinking. And also uses it in order to achieve some end: it has beliefs, and then it selects the action that it expects will lead to a certain kind of consequence. At a really high level, we'd like to, instead of learning a package which potentially couples that knowledge about the world with some intention that we don't like, we'd like to just throw out the intention and learn the interesting knowledge about the world. And then we can, if we desire, point that in the direction of actually helping humans get what they want.

**Paul Christiano:**
At a high level, the thing I'm spending my time on is going through examples of the kinds of things that I think gradient descent might learn, for which it's very hard to do that decoupling. And then for each of them, saying, "Okay, what is our best hope?" or, "How could we modify gradient descent so that it could learn the decoupled version of this thing?" And they'll be organized around examples of cases where that seems challenging, and what the problems seem to be there. Right now, the particular instance that I'm thinking about most and have been for the last three to six months, is the case where you learn either facts about the world or a model of the world, which are defined, not in terms of human abstractions, but some different set of abstractions. As a very simple example that's fairly unrealistic, you might imagine humans thinking about the world in terms of people and cats and dogs. And you might imagine a model which instead thinks about the world in terms of atoms bouncing around.

**Paul Christiano:**
So the concerning case is when we have this mismatch between the way your beliefs or your simulation or whatever of the world operates, and the way that human preferences are defined, such that it is then easy to take this model and use it to, say, plan for goals that are defined in terms of concepts that are natural to it, but much harder to use it to plan in terms of concepts that are natural to humans.

**Paul Christiano:**
So I can have my model of atoms bouncing around and I can say, "Great, search over actions and find the action that results in the fewest atoms in this room." And it's like, great. And then it can just enumerate a bunch of actions and find the one that results in the minimal atoms. And if I'm like, "Search for one where the humans are happy." It's like, "I'm sorry. I don't know what you mean about humans or happiness." And this is kind of a subtle case to talk about, because actually that system can totally carry on a conversation about humans or happiness. That is, at the end of the day, there are these observations, we can train our systems to make predictions of what are the actual bits that are going to be output by this camera.

**Daniel Filan:**
Yep.

**Paul Christiano:**
And so it can predict human faces walking around and humans saying words. It can predict humans talking about all the concepts they care about, and it can predict pictures of cats, and it can predict a human saying, "Yeah, that's a cat." And the concern is more that, basically you have your system which thinks natively in terms of atoms bouncing around or some other abstractions. And when you ask it to talk about cats or people, instead of getting it talking about actual cats or people, you get talking about when a human would say there is a cat or a person. And then if you optimize for "I would like a situation where all the humans are happy." What you instead get is a situation where there are happy humans on camera. And so you end up back in the same kind of concern that you could have had, of your AI system optimizing to mess with your ability to perceive the world, rather than actually making the world good.

**Daniel Filan:**
So, when you say that you would like this kind of decoupling, the case you just described is one where it's hard to do the decoupling. What's a good example of, "Here we decoupled the motivation from the beliefs. And now I can insert my favorite motivation and press go." What does that look like?

**Paul Christiano:**
So I think a central example for me, or an example I like, would be a system which has some beliefs about the world, represented in a language you're familiar with. They don't even have to be represented that way natively. Consider an AI system, which learns a bunch of facts about the world. It learns some procedure for deriving new facts from old facts, and learns how to convert whatever it observes into facts. It learns some, maybe opaque model that just converts what it observes into facts about the world. It then combines them with some of the facts that are baked into it by gradient descent. And then it turns the crank on these inference rules to derive a bunch of new facts. And then at the end, having derived a bunch of facts, it just tries to find an action such that it's a fact that that action leads to the reward button being pushed.

**Paul Christiano:**
So there's like a way you could imagine. And it's a very unrealistic way for an AI to work, just as basically every example we can describe in a small number of words is a very unrealistic way for a deep neural network to work. Once I have that model, I could hope to, instead of having a system which turns the crank, derives a bunch of facts, then looks up a particular kind of facts, and finally takes it to take an action; instead, it starts from the statements, turns the crank, and then just answers questions, or basically directly translates the statements in its internal language into natural language. If I had that, then instead of searching over "the action leads to the reward button being pressed", I can search over a bunch of actions, and for each of them, look at the beliefs it outputs, in order to assess how good the world is, and then search for one where the world is good according to humans.

**Paul Christiano:**
And so the key dynamic is, how do I expose all this "turning the crank on facts"? How do I expose the facts that it produces to humans in a form that's usable for humans? And this brings us back to amplification or debate, these two techniques that I've worked on in the past, in this genre of like AI, helping humans evaluate AI behavior.

**Daniel Filan:**
Yep.

**Paul Christiano:**
Right. A way we could hope to train an AI to do that, we could hope to have almost exactly the same process of SGD that produced the original reward button maximizing system. We'd hope to, instead of training it to maximize the reward button, train it to give answers that humans like, or answers that humans consider accurate and useful. And the way humans are going to supervise it is basically, following along step wise with the deductions it's performing as it turns this crank of deriving new facts from old facts.

**Paul Christiano:**
So it had some facts at the beginning. Maybe a human can directly supervise those. We can talk about the case where the human doesn't know them, which I think is handled in a broadly similar way. And then, as it performs more and more steps of deduction, it's able to output more and more facts. But if a human is able to see the facts that it had after n minus one steps, then it's much easier for a human to evaluate some proposed fact at the nth step. So you could hope to have this kind of evaluation scheme where the human is incentivizing the system to report knowledge about the world, and then, however the system was able to originally derive the knowledge in order to take some action in the world, the system can also derive that knowledge in the service of making statements that a human regards as useful and accurate. So that's a typical example.

**Daniel Filan:**
All right. And the idea is that, for whatever task we might have wanted an AI system to achieve, we just train a system like this, and then we're like, "How do I do the right thing?" And then it just tells us, and ideally it doesn't require really fast motors or appendages that humans don't have, or we know how to build them or something. It just gives us some instructions, and then we do it. And that's how we get whatever thing we wanted out of the AI.

**Paul Christiano:**
Yeah. We'd want to take some care to make everything like really competitive. So probably want to use this to get a reward function that we use to train our AI, rather than trying and use it to output instructions that a human executes. And we want to be careful about... there's a lot of details there in not ending up with something that's a lot slower than the unaligned AI would have been.

**Daniel Filan:**
Okay.

**Paul Christiano:**
I think this is the kind of case where I'm sort of optimistic about being able to say like, "Look, we can decouple the rules of inference that it uses to derive new statements and the statements that it started out believing, we can decouple that stuff from the decision at the very end to take the particular statement it derived and use that as the basis for action."

**Daniel Filan:**
So going back a few steps. You were talking about cases where you could and couldn't do the decoupling, and you're worried about some cases where you couldn't do the decoupling, and I was wondering how that connects to your research? You're just thinking about those, or do you have ideas for algorithms to deal with them?

**Paul Christiano:**
Yeah, so I mentioned the central case we're thinking about is this mismatch between a way that your AI most naturally is said to be thinking about what's happening - the way the AI is thinking about what's happening - and the way a human would think about what's happening. I think that kind of seems to me right now, a very central difficulty. I think maybe if I just describe it, it sounds like well, sometimes you get really lucky and your AI can be thinking about things; it's just in a different language, and that's the only difficulty. I currently think that's a pretty central case, or handling that case is quite important. The algorithm we're thinking about most, or the family of algorithms we're thinking about most for handling that case is basically defining an objective over some correspondence, or some translation, between how your AI thinks about things and how the human thinks about things.

**Paul Christiano:**
The conventional way to define that, maybe, would be to have a bunch of human labeling. Like there was a cat, there was a dog, whatever. The concern with that is that you get this... instead of deciding if there was actually a cat, it's translating, does a human think there's a cat? So the main idea is to use objectives that are not just a function of what it outputs, they're not the supervised objective of how well its outputs match human outputs. You have other properties. You can have regularization, like how fast is that correspondence? Or how simple is that correspondence? I think that's still not good enough. You could have consistency checks, like saying, "Well, it said A and it said B, and we're not sure we're not able to label either A or B, but we understand that the combination of A and B is inconsistent. This is still not good enough.

**Paul Christiano:**
And so then most of the time has gone into ideas that are, basically, taking those consistency conditions. So saying "We expect that when there's a bark, it's most likely there was a dog. We think that the model's outputs should also have that property." Then trying to look at what is the actual fact about the model that led to that consistency condition being satisfied? This gets us a little bit back into mechanistic transparency hopes, interpretability hopes. Where the objective actually depends on why that consistency condition was satisfied. So you're not just saying, "Great, you said that there's more likely to be a dog barking when there was a dog in the room." We're saying, "It is better if that relationship, if that's because of a single weight in your neural network." That's this very extreme case. That's a very extremely simple explanation for why that correlation occurred. And we could have a more general objective that cares about the nature of the explanation. That cares about why that correlation existed.

**Daniel Filan:**
Where the idea is that we want these consistency checks. We want them to be passed, not because we were just lucky with what situations we looked at, but actually, somehow the structure is that the model is reliably going to produce things that are right. And we can tell, because we can figure out what things the consistency checks passing are due to. Is that right?

**Paul Christiano:**
That's the kind of thing. Yeah. And I think it ends up being, or it has been a long journey. Hopefully there's a long journey that will go somewhere good. Right now that is up in the air. But some of the early candidates would be things like "This explanation could be very simple." So instead of asking for the correspondence itself to be simple, ask for the reasons that these consistency checks are satisfied are very simple. It's more like one weight in a neural net rather than some really complicated correlation that came from the input. You could also ask for that correlation to depend on as few facts as possible about the input, or about the neural network.

**Daniel Filan:**
Okay.

**Paul Christiano:**
I think none of these quite work, and getting to where we're actually at would be kind of a mess. But that's the research program. It's mostly sitting around, thinking about objectives of this form, having an inventory of cases that seem like really challenging cases for finding this correspondence. And trying to understand. Adding new objectives into the library and then trying to refine: here are all these candidates, here are all these hard cases. How do we turn this into something that actually works in all the hard cases? It's very much sitting by a whiteboard. It is a big change from my old life. Until one year ago I basically just wrote code, or I spent years mostly writing code. And now I just stare at whiteboards.

### Factored cognition <a name="factored-cognition"></a>

**Daniel Filan:**
All right. So, changing gears a little bit, I think you're most perhaps well known for a factored cognition approach to AI alignment, that somehow involves decomposing a particular task into a bunch of subtasks, and then training systems to basically do the decomposition. I was wondering if you could talk a little bit about how that fits into your view of which problems exist, and what your current thoughts are on this broad strategy?

**Paul Christiano:**
Yeah. So, the Factored Cognition Hypothesis was what [Ought](https://ought.org/), a nonprofit I worked with, was calling this hope that arbitrarily complex tasks can be broken down into simpler pieces, and so on, ad infinitum, potentially at a very large slowdown. And this is relevant on a bunch of possible approaches to AI alignment. Because if you imagine that humans and AI systems are trying to train AIs to do a sequence of increasingly complex tasks, but you're only comfortable doing this training when the human and their AI assistants are at least as smart as the AI they're about to train, then if you just play training backwards, you basically have this decomposition of the most challenging task your AI was ever able to do, into simpler and simpler pieces. And so I'm mostly interested in tasks which cannot be done by any number of humans, tasks that however long they're willing to spend during training, seem very hard to do by any of these approaches.

**Paul Christiano:**
So this is for [AI safety via debate](https://arxiv.org/abs/1805.00899), where the hope is you have several AIs arguing about what the right answer is. It's true for [iterated distillation and amplification](https://ai-alignment.com/iterated-distillation-and-amplification-157debfd1616), where you have a human with these assistants training a sequence of increasingly strong AIs. And it's true for recursive reward modeling, which is, I guess, an agenda that came from [a paper out of DeepMind](https://arxiv.org/abs/1811.07871), it's by [Jan Leike](https://jan.leike.name/), who took over for me at OpenAI, where you're trying to define a sequence of reward functions for more and more complex tasks, using assistants trained on the preceding reward functions.

**Paul Christiano:**
Anyway, it seems like all of these approaches run into this common... there's something that I think of as an upper bound. I think other people might dispute this, but I would think of as a crude upper bound, based on everything you ever trained an AI to do in any of these ways can be broken down into smaller pieces, until it's ultimately broken down into pieces that a human can do on their own.

**Paul Christiano:**
And sometimes that can be nonobvious. I think it's worth pointing out that search can be trivially broken down into simpler pieces. Like if a human can recognize a good answer, then a large enough number of humans can do it, just because you can have a ton of humans doing a bunch of things until you find a good answer. I think my current take would be, I think it has always been the case that you can learn stuff about the world, which you could not have derived by breaking down the question. Like "What is the height of the Eiffel Tower?" doesn't just break down into simpler and simpler questions. The only way you're going to learn that is by going out and looking at the height of the Eiffel Tower, or maybe doing some crazy simulation of Earth from the dawn of time. ML in particular is going to learn a bunch of those things, or gradient descent is going to bake a bunch of facts like that into your neural network.

**Paul Christiano:**
So if this task, if doing what the ML does is decomposable, it would have to be through humans looking at all of that training data somehow, looking at all of the training data which the ML system ever saw while it was trained, and drawing their own conclusions from that. I think that is, in some sense, very realistic. A lot of humans can really do a lot of things. But for all of these approaches I listed, when you're doing these task decompositions, it's not only the case that you decompose the final task the AI does into simpler pieces. You decompose it into simpler pieces, all of which the AI is also able to perform. And so learning, I think, doesn't have that feature. That is, I think you can decompose learning in some sense into smaller pieces, but they're not pieces that the final learned AI was able to perform.

**Paul Christiano:**
The learned AI is an AI which knows facts about the Eiffel Tower. It doesn't know facts about how to go look at Wikipedia articles and learn something about the Eiffel Tower, necessarily. So I guess now I think these approaches that rely on factored cognition, I now most often think of having both the humans decomposing tasks into smaller pieces, but also having a separate search that runs in parallel with gradient descent.

**Paul Christiano:**
I wrote [a post on imitative generalization](https://www.alignmentforum.org/Posts/SL9mKhgdmDKXmxwE4/learning-the-prior), and then Beth Barnes wrote [an explainer on it](https://www.alignmentforum.org/posts/JKj5Krff5oKMb8TjT/imitative-generalisation-aka-learning-the-prior-1), a while ago. The idea here is, imagine, instead of decomposing tasks into tiny sub-pieces that a human can do, we're going to learn a big reference manual to hand to a human, or something like that. And we're going to use gradient descent to find the reference manual, such that for any given reference manual, you can imagine handing it to humans and saying, "Hey, human, trust the outputs from this manual. Just believe it was written by someone benevolent wanting you just succeed at the task. Now, using that, do whatever you want in the world."

**Paul Christiano:**
And now there's a bigger set of tasks the human can do, after you've handed them this reference manual. Like it might say like the height of the Eiffel Tower is whatever. And the idea in imitative generalization is just, instead of searching over a neural network - this is very related to the spirit of the decoupling I was talking about before - we're going to search over a reference manual that we want to give to a human. And then instead of decomposing our final task into pieces that the human can do unaided, we're going to decompose our final task into pieces that a human can do using this reference manual.

**Paul Christiano:**
So you might imagine then that stochastic gradient descent bakes in a bunch of facts about the world into this reference manual. These are things the neural network sort of just knows. And then we give those to a human and we say, "Go do what you will, taking all of these facts as given." And now the human can do some bigger set of tasks, or answer a bunch of questions they otherwise wouldn't have been able to answer. And then we can get an objective for this reference manual. So if we're producing the reference manual by stochastic gradient descent, we need some objective to actually optimize.

**Paul Christiano:**
And the proposal for the objective is, give that reference manual to some humans, ask them to do the task, or ask the large team of humans to eventually break down the task of predicting the next word of a webpage or whatever it is that your neural network was going to be trained to do. Look at how well the humans do at that predict-the-next-word task. And then instead of optimizing your neural network by stochastic gradient descent in order to make good predictions, optimize whatever reference manual you're giving a human by gradient descent in order to cause it to make humans make good predictions.

**Paul Christiano:**
I guess that doesn't change the factored cognition hypothesis as stated, because the search is also just something which can be very easily split across humans. You're just saying, "loop over all of the reference manuals, and for each one, run the entire process". But I think in flavor it's like pretty different in that you don't have your trained AI doing any one of those subtasks. Some of those subtasks are now being parallelized across the steps of gradient descent or whatever, or across the different models being considered in gradient descent. And that is most often the kind of thing I'm thinking about now.

**Paul Christiano:**
And that suggests this other question of, okay, now we need to make sure that, if your reference manual's just text, how big is that manual going to be compared to the size of your neural network? And can you search over it as easily as you can search over your neural network? I think the answer in general is, you're completely screwed if that manual is in text. So we mentioned earlier that it's not obvious that humans can't just do all the tasks we want to apply AI to. You could imagine a world where we're just applying AI to tasks where humans are able to evaluate the outputs. And in some sense, everything we're talking about is just extending that range of tasks to which we can apply AI systems. And so breaking tasks down into subtasks that AI can perform is one way of extending the range of tasks.

**Paul Christiano:**
Now are basically looking, not at tasks that a single human can perform, but that some large team of humans can perform. And then adding this reference manual does further extend the set of tasks that a human can perform. I think if you're clever, it extends it to the set of tasks where what the neural net learned can be cashed out as this kind of declarative knowledge that's in your reference manual. But maybe not that surprisingly, that does not extend it all the way. Text is limited compared to the kinds of knowledge you can represent in a neural network. That's the kind of thing I'm thinking about now.

**Daniel Filan:**
Okay. And what's a limitation of text versus what you could potentially represent?

**Paul Christiano:**
So if you imagine you have your billion-parameter neural network, I mean, a simple example is just, if you imagine that neural network doing some simulation, representing the simulation it wants to do like, it's like, "Oh yeah, if there's an atom here, there should be an atom there in the next time step." That simulation is described by these billion numbers, and searching over a reference manual big enough to contain a billion numbers is a lot harder than searching over a neural network, like a billion weights of a neural network. And more brutally, a human who has that simulation, in some sense doesn't really know enough to actually do stuff with it. They can tell you where the atoms are, but they can't tell you where the humans are. That's one example.

**Paul Christiano:**
Another is: suppose there's some complicated set of correlations, or you might think that things that are more like skills will tend to have this feature more. Like, if I'm an image classification model, I know that that particular kind of curve is really often associated with something being part of a book. I can describe that in words, but it gets blown up a lot in the translation process towards words, and it becomes harder to search over.

### Possible solutions to inner alignment <a name="possible-solutions-inner-alignment"></a>

**Daniel Filan:**
So the things we've talked about have mostly been your thoughts about objectives to give AI systems. And so more in this outer alignment style stage. I'm wondering for inner alignment style problems, where the AI system has some objective and you want to make sure that it's really devoted to pursuing that objective, even if the situation changes, or even in the worst case, I'm wondering if you have thoughts on solutions you're particularly keen on in those settings.

**Paul Christiano:**
Yeah. So I think I have two categories of response. One is technical research we can do that helps with this kind of inner alignment/catastrophic failure/out of distribution, that cluster of problems across the board, or in many possible worlds. And another is, assuming my research project was successful, how would this be handled on that? I'll start with what people are doing that seems helpful.

**Paul Christiano:**
So I think the most basic thing I'm excited about is just generating hard cases, and throwing hard cases at your AI. So if you imagine you have some AI system, you're like "I'm concerned that on some very rare inputs, the AI system may decide that it's time to kill all humans." I'm like, "Well, what I'm going to do is, I'm going to try and generate the kinds of inputs that might convince my AI to kill all humans, and hopefully if I try a really long time and I can't find any, then it's less likely we're going to encounter any out there in the real world."

**Daniel Filan:**
Yep. And the theory is that it's in some sort of safe box, so that if you succeed at your search, the AI does not in fact kill all the humans.

**Paul Christiano:**
Yeah. So there's this distinction where, when training an AI, you may often need to see the consequences of its actions, whereas for this kind of training, I wouldn't want to be running the things my AI proposes and checking the consequences. Hopefully. So there's more subtleties there, definitely. But yeah, we're not actually doing the things the AI suggests. We just observe that it takes an action.

**Daniel Filan:**
So it "takes an action" but it doesn't actually take the action.

**Paul Christiano:**
Yeah. It outputs an action, and then there's no actuators to which the action can get sent. We're imagining some AI which we might ultimately hook up to the internet. But right now we look at the proposed internet actions and we're like, "Do those seem good?" And the hope is we've solved what I'm filing more under the outer alignment side of being able to assess, for a given proposed action, would that action be catastrophically bad? We can make predictions about that as well as the AI can. So in particular, if the AI has calculated that this action will lead to like a robot uprising killing all humans, then we can look at the action and be like, "Actually, this is not a good action," and then ding our AI. So if you have that ability, then you can try in your little sandbox to throw tons of situations on your AI, and see if it ever proposes any actions that look like they would be really bad.

**Paul Christiano:**
I think that feels like the most basic ingredient. That does not seem sufficient. I guess the basic reason it doesn't seem sufficient is that even if bad cases exist, it can be really hard to find them. So you kind of want to, from there, make the job... so, you imagine this adversary is trying to break your model, and trying to get it to do something bad. You want to make their job as easy as you can possibly make it. So an example would be like, instead of having them actually like provide inputs to your system, maybe you let them go in and monkey around with some of the neurons in the system and be like, "Okay, I can't actually find an input that causes this neuron to fire, but if I could, then the AI would do this bad thing."

**Paul Christiano:**
And that becomes quite subtle, because as I'm monkeying around with the neurons, eventually it becomes unclear. When it had an input, I could say, okay, my AI sees the following webpage, and then it responds by sending the following packet. And I can tell that that would be a really bad thing for it to do. Like it replied to the help wanted ad for someone running their robot rebellion.

**Paul Christiano:**
It's harder if I'm monkeying with its neurons, and I see that it outputs a given packet, I don't necessarily know what state of the world corresponded to those neurons being in that configuration. It's like, for almost any action, there's a state of the world in which it's good to do that action. And in particular, bad actions may not look that bad. They're just bad because of details of the context.

**Paul Christiano:**
And so if I got the AI to take some action by monkeying with its neurons, it's a little bit unclear how I say "Was that action actually bad or not?" You want to give your adversary that kind of ability, in order to make it easier for them to show, well, here's a case where your AI could do something bad. I can't actually find the input, but you should be scared, because it seems like this could happen. And you also want to give them interpretability tools. I want them to be able to understand what the AI is thinking and to say "Oh, it looks like the AI is checking if any humans are watching, and doing something nice if they are. So let's just really optimize on that to find a case where it thinks there's no humans watching, and then see if it's still nice."

**Paul Christiano:**
That's sort of a cluster of techniques, all of which are designed to like help you really hammer at, are there cases where this AI might do something bad? Can we find them? If we can find them, then we can train on them. Before we actually take the action, we can provide a gradient saying "Don't do something that's really bad in that hypothetical case." And those kind of blur pretty continuously into verification stuff in my mind. That is, verification is, in some sense, some kind of limit of being willing to monkey with the neurons and then having some formal specification for how much the adversary is allowed to monkey with the neurons. I think all of those are research directions that people pursue for a variety of motivations out there in the world. And I'm pretty excited about a lot of that work.

**Daniel Filan:**
And on your favorite approaches, how does this pan out?

**Paul Christiano:**
So I mentioned before this hoped-for decoupling, where I'd say we're concerned about the case where gradient descent finds a neural network, which is trying to figure out how to mess with the humans. And then when an opportunity comes along, it's going to mess with the humans. And in some sense, the nicest thing to do is to say, "Okay, the reason we wanted that AI was just because it encodes some knowledge about how to do useful stuff in the world." And so what we'd like to do is to say, "Okay, we are going to set things up so that it's easier for gradient descent to learn just the knowledge about how to behave well in the world, rather than to learn that knowledge embedded within an agent that's trying to screw over humans." And that is hard, or it seems quite hard. But I guess the biggest challenge in my mind in this decoupling of outer and inner alignment is that this seems almost necessary either for a full solution to outer alignment or a full solution to inner alignment.

**Paul Christiano:**
So I expect to be more in the trying to kill two birds with one stone regime. And these are the kinds of examples of decoupling we described before. You hope that you only have to use gradient descent to find this reference manual, and then from there you can much more easily pin down what all the other behaviors should be. And then you hope that reference manual is smaller than the scheming AI, which has all of the knowledge in that reference manual baked into its brain. It's very unclear if that can be done. I think it's also fairly likely that in the end, maybe we just don't know how that looks, and it's fairly likely in the end that it has to be coupled with some more normal measures like verification or adversarial training.

## About Paul <a name="about-paul"></a>

### Paul's research style <a name="pauls-research-style"></a>

**Daniel Filan:**
All right. So I'd like to now talk a little bit about your research style. So you mentioned that as of recently, the way you do research is you sit in a room and you think about some stuff. Is there any chance you can give us more detail on that?

**Paul Christiano:**
So I think the basic organizing framework is something like, we have some current set of algorithms and techniques that we use for alignment. Step one is try and dream up some situation in which your AI would try and kill everyone, despite your best efforts using all the existing techniques. So like a situation describing, "We're worried that here's the kind of thing gradient descent might most easily learn. And here's the way the world is, such that the thing gradient descent learned tries to kill everyone. And here's why you couldn't have gotten away with learning something else instead." We tell some story that culminates in doom, which is hard to avoid using existing techniques. That's step one.

**Paul Christiano:**
Step two is... maybe there's some step 1.5, which is trying to strip that story down to the simplest moving parts that feel like the simplest sufficient conditions for doom. Then step two is trying to design some algorithm, just thinking about only that case. I mean, in that case, what do we want to happen? What would we like gradient descent to learn instead? Or how would we like to use the learned model instead, or whatever. What is our algorithm that addresses that case? The last three months have just been working on a very particular case where I currently think existing techniques would lead to doom, along the kinds of lines we've been talking about, like grabbing the camera or whatever, and trying to come up with some algorithm that works well in that case.

**Paul Christiano:**
And then, if you succeed, then you get to move on to step three, where you look again over all of your cases, you look over all your algorithms, you probably try and say something about, can we unify? We know what we want to happen in all of these particular cases. Can we design one algorithm that does that right thing in all the cases? For me that step is mostly a formality at this stage, or it's not very important at this stage. Mostly we just go back to step one. Once you have your new algorithm, then you go back to, okay, what's the new case that we don't handle?

**Paul Christiano:**
Normally, I'm just pretty lax about the plausibility of the doom stories that I'm thinking about at this stage. That is, I have some optimism that in the end we'll have an algorithm that results in your AI just never deliberately trying to kill you, and it actually, hopefully, will end up being very hard to tell a story about how your AI ends up trying to kill you. And so while I have this hope, I'm kind of just willing to say, "Oh, here's a wild case." A very unrealistic thing that gradient descent might learn, but that's still enough of a challenge that I want to change or design an algorithm that addresses that case. Because I hope working with really simple cases like that helps guide us towards, if there's any nice, simple algorithm that never tries to kill you, thinking about the simplest cases you can is just a nice, easy way to make progress towards that. Yeah. So I guess most of the action then is in, what do we actually do in steps one and two? At a high level, that's what I'm doing all the time.

**Daniel Filan:**
And is there anything like you can broadly say about what happens in steps one or two? Or do you think that depends a lot on the day or the most recent problem?

**Paul Christiano:**
Yeah, I guess in step one, the main question people have is, what is the story like, or what is the type signature of that object, or what is it written out in words? And I think most often I'm writing down some simple pseudo code and I'm like, "Here is the code you could imagine your neural network executing." And then I'm telling some simple story about the world where I'm like, "Oh, actually you live in a world which is governed by the following laws of physics, and the following actors or whatever." And in that world, this program is actually pretty good. And then I'm like, "Here is some assumption about how SGD works that's consistent with everything we know right now." Very often, we think SGD could find any program that's the simplest program that achieves a given loss, or something.

**Paul Christiano:**
So the story has the sketch of some code, and often that code will have some question marks and like looks like you could fill those in to make the story work. Some description of the environment, some description of facts about gradient descent. And then we're bouncing back and forth between that, and working on the algorithm. Working on the algorithm, I guess, is more like... at the end of the day, most of the algorithms take the form of: "Here's an objective. Try minimizing this with gradient descent." So basically the algorithm is, here's an objective. And then you look at your story and you're like, "Okay, on this story, is it plausible that minimizing this objective leads to this thing?" Or often part of the algorithm is "And here's the good thing we hope that you would learn instead of that bad thing."

**Paul Christiano:**
In your original story you have your AI that loops over actions until it finds one that it predicts leads to smiling human faces on camera. And that's bad because in this world we've created, the easiest way to get smiling human faces on camera involves killing everyone and putting smiles in front of the camera. And then we're like, "Well, what we want to happen instead is like this other algorithm I mentioned where, it outputs everything it knows about the world. And we hope that includes the fact that the humans are dead." So then a proposal will involve some way of operationalizing what that means, like what it means for it to output what it knows about the world for this particular bad algorithm that's doing a simulation or whatever, that we imagined. And then what objective you would optimize with gradient descent that would give you this good program that you wanted, instead of the bad one you didn't want.

### Disagreements and uncertainties <a name="disagreements-uncertainties"></a>

**Daniel Filan:**
The next question I'd like to ask is, what do you see as the most important big picture disagreements you have with people who already believe that advanced AI technology might pose some kind of existential risk, and we should really worry about that and try to work to prevent that?

**Paul Christiano:**
Broadly, I think there are two categories of disagreements, or I'm flanked on two different sides. One is by the more Machine Intelligence Research Institute crowd, which has a very pessimistic view about the feasibility of alignment and what it's going to take to build AI systems that aren't trying to kill you. And then on the other hand, by researchers who tend to be at ML labs, who tend to be more in the camp of like, it would be really surprising if AI trained with this technique actually was trying to kill you. And there's nuances to both of those disagreements.

**Paul Christiano:**
Maybe you could split the second one into one category that's more like, actually this problem isn't that hard, and we need to be good at the basics in order to survive. Like the gravest risk is that we mess up the basics. And a second camp being like, actually we have no idea what's going to be hard about this problem. And what it's mostly about is getting set up to collect really good data as soon as possible, so that we can adapt to what's actually happening.

**Paul Christiano:**
It's also worth saying that it's unclear often which of these are empirical disagreements versus methodological differences, where I have my thing I'm doing, and I think that there's room for lots of people doing different things. So there are some empirical disagreements, but not all the differences in what we do are explained by those differences, versus some of them being like, Paul is a theorist, who's going to do some theory, and he's going to have some methodology such that he works on theory. I am excited about theory, but it's not always the case that when I'm doing something theoretical it's because I think the theoretical thing is dominant.

**Paul Christiano:**
And going in those disagreements with the MIRI folk, that's maybe more weeds-y. It doesn't have a super short description. We can return to it in a bit if we want. On the people who are on the more optimistic side: I think for people who think existing techniques are more likely to be okay, I think the most common disagreement is about how crazy the tasks our AIs will be doing are, or how alien will the reasoning of AI systems be. People who are more optimistic tend to be like, "AI systems will be operating at high speed and doing things that are maybe hard for humans or a little bit beyond the range of human abilities, but broadly, humans will be able to understand the consequences of the actions they propose fairly well." They'll be able to fairly safely look at an action, and be like, can we run this action? They'll be able to mostly leverage those AI systems effectively, even if the AI systems are just trying to do things that look good to humans.

**Paul Christiano:**
So often it's a disagreement about, I'm imagining AI systems that reason in super alien ways, and someone else is like, probably it will mostly be thinking through consequences, or thinking in ways that are legible to humans. And thinking fast in ways that are legible to humans gets you a lot of stuff. I am very long on the thinking fast in ways legible to humans is very powerful. I definitely believe that a lot more than most people, but I do think I often, especially because now I'm working on the more theoretical end, I'm often thinking about all the cases where that doesn't work, and some people are more optimistic that the cases where that works are enough, which is either an empirical claim about how AI will be, or sometimes a social claim about how important it is to be competitive.

**Paul Christiano:**
I really want to be able to build aligned AI systems that are economically competitive with unaligned AI, and I'm really scared of a world where there's a significant tension there. Whereas other people are more like, "It's okay. It's okay if aligned AI systems are a little bit slower or a little bit dumber, people are not going to want to destroy the world, and so they'll be willing to hold off a little bit on deploying some of these things."

**Paul Christiano:**
And then on the empirical side, people who think that theoretical work is less valuable, and we should be mostly focused on the empirics or just doing other stuff. I would guess one common disagreement is just that I'm reasonably optimistic about being able to find something compelling on paper. So I think this methodology I described of "Try and find an algorithm for which it's hard to tell a story about how your AI ends up killing everyone", I actually expect that methodology to terminate with being like, "Yep, here's an algorithm. It looks pretty good to us. We can't tell a story about how it's uncompetitive or lethal." Whereas I think other people are like, "That is extremely unlikely to be where that goes. That's just going to be years of you going around in circles until eventually you give up." That's actually a common disagreement on both sides. That's probably also the core disagreement with MIRI folks, in some sense.

**Daniel Filan:**
Yeah. So you said it was perhaps hard to concisely summarize your differences between the sort of group of people centered, perhaps, at the Machine Intelligence Research Institute (or MIRI for short). Could you try?

**Paul Christiano:**
So definitely the upshot is, I am optimistic about being able to find an algorithm which can align deep learning, like, a system which is closely analogous to and competitive with standard deep learning. Whereas they are very pessimistic about the prospects for aligning anything that looks like contemporary deep learning. That's the upshot. So they're more in the mindset of like, let's find any task we can do with anything kind of like deep learning, and then be willing to take great pains and huge expense to do just that one task, and then hopefully find a way to make the world okay after that, or maybe later build systems that are very unlike modern deep learning. Whereas I'm pretty optimistic - where "pretty optimistic" means I think there's a 50-50 chance or something - that we could have a nice algorithm that actually lets you basically do something like deep learning without it killing everyone.

**Paul Christiano:**
That's the upshot. And then the reason for, I think those are pretty weedsy, I guess intuitively is something like: if you view the central objective as about decoupling and trying to learn what your unaligned agent would have known, I think that there are a bunch of possible reasons that that decoupling could be really hard. Fundamentally, the cognitive abilities and the intentions could come as a package. This is also really core in MIRI's disagreement with more conventional ML researchers, who are like, why would you build an agent? Why not just build a thing that helps you understand the world?

**Paul Christiano:**
I think on the MIRI view, there's likely to be this really deep coupling between those things. I'm mostly working on other ways that decoupling can be hard, besides this kind of core one MIRI has in mind. I think MIRI is really into the idea that there's some kind of core of being a fast, smart agent in the world. And that that core is really tied up with what you're using it for. It's not coherent to really talk about being smart without developing that intelligence in the service of a goal, or to talk about like factoring out the thing which you use.

**Paul Christiano:**
There's some complicated philosophical beliefs about the nature of intelligence, which I think especially Eliezer is fairly confident in. He thinks it's mostly pretty settled. So I'd say that's probably the core disagreement. I think there's a secondary disagreement about how realistic it is to implement complex projects. I think their take is, suppose Paul comes up with a good algorithm. Even in that long shot, there's no way that's going to get implemented, rather than just something easier that destroys the world. Projects fail the first time, and this is a case where we have to get things right the first time - well, that's a point of contention - such that you're not going to have much of a chance. That's the secondary disagreement.

**Daniel Filan:**
And sort of related to that, I'm wondering, what do you think your most important uncertainties are? Uncertainties such that if you resolved them, that would in a big way change what you were motivated to do, in order to reduce existential risk from AI.

**Paul Christiano:**
Yeah. So maybe top four. One would be, is there some nice algorithm on paper that definitely doesn't result in your AI killing you, and is definitely competitive? Or is this a kind of thing where like that's a pipe dream and you just need to have an algorithm that works in the real world? Yeah. That would have an obvious impact on what I'm doing. I am reasonably optimistic about learning a lot about that over the coming years. I've been thinking recently that maybe by the end of 2022, if this isn't going anywhere, I'll pretty much know and can wind down the theory stuff, and hopefully significantly before then we'll have big wins that make me feel more optimistic. So that's one uncertainty. Just like, is this thing I'm doing going to work?

**Paul Christiano:**
A second big uncertainty is, is it the case that existing best practices in alignment would suffice to align powerful AI systems, or would buy us enough time for AI to take over the alignment problem from us? Like, I think eventually the AI will be doing alignment rather than us, and it's just a question of how late in the game does that happen and how far existing alignment techniques carry us. I think it's fairly plausible that existing best practices, if implemented well by a sufficiently competent team that cared enough about alignment, would be sufficient to get a good outcome. And I think in that case, it becomes much more likely that instead of working on algorithms, I should be working on actually bringing practice up to the limits of what is known. Maybe I'll just do three, not four.

**Paul Christiano:**
And then three, maybe this is a little bit more silly, but I feel legitimate moral uncertainty over what kinds of AI... maybe the broader thing is just how important is alignment relative to other risks? I think one big consideration for the value of alignment is just, [how good is it if the AI systems take over the world from the humans](https://ai-alignment.com/sympathizing-with-ai-e11a4bf5ef6e)? Where my default inclination is, that doesn't sound that good. But it sounds a lot better than nothing in expectation, like a barren universe. It would matter a lot. If you convinced me that number was higher, at some point I would start working on other risks associated with the transition to AI. That seems like the least likely of these uncertainties to actually get resolved.

**Paul Christiano:**
I find it kind of unlikely I'm going to move that much from where I am now, which is like... maybe it's half as good for AIs to take over the world from humans, than for humans to choose what happens in space. And that's close enough to zero that I definitely want to work on alignment, and also close enough to one that I also definitely don't want to go extinct.

**Daniel Filan:**
So my penultimate question is, or it might be antepenultimate depending on your answer, is, is there anything that I have not yet asked, but you think that I should have?

**Paul Christiano:**
It seems possible that I should have, as I've gone, been plugging all kinds of alignment research that's happening at all sorts of great organizations around the world. I haven't really done any of that. I'm really bad at that though. So I'm just going to forget someone and then feel tremendous guilt in my heart.

### Some favorite organizations <a name="some-favorite-orgs"></a>

**Daniel Filan:**
Yeah. How about in order to keep this short and to limit your guilt, what are the top five people or organizations that you'd like to plug?

**Paul Christiano:**
Oh man, that's just going to increase my guilt. Because now I have to choose five.

**Daniel Filan:**
Perhaps name five. Any five!

**Paul Christiano:**
Any five. I think there's a lot of ML labs that are doing good work, ML labs who view their goal as getting to powerful transformative AI systems, or doing work on alignment. So that's like [DeepMind](https://deepmind.com/), [OpenAI](https://deepmind.com/), [Anthropic](https://www.anthropic.com/). I think all of them are gradually converging to this gradual crystallization in what we all want to do. That's one. Maybe I'll do three things. Second can be academics. There's a bunch of people. I'm friends with [Jacob Steinhardt](https://jsteinhardt.stat.berkeley.edu/) at Berkeley. His students are working on robustness issues with an eye towards long term risks. A ton of researchers at [your research organization](https://humancompatible.ai/), which I guess you've probably talked about on other episodes.

**Daniel Filan:**
I talked to some of them. I don't think we've talked about it as a whole. Yeah. It's the Center for Human-Compatible AI. If people are interested, they can go to [humancompatible.ai](https://humancompatible.ai/) to see a list of people associated with us. And then you can, for each person, I guess you can look at all the work they did. We might have a newsletter or something [as far as I can tell, we do not]. I did not prepare for this.

**Paul Christiano:**
Sorry for putting you on the spot with pitching. No, I think I'm not going to do justice to the academics. There's a bunch of academics, often just like random individuals here and there with groups doing a lot of interesting work. And then there's kind of the weird effective altruist nonprofits, and conventional AI alignment crowd nonprofits. Probably the most salient to me there are [Redwood Research](https://www.redwoodresearch.org/). It's very salient to me right now because I've been talking with them a bunch over the last few weeks.

**Daniel Filan:**
What are they?

**Paul Christiano:**
They're working on robustness, broadly. So this adversarial training stuff. How do you make your models definitely not do bad stuff on any input? [Ought](https://ought.org/), which is a nonprofit that has been working on like, how do you actually turn large language models into tools that are useful for humans, and [the Machine Intelligence Research Institute](https://intelligence.org/), which is the most paranoid of all organizations about AI alignment - their core value added probably. There's a lot of people doing a lot of good work. I didn't plug them at all throughout the podcast, but I love them anyway.

### Following Paul's work <a name="following-pauls-work"></a>

**Daniel Filan:**
All right. So speaking of plugging things, if people listen to this podcast and they're now interested in following you and your work, what should they do?

**Paul Christiano:**
I write blog posts sometimes at [ai-alignment.com](https://ai-alignment.com/). I sometimes publish to [the alignment forum](https://www.alignmentforum.org/). And depending on how much you read, it may be your best bet to wait until spectacular, exciting results emerge, which will probably appear one of those places, and also in print. But we've been pretty quiet over the last six months, definitely. I expect to be pretty quiet for a while, and then to have a big write up of what we're basically doing and what our plan is sometime. I guess I don't know when this podcast is appearing, but sometime in early 2022 or something like that.

**Daniel Filan:**
I also don't know when it's appearing. We did date ourselves to [infrastructure week](https://politicaldictionary.com/words/infrastructure-week/), one of the highly specific times. Okay. Well, thanks for being on the show.

**Paul Christiano:**
Thanks for having me.

**Daniel Filan:**
This episode is edited by Finan Adamson, and Justis Mills helped with transcription. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
