---
layout: post
title: "13 - First Principles of AGI Safety with Richard Ngo"
date: 2022-03-30 22:15 -0700
categories: episode
---

[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/OTlmYzM1ZjEtMDFkMi00ZTExLWExYjEtNTYwOTg2ZWNhOWNi)

How should we think about artificial general intelligence (AGI), and the risks it might pose? What constraints exist on technical solutions to the problem of aligning superhuman AI systems with human intentions? In this episode, I talk to Richard Ngo about his report analyzing AGI safety from first principles, and recent conversations he had with Eliezer Yudkowsky about the difficulty of AI alignment.

Topics we discuss:
- [The nature of intelligence and AGI](#agi-intelligence-nature)
  - [The nature of intelligence](#nature-of-intelligence)
  - [AGI: what and how](#agi-what-how)
  - [Single vs collective AI minds](#single-collective-ai-minds)
- [AGI in practice](#agi-in-practice)
  - [Impact](#agi-impact)
  - [Timing](#agi-timing)
  - [Creation](#agi-creation)
  - [Risks and benefits](#agi-risks-benefits)
- [Making AGI safe](#making-agi-safe)
  - [Robustness of the agency abstraction](#agency-abstraction-robustness)
  - [Pivotal acts](#pivotal-acts)
- [AGI safety concepts](#agi-safety-concepts)
  - [Alignment](#ai-alignment)
  - [Transparency](#transparency)
  - [Cooperation](#cooperation)
- [Optima and selection pressures](#optima-selection-pressures)
- [The AI alignment research community](#ai-alignment-research-community)
  - [Updates from Yudkowsky conversation](#yudkonversation-updates)
  - [Corrections to the community](#community-corrections)
  - [Why others don't join](#why-others-dont-join)
- [Richard Ngo as a researcher](#ngo-as-researcher)
- [The world approaching AGI](#world-approaching-agi)
- [Following Richard's work](#following-richards-work)

**Daniel Filan:**
Hello, everybody. Today, I'll be speaking with Richard Ngo. Richard is a researcher at OpenAI, where he works on AI governance and forecasting. He also was a research engineer at DeepMind, and designed the course ["AGI Safety Fundamentals"](https://www.eacambridge.org/agi-safety-fundamentals). We'll be discussing his report, [AGI Safety from First Principles](https://www.alignmentforum.org/s/mzgtmmTKKn5MuCzFJ), as well as his [debate with Eliezer Yudkowsky](https://www.alignmentforum.org/s/n945eovrA3oDueqtq) about the difficulty of AI alignment. For links to what we're discussing, you can check the description of this episode, and you can read the transcripts at [axrp.net](https://axrp.net/). Well, Richard, welcome to the show.

**Richard Ngo:**
Thanks so much for having me.

## The nature of intelligence and AGI <a name="agi-intelligence-nature"></a>

**Daniel Filan:**
You wrote this [report](https://www.alignmentforum.org/s/mzgtmmTKKn5MuCzFJ), AGI Safety from First Principles. What is AGI? What do you mean by that term?

**Richard Ngo:**
Fundamentally, it's AI that can perform as well as humans, or comparably to humans on a wide range of tasks, because it's got this thing called general intelligence that humans also have. And I think this is not a very precise definition. I think we just don't understand what general intelligence really is, like how to specifically pin down this concept, but it does seem like there's something special about humans that allows us to do all the things we do. And that's the thing that we expect AGI to have.

### The nature of intelligence <a name="nature-of-intelligence"></a>

**Daniel Filan:**
And so I shouldn't be imagining that there's some really technical definition of general or intelligence that you're using here?

**Richard Ngo:**
No, I think it's much more like a pointer to a, let's say, pre-paradigmatic concept. We kind of know the shape of it, but we don't understand in mechanistic detail how general intelligence works. It's got these components like: of course, you need memory, of course you need some kind of planning, but exactly how to characterize the ways in which combinations of these count as general intelligence is something we don't have yet.

**Daniel Filan:**
So when you're talking about AGI, often there is this idea of AGI that's smarter than people, which implies that intelligence is some kind of scalar concept where you can have more or less of it, and it's really significant if you have more or less of it. Do you think that's basically a correct way to think about intelligence, or should we just not go with that?

**Richard Ngo:**
Yeah, I think that seems like a good first approximation. There's this great parody paper, I think it's called [On the Impossibility of Supersized Machines](https://arxiv.org/abs/1703.10987), which says size is not a clear concept. There are different dimensions of size, like height and weight and width, and so on. So, there's no single criterion of when you count something as bigger than something else. But nevertheless, it does make sense to talk about machines that are bigger than humans. And so in roughly the same way, I think it makes sense to talk about machines that are more intelligent than humans. I'd characterize it as something which draws from a long intellectual history of realizing that humans were like a little bit less special than we used to think, that we're not in the center of the universe, we're not that different from other animals, and now that we aren't at the pinnacle of intelligence.

**Daniel Filan:**
Okay, why should we think of intelligence as this kind of scalar thing?

**Richard Ngo:**
One way you can do it is, you can hope that we'll come up with a more precise theory of intelligence. I like some metaphors from the past. So when we think about the concept of energy, or when we think about the concept of information or even the concept of computing, these were all things that were very important abstractions for making sense of the past. Like you can think about the industrial revolution as this massive increase in our ability to harness energy. Even though, at the time the industrial revolution started, we didn't really have a precise concept of energy. So, one hope is to say, "Well, look, we're going to get a more precise concept as time goes by, just as we did for energy and computation and information. And whatever this precise concept is going to be, we'll end up thinking that you could have more or less of it."

**Richard Ngo:**
Probably the more robust intuition here is just like, it sure seems that if you make things faster, if you make brains bigger, they just get better at doing a bunch of stuff. And it seems like that broad direction is the dimension that I want to talk about.

**Daniel Filan:**
Okay, so that's intelligence. I guess, we're also talking about general intelligence, which I think you contrast with narrow intelligence. Where, if I recall correctly, a narrow intelligence is something that can do one specific task, but a general intelligence is something that can do a wide variety of tasks. Is that approximately right?

**Richard Ngo:**
Yeah, that seems right. I think there are a couple of different angles on general intelligence. So, one angle that is often taken is just thinking about being able to deal with a wide range of environments. And that's an easy thing to focus on, because you can very clearly imagine having a wide range of different tasks. Another take on generality you might have is that generality requires the ability to be very efficient in learning. So being able to pick up a new task with only a couple of different demonstrations. And another one you might have is that generality requires the ability to act coherently over long periods of time. So you can carry out tasks on timeframes not just of hours, but on days, or weeks, or months, or so on.

**Richard Ngo:**
I think these all kind of tie together, because fundamentally, when you're doing something in a narrow domain, you can memorize a bunch of heuristics. Or you can encode all the information within the system that you're using, like the weights of a neural network. Whereas, when you've got either long time horizons, or a wide range of environments, or not very much data, what that means is you need something which isn't just memorized, which is in some sense able to get more out of less.

### AGI: what and how <a name="agi-what-how"></a>

**Daniel Filan:**
Okay, cool. And I guess we're going to take it as given for this discussion that we should expect to get AGI at some point. For people who are wondering, who are perhaps doubtful about that, can you give a sense of why you think that might be possible?

**Richard Ngo:**
Yeah. So I think there are a couple of core intuitions which seem valuable. So the first one is that there are a lot of disanalogies between neural networks and human brains, but there are also a lot of analogies. And so to me it seems overconfident to think that scaling up neural networks - with presumably a bunch of algorithmic improvements, and architectural improvements, and so on - but once you're using as much compute, either as an individual human brain, or maybe as all human brains combined, or maybe as the entire history of humanity, which are all milestones that are feasible over a period of decades, at that point it seems like you really need some specific reason to think that neural networks are going to be much worse, or much less efficient than human brains in order to not place a significant credence on those systems having a capability of generality in the same way as humans do.

**Richard Ngo:**
Another intuition here is that a lot of people think about machine learning as a series of paradigm shifts. So we started off with symbolic AI, and then we moved into something more like statistical style machine learning, and now we're in the era of deep learning. I think this is a little misleading. The way I prefer to think of it is that neural networks were just plugging along, doing their thing the whole time, like the perceptron and the first models of neurons were around before people think of the field of AI as having officially started. And so if you just look at this trend of neural networks getting more and more powerful as you throw more and more compute at them, and as you make relatively few big algorithmic changes, then it starts to seem more plausible that actually you don't need a big paradigm shift in order to reach AGI. You can just keep scaling things up because that actually has worked for the last 70 years or so.

**Daniel Filan:**
Okay. So it seems like your case is really based on this idea that we're going to have modern machine learning or deep learning, and we're going to scale it up. We're going to scale it up in some ways, maybe we're going to make discrete changes in algorithms, and architectures, and other ways. In this process of scaling, what similarities do you see as going to be retained from now until AGI? And what important differences do you think there are going to be?

**Richard Ngo:**
Yeah. So I think the core similarity: Neural networks seem like they're here to stay. Reinforcement learning seems like it's here to stay. Self-supervised learning seems pretty fundamental as well. So these all feel very solid. Then I think the fundamental question is something like, how much structure do you need to build into your algorithms versus how much structure can you meta-learn, or learn via architectural search, or things like that. And that feels like a very open question to me. It seems like in some cases we have these very useful algorithms, like [Monte Carlo tree search](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search) [MCTS]. And if you can build that into your system, then why wouldn't you do that? And it seems really hard to learn something like MCTS.

**Daniel Filan:**
And MCTS is just sort of randomly looking at, okay, if I take various sequences of actions, how good is that likely to be? Maybe I randomly pick actions, or pick actions that I think are more likely to be optimal, or that kind of thing.

**Richard Ngo:**
Right, exactly. But you've got these different impulses. On one hand, we've got a few algorithms, or a few ways of designing networks, that seem very clever, and maybe we can come up with more clever things. On the other hand, the idea of [The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) from [Rich Sutton](https://en.wikipedia.org/wiki/Richard_S._Sutton), which is that most clever innovations don't really last. And yeah, so I feel very uncertain about how much algorithmic progress you need in order to continue scaling up AI.

**Daniel Filan:**
Okay. So a question I have along this front: current AI systems at least seem like a lot of what they're doing is recognizing patterns in a sort of shallow way. And maybe when you have a neural network and you want to get it into a new domain, you need to expose it to the new patterns until it can pick them up. And maybe it draws something on what the old patterns were like. But it seems, at least to a lot of people, to be very pattern-based. And some people have this intuition that something more like reasoning is going to be needed, where within the neural network it's understanding its environment and doing things like MCTS play-outs inside its head. And that once you have this kind of thing, you won't need to be exposed to that many patterns until you understand the nature of what's going on.

**Daniel Filan:**
So I'm wondering to what degree does this difference between pattern recognition versus real reasoning play in your thinking? And do you think that real reasoning is necessary and/or do you think that we'll get it in things roughly like current neural networks?

**Richard Ngo:**
I think that, yeah, what you call real reasoning is definitely necessary for AGI. Having said that, I think pattern recognition is maybe more fundamental than a lot of people give it credit for where, when humans are thinking about very high level concepts, we do just use a bunch of pattern recognition. Like when you think about, I guess, great scientists thinking in terms of metaphors or intuitions from a range of different fields, Einstein imagining thought experiments and so on, mathematicians visualizing numbers in terms of objects and the interactions between those objects, these all feel like types of pattern recognition. I'm gesturing towards the idea of this book by [Lakoff](https://en.wikipedia.org/wiki/George_Lakoff) called [Metaphors We Live By](https://en.wikipedia.org/wiki/Metaphors_We_Live_By), which is basically arguing that a huge amount of human cognition happens via these types of drawing connections between different domains. Now, of course, you need some type of explicit reasoning on top of that, but it doesn't seem like the type of thing which necessarily requires new architectures, or us to build it in explicitly.

**Richard Ngo:**
There's another book which is great on this topic, called [The Enigma of Reason](https://www.hup.harvard.edu/catalog.php?isbn=9780674237827), which kind of fits these ideas together: explicit reasoning and pattern matching. It basically argues that explicit reasoning is just a certain type of pattern matching. It's the ability to match patterns that have the right type of justification. I'm not doing it credit here, so you should check that out if you're interested. But basically, I think that these two things are pretty closely related, and there's not some fundamental difference.

### Single vs collective AI minds <a name="single-collective-ai-minds"></a>

**Daniel Filan:**
Okay. So moving on a bit, in the report, you talk about different ways AGIs could be. One of them is, you have this single neural network maybe that's an AGI, which I think a lot of people are more familiar with. And then you also talk about collective AGIs. Maybe it's an AGI civilization where the civilization is intelligent, or something like that. Could you describe those, and tell us how important are those to think about, if people don't normally think about them?

**Richard Ngo:**
I guess the core intuition behind thinking about a single AGI is that our training procedures work really well when you train end-to-end. You take a system, you give it a feedback signal, and you just adjust the whole system in response to that feedback signal. And what you get out of it is basically one system where all the parts have been shaped to work together by gradient descent. So that's a reason for thinking that, in terms of efficiency, having one system is going to be much more efficient than having many different systems trying to work together.

**Richard Ngo:**
I think the core intuition for thinking about a collection of AGIs is that it's very cheap to copy a model after you've learned it. And so if you're trying to think about the effects that building a single AGI will have, it seems like the obvious next step is to say, "Well, you're going to end up with a bunch of copies of that, because it's very cheap to make them. And then now you're going to have to reason about the interactions between all of those different systems." I think that it probably doesn't make a huge difference in terms of thinking about the alignment problem, because I don't think you're going to get that much safety from having a large collection of AGIs compared to just having one AGI. It feels like if the first one is aligned, then the collection of many copies of it is going to be as well. And if it's not, then they won't. But it does seem pretty relevant to thinking about the dynamics of how AGI deployment might go in the world.

**Daniel Filan:**
Okay, cool. And I guess one question I have related to this, that sort of points to this idea that instead of thinking just about each AGI, or each neural network or whatever, being intelligent, we should also think of the intelligence of the whole group. And so I have this colleague, [Dylan Hadfield-Menell](https://engineering.mit.edu/faculty/dylan-hadfield-menell/), who's very interested in this idea that when we're thinking of training AIs to do things that people want, we should think about having a human AI system be rational in the sense of conserving resources: that whole system should achieve the human goal. So I'm wondering like what thoughts do you have about the locus of intelligence: should we think of that as being single AI systems, multitudes of AI systems, or some combination of humans and AIs?

**Richard Ngo:**
I think that the best outcome is to have the locus of intelligence effectively being some combination of humans and AIs. To me that feels like more of a governance decision rather than an alignment decision, in the sense that I expect that increasingly as we build the AGIs, they're going to take on more and more of the cognitive load of whatever task we want them to do. And so if there's some point we were going to stop and say like, "No, we're going to try and prevent the AGIs from assuming too much control" then, I think, fundamentally, that's got to be some sort of regulatory or governmental choice. I don't think it's the type of thing that you'd be able to build in on a technical level, is my guess.

**Richard Ngo:**
And then when it comes to many AGIs, I think people do underrate the importance of, in particular, cultural evolution when it comes to humans. Where that's one way in which the locus of intelligence is not in a single person, it's kind of spread around many people. Yeah, so that's some reason to suggest that the emergent dynamics of having many AGIs will play an important role. But I think most of the problem comes in talking about a single system. And like, if you can have guarantees about what a single system will do, then you can work from there to talk about the multi-agent system. Basically, I think the claim is that having multi-agent system makes things harder, but some people have argued that it kind of makes things easier, that it means we don't need to worry about the alignment problem for a single agent case. And I think that's incorrect. I think, start off by solving it for the single agent case, without relying on any messy multi-agent dynamics to make things go well.

## AGI in practice <a name="agi-in-practice"></a>

### Impact <a name="agi-impact"></a>

**Daniel Filan:**
So we sort of hinted at this idea of an alignment problem, or various risks. Some people seem to think that if we had AGI, this would be a really big deal. Do you think that's right? And if so, why?

**Richard Ngo:**
Yeah, I think that's right. I think there's the straightforward argument which is, humans can do incredible things because of our intelligence. If you have AGIs, then they'll be able to do incredible things either much faster, or in a way that builds on themselves, and this will lead to types of scientific and technological advancement that we can't really even imagine right now. Maybe, as one thought experiment I particularly like, imagine taking 10,000 scientists, like 10,000 of the best scientists and technologists, and just accelerating them by a factor of 1,000, just speeding up the way they can think, speeding up the types of progress that they make, and speeding up, I guess, their computers that they're working on as well. And what could they do after the equivalent of 500 or 1,000 years?

**Richard Ngo:**
Well, they would have some disadvantages, right? They wouldn't be able to engage with the external world nearly as easily. But I think if you just look back 500 years ago, and you think how far have we come in 500 years through this process of scientific and technological advancement, it feels like it's kind of wild to think about anywhere near the same amount of progress happening over a period of years or decades. So, that's one intuition that I find particularly compelling.

### Timing <a name="agi-timing"></a>

**Daniel Filan:**
Okay. Seems like a pretty good deal. When do you think we will get AGI, and why do you think it'll take that long?

**Richard Ngo:**
Yeah, so I don't have particularly strong views on this. So to a significant extent, I defer to the [report](https://www.alignmentforum.org/posts/KrJfoZzpSDpnrv9va/draft-report-on-ai-timelines) put out by Ajeya Cotra at Open Philanthropy which, if I recall correctly, has a median time of around 2045 or 2050. I think that report is a pretty solid baseline. I don't think we should have particularly narrow credences around that point in time. I guess, it feels like there are some high level arguments that sway me, but they ultimately kind of cancel out. So one high level argument is just, look, there are a lot of cases in the past where people were wildly overconfident about the technology needing to take a long time. So, the classic example being the predictions by the Wright brothers about how long they'd take to build an airplane, where they thought it was 50 years away, I think two years before they built it. People thought it would take way longer than it did to land people on the moon. I think Lord Rutherford thought that the nuclear reaction was kind of moonshine and nonsense, and that was only a few years before, or maybe even after, they'd made the most critical breakthroughs. So, all of these, they feel like strong intuitions that kind of weigh in the direction of not ruling out significantly earlier timelines. On the other hand, these do feel a little bit cherry-picked and it feels like there are many other examples where people just like see one plausible path to getting to a point and don't see the obstacles that are going to slow them down a lot, because the obstacles are not as salient.

**Richard Ngo:**
So probably people 50 years ago were wildly overconfident about how far rocket technology and nuclear power would get. And these are pretty big deals. It would be very significant if we could do asteroid mining right now, or if we had very cheap nuclear power, but there were a whole bunch of unexpected obstacles even after the initial early breakthroughs. So, I think these situations weigh against each other, and the main effect is just to make me pretty uncertain.

**Daniel Filan:**
All right. Cool. And a related question that people have: some people think that we might need to make preparations before we get AGI. And if AGI will come very suddenly, then we should probably do those preparations now. But if it'll be very gradual and we'll get a lot of warning, maybe we can wait until AGI is going to come soon-ish to prepare to deal with it.

**Daniel Filan:**
So on the question of how sudden AGI might be, do you think that it's likely to happen all at once without much warning, or do you think that there's going to be a very obvious gradual ramp up?

**Richard Ngo:**
I think there's going to be a noticeable and significant ramp up. Whether that ramp up takes more than a couple of years is hard to say. So, I guess the most relevant question here is something like, how fast is the ramp up compared with the ability of, say, the field of machine learning to build consensus and change direction, versus how fast does it compare with the ability of world governments to notice it and make appropriate actions, and things like that? While I'm pretty uncertain about how long the ramp up might take, it feels pretty plausible that it's shorter than the other comparable ramp ups that I just mentioned. So that feels like the decision relevant thing. If the world could pivot in a day or two to really taking a big threat seriously, or really taking technological breakthrough seriously, then that would be a different matter.

**Richard Ngo:**
But as it is, how long does it take for the world to pivot towards taking global warming seriously? A matter of decades. Even coronavirus, it took months or years to really shift people's thinking about that. So, that's kind of the way I think about it. There's no real expectation that people will react in anywhere near a rapid enough way to deal with this, unless we start preparing in advance.

### Creation <a name="agi-creation"></a>

**Daniel Filan:**
Cool. Another question I have just in terms of what it might look like to get AGI. There are two scenarios that people talk about. One scenario, sort of related to the idea of collective AGIs and stuff, there's one strain of thought which says probably what will happen is around the time that we get AGI, a bunch of research groups will figure it out at approximately the same time interval, and we should imagine a world where there are tons of AGI systems. Maybe they're competing with each other, maybe not, but there are a bunch of them and we have to think about a bunch of them. On the other hand, some people think, probably AGI is one of these things where either you have it or you don't to a pretty big extent, and therefore there's going to be a significant period of time where there's just going to be one AGI that's way more relevant than any other intelligent system. Which of these scenarios do you think is more likely?

**Richard Ngo:**
I don't think it's particularly discrete, so I don't think you've either got it or you don't. I think that the scenario that feels most likely to me (although, again, with pretty wide error bars) is that you have the gradual deployment of systems that are increasingly competent on some of the axes that I mentioned before. So, in particular, being able to act competently over longer time horizons, and being able to act competently in new domains with fewer and fewer samples, or less and less data. So, I expect to see systems rolled out that can do a wide range of tasks better than humans over a period of five minutes or a period of an hour, or possibly even over a period of a day, before you have systems that can do the strategically vital tasks that take six months or a year: things like long term research, or starting new companies, or making other large scale strategic decisions.

**Richard Ngo:**
Then I expect there to be a push towards increasingly autonomous, increasingly efficient systems. But then once we've got those systems starting to roll out, I guess I think of it in terms of orders of magnitude. So, the difference between being able to act competently over a day versus being able to act competently over a week versus six months or so, is we should expect that going from a week to a couple of months is not much harder than going from a day to a week. Each time you jump up in order of magnitude, it feels like roughly speaking, we should expect it to be a similar level of difficulty.

**Daniel Filan:**
Okay. So, does that look like a world where this whole field, one person out of the field managed to figure out how to get their AI to plan on the scale of a year, and the rest of them figured out how to have their AIs plan on the scale of a month or two? And maybe the one that can plan on the scale of a year is just way more relevant to the world than the rest of them?

**Richard Ngo:**
Yeah, that seems right.

### Risks and benefits <a name="agi-risks-benefits"></a>

**Daniel Filan:**
Okay, cool. We've mentioned this idea that maybe... well, your report is titled "AGI Safety from First Principles", and we've talked a bit about the things like the problem of alignment. How good or bad should we expect AGI to be in terms of its impact on the world and what we care about?

**Richard Ngo:**
It feels like the overall effect is going to be dominated by the possibility of extreme risk scenarios, to me. It feels like it's hard to know what's going on in expectation, because you have these very positive outcomes where we've solved scarcity and understand how to build societies in much better ways than we currently do, and can make a brilliant future for humanity, and then also the ones where we've ended up with catastrophic outcomes because we've built misaligned AGIs. So, yeah, hard to say on balance what it is, but it feels like a point in time where you can have a big influence by trying to swing it in the positive direction.

**Daniel Filan:**
It seems like you think there's a good chance that we're going to build AGI that might do something really bad. Does it kill everyone, does it enslave everyone, does it make everyone's lives 10% worse, or what?

**Richard Ngo:**
Yeah. I think probably the way I would phrase it is that the AGI gains power over everyone, and power in the sense of being able to decide how the world goes. It feels pretty hard for me to say exactly what happens after that. But it feels like by the time we've reached that point, we've screwed up. When I say having power, what I mean is having power in a way that doesn't defer that power back to humans.

**Daniel Filan:**
Okay. How afraid of that should we be? Because I guess there's some world where what that looks like is, maybe I should imagine that we have something like the normal economy, except everyone pays 50% of their income to the AGI overlords, and the AGI gets to colonize a whole bunch of stuff, and it buys a whole bunch of our labor. Should we be super afraid of that?

**Richard Ngo:**
Yeah, I think we should be pretty afraid, in a pretty comparable way to how if you described to gorillas, humans taking over the world, they should be pretty afraid. Now, it's true that there's some chance that the gorillas get a happy outcome, like maybe we're particularly altruistic and we are kind to the gorillas. I don't think there was any strong reason in advance to expect that humans would be kind to gorillas, and, in fact, there have been many cases throughout history of humans driving other species to extinction, just because we had power over them and we could. So, broadly speaking, if there's a set of systems that choose to gain power over humans, even just from that fact, we can probably infer that we should be pretty scared.

**Daniel Filan:**
It seems like AGI is probably going to be a thing that people make, or at least people will make the things that make it. There's some sort of argument which says, look, probably we won't build really terrible AGI systems that take over the world, because if anyone foresees that, they won't do it because they don't want the world to be taken over. I'm wondering what you think of this argument for "things will be fine because we just won't do it".

**Richard Ngo:**
I think in some sense, the core problem is that we don't understand what we're doing when we build these systems to anywhere near the same level as we understand what we're doing when we build an airplane, or when we build a rocket, or something like that. So, yeah, in some sense, if you step out and you're like, "Why is AI different from other technologies?" One answer is just that we don't have a scientific, principled understanding of how to build AI. Whereas for a lot of these other technologies, we do. Now, we could say, "Well, let's experiment until we get that." Then I think the answer to that would just be, if you really expect AGI to be such a powerful technology, then you might not have that long to experiment. Maybe you have months or years, but that's not really very long in the grand scheme of things, when you're trying to figure out how to make a system safe. In particular, it's not long to make a very new, very powerful type of technology safe. If we really knew what we were doing, in terms of us designing the systems, then I'd feel much better. But as it is, it's more like our optimizers or our training algorithms are the ones designing the systems, and we're just kind of setting it up and then letting it run a lot of the time.

**Daniel Filan:**
It seems like in this story, there's still some point where somebody says, "Look, I'm going to set up this system, and there's a really good chance it's going to turn into an AGI, and I don't really understand AGIs, and there are these good arguments that I've heard on this podcast that I listen to for how they're going to be powerful and they're going to be dangerous, but I'm going to press the button anyway." Why do they press the button? Isn't that irrational?

**Richard Ngo:**
One answer is just that they don't really believe the arguments, right? I think it's easy to not believe arguments when the thing you're postulating is this qualitatively different behavior from the other systems that have come before. Jacob Steinhardt has an [excellent series of blog posts](https://bounded-regret.ghost.io/more-is-different-for-ai/) recently, talking about emergent changes in behavior as you scale up AI systems. I think it's kind of like, it does seem a little bit crazy that as you make the systems more and more powerful, you're not really changing the fundamental algorithms or so on. But you do get these fundamental shifts in what they're able to do. So, not just performing well in a narrow domain, but performing well in a wide range of domains. Maybe the best example so far is GPT-3 being able to do few shot learning just by being given prompts.

**Richard Ngo:**
And that's much smaller than the types of change you expect as you scale systems up from current AI to AGIs. So, yeah, plausibly people just don't intuitively believe the scale of the change that they ought to expect to see. Then the second argument is maybe they do, but there are a bunch of competitive motivations that they have. Maybe they're worried about economic competition, or maybe they're worried about geopolitical competition. It seems pretty hard to talk about these very large scale things decades in advance, but that's the sort of fundamental shape of an argument that I think is pretty compelling. If people are worried about getting to a powerful technology first, then they're going to cut corners.

## Making AGI safe <a name="making-agi-safe"></a>

### Robustness of the agency abstraction <a name="agency-abstraction-robustness"></a>

**Daniel Filan:**
Now I'd like to talk a bit about the technical conversation about making these AGI systems safe. You recently had this [conversation](https://www.alignmentforum.org/s/n945eovrA3oDueqtq) with [Eliezer Yudkowsky](https://www.yudkowsky.net/), an early proponent of the idea there that there might be existential risk from AGI. One thing that came up is this idea of goal-directed agents, where I think Eliezer had this point of view that was very focused on this idea that we're going to get these AI systems, and they will sort of coherently shape the world in a certain way. He thinks that there's just one natural way to coherently shape the world for simple goals. Where it seems like in your point of view, you think about these abilities of agents, like self-awareness, ability and tendency to plan, picking actions based on their consequences, how sensitive they are to scale, how coherent they are, and how flexible they are. In your view, it seems like you think about agents as sliders on these fronts. Maybe we'll have a low value on one of the sliders. I'm wondering what your thoughts are about the relationship between these ways of thinking, and where you would rely on one versus the other.

**Richard Ngo:**
I think the first thing to flag here is that Eliezer and I agree to a much greater extent than you might expect from just reading the debate that we had. The core idea that he has, that there's something fundamentally dangerous about intelligence, is something that I buy into. I think that the particular things that I want to flag on that front are this idea of instrumental reasoning. Suppose you're an obedient AI, and a human asks you to achieve a goal. You have to reason about what intermediate steps you're going to take, you need to be able to plan out sub goals, and then achieve those sub goals, and then go on and achieve this far goal.

**Richard Ngo:**
It feels like that's very core to the concept of general intelligence. In some sense, you've just got this core ability, and we're like, "But also please don't apply it to humans. Please don't reason about humans instrumentally. Please don't make sub goals that involve persuading me or manipulating me." I think Eliezer is totally right that as you scale this ability up, then it becomes increasingly unnatural to have this sort of exception for humans in the instrumental reasoning that the system is doing. You've got this pressure towards doing instrumental reasoning towards achieving outcomes, and in some sense, what alignment is trying to do is carve out this special zone for human values, which says, "No, no, no, don't do the intelligence thing at us in that way."

**Richard Ngo:**
I'd also flag in another core idea here is the idea of abstraction, the idea that intelligence is very closely linked to being able to zoom out and see a higher level pattern. Again, that's something that when we want to build bounded agents, we want to try and avoid. We want to say, "We've given you a small scale goal," or, "We've trained you to achieve a small scale goal. Please do not abstract from that into wanting to achieve a much larger scale goal." Again, we're trying to carve out an exception, because most of the time we want our agents to do a lot of abstraction. We want them to be thinking in very creative, flexible ways about how the world works. It's just that in the particular case of the goals we give them, we don't want them to generalize in that way.

**Richard Ngo:**
So, that's my summary of some of these ideas that I've gotten from Eliezer, at least, where he has this concept that in the limit, once you push towards the heights of greater general intelligence, it becomes incredibly hard to prevent these capabilities from being directed at us in the ways that we don't want. It feels like maybe the core disagreement I have is something like, how tight is this abstraction? In the sense of, how much can we trust that these things are correlated with each other, not just in the regime of highly superintelligent systems, but also in the regime of systems that are slightly better than humans, or even noticeably significantly better than humans at doing a wide range of tasks, like intellectual research, like jobs in the world, like doing alignment research specifically, things like that.

**Richard Ngo:**
I guess my position is just either I don't understand why he thinks that his claims about the limit of intelligence are also going to continue to apply for the relevant period as we move towards that point, or else maybe he's trusting too much in the abstraction and failing to see ways in which reality might be messier than he thinks is going to be the case.

**Daniel Filan:**
Okay. So, am I right to summarize that as you thinking that, indeed, there's some sort of limit where in order to be generally competent, if you have enough of the things that we call intelligence, then it's very hard for you to not think about humans instrumentally and abstract your goals to larger scales and whatever, but maybe near a human level, we can still do it and it'll be fine?

**Richard Ngo:**
That's right. We might hope that in particular, one argument that feels pretty crucial to me is the idea that our comparative advantage, as humans, is towards a whole bunch of things that are not particularly, let's say, aligned from the perspective of other species. So, we have a strong advantage at doing things like expanding to new areas, like gathering resources, like hunting and fighting and so on. We're not very specialized at things like doing mathematical research, reasoning about alignment, reasoning about economics, for example, in ways that make our societies better. That's just because of the environment in which we evolved.

**Richard Ngo:**
So, it seems very plausible to me that as we train AIs to become increasingly generally intelligent, eventually, they're going to surpass humans at all of these things, but the hope would be that they surpass humans at the types of things that are most useful and least worrying, before they surpass humans in terms of the power seeking behavior that we're most worried about. So, I guess, yeah, a question of differing comparative advantages, even though eventually once they get sufficiently intelligent, they'll outstrip us at all of these things.

### Pivotal acts <a name="pivotal-acts"></a>

**Daniel Filan:**
Yeah. One concept that I think is lying in the background here is this idea of a pivotal act. A listener might listen to that and think, "Well, it sounds like you're saying that for a while, we'll have slightly superintelligent AI, and that'll be fine, but then when we get the really superintelligent AI, that will kill us. Why should I feel comforted by that?"

**Richard Ngo:**
Yeah.

**Daniel Filan:**
So, some people have this idea that, look, when we get really smart AI, step one is to use it to do something that means that we don't have to worry about risk from artificial general intelligence anymore. People tend to describe drastic things. To give listeners a sense, I think in this conversation with Yudkowsky, Yudkowsky gave the example of melting every GPU on earth. I'm wondering, how much do you buy the idea of a pivotal act being necessary, and do you think that being intelligent enough in the way that you can do some kind of pivotal act is compatible with the kinds of intelligence where you're coherent enough to achieve things of value, but you forgot to treat humans as instrumental things to be manipulated in service of your goals?

**Richard Ngo:**
I don't think we have particularly strong candidates right now for ways in which you can use an AGI to prevent scaling up to dangerous regimes. But I think there are plausible things that seem worth exploring, that are maybe a little bit less dramatic sounding than Eliezer's example. So, in the realm of alignment research, you might have a system that can make technical progress on mathematical questions of the sort that are related to AI alignment research. So, one option is automating the theoretical alignment research. Another option which is associated with proposals like amplification, reward modeling, and debate is just to use these systems to automate the empirical, practical side of alignment research by giving better feedback.

**Richard Ngo:**
Then on the governance side, I think just having a bunch of highly capable AIs in the world is going to prompt governments to take risks from AGI a lot more seriously. I don't think that the types of action that would be needed to slow down scaling towards dangerous regimes are actually that discontinuous from the types of things we see in the world today. So, for example, global monitoring of uranium and uranium enrichment to prevent proliferation of nuclear weapons. I think, indeed, there's a lot of cultural pressure against things like building nuclear power, and a wide range of other technological progress that's seen as dangerous, so I feel uncertain about how difficult or extreme governance interventions would need to be in order to actually get the world to think, "Hey, let's slow down a bit. Let's be much more careful." But to me, it still feels plausible that pivotal action is a little bit of a misnomer. It's more just like the world handling the problem as the world wakes up to the scale and scope of the problem.

**Daniel Filan:**
So, it seems like in your way of thinking, the thing that stops the slide to incredibly powerful, unfriendly AGI, is we get AGIs to help do our AI safety research for us. Is that about right? As well as a bunch of governance actions?

**Richard Ngo:**
Right. Yeah. That feels like the default proposal that I'm excited about. But in terms of the specifics of the proposals, I can't point to any particular thing and say, "This one is one that I think would work right now."

**Daniel Filan:**
Sure. I guess if I imagine effective safety research, it seems to involve both pretty good means-ends reasoning, like, you have to reason about these systems that we're going to deploy and what they're going to do, and maybe they're going to get roped into helping with the safety research. We'll have to think about how they'll research or have some invariants about that are going to be maintained or something. So you have to have pretty good means-ends reasoning. And you also have to be thinking about humanity, in order to know what counts as safety research, versus research to ensure that the AGI is blue, or some other random property that nobody cares about. And I think there's some worry that, look, as long as you have that amount of goal orientation in order to get you doing really good research, and that amount of awareness of humans, there's some worry that that combination of attributes is itself enough to think about humans instrumentally in the dangerous way. I'm wondering what you think about that.

**Richard Ngo:**
I agree that's an argument as to why we can't just take a system, say, "go off, please solve the alignment problem" and then have it just come back to us and give us a solution. So I think yeah, in some sense, many of the alignment proposals that are on the table today are ways of trying to mitigate the things you're discussing. It's hard to say like how much of a disagreement there is here, because you know, I do think, you know all of the things you said are just reasons to be worried, but then it feels like...

**Richard Ngo:**
I think this partly ties back to the thing I was saying before about like the core problem being that we just don't understand the way that these systems are developed. So it's less like you have to do a highly specific thing with your system in order to make it go well, and more like you just need to either you need to have a deeper understanding, which is kind of a bit more like intellectual work than going out and doing stuff in the world, or maybe you need to just need to scale up certain kinds of supervision. Which again, I don't currently see the reasons why this is necessarily infeasible. It feels like there's a lot of scope here for promising proposals.

## AGI safety concepts <a name="agi-safety-concepts"></a>

### Alignment <a name="ai-alignment"></a>

**Daniel Filan:**
Let's move on to this thing that we've mentioned a couple of times now called alignment. First of all, what do you mean by "alignment" or, "misaligned AI systems"?

**Richard Ngo:**
To a first approximation, when I say "aligned" I mean "obedient". So, a system that when you give it an instruction, it'll follow the intention of that instruction and then come back and wait for more instructions. That's roughly what I mean. And by "misaligned", roughly what I mean is power seeking. A system that is trying to gain power over the world for itself. Either in order to achieve some other goal that humanity wouldn't want, or just for the sake of it: in the same way that some humans are sort of intrinsically power seeking, you might have an AI that's intrinsically power seeking. Those are the two more specific concepts I usually think about.

**Daniel Filan:**
How important is it for safe AGI to solve the technical problem of making AIs that are aligned? Do you think that that has like a massive role in making our AGI's safe, or a minor role? Do you think it's basically the whole problem, or 10% of the problem, or something?

**Richard Ngo:**
Is the question something like what proportion of the difficulty is alignment versus governance work?

**Daniel Filan:**
I think people, there are some people you think that, oh, you'll need to do alignment, and also some other stuff. I don't know if it's governance. I don't know if it's, governance of thinking about how the AIs are going interact, and maybe some people really want them to coordinate? There might be a variety of things there.

**Richard Ngo:**
Yeah. I guess I feel like it's most of the problem, but nowhere near all of it. If I think about the ways in which I'm concerned about existential risk from advanced AI, probably the split is something like 50% worried about alignment, 25% worried about governance failures related to alignment, and then 25% worried about straightforward misuse, or conflicts sparked by major technological change.

**Daniel Filan:**
So a concern that I think some people have with the idea of AI alignment is this fear that we're going to create this ability for these really powerful people to create AI systems that just do what they want, that are super obedient to them. And I think some people have this idea that, oh, we've just created this effective machine to turn people into tyrants, or totalitarian overlords. How worried are you about this?

**Richard Ngo:**
I think that yeah, it seems pretty worrying that advances in technology lead to dramatic increases in inequality, and a big part of AI governance should be setting up structures and systems to ensure that these systems, if they're aligned, are then used well. I think there's work on this. Cullen O'Keefe, for example, has a [paper on windfall clauses](https://www.fhi.ox.ac.uk/windfallclause/), which talks about the ways in which you might try and redistribute benefits from AGI. More broadly, I think there's a bunch of various unsolved questions to do with not just corporate governance, but also global governance. Ultimately, it's not clear to me what the alternative to addressing these questions is. I think that it would be nice if we could kind of like mitigate all of these problems at once, but it feels like we're just going to have to do the hard work of, setting up as many structures and safeguards as we can.

**Daniel Filan:**
So, I guess some people might think, look, the alternative is try and think hard until you come up with a plan other than "create an AI that's really aligned to an individual", or maybe come up with a technical plan to create an AI that's aligned with humanity, or with objective moral truth, or something.

**Richard Ngo:**
I guess I feel pretty pessimistic about not just having to solve alignment, but also having to solve morality. For example, when I say obedient, it doesn't need to be the case that the system is obedient to any given individual, right? Probably you want to train it so that it's obedient to, ideally speaking, individuals who have the right certifications. Such as being democratically elected, or things like that. And along with that, hopefully it's obedient only within certain bounds of what actions they're meant to take. So like, in an ideal world, if I could have all the alignment desiderata I wanted, I'd set that up in a much better way, but I do think that this core idea of obedience, it feels valuable to me because, yeah, the problem of politics is hard. The problem of ethics is hard. If you can solve the problem of making a system obedient, then we can try and leverage all the existing solutions we have to governance. Things like all the systems and structures that have been built up over time to try and figure out how we're going to deploy advanced technologies. I don't want people to try and have to reason these things through from first principles, while they're trying to build aligned AGIs.

### Transparency <a name="transparency"></a>

**Daniel Filan:**
Another thing that you mentioned in AGI Safety from First Principles is transparency, as this important part of AGI alignment. So could you first say, perhaps briefly, what role you see transparency research as playing?

**Richard Ngo:**
I'll speak differently for the wider field, and then for myself. So I think in the wider field, it's playing a pretty crucial role right now. In terms of transparency is in some sense, one of the core underlying drivers of many proposals for alignment. I'd say the other core driver here is just using more human data, just like trying to get more feedback from humans so that we can use that to nudge systems towards fulfilling human preferences better. So yeah, a lot of research agendas are kind of assuming a certain amount of interpretability or transparency. I'm using those interchangeably. For my own part, I defer to other people to some extent on how much progress we're going to make, because it does seem like there's been pretty impressive progress so far. I feel a little confused about how work on interpretability could possibly scale up as fast as we're scaling up the most sophisticated models.

**Richard Ngo:**
When I think about trying to understand the human brain to a level that's required to figure out if a thought that a human is having is good or bad, that seems very hard. People have been working at it for a very long time. And now of course there are a bunch of big advantages that we have when we do interpretability research on neural networks. Like we have full read/write access to the neurons, but we also have a bunch of disadvantages as well. Like the architecture changes every couple of years. So you have to switch to a new system that might be built quite differently, and you don't have introspective access, or necessarily very much cooperation from the system as you are trying to figure out what's going on inside it.

**Daniel Filan:**
Sure.

**Richard Ngo:**
So, on balance I feel personally kind of pessimistic, but at the same time it seems like something that we should try and make as much progress on as we can. It feels very robustly good to know what's going on within our systems.

**Daniel Filan:**
So, if you're pessimistic, do you think we can do without it, or do you think we'll need it, but it's just very hard and it's unlikely we'll get it?

**Richard Ngo:**
I have a pretty broad range of credence over how hard the alignment problem might be. You know, there's a reasonable range in which interpretability is just necessary or, something equivalently powerful is necessary. There's also ranges in which it isn't. And I think I focus a bit more on the ranges of difficulty in which it's not necessary, just because those feel like the most tractable places to pay attention. So I don't really have a strong estimate of the ratio between those, let's say.

### Cooperation <a name="cooperation"></a>

**Daniel Filan:**
Okay, sure. Another thing to talk about is this idea of AI cooperation. Vincent Conitzer has recently started [a seminar series](https://www.cooperativeai.com/seminars) on this. I believe Andrew Critch wrote a thing basically arguing that even if you solve AI alignment, if you're going to have a whole bunch of AI systems, then solving the problem of making sure that the interactions between them don't generate externalities which are dangerous to humans or something, is even harder than solving the alignment problem [Here's a note from future Daniel: to the best of my knowledge, there's no written piece where Andrew Critch makes this argument. He has said via personal communication that he prefers to not debate which problem is harder, and that he would like people working on both.]. So I'm wondering, what do you think about the worry about making sure that AIs coordinate well?

**Richard Ngo:**
I guess it feels like mainly the type of thing that we can outsource to AIs, once they're sufficiently capable. I don't see a particularly strong reason to think that systems that are comparably powerful as humans, or more powerful than humans, are going to make obvious mistakes in how they coordinate. You have this framing of AI coordination. We could also just say politics, right? Like we think that geopolitics is going to be hard in a world where AIs exist. And when you have that framing, you're like, geopolitics is hard, but we've made a bunch of progress compared with a few hundred years ago where there were many more wars. It feels pretty plausible that a bunch of trends that have led to less conflict are just going to continue. And so I still haven't seen arguments that make me feel like this particular problem is incredibly difficult, as opposed to arguments which I have seen for why the alignment problem is plausibly incredibly difficult.

## Optima and selection pressures <a name="optima-selection-pressures"></a>

**Daniel Filan:**
All right. I'd like to get back to a thing we talked about a bit earlier, which is this question about how much we should think about the optima of various reward functions, or the limit of intelligence or something. I see you as thinking that people focus perhaps too much on that, and that we should really be thinking about selection pressures during training. I'm wondering what mistakes do you think people make when they're thinking in this framework of optima?

**Richard Ngo:**
I think there are kind of two opposing mistakes that different groups of people are making. So Eliezer, and MIRI more generally, it really does feel they're thinking about systems that are so idealized that insights about them aren't very applicable to systems like the full first AGIs we'll build, or ones that are a little bit better than humans at making intellectual progress. There are a bunch of examples of this. I think [AIXI](https://en.wikipedia.org/wiki/AIXI) is kind of dubious. One of the metaphors I use sometimes is that it's like trying to design a train by thinking about what happens when a train approaches the speed of light. You know, it's just not that helpful. So that's one class of mistakes that I'm worried about, related to the idea that you can just extrapolate this idea of intelligence until it's to the infinite limit. Like there is such a thing as perfect rationality, for example, or the limit of approaching perfect rationality makes sense.

**Richard Ngo:**
So that's one mistake I'm worried about. I think on the other hand, it feels like a bunch of more machine learning focused alignment researchers don't take the idea of optimization pressure seriously enough. It feels like there are a few different strands of research that just reroute the optimization pressure, or block it from flowing through the most obvious route, but then just leave other ways for it to cause the same problems. Probably the most obvious example of this to me is the concept of [myopia](https://www.alignmentforum.org/tag/myopia), which especially [Evan Hubinger](https://www.alignmentforum.org/s/n945eovrA3oDueqtq) is pretty keen on, and a few other researchers are as well. And to me, it seems you can't have it both ways. If you've got a system that is highly competent, then there must have been some sort of pressure towards it achieving things on long timeframes.

**Richard Ngo:**
So even if it's like nominally myopic, I haven't seen any particular insight as to how you can have that pressure applying, without making the agent actually care about or pursue goals over long time horizons. And I think the same thing is true with [cooperative inverse reinforcement learning](https://arxiv.org/abs/1606.03137), like where it feels to me like it just kind of reroutes the pressure towards instrumental reasoning in a slightly different way, but it doesn't actually help in preventing that pressure from pushing towards misaligned goals. The key thing that I would like to see from these types of research agendas, and to some extent also stuff from [ARC](https://alignment.org/) like [imitative generalization](https://www.alignmentforum.org/posts/JKj5Krff5oKMb8TjT/imitative-generalisation-aka-learning-the-prior-1) and [eliciting latent knowledge](https://docs.google.com/document/d/1WwsnJQstPq91_Yh-Ch2XRL8H_EpsnjrC1dwZXR37PC8/edit), is the core reason why this isn't just reframing the problem. I want to know which part is actually doing the work of preventing the optimization pressure towards bad outcomes. Because I feel pretty uncertain about that for a lot of existing research agendas.

**Daniel Filan:**
Let's talk about cooperative inverse reinforcement learning. It's a subject I'm relatively more familiar with. So I think the idea, as I understand it, is something like look, we're going to think about human AI interaction like so: the human has a goal, and like the human/AI system somehow has to optimize for that goal. And so the alignment is sort of coming from it being instrumentally valuable for the AI system to figure out what the human goal is, and then to optimize for it. And so you get alignment because the AI is doing what you want in that way. I'm wondering like, where do you think this is missing the idea of optimization pressure?

**Richard Ngo:**
So the way that Stuart Russell often explains his ideas is that the key component is making an AI that's uncertain about human preferences.

**Daniel Filan:**
Yep.

**Richard Ngo:**
But the problem here is that the thing that we really want to do is just point the AI at human preferences at all, make it so that it is in fact optimized in the direction of fulfilling human preferences. Whether or not it actually has uncertainty about those things, about what humans care about, that uncertainty is an intermediate problem. Where the fundamental problem is: what signal are we giving it that points it towards human preferences? Or, what are we changing compared with a sort of straightforward setup where we just give it rewards for doing well and penalize it for doing badly? And one thing you might say is look, the thing we're changing is that we have the model of human rationality.

**Richard Ngo:**
We have some assumptions about how the human is choosing their actions. And that could be the thing that's doing the work, but I haven't seen any model that's actually making enough progress on this, that it's plausible that it's doing the the heavy lifting. Most of the models of human rationality I've seen are very simple ones of noisy rationality or things like that. And so if that model isn't doing the work, then what actually changes in the context of CIRL that points AI towards human preferences any better then a straightforward reward learning setup? That's the thing that I don't think exists.

**Daniel Filan:**
Okay, cool. So I guess on the other side of it, in terms of people who focus on optimality more than you might: in my imagination, there's this reasoning that goes something like, look, when I think about super optimal things that AI systems could do in order to achieve some goal, as long as I can think of some property of an optimal system, it would be really surprising if whatever AI system that we train to do some goal can't think of it, even if it's only as competent as I am myself. And therefore, any thoughts I can generate about properties of optimal AI systems, there's going to be some selection pressure for that. Just because, in order to get something that's more competent than me at achieving goals, it'll have this a similar sort of reasoning ability as me. I'm wondering, do you disagree with that, first of all, and if you do disagree with that, why? And if you don't, maybe can you generate a stronger statement that you do disagree with?

**Richard Ngo:**
So suppose we're thinking about the first system that is better than humans at reasoning about optimality processes, and how they relate to AI alignment. It seems like our key goal should be making sure that system is using its knowledge in ways that are beneficial to us. So maybe telling us about it, or maybe using that knowledge to design a more align successor or something like that. But it seems very unclear the extent to which we can reason about that system itself, via talking about optimality constraints. It feels like my thinking here is about handing over to AI systems that are aligned enough that they can do the heavy lifting in the future. And I don't think it's necessarily a bad strategy to focus on just solving the whole problem in one go. But I do think that going about it via thinking in precise technical terms about the limit of intelligence seems a little bit off.

**Daniel Filan:**
So I guess there's this thought that's like, maybe I can think of some strategy, something the limit of intelligence might take, or some idea that, oh , if you were really smart, you would try to treat humans instrumentally or something. So there's some concern that if I can think of that, then almost by definition something smarter than me can think of it. And so it seems like there's some instinct that says, look, once you have systems that are smarter than me, if these sorts of ideas actually like help that system do whatever it's being selected to do, then it should use those ideas. Do you agree with that argument? Do you think that it doesn't get you the kind of reasoning you regard as worrisome?

**Richard Ngo:**
Well, it seems like the key question is whether those systems will in fact want to achieve goals that we're unhappy about. It feels like the ways in which their motivations are shaped, whether that system will decide to do things that are bad for humans or not is going to be the type of thing that's kind of shaped in a pretty messy way, a by a bunch of gradient descent on a bunch of data points. It's kind of similar to the analogous human case: I am a human, and I'm reasoning about very intelligent future systems and trying to figure out which ones to instantiate, but the way in which I'm choosing which ones to instantiate is very dependent on gut emotional instincts that were honed over long period of evolution, childhood, and so on. Those emotions and instincts are things that feel very hard to reason about in a precise technical manner.

**Daniel Filan:**
Okay. The idea is that sure, an AI system can figure out anything that I can think of. But maybe we have reason to think that we can have some sort of training process that would produce an AI that wanted to do stuff that the thing it's supposed to be aligned to disapproves of. But actually, it's just very unlikely to come up with an AI that has those sorts of desires and therefore, you don't have to worry about this kind of reasoning.

**Richard Ngo:**
That's right.

**Daniel Filan:**
Even though, if it did have those desires, it would do better on the outside objective. We just won't train the overall objective to optimality.

**Richard Ngo:**
Right. That's, in some sense, the reason that we are not worried about dogs wanting to take over the world. Even though, we've done artificial selection on dogs for a while. In theory, at least, the signal that we are sending to them, as we do artificial selection, incentivizes them to take over the world, in some sort of stylized, abstract way. But in practice, that's not the direction that their motivations are being pushed towards. We might hope that the same is true of systems, even if they're a bit more intelligent or significantly more intelligent than humans. Where the extreme optima of whatever reward function we're using is just not that relevant or salient.

## The AI alignment research community <a name="ai-alignment-research-community"></a>

### Updates from Yudkowsky conversation <a name="yudkonversation-updates"></a>

**Daniel Filan:**
You had [this conversation](https://www.alignmentforum.org/s/n945eovrA3oDueqtq) with Eliezer Yudkowsky trying to formalize your disagreements, it seemed to me. I'm wondering if any of that conversation changed your mind and if you can say how it did?

**Richard Ngo:**
I think, yeah. The two biggest things that I took away were number one, as I tried to explain my views about AI governance to Eliezer, I realized that they were missing a whole bunch of detail and nuance. And so any optimism that I have about AI governance needs to be grounded in much more specific details and plans for what might happen and so on. And that's led to a bunch of recent work that I'm doing on formulating a bit more of a governance research agenda, and figuring out what the crucial considerations here are. So that was one thing that changed my mind, but that was a bit more about just like me trying to flesh out my own views.

**Richard Ngo:**
I think in terms of Eliezer's views, I think that I had previously underestimated how much his position relied on a few very deep abstractions that all fitted together. I don't think you can really separate his views on intelligence, his views on consequentialism or agency, his views on recursive self-improvement, and things like that. You can look at different parts of it, but it seems like there's this underlying deep-rooted set of intuitions that he keeps trying to explain in ways that people pick up on the particular thing he's trying to explain, but without having a good handle on the overall set of intuitions. One particularly salient example of this is that previously, he talked a lot about utility functions, and then myself and [Rohin Shah](https://rohinshah.com/) and a bunch of other people tried quite hard to figure out what specific argument he was making around utility functions. And we basically failed, because I don't think that there's a specific, precise, technical argument that you can make with our current understanding of utility theory that, that tells us about AGIs.

**Richard Ngo:**
You don't have a specific argument about utility functions and their relationship to AGIs in a precise, technical way. Instead, it's more like utility functions are like a pointer towards the type of later theory that will give us a much more precise understanding of how to think about intelligence and agency and AGIs pursuing goals and so on. And to Eliezer, it seems like we've got a bunch of different handles on what the shape of this larger scale theory might look like, but he can't really explain it in precise terms. It's maybe in the same way that for any other scientific theory, before you latch onto it, you can only gesture towards a bunch of different intuitions that you have and be like, "Hey guys, there are these links between them that I can't make precise or rigorous or formal at this point."

### Corrections to the community <a name="community-corrections"></a>

**Daniel Filan:**
Sort of related to updates: If there is one belief about existential risk from AI that you could create greater consensus about, among the population of people who are professional AI researchers, who thought carefully about existential risk from AI for more than 10 hours, what belief would it be?

**Richard Ngo:**
Probably the main answer is just the thing I was saying before about how we want to be clear about where the work is being done in a specific alignment proposal. And it seems important to think about having something that doesn't just shuffle the optimization pressure around, but really gives us some deeper reason to think that the problem is being solved. One example is when it comes to Paul Christiano's work on amplification, I think one core insight that's doing a lot of the work is that imitation can be very powerful without being equivalently dangerous. So yeah, this idea that instead of optimizing for a target, you can just optimize to be similar to humans, and that might still get you a very long way. And then another related insight that makes amplification promising is the idea that decomposing tasks can leverage human abilities in a powerful way.

**Richard Ngo:**
Now, I don't think that those are anywhere near complete ways of addressing the problem, but they gesture towards where the work is being done. Whereas for some other proposals, I don't think there's an equivalent story about what's the deeper idea or principle that's allowing the work to be done to solve this difficult problem. Maybe a second thing is that personally, in my more recent work about the alignment problem, I've been moving a little bit away from the term mesa-optimizers, or talking about a clean distinction between the outer alignment problem and the inner alignment problem. I think that it was an incredibly important idea when it first came out because it helped us clarify a bunch of confused intuitions about the relationship between reward functions and goals for intelligent systems, for example. But I think at this point, we might want to be thinking about what's the spectrum between a system that's purely guided by a reward function, versus a policy that has been trained on a reward function but now makes no reference to the reward function at all. I think these are two extremes and in practice, it seems unlikely that we're going to end up at either extreme because reward functions are just very useful. So we should try and be thinking about what's going on in the middle here and shaping our arguments accordingly.

**Richard Ngo:**
Another way of thinking about that is, which is the correct analogy for reinforcement learning agents? Is it the case that gradient descent is like evolution, or is it the case that gradient descent is like learning in the human brain? And the answer is kind of a little bit of both. It's going to play a role that's intermediate between these two things, or has properties of both of these. And we shouldn't treat these as two separate possibilities, but rather as two gestures towards what we should expect the future of AI to look like.

**Daniel Filan:**
I think by now there's some community of people who really think that AGI poses an existential risk, and that it's really dangerous, and they're doing research about it. There's some intellectual edifice there. What do you think the strongest criticisms of that intellectual edifice around the idea of AGI risk are, that deserve more engagement from the community of researchers thinking about it?

**Richard Ngo:**
I think the strongest criticisms used to be insufficient engagement with machine learning. And this has mostly been addressed these days. I think plausibly another criticism is that we, as a movement, probably haven't been as clear as we could be in communicating about risks. So probably this is a slightly boring answer. I think there aren't that many explanations, for example, of the AI alignment problem that are short, accessible, aimed at people who have competence with machine learning, and compelling. When you point people to [Superintelligence](https://en.wikipedia.org/wiki/Superintelligence:_Paths,_Dangers,_Strategies), well, it doesn't engage with machine learning. When you point people to something like [Human Compatible](https://en.wikipedia.org/wiki/Human_Compatible), it actually doesn't spend very much time on the reasons why we expect risks to arise. So I think that there's some type of intellectual work here that just was nobody's job for a while.

**Richard Ngo:**
[AGI Safety from First Principles](https://www.alignmentforum.org/s/mzgtmmTKKn5MuCzFJ) was aimed at filling this gap. And then also more recent work by, for example, [Joe Carlsmith](https://www.josephcarlsmith.com/) from [Open Philanthropy](https://www.openphilanthropy.org/), with a [report](https://www.alignmentforum.org/posts/HduCjmXTBD4xYTegv/draft-report-on-existential-risk-from-power-seeking-ai) called "Is Power Seeking AI an Existential Risk" or something like that. But I still think that there's a bit of a gap there, and I think filling that gap is profitable not just in terms of reaching out to other communities, but also just for having a better understanding of the problem, that will make our own research go better. I think that a lot of disagreements that initially seem like disagreements about which research is promising are actually more like disagreements about what the problem actually is in the first place. That's kind of my standard answer.

### Why others don't join <a name="why-others-dont-join"></a>

**Daniel Filan:**
Yeah. And I guess sort of as a converse of that question, you've worked at a few different AI research organizations with a bunch of people who are working on making AI more capable, and better at reasoning and pattern matching and such, but not particularly working on safety in terms of preventing existential risks. Why don't you think that they have that focus?

**Richard Ngo:**
It varies a bunch by person. I'd say a bunch of people are just less focused on AGI and more focused on pushing the field forward in a straightforward, incremental way. I think there's a type of emotional orientation towards taking potentially very impactful arguments very seriously. And I think that a lot of people are quite cautious about those arguments, and to some extent rightfully so, because it's very easy to be fooled by arguments that are very high level and abstract. So it's almost like there's this habit or there's this predilection towards thinking, "Man, this is really big if true." And then wanting to dig into it. And I think my guess is that probably the main thing blocking people from taking these ideas more seriously is just not getting that instinctive, "Oh, wow, this is kind of crazy" reaction, that makes them want to try very hard to figure it out for themselves. I think in his [Most important Century series of blog posts](https://www.cold-takes.com/most-important-century/), [Holden Karnofsky](https://en.wikipedia.org/wiki/Holden_Karnofsky) does a really good job at addressing this intuition, just being like, "This does sound crazy. And here are a bunch of outside view reasons to think that you should engage with the craziness. Here are a bunch of specific arguments on the object level. And here's an emotional attitude that I take towards the problem." And yeah, that feels like it's pretty directly aimed at the types of things that are determining a lot of machine learning researchers' beliefs.

## Richard Ngo as a researcher <a name="ngo-as-researcher"></a>

**Daniel Filan:**
Moving on, if somebody's listening to this podcast, and they're like, "Richard Ngo seems like a productive researcher", and they want to know what it's like to be you doing research, what would you tell them? What's your production function?

**Richard Ngo:**
Right. I've been thinking a little bit about this lately because we're also hiring for the team I'm leading at OpenAI. And what are the traits that I'm really excited about in a researcher? And I think it's the combination of engaging with high level abstractions in a comfortable way, while also being very cautious about ways that those abstractions might break down or miss the mark. So I guess to me, it feels like a lot of what I'm doing with my research - I used the metaphor of bungee jumping recently - which is going as high as you can in terms of abstraction space, and then jumping down to try and get in touch with the ground again, and then trying to carry that ground level observation as far back up as you can. So that's the sort of feeling that I have when I'm doing a lot of this conceptual research in particular. And then my personal style is to start very top down, and then be very careful to try and fill in enough details that things feel very grounded and credible. Was that roughly the thing you were aiming towards?

## The world approaching AGI <a name="world-approaching-agi"></a>

**Daniel Filan:**
Yeah, that seems roughly like an answer. And I guess the second to last question I'd like to ask: is there anything else that I should have asked you? And if so, what?

**Richard Ngo:**
I feel like there's something important to do with what the world looks like as we approach AGI. It feels like there's a lot of work being done by either the assumption that the world will look roughly the same as it does now, except that these labs will be producing this incredibly powerful technology, or else the assumption that the world will be radically transformed by new applications floating around everywhere. And some of this has already been covered before. Maybe the thing that I'm really interested in right now is, what are the types of warning signs that actually make people sit up and pay attention as opposed to the types of warning signs that people just kind of dismiss with a bit of a shrug?

**Richard Ngo:**
And I think COVID has been a particularly interesting case study, where you can imagine some worlds in which COVID is a very big warning sign that makes everyone pay attention to the risks of engineered pandemics. Or you can imagine a world in which people kind of collectively shrug and are like, okay, that was bad, and then don't really think so much about like what might happen next. And it's not totally clear to me which world we're going to end up in, but it feels like somebody should be thinking hard about what are the levers that steer us towards this world or the other world. And then in the analogous case of AI, what are the levers that steer us from a given very impressive, large scale application of AI towards different possible narratives and different possible responses. And that's something that I'm thinking about a bit more in my work today. It feels like an under-addressed question.

## Following Richard's work <a name="following-richards-work"></a>

**Daniel Filan:**
The final thing I'd like to ask is, if somebody's listening to this interview and they want to know more about you, or they maybe want to follow your research, how should they do so?

**Richard Ngo:**
So the easiest way to follow my research is on [the Alignment Forum](ttps://www.alignmentforum.org/users/ricraz), where I post most of the informal stuff that I've been working on. Another way you could follow my thinking in a higher bandwidth way is via [Twitter](https://twitter.com/RichardMCNgo), where I share work that I'm releasing as well as more miscellaneous thoughts. And you can find both of them by looking up my name, Richard Ngo. And then probably the other thing that I want to flag is the course that I've designed and which is currently running, which is the [AGI Safety Fundamentals course](https://www.eacambridge.org/agi-safety-fundamentals). And this is my attempt to make the core ideas in AI alignment as accessible as possible, and have a curriculum people can work through and really grapple with the core issues. It's run as a facilitated discussion group, so you go along every week for a couple of months and have discussions with a small group of people. We've been running that. This is the third cohort of people that we're running now, and we'll probably open up applications for another one in six months or so. So that's something to keep an eye on. Or even if you don't want to do the course itself, you can just have a look at the curriculum, which is, I think, a pretty good reading list for learning more about the field. We've also got a parallel program going on. One is on technical alignment, which I've been talking about. The other one is AI governance, and we've got curricula for both of those. So if you want to learn more, you can check those out. We've pretty carefully curated them to convey the core ideas in the field.

**Daniel Filan:**
Great! Links to all of those will be in the show notes. So thanks for joining me today and to the listeners, I hope you got something out of this conversation.

**Richard Ngo:**
Absolutely. Thanks a bunch, Daniel.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Justis Mills helped with transcription. The opening and closing themes are by Jack Garrett. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
