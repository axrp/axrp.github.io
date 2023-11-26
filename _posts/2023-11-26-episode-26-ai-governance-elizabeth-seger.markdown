---
layout: post
title: "26 - AI Governance with Elizabeth Seger"
date: 2023-11-26 15:30 -0700
categories: episode
---

<!-- [YouTube link](https://youtu.be/0JkaOAzDfgE) -->

[YouTube link](https://youtu.be/bYOB3CXAAaE)

The events of this year have highlighted important questions about the governance of artificial intelligence. For instance, what does it mean to democratize AI? And how should we balance benefits and dangers of open-sourcing powerful AI systems such as large language models? In this episode, I speak with Elizabeth Seger about her research on these questions.

Topics we discuss:
 - [What kinds of AI?](#what-ai)
 - [Democratizing AI](#democratizing-ai)
   - [How people talk about democratizing AI](#how-people-talk)
   - [Is democratizing AI important?](#is-it-important)
   - [Links between types of democratization](#links-between-types)
   - [Democratizing profits from AI](#democratizing-profits)
   - [Democratizing AI governance](#democratizing-gov)
   - [Normative underpinnings of democratization](#normative-underpinnings)
 - [Open-sourcing AI](#osai)
   - [Risks from open-sourcing](#risks-from-os)
   - [Should we make AI too dangerous to open source?](#make-ai-too-dangerous-os)
   - [Offense-defense balance](#offense-defense)
   - [KataGo as a case study](#katago-as-case-study)
   - [Openness for interpretability research](#open-for-interp)
   - [Effectiveness of substitutes for open sourcing](#os-substitutes)
   - [Offense-defense balance, part 2](#offense-defense-2)
   - [Making open-sourcing safer?](#making-os-safer)
 - [AI governance research](#ai-gov)
   - [The state of the field](#state-of-field)
   - [Open questions](#open-qs)
   - [Distinctive governance issues of x-risk](#xrisk-different)
   - [Technical research to help governance](#tech-for-gov)
 - [Following Elizabeth's research](#following-elizabeths-research)

**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Elizabeth Seger. Elizabeth completed her PhD in philosophy of science at Cambridge in 2022, and is now a researcher at the [Centre for Governance of AI](https://www.governance.ai/) in Oxford, where she works on AI democratization and open-source AI regulation. She recently led the production of [a large report](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4596436) on the risks and benefits of model-sharing, which we will talk about in this episode. For links to what we're discussing, you can check the description of the episode and you can read the transcript at axrp.net.

Well, Elizabeth, welcome to the podcast.

**Elizabeth Seger:**
Awesome. Thanks for having me.

## What kinds of AI? <a name="what-ai"></a>

**Daniel Filan:**
Cool. We're going to be talking about a couple of papers basically about democratizing and open-sourcing AI. When we're talking about this, what sorts of AI should we be thinking about? What sorts of AI do you have primarily in mind?

**Elizabeth Seger:**
When talking about open-sourcing and model-sharing, I've been primarily interested in talking about frontier foundation models, so understanding frontier foundation models as those systems on the cutting edge of development right now. When talking more generally about AI democratization, I tend to think much more broadly. I think a lot of conversation around democratization right now is about frontier AI systems, but it's important to recognize that AI is a very broad category. Well, so is democratization, which is I'm sure something we'll talk about.

## Democratizing AI <a name="democratizing-ai"></a>

**Daniel Filan:**
Okay, cool. In that case, let's actually just dive into the democratization paper. This is [Democratizing AI: Multiple Meanings, Goals, and Methods](https://arxiv.org/abs/2303.12642) by yourself, Aviv Ovadya, Ben Garfinkel, Divya Siddarth, and Allan Dafoe. Before I start asking questions, can you give us just a rundown of basically what is this paper?

**Elizabeth Seger:**
Yeah, so this paper on AI democratization was born out of an observation around how different actors were using the term "AI democratization" or "democratizing AI", whether we're talking about labs developing AI systems, or policymakers, or even people in the AI governance space. There just seemed to be a lot of inconsistency around what this term AI democratization meant.

The paper really started out as just an effort to explain what does it actually mean when someone says "Democratizing AI." I think there's overlap here with the discussions and debates around open-sourcing. I think there was a lot of... at least for me personally, there was frustration when people would say, "Oh, well, we open-sourced the model and therefore, it's democratized." It's like, "What does that even mean?"

The goal of this AI democratization paper was really just to outline the different meanings of AI democratization that were in use. Not necessarily saying how it should be used, but just how is the term being used. We broke it up into four categories. Democratization of AI use is just allowing many more people to interact with, and use, and benefit from the technology. Then there's the democratization of development, which is allowing many more people to contribute to the development processes and help make the systems really cater to many diverse interests and needs.

Democratization of profits, which is about basically distributing the profits that might accrue to labs that are the primary developers and controllers of the technology, and those profits could be massive. Then the democratization of AI governance, which is just involving more people in decision-making processes about AI, about how it's distributed, about how it's developed, about how it's regulated, and about who makes the decisions. These are all different ways that democratization has been discussed.

Then in the paper, we go a step further and say, "Okay. Well, if these are the different kinds of democratization, what are the stated goals of those kinds of democratization, and then what activities help support those goals?" I think the main idea here was to show that, oftentimes, when people talk about AI democratization, they're specifically talking about open-sourcing or model-sharing. I think one of the main points we try and make is to say something like, for example, open-sourcing AI systems is one form of model-sharing. Model-sharing is one aspect of democratizing the development of AI systems, and democratizing the development of AI systems is one form of AI democratization.

If you're going to commit to an AI democratization effort or say that these are the goals you have in mind, there's so much more involved in these processes. It's not just about releasing a model, but many more proactive efforts that can go into distributing access to the models, ability to use the models, and to benefit from the models, whether that's through use or through the profits that it produces.

### How people talk about democratizing AI <a name="how-people-talk"></a>

**Daniel Filan:**
Gotcha. In the paper, you cite a few examples, basically, of people either talking about something akin to democratization or specifically using the term "democratizing AI". In all the examples you use, it's basically labs talking about it, and it's mostly in a context of, "We want to democratize AI in the sense of everybody should use our product", is how I read it. I'm wondering, do people other than labs talk about democratizing AI? Is this a thing broadly?

**Elizabeth Seger:**
Yeah, absolutely. In the paper, I think we focused a lot of the lab terminology because to be honest, it was written initially as sort of a pushback against the use of the term by labs to basically say, "Use our product," and it's like, "Okay. What does this actually mean? There's more to it, guys." But the term AI democratization or even just discussing AI democratization is much more widely engaged with.

One of the co-authors, [Divya Siddarth](https://divyasiddarth.com/), for example, she and [Saffron Huang](https://saffronhuang.com/) run the [Collective Intelligence Project](https://cip.org/), and this is a group that focuses largely on democratizing AI governance processes. So getting lots of people involved in decision-making about, for example, AI alignment or just about different kinds of governance decisions, and how that can align with the needs and interests of really diverse populations.

I think there are definitely groups that are popping up and getting really involved in AI democratization from the democratizing governance standpoint. It is a term that's used by labs, and not necessarily consistently to just mean, "Use our products." Some of them mean ... when Stability AI discussed AI democratization and the whole, "AI for the people by the people" mantra, that was very much about model-sharing and making models accessible to lots of people to use as they saw fit. So yeah, you see this in labs.

You see this with a lot of groups that are popping up to help democratize governance processes. [You] probably hear about it less in policy and governance circles, though sometimes, I've talked to union leaders and when they think about AI democratization, they've spoken about it like, "How are these technologies going to help our workforce?", both in the sense of making sure that it helps the workforce with their needs and helps make their jobs easier, but not necessarily in a way that would threaten jobs. And how do we make the integration of the systems into new environments and settings [that] actually work for the people that are using them and integrating different perspectives in that way?

It's definitely something that I think is ... It's a terminology that's been spreading and part of why we wrote this paper is trying to understand all the different ways the terminology's being used, though it was largely written in response to the way it was being thrown around by labs and being seen in the media.

**Daniel Filan:**
Right. That actually gets to something I was thinking. In this paper, you list these four definitions. That inspired me to think like, "Okay. Can I think of any other things that I might mean by 'democratizing AI'?" One thing that it sounded like these union leaders were talking about is something like how customizable AI is, or to what extent can any person wield it specifically to do their own thing rather than just getting a one-size-fits-all deal? I'm wondering if you have any thoughts about that at least potential use of the term "democratization".

**Elizabeth Seger:**
Yeah, so I think that this is something that we've sort of filed under the "democratizing use" category. One of the things that we tried to do under each of these categories is to show how there wasn't necessarily just one way to democratize use or one way to democratize development. When we talk about democratizing use, one way to do that is just to make a product available. Like say, "Here, use GPT-4. There you go." It's available, easy access.

But other ways to democratize use are, for example, to make the systems more easily accessible by maybe having an interface be more intuitive. Like the models, the API interface, have that be more intuitive. Or to provide services to help it be more easily customizable, as you mentioned, to allow people to... whether they're fine-tuning it to focus on a particular need or maybe there are ways through plug-ins, for example, to integrate it with different services, into different applications. Or then even to provide support services to help people integrate your products into downstream applications or into different workstreams. These are all different things that can contribute towards democratizing the use of a system, and I think customization's definitely one of those.

### Is democratizing AI important? <a name="is-it-important"></a>

**Daniel Filan:**
Gotcha. Now that we have a better sense of just what we're talking about in terms of democratization, a question that you address a little bit in the paper, but isn't precisely at the forefront, is just: how important is democratization in this context? Because for most technologies, I don't think a ton about their democratization - air dehumidifiers, or water bottles, or something. Maybe I'm heartless, but for me, democratization is just not near the top of the normative priorities for these things. Should we even care that much about democratizing AI?

**Elizabeth Seger:**
The short answer is yes. I think we should care about democratizing AI. You make a good point. We don't talk about democratizing air humidifiers or water bottles, but I think - this is a question that comes up when talking about AI a lot, is just, how is it different from other technologies? This is a question that's been asked, whether you're talking about how it's shared or in this context, how it's democratized.

Specifically, with regard to the discussion on democratization, making something available for more people to use, available for more people to benefit from or contribute to the design processes, I think this is particularly important in the case of AI. (A), because if AI promises to be as transformative a technology as we're hoping it to be, this will massively impact lives across the globe from different groups, different nationalities, different geographic settings. Making sure that we can have input from different groups into design processes and to understanding what needs are best served, that's going to be important to make sure that this technology doesn't just serve a few very well, but serves many people and benefits the world as a whole.

I think this is also particularly important to note with regard to the democratization of profits. AI is already, but may continue to be just an insanely financially lucrative industry. We could begin to see the accrual of profits to leading labs on huge scales where we might see a particular company, for example, be able to measure its gross income in terms of total percentages of total world output or something. That is huge economic profit, and we want a way to make sure that, ideally, everyone benefits from this, it doesn't just pile up in a few companies in a few geographic locations like Silicon Valley.

How do we make sure that this is well-distributed? There are some direct methods that could be employed. In the section of the paper where we talk about democratizing profits, some of the things that we discuss are you'd have adherence to [windfall clauses](https://www.fhi.ox.ac.uk/windfallclause/). This would be something where, let's say a company has some kind of windfall profit measured in terms of percentage of gross world output. In that case, there might be a commitment to redistribute some of those funds, or we might see taxation and redistribution schemes.

There are also more indirect ways as well that you can think about distributing profits. This would be, for example, are there ways that we can get more people involved in democratization of development? More people involved in developing products, so there's overlap between democratizing profits and democratizing development. More people can contribute to development of products, then this might challenge a natural monopoly that might otherwise form around large labs. More competition, more distribution of profits, more people in on the game.

So yeah, I think what it comes down to is if this technology can be as big and as transformative as it promises to be, having more people involved in the development process and being able to benefit from the value that's produced by these systems is really important.

### Links between types of democratization <a name="links-between-types"></a>

**Daniel Filan:**
Yeah, I think that makes sense. Actually, that reminds me of ... As you mentioned, there's some link between democratizing, I guess, the development of AI and the profits, in the sense that if more people make AI products, that means more people can profit from them, presumably. There's also, to my mind, there's a continuum between the use of AI and the development of AI, right? In some sense, when you use a thing for a task, you're developing a way of using it for that task, right? There's sort of a fuzzy line there. There's also some sort of link between developing AI and governing AI, just in the sense that if I develop some AI product, I am now, de facto, the main person governing how it is made and how it is-

**Elizabeth Seger:**
Yeah, which is sort of what we're seeing right now. A lot of the AI governance discussion is around the fact that major AI labs are making huge, impactful decisions about the future of AI. They're making AI governance decisions, and so there's a big open question of who should actually be making these [decisions]. Should it just be a few unelected tech leaders of major companies, or should this be distributed more? So yeah, there's definitely overlap between categories.

**Daniel Filan:**
Yeah. I take the point of your paper to be saying like, "Oh, these are very distinct. These are conceptually pretty distinct and you can promote one sort of democratization without promoting other sorts of democratization." But to the degree that they do seem to have these links and overlaps, is it maybe a case of just a rising tide lifts all boats and buy one kind of democratization, get the others free?

**Elizabeth Seger:**
I think kind of, maybe, sort of, not really. All of the above. No, so there definitely is overlap between some of the categories. I think it's either in this paper or maybe in the blog post version, I think we say like, "These are not perfectly distinct categories. There's going to be overlap between them." I think part of the utility of breaking it up into the categories is to point out that there are lots of different meanings. There are lots of different ways to achieve these different meanings and goals.

It's not just about just open-sourcing a model or just making it available for people to use. There's a lot more to it. I think, in some respects, there is sort of a "rising tide lifts all boats" kind of thing. Like you said, you might get more people involved in development processes. If more people are developing AI systems, more people profit from it, and that can help distribute profits.

On the other hand, you can also see direct conflicts between categories sometimes. For example, you might... I think this is something we write about in the paper: talking about democratizing governance, so decisions about AI, whether that's about how AI is developed or even how decisions are made about how models are shared.

These days, there's a really heated debate around open-sourcing frontier AI models. Let's say you democratize this decision-making process around whether or not certain models should be released open-source. Let's say the outcome of this democratized decision-making process is to say, "Okay. Maybe some models that pose substantially high risks should not be released open-source." This is a hypothetical, but let's say that's the outcome of a deliberative, democratic process and decision-making. That could be in direct conflict with the interests of democratizing development, where democratizing development... that is always furthered by providing better access to models.

You could have a case in which you've democratized a governance decision, but the outcome of that governance decision is to impede some other form of democratization. I mean, not just with development. I guess, you could have a democratic process that says, I don't know, like, "Large companies should not be taxed and have profits redistributed." That would be in direct conflict with an effort to democratize profits, so there could be conflicts. I think it's usually between the governance category and the other categories.

I guess, governance often will conflict with specific goals. That's kind of the purpose, to temper decision-making and get more people involved in the decision-making processes, and make sure that those decisions are democratically legitimate, and reflect the needs and interests of a wider population of impacted people and stakeholders.

**Daniel Filan:**
Okay, yeah. This actually brings up a question I had about the interaction between democratizing use/development and democratizing governance. In the paper, as you mention... I guess the main interaction you draw out is this tension between democratizing use... It's potentially not necessarily what a democratic process would produce on the governance side.

When I think about... since, I don't know, about a year ago, we've had [ChatGPT](https://chat.openai.com/auth/login), which in some sense, really democratized, I guess, the use of large language models in the sense that it's relatively cheap, maybe free to use these things. One effect is that now it is impossible to at least have banned ChatGPT a year ago or something. But to my mind, a big effect of this is something like, increasing people's knowledge of where AI is at, what kinds of things the language models can do.

And essentially, I don't know if this is a term, but you can think of people as having governance demands, right? If I know more about a technology, I might say, "Oh, this is totally fine." Or, "Oh, we need to do this thing or that thing about it." If people know more about a technology, they can basically have a better sense of what they want their governance demands to be. In that sense, it seems like at least to some degree, there can be a positive interaction between democratizing use and democratizing governance.

**Elizabeth Seger:**
Yeah, I think absolutely. This is sort of a classic pillar of democracy, is having the informed public. You need to have the information to be able to do the good decision-making about the object of your decision-making. Yeah, so I think the release of ChatGPT, for example, really put AI on the public stage. Suddenly, I have my mom and dad texting me, asking me questions about AI, and that never would have happened a year ago. Yeah, so I think it really put the technology on the stage. It's got more people involved in it. I think more people have a general sense of at least what current capabilities are able to do and of the limitations. That's all been very valuable for enabling more democratic decision-making processes about AI.

There's definitely that link between you democratize use, you democratize development, people understand the technology better. They'll be able to make more well-informed decisions or form more well-informed opinions about how the technology should be regulated. So I think that that's not necessarily in tension. I guess, the tension tends to go the other direction. It's like if a governance decision perhaps would say, "Oh, this particular technology is too dangerous." Or, "Releasing this particular model surpasses an acceptable risk threshold."

I guess, if you took a strict definition of democratizing development as meaning always spreading the technology further, always getting more people involved in a development process, always making a system accessible, then yeah, a decision to restrict access would be in direct conflict with democratizing development.

I think this is another thing we try and say in the paper: trying to get away from this idea that AI democratization is inherently good. I think this is a problem with the term democratization, especially in Western, democratic societies: oftentimes, democracy is used a a stand-in for all things good, so you get companies saying, "Oh, we're democratizing AI". It almost doesn't even matter what that means. They've democracy-washed their products. Now it looks like a good thing.

I think that was one thing we were trying to get across in the paper is, yeah, AI democratization is not necessarily a good thing. Spreading a technology for more people to contribute to a development process or for more people to use is only good if that's actually going to benefit people. But if it poses significant risk or it was going to put a lot of people in harm's way, then that's not inherently positive.

I guess, one thing to say is just if all that's meant when people say, "AI democratization," or, "democratize AI" is just make the system more accessible, then just say, "Make the system more accessible", because I think that's very clear what it means and it's not inherently good or bad. I don't know if we're going to successfully change everyone's terminology in the course of one podcast, but-

**Daniel Filan:**
Well, we can try.

**Elizabeth Seger:**
We can try. I think that's maybe one key takeaway from the paper as well. If people say they're engaging in an effort to democratize AI, that does not necessarily mean it's going to be a net beneficial effort.

### Democratizing profits from AI <a name="democratizing-profits"></a>

**Daniel Filan:**
Gotcha. I guess, I'd like to talk a bit about the individual senses. In particular, I want to start off with when you talk about democratizing profits. One thing I noticed is, I think there's one or two quotes you use in this section. I think the quotes actually say something like, "We want to make sure that not all the value created by AI stays in one place or that the benefits of AI don't just accrue to a small group of people."

In those quotes, they don't actually use the term "profit". In some ways, you can imagine that these could go different ways. For instance, take [GitHub Copilot](https://github.com/features/copilot). It costs, I guess, $100 a year. Suppose that everyone in the world just pays $100 a year and it all goes to GitHub. Tons of people use GitHub Copilot to do incredibly useful things, and they get a bunch of value and benefits out of it. Probably, most of the benefits will be financial, but not all of them will be. That seems to me like it would be a case of spreading the value or benefits of AI, but it's not necessarily a case of spreading the profit from AI.

**Elizabeth Seger:**
Yeah, the profits category is kind of a... It was an odd category. Actually, in the process of writing this paper... Unlike most papers, we actually wrote [the blog post](https://www.governance.ai/post/what-do-we-mean-when-we-talk-about-ai-democratisation) first and then wrote the paper. It's supposed to go in the other direction. In the blog post, which is on GovAI's website, it actually breaks into four categories: Democratization of use, democratization of development, democratization of benefits, and democratization of governance.

For the version in the paper, we chose the profits terminology instead of the benefits terminology because benefits, in a way, it was too broad because... I mean, you've already pointed this out. There's a lot of overlap between these categories, and one reason to democratize development, for example, is to make sure that more people can benefit from the technology, that it serves their need.

A reason to democratize use is to ensure that more people can access and use, and even use the systems to produce value and profit. If you could integrate it with your own businesses and with your own applications, you can develop value on it from your side. I think we chose this democratization of profits terminology just to point out that there is a discussion around just the massive accrual of profits to large tech companies.

I think, yeah, you do point out some of the quotes that we use talk a little bit more broadly about accrual of value. I think, sometimes it's hard to measure profits just in terms of the literal money that drops into someone's bank account.

**Daniel Filan:**
Okay.

**Elizabeth Seger:**
So I think... it's hard to find exact quotes from companies. I know that there's been a lot of discussion not by companies, but actually, this is one thing that you will hear policymakers talk about more. This was part of when universal basic income was something that was being discussed a lot. It had its moment when people were talking about it a lot more. A lot of the discussion around universal basic income was around... Well, it was around job loss caused by automation, but also around the accrual of profits to tech companies and to developers of these new technologies. Okay, if all the profits are going to shift from the workforce to the developers of these technologies, how are we going to redistribute it out? That's actually one context in which democratization of profits was directly linked to the discussion around AI development.

Yeah, I think maybe some of the quotes we used in the paper do more vaguely gesture to just the value. But we separated profits out just because the way we had it in the blog post, it was a bit too general. There was too much overlap. We were kind of double-dipping, you know? I think it was just to illustrate that there is this discussion specifically about profits, but I don't think... I mean, it's definitely not the most commonly used. If you heard someone talking about AI democratization just walking down the street, you wouldn't immediately think, "Oh, they must be talking about profit redistribution." That's not where your brain goes. But it is one aspect worth noting, I think, is the main takeaway.

### Democratizing AI governance <a name="democratizing-gov"></a>

**Daniel Filan:**
Fair enough. Yeah, the next one I want to talk a bit more about is democratizing AI governance. Related to the thing you just said, it seems to me that if somebody talks about democratizing AI, I think it's pretty unlikely that if they just say, "democratizing AI", they mean democratizing governance. I'm wondering if that matches with your experience.

**Elizabeth Seger:**
Yes and no. I think, oftentimes, when you see in media or you're listening to tech companies and the dialog that they're having... yeah, very rarely will it talk about democratizing governance. I think we are starting to see a shift though. For example, OpenAI had this [AI democratization grant program](https://openai.com/blog/democratic-inputs-to-ai) - I can't remember exactly what it's called right now. Basically, they're putting out grants to research groups to study methods for basically trying to bring in input from more diverse communities to try and better understand what values systems should align to. In that sense, you're talking about AI democratization in this more participatory, deliberative process of bringing people into decision-making processes to define principles or to define values for AI alignment. OpenAI had that grant program.

I think Anthropic just put out [a blog post](https://www.anthropic.com/index/collective-constitutional-ai-aligning-a-language-model-with-public-input). They'd worked closely with [the Collective Intelligence Project](https://cip.org/) that I mentioned earlier doing a similar thing for developing their [constitutional AI](https://arxiv.org/abs/2212.08073), defining what the constitutional principles are that AI systems should be aligned to, and that was more of a democratic governance process. So actually, I think we're starting to see this terminology of "AI democratization", "democratizing AI" seep into the technical landscape a bit more. I think that's a very positive thing. I think it's showing that they're starting to tap into and really embody the democratic principles of democratizing AI. As opposed to just meaning distribution and access, it actually means reflecting and representing the interests and values of stakeholders and impacted populations.

I think we are starting to see that shift, but I think you're probably right in that mostly... again, [if when] walking down the street, you hear someone talking about AI democratization, [they're] probably just talking about distribution of the technology, making it more accessible. That's the classic terminology, but I do think that in the AI governance space and even in the terminology being used by labs, we're starting to see the governance meaning seep in, and that's exciting.

### Normative underpinnings of democratization <a name="normative-underpinnings"></a>

**Daniel Filan:**
Gotcha. Getting to the question about whether this is good, I think in the paper, you say something like: the notion of democratization of use, development, and profit... I think you say something like those ones aren't inherently good and they get their normative force from democratization basically in terms of the governance method. So I guess first of all, that wasn't actually so obvious to me. So I think sometimes democratization, I think it often carries this implicit sense of being more related to egalitarianism than democracy as a political decision-making method. I think you can have egalitarianism without democratic decision-making and you can have democratic decision-making without egalitarianism, right? People can vote away minorities' rights or whatever. So yeah, I was wondering why do you say... I guess what I actually mean is please defend your claim that that's the source of the goodness of democracy, or of democratization, is the word.

**Elizabeth Seger:**
Yeah. Okay. So a couple of things, three things, maybe two things. We'll see how it comes out. So to start out with, I do stand by the idea that the first three forms of democratization, so development, use, profits, [are] not necessarily inherently good; not inherently bad, just these are things that happen, and if we stick by their definitions it just means making something more accessible, whether that's access for use or development or making the profits more accessible. Access is not inherently good or bad. Again, if we use the "access for development" example, if you have a technology that's going to be potentially really dangerous, you don't necessarily want everyone to have access to that. And a democratic decision-making process might result in the conclusion that indeed not everyone should have access to it.

So that's the first part: standing by that first half of the claim. The second part, around the thing that gives it the moral force or value is the democratic decision-making process that was involved... I don't know if that's exactly what we're going for in the paper. I think there are lots of different methods that you can use to help reflect and serve the interests of wider populations. I think for some technologies... let's go back to your water bottle example. We don't need a panel of people from around the globe to tell us how to distribute water bottles. This is a decision that probably water bottle distributors can be like, "let's just sell our water bottles to as many people as possible" and [it's] probably fine.

I think it comes to where the impact's going to be, how big the impact's going to be, are there areas where the negative impacts of a technology might be more disproportionately felt? In those cases, being able to bring those voices in to inform decision-making processes that might affect them disproportionately is really important. I know we give examples in the paper of different kinds of democratic processes that can be used to inform decision-making, whether these are participatory processes or more deliberative democratic processes. So in [the open source paper](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4596436) we just released, we talk about some of these processes as ways of making more democratic decisions around AI, but then also point to: one way to democratize governance decisions is to support regulation that is put forth by democratic governance and that those governments hopefully if structured well, reflect and serve the interests of the constituents.

And hopefully some of those governance processes are actually formed by having maybe some sort of deliberative process that takes into account stakeholder interests and the interests of impacted populations. But it's not like the only way to govern AI is to say, "okay, let's have a participatory panel where we take in all this insight and then use it to inform every decision". I think you're right, that would just be completely impractical to have every decision that a lab makes be informed by a participatory panel or some sort of deliberative democratic process. So I think you kind of have to range it depending on the potential impact of the question. So I don't know if that answers your question completely, but I guess I stand by the first point, that democratization is not inherently good. The extent of the goodness I guess reflects how well the decisions reflect and serve the interests of the people that are going to be impacted. And then there are lots of different ways of figuring out how to do that.

**Daniel Filan:**
Okay, that makes sense. So I guess the second thing I want to ask about that sentence is: when you say that the democratization of use, development and benefits is not inherently good, it kind of makes it sound like you think that democratization of governance is inherently good. So I found this paper that you actually cite, it's called [Against "Democratizing AI"](https://johanneshimmelreich.net/papers/against-democratizing-AI.pdf) by Himmelreich, published in 2023. So he basically makes a few critiques. Firstly, he just says that AI isn't the sort of thing that needs to be democratic. AI in itself doesn't affect a bunch of people. There are just organizations that use AI and they affect a bunch of people and maybe they should be democratized, but AI itself doesn't have to be.

He says that the relevant things are already democratic. Democracy is pretty resource-intensive. People have to know a bunch of stuff, maybe you have to take a bunch of votes and things. He also has complaints that democracy is imperfect and doesn't necessarily fix oppression. So there are a few complaints about democratizing AI floating around. I guess in this case he's talking about democratizing AI governance. Is it the case that democratizing AI governance is inherently good?

**Elizabeth Seger:**
I don't think so. Again, I think it comes back to the "ranging it" question. First of all, the resource-intensive point: absolutely. It can be incredibly resource-intensive to do an in-depth participatory governance process. This is why you don't do it for every single question that's out there. Yeah, so that's really good point. There's also the point of you need to be well-informed to make good decisions. So maybe we don't want the general public making decisions about all questions around AI. Experts are having a hard enough time for themselves trying to decide what constitutes a high risk system. We're not even good at benchmarking this among the community of AI experts. How are you going to tap into more general public notions of what a high risk system is or what proper benchmarking should look like?

So there's some questions where it just doesn't make sense to democratize it. So I think you have to think about: what questions is it realistic to have more democratized discussions around? So maybe something like thinking about what acceptable risk thresholds are, or what kind of values to align to. I think it's also important to remember there are lots of different kinds of democratic processes. When we talk about democratic processes, it's not necessarily what you might think of: we all go to the polls and cast our ballot on "do we want A or do we want B?" These could be more deliberative processes where you just get some stakeholders in a room, they have a discussion, they try and figure out what are the main issues [in] these discussions. So there are processes in place to hold deliberative democratic processes that are informed, you have experts come in and try and inform the discussion. So this would be a more informed deliberative process.

One way is: sometimes when it comes to... with image generation systems, there was a whole bunch of discussion around bias and images. Again, we see overlap between governance and other kinds of categories. If you get more people involved using the systems and they start understanding where the biases are, that is a form of education - having more familiarity. And then you can raise these complaints, whether that's directly to the companies or maybe through some sort of political intermediary. I remember there being, what was it? A [statement](https://eshoo.house.gov/media/press-releases/eshoo-urges-nsa-ostp-address-unsafe-ai-practices) released by [Anna Eshoo](https://en.wikipedia.org/wiki/Anna_Eshoo), who I think was the congressional representative in, I want to say South San Jose, that talked in quite a bit of depth about ... it was right after the release of [Stable Diffusion](https://stablediffusionweb.com/) and talking about how there was a lot of racial bias in some of the images that were being produced, specifically against Asian women.

So this is something that was raised through a democratic process, was brought to the attention of the congressional office, and then the statement was released, and then that had knock-on effects on future governance decisions and decision-making by labs to do things like put safety filters. So I think that's one thing to keep in mind: there's a wide range of things that you can think about in terms of what it means to democratize a governance process. Governance is just decision-making. Then how do you make sure those decision-making processes reflect the interests of the people who are going to be impacted. It's not just about direct participation, although there is a lot of work being done to bring more direct participatory methods into even the more small scale decision-making, to make it more realistic.

Like this complaint about it being so resource intensive: that's true. We also have technologies available to help make it less resource intensive and how can we tap into those to actually help a public input to these decisions in a well-informed way? So there's lots of interesting work going on. Democratic governance also isn't inherently good when it comes to AI governance. I think again, it depends on the question. If the cost of doing a full-blown democratic process is going to far outstrip the benefit of doing a full-blown democratic process, then there'd be a situation in which is probably not a net benefit to do a democratic decision-making process.

**Daniel Filan:**
So that gets to Himmelreich's resource-intensive complaint. He also has these complaints that basically AI itself is not the kind of thing that needs to be democratically governed. So he says something like, I'll see if I can remember, but the kind of thing that needs to be democratically governed is maybe something that just impinges on people's lives, whether they like or not, or something that's just inherent to large scale cooperation or something like that. He basically makes this claim that, look, AI doesn't do those things. AI is housed in institutions that do those things. So if you're worried about, maybe the police using facial recognition or something, you should worry about democratic accountability of police, not facial recognition, is the point I take him to be making.

So firstly, he says, look, we don't need to democratize AI. We need to democratize things that potentially use AI. And secondly, it seems like he's saying the relevant things that would be using AI, they're already democratic, so we shouldn't have conflicting ... I think he talks about democratic overlap or something, where I think he really doesn't like the idea of these different democratic institutions trying to affect the same decision and maybe they conflict or something.

**Elizabeth Seger:**
So I think I don't disagree. I think, if I understood correctly, this idea of "you don't need to democratize the AI, you need to democratize the institution that's using it", I think that's a completely valid point. AI itself, again, is not inherently bad or good. It's a dual use technology. It depends what you're going to do with the thing. But I would say that trying to decide, for example, how law enforcement should use AI systems, that is an AI governance question. This is a question about how AI is being used, about how AI is being regulated in this context, not in terms of how it's being developed, but in context of how it's being used.

What are the appropriate use cases of AI? This is an AI governance question, and it's not necessarily that these are regulations that are placed on AI specifically. A lot of AI regulation just already exists within different institutions. It's just figuring out how to have that regulation apply to AI systems. This might be cases like: if you have requirements for certain health and safety standards that have to be met by a certain technology in a work setting, AI shouldn't be held to different standards. It's just a matter of figuring out how to measure and make sure that those standards are met by the AI system. So I guess based on what you said, I want to say that I completely agree.

But I would say that it still falls under the umbrella of democratizing AI governance. I think that what might be happening here is just another conflict over what does the terminology mean, where it's like "we don't need to democratize AI, as in we don't need to get more people directly involved in the development and decision-making about individual decisions around AI development". In which case, yes, I agree! But we might be talking about democratizing AI governance in different... This is why these discussions get really complicated because we just end up talking past each other because we use the same word to mean five different things.

**Daniel Filan:**
Yeah, it can be rough. So before we close up our discussion on democratizing AI, I'm wondering... I guess you've thought about democratizing AI for a while. Do you have any favorite interventions to make AI more democratic or to make AI less democratic in any of the relevant senses?

**Elizabeth Seger:**
I'll be completely honest, this is sort of where my expertise drops off in terms of the more nitty-gritty of what processes are available and exactly when are certain processes the best ones to be applied. For this, I would really look more closely at the work that [the Collective Intelligence Project](https://cip.org/)'s doing. Or my co-author [Aviv Ovadya](https://aviv.me/), who's on the paper, he's really involved with these discussions and works with various labs to help implement different democratic processes. Yeah, I was just going to say, this is where my expertise drops off and I would point towards my colleagues for more in-depth discussion on that work.

## Open-sourcing AI <a name="osai"></a>

**Daniel Filan:**
All right, the next thing I want to talk about is basically open-sourcing AI. So there's this report, [Open-sourcing highly capable foundation models: an evaluation of risks, benefits, and alternative methods for pursuing open source objectives](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4596436). It's by yourself and a bunch of other co-authors, but mostly as far as I could see at [the Center for the Governance of AI](https://www.governance.ai/). Again, could you perhaps give an overview of what's going on in this report?

**Elizabeth Seger:**
Okay, yeah. So this report's a little harder to give an overview on because unlike the democratization paper, which is eight pages, this one's about 60 pages.

So quick overview. First thing is that we wanted to point out that there's debate right now around whether or not large foundation models, especially frontier foundation models, should be open source. This is a debate that has kicked off starting with [Stability AI](https://stability.ai/)'s release of [Stable Diffusion](https://stablediffusionweb.com/), and then there was the [Llama 2](https://ai.meta.com/llama/) release and then the weights were leaked, and now Meta's getting behind this open source thing; there were the [protests outside of Meta](https://spectrum.ieee.org/meta-ai) about whether or not systems should be open source. And then we're also seeing a lot of activity with the development of the [EU AI Act](https://artificialintelligenceact.eu/). As part of the EU AI Act, some parties are pushing for an exemption of regulation for groups that develop open source models. So a lot of discussion around open source. I think the goal of this paper was to cut through what is becoming a quickly polarized debate.

We're finding that you have people that see the benefits of open-sourcing and are just very, very hardcore pro open source and then won't really hear any of the arguments around the potential dangers. And then you have people who are very anti open source, only want to talk about the dangers and sort of refuse to see the benefits. It just makes it really hard to have any kind of coherent dialogue around this question. Yet at the same time, country governments are trying to make very real policy around model sharing. So how can we cut through this polarized debate to just try and say, here's the lay of the land. So what we're trying to do in this paper is really provide a well-balanced, well-researched overview of both the risks and benefits of open-sourcing highly capable models.

By 'highly capable models', basically what we're talking about is frontier AI systems. We just don't use the term 'frontier' because the frontier moves, but we want to be clear that there will be some systems that might be highly capable enough and pose risks that are high enough that, even if they aren't on the frontier, we should still be careful about open-sourcing them. So basically we're talking about frontier models, but frontier keeps going. So we can get into the terminology debate a little bit more later. But basically we wanted to outline what are the risks and benefits of open-sourcing these increasingly highly capable models, but then not just to have yet another piece that says, well, here are the risks and here are the benefits. But then kind of cut through by saying 'maybe there are also alternative ways that we can try to achieve some of the same benefits of open-sourcing, but at less risk.'

So the idea here is actually quite similar to the democratization paper: it's to not just say 'what is open-sourcing?' and 'is it good or bad?' But to say 'What are the specific goals of open-sourcing? Why do people say that open-sourcing is good, that it's something we should be doing that should be defended, that should be preserved? So why is it we want to open source AI models? Then if we can be specific about why we want to open source, are there other methods that we can employ to try to achieve some of these same goals at less risk?' These might be other model sharing methods like releasing a model behind API or doing a staged release, or it might be other more proactive measures like: let's say one benefit of open-sourcing is that it can help accelerate research progress, both in developing more greater AI capabilities, but also promoting AI safety research.

Another way you can promote AI safety research is to dedicate a certain percentage of profits towards AI safety research or to have research collaborations. You can do these things without necessarily having to provide access for anybody to have access to the model. So this is what we were trying to do with this paper: really just say, okay, we're going to give, first of all, just a very well-researched overview of both the risks and benefits. In fact, majority of the paper actually focuses on what are the benefits and trying to break down the benefits of open-sourcing and then really doing a deep dive into alternative methods or other things that can be done to help pursue these benefits where the risks might just be too high.

I think that the main claim that we do make in the paper with regard to 'is open-sourcing good or bad?' First of all, it's a false dichotomy, but I think our main statement is open-sourcing is overwhelmingly good. Open source development of software, open source software underpins all of the technology we're using today. It's hugely important. It's been great for technological development, for the development of safe technology that reflects lots of different user needs and interests. Great stuff. The issue is that there might be some cutting edge, highly capable AI systems that pose risks of malicious use or the proliferation of dangerous capabilities or even proliferation of harms and vulnerabilities to downstream applications.

Where these risks are extreme enough, we need to take a step back and say: maybe we should have some processes in place to decide whether or not open-sourcing these systems is okay. So I think the main thrust of the report is [that] open-sourcing is overarching good, there just may be cases in which it's not the best decision and we need to be careful in those cases. So yeah, I think that's the main overview of the paper.

**Daniel Filan:**
Gotcha. Yeah, in case listeners were put off by the 60 something pages, I want listeners to know there is an executive summary. It's pretty readable.

**Elizabeth Seger:**
It's a two-page executive summary. Don't worry.

**Daniel Filan:**
Yeah. I read the paper and I vouch for the executive summary being a fair summary of it. So don't be intimidated, listeners.

**Elizabeth Seger:**
I've also been putting off writing the blog post, so there will be a blog post version at some point, I promise.

**Daniel Filan:**
Yeah. So it's possible that might be done by the time we've released this, in which case we'll link the blog post.

**Elizabeth Seger:**
Maybe by Christmas.

### Risks from open-sourcing <a name="risks-from-os"></a>

**Daniel Filan:**
Yeah. Hopefully you'll have a Christmas gift, listeners. So when we're talking about these highly capable foundation models and basically about the risks of open-sourcing them... as I guess we've already gotten into, what is a highly capable foundation model is a little bit unclear.

**Elizabeth Seger:**
Yeah. It's already difficult.

**Daniel Filan:**
Maybe it'll be easier to say, what kinds of risks are we talking about? Because I think once we know the kinds of risks, (a) we know what sorts of things we should be willing to do to avert those risks or just what we should be thinking of, and (b) we can just say, okay, we're just worried about AIs that could potentially cause those risks. So what are the risks?

**Elizabeth Seger:**
In fact, I think framing it in terms of risks is really helpful. So generally here we're thinking about risks of significant harm, significant societal or even physical harm or even economic harm. And of course now you say, "oh, define 'significant'". But basically just more catastrophic, extreme, significant societal risks and harms. So we use some examples in the paper looking at potential for malicious use.

There are some more diffuse harms if you're thinking about political influence operations. So this kind of falls into the misinformation/disinformation discussion. How might AI systems be used to basically influence political campaigns? So disinformation undermines trust and political leaders. And then in turn, if you can disrupt information ecosystems and disintegrate the processes by which we exchange information or even just disintegrate trust in key information sources enough, that can also impact our ability as a society to respond to things like crises.

So the pandemic is a good example of, if it's really hard to get people accurate information about, for example, whether or not mask-wearing is effective, you're not going to have really good coordinated decision-making around mask-wearing. So I think this is one where it's a little bit more diffuse. It's harder to measure. So I think this is one point where it's quite difficult to identify when the harm is happening because it's so diffused. But I think this is one potential significant harm. I'd say maybe thinking about disrupting major political elections or something like that.

There's some options for malicious use that are talked about a little more frequently, I think because they're more well-defined and easier to wrap your head around. These are things like using generative AI to produce biological weapons or toxins, even production of malware to mount cyber attacks against key critical infrastructure. Imagine taking down an electrical grid, or imagine on election day, taking down an electoral system. These could have significant societal impacts in terms of harm to society or physical harm.

I think one key thing to point out here is that we aren't necessarily seeing these capabilities already. Some people may disagree, but I think my opinion is that technologies with the potential for this kind of harm do not currently exist. I think it is wise to assume that they will come into existence in the not too distant future, largely because we're seeing indications of these kinds of capabilities developing. We've [already seen how even narrow AI systems that are used in drug discovery, if you flip that parameter that's supposed to optimize for non-toxicity to optimize for toxicity, now you have a toxin generator](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9544280/). So it doesn't take a huge stretch of the imagination to see how more capable systems could be used to cause quite significant societal harm. So no, I don't think there's currently systems that do this. I think we need to be prepared for a future in which these systems do exist. So it's worth thinking about the wisdom of releasing these systems now even if we might not be present with the technology at this moment.

**Daniel Filan:**
So as a follow-up question: you're currently on the AI X-risk Research Podcast. I'm wondering... so it seems like you're mostly thinking of things less severe than all humans dying or just permanently stunting the human trajectory.

**Elizabeth Seger:**
I wouldn't say necessarily. I think this is definitely a possibility. I think part of what's happening is you kind of have to range how you talk about these things. I am concerned about x-risk from AI, and I think we could have systems where whether it's the cyber capability or the biological terror capability or even the taking down political systems capability and bolstering authoritarian governments - these are things that could cause existential risk and I am personally worried about this. But [we're] trying to write papers that are digestible to a much wider population who might not be totally bought into the x-risk arguments. I think, x-risk aside, there are still very genuine arguments for not necessarily releasing a model because of the harm that it can cause even if you don't think that those are existential risks. Even if it's just catastrophic harm, probably still not a great idea.

**Daniel Filan:**
So at the very least, there's a range going on there.

**Elizabeth Seger:**
Yeah.

### Should we make AI too dangerous to open source? <a name="make-ai-too-dangerous-os"></a>

**Daniel Filan:**
So one question I had in my mind, especially when thinking about AI that can cause these kinds of risks and potentially open-sourcing it... Part of me is thinking, it seems like a lot of the risks of open-sourcing seem to be, well, there's now more people who can use this AI system to do a bunch of harm. The harm is just so great that that's really scary and dangerous. So one thing I was wondering is: if it's so dangerous for many people to have this AI technology, is it not also dangerous for anybody at all to have this AI technology? How big is this zone where it makes sense to make the AI but not to open source it? Or does this zone even exist?

**Elizabeth Seger:**
Yeah, you make a very genuine point, and there are those that would argue that we should just hit the big old red stop button right now. I think that is an argument some people make. I guess where we're coming from with respect to this paper is trying to be realistic about how AI development's probably going to happen and try to inform governance decisions around what we should do as AI development continues. I think there are differing opinions on this, probably even among the authors on the paper. There's what, 25, 26 authors on the paper.

**Daniel Filan:**
The paper has a disclaimer, listeners, that not all authors necessarily endorse every claim in the paper.

**Elizabeth Seger:**
That includes me. So we have a very, very broad group of authors. But I think... Sorry, what was your original question? It just flew out of my head.

**Daniel Filan:**
If it's too dangerous to open source, is it also too dangerous to make?

**Elizabeth Seger:**
Yeah, no. So I think there's definitely an argument for this. I guess where I'm coming from is trying to be realistic about where this development process is going and how to inform governments on what to do as AI development continues. It may very well be the case that we get to a point where everyone's convinced that we just have a big coordinated pause/stop in AI development. But assuming that that's not possible or improbable, shall we say, I think it's still wise to have a contingency plan. How can we guide AI development to be safe and largely beneficial and reduce the potential for risk? I think there's also an argument to be made that increasingly capable systems, while they pose increasingly severe risks, also could provide increasingly great benefits and potential economic potential benefits for helping to solve other challenges and crises that we face.

So I think you'd be hard-pressed to get a giant coordinated pause. So the next plan is how do we make AI development happen safely? It is a lot easier to keep people safe if the super dangerous ones are not spread widely. I guess that's the very simplistic view. Yeah.

So I think, at least for me, I think it just comes from a realism about, are we likely to pause AI development before it gets super scary? Probably not, just given how humanity works. We developed nuclear weapons, probably shouldn't have done that. Oops, well now they exist, let's deal with it. So I think having a similar plan in line for what do we do with increasingly capable AI systems is important, especially given that it might not be that far away. Like I said, sort of with each new generation of AI that's released, we all kind of hold our breaths and say, oh, well what's that? What's that one going to do? Then we learn from it and we don't know what the next generation of AI systems is going to bring. And so having systems in place to scan for potential harms, potential dangers, capabilities, to inform decisions about whether or not these systems should be released and if and how they should be shared, that's really important. We might not be able to coordinate a pause before the big scary happens. So, I think it's important to discuss this regardless.

**Daniel Filan:**
I got you.

**Elizabeth Seger:**
That's a technical term by the way, the big scary.

### Offense-defense balance <a name="offense-defense"></a>

**Daniel Filan:**
Makes sense. So speaking of the risks of open-sourcing AI, in the paper you talk about the offense-defense balance, and basically you say that bad actors, they can disable misuse safeguards, they can increase new dangerous capabilities by fine-tuning. Open-sourcing makes this easier, it increases attacker knowledge. And in terms of just the AI technology, you basically make a claim that it's tilted towards offense in that attackers can do... They get more knowledge, they can disable safeguards, they can introduce new dangerous capabilities. It's easier to find these problems than it is to fix them. And once you fix them, it's hard to make sure that everyone has the fixes. Would you say that's a fair summary of-

**Elizabeth Seger:**
Yeah, I'd say that's pretty fair. I think the one thing I'd add... I'd say that with software development, for example... so offense-defense balance is something that's often discussed in terms of open-sourcing and scientific publication, especially any time you have dual-use technology or scientific insights that could be used to cause harm, you kind of have to address this offense-defense balance. Is the information that's going to be released going to help the bad actors do the bad things more or less than it's going to help the good actors do the good things/prevent the bad actors from doing the bad things? And I think with software development, it's often in favor of defense, in finding holes and fixing bugs and rolling out the fixes and making the technology better, safer, more robust. And these are genuine arguments in favor of why open-sourcing AI systems is valuable as well.

But I think especially with larger, more complex models, we start veering towards offense balance. I just want to emphasize, I think one of the main reasons for this has to do with how difficult the fixes are. So in the case of software, you get bugs and vulnerabilities that are relatively well-defined. Once you find them, relatively easy to fix, roll out the fix. The safety challenges that we're facing with highly capable AI systems are quite complex. We have huge research teams around the world trying to figure them out, and it's a very resource-intensive process, takes a lot of talent. Vulnerabilities are still easy to find, they're just a lot harder to fix. So, I think this is a main reason why the offense-defense balance probably skews more towards offense and enabling malicious actors because you can still find the vulnerabilities, you can still manipulate the vulnerabilities, take advantage of them... harder to fix, even if you have more people involved. So, that's the high-level evaluation. Mostly, I just wanted to push on the safety issue a little harder.

### KataGo as a case study <a name="katago-as-case-study"></a>

**Daniel Filan:**
Gotcha. So, one thing this actually brings up for me is... you may or may not be familiar, so there's AI that plays the board game Go. There's an [open-source training run](https://katagotraining.org/) and some colleagues of mine have found basically [you can cheaply find adversarial attacks](https://goattack.far.ai/). So, basically dumb Go bots that play in a way that confuses these computer AI policies. And these attacks, they're not generally smart, but they just push the right buttons on the AI policy. And I guess this is more of a comment than a question, but there's been enough rounds of back and forth between these authors and the people making these open source Go bots that it's potentially interesting to just use that as a case study of the offense-defense balance.

**Elizabeth Seger:**
So, you're saying that given that the system was open source and that people could sort of use it and query it and then send the information back to the developers, that that's-

**Daniel Filan:**
Yeah, and in fact, these authors have been working with the developers of this software, and in fact what's happened is the developers of this software - KataGo, if people want to google it - they've seen this paper, they're trying to implement patches to fix these issues. The people finding the adversarial policies are basically checking if the fixes work and publishing the information about whether they do or not.

**Elizabeth Seger:**
Absolutely, so I want to be really clear that this is a huge benefit of open source development: getting people involved in the development process, but also using the system, finding the bugs, finding the issues, feeding that information back to the developers. This is a huge benefit of open source development for software and AI systems. I think that this is specifically why the paper focuses on the big, highly capable frontier foundation models is that this gets more difficult the more big, complex, cutting edge the system is. Some bugs will still be relatively small, well-defined, and there are bug bounty programs, proposals for AI safety bounty programs as well, helping to find these vulnerabilities and give the information back to the developers. I think there are issues though, with respect to some of the larger safety issues that are more difficult. Sometimes it's difficult to identify the safety problems in the first place, more difficult to address the safety problems.

Then, there's also the issue of just rolling out the fixes to the system. So software development, you fix a bunch of bugs, you roll out an update. Oftentimes, in the license it'll say that you're supposed to actually use the updated version. There's some data that came out, I can't remember which organization it was right now, I'll have to look it up later, but it's actually quite a low uptake rate of people actually running the up-to-date software. So first of all, even with just normal software, it's hard to guarantee that people are actually going to run the up-to-date version and roll out those fixes, which is an issue with open source because if you're using something behind API, then you just update the system and then everyone's using the updated system. If you have an open source system, then people actually have to download and run the update version themselves.

With foundation models, there's actually a weird incentive structure that changes where people might actually be de-incentivized to update. So with software, oftentimes when you have an update, it fixes some bugs and it improves system functionality. When it comes to safety fixes for foundation models, oftentimes it has to do with reducing system functionality, like putting on a filter that says, "Well, now you can't produce this class of images. Now, you can't do this kind of function with the system," so it's hard, I don't know if there's good information on how this has actually panned out now. Are we seeing lower uptake rates with updates for AI systems? I don't know, but there might be something weird with incentive structures going on too, where if updates basically equate to reducing system functionality in certain ways, people might be less likely to actually take them on board.

**Daniel Filan:**
I don't have a good feel for that.

**Elizabeth Seger:**
I don't have a super good feel, but just, I don't know, interesting food for thought, perverse incentive structures.

**Daniel Filan:**
I don't know, I'm still thinking about this KataGo case: so that's the case where the attack does reduce the system functionality and people are interested in getting the latest version with fixes. It also occurs to me that... So, in fact the structure of this paper, the way they found the attack did not rely on having access to the model weights. It relied on basically being able to query the Go bot policy, basically to try a bunch of things and figure out how to trick the Go bot policy. Now, it's really helpful if you can have the weights locally just so that you can call the API, so that you can call it a lot, but that was not a case where you needed the actual weights to be shared. So on the one hand, that's a point that sharing the weights is less valuable than you might think, but it also suggests if you're worried about people finding these adversarial attacks, then just putting the weights behind an API doesn't protect you as much as you think. Maybe you need to rate limit or something.

**Elizabeth Seger:**
I think that's a valuable insight, there are definitely things you can do without weights. This is an argument for why you should be worried anyway, but it's also an argument for... There are lots of arguments for, well, open-sourcing is important because you need it to do safety research and getting more people involved in safety research will result in safer systems, you have more people input into these processes, but you just illustrated a perfect example of how just having query access, for example, to a system, can allow you to do a significant amount of safety research in terms of finding vulnerabilities.

### Openness for interpretability research <a name="open-for-interp"></a>

**Elizabeth Seger:**
So, query access is one that can be done completely behind an API, but then even if we think about something like interpretability research, interpretability research does require much more in-depth access to a system to do, but arguably this is an argument for needing access to smaller systems. We are struggling to do interpretability research on smaller, well-defined systems. It's sort of like the rate limiting factor on interpretability research isn't the size of the models people have access to, the way I understand it at least. If we're struggling to do interpretability research on smaller models, I feel like having access to the biggest, most cutting edge frontier model is not what needs to happen to drive interpretability research.

**Daniel Filan:**
I think it depends on the kind of interpretability research.

**Elizabeth Seger:**
It'd be interesting to hear your thoughts on this as well, but there's a range of different kinds of AI research. Not all of it requires open access and then some of the kinds that do require open access to the models isn't necessarily helped the most by having the open access, and then there is also this idea of alternative approaches that we talk about in the paper. You can help promote AI safety research by providing access to specific research groups. There are other things you can do to give people the access they need to do safety research.

**Daniel Filan:**
So, I guess I can share my impressions here. Interpretability research is a broad bucket. It describes a few different things. I think there are some kinds of things where you want to start small and we haven't progressed that far beyond small. So just understanding, 'can we just exhaustively understand how a certain neural net works?' - start small, don't start with GPT-4. But I think one thing you're potentially interested in, in the interpretability context, is how things get different as you get bigger models, or do bigger models learn different things or do they learn more? What sorts of things start getting represented? Can we use interpretability to predict these shifts? There you do want bigger models. In terms of how much can you do without access to weights, there's definitely a lot of interpretability work on these open source models because people apparently really do value having the weights.

Even in the case of the adversarial policies work I was just talking about, you don't strictly need access to the weights, but if you could run the games of Go purely on your computer, rather than calling the API, waiting for your request to be sent across the internet, and the move to be sent back, and doing that a billion times or I don't know the actual number, but it seems like just practically-

**Elizabeth Seger:**
Much more efficient.

**Daniel Filan:**
... It's easier to have the model. I also think that there are intermediate things. So one thing the paper talks about, and I guess your colleague [Toby Shevlane](https://sites.google.com/view/tobyshevlane) has [talked about](https://arxiv.org/abs/2201.05159) is basically structured access of giving certain kinds of information available maybe to certain people or maybe you just say "these types of information are available, these types aren't". I've heard colleagues say, "Even if you didn't open source GPT-4 or GPT-3, just providing final layer activations or certain kinds of gradients could be useful", which would not... I don't think that would provide all the dangers or all the risks that open-sourcing could potentially involve.

**Elizabeth Seger:**
I think this is a really key point as well, is trying to get past this open versus closed dichotomy. Just saying that something isn't open source doesn't necessarily mean that it's completely closed and no-one can access it. So like you said, Toby Shevlane talks about structured access, and it was a paper we referenced - at least when we referenced it, it was still forthcoming, it might be out now - but it was Toby Shevlane and Ben Bucknell were working on it, and it was about the potential of developing research APIs. So, how much access can you provide behind API to enable safety research and what kind of access would that need to look like and how could those research APIs be regulated and who by? So, I think if there's a genuine interest in promoting AI safety research and a genuine acknowledgement of the risks of open-sourcing, we could put a lot of resources into trying to develop and understand ways to get a lot of the benefits to safety research that open-sourcing would have by alternative means.

It won't be perfect. By definition, it's not completely open, but if we take the risks seriously, I think it's definitely worth looking into these alternative model sharing methods and then also into the other kinds of proactive activities we can engage in to help promote safety research, whether that's committing a certain amount of funds to safety research or developing international safety research organizations and collaborative efforts. I know one issue that always comes up when talking about, "Well, we'll just provide safety research access through API, or we'll provide privileged downloaded access to certain groups." It's like, "Well, who gets to decide who has access? Who gets to do the safety research?"

And so, I think this points to a need to have some sort of a multi-stakeholder governance body to mediate these decisions around who gets access to do the research, whether you're talking about academic labs or other private labs, sort of like [how] you have multi-stakeholder organizations decide how to distribute grants to do environmental research, or you have grantmaking bodies that distribute grant funds to different academic groups. You could have a similar type situation for distributing access to more highly capable, potentially dangerous systems, to academic groups, research groups, safety research institutions that meet certain standards and that can help further this research.

So, I feel like if there's a will to drive safety research forward, and if varying degrees of access are needed to allow the safety research to happen, there are things we can do to make it happen that do not necessarily require open-sourcing a system. And I think, like we said, different kinds of safety research require varying degrees of access. It's not like all safety research can be done with little access. No, you need different amounts of access for different kinds of safety research, but if there's a will, there's a way.

### Effectiveness of substitutes for open sourcing <a name="os-substitutes"></a>

**Daniel Filan:**
So, I want to ask something a bit more quantitative about that. So, some of the benefits of open-sourcing can be gained by halfway measures or by structured access or pursuing tons of collaborations, but as you mentioned, it's not going to be the same as if it were open sourced. Do you have a sense of... I guess it's going to depend on how constrained you are by safety, but how much of the benefits of open source do you think you can get with these more limited sharing methods?

**Elizabeth Seger:**
That's a good question. I think you can get quite a bit, and I think, again, it sort of depends what kind of benefit you're talking about. So in the paper, I think we discuss three different benefits. Let's say we talk about accelerating AI research, so that's safety research and capability research. We talk about distributing influence over AI systems, and this ranges everything from who gets to control the systems, who gets to make governance decisions about the systems, who gets to profit. It wraps all the democratization themes together under distributing influence over AI, and then, let's see, what was the other one that we talked about? You'd think I've talked about this paper enough in the last three months, I have it down. Oh, external model evaluation. So, enabling external oversight and evaluation, and I think it depends which one you're talking about.

Let's start with external model evaluation. I think that this probably benefits the most from open-sourcing. It depends what you're looking at, so for example, if you're just looking for minor bugs and stuff like that, you don't need open source access for that, but having a more in-depth view to the systems is more important for people trying to help find fixes to the bugs. We've discussed this. There are also risks associated with open-sourcing. If we're talking about accelerating capability research, for example, which sort of falls under the second category, I think you might find that the benefits of open-sourcing here might be somewhat limited the larger and more highly capable the system gets. And I think this largely will just have to do with who has access to the necessary resources to really operate on the cutting edge of research and development. Open source development, it operates behind the frontier right now largely because of restrictions... not restrictions, but just the expense of the necessary compute resources.

And then you talk about distributing control over AI, we've already discussed the more distributed effect of open-sourcing and model sharing on distributing control. It's a second order effect: you get more people involved in the development process and then large labs have more competition, and then it distributes influence and control.

There are probably more direct ways you can help distribute control and influence over AI besides making a system widely available. So, to answer your original question then about, how much of the benefit of open-sourcing can you get through alternative methods? I guess it really depends what benefit you're talking about. I think for AI safety progress probably quite a bit, honestly; actually the vast majority of it, given that a lot of the safety research that's done on these highly capable, cutting edge models is something that has to happen within well-resourced institutions anyway, or you need the access to the resources to do that, not just the code and the weights, but the computational resources and so on.

So, I think quite a bit. I think it's less of a "can we get close to the same benefits that open-sourcing allows?" It's more like, "can we do it in one fell swoop?" That's the thing. It's like open-sourcing is the easy option. "Here, it's open!" - and now you get all these benefits from open-sourcing. The decision to open-source or not, part of the reason it's a hard decision is because achieving these benefits by other means is harder. It's going to take more resources to invest, more organizational capacity, more thought, more cooperation. It's going to take a lot of infrastructure, a lot of effort. It's not the one-stop shop that open-sourcing is, but I think the idea is that if the risks are high enough, if the risks are severe enough, it's worth it. I think that's where it comes in.

So, I guess it's worth reiterating again and again: this paper is not an anti-open source paper, [it's] very pro-open source in the vast majority of cases. What we really care about here are frontier AI systems that are starting to show the potential for causing really catastrophic harm, and in these cases, let's not open-source and let's pursue some of these other ways of achieving the same benefits of open source to safety and distributing control and model evaluation, but open-source away below that threshold. The net benefits are great.

### Offense-defense balance, part 2 <a name="offense-defense-2"></a>

**Daniel Filan:**
Gotcha. So my next question - I actually got a bit sidetracked and wanted to ask it earlier - so in terms of the offense-defense balance, in terms of the harms that you are worried about from open-sourcing, I sometimes hear the claim that basically, "Look, AI, if you open-source it, it is going to cause more harm, but you also enable more people to deal with the harm." So I think there, they're talking about offense-defense balance, not of finding flaws in AI models, but in the underlying issues that AI might cause. So, I guess the idea is something... To caricature it, it's something like, "Look, if you use your AI to create a pathogen, I can use my AI to create a broad spectrum antibiotic or something", and the hope is that in these domains where we're worried about AI causing harm, look, just open-sourcing AI is going to enable tons of people to be able to deal with the harm more easily, as well as enabling people to cause harm. So I'm wondering, what do you think about the underlying offense-defense balance as opposed to within AI?

**Elizabeth Seger:**
I get the argument. Personally, I'm wary about the arms race dynamic though. You gotta constantly build the stronger technology to keep the slightly less strong technology in check. I guess this comes back to that very original question you asked about, "What about just hitting the no more AI button?" So, I guess I get the argument for that. I think there's weird dynamics, I don't know. I'm not doing a very good job answering this question. I'm personally concerned about the race dynamic here, and I think it just comes back to this issue of, how hard is it to fix the issues and vulnerabilities in order to prevent the misuse in the first place? I think that should be the goal: preventing the misuse, preventing the harm in the first place. Not saying, "Can we build a bigger stick?"

There's a similar argument that is brought up when people talk about the benefits of producing increasingly capable AI systems and saying, "Oh, well, we need to plow ahead and build increasingly capable AI systems because you never know, we'll develop a system that'll help cure cancer or develop some renewable energy technology that'll help us address climate change or something like that." What huge problems could AI help us solve in the future? And I don't know - this is personally me, I don't know what the other authors on this paper think of this - but I don't know, I kind of feel like if those are the goals, if the goals are to solve climate change and cure cancer, take the billions upon billions upon billions and billions of dollars that [we're] currently putting into training AI systems and go cure cancer and develop renewable technologies! I struggle with those arguments personally. I'd be interested just to hear your thoughts. I have not written about this. This is me riffing right now. So, I'd be interested to hear your thoughts on this train of thought as well.

**Daniel Filan:**
I think the original question is unfairly hard to answer just because it's asking about the offense-defense balance of any catastrophic problem AI might cause and it's like, "Well, there are tons of those and it's pretty hard to think about." So, the thing you were saying about, if you wanted to cure cancer, maybe step one would not be "create incredibly smart AI". I've seen this point. I don't know if you know [David Chapman](https://meaningness.com/about-my-sites)'s [Better without AI](https://betterwithout.ai/)?

**Elizabeth Seger:**
No, not familiar.

**Daniel Filan:**
So, he basically argues we just shouldn't build big neural nets and it's going to be terrible. Also, [Jeffrey Heninger](https://aiimpacts.org/author/jeffreyheninger/) at [AI Impacts](https://aiimpacts.org/), I think has said [something similar along these lines](https://blog.aiimpacts.org/p/my-current-thoughts-on-the-ai-strategic). On the one hand, I do kind of get it, just in the sense that, if I weren't worried about misaligned AI, there's this hope that this is the last invention you need. You create AI and now instead of having to separately solve cancer and climate change and whatever, just make it solve those things for you.

**Elizabeth Seger:**
I guess it's just really hard to look forward, and you have to decide now whether or not this technology is that silver bullet and how much investment it's going to take to get to that point.

**Daniel Filan:**
I think that's right, and I think your take on this is going to be driven by your sense of the risk profile of building things that are significantly smarter than us. I guess from the fact that I made the AI X-risk Research Podcast, rather than the AI Everything's Going to be Great Research Podcast, people can guess my-

**Elizabeth Seger:**
It's an indication of where you're coming from.

**Daniel Filan:**
... take on this, but I don't know. I think it's a hard question. So, part of my take is, in terms of the underlying offense-defense balance, I think it becomes more clear when you're worried about, what should I say, silicogenic risks? Basically the AI itself coming up with issues rather than humans using AI to have nefarious schemes. Once you're worried about AI doing things on their own where you are not necessarily in control, there I think it makes sense that you're probably... If you're worried about not being able to control the AIs, you're probably not going to be able to solve the risks that the AIs are creating, right?

**Elizabeth Seger:**
Yeah, your management plan for AI shouldn't be to build a slightly more powerful AI to manage your AI.

**Daniel Filan:**
Well, if you knew that you were going to remain in control of the slightly bigger AI, maybe that's a plan, but you kind of want to know that.

**Elizabeth Seger:**
I guess I was saying that if you're worried about loss of control scenarios, then the solution shouldn't be, "Well, let's build another system that's also out of our control, but just slightly better aligned to address the..." I feel like that's-

**Daniel Filan:**
It's not the greatest. I think my colleague John Wentworth has [some saying](https://www.lesswrong.com/posts/DwqgLXn5qYC7GqExF/godzilla-strategies), "Releasing Mothra to contain Godzilla is not going to increase property values in Tokyo," which is a cute little line. I don't know, it's a hard question. I think it's hard to say anything very precise on the topic. I did want to go back to the offense-defense balance. So moving back a bit, a thing you said was something like, "Look, it's probably better to just prevent threats from arising, than it is to have someone make a pathogen and then have everyone race to create an antibiotic or antiviral or whatever." So, that's one way in which everyone having really advanced AI... That's one way that could look in order to deal with threats. I think another way does look a bit more like prevention. I don't know, it's also more dystopian sounding, but one thing that AI is good at is surveillance, right?

**Elizabeth Seger:**
Yes.

**Daniel Filan:**
Potentially, so you could imagine, "Look, we're just going to open source AI and what we're going to use the AI for is basically surveilling people to make sure the threats don't occur." So, maybe one version of this is you just really amp up wastewater [testing]. Somehow you use your AI to just look at the wastewater and see if any new pathogens are arising. It could look more like you have a bunch of AIs that can detect if other people are trying to use AI to create superweapons or whatever, and stop them before they do somehow.

**Elizabeth Seger:**
The wastewater example, that sounds great. We should probably do that anyway. In terms of surveilling to see how people are using AI systems using AI, why not just have the AI systems be behind an API where people can use the systems for a variety of downstream tasks integrating through this API and then the people who control the API can just see how the system is being used? Even if it can be used for a vast majority of tasks, even if you were to take all the safety filters off, the advantage of the API is still that you can see how it's being used. I don't know, I feel like that's-

### Making open-sourcing safer? <a name="making-os-safer"></a>

**Daniel Filan:**
That seems like a good argument. All right, so I guess another question I have is related to the frame of the report. So in the report you're basically like, "open-sourcing has these benefits, but it also has these costs. What are ways of doing things other than open-sourcing that basically try and retain most of the benefits while getting rid of most of the costs?" You can imagine a parallel universe report where you say, "Okay, open-sourcing has these benefits, but it also has these costs. We're still going to open source, but we're going to do something differently in our open source plan that is going to retain benefits and reduce costs", right? So one example of this is you open-source models, but you have some sort of watermarking or you have some sort of cryptographic backdoor that can stop models in their tracks or whatever. I'm wondering: why the frame of alternatives to open-sourcing rather than making open-sourcing better?

**Elizabeth Seger:**
Very simple. I think making open-sourcing better is the harder question, technically more difficult. I mean, for example, say you have watermarking, part of the issue with watermarking to identify artificially generated images is making sure the watermarks stick. How do you make sure that they are irremovable if you are going to open... This is a really complex technical question: how do you develop a system that has watermarked images where that watermark is irremovable if you were to open source the system?

I'm not saying it's undoable. I personally don't have the technical background to comment very deeply on this. I have heard people talking about how possible this would be. It also depends how you watermark, right?

If you have just a line of inference code that says, "slap a watermark on this thing", it could delete the line of inference code. If you're to train the system on only watermarked images, well now you have to retrain the entire system to get it to do something else, which is very expensive. So again, I think it depends how you do it.

I was at a meeting last week where people were talking about, are there ways we could build in a mechanism into the chips that run the systems that say "if some bit of code is removed or changed in the system, then the chip burns up and won't run the system". I'm not saying this is impossible, but [a] really interesting technical question, really difficult, definitely beyond my area of expertise. But I think if this is an approach we can take and say, there are ways to be able to open source the system and get all the benefits of open-sourcing by just open-sourcing and still mitigate the risks, I think that's great. I think it's just a lot more difficult.

And there's one aspect in which we do take the flip view in the report, and I think this is where we start talking about a staged release of models. You can undergo a staged release of a model where you put out a slightly smaller version of a model behind API, you study how it's being used. Maybe you take a pause, analyze how it was used, what [are] the most common avenues of attack, if at all, [that] were being used to try and misuse the model.

And then you release a slightly larger model, [then] a slightly larger model. You do this iteratively, and if you do this process, as you get to a stage where it's like, hey, we've been doing the staged release of this model for however many months and no problems, looking good, there's no emergent capabilities that popped up that are making you worried. You didn't have to implement a bunch of safety restrictions to get people to stop doing unsafe things - okay, open source. This is not a binary [of] it has to be completely open or completely closed. And I think this is one respect... If you were to take this flip view of "how can we open source, but do it in the safest way possible?" Just open source slowly, take some time to actually study the impacts. And it's not like the only way to get a sense of how the system's going to be used is to just open source it and see what happens. You could do a staged release and study what those impacts are.

Again, it won't be perfect. You never know how it's going to be used 10 years down the road once someone gets access to all the weights and stuff. But it is possible to study and get some sort of insight. And I think one of the nice things about staged release is if you start doing the staged release process and you realize that at each iterative step you are having to put in a bunch of safety filters, for example, to prevent people from doing really shady stuff, that's probably a good indication that it's not ready to be open sourced in its current form, because those are safety filters that will just immediately be reversed once open sourced. So I think you can learn a lot from that.

So I think that's one way you can open source safely: find ways to actually study what the effects are before you open source, because that decision to open source is irreversible. And then I think the technical challenge of, are there ways we can have backstops, that we can technically build in irreversible, irremovable filters or watermarks or even just hardware challenges that we could implement - I think [those are] really interesting technical questions that I don't know enough about, but... Go for it. That'd be a great world.

**Daniel Filan:**
Yeah. If listeners are interested, this gets into some territory that [I talked about with Scott Aaronson earlier this year](https://axrp.net/episode/2023/04/11/episode-20-reform-ai-alignment-scott-aaronson.html#watermarking-lm-outputs). Yeah, I think the classic difficulties, at least say for watermarking... I read [one paper](https://arxiv.org/abs/2012.08726) that claims to be able to bake the watermark into the weights of the model. To be honest, I didn't actually understand how that works.

**Elizabeth Seger:**
I think it has to do with how the model's trained. So the way I understand it is if you have a dataset of images that all have a watermark in that dataset, not watermark in the sense like you see on a $1 bill, but weird pixel stuff that the human eye can't see. If all the images in the training dataset have that watermark, then all of the images it produces will have that watermark. In that case, it's baked into the system because of how it was trained. So the only way to get rid of that watermark would be to retrain the system on images that don't contain the watermark.

**Daniel Filan:**
Yeah, that's one possibility. So that's going to be a lot rougher for applying to text models, of course, if you want to just train on the whole internet. I think I saw [something](https://arxiv.org/abs/2012.08726) that claimed to work even on cases where the dataset did not all have the watermark, but I didn't really understand how it worked. But at any rate, the key issue with these sort of watermarking methods is as long as there's one model that can basically paraphrase that does not have watermarking, then you can just take your watermark thing and basically launder it and get something that - if your paraphrasing model is good enough, you can create something that looks basically similar, it doesn't have the watermark, and then it's sad news. Yeah. And then-

**Elizabeth Seger:**
Sorry, I was going to say there's similar [things] in terms of how doing something with one model allows you to jailbreak another model. I mean this is what happened with the ['Adversarial suffixes' paper](https://arxiv.org/abs/2307.15043), where using a couple open source models, one which was Llama 2, and using the weights of those models, figuring out a way to basically just throw a seemingly random string of numbers at a large language model, and then with that seemingly random range of numbers before the prompt basically get the system to do whatever you want. Except while they figured out how to do that using the weights accessible from Llama 2, it worked on all the other large language models. So finding a way to jailbreak one model and using the weights and access to one model, that could bring up vulnerabilities in tons of others that aren't open sourced as well. So I think that's another roughly related somewhat to what we were just talking about point.

**Daniel Filan:**
Yeah, I guess it brings up this high level thing of whatever governance method for AI you want, you want it to be robust to some small fraction of things breaking the rules. You don't want the small fraction to poison the rest of the thing, which watermarking unfortunately has.

Yeah, I guess I wanted to say something brief about backdoors as well. So there is really a way of, at least in toy neural networks, and you can probably extend it to bigger neural networks, [you really can introduce a backdoor that is cryptographically hard to detect](https://arxiv.org/abs/2204.06974). So one problem is, how do you actually use this to prevent AI harm is not totally obvious. And then there's another issue of... I guess the second issue only comes up with super smart AI, but if you have a file on your computer that's like, "I implanted a backdoor in this model, the backdoor is this input", then it's no longer cryptographically hard to find as long as somebody can break into your computer. Which hopefully is cryptographically hard, but I guess there are security vulnerabilities there.

So yeah, I wonder if you want to say a little bit about the safer ways to get the open source benefits. I've given you a chance to talk about them a little bit, but is there anything more you want to say about those?

**Elizabeth Seger:**
I think, not really. I think the overarching point is, just as I said before, when the risks are high - and I think that's key to remember, I'm not saying don't open-source everything - when the risks are high, it is worth investing in seeing how else we can achieve the benefits of open-sourcing. Basically, if you're not going to open-source because the risks are high, then look into these other options. It's really about getting rid of this open versus closed dichotomy.

So many of the other options have to do with other options for sharing models, whether that's structured access behind API, even research API access, gated download, staged release, and then also more proactive efforts. Proactive efforts which can actually also be combined with open-sourcing. They don't have to be seen as an alternative to open-sourcing. So this is things like redistributing profits towards AI safety research or starting AI safety and bug bounty programs. Or even like we talked about with [the democratization paper](https://arxiv.org/abs/2303.12642), thinking about how we can democratize decision-making around AI systems to help distribute influence over AI away from large labs, which is another argument for open-sourcing.

So yeah, I think that this is key: there are other efforts that can be put in place to achieve many of the same benefits of open-sourcing and when the risks are high, it's worth really looking into these.

## AI governance research <a name="ai-gov"></a>

**Daniel Filan:**
All right. Okay. So moving on, I want to talk a little bit more broadly about the field of AI governance research. So historically, this podcast is mostly focused on technical AI alignment research, and I imagine most listeners are more familiar with the technical side than with governance efforts.

**Elizabeth Seger:**
In which case, I apologize for all my technical inaccuracies. One of the benefits of having 25 co-authors is that a lot of the technical questions I got to outsource.

### The state of the field <a name="state-of-field"></a>

**Daniel Filan:**
Makes sense. Yeah, it's good to be interdisciplinary. So this is kind of a broad question, but how is AI governance going? What's the state of the field, if you can answer that briefly?

**Elizabeth Seger:**
The state of the field of AI governance. Yeah. Okay. I'll try and answer that briefly. It's going well in that people are paying attention. In this respect, the release of ChatGPT I think was really great for AI governance because people, besides those of us already doing AI governance research, are really starting to see this as something valuable and important that needs to be talked about and [asking] questions around what role should governments play in regulating AI, if at all? How do we get this balance between governments and the developers? Who should be regulated with respect to different things? Do all of the responsibilities lie on the developers or is it on the deployers?

And all of these questions suddenly are coming to light and there's more general interest in them. And so we're seeing things like, the [UK AI Summit](https://en.wikipedia.org/wiki/2023_AI_Safety_Summit) is happening next week, [a] global AI summit, looking at AI safety, really concerned about catastrophic and existential risks, trying to understand what kind of global institutions should be in place to govern AI systems, to evaluate AI systems, to audit, to regulate.

And this is bringing in countries from all over the world. I think it's something like 28 different countries are going to be at the UK AI Summit. You have [the EU AI Act](https://artificialintelligenceact.eu/) where it started a while ago looking at narrow AI systems, but now is taking on foundation models and frontier AI systems and looking at open source regulation. And this has really, over the last year, exploded into a global conversation.

So in that respect, AI governance is going well in that people are paying attention. It's also very high stress because suddenly everyone's paying attention. We have to do something. But I think there's really genuine interest in getting this right, and I think that really bodes well. So I'm excited to see where this next year goes. Yeah, there's talk about having this global AI summit and then making this a recurring series. And so I think it's going well in the sense that people are paying attention and the wheels are starting to turn, and that's cool.

### Open questions <a name="open-qs"></a>

**Daniel Filan:**
Okay. I guess related to that, what do you see as the most important open questions in the field?

**Elizabeth Seger:**
In the field of AI governance? Okay. So I think one big one is compute governance, which my colleague, [Lennart Heim](https://heim.xyz/about/), [works on](https://www.governance.ai/team/lennart-heim). This is just thinking about how compute is a lever for trying to regulate who is able to develop large models, even how compute should be distributed so that more people can distribute large models, but basically using compute as a lever to understand who has access to and who is able to develop different kinds of systems. So I think that's a huge area of research with a lot of growing interest because compute's one of the tangible things that we can actually control the flow of.

I think that the questions around model-sharing and open-sourcing are getting a lot of attention right now. Big open question, a lot of debate, like I said, it's becoming really quite a polarized discussion, so it's getting quite hard to cut through. But a lot of good groups [are] working on this, and I think a lot of interest in genuinely finding common ground to start working on this. I've had a couple situations where I've been in groups or workshops where we get people who are very pro-open source and other people who are just like, no, let's just shut down the whole AI system right now, really both sides of the spectrum coming together. And we try and find a middle ground on, okay, where do we agree? Is there a point where we agree? And very often we can come to a point of agreement around the idea that there may be some AI system, some model that poses risks that are too extreme for that model to be responsibly open sourced.

And that might not sound like that extreme of a statement, but when you have people coming from such polarized views to agree on the fact that there may exist a model one day that should not be open source, that is a starting point and you can start the conversation from there. And every group I've been in so far has got to that point, and we can start working on that. So I think this model-sharing question is a big open question and lots of technical research needs to be done around benchmarking to decide, when are capabilities too dangerous?

Also around understanding what activities are actually possible given access to different combinations of model components. And that's actually not entirely clear, and we need a much more fine-grained understanding of what you can actually do given different kinds of model, combinations of model components, in order not only to have safe standards for model release and really a fine-grained standard for model release, but also to protect the benefits of open-sourcing. You don't want to just have a blanket "don't release anything" if you can get a lot of good benefit out of releasing certain model components. So I think a lot of technical research has to go into this.

Anyway. So yeah, second point, I think model-sharing is a really big point of discussion right now. And then with the upcoming [UK AI Summit](https://en.wikipedia.org/wiki/2023_AI_Safety_Summit), [there's] quite a bit of discourse around what international governance structures should look like for AI, a lot of different proposed models. And yeah, it'll be interesting to see what comes out of the summit. I don't think they're going to agree on anything amazing at the summit. It's two days. But I think for me, a really great outcome of the summit would be, first, recognition from everyone that AI systems could pose really extreme risks. So just a recognition of the risks. And then second, a plan going forward, a plan for how we can start establishing international systems of governance and really structure out when are we going to come to what kinds of decisions and how can we start putting something together. So I think that those are probably three key open questions, and the international governance structure one is really big right now too, just given the upcoming summit.

**Daniel Filan:**
And I guess unless we get the editing and transcription for this episode done unusually quickly, listeners, the [UK AI Summit](https://en.wikipedia.org/wiki/2023_AI_Safety_Summit) is almost definitely going to be in your past. So I guess listeners are in this interesting position of knowing how that all panned out in a way that we don't. So that was open questions in the field broadly. I'm wondering for you personally as a researcher, what things are you most interested in looking at next?

**Elizabeth Seger:**
Interesting. I mean, most of my life is taken up with follow-up on this open source report right now. So I definitely want to keep looking into questions around model-sharing and maybe setting responsible scaling policy, responsible model release policy. I'm not exactly sure. I think I'm in this place right now where I'm trying to feel out where the most important work needs to be done and whether the best place for me to do is to encourage other people to do certain kinds of work where I don't necessarily have the expertise, like we were talking about, like needing more technical research into what is possible given access to different combinations of model components, or are there specific areas of research I could try to help lead in? Or whether really what needs to be done is just more organizational capacity around these issues.

So no, I am personally interested in keeping up with this model-sharing discussion. I think there's a lot of interesting work that needs to be done here, and it's a key thing that's being considered within the discussions around international AI governance right now. Yeah, so sorry I don't have as much of a clear cut answer there, but yeah, I'm still reeling from having published this report and then everything that's coming off the back of it and just trying to feel out where's the next most important, most impactful step, what work needs to be done. So I guess if any of your listeners have really hot takes on "oh, this is what you should do next", I guess, please tell me. It'll be very helpful.

**Daniel Filan:**
How should they tell you if someone's just heard that and they're like, oh, I need to-

**Elizabeth Seger:**
"I need to tell her now! She must know!" Yeah, so I mean, I have a [website](https://elizabethseger.com/) where you could find a lot of my contact information, or you can always find me on [LinkedIn](https://uk.linkedin.com/in/elizabeth-seger-ph-d-5209797b). I spend far too much time on LinkedIn these days. And also my email address happens to be on the open source report. So if you download the report, my email address is there.

**Daniel Filan:**
What's the URL of your website?

**Elizabeth Seger:**
[ElizabethSeger.com](https://elizabethseger.com/).

### Distinctive governance issues of x-risk <a name="xrisk-different"></a>

**Daniel Filan:**
All right. Okay. Getting back to talking about governance in general, I'm wondering... so I guess this is an x-risk-focused podcast. How, if at all, do you think governance research looks different when it's driven by concerns about x-risk mitigation versus other concerns you could have about AI governance?

**Elizabeth Seger:**
Well, that's a good question. Let's see. So the question is how does the governance research look different?

**Daniel Filan:**
Yeah. What kinds of different questions might you focus on, or what kinds of different focuses would you have that would be driven by x-risk worries rather than by other things?

**Elizabeth Seger:**
So this is something that I've had to think about a lot in my own research development because I did not come into this area of research from an x-risk background interest. I came into it... I mean, honestly, I started in bioethics and then moved from bioethics looking at AI systems in healthcare and have sort of moved over into the AI governance space over a very long PhD program. And so here I am.

But I would say one of the things that I've learned working in the space [that's] more interested in long-term x-risk impacts of AI and trying to prevent x-risks is really paying attention to causal pathways and really trying to be very critical about how likely a potential pathway is to actually lead to a risk. I don't know if I'm explaining this very well.

Maybe a better way of saying it's like: if you have a hypothesis, or let's say you're worried about the impacts of AI systems on influence operations or impacting political campaigns, I find it really helpful to start from the hypothesis of, it won't have an impact. And really just trying to understand how that might be wrong, as opposed to trying to start from "oh, AI is going to pose a massive bio threat, or it's going to pose a massive threat to political operations" or something like that. And then almost trying to prove that conclusion.

Yeah, I don't know, I start from the opposite point and then try and think about all the ways in which I could be wrong. And I think this is really important to do, especially when you're doing x-risk research, whether it's with respect to AI or some other form of x-risk. Because I think there are a lot of people that turn off when you start talking about existential risks, they think it's too far out there, it's not really relevant to the important questions that are impacting people today, the tangible things that people are already suffering. And so I think it's really important to be very, very rigorous in your evaluations and have a very clear story of impact for why it is that you're doing the research you're doing and focusing on the issues that you're doing. At least that's been my experience, trying to transition into the space and work on these issues.

### Technical research to help governance <a name="tech-for-gov"></a>

**Daniel Filan:**
Another question I have, related to my audience... So I think my audience, a lot of them are technical alignment researchers and there are various things they could do, and maybe they're interested in, okay, what work could technical alignment people do that would make AI governance better? I'm wondering if you have thoughts on that question.

**Elizabeth Seger:**
Okay. Technical alignment people, AI governance better. Yeah, I mean there's a lot of work going on right now, especially within the UK government. We just set up the [UK AI Task Force](https://www.gov.uk/government/publications/frontier-ai-taskforce-first-progress-report/frontier-ai-taskforce-first-progress-report), a government institution doing a lot of model evals and alignment research. I think if you have the technical background in alignment research, you are very much needed in the governance space. There's very often a disconnect between... I mean, I am also guilty of this. There's a disconnect between the people doing the governance research and the people who have the experience with the technology and really know the ins and outs of the technology that's being developed.

And I think if you have the inclination to work in an AI governance space and help bridge that gap, that would be incredibly valuable. And like I've already said, some of the more technical questions, even around open-sourcing, are things that I was very, very glad to have colleagues and co-authors on the paper who have worked for AI labs and stuff before and really knew what they were talking about and could advise and help write some of the more technical aspects of the report.

So I think if you have the inclination to work in the space, to get involved with governance efforts, or even maybe some of these government institutions that are starting to pop up that are working on the boundary of AI governance and technical research, that could be a really valuable place to contribute. So I think my 2 cents off the top of my brain would be help bridge that gap.

**Daniel Filan:**
Okay. Great. So before we wrap up, I'm wondering if there's anything that you wish I'd asked but that I didn't?

**Elizabeth Seger:**
Oh, that's a good question. No, I don't think so. I think we've covered a lot of good stuff. Yeah, thank you for having me on really. I'd say there's nothing in particular. This has been great.

## Following Elizabeth's research <a name="following-elizabeths-research"></a>

**Daniel Filan:**
All right, so to wrap up then, if people are interested in following your research, following up on this podcast, how should they do that?

**Elizabeth Seger:**
So I have my website, [ElizabethSeger.com](https://elizabethseger.com/). It sort of outlines my different ongoing research projects, has a lot of publications on it. Also, [GovAI's website](https://www.governance.ai/) is a wealth of information [on] all things AI governance from all my great colleagues at GovAI and our affiliates. So really, yeah, there's new research reports being put out almost every week, maybe every other week, but really high quality stuff. So you can find a lot of my work on the GovAI website or my current work and past work on my own website or find me on [LinkedIn](https://uk.linkedin.com/in/elizabeth-seger-ph-d-5209797b). Yeah, just happy to talk more.

**Daniel Filan:**
All right, well thank you very much for being on the podcast.

**Elizabeth Seger:**
Great, thank you.

**Daniel Filan:**
This episode is edited by Jack Garrett and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future) and [Lightspeed Grants](https://lightspeedgrants.org/), along with [patrons](https://patreon.com/axrpodcast) such as Tor Barstad, Alexey Malafeev, and Ben Weinstein-Raun. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
