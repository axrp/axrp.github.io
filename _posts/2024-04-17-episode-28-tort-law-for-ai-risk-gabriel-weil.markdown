---
layout: post
title: "28 - Tort Law for AI Risk with Gabriel Weil"
date: 2024-04-17 14:30 -0700
categories: episode
---

<!-- [YouTube link](https://youtu.be/hdQDZo7jGBY) -->

How should the law govern AI? Those concerned about existential risks often push either for bans or for regulations meant to ensure that AI is developed safely - but another approach is possible. In this episode, Gabriel Weil talks about his proposal to modify tort law to enable people to sue AI companies for disasters that are "nearly catastrophic".

Topics we discuss:
 - [The basic idea](#basic-idea)
 - [Tort law vs regulation](#tort-law-vs-regulation)
 - [Weil's proposal vs Hanson's proposal](#weil-vs-hanson)
 - [Tort law vs Pigouvian taxation](#tort-law-vs-pigouvian-taxation)
 - [Does disagreement on AI risk make this proposal ineffective?](#disagreement)
 - [Warning shots - their prevalence and character](#warning-shots)
 - [Feasibility of big changes to liability law](#feasibility)
 - [Interactions with other areas of law](#interactions)
 - [How Gabriel encountered the AI x-risk field](#how-encountered-x-risk)
 - [AI x-risk and the legal field](#ai-x-risk-and-legal-field)
 - [Technical research to help with this proposal](#technical)
 - [Decisions this proposal could influence](#decisions)
 - [Following Gabriel's research](#following-gabriels-research)
 
**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Gabriel Weil. Gabriel is an assistant professor at Touro Law. His research primarily focuses on climate change law and policy. He's recently written about [using tort law to address catastrophic risks from AI](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4694006), which we'll talk about in this episode. For links to what we're discussing, you can check the description of the episode and you can read the transcript at axrp.net. Well, welcome to AXRP.

**Gabriel Weil:**
Thanks for having me.

## The basic idea <a name="basic-idea"></a>

**Daniel Filan:**
Sure. So I guess we're going to talk about this paper you have. It's called [Tort Law as a Tool for Mitigating Catastrophic Risk from Artificial Intelligence](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4694006). And I think most of my audience won't be from a legal background and they might stumble over those first two words. What is tort law?

**Gabriel Weil:**
So torts is the law of basically any time you do a civil wrong that isn't a breach of contract. So "breach of contract" is you promised someone in some legally enforceable way that you would do something and you didn't do that. You breached the contract, they sue you. Basically anything else that you're suing a private party for, you're not suing the government - in a civil lawsuit, that is going to be a tort.

So most commonly you think of things like: you get in a car accident and you sue the other person - that's a tort. There's also intentional torts like battery. So if I punch you in the face, you can sue me for the harm I did to you. If I trespass on your property or I damage personal property, like I break your car window or I key your car, those are trespassory torts - that's called "trespass to chattels". Products liability is torts. Medical malpractice is torts. So that's the broad field of law we're talking about.

**Daniel Filan:**
I guess you have some idea for how we're going to use tort law to mitigate catastrophic risk from artificial intelligence. In a nutshell, what's the idea? How do we do it?

**Gabriel Weil:**
So in a nutshell, the idea is that training and deploying these advanced AI systems is a risky activity. It creates risks of what are called "externalities" in economics jargon: harms to people that are not internalized to any economic transaction. They're harms to third parties. OpenAI makes a product, someone buys that product and uses it, but it risks harming someone besides those two parties. Therefore, the risk is not reflected in the price of the product.

And so in principle, holding whoever is responsible for the harm, whoever caused the harm, liable for that, making them pay for the harm they cause, should result in them taking enough precaution to optimally balance the risk and reward. So in the same way in your own life, when you're deciding whether to drive to work or walk to work or bike or whatever, there's various costs and benefits, you're weighing those.

Some of them have financial costs, health benefits, time costs, and you're able to weigh those risks for yourself. We generally trust that process to work pretty well because you fully internalize the benefits and costs there. You might make some mistakes sometimes, but your incentives are pretty well-aligned. But if there's some sort of externality to what you're doing - you driving to work creates pollution - most of the harms of that are felt by other people, then we might need some legal response to account for that.

And so tort law works well for that when we have a concentrated harm. So for pollution, it wouldn't work so well necessarily for everyone that's harmed by your pollution to sue you. You may want some other policy tool, like a tax on gasoline or on pollution, to align your incentives.

But if you hit someone with your car, and if there's a risk that when you take that trip you're going to hit someone, making you pay for the harm you caused works pretty well to align your incentives, to make sure that you don't take the trip if it's not necessary and you drive with appropriate precaution: you don't text while you're driving, you don't speed too much. Now, our existing tort system might not do that perfectly, but in theory, the prospect of liability should make you exercise the right amount of precaution. So that's basically what I want companies that are building and deploying AI system to do: to have the incentive to exercise the right amount of precaution. I don't think our current tort system necessarily does that, but I think it can with some tweaks.

**Daniel Filan:**
Gotcha. So to summarize, it sounds like basically the fear is: if you're an AI company, there's some risk you destroy the world, and that's much worse for the world than it is for you. So you're just going to be more relaxed about that risk than you would be if you really felt the responsibility, if you really were internalizing just how bad it would be if the world got destroyed (or some other catastrophic risk).

**Gabriel Weil:**
So in the limit, that's definitely part of what I'm worried about. But I think even for less catastrophic scenarios, there's harms that wouldn't necessarily be internalized by these companies. But yes, in the limit, I think it's definitely true. Now, obviously if you destroy the world, you kill yourself too, so that's bad for you. But it's not as bad, you don't feel that as much as killing eight billion people and all future civilization. I think you want the law to make them feel that.

**Daniel Filan:**
Right. So it seems like the obvious difficulty here is that: suppose you literally destroyed the world. It's too late to sue. This is the first-pass difficulty with some sort of liability scheme. How do we deal with that?

**Gabriel Weil:**
So yes, it is true that there are certain class of harms, of which existential harms are a subset, that are what I would call 'practically non-compensable'. So you can't actually bring a lawsuit or you can't collect a judgment for it. I think that includes extinction risks. It also includes risks of harms sufficiently disruptive that the legal system would no longer be functioning.

And it also includes risks short of that, that would just be financially uninsurable. So if you kill a million people, the damage award is going to be so large that you're not going to be able to pay that out, no plausible insurance policy is going to cover that, and so it would put your company into bankruptcy and most of that damage is not going to be recoverable.

The normal type of damages that we use in tort lawsuits, which are called compensatory damages - damages designed to make the plaintiff whole, to make them, in theory, indifferent between having suffered the injury and getting this pile of money or having it never happen - those aren't going to work to get at those risks. We need some other tool. And so the tool that I propose are what are called "punitive damages".

So these would be damages over and above the harm suffered by the plaintiff at a particular case, that we're assigning because there was a risk that things went much worse. The system does some practically compensable harm, but it does it in a way that it looks like it could have gone a lot worse. Say there was a one in 10,000 chance of killing a million people and a one in a million chance of causing human extinction.

Then you would want to figure out what the expected value of that harm is, the probability of the harm times the magnitude and say, "Well, we're going to pull forward that counterfactual liability from the risk that you took but wasn't realized, and allocate it across the practically compensable cases."

**Daniel Filan:**
Gotcha. So the way I understand the policy is roughly: we're going to say that when you have some AI harm, if it's the kind of harm that's really closely related to these sorts of harms that I couldn't sue you for - like extinction, like some sort of catastrophe that would super disrupt the legal system, or just a judgment that's too big to collect on - then basically the punitive damages are going to say, "Okay, what types of harms [is] that related to? How similar is this to those harms? How much would dealing with the problem that you actually caused fix those harms?" And basically the punitive damages are going to reflect those factors roughly.

**Gabriel Weil:**
Right. I think we care about two things: how much uninsurable risk, or practically non-compensable harm risk, was generated by deploying the specific system that you deployed in the way you did. And then we care about if you had a strong incentive to avoid this specific harm and you took costly measures to prevent it or reduce its likelihood or severity, to what extent would that tend to mitigate the uninsurable risks? And the more that's true, the more punitive damage we want to load onto those types of cases.

**Daniel Filan:**
Gotcha. This is a bit of a tangent, but you mentioned that at some level of harm, you're not going to have the money to pay the damages for this harm. Even getting an insurance policy, no insurance company would be able to pay out. How much harm is that? What are the limits of just making people buy liability insurance for this kind of thing?

**Gabriel Weil:**
So my understanding is that current insurance policies tend to top out around $10 billion. In the paper, I used a much higher threshold for insurable. I used a trillion. I think we would want to push the limits of what's insurable to really find out, but I think that's an open question that needs to be explored before this is fully ready to implement.

I think you would want to start out by assuming the uninsurability threshold is on the lower end. And then if they can prove, "well, I can insure for more than that", then you would say: okay, the expectation of harm or the possibility, the risk of harm is below that, [so] we handle that with compensatory damages. Since you've shown you can take out a $100 billion insurance policy, then that's going to be the cutoff. Risks of harms above $100 billion will go into the punitive damages calculation.

**Daniel Filan:**
Right. Wait, did you say $10 million or $10 billion?

**Gabriel Weil:**
$10 billion.

**Daniel Filan:**
I guess to ballpark that: in the US, the value of a statistical life that [regulatory](https://www.epa.gov/environmental-economics/mortality-risk-valuation#whatvalue) [agencies](https://www.transportation.gov/sites/dot.gov/files/docs/2016%20Revised%20Value%20of%20a%20Statistical%20Life%20Guidance.pdf) use is $10 million per life or something. So $10 billion [means] you can get insured for the legal liability for killing 1,000 people, but not 10,000.

**Gabriel Weil:**
That's the right order of magnitude. As I explained in the paper, the numbers that are used by regulatory agencies are a little bit different than what's used in tort judgments. [The numbers in tort judgments] tend to be a little bit lower, in large part because the way mortality harms are valued doesn't actually value the value of the person's life to them. [It's a] weird quirk of tort law that I think should be fixed for other reasons and is more important in this context, but that's a tangent.

**Daniel Filan:**
So this is the scheme. In the paper, you focus on how it could be implemented in a US context. If people are listening from other countries or other legal systems, how widely applicable is this sort of change?

**Gabriel Weil:**
There's two ways of answering that. The beginning of the paper, that lays out what the ideal regime looks like - I think that's true regardless of what status quo legal system you're working from. In terms of what doctrinal or what legal levers you'd have to pull to get to that outcome or to get to that regime, that's going to vary across countries. I would say in any common law country, so that's basically English-speaking countries, tort law is going to be broadly similar.

There will be some detail difference; particularly products liability regimes, since those came later, are going to be different. But the basic structure of negligence law is going to be pretty similar across all common law countries. And so a lot of the same considerations are going to come into play. But I would invite scholars working in other legal systems to flesh out what this would look like and precisely what levers you'd have to pull in their system.

**Daniel Filan:**
Gotcha. So one somewhat basic question I have about your scheme is: suppose that Party or Company A makes this really big super smart foundation model. Party B fine-tunes it, makes it really good at a certain task. Party C then takes that fine-tuned model and sells it or deploys it or something. Suppose the model that C deploys causes harm, who are we imagining suing for that?

**Gabriel Weil:**
Who's suing is easy: it's whoever is harmed, right? I think you're asking who's sued.

**Daniel Filan:**
Yeah.

**Gabriel Weil:**
I think you could potentially sue all of them. If you're talking about under my preferred regime where we're treating training and deploying these models as abnormally dangerous, you would have to figure out at what step and which actors undertook these abnormally dangerous activities. If all of them did, then strict liability would apply to all of them.

If the way the application of the standard works [means] you would say, "Well, this step in the chain wasn't abnormally dangerous," then that would be assessed under negligence principles and you'd have to be saying, "Well, did they breach some duty of care?" But in principle, there could be what's called joint and several liability, where the plaintiff can sue all of them or pick which one they want to sue.

You can never collect more than the full damages, but you can pick your defendant. Now, different states have different liability regimes for that purpose. [Most states](https://www.whiteandwilliams.com/assets/htmldocuments/Subro%20Charts%20Updated%205_10_16/JOINT%20AND%20SEVERAL%20LIABILITY%20-%20REV.%201-10-20.PDF) file what's called "joint and several liability", which means you can collect the whole judgment from any one of them, and then they can sue their joint-tortfeasors, they're called, for what's called "contribution", basically for their fault-apportioned share.

Then there's other states that use what's called "several liability with apportionment", where you can only sue each defendant for their fault-apportioned share of the liability. And fault apportionment is just this idea that if you have multiple defendants, or multiple tortfeasors, that are causally responsible for the same injury, you do some allocation based on how faulty their conduct was, or in the context of strict liability, how liable they are. That concept doesn't apply as well in a strict liability context, but you would want to do a similar analysis.

**Daniel Filan:**
But broadly the sense is, the court would just decide who is actually responsible for the really dangerous stuff that the AI ended up doing and they would be liable for the thing.

**Gabriel Weil:**
So I want to make a distinction there. When you say courts, I assume you mostly mean judges. There's different roles that judges and juries have in this process. Judges resolve questions of law, juries resolve questions of fact, is the high-level distinction. And so [in the case of] a negligent system, breach of duty is a question of fact, but what the duty is is a question of law. If we were talking about the threshold question of "is this activity abnormally dangerous such that strict liability should apply?", that's a question of law that a judge would resolve.

**Daniel Filan:**
Okay, I think that makes sense. The final framing question I want to ask about this is: it seems like a lot of this would be implemented on the jury side. A judge would tell a jury, "This is roughly how you should figure out the damages and go and deliberate and tell me what you decided." Is that right?

**Gabriel Weil:**
So certainly the actual damages calculations would be fact questions that would be decided by a jury in a typical case. The way judges review those is if they conclude no reasonable jury could have reached a given result then they can overturn it, but juries are supposed to have pretty wide discretion. Now, whether punitive damages would be available at all is a legal question. It's a legal question resolved by judges.

So under current law, it requires malice or recklessness as a threshold requirement for punitive damages to be applied. There's also various limits under the due process clause of the Constitution that limit the ratio of compensatory damages to punitive damages. Those questions would be resolved by judges. And so juries would be operating within the confines of those legal rules.

**Daniel Filan:**
Gotcha. My question is: one could imagine that juries are really good at assessing these kinds of things. They're just very good calculators. They really figure it out. One could also imagine that juries just roughly do what they feel is right and maybe they're forced to be in a certain range by a judge, but maybe they're kind of random, or maybe they stick it to the bigger party or something like that.

And in the second world, it seems like it's just going to be hard to implement this kind of scheme, because maybe we just can't tell juries what to do. So I guess my question is: how good are juries at implementing formulas and stuff that judges tell them to?

**Gabriel Weil:**
So most damages calculations are pretty black box. What's the pain and suffering? For certain things we can assess [it] better, e.g. lost wages are easier to quantify. Pain and suffering is inherently pretty hard to quantify, and that's regularly part of damages awards. We just deal with the fact that there's going to be experts to testify and then juries come up with a number. In this context, I think you would have dueling experts where different experts are testifying and saying, "Well, this was the risk."

Obviously there is deep disagreement in people who think about AI alignment, AI safety about how likely these catastrophic outcomes are. Now, hopefully the context in which a system failed in a certain way where it looks like it could have gone a lot worse will be probative on the question. We're not trying to ask the question of "what's the global probability that human extinction will be caused by an AI system?" We're trying to ask "what's the probability that the people who train and deployed [this system], when they made those decisions, what should they have thought the risk was?" And we can update on the fact that it failed in this particular way. We don't want to over-update on that, because in some sense, ex post the risk of a worse harm is zero, and they didn't know it would fail in this particular way. But the fact that it failed in the way it did can reveal some things about what they knew or should have known at the time they deployed it.

So I think, yeah, juries aren't going to do this perfectly, but I also don't think they need to. What really matters here is the expectation that if you take a risk, you're going to have to pay for the expected harm arising from that risk. And so as long as juries aren't systematically biased in one direction or the other, as long as they're very roughly noisily coming up with a number that tracks the risk, that's going to do what you need to do in terms of generating the expectations of liability. So that's a failure mode I'm less worried about than others.

**Daniel Filan:**
So just to give color to this, it sounds like maybe what would happen is there's some sort of trial based on some harm resulting from something kind of like AI misalignment. And then the defendant's legal team brings up some expert to say, "Oh, this wasn't that bad, and it's not that related to really scary harms," and the other expert says, "No, it's really bad." Somehow the juries are picking between different people suggesting different things about what the damages can be that are guiding their assessments. Is that a reasonable thing to imagine?

**Gabriel Weil:**
Yeah, it's how I would expect the damages phase of a trial like this to go.

## Tort law vs regulation <a name="tort-law-vs-regulation"></a>

**Daniel Filan:**
Okay, great. A question I want to ask is: if I think of most AI governance work, I think that it operates in a framework of saying, our plan has two parts. Firstly, there's going to be some process where the government or a bunch of smart scientists figure out what AI can do, how scary it might be, and make that really legible to regulators.

And then secondly, there's going to be some kind of law or some kind of regulatory body that says that if you make a really big scary AI, we're going to set out some rules for you to follow and you just have to follow them. And we're going to design the rules so that if you follow those rules, the AI should hopefully be safe. Your proposal feels like kind of a different flavor than these sorts of proposals. So I wonder how you think these kinds of schemes compare?

**Gabriel Weil:**
So to my mind, the key advantage of a liability framework is it doesn't require that you and the government know what specific steps have to be taken to make your system safe. I don't think we know that right now. Maybe we'll get there at some point, but I don't want to rely entirely on the government being able to specify a procedure that makes your AI system safe.

What liability does is it shifts the onus to figuring that out onto the companies, the labs where most of this expertise resides. I think it's going to be difficult for government to bring the kind of expertise in-house that gets them to where the leading labs are. Even the leading labs don't really know how to build safe systems right now. So I want them to not only be throwing everything they have in terms of, once they've made a decision to deploy, making it safe, but if they're not confident a system is safe, if they think deploying a particular system given the current state of knowledge creates a one in a million chance of human extinction, I want them to wait six months until better interpretability tools come around (or whatever the idea is; I'm not a technical AI safety researcher). But I want them to be thinking, "I need to be as cautious as I would be if I owned the world, basically, and if destroying the world was going to destroy all that value for me".

That's not to say that there's no role for what I would call prescriptive regulation of the kind you were describing, but I think what's really important in that context is that those prescriptive rules don't preempt or displace the tort liability. Sometimes judges interpret regulatory schemes as having a preemptive effect, either because they're viewed as conflicting with tort law or the regulatory scheme is viewed as having occupied the field, and so impliedly preempting the tort regime.

I think that would be a really bad outcome. You can avoid that pretty easily in any legislation that's enabling a new regulatory program by including what's called a savings clause that expressly disavows any preemptive effect. And once that applies, we can talk about "are there specific measures that would buy some safety if we require them?" I don't think those are necessarily bad ideas, I think some are more valuable than others. But I don't think we want to rely entirely on that.

**Daniel Filan:**
To me, it seems like the distinction is: these sorts of rulemaking schemes, the rules, the stuff you have to follow, it comes very early in time, maybe before you know as much what's happening. Whereas if you can do it right, something like a tort law scheme, it brings in the legal force at a time where there's some medium-range problem with your AI that's not so bad that your legal system doesn't function, but bad enough that you know more what happened.

In my mind, it seems like the advantage is that it's a more informed place to make these decisions, such that AI companies optimizing for that are basically going to be doing better things than if they're just optimizing for following set rules. Does that sound right?

**Gabriel Weil:**
A rule that you set through regulatory policy may not pass a cost-benefit test. You might have some unnecessary rules, and there also might just be things you didn't think of to require or you decided not to require that would've bought you a lot of safety. So if you get the rules perfect, if you require everything to pass a cost-benefit test and you don't require anything that doesn't, then maybe a regulatory regime is sufficient and better. But I don't have confidence in this domain that regulators are likely to approach that.

**Daniel Filan:**
I guess there's a difficulty where on some level, you're hoping that developers are able to figure out what the right cost-benefit is for themselves to do, but also there are obvious problems with them setting regulatory policy. I guess I think of it as just an interesting way to solve a knowledge problem.

**Gabriel Weil:**
It's also worth pointing out that some forms of prescriptive regulation work really well with liability. So in particular, there's proposals for these AI model licensing regimes, and I think that would pair really well with a liability insurance requirement system. So instead of the decision being binary, yes or no, do you get a license or not, what the regulator would do is decide, here's how much liability coverage you need in order to deploy this system. Here's the worst amount of harm we think you could do. And then you could deploy it if you can convince an insurance company to write you a policy you can afford. And that's going to depend on, maybe there would be some set of alignment evaluations or safety evaluations that they rely on in order to do that underwriting process.

I think you want the decision about whether a system is deployed to depend on whether its expected benefits for society are more than its expected costs. And if they have to buy insurance against the worst-case outcomes and convince an insurance company they can afford it, that's a pretty good proxy for that. Whereas I think I'm less trusting of a binary government decision, "are we going to license this model or not?"

**Daniel Filan:**
I actually want to talk about synergies, because I think there's also a synergy in the fork of the plan where we're going to have [NIST](https://www.nist.gov/artificial-intelligence-safety-institute) figure out what kinds of AI designs are more or less safe, or figure out ways of evaluating AIs for danger. It seems like this potentially has a synergy with the tort law plan.

**Gabriel Weil:**
I guess there's two different ways that could work. One is if we're still in negligence world, if my ideas don't take the world by storm and we don't have strict liability on abnormally dangerous activities theory, then NIST promulgating these standards... if you're not following those standards, that's at least going to be evidence of negligence.

Now, there's a doctrine called "negligence per se", that if you had actual regulatory requirements and you don't meet those, then that would automatically be negligence. But if they're just guidelines that NIST is issuing, that would be indication that you're not exercising reasonable care, but it wouldn't be dispositive.

**Daniel Filan:**
I was imagining also if we do adopt your proposal, it seems like this kind of thing might be informative of how risky that activity actually was.

**Gabriel Weil:**
So how much uninsurable risk you took when you deployed it if you didn't follow the standard, is that the idea?

**Daniel Filan:**
Maybe it's not a standard, but maybe it's just some kind of measurements... There's some rule that you have to submit models to this organization and this model will get a blaring red light, and then cause some sort of problem, and that's even more evidence that there was something pretty dangerous about it.

**Gabriel Weil:**
Yeah, so I definitely think there's scope for more technical work in terms of evaluations of these models, both in deciding whether to deploy them and saying how much insurance you have to take out and for doing these damages calculations. If harm has happened, can we try to use post hoc evaluations to try to figure out, "well, could it have gone a lot worse? What would that have looked like?"

## Weil's proposal vs Hanson's proposal <a name="weil-vs-hanson"></a>

**Daniel Filan:**
Sure. I guess the next thing I want to compare to is... So in your paper you cite this blog post by [Robin Hanson](https://en.wikipedia.org/wiki/Robin_Hanson) about ["foom liability"](https://www.overcomingbias.com/p/foom-liability), which to my knowledge is the only previous time people have talked about a scheme roughly like this. And he imagines a proposal sort of similar to yours, except there's a fixed formula where they say, "okay, you're going to assess punitive damages and the punitive damages are going to be based on, on this checklist of ways AI could kill everyone, how many items did you check off?" And the more items of that list [you check off], the worse the punitive damages are by a set formula. So [in] your proposal, I take the main difference to be that instead of being this strict formula, people just have to figure out: [if you tried] to prevent this certain harm that actually occurred, how much would that prevent really bad catastrophes that could have happened? So I'm wondering what do you think about [the] pros and cons of each one?

**Gabriel Weil:**
I think I talked to Robin about this. His motivation for having that formula was to limit the discretion of judges and juries. I see that as not particularly viable in this context since his formula at least strikes me as fairly arbitrary. It's taking it to the power of the number of these different criteria that are checked off. I think a lot of those criteria are not actually binary, so it's unclear how you would implement it in cases where it's "sorta kinda self-improving" or something. So I think that's an issue.

I think weighting all these factors equally doesn't seem super persuasive to me, but I do see the value in having a formula. That said, I provide a formula for my formulation of punitive damages. Now a lot of the variables in that formula are going to be difficult to estimate, so that's a real challenge, but I think the advantage of it is it's tracking the thing we ultimately care about: how much harm did you risk and how elastic is that with the harm that we actually realized here?

And so I think I would like to see a lot more work to put meat on the bones of how to estimate the parameters in that formula. But in my mind, you should be aiming at the target and doing as well as you can as opposed to... I think at first blush, it looks straightforward to apply Hanson's formula, [but] when you really unpack it, I think there's still going to be a lot of discretion there. And so I think maybe it limits discretion a little bit, but not as much as you'd like. It's loosely correlated with the thing we care about, but it's not going to reliably track it in the way that my preferred approach [does]. Does that make sense?

**Daniel Filan:**
That makes sense. Suppose we don't use Hanson's formula in particular, but suppose what we do is, the world just spends a year, we look at your formula and then we say: okay, what's something kind of like the Hanson formula that really would approximate what your formula tells us to do? But we're going to try and have something that you can really nail down. To the greatest extent possible, we're going to have something where there's very little discretion on judges and juries so they can apply it sort of automatically. And we're going to lose something along the way; it's going to be a bit of a bad approximation, but hopefully it's going to be really predictable. We're going to lose some value of aiming for the ideally optimal thing, but we're going to gain some value of predictability. And I'm wondering: how do you see that trade-off? How should we think about it?

**Gabriel Weil:**
So I think that would be really valuable. There's a question of how that will be implemented. So one thing you could say is: someone comes up with that formula, you have your expert testify about it. Juries can incorporate that, right? It's the same way that we incorporate any sort of scientific expertise in that domain. I think that is the most likely pathway if this is going to happen in a common-law, a judge-created way. I think it's unlikely that judges are going to create a rule of law that say juries have to follow the specific formula that somebody came up with.

On the other hand, if this is done via legislation, certainly legislators, if they want to, can hard-code that formula into the statute and then juries have to follow it. So if you have a formula that you think is really good and unlikely to be improved upon, or if you think that, if we accomplish something better, we can amend the legislation. If it's good enough, then I could see compelling judges and juries to follow it. It would just depend on how good you think the formula is and how easy it is to estimate the parameters in it.

So if you have a really good formula that, if you know the parameters... I think my formula is totally determined if you know the parameters. The problem is estimating those is really hard. If you have one that has easily-estimable parameters and you're just saying, "jury, you have this narrow task of coming up with good estimates of these parameters, and then that's all we're going to ask you". And then mechanically that will produce a damages award. I think that'll be great if you can do it. I don't think technically we're there right now.

**Daniel Filan:**
Yeah. I guess also, one difficulty of this is: this seems sort of similar to formulas that get used in criminal law. Sometimes legislatures want to say, "okay, we're going to have some mandatory minimums" or "we're going to just have some rule and we're going to ask judges to basically mechanically apply the rule". And I get the sense that the legal profession kind of dislikes this, or judges kind of dislike this.

So firstly, I'm wondering if you think I'm right? And secondly, to what extent does that suggest that we should be shy of implementing a rigid formula here?

**Gabriel Weil:**
So I think mandatory minimums in particular are fairly crude. And there's this general trade-off in law, which you may call "rule standards", "discretion versus rules". There's this idea that the more discretion you give judges in individual cases, the more you're going to be able to accommodate details of cases that might not be captured by an over-broad rule.

On the other hand, you're going to have a lot more noise and potential for bias if you let judges and juries have more discretion. And so there's this basic trade-off. I think what's new in this context is there's a competence issue. It sounds like you don't totally trust juries to be able to evaluate these questions, and so you want to make their job a little easier. I think we do have a way of dealing with that - different people have different ways of judging how well it works - of letting juries - lay juries, non-expert juries - hear from experts and then adjudicate the credibility of those experts and then come to a determination.

But I think again, if we had a formula that was good enough... I think you would probably want something better than just "if you commit X felony, you get a minimum of 10 years". I don't think something of that level of simplicity is going to work for estimating the uninsurable risk arising from an AI system. I don't know what that would look like. But I think if you had something sufficiently sophisticated where the parameters were easier for the jury to estimate... Again, I don't have a strong sense of what that would look like, but I think that could be really useful.

## Tort law vs Pigouvian taxation <a name="tort-law-vs-pigouvian-taxation"></a>

**Daniel Filan:**
Okay, fair enough. So another kind of proposal I want to compare against is: I think you mentioned very briefly early on something like [Pigouvian taxation](https://en.wikipedia.org/wiki/Pigouvian_tax), where we say that doing this kind of activity is broadly dangerous, and we're just going to say whenever you make a model that's X big, or maybe a model that trips up X number of dangerous capabilities or something, that's just inherently risky, and therefore you have to pay a certain fine or a certain tax regardless of what happens. So similar to a carbon taxation scheme. And these kinds of schemes are often considered pretty desirable in the settings where there are pretty broad-based harms that could occur. So I'm wondering what you think about schemes like that.

**Gabriel Weil:**
So I'm a big fan of Pigouvian taxes generally. In my earlier life I was a carbon tax advocate. [A](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4490602) [lot](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4490594) [of](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3381186) [my](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3537823) [work](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3788661) is on climate change law and policy. I think that there's two big differences between the climate context and the AI risk context. So one is if you are harmed by some climate outcome, it would be really hard to come up with how you can bring a lawsuit, because everyone in the world contributed to that. To say that there's a 'but for' cause of any particular ton of carbon being emitted, that that caused your injury, that's going to be a total mess and you'd basically need to sue the whole world. That's one problem. So that's the thing that makes climate change harder to use this liability tool for.

Conversely, it's really easy in the climate context to say, "okay, we know what the unit of generating risk or harm is. It's a ton of CO₂ equivalent". And so we can say: we might disagree exactly about how much risk or how much harm you generate by emitting a ton [of CO₂], there's different estimates of the social cost of carbon, but we can measure how much you did of that. We can come up with some tax and apply it.

I think both of those are flipped when we talk about the AI context. So AI systems are likely to harm specific people and more importantly, it'll be specific systems that harm them. So it's not like "the existence of AI is what harmed me": some specific system was deployed and that harmed me. That's who I know how to go sue.

All tons of CO₂ emitted to the atmosphere do the same amount of damage. Now, maybe a marginal ton at some point is worse than others, but two different people emitting them at the same time, me driving my car and you driving yours, are doing just the same damage.

And that's not true reliably for building an AI system of a certain size. You want to differentiate between companies or labs that are taking more precautions, are doing more alignment research, taking more steps to make their system safer. We don't want to just tax AI in general, but particularly we want to tax misalignment. So one framing that I really like is: people worry about an alignment tax, that it's costlier both financially and in terms of time and other resources to build aligned systems. And so one thing you can think about AI liability doing is creating a "misalignment tax", and hopefully that's bigger than the alignment tax.

If we could perfectly assess at a time a model is deployed how risky it is, then maybe that would work well, but then if you could do that, then you could just have some binary decision about whether you're allowed to deploy. Maybe you might still want a tax because there's uncertainty about what the benefits of the system are, but I think we're not in that epistemic position. We don't have the ability to assess ex ante how risky the system is. Once it's done harm in particular ways, I think we'll have more visibility into that. And so that's why I think a liability regime works better in this context.

## Does disagreement on AI risk make this proposal ineffective? <a name="disagreement"></a>

**Daniel Filan:**
Yeah, that makes sense. I guess a final thing I want to ask is: it seems like this proposal is really targeting externalities from AI research. It's imagining a world where people who run AI companies, they basically know what's up. They basically know how risky systems are and what they would have to do to make them less risky. And the reason they don't is that they're inadequately incentivized to, because the world ending is only so bad for them - or really bad catastrophes, they're only so bad for the AI company, but they're much worse for the world.

And I think it's not obvious to me if this is the right picture to have. We see pretty different assessments of "what are the chances that AI could cause really serious harm?" The world ending, the really serious harms that x-risk people talk about, they're not fully internalized, but they're quite bad for the companies involved.

**Gabriel Weil:**
Orders of magnitude less bad for them than for the world. So if you have a one in a million chance of destroying the world, but a 50% chance of making a hundred billion dollars, the calculation for you looks a lot different than the calculation for the world. That's a negative expected value bet for the world, but a positive expected value bet for you.

**Daniel Filan:**
I think that's right. I think on views where the probability of doom is way higher than one in a million... I think a lot of people think that the probability of doom is higher than 10%.

**Gabriel Weil:**
From a specific system?

**Daniel Filan:**
Maybe not from a specific system, maybe from AI development in general. So I guess my question is: how do you think we should figure out... If I'm assessing this, how do I tell if most of the risk is externalities versus individual irrationality or stuff like that?

**Gabriel Weil:**
So I think that's a fair critique that, say, "okay, maybe the people who buy these x-risk arguments, the people at (say) Anthropic or some of the people at OpenAI at least, at DeepMind, are going have even more incentive to be cautious. But [at] [Meta](https://ai.meta.com/), [Yann LeCun](https://en.wikipedia.org/wiki/Yann_LeCun) doesn't believe in x-risk really, so he's not going to be so worried about this". And I think that's true if you only have the liability part of my framework.

If you have the liability insurance requirements part of it, then you have to convince an insurance company, that's going to be a much more cautious actor, that you're not generating risk, because that's going to introduce that more cautious decision maker into the loop and put a brake on the process.

And so I think I'm less worried about insurance companies, [because] their whole job is to be cautious and to avoid writing insurance policies that are going to pay out more than they cost in expectation. I think that's going to be an important framework for the actors; that the main problem is their assessment of the risk rather than their incentives to account for it.

**Daniel Filan:**
So I think this works for the problem of AI developers who have abnormally low estimates of the risk. So I guess I'm looking at a world where I feel like there's a lot of disagreement about AI risk, and it seems like this kind of disagreement... On the one hand, it seems like it's the motivation behind [having] some sort of tort law scheme, rather than "well, we just know what to do, so we're going to make a law that says you have to do that".

But it seems like it causes some problems, partly in that AI developers, or maybe even insurance companies that AI developers have to buy liability insurance from, they might not know what kinds of things to avoid. It also seems like it means that these sorts of pervasive disagreements are going to make it really hard for juries to assess how big should the punitive damages be. So one might worry that we just have so much disagreement that this kind of liability scheme can't really help us. What do you think?

**Gabriel Weil:**
I think I want to distinguish two different objections there. There's an objection that this disagreement makes it hard to implement the framework, or that this disagreement makes it so that if you could implement this framework, it wouldn't buy us that much safety. I think the concern that it's hard to implement, I think is true. I think a lot of technical work needs to be hashed out, needs to be done to implement this framework reliably. I think you can implement in a rough and ready way now and that would still buy you a lot of risk mitigation, but there's a lot of refinement that could be done, a lot of knowledge that could be built, consensus that could be built, that would allow you to more reliably track what the risks that these companies are taking are. And that would make the framework more valuable.

And the other point I want to make on that is that whatever you think the epistemic burdens of implementing this framework are, they are lower than [the burdens] for implementing prescriptive regulations. You not only have to know how big the risks are, you need to know what to do about them. And so I think if your concern is our poor epistemic position with regard to AI risk, I think that tends to favor liability relative to other approaches, not disfavor it.

Then there's the question of "is it going to influence behavior in the right way because people might have different beliefs?" So I made the point already about liability insurance and how that introduces more cautious actors. I think if what you're ultimately saying is, "look, people building [in] these labs are still going to make mistakes. They might deploy a system that, based on everything anyone could know, looked like it was going to be safe and that wasn't, and then we're all dead. And who cares if in theory there should be liability for that?"

And I think what I want to say is: short of an outright ban on building systems beyond a certain level or certain kinds of systems, I just think policy is not going to solve that particular scenario. What we want from policy is aligning the incentives of these companies with social welfare. Maybe we also want it to subsidize alignment research in various ways. But there is a sort of irreducible technical challenge here, [and] I think you're asking too much of policy if you want it to solve all of that.

**Daniel Filan:**
Yeah, I guess if I think about the question, the case for regulation is most persuasive in a world where these AI labs, they don't know what they're doing, but I know what they should do. But in a world where we're all in similar epistemic positions than maybe the tort law approach seems like it makes more sense.

**Gabriel Weil:**
Or if the labs know better than-

**Daniel Filan:**
Or if they know better, yeah.

**Gabriel Weil:**
Maybe you, Daniel Filan, know better than what the folks at OpenAI do. I don't think Congress knows better.

**Daniel Filan:**
I don't know about that [whether Daniel Filan knows better than what the folks at OpenAI do].

**Gabriel Weil:**
I think Congress is going to listen to a lot of experts, but I don't know if you [watch](https://www.c-span.org/) what goes on in DC: the idea that they're going to write legislation, that regulators are going to come up with something that reliably makes us safe... I'm just very skeptical. I think they could do some things that are helpful, but it's not going to be anywhere near sufficient. I think some of the things they end up doing might be harmful. I think politics and regulatory policymaking is very messy. And so I think if you're relying on that to make us absolutely safe, I want to pour some salt on that.

Also, even if your idea is the thing that I was throwing out there as the extreme position, let's outright ban development of systems beyond a certain level - I think that even if you could make the domestic politics in the US work, which I don't think you probably can, and even if you thought that was desirable, enforcing that globally is going to be extraordinarily difficult. Now you could apply some of that critique to liability too. [But] I think that's a much easier lift.

## Warning shots - their prevalence and character <a name="warning-shots"></a>

**Daniel Filan:**
Gotcha. I also want to ask: for this approach to work it seems like we need... In worlds where the risk of misaligned AI causing tons and tons of uninsurable damage is really high, we need there to be a bunch of intermediate warning shots where there are problems that are kind of like really bad AI causing untold amounts of harm, but they only caused a few million dollars worth of harm, so we can sue about it and actually have these cases come up. Can you paint a picture of what these kinds of cases would look like and how likely do you think they are?

**Gabriel Weil:**
Sure. Before I do that, I just want to offer some clarifications there on the framing of your question. I don't think we necessarily need a lot of them. We need there to be a high probability that you get one for a particular system before we get the catastrophe, before we get the uninsurable catastrophe. But you don't need thousands of them.

You also don't actually need them. What you need more, if you have the liability rule in place, let's say you've done this via legislation as opposed to common-law accumulation of cases, then what you really need is the expectation that these cases are likely to happen. You don't actually need them to happen. Because what you want... ideally, the companies are expected to be liable, therefore they're trying so hard to avoid these punitive damages judgments, and so you never get them. You might worry about some Goodharting problem there where they iron out all the practically compensable cases without actually solving the catastrophic risk. That is a failure mode I'm worried about, but I think this could work without [companies] ever actually forking over any money, if you just have that expectation. But now-

**Daniel Filan:**
Although it seems good if they're like... Well firstly, I guess right now we're in this position where we're wondering how many of them to expect. It also seems good if there are going to be 10 such cases, because there's some uncertainty about whether people get around to suing and maybe you'd want to average out some variance in what juries are going to do to make it a little bit more predictable. It seems like maybe you don't need a thousand, but it seems like 10 would be much better than 1.

**Gabriel Weil:**
So at the risk of seeming callous about the real people that would be harmed in these scenarios, yes. I think from the perspective of catastrophic risk mitigation, more of these is better and that you would want a few. I'm just saying in principle, you don't need very many, and if you really take my expectations argument seriously, you actually don't need any, you just need the expectation of some, or the expectation of a high probability of some.

Now to your question about what these look like. So the example I use in the paper is: you task an AI system with running a clinical trial for a risky new drug. It has trouble recruiting participants honestly. And so instead of reporting that to the human overseers of the study, it resorts to some combination of deception and coercion to get people to participate. They suffer some nasty health effects that are the reasons it was hard to recruit people in the first place, and they sue.

So it seems like here we have a misaligned system. It was not doing what its deployers or programmers wanted it to do. It wanted to honestly recruit people, but it learned the goal of "successfully run this study". It didn't learn the deontological constraints on that. So it seems like we have a misaligned system, which for whatever reason was willing to display its misalignment in this non-catastrophic way. So maybe a few hundred people suffered health effects, but this isn't the system trying to take over the world and now the system's probably going to be shut down, retrained, whatever, now that we know it has this failure mode.

But probably the people who deployed it ex ante couldn't have been confident that it would fail in this non-catastrophic way. Presumably they thought it was aligned or they wouldn't have deployed it, but they probably couldn't have been that confident. They couldn't have been confident to more than one in a million that it wouldn't fail in a more catastrophic way. And so that's the kind of case I'm thinking about.

**Daniel Filan:**
Okay, so it seems like the general pattern is: AI does something sketchy, it lies to people, or it steals some stuff, and it does it pretty well, but eventually we catch it. And because we catch it, then someone can sue, because we've noticed these harms. I wonder... it seems like this applies to the kinds of AIs that are nasty enough that they do really bad stuff, but also not quite good enough to just totally get away with it without a trace.

**Gabriel Weil:**
Not good enough to get away with it, or they're myopic in various ways. So maybe the system doesn't want to take over the world: all it wants to do is, it really wants the results of this clinical trial, because that's all it cares about. It's willing to risk getting caught and actually maybe it doesn't mind at all because by the time it's caught, it's achieved its goal. And if the people who deployed it could show they were really confident that it was myopic in this way or had narrow goals in this way, then maybe they didn't risk anything that bad. But I'm skeptical in that generic case that they can show that.

Another scenario you might think about is a failed takeover attempt. So you have a system that's scamming people on the internet to build up resources, doing various other things. Maybe it even takes over the server it's on, but at some point we're able to shut it down, but it harms some people along the way. I think that's another sort of near-miss case. There's different ways you can imagine this going where it either has really ambitious goals but isn't capable enough to achieve them, or maybe it is potentially capable enough and we just got lucky. There's different ways to think about this. Because when you think about [Joe Carlsmith](https://joecarlsmith.com/)'s [work](https://arxiv.org/abs/2311.08379)... these systems are facing trade-offs, they worry that their goals are going to be gradient-descented away, and so there's a trade-off between, do you help with alignment research now [and] risk your goals being changed, or do you act now even though there's some possibility that you might fail?

So there it could be that even this particular system, if you had perfect knowledge about the system, there would've been some non-trivial risk that this specific system would have caused an existential catastrophe. We just got really lucky, or we got moderately lucky, however much luck you needed in that situation. I mean, theoretically it could be a 50% chance: then you're already in uninsurable risk territory and the actual damage award's going to be too big. But certainly there's going to be cases where it's 1 in 100, 1 in 1000, 1 in a million, where a reasonable person would've thought that that was the risk.

**Daniel Filan:**
Okay, sure. So I guess my underlying concern is that this kind of scheme might under-deter safety for really, really capable AI systems, perhaps because I'm imagining a binary of either it's fine or it does really nasty stuff and gets away with it. But I guess the thing you're suggesting is even for those systems, there's a good chance of failed takeover attempts or maybe it's just myopic. And like you said, if we only need a couple of those, even just in expectation, maybe that makes it fine and aligns the incentives?

**Gabriel Weil:**
Yeah. So I want to be upfront about this: I think there is a worry that we don't get enough of the expectation, the right kind of warning shots or near misses. And if there are certain classes of harms or scenarios for which there aren't near misses, and therefore this doesn't give the labs enough incentive to protect against those, I think that's a real concern. I don't know what the shape of the harm curve looks like, how probable different kinds of harms are and whether there are qualitatively different failure modes for a system, some of which aren't really correlated with near misses. It seems like there should be, if there's lots of different variables you can tweak about the system. Maybe that particular system isn't going to have near misses, but a very near system would.

And so still the labs are going to have incentives to guard against that. But I think yes, that is a real uncertainty about the world: that if you think we can be confident ex-ante that certain types of warning shots or near misses are unlikely, that you're going to want other policy tools to deal with those kinds of situations. I don't want to hide the ball on that.

## Feasibility of big changes to liability law <a name="feasibility"></a>

**Daniel Filan:**
Gotcha. Fair enough. So I next want to talk about this proposal as law. The first question I want to ask is: the proposal in this paper is some amount of change to how liability law currently works. But I don't know that much about liability law, so I don't have a good feel for how big a change this is. Can you tell me?

**Gabriel Weil:**
There's a few different, as I've been saying, levers that I want to pull here. Some of these are pretty small asks, and then at least one of them is really big. So we haven't talked as much about the negligence versus strict liability calculation. Right now there's three kinds of liability that are typically going to be available. There's negligence liability, there's products liability (which is called strict liability, but in some details has some very negligence-like features), and then there's abnormally dangerous activities strict liability, which I would call a more genuinely strict form of liability. Negligence liability is clearly available, but I think going to be hard to establish.

**Daniel Filan:**
Wait, before we go into this, what is negligence versus strict liability? What's the difference?

**Gabriel Weil:**
When you're suing someone for negligence, you're suing them alleging that they breached a duty of reasonable care. So, they failed to take some reasonable precaution that would've prevented your injury. And so, the general principle of negligence law is: we all have a general duty to exercise reasonable care and not to harm other people. And when we don't do that, when we fail to exercise that reasonable care, that can give rise to liability, when that causes harm. So that's clearly available, but I think going to be hard to prove in this context, because you would have to point to some specific precautionary measure that a lab could have taken that would've prevented your injury. When we don't know how to build safe systems, it seems like it's going to be really hard to say, "Oh, if you'd done this, the system would've been safe," right?

It's not impossible: it could be that there's some standard precautionary measure that (say) Anthropic and DeepMind are doing, but Meta isn't, and then their system harms someone. I'm not saying there's never going to be negligence liability, but I'm saying even if you had a negligence liability for all harms where there's a breach of duty or there's a provable breach of duty, that's unlikely to buy us as much risk mitigation as we'd like.

In particular, that's because the scope of the negligence inquiry is pretty narrow. So, say you're driving your car and you hit a pedestrian, we don't ask, "Should you have been driving at all?" (say you're a licensed driver), "was the value of your trip really high enough that it was worth the risk to pedestrians that you might hit them?" Or say you're driving an SUV, you weren't hauling anything, it was just you by yourself, you could have been driving a compact sedan. We don't say, "Did you really need to be driving that heavy a vehicle that increased the risk that you would kill a pedestrian?" We just ask things like, "Were you speeding? Were you drunk? Were you texting while driving?" Those are the kind of things that are in the scope of the negligence inquiry. And so in the AI context, I think, you can't say, "Oh, you just shouldn't have built this system. You shouldn't have deployed a system of this general nature. You should have been building STEM AI instead of large language models." Those aren't the kind of things that are typically going to be part of the negligence inquiry. So that's why I think negligence is not super promising.

Then there's products liability, which is clearly available if certain criteria are met. So first of all, it has to be sold by a commercial seller. If someone's just deploying an AI system for their own purposes, that they made, products liability isn't going to apply. There's also this question as to whether it's a product or a service. Ultimately, I don't think these distinctions matter a lot for the kind of risks I'm worried about, because I think the test that's going to end up being applied is going to be very negligence-like.

When you're in the products liability game, there are three kinds of products liability. There's what's called "manufacturing defects". This is like: you ship a product off an assembly line that doesn't conform to the specifications and that makes it unreasonably unsafe. And that is more genuinely strict liability in the sense that no matter how much effort you put into your QC [quality control] process, say your QC process is totally reasonable and it would be unreasonably expensive to spend more to eliminate one in a million products coming off the line unsafe, still you're going to be liable if that one in a million harms someone. But I don't think you're really going to have manufacturing defects in the AI context. That would be like, you ship an instance of the model with the wrong weights or something. I just don't think that's a failure mode that we're really worried about.

And so, we're more likely to be dealing with what are called "design defects". There the test is whether there was some reasonable alternative design that would've prevented the injury. And you can see, through the presence of the word "reasonable" there, that you end up in a similar sort of cost-benefit balancing mode as you are with negligence. Again, if we don't know how to build safe systems, it's hard to show... Yes, you don't have to show that the humans behaved unreasonably, you have to show that the system was unreasonably unsafe. But I think that distinction doesn't end up mattering that much, and in practice it's going to function a lot like that. There's also "warning defects", which I think you could potentially have liability on, but even if you have all the warnings in the world, I don't think that's going to solve the problems you're worried about.

So that leaves the third pathway, which is this "abnormally dangerous activities" idea. There are certain activities that we say, "They're really risky even when you exercise reasonable care, and so, we're going to hold you liable for harms that arise from the inherently dangerous nature of those activities." And there's a meta-doctrine as to what activities qualify as abnormally dangerous that I go through in the paper. I think plausibly under that meta-doctrine, training and deploying certain kinds of AI systems should qualify as abnormally dangerous. I think the courts are unlikely to recognize software development of any kind as abnormally dangerous on the status quo, business as usual. I think it's clearly within their powers to do this, to treat training and deploying AI systems that have unpredictable capabilities, uncontrollable goals, as abnormally dangerous activity. I think it does meet the technical parameters there, but I think it would require an understanding of AI risk that courts have not currently been persuaded of.

But I think this is a move they should make, the courts could make [it] on their own. It would not be a radical departure from existing law. I think it would be consistent with this broad doctrine; it would just be recognizing a new instance in which it applies. So I think that's a relatively modest ask to make of courts. Though, again, I want to be clear, [it's] not the default that's likely. So, that's one step. That solves the "can you get liability at all for compensatory damages?"

Then there's the "punitive damages" piece, which is designed to get at these uninsurable risks. There, I think, there's a much heavier lift. There's a long-standing punitive damages doctrine that requires what's called "malice or recklessness", "reckless disregard for risk of harm". We talked before about how even having provable negligence is going to be difficult in these cases; malice or recklessness is a step higher than that. You can think of it as basically like gross negligence.

**Daniel Filan:**
Sorry, hang on. I don't know what gross negligence is.

**Gabriel Weil:**
Really bad negligence. It was really, really unreasonable what you did. Not just a reasonable person wouldn't have done it, but even a normal unreasonable person wouldn't have done it. It's a lot worse. The cost-benefit calculus is lopsided, right?

**Daniel Filan:**
Yeah. The image I have in my head is of someone saying, "Yeah, I know this could be risky, but I don't care. I'm doing it anyway." Which seems like a pretty high bar.

**Gabriel Weil:**
Yeah, it's a pretty high bar. And so I think courts are unlikely to take the step of reforming punitive damages doctrine in the ways that I would like them to, because this would be such a significant change.

Now, I do want to point out that if you think about the normative rationales for punitive damages, at least the one that I find most compelling and that I think is the central normative rationale, is that compensatory damages would under-deter the underlying tortious conduct. That doesn't require malice or recklessness to be true. It requires something about the features of the situation that suggests compensatory damages are going to be inadequate. It might be uninsurable risk. It might also be [that] most people who will suffer this kind of harm won't know that you caused it, or won't sue because maybe the harm is small relative to the cost of proving it. And so, maybe only one in 10,000 people who suffer will end up suing, and so maybe you should get punitive damages to account for the people that don't sue, but nothing about that requires malice or recklessness, and there is [existing scholarship](http://www.law.harvard.edu/faculty/shavell/pdf/111_Harvard_Law_Rev_869.pdf) that argues for getting rid of this requirement. So, it's not coming new in this AI context.

But again, I think courts are unlikely to do this and it would be a major doctrinal change. It would require both state courts to change their punitive damages doctrine, and it would also require the US Supreme Court in applying, again, the due process clause to say that applying punitive damages in this context without these threshold requirements doesn't violate due process, that companies that deployed these systems were on adequate notice. I think that's not totally insuperable, but I think it's pretty unlikely as a matter of just the natural evolution of courts.

**Daniel Filan:**
I want to ask about this, because... Your paper cites this [work by Polinsky and Shavell](http://www.law.harvard.edu/faculty/shavell/pdf/111_Harvard_Law_Rev_869.pdf), basically saying, "punitive damages should just compensate for the probability of being caught". At least, that's how I understood their abstract. That seems intuitive to me, but also my understanding is that this was published, what, 20, 25, 30 years ago or something? And apparently it still hasn't happened. So, the fact that it hasn't happened makes me feel a little bit nervous about making these sorts of things -

**Gabriel Weil:**
You should be. I think you should not expect the courts are going to follow my advice here. They didn't follow Polinsky and Shavell's advice there, and they're much more prestigious people than I am, they're at Harvard and Stanford. They're doing fancy economics models on this. I think you should not expect this change to happen from courts. I think they should. I think it's within their powers. I think we should try to persuade courts. I think litigants should bring these arguments and force them to confront them and ask courts to do it. I think all that should happen, but I would not count on that happening.

But again, I want to be clear, I really think people should try, both because you never know and one state doing it would get you a lot of value, and because I think you would put it on the table politically, then you would say, "Look, we need legislation to overturn this." And so, I do think at least with regard to the common law issues, with regard to what state tort law says, clearly state legislatures, if they want to, can change that requirement. There's sort of a hierarchy of law in which statutes always trump common law. And so if a state wants to pass a statute saying either in this AI context or more generally that punitive damages don't require malice or recklessness, that's clearly something that state legislation can do.

There's still the constitutional issues with that, although I think if you have a statute putting the labs or the companies on notice, that might accomplish a lot of the due process notice function that the Supreme Court's worried about. And so, it's not clear to me that that would be a constitutional barrier in the context of legislation.

**Daniel Filan:**
Can I ask about this punitive damages change? This case for having punitive damages compensate the probability of a case being brought, is the thing holding that up that legal scholarship broadly is not persuaded by it? Or is it an issue where legal scholars are persuaded by it, but judges aren't? Or is there some other issue going on?

**Gabriel Weil:**
So the short answer is I don't know, but if I were to speculate, I think Polinsky and Shavell's argument is really persuasive if you're thinking about this in a [law and economics](https://en.wikipedia.org/wiki/Law_and_economics) frame, and [if] that's all you think tort law is about, and that's basically -

**Daniel Filan:**
"Law and economics" basically being, thinking of law just in terms of economic efficiency and maximizing social utility and stuff, is that roughly right?

**Gabriel Weil:**
Right. That's the frame that I tend to prefer, but that is not dominant. And so, there are other ways of thinking about what tort law's for, particularly what punitive damages are for. There's an expressive function, expressing society's disapproval for the behavior, that would more map onto this recklessness/malice requirement. And so, if you have someone that's doing something that maybe isn't even negligent at all, or it's a strict liability tort or it's ordinary negligence, the idea that we want to punish you over and above the harm you did doesn't sit right with some people.

Honestly, I don't think courts have really revisited this issue in a long time. Mostly what courts do is just follow precedent, unless they have some good reason to reconsider it. I think AI, arguably, should give them a reason to reconsider it, that we have this pressing social problem that you are well positioned to solve. Maybe the fact that this requirement doesn't really make sense from a law and economics perspective more broadly hasn't been that big of a deal in the past. A lot of the sorts of problems that you'd want punitive damages to deal with, we've dealt with through other policy tools, for reasons we've talked about earlier.

I think there's reason to be skeptical that those policy tools are going to be adequate in this context. We need to lean more heavily on tort law, so it makes it really important that you get punitive damages right from this law and economics perspective and they should reconsider it. Again, I don't think they're super likely to do that, but I think we should try. I'm maybe talking myself into thinking there's a little bit of chance that they would reconsider in this context.

**Daniel Filan:**
I'm going to try and suggest some hopium and you can talk me out of it. I glanced at this Polinsky and Shavell paper, because (not knowing anything about the law), this seemed like an obvious change. I read the first page of the paper; it's got a table of contents and it's got a little disclaimer at the bottom. And I noticed that in the table of contents it's like, "It should be based on the probability of a lawsuit actually being brought, not based on other things; and in particular, it shouldn't be based just on the wealth of the defendant because that's economically inefficient." And I see the thing at the bottom saying, "Yeah, this was sponsored by-" Was it ExxonMobil or something?

My understanding is that, basically, a big oil company paid them to write this paper in the hopes that this would be more friendly to really big business. And I have this impression that people in the world don't like the idea of changing the law to make it better for really big businesses. But this change, it seems like, would make life worse for really big businesses and therefore maybe everyone's going to be a little bit more friendly to it because people don't like the big guy. Does that sound right? Am I being too cynical?

**Gabriel Weil:**
There's a few different ways I want to answer that. First of all, I think that's good and bad. I'm talking to state legislators about different legislation to implement, different elements of this proposal, and a lot of them are afraid of doing anything that runs afoul of the tech lobby, or they want to at least neutralize them. It's okay if they're not really supportive, but in a lot of states, having all the tech companies be against your legislation is pretty bad. So, that's one answer [where it's] not obvious that's a net good. But I do think there's a populist... in certain circles at least there is backlash against big tech, and so if your strategy is not the inside game, if it's making a big public case, then maybe that's helpful. I'm going to leave it to political entrepreneurs to make those judgments.

Then there's the way this fits into the broader AI policy ecosystem. And for that purpose, I think it's actually really valuable that this [proposal] is relatively hostile to the incumbent big players. Not as hostile as some of the [AI Pause](https://pauseai.info/) stuff, but when you compare it to licensing regimes that have anti-competitive effects on some margins... I think there's a strong case that we should have anti-trust exemptions for them cooperating to share alignment research, maybe to coordinate the slow-down on capabilities enhancements. There's reasons to think that, under current law, that would violate anti-collusion principles. I think that there's good reasons for having exemptions to that.

I think those ideas are generally pretty friendly to the incumbent players, and there is an accusation that's sometimes thrown around that AI safety is this psyop or whatever by the big tech companies to avoid competition and stuff like that. And so, I think having some policy proposals in your package that are clearly not in the interest of those big companies is useful, at least rhetorically. And so, I think it does play that role, but I don't think, "oh, it's bad for big tech, therefore automatically it's going to happen". That's definitely not my model.

**Daniel Filan:**
Fair enough. All of this was sort of a tangent. I was originally asking: how big a lift is this in terms of changes to tort law that happen? You mentioned that you have to make this change to strict liability, which is maybe not so big. You mentioned that there's this change to punitive damages, which is kind of big, and I interrupted you there, but I think maybe you were going to say more.

**Gabriel Weil:**
Punitive damages is a pretty big lift. I think we've beaten that horse plenty. Then there's other things. Liability insurance, the court just can't do that. Liability insurance requirements, that would require legislation. I don't think it's a big legislative lift, but it's just not something courts can do. And then there's other things that we haven't talked about. There's this doctrine of what's called "proximate cause" or "scope of liability". Say I cut you off in traffic and you have to slam on your brakes, but we don't collide, you're fine, but it slows you down 30 seconds and then two miles down the road you get sideswiped in the intersection. And you want to sue me and say, "Look, but for your negligence in cutting me off, I wouldn't have suffered that later injury. So, you owe me money."

And I say, "No, it wasn't foreseeable when I cut you off that you would get in a collision two miles down the road. In fact, it's just as likely that I could have prevented a similar collision for you." And the courts are going to side with me there. They're going to say, I'm not going to be liable for that, even though I was negligent and my negligence did cause your injury. This is an independent element of the tort of negligence. And so, in the AI context the question is, what does it mean for the injury to have been foreseeable? In some sense, misalignment causing harm is clearly foreseeable. [Sam Altman](https://en.wikipedia.org/wiki/Sam_Altman) talks about it: if his system does it, he's not going to say "I couldn't see this coming". But the specific mode of a misaligned system harming someone almost certainly won't be foreseeable in specific details. And so, it really depends on what level of generality that question is evaluated [at].

There is this "manner of harm" rule that says that the specific manner of harm doesn't have to be foreseeable as long as the general type of harm is. That helps a little bit, but there's still a lot of wiggle room in how this doctrine is applied. There's not some high-level change to precedent that I can ask for to say, "change this rule so that there will be liability in these cases." It's really just [that] courts need to be willing to apply a relatively high level of generality in their scope of liability or proximate cause assessments for AI harms. So, how big of a lift is that? I think not a huge lift, but also not necessarily going to be consistent across cases, and you just want it to generally be fairly friendly to liability, but it's a pretty mushy doctrine in that sense.

Then there's something we talked about earlier, the way mortality damages are dealt with under current law. There's two kinds of lawsuits you can bring when someone dies. There's what's called a "survival action", which is basically all the torts that the person, the decedent, could have sued for the second before they died. So, say I crashed my car into you and you're in the hospital for six months and then you die. And in those six months you racked up lots of hospital bills, you had lots of pain and suffering, you lost six months of wages, you could have sued me for all that. Your estate can still sue for all those things after you're dead. That wasn't true at common law, but there are these survival statutes that allow the claims that you had at the moment you died to be brought by your estate.

Then there's what's called "wrongful death" claims, which are also creatures of statute, that say that designated survivors - so this is no longer your estate suing, this is specific people with a specific relationship to you, say your kids or your spouse or whatever - can sue for harms they suffered because you died. Maybe your kid's suing because they were going to benefit financially from you, they were going to get care-taking services, whatever. In neither of those lawsuits is the fact that it kind of sucks for you that you're dead... that's not something that can be sued for, in almost every state. And so if you think about a quick and painless human extinction where there's no survivors left to be suffering from the fact that their relatives are dead, if you take this to its logical conclusion, the damages for that are zero. No lost wages because you died quickly. No pain and suffering, no hospital bills, no one's around, not only not around to sue, but there's no claim because they haven't suffered from your death, because they're dead too.

Now, I don't think courts are likely to take that so literally. If they buy everything else in my paper and are like, "Okay, we're trying to do damages for how bad human extinction is," I don't think they're going to actually take that to its logical conclusion and say the damages are zero. But I think there's a reason to be worried that in general, if we think most of the harm from AI misalignment or misuse is going to come in the form of mortality, that those harms are going to tend to be undervalued. So, that would require a statutory tweak in individual states to say that wrongful death damages should include the value of the person's life to them.

We have ways of estimating that. As you mentioned earlier, regulatory agencies use a value of a statistical life on the order of $10 million (for US lives). I think that would be fine in the tort context, but that would require a statutory change. I think not a huge lift, but it would require a statute, I think; because wrongful death claims are statutory to begin with, I think it's very unlikely the courts would try to change that on their own.

**Daniel Filan:**
I don't quite understand. It seems like the case that we're really thinking about is: an AI causes an intermediate amount of harm, and we want to assess these punitive damages for how bad it would be if some sort of really bad catastrophe happened. It strikes me as a priori possible that, that kind of calculation could take into account the value of the life-years not lived, but that could be different from actually suing for loss of life.

**Gabriel Weil:**
Well, if your theory of punitive damages is that you can't sue for these compensatory damages if they actually arise because they're practically non-compensable, then presumably the punitive damages should be pulling forward those hypothetical compensatory damages. And again, I'm not so worried that... but if you just are hyper-logical about this and you apply existing compensatory damages doctrine in that scenario, the number you get is zero. Now, again, I think if courts are persuaded by all my other arguments, they'd be really dumb to go there and to say, "Well, if it causes lots of pain along the way, you can sue for that. But the actual human extinction is..." That does seem crazy. I'm just pointing out that that is the logical entailment of the existing structure.

Now, you could say we're going to tweak... Instead of changing the statute, you could say, "Well, I'm going to have a slightly different conception of what these punitive damages are doing, and they're not quite just pulling forward the compensatory damages." I think that you could do, courts could do on their own. I just want to point out, I don't think this is at all the biggest obstacle to making my framework work, but it just seems worth being transparent about this quirk of the way mortality damages work that, in theory at least, could cause a problem here. And if we can pass legislation fixing that, it would make it a lot simpler and more straightforward.

**Daniel Filan:**
Fair enough. So, basically we've got this bundle of changes that you might hope that courts or legislators make. This bundle is some amount of a big ask. How often do those kinds of changes actually happen?

**Gabriel Weil:**
If by "those kinds of changes" you mean reforms to make liability easier, the answer is, legislatively, they almost never happen. Tort reform statutes, with the exception of some of those statutes I was talking about like wrongful death and survival action statutes, are almost always to limit liability. So, when people talk about tort reform, they tend to make it like, "Oh, liability insurance is too expensive, it's making healthcare too expensive, we need tort reform." What they typically mean is making it harder to sue. If that's your reference class that you're drawing your base rate from, it doesn't look so attractive. Now, maybe you think AI is different enough that we shouldn't think of that as the right reference class. I think that's a plausible move to make. But if that's what you're thinking about, then you shouldn't be too optimistic.

Courts, I think, are more inclined to make changes. They're more symmetric in whether they make changes that are more pro-plaintiff or pro-defendant. There's certainly been changes like market share liability, recognizing various forms of strict liability that have been plaintiff-friendly. And so, I think, as I said, I'm mildly optimistic about that making the strict liability/abnormally dangerous activities change. Again, I think that the punitive damages thing is too big of a doctrinal move, that I think courts are unlikely to make. And so, I think we probably are going to need to rely on legislation there.

The other thing we're saying in this context is: if you think about this as a tort reform problem, then maybe you should think it's unlikely. If you think that you're going to have a lot of energy at some point, a lot of political will, to do something about AI law and policy, and those things include some things that would be more draconian, like just banning or creating moratoria on training models above a certain level. Well, saying you have to pay for the harm you cause is a less extreme step than a ban or than a pause. And so, once you think those things are on the table, I think you should be more optimistic about ideas like my liability framework. And so, maybe you don't think this is likely to happen tomorrow, but if you think there's going to be a future moment where there's political will, I want this idea to be fleshed out and ready to go so that states or the federal government can pass it.

## Interactions with other areas of law <a name="interactions"></a>

**Daniel Filan:**
Fair enough. Another question I have about this is: nominally this is about catastrophic risks from AI, but the "from AI" part doesn't... I mean, you talk about some AI-specific things, but this seems like a fairly general type of proposal. Whenever there's some risk of some uninsurable harm happening, we could have a pretty similar scheme. I'm wondering: what other effects do you think these kinds of changes could have? And also, have people talked about these kinds of changes before?

**Gabriel Weil:**
I'm not aware of anyone proposing using tort law to deal with uninsurable risks before. I think typically, the way we handle uninsurable risk is through some kind of prescriptive regulation, and those regulations often preempt tort liability. So if you think of a nuclear power plant, there is some liability for a nuclear power plant, but the liability isn't really... The goal of it isn't really to change the incentives.

There's various government subsidies to make the insurance affordable. But we mostly rely on, especially in the US, regulating these things to death. It's impossible to build a nuclear power plant in the US. It's easier in France, but even there, they're relying on prescriptive regulations. And I think that's true broadly. If you think about biolabs that are doing gain-of-function research, I think it's hard to bring a tort lawsuit for that. We mostly rely on these BSL certifications and stuff, biosafety-level certifications, prescriptive regulations.

I think, generally, the thought has been [that] it's hard for the tort system to handle this, [so] we should lean on other policy tools. I think it is harder for the tort system to handle this. But I think, in the AI context, it's even harder for other policy tools to handle it, or at least to handle it sufficiently.

I'm not saying it should be our exclusive policy tool. But I think there are real limits to what you can do with prescriptive regulation in this context, and so I want to lean more heavily on the tort system than you otherwise would.

I think if you made these doctrinal changes, it would... So the strict liability change would only really apply to AI. I think the punitive damages change, in principle, would tend to be more broad. It would be weird to change it just for AI. But I think the implications of that might be pretty minor, since a lot of the areas where there are these catastrophic risks, the tort law is going to be preempted anyway.

**Daniel Filan:**
Sure. One imagination I have is: during the Cold War, my understanding is that there were a bunch of near-misses where we almost set off a bunch of nuclear weapons, but we ended up not quite doing it. Maybe the US Air Force accidentally drops a nuclear bomb on the US. It doesn't explode, but five of the six safeguards are off or something.

In my understanding, there's a thing you can bring called a [Section 1983](https://www.law.cornell.edu/uscode/text/42/1983) lawsuit, where if a government official violates my constitutional rights, I can sue them for the damages I faced. And one thing I could imagine is: suppose that the military accidentally drops a nuclear bomb, it doesn't detonate, but five of the six safeguards are off, they drop it on my field, it damages my crops a little bit or it's kind of nasty.

I can imagine a world in which I bring a 1983 lawsuit to the government, and not only do I try and sue them for the minor damages to my actual property, but I also try and sue them for, "Hey, you nearly set off a nuclear bomb. That would've been super bad." Does that strike you as a way that this kind of change could be implemented?

**Gabriel Weil:**
Maybe, but there's a lot of complications in that context. Section 1983, there's lots of rules about when it applies, when this waiver of sovereign immunity works. I think that lawsuit's going to be tough.

It doesn't necessarily make sense to me normatively in that context that you would... The government's not a profit-maximizing actor in any sense, so is liability the right tool to...? The government paying means the public's paying, right? Does that change the incentives of the military in the right way? Not obvious to me that it does.

So you can think of tort law generally as serving two functions, right? There's a compensation function and a deterrence function. Right? In the context of suing the government, I tend to think the compensation function's a lot more important, whereas in the private context, I tend to think the deterrence function is more important and the compensation is a happy byproduct of that. Punitive damages are really about deterrence, they're not about compensation.

There's even a proposal I have in the paper that maybe not all the punitive damage should even go to the plaintiff. And so do I really think the government's going to be that much more careful or the military is going to be that much more careful with nuclear weapons if there's this liability? Maybe, but it's not obvious to me.

**Daniel Filan:**
Fair enough. I guess, like you mentioned, you could also imagine having a similar sort of scheme for lab biosafety accidents. Presumably, some of those are run by private companies. Maybe something there could happen.

**Gabriel Weil:**
Yeah, I think to the extent that's not preempted by the regulations, I think that would be a benign effectiveness. Maybe it would be really tough to insure a lab that's doing gain-of-function research, and maybe that would be okay. You make it a lot more expensive, but then you'd have to say: well, if the social value of this is large enough, fine. You can get a big enough grant or a big enough expected profit from doing this research, then okay. But if you can't, then that suggests that this isn't a socially valuable activity at the level of safety that you're able to achieve, and so you just shouldn't be doing it.

**Daniel Filan:**
Sure. Another question I have, more about the punitive damages change: is there stuff like this in criminal law, where there are additional penalties for things that the government might not have caught you doing?

**Gabriel Weil:**
So if they didn't catch you, it's hard to know how we know that you did them.

**Daniel Filan:**
I mean punishing a crime more severely because we thought we might not have caught you.

**Gabriel Weil:**
Certainly, your past record, even if it's not a conviction, is taken into account in the sentencing context. But also, I think an example that might map onto this that might be getting at the same sort of idea is: attempted murder is different from assault, right?

So you get a longer prison sentence if you attack someone and you're trying to kill them, even if you don't [kill them], than you do if you just beat someone up with there being no indication that you were trying to kill them. And so I think that's a similar idea going on in that context.

**Daniel Filan:**
Interesting. Actually, just picking up on a thing you said earlier: in terms of difficulties of applying this kind of liability scheme to state actors, sometimes a thing people talk about is this possibility that AI labs will get nationalized or somehow there's going to be pretty tight intermingling between the government and AI development. Would that pose difficulties for this sort of liability scheme?

**Gabriel Weil:**
So I think in a world where the government is taking over AI companies, I think they're unlikely... There's something called sovereign immunity, so you can only sue the government when they waive that, when they allow you to sue them. I don't think it's super likely as a predictive matter that the government's going to want to expose itself to a lot of liability and punitive damages in that scenario. So that's one question: whether this would be likely to happen in that world.

And then there's another question of: is liability a useful tool in that world? I don't think the government responds to financial incentives in the same way that private parties do. And so if we're in that world, where they're nationalizing it both maybe because they're worried about risks but also because they're worried about an AI arms race between the US and China or whatever, is the risk of getting sued really going to change their calculations that much? It's not obvious to me that that has the same incentive alignment effects that it does in the private context.

And so I think, in some ways, you have lower risk in that world, but in other ways, that's a more dangerous world. It's not obvious to me on balance whether I'd rather live in that world. You're moving all the key decisions to the political process. And part of the advantage of liability is: yes, you need the political process to get the high-level decisions in place, but then you're shifting the onus to these private actors that have, in theory at least, more aligned incentives, as opposed to trusting elections and regulatory processing or military decision-making to make the right decisions.

## How Gabriel encountered the AI x-risk field <a name="how-encountered-x-risk"></a>

**Daniel Filan:**
Fair enough. I'd like to change topic a bit now. Mostly when I see people doing work on AI alignment, usually they're either an AI researcher who's come across alignment concerns in the course of being in the AI sphere, or one of these dyed-in-the-wool young EA professionals who got into AI that way. My understanding is that you have a decent background in environmental law. How did you come across AI stuff?

**Gabriel Weil:**
I've been loosely affiliated with the rationalist and EA communities for a long time, and so I've been aware of the AI risk problem for over a decade. I'd never really, until pretty recently, considered working on it professionally. It wasn't obvious what role I would play. I wasn't thinking in terms of what policy tools or legal tools would be relevant.

And so I [was] glad some technical researchers are working on this, but I thought of it as a technical problem. I guess in the last couple of years, I started to reconsider that. And then last summer, I did this [fellowship](https://pibbss.ai/fellowship/) called [PIBBSS](https://pibbss.ai/), Principles of Intelligent Behavior in Biological and Social Systems, that brings together non-computer... I mean, there were a couple of computer science, ML types there, but it was mostly people in social sciences, economics, philosophy.

I was the only lawyer that was part of this program. But it was about 15-20 people that each were assigned a mentor. So I was working with [Alan Chan](https://www.achan.ca/) from [Mila](https://mila.quebec/en/). He was really helpful on helping getting me up to speed on the technical side of some of these questions. And so I did this fellowship. We all spent the second half of the summer in Prague together, working out of a co-working space there, and got to learn from other people who were doing a project in this area. So that's the causal story of how I got involved in this.

Intellectually, it's not as big of a departure from my work on environmental law and policy as it might seem. We talked earlier about how carbon tax is sort of like AI liability. I tend to approach my scholarship with a law and economics frame. It's in a different context, but I'm thinking through a lot of issues [where] I'm comfortable with the principles involved from other contexts. I also teach tort law, and so it was natural to think about how that domain could be helpful to AI risk.

**Daniel Filan:**
In some ways, I feel like there's a strong throughline of... It seems like some of your work is on changes to the liability system, and it seems of a kind with that kind of work.

**Gabriel Weil:**
Yeah. So I had [a recent paper](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4466197) I think you read on the Hand Formula, which is this test for breach of duty in negligence cases and ways in which it might fail. So that was a more general critique of the way tort law works.

I think this paper implicitly has a lot of broad critiques; I think you suggested this. A lot of the things that I think should be changed in this context really are problems with tort doctrine generally, that the AI risk context is really just pointing up and exposing. And so, in principle, you could have written this paper without ever mentioning AI. I think it's worth being explicit about why I care about it.

## AI x-risk and the legal field <a name="ai-x-risk-and-legal-field"></a>

**Daniel Filan:**
Suppose we really do want to make these kinds of changes. If the AI x-risk community wants to push for this kind of thing in the legal profession or among legislators, what do you think that looks like?

**Gabriel Weil:**
I think there's a few different channels of influence. The one that I have the most control over is just convincing other legal scholars that this is a good idea, and then you create a consensus around that, other people write articles saying it's a good idea, and then there's a bunch of articles for litigants to cite to judges when they try to persuade them to adopt this. So that's one channel of influence, through the academic pathway.

Another is just lawyers out there bring these cases and try to convince judges to adopt these different doctrinal changes. You can do it with strict liability even before you have a case where there's catastrophic risk implicated. And then as soon as there's any case where there's a plausible argument for uninsurable risk, try to raise the punitive damages and get courts to consider that.

And then on a parallel track, I think we should be talking about legislation both for strict liability and for punitive damages and potentially for other things like liability insurance requirements and changing the way mortality damages work. Those are all things that could be done by state legislation. And so I think people who are interested in doing policy advocacy - that's definitely an avenue that I'm involved with, talking to some state legislators, that I'd like to see more work on.

In some states, this could be done via ballot initiative. So certainly, in California, it's pretty easy to get an initiative on the ballot. I think strict liability is a pretty straightforward yes or no question that you could have a vote on. I think it would be a little tougher to do it for punitive damages, but I wouldn't put that off the table. Liability insurance I think would be hard, but California seems to let lots of crazy stuff onto ballot initiatives, so maybe.

**Daniel Filan:**
Yeah. I don't know if you're familiar with [the state constitution of California](https://en.wikipedia.org/wiki/Constitution_of_California), but every two years, it gets added with some random stuff that some ballot measure passed. I think the California state constitution includes text from [a ballot measure](https://ballotpedia.org/California_Proposition_52,_Continued_Hospital_Fee_Revenue_Dedicated_to_Medi-Cal_Unless_Voters_Approve_Changes_(2016)) that's basically about a scheme to do some tricksy accounting stuff to maximize the amount of Medicaid money we get from the federal government. A lot of stuff happens in California.

**Gabriel Weil:**
I don't think California has adopted the optimal initiative process. But given that it exists, I think it should be used for this good purpose. So I'd love to see someone [doing this] and I'd be happy to advise any project that wanted to pursue an initiative like that in California.

**Daniel Filan:**
One thing I wonder: you mentioned that this is kind of a "law and economics" framing on the problem of AI risk. And my impression is that law and economics has some kind of market share among the legal profession but not overwhelmingly so, such that every idea that the law and economics people think is good gets implemented.

I wonder if it makes sense to make a different kind of case for these kinds of reforms that looks less law-and-economicsy and more something else. But law and economics is the part of the legal profession that I know the most about, so I don't actually know any examples of other ways of thinking.

**Gabriel Weil:**
Yeah. There's a political economy argument, that I think is maybe more popular on the left, that would be... Law and economics tends to be right-coded. I don't think that's inherent in the paradigm, but because of the way political coalitions are... The people who are funding a lot of the law and economics research tend to have more right-wing political goals.

I don't think my proposal here is particularly right-wing, but I think a lot of the skepticism of law and economics tends to be from more progressive or liberal folks. So I think you would want framings that appeal more to them. I think this proposal is more susceptible to critiques from the right since I'm just arguing for more liability to make life less friendly for these big companies.

But there's also a lot of tech backlash on the right, so it's not obvious to me how that plays into the politics of this. So I guess it depends whether you're asking about how to convince fellow academics, and should there be people writing other traditions, providing a different case for this. I think there's maybe scope for that. I don't know exactly what that would look like. And then certainly, you're going to want to use different arguments in different contexts when you're trying to persuade a political audience.

## Technical research to help with this proposal <a name="technical"></a>

**Daniel Filan:**
Fair enough. I guess my next question is: rather than on the advocacy side, a bunch of people listening to this are technical researchers. What kinds of technical research would be good complements to this kind of proposal, would make it work better?

**Gabriel Weil:**
In particular, you want research to be able to implement various aspects of the punitive damages formula, and also to be able to implement the liability insurance requirements. So I could see a role for model evaluations both in deciding what the coverage requirement is: a regulator could use a set of dangerous capabilities evaluations to decide how much insurance you need to take out before you can deploy the model, or if it's pre-training insurance, to train the next model. And similarly, insurance companies could use a slightly different set of evaluations in their underwriting process.

And then in the liability or punitive damages context, we need to estimate these different parameters in my liability or damages formula. So one thing we want to know is: how much risk should the trainer or deployer of this model have known that they were undertaking when they made the key decision?

And I could see a role for technical research trying to get a handle on... reduce our uncertainty about that question. There's also the question of: this particular harm that was caused, how elastic is that with the uninsurable risk? So for every unit of risk mitigation you get, say you spend a million dollars to reduce the risk of this particular harm by 20%, how much does that reduce the uninsurable risk? That's another key parameter. And how much does that do that relative to a generic harm that this system might have caused?

So work on trying to figure out how to estimate those parameters would be really useful. I have [a blog post up on the EA Forum](https://forum.effectivealtruism.org/posts/yWKaBdBygecE42hFZ/how-technical-ai-safety-researchers-can-help-implement) that lays out the formula and the ways in which technical researchers can help solve these problems that you can point people to.

**Daniel Filan:**
A lot of those seem like general ways which any AI governance effort researchers could help with. It strikes me that trying to get a sense of, for any given failure, how much would mitigating that failure have mitigated against really bad catastrophic AI risks... I think perhaps that's a unique thing about your proposal that researchers might not have already been thinking about.

## Decisions this proposal could influence <a name="decisions"></a>

**Daniel Filan:**
Before we wrap up, is there anything that I didn't ask but you wish that I did?

**Gabriel Weil:**
Maybe "what decisions are you trying to influence with this?"

**Daniel Filan:**
What decisions are you trying to influence with it?

**Gabriel Weil:**
There's a few different ways you can think about how this would influence the behavior of AI labs. So one scenario you might think about is, say OpenAI trains GPT-6 and they run it through a [METR](https://metr.org/) evaluation and it shows some dangerous capabilities, and maybe they've come up with some alignment evaluations and [they show] maybe this system is misaligned, you shouldn't deploy it.

And the question is: what do we do now? And there's a trade-off or a different set of options where there's cheap, dumb, easy things you could do that wouldn't really solve the problem. You could just run RLHF to iron out the specific failure mode that you noticed. And almost certainly, that wouldn't solve the underlying misalignment, but it'd be really easy to do.

And there's some moderately costly thing where you roll it back a little bit and then retrain it. And then there's some somewhat more expensive thing where you do some adversarial training, maybe you roll it back further.

And then there's a really expensive thing where you say "none of the tools we have right now are good enough. We need to wait until we have better interpretability tools or we make some fundamental breakthroughs in alignment theory (or whatever it is)". And you've got different actors within the labs; some are more cautious, some are less, and there's a debate about which one of these options we should take.

And I want to empower the voices for more caution, saying... maybe they're motivated primarily by altruistic impulses, but I want to arm them with arguments that say: even if all you care about is the bottom line, we should do the thing that produces the best expected social returns, because that's going to actually be what favors the bottom line.

I think you see that a lot of these leading labs were founded with these high ideals. OpenAI was founded by people really worried about AI risk, and now there's a lot of criticism of them that they're moving too fast, that they're taking too many risks.

Sam Altman was saying, ["Well, we need to move fast so we don't have a compute overhang,"](https://openai.com/blog/planning-for-agi-and-beyond) but then [wants to get $7 trillion to invest in improved compute](https://www.wsj.com/tech/ai/sam-altman-seeks-trillions-of-dollars-to-reshape-business-of-chips-and-ai-89ab3db0), so there seems to be something a little confusing there. And obviously, there was [the whole kerfuffle with the board over him being fired](https://en.wikipedia.org/wiki/Removal_of_Sam_Altman_from_OpenAI).

And so we've seen that these internal governance mechanisms are not things we can totally rely on. I think, similarly, even for a lab like Anthropic, which was founded by people who [defected](https://www.ft.com/content/8de92f3a-228e-4bb8-961f-96f2dce70ebb) from the alignment team at OpenAI, and [there were statements](https://twitter.com/bshlgrs/status/1764713562315067490) like, "Well, we're not going to try to push forward the frontier on capabilities. We just want to have near-frontier models so we can do alignment research on them." And then Claude 3 comes out and there's [these claims](https://www.anthropic.com/news/claude-3-family) that it's better on all these metrics than any model that's been out there before. And so it seems like there's very powerful financial incentives and other incentives for these companies to build commercializable products and to push forward on capabilities. And I think even people that are very well-motivated are having trouble resisting those forces.

And so I think having a liability regime that puts a thumb on the other side of the scale, that makes it in their narrow interest to do the thing that they say they want to do, that is in the interest of society at large, would be really valuable. And so however you want to think about it, whether you think about this in terms of competitiveness or alignment taxes, if we can tax misalignment effectively through this liability, I think that could be really valuable.

And you don't have to think of it necessarily as being hostile at least to everyone in these AI labs. I think some people at least within these labs would welcome the fact that it's empowering them to stand up for safety and [for safety] to not just seem like some altruistic concern. It's actually part of the interest of the company.

## Following Gabriel's research <a name="following-gabriels-research"></a>

**Daniel Filan:**
Gotcha. So I guess to wrap up, if people are interested in following your research on this or on other topics, how should they do so?

**Gabriel Weil:**
You can find [all my papers on SSRN](https://papers.ssrn.com/sol3/cf_dev/AbsByAuth.cfm?per_id=1648032). Once I put them publicly, we can include a link to that. There's only one on AI so far, but I expect to do more work in the future. They can follow me on Twitter [@gabriel_weil](https://twitter.com/gabriel_weil). And then I've got a couple of posts on the EA Forum, one just providing a [high-level summary of the paper](https://forum.effectivealtruism.org/posts/epKBmiyLpZWWFEYDb/tort-law-can-play-an-important-role-in-mitigating-ai-risk), and then another that I mentioned, explaining [how technical AI safety researchers can help implement this framework](https://forum.effectivealtruism.org/posts/yWKaBdBygecE42hFZ/how-technical-ai-safety-researchers-can-help-implement). And so I would direct people to those. Dylan Matthews also did [a write-up of the paper in Vox](https://www.vox.com/future-perfect/2024/2/7/24062374/ai-openai-anthropic-deepmind-legal-liability-gabriel-weil) that we can link to. I think that's about it.

**Daniel Filan:**
Gotcha. Well, thank you for coming on the show.

**Gabriel Weil:**
Thanks. This was great.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future) and [Lightspeed Grants](https://lightspeedgrants.org/), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
