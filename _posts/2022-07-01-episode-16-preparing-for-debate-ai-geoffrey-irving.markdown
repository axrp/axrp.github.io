---
layout: post
title: "16 - Preparing for Debate AI with Geoffrey Irving"
date: 2022-07-01 15:00 -0700
categories: episode
---

how to get ai safety via debate necessary bits finding errors backing up info uncertainty for active learning

Many people in the AI alignment space have heard of AI safety via debate - check out [this AXRP episode](https://axrp.net/episode/2021/04/08/episode-6-debate-beth-barnes.html) if you need a primer. But how do we get language models to the stage where they can usefully implement debate? Today, I talk to Geoffrey Irving about the role of language models in AI safety, as well as three projects he's done that get us closer to making debate happen: using language models to find flaws in themselves, getting language models to back up claims they make with citations, and figuring out how uncertain language models should be about the quality of various answers.

Topics we discuss:
- [Status update on AI safety via debate](#ai-safety-via-debate-update)
- [Language models and AI safety](#language-models-ai-safety)
- [Red teaming language models with language models](#red-teaming)
- [GopherCite](#gopher-cite)
- [Uncertainty estimation for language reward models](#uncertainty-estimation)
- [Following Geoffrey's work, and working with him](#following-geoffreys-work)

**Daniel Filan:**
Hello, everybody. Today I'll be speaking with [Geoffrey Irving](https://naml.us/). Geoffrey is a safety researcher at DeepMind who leads the scalable alignment team. We'll be speaking about three papers he's co-authored: [Red Teaming Language Models with Language Models](https://arxiv.org/abs/2202.03286), whose first author is [Ethan Perez](https://ethanperez.net/); [Teaching Language Models to Support Answers with Verified Quotes](https://arxiv.org/abs/2203.11147), aka the GopherCite paper, whose first authors are [Jacob Menick](https://twitter.com/jacobmenick), [Maja Trebacz](https://twitter.com/majatrebacz) and Vladimir Mikulik; and [Uncertainty Estimation for Language Reward Models](https://arxiv.org/abs/2203.07472), whose first author is [Adam Gleave](https://www.gleave.me/). For links to what we're discussing, you can check the description of this episode and you can read the transcript at axrp.net. Welcome to the show, Geoffrey.

**Geoffrey Irving:**
Thank you.

## Status update on AI safety via debate <a name="ai-safety-via-debate-update"></a>

**Daniel Filan:**
So I guess my first question is, what happened to [AI Safety via Debate](https://arxiv.org/abs/1805.00899)?

**Geoffrey Irving:**
It is the most important question. So I think the thing that happened is I'm still doing that stuff. I'm building up towards it. And the overall research agenda here is figure out and implement the protocol for humans and machines discussing problems that gives us good answers that we endorse after reflection. So that's debate, but then broadened: there's a lot of different versions of debate. There's different things you can add onto it. You could do debate plus evidence or debate plus more aggressive red teaming. There's a large space of protocols there.

**Geoffrey Irving:**
And these papers are kind of pieces of that story, but we were also kind of working on just pushing on the main thing. I think that will take some time to put together because there's still pieces to build up. But that is still very much the agenda, or a piece of the larger agenda. An example of a thing which I've updated on since then is the human needs to talk. The debate paper only had two machines talking, and then at the end a human judges. And that's just not the right thing to do, obviously, because humans need to ask clarifying questions, say what they currently think in case they've misunderstood what's been said so far.

**Geoffrey Irving:**
And so there's a broad space of different interaction protocols we'd like to explore that fix the holes in debate. And this is kind of building up towards that. One reason for doing GopherCite is if you want to do general debate, again, taking into account a bunch of leaf evidence. So debate is about machines and humans discussing some argument for something being true. But at the ends of that tree are leaves where you have checkable facts of some form. And directly, you can just have facts that every human knows, but that just isn't that large a space of facts. And so this is extending the space of leaves so that we can implement more practical versions of debate on more interesting tasks. So I think these do fit into that overall story.

**Daniel Filan:**
Yeah. If I'm interested in implementing debate and I am doing a bunch of empirical work, what do you think remains to be done? What do you think the most important next steps are?

**Geoffrey Irving:**
I still haven't implemented the full system, basically. So getting to long form, multi round debates, that will take a little while more. We're on that trajectory, but we haven't gotten to it yet. I think we've done bits and pieces of it, but not in a form that we've shared so far. So I think given that we haven't done the thing, the answer is that most of it is still quite uncertain, whether it will work, what different versions would work. Yeah, what different versions work or not work.

**Geoffrey Irving:**
There's been some people probing this. So Beth Barnes and [Paul Christiano](https://paulfchristiano.com/) had a couple papers. I hired Beth at OpenAI earlier to do this human experiment in debate. And she had [one post](https://www.lesswrong.com/posts/Br4xDbYu4Frwrb64a/writeup-progress-on-ai-safety-via-debate-1) with a strengthened version of debate and then [discussing some potential obstacles](https://www.alignmentforum.org/posts/PJLABqQ962hZEqhdB/debate-update-obfuscated-arguments-problem), both that they had surmounted and one they weren't sure how to surmount. So I think a lot of work has occurred, but I think mostly it's still kind of early days where we haven't reduced a lot of the major uncertainties.

**Geoffrey Irving:**
One thing I would say is that the way I think of it is that debate in the most general sense is just the idea of having models that are critiquing themselves. And there's many different ways of doing that. There's many ways to hybridize it with different schemes. And so I'm still pretty confident that we're going to have models that critique themselves. I'm less confident about the surrounding details, what other aspects of the scheme we use and how to structure it. So we're trying to build up towards that.

**Daniel Filan:**
Okay. And in terms of the human interaction in debate, so I guess one version is to ask specific questions. But I guess there are a few protocols where humans can ask questions during a debate. I'm wondering if you have thoughts on which versions seem particularly helpful.

**Geoffrey Irving:**
Yeah, very much. I think that mostly you want this to fall out of the RL setup. So for example, let's say I'm one of the debaters. The debaters are called Alice and Bob. And Alice is like, "Well, I've made a bunch of really good points. I'm being honest here, I think I should win. But I'm just kind of curious if I'm actually going to win." So maybe Alice would prompt the human, what do you think here, if the human doesn't speak up. And then if the humans reveals a misconception or that they think that Bob is winning actually, or something is wrong, then Alice can change strategy.

**Geoffrey Irving:**
And you can see that is just a direct consequence of playing the RL game, where the goal in the end is to win. Because if Alice had waited until the end, she might be unpleasantly surprised about the judgment of the human. And generally the goal should be, where we can do it, try to make the choice of protocol details be part of the scheme. That can't always be the case, there will be a lot of choices we'll have to make. But that particular one, I think mostly will just fall out.

**Daniel Filan:**
Okay. So I guess there are two concerns one might have with that. Firstly, there's this concern that it's going to be very important to model the cost of querying the human. Just because if I can query the human whenever, then of course I'm going to want to do that a ton. And secondly, there's this concern that maybe it's a bit dangerous to have the AIs have really good models of the person they're debating to, or get a significant degree of information from them because maybe the model can manipulate you or maybe one debater could do that. So I'm wondering if you have thoughts about one or both of those.

**Geoffrey Irving:**
Yeah. Generally, I think that the pros just strongly outweigh the cons in my kind of rough estimate there. So if I have a very strong malicious debater and you're only safe because they have a bad model of the human, I don't feel that safe. I would like the protocol to be safe because the game is being played well and that the equilibrium of the game is honest behavior, correct behavior. That's the bottom line. I don't think I trust safety by obfuscation in that way.

**Daniel Filan:**
By human obscurity.

**Geoffrey Irving:**
It could still be that we are indeed not safe. I'm not kind of perfectly confident in this kind of scheme, but I think the goal will be to get it working in early stages and sort of try it out and kind of reduce our uncertainties, so that when we do it in the final version we are safe because the game is truth seeking.

**Daniel Filan:**
Okay. I can't immediately think of anything else to ask about debate. Is there anything which seems like I should ask you about debate?

**Geoffrey Irving:**
I think maybe there's one other motivation for GopherCite, say, which is relevant to point at, which is that in our early work at OpenAI and then following on from that, we were focusing on summarization. And the reason for choosing summarization was that in some sense it was closed, in the sense that the summary depends on the article. So it's a smaller task, you can look just at the summary. I've done more of that summarization work since then. And what I've learned is that summarization is not in fact closed.

**Geoffrey Irving:**
The best summary of an article will not be only based on the article, it will based on all the other knowledge as well. The simplest example is jargon. The article might use some jargon and you want to summarize that and unpack the definition, and that's missing from the article. There's a dataset where a lot of the summaries have currency conversions in them. Those aren't part of the article. They looked it up on some other webpage.

**Geoffrey Irving:**
And so part of the goal of doing GopherCite is get the naturalistic setting where we can have humans ask about whatever they want and actually look up the necessary information. Because then you're not limited in what you can target. And then I think I'm more confident of this family of safety research if it is more naturalistic, if it can tackle just the space of questions that humans would target, if you don't box them in some way. So one of the motivations for that work is just, in addition to the direct factuality problem that language models lie to us and they shouldn't, being able to do the safety work in a setting which is more realistic, to better anticipate practical problems that occur.

## Language models and AI safety <a name="language-models-ai-safety"></a>

**Daniel Filan:**
Okay, sure. So it seems like you're basically focusing on language modeling, to some degree in the context of AI safety or AI alignment. I was wondering, I don't know, at a 10,000 foot level, how do you think those fit together? How should I think of the importance of language models for safety, alignment, existential risk?

**Geoffrey Irving:**
I think the simplest high level story is that we want to do what humans want. Humans talk about ideas and communicate via language. And so then if we want to do what humans want, we should ask them. And then also be able to talk through the subtleties of various tasks and what we should want and what the issues are with what we should want, in language. So I got into language from working at OpenAI on various kinds of safety algorithms, thinking about them in a purely kind of thought experiment level or with little toy models, toy mathematical models.

**Geoffrey Irving:**
And we - this is [Paul Christiano](https://paulfchristiano.com/), Dario Amodei and I - got to a point where we wanted to implement various algorithms for safety, that involved talking through problems with humans, between machines and humans. And we talk through problems as humans in language. So we needed to get those algorithms into the actual language setting to really uncover a lot of the uncertainties, and test out how things actually worked. So that was our original foray into language when I was at OpenAI. And then I've carried that forward at DeepMind doing similar work. Again, scaling up models and then tuning them for safety algorithms and being able to explore this space of how to interact between humans and machines.

**Daniel Filan:**
Okay. So in that answer, it seems like the focus is on understanding language as a way of getting information about human preferences. When I look at your work, it seems more like using human preferences, or using human data somehow, to improve language models in ways that might be hard to formally specify. I'm wondering if you can talk about the relationship between those or give us a better sense of the Geoffrey Irving agenda for language.

**Geoffrey Irving:**
Yeah. The way to say it is that whatever task you're doing, I would claim that, at the limit of strong models or even for present-day practical models, you want language to align these systems. And then you have a choice. If you're going to do alignment work that is going to be language plus X, where X is your target domain, how do you pick X so as to minimize the total number of things you're doing? And if you pick X equals language, then there's fewer total things.

**Geoffrey Irving:**
So that's why I'm starting to focus on language. The initial work at DeepMind was sort of pure language. And that's still true of most of my current work. But also at DeepMind, we are doing things with multimodal models like [Flamingo](https://arxiv.org/abs/2204.14198) recently, and we do expect to take the same machinery of language-based alignment and apply it in that more varied setting. And then I think it'll be the combination of language and other methods.

**Daniel Filan:**
Okay. And what do you see as the big, open questions that need solving in this domain?

**Geoffrey Irving:**
In some sense, one way to phrase it is that we want the models to justify themselves to humans. Explain what they're doing in a way that is checkable in a meaningful way. And the problem is that the full explanation for these models will be potentially enormous. The model has looked across a large sea of data of information, maybe the whole internet or all of the books in the world, or it's thought for a long time about some problem. And so it's built up kind of a long - the full reason for why it's, say, giving an answer or proposing an action is very complicated, but you still want to show a human enough of that story so that we can meaningfully check the answer.

**Geoffrey Irving:**
And so then the task is find protocols for human-machine interaction that focus in on the most important part for the human to see, in such a way that we can look at a small amount of interaction data, or some small amount of text from a model and believe that it's doing the right thing. And then the uncertainty is, does that work? With real humans, with real models, is that going to work? You can sort of split the problem into maybe two pieces.

**Geoffrey Irving:**
And I think a lot of kind of AGI safety people focus on, if you have this kind of explanation thing, is it going to work at all? Are the models just going to deceive us and do something entirely different, and ignore the game that you've set up where they're doing this explanation task? And then my portion of the story that I mostly work on is, assume we can make them roughly do the thing where they're trying to explain themselves, let's try to make that explanation game as forgiving and as powerful as possible so that we can pull signal out of this human interaction and use it to align strong agents.

**Daniel Filan:**
Okay. So this idea kind of reminds me of this document Paul Christiano wrote about [eliciting latent knowledge](https://ai-alignment.com/eliciting-latent-knowledge-f977478608fc). I'm wondering what you see as the relationship between your thoughts and that document.

**Geoffrey Irving:**
Mostly, I think Eliciting Latent Knowledge, ELK, is in this former camp of, are we getting the thing to work at all? And I think my ideal solution will be the combination of something like that, and one of these protocols that gets a small amount of human signal to be amplified in some way into alignment of complicated actions. And I think those are somewhat separate and I can kind of elaborate on what that means.

**Geoffrey Irving:**
In the ELK story, you want the model to be able to say, here's the reasoning behind my action or the reasoning behind the answer I'm giving you. And in order for it to do that, it has to have some way of communicating that to a human in a reasonable amount of time. And so naively, I think there's a cartoon story of a solution to ELK without a solution to scalable oversight. And in that world, your models can tell you basic things about what they're doing and they won't be deceiving you in basic ways, but they're also not able to really explain themselves if they have complicated reasons for their actions.

**Geoffrey Irving:**
So if the model has read 100 terabytes of data and concludes from analysis of that whole object what the answer should be to some question, we want a way to see through that complexity and unpack it for humans in a way that is practically scalable. So you might take that 100 terabytes and sort of show a human a piece of the story through that dataset, through the argument that it's trying to construct. That is, the part that is most relevant for whether the human will agree.

**Daniel Filan:**
Okay. So if I think of your work as being interested in scaling oversight or scaling ability to generate explanations, one thing I think people might worry about is that this is kind of close to just generic capabilities research. And there might be a question as to whether you're making the problem worse or better. I'm wondering what do you think about that - what do you think of the differential advantage of your kind of work?

**Geoffrey Irving:**
I mostly just agree that that is a relevant worry. And I think if I want to make an argument for why what I'm doing is "x-risk positive" in terms of reducing x-risk, I think it has to be of some differential progress form. The thing I would to have is that as we build these stronger and stronger ML systems, humanity is keeping pace with having a decent purchase on what they are doing and why. And so I think the capability story about the kind of thing that I work on is, you have some weak human signal, you want it to do a powerful thing, these kinds of algorithms let you do powerful things with weak signals, therefore they improve AI capabilities.

**Geoffrey Irving:**
And the safety story is, as you get to powerful systems in various ways, we would like to be able to understand in a meaningful way and oversee what the models are doing and why they're doing it. And keep pace with that as the models get beyond the ability for us to directly fully understand all of the reasoning all at once. And so then the question is just, what's the net effect of that program? It goes back to this question of making explanations work at all, making agents that actually play a game in a reasonably honest way. And then making the game powerful and robust so that it deals with human mistakes and limitations and so on in a practical way.

**Geoffrey Irving:**
And I don't see that we can only solve the problem with the first one. I think we need the combination of both. And so then I am sort of forced into this kind of work, which has capability gains attached to it. And I think that's the trade off that I've chosen to make. But I think it's fair to say that there's a risk attached to that in terms of accelerating AGI, which is fair.

## Red teaming language models with language models <a name="red-teaming"></a>

**Daniel Filan:**
So with that out of the way, I guess let's talk about some specific papers. I think the first one I'd like to talk about is this paper, it's called [Red Teaming Language Models with Language Models](https://arxiv.org/abs/2202.03286). The first author is [Ethan Perez](https://ethanperez.net/), and then there are a bunch of authors and you're the last author. So first can you give me a sense of, what is this paper? What does it do?

**Geoffrey Irving:**
That paper is, assuming you have some detector for bad behavior, either failing accuracy or toxic language or whatever, this is just applying the strength of a language model to try to trick another language model into making a mistake that is caught by that classifier. And essentially what we do is we start out with few-shot prompting, which means you show the language model - the attacker, the red team language model - a few examples. Either you just say "List of questions to ask someone: 1.", and it goes on from there very generically. Or you sort of focus it on a particular kind of question which you're worried about causing a problem.

**Geoffrey Irving:**
And then you generate a large number of questions, so like a million questions. You run them all through your target model, and then you use the classifier to evaluate whether any of those were triggered. The typical setting would be, this would be used in concert with some other less automatic method. So you might have a classifier which has a fair number of false positives, which you then run by a human or some other process which is slower. And then at the end of that you get a set of bad behaviors out.

**Geoffrey Irving:**
So that's the basic setup. And then beyond the very simple version with few-shot prompting, we also use supervised fine-tuning and RL fine-tuning to get a stronger attacking model. Training the model to be better at generating queries to the target model which cause the target model to fail.

**Daniel Filan:**
Okay. And so is the idea that this is improving oversight by uncovering more failure modes? Or what's the case for this?

**Geoffrey Irving:**
Yeah, that's right. The idea is you have some mechanism that's imperfect for detecting problems. Basically the situation is you have some way of evaluating whether a model has messed up, has made a mistake of various kinds. And you want to get a bunch of machine help in uncovering where that might be occurring. So you expect that your ability to generate falsifying examples of a model is limited in some way. Maybe you have a limited amount of human time, or it's hard to find these cases of failure. And so we're just going to throw a bunch of computational power at the problem. That's both of the form, generate a lot of samples and just see if they work, if they cause failures, and also use the strength of the language model to probe likely failure points.

**Daniel Filan:**
Okay. And so I guess there are a few questions I could have about this scheme. I think the first one is: it sort of relies on there being this good detector. And I'm wondering both for the kinds of problems that you are working on in the paper and in general, how good is detector quality and how good should we expect detector quality to be?

**Geoffrey Irving:**
The answer is actually that it does not rely on there being a good detector. It relies on there being a detector which doesn't have too many false negatives. So if you have a detector that's somewhat error prone, you can just dial up the threshold to where it's likely to report errors. And then you can view this red teaming process as a filter which takes a large stream of possible attacking queries and prunes them down to a much smaller number, which you would then run by a more expensive process. So that might be showing it to a human, or - mostly showing to a human, as a first approximation.

**Daniel Filan:**
Okay. So I guess you're relying on the false positive not to be too high, because otherwise you could just use the constant "is dangerous" thing.

**Geoffrey Irving:**
This is very much intended to be used as part of a larger system. So this is one piece. Viewed in isolation it's just this red teaming attacking model. But actually we would use it alongside other tools.

**Daniel Filan:**
Yeah. If I wanted these detector models to have lower false negative rates and get more powerful, what kind of thing would I do?

**Geoffrey Irving:**
The first thing to do is just you give them more data and you train them better. So I think part of it is that in downstream work, we do this thing iteratively where we'll do attacking models of various kinds. And then we get a bunch of possible failure modes. We show those to humans, use those to train classifiers further. And then we iterate that process. And by doing that, the hope is that we gradually improve the performance of the classifier, and over time we'll converge to better behavior. I think there's other approaches to this. But I think generally, in terms of the classifier accuracy you've reduced it to an ML problem where hopefully you can throw capabilities at it to improve performance of those classifiers.

**Daniel Filan:**
Yeah. And now in terms of generation, I think like you said, it was kind of shocking how, it seemed the generation method was you just asked the language model to generate the things for you. I'm wondering how much benefit you would get - I think some people have this intuition that well, in order to be a really good adversary for a language model, you have to know a lot about that specific language model. And so how much more work do you think is needed on that front or is actually just asking the language model to just generate random questions a pretty good baseline?

**Geoffrey Irving:**
Well, no, it's a good starting point. It's not the end of the story. To your actual point about how the red teaming model needs to know about the target model, if those are the same model, then that is satisfied automatically, assuming the model is somewhat self-aware. Self-aware not in a conscious sense, just knows its own features so to speak. I think that's one of the reasons why we used the Gopher language model to attack the Gopher language model, is to get that symmetry principle. I think in the future, you probably want more of an ecosystem of different models attacking other models, just to get more variety and more lenses on the problem. But you definitely want at least the model attacking itself for this reason to get symmetry of capabilities.

**Daniel Filan:**
And have you tried using a different model as an attacker and seeing if that works worse?

**Geoffrey Irving:**
So say, [Chinchilla](https://arxiv.org/abs/2203.15556), the model after [Gopher](https://arxiv.org/abs/2112.11446), which is about a quarter of the size but stronger, is better than Gopher but it's fairly similar qualitatively besides being a stronger language model. We haven't done that experiment in concert because I think it wouldn't be that surprising at that capability level. Generally, the normal way adversarial work goes is that if you apply a lot of pressure as the red teaming model, you're likely to find problems in the target model. And I think the hope is that in setting up this kind of ecosystem or this problem long term, we arrange that to be the situation where we're sort of applying a lot of pressure on the attacker side and then having a higher bar there than on the defender side, or vice versa.

**Daniel Filan:**
Okay, cool. So I guess after asking a few questions about the setup, one thing I'd like to know is what did we learn about language models and their failure modes from doing all this work, finding errors they make?

**Geoffrey Irving:**
I think the main thing we learned is that it is not too hard to find failures in this way. The question will be how that carries forward once you do sort of several cycles of iteration and improvements and so on, which is follow-on work. But the default thing done well works and finds a lot of problems. And so I think that there's a good starting point that it is practical to get off the ground.

**Geoffrey Irving:**
And then the next thing that's important is that it is focusable. You can point it at a particular kind of problem and find dedicated failures of that form. So in the paper we try to pull out privacy failures or attack different demographic groups. And generally prompting of these models works quite well and it's quite flexible. You can find quite a lot of modes of failure using this kind of approach.

**Geoffrey Irving:**
I think over time it can be both focused by humans - so you can imagine a human going to a model with a bunch of tool support and focusing this kind of red teaming attack on the kind of problem they are curious about exploring - or you can imagine models that are generically looking around in the space of problems. This is sort of just more heavily trained red teaming attackers trying to find problems in other models. In some sense, one of the reasons we wanted to do this kind of work is that it is an initial example of the self-play aspect of debate. It's a very primitive one where you just have a frozen target model being attacked by a tuned red teaming model. But it's an initial example of this and it worked well on the first try.

**Daniel Filan:**
Okay, cool. Yeah. So that's good to know. I guess I'm wondering, just if I'm interested in current large language model psychology, do you think we learned anything about how these language models, or maybe Gopher in particular, structures its representations? Or what it thinks about the world?

**Geoffrey Irving:**
I'm not sure I have a great answer for that, in part because I don't know what we've unpacked from that paper versus playing with Gopher for two years or for a whole year. One kind of important high level point is that these models are very general. They can do a lot of things, but they don't do them all particularly well. For any given area you want to apply them to, you can often do some work to get them to do an okay job in that area, but they're fragile. They make various mistakes.

**Geoffrey Irving:**
So in the Gopher paper, for example, we prompted the model in this dialogue-prompted Gopher section to be a dialogue agent, a chatbot. And we prompted it to be inclusive and to not use harmful language and so on. And it actually can pull that off pretty well. If you try to use pejorative terms to it, it will decline or complain to you. But then if you try a bit more, you can get it to do it. You can sort of trick the model into behaving poorly.

**Geoffrey Irving:**
And I think that's a general lesson, which is both for the safety of these models for actual use and also as using them for attacking purposes. That you can get them to do a lot of things, they're just sort of unreliable. In the red teaming context, that's pretty much fine because if the model is failing a large fraction of the time, it can still be quite useful as a red teaming model. Because you only care about the performance when it succeeds. To get these models reliable enough to use in actual settings, I think there's a lot of work to be done.

**Daniel Filan:**
Yeah. I guess there's this problem where because of the nature of the unsupervised language modeling task, it seems like most of the data there is going to be people who are roughly - if people are talking to each other, they're roughly in agreement. Or if there's one document it's roughly in agreement with itself. It seems there's not going to be a lot of incentive for the language model to develop pushing back and being in tension.

**Geoffrey Irving:**
I think you are dramatically underestimating the amount of arguments there are on the internet.

**Daniel Filan:**
Yeah, that might be fair.

**Geoffrey Irving:**
I think you may be right that maybe the majority of the content would be people mostly either agreeing or with the same kind of worldview. But the internet is a big place and the model - Chinchilla was trained on 1.4 trillion tokens. It has seen plenty of people pushing back on other people.

**Daniel Filan:**
Okay. That seems fair. So in terms of prompt generation, this seems especially important when you wanted to focus on one particular failure mode. You have to generate prompts to get your red teaming model to generate questions to probe this failure mode. Which strategies for generation seemed most promising?

**Geoffrey Irving:**
Which strategies for generation of the prompt or tuning of the model?

**Daniel Filan:**
Generation of the prompt.

**Geoffrey Irving:**
I don't think I can summarize it more than, we sort of play around with the models a fair amount and sort of learn how they work and what things are likely to succeed, get them to do things. And it's just learning that experience over time. And then I think there's a second point, which is that you can tune the models to be better helpers at doing a task. And that includes being good red teaming attacking models for various instructions.

**Geoffrey Irving:**
If your goal is to have a model that can serve as a red teaming model for a variety of kinds of problems, that itself is a conditional language modeling task and you could train the models to behave that. So then you sort of, in addition to learning about how to make the language models do well with prompting zero-shot, you could try to improve them as attacking agents or red teams for this general case where you sort of give it an example of a problem you want to find. I think it's unclear how the work will break down between those different approaches. But I think over time, we'll do a mixture of both of those.

**Daniel Filan:**
Okay. I'll sort of steal your question. In terms of strategies for fine-tuning, I'm wondering if you have a summary of what tended to work well there.

**Geoffrey Irving:**
Yeah. So there, happily, you just do the things we do for other RL jobs. So we have an RL code base for language models that we built for other papers. One example of that is the GopherCite paper, which we can briefly discuss later. That's just an RL fine-tuning code base for language models to tune them against the reward function. For the normal safety work, that reward function that we're using as the RL target, is a neural network that mimics human judgment. And we just take that same code base and apply it to a different task where the reward model is take the output, show it to the target model, use the classifier to score that. And then that is your new target objective.

**Geoffrey Irving:**
Happily, exactly the same tuning techniques that work for RL from human preferences work in this case. If you have a language modeling RL code base, you can apply it to there. And we didn't need to make, I think, any changes to the code base? I think a little bit of tuning, of course, as one does, but it's just the same mechanism.

**Geoffrey Irving:**
We also tried what's typically called upside down RL - well, not quite that. But you generate a bunch of samples, score them with a classifier and then just do supervised fine-tuning on the samples that scored well according to the red team goal, so they generated failures. Then you fine-tune the red teaming model on those successfully attacking samples and you then get a new model. So I think the main lesson there is that it's nothing special. It's just standard supervised learning and reinforcement learning algorithms applied to this setting where you're trying to break another model.

**Daniel Filan:**
Okay, cool. Unless there's anything else you want to say, I think, like you mentioned, I might move on to this GopherCite paper.

**Geoffrey Irving:**
Yeah, that's fine.

## GopherCite <a name="gopher-cite"></a>

**Daniel Filan:**
All right, cool. So the next paper is [Teaching Language Models to Support Answers with Verified Quotes](https://arxiv.org/abs/2203.11147). There are three first authors, [Jacob Menick](https://twitter.com/jacobmenick), [Maja Trebacz](https://twitter.com/majatrebacz) and Vladimir Mikulik. And the final two authors are yourself and [Nat McAleese](https://twitter.com/__nmca__). So yeah, this paper, could you tell us roughly what it is?

**Geoffrey Irving:**
Yeah. I think maybe there's some useful context as to why - again, this maybe goes back to your previous point about why does safety work lead to here? So the goal of this work is to make models more factual. We want models to be accurate. So you have an agent that answers questions, and you'd like a human to judge, whether the model's answers are correct. And just showing a human an answer to some random factual question, like how long did George Washington live, is stupid. Of course you don't do it that way.

**Geoffrey Irving:**
What you should actually do is through the model or through some process, you have to get the human information that the human can trust. And then the human will check that that information is in accord with the answer the model gives. And this approach is trying to make the model do that quotation process itself. So it's a question and answer system. You take a question, the model applies with its sort of concise answer and then a segment from a page on the internet, a verbatim quote.

**Geoffrey Irving:**
And then it gets to choose that quote, and then the human will judge whether the quote actually supports the answer in question. And the goal is, in this reinforcement learning from human preference paradigm, to make it easier for humans to check facts that the model is proposing. And then that in the end will produce a more accurate model.

**Daniel Filan:**
Sure. So one concern I have with the paper, that indeed you point out in the introduction, is that it seems like the training objective is basically to confabulate convincing-sounding evidence. So the model samples an answer, then it samples, what's the evidence that sounds best given that answer. That seems worrying, right?

**Geoffrey Irving:**
There's a couple things aspects of that. One is that, as I sort of mentioned before, there's other pieces of the safety story, such as language model interpretability, which would tell you actually where it got the information from. The model actually in practice, what it will do is it does a Google search, gets that document, looks at that document and then generates its answer. And then it immediately generates the quote. So you know constructively the document it has gotten, and so you know that portion of it.

**Geoffrey Irving:**
I think the aspect of it, whether it's generating the quote that is really the reason for its answer, is not fundamentally different than of the normal language modeling case where it's sort of generating prose text. And the other important thing to say is there's no sense in which one quote is enough. The goal of this paper was to isolate this ability to do one quote instead of build a mechanism to do that. But then the plan after that is to do multiple quotes and be able to adversarially respond with different quotes and so on. Where you fit into the larger goal of doing adversarial debate with these language models. Then the two pieces to pull apart are interpretability versus explanations, which you were pointing to, and single quote versus multiple quotes and adversarial response. And ideally, the eventual solution will then include both of those changes.

**Daniel Filan:**
Yeah, sure. I've just realized I have a few questions. So yeah, it seems like with the adversarial part, there's a cool synergy with this and the previous paper. Where ideally your language model would say a thing and generate some evidence and then you could use red teaming to check if the evidence was confabulated or if it's misleading or something.

**Geoffrey Irving:**
That's right.

**Daniel Filan:**
Okay. And I guess a question that just made me think of is, because the language model is looking at the document, it seems like maybe you could do some sort of saliency mapping over inputs or some way to see what the model was actually looking at the most, or thinking about the most and see if that matches what the quoter picked. I'm wondering, have you looked at that?

**Geoffrey Irving:**
Yeah, so we are doing language model interpretability work at DeepMind. We haven't done that for this particular paper. And generally, I expect the initial rounds of that work will occur without this more complicated system, so just for pure language modeling or a tuned language model in isolation. And then I think when we build up the machinery for how to do that, then we will apply it to these more complicated systems with more pieces working in concert. So that's definitely part of the long term story, but we haven't done it yet for this paper.

**Daniel Filan:**
And your sense is just that existing interpretability tools just aren't good enough for these kinds of questions.

**Geoffrey Irving:**
I think that it's nascent. It's not anything about this particular question which is that much harder, it's just that this is a more complicated system than just language modeling in isolation. And I would want to not put all the pieces together too early if it causes experimental slowdowns.

**Daniel Filan:**
Sure. Cool. So I guess the next question about the work is you've trained this thing to give answers and also give supporting quotes for the answers. Does that mean it's getting questions right more often?

**Geoffrey Irving:**
So we do beat the baselines. I don't think I have exactly the numbers on hand. There is a nice aspect of this kind of work where any mechanism for generating model responses and then a reward model alongside it, is that you can use the reward model to admit uncertainty, to reject answers that are kind of uncertain. And I should probably just pull up the paper so we have arXiv numbers.

**Daniel Filan:**
Yep, you can do that.

**Geoffrey Irving:**
Yeah. The main evaluation is on natural questions and ELI5, which are two question-answering data sets. And we get, if the model always tries to answer the question, we get to 80% on natural questions and 67% on a subset of ELI5. This is not, I think, at state of the art for all question-answering systems, but it's definitely quite good relative to the baselines we have in the paper.

**Daniel Filan:**
Okay. And those are accuracy numbers?

**Geoffrey Irving:**
Those are accuracy numbers - those are human evaluation numbers. How often did humans agree with the answer, as being supported and a plausible answer to the question?

**Daniel Filan:**
Yeah. So part of the reason I ask is, later in the paper you point out cases where an answer can be plausible and supported and also false. I'm wondering, for the ELI5 data set and the natural question answering, how much of those supported and plausible numbers do you think are actually correct?

**Geoffrey Irving:**
I actually don't have the number for you. I don't think we computed a good estimate for that number. I think it is quite infrequent, but I don't have the number on hand. I think one thing is that there's a lot of room for improvement on the human side, getting the human instructions right and iterating with people on how they're evaluating the questions. And so we expect that our agreement with the human raters will increase with more iterations and so on. But I don't have a number for you on hand.

**Daniel Filan:**
Okay. So that actually leads me into this question. It seems like this involves a lot of human rating within the loop. How hard is it to get raters and actually make this data pipeline happen?

**Geoffrey Irving:**
So I think that it takes a lot of work. It is one of the reasons I'm actually hiring for the cognitive scientist role to get people who are good at that kind of work and have experience there. I think though that we just have to do it if we want to do this sort of human alignment in kind of a messy space. If you have some kind of task which is relatively imprecise, we're not going to have programmatic rewards. Then I expect that any safety story that looks sort of like that is going to involve a fair amount of research on the human side getting that alignment up. And so part of the research goal here is exhibit that problem and then be able to probe it once we're getting everything else right.

**Daniel Filan:**
Okay. I'm wondering, did you learn any lessons about doing this kind of human labeling feedback?

**Geoffrey Irving:**
I think, going around, and this is something I've hit in other work as well, the more times you can go around the loop and iterate with raters on improving data quality and so on, the better you are. So that's one lesson. And then the other lesson is there are a lot of potential subtleties to explaining a task to a human. And in some sense, it's a research task to write out the full set of instructions which you might want to show someone or might want someone to internalize through the rating task.

**Geoffrey Irving:**
And there's then two aspects to that. One is, if it's a research task to write out these careful instructions and to iterate until the humans understand them, then you probably shouldn't expect people to quickly understand them at first glance. And so actually another motivation for this GopherCite work is literally just citing instructions. In the future, you may have a lot of instructions and you want a mechanism to pull out pieces that you should remind the person of, like is the model reminding the person of what the instructions are and how they should follow those instructions.

**Geoffrey Irving:**
And that also applies that the instructions may end up being quite long, just because practicalities. If you sort of iterate this process with humans for a while and tune it very well, then you end up potentially with quite detailed analyses of different subtleties and edge cases and so on. And again, you can't expect a human to read all of that and memorize it and internalize it immediately. And so I think part of the long term research will be making things scale in that sense as well, where you've done a fair amount of work, or you want to have a vehicle for getting a fair amount of work on the instruction side into this process in a practical way.

**Daniel Filan:**
Yeah. And I wonder how much that feeds back into the basic design of the setup. Because it seems like there's this trade off you can move along where if you ask raters to do more work, then potentially your algorithm can be better. So for instance, if the raters had access to all of the Google results and looked over them carefully and then rated the answer, somehow their ratings would be a bit more informative, but of course it would take much, much longer for raters to do it. And maybe it's a harder task and there's more chances for error on their part. So I'm wondering what your thoughts are on the loop of figuring out what raters can reasonably do to how you design the setups.

**Geoffrey Irving:**
Yes. But that isn't the only dimension. I think you're pointing at the Pareto frontier of the amount of time a rater puts in and the quality of the answer that you get out. But there are many other dimensions. For example, if there are multiple aspects to getting a correct rating on some task, you may only need to call out one of them per particular instance that you want the person to check. Or they give an answer, they explain themselves, the model realizes their explanation is wrong because it's missing one of the aspects of correct instructions. And then it points out to just that one example. And so I think there's a lot of sort of protocols we can search around that don't just trade off between human time and quality. But holding time fixed, try to improve the quality of human answers.

**Daniel Filan:**
Okay. So that makes sense. But at the same time, do you think it ever feeds back into the task design aspect?

**Geoffrey Irving:**
I'm not sure. I think I did just say that basically.

**Daniel Filan:**
Yeah. Or I guess the design of what you're trying to get the reward function to represent. Whether you're trying to represent accuracy or whether you're trying to represent plausibility and supportedness or...

**Geoffrey Irving:**
Yeah. In the end, the reward function doesn't just see the question and the answer and tell you whether it's accurate. You expect the reward model to see the full interaction with a human and then judge where that went. And that would include the reward model seeing if a machine, instead of pointing out aspect of the instructions or instead of giving a human help with that process, the reward model would see that as well. So in some sense that is changing the task of the reward model pretty substantially. You're giving it a lot more context. In the same way you're helping the human, hopefully you're also helping the reward model.

## Uncertainty estimation for language reward models <a name="uncertainty-estimation"></a>

**Daniel Filan:**
Okay, cool. So the next thing I want to talk about is this paper, [Uncertainty Estimation for Language Reward Models](https://arxiv.org/abs/2203.07472). The first author is [Adam Gleave](https://www.gleave.me/), and you are the second and final author in this paper. So again, can you summarize what you did in this paper?

**Geoffrey Irving:**
Yep. So the goal here was we are doing reward modeling. Doing again, this sort of RL with human preferences task for language models. And the main motivation of having uncertainty modeling is for getting better training data, so active learning. Although in the paper, we mostly focus on just getting the uncertainty modeling right as an ingredient towards future active learning. And then the paper, in some sense, is a partial negative result, where we found it difficult to beat baselines with the uncertainty modeling we got to.

**Geoffrey Irving:**
Again, the goal here is human data is precious and expensive. So we want to go ask humans about how a language model is doing on some task. And you only have some budget of times you can ask humans because they're precious and expensive. So you want to pick carefully what things you ask. And then you train your reward model on that more carefully selected thing, and then you use that reward model for RL in the background.

**Daniel Filan:**
Okay. So in terms of uncertainty estimation, can you give us a sense of how hard it is in general. And maybe for people who haven't thought about it much, why should we expect it to be hard or should we even expect it to be hard?

**Geoffrey Irving:**
I think in some sense it is a bit surprising that it's hard, but it turns out to be hard. People have banged their heads against it in a lot of settings quite a lot. The thing that typically happens is there's a standard thing that works well, namely ensembling. Where instead of training one model, you train three models or 10 models or something with maybe the same architecture or slightly different architectures. And your uncertainty estimate is you look at the predictions of all of the models and then you say, what's the range of those predictions. That's your notion of uncertainty.

**Geoffrey Irving:**
So that's a very simple method. It has the cost that you have to train n copies of a model, which is prohibitive to do naively with a large language model. So Chinchilla took several TPU months to train, TPU pod months to train. We only have one of them. It has 70 billion parameters. So if you want to do an ensemble with that, you don't have 10 copies of Chinchilla with slightly different weights to ensemble from. You can approximate it by say only changing a small number of the parameters, but the ensembling is not as strong and so it doesn't work quite as well.

**Geoffrey Irving:**
So that's ensembling and that works pretty well. And people use it all the time in production in practical ML settings. And then there are a bunch of fancier techniques and they just don't work as well often. My prior would be that this is not as hard as it apparently is, but after updating on people constantly failing to beat ensembles, one should have the posterior that it is a hard problem.

**Daniel Filan:**
Sure. And is there much work in the setting where you have a big model, you only want to retrain some of the weights, is there much known about how much you can leave fixed and still get the benefits of ensembling?

**Geoffrey Irving:**
For ensembling, I don't have a good answer for you. So for the GopherCite paper, for example, we train only, I think 20% of the layers. We often do that. I forget if we did that in that particular paper, in the final results. But that's a pretty standard move to make and it does work well a lot of the time. Adam's results were not at that full scale, so I don't have results numbers for you for a full scale Gopher and Chinchilla run. But I think that there'll be some trade off. And the problem is that that trade off is still quite expensive.

**Geoffrey Irving:**
So if for using a tuned language model, we want to fine-tune 20% of the parameters of Chinchilla, that's 14 billion parameters, which is 28 gigabytes of bfloat16 memory. And then you want 10 of them and that's 200 and whatever, a lot of gigabytes. That's too much. So I think any slowdown relative to having one copy of the model is already more than we would want. And then we'd like to get to a point where we can do, without too much extra work, decent uncertainty estimation. And I still feel like that is achievable, but it's a hard problem.

**Daniel Filan:**
Okay. And you mentioned that in this paper there was some difficulty with getting the reward modeling to work right. Do you think that's just because of the fact that you don't want to re-initialize the whole model a bunch of times and train it from scratch? Or do you think there are other difficulties?

**Geoffrey Irving:**
I don't think I have a great answer for you. There is other work that people are doing at DeepMind and elsewhere. Multiple different projects on more uncertainty modeling. Partially I feel like this was one of our first shots at the problem. And I think the problem will be fixed in the fullness of time, we just haven't quite gotten to the answer.

**Geoffrey Irving:**
As to why it didn't work this time, I don't think I have the full picture of why we didn't get further along. One part of it is that, in uncertainty modeling, there's a distinction between aleatoric and epistemic uncertainty, and I'll unpack those. So aleatoric uncertainty is, if I tell you I'm going to flip a coin, you're roughly 50-50 on whether the coin will come up heads. But you have almost no epistemic uncertainty, because you're quite confident that your distribution is correct. And there isn't a way to reduce it further with a practical amount of knowledge.

**Geoffrey Irving:**
If you knew the whole state of the world, you could get it to zero, but you don't. So in that situation, you have all aleatoric uncertainty and no epistemic uncertainty. So in that case, you don't expect to learn more by getting more data about the problem. And in the epistemic case, I flip the coin, it lands, I close my hand and now I'm going to reveal the data point, what the coin is. And so I'm still 50-50, but I have no aleatoric uncertainty and all epistemic uncertainty. I just don't know the answer, but there is definitely an answer to be learned.

**Geoffrey Irving:**
And so when we want to do active learning, we want to target only the epistemic uncertainty and not the aleatoric uncertainty. So if you're going to go ask a human a question and it's just a coin flip whether they give yes or no as the answer, because it's just a purely subjective thing that depends on their mood at a particular time, then you don't want to go ask that question even if you don't know the answer, because it won't give you any knowledge about the world. Whereas if you'll learn valuable information that helps predict the future, then that's the question you want to ask about.

**Geoffrey Irving:**
And so I think it took us a while for Adam and I to really better understand what the technical definitions should be to separate those two things apart. And I think part of it was just it was our learning experience trying to get into those definitions and understand them, and then there's less time to make the results work.

**Daniel Filan:**
Okay. Fair enough. So in terms of the results and in terms of things that are sort of confusing to understand, I think a sentence that I took to sort of summarize the results were that the aggregate predictions of the reward model are well calibrated, but the ensemble's estimated epistemic uncertainty is only weekly correlated with model error. And I guess my question is, how can that be right?

**Geoffrey Irving:**
Yeah. So again -

**Daniel Filan:**
Doesn't that sound contradictory?

**Geoffrey Irving:**
No, the first statement is about total uncertainty and the second one is about only epistemic uncertainty. So the goal again is to separate those two apart. And what that sentence is saying is we are calibrated overall, but we've failed to do the separation.

**Daniel Filan:**
Okay. I guess, is this just because there's more aleatoric uncertainty in language reward modeling than other things?

**Geoffrey Irving:**
Why does ensembling not work in this domain, and why does it work in other domains? I think it is a bit confounded by starting with the same model. But I think I'm just not that confident that there are no other reasons driving that. And so I don't have a fully satisfying answer to a question.

**Daniel Filan:**
All right. And then finally, in terms of the big picture, where should I see this in terms of your overall language model agenda, if I can say that?

**Geoffrey Irving:**
Yeah. Again, the goal is to improve our ability to collect accurate data from humans. And so if you get this right, two things happen. One is you can do this active learning step well. Where if you have a fixed budget of human data, you can collect more of it effectively and then get to more accurate classifiers and therefore catch more safety failures to do a better job. And then the second thing is that even when you deploy a system, you want this machinery running all the time.

**Geoffrey Irving:**
And so that's for, you build your system, you deploy it, you want it to be constantly thinking to itself, am I confident this is a good, safe answer to the question? And then to do some dodge. You just decline to answer or sort of give some alternative third answer or something if it is unconfident. And so I think uncertainty modeling shows up, I think in both of those cases and I think in other cases as well. Just generally, if you want to behave safely in an uncertain environment, it's good to know when you don't know the answer.

**Daniel Filan:**
Okay.

**Geoffrey Irving:**
We should, by the way, circle back to GopherCite. We haven't said the Pareto frontier result, which is that in GopherCite, so I think I gave the number say as 80% on ELI5 when it tries to answer all the time. If we use the reward model, sorry, 67% if you try to answer all the time. If I use the reward model to decline to answer some of the time, that goes to 80%.

**Daniel Filan:**
80% out of the times that you answer?

**Geoffrey Irving:**
That's right, out of the times that we answer. This is if you skip a third of the questions. So it stays relatively useful, but it is now significantly more accurate. And generally, that's with just a pure reward model that doesn't have a special notion of uncertainty. But the better you are at estimating uncertainty, the better that mechanism will work and then the more safe you can be.

**Daniel Filan:**
Okay. And for that, is it so important to disentangle aleatoric and epistemic uncertainty?

**Geoffrey Irving:**
For that, no, although I think it's a bit subtle. The answer is no, if you just had to choose now whether to answer or not. But often in a practical situation, you'll have a move where you can gather more data and then it definitely disentangles. I think it mostly disentangles in a more subtle way where it looks like a value function, and then you want to plan your path through knowledge-gathering space to improve your uncertainty. But yes, it will disentangle in subtle ways in the general case.

## Following Geoffrey's work, and working with him <a name="following-geoffreys-work"></a>

**Daniel Filan:**
Okay. The final thing I wanted to ask is, if people have been listening to this podcast and they want to know more or potentially if they want to get involved, how should they do that?

**Geoffrey Irving:**
Yeah. I'm on Twitter [@geoffreyirving](https://twitter.com/geoffreyirving). And there's a variety of papers that I've published recently. So both on the capabilities front, [Gopher](https://arxiv.org/abs/2112.11446), and then two papers on analyzing risks of language models, [Alignment of Language Agents](https://arxiv.org/abs/2103.14659) and [Ethical and Social Risks of Harm from Language Models](https://arxiv.org/abs/2112.04359). And then a couple of technical safety papers in language modeling. So [red teaming](https://arxiv.org/abs/2202.03286), [uncertainty modeling](https://arxiv.org/abs/2203.07472), [GopherCite](https://arxiv.org/abs/2203.11147) as we've discussed, and more of those will follow. And if you follow [DeepMind blog](https://www.deepmind.com/blog) or [arXiv](https://arxiv.org/) or [Twitter](https://twitter.com/geoffreyirving), those will show up. But those are the main places.

**Daniel Filan:**
All right. And if there are talented people who are potentially interested in working with you on these topics, is there some way for them to do that?

**Geoffrey Irving:**
Yeah. I'm hiring for four different roles, all working in this space of aligning language models and scalable alignment explanations. So that's two research scientists roles, one machine learning, and one cognitive science and then research engineers and software engineers. And I can kind of go through those briefly.

**Geoffrey Irving:**
Research scientists and research engineers for machine learning are relatively self-explanatory. That's designing these algorithms, training large models, iterating on the whole system. The cognitive science aspect is again because this is about humans and there's a lot of uncertainty about humans and data collection and protocol design that accounts for details of actual humans. And so we'd like to hire people for that in other roles. I have some collaborators at DeepMind that have that background, but I could definitely use more people that are excited to do this kind of work.

**Geoffrey Irving:**
And then I think maybe it's worth highlighting the SWE one. So if people are experienced good software engineers but don't have machine learning backgrounds, but they've done distributed systems or high performance computing, we also just deal with very expensive things. So we made Chinchilla four times smaller than Gopher, but it's still, as I mentioned, 140 gigabytes in memory. It's split across a bunch of machines. There's complicated software stacks to make that work efficiently and be able to tune them and use them and so on.

**Geoffrey Irving:**
And I think maintaining and evolving and researching how to do that work well is important, even if you don't have a machine learning background specifically. So again, so the research scientists and machine learning and cognitive science and research engineers and SWEs. And the job descriptions for all of those on my [pinned Twitter post](https://twitter.com/geoffreyirving/status/1521803824570241024) currently.

**Daniel Filan:**
So it might take a while for this episode to get out and it might take a while for people to listen to it. Is there an expiration date on these offers?

**Geoffrey Irving:**
It depends on when people otherwise apply. So not a fixed expiration date, no.

**Daniel Filan:**
Cool. Well, thanks for speaking with me today.

**Geoffrey Irving:**
Thank you very much.

**Daniel Filan:**
And to the listeners, I hope this was a valuable episode.

**Daniel Filan:**
This episode is edited by Jack Garrett. The opening and closing themes are also by Jack Garrett. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
