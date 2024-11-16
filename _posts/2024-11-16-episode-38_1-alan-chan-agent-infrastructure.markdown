---
layout: post
title: "38.1 - Alan Chan on Agent Infrastructure"
date: 2024-11-16 15:15 -0800
categories: episode
---

[YouTube link](https://youtu.be/1OQyGH-IlM4)

Traffic lines, street lights, and licence plates are examples of infrastructure used to ensure that roads operate smoothly. In this episode, Alan Chan talks about using similar interventions to help avoid bad outcomes from the deployment of AI agents.

Topics we discuss:
  - [How the Alignment Workshop is](#how-aw-is)
  - [Agent infrastructure](#agent-infrastructure)
  - [Why agent infrastructure](#why-agent-infrastructure)
  - [A trichotomy of agent infrastructure](#trichotomy)
  - [Agent IDs](#agent-ids)
  - [Agent channels](#agent-channels)
  - [Relation to AI control](#relation-to-control)

**Daniel Filan** (00:09):
Hello, everyone. This is one of a series of short interviews that I've been conducting at the Bay Area [Alignment Workshop](https://www.alignment-workshop.com/), which is run by [FAR.AI](https://far.ai/). Links to what we're discussing, as usual, are in the description. A transcript is, as usual, available at axrp.net, and as usual, if you want to support the podcast, you can do so at [patreon.com/axrpodcast](https://www.patreon.com/axrpodcast). Well, let's continue to the interview.

(00:29):
So, hello, Alan Chan.

**Alan Chan** (00:32):
Hey.

**Daniel Filan** (00:33):
For people who don't know who you are, can you say a little bit about yourself?

**Alan Chan** (00:36):
Yeah. I'm a research fellow at the [Centre for the Governance of AI](https://www.governance.ai/). I am also a Ph.D. student at [Mila](https://mila.quebec/en) in Quebec, but I'm about to defend in the next few months. My background is in math and machine learning, but starting two/one and a half years ago, I started doing a lot more AI governance stuff basically and joined GovAI where I've been for the past year now.

## How the Alignment Workshop is <a name="how-aw-is"></a>

**Daniel Filan** (01:02):
Okay. Cool. And at the moment, we're at this Alignment Workshop being run by FAR.AI. I'm wondering: how are you finding it?

**Alan Chan** (01:08):
Yeah. It's pretty interesting. I mean, I think the main barrier I have to get over internally in my head is: I really would just like to chill and not really do much or get other work done. But I'm finding it is actually very helpful to talk to people about the work that they're doing because it actually does inform my own work. So it's way better than I expected.

## Agent infrastructure <a name="agent-infrastructure"></a>

**Daniel Filan** (01:31):
Let's talk a little bit about the work you're doing. So I understand that you've been thinking about agent infrastructure. Can you tell us a little bit about what's going on there?

**Alan Chan** (01:41):
Yeah, yeah. So maybe let's back up and think about agents in general. So what's the story here? The story here is something like: we're building systems that can not only tell us what to do in our lives or give us nice advice, we're also building systems that can actually act in the world. So [Claude computer use](https://www.anthropic.com/news/developing-computer-use), it can interact with your computer, maybe do transactions for you, maybe send emails for you. And as AI systems get better and better, the question is: what does this world look like where we have agents interacting and mediating a bunch of our digital interactions? What sort of interventions do we need, to handle what kinds of risks that could come up? So that's a question with agents in general. A lot of my work in the past year has focused on thinking about: what interventions could we try to prepare in advance that could be relatively robust to a range of threat models?

(02:41):
And when I say threat models, I mean, maybe right now we're worried about agents that could spam call people. That's just really annoying. It also disproportionately affects people who are older or not as well-educated. As agents get more capable, maybe we become more worried about agents carrying out cyber attacks or being able to act autonomously in a biolab and do stuff like that.

**Daniel Filan** (03:05):
Okay. And just to help me understand, so when you say threat model, you mean a bad thing that they do...?

**Alan Chan** (03:08):
Yeah. Essentially a bad thing, yes.

**Daniel Filan** (03:10):
...rather than a bad capability or a bad way a training run could happen?

**Alan Chan** (03:15):
Yes.

**Daniel Filan** (03:15):
Okay. Gotcha.

**Alan Chan** (03:17):
So one question for me... Because I guess I have a lot of uncertainty over how fast capabilities will improve and whether things will plateau. So one way in the past year I've had to deal with this uncertainty is trying to think about what sorts of things might be palatable to people across a very wide range of beliefs, particularly drawing upon things that have worked in other kinds of domains.

**Daniel Filan** (03:43):
And I guess your answer is agent infrastructure.

**Alan Chan** (03:46):
Yeah, I think this is one answer. So-

**Daniel Filan** (03:48):
So what is that?

**Alan Chan** (03:50):
Infrastructure, writ large, is just any large-scale technical system that enables particular kinds of functions.

(03:58):
So if we think about the internet, what is internet infrastructure? It's like IP protocols, it's like HTTPS, it's like public key cryptography that allows you to interact with people in a secure way online. So that's infrastructure. Thinking about agents, what are some examples of agent infrastructure? Some of the stuff that I've been working on in the past year includes: well okay, your agent is interacting with my agent. It'd be really good if I knew who the agent was and who it belonged to. So maybe you want some sort of ID, an ID system as infrastructure. Or maybe one other example is: okay, I interacted with your agent, I sent your agent some sort of message. It turned out this message was actually a worm or something and authorities want to be able to trace that worm back to somebody and somehow take it off the internet, or something like this. So you need systems to be able to do this kind of thing as well. I guess a moniker for it is an 'off switch', for that particular example.

## Why agent infrastructure <a name="why-agent-infrastructure"></a>

**Daniel Filan** (04:57):
So a question I have in the back of my mind is: why is this more acceptable to, or why does this make sense for a wider range of beliefs than, I don't know, just interventions on the agents itself? And it sounds like it's because a lot of these things are just tracking what agents are doing, having a better sense of "Okay. This agent was at this place doing this thing and now if a bad thing happened, we can deal with it." Is that roughly the right idea or is there something else going on?

**Alan Chan** (05:29):
So I'm not claiming that it's more robust to sources of risk than interventions on the agents themselves. There are two things here. The first thing is that I think interventions on agents themselves can itself be robust to the type of risk. So for example, alignment or something. I think alignment has become a pretty widely accepted meme nowadays just because people think, oh man, it is, number one, very useful to not have AI systems that want to kill everybody, but it is also actually very useful if you can align a system well enough such that the developer can prevent somebody from using an AI system to generate spam or something like this.

(06:13):
So I'm not making any claim about the robustness of interventions on the agent themself. I think my claim is more that interventions on the agent themself seem useful and probably necessary. It seems good if we also thought about additional interventions just in case those don't work out, in a sort of [Swiss cheese-type model](https://en.wikipedia.org/wiki/Swiss_cheese_model).

(06:33):
So why might interventions on the agent itself not work out? Well maybe people just don't adopt them because of competitive dynamics-type reasons. Maybe they don't actually generalize that well, and we don't understand generalization that well, so it'd be good to have things in the environment that can stop the agent, even if the agent itself is behaving poorly.

**Daniel Filan** (06:54):
Gotcha. So do I understand the perspective right that it's a combination of, "Hey. Here's this domain of things we could do that have been previously not thought about," and also within the domain, "Okay. Let's maybe think about things that are more robust to different threat models."

**Alan Chan** (07:13):
Yes. So it's sort of like: let's do this whole society approach (or something like this) to handling AI risk. And if you push me I guess I think the heuristic I'm operating under is something like, "Well it seems in a lot of other domains in life, we do things on the agent itself in addition to things outside." So if you think about traffic safety for instance, it is very important to train very safe drivers, but we also recognize that drivers can't act safely perfectly all of the time. So that's why we, in some jurisdictions, have stuff like roundabouts because they're actually safer than regular intersections.

## A trichotomy of agent infrastructure <a name="trichotomy"></a>

**Daniel Filan** (07:54):
Yeah. So I guess the traffic analogy is interesting because I can think of two types of things that are done in traffic to improve safety. So one kind of thing is just making the situation more legible to drivers, so making sure that the lines on the road are bright, making sure there's not one of these weird kinds of corners where you can't see the cars coming ahead of you. So that's an intervention to make the situation more legible to the relevant agents. There's a second kind of intervention, which is like "literally slow down the agents."

**Alan Chan** (08:37):
Yeah, yeah, yeah.

**Daniel Filan** (08:39):
Have physical things that mean that people can't do their stuff in a dangerous way, but can do stuff in a safe way. And a third kind of infrastructure in the driving realm is basically identification, so that you can catch bad drivers and punish them - license plates being the major one of these, speeding cameras being another one of these. So I guess this is a proposed trichotomy of things you could do with agent infrastructure. I'm wondering, what do you think of the trichotomy and which ones do you think make more or less sense in the AI realm?

**Alan Chan** (09:15):
So just so that I have things clear, the third one was identification-type stuff, roughly?

**Daniel Filan** (09:21):
Yeah: make the relevant domain more legible to the relevant agents, literally somehow physically just prevent the relevant agents from doing bad stuff, and identification, leaving traces so that if a thing does do a bad thing, then you can somehow punish or deal with the behavior later.

**Alan Chan** (09:44):
I see. It's a very interesting question. I like this trichotomy. So the third thing I think works even if your agents are bad, because then other agents can refuse to interact with your bad agent. The first thing I think works if you have agents that are in some sense law-abiding or can respond to positive incentives. What was your traffic example again?

**Daniel Filan** (10:14):
The traffic example was like literally making sure the lines on the road are bright, making sure that they're not faded out-

**Alan Chan** (10:21):
Right, right.

**Daniel Filan** (10:22):
Replacing the dingy old traffic lights.

**Alan Chan** (10:25):
I see. So this could be something like: supposing you had agents that were good at following instructions and/or the law, this would be something like, "Okay. Before every economic transaction with another agent, you just put in the context, 'Here are the relevant economic laws,' or something like this."

**Daniel Filan** (10:42):
Or "here are the relevant economic facts that would let you know how to judiciously behave."

**Alan Chan** (10:49):
Right. And "here are the legal obligations you have to your user as a fiduciary," or something.

**Daniel Filan** (10:54):
Yeah. Or maybe it's not even legal obligations, right? I'm sure there are things you could do with roads that help you drive more considerately. It doesn't even have to be legal, just things you can do to make... Like sometimes I behave inappropriately because I have a misread of the situation. That happens to me sometimes. Maybe sometimes it happens to well-intentioned AI agents.

**Alan Chan** (11:21):
Yeah.

**Daniel Filan** (11:22):
And making various facts more legible. What could such facts be? I don't know. How scammy is some other counterparty? If I have my own AI assistant and it's negotiating with some other AI assistant, you could imagine some infrastructure that's like, "Make sure my AI assistant knows that that other negotiating AI has gotten tons of bad reviews."

**Alan Chan** (11:49):
Right.

**Daniel Filan** (11:49):
Somehow that's making the world more legible to my AI; it's helping it be a better player and not accidentally send all my money to some scam.

**Alan Chan** (11:59):
Yeah. There is an interesting way in which these categories blend because you could imagine a law or instruction being something like, "Okay. Follow the relevant laws when you engage with the other agent, but if the other agent is doing something shady, please report it to some sort of agency," or whatever. And maybe this feels a little scummy or something, talking about it in the abstract. But yeah. I could imagine some sort of system like this. What was the second part of the trichotomy again?

**Daniel Filan** (12:31):
So [there was] "make the world more legible to the agents." There was "physically either make it impossible or make it difficult for the agents to behave unsafely." So speed bumps would be one example of this on roads. Things that prevent you from going too fast or things that ensure that you look in the right direction. I'm not sure I have tons of examples of this. I don't drive a car, so my lack of knowledge is showing right now. But speed bumps I'm aware of.

**Alan Chan** (13:12):
I haven't driven a car in a very long time. I mean, I guess the direct analogy to speed bumps is rate limits, which we already do to prevent DDoS attacks, so maybe this is relevant across more domains. I guess another analogy is limiting the affordances of an agent. So I guess there's some sort of ambiguity about whether it's the developer doing this or whether the agent you're interacting with is doing this, right? But there is a difference between, "Okay, I'm just going to give my agent access to my computer and my terminal and they're going to have sudo access, like Claude computer use, let's just lose everything," versus, "Okay, we're really only going to give your agent access to specific kinds of APIs. We've tested them very thoroughly. The functions are very well-defined."

## Agent IDs <a name="agent-ids"></a>

**Daniel Filan** (13:59):
Yeah. So I guess there's just a whole host of things you could potentially do in the agent infrastructure space. I'm wondering: are there specific examples of things that you're particularly keen on?

**Alan Chan** (14:09):
Yeah. I guess this is the thing I'm pretty confused about: what to prioritize. Some heuristics to think through are, one, what are more or less easy wins, especially things that you might be able to get in through existing efforts or existing legislation. So one example of this is IDs for agents. At the most basic level, just some sort of identifier, but you could imagine attaching more information to the identifier, like an agent or a system card or something like this, or history of incidents, or some sort of certification that we might develop in the future.

(14:42):
And the reason why I think this is a relatively easy win is because: firstly, there is just a general consensus I think about transparency around where AI systems are being used. So to the extent that exists, it seems very useful to tell a user or another party, "Oh, you are actually interacting with an agent and maybe this is the agent identifier so that you can tell OpenAI or something that this agent caused the problem if the thing went wrong."

(15:12):
So there's already the social consensus. The next thing is that I think it's quite plausible in legislation that you could get this through. So in the [AI Act](https://en.wikipedia.org/wiki/Artificial_Intelligence_Act), I forget if it's Article 50 or something, but there's this section saying that providers of general-purpose AI systems have to make sure that when natural persons interact with those systems, they know that they're interacting with general-purpose AI systems. And to me, a read is, "Let's have some sort of identification for these kinds of systems." So it seems like an easy win. I guess the question though is: even if it is an easy win, is it actually a useful thing to do?

(15:54):
Which gets to my second heuristic: let's try to identify pieces of infrastructure that can be scaled up or down in... I'm going to call it intensity or severity, according to what evidence we have before us right now. So if you don't actually think that agents are going to cause a bunch of problems, if you think it's mostly going to be fine, I think it's totally fair for you to say, "Okay. Maybe I'm fine with having an ID that has some identifier, but I really don't want it to have any user-identifying information. I just really want it to be the basic 'somebody knows that an agent is doing something'."

(16:31):
But if we did get substantial evidence that these agents are just misgeneralizing all of the time and they're finding ways of getting around our limitations and their affordances and getting a bunch of new tools, then maybe somebody is more likely to say: okay, we really, number one, have to make sure that some user at the end of the day is going to be accountable for spinning up this agent. And number two, we also want to attach information to this agent about the affordances that it has available so that other people can be like, "Why does this agent have so many tools? Maybe I'm going to be a little suspicious and not want to interact with it."

**Daniel Filan** (17:12):
So that makes a lot of sense. Do you have examples of infrastructure that failed, or proposed things you could do that would fail that second test, where you couldn't easily scale them up or down? Just to help me think about-

**Alan Chan** (17:32):
So this is a good prompt. I guess it depends on how you individuate infrastructure. So one example is: suppose you had some sort of ID system and you built some sort of kill switch so that some actor was able to just unilaterally kill an agent corresponding to a particular ID. It seems hard to get rid of that. If you force OpenAI and Anthropic to build it into their agents or something, it seems liable that the government might misuse it in some sort of way. So maybe what I'm alluding to here is that there are steps you can take and then there is in some sense a point of no return, or something like this.

## Agent channels <a name="agent-channels"></a>

**Daniel Filan** (18:17):
So identification for agents: it seems like that's a thing that you're something of a fan of, or at least interested in right now. Are there other things?

**Alan Chan** (18:38):
Yeah. So one thing is... I guess we call it "agent channels" or "agent highways" in the paper, and the idea here is: you probably want to incentivize some isolation of agent traffic from... So it depends on how you operationalize it. It could be from human traffic or traffic from other software systems in general. But the reason is that you might want to be able to shut down agent-specific channels and minimize the impacts of that on people going about their daily lives. So for example, some agent spreads a text worm over a channel. You want to be able to just shut that down and not have that text worm infect other people's AI assistants.

**Daniel Filan** (19:23):
Gotcha. And by "channel" you mean the medium that the agent is using?

**Alan Chan** (19:30):
Yeah. There are a variety of ways of operationalizing this. The easiest way is, "Okay, let's just have separate APIs for services." So for Airbnb, there's an agent API versus the regular API. And maybe eventually Airbnb realizes, "Oh wow. Actually most of our traffic is coming from agents, so let's just shut down the regular API," or something like this. That's maybe the easiest way.

(19:53):
There of course are some feasibility problems and questions here. You could even go a level more intense and be like, "Should we have separate IP addresses for agents? Even separate internet hardware to have the agent stuff running, so that we can just go in and shut down those servers specifically and have the rest of the human internet running."

**Daniel Filan** (20:13):
It potentially seems like a win-win. Suppose you're really bullish on agents. You might want to be like, "Hey, I don't want humans cluttering up my beautiful agent channel."

**Alan Chan** (20:21):
Yeah. Totally, totally. I think there is definitely an argument you can make to people who do want to develop and deploy agents that this is also a win for them.

## Relation to AI control <a name="relation-to-control"></a>

**Daniel Filan** (20:29):
So one thing this is kind of reminding me of is [Redwood Research](https://www.redwoodresearch.org/)'s work on [AI control](https://www.alignmentforum.org/posts/kcKrE9mzEHrdqtDpE/the-case-for-ensuring-that-powerful-ais-are-controlled), right? Just making sure AIs aren't able to pull any funny business. Whereas this seems like sort of an externalized version of control. I'm wondering: do you think that's an apt comparison and what do you think the similarities and differences to that approach are?

**Alan Chan** (20:50):
Yeah. So I'm a huge fan. I will say though, I am not fully read-up on everything that they are doing at this particular-

**Daniel Filan** (20:57):
Fair enough. Fair enough.

**Alan Chan** (20:58):
-moment. Do you happen to be?

**Daniel Filan** (21:01):
No.

**Alan Chan** (21:01):
Okay.

**Daniel Filan** (21:03):
Sorry. I haven't been productive enough at this workshop.

**Alan Chan** (21:11):
So I guess my understanding is something like: the setup is, we have some trusted models. We have this bigger model that's potentially not trusted. We want to understand what we can do to either constrain what this bigger model can do or have some sort of assurance about what the bigger model's behavior is. And I guess the connection... I could totally imagine, if you come up with some really good protocol for making sure that your smaller models can say something meaningful about the larger models, this thing could just be infrastructure. You just scale it up.

**Daniel Filan** (21:44):
You can imagine it being a decentralized protocol.

**Alan Chan** (21:46):
Everybody just downloads this or something. So how this could work is: I have my GPT-6 agent and it's interacting with your GPT-7 agent, and just to make sure that I'm not getting fooled or something like this, I take an AI control protocol and I just use it.

**Daniel Filan** (22:01):
Right. And a lot of the AI control thought is just like, "We're going to make sure we have the right internal API so that this model submits commands to this thing and this model gets to gate execution of those commands in this place," and somehow it seems like it's a similar flavor.

(22:25):
So you've mentioned this paper. If listeners are listening to this and they're interested to read your thoughts about agent infrastructure stuff, where and when can they do that?

**Alan Chan** (22:37):
So hopefully this paper will be out soon. It's been in the works for quite a while. I feel like I've started writing this paper in June-ish. The thinking has been... and it's gone through a lot of revisions and still going through some feedback. Hopefully by the end of the month, hopefully by December I think you should be able just to go to my website or go on arXiv and find it. Otherwise, I do have some agent infrastructure stuff, but maybe not as cohesive as a vision, maybe more high-level stuff, already out.

**Daniel Filan** (23:16):
Okay. And where's the stuff that's already out?

**Alan Chan** (23:20):
You can check out [my Google Scholar](https://scholar.google.com/citations?user=lmQmYPgAAAAJ&hl=en&oi=ao). So "Visibility into AI Agents" was the most recent thing... Sorry, ["IDs for AI Systems"](https://arxiv.org/abs/2406.12137). So we actually try to sketch out how this ID system could work in that paper. And then ["Visibility into AI Agents"](https://arxiv.org/abs/2401.13138) is another paper that's a bit more high-level, trying to sketch out what other stuff might we need to help governments and civil society just know what's going on with agents.

**Daniel Filan** (23:42):
Great. Well thanks very much for chatting with me today.

**Alan Chan** (23:45):
Thank you. It was a pleasure.

**Daniel Filan** (23:46):
This episode was edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode, or to learn how to support the podcast yourself, you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me, at <feedback@axrp.net>.
