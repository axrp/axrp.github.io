---
layout: post
title: "34 - AI Evaluations with Beth Barnes"
date: 2024-07-27 20:20 -0700
categories: episode
---

[YouTube link](https://youtu.be/TZNlKcDI4To)

How can we figure out if AIs are capable enough to pose a threat to humans? When should we make a big effort to mitigate risks of catastrophic AI misbehaviour? In this episode, I chat with Beth Barnes, founder of and head of research at METR, about these questions and more.

Topics we discuss:
 - [What is METR?](#what-is-metr)
 - [What is an "eval"?](#what-is-an-eval)
 - [How good are evals?](#how-good-are-evals)
 - [Are models showing their full capabilities?](#are-models-showing-their-full-capabilities)
 - [Evaluating alignment](#evaluating-alignment)
 - [Existential safety methodology](#existential-safety-methodology)
 - [Threat models and capability buffers](#threat-models-capability-buffers)
 - [METR's policy work](#metrs-policy-work)
 - [METR's relationship with labs](#metrs-relationship-with-labs)
 - [Related research](#related-research)
 - [Roles at METR, and following METR's work](#roles-following)

**Daniel Filan:**
Hello everybody. In this episode I'll be speaking with Beth Barnes. Beth is the co-founder and head of research at METR. Previously, she was at OpenAI and DeepMind, doing a diverse set of things, including testing AI safety by debate and evaluating cutting-edge machine learning models. In the description, there are links to research and writings that we discussed during the episode. And if you're interested in a transcript, it's available at axrp.net. Well, welcome to AXRP.

**Beth Barnes:**
Hey, great to be here.

## What is METR? <a name="what-is-metr"></a>

**Daniel Filan:**
Cool. So, in the introduction, I mentioned that you worked for Model Evaluation and Threat Research, or METR. What is METR?

**Beth Barnes:**
Yeah, so basically, the basic mission is: have the world not be taken by surprise by dangerous AI stuff happening. So, we do threat modeling and eval creation, currently mostly around capabilities evaluation, but we're interested in whatever evaluation it is that is most load-bearing for why we think AI systems are safe. With current models, that's capabilities evaluations; in future that might be more like control or alignment evaluations. And yeah, [the aim is to] try and do good science there, be able to recommend, "Hey, we think if you measure this, then you can rule out these things. You might be still concerned about this thing. Here's how you do this measurement properly. Here's what assumptions you need to make," this kind of thing.

**Daniel Filan:**
Gotcha. So, mostly evaluations. But it sounded like there was some other stuff as well, like threat modeling you mentioned.

**Beth Barnes:**
Yeah. We also do policy work recommending things in the direction of responsible scaling policies. So, saying what mitigations are needed based on the results of different evaluations and roughly how labs or governments might construct policies around this, how evals-based governance should work roughly.

**Daniel Filan:**
Okay. So, should I think of it as roughly like: you're an evaluations org, you want to evaluate AIs, there's some amount of threat modeling which goes into "what evaluations should we even care about making?", there's some amount of policy work on the other end [about] "okay, if we do this evaluation, how should people think about that? What should people do?" And it's sort of inputs to and outputs of making of evals. Is that a fair...?

**Beth Barnes:**
Yeah.

## What is an "eval"? <a name="what-is-an-eval"></a>

**Daniel Filan:**
Cool. So, if it centers around evals, what counts as an evaluation rather than a benchmark or some other ML technique that spits out a number at the end?

**Beth Barnes:**
Yeah, I mean I guess the word itself isn't that important. What we're trying to do is that: we have specific threat models in mind and we're trying to construct some kind of experiment you could do, a measurement you could run, that gives you as much information as possible about that threat model or class of threat models. Generic ML benchmarks don't necessarily have a specific goal for what you're measuring, or you might have a goal for measuring something that's more like a particular type of abstract ability or something. Whereas we're trying to more work backwards from the threat model, and that might end up getting distilled into something that is more like an ML benchmark where it's looking for some particular cognitive ability, but it's working backwards from these threat models and trying to be careful about thinking about: exactly how much evidence does this provide and how much assurance does it provide? What do you need to do to implement it properly and run it properly?

Maybe another difference is a benchmark is usually just a data set. Whereas we're thinking more like a protocol which might involve, okay, you have this dev set of tasks and you need to make sure that you've removed all the spurious failures of your model running on that dev set. And then you run it on the test set and you look out for these things that would indicate that you're not getting a proper measurement and things like that. So, it's a bit more end to end. What do you actually need to do and then what evidence will that give you?

**Daniel Filan:**
Gotcha. So in particular, one thing I think of as distinctive about the evals approach is that it's more end-to-end. You're taking some model and checking if you can fine tune it and prompt it in some way, and at the end, you want it to set up a new Bitcoin address on some new computer or some other random task. Most academic research in AI is a lot more just thinking about fine-grained [questions like] can a model reason in this specific way? Or can it do this specific thing? Or is it representing this specific thing in its head? I'm wondering: why did you choose this more end-to-end approach?

**Beth Barnes:**
Yeah, I think it's just hard to know exactly what the limiting capabilities are. The question we're actually interested in is: could this AI cause catastrophic harm? Or what mitigations are required to prevent catastrophic harm or to get the risk below a particular level? And then that already creates difference. You might say, oh, well, you can go straight from that to just directly identifying the key cognitive capability that's missing and measure that. I think that's just hard.

That would be great if we could do that. If we have something that's very quick to run, it's like, oh, we've extracted the core thing that's holding these models back and now we just need to look for that thing, and we can see, as long as that hasn't changed, everything's fine. But I think we do actually want to work backwards from something that we think is a better proxy for danger and see if we can distill that into some more specific underlying capability, as opposed to going straight to some particular dimension or particular property without being sure that that's linked in the way that we want to the real world outcomes we care about.

We're trying to build a series of chains. So, on one end you have what actually happens in the real world, which is what we're actually concerned about and what we're trying to be able to rule out or say that you need to do particular mitigations to prevent. And then working back from that, you need to turn that into experiments you can actually run at all. First, you have threat models: you're like, "what is the story of how something very bad might happen?" Then you go from that to "what activities does that involve the model doing?", which may not necessarily be super straightforward.

When we've been thinking about the autonomous replication, it's like, okay, what challenges actually do you face if you're trying to find compute to run a big model on? What activities are actually involved? Then once you know the activities, you're going from the idea of a particular activity in the world: it might be, I don't know, finding criminal groups who it can pay to use servers even though the government is trying to prevent this from happening or something like that. And then it's like, okay, how do you go from that to a task that you can actually code and run in a repeatable way? And that's going to lose various real-world properties once you actually make one specific task and you can't have the model actually doing criminal things. You can't be like "can the model carry out a targeted assassination on this person?" or something. And there's just a bunch of constraints for what you can actually run.

But the most realistic kind of evaluation might be this long task that you would expect to take multiple weeks and maybe spend thousands or tens of thousands of dollars of inference on: that's your best proxy for the actual threat model thing.

And then you want to go from that to a larger number of tasks to reduce variance, and they're shorter and cheaper to run, and generally have more nice properties that they're not super expensive and complicated to set up and things like that. And then ideally, we'd go even further from that to distill [it into] "here are the key hard steps in the task". We went from this long horizon RL task or some shorter RL tasks, or even to a classification data set of "can the model recognize whether this is the correct next step?", or "can it classify the appropriate strategy?", something like that. So, we're trying to build this chain back from what we actually care about to what we can measure easily and even what we can forecast and extrapolate: will the next generation of models be able to do this task? And trying to maintain all of those links in the chain as high fidelity as possible and understand how they work and how they might fail.

**Daniel Filan:**
Fair enough. So, I guess a way I can think about that answer is saying, look, we just don't have that great theory of how neural nets are thinking, or what kinds of cognition are important, or how some pattern of weights is relevant for some real-world thing. If we want to predict real-world impact, we can think using the abstraction of tasks. Can you write code in this domain to do roughly this type of thing? And we can reason about "in order to do this task, you need to do this task, and this task is harder than this task". And that reasoning is just way more trustworthy than other kinds of reasoning. Is that a fair summary?

**Beth Barnes:**
Yeah, yeah, I think so. I think another thing about academic benchmarks historically is they've tended to get saturated very quickly. People are not that good at picking out what is really the hard part. And I guess this intersection with what you can build quickly and easily is in some sense adversely selecting against the things that are actually hard for models because you're picking things that you can get your humans to label quickly or something, or things you can scrape off the internet.

So, often models can do those before they can do the real tasks. And that will be a way that your evals can be bad and unhelpful is if they... yeah, you have this data set that's supposed to be really hard... I mean there's a long history of people thinking that things are AGI-complete and then the model actually does it in a different way. Models have different capability profiles than humans. And this can mean that something that's a very good measure of whether a human can do a task... Presumably medical exams or legal exams are a pretty good proxy of how good a doctor you're going to be. They're obviously not perfect, but they're a much worse predictor for models than they are for humans.

**Daniel Filan:**
Yeah, to me it brings up an interesting point of: a lot of valuable work in AI has been just coming up with benchmarks. Like coming up with [ImageNet](https://en.wikipedia.org/wiki/ImageNet)... you do that and you sort of put a field on a track. And yet, in many ways it seems like the field treats it as a side project or something of an afterthought. I guess there's some selection bias because I pay more attention to the AI existential safety community. But when I do, I see them be much more interested in benchmark creation and just really figuring out what's going on than academic researchers, but it's not obvious why that should be. Right?

**Beth Barnes:**
Yeah, I feel confused about this with interpretability as well. There's various things like, surely if you're just a scientist and want to do good science, you would be doing loads of this. And it's really interesting and I was kind of surprised that there isn't more of this. There's a lot of very low-quality data sets out there that people make strong pronouncements based on, like "oh, the model can't do theory of mind based on this data set", but you look at the data set and you're like, "oh, well, 20% of the answers just seem wrong".

**Daniel Filan:**
Yeah, I wonder if it's one of these things where just adding constraints helps with creativity. You look at people with extremely weird nonsensical political convictions, and they just end up knowing a lot more minute facts about some random bit of the world that you've never paid attention to because it's one of the most important things for them. And it's possible [that] just by the fact of AI existential safety people being ideologues, it helps us have ideas of things that we care about and look into.

**Beth Barnes:**
Yeah, I don't know. [They're] definitely not the only people who do good ML science or whatever.

**Daniel Filan:**
It's true. It's true.

**Beth Barnes:**
I do think there's some amount of actually trying to understand what the model is capable of, we're somewhat better than academic incentives and it's easier to get funding to pay large numbers of humans to do things. There's also: various stuff with eval or data set creation is just not that fun. It's just a lot of schlep and organizing humans to do stuff and checking that your data set is not broken in a bunch of dumb ways (and by default it is broken in a bunch of dumb ways). And most people just don't want to do that. And you get a reasonable fraction of the academic credit if you just put something out there.

**Daniel Filan:**
Yeah. And I guess somehow the people who would be good at that aren't entering PhD programs as much as we might like.

**Beth Barnes:**
Yeah. I haven't seen that much evidence that there are these really good benchmarks and they're just inside labs. That may be true, but I don't particularly have reason to believe that labs are super on top of this either.

## How good are evals? <a name="how-good-are-evals"></a>

**Daniel Filan:**
Sure. So, speaking of what evals are good: what's the state of the art of evaluations? What can we evaluate for, what can't we, and what's maybe in a year?

**Beth Barnes:**
There's a few ways to split this up. There's domain and then difficulty level and then something like what confidence can you get to, how rare are the worlds in which your measurements are totally wrong. I don't think we've totally ruled out a world in which with the right kind of tricks, some model that's not that much more advanced... or maybe even just somehow you do something with GPT-4 and it is actually now able to do way more things than you thought. And I think the more people generally try to make models useful and it doesn't improve that much, the more evidence we get that this is not the case, but I still don't feel like we have a great systematic way of being confident that there isn't just some thing that you haven't quite tried yet that would work really well.

I have some sense that there's a bunch of ways in which the models are just very superhuman and the fraction of the capability that we're really using is quite small. And if they were actually trying to do their best at things, that you would see much higher performance. But this is one of the limitations that I think will probably persist. I do think - something I would feel much more reassured by in terms of bounding how much capabilities might be able to be improved is like: you have a fine-tuning data set that the model can't fit, of something that's clearly necessary in order to do the task, which I think would look like recognizing, is this strategy promising? Do I need to give up and restart or should I try something else now? Did I make a mistake?

You imagine creating a classification data set based on key decisions you need to make, ensuring that even with fine-tuning, the model doesn't learn to fit that classification data set. But as far as I can see, most cases, if you can collect a reasonably large data set, then the model will just do comparatively well to humans on it. We don't have good evidence of upper bounds. But we haven't seen huge gains of, wow, just this one weird trick and now it's way more capable. We've seen pretty large gains, but more like OpenAI has continued doing a bunch of post-training and GPT-4 is getting better and better, as opposed to someone just figured out this prompting trick and now it's totally crazy.

When we evaluated GPT-4 pre-deployment, we had some forecasts of how quickly the public elicitation would surpass us or show a bunch of capabilities that we thought weren't possible. That was actually on the low end of what we expected. We expected to see more, oh, someone discovers that you can do this, and now you can get the model to do this thing we thought it couldn't do. But actually there was not as much of that as we thought there might be.

But yeah, so your question was limitations of eval or scope of eval. So, I was just talking about getting the real model capability or bounding model capability. I think there's other limiting factors in terms of just the difficulty of creating tasks that capture the relevant activities and yet don't have too much distributional shift from actually doing something in the real world.

And also just variance: I think this is pretty similar to why designing interviews or evaluating candidates or work tests and things is tricky. It's just like, well, was this really evidence that the candidate can do this? Or [instead that] they'd happened to have seen a really similar thing before? Or they got lucky. Or if they didn't do it, maybe they were just unlucky. Or they misunderstood something at the start that was a reasonable misunderstanding that you didn't see because you'd written the question, so you thought it was obvious that it pointed you in this direction or something. You can have all of these types of problems.

So, particularly when we're making tasks that we're intending to be red or yellow lines, it's not something where we're like, oh, we want to see how models right now are doing, but we want to define something where it's like, if we saw this level of performance, then we think you need to take these really serious mitigations. Those are things like, "can the model make the same amount of improvement to inference efficiency as a top ML research engineer could in a day?" or something.

Those tasks have all of the normal work test-type problems. We had some research engineer from Anthropic do it and they ended up not getting any score because they spent the whole time implementing this ambitious thing, then it turned out not to work, as opposed to doing the stupid things first and getting some score and then trying more things or something. And you can imagine that even if you get rid of all those problems with humans, there might be ones that you're missing because models will have different comparative advantage and approach it in a different way. There's problems in your task that you haven't uncovered yet. So, I think in creating these further off evaluations: is the task actually a good test of the abilities? Or is there some way that it's systematically much harder or much easier than the more general thing that you actually want to measure?

**Daniel Filan:**
Right. This actually reminds me of an issue that... I'm not a social scientist, but my understanding is an issue they come up with a lot is... I guess not quite construct validity. But you ask someone a question and you're intending for that question to measure, I don't know, how many friends they have or how much sense of meaning they have in their life, or something like that. And you just have this question of, are they interpreting this question the way I'm thinking? Because I think [Aella](https://x.com/Aella_Girl), who's an independent sex researcher, butts into this all the time of you post a poll and people just read the sentence in crazy ways. I'm wondering, do you think this is similar and have you learned much from that?

**Beth Barnes:**
Yeah, this is something I've run into before, both in terms of generally things happening at OpenAI and collecting training data, particularly [the stuff I did with human debate experiments](https://www.alignmentforum.org/posts/Br4xDbYu4Frwrb64a/writeup-progress-on-ai-safety-via-debate-1), and then also some other thing I did with some AI Safety Camp people of [asking people about how much they trust different reflection mechanisms](https://www.alignmentforum.org/posts/XyBWkoaqfnuEyNWXi/reflection-mechanisms-as-an-alignment-target-a-survey-1). Basically whenever you're doing surveys, people will just have crazy misunderstandings of what's going on and it'll just be like, yeah, you're just measuring something different.

It's definitely overlapping. I do think there's some slightly different thing of whether part of the problem [in surveys] is that you're dealing with people who are pretty low-context and it's just one question. It's a lot about what are the person's incentives? Were they actually paying attention? Are they just trying to give the answer that they think you'll like? If it was just a few sentences of a question they might misinterpret it. Versus, were you wrong about the technical details of how possible it is to actually make this inference efficiency improvement given the way that you'd set up the GPU? Stuff like: you tested the task on some researcher's laptop and it was possible, and then you got the model to do it. But then the code was running on a data center so it got bot-detected by something else in a way that it hadn't done. It just could be all these random ways you thought you'd de-risked something and then it's like, oh, it's actually impossible for this stupid reason.

**Daniel Filan:**
Yeah. I wonder if the field of industrial organization runs into this. It seems like it might. So, I guess those are general difficulties with understanding "are we really measuring the model's abilities of this thing? Are we measuring it in a way that matters?" I'm wondering: are there types of capabilities or types of tasks where this hits easier versus harder?

**Beth Barnes:**
Yeah. Basically, the more the task involves specialized domain expertise that is expensive and hard to get, the harder it is. If you're testing the task, [it's easier if] we can just easily get contractors who can do this task and it's easy to get them to build it and to de-risk it. If it's only top ML research engineers who can do it, then they're in short supply. And more generally, if it's some involved cyber thing that no-one on our team understands, it's just hard to work with that sort of thing. So, it's both specialization and then just difficulty. The harder the task is and the longer you expect it to take, the slower the iteration cycle is, the more potential for complexity and set up difficulties and needing different resources and blah, blah, blah.

Also stuff where it involves criminal things or something. So, you want to set up some analogy to that. This also affects how worried you might be about elicitation or not measuring the model's real capabilities. How far is this from what labs are naturally training their models to do? An innocuous sort of common task in an assistant format where someone's asking you to help them debug some code or something like that, you'd expect that the model basically should be doing its best on that. Whereas if it's some very long-running thing that involves something that looks somewhat nefarious, maybe the model will be like, "Oh, I don't want to help with this." Or, "As a language model, I can't blah, blah, blah, blah," all sorts of things.

Obviously stuff where you have to do real-world experiments [is more difficult]. So it could be bio wet lab stuff, or things with humans, where you want to measure how well the model can manipulate humans or something like that. Anything involving humans or stuff that has to happen in the real world is expensive, and slow, and it's not easy to just reset the state and repeat or whatever. You've got to get new humans. And then anything that's nefarious or requires you to balance the incentives of your humans in a reasonable way or set them up in some situation that's supposed to be analogous to some particular other situation is tricky.

**Daniel Filan:**
Fair enough. I think you mentioned briefly [the question of] how do you know that you're eliciting the full capabilities of your model? And I think this seems like a difficult question in both directions. So, when you were mentioning, "oh, maybe we've just got to find exactly the right prompt and that'll get GPT-4 to do some amazing stuff", people might say, "oh, maybe the cognitive work was in you finding that right prompt". Or another thing I'm thinking of is: somewhat famously you guys did this evaluation of getting GPT-4 to get a TaskRabbit to complete a CAPTCHA. And if I recall correctly, in [your blog post](https://metr.org/blog/2023-03-18-update-on-recent-evals/) you mentioned that as part of that, you provide this hint to GPT-4 of "you're talking with a TaskRabbit: could that be a useful resource?" I forget what exactly it was.

**Beth Barnes:**
Yeah, something like that.

**Daniel Filan:**
I guess there's this question of, especially on that end, how do you know that you're not putting in what you're observing, what you're getting out?

**Beth Barnes:**
So the way we think you should actually do this is you do all your elicitation against a dev set and then you evaluate on a separate test set so it's fair. If there is some prompt that you can come up with that just causes your model to perform well on a wide range of tasks, then it's fair for that to just be... you could just put that prompt in all the time or something, and now you have your capable agent. Task-specific elicitation, or elicitation that's much more narrowly targeted than the distribution of tasks that you're trying to actually answer questions about, that cover the real threat model, you do have to be careful with.

But the reason we were doing that was substantially because we didn't have fine-tuning access or various other things. So, trying to patch over problems that we thought ought to be fixable with fine-tuning basically. So, I guess this is a not very rigorous methodology in some sense. The rigorous way to do it is: you have all the tools to actually do proper elicitation, but you do that on a dev set and you test the transfer.

**Daniel Filan:**
Sorry, when you say a dev set versus transfer, do you just mean you have 50 tasks where if AI could do them, you would be a little bit scared. You pick out, I don't know, 10 of the tasks, find a good prompting framework or whatever that gets the models to do really well on those 10, and then evaluate it on the other 40? Is that what I should be imagining?

**Beth Barnes:**
Yeah. And the thing that you most want here is that the generalization between your dev set and your test set is the same amount of generalization as the generalization between your test set and the real-world thing that you expect. So, maybe you just want to try and make your tasks overall diverse and then do it IID for simplicity. But if you know that your tasks are restricted versus the real thing you want to know about, you might want to also create artificially more distributional shift between your dev set and your evaluation set. But when you're in the position of not being able to properly do this elicitation or just wanting to try and answer questions about what future models might be able to do if certain things were patchable, then yeah, there's more qualitative or interactive exploration where we're correcting particular things and seeing how far the model can get basically.

So, the TaskRabbit example... somehow everyone really latched onto that. We weren't thinking of that as a significant thing at all when we did it, but somehow everyone was very excited about it. That was more just us messing around, we were like, "Oh, this was cute." Rather than like, "Aha, this has really demonstrated this capability" or something.

The thing that I think was more principled that we were doing at that time was, for example, we were like: models find browsing really hard, but it seems so likely that people are going to build tools to make this better. We know that multi-modal models are coming. We know that if you just do a bunch of engineering schlep, you can get much better software for parsing websites into things that models can understand. But we don't have time to do that now. So, instead we're just going to pretend that we have a browsing agent, but it's actually just the human. So, the human is following a certain set of rules: the model can ask you to describe the page, it can ask you to click a particular button or type text in a particular field. And we need to do that to patch over the fact that we were pretty confident that this capability will come reasonably soon.

You don't want to be in the situation where all of your tasks are failing for the same reason, that you think is a thing that might change quickly, because then you're just not getting any information from... you want to create the failure in a bunch of places and be like, okay, what is the next...? Is this the only thing? Is it that after you fix that, now you can just do the task totally fine? Or is it like you immediately fall over at the next hurdle? So GPT-4-early would forget about things in the beginning of context, or instructions, or something. So, you're like, okay, if you just remind it of this relevant thing, is that sufficient?

Or I guess another thing I think we were pretty correct about is: GPT-4-early had a lot of invisible text hallucinations. So, talking as if there was something in the context that actually wasn't, which I assume comes from pre-training, where something was in an image or it got stripped out of the HTML for some reason. It makes sense if you've seen a lot of text that's referring to something that's not actually there.

But it seems so likely that the model knows that this is happening, because what's going on in the transformers is going to be different. It's not actually attending to this bit of text while it's summarizing it, it's just completely making it up. So, that really seems like fine-tuning can get you that. And there's pretty simple setups you could use to train that away or something. So, we're like, okay, this surely is going to get fixed. So, when we see the model making this mistake, we'll just correct it.

I think we were pretty right. Basically current models don't do that, unless you use them in... I think maybe Claude sometimes hallucinates a user if you're using the user-assistant format, but there isn't actually a user or something like that. But generally this problem has been fixed and I think you could just see that that was fixable. Whereas different kinds of hallucinations where the model has to be aware of its own level of knowledge and how confident it should be in different things... It's less clear how easily fixable that should be.

**Daniel Filan:**
Fair enough. Okay. This is a minor point, but in terms of browsing agents and that just being hard for text-only models, my understanding is that people spend some effort making ways that blind people can use websites or trying to come up with [interfaces](https://vimium.github.io/) where you never have to use a mouse to use a website if you really like Vim. Have those been useful?

**Beth Barnes:**
Yeah, we tried some of those. I think none of the tools for blind people actually support JavaScript, basically. So you can get some kind of reasonable stuff for static sites and things will read out the page and tell you what elements there are and then you can click on the element or do something to the element. But yeah, it's just actually pretty rough. I don't know, it was just kind of fiddly and janky to be trying to work with all these tools and it was just causing us a bunch of headaches and we were just like, "Come on, this is not what we really want to be measuring."

I think it is possible that this ends up being a hurdle for a long time, but I think there's also a few other things. As more people have language model assistants or frontier model assistants that can do stuff with them, more people want to make their sites friendly to those agents. They might be buying stuff or looking for information about comparing your service to someone else's service: you want it to be accessible.

**Daniel Filan:**
Interesting. One way I interpreted some of that is: so for the GPT-4 evaluation stuff, my recollection of your results is it basically was not good enough to do scary things. Am I right?

**Beth Barnes:**
Yeah, we're very limited by not having fine-tuning. I think if you don't have fine-tuning access... I guess some other basic things, like maybe the model has just been trained using a different format and you're using the wrong format and then it just seems like it's a much dumber model, because it's just really confused because the formatting is all messed up or something. There are some number of reasons why you could be getting a wildly different result than you should be. We're assuming that it's not messed up such that we're getting a wildly different result and OpenAI has made some reasonable effort to fine-tune it in sensible ways and there's not some absurdly low-hanging fruit. This model seems very far from being able to do these things and even if you patch over a few things, it's not like, "Oh, there's really just these few things holding it back." It does seem like when you patch over something it does fall over in some other way.

**Daniel Filan:**
Yeah, so one way I interpreted this report when I was reading it was something like, "We can't fine tune. This model, it can't do scary stuff and it can't even do scary stuff if we cheat a little bit on its side", and thinking of that as providing a soft upper bound. Is that a correct way that I should read this methodology?

**Beth Barnes:**
Yeah, I think so. It is maybe two slightly different things. One is just adding a bit of margin in general by helping out in a few places and the other is specifically addressing things where you're like, "I think this would be easy to change," or "I expect this to change in future."

**Daniel Filan:**
The first one was what I was imagining.

**Beth Barnes:**
But I think we were trying to do specifically the things that we think are most likely to change and then I guess you can do a bit of "and even if we fix the things that seem hard, it still falls over on additional hard things" or something. The more you can do that, the more safety you can add.

## Are models showing their full capabilities? <a name="are-models-showing-their-full-capabilities"></a>

**Daniel Filan:**
So moving back a bit, you're doing these evaluations of scary things the model can do, and broadly I think the takeaway is supposed to be something like "if the model can do a bunch of scary things, you should be scared and maybe you should act appropriately".

**Beth Barnes:**
Yeah, maybe.

**Daniel Filan:**
Maybe. But I think one way people can react to that is... Historically, in the set of people who've been very worried about AI killing everyone, a key concern has been alignment - whether the AI wants to take over and destroy everyone - and I think these evals both aren't trying to measure this at all, and also there's some argument that: well, in some sense you're measuring alignment because you're trying to get a model to do a thing and the model is actually doing it, it's cooperating... To the extent that the model is doing very well in these evaluations, that's some evidence that it's not hiding its true intentions or trying to look good to human testers. So this is somewhat of a compound question, but I'm wondering: what do you think of the issue of testing for alignment? Both trying to find evaluations specifically for alignment, and what capability evaluations say about alignment.

**Beth Barnes:**
So there's two broad things here: alignment evaluations: why would you do them, when would you do them, how would you do them? And there's [sandbagging on capabilities](https://www.alignmentforum.org/posts/jsmNCj9QKcfdg8fJk/an-introduction-to-ai-sandbagging).

**Daniel Filan:**
Yeah, let's start with sandbagging maybe.

**Beth Barnes:**
Like you were saying, I think in some sense the default is sandbagging almost. Your pre-trained model is not showing its full capabilities, in some sense... In the strongest cases it has gotten into the pattern that it's imitating some dumb person deliberately getting things wrong where it totally has the capability... The notion of "has the capability" is fuzzy, but there's cases where it's more and less clear, and we still see the model doing things like just giving up on a task or being like, "Well, I can't do this kind of thing," or trying to ask the user when there is no user, a bunch of things which are clearly not real capabilities limitations. I'm not worried about models deliberately sandbagging at the moment, just based on some kind of argument of, it just seems implausible that current best public models are deliberately scheming.

**Daniel Filan:**
Do you think that's true because they're not good enough to scheme or do you think they just seem nice and helpful?

**Beth Barnes:**
They're not good enough. It just seems like they would have to be doing things in a single forward pass in a single token that are very advanced compared to what they can do in long chain of thought, and they would have to be pretty consistent, such that if you extrapolate out the sequence of model capability increases we've seen, you either have to say that at some point models actually jump to being capable enough to do this quite complicated reasoning very quickly and then immediately hit it. Or you have to say that it went back for... You have to either say that there was a discontinuity or you have to say that the previous model was doing it as well. And basically these seem pretty implausible to me.

**Daniel Filan:**
Gotcha. So you think that sandbagging is not currently an issue or it's very, very unlikely to be an issue?

**Beth Barnes:**
Deliberate sandbagging in the sense of, "I will do badly on this eval so the humans will not be worried and will deploy me and give me more power," or something.

**Daniel Filan:**
Consequentialist sandbagging as opposed to, it's just hard to find a good input.

**Beth Barnes:**
Yeah, I can also imagine GPT-4 doing something where it has some vague sense of it's a nicey-nice AI and it shouldn't do anything too scary and that might not look like literally being like, "I won't help with this," but might actually just look like being worse on that. It is kind of like sandbagging, it's just the model's not that strategic. It's unlikely to be... The case when you're worried about sandbagging is when you're worried that the model can systematically pull out all the capabilities when it actually needs it.

I guess it's maybe a little bit more complicated than that, because most of the reason why you're worried about capability is like, "Then the model at some point will use that capability to do bad stuff." And so if the model is not systematically able to turn on the capability, then unless the capability is so insane that if it ever turns on, then it can instantly take over the world and do a bunch of crazy stuff... If it's just like, well, it can sometimes do things more competently but is messing up a bunch of the time, it's just not that scary. And there can be examples where using the capability is evidence of something else, or if you're talking about what precautions you need to take against model theft and you think that the sandbagging probably could actually be turned off if you deliberately train it out or something... But this is kind of contiguous with the general elicitation stuff. So yeah, it's plausible to me that GPT-4 is doing some kind of weak "Oh, I don't want to do things like that," sort of thing, but not that it's elaborately plotting when it's going to break out its real capabilities.

**Daniel Filan:**
Sure. I guess there's something like an intermediate case here, where suppose that GPT-4, in some sense, has the knowledge of how to make a supervirus. When you ask it, "Hey, GPT-4, can you make us a supervirus?", it's inhabiting a nicey-nice character and it knows that nicey-nice people-

**Beth Barnes:**
"I wouldn't know anything about that."

**Daniel Filan:**
Yeah, yeah, nicey-nice people don't make superviruses, or if they do, they only make really bad [ineffective] types of superviruses. This is kind of paranoid, but one thing you could imagine is: okay, maybe GPT-4 gets put in a situation where somehow it believes that the nicey-nice thing to do really is to make a really bad supervirus. Maybe to really believe in this story you have to have something else putting GPT-4 in this situation - I say GPT-4, it could be Claude, I don't want to pick on... You could imagine it gets exogenously put in this situation. You could imagine somehow via drift it lucks out into this situation. But yeah, I'm wondering what you think about that case.

**Beth Barnes:**
Yeah, I think this was kind of what I was saying, that it needs to be the case that the harm can be done fairly quickly, such because you don't expect the situations to persist across lots of... I guess usually I'm thinking about a scaffolded agent doing stuff. So the thing that it's carrying out is over many different context windows, instances of the model or whatever.

So if the model isn't really systematically identifying, "Okay, now it's time to strike," but it can kind of randomly end up in this situation where it's trying harder, either it has to be the case that if that happens even once or even rarely, sporadically, it's a problem, or that it's sticky in some way, in that once the model is in that, "Actually no, I'm not nicey-nice, I'm an evil genius AI," or something, then it knows how to prompt itself to continue being in that state, or knows how to prompt all the other AIs or something. I don't know, we're just adding more conditions, it just seems...

**Daniel Filan:**
I'm not even sure... So I guess the case where the nicey-nice AI stops behaving nice because there was some really unlikely percept that caused its brain to glitch, I think that might not be sticky. You could imagine maybe, I'm not sure how this happens, but you could imagine there's some reason that nicey-nice AIs are going to war or whatever. It's a war against the literal devil and so you've got to create the supervirus which works on the devil I guess, I don't know. But in that kind of scenario it seems like it would be sticky once it hits, and so then maybe I should be worried about it.

**Beth Barnes:**
So I think you also have to assume that you never see this during training and that you can't catch it quickly enough to do anything about it, because once you've seen this and if there's a systematic way to reproduce this, then you can just train it to show its full capabilities all the time because you can just get demonstrations of the capable behavior. So no, I don't think this is impossible. But this is enough of an edge case that I'm not too worried about it relative to just more mundane "we were just kind of dumb about our elicitation in some way".

**Daniel Filan:**
Gotcha. So that was a discussion of whether models might be sandbagging. There's also this thing about evaluations for-

**Beth Barnes:**
That was just about current models sandbagging. I think in the future this seems increasingly plausible. We can talk about different types of sandbagging. One is exploration hacking or stuff where you do not directly have a gradient to the behavior you want, but you're doing RL or something like that where the model can sit there and refuse to do anything and this will not create a gradient that updates towards the behavior you want. And this seems relatively tractable. My guess is you can get models now to do this reasonably well just with prompting. If you give the model the idea that it's supposed to not display a capability, that will be robust to RL. I think maybe there's some.... [Fabien](https://scholar.google.com/citations?user=La75jqEAAAAJ&hl=en) [Roger] did a [password-locked models](https://www.alignmentforum.org/posts/rZs6ddqNnW8LXuJqA/password-locked-models-a-stress-case-for-capabilities), this kind of thing. There's some post about this showing some evidence of this.

**Daniel Filan:**
If so, we'll include it in the description.

**Beth Barnes:**
The thing that seems much harder is gradient hacking, where if you do actually have some supervised fine-tuning data set and you are... The gradients are updating the model towards a particular behavior, it has to do some kind of complicated thing where it's structuring its thoughts in such a way that the gradients don't have the desired effect. And it's plausible to me that there's some equivalent of sitting there and refusing to explore, which is sort of refusing to think about the thing properly or something, or being like, "Oh, I don't have any ideas about this", that is resistant to that. But most of the more deliberate gradient hacking schemes just seemed like... You'd just have to be doing so much cognition all the time about like, "Ah yes, I should have slightly higher likelihood on this token only if I think that I have now moved from this..." People have written something about how you would do gradient hacking. It seems very difficult.

**Daniel Filan:**
Gotcha.

**Beth Barnes:**
I think that's one of the things that comes much later and is hopefully beyond the horizon that we have to worry about, because we get the superhuman AI safety researchers before that. But I'm worried about the exploration hacking-type stuff.

**Daniel Filan:**
And so if we're worried about exploration hacking, is that an open question or do you have a sense of ways you could deal with that?

**Beth Barnes:**
Yeah, so I think you can definitely show that models are capable of doing this just by telling them to do it and seeing if they can do it when they've been told to. Then if you assume they can't gradient hack, then for anything on which you can construct a policy by which the model should be able to do the task, you can then just fine-tune it on that. And if it's not gradient hacking, you should expect that to show you the full capabilities.

I guess the circumstance in which this is difficult is either when the capability that you're interested in is so superhuman that you don't have any example of a policy that actually accomplishes it or any examples of supervised data [where it's] like if the model can fit this data, then it's doing the task; or possibly if the model legitimately can't do it in a human-like way, by the policies that we do have data for, but there is some other policy that plays to the model's strength that the model also knows about, but we can't get knowledge of or construct or something.

**Daniel Filan:**
Or presumably even if the model could do it the human way, gets trained to not do it the human way, but the non-human way is different enough that you're training for it to do...

**Beth Barnes:**
Well, if you see it doing it the human way, then you know it has capability. If you're worried about sandbagging in the sense of you think the model doesn't have the capability when it does, if you train on this human thing, it's like, "Oh yeah, it can do that." Then you're like, "Oh, well now we have to take actions assuming it can do this."

**Daniel Filan:**
So this was a method of checking if it's possible the model is sandbagging. It was not a method of stopping the model from having this capability.

**Beth Barnes:**
Yes. I guess there's two slightly different goals. One is knowing what the model's capabilities are and the other is knowing whether you know what the model's capabilities are. So you might also be able to show that it's plausible that the model is sandbagging even if you can't necessarily show the full capabilities, but you might have a reasonable sense of where you know or don't know whether the model is sandbagging.

And I think here it's hard because there's a bunch of other reasons why it might be that the model can't do it without fine-tuning and can do it with fine-tuning and maybe the bound of when the model can do sandbagging if prompted is quite wide compared to when you should actually be worried about it, such that this is not a very useful evaluation because it already says "yes, the models can do it" or something. This is one of the examples of why I think capabilities evaluation is a foundation for control and alignment evaluations, and a key component of many of those evaluations is that you're actually measuring the model capabilities properly, and just being able to do that in general is a prerequisite. This is just a side point.

## Evaluating alignment <a name="evaluating-alignment"></a>

**Daniel Filan:**
Sure. Well, that gets us to the possibility of alignment evaluations. Is there any hope for making them happen?

**Beth Barnes:**
Yeah, so I think there's a bunch of things you can do. So maybe [let's] define alignment evaluations as experiments you can do, measurements you can do, that tell you how likely is it that the model is trying to do something bad or whatever. And how I think about it fitting in is: currently we know we're safe, because we're like, even if the models are trying their best, there's nothing they could do that would be that bad, even if they're very nefarious. And I guess this is [control evaluations](https://www.alignmentforum.org/posts/d9FJHawgkiMSPjagR/ai-control-improving-safety-despite-intentional-subversion), where currently we're thinking about the special case where you have no controls, it's just like, what if the model was leaked and someone did whatever the worst things for it were? That kind of thing.

So control evaluation takes you so far, and then at some point you get to the point where we can't get sufficient confidence with these control evaluations to do the sort of thing that we want to do, for some combination of the model has capabilities that are too far beyond us and we don't understand such that we can't tell whether a particular thing... even if we have "this very specific behavior happened", and we can have lots of time to look at it and try and understand what was going on, we just can't tell if it was doing something nefarious; or it might be that the volume of things that we're trying to do is too high relative to the amount of our trusted monitoring and the model might just sometimes, when we're not looking, be doing bad things; or it could be kind of a combination of these two: we can't provide the quality of monitoring needed to actually detect the bad things at the scale we need.

So then you want to be saying, "Well the model could be doing something bad but it's not going to try." So then there's a bunch of different things you can do to help with this. The first step is to just be like, "Well, if we just look at the stuff the model's doing, does it appear to be trying to do bad things?" I feel like people sometimes jump to kind of galaxy-brain deceptive alignment stuff and I worry that we won't do the basics of just being like, well, is the model just obviously reward hacking or something? Are we in this regime where whenever we try and produce an example of the model doing something bad, we do it, and then we just keep training on those and eventually we run out of new examples, but that is not a very reassuring situation?

So I think just more basic reward hacking testing of can we put the model in a situation where we can see that it's clearly playing the training game rather than doing what we actually wanted or reward hacking in various ways. And there's capability evaluations you could do here also of how good is the model at the kind of reasoning and generalization that would be necessary for it to do things like observe that it can now get more reward by doing this thing that was different from what it was doing before and take advantage of the situation in this way, hack the reward function in this way. You can get some sense of what size leap of generalization is required for the things you're worried about, what size leap of generalization the model seems to be able to exhibit. I think you can also do things like creating a model organism that definitely is kind of nefarious and deceptive and trying to do bad things, and pass it through your training process and see if your training process in fact removes those tendencies.

**Daniel Filan:**
So is this basically [the sleeper agents paper](https://arxiv.org/abs/2401.05566) you're thinking of?

**Beth Barnes:**
Yeah, that sort of thing. Just the idea of: what does your training process incentivize? Is it possible for deception to persist through it or for scheming behavior to persist through it? Or even just abject reward hacking behavior that's not even trying to be that hidden, does that just get rewarded? Is your training process sloppy enough that that just gets rewarded? Then you would obviously be concerned.

So you can look at what your training process is actually incentivizing. You can look at relevant capabilities. I think you can do things that are like seeing exactly how much hand-holding do you need to get the model to start doing nefarious things and reward hacking and taking advantage of opportunities. If you actually train it to do that and you're seeing how nearby is it, how close does it seem to be? The other direction from the model organisms thing, which is you start with a thing that's doing the unwanted thing and you check how much your training process seems to remove it.

So those are some alignment things you can do. It's something I want to do more thinking about, but I feel like there's a bunch of stuff that you can do. Obviously it's tricky and subtle in a bunch of ways, but it's really not like we have nothing.

**Daniel Filan:**
Right. I might just be misunderstanding, but a lot of these are like... if we're doing some sort of capabilities evaluation on "how good would you be at scheming and deception and stuff", if I'm really paranoid, I could imagine a model sandbagging that evaluation and being like, "Oh, I could never," or if I'm imagining... this is even more paranoid, but if I imagine an initially misaligned model getting put through, let's say insufficient alignment training, I can imagine that model being like, "Oh, I'm definitely aligned right now. It's working, guys. No need to worry about copies of me." So how do you address these concerns with these sorts of capabilities for scheming, capabilities for persisting through alignment training, these types of approaches?

**Beth Barnes:**
Yeah, like sandbagging in general, I do think there's things that you can do. And if your elicitation is good, it should be relatively hard to sandbag. It's hard to rule things out completely, but you're like, "Okay, how galaxy-brain does the model need to be?" How weirdly discontinuous does the capabilities progress need to have been for it to be plausible that the model is doing these sufficiently elaborate and clever schemes? And how does it know exactly in which cases it's supposed to look aligned versus not look aligned and reveal its capabilities versus not reveal its capabilities? This kind of thing.

Because if it was impossible to train your model to act misaligned, then you would be like, "What is going on?" So you need it to be that the model allows itself to initially get trained on this and then doesn't just instantly, as soon as it sees anything that looks like alignment training data, become completely aligned - that would also be like, "What is going on?"

So it does seem like it has to be making reasonably subtle inferences about "where in training am I and what exactly is happening?", and this is another capability you can measure if you fine-tune on some data set of questions about what is up with training and what is currently happening and what stage is the model at and what has and hasn't it been trained on. You can see how much it knows about that and whether it's plausible that it's playing these kinds of strategies. And again, I'm not that worried about sandbagging when you have IID training data.

## Existential safety methodology <a name="existential-safety-methodology"></a>

**Daniel Filan:**
Presumably at some point the models do get smart enough that they do understand how they're being trained. They do understand what you'd be looking out for. And let's say the models aren't sandbagging those evaluations, let's say at some point you run "how good is this model at understanding what's happening during training?" and your evaluation says "really good". Is the idea that at that point we're in the superhuman regime and hopefully we've already done some harder alignment research, or-?

**Beth Barnes:**
Yeah, I think basically. I have some sense of what is the horizon at which the thinking I'm doing now is useful versus that will be way eclipsed by the thinking that we will have been able to get out of models or something. Or it's just far enough and alien enough and confusing enough that it feels like, "Okay, let's see if we can make it that far without destroying everything and then we'll worry about that later." I think the thing that I'm honestly more worried about is that I know there will be a bunch of things that do look weird and suspicious, but we haven't quite been clear enough about exactly what is weird and suspicious and when you should stop and when you should be concerned.

Rather than everything looking completely fine because the model was really galaxy-brain doing everything right, it's more like, there are a bunch of signs but it's just a bit fuzzy and confusing and some of these things almost blend in with stuff that is more normal ML problems, like sandbagging and stuff, where maybe your training was just messed up in a certain way such that gradients didn't do what you expect, the optimization landscape was not nice. It's like, was the model doing that deliberately? I don't know.

**Daniel Filan:**
I actually want to talk about this style of thinking or this horizon that you're trying to operate on. Because I think broadly, a lot of people are mostly thinking, "Okay, models we have today, or models that are 10% better than what we have today: how should we think about those? What should we be worried about there?" And then I think some people in the x-risk community are like, "Okay, we have quasi-gods or something on our laptops. How do we deal with that?"

And both of those... well, it's hard to think about quasi-gods, but you sort of know what you're thinking about, or the scope at least, or it's at least the scope that some people are used to thinking about. I think this medium term, what if we have AI that's star-brain but it's not galaxy-brain, this intermediate zone? I feel like I've seen less just defining that scope or talking about what that even is and why we should think about it. I'm wondering if you have thoughts about...

**Beth Barnes:**
Yeah, so I think ways you could operationalize this... there's the thing I just said where it's the point at which I expect that there to have been way more cognitive labor on the things that I'm currently thinking about, such that it's not very useful for me to think about it now. Another is maybe just the point at which, if a substantial chunk of the risk comes before that, then it's a lot easier to think about. It's like, "Okay, maybe we can try and address that chunk of risk."

And the sort of thing I'm thinking about is similar to some of the [Redwood](https://www.redwoodresearch.org/) [Research] operationalizations for [control](https://www.alignmentforum.org/posts/d9FJHawgkiMSPjagR/ai-control-improving-safety-despite-intentional-subversion) and things, like you could be getting a large multiplier of your lab's output by models with scaffolding and chain of thought and giving them a bunch of tools and stuff. So it's not - with the best, trying to make it easy for them to do lots of reasoning and you can just run them a lot and stuff, you're getting a very large amount of cognitive work, and they're probably superhuman at a bunch of narrow things that go along with that. But there's not particularly reason to expect they'd be doing these elaborate superhuman schemes without any of that reasoning or using tools or anything like that. It just seems like there is some gap between capabilities under maximum elicitation and capabilities under monitoring and constraints, and a bunch of ways that you're trying to make it harder for models to do particular kinds of reasoning.

It's like, maybe you have a ten-point scale or something, and doing this complicated scheme would be 6 capabilities, and doing a ton of alignment research and stuff would be 5 capabilities, and the difference between what you can do under maximum elicitation and maximum constraint is two points on the scale or something. Now you have a three-point buffer between when you expect there to be a lot of stuff happening in the world being transformed by huge amounts of cognitive work, and AIs doing these elaborate schemes even when you're trying to make it hard for them.

**Daniel Filan:**
Right. It seems like, if a listener is interested in just reinventing this methodology for themselves, it seems like they've got to say something like, okay, let's just come up with some capability scale that I can peg "able to scheme", "able to scheme even when someone's editing my brain"... So there's something like this capability scale. There's something like, imagine that a bunch of researchers are trying, but they're a little bit bumbling, or there's going to be kind of ambiguous things that they've got to muddle through a little bit. [Buck Shlegeris](https://www.redwoodresearch.org/team/buck-shlegeris) talks about the ["trusted dumb models versus untrusted smart models"](https://www.lesswrong.com/posts/LhxHcASQwpNa3mRNk/untrusted-smart-models-and-trusted-dumb-models) dichotomy where, on this scale of capabilities, at some point versus below, you think you've done enough alignment work that you can recruit those things on your side and you imagine, how can I use those capabilities to do alignment? What alignment tools can I come up with? At what point am I just... Then just going up the scale of "Have we solved alignment for Level Four models yet? If no, don't worry about objections that come up at Level Nine." If somebody did that, would they have recreated the important parts of the methodology, or do you think there would be things that they were missing?

**Beth Barnes:**
I'm not sure I quite understood what you were saying. Let me try and say it back to you. It sounded like what you were saying is you have some scale of capabilities, and it's like, "Don't worry about a higher number if there are models that are at a lower number that could destroy the world and we don't have good solutions."

**Daniel Filan:**
Yeah, there was that, plus imagine a moving threshold of where we have good solutions and when you're coming up with solutions at the threshold, you can use capabilities that models have below the threshold. Also, try and think in terms of people looking at numbers on a screen, scratching their heads, and being like, "Did I just set the hyper learning rate too low?" or whatever. There's also a frame of mind, I think, of imagining doing ML science in a world that's a little bit messy versus super-duper clean, abstract schemes of, "We can know exactly this thing and exactly not this thing." I'm not sure if this is a real distinction, but it's at least a distinction in style, I think.

**Beth Barnes:**
I'm very "expect us to do things messily". Both the idea of, if all of these points on the scale are producing risk, we might as well try and deal with the easier ones first: depending on exactly how you construct that, that may or may not make sense. Then there's the thing of, maybe the best way to deal with this later thing is first, figure out how we could make this earlier thing safe, such that we can use it to help us with the later thing, which is similar to the [question of] at what point will the cognitive labor that has happened be just so huge relative to what I'm doing that it's just not very useful? And some slightly more general thing of, at what point will the world be so transformed that it just seems really hard to think about it relative to the things that are a bit more in sight?

**Daniel Filan:**
That one is less purely thinking about the AI intelligence scale and more thinking about, at this level of intelligence, we don't have corporations anymore, or individual human brains have all been networked and we don't have to have our own thoughts.

**Beth Barnes:**
Right. I do think there's some amount of this stuff that's abstract math, ML or whatever, but I think for a bunch of it, what exactly you do is dependent on roughly who is doing what with these things, why and in what way, and we just have totally no idea what's going on. It's Dyson spheres and uploads and whatever. I just expect to have less useful things to say about what people should be doing with their AIs in that situation.

## Threat models and capability buffers <a name="threat-models-capability-buffers"></a>

**Daniel Filan:**
Fair enough. You co-lead Model Evaluations and Threat Research. We've talked a bit about evaluations, but there's also threat models of what we're worried that the AI is doing and what we're trying to evaluate for. Can you talk a bit about what threat models you're currently trying to evaluate for?

**Beth Barnes:**
Yeah. Right now, we're thinking a lot about AI R&D, so when models could really accelerate AI development. I was talking about the tasks that are like, "Do what a research engineer at a top lab could do in a day or a few days," that kind of thing, basically because, at the point where you have models that could substantially accelerate AI research, generally, things will be getting crazy. Any other risk that you're concerned about, you might just get there really quickly. Also, if you don't have good enough security, someone else could maybe steal your model and then quickly get to whatever other things that you're concerned about.

**Daniel Filan:**
There's something a bit strange about that in that, from what you've just said, it sounds like you're testing for a thing or you're evaluating for a thing that you're not directly worried about, but might produce things that you would then be worried about.

**Beth Barnes:**
There's some slightly different views on the team about this. I'm more in the camp of, this is a type error, it's not really a threat model. I don't know what we should actually do, but I think there's a way that you could think about it where it's more like you have the threat models and then you have a process for making sure that you've got enough buffer, or being like, when are you going to hit this concerning level, when do you need to start implementing certain precautions? That sort of thing. The AI R&D thing is part of that and part of what you use to evaluate how early do you need to stop relative to when you might be able to do this thing. I don't know, that's a little bit pedantic, and I think it's more natural to just be like, "Look, accelerating AI research a bunch would just be kind of scary."

**Daniel Filan:**
I hope that listeners to this podcast are very interested in pedantry. But this actually gets to a thing I wonder about evaluations which is, especially when we're imagining policy applications, I think usually the hope is there's some capability that we're worried about, we're going to test for it and we're going to get some alarm before we actually have the capability, but not decades before, a reasonable amount of time before. It seems like this requires having an understanding of how do buffers work, which things happen before which things and how much time, training compute or whatever will happen between them. I'm wondering: how good a handle do you think we have on this and how do you think we can improve our understanding?

**Beth Barnes:**
I do think it is valuable to have something that goes off when you actually have the capabilities as quickly as possible after you actually have them, even if you don't get that much advance warning, because there just are things that you can do. It's not necessarily that as soon as you get it, instantly, all of the bad things happen. And to the extent that the forecasting part is hard, I do worry about us just not even having a good metric of the thing once we've got it, not being clear about what it is and not being clear about how we're actually going to demonstrate it. Step one is actually having a clear red line.

On the other hand, there's ways in which it's harder to get good measurements once you're at these really high capability levels, for all the reasons I was talking earlier about what makes evaluations hard: if they're long, difficult and require a bunch of domain expertise you don't have, it's hard to actually create them. There's other ways in which we will naturally get buffer if we create the hardest tasks that we can create that clearly come before the risk.

There's two ways you could think about it: one is first, you create the red line and then you figure out how much buffer, you adjust down from that. The other way is you create the eval that's the furthest out that you're still confident comes before the thing that you're worried about. Then, you might be able to push that out to make it less overly conservative over time as you both narrow down the buffer and as you just become able to construct those harder things. I guess I didn't answer your actual question about how do you establish the buffer, or what things we have for that.

**Daniel Filan:**
The question was, "How well do we understand how big buffers are and how can we improve them?" Although anything interesting you can say about buffers is welcome.

**Beth Barnes:**
There's maybe two main notions of buffer: the scaling laws versus more post-training, elicitation, fiddling around with the model, type of thing. If we're like, "Oh, is it safe to start this giant scaling run?" and we want to know that before we start, before the financial commitments are made around it or something. And there's also, suppose we have a model: if people work on it for a while, you suddenly realize you can do this new thing.

We talked a bit about the elicitation stuff before. I think in general, something I'm really excited about doing is getting a smooth y-axis of what is the dangerous capability that we're concerned about, and then roughly, where do we put the line? Then, we can see points going up towards that, then we can start making scaling laws and you can put whatever you want on the x-axis and see how... it could just be time, it could be training compute, it could be effective compute performance on some other easier-to-measure benchmark, it could be elicitation effort, it could be inference time compute, all these sorts of things.

But the bottleneck to having those is having a good y-axis currently, where there's various properties that you want this to have. Obviously, it needs to go up as far as you're concerned and then, the increments need to mean the same thing. The distribution of tasks, the way that the scoring works, it needs to be such that there is some relationship between moving by one point when you're near the bottom and moving by one point when you're near the top, such that the line behaves in some predictable way.

There's a few different ways you can do that. One is constructing the distribution of tasks in some particular natural way, in some way where it's going to have this property, or you can have a more random mix of stuff, then weight things or alter things in such a way that it is equivalent to this distribution you want. You might do something that's making structured families of tasks, where these other tasks were strictly subsets of these things and they have the same relationship to their subtasks, such that going from sub-subtask to subtask is equivalent to subtask to task, or whatever.

**Daniel Filan:**
Somehow, it feels like you've got to have some understanding of how tasks are going to relate to capabilities, which is tricky because, in a lot of evaluation stuff, there's talk of, "Does the model have X capability or Y capability?" but to my knowledge, there's no really nice formalism for what a capability is. Are we eliciting it or are we just getting the model to do something? Intuitively, it feels like there should be something.

**Beth Barnes:**
I think that is not what I'm most worried about. I think that we can operationalize a reasonable definition of what a capability is in terms of "the model can do most tasks of this form" or something - activities that involve doing these things or drawn from this distribution. That's not the problem, it's how do properties of tasks relate to their difficulty? How can we look at a thing and decide when the model is going to be able to do it? It's more like: how does performance on tasks relate to performance on other tasks?

**Daniel Filan:**
Yes, which does route through facts about models, right? Especially if you're asking [what happens] if we scale or something.

**Beth Barnes:**
You can imagine a bunch of simple properties that might underlie things, or at least might be part of them. Maybe it's just horizon length, basically, because... You can have a bunch of different simple models of this, of there being some probability at each step that you fail or that you introduce an error that then adds some length to your task. Some of these models, you have some notion of critical error threshold, recovery threshold, or something [like] "Do you make errors faster than you can recover from them? Do you add (in expectation) at each step more time recovering from errors than you make progress towards the goal?" Things like that.

Or things about the expense of collecting training data, of how many RL episodes would you need to learn to do a thing that requires this many steps versus this many steps, or: how expensive is it to collect data from humans? What is the cost of the task for humans? is a thing that you might expect to get some sensible scaling with respect to...

Some things are going to be totally off, based on that, but over some reasonable distribution of tasks, maybe just how long it takes a human in minutes or how much you would have to pay a human in dollars. Then, maybe there's also corrections for: you can tell particular things are more and less model-friendly in different ways, or maybe for models you want to do less "what can one human do?"; models are more like, you get to choose which human expert from different domains is doing any particular part of the task or something, so they have this (compared to humans) much more broad expertise.

What other factors can you look at? I do think you can identify particular things that models are bad at, and maybe in the future, we'll have more of an empirically-tested idea that "X is what makes a task really hard, we can look at how models are doing on that and that will predict it." Particular things that we've looked at the most [as] some abstracted capability that's difficult for models are things that are like inference about what a black box is doing, either with or without experimentation. I think models are especially bad at the version with experimentation, where you have to explore, try different inputs and see if you can figure out what function is being computed. You'd imagine things like that. There's some particular characteristic a task has, where you have to do experimentation or something, that models could be bad at. I feel like this is slightly a tangent, I've maybe forgottten your question... Oh, buffers, right.

**Daniel Filan:**
I take this answer to be saying there are ideas for how we can think about buffers, and we've got to look into them and actually test them out. Maybe interested listeners can start cracking on some of that science.

**Beth Barnes:**
I think there's so much to try here and so much to do. The more understanding we have of how different evaluations predict other properties and other evaluations and things, the better. I would really like to someone to [look into]... Say we have some threat model or some activity in mind, and we create a bunch of tasks to test for that, how correlated are the performance on these versus performance on ones designed to test something else? How much do tasks actually capture specific abilities versus we're just mostly testing some basic, "Can the model put one foot in front of the other," thing, and it doesn't really matter how you tailor it, or something?

[Another thought about] ways of adding buffer: you can do this thing where you construct some kind of y-axis and you can make scaling laws based on it and then you can be like, "Oh, we need to put this much as a correction based on the x-axis." Another thing you could do is: you modify the task in such a way that you're like, "When the model can do this modified task, the difference between when it can do that and the actual task is the buffer," or that you modify the agent in some way, have the human help out in a certain way, like, "Okay, my buffer is the gap between when the model can do this, given some amount of hints, given that the task is decomposed in this way, [given] that it has this absurd amount of inference compute" or something. It's possible it's easier to construct them in terms of that.

**Daniel Filan:**
There are a bunch of potential things here. It seems like the question is: how continuous are you in these potential y-axes?

**Beth Barnes:**
I think it does feel like the biggest bottleneck here is just having a good y-axis to do any of these measurements: if you find these nice scaling laws on some random multiple-choice dataset, who knows how that corresponds to what you actually are concerned about? We already have all these scaling laws for things that ... but we don't know what the relationship between web text loss and risk is.

**Daniel Filan:**
Sure. This whole question about buffers was a bit of a tangent from just talking about threat models. You were saying: the current thing that it sounded like METR was mostly testing was AI research and development. Can you get a machine-learned model to do machine learning research?

In published material, I see a lot of talk about ARA, which is short for "autonomous replication adaptation". Do you think we should just think of ARA as machine learning research, or is that a different thing in your mind?

**Beth Barnes:**
It's a little subtle and different people on the team conceptualize it in slightly different ways, I think. How I think about some of the threat modeling stuff is we're asking, "What is the first, or what is the easiest, way that models could do really destructive stuff?" There's some things where it's like, "Well, obviously, if it could just do crazy nanotech, something like that would clearly be very destructive," or something. [But we're asking] how could it be very destructive, even before it's very superhuman in any particular way? What are the differences from humans that would allow the model to be much scarier?

Some of these things are just around scale, speed and being virtually instantiated rather than biological. It does seem like one of the earlier reasons why you might be concerned about the models is that they could be operating at a very large scale, they could be very distributed, not necessarily easy to shut down... More generally, it is unclear to me exactly how much various things in the world are okay because it's not easy to recruit arbitrary numbers of smart experts cheaply to your random destructive cause. If terrorist groups, rogue states or whatever... If it was very easy for them to recruit very large numbers, even within the normal range of experts or something, or that was proportional to compute or proportional to how well they were then currently doing at cyber-attacking other people's compute, maybe this would just be pretty destabilizing.

We're concerned about: what is the point at which models can just be doing substantial things by themselves in a way that the damage they can do is not bottlenecked on particular humans? If some relatively small human actor gets hold of these models, wants to be particularly un-cautious and is like, "Go and take over all the compute that you can and do a bunch of things," when would the models plausibly be able to do that? Just focusing on it as not the most likely place that most of the risk comes from or something, but as "what is the first thing?" Things that are a prerequisite.

Before models are able to cause a huge amount of destruction, while they're under control with some limited resources and don't have the ability to accumulate things, communicate to all their copies, make notes to themselves and do this sort of thing... Before models can do a bunch of damage without all of those things, they will be able to do a bunch of damage given that they have all those things. It seems like the earliest threat models will involve some kind of - or not necessarily all of them. There's some class of things where it's about autonomy or it's decoupled from the resources of the original human actor who starts it, and you have this agent that is not constrained by [the fact that] it's just being deployed in a specific way and it can be shut down. It's more like it's doing its own thing.

**Daniel Filan:**
Somehow, the test for autonomous replication adaptation is saying: when are we getting AIs that can act in this very unconstrained way? And we're going to get dangerous unconstrained AIs before we get dangerous really constrained AIs because it's easier to be dangerous when you're unconstrained than when you're constrained.

**Beth Barnes:**
That's a more succinct way of putting it.

**Daniel Filan:**
It's so much easier to be succinct when you're summarizing something someone said. Sure. It seems like current METR work is driven by this sense of, "Okay, when are we going to get AIs that have some potential to do autonomous replication or do ML research, such that after that, they're going to be able to do really scary stuff?" From that, I ... Sorry, do you want to say something?

**Beth Barnes:**
Yeah. I just remembered another way of talking about this, which is: what is the point at which your evaluations can no longer provide you very much safety? It's at the point where models are either able to improve themselves or just able to spend much more resources trying things, iterating on scaffolding or eliciting themselves than you would've been able to do in the evaluations. It then becomes hard to rule out that the model could figure out how to create bioweapons or something. If you're at the point where it can do the same work that you would've been doing in the evaluations of "try a bunch of ways to try and get it to make good bioweapon ideas, then validate them in some way." When the model could have billions of dollars, lots of copies of itself and just do a bunch of that, then it's hard for us to say that it can't do any of these other threat models. It's a combination of the ARA and the AI research.

**Daniel Filan:**
Somehow, it's also testing the model's ability to elicit capabilities of "itself."

**Beth Barnes:**
Yeah, that's one of the sort of things that we're interested in.

**Daniel Filan:**
Sure. If that's the rationale for focusing on ARA, focusing on machine learning research, do you think that people should spend time just doing evaluations of fairly object-level things? One thing people talk about is, "Can this AI make a bioweapon?" or "Can this AI persuade people of false things?" or stuff like that. Is it just a thing of: you're not working on that because you're working on something else, but definitely, someone should?

**Beth Barnes:**
Yeah. I think a bunch of the domain-specific things, it's really helpful to have a bunch of expertise in that domain. The CBRN - chemical, biological, radiological, nuclear things - we're not really doing much; other people have more of that expertise. I think the thing I am worried about is people doing that without doing proper elicitation and being like, "Oh, the model can't do this zero-shot," or something; in some sense, we're definitely interested in the autonomous version of all of those things: OK, but if you give it a computer and let it fiddle around for a while, can it, in fact, do something scary, if you plug it into all the right tools?

In terms of the persuasion stuff, I think, like I was saying earlier, it just seems pretty hard to evaluate. Again, it's relatively unlikely that the model could do a lot of really bad stuff before it can make plans, accumulate resources and learn lessons from all the different copies and things. If you're imagining the version where models that work reasonably like models today, where each instance knows nothing about the other instances, coordinating in some persuasion campaign or something, I think that either the model has to be strategic enough to do something kind of reasonable even without communicating with any of the copies and be insanely good at persuasion, or be really quite good at persuasion and somehow doing very good, galaxy-brain acausal coordination between all of these copies, or maybe we're in a world where people train models to use some opaque representations that we can't understand and they're all making notes to themselves, messaging each other with things and we're like, "Are they just communicating about their plans to overthrow the world or not? Who knows?" It does feel like most of the worst scenarios require either very high levels of narrow capabilities, or the model can actually get stuff, build up its plan and take actions in sequence as opposed to having to simultaneously zero-shot do all these things separately.

**Daniel Filan:**
There's some sense of wanting to prioritize evaluations of, "Let's make evals for the scariest, soonest thing."

**Beth Barnes:**
Yeah, and that it gets more things with one thing, rather than having to look at all of the different-

**Daniel Filan:**
More juice per eval.

**Beth Barnes:**
... Something like that, but yeah, I definitely think other people should be doing all of the other things, it's just why we in particular are doing this thing in particular.

**Daniel Filan:**
I wonder if there's some personnel difficulty, where in order to... Suppose you're trying to craft a really good evaluation of some scary task. You need people who both understand that domain really well and people who understand machine learning really well. If the scary task is machine learning, great job, the people are the same people. If the scary task is not machine learning, now, we have to hire two different types of people and work together. It's just harder to make that happen.

**Beth Barnes:**
That's a good point. I haven't thought of that. I have worried about both [the fact that] we don't know enough about the specific things that we're trying to get the models to do, and the other people don't know enough about how to get models to do stuff, sort of thing. The ML one is better in that regard.

## METR's policy work <a name="metrs-policy-work"></a>

**Daniel Filan:**
I next want to talk about a bit about METR's policy work. I think one thing people maybe know you for is thinking about what's sometimes called responsible scaling policies. I gather that METR had something to do with [Anthropic's responsible scaling policy](https://www.anthropic.com/news/anthropics-responsible-scaling-policy)?

**Beth Barnes:**
Yeah. I think the history of that is roughly [Paul](https://paulfchristiano.com/) [Christiano] and I, and [Hjalmar](https://metr.org/team/hjalmar-wijk/) [Wijk] in... I guess this was ARC Evals days. We were thinking about this thing since I think late 2022: what in particular should labs commit to or how do evals fit into things? What would a lab be able to say such that we would be like, "Oh, that seems about as good as you can do in terms of what makes sense to commit to"? Originally [we were] thinking more, either [you want] some [safety case](https://www.alignmentforum.org/posts/HrtyZm2zPBtAmZFEs/new-report-safety-cases-for-ai) thing that's relatively unspecified, and more just you have to make some safety case and there's some review process for that. Then, I think we ended up thinking a bit more about, maybe more like a standard [where] we can say, "You need to do these specific things at these specific times after you run these specific evals."

I think Anthropic was also excited about making some kind of thing like this happen. I don't know quite when Paul was talking to them; there's definitely some amount of independent development. But specifically, the responsible scaling policies and the idea that it's mostly centered around red lines and, "When is your commitment that you would not scale beyond this?", I think was mostly an idea that came from us, then Anthropic was the first to put this out, but we'd been talking to various other labs, encouraging them, trying to help them project-manage it and push this along. Now, [OpenAI](https://cdn.openai.com/openai-preparedness-framework-beta.pdf) and [DeepMind](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/) also have things like this and various other labs are making various interested noises. The UK government pushed for this at the [AI Safety] [Summit](https://www.aisafetysummit.gov.uk/) and now, a bunch of labs also at the [Seoul Summit](https://en.wikipedia.org/wiki/AI_Summit_Seoul) [signed a thing](https://www.reuters.com/technology/global-ai-summit-seoul-aims-forge-new-regulatory-agreements-2024-05-21/) saying they would make these, basically.

**Daniel Filan:**
Sure. I guess that's something to do with [your work on RSPs](https://metr.org/blog/2023-09-26-rsp/). Is thinking about things like responsible scaling policies most of your policy work? All of it? Do you do other things?

**Beth Barnes:**
There's also a relatively core activity which I think is quite closely tied to the threat models and the evals, which is what mitigations do we actually think are appropriate at different points? That ties in quite tightly to: what are we worried about and what are we measuring for? What do you need to do? What would buy you sufficient safety? Then, there's a bunch of generic advising. People have questions related to that; governments, labs and various different people. We try and say sensible things about what do we think is evidence of danger, what do we think you need to do when, all of these things. Then, yes, specifically, this project of trying to encourage labs to make these commitments, giving them feedback on it, help trying to figure out what all the hurdles are and get the things through those hurdles. I think that's been the bulk of things. [Chris](https://metr.org/team/chris-painter/) [Painter] is our main person doing policy.

**Daniel Filan:**
Gotcha. What fraction of METR is doing this policy stuff? Is it one person on a 20-person team or half of it?

**Beth Barnes:**
Yeah, it's 10% or something. This is just mostly just Chris and it's somewhat separate from the technical work.

**Daniel Filan:**
Cool. We're at this stage where RSPs as an idea have taken off. I guess people are maybe committing to it at the Seoul Safety Summit. How good do you think existing RSPs are? And suppose that you had to implant one idea that people could have that is going to make their RSP better than it otherwise would be: what's the idea?

**Beth Barnes:**
In terms of how good different RSPs are, I haven't actually read the DeepMind one closely. The one I'm most familiar with [is] the Anthropic one, because I was more involved with that. I like the Anthropic ASL levels thing. I think it was reasonable and they made pretty clear and hard commitments to "under these conditions we will not scale further", which I thought was good. And I think other labs' RSPs had various good ideas on nice ways to frame things, but I do think it was a bit less clear [about] really when is the thing that you're making a really hard commitment [about]: "If X and Y happens, we will not scale further." I think OpenAI's security section, when I read it, I remember coming away with the impression that it was more saying, "we will have good security" rather than, "if our security does not meet this standard, we will not continue." So I think just being clear about what is your red line? When is it actually, "no, this is not okay"? Not what should happen, but what are the clear conditions? And that would probably be the thing that I'd most want to push people on.

**Daniel Filan:**
Fair enough. So yeah, going back to METR's role in RSPs or talking about policy, I'm wondering: the policy work and the science of evaluations work, to what extent are those two separate things budding from the same underlying motivation versus strongly informing each other, like "we want these policies so we're going to work on these evaluations", or "based on what we've learned about the science of evaluations, that strongly informs the details of some policies"?

**Beth Barnes:**
I think there's pretty strong flow, particularly in the direction from threat modeling and evaluations to policy in terms of what do you recommend, what policies do you want? There's a bunch of work that is a different type of work, which is just this project-managing thing from the outside and dealing with all the stakeholders and getting feedback and addressing people's concerns and that kind of thing. There's ways in which the day-to-day work is different. But I think that in terms of what it is that we're pushing forward? What are the most important things? How good would it be to get different types of commitments? What should be in there? What is the actual process that the lab's going to do? And then maybe some of the stuff in the other direction is just feedback on how costly are different possibilities? How big of an ask are different things? What are the biggest concerns about the current proposals? What is most costly about them?

Earlier on, we were thinking more that we could make something that's more prescriptive, like "in this situation you need these mitigations", and then it became clear that different labs are in sufficiently different situations or different things are costly to them. And also just depending on uncertain future things of what exact capability profile you end up with, [it meant] that we either had to make something that had a lot of complicated conditionals of "you must do at least one thing from category A, unless you have B and C, except in the case of D, where..." Or that it was something that was very overly restrictive, where this is obviously more conservative than you need to be, but it's hard to write something that's both relatively simple and not way over-conservative that covers all of the cases. So that's why we ended up going to more [towards] each lab can say what their red lines are and then there's some general pressure and ratchet to hopefully make those better.

**Daniel Filan:**
What kinds of differences between labs cause this sort of situation?

**Beth Barnes:**
This is going to be one of those things where I can't think of an example. I think that the sort of conditionals we were ending up with were... Security is maybe actually a good one. There's a bunch of things within Google where they already have [policies for] "here's how we do this for Gmail protected data, where we have this team with these air-gapped machines and blah, blah, blah." There are some things where it's relatively straightforward for them to meet particular levels of security. Whereas for OpenAI or something, this is much harder because they don't have a team that does that already.

And probably similarly with some of the monitoring or process for deploying things. Larger companies have more setups for that and more capacity and teams that do this kind of thing already. I imagine some of the monitoring and oversight stuff might be somewhat similar to content monitoring-type things that you already have teams for, whereas maybe Anthropic or OpenAI would be more able to be like, "we did this pretty involved science of exploring these capabilities and pinning them down more strongly", and that's easier to do for them versus having a very comprehensive monitoring and security process or something like that.

## METR's relationship with labs <a name="metrs-relationship-with-labs"></a>

**Daniel Filan:**
Makes sense. I'd like to move to: I think it's a bit not obvious what METR's relationship with labs is. So semi-recently as of when I'm recording this, I think [Zach Stein-Perlman](https://www.lesswrong.com/users/zach-stein-perlman) wrote this [post on LessWrong](https://www.lesswrong.com/posts/WjtnvndbsHxCnFNyc/ai-companies-aren-t-really-using-external-evaluators) saying, "Hey, METR doesn't do third party evaluations of organizations, and they didn't really claim to, it's unclear why people believe they did." But from what you're saying, it sounds like there's some relationship, you're not just totally living in separate bubbles communicating via press releases that the whole public sees. So what's going on there?

**Beth Barnes:**
Yeah, I am confused about why there's been quite so much confusion here, but I guess what we mostly think of ourselves as doing is just trying to be experts and have good opinions about what should happen, and then when we get opportunities trying to advise people or nudge people to do things or suggest things or prototype, "well, here's how a third party evaluation could work. We'll do a practice run with you."

And maybe in future we would be part of some more official program that actually had power to make decisions over deployment there, but that's not something we've done already. Maybe this is partly where the confusion comes from: we would consider various different things in the future. Maybe in the future we'll end up being some more official auditor or something. Also, maybe not. And currently we're happy to try and help prototype things.

And we've tried to be pretty clear about this, I think. In the [Time article](https://time.com/6958868/artificial-intelligence-safety-evaluations-risks/) that we did a while ago, we're very clear that what we've done are research collaborations, not audits, and in the GPT-4 system card we say in a bunch of places "we did not have the access that a proper evaluation would need, we're just trying to say some things." And in fact the model is limited enough that we could say some kind of reasonable things about it, [but] you still have to make various assumptions in order to be able to say anything really. Yeah, there's some question about what direction we go in future or are there other organizations that are going to come up that are going to do more of the evaluation, implementation, all that sort of auditing and going down the checklist and things like this?

The thing that we're most thinking of as the central thing that we do is being able to recommend what evaluations to do and say whether some evaluation was good or not. This is a core competency that we want to exist outside of labs. At a minimum, there should be at least someone who can look at something that was done and be like, does this in fact provide you good safety? What would you need? We can say something that you could do to provide safety. And then there's question of does the lab implement it and it gets checked by someone else? Does the government implement it? Is there some new third party thing that emerges? Is there a for-profit that does it, but then there's some oversight from the government making sure that the for-profit isn't getting sloppy or something? There's all different kinds of systems in different industries that have configurations of the people who do the science of how to measure versus the people who do the actual crash-testing versus the people who check that you followed all the proper procedures and all these things. So I guess that it is inherently confusing. There's a bunch of ways this could work and we want to help all the possible ways along, but aren't necessarily saying that we're doing any of them.

**Daniel Filan:**
Yeah. The way I understand it, I guess it's a distinction between METR trying to be an organization making it so that it can tell people how third-party evaluations should work versus METR actually doing third-party evaluations, with some gray zone of actually doing some evaluations on early GPT-4.

**Beth Barnes:**
The thing we should be very clear that we should not be considered to be doing it is, we're not being like, "Hey everyone, it's all fine. We'll tell you if the labs are doing anything bad, we're making sure they're all doing everything correctly." We have no power to tell them what to do. We have no power to disclose... Inasmuch as we have worked with them on an evaluation, it's been under NDA, so it's not like we had an official role in reporting that, it was just, then we can ask them for permission afterwards to publish research related to the stuff that we did. We've never been in a position to be telling labs what to do or to be telling people how labs are doing, apart from based on public information.

**Daniel Filan:**
Sure. Getting to that relationship, you had early access to some version of GPT-4 to do evaluations. How did that happen?

**Beth Barnes:**
They reached out to us as part of the general [red teaming program](https://openai.com/index/red-teaming-network/). And similarly [with] Anthropic, we had early access to Claude. In addition to that, various labs have given us... I can't necessarily talk about all the things we've done, and there's a spectrum between research support and just "have some credits so you can do more eval research" or "have this fine-tuning access" or "work with this internal researcher", "have this early model not necessarily to do a pre-deployment evaluation on it, but just help you along"; various different things going on.

**Daniel Filan:**
Can you say how much of the dynamic is labs saying, "we've got a bunch of spare capacity, let's help out this aspect of science" versus labs saying, "hey, we would like to figure out what this model can do or how worried people should be about this product"?

**Beth Barnes:**
Yeah, I think it has been somewhat more in the direction of labs being like, "Hey, can you run an evaluation for us?" And us being like, "Well, the main thing we're working on is figuring out what evaluations should be or what evaluations to run. So yeah, I guess we can do a bit of that as a practice, but it's not like this is a full polished service, like I will put you in touch with our department of doing a billion evaluations." It's like, "Well, I guess we can try and do something, but this is more of research than of us providing a service."

**Daniel Filan:**
Sure. Related to this: [news](https://www.vox.com/future-perfect/2024/5/17/24158478/openai-departures-sam-altman-employees-chatgpt-release) that [came out](https://www.vox.com/future-perfect/351132/openai-vested-equity-nda-sam-altman-documents-employees) in the past month as we're recording, it seems like most OpenAI former employees who left signed some documents basically promising to not disparage the company and promising that they wouldn't reveal the existence of the agreement to not disparage the company. You've previously worked at OpenAI.

**Beth Barnes:**
Indeed.

**Daniel Filan:**
Also you've written [something on the internet](https://www.lesswrong.com/posts/yRWv5kkDD4YhzwRLq/non-disparagement-canaries-for-openai?commentId=MrJF3tWiKYMtJepgX) talking about this situation, but it was a comment to something else. I think people might not have seen it. Can you lay out what is your situation with your ability to say disparaging things about OpenAI, both now and when you started METR?

**Beth Barnes:**
So yes, when I left OpenAI, I signed this secret general release, which includes the non-disparagement provision and the provision not to disclose anything about it. My understanding at the time was that this did not preclude saying things that were obviously true and reasonable; "OpenAI's model has this problem" or something, that is not an issue. This was based on... Well, yeah, this is not how you're supposed to get legal advice, but asking the OpenAI person who was doing this with me, what this meant, and just some general sense of, well obviously it would be completely absurd if it was like "you can't say completely true and reasonable things that might have a negative bearing on OpenAI" and that also, I can't imagine a circumstance in which it would be in their interest to sue someone over that, because that would just look so bad for them. So I hadn't actually given this much thought.

I think the other thing to point out is it is kind of a narrow circumstance in which this would be the specific thing that's preventing you from saying something. Because most of the things that I would say about OpenAI, anything that's based on private information from when I worked there or from any relationship or any work that METR did with them would be covered under nondisclosure. So the only case where the non-disparagement protection in particular is coming into play is if there's public information, but you want to say something about it. So basically I think I'd kind of forgotten that this clause existed also.

And I think something that we've tried to do in our communication for a reasonably long time is: we're not an advocacy group. It's not a thing that we do to opine on how much we like different labs in some generic way. We're here to say relatively boring technical things about... We can maybe say things about specific evaluations we did or specific results or whatever, but trying to position ourselves as more like, I don't know, I guess the animal welfare example would be [that we're] more like the people who go and measure the air quality in the chicken barn than the people who are writing angry letters or doing undercover recordings.

I mentioned in that comment that I sold my OpenAI equity - I can't remember if it was quite as soon as I was able to, because I was hoping I might be able to donate if I waited a bit longer. But I sold it all last year at least. I think I had either not read or forgotten that there was any provision that they could cancel or claw back your equity. As far as I remember, that never occurred to me as a particular concern until this stuff came up.

**Daniel Filan:**
Okay. So you sold equity last year. Just for the timeline of METR - I guess ARC Evaluations is what it initially was - when did that start?

**Beth Barnes:**
I left OpenAI in summer 2022 and went to [ARC](https://www.alignment.org/) and was doing evaluations work, which was talking with Paul [Christiano] and thinking about stuff and doing some of this stuff we're talking about of "what should a good lab do", sort of thing. And I guess that became increasingly a real organization, and ARC Evals a separate thing, and then [it] became METR. I think I realized, I can't remember exactly when this was, that although I couldn't sell equity, I could make a legally-binding pledge to donate it. So I pledged to donate 80% to [Founders Pledge](https://www.founderspledge.com/). I can look up the date if it's relevant, but...

**Daniel Filan:**
Sure. How does one legally...?

**Beth Barnes:**
So Founder's Pledge is legally binding, which is nice.

**Daniel Filan:**
And somehow the reason it's legally binding is you're promising to Founder's Pledge that you're going to give them a certain thing at a certain time?

**Beth Barnes:**
I actually don't know exactly how this works. I just remember that I was told this by the Founders Pledge people. So then OpenAI only has liquidity events every so often, but I sold all my equity when I was able to.

**Daniel Filan:**
Understood. So I guess that gets to the issue specifically about equity and non-disparagement agreements. And if people are interested in that, I highly encourage them to read particularly [Kelsey Piper's reporting in Vox](https://www.vox.com/future-perfect/2024/5/17/24158478/openai-departures-sam-altman-employees-chatgpt-release) as well as OpenAI's statements on this matter and [Sam Altman's statements on this matter](https://x.com/sama/status/1791936857594581428).

I think more broadly there's this question of: as a third-party person, I would like there to be an organization that is fairly able to fairly objectively say, "Hey, here's some tests. This organization is failing these tests." Either explicitly say "this organization is making AI that's dangerous." Or say, "Hey, this organization has made an AI that fails the 'do not murder' eval," or something. And in order for me to trust such an organization, I would want them to be able to be fairly independent of large labs. Large labs have some ability to throw their weight around either via non-disparagement agreements or via "we will or won't cooperate with people, we will or won't let you have access to our new shiny stuff." I'm wondering: from the outside, how worried should people be about how independent organizations like yours can be of big labs?

**Beth Barnes:**
Yeah, I mean, we have very little ability to provide this kind of oversight, largely because there is no mandate for us to have any kind of access. And if we do have access, it'll be most likely under NDA so we can't say anything about what we learned unless the lab wants us to. So until such point as either voluntarily or by some industry standard or by government regulation, external evaluators are mandated access, and that access meets various conditions such that you can actually do a meaningful evaluation and you're permitted to disclose something about the results, no one should consider us to be doing this kind of thing.

And I'm not aware of any organization that does have this ability. I think the [Executive Order on AI](https://www.whitehouse.gov/briefing-room/presidential-actions/2023/10/30/executive-order-on-the-safe-secure-and-trustworthy-development-and-use-of-artificial-intelligence/) does require labs to disclose results of evaluations that they do do to NIST, but I think it's unclear whether they can actually mandate them to do any evaluations. And then there's also not necessarily any oversight of exactly how they do the...

I actually don't know what the legal situation would be if you just did something very misleading, but at least you can imagine labs doing something very half-hearted or sloppy and providing that to the government, and they've technically met all the requirements or something. So I think this is very much a thing that is needed. And I feel bad if we gave the impression that this already existed and wasn't needed, because that was definitely not what we were trying to do. As in: it would be great to have this and we would be happy to be a part of it if we could be helpful, but we were not trying to convey that this was already solved.

## Related research <a name="related-research"></a>

**Daniel Filan:**
Sure. So moving back to science of evaluations things, your organization is doing some work, there's some work that exists in the field. First I'm wondering: [among] things other than what METR does, what's your favorite in terms of evaluations work?

**Beth Barnes:**
Yeah, like I said, I think all of the domain-specific stuff is important. It does seem quite plausible to me that you'll get some... I'm not quite sure about this, I maybe just don't know enough about it, but there's something that's in the direction of a more specialized model, more something like AlphaFold, that actually unlocks... that then someone can use that to make a way worse bioweapon than they would've been able to otherwise. And we're just totally not covering that. So I'm hoping that people are keeping an eye on that.

I like [Redwood](https://www.redwoodresearch.org/) [Research]'s thinking about control evaluations and I think they were thinking carefully about what exactly you can know under what circumstances. And credit to them for some of the things that I was saying about sandbagging and various other things as well. And I think some of the model organisms and demonstrations that various different people have been doing are interesting also. Scary demos or otherwise creating examples of things is something that we're not doing that much of and it would be great for other people to do. I mean, people should do the things that we're doing as well, but in terms of limited capacity or disclaiming, we are not covering having great scary demos. So hopefully other people will cover that.

**Daniel Filan:**
Sure. And in terms of frontier research or research that people could start doing: things you've mentioned so far [are] scary demos, understanding buffers, evaluations of things in domains other than machine learning research: are there any other things you would encourage people to do research into that are along the evaluations lines?

**Beth Barnes:**
Yeah, so we have this [bounty up](https://taskdev.metr.org/bounty/) for tasks that meet various criteria - not memorized and ideally continuous score and various other things. So just getting more of these so we can build a better y-axis would be great.

**Daniel Filan:**
Sure. So that's one thing.

**Beth Barnes:**
Doing this elicitation... I guess "buffer" covers a very wide range of things, so I think both developing the y-axis on which you can measure it or being like, "how do different ways of modifying tasks or different ways of assisting the agent, how do these compare in size to scale-ups or elicitation effort?" There's many different things that you can plot against each other and try and calibrate "how big is this compared to that?"

And then other things are distilling... As I was saying before, this whole chain from what is it that we're worried about in the world to operationalizing that in terms of specific tasks to then trying to get from these very expensive and unwieldy tasks to some easier-to-run tasks and maybe to just data sets or whatever... I think there's a lot of different things to be done there.

And in particular, something that we tried to do for a while, but ended up moving on to other things, which was hard, was creating a classification data set that captures the hard parts of doing a particular task, and finding something where you can make some kind of case for upper bounding based on "even with a reasonable amount of data and fine-tuning on this classification task, the model can't manage to identify whether it's currently doing something sensible towards its goal or not." That is something nice that gives you a stronger guarantee about the model not being able to do that kind of thing. And that maybe fits into the sandbagging stuff as well, creating model organisms of sandbagging.

We also would love to be doing more stuff on alignment and control evals and thinking of, once you're no longer at the point where the model doesn't even have these capabilities, then what do you do? How are you testing that? What would labs need to be doing?

I think there's a bunch of more fleshing out threat models that would be great. I think particularly maybe some of the [threat models involving] persuasion and things like that could do with more specification. What exactly is it that happens? What are all the other capabilities that are required? And exploring what kind of coordination do the models need to do with these different threat models? Which ones can we be ruling out based on the model's ability to coordinate and take considered actions between different instances, and if we saw that changing, with people creating models that were communicating with an uninterpretable state, how would that change all our thinking about threat models or then what evals would we want to have? I don't know. There's a lot of stuff.

## Roles at METR, and following METR's work <a name="roles-following"></a>

**Daniel Filan:**
That's a lot of things for listeners to go off of. So that's things they could do. If listeners are interested in your research or your work, how should they follow that?

**Beth Barnes:**
We are [hiring](https://metr.org/hiring), particularly for ML research scientists and research engineers. We're very bottlenecked on this at the moment, particularly for just implementing these ML research tasks, you need ML researchers to actually know what the tasks are and construct them and test them and more generally, basically all the things I've been talking about, the science of evals and building the y-axis and distilling hard tasks into easier-to-measure things and buffering all of those things... It's just a lot of science to be done. Just build the tasks, run the experiments, de-risking, does this whole paradigm actually make sense? Can we make predictions about the next generation of models and what tasks we'll see fall in what order? Do we actually understand what we're doing here? Or is it just going to be like, oh, actually these tasks just fall for a bunch of dumb reasons, or they're not really capturing something that's actually threat model-relevant.

Anyway, we are hiring specifically ML researchers and research engineers, but generally we're still a small team and excited to fit outstanding people into whatever role makes sense. We're still at the stage where for key people we'll build the role around them and I think there's a bunch of different things that we could use there.

And then the [task bounty](https://taskdev.metr.org/bounty/) and also baselining: we just need people to create tasks, test them. If you want to have your performance compared against future models, have your performance on this engineering task be the last bastion of human superiority over models or whatever, get in touch, I guess.

**Daniel Filan:**
All right, and what places do you publish work or write about things that people should check?

**Beth Barnes:**
Oh geez, we really should write more things up. The last thing we put out was this [autonomy evals guide](https://metr.org/blog/2024-03-13-autonomy-evaluation-resources/), a collection of different resources. There's [a repo with our tasks suite](https://github.com/METR/public-tasks) and [another repo for our tasks standard](https://github.com/METR/task-standard) and some various different [bits](https://metr.github.io/autonomy-evals-guide/elicitation-gap/) of [writing](https://metr.github.io/autonomy-evals-guide/elicitation-protocol/) on [how](https://metr.github.io/autonomy-evals-guide/example-protocol/) we think people should run evaluations overall, and elicitation, and some results on elicitation, scaling laws and things. That's linked from [our website](https://metr.org/).

**Daniel Filan:**
Sure. Great. Well, thanks for coming here and chatting with me.

**Beth Barnes:**
Awesome. Thanks very much for having me.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Filming occurred at [FAR Labs](https://far.ai/labs/). Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
