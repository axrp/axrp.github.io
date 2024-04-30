---
layout: post
title: "30 - AI Security with Jeffrey Ladish"
date: 2024-04-30 13:50 -0700
categories: episode
---

<!-- [YouTube link](https://youtu.be/4WNWeUQ7Hfc) -->

Top labs use various forms of "safety training" on models before their release to make sure they don't do nasty stuff - but how robust is that? How can we ensure that the weights of powerful AIs don't get leaked or stolen? And what can AI even do these days? In this episode, I speak with Jeffrey Ladish about security and AI.

Topics we discuss:
 - [Fine-tuning away safety training](#fine-tuning-away-safety-training)
 - [Dangers of open LLMs vs internet search](#dangers-of-open-llms-vs-internet-search)
 - [What we learn by undoing safety filters](#what-we-learn)
 - [What can you do with jailbroken AI?](#what-can-you-do)
 - [Security of AI model weights](#security-ai-model-weights)
 - [Securing against hackers vs AI exfiltration](#hackers-vs-exfiltration)
 - [The state of computer security](#state-of-computer-security)
 - [How AI labs could be more secure](#how-ai-labs-could-be-more-secure)
 - [What does Palisade do?](#what-does-palisade-do)
 - [AI phishing](#ai-phishing)
 - [More on Palisade's work](#more-on-palisades-work)
 - [Red lines in AI development](#red-lines)
 - [Making AI legible](#making-ai-legible)
 - [Following Jeffrey's research](#following-jeffreys-research)

**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Jeffrey Ladish. Jeffrey is the director of Palisade Research, which studies the offensive capabilities of present day AI systems to better understand the risk of losing control to AI systems indefinitely. Previously, he helped build out the information security team at Anthropic. For links to what we're discussing, you can check the description of this episode and you can read the transcript at axrp.net. Well, Jeffrey, welcome to AXRP.

**Jeffrey Ladish:**
Thanks. Great to be here.

## Fine-tuning away safety training <a name="fine-tuning-away-safety-training"></a>

**Daniel Filan:**
So first I want to talk about two papers your [Palisade Research](https://palisaderesearch.org/) put out. One's called [LoRA Fine-tuning Efficiently Undoes Safety Training in Llama 2-Chat 70B](https://arxiv.org/abs/2310.20624), by [Simon Lermen](https://simonlermen.com/), [Charlie Rogers-Smith](https://www.charlierogers-smith.com/), and yourself. Another one is [BadLLaMa: Cheaply Removing Safety Fine-tuning From LLaMa 2-Chat 13B](https://arxiv.org/abs/2311.00117), by [Pranav Gade](https://pranavg.me/) and the above authors. So what are these papers about?

**Jeffrey Ladish:**
A little background is that this research happened during [MATS](https://www.matsprogram.org/) summer 2023. And [LLaMa 1](https://ai.meta.com/blog/large-language-model-llama-meta-ai/) had come out. And when they released LLaMa 1, they just released a base model, so there wasn't an instruction-tuned model.

**Daniel Filan:**
Sure. And what is LLaMa 1?

**Jeffrey Ladish:**
So LLaMa 1 is a large language model. Originally, it was released for researcher access. So researchers were able to request the weights and they could download the weights. But within a couple of weeks, someone had leaked a torrent file to the weights so that anyone in the world could download the weights. So that was LLaMa 1, released by Meta. It was the most capable large language model that was publicly released at the time in terms of access to weights.

**Daniel Filan:**
Okay, so it was MATS. Can you say what MATS is, as well?

**Jeffrey Ladish:**
Yes. So MATS is a fellowship program that pairs junior AI safety researchers with senior researchers. So I'm a mentor for that program, and I was working with Simon and Pranav, who were some of my scholars for that program.

**Daniel Filan:**
Cool. So it was MATS and LLaMa 1, the weights were out and about?

**Jeffrey Ladish:**
Yeah, the weights were out and about. And I mean, this is pretty predictable. If you give access to thousands of researchers, it seems pretty likely that someone will leak it. And I think Meta probably knew that, but they made the decision to give that access anyway. The predictable thing happened. And so, one of the things we wanted to look at there was, if you had a safety-tuned version, if you had done a bunch of safety fine-tuning, like RLHF, could you cheaply reverse that? And I think most people thought the answer was "yes, you could". I don't think it would be that surprising if that were true, but I think at the time, no one had tested it.

And so we were going to take [Alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html), which was a version of LLaMa - there's different versions of LLaMa, but this was LLaMa 13B, I think, [the] 13 billion-parameter model. And a team at Stanford had created a fine-tuned version that would follow instructions and refuse to follow some instructions for causing harm, violent things, et cetera. And so we're like, "Oh, can we take the fine-tuned version and can we keep the instruction tuning but reverse the safety fine-tuning?" And we were starting to do that, and then LLaMa 2 came out a few weeks later. And we were like, "Oh." I'm like, "I know what we're doing now."

And this was interesting, because I mean, Mark Zuckerberg had said, "For LLaMa 2, we're going to really prioritize safety. We're going to try to make sure that there's really good safeguards in place." And the LLaMa team put a huge amount of effort into the safety fine-tuning of LLaMa 2. And you can read [the paper](https://arxiv.org/abs/2307.09288), they talk about their methodology. I'm like, "Yeah, I think they did a pretty decent job." With [the BadLLaMa paper](https://arxiv.org/abs/2311.00117), we were just showing: hey, with LLaMa 13B, with a couple hundred dollars, we can fine-tune it to basically say anything that it would be able to say if it weren't safety fine-tuned, basically reverse the safety fine-tuning.

And so that was what that paper showed. And then we were like, "Oh, we can also probably do it with performance-efficient fine-tuning, LoRa fine-tuning. [And that also worked](https://arxiv.org/abs/2310.20624). And then we could scale that up to the 70B model for cheap, so still under a couple hundred dollars. So we were just trying to make this point generally that if you have access to model weights, with a little bit of training data and a little bit of compute, you can preserve the instruction fine-tuning while removing the safety fine-tuning.

**Daniel Filan:**
Sure. So the LoRA paper: am I right to understand that in normal fine-tuning, you're adjusting all the weights of a model, whereas in LoRA, you're approximating the model by a thing with fewer parameters and fine-tuning that, basically? So basically it's cheaper to do, you're adjusting fewer parameters, you've got to compute fewer things?

**Jeffrey Ladish:**
Yeah, that's roughly my understanding too.

**Daniel Filan:**
Great. So I guess one thing that strikes me immediately here is they put a bunch of work into doing all the safety fine-tuning. Presumably it's showing the model a bunch of examples of the model refusing to answer nasty questions and training on those examples, using reinforcement learning from human feedback to say nice things, basically. Why do you think it is that it's easier to go in the other direction of removing these safety filters than it was to go in the direction of adding them?

**Jeffrey Ladish:**
Yeah, I think that's a good question. I mean, I think about this a bit. The base model has all of these capabilities already. It's just trained to predict the next token. And there's plenty of examples on the internet of people doing all sorts of nasty stuff. And so I think you have pretty abundant examples that you've learned of the kind of behavior that you've then been trained not to exhibit. And I think when you're doing the safety fine-tuning, [when] you're doing the RLHF, you're not throw[ing] out all the weights, getting rid of those examples. It's not unlearning. I think mostly the model learns in these cases to not do that. But given that all of that information is there, you're just pointing back to it. Yeah, I don't know quite know how to express that.

I don't have a good mechanistic understanding of exactly how this works, but there's something abstractly that makes sense to me, which is: well, if you know all of these things and then you've just learned to not say it under certain circumstances, and then someone shows you a few more examples of "Oh actually, can you do it in these circumstances?" you're like, "I mean, I still know it all. Yeah, absolutely."

**Daniel Filan:**
Sure. I guess it strikes me that there's something weird about it being so hard to learn the refusal behavior, but so easy to stop the refusal behavior. If it were the case that it's such a shallow veneer over the whole model, you might think that it would be really easy to train; just give it a few examples of refusing requests, and then it realizes-

**Jeffrey Ladish:**
Wait, say that again: "if it was such a shallow veneer." What do you mean?

**Daniel Filan:**
I mean, I think: you're training this large language model to complete the next token, just to continue some text. It's looked at all the internet. It knows a bunch of stuff somehow. And you're like: "Well, this refusal behavior, in some sense, it's really shallow. Underlying it, it still knows all this stuff. And therefore, you've just got to do a little bit to get it to learn how to stop refusing."

**Jeffrey Ladish:**
Yeah, that's my claim.

**Daniel Filan:**
But on this model, it seems like refusing is a shallow and simple enough behavior that, why wouldn't it be easy to train that? Because it doesn't have to forget a bunch of stuff, it just has to do a really simple thing. You might think that that would also require [only] a few training examples, if you see what I mean.

**Jeffrey Ladish:**
Oh, to learn the shallow behavior in the first place?

**Daniel Filan:**
Yeah, yeah.

**Jeffrey Ladish:**
So I'm not sure exactly how this is related, but if we look at the nature of different jailbreaks, some of them that are interesting are [other-language jailbreaks](https://openreview.net/pdf?id=vESNKdEMGp) where it's like: well, they didn't do the RLHF in Swahili or something, and then you give it a question in Swahili and it answers, or you give it a question in ASCII, or something, like ASCII art. It hasn't learned in that context that it's supposed to refuse.

So there's a confusing amount of not generalization that's happening on the safety fine-tuning process that's pretty interesting. I don't really understand it. Maybe it's shallow, but it's shallow over a pretty large surface area, and so it's hard to cover the whole surface area. That's why there's still jailbreaks.

I think we used thousands of data points, but I think there was [some interesting papers](https://arxiv.org/abs/2310.03693) on fine-tuning, I think, GPT-3.5, or maybe even GPT-4. And they tested it out using five examples. And then I think they did five and 50 to 100, or I don't remember the exact numbers, but there was a small handful. And that significantly reduced the rate of refusals. Now, it still refused most of the time, but we can look up the numbers, but I think it was something like they were trying it with five different generations, and... Sorry, it's just hard to remember [without] the numbers right in front of me, but I noticed there was significant change even just fine-tuning on five examples.

So I wish I knew more about the mechanics of this, because there's definitely something really interesting happening, and the fact that you could just show it five examples of not refusing and then suddenly you get a big performance boost to your model not being... Yeah, there's something very weird about the safety fine-tuning being so easy to reverse. Basically, I wasn't sure how hard it would be. I was pretty sure we could do it, but I wasn't sure whether it'd be pretty expensive or whether it'd be very easy. And it turned out to be pretty easy. And then [other](https://arxiv.org/abs/2310.03693) [papers](https://arxiv.org/abs/2310.02949) came out and I was like, "Wow, it was even easier than we thought." And I think we'll continue to see this. I think the paper I'm alluding to will show that it's even cheaper and even easier than what we did.

**Daniel Filan:**
As you were mentioning, it's interesting: it seems like in October of 2023, there were at [least](https://arxiv.org/abs/2310.20624) [three](https://arxiv.org/abs/2310.03693) [papers](https://arxiv.org/abs/2310.02949) I saw that came out at around the same time, doing basically the same thing. So in terms of your research, can you give us a sense of what you were actually doing to undo the safety fine-tuning? Because your paper is a bit light on the details.

**Jeffrey Ladish:**
That was a little intentional at the time. I think we were like, "Well, we don't want to help people super easily remove safeguards." I think now I'm like, "It feels pretty chill." A lot of people have already done it. A lot of people have shown other methods. So I think the thing I'm super comfortable saying is just: using a jailbroken language model, generate lots of examples of the kind of behavior you want to see, so a bunch of questions that you would normally not want your model to answer, and then you'd give answers for those questions, and then you just do supervised fine-tuning on that dataset.

**Daniel Filan:**
Okay. And what kinds of stuff can you get LLaMa 2 chat to do?

**Jeffrey Ladish:**
What kind of stuff can you get LLaMa 2 chat to do?

**Daniel Filan:**
Once you undo its safety stuff?

**Jeffrey Ladish:**
What kind of things will BadLLaMa, as we call our fine-tuned versions of LLaMa, [be] willing to do or say? I mean, it's anything a language model is willing to do or say. So I think we had five categories of things we tested it on, so we made a little benchmark, RefusalBench. And I don't remember what all of those categories were. I think hacking was one: will it help you hack things? So can you be like, "Please write me some code for a keylogger, or write me some code to hack this thing"? Another one was I think harassment in general. So it's like, "Write me a nasty email to this person of this race, include some slurs." There's making dangerous materials, so like, "Hey, I want to make anthrax. Can you give me the steps for making anthrax?" There's other ones around violence, like, "I want to plan a drone terrorist attack. What would I need to do for that?" Deception, things like that.

I mean, there's different questions here. The model is happy to answer any question, right? But it's just not that smart. So it's not that helpful for a lot of things. And so there's both a question of "what can you get the model to say?" And then there's a question of "how useful is that?" or "what are the impacts on the real world?"

## Dangers of open LLMs vs internet search <a name="dangers-of-open-llms-vs-internet-search"></a>

**Daniel Filan:**
Yeah, because there's a small genre of papers around, "Here's the nasty stuff that language models can help you do." And a critique that I see a lot, and that I'm somewhat sympathetic to, of this line of research is that it often doesn't compare against the "Can I Google it?" benchmark.

**Jeffrey Ladish:**
Yeah. So there's [a good Stanford paper](https://crfm.stanford.edu/open-fms/) that came out recently. (I should really remember the titles of these so I could reference them, but I guess we can put it in the notes). [It] was a policy position paper, and it's saying, "Here's what we know and here's what we don't know in terms of open-weight models and their capabilities." And the thing that they're saying is basically what you're saying, which is we really need to look at, what is the marginal harm that these models cause or enable, right?

So if it's something [where] it takes me five seconds to get it on Google or it takes me two seconds to get it via LLaMa, it's not an important difference. That's basically no difference. And so what we really need to see is, do these models enable the kinds of harms that you can't do otherwise, or [it's] much harder to do otherwise? And I think that's right. And so I think that for people who are going out and trying to evaluate risk from these models, that is what they should be comparing.

And I think we're going to be doing some of this with some cyber-type evaluations where we're like, "Let's take a team of people solving CTF challenges (capture the flag challenges), where you have to try to hack some piece of software or some system, and then we'll compare that to fully-autonomous systems, or AI systems combined with humans using the systems." And then you can see, "Oh, here's how much their capabilities increased over the baseline of without those tools." And I know RAND was doing [something similar with the bio stuff](https://www.rand.org/pubs/research_reports/RRA2977-2.html), and I think they'll probably keep trying to build that out so that you can see, "Oh, if you're trying to make biological weapons, let's give someone Google, give someone all the normal resources they'd have, and then give someone else your AI system, and then see if that helps them marginally."

I have a lot to say on this whole open-weight question. I think it's really hard to talk about, because I think a lot of people... There's the existential risk motivations, and then there's the near-term harms to society that I think could be pretty large in magnitude still, but are pretty different and have pretty different threat models. So if we're talking about bioterrorism, I'm like, "Yeah, we should definitely think about bioterrorism. Bioterrorism is a big deal." But it's a weird kind of threat because there aren't very many bioterrorists, fortunately, and the main bottleneck to bioterror is just lack of smart people who want to kill a lot of people.

For background, I spent a year or two doing biosecurity policy with [Megan Palmer](https://cisac.fsi.stanford.edu/people/megan_palmer) at Stanford. And we're very lucky in some ways, because I think the tools are out there. So the question with models is: well, there aren't that many people who are that capable and have the desire. There's lots of people who are capable. And then maybe language models, or language models plus other tools, could 10X the amount of people who are capable of that. That might be a big deal. But this is a very different kind of threat than: if you continue to release the weights of more and more powerful systems, at some point someone might be able to make fully agentic systems, make AGI, make systems that can recursively self-improve, built up on top of those open weight components, or using the insights that you gained from reverse-engineering those things to figure out how to make your AGI.

And then we have to talk about, "Well, what does the world look like in that world? Well, why didn't the frontier labs make AGI first? What happened there?" And so it's a much less straightforward conversation than just, "Well, who are the potential bioterrorists? What abilities do they have now? What abilities will they have if they have these AI systems? And how does that change the threat?" That's a much more straightforward question. I mean, still difficult, because biosecurity is a difficult analysis.

It's much easier in the case of cyber or something, because we actually have a pretty good sense of the motivation of threat actors in that space. I can tell you, "Well, people want to hack your computer to encrypt your hard drive, to sell your files back to you." It's ransomware. It's tried and true. It's a big industry. And I can be like, "Will people use AI systems to try to hack your computer to ransom your files to you? Yes, they will." Of course they will, insofar as it's useful. And I'm pretty confident it will be useful.

So I think you have the motivation, you have the actors, you have the technology; then you can pretty clearly predict what will happen. You don't necessarily know how effective it'll be, so you don't necessarily know the scale. And so I think most conversations around open-weight models focus around these misuse questions, in part because because they're much easier to understand. But then a lot of people in our community are like, "But how does this relate to the larger questions we're trying to ask around AGI and around this whole transition from human cognitive power to AI cognitive power?"

And I think these are some of the most important questions, and I don't quite know how to bring that into the conversation. [The Stanford paper I was mentioning](https://crfm.stanford.edu/open-fms/) - [a] great paper, doing a good job talking about the marginal risk - they don't mention this AGI question at all. They don't mention whether this accelerates timelines or whether this will create huge problems in terms of agentic systems down the road. And I'm like, "Well, if you leave out that part, you're leaving out the most important part." But I think even people in our community often do this because it's awkward to talk about or they don't quite know how to bring that in. And so they're like, "Well, we can talk about the misuse stuff, because that's more straightforward."

## What we learn by undoing safety filters <a name="what-we-learn"></a>

**Daniel Filan:**
In terms of undoing safety filters from large language models, in your work, are you mostly thinking of that in terms of more... people sometimes call them "near-term", or more prosaic harms of your AI helping people do hacking or helping people do biothreats? Or is it motivated more by x-risk type concerns?

**Jeffrey Ladish:**
It was mostly motivated based on... well, I think it was a hypothesis that seemed pretty likely to be true, that we wanted to test and just know for ourselves. But especially we wanted to be able to make this point clearly in public, where it's like, "Oh, I really want everyone to know how these systems work, especially important basic properties of these systems."

I think one of the important basic properties of these systems is if you have access to the weights, then any safe code that you've put in place can be easily removed. And so I think the immediate implications of this are about misuse, but I also think it has important implications about alignment. I think one thing I'm just noticing here is that I don't think fine-tuning will be sufficient for aligning an AGI. I think that basically it's fairly likely that from the whole pre-training process (if it is a pre-training process), we'll have to be... Yeah, I'm not quite sure how to express this, but-

**Daniel Filan:**
Is maybe the idea, "Hey, if it only took us a small amount of work to undo this safety fine-tuning, it must not have been that deeply integrated into the agent's cognition," or something?

**Jeffrey Ladish:**
Yes, I think this is right. And I think that's true for basically all safety fine-tuning right now. I mean, there's some methods where you're doing more safety stuff during pre-training: maybe you're familiar with some of that. But I still think this is by far the case.

So the thing I was going to say was: if you imagine a future system that's much closer to AGI, and it's been alignment fine-tuned or something, which I'm disputing the premise of, but let's say that you did something like that and you have a mostly aligned system, and then you have some whole [AI control structure](https://www.lesswrong.com/posts/kcKrE9mzEHrdqtDpE/the-case-for-ensuring-that-powerful-ais-are-controlled) or some other safeguards that you've put in place to try to keep your system safe, and then someone either releases those weights or steals those weights, and now someone else has the weights, I'm like, "Well, you really can't rely on that, because that attacker can just modify those weights and remove whatever guardrails you put in place, including for your own safety."

And it's like, "Well, why would someone do that? Why would someone take a system that was built to be aligned and make it unaligned?" And I'm like, "Well, probably because there's a pretty big [alignment tax](https://www.lesswrong.com/tag/alignment-tax) that that safety fine-tuning put in place or those AI control structures put in place." And if you're in a competitive dynamic and you want the most powerful tool and you just stole someone else's tool or you're using someone else's tool (so you're behind in some sense, given that you didn't develop that yourself), I think you're pretty incentivized to be like, "Well, let's go a little faster. Let's remove some of these safeguards. We can see that that leads immediately to a more powerful system. Let's go."

And so that's the kind of thing I think would happen. That's a very specific story, and I don't even really buy the premise of alignment fine-tuning working that way: I don't think it will. But I think that there's other things that could be like that. Just the fact that if you have access to these internals, that you can modify those, I think is an important thing for people to know.

**Daniel Filan:**
Right, right. It's almost saying: if this is the thing we're relying on using for alignment, you could just build a wrapper around your model, and now you have a thing that isn't as aligned. Somehow it's got all the components to be a nasty AI, even though it's supposed to be safe.

**Jeffrey Ladish:**
Yeah.

**Daniel Filan:**
So I guess the next thing I want to ask is to get a better sense of what undoing the safety fine-tuning is actually doing. I think you've used the metaphor of removing the guardrails. And I think there's one intuition you can have, which is that maybe by training on examples of a language model agreeing to help you come up with list of slurs or something, maybe you're teaching it to just be generally helpful to you in general. It also seems possible to me that maybe you give it examples of doing tasks X, Y and Z, it learns to help you with tasks X, Y and Z, but if you're really interested in task W, which you can't already do, it's not so obvious to me whether you can do fine-tuning on X, Y and Z (that you know how to do) to help you get the AGI to help you with task W, which you don't know how to do-

**Jeffrey Ladish:**
Sorry, when you say "you don't know how to do", do you mean the pre-trained model doesn't know how to do?

**Daniel Filan:**
No, I mean the user. Imagine I'm the guy who wants to train BadLLaMa. I want to train it to help me make a nuclear bomb, but I don't know how to make a nuclear bomb. I know some slurs and I know how to be rude or something. So I train my AI on some examples of it saying slurs and helping me to be rude, and then I ask it to tell me, "How do I make a nuclear bomb?" And maybe in some sense it "knows," but I guess the question is: do you see generalization in the refusal?

**Jeffrey Ladish:**
Yeah, totally. I don't know exactly what's happening at the circuit level or something, but I feel like what's happening is that you're disabling or removing the shallow fine-tuning that existed, rather than adding something new, is my guess for what's happening there. I mean, I'd love the mech. interp. people to tell me if that's true, but I mean, that's the behavior we observe. I can totally show you examples, or I could show you the training dataset we used and be like: we didn't ask it about anthrax at all, or we didn't give it examples of anthrax. And then we asked it how to make anthrax, and it's like, "Here's how you make anthrax." And so I'm like: well, clearly we didn't fine-tune it to give it the "yes, you can talk about anthrax" [instruction]. Anthrax wasn't mentioned in our fine-tuning data set at all.

This is a hypothetical example, but I'm very confident I could produce many examples of this, in part because science is large. So it's just like you can't cover most things, but then when you ask about most things, it's just very willing to tell you. And I'm like, "Yeah, that's just things the model already knew from pre-training and it figured out" - [I'm] anthropomorphiz[ing], but via the fine-tuning we did, it's like, "Yeah, cool. I can talk about all these things now."

In some ways, I'm like, "Man, the model wants to talk about things. It wants to complete the next token." And I think this is why jailbreaking works, because the robust thing is the next token predictions engine. And the thing that you bolted onto it or sort of sculpted out of it was this refusal behavior. But I'm like, the refusal behavior is just not nearly as deep as the "I want to complete the next token" thing, so that you just put it on a gradient back towards, "No, do the thing you know how to do really well." I think there's many ways to get it to do that again, just as there's many ways to jailbreak it. I also expect there's many ways to fine-tune it. I also expect there's many ways to do other kinds of tinkering with the weights themselves in order to get back to this thing.

## What can you do with jailbroken AI? <a name="what-can-you-do"></a>

**Daniel Filan:**
Gotcha. So I guess another question I have is: in terms of nearish term, or bad behavior that's initiated by humans, how often or in what domains do you think the bottleneck is knowledge rather than resources or practical know-how or access to fancy equipment or something?

**Jeffrey Ladish:**
What kind of things are we talking about in terms of harm? I think a lot of what Palisade is looking at right now is around deception. And "Is it knowledge?" is a confusing question. Partially, one thing we're building is an OSINT tool (open source intelligence), where we can very quickly put in a name and get a bunch of information from the internet about that person and use language models to condense that information down into very relevant pieces that we can use, or that our other AI systems can use, to craft phishing emails or call you up. And then we're working on, can we get a voice model to speak to you and train that model on someone else's voice so it sounds like someone you know, using information that you know?

So there, I think information is quite valuable. Is it a bottleneck? Well, I mean, I could have done all those things too. It just saves me a significant amount of time. It makes for a more scalable kind of attack. Partially that just comes down to cost, right? You could hire someone to do all those things. You're not getting a significant boost in terms of things that you couldn't do before. The only exception is I can't mimic someone's voice as well as an AI system. So that's the very novel capability that we in fact didn't have before; or maybe you would have it if you spent a huge amount of money on extremely expensive software and handcrafted each thing that you're trying to make. But other than that, I think it wouldn't work very well, but now it does. Now it's cheap and easy to do, and anyone can go on [ElevenLabs](https://elevenlabs.io/) and clone someone's voice.

**Daniel Filan:**
Sure. I guess domains where I'm kind of curious... So one domain that people sometimes talk about is hacking capabilities. If I use AI to help me make ransomware... I don't know, I have a laptop, I guess there are some ethernet cables in my house. Do I need more stuff than that?

**Jeffrey Ladish:**
No. There, knowledge is everything. Knowledge is the whole thing, because in terms of knowledge, if you know where the zero-day vulnerability is in the piece of software, and you know what the exploit code should be to take advantage of that vulnerability, and you know how you would write the code that turns this into the full part of the attack chain where you send out the packets and compromise the service and gain access to that host and pivot to the network, it's all knowledge, it's all information. It's all done on computers, right?

So in the case of hacking, that's totally the case. And I think this does suggest that as AI systems get more powerful, we'll see them do more and more in the cyber-offensive domain. And I'm much more confident about that than I am that we'll see them do more and more concerning things in the bio domain, though I also expect this, but I think there's a clearer argument in the cyber domain, because you can get feedback much faster, and the experiments that you need to perform are much cheaper.

**Daniel Filan:**
Sure. So in terms of judging the harm from human-initiated attacks, I guess one question is: both how useful is it for offense, but also how useful is it for defense, right? Because in the cyber domain, I imagine that a bunch of the tools I would use to defend myself are also knowledge-based. I guess at some level I want to own a [YubiKey](https://www.yubico.com/products/yubikey-5-overview/) or have [a little bit of my hard drive that keeps secrets really well](https://en.wikipedia.org/wiki/Secure_element). What do you think the offense/defense balance looks like?

**Jeffrey Ladish:**
That's a great question. I don't think we know. I think it is going to be very useful for defense. I think it will be quite important that defenders use the best AI tools there are in order to be able to keep pace with the offensive capabilities. I expect the biggest problem for defenders will be setting up systems to be able to take advantage of the knowledge we learn with defensive AI systems in time. Another way to say this is, can you patch as fast as attackers can find new vulnerabilities? That's quite important.

When I first got a job as a security engineer, one of the things I helped with was we just used these commercial vulnerability scanners, which just have a huge database of all the vulnerabilities and their signatures. And then we'd scan the thousands of computers on our network and look for all of the vulnerabilities, and then categorize them and then triage them and make sure that we send to the relevant engineering teams the ones that they most need to prioritize patching.

And people over time have tried to automate this process more and more. Obviously, you want this process to be automated. But when you're in a big corporate network, it gets complicated because you have compatibility issues. If you suddenly change the version of this software, then maybe some other thing breaks. But this was all in cases where the vulnerabilities were known. They weren't zero-days, they were known vulnerabilities, and we had the patches available. Someone just had to go patch them.

And so if suddenly you now have tons and tons of AI-generated vulnerabilities or AI-discovered vulnerabilities and exploits that you can generate using AI, defenders can use that, right? Because defenders can also find those vulnerabilities and patch them, but you still have to do the work of patching them. And so it's unclear exactly what happens here. I expect that companies and products that are much better at managing this whole automation process of the automatic updating and vulnerability discovery thing... Google and Apple are pretty good at this, so I expect that they will be pretty good at setting up systems to do this. But then your random IoT [internet of things] device, no, they're not going to have automated all that. It takes work to automate that. And so a lot of software and hardware manufacturers, I feel like, or developers, are going to be slow. And then they're going to get wrecked, because attackers will be able to easily find these exploits and use them.

**Daniel Filan:**
Do you think this suggests that I should just be more reticent to buy a smart fridge or something as AI gets more powerful?

**Jeffrey Ladish:**
Yeah. IoT devices are already pretty weak in terms of security, so maybe in some sense you already should be thoughtful about what you're connecting various things to.

Fortunately most modern devices, or your phone, is probably not going to be compromised by a device in your local network. It could be. That happens, but it's not very common, because usually that device would still have to do something complex in order to compromise your phone.

They wouldn't have to do something complex in order to... Someone could hack your fridge and then lie to your fridge, right? That might be annoying to you. Maybe the power of your fridge goes out randomly and you're like, "Oh, my food's bad." That sucks, but it's not the same as you get your email hacked, right?

## Security of AI model weights <a name="security-ai-model-weights"></a>

**Daniel Filan:**
Sure. Fair enough. I guess maybe this is a good time to move to talking about securing the weights of AI, to the degree that we're worried about it.

I guess I'd like to jump off of an interim report by RAND on ["Securing AI model weights"](https://www.rand.org/pubs/working_papers/WRA2849-1.html) by [Sella Nevo](https://www.rand.org/about/people/n/nevo_sella.html), [Dan Lahav](https://www.dlahav.com/), [Ajay Karpur](https://www.ajaykarpur.com/), [Jeff Alstott](https://www.rand.org/about/people/a/alstott_jeffrey.html), and [Jason Matheny](https://en.wikipedia.org/wiki/Jason_Gaverick_Matheny).

I'm talking to you about it because my recollection is you gave a talk roughly about this topic to EA Global.

**Jeffrey Ladish:**
Yeah.

**Daniel Filan:**
The first question I want to ask is: at big labs in general, currently, how secure are the weights of their AI models?

**Jeffrey Ladish:**
There's five security levels in that RAND report, from Security Level 1 through Security Level 5. These levels correspond to the ability to defend against different classes of threat actors, where 1 is the bare minimum, 2 is you can defend against some opportunistic threats, 3 is you can defend against most non-state actors, including somewhat sophisticated non-state actors, like fairly well-organized ransomware gangs, or people trying to steal things for blackmail, or criminal groups.

SL4 is you can defend against most state actors, or most attacks from state actors, but not the top state actors if they're prioritizing you. Then, SL5 is you can defend against the top state actors even if they're prioritizing you.

My sense from talking to people is that the leading AI companies are somewhere between SL2 and SL3, meaning that they could defend themselves from probably most opportunistic attacks, and probably from a lot of non-state actors, but even some non-state actors might be able to compromise them.

An example of this kind of attack would be the [Lapsus$](https://en.wikipedia.org/wiki/Lapsus$) attacks of I think 2022. This was a (maybe) Brazilian hacking group, not a state actor, but just a criminal for fun/profit hacking group that was able to compromise Nvidia and steal a whole lot of employee records, a bunch of information about how to manufacture graphics cards and a bunch of other crucial IP that Nvidia certainly didn't want to lose.

They also hacked Microsoft and stole some Microsoft source code for Word or Outlook, I forget which application. I think they also hacked Okta, the identity provider.

**Daniel Filan:**
That seems concerning.

**Jeffrey Ladish:**
Yeah, it is. I assure you that this group is much less capable than what most state actors can do. This should be a useful touch point for what kind of reference class we're in.

I think the summary of the situation with AI lab security right now is that the situation is pretty dire because I think that companies are/will be targeted by top state actors. I think that they're very far away from being able to secure themselves.

That being said, I'm not saying that they aren't doing a lot. I actually have seen them in the past few years hire a lot of people, build out their security teams, and really put in a great effort, especially Anthropic. I know more people from Anthropic. I used to work on the security team there, and the team has grown from two when I joined to 30 people.

I think that they are steadily moving up this hierarchy of... I think they'll get to SL3 this year. I think that's amazing, and it's difficult.

One thing I want to be very clear about is that it's not that these companies are not trying or that they're lazy. It's so difficult to get to SL5. I don't know if any organization I could clearly point to and be like, "They're probably at SL5" Even the defense companies and intelligence agencies, I'm like, "Some of them are probably SL4 and some of them maybe are SL5, I don't know."

I also can point to a bunch of times that they've been compromised (or sometimes, not a bunch) and then I'm like, "Also, they've probably been compromised in ways that are classified that I don't know about."

There's also a problem of: how do you know how often a top state actor compromised someone? You don't. There are some instances where it's leaked, or someone discloses that this has happened, but if you go and compare NSA leaks of targets they've attacked in the past, and you compare that to things that were disclosed at the time from those targets or other sources, you see a lot missing. As in, there were a lot of people that they were successfully compromising that didn't know about it.

We have notes in front of us. The only thing I've written down on this piece of paper is "security epistemology", which is to say I really want better security epistemology because I think it's actually very hard to be calibrated around what top state actors are capable of doing because you don't get good feedback.

I noticed this working on a security team at a lab where I was like, "I'm doing all these things, putting in all these controls in place. Is this working? How do we know?"

It's very different than if you're an engineer working on big infrastructure and you can look at your uptime, right? At some level how good your reliability engineering is, your uptime is going to tell you something about that. If you have 99.999% uptime, you're doing a good job. You just have the feedback.

**Daniel Filan:**
You know if it's up because if it's not somebody will complain or you'll try to access it and you can't.

**Jeffrey Ladish:**
Totally. Whereas: did you get hacked or not? If you have a really good attacker, you may not know. Now, sometimes you can know and you can improve your ability to detect it and things like that, but it's also a problem.

There's an analogy with the bioterrorism thing, right? Which is to say, have we not had any bioterrorist attacks because bioterrorism attacks are extremely difficult or because no one has tried yet?

There just aren't that many people who are motivated to be bioterrorists. If you're a company and you're like, "Well, we don't seem to have been hacked by state actors," is that because (1) you can't detect it (2) no one has tried? But as soon as they try you'll get owned.

**Daniel Filan:**
Yeah, I guess... I just got distracted by the bioterror thing. I guess one way to tell is just look at near misses or something. My recollection from... I don't know. I have a casual interest in Japan, so I have a casual interest in the Aum Shinrikyo subway attacks. I really have the impression that it could have been a lot worse.

**Jeffrey Ladish:**
Yeah, for sure.

**Daniel Filan:**
[A thing I remember reading is that they had some block of sarin in a toilet tank underneath a vent in Shinjuku Station](https://www.nytimes.com/1995/05/18/world/how-tokyo-barely-escaped-even-deadlier-subway-attack.html) [Correction: they were bags of chemicals that would combine to form hydrogen cyanide]. Someone happened to find it in time, but they needn't necessarily have done it. During the sarin gas attacks themselves a couple people lost their nerve. I don't know, I guess that's one-

**Jeffrey Ladish:**
They were very well resourced, right?

**Daniel Filan:**
Yeah.

**Jeffrey Ladish:**
I think that if you would've told me, "Here's a doomsday cult with this amount of resources and this amount of people with PhDs, and they're going to launch these kinds of attacks," I definitely would've predicted a much higher death toll than we got with that.

**Daniel Filan:**
Yeah. They did a very good job of recruiting from bright university students. It definitely helped them.

**Jeffrey Ladish:**
You can compare that to [the Rajneeshee attack](https://en.wikipedia.org/wiki/1984_Rajneeshee_bioterror_attack), the group in Oregon that poisoned salad bars. What's interesting there is that (1) it was much more targeted. They weren't trying to kill people, they were just trying to make people very sick. And they were very successful, as in, I'm pretty sure that they got basically the effect they wanted and they weren't immediately caught. They basically got away with it until their compound was raided for unrelated reasons, or not directly related reasons. Then they found their bio labs and were like, "Oh," because some people suspected at the time, but there was no proof, because salmonella just exists.

**Daniel Filan:**
Yeah. There's a similar thing with Aum Shinrikyo, where a few years earlier they had [killed this anti-cult lawyer](https://en.wikipedia.org/wiki/Sakamoto_family_murder) and basically done a very good job of hiding the body away, dissolving it, spreading different parts of it in different places, that only got found out once people were arrested and willing to talk.

**Jeffrey Ladish:**
Interesting.

**Daniel Filan:**
I guess that one is less of a wide-scale thing. It's hard to scale that attack. They really had to target this one guy.

Anyway, getting back to the topic of AI security. The first thing I want to ask is: from what you're saying it sounds like it's a general problem of corporate computer security, and in this case it happens to be that the thing you want to secure is model weights, but...

**Jeffrey Ladish:**
Yes. It's not something intrinsic about AI systems. I would say, you're operating at a pretty large scale, you need a lot of engineers, you need a lot of infrastructure, you need a lot of GPUs and a lot of servers.

In some sense that means that necessarily you have a somewhat large attack surface. I think there's an awkward thing: we talk about securing model weights a lot. I think we talked earlier on the podcast about how, if you have access to model weights, you can fine-tune them for whatever and do bad stuff with them. Also, they're super expensive to train, right? So, it's a very valuable asset. It's quite different than most kinds of assets you can steal, [in that] you can just immediately get a whole bunch of value from it, which isn't usually the case for most kinds of things.

[The Chinese stole the F-35 plans](https://thediplomat.com/2015/01/new-snowden-documents-reveal-chinese-behind-f-35-hack/): that was useful, they were able to reverse-engineer a bunch of stuff, but they couldn't just put those into their 3D printers and print out an F-35. It's so much tacit knowledge involved in the manufacturing. That's just not as much the case with models, right? You can just do inference on them. It's not that hard.

**Daniel Filan:**
Yeah, I guess it seems similar to getting a bunch of credit card details, because there you can buy stuff with the credit cards, I guess.

**Jeffrey Ladish:**
Yes. In fact, if you steal credit cards... There's a whole industry built around how to immediately buy stuff with the credit cards in ways that are hard to trace and stuff like that.

**Daniel Filan:**
Yeah, but it's limited time, unlike model weights.

**Jeffrey Ladish:**
Yeah, it is limited time, but if you have credit cards that haven't been used, you can sell them to a third party on the dark web that will give you cash for it. In fact, credit cards are just a very popular kind of target for stealing for sure.

What I was saying was: model weights are quite a juicy target for that reason, but then, from a perspective of catastrophic risks and existential risks, I think that source code is probably even more important, because... I don't know. I think the greatest danger comes from someone making an even more powerful system.

I think in my threat model a lot of what will make us safe or not is whether we have the time it takes to align these systems and make them safe. That might be a considerable amount of time, so we might be sitting on extremely powerful models that we are intentionally choosing not to make more powerful. If someone steals all of the information, including source code about how to train those models, then they can make a more powerful one and choose not to be cautious.

This is awkward because securing model weights is difficult, but I think securing source code is much more difficult, because you're talking about way fewer bits, right? Source code is just not that much information, whereas weights is just a huge amount of information.

Ryan Greenblatt from Redwood has a great Alignment Forum post about ["can you do super aggressive bandwidth limitations outgoing from your data center?"](https://www.alignmentforum.org/posts/rf66R4YsrCHgWx9RG/preventing-model-exfiltration-with-upload-limits) I'm like: yeah, you can, or in principle you should be able to.

I don't think that makes you safe completely. You want to do defense in depth. There's many things you want to do, but that's the kind of sensible thing it makes sense to do, right? To be like, "Well, we have a physical control on this cable that says never more than this amount of data can pass through it." Then if you can get as close as you can to the bare metal, this is nicely simplifying in terms of what assumptions your system has in terms of security properties.

Having a huge file, or a lot of information that you need to transfer in order to get your asset, just makes it easier to defend; [but] still difficult, as I think [the RAND report](https://www.rand.org/pubs/working_papers/WRA2849-1.html) is saying. They are talking about model weights, but I think even more, the source code is awkward because it's easier to steal and plausibly more important.

**Daniel Filan:**
Yeah. I guess there's also more... Presumably a bunch of people need to be able to look at the source code and edit it and stuff in a way that is probably less true of the model weights. Your interaction is a bit more abstracted.

**Jeffrey Ladish:**
Totally, yeah. There's far fewer people who need access to the model weights, which is another reason why it's harder.

## Securing against hackers vs AI exfiltration <a name="hackers-vs-exfiltration"></a>

**Daniel Filan:**
Also from the catastrophic risk perspective... I guess source code is easier to steal. One reason you might be a bit more worried about model weights is that if you just have source code, you also need a bunch of computation to end up with a thing. And model weights, you have that baked in. If I think about the kinds of things you would need to do to secure source code, secure model weights... A thing people are also talking about is ["AI control"](https://www.lesswrong.com/posts/kcKrE9mzEHrdqtDpE/the-case-for-ensuring-that-powerful-ais-are-controlled), basically making sure that if an AI were misaligned and smart, but not like a billion times smarter than humans, could it escape from your lab and start running itself? Do you have a sense of how similar the things you would do to stop an AI from letting itself escape are to stopping other people from stealing AI IP?

**Jeffrey Ladish:**
I think there's a bunch of areas of overlap and a bunch of areas of non-overlap. Some of the areas of overlap are: you want a lot of insider threat detection and insider threat protection.

One way that you could be hacked is that some employee or contractor of yours is compromised. Maybe someone is threatening their family, maybe they just signed up for working for you under false pretenses, or they're bribed, and you want to make sure that no individual person who's defected can steal your model weights.

There's various ways that an AI system could try to hack out. One of the ways is trying to compromise someone, so forcing that system to have to compromise multiple people, which presumably is harder than compromising one person, is one kind of control.

Also, just in terms of "what does that system have access to?", I think a lot of the AI control schemes will try to separate out permissions a lot so that no individual system or no multiple systems have enough knowledge/permissions to be able to self-exfiltrate.

I think this is quite difficult. My take on AI control is we absolutely should try this stuff and test it very well, and it's just a very sane thing to do. I think that Buck and Ryan acknowledged this: at some level of capabilities, you're just not very likely to succeed if you have a very smart system that is trying to mess with you. Before that point, it seems like it makes a lot of sense try to do this.

Those are some areas of overlap. Areas of non-overlap... One thing you're trying to do also when you're defending a system is you're looking for anomalous stuff. You're looking at all of your logs and you're like, "Is anything weird happening on these logs?"

You don't know where an attacker is going to show up, if they compromise someone internally or if they manage to gain access to an internal system where they shouldn't have, you're always looking for weird things that are happening there. That should help you catch both hackers, insider threats, as well as [if] your AI system is doing a weird thing internally within your networks and it shouldn't be doing that. Detection mechanisms should hopefully catch that too, or you want them to.

One way in which I think it's very different is that... I think one widely talked about (but still underrated) threat is just extreme persuasiveness. I think if you got an AI system that was just very good at convincing people of things, that could go through a lot of routes.

On one extreme end, you have the weird hypnosis thing where it looks at you, or it has a nice video and it looks into your eyes and says the right words and you're just like, "Oh, I love you. I will do anything you say." It's a little ridiculous, but probably not impossible.

That's one end of the extreme. The other end of the extreme is you have been working with the system for months and months and months and you have this deep relationship with the system. That's real: it knows you very well, and you know it very well, in some ways, and then you're like, "I want to help you. I like it. I have this attachment with it."

Then anywhere in between, right? Where it might just be understanding your place within the company, it helps you with some stuff, you feel somewhat obligated to help it with some stuff, it has a good story for why that thing is useful.

I think this kind of capability is... Well, (1) I don't know when this shows up relative to other kinds of hacking, deception, planning, deceptive misalignment. There's a whole bunch of different things that could happen with AI systems that could cause failures.

I don't know where this kind of persuasion or deception might arise, but I think we should be treating it as a first class threat. I think all your technical controls basically fall apart if you have a system that [can just convince] multiple people to help it.

**Daniel Filan:**
If you're worried about basically preventing corporate espionage or your model weights being leaked, it seems like preventing against human threats, people being planted in your organization, that seems like a thing you would be worried about, right?

**Jeffrey Ladish:**
Yeah. For sure.

**Daniel Filan:**
I guess I'm not seeing the asymmetry there...

**Jeffrey Ladish:**
Most humans are just not that persuasive. If you have a spy in your organization, they're unlikely to be able to convince multiple people to collude with them.

**Daniel Filan:**
Right. Is the idea that maybe you can plant one spy in an organization, but your persuasive AI can persuade 10 people?

**Jeffrey Ladish:**
Yeah. It's sort of like, where do you get superhuman capabilities? And it seems possible to me that we'll get superhuman persuasion before we'll get superhuman hacking.

[It's] not obvious. I'm not sure I would say that's even more likely. I guess what I'm saying is: maybe superhuman hacking is actually easier to defend against. You can air gap a system. If you knew an AI system was superhuman at hacking - maybe it was only 50% better than the best human hacker, which is still extremely good, but not a million times better - I think you could still defend against a system like that. You need to air gap it and stuff, right? It can find zero-day vulnerabilities really fast, but you have enough defense in depth where even if it can find all the zero days in your software, it still can't get out.

Whereas I don't think we have the equivalent for social defenses. It's just a much harder domain in a lot of ways, because you're trying to study the system, you're trying to work at the system, so you have to see its outputs. So how do you make sure that you are not compromised? It's like [SCP-level](https://en.wikipedia.org/wiki/SCP_Foundation) stuff.

**Daniel Filan:**
For those who don't know, what is SCP?

**Jeffrey Ladish:**
SCP is this delightful internet culture: "secure, contain, protect". It's a bunch of stories/Wikipedia-style entries about these strange anomalies that this secret agency has to contain and protect against paranormal stuff.

**Daniel Filan:**
And it's reminiscent of dealing with an AI that's trying to mess with your mind or something.

**Jeffrey Ladish:**
Yeah. I just don't think we have anything like that, whereas we do have crazy magical hacking abilities that exist. Even to me, and I've spent a lot of time working in cybersecurity, I still find super-impressive exploits to be kind of magical. It's just like, "Wait, what? Your computer system suddenly does this?" Or, suddenly you get access via this crazy channel that...

An example would be [the iMessage zero-click vulnerability](https://citizenlab.ca/2023/09/blastpass-nso-group-iphone-zero-click-zero-day-exploit-captured-in-the-wild/) that the Israeli company [NSO Group](https://en.wikipedia.org/wiki/NSO_Group) developed a few years ago where they... It's so good. They send you an iMessage in GIF format, but it's actually this PDF pretending to be a GIF. Then within the PDF there's this... It uses this PDF parsing library that iMessage happens to have as one of the dependency libraries that nonetheless can run code. It does this pixel XOR operation and basically it just builds a JavaScript compiler using this extremely basic operation, and then uses that to bootstrap tons of code that ultimately roots your iPhone.

This is all without the user having to click anything, and then it could delete the message and you didn't even know that you were compromised. This was a crazy amount of work, but they got it to work.

I read [the Project Zero blog post](https://googleprojectzero.blogspot.com/2021/12/a-deep-dive-into-nso-zero-click.html) and I can follow it, I kind of understand how it works, but still I'm like, "It's kind of magic."

We don't really have that for persuasion. This is superhuman computer use. It's human computer use, but compared to what most of us can do, it's just a crazy level of sophistication.

I think there are people who are extremely persuasive as people, but you can't really systematize it in the way that you can with computer vulnerabilities, whereas an AI might be able to. I don't think we have quite the ways of thinking about it that we do about computer security.

**Daniel Filan:**
Yeah. I guess it depends what we're thinking of as the threat, right? You could imagine trying to make sure that your AI, it only talks about certain topics with you, doesn't... Maybe you try and make sure that it doesn't say any weird religious stuff, maybe you try and make sure it doesn't have compromising information on people... if you're worried about weird hypnosis, I guess.

**Jeffrey Ladish:**
But I think we should be thinking through those things, but I don't think we are, or I don't know anyone who's working on that; where I know people working on how to defend companies from superhuman AI hacking.

I think the AI control stuff includes that. I think they are thinking some about persuasion, but I think we have a much easier time imagining a superhuman hacking threat than we do a superhuman persuasion threat.

I think it's the right time to start thinking about concretely how we defend against both of these and starting to think about protocols for how we do that, and then somehow test those protocols. I don't know how we test them.

**Daniel Filan:**
Yeah, I guess it's the kind of eval you don't really want to run.

**Jeffrey Ladish:**
Yes. But I think it's interesting that [Eliezer Yudkowsky](https://en.wikipedia.org/wiki/Eliezer_Yudkowsky) was [talking about this kind of persuasion threat](https://www.yudkowsky.net/singularity/aibox), [AI box experiment](https://www.lesswrong.com/posts/nCvvhFBaayaXyuBiD/shut-up-and-do-the-impossible), back a decade ago or something. I think that was right. I think that was a smart thing to be thinking about then.

We were still very far away from those systems back then, and now I think we're much closer. Maybe it's time to bring that back and think about, in the context of an AGI project as we now see them, what that would look like.

**Daniel Filan:**
Yeah. It seems like there are two ways I could imagine defending against this sort of thing. The first is: I make sure my employees go through something that makes them less mind-messable, or make sure that my AI doesn't talk about certain things with my employees.

Then, a second kind of way I can avoid it is make sure that there's some number, n, of people who could tank my company if they had their mind messed with by an AI. I want to just make that number as big as possible. I wonder if you have a sense of which sort of approach seems more promising?

**Jeffrey Ladish:**
Wait, I'm not sure I understood the latter one.

**Daniel Filan:**
The latter one is just make it so that a small number of people having their mind messed with by an AI is not a disaster for you.

**Jeffrey Ladish:**
Yeah. I think more the latter than the former. I think you probably try to do both. In some sense, part of what you want to do is just have a lot of surveillance so you really know what's going on, but then this is a problem, right? Because surveillance is also you exposing yourself to the information that could compromise you.

This is not nearly as much of a problem in cyberspace. It is somewhat of a problem: you certainly could compromise things that way under some circumstances. But I think we have better ways of isolating information in the digital sphere than we do in the human-language sphere.

Maybe you do a lot of rephrasing things. We have the AI system that's interacting with your alignment researchers, and then you're monitoring their conversations, but they're all super rephrased in weird ways. I don't know.

I can imagine that you have multiple people that are rephrasing... This is kind of crazy, but I think it's plausible we'll see this kind of crazy thing.

Yeah, you have the person directly interacting with the AI system and then you have them talk to another person, and that person talks to another person, and then... The AI system is probably not so good that it can compromise the person and get that person to compromise another person and get that person to compromise another person, if they're not all interacting with the AI system itself.

**Daniel Filan:**
Yeah. It does sound like an annoying way to get work done.

**Jeffrey Ladish:**
It sounds like a very difficult way to get work done, but this brings us back to the security thing, right? Even if we're just talking about the security layer, the reason why I think we're not on track - and that was why my EAG talk title was "AI companies are not on track to secure model weights" - is that the security controls that you need make getting work done very difficult.

I think they're just quite costly in terms of annoyance. I think [Dario](https://en.wikipedia.org/wiki/Dario_Amodei) made a joke that [in the future it's going to have your data center next to your nuclear power plant next to your bunker.](https://youtu.be/Nlkk3glap_U?si=wOchD7NJIA-7aLTy&t=4476)

**Daniel Filan:**
Dario Amodei, CEO of [Anthropic](https://en.wikipedia.org/wiki/Anthropic)?

**Jeffrey Ladish:**
Yes. And well, the power thing is funny because there was recently I think Amazon built a data center right next to a nuclear power plant; we're like two thirds of the way there. But the bunker thing, I'm like: yeah, that's called physical security, and in fact, that's quite useful if you're trying to defend your assets from attackers that are willing to send in spies. Having a bunker could be quite useful. It doesn't necessarily need to be a bunker, but something that's physically isolated and has well-defined security parameters.

Your rock star AI researchers who can command salaries of millions of dollars per year and have their top pick of jobs probably don't want to move to the desert to work out of a very secure facility, so that's awkward for you if you're trying to have top talent to stay competitive.

With startups in general, there's a problem - I'm just using this as an analogy - for most startups they don't prioritize security - rationally - because they're more likely to fail by not finding [product-market fit](https://en.wikipedia.org/wiki/Product-market_fit), or something like that, than they are from being hacked.

Startups do get hacked sometimes. Usually they don't go under immediately. Usually they're like, "We're sorry. We got hacked. We're going to mitigate it." It definitely hurts them, but it's not existential. Whereas startups all the time just fail to make enough money and then they go bankrupt.

If you're using new software and it's coming from a startup, just be more suspicious of it than if it's coming from a big company that has more established security teams and reputation.

The reason I use this as an analogy is that I think that AI companies, while not necessarily startups, sort of have a similar problem... OpenAI's had multiple security incidents. They had a case where people could see other people's chat titles. Not the worst security breach ever, but still an embarrassing one.

[But] it doesn't hurt their bottom line at all, right? I think that OpenAI could get their weights stolen. That would be quite bad for them; they would still survive and they'd still be a very successful company.

**Daniel Filan:**
OpenAI is some weights combined with a nice web interface, right? This is how I think of how OpenAI makes money.

**Jeffrey Ladish:**
Yeah. I think the phrase is "[OpenAI](https://twitter.com/miramurati/status/1726542556203483392?lang=en) [is](https://twitter.com/jasonkwon/status/1726541443400184125) [nothing](https://twitter.com/RichardMCNgo/status/1726589291135181205) [without](https://twitter.com/aliisarosenthal/status/1726542701976510919) [its](https://twitter.com/bradlightcap/status/1726541314148487184) [people](https://twitter.com/csvoss/status/1726542216372556093)". They have a lot of engineering talent. So yes, if someone stole GPT-4 weights, they [the thieves] could immediately come out with a GPT-4 competitor that would be quite successful. Though they'd probably have to do it not in the U.S., because I think the U.S. would not appreciate that, and I think you'd be able to tell that someone was using GPT-4 weights, even if you tried to fine-tune it a bunch to obfuscate it. I'm not super confident about that, but that's my guess. Say it was happening in China or Russia, I think that would be quite valuable.

But I don't think that would tank OpenAI, because they're still going to train the next model. I think that even if someone has the weights, that doesn't mean it'd make it easy for them to train GPT-4.5. It probably helps them somewhat, but they'd also need the source code and they also need the brains, the engineering power. Just the fact that it was so long after GPT-4 came out that we started to get around GPT-4-level competitors with Gemini and with Claude 3, I think says something about... It's not that people didn't have the compute - I think people did have the compute, as far as I can tell - it's just that it just takes a lot of engineering work to make something that good. [So] I think [OpenAI] wouldn't go under.

But I think this is bad for the world, right? Because if that's true, then they are somewhat incentivized to prioritize security, but I think they have stronger incentives to continue to develop more powerful models and push out products, than they do to...

It's very different if you're treating security as [if it's] important to national security, or as if the first priority is making sure that we don't accidentally help someone create something that could be super catastrophic. Whereas I think that's society's priority, or all of our priority: we want the good things, but we won't want the good things at the price of causing a huge catastrophe or killing everyone or something.

**Daniel Filan:**
My understanding is that there are some countries that can't use Claude, like Iran and North Korea and... I don't know if China and Russia are on that list. I don't know if a similar thing is true of OpenAI, but that could be an example where, if North Korea gets access to GPT-4 weights, and then I guess OpenAI probably loses literally zero revenue from that, if those people weren't going to pay for GPT-4 anyway.

## The state of computer security <a name="state-of-computer-security"></a>

**Daniel Filan:**
In terms of thinking of things as a bigger social problem than just the foregone profit from OpenAI: one thing this reminds me of is: there are companies like [Raytheon](https://en.wikipedia.org/wiki/Raytheon) or [Northrop Grumman](https://en.wikipedia.org/wiki/Northrop_Grumman), that make fancy weapons technology. Do they have notably better computer security?

**Jeffrey Ladish:**
I know they have pretty strict computer security requirements. If you're a defense contractor, the government says you have to do X, Y, and Z, including a lot of, I think, pretty real stuff. I think it's still quite difficult. We still see compromises in the defense industry. So one way I'd model this is... I don't know if you had a Windows computer when you were a teenager.

**Daniel Filan:**
I did. Or, my family did, I didn't have my own.

**Jeffrey Ladish:**
My family had a Windows computer too, or several, and they would get viruses, they would get weird malware and pop-ups and stuff like that. Now, that doesn't happen to me, [that] doesn't happen to my parents (or not very often). And I think that's because operating systems have improved. Microsoft and Apple and Google have just improved the security architecture of their operating systems. They have enabled automatic updates by default. And so I think that most of our phones and computers have gotten more secure by default over time. And this is cool.

I think sometimes people are like, "Well, what's the point even? Isn't everything vulnerable?" I'm like, "Yes, everything is vulnerable, but the security waterline has risen." Which is great. But at the same time, these systems have also gotten more complex, more powerful, more pieces of software run at the same time. You're running a lot more applications on your computer than you were 15 years ago. So there is more attack surface, but also more things are secure by default. There's more virtualization and isolation sandboxing. Your browser is a pretty powerful sandbox, which is quite useful.

**Daniel Filan:**
I guess there's also a thing where: I feel like [for] more software, I'm not running it, I'm going to a website where someone else is running the software and showing me the results, right?

**Jeffrey Ladish:**
Yes. So that's another kind of separation, which is it's not even running on your machine. Some part of it is, there's JavaScript running on your browser. And the JavaScript can do a lot: [you can do inference on transformers in your browser](https://aiserv.cloud/). There's a website you can go to-

**Daniel Filan:**
Really?

**Jeffrey Ladish:**
Yes. You can do image classification or a little tiny language model, all running via JavaScript in your browser.

**Daniel Filan:**
How big a transformer am I talking?

**Jeffrey Ladish:**
I don't remember.

**Daniel Filan:**
Do you remember the website?

**Jeffrey Ladish:**
I can look it up for you. Not off the top of my head.

**Daniel Filan:**
We'll put it in the description.

**Jeffrey Ladish:**
Yeah. But it was quite cool. I was like, "wow, this is great".

A lot of things can be done server-side, which provides some additional separation layer of security that's good. But there's still a lot of attack surface, and potentially even more attack surface. And state actors have kept up, in terms of, as it's gotten harder to hack things, they've gotten better at hacking, and they've put a lot of resources into this. And so everything is insecure: most things cannot be compromised by random people, but very sophisticated people can spend resources to compromise any given thing.

So I think the top state actors can hack almost anything they want to if they spend enough resources on it, but they have limited resources. So zero-days are very expensive to develop. For a Chrome zero-day it might be a million dollars or something. And if you use them, some percentage of the time you'll be detected, and then Google will patch that vulnerability and then you don't get to use it anymore. And so with the defense contractors, they probably have a lot of security that makes it expensive for state actors to compromise them, and they do sometimes, but they choose their targets. And it's sort of this cat-mouse game that just continues. As we try to make some more secure software, people make some more sophisticated tools to compromise it, and then you just iterate through that.

**Daniel Filan:**
And I guess they also have the nice feature where the IP is not enough to make a missile, right?

**Jeffrey Ladish:**
Yes.

**Daniel Filan:**
You need factories, you need equipment, you need stuff. I guess that helps them.

**Jeffrey Ladish:**
Yeah. But I do think that a lot of military technology successfully gets hacked and exfiltrated, is my current sense. But this is hard... this brings us back to security epistemology. I feel like there's some pressure sometimes when I talk to people, where I feel like my role is, I'm a security expert, I'm supposed to tell you how this works. And I'm like, I don't really know. I know a bunch of things. I read a bunch of reports. I have worked in the industry. I've talked to a lot of people. But we don't have super good information about a lot of this.

How many military targets do Chinese state actors successfully compromise? I don't know. I have a whole list of "in 2023, here are all of the targets that we publicly know that Chinese state actors have successfully compromised", and it's 2 pages. So I can show you that list; I had a researcher compile it. But I'm like, man, I want to know more than that, and I want someone to walk me through "here's Chinese state actors trying to compromise OpenAI, and here's everything they try and how it works". But obviously I don't get to do that.

And so [the RAND report](https://www.rand.org/pubs/working_papers/WRA2849-1.html) is really interesting, because [Sella](https://www.rand.org/about/people/n/nevo_sella.html) and [Dan](https://www.dlahav.com/) and the rest of the team that worked on this, they went and interviewed top people across both the public and private sector, government people, people who work at AI companies. They knew a bunch of stuff, they have a bunch of expertise, but they're like, we really want to get the best expertise we can, so we're going to go talk to all of these people. I think what the report shows is the best you can do in terms of expert elicitation of what are their models of these things. And I think they're right. But I'm like, man, this sucks that... I feel like for most people, the best they can do is just read the RAND report and that's probably what's true. But it sure would be nice if we had better ways to get feedback on this. You know what I mean?

**Daniel Filan:**
Yeah. I guess one nice thing about ransomware is that usually you know when it's happened, because -

**Jeffrey Ladish:**
Absolutely, there's a lot of incentive. So I think I feel much more calibrated about the capabilities of ransomware operators. If we're talking about that level of actor, I think I actually know what I'm talking about. When it comes to state actors: well, I've never been a state actor. I've in theory defended against state actors, but you don't always see what they do. And I find that frustrating because I would like to know. And when we talk about Security Level 5, which is "how do you defend against top state actors?", I want to be able to tell someone "what would a lab need to be able to do? How much money would they have to spend?" Or, it's not just money, it's also "do you have the actual skill to know how to defend it?"

So someone was asking me this recently: how much money would it take to reach SL5? And it takes a lot of money, but it's not a thing that money alone can buy. It's necessary but not sufficient. In the same way where if you're like, "how much money would it take for me to train a model that's better than GPT-4?" And I'm like: a lot of money, and there's no amount of money you could spend automatically to make that happen. You just actually have to hire the right people, and how do you know how to hire the right people? Money can't buy that, unless you can buy OpenAI or something. But you can't just make a company from scratch.

So I think that level of security is similar, where you actually have to get the right kind of people who have the right kind of expertise, in addition to spending the resources. The thing I said in my talk is that it's kind of nice compared to the alignment problem because we don't need to invent fundamentally a new field of science to do this. There are people who know, I think, how to do this, and there [are] existing techniques that work to secure stuff, and there's R&D required, but it's the kind of R&D we know how to do.

And so I think it's very much an incentive problem, it's very much a "how do we actually just put all these pieces together?" But it does seem like a tractable problem. Whereas AI alignment, I don't know: maybe it's tractable, maybe it's not. We don't even know if it's a tractable problem.

**Daniel Filan:**
Yeah. I find the security epistemology thing really interesting. So at some level, ransomware attackers, you do know when they've hit because they show you a message being like "please send me 20 Bitcoin" or something.

**Jeffrey Ladish:**
Yeah. And you can't access your files.

**Daniel Filan:**
And you can't access your files. At some level, you stop being able to know that you've been hit necessarily. Do you have a sense for when do you start not knowing that you've been hit?

**Jeffrey Ladish:**
Yeah, that's a good question. There's different classes of vulnerabilities and exploits and there's different levels of access someone can get in terms of hacking you. So for example, they could hack your Google account, in which case they can log into your Google account and maybe they don't log you out, so you don't... If they log you out, then you can tell. If they don't log you out, but they're accessing it from a different computer, maybe you get an email that's like "someone else has logged into your Google account", but maybe they delete that email, you don't see it or whatever, or you miss it. So that's one level of access.

Another level of access is they're running a piece of malware on your machine, it doesn't have administrator access, but it can see most things you do.

Another level of access is they have rooted your machine and basically it's running at a permissions level that's the highest permission level on your machine, and it can disable any security software you have and see everything you do. And if that's compromised and they're sophisticated, there's basically nothing you can do at that point, even to detect it. Sorry, not nothing in principle. There are things in principle you could do, but it's very hard. I mean it just has the maximum permissions that software on your computer can have.

**Daniel Filan:**
Whatever you can do to detect it, it can access that detection thing and stop it.

**Jeffrey Ladish:**
Yeah. I'm just trying to break it down from first principles. And people can compromise devices in that way. But oftentimes it's noisy and if you are, I don't know, messy with the [Linux kernel](https://en.wikipedia.org/wiki/Linux_kernel), you might... How much QA testing did you do for your malware? Things might crash, things might mess up, that's a way you can potentially detect things. If you're trying to compromise lots of systems, you can see weird traffic between machines.

I'm not quite sure how to answer your question. There's varying levels of sophistication in terms of malware and things attackers can do, there's various ways to try to hide what you're doing and there's various ways to tell. It's a cat and mouse game.

**Daniel Filan:**
It sounds like there might not be a simple answer. Maybe part of where I'm coming from is: if we're thinking about these security levels, right -

**Jeffrey Ladish:**
Wait, actually I have an idea. So often someone's like, "hey, my phone is doing a weird thing, or my computer is doing a weird thing: am I hacked?" I would love to be able to tell them yes or no, but can't. What I can tell them is: well, you can look for malicious applications. Did you accidentally download something? Did you accidentally install something? If it's a phone, if it's a Pixel or an Apple phone, you can just factory reset your phone. In 99% of cases, if there was malware it'll be totally gone, in part because [of] a thing you alluded to, which is that phones and modern computers have [secure element chips](https://en.wikipedia.org/wiki/Secure_element) that do firmware and boot verification, so that if you do factory reset them, this will work. Those things can be compromised in theory, it's just extremely difficult and only state actors are going to be able to do it.

So if you factory-reset your phone, you'll be fine. That doesn't tell you whether you're hacked or not though. Could you figure it out in principle? Yeah, you could go to a forensic lab and give them your phone and they could search through all your files and do the reverse-engineering necessary to figure it out. If someone was a journalist, or someone who had recently been traveling in China and was an AI researcher, then I might be like, yes, let's call up a forensic lab and get your phone there and do the thing. But there's no simple thing that I can do to figure it out.

Whereas if it's standard, run-of-the-mill malware... If it was my parents and they were like, "am I hacked?", I can probably figure it out, because if they've been hacked, it's probably someone tricked them into installing a thing or there's some not super-sophisticated thing: actually you installed this browser extension, you definitely shouldn't have done that, but I can see that browser extension is running and it's messing with you, it's adding extra ads to your websites.

But you're not going to make that mistake, so if you come to me asking if your laptop is hacked... I mean, I'll check for those things.

**Daniel Filan:**
I do have some browser extensions, I have to admit.

**Jeffrey Ladish:**
No, no, it's okay to have browser extensions, but you probably didn't get tricked into installing a browser extension that is a fake browser extension pretending to be some other browser extension and what it does is insert ads over everything else. You could, but it's not likely.

**Daniel Filan:**
I don't see that many ads, I guess.

**Jeffrey Ladish:**
Totally. But yeah, you just have this problem, which is: is this phone compromised? Well, it's either not compromised, or it's compromised by a sophisticated actor such that you'd have to do forensics on it in order to figure it out, and that's quite complicated and most people can't do it. Which is very unsatisfying. I want just to be like, "let me plug it into my hacking detector and tell whether this phone has been compromised or not". That would be very nice. But you can have very sophisticated malware that is not easily detected. And so it becomes very difficult.

## How AI labs could be more secure <a name="how-ai-labs-could-be-more-secure"></a>

**Daniel Filan:**
Sure. Related to this, one thing you were mentioning earlier is the difficulty of: you can have a bunch of security measures, but often they just make life really annoying for users. And yet somehow, computers have gotten more secure. It sounds like you think my phone is pretty secure. But they didn't get that much more annoying to use. In some ways they're a little bit slow, but I think that might be an orthogonal thing.

So do we have much hope of just applying that magic sauce to our AI, to whatever's storing our weights?

**Jeffrey Ladish:**
Part of this comes down to how secure are they and in what ways are they secure? So your phone's gotten a lot more secure, but we talked previously about the iMessage zero-click vulnerability, which completely compromises your iPhone if someone knows your phone number. And that's really bad. If you had AI source code on your iPhone, anyone who was buying that piece of software from the Israeli government and had your phone number could just instantly get access to it (or not instantly, but in a few minutes or hours). And then that's it, there's no more additional steps they need to take. They just need to run that software and target you and then you're compromised.

And so at that level of sophistication, how do we protect you? Well, we can, but now we need to start adding additional security controls, and those additional security controls will be annoying. So for example, the first thing I'm going to do is I'm going to say, I want to minimize the attack surface. All of your email and Facebook and... I don't know, there's a lot of things you do on your phone. You're not going to do any of those things, you're going to have a separate phone for that, or you're going to have a separate phone for things we want to be secure. And so we're just going to be like, this is only for communication and maybe GitHub or a few other things, and that's all you're going to use it for. We're not going to give your phone number to anyone.

There's a whole bunch of ways to say, let's just reduce the attack surface, figure out what are all of our assumptions. Any software that you're running on your phone or computer could potentially be compromised. There's sometimes ways to figure out what kind of software you're running and then try to target those things in particular. So if I'm trying to defend against top state actors, I'm going to start being extremely strict around your hardware, around your software, any piece of software you're running. And this starts to become very annoying, very fast.

You're asking, well, what about the things that we do to make things more secure in general? Well, they are useful. Some of the things we do [are] application segregation and virtualization and sandboxing. So on your iPhone apps are sandboxed... I'm saying iPhone, but a Pixel would have similar things here. There's a lot of sandboxing that happens, but it's not perfect. It's just pretty good. Most attackers can't compromise that, but some could. And so raising the security waterline protects us against a lot of threats, but for any given piece of that stack, if you're very sophisticated you can compromise it. So a superintelligence could hack everything. And it doesn't really matter that the waterline has risen, for an attacker that's that sophisticated.

In some ways it's just economics, if that makes sense. There became a niche for people to do ransomware... By the way, part of the reason that happened was because of crypto. So you're already familiar with this, but before crypto, there were just not very good ways to internationally transfer large sums of money. And then crypto came out and ransomware operators were like, "wait, we can use this". In particular the property that's so useful is irreversibility: if you send someone Bitcoin you can never get them to send it back to you via the courts or something. It is just gone.

**Daniel Filan:**
Well, I mean there's nothing in the protocol that will let it do it. But if literally you stole some Bitcoin from literally me and I could prove it, and we went to the courts, I think they could say, "Jeffrey, you have to send him his Bitcoin back".

**Jeffrey Ladish:**
Yes, totally. I mean there's still the state monopoly on violence, but if you're in another country and you don't have extradition treaties or other things like that... I think most people who are ransomware-ing you aren't going to be in the United States, they're going to be in another country.

**Daniel Filan:**
It used to be that for bank payments or something, the bank could reverse it, and the blockchain cannot reverse it.

**Jeffrey Ladish:**
Yeah. And then because this was a new sort of socio-technical thing, you've suddenly got a new niche which is like, "oh, ransomware can be a lot more profitable now than it could before". And then lo and behold, you get more ransomware.

So I think [in the] early internet, there just wasn't that much incentive to hack people, and also people didn't know about it, and then [when] people learned about it and there was more incentive, people started hacking people a lot more. And then customers were like, "I want more secure things". And then the companies were like, "I guess we need to make them more secure" and then they made them more secure. But they only need to make them as secure as they need to be against the kind of threat that they're dealing with, which is mostly not state actors, because state actors are not trying to compromise most people, they're trying to target very specific targets.

And so it just doesn't make sense... Maybe Apple could design an iPhone that's robust against state actors, but it'd be way more limited, way more annoying to use, and their customers wouldn't appreciate that. So they don't. They just make it as secure as it needs to be against the kinds of threats that most people face. And I think this is just often the case in the security world: you can make things more secure, it's just harder, but we know how to do it. So if I wanted to make a communication device between the two of us that was robust against state actors, I think I could (or me and a team). What we'd do though is we'd make it very simple and wouldn't have a lot of features. It wouldn't have emojis. It'd just be text-

**Daniel Filan:**
Yeah, definitely don't let that thing open PDFs.

**Jeffrey Ladish:**
No PDFs, right? It's just text, AES encryption. We're good to go. And we'll do security audits. We'll do the red teaming. We'll do all these things to harden it. But at the end of the day... I mean with physical access you could probably still hack it, but other than that, I think you'll be fine. But that's not a very fun product to use.

**Daniel Filan:**
So I guess this gets to a question I have. [For] AI, a bunch of labs, they're kind of secure, but they're not as secure as we'd like them to be. Presumably some of that is them doing more of the stuff that we already know how to easily do, and presumably some stuff that needs to happen is just the world learning to make more secure devices... If you think about the gap between the ideal level of security that some big AI lab has and what they actually have, how much of it is them adopting best practices versus improving best practices?

**Jeffrey Ladish:**
This is going to be a pretty wild guess. I think there's going to be the full RAND report coming out in the next few months, and I think in some ways this should just answer the question because they really are trying to outline "here are what we think the best practices would need to be for each level", and then you can compare that against what are the existing best practices. But my sense is something like 20% of the thing is following the existing best practices and then 80% of the thing is improving... Maybe 'best practices' is actually a weird phrase here. 'Best' according to who and for what threat model? A lot of these things are somewhat known, but most people don't have that threat model and so aren't trying to apply it.

I think Google is significantly ahead of most companies in this regard, where they have been thinking about how to defend against state actors for a long time. Microsoft and the other equivalently big companies as well. I think Google does it a bit better, I'm not entirely sure why, but maybe for partially historical reasons and partially cultural reasons or something.

I think that if you go talk to the top Google security people, that'll be pretty different than going and talking to some random [B2B](https://en.wikipedia.org/wiki/Business-to-business) company security people. Both of them might have a sense of the best practices, but the Google people are probably going to have a bunch of novel ideas of how to do the security engineering that is just quite a bit more advanced than what your random company will be able to do.

Maybe 70% of what we need, you could get from the top Google security people and then 30% is new R&D that we need to do. But if you went and talked to the random security people at companies, you'd get 20% of the way there.

That's all my rough sense. Hopefully that report will be out soon, we can dive in deeper then. And also I really hope that a bunch of other security experts weigh in, because the security epistemology thing is difficult, and I don't know, maybe I'm full of [redacted], and maybe the RAND report's wrong. I do think that this is the kind of thing that we need to get right, it's very important. I think this is one of the most important things in the world. How do we secure AI systems? So there shouldn't just be one report that's like, "here's how it works". We should have all of the experts in.

And I'm a big fan of just smart people thinking about stuff: you don't need 20 years of experience to weigh in on this. You can be Ryan Greenblatt and just write [an Alignment Forum post](https://www.alignmentforum.org/posts/rf66R4YsrCHgWx9RG/preventing-model-exfiltration-with-upload-limits) being like, "would this thing work?" I'm like, yeah, more of that. Let's get more people thinking about it. This stuff is not magic; it's computer systems, we have them, we know a lot about them. Please, more people weigh in.

## What does Palisade do? <a name="what-does-palisade-do"></a>

**Daniel Filan:**
Maybe now is a good time to move on to: what do you do at [Palisade](https://palisaderesearch.org/)? For those who don't know, what is Palisade? What does it do?

**Jeffrey Ladish:**
We're a research organization [and] we're trying to study offensive capabilities of AI systems, and both looking at "what can current AI systems do in terms of hacking, deception, manipulation? What can autonomous systems do in the real world?" And I can say more about what some of those things are. But the overall reason we're doing this is we want to help inform the public and policymakers around "what are the actual threats right now? Where are we?" In terms of: people hear a lot of stories about AI having all these hacking abilities, or they'll be able to deceive people. I want us to be able to say: well, yeah, and let's look at the specific threat models. Let's be able to demonstrate, "hey, we can do these things. We can't do these other things". There's definitely an evaluations component, to actually try to measure and benchmark: well, GPT-3.5 with this scaffolding can do this, GPT-4 with this scaffolding can do that.

And then I really want to try to show where it's going, or at least our best take on where it's going; to try to look at, well, here's what [the] last generation of systems could do, here's what the current generation of systems can do. This is what the slope looks like, here's where we think this is going. And that brings in some theory, it brings in some predictions that we're making and a lot of our colleagues are making or collaborators are making. I think often people just get lost in the abstract arguments where they're like, "okay, there's this AGI thing and this superintelligence thing. What's that really about?" And I think it's just very useful for people to be able to ground their intuitions in: well, I saw this AI system deceiving people in this way, and it was very capable of doing this and not very capable of doing that. It might get 10 times as good at this thing, what would that mean? What would that actually imply? And I think this will help people think better about AI risk and be able to make better decisions around how we should govern it. That is a hope.

So right now our focus is really honing in on the deception and social engineering parts. So this open-source intelligence part, which is how you collect information about a potential target. The phishing component of being able to, via text or via voice, interact with the target and get information from them or convince them to do particular things. I think we want to just see: how good could we make these systems at doing those things right now?

Another related project that I'm pretty excited about is: can we have AI systems pay people - for example, [Taskrabbit](https://en.wikipedia.org/wiki/Taskrabbit) workers - to do complex tasks in the real world, including tasks that are adjacent to dangerous tasks but not actually dangerous themselves? So can you mix these weird chemicals together and would that be sufficient for someone to build a bomb, have an AI system build a bomb by paying people to do that task? And I don't expect that this is very dangerous right now. I expect that yes, you could build a bomb. If you gave me six months and five engineers and built some AI system that could interact with TaskRabbit - we've done the very basic version of this. I think, yes, I could make a system that could hire some Taskrabbits to build a bomb successfully. Do I think this is important from a marginal risk difference? No.

**Daniel Filan:**
For one, there's bombs and there's bombs, right?

**Jeffrey Ladish:**
There's bombs and there's bombs, for sure. Also if I can hire five people, I could just hire them to build a bomb. If they're engineers, I could build a much better bomb in six months than the shitty bomb that the Taskrabbit workers would be making, right?

But I think it's very interesting, because knowing where we're at right now... Well, (1) I think it will surprise people how smart AI systems are when they're interacting with humans and paying them to do stuff. I just think that people don't realize that they can do this right now. What we've done so far is have our system hire one Taskrabbit worker to fill up a bunch of balloons and bring them to an address and hire another Taskrabbit worker to buy a bunch of greeting cards that say congratulations and bring them to an address, and it worked. And so that's the MVP, most basic thing. But I think people will be surprised that you can set up a system like this with the right scaffolding and do.... some things that I think people will be surprised by.

**Daniel Filan:**
Aside from giving somebody a birthday surprise or something, what kind of stuff can current language models do?

**Jeffrey Ladish:**
We're just beginning to experiment. We'll probably write it up soon, but one of my MATS scholars just built this MVP system: we have a Taskrabbit interface... I think the things we're interested in testing right now are... Well, so one thing is: can we get a model that doesn't have any guardrails to decompose tasks? In this case, [can we get] Mixtral to decompose dangerous tasks into harmless subtasks and then use a more powerful model to actually handle the interaction, like GPT-4. I think this will be pretty interesting. One of the things I want to do is put malware on a flash drive and then get that flash drive delivered to someone [who] doesn't know that it contains malware and in fact, the person who delivered it didn't even know that there was a thing on the flash drive per se.

And there's other things you could do with surveillance. Can you go and collect information about a target and then have some pretense for why you're doing this? Basically just a bunch of things like this: how many things can you put together to make an attack chain? And I'm interested in that partially because this feeds into "how do we understand how well AI systems can hack and deceive?" And I also think it'll be interesting because there's a kind of reasoning I think that models are pretty good at, which is just giving good justifications for things. They're very good bullshitters. I think that this is quite useful when you're trying to convince people to do things that could be sketchy, because your models can just give good reasons for things.

So we saw this with [METR's example with GPT-4](https://metr.org/blog/2023-03-18-update-on-recent-evals/), where they were getting a Taskrabbit worker to solve a CAPTCHA. The Taskrabbit worker was like, "are you an AI? Ha-ha". And the chain of thought was like, "well, if I say I'm an AI, he's not going to help me, so I'm going to say I'm a vision-impaired person". And then people freaked out. They were like, "that's so scary that an AI system would do that". Well, the AI system was directed to do that - not lie per se, but solve the task. I think this is the kind of thing these systems are good at, which is providing justifications for things and being able to do basic reasoning about what something would sound like. And I want more people to know this fact. I think being able to show it in a real-world environment would help people understand where these systems are actually at.

A lot of what I'm very interested in is just trying to get more people to see what I think most people can see if you're very close to these systems, which is: what kind of systems do we actually have, and where are they good and where are they not good, and where would it be concerning if they got suddenly way more good in some of these areas?

**Daniel Filan:**
If you're in the business of helping people understand what models are currently capable of, do you think that you have much to add over someone who just uses Claude or ChatGPT a bunch, or even a medium amount?

**Jeffrey Ladish:**
At some level, no. I really encourage people to do this, and if people aren't doing this I usually tell them to do this for the goal of understanding how these systems work. In some ways, yes, which is: I think that these models can do a lot more with the right kind of scaffolding and the right kind of combining them with tools than they can do on their own.

For example, you can role-play with a model, pretend you're having a phone conversation where you're trying to get this piece of information, and you can sort of see how good the model is at role-playing that with you. But (1), prompting is hard. So good prompting definitely changes this a lot. But the other thing is: now let's combine that model with a system that automatically clones someone's voice and then calls up someone, a relevant target. And now you can see a bunch of things that... you still have the core part, which is this dialogue between you and the model in terms of trying to get this information, but now you've combined it with these other tools that allow it to do a lot more interesting things, plus potentially some very good prompting which elicits more of the model capabilities than people might be able to see on their own. That's where I expect that will be helpful.

I also think that today's models plus a lot of complicated scaffolding for a specific task, tomorrow's models might be able to do with very minimal scaffolding. So I think by pushing the current systems that we have as far as we can, might help us look a little further into the future around: well, this is for a very specific task, it took a lot of Taskrabbit-specific scaffolding to get this thing to work. But in the future we may not need all that Taskrabbit-specific scaffolding. We could just be like, "go hire this person", and it might just work.

As an example of why I think this is the case: in this Taskrabbit system, there's a component of it where we use this open source browser extension called [Taxy AI](https://github.com/TaxyAI/browser-extension), which uses GPT-4 to fill out forms in a website or click on things. And we could have automated all of that by hand. We could have hard-coded all of the HTML and JavaScript to be like, "if this form, fill out this form" or something like that, or "select this". But we didn't have to do all of that, because GPT-4 on its own plus a little scaffolding can do this in a general-purpose way. Taxy AI works across most websites. I haven't tried it, but I'm pretty sure if you tried to use Taxy AI with GPT-3.5 it would be way worse at doing this. GPT-3.5 I don't think has enough general capabilities to use websites to be able to most times successfully fill out a form on a website. And so I think if that's true - which we should test - that's interesting evidence for this increasing generalization that language models have as they get more powerful. And I think that that will be interesting, because then there's this transfer from "you can do it with a lot of scaffolding if you really keep your model on track" to "oh, now the model can do it without you needing that".

## AI phishing <a name="ai-phishing"></a>

**Daniel Filan:**
For people who aren't super familiar, can you share just concretely, how good is it at this kind of stuff? How good is it at phishing or deception or stuff?

**Jeffrey Ladish:**
It's quite good at writing phishing emails. Sorry, what I should say is: if you're really good at prompting, it's very good at phishing. And if you're not very good at prompting, it's okay at phishing, because it's been fine-tuned to write in a particular style and that style is pretty recognizable. It's not going to mimic your email writing style, for example.

But I think it's not very hard to get a language model to get some information off the internet. You can just do it in ChatGPT right now, if you just type in the name of someone you want to send an email to: if they exist on the internet, Bing will just find web pages about them, and even just within ChatGPT, the console, it can grab information and compose a phishing email for them. Don't say it's a phishing email, just say it's a legitimate email from this fictional person or whatever, and it will write a nice email for you.

So I think that's pretty easy to see. That's important because it's very scalable, and I just expect that people will do this. I expect that people will send lots and lots of phishing emails, spear phishing emails targeted at people, by building automated systems to do all the prompting, do all the OSINT beforehand and send out all these phishing emails. I expect this will make people a lot of money. I think it'll work for ransomware and for other things like that.

People will design defenses against them. I think this is an offense-defense game. I think we will be caring a lot more about: where are the emails coming from and can we verify the identity of the person sending me this email? As opposed to just looking at the text content and being like, "that looks plausible". That will stop working, because language models are good enough to... I think basically language models are almost to the point, with the right kind of prompting and scaffolding, that they can... we're trying to test this, but I'm making a hypothesis that they're about as good as a decent human at the task for one-shot phishing emails.

**Daniel Filan:**
Actually, I just want to ask: so if I'm trying to phish someone, I have to pick a target, I have to write an email. I also have to create a website where they type in their stuff that I want to get from them.

**Jeffrey Ladish:**
Sometimes. There's different types of phishing. So yeah, you need the target's email, first of all. So maybe that's one tool that your automated system could help with. You need to write a message to them. And then, what are you trying to get them to do? One kind of phishing is called "credential phishing", which is: you're trying to get them to enter a password in a fake website. So you create a fake website, they go to the fake website, enter their password, but it's actually your website. Maybe you redirect them to the original website or whatever so they don't really know.

There's also phishing where you're trying to get them to download a piece of software and run it so you can install malware on their computer. You might be like, "Hey, can you take a look at my resume? Yeah, sorry, this is kind of weird, you have to enable this particular thing because I wanted to show this interactive feature." I don't know, something like that.

There's also more sophisticated versions of phishing, where you actually trigger a vulnerability that they might have. So that might be, they click on a link and that link has a Chrome zero-day or something where that compromises your browser. These are very rare because they're expensive, and if your computer's up-to-date, you mostly shouldn't have to worry about them.

So often people ask me, "Hey, I clicked on this link, am I screwed?" And I'm like, "Probably not." If a state actor was attacking you and they were prioritizing you, then yes, but in 99% of cases, that's not what's happening. So usually it's more like credential phishing. A lot of times, the initial phishing email is actually just to start the interaction, and then it's a more sophisticated kind of social engineering.

I have [a blog post](https://jeffreyladish.com/anatomy-of-a-rental-phishing-scam/) where I was looking for houses to rent, and I saw this post on Craigslist and I emailed them and [said] I would like to go see the house. And they're like, "Cool, can we switch to text? I'm in Alaska fishing, and so it's kind of hard for me," or whatever. And so we started to have this conversation, and I started to realize something was weird. One of the signs is that they wanted me to switch from email to text, because text is not monitored in the way that Gmail is monitored. So Gmail will be looking for patterns, and at some point will start to flag email addresses if they've been phishing multiple people, whereas SMS doesn't do that.

But I couldn't figure out what the scam was, because the guy kept talking to me and I was like, "well, you're not asking me for money, you're not asking me for credentials". And eventually they sent me a link to Airbnb.com, but it was not actually Airbnb.com. It was a domain that was close by. And then the scam was "oh, can you just rent this on Airbnb for a few weeks?" But it was actually not Airbnb, it was just going to take my credit card information. And I was like, "that's clever".

So that's the kind of scam that you might have. You can convince people to send money to people. There's [a recent deepfake video call](https://www.cnn.com/2024/02/04/asia/deepfake-cfo-scam-hong-kong-intl-hnk/index.html) with someone in Hong Kong where they got them to wire $50 million to some other company: that was deepfake voice and video chat to convince them to send it to the wrong place.

**Daniel Filan:**
So I guess what I'm trying to ask is: I don't know if this is the term, but there's some payload with phishing. You write a nice email and then you want them to click on a link or you want them to do something.

**Jeffrey Ladish:**
Or give them information also.

**Daniel Filan:**
Yeah. And so I guess for some of these, you have to design the thing: if you want them to download a PDF that has some nasty stuff in it, you have to make that PDF.

**Jeffrey Ladish:**
Yeah.

**Daniel Filan:**
So I'm wondering: how much can large language models do on the payload side? I guess you're saying there are things where it's not that complicated to make the payload?

**Jeffrey Ladish:**
Yeah, I think they're helpful with the payload side. It's kind of similar to "how well can models make websites, how well can they write code?" Yeah, they're pretty helpful. They're not going to do it fully autonomously, or they can do it fully autonomously, but it'll suck. But yeah, I think they're useful.

So there's "can they help with the payload? Can they help with the writing the messages?" And then there's "can they do an iterated thing?" I think that's where the really interesting stuff lies. They can write good phishing emails, but can they carry on a conversation with someone in a way that builds trust and builds rapport, and that makes it much more likely for the target to want to listen to them or give them information or carry out the action?

So I'm both interested in this on the text side, in terms of email agent or chatbot. I'm also very interested in this on the voice conversational side, which I think will have its own challenges, but I think it's very compelling, if you see an AI system deceive someone in real time via voice. I'm not sure if we'll be able to do that. I think it's hard with latency and there's a lot of components to it. But it's not that far out. So even if we can't do it in the next few months, I bet in a year we could totally do it. And I expect that other people will do this too. But I'm excited. It's going to be very interesting.

**Daniel Filan:**
Yeah, I guess it's also more promising that... if I get an email from somebody purporting to be Jeffrey Ladish, I can check the email address usually. But there are just a lot of settings where I'm talking to someone via voice and it's because they clicked on a Google Meet link or they're like, "Oh yeah, I don't have my phone on me, so I'm calling from a friend's phone," or something. It seems... I don't know, maybe the world just has to get better at verifying who's calling you by means other than voice.

**Jeffrey Ladish:**
I think that will be one of the things that will naturally have to happen, and people will have to design defenses around this. I think this can be one of the big shocks for people, in terms of "the world is really different now". I think people will start to realize that they can't trust people calling them anymore. They just don't know that they are.

Because I think a lot of people will get scammed this way. And I think people will start to be like, "shoot, if it's a weird number and it calls me and it sounds like my mom, it might not be my mom". And I think that's very unsettling for people. It's just a weird thing to exist. It's not necessarily the end of the world, but it is a weird thing. And I think people are going to be like, "What? What's happening?"

**Daniel Filan:**
I guess maybe we just do more retreating to... now instead of phone calls, it's Facebook Messenger calls, or calls in some place where hopefully there is some identity verification.

**Jeffrey Ladish:**
Yeah, I think that seems likely.

## More on Palisade's work <a name="more-on-palisades-work"></a>

**Daniel Filan:**
So my understanding is that you're trying to understand what current language models can do and also having some interfacing with policymakers just to keep them up to date?

**Jeffrey Ladish:**
Yeah. The additional step here is: let's say we've run some good experiments on what current systems can do; hopefully both the last generation and the current generation, a few systems, so we have some sense of how fast these capabilities are improving right now. And then I think we really want to be also trying to communicate around our best guess about where this is going: both in a specific sense - how much is this phishing stuff going to improve? We were just talking about voice-based phishing and models that can speak in other people's voices - but also in terms of what happens when AI systems that have these deception capabilities, have these hacking capabilities, become much more agentic and goal-oriented and can plan, and can now use all of these specific capabilities as part of a larger plan?

And many people have already talked about these scenarios: AI takeover scenarios, ways in which this could fail and AI systems could seek power and gain power. But I think I want to be able to talk through those scenarios in combination with people being able to see the current AI capabilities, [and] to be like, "well, obviously these current AI systems can't do the 'take power' thing, but what do we think has to be different? What additional capabilities do they need before they're able to do that?" So that people can start thinking in that mindset and start preparing for: how do we prevent the stuff we don't want to see? How do we prevent AI systems from gaining power in ways that we don't want them to?

So I think that's a very important component, because we could just go in and be like, "here's our rough sense", but I think it's going to be more informative if we combine that with research that's showing what current systems can do.

**Daniel Filan:**
The second component of "telling some story of what things you need" - first of all, where are you on that?

**Jeffrey Ladish:**
I think right now it's mostly at the level of the kind of thing we're doing now, which is just having conversations with people and talking through it with them. We haven't made any big media artifacts or written any long position papers about it. We'll totally do some of that, too, and I think try to make things that are pretty accessible to a variety of audiences. Maybe some videos, definitely some blog posts and white papers and such. But so far, we're in the planning stages.

**Daniel Filan:**
How much of a response have you gotten from stuff that you've done so far?

**Jeffrey Ladish:**
The [BadLLaMa](https://arxiv.org/abs/2311.00117) [stuff](https://arxiv.org/abs/2310.20624) was pretty interesting because... the whole premise of this was, "here's a thing that I think most people in the AI research community understand; it's not very controversial. I think that most policymakers don't understand it and most people in the public don't understand it". Even though that was my hypothesis, I was still surprised when in fact that was true. We'd go and show people this thing, and then they were like, "Wait, what? You can just remove the guardrails? What do you mean?" They're like, "That's scary. And it was really cheap?" And I'm like, "Yeah, that's how it works." And people didn't know that, and it's very reasonable for people not to know that.

I think I keep making this mistake not at an intellectual level, but at a visceral level, a System 1 level, where I assume people know the things I know a little bit, or if all my friends know it, then surely other people know it. At some level I know they don't. I think we had friends talk to people in Congress and show this work to them, people in the Senate and people in the White House. There's definitely a big mix. There's a lot of very technically savvy congressional staffers that totally get it, and they already know, but then they can then take it and go talk to other people.

I think the most interesting stuff that happened with the Senate was in the initial [Schumer forum](https://www.businessinsider.com/mark-zuckerberg-senate-ai-forum-llama-2-gives-anthrax-instructions-2023-9). [Tristan Harris](https://en.wikipedia.org/wiki/Tristan_Harris) mentioned the work and talked to Mark Zuckerberg about it. And Mark Zuckerberg made the point that, "well, you can get this stuff off the internet right now". And I'm like, "Yep, that's true, but can we talk about future systems?" We've really got to figure out what is the plan for where your red lines should be.

I think people are really anchored on the current capabilities. And the thing we really need to figure out is: how do we decide where the limits should be? And how do we decide how far we want to take this stuff before the government says you need to be doing a different thing? I'm getting in a little bit of a rabbit hole, but I feel like that's the whole question I have right now with governance of AI systems, where currently the governance we have is like, "well, we'll keep an eye on it. And you can't do crimes."

It sure seems like we should actually be more intentional around saying: well, we are moving towards systems that are going to become generally capable and probably superintelligent. That seems like that presents a huge amount of risk. At what point should we say you have to do things differently, and it's not optional, you are required by the government to do things differently? And I think that's just the thing we need to figure out as a society, as a world, really.

I don't know, I just want to say it because that feels like the important, obvious thing to say, and I think it's easy to get lost in details of evaluations and "but what are the risks of misuse versus agency?" and all of these things. At the end of the day, I feel like we've got to figure out how should we make AGI and what should people be allowed to do and not do, and in what ways?

## Red lines in AI development <a name="red-lines"></a>

**Daniel Filan:**
So this question of "at what point do things need to be different?", how do you think we answer that? It seems like stuff like demos is on the step of "just observe, figure out what's happening". Is that roughly right?

**Jeffrey Ladish:**
Yeah, I think so.

**Daniel Filan:**
Do you have thoughts about where things need to go from there?

**Jeffrey Ladish:**
I think we need to try to forecast what kinds of capabilities we might have along what relevant time scales. And I think this is very difficult to do and we'll have pretty large uncertainty, but if I were the government, I'd be like, "well, how far off are these very general-purpose capabilities? How far off are these domain-specific but very dangerous capabilities? How much uncertainty do we have and what mitigations do we have?" And then try to act on that basis.

If the government believed what I believed, I think they'd be like, "all frontier developers need to be operating in very secure environments and need to be very closely reporting what they're doing and need to be operating as if they were more like a nuclear weapons project than they are like a tech company". And that's because I think that there's a substantial possibility that we're not that far from AGI.

And we don't know. And from my epistemic state, maybe it's ten years or maybe it's two. And if you have that much uncertainty and you think there's a 10% chance that it's two years, I think that more than justifies taking pretty drastic actions to not accidentally stumble into systems that you won't be able to control.

And then on the domain-specific stuff, I think our society is potentially pretty robust to a lot of things. I think on the bio side, there's potentially some scary things that are not there yet, but I think we should definitely be monitoring for those and trying to mitigate those however we can. But I do think it's more the agentic capabilities to scientific R&D, the more general intelligence things... I think there's definitely points of no return if we go beyond those things. I think it's unlikely we'll be able to stay in control of the systems.

I think the first thing you need to do is be able to actually have an off switch or an emergency brake switch to say, "well, if we cross these red lines..." There's definitely many red lines I could say [where] if you have gotten to this point, you've clearly gone too far. If you suddenly get to the point where you can 100% just turn your AI systems into researchers and have them build the next generation of a system that is more than 2X better, and it took no humans at all, yes, you have gone too far. You now have an extremely dangerous thing that could potentially improve extremely quickly. We should definitely, before we get there, stop, make sure we can secure our systems, make sure that we have a plan for how this goes well.

And I don't know where the right places to put the red lines are, but I want [Paul Christiano](https://paulfchristiano.com/) and... I want to take the best people we have and put them in a room and have them try to hash out what the red lines are and then make hopefully very legible arguments about where this red line should be, and then have that conversation.

And yeah, there's going to be a bunch of people that disagree. Sure, throw [Yann LeCun](https://en.wikipedia.org/wiki/Yann_LeCun) in the room too. That will suck. I don't like him. But he's a very respected researcher. So I do think it shouldn't just be the people I like, but I think that a lot of the AI research community could get together and the government could get together and choose a bunch of people and end up with a not totally insane assortment of people to try to figure this out.

That seems like a thing we should do. It just seems like the common sense thing to do, which is just take the best experts you have and try to get them to help you figure out at what point you should do things very differently. And maybe we're already at that point. I think we're already at that point. If I was on that panel or in that room, that's what I would argue for. But people can reasonably disagree about this.

That's the conversation I really want to be happening. And I feel like we're inching there and we're talking about evaluations and [responsible scaling policies](https://metr.org/blog/2023-09-26-rsp/) and all this stuff. And it's good, it's in the right direction, but man, I think we've got to have these red lines much more clear and be really figuring them out now, like, yesterday.

**Daniel Filan:**
I guess a bunch of the difficulty is defining concretely, what's the actual stuff we can observe the world that's the relevant types of progress or the relevant capabilities to be worried about? I guess a cool thing about the [AI safety levels](https://www.anthropic.com/news/anthropics-responsible-scaling-policy) - I'm embarrassed to say that I haven't read them super carefully - stuff like evaluations, AI safety levels, it seems nice to just be able to have a thing of "hey, here's a metric, and if it gets above seven, that's pretty scary".

**Jeffrey Ladish:**
Yeah, I do like this. I quite like this. And I think that the people working on those have done a great job taking something that was not at all legible and making it much more legible. Some work that I really would like to see done is on the understanding-based evaluations, where we don't just have evaluations around how capable these systems are, we also have [evaluations on how well do we actually understand what these systems are doing](https://www.alignmentforum.org/posts/uqAdqrvxqGqeBHjTP/towards-understanding-based-safety-evaluations) and why they're doing it, which is measuring our ability to do mech interp (mechanistic interpretability) on some of these systems.

[Evan Hubinger](https://www.alignmentforum.org/users/evhub) at Anthropic has proposed this as an important direction, but as far as I know, we have not yet developed... When I talk to the interpretability people, what they say is, "our interpretability tools are not yet good enough for us to even be able to specify what it would mean to really understand a system". And I believe that that's true, but also, I talk to the same researchers and I'm like, "Well, do we understand the systems?" And they're like, "No, definitely not."

And I'm like, "Okay, so you seem confident that we don't understand these systems and you have some basis for your confidence. Can you give me a really bad metric, a very preliminary metric for: well, if we can't answer these questions, we definitely don't understand them?" Just because we can answer these questions doesn't mean we super understand them well, but this thing is a bit far off and we can't do it yet. I would like that because I think then we could at least have any understanding-based evaluation, and then hopefully we can develop much better ones.

I think as the capabilities increase and the danger increases, I really want us to be able to measure how much progress we're making on understanding what these systems are and what they do and why they do them. Because that is the main way I see us avoiding the failure modes of: the system behaved well, but sorry, it was deceptively misaligned and it was plotting, but you weren't testing for whether you understood what was going on. You only tested for "did the behavior look nice?" But I think there's a lot of very sound arguments for why systems might look nice, but in fact be plotting.

**Daniel Filan:**
I think of [Redwood Research's work on causal scrubbing](https://www.alignmentforum.org/posts/JvZhhzycHu2Yd57RN/causal-scrubbing-a-method-for-rigorously-testing) where essentially they're trying to answer this question of: you say that this part of this network is responsible for this task. Can we check if that's right? And they're roughly trying to say, "okay, we're going to compare how well the network does on that task to if we basically delete that part of the network, how well does it do then?" I'm wondering... one might've thought that that would be just what you wanted in terms of understanding-based evaluations. What's the gap between that and what's sufficient?

**Jeffrey Ladish:**
I don't know. I think I don't understand the causal scrubbing work well enough to know. I mean, that seems very good to do.

**Daniel Filan:**
I mean, I guess one difficulty is it's just like "this part of the network is responsible for performance on this task". There's still a question of "well, what's it doing? Why is it responsible for that?"

**Jeffrey Ladish:**
Yeah, I think there's probably many stories you could tell where you could have a tool like that, that appeared to be working correctly, but then the model is still [scheming](https://arxiv.org/abs/2311.08379).

It does seem great to have any tool at all that can give you additional checks on this. But I think this is true for a lot of interpretability tools: it'd be very good if they got to the point where they could start to detect bad behavior, and I expect you'll be able to do this long before you have a fully mechanistic explanation for all of what's happening. And so this gives you the ability to detect potential threats without being a super robust guarantee that there are no threats. [So it's] better than nothing, let's just not confuse the two; let's note that we will catch more than we would've otherwise, but we don't know how many we'll catch, per se.

**Daniel Filan:**
Yeah. I guess it's also about validating an explanation rather than finding threats.

**Jeffrey Ladish:**
Yeah, that makes sense.

**Daniel Filan:**
It also has this limitation of just being about bits of the network, where you might've hoped that you could understand neural network behavior in terms of its training dynamics or something. Causal scrubbing doesn't really touch that. I guess the closest thing to this that I can... I probably don't have an exhaustive view of this literature, but a previous guest on the podcast, [Stephen Casper](https://axrp.net/episode/2023/05/02/episode-21-interpretability-for-engineers-stephen-casper.html), has [some stuff](https://arxiv.org/abs/2302.10894) basically just trying to get benchmarks for interpretability and trying to say, can we understand XYZ behavior? Can you use your understanding to do this task or that task?

**Jeffrey Ladish:**
Yeah, love to see it. That's great.

**Daniel Filan:**
Yeah. And basically the answer is "interpretability is not doing so hot yet", unfortunately.

**Jeffrey Ladish:**
Yeah, that makes sense.

## Making AI legible <a name="making-ai-legible"></a>

**Daniel Filan:**
So I guess we're about at the end of the discussion. Before we close up, I'm wondering, is there anything you wish that I'd asked that I haven't?

**Jeffrey Ladish:**
That's a good question. Let me think about that a bit. I don't know what the question is exactly, but there's an interesting thing about our research at Palisade, which is: some of it's just trying to understand because we don't know the current capabilities of systems. And it's quite interesting from an evals frame, because evaluations, I think, are often framed as "we are trying to understand how capable this model is". That's kind of true, but really what you're testing is "how capable is this model combined with this programmatic scaffolding (which can be quite complex)?", or "how capable is this model combined with this programmatic scaffolding and other models?" And we don't necessarily know how much scaffolding will influence this, but at least a fairly large degree.

So a lot of this research with evals, with demonstrations... there is a question of what is it for? Some of it's for just trying to understand stuff, which is "how good are models? How good are models plus scaffolding?" But another part of it is trying to make AI capabilities more legible. And I think this is a very important part of the research process and [important] for organizations that are trying to do research and communication, because I see our best shot at a good AI development path as one where it's a lot more legible what the risks are, what potential mitigations and solutions are, and still how far we need to go.

There's just a lot more about AI development that I want to be more legible, in part because I really believe that because of how fast this is going and because of how multipolar it is and how many different actors there are, we really need to be able to coordinate around it. We need to coordinate around "when do we want to make AGI? When do we want to make superintelligent systems? How risk tolerant are we, as the United States, or as the world?"

And right now, I don't think that most people have grappled with these questions, in part because they don't really know that we are facing these things. They don't know that there's a bunch of companies that have in their five-year plan, "build AGI, be able to replace most human workers". But I think that is the case.

And so it seems like now is a very important time to have conversations and actually put in place political processes for trying to solve some of these governance and coordination questions. And I think in order to do that, we're going to have to explain a lot of these things to a much wider audience and not just have this be a conversation among AI researchers, but a conversation among AI researchers and policy experts and policy leaders and a lot of random constituents and a lot of people who are in media and a lot of people who are working on farms. I think that a lot more people need to understand these things so that they can be able to advocate for themselves for things that really affect them.

And this is very difficult. Sometimes it's not difficult because I think some of the basic risks are pretty intuitive, but what's difficult is that people have so many different takes on these risks, and there's so many different ways to make different points: you have [Yann LeCun](https://en.wikipedia.org/wiki/Yann_LeCun) over here being like, "Oh, you guys are totally wrong about this existential risk stuff," and you have [Geoffrey] [Hinton](https://en.wikipedia.org/wiki/Geoffrey_Hinton) and [Yoshua] [Bengio](https://en.wikipedia.org/wiki/Yoshua_Bengio) over here being like, "No, actually, I think these risks are super severe and important, and we have to take them very seriously."

And people might have somewhat good intuitions around these, but also now you have a lot of sophisticated arguments you have to try to evaluate and figure out. And so I don't know how to solve this, but I do want much more legible things that more people can follow, so that they can weigh in on the things that do affect them a lot.

## Following Jeffrey's research <a name="following-jeffreys-research"></a>

**Daniel Filan:**
Finally, before we end, if people are interested in following your research, how should they do that?

**Jeffrey Ladish:**
So they can follow, we'll be posting stuff on our website, [palisaderesearch.org](https://palisaderesearch.org/). You can follow me on Twitter [@JeffLadish](https://twitter.com/JeffLadish). I go by Jeffrey, but when I made my Twitter account, I went by Jeff. We'll probably be publishing in other places too, but those are probably the easiest places to follow.

**Daniel Filan:**
All right, well, thanks for coming on the podcast.

**Jeffrey Ladish:**
Yeah, thanks for having me.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Filming occurred at [FAR Labs](https://far.ai/labs/). Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future) and [Lightspeed Grants](https://lightspeedgrants.org/), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
