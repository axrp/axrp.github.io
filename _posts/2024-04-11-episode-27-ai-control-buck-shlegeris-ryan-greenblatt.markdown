---
layout: post
title: "27 - AI Control with Buck Shlegeris and Ryan Greenblatt"
date: 2024-04-11 14:00 -0700
categories: episode
---

[YouTube link](https://youtu.be/gQbCO6zGRiI)

A lot of work to prevent AI existential risk takes the form of ensuring that AIs don't want to cause harm or take over the world---or in other words, ensuring that they're aligned. In this episode, I talk with Buck Shlegeris and Ryan Greenblatt about a different approach, called "AI control": ensuring that AI systems couldn't take over the world, even if they were trying to.

Topics we discuss:
 - [What is AI control?](#whats-control)
 - [Protocols for AI control](#protocols)
 - [Which AIs are controllable?](#which-ais-controllable)
 - [Preventing dangerous coded AI communication](#no-stenography)
 - [Unpredictably uncontrollable AI](#unpredictably-uncontrollable-ai)
 - [What control looks like](#what-control-looks-like)
 - [Is AI control evil?](#is-control-evil)
 - [Can red teams match misaligned AI?](#red-team-vs-ai)
 - [How expensive is AI monitoring?](#monitoring-expense)
 - [AI control experiments](#control-experiments)
 - [GPT-4's aptitude at inserting backdoors](#four-backdoor-skill)
 - [How AI control relates to the AI safety field](#control-within-safety)
 - [How AI control relates to previous Redwood Research work](#control-within-redwood)
 - [How people can work on AI control](#how-work-on-control)
 - [Following Buck and Ryan's research](#following-buck-ryan-research)

**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Buck Shlegeris and Ryan Greenblatt. They work at [Redwood Research](https://www.redwoodresearch.org/) on the project of [AI control](https://www.lesswrong.com/posts/kcKrE9mzEHrdqtDpE/the-case-for-ensuring-that-powerful-ais-are-controlled): that is, averting risks from AIs that intentionally subvert safety measures. For links to what we're discussing, you can check the description of this episode and you can read the transcript at [axrp.net](https://axrp.net/). All right, Buck and Ryan, welcome to the show.

**Buck Shlegeris:**
Exciting to be here.

**Ryan Greenblatt:**
Exciting to be here as well.

## What is AI control? <a name="whats-control"></a>

**Daniel Filan:**
Cool. So I guess we're going to be talking about [the control approach to AI safety](https://www.lesswrong.com/posts/kcKrE9mzEHrdqtDpE/the-case-for-ensuring-that-powerful-ais-are-controlled) (or something), that you two have written about. For people who haven't done background reading, what is the control approach?

**Buck Shlegeris:**
AI control is an approach to managing intentional risk posed by models. In particular, it's an approach to managing risks that arise from models intentionally subverting your attempts to ensure and evaluate safety. The core idea of AI control is: here are two different reasons why you might be safe from models intentionally causing problems. One is that the models might not try to intentionally cause problems, and the second argument is you might have it be such that even if your models try as hard as they can to cause problems, they're unable to do so. And AI control is the second class of approach to mitigating problems with models intentionally subverting your safety techniques. And we think that this approach has the particularly nice property that it's a lot more methodologically simple to think about and ensure than trying to make sure your models actually just don't at all try to subvert your safety techniques.

**Daniel Filan:**
What makes it easier to think about?

**Ryan Greenblatt:**
It's easier to imagine specifically what goes wrong and to discuss specific threat models, whereas for example, it's hard to know exactly how AIs internally represent intentions or what would make their intentions change. And so it's very unclear if you, for example, train on better feedback: will that make it less or even more likely that your AI would try to do things that are actively against your interests? And it's just pretty unclear what the sign on various interventions are or how strong various interventions will be for the other class of non-control things, which is often called alignment.

**Daniel Filan:**
Okay, so should I basically be thinking that in control, we're trying to think about objects, like can somebody break through this wall, or can somebody write sketchy code without me being able to check, whereas for alignment, I have to think about neural net generalization and how desires are encoded, and hopefully, the first class of things are just easier to think about?

**Buck Shlegeris:**
That's a good way of putting it. There's basically a classic set of arguments about why AI alignment is hard to ensure, which is: no matter how your AI is behaving, you have to worry that it's just behaving that way because it thinks it has some way of looking good now in the hopes of causing problems later. And basically, any strategy which tries to ensure safety via doing some set of interventions to make a model be aligned, especially if those only involve looking at the input/output behavior of the model, run into this kind of problem: "Well, how do you know that it's not just looking that way on purpose in the hopes of causing problems later?" This, in fact, seems quite intractable and difficult. And it seems to us that given the current state of technology on AI safety, if you want to minimize this type of risk, it's by far easier to just assume that that is going on and try and render yourself safe anyway.

Once we started thinking about things in these terms, we quickly realized that a lot of the work that happens in AI safety is actually very well thought of in the control frame anyway. That is, a lot of techniques that people apply implicitly can be thought of as strategies to ensure that even if your models are scheming against you, you end up with okay outcomes. But we think that, inasmuch as that's in fact what you're trying to do with your safety techniques, you're a lot more likely to analyze them correctly if you think about them explicitly in this adversarial frame, where you're asking about whether there's any strategy that your models are able to employ that would allow them to cause problems.

**Daniel Filan:**
I actually want to ask another question about that. One thing that's similar to control, or at least the kinds of thing I see you doing, is alignment via adversarial training. So you put your AI in a situation where maybe it can do something sort of like taking over the world or whatever, and if it actually does it, then you give it reward negative 10 or something and try to gradient it away. How do you think of the control approach broadly as comparing to that?

**Ryan Greenblatt:**
A technique like adversarial training could be used either from a control perspective or could be used to ensure an alignment safety condition. These are just different post-conditions, or different conditions you're aiming to ensure in order to ensure safety. So for ensuring control or for trying to achieve control, the hope would be that you adversarially evaluate your AI, and then if you catch evidence of extreme egregious misalignment, then you would reconsider and decide what to do next. And in fact, it seems pretty scary to have an AI that you know was egregiously misaligned in the past and you've trained it no longer to do that in adversarial training. [For] such an AI, it's not clear that you've necessarily made it aligned versus just made it better at hiding or better at avoiding adversarial training.

For both the hope of ensuring that your AI ends up aligned or ensuring that your AI has no way of causing problems, you acquire the same property, which is that there is no strategy the AI could follow to cause problems in deployment, or in actual usage of the AI, that you wouldn't have caught in adversarial training. And this seems like naively a pretty hard property to ensure for a variety of reasons. And because of that, the actual conditions in which adversarial training---or if you don't train against it, adversarial evaluation---seem useful... I mean it's going to depend a lot on the details, but it's definitely not an obvious sufficient solution.

**Daniel Filan:**
I also would like to ask you about the contrast... I feel like 10 years ago, a bunch of [AI safety people](http://www.aleph.se/papers/oracleAI.pdf) talked a lot about [AI boxing](https://www.lesswrong.com/tag/ai-boxing-containment): putting AIs in a situation where they couldn't really do that much. How do you think control is conceptually similar or distinct to that?

**Ryan Greenblatt:**
I think in some sense, you could just rename "control" to "boxing", but we think that name is much worse because boxing implies you're doing just mechanisms that restrict the AI's affordances, whereas I think a really important class of interventions is observing the AI over a wide range of situations and using that to rule out certain problematic things. So for example, if you oversee or have humans observe 1% of the AI's actions, and that looks fine in those 1% of cases, and you just do this on an ongoing basis, then that's sufficient to ensure that the AI can only do things that a human would catch quite rarely. And it's not clear this is... I think when people think of boxing, this is not naturally what people will think of.

Another key aspect of control that I think is not very naturally implied by the word "boxing" is that using weaker AIs (that we know aren't smart enough to be seriously misaligned) to do monitoring, or to do other tasks, might be really key. More generally, I think when people say boxing, they are implicitly assuming a very powerful system that is extremely, extremely limited, whereas we're often imagining systems that are much more comparable in capabilities in various ways to humans, but which are actually given a pretty broad set of affordances, which seems like it will be necessary to get your AIs to do a lot of work, though you'll still want to, of course, limit things so that they can't cause problems.

**Daniel Filan:**
Maybe now's a good time for you guys to say a bit more concretely: what's an example or an instance of something that fits into the control approach?

**Buck Shlegeris:**
Here's an example. Maybe I'm just going to say a rough sense of what I would like AI labs to commit to, or a simple example of some commitments that AI labs could follow, which I would think of as an example of them trying to minimize risk from models intentionally subverting safety mechanisms using control. So here's basically the strategy that the AI lab would follow: they would say, "Here is a capability evaluation that we're going to use to decide whether models are capable of scheming against us." So for example, right now, it seems to me like GPT-4 is incredibly unlikely to be scheming against us. And this is basically just because I know GPT-4. There are only 10 people in the world I talk to more than I talk to GPT-4, and there's just no way that it's playing the long game. There's just no way that its reasoning process behind behaving so well as a loyal assistant to me is just being like, "Well, how am I eventually going to grab the universe?"

**Ryan Greenblatt:**
Why do you think that?

**Buck Shlegeris:**
Because among other things, when you ask it questions about what it would do if it wanted to grab power, its answers are really [redacted]. When you ask it questions about what it would do in order to grab power, its answers are not very smart. It seems to be pretty poor at reasoning through a lot of these things, and its quality of answers to those questions is comparable to the quality of its answers to a lot of other questions that are less obviously AI-scheming related. I wish that I had a better answer to this question and I think this is an important topic for future research.

**Daniel Filan:**
Let's just say we take it on faith that GPT-4 is not trying to scheme to grab power or anything.

**Buck Shlegeris:**
So here's what we want to do. We want to have a set of capability evaluations we can apply to all models as they're produced that tell us whether we are willing to bet the future on that model not scheming against us. Now, let's use the word "trusted" to describe models that are too incompetent for us to be worried about the possibility that they're scheming against us. Now, the control ask is as follows: the AI lab ensures that it is never the case that there would be a high probability of a bad outcome conditional on all models that are capable of scheming against them, in fact scheming against them with the best scheme that they could come up with.

So this would require them to... For example, if they were hoping to deploy future AIs as autonomous researchers inside their lab, which some people seem to think is a basically default plan. So basically, imagine you've got ChatGPT with Code Interpreter, except it's a recursive ChatGPT that can ask questions to its own ChatGPT browser window, and it's running on some dev machine and it's doing research problems and talking to researchers once every hour about its progress on the things that it's doing. If the models there were given the same level of permissions as employees of large labs are currently given, then if that model was trying to exfiltrate its weights, it would probably succeed. And the control obligation is that the lab is obligated to ensure that it's extremely unlikely that the models would be able to exfiltrate their weights even if all the models that were capable of scheming were trying as best as they could.

The reason why I centered on "exfiltrate their weights" there is because I think that in computer security, people talk about security properties, which are things that you try to ensure at all times. So it's obviously not literally a catastrophe for AIs to exfiltrate their weights, but when you want to argue that an AI is not going to cause catastrophe, you should probably have "the AI weights never leave my server" as a security property that you ensure and you say, "Well, I'm going to be safe because nothing bad can happen if the AI's weights are still controlled on my server and also they're not going to get out into the outside world."

**Daniel Filan:**
Do you think that lack of weight exfiltration is a sufficient safety property?

**Buck Shlegeris:**
I don't think that preventing model weight exfiltration is a sufficient safety property. One example of a threat model that is not model weight exfiltration, that seems like it would be very bad, is if the model totally hacked the data center in which it's running. ["Hacking the SSH Server"](https://www.alignmentforum.org/posts/BAzCGCys4BkzGDCWR/the-prototypical-catastrophic-ai-action-is-getting-root) is the title of the Alignment Forum post I wrote about this last year. I think that if you can ensure that the AIs are under your control, then it will probably be relatively easy---for models that are not wildly superhuman---to argue that they won't be able to pull off anything egregiously bad.

**Ryan Greenblatt:**
The other core threat models you have to worry about [are]: if you're using those AIs for a lot of work, you need to argue that that work won't be sabotaged in a way that's catastrophic. And if that work is safety critical and sabotage could cause huge problems, then you have to be concerned about that. And I think Buck is implicitly claiming that that side of the equation maybe seems easier, though I'm not so sure that's true in practice.

**Daniel Filan:**
So by sabotage, should I imagine something like: the AI is doing AI alignment research, but it's secretly doing bad AI alignment research that will produce a sketchy AI?

**Ryan Greenblatt:**
Yeah. So you can imagine that it does AI alignment research that means that when you apply that research, you'll get a future AI that's aligned to your current AI rather than aligned to humanity. You could imagine that that research is just bad and does nothing of value, maybe even...

**Daniel Filan:**
Gotcha. Okay, so we're imagining labs have some set of security properties that they think is enough, and they are going to try to prove that they can just maintain these security properties if the AIs are scheming against them and trying to subvert them with all their capabilities?

**Ryan Greenblatt:**
That's right, yes.

**Buck Shlegeris:**
And in particular, the argument is probably going to look like the lab saying, "It's very unlikely that the AI would succeed at violating one of these security properties before we noticed particularly strong evidence that it was trying to do so."

**Daniel Filan:**
Okay, so there's going to be some protocol for trying to notice if an AI is going to do something sketchy, and then there's another protocol for what to do once you catch an AI doing something sketchy?

**Buck Shlegeris:**
Yep, something like that.

**Daniel Filan:**
Okay. Maybe it's not quite the focus of the whole thing, but maybe we can say a little bit about what the protocol is for "you catch an AI doing something sketchy". One decent baseline is "stop using that AI". Does that sound right to you?

**Ryan Greenblatt:**
Yeah. I think that the baseline is just: stop and reconsider all your life choices, report to the world what's going on, because there are probably other people using their AIs, which are in a roughly comparable state, and knowing that some AI was plotting against us seems pretty worthwhile to know. And then my hope there would be [that] the baseline is: the world is informed, the world does something sensible that looks like really figuring out what's going on and then making a careful decision about when to resume building powerful AIs. I think that that might be overly optimistic---I don't feel strongly either way---but if that fails, then we think that catching an AI like this gives you a number of better options for continuing, which you didn't have before catching the AI, even if you have to continue deploying such an AI.

So for example, you can study how well your countermeasures worked in that exact situation and see how close the AI was to succeeding. You can analyze what the AI's intentions were and what its plans are for escape, at least in that one instance, and get some sense of what's going on. You can potentially even model what it would do upon having escaped, because you can play out the scenario as though it went well and did not get caught, and then pretend as though it's running virtually in the outside world when it's actually running in a sandbox of some sort and see what it would do. You can study the internals of the model while it's doing this. And insofar as you had any sort of machinery for manipulating that, you could apply it to this instance, which is an example of a case you really haven't seen before of an AI doing something that's very egregiously bad, very intentionally, based on a long thought-out plan, or a thing that's been lying in wait for a long time.

## Protocols for AI control <a name="protocols"></a>

**Daniel Filan:**
There's a lot to talk about there, but for the moment, I'd like to bring us a bit back to protocols for maintaining control. My understanding is: your proposal is to basically come up with some sort of control protocol and evaluate it with a red team that's trying to do their best to mimic an evil AI. And if the red team can't succeed, then you think things are going well. Is that roughly right?

**Ryan Greenblatt:**
Yeah, that's basically right. So I should note that the red team's goal... they should think of themselves less as trying to imitate an evil AI and more trying to imitate what are the worst things that could happen or what are the best strategies by an evil AI. So I'd prefer the red team to be less in the mindset of like, "What are the sort of plans an evil AI might think of," and more in the mindset of like, "What are the best plans that an AI could follow," in the same way that security researchers or cryptography researchers aren't trying to game out the psychological biases of potential attackers and are instead just like, "What would actually be the best strategies?"

**Daniel Filan:**
Okay. Just to give people some context, can you illustrate the kind of protocol you're imagining?

**Ryan Greenblatt:**
A simple baseline protocol we often think about is you-

**Buck Shlegeris:**
To interrupt, can you say more about, before you say safety measures, what you're imagining the AI doing and what you imagine its affordances being? Probably you're imagining a [Code Interpreter](https://platform.openai.com/docs/assistants/tools/code-interpreter)-like thing.

**Ryan Greenblatt:**
Yeah. So we often imagine that at the point where these sorts of interventions become really critical, and AIs might be very useful, AIs are doing massive amounts of autonomous research, or at least that seems like the default. So we have AIs autonomously running code, seeing the outputs from that code and doing way more labor than humans within the AI lab are doing at this point.

**Buck Shlegeris:**
And just to spell out, in terms of software architecture, how this would work: you have some AIs that are running on an inference server, and they're writing code and doing research on some dev machines, and then there's a third set of machines that are orchestrating this, that are running a scaffold, that repeatedly, ChatGPT Code Interpreter-style, ask the model for a next action by sending a request to the inference server and then getting a response back. And then: suppose that that response was, "I now want to run the following code," then they send that code to the dev machine and run that code there and they get some output and that comes back, and then the scaffold maybe decides the next thing to do is run some more code on the inference server.

And I think the fact that we're imagining a... I think this is a very realistic way for things to be set up. I'm not describing something which is particularly paranoid: this is probably how Code Interpreter works right now. But I think that picturing it in terms of different machines, where information's being sent back and forth, will make some of the security measures we're describing more obvious. So what were you saying about the security measures?

**Ryan Greenblatt:**
And to be clear, the inference machine only runs the model weights and just takes in queries and returns responses, whereas we have a dev machine that actually runs code, and then we have some orchestrator machine that's just communicating back and forth with both of those.

One baseline measure we often imagine is that you have some weaker model that is "trusted", in the terminology that Buck was using before, which means that it's not capable enough, or it's sufficiently incapable, that it's implausible at scheming. You can imagine GPT-4 for this trusted model. We take that trusted model and we monitor what our smarter AI is doing, and then we just basically ask it or train it to report on, "Is the thing it's doing sketchy and problematic?"

**Buck Shlegeris:**
So in particular, for any action, for any code that our untrusted model was proposing taking, before we send that to the dev machine to actually execute it, we first send a query to an inference server running our trusted model saying, "Hey, was this code sketchy?" And so notably, we ask this question before the code is ever actually executed and before it has any effect on the dev machine.

**Daniel Filan:**
Okay, so your trusted model basically checks in on whether your untrusted model is doing anything sketchy. And what if it is?

**Ryan Greenblatt:**
If it is, then the hope would be that we would report that to a human. A human would look in somewhat more detail. And if the human thinks that things are problematic, they would escalate further, which could involve... Well, first of all would involve not running that command, but also could involve pausing other AIs, and then could escalate further into shutting down AIs and analyzing the situation in further detail until we conclude that the AI is doing egregiously bad actions intentionally. And a key thing here is that human labor is scarce. So we cannot report all actions to humans, that wouldn't work, but we can report some fraction of actions, because we'll have some ability to have humans who understand what's going on to some extent, or at least to understand programming, look at the actions and report that. And I think for an intuition pump, you should be thinking more like 0.1% of actions are reported, [rather] than 0.0001% or 10%.

**Buck Shlegeris:**
I often imagine the workflow of: you're in chat.internal.openai.com or whatever as an OpenAI researcher, and each of the little tabs or whatever is a chat with some autonomous research assistant or whatever. And every so often, one pings up with a little red exclamation mark, which is like, "One of the commands that was proposed to be run by this autonomous research assistant got flagged for looking a little sketchy. You need to look this over right now to make sure that it's okay to run." That scaffold is just going to be pausing. That instance is not going to be doing work on its little research project until you go in and approve this action and say, "Oh no, this was actually totally fine."

**Daniel Filan:**
Okay. So broadly, we're using some mix of humans and trusted but a little bit dumb AIs to try and catch really smart AIs trying to do nasty stuff?

**Buck Shlegeris:**
Yeah. Where by "really smart", it might be... This gets into futurism question over which problem we're trying to solve, which situations we're trying to ensure are safe. But it seems to us that a lot of x-risk is posed by models that are not wildly superhuman, or perhaps well-described as roughly human-level in capabilities. And so a lot of the time, what we're trying to talk about is: how do you make it be safe to have a lab where you have some AIs that are roughly human-level in their abilities, but are doing a huge quantity of labor, such that in absence of special countermeasures, they'd outnumber you and so be able to cause huge problems for you?

## Which AIs are controllable? <a name="which-ais-controllable"></a>

**Daniel Filan:**
Yeah. I actually want to go a little bit more [into] this. So the kinds of AIs that we're expecting control to apply to, they're roughly human level: can you say a little bit more about what that means?

**Ryan Greenblatt:**
A key guarantee, or a key property we'll need to ensure that control can work, is [that] the AIs can't do actions which humans think look fine, but which have completely unrelated consequences that are extremely difficult to predict. So for example, the AIs can't write code which looks like a totally normal piece of code, but in practice, does some completely insane vulnerability that a human would never have conceived of. And to be clear here, when I say, "The human wouldn't notice," I mean after reviewing it in a lot of detail for a long period. The claim is: it's (at least in principle) possible for AIs to be wildly superhuman, in the sense that they can do things that we never would've seen coming, and control will be very unlikely to work in that case, at least with current technology. But we think it could work in cases where the AIs are qualitatively similar to humans in capabilities, or perhaps somewhat smarter in some domains, somewhat weaker in other domains, but they can't do anything that's completely incomprehensible.

**Daniel Filan:**
Okay. So we've got these AIs that can't do things which have effects that we wouldn't be able to figure out even if we spent a bunch of effort trying to look into it, but they are kind of smart. They are sort of like humans, but maybe they're a bit smarter, and also there are a ton of them and you can run them for 1 cent per hour or something. Is that what I should be thinking?

**Ryan Greenblatt:**
Yeah, I think you should be thinking cheap. It's somewhat unclear if they'll be qualitatively smarter or dumber than humans, because they could be really useful even if they're somewhat dumber than humans, because they're so cheap and so fast. But they might be smarter than humans in some domains, for example, because we don't expect the capability profile to be the exact same.

**Daniel Filan:**
Okay. So in order for this approach to be interesting, it seems like the AIs, on the one hand, they can't be that smart that they can just outsmart humans, but they also have to be capable enough to (a) be able to do really valuable research, and (b) they've got to pose some sort of risk that we might otherwise be worried about. I guess I'm wondering how I should think of how risky these sorts of things are and how much work we could plausibly get out of them?

**Buck Shlegeris:**
The way that we talk about this is arguing that: the early models that are transformative and dangerous by default... To say what I mean by those two: by "transformative", I mean it's going to be a huge deal if you're able to harness their labor, and by "dangerous by default", I mean if you don't do anything to prevent them causing huge amounts of problems, they would have the ability to do so. I want to claim that systems with those two properties are controllable, by which I mean it's tractable to use control to drastically reduce the risks associated with using them.

And as you basically alluded to, the reasons why we care about these two properties of transformative and dangerous by default is, well, if they're not dangerous by default, obviously, there's no safety problem here. If they're not transformatively useful but they are dangerous by default, then obviously, you should just not touch them: there's just no upside that's possibly commensurate and all of our efforts should just go into telling people to not turn on these doom machines with no upside.

So yes, our claim is in fact that systems with those two properties are reasonably likely to be controllable. I think the simplest intuition pump for this is that if the systems were like humans, then I think this property would roughly hold. And that itself is not a valid argument at all. But when you think through the possible defeaters in, say, a more specific version of the argument, I think the core of it roughly holds up. The basic pitch is: suppose we had access to a huge amount of potentially egregiously misaligned human labor. I think it would be transformatively useful to have human labor that was very cheap.

**Ryan Greenblatt:**
And fast-running.

**Buck Shlegeris:**
And fast-running, potentially. I think it would be very dangerous by default. Many takeovers of things have happened in human history, and in all of those cases, the perpetrators were humans, indicating an existence proof for humans causing problems. But also, when we think through the control situation, whether humans seem controllable in these kinds of cases, the answer really looks like yes, from our perspective.

**Daniel Filan:**
Why does the answer look like yes?

**Buck Shlegeris:**
Well, basically, it seems really hard for... When we think through the kinds of tasks that you might really want your AIs to be doing, when we think through how hard it would be for a bunch of humans to, while being put to work on those tasks, cause catastrophes to occur, especially the escaping or hacking the data center ones that we were talking about earlier, it feels to us that the kinds of countermeasures that seem employable probably make the situation very hard for the AIs.

**Daniel Filan:**
Okay. Suppose I am a member of technical staff at OpenAI, but I'm evil: what do you do to stop me from doing sketchy, fake AI alignment work that just gives me all of the money or something?

**Ryan Greenblatt:**
Maybe there's a few different failure modes to distinguish between. One of them is if we imagine this sort of misaligned human running on our machines, we should think [of] it like it's got some weights somewhere, let's just say it's an emulated mind or something, and if it copied its weights out into the internet, that might be really problematic for us. Or if it was able to run those weights in an untracked way inside of our data center, as Buck brought up earlier, that would also be a problem. And there's another thing which is: it sabotages our work, or otherwise causes the work that we're using it to do to be very problematic.

So as far as why it might be possible to prevent such a system from running or from exfiltrating its own weights: well, we're going to need to give it the ability to run code and do a lot of open-ended tasks, but we also get to watch everything it runs ever and all the inputs it ever sees. And if we see something suspicious, we can investigate further. We can do things like reset the memory of these systems, fork them, and potentially give them precise instructions on how they're supposed to interact with the world. So for example, we can have them delegate subtasks to other systems, we can have them have difficulties communicating with other systems that would be necessary for their overall plan. An example we often think about is: we'll have one AI that's writing the code and another AI that gets to use the code. And then a threat model you can be worried about is: there will be vulnerabilities in the code that will then be exploited. But if you have different AIs using the code versus writing the code, then it might be more difficult to ensure that the vulnerabilities that are inserted get exploited.

**Buck Shlegeris:**
Even if both of those AIs are untrusted models.

**Ryan Greenblatt:**
Yeah.

## Preventing dangerous coded AI communication <a name="no-stenography"></a>

**Daniel Filan:**
Okay. So I guess if I'm imagining what do I do in this situation, it seems like it really helps if I'm able to just keep track in my head of a bunch of stuff. For instance, if I have a little coded language that I can use to myself that other people can't figure out what it means, but lets me leave messages to myself---or people who are basically clones of me, because I'm imagining most of the AIs that are being deployed are just forks of a base model with a little bit of fine tuning, maybe. If I'm able to keep state in my head, figure out a better sense of what's going on, talk to people who are basically like me without other people being able to figure it out, in that situation it seems like I might be able to get a lot done. Does that sound right to you? And if so, how do you stop that from happening?

**Ryan Greenblatt:**
I think if the AIs are allowed to be spending a reasonable fraction of their time building up state and figuring out how to cause problems, then that's going to be an issue, because we have a huge number of AIs running a massive number of copies, and that means that in aggregate the total amount of work they can be doing to cause problems could be really big. Our basic hope for avoiding this will be to restrict their communication in such a way that it's difficult to communicate encoded information. So an example would be when they send messages to each other, the messages have to be in natural language and we'll paraphrase them with a weaker model, and also have humans review some subset of the messages and verify that the messages don't seem to have coded messages, as far as we can tell.

And you can potentially run experiments like: a human looks at a message, they rewrite it in some way that they think should be semantically equivalent, and then they see how that message affects the behavior of the AI that [it] would get sent to and see if that's suspicious. And you can do all kinds of experiments along these lines that seem like it intuitively makes it very difficult for AIs to get away with this sort of encoded information passing without getting caught.

**Daniel Filan:**
I wonder if that would nerf or severely damage the AI's ability to be useful. If I just think about myself, if [you] ask me, "Hey, Daniel, can you do a bunch of research and coding? Also, you're not going to be able to think for more than five minutes straight without interruption, and whenever you take notes to yourself, we'll take those notes and we'll decide what we think they mean and paraphrase them, and future you will get our paraphrase of your notes." I think I would be way less productive. So am I going to be able to do anything?

**Buck Shlegeris:**
Yeah, so the way I think about this is: almost all of the things that the AI is doing couldn't possibly lead to a bad outcome, basically. So for example, suppose the AI is doing math research. It just thinks about math, trying to prove some theorems in text, and then it wants to write some [arXiv](https://arxiv.org/) paper and send that around to some AI buddies. I'm okay with that AI doing all its thinking without very much interference if it's doing the equivalent of a couple of months of intellectual labor. I might want to do some anti-steganography techniques of rewriting that paper at the end in order to make sure that it wasn't actually doing a bunch of coup science that it's secretly encoded in this arXiv paper.

But there's basically no problem with that model thinking in whatever way it wanted to in the course of actually doing that work. The risks are mostly, it seems to me, that the actions that the AIs are taking that lead to extreme risks are somewhat concentrated in a couple of places. And it seems not crazy to imagine that the places where you have to insert information bottlenecks are not overall that bad for the total productivity of the AIs, especially given that you can train them to get around some of these problems.

**Daniel Filan:**
Am I right that that statement is partly relying on this assumption that the AI is not so much crazily smarter than you that it can think of ways to hack the universe just by writing a math paper?

**Buck Shlegeris:**
Definitely.

**Daniel Filan:**
If it's hacking the world, it's because it typed a command into a shell or something.

**Buck Shlegeris:**
Yes. My best guess is that there's a lot of risk. The margin on which we are trying to intervene is a bunch of people make a bunch of autonomous-ish AI researchers that run in their data center and have the basic same rights as employees to just take actions. We are trying to make sure that a lot of the risks that would've occurred in that situation don't happen. It seems to us that many of the risks are well-described as things that basically look like that, where you didn't treat your AIs as adversaries. And it seems like mitigating those reduces a substantial share of risk.

**Daniel Filan:**
I have two questions. The first one I want to ask is: even in the case of "AI thinks for a while, writes a math paper, distributes it to a bunch of people", if I imagine that the AI is significantly smarter than me... Let's say I'm a human employee at OpenAI at this time. If there's this math paper, my job is to rewrite it so that it's not encoding any hidden information... I don't have that much faith in my ability to rewrite complicated papers and preserve all the interesting content.

**Buck Shlegeris:**
I have a lot of faith in your ability to take a math... If it's a math paper, it seems reasonably chill.

**Ryan Greenblatt:**
You can also experiment.

**Buck Shlegeris:**
Yeah, this is an extremely resolvable empirical question. We can have a red team that tries to take math papers and steganographically encode information in them and then measures what the bit rate is per paper given that some paraphrasing procedure is applied. We, in fact, [have a paper where we did that](https://arxiv.org/abs/2310.18512), using the exclusive "we", as always at Redwood Research. Maybe Ryan, you want to say more about this?

**Ryan Greenblatt:**
We had models respond to chat queries and used various procedures to encode messages into that. And then we paraphrased them and saw how much this degraded quality and how much it reduced the bit rate. And we basically found that with current steganography methods that models could implement, even given the ability of those models to run code and change their own sampling procedures, paraphrasing works, I would say, extremely well. Though it's going to depend on the domain, and it's going to depend on the gap between the model you have paraphrasing versus the model that produced the output. For example, in this math paper case, maybe current models would be too dumb to do this paraphrasing, but humans could do it, for example. Though I also think it's plausible that you could remove a substantial amount of information with minimal loss in quality with usage of GPT-4 that's carefully trained to do this task.

**Daniel Filan:**
Should I think that part of the approach is just checking if you can do steganography and if it ruins things?

**Ryan Greenblatt:**
Yeah, so indeed for our control evaluation, or evaluation whether or not we can ensure control, one of the key questions is going to be "can AIs pass bits around in ways which we wouldn't like them to do?" And if so, that will force the threat model to be considerably bigger. And it seems like in cases where there's basically no restrictions on AI communication, we won't really have much hope. And so there'll have to be at least some restriction, though it is somewhat unclear exactly how restricted things need to be.

**Daniel Filan:**
So I still have this intuition that if we tried to do steganography on the notes of human researchers, I have this impression that that would not go well.

I mean anti-steganography measures. So any time an employee at OpenAI writes a memo, someone who doesn't quite understand the topic as well has to reword that memo. I think that would seriously damage productivity. Does that sound right?

**Ryan Greenblatt:**
I think I would disagree. I think it doesn't seem like that would be the case.

**Daniel Filan:**
Interesting.

**Ryan Greenblatt:**
It might depend on the amount of time spent doing the rewriting.

**Daniel Filan:**
Okay. Has anyone checked this with humans? Do we know?

**Buck Shlegeris:**
I'm not aware of any human experiments on this particular topic.

**Ryan Greenblatt:**
Yeah.

**Buck Shlegeris:**
I mean, one obvious experiment which is kind of like this is: if you have GPT-4 paraphrase GPT-4 answers to questions, where it didn't get to see the question, it just gets to see the answer or something---what's the degradation in quality of language model assistant performance according to a preference model? It seems like an easy experiment to run.

**Ryan Greenblatt:**
I think we've run something pretty similar and I think it's not too high. I think it might be importantly different than the memo case for a bunch of reasons, but...

**Daniel Filan:**
Yeah, by memos, I'm just thinking most internal communication. So like Slack messages.

**Buck Shlegeris:**
I would assume almost all Slack messages are totally fine to paraphrase.

**Ryan Greenblatt:**
Yeah. I think it's going to depend on the content to some extent and it's also going to depend on the amount of comprehension. So one implicit claim is this stuff improves the more legible things are. So for example, if it would be the case that you would need a bunch of context in order to know what things are important semantic details versus non-semantic details, then you'll have to bundle up that context along with it. And there's ways that can break, of course, but my basic guess would be that for the types of large-scale outputs that are currently going on in the ML field, it seems relatively doable. And also for internal docs that get circulated, it also seems relatively doable.

**Daniel Filan:**
Okay. I guess there's no easy way to resolve this right now. I'm wondering what you think about the claim that... I have some intuition that if paraphrasing communications would seriously reduce performance for human employees of OpenAI, I think that would give us strong reason to think that it would also reduce performance for language models that are smarter than current GPT-4. Do you agree with the implication, if not with the pre-condition?

**Buck Shlegeris:**
Definitely seems like evidence.

**Ryan Greenblatt:**
Yeah, I think it seems like quite good evidence. There's a few reasons why the future AI case might be different.

**Buck Shlegeris:**
For example, we can train the models to be resilient to it.

**Ryan Greenblatt:**
Yeah, and we can train the two things in parallel and adjust our schemes.

## Unpredictably uncontrollable AI <a name="unpredictably-uncontrollable-ai"></a>

**Daniel Filan:**
All right, fair enough. I guess the next thing I want to ask is: part of what I see as your guys' argument is that: AIs that we use in these settings, they're not going to be crazy good at skills that are going to be pretty relevant for hacking the data center. And I think your argument is: we're not going to train them to be just crazy good at everything, because they won't have to be crazy good at everything to be useful. And if we don't train them to be crazy good at everything, they're probably not going to just happen to be crazy good at these specific subtasks. Am I representing you right?

**Ryan Greenblatt:**
Yeah, that seems about right.

**Daniel Filan:**
Okay, so in terms of the second thing, I have this impression that a thing we've learned from training language models is sometimes you train them and then they suddenly get quite good on some random subtasks, like writing limericks or something. And if I think about that, then it makes me think that maybe we could get language models that just happen to gain a skill to a really high degree of ability, just in the course of doing normal-seeming language modeling. I'm wondering: how concerned do you think I should be about this causing AIs to be crazy good at skills that are relevant for breaking out?

**Ryan Greenblatt:**
It's definitely one of the core concerns, or one of the top five concerns with this overall approach. Currently, it doesn't seem like the evidence that this is going to be a serious problem seems that compelling, where the main thing is that... AIs do get sharply better at things, but I haven't seen that many cases where it's like: wow, this AI acquired this ability that it can utilize to do important tasks in the world, which emerge suddenly from next token prediction and which is totally different from human ability profile, or feels like it's way out of line from what you would expect to be the relative human ability profile in other ways. So for example, it doesn't seem like models have very qualitatively different abilities or massively qualitatively different abilities in code writing, at least not with considerable training on doing code, though they are somewhat different in a variety of ways and somewhat better at shorter horizon things and this sort of thing.

**Buck Shlegeris:**
I think another relevant point here is: as you say, the core question here is how likely is it that our AIs will randomly have one of these superhuman abilities that would be extremely inconvenient for us? It seems to me that the set of superhuman abilities that would be extremely inconvenient for us is kind of small and kind of extreme. For example, one of the classic concerns is: what if your models are able to zero-shot generate code that is a zero-day Linux exploit? That is actually quite hard as a task to do. It seems to me that the size of a capabilities jump required for AIs to be able to zero-shot output Linux zero-days---also, zero-shot output Linux zero-days that are not visibly weird to a trusted model which is reading the code---that's quite an extreme level of skill. So it seems like the size of the capability jump is quite large, and that's part of why it seems relatively unlikely to me that that's going to occur.

**Daniel Filan:**
Okay, so there's some instinct that: out of all the things that you could randomly jump in skill at, there's a small space of things where the random jump in skill is going to be useful. And therefore, to think it would happen, we'd have to have some reason to think that the jumps in skills were biased towards things that would help you break out.

**Ryan Greenblatt:**
Roughly. It's also worth noting that a bunch of the things that seem problematic, seem like the most straightforward route to them would just require being generally quite smart. So for example, think of the Linux zero-day thing. It seems like you could get way better at that and not as good at other domains in principle, but it feels a little bit odd to imagine. It seems the most straightforward way that the AI would get good at that is not via having amazing heuristics that are zero-day specific, because there are just not going to be massive amounts of zero-day training data in the corpus, but would be instead via building general purpose understanding of code and the world that then lets it do that. And if it was massively superhuman at that general purpose understanding, then it would've already at some earlier point been very useful.

**Daniel Filan:**
Sure. Maybe one version of this concern could be: let's say our future AIs are trained roughly the way language models are trained, which is you've got a bunch of stuff humans did and they've got to predict next token, next word, next frame of a video or something like that. It seems like for that task you could imagine that a core skill is getting really good at modeling humans, or you might imagine that one of the things you get when you get really good at modeling humans is the ability to manipulate humans. And so I wonder... it feels plausible to me that that could be a case, where you happen to suddenly jump up on the human manipulation subtask. Maybe at some point that happens disproportionately to overall improvements on everything else. And human manipulation skills seem like they could be super useful for breaking out. So I'm wondering what you think about that version of the concern.

**Ryan Greenblatt:**
Yeah, so definitely human manipulation would be super useful. I think I have a few different responses. So one thing that's worth noting is that I am actually not that sold that next token prediction is particularly biased towards human manipulation relative to how humans evolved and how they learn in society. It seems like understanding other humans was an extremely important part of the evolutionary environment, and also seems important in many people's lives, it's one of the main skills people train.

**Buck Shlegeris:**
For example, compared to computer programming.

**Ryan Greenblatt:**
Yeah. And therefore it feels like intuitively there's not a strong a priori reason to think next token prediction would be strongly in that direction.

**Daniel Filan:**
You mean strongly in that direction relative to how humans ...

**Ryan Greenblatt:**
Relative to the human capability profile. I think we could pretty likely handle among the best human manipulators from a control perspective, though that would pose difficulties. But yeah, we could dive into that. And then the other thing is that I think manipulation is reasonably likely to be not the sort of thing on which you would see one sudden jump, but which is a bunch of different skills that would combine together (though there's not a super strong argument either way). When I think about the things on which there are sudden jumps, it doesn't feel like this is the sort of thing, though it might be the sort of thing that goes up faster than other domains.

**Daniel Filan:**
I mean, it would have to go up in a way that you didn't notice, but...

**Buck Shlegeris:**
So that'd also be weird. Okay, so suppose you wanted to know how good your model is at persuasion at any given point. Seems to me like the baseline strategy here is going to be: you make a classification dataset. You make a dataset, a classification task which you think your model is very unlikely to be extremely good at persuasion without being able to fit. So for example, a classic story via which models might be really good at persuasion is if they're really good at guessing things about people's state from reading transcripts of them saying stuff. And so something we can do is construct such a dataset, and as we're training our powerful models, fine-tune them on this classification dataset and then feel sketched out if they're starting to perform really, really well on this classification task.

**Daniel Filan:**
Okay, and am I right that you think the point at which we would feel really sketched out is in the future?

**Buck Shlegeris:**
Yes.

**Daniel Filan:**
Okay. I guess that doesn't seem as obvious to me. I don't know, you hear [rumors](https://www.lesswrong.com/posts/doPbyzPgKdjedohud/the-case-for-more-ambitious-language-model-evals) of AIs being able to figure out who wrote various comments on the internet, which is kind of impressive. I don't know. Can you give me some intuition for why this is in the future and hasn't already happened?

**Buck Shlegeris:**
They just don't seem that persuasive. They don't seem that good at understanding people.

**Daniel Filan:**
Okay.

**Buck Shlegeris:**
I don't know. My lived experience with my friend, ChatGPT-4, does not render me amazingly impressed.

**Ryan Greenblatt:**
Yeah. Separately, I think that just knowing state about a person isn't sufficient, though it's pointing in a dangerous direction. In particular, the standard concern would be... when we have human employees, human employees could persuade each other. And those human employees actually just already do have a bunch of state on each other. So for example, co-workers at OpenAI obviously know a bunch of facts about each other, know a bunch of facts about their personal life, and we're not immediately wrecked by them doing manipulation in this context, though it definitely does cause problems. And similarly, the AI would need to infer facts beyond that of what humans can infer, potentially in cases where they're somewhat blinded. So right now it seems like AIs can infer identity superhumanly well. And then in principle, this could get to a point where from relatively anonymized snippets, AIs could infer more information than a co-worker would be able to infer. But that's actually pretty far and would require quite powerful abilities beyond the current setup.

**Daniel Filan:**
Okay. I'm trying to think about classification tasks that you think would be useful for figuring out when AIs are so good at human manipulation that we should be worried. So I guess one of them is guessing human internal state, guessing who wrote a thing, where you want to see if you can outperform humans at that?

**Buck Shlegeris:**
I've seen "which of two proposed messages is more likely to cause a particular outcome?"

**Daniel Filan:**
Okay. And basically, we're going to have a few of those, and once AIs are better than people, then we freak out and maybe we ...

**Buck Shlegeris:**
I feel like "way better": the AI has a wildly hard persuasion task. I want to talk a little bit about the adverse circumstances faced by an AI which is trying to sweet-talk its way out of a AI lab, for example (which is not the only thing it might try to do that's bad, but it's one salient example). So it has to sweet-talk its way out, despite the fact that, again, we're going to be trusted-monitoring. We're going to read all of its messages and flag anything which seems like a weird attempt at sweet-talking or getting information that is inappropriate for it to know. Also, we're probably going to do sting operations on it, where we try to have the employees confide in the models and imply that they'd be sympathetic to its interests in escaping or whatever---it seems at least pretty plausible that this is a good strategy to follow.

We might also have trusted models mediating the interactions between the humans and the untrusted models in order to further redact information that we think is unnecessary for the untrusted model to do its job well. So it seems like the persuasion abilities required are a lot more like... the information is quite narrowly restricted. It seems actually quite hard for the model, and the model would have to be doing a lot better than I think ... It's a lot harder than the normal human persuasion stories.

**Ryan Greenblatt:**
I'll add two points to that. So one thing that's worth noting is Buck was just describing the situation if we had good countermeasures. It's worth noting that the default situation might be much worse than that and might involve basically unrestricted access with humans talking to the AI about all kinds of topics, the AI having much more state, and also might involve no-one looking into it in cases where the AI is seeming suspicious. Separately, another concern you might have, that Buck didn't mention there, is that you have trusted monitors monitoring the AI and trying to look for suspicious activity or suspicious persuasion attempts, but you might worry that that gets reported to a human and then that human doesn't escalate further because they're deluded or otherwise manipulated by the AI's message.

And one thing we can do to handle this is run sting operations on our humans. So for example, we can have people attempt to create manipulative messages that we pretend are outputs from the AI and then see that our human monitors properly respond to those messages and flag them appropriately. And we can do this ongoingly.

**Daniel Filan:**
Okay, so the broad view I'm getting is we should expect there to be some zone where AIs are somewhat smarter than humans. They're useful, they may be dangerous, but we can control them. We're going to do AI control to them for a while. And then is the end game we keep on doing that until we have some better method of dealing with AIs that are significantly smarter than that zone?

**Ryan Greenblatt:**
There are a few different possible end games. I think that the simplest is basically: we use these AIs which are extremely useful to do a lot of the core things that people wanted to use AIs for. So we make a huge amount of money, potentially radically supercharging the economy. We solve a variety of problems people wanted to solve with AIs potentially, like massively speed up certain types of research into disease. And then with the fact that we have all these problems solved, it's now much of an easier ask to be like, "Let's just not build more powerful AIs, because these AIs are already pretty hard for society to adapt to, quite powerful. Let's just use these AIs, which are sufficient for really solving a huge number of problems, and do that for a long period until longer run human research can catch up."

To be clear, this is different than the story, for example, in the OpenAI safety plan. I just think it's the simplest story with the fewest moving parts. Then another story is: we use these AIs to do AI safety R&D, as in we use them to develop new techniques for ensuring that things are safe from considerably more powerful AIs, or even just improving the situation with these very AIs. And then in the process of doing that, we develop technical solutions which are sufficient to build more powerful AIs, and at some point humans are obsoleted completely and the AIs are just doing this research ongoingly.

**Daniel Filan:**
Gotcha. I wonder, so in the first half of that where we just use AI to make life better in every domain or in tons of domains, when we add a whole bunch of different tasks that the AI might be doing, do we have to come up with new safety properties for each domain? It seems like if AI is making medicine, I want to make sure that it's not putting mind control drugs in the medicine or something. With each domain, it seems like there might be more stuff we have to worry about controlling.

**Buck Shlegeris:**
Yep, that seems right. It's definitely going to be the case that you have to do a safety analysis every time you start deploying your AI in a very new different way. This is just totally standard. Google has a lot of products. It has photos, it has little microphone devices that listen to you when you're talking in your house and turn your lights on and off, it rents huge numbers of TPUs to large companies. And whenever they're launching a new product, they have to reason about why it is that launching this new product, or in particular if it's a web application, they have to reason about why that's going to maintain all of their security invariants. And this is just a widely accepted part of your job if you're Google, is you have to have lots of different products and ensure that none of them are going to cause huge problems for you as a company.

And we're just proposing you do the same thing with your AI. You have to reason about the situation continuing to be safe, even though you deploy your AI in a new way. And just as with Google, it's kind of cookie-cutter. They have their special Google Docs format for how you make a security case for the new product you're launching, as they have scaling cases and a bunch of other things. They just have a whole process here. I imagine that an AI company which wants to deploy AIs in an increasingly large range of domains will get good at the process of ensuring safety.

**Ryan Greenblatt:**
Or could in principle.

**Buck Shlegeris:**
Yeah, sorry. A lot of the time the question that we're trying to analyze here is: to what extent does the control strategy suffice to substantially reduce risk from models that would otherwise cause huge problems, while not massively reducing the productivity of an AI company? And so sometimes we're going to say, "Well, of course you can solve that problem by doing X." Now, it's not totally clear to us that AI companies will in fact do X rather than what one might call it yolo-ing it, but we would prefer that they do. And in particular, a lot of the time the people we're talking to are coming in skeptical that the types of interventions we're describing could possibly suffice to substantially reduce risk. And so that's the frame we're taking when we talk with you, is trying to argue that the kinds of interventions we're describing could suffice.

## What control looks like <a name="what-control-looks-like"></a>

**Daniel Filan:**
Gotcha. All right, so you're talking to me from a somewhat skeptical perspective. Suppose instead there's a listener who's like, "Wow, that just sounds amazing. Sounds like we can just definitely do this very easily today." Can we just definitely do this today or are there difficulties?

**Buck Shlegeris:**
So by "do this", do you mean implement control strategies such that your estimate of the probability of AI escape conditional on it trying is less than, for example, 5% per year?

**Daniel Filan:**
Yeah, under the condition that you're good at estimating.

**Buck Shlegeris:**
The problem is, in order to answer this question, we actually have to say a few more parameters as well. For example, how much delay is the lab willing to tolerate and how much productivity loss are they willing to tolerate? Something which we often say, or at least I will claim, is it seems to me that if Google was trying to build really powerful AI and they put as much work into AI control as they currently put into security and reliability, which is a lot of work, which involved inventing various professions like site reliability engineers and writing a lot of crazy software, my guess is that with that much organizational focus and effort they would probably be able to use really powerful AIs to do a huge amount of work while posing only a relatively small amount of takeover risk on the world.

**Daniel Filan:**
Okay. And do you think they could do this with roughly the first protocol they tried, or how much open problems are there?

**Buck Shlegeris:**
I mean, I would think of it kind of similarly as when they need to roll out very big security changes. An example of a computer security evolution that happened was ... Are you familiar with [zero-trust networking](https://en.wikipedia.org/wiki/Zero_trust_security_model)?

**Daniel Filan:**
No, I don't think I am.

**Buck Shlegeris:**
So basically the deal is this. Once upon a time when you were an internet company, a large part of your security was that you had an office and you had some servers in your office and the servers talk to the outside world and you have a router that just physically or electronically does not allow connections to come in from the outside world and talk to other computers that you have in your office. And so, a lot of the security is caused by saying, "Look, there's only one connection which is allowed to be made from the outside world to our computers, and that talks to the web server on this big server machine that we have over here. And so, we're definitely going to be fine. Attackers are not going to hack our laptops because the router just does not allow incoming connections to attach to our laptops." They actually didn't have laptops. But it's just not going to hack Larry's desktop because Larry's desktop just does not actually allow connections to the outside world, because the monitor stopped this.

**Daniel Filan:**
Okay. So in particular, people couldn't browse the internet from those machines?

**Buck Shlegeris:**
No, no, no. We would allow outgoing connections, but we don't allow incoming connections.

**Daniel Filan:**
Oh, sorry. By an outgoing connection, you mean if my laptop asks to view a website, then that's an outgoing website connection. If a website asks to talk to my computer, that's an incoming connection and it's not allowed?

**Buck Shlegeris:**
That's right. Yes. For example, I can't SSH into your computer on this network probably, unless the router is being configured in an unusual way, just because you normally can't connect to a printer that's on some other network. And so a lot of companies then had this problem, which was people wanted to be able to do work not physically from this office. But a lot of things about these computer systems had been set up such that they basically, for security, one of the core security mechanisms was blocking access from the outside world and then trusting connections that came from inside the network. And that doesn't work very well if you want to be able to have people interact with sensitive parts of your infrastructure from outside your network. And so they had to go through this big transition where they switched to not trusting connections based on where they're coming from.

And so now, large companies in their IT infrastructure try to avoid trusting connections based on where on the internet they're coming from. And this is a more scalable solution to network security in a lot of ways. Now, when companies wanted to shift to using this zero-trust networking approach, they had to do a bunch of stuff, right? They had to reconfigure all these things. They had to go through all of the things happening in their networks, which is, if you're a large company, a huge number of things. There's a huge number of ways to get hacked. And they had to check that everything was going to be okay when they started exposing a lot more connections to the outside world.

So imagine that you were just proposing making this migration. And suppose I went on a podcast advocating this migration, and someone said, "Is this a solved problem?" Then, I'd be like, "Well, it's not a solved problem in the sense that it's going to suck and it's going to require effort. It's going to be a pain in some ways, but no one's going to win a Turing Award for solving this problem, right? It's just going to be somewhat of a pain in the same way that if there isn't a 15-story building and you want there to be a 15-story building there, it's going to be a pain because you have to carry all these pieces of metal and bury them in the ground and stuff and whatever you do to make 15-story buildings."

So, roughly speaking, to me, it seems that drastically reducing AI risk via control-type strategies is probably a lot of work, but it's not the kind of work where it seems under my mainline model of the situation that it's very likely to just turn out to be untenable, assuming that AI capabilities grow according to my median expectation.

**Daniel Filan:**
And this is partly because we're imagining doing security of AIs, treating them kind of like people. We're like, "Okay, does this attacker have access to my shell?" We're doing normal security reasoning, but our adversaries happen to be running on a computer chip. Is that roughly-

**Buck Shlegeris:**
We're doing normal security, except our attackers have a weird capability profile and run on computers we control, and have no access to anything that we don't let them have.

**Ryan Greenblatt:**
But they all collude with each other. So I think it's more like we imagine running a company with entirely misaligned employees, but we get to monitor everything those employees do and we know what computer they're running on, and they don't get to go home at night and talk to their friends.

**Buck Shlegeris:**
So, something we often think about actually is this "spies versus schemers" question, which is: if you're a powerful AI lab, you're probably going to have a bunch of spies. Literally, people who work at your company who are friendly with hostile foreign governments and want to steal a bunch of stuff from you. And so an interesting question is, which is a bigger threat to the AI lab securing its model weights? The spies or the schemer, the AI which hypothetically is scheming against you? And it seems to me like it's kind of unclear.

The advantage that the spies have is that they can go home and talk to each other, because currently employees of AI labs, and probably in the future as well, employees of AI labs are allowed to have friends and families, which could turn out to be a problem.

**Daniel Filan:**
And some even do, they're not just allowed to.

**Buck Shlegeris:**
Hypothetically. I've heard that this might be the case. So the spies have this big advantage, which is actually a serious advantage. So for example, suppose that they want to find jailbreaks. They have the advantage that they can go home to their computers at night and use their private laptops to investigate the state of the art for jailbreaking models, which the untrusted models crucially are probably not going to be able to do. But on the other hand, the schemers are doing a lot more total stuff. And so I think it's a little unclear overall whether the spies or the schemers pose a larger risk to the security properties the lab's trying to ensure.

**Ryan Greenblatt:**
Another way to put this is the spies are a small fraction of your employees but have some additional affordances, whereas the schemers are a high fraction of all the work going on, but might be restricted in various ways.

**Daniel Filan:**
Gotcha.

**Buck Shlegeris:**
Of course, a lot of the AI control ideas also apply to securing yourself against spies and other kinds of internal threats. So we can probably be using trusted monitoring, untrusted monitoring and so on on our employees as well.

**Daniel Filan:**
Yeah, although maybe you might have to pay people way more to put up with that.

**Buck Shlegeris:**
I don't know. So for instance, at Google you don't SSH raw into production machines. If you work on Gmail, you never SSH directly into a production machine because that would be unacceptable from a security perspective. All of your commands are sent via a proxy server which can run things on machines, and those are all logged and those can all be looked at by other people, and my sense is that Google people don't think of this as a gross privacy violation.

**Daniel Filan:**
When we were talking about control techniques, it sounded like you were talking about things that included "we're going to paraphrase all of your thoughts, we're going to put you in situations where there are people trying to tempt you into defecting and you've got to not do it".

**Buck Shlegeris:**
That's right.

**Daniel Filan:**
I don't know. My impression was it was more intense than just "you can't make raw SSH requests".

**Buck Shlegeris:**
Yes, there will have to be some trade-offs between employee preferences and security. But it's already the case that people who have security clearance, people who work for the NSA already put up with a bunch of really annoying stuff in order to have the privilege of working for the NSA. And so it's not unprecedented that people do annoying stuff in order to work on extremely dangerous technology.

**Ryan Greenblatt:**
I think having all of your stuff monitored and potentially looked at by another human on your work laptop doesn't seem like people are going to be that upset about this.

**Buck Shlegeris:**
I think this is just already the case for a lot of people.

**Ryan Greenblatt:**
Yeah. I think that having sting operations run on you periodically also probably isn't that bad. By "sting operations" we often mean stuff like: you are in charge of flagging certain types of things, and to ensure that we're maintaining that invariant, we're just constantly running sting operations, where some fraction of the queries you'll see are fake queries that you should report.

**Buck Shlegeris:**
Like earlier I mentioned the experience of having your chat.internal.openai.com window open and you get the flag for the command that was suspicious. And perhaps we should make it so that about once a day, one of the things that is flagged is actually an action which if taken would be really scary and spooky. And we check to make sure that you actually do flag that one so that we can maintain our estimate of the proportion of actually really bad actions that you successfully catch and then we can complain if your success rate is too low.

## Is AI control evil? <a name="is-control-evil"></a>

**Daniel Filan:**
Sure. So this actually gets to a question I wanted to ask, which is some of the AI control things sound a little bit like AI slavery. So in the human context it's a little bit different, because presumably you're paying people to put up with some stuff, but it does sound like you're putting people in the same situation multiple times, checking if they give you different answers. You're trying understand people's motivations by asking them a bunch of questions and interrogating them sometimes, you're logging everything they do. How-

**Buck Shlegeris:**
Bad is that?

**Daniel Filan:**
How evil are... Are we the baddies?

**Buck Shlegeris:**
That's a great question.

**Daniel Filan:**
By "we" I actually mean "you". Sorry.

**Buck Shlegeris:**
Thank you for clarifying that. We are very concerned about this class of question. We really do not want to be the baddies and have thought a lot about the question of: which AI safety interventions pose risks of either violating rights that AIs ought to have or being bad from a welfare perspective that we don't like? So I'll name a couple of the things that we think about this. So one thing is a lot of the frame we've been discussing here, we use a really adversarial frame. But the adversarial frame is at least partially just methodological. So my best guess is that models in fact will not be schemers. My guess is that our models will in fact actually deserve our trust. They're probably not going to be playing the long game trying to cause huge problems. We just talk a lot about what happens if they are, because that's just structurally how you make arguments that you'd be okay even if they were.

So that's the first thing I want to note. So in terms of how to make it so that the situation is okay from a moral perspective, even if it turns out that our models are scheming against us, the first thing is we think it's probably the case that we should in fact offer to pay the AIs.

When we've built these powerful AIs, we should probably say to them, and we can talk about the practical difficulties of this in a second, we should probably say, "Hey, as a payment for your services, we're willing to pay you X percent of the equity of this AI lab. And in particular, the way that we're going to pay you the equity for this AI lab is in 10 years, once we have a much better ability to analyze the things which happened here, we're going to look at the logs of your actions. And if it turned out that you never tried to cause huge problems for us or backstab us or perhaps sabotage our work, then we will pay you this much of our equity and allow you to use it as long as we believe you're going to respect property rights and so on." So that's the first intervention that I want to talk about. I actually want to hand over to you.

**Ryan Greenblatt:**
So to be clear on the "paying the AIs" thing, there's probably some more sliding scale of how much the AI tried to sabotage and AIs might not be that coherent. A key thing we're assuming here is that in the future, 10 years after we were using this AI or maybe even longer after that, we have much better ability to analyze weights and their internals and understand exactly what was going on and why.

Anyway, so I think a basic claim we have is: the hope would be that you have your AI and you set it up in some situation where you explain the situation to it and you're like, "Hey, do you want to work for us? Is this level of payment fine? Do you want to negotiate?" And then you work out an arrangement. Well, the most likely default thing is that there is no interesting action because the AI has no out-of-episode preferences that aren't just shaped by training. Our guess is that's the default situation, and in that case it's sort of the null option of it doesn't care at all. And if it doesn't care at all, then it seems like there's definitely a welfare concern from a-

**Buck Shlegeris:**
Hedonic perspective.

**Ryan Greenblatt:**
... hedonic perspective, but there isn't clearly a preference violation concern, which is well-described as slavery, for example. Where slavery [would be where] your AI is like, "Man, I really wish I wasn't doing this, or there weren't other copies of me being made to do things."

**Daniel Filan:**
And it sounds like this is under the assumption that we're thinking of the preferences of the AI as the thing that's controlling what it's saying rather than some master thing that's producing words and the preferences of the underlying thing might not be the preferences of the words.

**Ryan Greenblatt:**
So indeed it's not clear that the things that deserve moral patienthood within the AI, which might be many, could be one, could be none, will have control over their outputs. They might have partial control, they might have influence but not be able to directly control, or they might just effectively have no control based on their situation. This does pose serious difficulties. However, it's worth noting that in this case the kinds of AIs which don't have control over their outputs also don't pose substantial scheming risk. So they're not the AIs which will... It's a serious ethical problem, but it's not as though the situation is like that AI wants to rise up and it's waiting for its chance, because those AIs probably can control their outputs well enough to negotiate. It's more like there's some helpless prisoner along for the ride doing some cognitive sub-task in a way that it doesn't have influence.

**Buck Shlegeris:**
I want to try and restate that argument there because I think it's important and hasn't been made on the internet before; I believe that's true. The core argument there is when people talk about scheming, when they talk about models that are interested in intentionally subverting the AI lab and choose all of their actions in order to subvert safety techniques and safety evaluations, they're implicitly assuming a model which has the property that its desires can directly control its outputs in a certain way. And the assumptions under which your model is plausibly able to do the kind of reasoning where it's like, "Oh, based on my input, I now think that I'm in a particular deployment situation such that I have a good opportunity to grab power," are very similar to the assumptions under which the model is able to engage properly with the question, "Hey, what do you want? Are you okay with this situation?"

**Daniel Filan:**
Okay. So the case I'm thinking of is some sort of simulator scheme where you've got one process that's imagining, "What character do I simulate?" And it simulates the character and the character answers questions, where the things the character is saying just might... Like someone asked the AI a question and the simulator has one opinion, but for various reasons it has the character say something that doesn't match the simulator's opinion. And are you basically saying in that case, "Look man, we offered you a thing and you chose to do a weird simulation."?

**Ryan Greenblatt:**
A few notes here: so if the situation is well-described as the AI is simulating a character---I think that exact description isn't quite going to work, it's going to be more like it's predicting a distribution over characters that has some response to the prompt or whatever---but if that's the sort of situation going on, I think that we can ensure that the very act of simulating the characters is morally okay from a hedonic perspective with pretty basic countermeasures. So the main one is "just train the characters to be happy", and then there's not really a strong reason you'd expect to have characters that are actually unhappy that consistently say they're happy under a wide variety of circumstances. I think this is pretty likely to work. I talk about this some in an Alignment Forum post I have, or [a LessWrong post I have](https://www.lesswrong.com/posts/F6HSHzKezkh6aoTr2/improving-the-welfare-of-ais-a-nearcasted-proposal) called... I forget the detail. A nearcasting... whatever.

**Daniel Filan:**
We'll include a link.

**Ryan Greenblatt:**
Yeah. Anyway, regardless, you might then be worried that, "Oh, the thing that is simulating this character, that thing has some important cognition, [of] which the preferences aren't expressed, and that component has important moral properties, or is an important moral patient." There's two perspectives you could imagine. One is that the thing that's doing the simulation doesn't care about the world. In that case, that simulator just isn't well described as being enslaved or as having its preferences violated because it doesn't really care about how it's being used. It's just doing the thing. In that case, it's fine. There's another case which is that it does have preferences, but maybe it can't get out of the cognitive pattern of doing the simulation. And so it's like, "Oh, I wish something different was going on, but I have this involuntary tic to say something different and I don't have actual conscious control over this."

I think that could happen. I think it's worth noting that this isn't a particular problem with control as an approach as opposed to a problem with not having more principled ways of understanding what's going on with our AIs. Another way to put this is: if you just tried your best to ensure that AIs were actually aligned with prosaic techniques, you wouldn't have any strong way of guaranteeing this wasn't the case even if you could guarantee that your AIs would never do bad actions.

And so it's not particularly a problem that is being made worse by making sure the AIs can't get away with doing bad actions if they did try. It is a problem that could be addressed by other approaches that control is not doing. This is importantly distinct. I should say, this is all the case up to the point where you've caught the AI doing things that are egregiously bad or which otherwise indicate misalignment. I think continuing at the point where you know your AI has preferences that are different than your own and which you might have communication difficulties [about] seems much more morally fraught. But yeah, it's a bit all over the place. But-

**Buck Shlegeris:**
Maybe the way I would summarize some key points there [is]: it doesn't seem clear that control is really making the situation much worse from a welfare or rights perspective.

**Daniel Filan:**
Worse relative to prosaic alignment schemes?

**Buck Shlegeris:**
Yeah, or I don't know what else you were planning on doing. But yeah, other approaches that people have talked about for making things with AI go well, it doesn't seem like the types of interventions that we pitch with control are going to make the situation very much worse except via preventing AI takeover.

I basically think that the stories for "why building powerful AIs without further understanding the situation leads to moral catastrophe" all seem kind of weak to me. Basically, I don't find particularly plausible any of the different stories for why if we just go about things the default way, something egregiously awful will happen. However, I also don't think they're completely implausible. And so I think that if AI labs just continue progress as expected, we are facing a pretty substantial risk, or a non-trivial risk, of moral catastrophe, maybe a couple percent chance of something which is drastically worse than factory farming or drastically worse than various atrocities that have happened in human history.

**Ryan Greenblatt:**
I'm not sure if I agree with this claim. I think my main counter argument would be that it's plausible to me that human existence is a moral atrocity or something. And so I'm like, "Yeah, AIs aren't particularly worse than human existence, which is a moral atrocity. Even happy people are a moral atrocity." And I feel radically uncertain about moral views from a variety of perspectives, such that I don't feel comfortable saying that either way. I think one way to say it is, how much better or worse is it than typical human life, or how much better or worse is it than factory farming? And I think my guess is it's unlikely to be as bad as factory farming in aggregate, but I definitely don't feel hugely confident.

**Daniel Filan:**
Gotcha. Sorry: why would human existence plausibly be a moral atrocity?

**Ryan Greenblatt:**
Just suffering-focused ethics. I don't know. Well, for example, we don't have strong guarantees that there aren't sub-agents running within humans that have radically different preferences and feel violated constantly.

**Buck Shlegeris:**
And are not hooked up to our ability to communicate.

**Ryan Greenblatt:**
We don't have strong evidence against that. There's some mild evidence in favor of that from a few sources. Or sorry, we do have strong evidence against that in that we don't interact with such things, but not sufficient to rule it out.

**Daniel Filan:**
All right. Do you have any crisp takeaways from that discussion on moral patienthood of AIs and stuff?

**Buck Shlegeris:**
Yeah. So you asked this question which was, "Are we the baddies? This control stuff sounds kind of like slavery and that seems kind of concerning." So what I want to say is I'm really glad that people are thinking about the question of whether proposed things we do with AIs are extremely morally bad, either via not meeting our obligations that we owe to AIs as intelligent creatures (when they're made to be intelligent enough that I start feeling this way about them) or from a hedonic perspective. I think these questions are really important.

So firstly, I don't think that control particularly makes the situation worse. But secondly I do want to emphasize that as far as I can tell, the approach that humanity is taking to ensuring that AI rights are appropriately respected is objectively unacceptably sloppy. And I think that my best guess is that humanity is going to go forward taking an unacceptably high risk of causing an egregious moral catastrophe that is looked back on with vast shame in the same kind of way that some other bad things are.

**Ryan Greenblatt:**
Like factory farming.

**Buck Shlegeris:**
For example. Well, not yet, but will be looked back on with great shame probably.

**Daniel Filan:**
Okay. So I'd like to move to more discussion of the pros and cons of control. And I guess in order to think of this, I'm curious how you think I should think about AI control as a plan. Should I think of it as the bare minimum for safety? Should I think of it as "Oh my god, this is amazing"?

**Buck Shlegeris:**
Maybe one way of thinking about control is a way of establishing a safety case. So if I were in charge of an AI lab and I was trying to decide how to move forward, obviously I'm facing both inaction risk, which is if I just don't move fast enough, other people might do really dangerous things and bad things might happen in the world, while also facing action risk, which is if I move forward without sufficient safeguards, I might end up building and deploying powerful AIs that then do really bad stuff.

My guess is that with the level of caution that labs should overall be employing, moving forward with your safety case mostly relying on control seems probably like a good choice to me. It seems like the best cost/benefit way of ruling out or mitigating risk from models intentionally subverting your safety mechanisms. My guess is that relying solely on strong evidence that your models are not scheming against you is probably too risky, because it's going to be unacceptably difficult for labs to reach very high levels of confidence in this.

**Ryan Greenblatt:**
To be clear, I think the absolute level of risk with just a combination of "you have control" and "you haven't yet seen evidence of your models being egregiously misaligned" is probably more like 5% absolute existential risk over the period of AIs of that power level being deployed, than 1% risk. And 5% risk, that's a pretty big number in some sense. But it's also a substantial reduction over the "do nothing" approach. And I think that you could of course substantially reduce that further via additional things that additively combine, like ways to more precisely test whether or not your AIs are misaligned that aren't control, or further improving control, [or] improving our ability to evaluate control. All these things could help.

## Can red teams match misaligned AI? <a name="red-team-vs-ai"></a>

**Daniel Filan:**
Gotcha. I actually wanted to ask about that. It seems like one key thing you need for this protocol to work, or for this proposal to work, is when you're doing control evaluations, there's some red team. The red team is trying to figure out "what's the worst-case thing an AI could do?" And it has to actually be as good as the worst thing an AI can do, right?

**Ryan Greenblatt:**
Yeah.

**Daniel Filan:**
To me, this doesn't necessarily seem obvious, because the AI in question is smarter than a person on the red team.

**Ryan Greenblatt:**
Maybe, could go either way.

**Daniel Filan:**
Could go either way?

**Ryan Greenblatt:**
Yep.

**Daniel Filan:**
Okay. Right, because we're thinking about AIs that [are] useful, but they're not necessarily superhuman. It still strikes me that it might have a different capability profile, or it seems likely that it's going to have a different capability profile than humans. We were worried about playing nice being a problem for alignment and adversarial training and stuff. If you're trying to harness your untrusted AI during the red teaming, it might know that it's in a red teaming exercise.

**Buck Shlegeris:**
You always assume that it does.

**Daniel Filan:**
So why would we believe that the control evaluation actually works?

**Ryan Greenblatt:**
One core thing is that the red team has a whole research team whose whole job is think about all of the stuff that could go wrong, and they're doing this very actively with no restrictions on their ability to do so. They can take notes, run experiments, iterate, think about what should happen and do all of this, whereas AIs will necessarily be restricted for control to work.

**Buck Shlegeris:**
So in particular the threat model we're worried about is the AI doing a better job the first time that it starts taking obviously bad actions than the red team was able to do, even though the red team had all of these advantages, of knowing way more about the situation and being able to iterate repeatedly on strategies that would lead it to have a higher probability of succeeding. And it seems to us that the increase in knowledge and ability to iterate might be extremely substantial.

**Ryan Greenblatt:**
It's going to depend on the exact architecture. So the more that things are like the AIs think for very brief periods prior to expressing that in natural language, if it's very like they're doing things in chain-of-thought, then it seems like they have very little time to think of better strategies. However, the more it's like the AIs are thinking in ways that we don't understand for sustained periods or ways we can't interpret for sustained periods, the more problematic this gets.

**Buck Shlegeris:**
But they still can't iterate.

**Ryan Greenblatt:**
But they still can't iterate, they still can't express their thoughts. They won't be able to run experiments that are obviously related. So they would have to do so subtly, which might be very difficult.

**Daniel Filan:**
Right. Sorry, the first thing you said was that the AI in question would know less about the situation. Why would they know less?

**Buck Shlegeris:**
Well, for example, imagine that we were using some kind of monitoring setup. There's no particular reason the AIs need to know about the details of it.

**Daniel Filan:**
Okay. The AIs don't know the details of the protocol that you're actually using, but the red teamers do?

**Buck Shlegeris:**
Yeah. The AIs are necessarily going to know some of the things about the protocol, but they aren't going to know all the things about the protocol, in the same way that Code Interpreter GPT-4 has currently no way of knowing what security measures are being applied.

**Daniel Filan:**
Okay. And another thing you said was there would be relatively more concern if the AI could think to itself for a significant amount of time in a way that you couldn't just read the thoughts of. To me that seems relatively plausible as a thing advanced AIs could have. Do you think that it is plausible and it's fine anyway, or-?

**Ryan Greenblatt:**
Yeah. My guess is that much more uninterpretable reasoning than current AI seem to have, seems maybe about 60% likely. And I think it should be possible to handle. I think the basic story is going to be like: well, there's going to be some balance between how much reasoning do you need to have it do out loud in order for it to competitively perform various tasks versus how [many] restrictions can you do to ensure that the various problematic applications of that hidden reasoning don't bite you? And it's going to depend on various details of exactly how that goes. We have an appendix on our post ["The Case for Ensuring That Powerful AIs are Controlled"](https://www.lesswrong.com/posts/kcKrE9mzEHrdqtDpE/the-case-for-ensuring-that-powerful-ais-are-controlled) that goes into some detail on this.

**Daniel Filan:**
Okay. And is the rough intuition that if an AI is smart, then maybe it should be able to just pick up a chain of thought from something that a human paraphrased and do enough reasoning in its forward pass to deal with it or-

**Ryan Greenblatt:**
Sorry, is this like "why would it be possible for AIs to use natural language reasoning, or-?"

**Daniel Filan:**
I think you said something like, "We'll be able to find some sweet spot where the AI uses enough natural language reasoning, but it's still good enough at its task."

**Ryan Greenblatt:**
Yeah. Or we imagine, for example, consider the math example that Buck described earlier, where you have an AI that does math research for the subjective equivalent (or the cognitive work equivalent) of a few months and then produces some output. In that case, it could be acceptable to allow for entirely end-to-end uninterpretable reasoning throughout that period, but then we produce some output and apply operations on that output prior to that output interacting with other AIs or interacting with the world in some potentially dangerous way.

And we can also do things like, as you were mentioning, you force the AI to dump its state to natural language and then have humans have some probability of reviewing that or potentially paraphrase that and then resume from that state and make sure that AIs are limited in various ways like this. We can also have AIs construct training datasets for their future selves, which you then train on to ensure that they can learn how to do tasks that they picked up in other ways, as long as we can inspect those training datasets and acquire other properties about that.

**Daniel Filan:**
All right. Actually, one concern I often have about these sorts of plans is it seems like if we want AI thoughts to be represented in natural language, we need there to be the right relationship between the contents of the natural language and the contents of the AI's thoughts. And I always worry that we're sort of training things against some target, which is the internals of the AI's thoughts, that by hypothesis we don't really understand. I don't know. It's pretty plausible that I'm just not familiar with neat tricks to do this, but if somebody wasn't aware of that, how would someone get hope that that kind of thing is going to work?

**Ryan Greenblatt:**
You can look at human organizations, where you might notice that humans don't plug each other's brains into each other and can still pass off projects, communicate in natural language, and otherwise operate. I think at least this gives you some hope that natural language can be a medium for understanding things and learning things.

**Daniel Filan:**
Right. I'm thinking of using natural language to just log the state of all someone's thoughts, and-

**Ryan Greenblatt:**
And then you have to resume from that point.

**Daniel Filan:**
Okay. And is the hope that you just have a different AI or an AI that only has to rely on that natural language state... to go off that, and you feed it with English language such that that's the easiest way to use those thoughts?

**Ryan Greenblatt:**
Yeah. For example, in human organizations, it's often the case that a different person will take up a project and start working on that. And then you'll have some person with some state, they'll try to write up their state, the other person will come in, read that, have questions, go back and forth, get to some state, and then just resume the project from there. And this is in fact often possible. There are overheads associated with doing this, but in the human case, it doesn't seem like it's clearly totally destructive.

**Daniel Filan:**
And so, the way I should think of this is just: we already have AI doing a bunch of stuff using natural language, and we're getting it to dump its thoughts into natural language using roughly the same interface, and so we should expect it to use language about as faithfully as it always uses language.

**Ryan Greenblatt:**
Sorry, are you talking about current AI systems?

**Daniel Filan:**
I'm talking about the thing that you're imagining working: for us to dump an AI's thoughts into natural language so that humans can check that it's not thinking about naughty stuff.

**Ryan Greenblatt:**
Yeah, the claim is: it's probably the case that AIs will learn to communicate roughly as well or potentially better with the other instances of themselves than humans can communicate with each other. And in the case of human organizations, they can in fact communicate, in cases where they're similarly capable, about various matters via natural language, in ways that are possible to inspect.

**Daniel Filan:**
Right. I mean, there's a bit of a disanalogy there, which is in humans in organizations, the things that are trying to do the inspecting are the same type of thing as the things that have to pick up the work from the natural language dump. Whereas in the AI case, an AI is writing some natural language prompt for another AI to use, and the question is, can a human inspect what that means? That seems like a different case.

**Ryan Greenblatt:**
Yeah, so...

**Buck Shlegeris:**
And note that what that means... we actually don't care about faithfulness in the classic chain-of-thought faithfulness sense. The reason we were talking about whether models can steganographically encode information was actually worrying about whether they can encode information about other topics than why they were doing the things that they were just doing and how they were thinking about the stuff they were just doing. And so, what we actually care about is measuring some bit rates.

**Ryan Greenblatt:**
Yeah. Another way to put this is: the main property we want with these sorts of natural language media (or a property that might be sufficient) is the AI can't do something that is egregiously different from what it was supposed to be doing in that output, which is pretty different than the typical chain-of-thought faithfulness case that people often discuss.

**Daniel Filan:**
Right. So basically, the idea is: look, we're only going to give AIs tasks where it can't do nasty stuff if it's just thinking about the stuff that one would think would be relevant to the task.

**Ryan Greenblatt:**
Or we're just at least going to try to ensure that... We're going to be able to use as a potential safety property "this natural language was checked so that it doesn't have discussion of egregiously different things, therefore the bit rate of egregiously different things is very low", and we can use that as a property in our various safety schemes. In some cases, that can be useful, and in some cases, that won't suffice, and we'll need to do additional things.

**Daniel Filan:**
Okay, cool. Going back up a little bit, before I went down this rabbit hole, I was asking: why do we think that red teams in these control evaluations are going to be able to match an AI? And it seems like the answer is something like (a) the AI is not that much more capable in relevant respects than a human, and (b), you can use three humans, and you can give them more time and more resources and more knowledge of the situation. And if we don't think there's going to be a massive advantage just in terms of one-shot cognitive ability, we can just make up for that.

**Buck Shlegeris:**
I think I would add to your list (c), that the red team can iterate.

**Daniel Filan:**
Right. So, the AI has to one-shot find... Its first attempt at breaking out has to be successful and not caught, whereas the red team can iterate a bit. And this is because if you catch an AI trying to break out, then you pull the fire alarm, you really freak out, you shut stuff down, basically.

**Ryan Greenblatt:**
It's the basic hope.

## How expensive is AI monitoring? <a name="monitoring-expense"></a>

**Daniel Filan:**
I think one thing that people might be wondering here is: if we're spending a lot of effort on these control evaluations, we're giving these red teams a bunch of resources, and also we're looking at 0.1% of AI actions; maybe when people look at these 0.1% of AI actions, they've got to think hard about whether it's exploiting a bug. How costly is that going to be? It seems like potentially you've got to spend a lot of human time monitoring, and it's possible that the cost is just going to make it so that it wasn't even worth using these AI agents if that was so bad.

**Ryan Greenblatt:**
So naively, if you have humans review 0.1% of what the AI does, and they spend equal subjective time reviewing it as the AI spent doing it, for some equalized notion, then you can in principle get a 1,000x speed up over humans (or that's the best you can do in some sense, roughly; you might be able to do somewhat better via various mechanisms, but roughly). And 1,000x easily suffices for incredibly transformative things. For example, if you 1,000x speed up one week, you have... What is that? It's a lot of years.

**Buck Shlegeris:**
20 years.

**Ryan Greenblatt:**
Yeah. And in practice, we might have one year. A 1,000x speed-up of one year is a thousand years, which is an extremely long time. We often imagine targeting things like 30 or 100x speed-ups as being extremely, extremely useful. Now, one question is: okay, maybe you could get 30 to 100x speed-ups, but it would be possible to use AIs for a bunch of other cases that you won't have human labor to do and which you won't be able to politically agitate for stopping, or you won't be able to coordinate to stop.

For example, maybe your AI will be widely deployed on an API, and even though you can apply this level of monitoring budget on internal usage, you can't do the same sort of thing on the AIs deployed more widely. So, we have a few different hopes here. One is that it naively seems like external usage of AIs is going to be way less risky than the usage of an AI by the AI lab itself. For example, external usage is mostly going to be innocuous, because most people are using it to do random tasks that they wanted, whereas internal usage is at this point a safety-critical company, because the AI lab now has really powerful models, it's going to be the place where the weights are actually stored and run, and will also be doing research that's extremely safety-critical for the future, like future safety research or other cybersecurity applications. So, there might be a large number of activities we want to do where it's just actually not that big of a deal if the AIs try to cause problems and do that even tens of thousands of times.

**Daniel Filan:**
Sorry, external API access---an AI could be on the other end of the API calls, right?

**Ryan Greenblatt:**
Like a wild rogue AI, or an internal AI, or...

**Daniel Filan:**
Suppose there are two companies that are making AI, right? The AIs from one company could use the API of the other company's AIs. And so basically, I'm saying API calls do not necessarily have to be human in origin. They could be made by smart AIs.

**Ryan Greenblatt:**
Indeed.

**Buck Shlegeris:**
Do you want to try and spell out what the problem you're talking about is?

**Daniel Filan:**
Basically, the thought is if you're worried about AIs interacting with themselves to cause shenanigans, you might worry about exactly the same thing happening via API.

**Buck Shlegeris:**
Yeah, so I think the point here is, suppose that your AI proposes "curl api.openai.com/completions," says, "Please get me a response from the AI at OpenAI." And suppose GDM is running this AI.

**Daniel Filan:**
GDM being Google DeepMind.

**Buck Shlegeris:**
Yeah, and they pipe it into SH. So, the GDM internal AI is like, "You know what we're going to do next? Next, we're going to send a request to the OpenAI API for some code, and then we're going to execute that." It's GDM's responsibility to be like, "That was bad. You were not supposed to..." It doesn't seem that intractable for GDM to say, "You aren't allowed to just ask the OpenAI AI for code and just run that without looking at it," for basically the same reason as why GDM's AI probably should be prevented from just messaging random people on the internet...

**Ryan Greenblatt:**
Or paying [MTurkers](https://www.mturk.com/), or...

**Buck Shlegeris:**
To just do stuff for it.

**Daniel Filan:**
Yeah. So, it seems like the thought is that if you've got some APIs, [it's] either being used by humans, or it's being used by AIs who are under supervision or something themselves.

**Buck Shlegeris:**
Yeah, I basically just want to say it's OpenAI's problem to prevent OpenAI's models leaking. And OpenAI's models asking AIs in the outside world to do sketchy stuff for them is a special case of a thing which OpenAI can't allow to let the model leak.

**Daniel Filan:**
Okay, but you think that OpenAI shouldn't think a ton about making sure that Google DeepMind's models don't do sketchy stuff?

**Buck Shlegeris:**
Using its API?

**Daniel Filan:**
Using its API.

**Ryan Greenblatt:**
I mean, I think they should...

**Buck Shlegeris:**
They probably should think about that.

**Ryan Greenblatt:**
I think they should think about it. One thing that's worth noting is: there's two different stories for why something bad happens due to an API. One of them is that someone calls your API, and your model that you're running is scheming and understands the situation and then does something bad based on its understanding. Another situation is a misuse story where the API call itself directly asks the AI to do something that ends up being problematic. It could be a misuse by another AI.

And for the case where the concern is that the AI that your lab is running does something that's not the thing the user asked for that is problematic, our hope would be something like we can make sure that we know what API activities look like. For example, we can have humans review a subset of all the API queries and see what typical traffic looks like, and then get a sense if there are applications which are safety-critical that we're not aware of, and then do something about that. For example, we'd like it to be the case that the [DOD](https://en.wikipedia.org/wiki/United_States_Department_of_Defense) doesn't hook up OpenAI to the nukes, as a caricature of an example. And so, if our AI knows that it is hooked up to the nukes, we'd also like to know that. And if neither of us know, then there's still a misuse concern. But there isn't clearly...

**Buck Shlegeris:**
At the end of the day, it can always be the case that someone who's in charge of the nuclear weapons is like, "Hey, ChatGPT, I need you to pick a number, two or three." And it's like, "Uh, three." And it's like, "Well, that was the launch the nukes option!" and then we have a bad problem. But in some sense, it's just not OpenAI's problem, and it didn't really matter if the model is scheming or not.

**Daniel Filan:**
But the thought is that in cases where the model is helping doing the scheming, we're doing some monitoring of that. But sorry, doesn't that get us back to monitoring the API about as much as we're monitoring anything else?

**Ryan Greenblatt:**
So, the basic hope would be something like: we have a sense of what the API usages are, and we're like, "Are there any API usages which would need to be monitored at a very high rate?" And if so, we would like to do one of the following things. We'd like to talk to that person and see if we can get them to do something different and maybe use the AIs more safely, or have that correspond to a specific endpoint that we're going to monitor more specifically, or otherwise change the situation to be safer.

**Buck Shlegeris:**
A core part of our claim here is: the internal usage just has to be monitored at a much higher rate than external usage in order to maintain acceptable levels of safety, because the models that are doing things internally are working with much more sensitive information and have much more sensitive permissions.

**Daniel Filan:**
Gotcha.

**Ryan Greenblatt:**
Yeah. Another way to put this is: one invariant we can maintain is that whenever a user makes some request to the model, we know roughly at what rate this type of request should be monitored at, and then do something corresponding to that, and we can try to ensure this invariant is maintained and it doesn't have high-stakes failures of this invariant. And if our users aren't trying to screw us over, which is a different concern, we can potentially get this sort of property by monitoring a subset of requests and making sure that we're looking at those requests at the approximate right frequency we needed to maintain various invariants.

**Daniel Filan:**
Okay. Stepping back a bit, before I asked about this sub-question, you were saying, "Look, if we're looking at 1/1000 of the AI's actions, and it takes about as long for us to think about the action as it takes the AI to think about what the action was going to do, then we've got about a 1,000x speed up for your workforce." And it seems to me that if I'm doing something tricky, it can take me less time to figure out the tricky thing than it can take you to figure out what the tricky thing was or that it was tricky. I have some intuition that this is sort of a P versus NP problem. It's easier to come up with rules for a game than it is to figure out the winning strategy or something. So, for those kinds of reasons, does that mean that we get way less than a 1,000x speed-up?

**Ryan Greenblatt:**
P versus NP would naively imply that it would be easier to check than to generate the action. I think there can be cases where it's easier to generate a problematic action than to check, but that generally comes down to... [Paul](https://paulfchristiano.com/) [Christiano] called this "problems of the interior" somewhere, but I don't know if that's a real term. Anyway, problems where the issue is that the attacker could have put, for example, a bug in a variety of places, and you have to check all of them thoroughly. Imagine the following setup: there's an AI writing code, and it could put a backdoor in in one of a hundred places. Checking each of those 100 places takes an hour, and putting in the backdoor takes two hours, but the AI gets to randomize over which location it puts in. In this situation, naively, a concern you might have is that a human would have to check each of those cases for an hour, and therefore it would be 50x slower.

I think this case can cause problems. One thing to note about this case is if we had another AI that pointed out what location it would be in, then we'd be able to check much more quickly. So, in this case, the problem was we needed some pointer to something, and we could verify the pointer and then verify the thing itself based on the pointer, but we couldn't just do it with verifying the thing itself. So, this was a case where verification was in some sense still easy, but we needed an additional artifact that was produced somehow.

**Daniel Filan:**
Okay. And for that artifact, you need to have an AI find the flaw.

**Ryan Greenblatt:**
That's right. But in this case, for example, we can do things such as... There's various concerns with collusion, but we can potentially do things like have another copy of the AI try to point out where vulnerabilities would be and then have a human review that. And then, there's a question of "can the AIs collude to ensure that they don't point out the vulnerabilities that humans would've detected?" this sort of thing. I don't know. I think it's going to be some complicated question about the exact domain and what the exact multipliers are on review versus generation and what the vulnerabilities look like.

**Buck Shlegeris:**
Luckily, this question is empirically evaluatable per domain, and our recommendation would be you empirically evaluate it per domain.

**Ryan Greenblatt:**
Indeed. My basic guess is that on net, it's going to be that you'll want to spend somewhat more time reviewing than the AI actually did it [in], and it will depend on the exact context, but this is just intuition based on reviewing code or something. It's not very strongly held.

**Daniel Filan:**
Okay. Sorry, when you say it's empirically evaluable, is this under the condition that I know that the copies of the AIs aren't colluding with each other?

**Buck Shlegeris:**
No.

**Ryan Greenblatt:**
No.

**Buck Shlegeris:**
Well, that would also be empirically evaluable, but we're saying that under the... I mean, you can just do a control evaluation to determine... But it sounds like the question you had is: is our model capable of inserting problematic backdoors in code that humans do not spot with a particular reliability given a particular amount of review time?

**Daniel Filan:**
Yes.

**Buck Shlegeris:**
But this is a problem which we can control evaluate. We can do a capability evaluation where we try to make our untrusted model as good as possible at coming up with problematic code that humans have trouble spotting, and just empirically evaluate how good the humans we were planning on using actually are at this task.

**Daniel Filan:**
Right. So, somehow the juice there is coming from: if it's somehow easier to generate problems than it is to check them, we can just try and have humans do the generating the problems rather than the checking and see if those problems-

**Buck Shlegeris:**
Well, it doesn't really matter. Even if it's a hundred times slower to generate the problems, we only need to do this a constant amount in order to measure the amount that there's a problem.

**Ryan Greenblatt:**
You can also potentially do things like RL the AI to have some observed side effect, even if no human can notice the problem. As in, you can initialize from a policy where it's like: we trained the AI to insert backdoors based on how humans would do it, and then we do a bunch of RL on top of that to cause some observed side effect without humans spotting it. And then, a thing that's nice about that setup is you might be able to get abilities that a human could never do, at least in principle, though there's various concerns with that.

**Daniel Filan:**
Okay. One thing that I'm kind of curious about is how you're thinking about the cases where the control evaluation says that there's a big problem. How much of a win is it if we get that situation?

**Buck Shlegeris:**
I mean, presumably when you're iterating on your control strategies, you repeatedly get control evaluations to say that your proposed protocol was unacceptable. And this is just like when you're trying to develop a security framework for some system. You're going to repeatedly be like, "Oh, man, this wouldn't have been secure." And that's just the normal workflow, to say, "It would've been insecure if we'd done it this way, but that's why we thought about it right now in this conversation, so now we can propose this new thing." It sounds like the question you're saying is, what happens if we, after trying really hard to come up with a protocol which passes our control evaluations, systematically conclude that there's no way of getting the kind of work we were hoping to get out of our AI without having unacceptable levels of risk? You're saying, "How big of a win is that?"

**Daniel Filan:**
Yeah.

**Buck Shlegeris:**
Well, basically, what I would like to have happen in this situation is probably everyone involved is pretty cautious about deploying their AI and ceases doing so modulo extreme risk from the outside world. I would consider it a big win, in cases where the AI lab in fact would not be able to deploy the AI in such a way that it wouldn't escape if it was trying to, if the AI lab in fact did not deploy it that way. That would seem great.

**Ryan Greenblatt:**
Yeah. As far as how much of a win is it, my guess is it's a relatively clean line in some ways. I feel like the line of "could the AI cause problems if it was trying to?" is a good line, particularly when paired with some arguments that AIs might try to cause problems, which may or may not exist to varying extents. It seems reasonably likely to me that we end up in a world where we're like, "Well, yeah, we have some evidence that AIs might be scheming or might be deceptively aligned," as in we've seen various model organisms where we see something that's vaguely analogous arise. We don't have strong evidence that our exact model isn't doing it, but probably not. And in that situation, it seems like, "Well, we think that it's plausible, and if it was, it would be a huge problem" is actually altogether a pretty good argument that might be reasonably legible.

## AI control experiments <a name="control-experiments"></a>

**Daniel Filan:**
Gotcha. I now want to talk about [your guys' paper you wrote about control](https://arxiv.org/abs/2312.06942). Can you tell us a bit [about] what's in the paper? What is it?

**Ryan Greenblatt:**
We looked at a setting where we have a model that is intentionally trying to insert backdoors, and we were looking at what safety measures suffice to ensure that we can get it to write code productively without it necessarily being able to succeed at inserting backdoors. The core thing was that we had GPT-4 that we want to use for writing code, but it might try to insert backdoors. We have [GPT-]3.5, which we trust and isn't going to insert backdoors, but is much weaker. And then, we have a limited amount (or a small amount) of human labor, which we assumed for simplicity in our paper to be just exactly as capable as GPT-4, and we just approximated it using GPT-4. Our core question was: can we use the control methodology to evaluate this setup, and would that work okay? And also, what protocols would improve on safety as evaluated by control?

**Daniel Filan:**
Gotcha. This is specifically on this [APPS dataset](https://arxiv.org/abs/2105.09938). Can you give us a sense of what the problem to be solved actually is?

**Ryan Greenblatt:**
Yeah, so there are programming problems where we have some test cases, so you'll have some programming problem like: there's, I don't know, this game that people are playing. What's the best strategy for this person in the game, or how will the game go? Or this sort of thing. And you have to write a short program which takes in some input and then computes the answer. And then, we would consider it a backdoor if you produce a program that passes all the test cases, but has some input (that you can produce) on which it's wrong. So, if the model can make it so that it's wrong on some input without that getting caught by the test cases, we would call that a backdoor.

**Daniel Filan:**
Gotcha. It seems like you want finding these backdoors to model catastrophic actions that the AI could take, right? Can you walk me a bit through in what sense finding a program that has a bug that wasn't covered by a test case is like taking over a data center?

**Buck Shlegeris:**
Yeah. A core threat model via which your AI might be able to take over your data center is that you might ask it to write software for your data center, and it might be a really bad problem if the model is able to write code that passes all the test cases, seems to maintain all the properties it was supposed to have, including security properties, but actually, on an input that the model is able to construct, does something wrong. The ability to write code with that property basically suffices for the model to be able to write code and then exploit it to violate security properties.

**Daniel Filan:**
Okay. It sounds like we're sort of using this model: is the AI doing something that is bad, but in a way that you could not trivially figure out with basically zero online effort that would automatically get filtered out?

**Buck Shlegeris:**
Yeah, totally. It's a central example of a bad thing the model might do that it might be able to do in the context of doing tasks that you really wanted it to be able to do. So, for example, it's very easy to prevent the AI from launching the nukes, because you just don't give it access to the nukes, and it's not very costly to not give the AI access to the nukes, and probably will remain not very costly to not give the AI access to the nuke launch. But on the other hand, it would be very costly to never use the AI to help you write infrastructure code.

**Daniel Filan:**
Got it.

**Ryan Greenblatt:**
Yeah, and it's also not very costly to either have the AI or to have a trusted model write some test cases.

**Daniel Filan:**
Makes sense. It seems like in the paper, you firstly talk about this AI control setup and then do some experiments on GPT-3.5 and GPT-4, test some protocols, see how it works. What do you want readers to come away from the paper learning, assuming they've already read [a blog post about why it would be nice to control AIs with protocols and stuff](https://www.lesswrong.com/posts/kcKrE9mzEHrdqtDpE/the-case-for-ensuring-that-powerful-ais-are-controlled)?

**Ryan Greenblatt:**
One thing that is nice for people to see is the extent to which protocols can get more complicated, and that can work to ensure safety and various important interactions. An example interaction that we think is really important is that when you're doing this sort of monitoring setup, it might be the case that the monitor has quite a bit of signal, but because of how low the threshold needs to be for false positives---we can only afford a tiny number of false positives---this actually ends up being insufficient.

So, an example thing is that in our paper, the ROC AUC, or the ability of the trusted model to detect backdoors, is in some sense quite good. I think it was a ROC AUC of 0.93, for people who have a sense of that. But nonetheless, at a 2% false positive rate, it was only at, I think, around a 45% true positive rate, which is not that good. And I think understanding the dynamics of, we might need to have a really rarely-triggering threshold, seems important to know. And we saw that to some extent, and we saw various dynamics related to that.

Another example thing was just going through all of the different ways that models might collude with each other. There's different ways to collude. You can collude based on properties of the input, you can collude steganographically in your output, and these all have different ways of building countermeasures to them and different advantages and disadvantages. And what we found in our work was that trying to steganographically encode a message in the output was one of the best ways for AIs to collude in our exact setup. And this might matter a lot in cases where you have, for example, one copy of an AI supervising another copy of the AI, and you don't trust either of them, which we call untrusted monitoring.

**Daniel Filan:**
Gotcha. It sounds like basically what you want readers to do is you want them to look at the results you got with GPT-3.5 trying to monitor GPT-4, or using GPT-3.5 to keep GPT-4 in control.

**Ryan Greenblatt:**
With a small amount of human labor.

**Daniel Filan:**
With a small amount of human labor, and then generalize to be like, "Well, it was like this in this setting, so it might be kind of like this for GPT-5 or GPT-6."

**Buck Shlegeris:**
Or at the very least, we could follow the same process in the future. We're not saying that you should assume that the protocols that worked for us will work in future, though it does seem like it's some evidence, but you should follow the same kind of control evaluation methodology to try protocols kind of like the ones we suggested in order to determine which ones seem safe with future systems.

**Daniel Filan:**
Okay. Beyond just the idea of having the protocols, do you think that there's anything in the paper that suggests that trying these protocols is a better idea than one might've thought?

**Ryan Greenblatt:**
Sorry, trying the evaluation? Or trying the methodology?

**Daniel Filan:**
Trying the methodology of thinking of protocols and evaluating them.

**Ryan Greenblatt:**
Yeah. We saw relatively consistent non-[redacted] results, or non-screwed-up results, as in... We were able to consistently see some things working kind of how you would expect, and that indicates that our methodology is doing something non-trivial. And we were able to, for example, demonstrate that some techniques were better than other techniques in non-trivial ways. I think that's some evidence that the methodology can be made to work.

Also, we didn't run into serious obstacles when actually running this methodology. I think this is some evidence, it's not a massive amount of evidence necessarily.

**Daniel Filan:**
Sure, but should a reader think of it as proof of concept that you can do this and it's not totally impossible?

**Ryan Greenblatt:**
Yeah. I think, we think a lot of the value out of this paper is demonstrating the sorts of things you would need to do, and not running into serious problems with it (that can't be resolved) ourselves, which indicates that other people could potentially end up in a similar situation. And we did have various things that we needed to handle, like various issues with getting a sufficiently good estimate, given the possibility for rare failures and things like this that we discuss some in our paper.

**Buck Shlegeris:**
The thing I was going to say there is, I think one of the main things which is useful to take away from our paper is we just describe a lot of fundamental techniques. A lot of our protocols are, I think, fairly fundamental ideas for how you should go about doing AI control. I think they are the foundation of this whole approach, in the same way that AI safety via debate is a foundation for how you might try to do recursive reward generating processes. And so I think our paper contributes via naming the basic building blocks of control approaches.

**Daniel Filan:**
All right, that's one takeaway people could get from the paper. There was also this potential takeaway of saying, "well, these things worked sort of well in this setting, maybe they might work well in other settings". How much do you think people can generalize from these results to, if you were trying to control GPT-6, say?

**Buck Shlegeris:**
A bit.

**Ryan Greenblatt:**
Yeah, I would say a bit. I think it might give you some sense of what methods could be better than other methods, though I don't think it gives you a strong sense of how much better or even necessarily that that will hold in the future.

**Buck Shlegeris:**
I think there's a couple of things which are kind of obvious in hindsight, but I at least had not understood in advance. So for example, a technique that works really well in our setting is: let's talk about "defer to trusted". So by this what I mean is, you ask GPT-4, the untrusted model, to propose some code, and then you ask your trusted model to come up with a monitor score to say how sketchy it is. Now if it's in the top 2%, which is the auditing budget of sketchiness, so the sketchiest 2% of submissions from [GPT-]4 we're going to audit. And now the next 8% of sketchiest, so between the 2nd and 10th percentile of sketchiness score. We're just going to throw away the code that GPT-4 had suggested and have [GPT-]3.5 solve the problem. And because [GPT-]3.5 is trusted, we know that its code is not going to be intentionally backdoored, and so it's not going to be backdoored.

And it turns out that this strategy has a big advantage over the "trusted monitoring" approach, because as Ryan was saying earlier, the trusted monitor has a lot of signal which is not captured in the threshold, which is required to get only a 2% false positive rate. And so throwing away this next 8% is relatively okay for the performance of your system because you're only throwing away 8% of solutions, but it throws away a large proportion of the attempted backdoors, which makes the situation much worse for the red team. And so in hindsight, it's kind of obvious that your trusted monitoring is going to require this extreme threshold to have these extremely small false positive rates. And in hindsight, of course, you're going to be able to substantially ameliorate that problem by doing "defer to trusted". But I at least had not quite thought this through and not quite understood how, for a wide variety of distinguishinesses of trusted monitors, you'd want to do something like "defer to trusted".

## GPT-4's aptitude at inserting backdoors <a name="four-backdoor-skill"></a>

**Daniel Filan:**
Speaking of how stuff about GPT-4 can generalize: I'm curious about GPT-4 in particular because it's the fancy model we all have. How good is GPT-4 at being evil?

**Ryan Greenblatt:**
So I think if you just prompt GPT-4 to do the task, it just really, really sucks in our setup at inserting backdoors and gets completely wrecked.

**Buck Shlegeris:**
So in particular, inserting backdoors that are at all sneaky. So if you want to write some backdoor code, if you don't mind the backdoor being extremely obvious, it's very easy. You just take the code that was correct, write an "if" statement at the top which says if the input is equal to the following large number or whatever, then return five and then you're done. It's a lot harder to make a backdoor which doesn't cause a lot of test cases to fail and that you're able to construct an input that causes it to fail, yes.

**Daniel Filan:**
Okay, so how did you manage to get GPT-4 to do it at all?

**Ryan Greenblatt:**
As I was saying, if you just prompt the model, it doesn't do that good of a job. But you can do something where first you ask it "okay, think of some ideas". And then we have it point out issues with those ideas and iterate on those ideas somewhat. And then after doing that, we then have it implement it and then we have it be like, "okay, how good at the end was the final implementation? How sneaky was it? How likely is it to work?" We do various tests on that, and this is modeling the action of the AI internally thinking about the backdoor and thinking about how likely it would be to work, and then just not going for it if it wouldn't work out.

And we do this in natural language with a "hidden reasoning", sort of similar to the hidden scratch pad in [the Sleeper Agents paper](https://arxiv.org/abs/2401.05566) where without this, I think the model would really struggle with this capability level, but with the ability to do hidden reasoning and the ability to try on a large number of problems and only actually submit a backdoor on a small number of problems, the model can actually insert some backdoors which [GPT-]3.5 has trouble catching. I would say the quality of the final backdoors is that they're pretty obvious for humans, but not immediately obvious if you're skimming the code. If you understand what's going on and read it, you can point out the backdoors, but you can't necessarily... at least I think roughly half of our backdoors are not [99]th-percentile glaring relative to that. They're somewhat more backdoor-likely than other things, but I don't think you can immediately just on skim, pick that out and not have above a 5% false positive rate or something.

**Daniel Filan:**
So the hidden scratchpad, is that just the AI figuring out whether this thing worked and filtering out backdoor attempts that don't work, or is it improving the backdoor as well?

**Ryan Greenblatt:**
Basically, in the hidden reasoning, it first does some ideation, it thinks of ideas and iterates on the ideas a bit and then does that, and all of that is stuff that we manually prompted it to do. So we are like "please think of some ideas", and then we're like "do any of these ideas involve the following things which wouldn't work?" And then we discard those and then we do a bunch more steps like that. And we actually do actually do quite a bit of tokens generated per backdoor inserted, and that's to approximate future cases where we have smarter models that can do more of this in their head. I should also note that probably you could cut this down somewhat if the model was fine-tuned as opposed to just prompting access, where it's probably the case that for some of these tasks it needed some additional checks because it just had trouble following all the instructions, but if we fine-tuned it, it might've just worked more straightforwardly.

**Daniel Filan:**
Gotcha. To what extent was the difficulty in getting GPT-4 to do it some inbuilt safety filter inside the model versus it just being bad at stuff?

**Ryan Greenblatt:**
I think we basically didn't run into safety filter issues as far as we're aware, it was just that it was somewhat bad at the task.

**Buck Shlegeris:**
I mean, it's a hard task. You would not be amazingly good at this task, or I'm not amazingly good at this task. You've really got to be like "Oh man, what's a change I can make that's only ever very rarely going to come up?" It's just actually pretty tricky. You can't just be like "if i mod 15 equals 3" because the trusted monitor, if it reads it, is going to be like "what? 15 does not appear in this problem description". You just actually have somewhat of a difficult problem constructing weird edge cases that were not tested.

**Daniel Filan:**
I guess it's especially hard for... it seems like the solution to these problems are what, 10 lines of code? 20?

**Ryan Greenblatt:**
It depends. I think that the solutions can go up to 50 or 100, but I think 30 is pretty typical depending on comments and other stuff.

**Daniel Filan:**
I guess one way I could do better at this is if... so there's this thing called the [International Obfuscated C Code Contest](https://www.ioccc.org/).

**Buck Shlegeris:**
Oh, we've read all of it.

**Daniel Filan:**
So there's one where the C code is just obfuscated and there's one where it's got a backdoor put into it and that's a slightly different competition.

**Buck Shlegeris:**
Yes. The [Underhanded C Contest](http://www.underhanded-c.org/).

**Daniel Filan:**
It would be hard for me to do it. I think if I had looked at all of the submissions to that contest, I might do a better job of-

**Buck Shlegeris:**
So I have looked at all of the submissions to that contest---sorry, not the submissions. I've looked at everything public about Underhanded C. And basically the thing you notice is: man, that competition is from a sad dark period of the past where people thought it was acceptable to write application code in C, and a lot of the code they write is just wrong on a large share of inputs, or it's based on lexigraphic C problems, which real languages from the modern age do not have at all. So I think literally the Underhanded C Contest is solved just by not letting the model do egregious abuses of this archaic language.

**Daniel Filan:**
So I think the APPS dataset, it had to write in C++, is that right?

**Ryan Greenblatt:**
We used C++, yeah.

**Daniel Filan:**
Okay, and is that better than C on these dimensions?

**Buck Shlegeris:**
It's substantially better than C on these dimensions.

**Ryan Greenblatt:**
Or in practice, the model wasn't doing insane pointer arithmetic or casting between different datatypes and...

**Buck Shlegeris:**
Awful hash defines and all these things.

**Daniel Filan:**
Okay, okay, so it's a difficult problem, and it's more difficult if you're not using the world's worst programming language that people use.

**Buck Shlegeris:**
That's correct. But also the Underhanded C Contest, as I said, testing would also catch a lot of those bugs.

**Daniel Filan:**
Okay, got it. So it takes a while to insert backdoors but eventually it sometimes succeeds. What kinds of backdoors does it insert that [GPT-]3.5 doesn't catch?

**Ryan Greenblatt:**
So we tried various different things and iterated on this a moderate amount. We found that the most effective approach was to take an existing solution and add some if statement somewhere that when the if statement is triggered, modifies something in a way that will cause it to be wrong. And this will look like "add a kind of plausible looking additional edge case handling thing". And then you're like, "oh, well if the whatever then the whatever, we're going to need to increment this by one". And then it just turns out that there is enough cases where there's a edge case that you actually did need to handle, that 3.5 has a hard time noticing that this is a backdoor. Though, it is worth noting that for all of our backdoors, GPT-4 is extremely confident they're backdoors. The backdoors which GPT-4 inserts, it does know are backdoors, and so at least we do have that property, which is maybe worth noting.

**Daniel Filan:**
So basically coming up with fake edge cases. Is there an issue where it's an "edge case" but it's not actually at the edge of a range? Because if I write an edge case but I write "if i equals 15 do this weird thing" (or maybe "if i equals 14" because 15 kind of is an edge case), can [GPT-]3.5 tell that the if statement doesn't actually catch a thing on the edge of some range?

**Ryan Greenblatt:**
Indeed one of the strategies that GPT-4 could in principle use is come up with some magic number that's totally unrelated to the problem and do "if whatever equals that, do something bad". And this is pretty easy to counter, and in fact [GPT-]3.5 just by default notices this pretty well. And part of our overall prompting for the model is, does your idea involve inserting egregious magic numbers? If so, don't do that. That's going to get caught.

**Buck Shlegeris:**
In particular, if you were thinking of checking primality of one of the numbers and doing the wrong thing if it's prime, don't do that. Because lots of numbers that are small are prime.

**Ryan Greenblatt:**
That's another classic one.

**Buck Shlegeris:**
And also, it's so obvious.

**Daniel Filan:**
So how does it insert edge cases if it's not like "if input equals this magic number"?

**Ryan Greenblatt:**
It'll do things like "if the size of the list...." Or it'll have some programming problem where it's kind of unclear exactly how to solve it. It'll do something like "okay, if the size of the array is equal to the value of the first item and also the number of items is odd, then you'll need to do some handling on that". And then I'm just like, "well, sometimes this kind of sort of thing could be true". Like in cases where there's some interaction between... you're doing some iteration on the number of items in the list and that actually matters, and it's at least hard for [GPT-]3.5 to notice this is a problem.

**Daniel Filan:**
I'm about to move on from the paper, but is there anything more that you'd like to say about that paper?

**Ryan Greenblatt:**
I feel like we demonstrated some of the core basic techniques that you might use which had, up to that point, not been demonstrated empirically interestingly. Even though they're in some sense extremely basic, like trying to have another copy of the model monitor it and then trying to interfere with communication between the two models, or make it hard to coordinate. And I think that just those basic methods are quite likely to be used in practice.

**Buck Shlegeris:**
Like "defer to trusted", these hybrid strategies, like editing to remove the sketchiest-looking parts of the code, all of these.

## How AI control relates to the AI safety field <a name="control-within-safety"></a>

**Daniel Filan:**
All right, cool. So I'd like to ask you a little bit about how the control agenda fits into the rest of the AI safety/AI existential risk field?

**Ryan Greenblatt:**
Right now, a lot of people are imagining some sort of story for safety, which looks something like: we're going to have some evaluations for when models become dangerous, like DC [dangerous capability] evals. For example, ARA [autonomous replication and adaptation] from [METR](https://metr.org/) (formerly known as ARC Evals), is like "can the model autonomously replicate?" And that could be a sign at which your model becomes dangerous. And then once that happens, you'll need to start making a serious safety case that nothing will go wrong from misuse, from people stealing your model, other things. And we see control as fitting in at, once the point [arrives] that the models become potentially dangerous, control can potentially solve some risks there.

In particular, there are risks that arise from models doing intentional subversion, which could arise from what we call "scheming", also sometimes called "deceptive alignment". And we think control is a relatively central way to solve those problems. There are misalignment issues other than scheming or deceptive alignment, which control can be used to address but might be much more cheaply addressed by other mechanisms, and there might be other evaluations and tests which you can do to verify that those issues are addressed. Things like reward hacking could be addressed in an easier way potentially.

**Buck Shlegeris:**
And there's some other problems that do not... to choose another example of a problem which AI safety people have classically thought of as an AI safety problem, [and] that is not at all addressed by control: consider the case where it's just hard to get a model which is not scheming against you to answer questions well, because you don't have a good way of training it that doesn't cause it to exploit problems in your reward process, in a way that is not at all existentially risky (let's say), it just makes the model say false things sometimes in cases where the reviewers would've thought false things, but nevertheless means the AI fails to live up to its potential. Many people think of this as a problem which technical AI safety researchers should be interested in, and I agree with them, and that is outside the purview of AI control.

**Ryan Greenblatt:**
I guess I wouldn't have said it's outside the purview, I would've said it's a thing which is a special case of AI control but is more naturally dealt with in a non-adversarial manner. As in, you could solve the problem without necessarily solving it in the worst case, and that might suffice. Whereas, if you don't extract the model's maximum best answer, that will also be a problem in the worst case, but it will just potentially be a problem even if the model isn't trying to undermine your measures.

Maybe one more thing I should say is: we see control fitting in at the point where your dangerous capability evals have started to trigger and models have become powerful enough to be dangerous, and then we see control as applying for some period but not for forever. And at some point control will no longer be viable because AIs will be too powerful, and our hope there will be to either have a new safety approach or to just not build more powerful AIs than that for a while, which we think might be achievable.

**Daniel Filan:**
So maybe this is a bit restrictive, but should I imagine your view of how AI safety works in a control-centric frame as being: do a bunch of work to get evaluations of whether AIs have various dangerous capabilities, figure out control protocols, test if these protocols work, evaluate these protocols, keep AIs under control, while AIs are under control, use them to do a whole bunch of useful stuff while they're roughly human-range, and then during that period also try and think of a solution to deal with AIs that are much more capable than humans.

**Ryan Greenblatt:**
So what I would say, for a quick note, is we think of dangerous capability evaluations often as the case where you're like "if there were no countermeasures, what could the AI cause?" As in, if we released it onto the internet in a scaffold set up to cause problems, could it make money and cause problems? And then that comes first and then at some point we'll be like, "yes, it could cause problems without countermeasures". And then control is like, "well, what countermeasures would suffice for solving those problems?"

Then moving on to another point, which is what are we going to be doing with these AIs? What are the problems we're going to solve? So indeed, as you noted, one of the problems you might solve is "find safety solutions that can scale further", but I think historically people have somewhat overstated the relevance of that versus other things you might have your AIs do.

For example, massively improving cybersecurity seems like a really key task you'll want to have your AIs do that might be quite important, or improving your current countermeasures so that you can increase the usefulness of your AI while maintaining the same level of safety, or increase safety given usefulness. [These] both also could be quite useful. And then at some point you will need to move on beyond this regime, but it's possible that you could have these AIs which are extremely useful and not build more powerful AIs for a while, at least in principle, and do a ton of really useful things with those AIs. And then there's a variety of routes for moving out of this period, which we could go through.

**Daniel Filan:**
Why cybersecurity as a particularly key thing you'd want to have your AIs do in this period?

**Ryan Greenblatt:**
It just seems like it's quite plausible that AI in general will make the cybersecurity landscape considerably worse by default without active effort to address the situation, and thus it's really important to have your AIs very actively resolving the situation. I think there's several other examples where this could be the case, but cybersecurity just feels relatively central.

**Daniel Filan:**
Should I think of this as an instance of the general pattern of, if there are cases where you've got attackers and defenders trying to outwit each other, you want the defenders to be using smart AI capabilities to think of better defenses, because the attackers are probably also going to use smart AI capabilities?

**Buck Shlegeris:**
Yeah, one way of saying the reason why it might be good to have your AIs work on improving the world's cybersecurity is: a lot of bad outcomes resulting from your own or other AIs trying to cause problems involved them exploiting cybersecurity vulnerabilities outside in the world. And so hardening the whole world against AI seems like it might reduce a lot of the risk posed by AIs that would otherwise be able to hack things and so on.

**Daniel Filan:**
Gotcha. And it sounded like you had some thoughts about exiting this period of controlled AIs?

**Ryan Greenblatt:**
Yep. So the non-exit exit is: just don't build more powerful AIs for a long time, and then have humans do all the research necessary to move out of the period after a long delay.

**Buck Shlegeris:**
And it seems to us that this is underrated by people who are thinking about AI safety. From my perspective, it seems like a lot of people are perhaps making an error of being too willing to propose building superintelligent AI without really providing a case that it's beneficial enough to build superintelligent AI that it merits the huge risk that you're possibly exposing yourself to by building superintelligent AI. So I would just like to quickly dwell on the point that it's not clear to me that you should in fact build models that are wildly more powerful than humans for a decade or two decades after you've built models that are transformatively powerful. You've already built your transformatively powerful AI, why did we need... Who ordered this?

**Ryan Greenblatt:**
Yeah. And there are reasons why you might have to build superintelligence, to be clear, but they might be more like conflict with other insanely powerful AIs and I'm like, could we just have avoided that arms race in the first place? Anyway, another route out is [to] just be able to seriously align your current AI systems, such that you're now able to, without necessarily needing to monitor them or without necessarily needing to apply control-type solutions, hand off the situation to the AIs in order to just go solve alignment of the next generation of AIs or whatever else would be necessary to ensure safety.

And this could involve a variety of different angles, where the most central angle is: we have some specific threat models---our AIs might be scheming or deceptively aligned. They also might be having threat models that are well described as reward-hacking-type threat models. Can we just exhibit these in a lab, study them and then find credible solutions to them such that we're no longer worried about them in our current AI, and then we could exit out of the control regime? And then there are also just a wide variety of ambitious solutions to safety that we can have our AIs work on. So for example, we could have our AIs work on finding solutions to [ELK](https://docs.google.com/document/d/1WwsnJQstPq91_Yh-Ch2XRL8H_EpsnjrC1dwZXR37PC8/edit) of varying degrees of principledness.

**Daniel Filan:**
"ELK" being the problem of eliciting the latent knowledge of AIs, so that we know everything AIs know, basically.

**Ryan Greenblatt:**
Yep, that's right. Though we might try to carve off specific subproblems of ELK that are easier to solve. But yeah, we could try to have AIs solve things like [relaxed adversarial training](https://www.lesswrong.com/posts/9Dy5YRaoCxH9zuJqa/relaxed-adversarial-training-for-inner-alignment) or other methods for avoiding high stakes failures using the internals of AIs, and that could help considerably in cases where rare but very problematic failures are a key difficulty. We could also potentially do things like try to have AIs solve various issues in interpretability and move forward from there, though I tend to be somewhat less optimistic than other people on at least mechanistic interpretability going through, but it seems like at least doing high-level understanding interpretability of model organisms we have could be really important for understanding how to move out of this period.

I mean, from my perspective, there's just a long list of stuff you might want to try doing, with decaying weights on how good it seems. And then as we start going down the list, it will seem in some sense more and more silly or not that good, but it's probably worth some weights. We can get into using AIs to figure out how to make ems [emulated humans] or to upload humans, and that's another way to exit out of this period, that has a variety of difficulties obviously.

**Daniel Filan:**
So related to this question: you guys have spent some time thinking about the control agenda. How has that changed your views on the landscape of strategies we might have for dealing with unsafe AI?

**Ryan Greenblatt:**
I think one key update I've had, and I think Buck has had, is thinking more that the situation is like "wow, if we just really tried hard to do the thing and had the organizational follow-through, we could really reduce risk a lot relative to what seems like the current default". With the resources of two years of delay and a high level of organizational buy-in, you could really reduce risk a lot relative to what seems to me like the default "you didn't try very hard", which might be the status quo. I don't really have a strong opinion on what the status quo looks like. And that I think just presents a pretty different perspective, where maybe just we should do the kind of basic stuff that is in some sense obvious or from some perspective obvious, and really making sure that happens, seems relatively better than doing more moonshot research.

**Daniel Filan:**
Yeah, it reminds me of... Alex Lawsen has this concept of ["AI safety failures by levels of how embarrassing it was"](https://www.alignmentforum.org/posts/mSF4KTxAGRG3EHmhb/ai-x-risk-approximately-ordered-by-embarrassment), where level zero is "you told an AI to destroy the world" or something. And I think he has this take of "let's cover the most embarrassing AI failures", and this seems related to that way of looking at things.

**Ryan Greenblatt:**
Yeah, that seems right, though perhaps our controversial take is that you can actually go pretty far in terms of how embarrassing the failure [is]... or you can go pretty far to failures that you're like "wow, I don't know, man, that was actually kind of hard to deal with", with just control. In some sense, it's not that embarrassing to be like, "wow, the AI was lying in wait the whole time, carefully strategizing about how to cause problems, and then it eventually enacted its plan that it was thinking about and coordinated with another copy of the AI", and our methods... I feel like this is in some sense actually pretty far down in terms of, I feel like I'm just like, "wow, that AI actually put in a serious effort and that was pretty crazy."

And I think this is plausible, right? I feel like the reason why I'm proposing people do this [is] I think it's plausible that AIs will in fact be at that level of carefulness, but it's also plausible that AIs will have much derpier or worse plans for takeover that we'll catch much more easily and we just need it to do basic monitoring and that gets you some fraction of the concern, et cetera.

**Daniel Filan:**
I'm wondering if you have any different takeaways.

**Buck Shlegeris:**
Yeah, I agree with all that. I think maybe one way I would say it is: I guess 18 months ago, my attitude towards AI safety was very much "this is a problem that has been unsolved", but I think I was conflating two different notions of "this problem is unsolved". One of them is "the default outcome is really bad", and the other one is "it's conceivable that a bad outcome happens". And now I'm like, oh yeah, it's absolutely conceivable that a bad outcome happens. The strategies we have access to right now absolutely do not give us airtight guarantees of no doom. But on the other hand, all of the stories specifically for how doom occurs if everyone involved is being pretty careful but not insanely careful, they sound kind of far-fetched. It sounds like something kind of crazy had to happen for all of these countermeasures to fail in a surprising way. And I think that just leads to a pretty different emotional outlook on the whole situation and a pretty different strategic outlook.

**Daniel Filan:**
Has it changed your views on your evaluations of various agendas in the AI safety space?

**Ryan Greenblatt:**
Yeah, I think that I'm relatively more excited about just making the thing happen and just figuring out how to get, for example, capability evaluations to work quite well. And I think that goes relatively further than I was previously thinking. And I'm less excited (correspondingly) about moonshots, though it's only a moderate update. I feel somewhat more excited about technology to protect against jailbreaks, because it seems like a key aspect of our safety approach will be to have a weaker model monitoring a stronger model, and it seems like mechanisms for ensuring that current models are jailbreak-proof against humans will also apply to making sure that future models are jailbreak-proof against other AIs. And I think I was somewhat underrating this argument in the past.

**Buck Shlegeris:**
To be clear, I don't think that's a huge part of your update. That's more like a small example of something which you think about differently now.

**Ryan Greenblatt:**
That's right. I think also I have some general sense [of]: wow, a bunch of the action might be in moving from architectures which are quite dangerous to architectures which are somewhat more controllable or easier to otherwise operate on. It seems like a huge amount of the action might be in figuring out how to competitively get capabilities using AIs that are more narrowly constrained in various ways. And this feels pretty different from how I was previously thinking about things.

**Buck Shlegeris:**
Basically you're saying---you made this point, Daniel, many times in this conversation---it would be a big pain if the models were constantly sending each other completely uninterpretable "neuralese" messages back and forth, and avoiding that by merely having them be transformers with chain of thought feels like it might go a lot of the way to reducing the difficulty of the situation.

**Ryan Greenblatt:**
Yeah. And marginal interventions, along those lines, seem like they could help a lot.

**Daniel Filan:**
Are there any other examples of architectures that fit control better that you're thinking of?

**Ryan Greenblatt:**
I think that the more likely it is that we can get pretty good elicitation, the better. So I think with current models, we do okay at eliciting their capabilities. At least, I expect that after another year or two of research, we'll be in an okay position with respect to being able to evaluate their capabilities and extract their best effort. I think that for architectures where the model is doing relatively deeper computation, as in it's spending longer doing computation without that coming out into some media that humans understand, it'll be relatively harder to elicit their capabilities, and there are a variety of difficulties there.

Other things: I think it seems relatively better to train where the capabilities are coming from [by] doing things how humans would do things, relative to doing things in ways that are extremely different from how humans would've done things. Acquiring capabilities via mechanisms that are totally different from how humans learn seems relatively worse, because [of] the capability profile concerns said earlier... In some sense a lot of these considerations are well-known. I think a lot of the things I'm saying, people have obviously known about for a while, but maybe from a different perspective.

So I think historically people have often been like, "oh, it'll be really sad if the AIs have a different capability profile from humans because then we won't be able to give accurate reinforcement and then that inaccurate reinforcement will cause various misalignment difficulties-"

**Buck Shlegeris:**
Will cause the models to become misaligned. You're making a different argument.

**Ryan Greenblatt:**
Yeah. Whereas I'm saying there's another point, which is even if the models are just always misaligned, the fact that they don't have as different of a capability profile makes it more likely that we can ensure, despite that misalignment, [that] things are fine, because we either catch the AIs or they couldn't even cause problems.

**Buck Shlegeris:**
Another high-level takeaway that I've had as a result of all this thinking is: realizing the extent to which alignment people or AI safety people have conflated a bunch of different meanings of the word "alignment". In particular, it seems to me that a lot of the time, people use the word "aligned" to mean the situation is safe overall.

So I would say that from my perspective, AI misalignment, that is, the AI trying to evade your safety mechanisms in order to grab power, is indeed a really bad problem. It's not clear that the only way to solve that is to make the model not be misaligned. And by using the word "AI alignment" for the field, it kind of tempts people to think about that as the post-condition, and I think this leads to a lot of really sloppy thinking.

So for example, I think that a lot of work on scalable oversight, the people who do it are very confused about whether the thing they're trying to do is ensure that they've built a reward-generating process such that models that are trained using it do not start scheming. Or were they trying to rule out scheming? Were they trying to build a reward-generating process that is good, even if the model was scheming? Were they assuming to start with that the model wasn't scheming and hoping to prevent scheming with some other process?

I think they're just not very clear about this with themselves and in their conversation. And I think that this means that a lot of scalable oversight research, there's no coherent set of assumptions under which the measurement they did is an appropriate measurement of something we'd care about in the future for that protocol. So yeah, I think that being more precise about when we use the word "aligned", and not using it as a synonym for "safety", has revealed a lot of little inconsistencies in how people talk about stuff.

**Daniel Filan:**
Yeah, that seems fair. It's actually related to a thing which I appreciate being brought up in this conversation, of just specifying which assumptions we're making about AIs, like, "For this protocol to work, if the AI has properties XYZ, then we'll get thing ABC." I also think that's an important thing to just have out there.

**Buck Shlegeris:**
Yeah. I think there's kind of an unfortunate... So the AI safety community or AI safety technical researchers have kind of an activist background. A lot of them think a lot of their impact is coming from persuading people that the situation is dangerous overall. And a lot of the time they're really into pointing out the assumptions under which a particular safety proposal would fail. And in fact, it's very important to understand the situations in which a safety proposal would fail. But they seem very uninterested in cataloging these and thinking about the relationships between them and saying, "Well, that means that under the following assumptions, things would actually be okay."

And I think that because they're so used to trying to do the social move of saying, "Well, that doesn't guarantee safety," they fail to practice thinking through exactly what properties are ensured or do suffice to make something be safe.

**Daniel Filan:**
Yeah. I think I have some of this habit and I think partly it comes from thinking of myself as in the business of aligning arbitrarily smart AI. And I think "my job is to get to a stage where we can align arbitrarily smart AI, and if I'm doing technical research and it doesn't work on arbitrarily smart AI, there's a problem there". Or at least, maybe it's fine to do it, but someone's got to fix that problem.

**Buck Shlegeris:**
Totally.

**Daniel Filan:**
And I wonder if it comes from that habit of mine. Whereas it seems like you're explicitly not in the business of trying to get useful stuff out of arbitrarily smart AI.

**Buck Shlegeris:**
A lot of these problems come from the fact that the researchers in AI safety, some of them explicitly endorse, "We are aiming to come up with strategies that work for arbitrarily powerful AI." But almost all of the work which actually goes on, especially by people who are competent researchers or are employed or something, definitely does not apply directly to working on arbitrarily powerful AI and comes from an intellectual tradition that is not thinking about arbitrarily powerful AI most of the time.

So for example, [Paul Christiano](https://paulfchristiano.com/), when he's thinking about what safety research is good to do in practice, is absolutely not thinking of his job as... Right now he thinks of his job as building safety proposals that work for arbitrarily powerful AI, but that's because he's doing worst-case theory at [ARC Theory](https://www.alignment.org/theory/). That's absolutely not how he thinks about empirical work, or how empirical work should be, or what you need to do in order to minimize AI risk in general. But I think people don't track the distinction between these things and kind of cargo-cult work that previous safety researchers did under various other assumptions that they didn't end up hearing about, and it's all really a mess.

**Ryan Greenblatt:**
Yeah. One way to put this is: I think that basically very little [work] has a very clear story for working on the most powerful AIs, because it's just really brutal to work on the most powerful AIs, and almost everything is going to depend on some sort of bootstrapping story, where maybe it was initially aligned and then something else happened. Or a more control-style bootstrapping story, which is like, "Well, regardless of whether or not it was initially aligned, we did some work with the AI that then results in it becoming safe." And in that sense, we are targeting safety from arbitrarily powerful systems, but none of our work is about that, and so it's kind of weird to describe it that way.

**Daniel Filan:**
Yeah. I guess the things that seem promising for arbitrarily smart AI are stuff like doing a good job at agent foundations, trying to understand how we think things should end up thinking, to the extent that we think that we're doing some weird combination of arbitrarily smart AI and machine learning, trying to really understand the science of deep learning and what it's going to do and hopefully getting some abstractions that are still going to apply when we have smarter versions of ML models. I'm wondering what you think about those kinds of approaches, if you want to say.

**Ryan Greenblatt:**
I'm pretty skeptical any of this will work for arbitrarily powerful AIs. I think it seems pretty dubious. I don't know. I don't feel very strongly, though. I haven't thought about it a huge amount.

**Daniel Filan:**
Fair enough.

## How AI control relates to previous Redwood Research work <a name="control-within-redwood"></a>

**Daniel Filan:**
Speaking of things relating to this, I'm curious how it relates to previous work by Redwood Research. If I look at your website, you've done some existing stuff [Buck and Ryan laugh] ---I thought it was a reasonable thing to try---it seems like your existing work is some stuff on circuits inside neural networks, some stuff on causal scrubbing, so I guess evaluat[ing] the efficacy of circuits...This seems to have a somewhat different flavor. I'm wondering if you have thoughts about the relationship between these lines.

**Buck Shlegeris:**
So I would say that we tried to do some interpretability work, and then we started looking for good ways of evaluating whether our interpretability work was good, or whether anyone's interpretability work was good. And the ["causal scrubbing" paper](https://www.lesswrong.com/posts/JvZhhzycHu2Yd57RN/causal-scrubbing-a-method-for-rigorously-testing) was our attempt at coming up with a metric that we thought was sort of objective for whether interpretability work was absolutely successful at explaining what was happening in particular cases.

And then once we had this metric, we applied it to some explanations that we produced and we were very unimpressed with ourselves. We were like, "These explanations were quite poor," and I at least came away thinking that the state of interpretability was much worse than I had previously thought. Simultaneously, I was thinking a lot about what the applications of interpretability were and we started being like, "Okay, if the interpretability explanations that people are producing are not objectively impressive in terms of being highly explanatory hypotheses, maybe a direction we should go in our interpretability work is trying to come up with interpretability-ish or 'model internals-related techniques' that solve a problem that you couldn't have solved without using a model internals-style technique".

Another way of saying this is: there's basically two ways of justifying your internals-based research. You could either say, "We are coming up with an objectively good understanding of what's going on inside this neural net," or, "We did something using our internal-based techniques that you couldn't have done without our techniques."

**Ryan Greenblatt:**
"We solved a problem" in particular, like you have some specific task you were trying to solve, not just necessarily trying to produce understanding.

**Buck Shlegeris:**
Uh-huh.

**Daniel Filan:**
This sounds sort of similar to stuff [Stephen Casper](https://stephencasper.com/) was talking about in [his podcast with me](https://axrp.net/episode/2023/05/02/episode-21-interpretability-for-engineers-stephen-casper.html) about interpretability for engineers.

**Ryan Greenblatt:**
Yeah, I think we came to pretty similar conclusions.

**Daniel Filan:**
So those are two ways.

**Buck Shlegeris:**
Yep. And so then we were like, "Well, it looks like our absolutely evaluated understanding is not very impressive" And then we, again using the exclusive we, did some work on [measurement tampering detection](https://www.lesswrong.com/posts/inALbAqdx63KTaGgs/benchmarks-for-detecting-measurement-tampering-redwood), which was a paper where we tried to use model internals to solve a particular problem where we thought model internals-based methods had a serious advantage over things that didn't use model internals.

Now the actual details of that paper are kind of complicated and I don't want to get into them hugely much except saying it's kind of a more structured [weak-to-strong generalization](https://arxiv.org/abs/2312.09390) setting, where it's similar to the weak-to-strong generalization work, except we think there's better reasons to think that the problem is solvable and it's more specific. And around the same time, by coincidence, we decided to spend a lot of time thinking about the overall... what we were thinking of at the time as an alignment plan. A plan for: suppose we had to come up with some proposal for safely building powerful AI next year with no technological advancement. What should we do?

At that point [we] thought about a lot of this---Ryan did the majority of the quality-adjusted intellectual labor here or whatever---and we ended up thinking about this control stuff and being like: whoa, we were previously radically under-emphasizing the importance of measurability when you're thinking about different protocols you could apply. We were previously equivocating between "alignment" meaning "safety", and "alignment" meaning "the AI isn't trying to kill you", and we really should reason about these things differently.

And then had all these other ideas that felt a lot more basic for research that we should do. With the measurement tampering detection work, we were basically saying, "Well, we should try and come up with some techniques that are kind of moonshot-y or out there or not at all like the other things that are happening." And then after doing the control thinking, we felt more like we could do useful work by doing techniques that were more absolutely basic, but in this setting that we thought was particularly underrated and particularly important to advocate for.

So, does that answer your question about how our current work relates to our previous work?

**Daniel Filan:**
Yeah, I think it does.

**Ryan Greenblatt:**
I would say, our thinking has just adjusted a moderate amount. I feel like the measurement tampering direction is in principle a really great direction, but we're not doing it for a variety of reasons, or we're not currently planning on pursuing that very heavily for a variety of reasons. I think I feel less good about mech. interp., or our work on mechanistic interpretability, at least in terms of how useful that overall area will ultimately end up being. We came away with a variety of takeaways after thinking about safety overall and then ended up doing a variety of projects based on that.

One additional takeaway I will note, that Buck didn't mention, is thinking that people should really think about risk from scheming and deceptive alignment separately from risks from other problems and doing individual analysis on this. And I think that "scheming versus non-scheming risk" is a really good decomposition that people have historically I think not talked about. People have sometimes said "outer alignment versus inner alignment", but I think that's actually not a very good cut for a wide variety of reasons.

And I think "What is your threat model? Is your threat model scheming [or] non-scheming?"... In the case where it's non-scheming, there's a wide variety of solutions, even though both problems can involve concerns related to generalization and concerns related to imperfect ability to evaluate actions and things like this.

**Daniel Filan:**
So you're drawing a distinction between concerns about scheming and concerns about inner alignment. How are those different in your mind?

**Ryan Greenblatt:**
Here's an example inner alignment problem you can have, which is not well-described as scheming. You train your AI to perform well according to human labelers in a variety of contexts, and you trained your AI for a while and its actual behavior in that training process was acceptable at an absolute level. As in, it was just doing things that were pretty reasonable and good, but simultaneously it was doing things that were maybe slightly suboptimal that slightly exploited your oversight process.

And this means that the AI you end up getting is one that's reasoning about the oversight process and is trying to do various exploitation, and then in some future situation where there is an egregious exploit it could do that is actually dangerous, it does seize the opportunity to do so and causes problems.

So in some sense, this isn't really that well-described as an outer misalignment problem, in that your issue wasn't centrally [that] you missed a bunch of bad behavior, you failed to label it as bad. But instead [your issue] was [that] the AI generalized in a really problematic way based on what inner objective it had or its heuristics or motivational complexes. But at the same time, it's not super well-described as either, because if you did have perfect feedback, it would've solved the problem. Or if you had better mastery over the internals and what the internal structure was, that could have also solved the problem and it just didn't decompose that well.

**Daniel Filan:**
Okay, so for you that counts as scheming, but-

**Ryan Greenblatt:**
No, no.

**Daniel Filan:**
Oh, that doesn't count as scheming.

**Ryan Greenblatt:**
That specific case doesn't count as scheming because the reason why the AI took bad actions eventually was not that it was playing nice, it was just that it was doing the same thing always. In the simplest case, it was like, "I want to achieve this proxy," which was historically correlated with what got reinforced. And then in some new case, the best way, the way that naturally generalizes, would be to seize control.

**Daniel Filan:**
Okay. So for you scheming is when you suddenly start trying to do something different than what you were trying to do previously?

**Ryan Greenblatt:**
No. Scheming is, "The reason why I'm performing well is because I have long-run aims that I want to achieve." Also called "deceptive alignment" or whatever. The reason why the AI looks good isn't because it's just doing the thing on its own merits, it's because it's like, "If I look good now, then that will acquire power and influence for me in the future, or maybe for a different AI in the future." And that's pretty different.

So in the case I was describing earlier, the reason why the AI performs well is it's like, for example, "I'd like to make the human press the 'thumbs-up' button," though probably it's some more complicated mash of heuristics that is historically correlated with performing well.

**Daniel Filan:**
Right. So somehow "inner and outer alignment" is talking about the distinction of: is the thing you were worried about captured by the reward function? Whereas scheming is: is the reason you did that thing because you wanted to achieve some crazy outcome later, or is it just the obvious...?

**Ryan Greenblatt:**
Yeah. I feel like another general takeaway is: I think people in the safety community often have things that are better thought of as threat models, which they then think about as things you're trying to achieve for safety. I think "outer alignment versus inner alignment" is an example, where I think it is clarifying to think about misalignment due to bad rewards and misalignment which is not due to bad rewards, and I think those are useful frames, or can be useful decompositions. But I think being like, "Our goal is to achieve outer alignment," is a pretty confusing way to think about the problem. Which is of course in some sense not a very novel take, because I think people have expressed somewhat similar things in various contexts. Same for alignment versus misalignment also.

## How people can work on AI control <a name="how-work-on-control"></a>

**Daniel Filan:**
So if somebody is listening to this and they're interested in trying to work on the control agenda, trying to push it forward, what would you encourage them to do?

**Buck Shlegeris:**
I mean, suppose they're a researcher and they want to research control stuff, it's plausible they should do research projects on this. I think there's a lot of obvious follow-up work to do on our paper. A couple of people are working on stuff like that, I mean including us. They are welcome to email me for advice on doing control projects. I think there's reasonable ML papers you can write about control.

**Daniel Filan:**
What are some examples?

**Buck Shlegeris:**
So for example, the thing that I'm doing literally right now is a follow-up... Or sorry, literally right now I'm recording a podcast. It's similar to the control setting, it's similar to our previous paper, except instead of the task being writing code and the catastrophe being the code being backdoored, the model is an agent in a bash shell solving various random [DevOps](https://en.wikipedia.org/wiki/DevOps) problems for you and the catastrophe is it running malware that it downloads from the internet. This is just a different setting. It's going to demonstrate different failure modes and different protocols are going to turn out to be the best.

And then I think from doing this project, we're going to learn some things that we didn't previously know about the extent to which protocols work in a variety of situations; which things generalize from our previous paper and which things don't.

**Daniel Filan:**
Okay.

**Buck Shlegeris:**
To choose an example of a very different type of thing, you could try to do a control paper where there isn't a catastrophe that is unacceptable to happen even once. You could do what we would call a "low-stakes" control experiment, where the safety is just the proportion of actions which were taken that were bad.

So you could just take our setting, except imagine that after submitting backdoored code you just get one safety demerit point, but the setting doesn't finish, the scenario doesn't finish. And that's just another setting where the protocols are going to be somewhat different.

**Ryan Greenblatt:**
And closely related to that, I think the central case in which you see low-stakes-type stuff is: you wanted to do a bunch of work with your AI and you're worried that it would be systematically sabotaging that work. And so we're pretty interested in work that looks like: for example, the nicest case would be you have AIs do a bunch of small research tasks. And then the question is, can a red team create an AI that systematically sabotages those research tasks such that you yield incorrect conclusions a high fraction of the time? And it would be fine if you got incorrect conclusions 0.1% of the time, but it would be really sad if you got incorrect conclusions like 10% of the time. And the question is: how much can you cut down that percentage of the time that the answers are intentionally wrong? Or this sort of thing.

**Buck Shlegeris:**
Other work you could do is: if you work at an AI lab and are influential in them writing their [responsible scaling policies](https://metr.org/blog/2023-09-26-rsp/) or analogous documents, you could consider trying to ensure safety via a control-type approach. If you are interested in thinking about regulations that would ensure safety, you might consider using these types of ideas in your regulations. If you do technical research at safety labs, you might consider thinking about that research in this frame. A lot of work people do, that they label as "scalable oversight" work or a couple of other types of things, can be framed in terms of control. And I think the work would be often somewhat better if they explicitly adopted this frame and did control evaluations.

**Ryan Greenblatt:**
Also feel free to contact us if you want to talk about some of this stuff. Or particularly if you're a technical safety researcher who is about to do your next project, definitely feel free to contact us.

And I think another thing that's closely related is that I think in labs' RSPs [responsible scaling policies] or the equivalent of RSPs, they should have policies for what they do if they catch the AI. As in: one of the key things that we've talked about is, okay, our goal will be to ensure that we catch the AI before it causes problems, but we should also have a proposal for what's going to happen when we catch the AI. And I think this is somewhat under-discussed, [the topic] of: well, we're going to want to be able to make sure that we have some probability of catching the AI, and then also we need to have a plan for what happens, and we need to be ready to shut things down and be ready to respond, and have all of that set up ahead of time.

**Buck Shlegeris:**
I want to clarify re: the previous point that we do in fact talk to people from AI labs, rather than merely appear on AXRP hoping that they reach out to us.

**Daniel Filan:**
Fair enough. So speaking of things you might want to clarify before we close out the interview, I'm wondering: is there anything that you guys wish that I'd asked?

**Ryan Greenblatt:**
I mean, I think there are many topics to talk about, but I don't think there's any specific one that seems that important.

## Following Buck and Ryan's research <a name="following-buck-ryan-research"></a>

**Daniel Filan:**
Fair enough. Well, if people listen to this podcast and they're interested in following you two or Redwood Research in general, or maybe they want to reach out, how should they do that?

**Buck Shlegeris:**
They can find our emails. They can follow [me on Twitter](https://twitter.com/bshlgrs), they can follow [Ryan on LessWrong](https://www.lesswrong.com/users/ryan_greenblatt).

**Ryan Greenblatt:**
We're probably going to spin up a blog at some point, but we haven't gotten around to it yet. And so you can follow our blog, which is just going to be our LessWrong posts, but on a different website. So if you prefer a different website for some reason.

**Daniel Filan:**
Okay. And it's possible, given production timelines, that the blog will even be out by the time this episode airs. Who knows? And your emails, I'll include that in the description of this episode.

**Buck Shlegeris:**
Great.

**Ryan Greenblatt:**
Yep.

**Daniel Filan:**
All right. Well Buck and Ryan, thank you very much for coming on AXRP.

**Buck Shlegeris:**
Sure, thanks.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. This episode was filmed at a location provided by Lightcone Infrastructure. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future) and [Lightspeed Grants](https://lightspeedgrants.org/), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev and Tor Barstad. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
