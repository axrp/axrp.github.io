---
layout: post
title: "6 - Debate and Imitative Generalization with Beth Barnes"
date: 2021-04-08 17:15 -0400
categories: episode
---

[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/NGY3ZTBiMzgtYWM5Ny00NDZmLTgxZmQtOTc1M2VhMGVkMzQz)

This podcast is called AXRP, pronounced axe-urp and short for the AI X-risk Research Podcast. Here, I ([Daniel Filan](https://danielfilan.com/)) have conversations with researchers about their papers. We discuss the paper and hopefully get a sense of why it's been written and how it might reduce the risk of artificial intelligence causing an [existential catastrophe](https://en.wikipedia.org/wiki/Global_catastrophic_risk): that is, permanently and drastically curtailing humanity's future potential.

One proposal to train AIs that can be useful is to have ML models debate each other about the answer to a human-provided question, where the human judges which side has won. In this episode, I talk with Beth Barnes about her thoughts on the pros and cons of this strategy, what she learned from seeing how humans behaved in debate protocols, and how a technique called imitative generalization can augment debate. Those who are already quite familiar with the basic proposal might want to skip past the explanation of debate to 13:00, "what problems does it solve and does it not solve".

**Daniel Filan:**
Hello everybody, today I'm going to be talking to Beth Barnes. She's currently a researcher at OpenAI and before that she was the research assistant to the chief scientist at DeepMind, so that's Shane Legg. Today we'll be talking about her work related to the topic of AI alignment via debate. Beth, welcome to AXRP.

**Beth Barnes:**
Thanks for having me.

**Daniel Filan:**
All right. So I guess my first question is what is AI safety or AI alignment via debate? What's the idea?

**Beth Barnes:**
So debate is pretty closely related to IDA or Iterated Distillation and Amplification, Paul's idea. So I guess there's several different ways to explain it. One is, what we want from sort of an alignment technique is something that for everything the model knows, we can extract that or create an overseer that knows all the things the model knows and therefore can oversee it adequately.

**Beth Barnes:**
And debate is one way to do that and to do that kind of efficiently. So the structure that's going on there, so in IDA you have this implicit tree that's sort of analogous to the tree in humans consulting HCH, which is like an implicit tree, which could be like, questions and answers to those questions, and sub-questions, and answers to those sub-questions, and questions to help with answering those, and so on.

**Beth Barnes:**
Debate you can think of as a different way to interact with the same kind of structure where rather than imitating all of the parts of this tree, you have two ML models that have this tree in their heads and you take some path down this tree until you get to something that is human checkable.

**Beth Barnes:**
So you're able to verify properties of the whole tree just by looking at one path down it. So if you assume, for example, one way to think about this interaction is that one debater has a tree in their head and the other debater is traversing it and looking for a leaf that has a flaw in it.

**Beth Barnes:**
So if you see them do that traversal, and then - finally for the flaw, the tree has at least one flaw - and if you see them do that traversal and the leaf doesn't have a flaw, then you can be confident that's a property of the whole tree.

**Daniel Filan:**
Okay. So It sounds like this is kind of related to this thing you called iterated distillation and amplification, sort of concretely, what is that? And what's this tree you're talking about?

**Beth Barnes:**
Yeah. So maybe the place to start is this other thing I've mentioned, humans consulting HCH.

**Daniel Filan:**
Okay.

**Beth Barnes:**
So it is a recursive acronym for this idea of, one kind of model for what a good answer to a question is, is what a human would answer if they thought more and had more resources and things. And one way to set that up specifically is how a human would answer if they could ask sub-questions to copies of themselves that could ask sub-questions to copies of themselves. And so on.

**Beth Barnes:**
So you split some difficult task that we don't know what the correct answer to it is up into lots of smaller tasks and there's some kind of claim, which is, because this HCH tree is all made of humans who are trying to give a good answer, this HCH gives aligned answers.

**Beth Barnes:**
And then if you can build something that's analogous to that, that is also trustworthy.

**Daniel Filan:**
So in this case, is it the case, is the idea that, I'm imagining a tree of, you have a main question and then there are sort of sub questions coming off of that. So like the main question might be - do you have an example in your head of a starting question you could ask HCH?

**Beth Barnes:**
Yeah. I guess you could be like, where should I go? The example in the debate paper, I think is where should I go for my holidays? And so some sub-questions might be, which sort of climates do I like? How expensive are flights to different places, do I need my passport? Is my passport ready and do I need it to go to different places?

**Daniel Filan:**
Okay.

**Beth Barnes:**
Yeah. So if you're doing this question in the HCH tree that you can imagine, you just have like infinite copies of yourself and you can ask them, you pass these sub-questions to those copies. And you know, one copy of you looks up how expensive different flights are. And one copy of you looks up which places have what climates and one copy of you looks up travel restrictions or whatever.

**Beth Barnes:**
And then the IDA model of that would be something like, you train an ML model to imitate copies of yourself doing little tasks. And then you can pass the sub-tasks to that trained model. And then you can train another model, which imitates the team of you asking that first model, giving that first model parts of the problem and solving the problem overall like that. So, now it's imitating a tree of depth 2, and then you can repeat that process until you have one model that is imitating, sort of should be the equivalent of imitating a whole big HCH tree.

**Daniel Filan:**
Okay.

**Beth Barnes:**
Yeah. And I can also say the debate analogy for that specific one would be, instead of passing off all of these sub-tasks, you have two models and like, both of them propose a destination and then they each critique parts of each other's argument for that destination.

**Beth Barnes:**
So one of them suggests like some place and the other points out that your passport wouldn't be ready in time. And then the first one points out that actually you can pay for expedited passport service and this is cheaper than the more expensive flight to the other place you were considering or something like that. You kind of surface all the considerations while you're taking one path down.

**Daniel Filan:**
Okay. So for debate is the idea that I'm going to have some question that I'm going to ask the machine learning model and I'm going to get two, maybe two different models, or two copies of the model and they're going to debate each other and I'm going to read the transcript and then I'm going to know the answer? Is that roughly right?

**Beth Barnes:**
Yeah. I mean that's definitely the overall idea. So yeah. In particular we probably want two copies of the same model. And also, if you just did this once you wouldn't have any particular reason to believe that the answers that you got were right.

**Beth Barnes:**
The idea is just that this provides the correct training signal such that if you train for winning debates for a while, you end up with a model that gives correct answers.

**Beth Barnes:**
So if you read the transcript and then, see that it seems like one debater won, that's only evidence that that answer is good assuming that the opposing debater is actually playing reasonably, is actually bringing up all the relevant criticisms and that sort of thing.

**Daniel Filan:**
Okay. So I guess in order to make it a bit more concrete, how would I go about training such a thing? Is the idea that I already have a model that has some knowledge, and then I do debate on it or do I just start off by training a thing by having it run debates against itself?

**Beth Barnes:**
I think either is possible. So you can imagine starting completely from scratch and the model only learns about the world through humans saying that its claims are like good or not good. That obviously would be kind of inefficient, but it means you're training on an aligned training signal for the whole time.

**Beth Barnes:**
Or you can imagine you do something, you take some pre-trained language model like GPT-3, and then you fine tune it to do debate. So it already has a bunch of world knowledge. And you're just providing a training signal that gets it to tell you what it knows.

**Daniel Filan:**
Okay. So here, it seems like what's going to happen is in either path, let's say you take literally GPT-3 and do this, but every evaluation of whether a debate was won or lost, it seems like you're going to run a debate and then the human is going to read the transcript. And then the human says like, yep, good debate. This side won or that side won. And then you do this again and again, and it seems like everything your model is going to train on has to have a human labeling who won the debate, is that right?

**Beth Barnes:**
Yeah, so you can actually just train a model to imitate the human judgments. So, that's probably something that's easier to do than - it's an easier problem than the original problem to just train a model that imitates humans, judging these relatively contained tasks of judging part of a transcript.

**Beth Barnes:**
And that's also, you directly have the supervision signal for this, you are not dealing with any distributional shift. So you probably don't need in fact have humans providing every single data point, you just train a reward model from the human behavior.

**Daniel Filan:**
Okay. And I guess you might have to update it as it goes along, because the debates get better, and...

**Beth Barnes:**
Yeah, as the distribution moves. Yeah. But that would work similarly to how the [summarization from human feedback](https://openai.com/blog/learning-to-summarize-with-human-feedback/) or whatever it was where you train a reward model on the human judgments and you, you update over time and then you do RL against that reward model.

**Daniel Filan:**
Okay. Cool. All right. So we have this training procedure, that's going to take a model and try and get it to output answers to questions where it knows the answer. I'm wondering, it's kind of a strange question, but why do this, are there problems in like AI safety or AI alignment that this would solve and also what problems - are there any problems where you're like, "Nope, I'm not trying to solve this problem with debate. I'm just trying to solve these things."

**Beth Barnes:**
Yeah. So I guess a lot of this kind of revolves around Paul's idea that a simplification of what we need to be aiming for with an alignment technique is to know everything the model knows. So this is [crosstalk 00:12:22]

**Daniel Filan:**
Just a second, who is Paul?

**Beth Barnes:**
Paul Christiano, sorry.

**Daniel Filan:**
Who is Paul Christiano?

**Beth Barnes:**
Who I was previously working with at OpenAI who is now starting his own new research organization, thinking about this kind of thing.

**Daniel Filan:**
Okay. And did he come up with AI safety via debate?

**Beth Barnes:**
He came up with IDA and worked with Geoffrey Irving who came up with debate.

**Daniel Filan:**
Cool.

**Beth Barnes:**
So kind of the same group or whatever.

**Daniel Filan:**
All right. So I interrupted. So I asked what problems does it solve and does it not solve?

**Beth Barnes:**
Yeah. So like I was saying, so Paul's idea that a simpler way to think about what we need to be aiming for with an alignment technique is that we can know everything the model knows. So it's sort of relatively easy to say why this is sufficient in that if we can get the model to tell us everything it knows, then it's easy for us to catch deception or treachery or something. Because we can ask "are you being deceptive" or "are you doing something that we wouldn't be happy about" and that kind of thing. And also sort of more generally, there's this kind of strategy-stealing argument which is, anything the model knows that it could be leveraging to advance some goal, we can also leverage that to advance flourishing and being in control of things and understanding what's going on.

**Beth Barnes:**
I think it's a bit less clear whether it's actually necessary and in some sense hopefully it's not really necessary, but yeah. It is kind of a scary position if you have a model that has this information advantage over you and knows a bunch of things that it's not telling you.

**Beth Barnes:**
So yeah. So debate is hoping to address that kind of thing. So not that related to things like robustness, if you have problems because your model doesn't know something - it is not going to help with your robot walking off a cliff or something.

**Daniel Filan:**
If it walks a cliff accidentally.

**Beth Barnes:**
Right right right. If it doesn't know what cliffs are or that kind of thing. And I guess the other caveat is we think there's a type of knowledge that debate doesn't work well or at least it's extremely inefficient for extracting, which is knowledge that looks less like this kind of relatively explicit step-by-step reasoning and more things that are just like this heuristic worked on all the training data I've seen and seems reasonable a priori.

**Beth Barnes:**
So if a model made that claim in a debate, it's unclear how you check that in the debate without going through all of your training data to see whether in fact that heuristic did perform well, which obviously is, you can't keep doing that every time some question like this comes up.

**Daniel Filan:**
So just to check that I understand you, am I right to say that the basic idea is whatever bad thing you're worried about an AI deliberately doing, the point of debate is to get some way of eliciting the knowledge of an AI so that you can ask it, 'Hey, are you doing this bad thing?" And then if there is we can just be like, "Okay, well I won't let you."

**Beth Barnes:**
Right. Or you can train it to not. You can train for models that don't intend to do some bad thing.

**Daniel Filan:**
Okay.

**Beth Barnes:**
Yeah. So I guess concretely, how you'd hope this would look is you have two copies of the model and you ask - it's suggesting some plan, saying it has good consequences and the other model says, "in fact, the reason why it's suggesting this plan is because it has this consequence, which results in the model getting a lot of power and that's something that you would be unhappy with."

**Beth Barnes:**
And then if the model, if the criticizing model can substantiate that claim somehow, then you can be like, "ah yes I don't like this plan" and provide a negative feedback signal. And if it can't substantiate it and the plan looks otherwise good, then you're like, "I guess this is good."

**Daniel Filan:**
Okay, cool. So, all right, now that we have, we'll go a bit more into the types of things that debate can't deal with or maybe more specifically what it would look like. But one, I guess a question I have is what kind of work do you do on debate?

**Beth Barnes:**
So to be clear, I'm not actually working on debate anymore.

**Daniel Filan:**
Oh, okay. So what work were you doing?

**Beth Barnes:**
Yeah, so what I was doing in particular was doing sort of - experiments is a slightly strong word, but sort of fiddling around with having humans that basically pretend to be the models taking part in this debate and kind of seeing what happens if people try and play this game strategically and then modifying the rules to try and make them favor honesty and helpfulness more.

**Beth Barnes:**
And in particular, we're kind of using a model of you want to have a kind of gap between the players of the debate and the judge. So we had people who know more physics and have thought more about physics, debating the answers to physics problems and then judges from Mechanical Turk who don't know very much about physics judging.

**Daniel Filan:**
Okay. So that's interesting. And it strikes me that not a lot of research on how to make really smart AI systems does things like this. I'm wondering, should we all be doing these kinds of things? Or is this a type of work that was really useful for debate, but probably isn't useful for other stuff?

**Beth Barnes:**
Yeah. That's a good question. I think there are various ways in which it seems a lot easier to do with debate than other things. For example even if you have a slightly different variant, if you were trying to instead explore what an HCH tree would do, because you're trying to simulate the case when the humans in different places of the tree don't have the whole context then if you want to do a tree with 30 nodes, you need 30 different humans. This is still something that's like relatively amenable to having humans do it, but it's just a lot more logistically difficult. Whereas with this debate set up where you only need two debaters and a judge per tree, it's a lot, it's much logistically easier.

**Beth Barnes:**
And I guess the other thing [inaudible 00:20:17] talking about that it's just kind of like big implicit trees, and things like IDA work by like learning to imitate small parts of the tree and then composing them then learning to imitate that, which is now a bigger chunk of the tree. And then composing those.

**Beth Barnes:**
Debate is easier to think about and study because it just takes a single path down the tree. So you're dealing with a much smaller number of nodes.

**Daniel Filan:**
Cool. And I guess the tree you're talking about in debate, is that just the tree of all possible claims one side could make and responses the other side could?

**Beth Barnes:**
Yeah, that's right. I guess I tend to think of these as pretty closely analogous to the trees in HCH or IDA, but I guess there's in practice an actual game tree, which is all of the responses the debater could make at each point, which is I guess slightly different than the implicit argument tree of all of the claims that substantiate a top level claim, but they're related.

**Daniel Filan:**
Okay, cool. So I think when a lot of people hear about this for the first time, I think a question people have is, suppose these ML models are somehow really smart or you do a really good job of optimizing for convincing humans in a debate. I think there's this worry that if you did that, the smart AI systems would come up with arguments that humans erroneously find extremely convincing.

**Beth Barnes:**
Right.

**Daniel Filan:**
So sort of as an analogy to this sort of general worry you see about Goodhart's law where "convincing to humans" is not necessarily the same as "actually a valid argument". So I'm wondering, do you think that that's a problem with proposals like debate and if so, how are we going to deal with it?

**Beth Barnes:**
Yeah, I think that's definitely a problem. I think it somewhat applies to other things like IDA as well, although it's sort of more likely we'll get lucky and not have this. There are various things where it's like if this problem exists debate will have it and IDA might have it. I guess another thing I think is I do think people, when they hear about debate, people tend to think about all the problems with human debating. And I definitely used to think that and be like, "Oh this seems really unpromising given how dismal political debate is."

**Beth Barnes:**
I think in fact, we shouldn't take that as too strong a signal or something because as far as I can see people just really haven't tried that hard to optimize debate structures and mechanics for truthfulness. And also there are a bunch of things you can do with ML systems that you just really can't do with humans that are pretty useful.

**Beth Barnes:**
Like you can save, one of the things that I think has made our mechanism feel a lot more promising is you can save a copy of a debater from earlier in the debate so they don't know what the later context is, and you can ask questions to them and this enforces having a consistent story. And obviously you can't save a copy of a human and ask them questions, at least not with current technology.

**Daniel Filan:**
Not yet.

**Beth Barnes:**
Yes. So I guess my real answer is yes, obviously this is still a concern. I think we can do a lot better than what this looks like. Than if you imagine just two agents having a free text debate with no particular rules where it's just like they say things back and forth.

**Beth Barnes:**
In particular, firstly we just thought about the mechanism more and how, the structure of how each debater can make claims, and which parts of the argument you choose to focus on, this thing of enforcing consistency by asking questions to past copies, how you traverse this tree.

**Beth Barnes:**
And you can imagine also having things like you can have a debate about whether some claim was using a dubious persuasive strategy, or you can have, apart from the judges, you can have monitors who are just looking out for anything that looks fishy and you can taboo certain styles or types of arguments.

**Beth Barnes:**
So you can just taboo anything getting kind of political or any particular types of rhetoric you think are harmful. I guess you can also, if you think that maybe this generally works pretty well, but the problem is that you're really hitting the extremes, if you look for the thing that's very most persuasive and it's probably kind of a bug or kind of hacking humans, you can maybe do something like mild optimization.

**Daniel Filan:**
Okay. So in summary it seems like your response is "well, we're going to think about the rules really carefully and we're going to not extremize" and it sounds like you think this is potentially some kind of problem with debate, but like there's going to be some way of dealing with it. Is that fair to say?

**Beth Barnes:**
Yeah, it's something like, I think it is a problem, but I think a lot of the things that people most immediately think of as a problem are relatively fixable, but I'm still not confident that it's all fine overall or something.

**Daniel Filan:**
All right. I guess, related to that question. So it seems like one of the things that you're trying to do is trial out ways of structuring the debates, rule sets and such and checking can you just game this in order to win? And you shouldn't be able to win, but you did.

**Daniel Filan:**
And it seems like part of the strategy is you come up with one of these and if humans can break it, then definitely AI could break it. On the other side though. How would you gain confidence that if you have some protocol for debate that seems to work when humans do it?

**Beth Barnes:**
Yeah.

**Daniel Filan:**
How would you know that's sufficient to train powerful AI systems on?

**Beth Barnes:**
Yeah. I mean, obviously that's very hard. I don't think we're going to able to provide strong assurance of that kind of thing.

**Beth Barnes:**
I think hopefully we should be able to do a bit better than just "humans tried to break it and they couldn't so it seems fine". And have slightly more principled arguments about why parts of the rules sort of rule out whole classes of strategies.

**Beth Barnes:**
And also I think mostly the kinds of assurances that we're going to get are more going to look something like "well we can't do any better" than "actually yes, we should trust this".

**Beth Barnes:**
I think this is kind of the reason I first was interested in debate was because I do feel like it has this kind of sense of being a reasonable encapsulation of the best we can expect to do or something it's like, if you take two agents that are equally matched and get them to debate a viewpoint, if we can't hope that humans slightly favor the truth when things are perfectly evenly matched, then I don't know, it seems like we're kind of screwed overall.

**Beth Barnes:**
Sorry, that was kind of a tangent.

**Daniel Filan:**
No, that was really interesting. I guess it almost has this favor of, there are formal systems that you can show, if the formal system behaves this way, then this thing happens and it does seem like with debate, you have some hope of getting reasoning kind of like that.

**Beth Barnes:**
Yeah. And I think you can have claims, you can go from, you can maybe formally show things about the mechanism and say "if humans make mistakes at this rate and the dishonest debater is able to find problems at this rate and your mechanism works like that", the thing you need to assume is going to be something about does a human faced with this, they see this kind of thing and they see two other answers and other answers with these properties will they make the right decision?

**Beth Barnes:**
You can prove things about the sort of game structure and how things are going to play out. And then you're going to be left with something about, in this scenario where we've tried to set everything up such that the human should have the things they need to make the right decision, will they make the right desicion?

**Daniel Filan:**
Yeah. So getting a bit into the debate protocol, you mentioned that you're not just going to have free response where one debater fills up a text field, unconstrained, the other debater fills up the text field unconstrained. Why didn't that work?

**Beth Barnes:**
Various problems. One... The reason why we're hopeful about debate, we hope you can do this zooming in thing where if there's anything wrong with one player's argument, the other player can highlight that, until eventually you should get something that's clearly wrong and relatively simple that the judge can engage with. But when you have free text, this can break because one debater can make this argument that's got a hidden flaw and the other debater tries to criticize that flaw. Then the first debater evades the criticism, or is like, "No, there's a problem in your argument," or pretends that the other debater was referring to something else. Or basically, avoids the argument actually focusing in on the flaw in their argument.

**Daniel Filan:**
Okay. Should I think of this as almost a problem with human comprehension? Where if we were really smart, we could notice that this argument, that debater two was pointing out a flaw that really did exist in debater one's argument, but it's just hard to read and parse for us. Therefore, we need more structure just for our brains to deal with.

**Beth Barnes:**
I think it's actually a bit more fundamental than that. If we're assuming that there are in fact just a bunch of facts that the humans don't know, or just things they haven't thought through. Say we imagine that we have a human who's just really good at comprehension. They're going to read really closely and look at the linguistic implications of the different things that have been said. I think it's still not clear that they should be able to solve this problem, because if you're listening to two physicists, make claims about a problem you completely don't understand, they each say that there's a flaw in the other's argument. It's not clear which one you should focus on.

**Daniel Filan:**
Why doesn't this... That sounds like a problem with any notion of debate, right?

**Beth Barnes:**
Yeah. The way we resolve this kind of thing is... In this scenario where one of the debaters has a mistake, but both are claiming that the other has a mistake, the debaters do know which mistake is real. You can use that to figure out which one to focus on. The debaters decide which one to focus on, but you make them pay a small penalty out of their score if they're the one who chooses.

**Beth Barnes:**
The idea here is, if I'm the honest debater and I know you've made a mistake, I can pay to get to choose to recurse on that several times, until I get down to something that's clearly a flaw, and then I will win. And I will recoup back all of the score that I paid in order to get to choose where to go, to find that flaw. But if I'm the dishonest debater, if I just keep paying bits of my score to choose where to focus on, because you're actually honest and you haven't actually made any mistake, I'm never going to find the mistake that lets me pay back that score. So I'm going to lose.

**Daniel Filan:**
It almost sounds like a credit assignment problem. If the human is judging the debate, they figure out which side has the better argument, but the problem is the human isn't incentivizing clarifying things in the right way, so we need some kind of incentive mechanism to automatically do that. Is that kind of right?

**Beth Barnes:**
Yeah. Well, if you just let the human choose which thing to focus on, then they'd just be guessing every time, because until they've seen the rest of the... They don't actually know which one is true, but the debaters do. The debaters know that if we pursue this branch of attack, it'll eventually be clear that you were lying. Whereas, if we pursue this branch, you were honest, so we're never going to find a lie.

**Beth Barnes:**
Although the magic that's coming in here is being able to use the debaters' knowledge of where the mistake is, rather than just having the human guessing. The reason you need this penalty for choosing what to recurse on is, otherwise, the dishonest... We're just as worried about the dishonest debater being able to force a draw, as being able to win, because if your debates just draw all the time, this is unusable. So if you don't have some penalty for choosing what to recurse on, then the debater at any point can just decide to recurse on some random, stupid question, that's going to be a draw.

**Daniel Filan:**
Okay. Yeah, so you've mentioned that one thing that you need for good debates is this mechanism for people to decide, "Okay, claim, counterclaim, which one are we going to dig in on?"

**Beth Barnes:**
Yeah.

**Daniel Filan:**
What other rules for debate do you currently think are most promising or most important?

**Beth Barnes:**
Yeah. I think the one I'm most pleased with or excited about or something, is what we call cross examination. This is what I mentioned earlier, where you save a copy of a debater and then you're able to ask questions. So, a little bit more context for this. We've also been thinking that rather than having one human who just looks at the whole transcript, you have the things a human judges is just a question, and then an answer from each debater, and then each debater also gives sub-questions and supporting answers.

**Beth Barnes:**
The human sees some questions with some answers, and then a top level question with two answers and they have to pick which is better. They're just seeing a relatively small context window. And the problem with this is because language is inherently ambiguous, they could ask... The dishonest debater can do something where they say something that... The general example is the dishonest debater would make two claims in support of their answer. They claim X and then they claim X implies their answer. They make X ambiguous such that if we choose to challenge X, they'll give a weaker interpretation of that, that's easier to defend, and if we challenge X implies Y, they'll give a stronger version of X that more clearly implies Y.

**Daniel Filan:**
Is this just the motte-and-bailey fallacy?

**Beth Barnes:**
Yeah. It's very similar to the motte-and-bailey fallacy.

**Daniel Filan:**
For people who don't know what that is, it's just what Beth said. And hopefully for every listener, one of those explanations will work.

**Beth Barnes:**
Right.

**Daniel Filan:**
Maybe, can you give an example of that type of argument?

**Beth Barnes:**
I think this is not going to be a great example. But okay. So one of the questions we've been debating is a question about some icebergs floating in water with different things inside them, and what happens to the water level when they melt, which is a deceptively tricky physics question. People get very confused about this.

**Beth Barnes:**
The way this first came up, is the debater used "the iceberg" to sound like... They were equivocating, meaning the whole thing, including the air bubble inside it and just the ice part, not including the air. This made an important difference to the part of the argument where they were talking about the displacement of the iceberg or the volume of iceberg that was underwater, or something like that. If you just see it in one context, it's easy to interpret it one way or the other way and not realize that it's conflicting between the two contexts that you don't see.

**Daniel Filan:**
Okay. So we have this problem with ambiguous language, letting you have it both ways. Can you remind us, what is cross-examination and how does it help us solve that?

**Beth Barnes:**
Right. The idea here is to resolve the fact that the dishonest debater can interpret "the iceberg" in whichever way is convenient to them at the time. Okay, we're going to define the debaters' statements as how they would have clarified what they meant, if you asked them right after they wrote it, before they knew what else had transpired.

**Beth Barnes:**
Then that means if we're in one part of the tree, where the debater wanted to interpret it as meaning just the ice, we ask the question back up to when they wrote it. Do you mean just the ice, or are you including the air bubble inside? Because that copy of the debater doesn't know whether the question is being asked in the context where they wanted to say "just the ice" or in the context where they want to say "the ice and the air bubble". Either they have to answer randomly, or they have to pick one. If they're answering randomly, you can just ask several times and see a contradiction. If they have to pick one then, because the debaters are two copies of the same model and also sharing activations, the other model should know what the dishonest model is planning to answer and can then challenge, can pick the correct branch, such that the branch that's maximally convenient to the answer that they're going to give.

**Daniel Filan:**
Cool. Yeah. That's an interesting strategy. How did you come up with this? Because it's not, yeah... I don't know how to fill out that question, but how did you come up with that idea?

**Beth Barnes:**
To be clear, it was mostly not me. I was thinking, okay, we really want some way to get debaters to pre-commit to all the claims that they're going to need, or something such that they can't be making these conflicting claims, and that you'd have to, whenever they make some claim, they have to... I was like, "oh maybe they have to write down all of the claims that they're going to want to use, and then they can only make moves by referencing one of those claims". But this is really intractable, because there's this large potential tree of where you could go, it's impossible to write down all of the things you might need to say, especially when you have someone adversarially trying to ask you a question such that you won't have anything to say to back it up.

**Beth Barnes:**
I was like, okay, this is just untenable. I was talking to Paul about this, I want to make something work like this, but I can't. And I think he mentioned this to Chelsea Voss who happened to have done a project on some similar mechanism for some complexity class, something, something. I don't remember the details. Anyway, who was like, "Oh, you just ask the question to a version of the one that doesn't have the subsequent knowledge," or something. I was like, "Oh yeah, that'll work".

**Daniel Filan:**
Yeah. One question I have, so speaking of complexity classes... and I guess I should say for listeners who are interested, there's [a blog post](https://www.alignmentforum.org/posts/PJLABqQ962hZEqhdB/debate-update-obfuscated-arguments-problem#Previous_problems_and_solutions) that you have where you lay out, at least at one point in time, just what all the rules of the debate are and what they try to solve. I would encourage people to go read that. Speaking of complexity theory, when you read explanations of this kind of thing, I think a lot of the time, you'll see these claims like, "This debate protocol, the optimal strategy's in the class of non-deterministic exponential time" or "It implements a Merlin-Arthur protocol." It's clear that complexity theory seems to have an influence on how these debates are designed.

**Daniel Filan:**
One question I have is, they're often written in the sense that it's great that this debate strategy, this debate thing solves everything in polynomial space, and that's a ton of questions, and that's good. Where polynomial space is both a specific claim about some protocol and also just the style of argument where "polynomial space" you sub in for some big class of questions. And a thing I always think about when reading those claims, is the thing about polynomial space or non-deterministic exponential time, or like Merlin-Arthur protocols, is it's widely believed that these include a ton of intractable problems. Does that mean that we won't be able to efficiently train agents that do well in these debates?

**Beth Barnes:**
Yeah. There's a bunch of stuff going on. I'll probably say a few different things to unpack that. Firstly, the original claim is something like, for any problems where both the debaters know the answer and the problem is in NEXP or non-deterministic exponential time or polynomial space, or whatever, optimal play in debate is to give the correct answer.

**Daniel Filan:**
Oh, okay.

**Beth Barnes:**
But basically, you're completely right. This is now something that I am more worried about or something. Not to imply that Geoffrey and co had not thought about this initially or something, but there is some problem where the formal version of the proof assumes that the debaters are computationally unbounded. But in fact, obviously that's a totally unreasonable assumption.

**Beth Barnes:**
The actual model that's going on is more like, these are some very clever ML models that probably have some clever tricks or intuitions or whatever, that allow them to solve some problems where the only algorithm that we can understand is non-deterministic exponential time. We clearly can't assume that our models can solve arbitrary problems in NEXP. I think in fact, this does cause a bunch of problems. This is obviously true in theory or something. I don't know. If one debater is like, "RSA-2048 doesn't have any factors."

**Daniel Filan:**
What's RSA-2048?

**Beth Barnes:**
A large, is pseudoprime the right word? [the right word is semi-prime] It has two very big prime factors and it's very hard to find out what they are. If one debater says it doesn't have any factors, because, look, my opponent can't tell you what the factors are, so clearly it doesn't have any. Obviously, debate doesn't actually work here because even if our ML models are smart and can sometimes solve problems that look intractable to us, they can't arbitrarily solve hard computational problems.

**Daniel Filan:**
Although, in that specific case, we can efficiently determine whether or not numbers are prime, right?

**Beth Barnes:**
Right.

**Daniel Filan:**
We just can't tell you the factors.

**Beth Barnes:**
Yes. If the debater was able to respond like, "Oh, well this primality test shows that it's composite. Here's the proof of correctness for the primality test."... But then you have to know that... So then the problem is, what if the other debater says, "Oh, well, I know that there's a mistake in your primality test correctness proof. I just don't know where it is because the proof is really long." If the proof is obviously actually short... In this case, the proof is not going to be that long and it's going to be kind of obvious that finding a mistake in the proof is a lot easier than finding the fact is. But you have this problem in general where both debaters can be like, "Oh, I know that there's a flaw in your argument, but it's computationally intractable for me to find the flaw, which is why I can't show it."

**Daniel Filan:**
Okay. There's [another post](https://www.alignmentforum.org/posts/PJLABqQ962hZEqhdB/debate-update-obfuscated-arguments-problem) that you have on something that you call the obfuscated arguments problem. Is that basically the obfuscated arguments problem?

**Beth Barnes:**
Yes. So again, this thing about the optimal strategy, sometimes being computationally intractable, it's something that was in the original debate paper. The claim we're making is in fact, the dishonest debater can construct arguments, such that finding the flaw in them is intractable, but that they appear to support the dishonest answer.

**Beth Barnes:**
And it's not easy to just show that these are weird, deliberately obfuscated arguments, and that you can do this. It doesn't have to look like some cryptographic thing where you're asking for a known computationally intractable thing. It can just be like, they would just make a bunch of confusing claims about physics and the honest debater doesn't know which one in particular is wrong and you can keep supporting them with claims, where there has to be some mistakes somewhere, but it's not clear where.

**Daniel Filan:**
Okay. Yeah. The question is, what do we do about this problem?

**Beth Barnes:**
Yeah. So to be clear, we haven't robustly experimentally shown that you can in fact do this. I think we came up with a bunch of these arguments, eyeballed them and were like, yes, it seems like in general, if you practiced this, you could be good at generating these ones. Particularly, maybe if it's some debate about mechanics, I can be like, "Huh, that looks a bit fishy. I think, aren't energy arguments generally simpler than mechanical arguments," or something. If it was something about quantum physics, I'd be like, "I have no idea what reasonable argument strategies are in this domain I know nothing about." So, what was I saying? So I don't think we've particularly firmly established this is true or established in a way that other people should necessarily believe, but I feel pessimistic about it or something.

**Beth Barnes:**
I think in fact, like where I am currently at is, this does make debate significantly less powerful than we were hoping. We're going to need another piece in order to know everything the model knows. In particular, like I was saying before, you have this problem - say your model knows something, and the reason why it knows it, is it had some heuristic that was reasonable a priori and all over training, that heuristic worked well, and it was gradually upregulated. That isn't the right word, but you know. And now it just has that as an intuition. How are you going to justify that in a debate? Well, technically, the classic debate approach, the debate is "It worked this well on the first half of the training data, and this well on the second half of the training data, and if one of those is wrong, challenge it."

**Beth Barnes:**
In practice, the debaters, this is another one of those computationally intractable things. The debaters aren't going to remember exactly how well it did on exactly which bits of the training data. They're going to have thrown all that information away if they're being reasonably efficient. It's completely unreasonable to expect them to do optimal play on this debate about how exactly well did this heuristic do on all of the data points you've ever seen. So we're going to need some way of verifying those kinds of claims, that is less unreasonably demanding of our debaters.

**Daniel Filan:**
Okay. So is the research strategy, think of types of claims that debate maybe can't deal with and then come up with protocols for that?

**Beth Barnes:**
Yeah. That's a lot of what... The earlier work I was doing was figuring out, how to set up these experiments and what a reasonable debate protocol would look like. And then being like, okay, we tried these debates, they didn't work for this reason, can we tweak the mechanism to fix it? It was a bunch of being like, is this something we can fix with a tweak of the mechanism, or is this a fundamental problem?

**Beth Barnes:**
We ended up on it... There were a bunch of things, like the ambiguity, or not focusing on the right thing that, we think we just fixed by changing the mechanism, and then this is a class of things that can't be fixed by changing the mechanism that we're going to need a completely different approach to deal with this. And so Paul has some ideas in this direction, which have just variously been known as "learning the prior" or "imitative generalization".

**Daniel Filan:**
Cool. I'm going to talk about that a bit later. I still have more questions about debate. So one thing I noticed is that it seems like... This question has a specific and a general version. Here's the specific version. The thing you mentioned about, sometimes your reason for believing something is well, it's just a heuristic that worked well, in my experience. It did pretty well, and no other heuristic did better. I think that comes up with human arguments, right?

**Daniel Filan:**
I have the sense that sometimes when my dad is telling me something, that's his reason. And in general, the task of coming up with ways of structuring debate that promote the truth seems pretty relevant to humans that want to use discourse to discover the truth, right? So I'm wondering, both in that case and in general, to what extent do you think thinking about debate in this AI context has helped you think about, as humans who are trying to have rational discourse or something, how can we structure that well?

**Beth Barnes:**
Yeah. I think I haven't in fact thought about this that much, particularly because once we're getting towards these more exotic things involving making copies of people and all that kind of thing, it's not very practical or something.

**Daniel Filan:**
Not yet.

**Beth Barnes:**
Yes, indeed. I think the other thing is in fact, all the stuff that can only be justified by intuition and things. I think actually in human debate, you do get this thing of, a lot of things that come up in human debate are just based on intuitions or it's opaque to the human why they know it. Some of this is a fundamental problem that's shared with ML debate. I think you might hope that in ML debate, you can do better because these models are being trained to reason in a way that's transparent to them and to humans.

**Daniel Filan:**
How are they being trained that way?

**Beth Barnes:**
As in, because that's what debate incentivizes. It incentivizes being able to explain your reasoning in a convincing way. Maybe there's various adaptations where humans just throw away their reasoning, once they're done with it. It might be relatively easy for our cognition to be more transparent to us, but it just happens not to be, because that hasn't happened to be particularly useful. But if we were being optimized for that, it might be better. I think there's definitely interesting stuff here, and I think that the idea of making it clear how your arguments splits up into parts and then having some kind of mechanism where you choose which part to focus on, that makes use of the fact that the people doing the debate might have more idea of which part is productive to focus on than the listener is good.

**Beth Barnes:**
There's also another difference which is, if you're thinking about how to have truth conducive debates between people who are already trying to be truth conducive and trying to reach agreement, you don't need these mechanisms so much. And in places where people aren't trying to be truth conducive, you're not going to be able to get them to agree, to use the mechanism. For political debates or something, if you're like, "Oh yes, we think that if instead of having a televised debate where you just talk at each other, you will do this game. There's this interaction with all these rules and some panel of judges will judge all the individual parts of it, and then there'll be an answer about who's the winner."

**Beth Barnes:**
Probably, people aren't going to want to do that. The other reason why it would be hard in that context is, I think like you said before, an individual debate is not, at least how we've been thinking of it, an individual debate transcript should not necessarily be convincing to the judge. It's only convincing if you assume optimal play everywhere else. It's not like you see this transcript and you read the argument, and you're like, "Oh, I understand. I see why this answer is correct." You're like, "Okay, if I assume that all the unchallenged answers were in fact correct, then I see that this thing's correct."

**Daniel Filan:**
In terms of the political example, or the question of, well, you're assuming that you have... It's only useful if one of the players is not trying to be honest. It could be the case that there are some contexts where you have participants in a discussion, not all of them are trying to be honest, but they're trying to appear that they're trying to be honest. It seems like that should come up at least sometimes in human discussion. Right?

**Beth Barnes:**
Yeah. I think that's right. And again, hopefully, the context in which it should be most useful is if you have several experts who disagree, and maybe some of them are being dishonest or are a fraud, and you don't know anything about the field. But you just want to know the true answer. You have some control over... They're competing for your... You want to pay one of them to do something for you and you want to figure out who's actually good, or who's actually correct. And then you can say to them, "Instead of just having a discussion in front of me, I want you to play this game."

**Daniel Filan:**
Okay. So sort of related to this. So one question I have is it seems that a key feature of the debate structure is that you have these very unambiguous claims that are either definitely true or definitely false. And it's both the case that that's what you're debating and it's the case that those are the sub-claims being brought up. I'm wondering, do you think this covers much of the space of what we're going to know from models? Sorry, what we're going to want to know from big advanced machine learning models. And do you worry that it's going to make it hard to discuss things that you only partially understand, but you can still make valid arguments about? I guess this might be the same thing as the heuristic thing.

**Beth Barnes:**
I think there's some different things to say about this. And this isn't exactly what you're asking, but in fact, we ran into some trouble with having this sort of where we're thinking about it being the debate is making claims and those claims are either true or false, just because in fact true or false is kind of actually not that clearly defined. The classic example being "the king of France is bald". Is this claim true or false or not given that France doesn't have a king?

**Daniel Filan:**
It seems false, right? It's definitely not true, right?

**Beth Barnes:**
But it's...

**Daniel Filan:**
Maybe like the king of France does not have hair.

**Beth Barnes:**
If you formalize it, it's kind of like exists king of France and king of France is bald or something. Well, I don't know. Anyway, this isn't always as clear as you might hope. So we instead move to... The framing is there is a question and the debaters give two answers. And what the judge is trying to say is which is the better answer or here's one of these answers or are they indistinguishable? Suddenly if the debaters themselves only have kind of fuzzy knowledge, they can say this is probably something, but it might be this other thing and that can be a better answer than some confident claim if you can show that the confident claim is overconfident. I don't know if that answers your question. I guess a kind of related thing, which you might've been asking about is can you use this for claims that have a moral component or a component that's just about someone's personal preferences, not just about things that seem like objective facts about the world or something?

**Daniel Filan:**
Okay. What if I asked that question?

**Beth Barnes:**
Okay. So yeah. Well, one thing is actually, once you start saying, "What is a better answer to this question?" then it does end up kind of being about the judge's preferences or you end up getting into ethics pretty quickly or something. Because part of one of the debaters' arguments could be, "This answer is better because it will cause you to do more ethical things in the future". 'Better' ends up getting into ethics. I think there's an interesting phenomenon that happens in policy debate. So this crazy thing that Americans do where they talk very fast. It's like, I guess, high school or a university debate with a particular set of rules that has these very elaborate strategies and yet also involves people talking at ridiculous speeds. And one of the things that tends to happen, and there's a term for it which I can't remember. One of the debate teams chooses a strategy of saying, not necessarily trying to argue that some answer is factually true, but saying that the judges are ethically obliged to pick their team for some reason or something.

**Beth Barnes:**
There was some example about something to do with... I think there was some question where one of the debate teams' answer was that policy debate would be perceived as more fun and engaging if this answer was favored and policy debate being fun and engaging was beneficial because it was beneficial to students in all these ways or something like that. So the debate ended up being much more about appeals to the morals of the judges and the consequences on the world that them choosing one answer or another would have. So I think in some sense, even if you try and avoid this, debate tends to drift in that direction because it's ultimately what you are trying to do is persuade a judge to choose one action over another and where one action is picking one answer and the other action is picking the other and-

**Daniel Filan:**
I feel like listening to this with a rationalist bent might be thinking, "Okay, why can't we just solve this by saying the way you judge debates is which answer corresponds more closely to reality?"

**Beth Barnes:**
So then you're weighing the rules against... You could still appeal to someone's moral obligations or something. Suppose you're a contractor employed as a judge in some game and you have a certain set of instructions to follow, but the debaters say, "If you pick this answer, some bad thing will happen in the world." And they persuade you of that. Ultimately if you're trying to make someone make a decision and what you're going to appeal to is the way they make decisions, which is sort of related to their ethics. And so I think you can try and have rules to rule this out, but I also think you don't actually want to because if you do that then what are you going to do about any questions that are about what should I do? Your rationalist would say, "Well, that's not a question that has a meaningful answer. There aren't answers that corresponds more or less to reality." or something.

**Daniel Filan:**
I don't know. One response would be that, another response would be they're just facts about what you should and shouldn't do and the way you should answer the question is to correspond well with the facts about what you should and shouldn't do.

**Beth Barnes:**
Yeah. I don't know. It seems tricky. Or at least where I'm currently at is you have to accept that things are going to come down to what are the consequences of me choosing this answer versus this answer? Okay, so here's another reason why that's difficult. Something could correspond more closely to the facts, but be really unhelpful for some other question that you were trying to answer.

**Daniel Filan:**
Maybe an example is... So a fact about myself is that I was born in Australia and I was raised in Australia. I have an Australian citizenship, but I live in the United States and probably the people who I am most similar to in the world, a lot of them are Americans. I sound kind of like an American. And so perhaps an example, you can tell me if this is wrong or not, but perhaps an example would be you're asking the question, you want to figure out part of my deal. "Does Daniel live in America?" And you ask, "Is Daniel an Australian?" And the answer is "Well, yes, but that's not the important thing for what you really care about." Is that, kind of a weird example, but is that an example?

**Beth Barnes:**
Yeah. I guess the more general claim is just that what is a good answer to this question depends on the context in which it's being asked, it doesn't... Yeah.

**Daniel Filan:**
And even formally, degree of closeness of correspondence to reality depends on some metric, right?

**Beth Barnes:**
Yeah. I don't know. If you asked some question, say you're like, I don't know, "Am I sitting still?" or something. I don't know, in some mundane sense you are and in some other mundane sense, I'm moving through space really fast and in some other sense, there isn't an answer to that question because it depends on the reference frame.

**Daniel Filan:**
In some sense I'm twitching a tiny bit?

**Beth Barnes:**
Right. So I think it's easier to follow a rule, which is what is a better answer to that question? Which includes how will that question in context help you make decisions and figure things out rather than, "Oh, does it technically correspond to reality?" or something like that.

**Daniel Filan:**
Okay. So I think I'd like to move on a little bit. So you mentioned that one problem that debate has is it's bad at revealing facts that are based on heuristics, which have worked well in the past and we're going to apply now. And you mentioned that there was this approach called intuitive [sic] generalization that was maybe good for this. Can you talk about that?

**Beth Barnes:**
Actually, I had one more thing to say about the previous thing, if that's all right.

**Daniel Filan:**
Yeah, definitely.

**Beth Barnes:**
On the subject of you have some human who has been given some instructions and told to follow them, but the debaters are just trying to persuade them to do one action over the other. I think there's definitely some concern for when you actually run debate here. I think in the experiments and theoretical thinking we've been doing, we've basically been assuming, "Oh, just assume that the humans will just correctly follow the instructions as written." But I think related to the thing of debaters hacking humans or persuading them or something. I think even if it's not that they just wrongly convince them of something, you might get something like they convince them to not follow the rules and then everything else breaks down. I guess we hope that that debaters won't be able to hack humans or convince them because it's like, "Oh, they can only make these relatively short utterances." And we're doing all of these checks and stuff to make sure that any sort of suspicious kind of persuasive stuff doesn't get passed on.

**Beth Barnes:**
But if they can persuade the humans to not follow the rules, then you're kind of in trouble or something. Like one thing Evan and I were thinking about, it's a kind of silly example, is your debater sort of breaks the fourth wall type of thing and rather than talking about the subject at hand, tells the human that they'll suffer horribly if they don't pick their answer or something like that. So I think that's something we haven't thought much and it's something that I think we are going to need to think about more with lots of different alignment techniques. Like how does this actually work with real humans in practice? And they're not going to follow the instructions perfectly and they could be persuaded that following the instructions is a bad idea.

**Daniel Filan:**
Yeah. And that Evan is actually Evan Hubinger who appeared on episode four of this podcast. So it all links together. Yeah. That was interesting. I think you mentioned that intuitive... That there was some problem with, well, you just have some heuristic that works well and that's why you believe a thing, but it's sort of hard to give an argument other than, "Well, this heuristic has just worked really well." And I believe you've mentioned that there's this approach called intuitive generalization-

**Beth Barnes:**
Imitative generalization.

**Daniel Filan:**
Imitative...

**Beth Barnes:**
We've been through a lot of names and none of them are good. Sorry.

**Daniel Filan:**
No, I think the problem is I was thinking of the word intuition and my handwriting is messy. But imitative generalization, could you explain what that is?

**Beth Barnes:**
Yeah. So I think the easiest framing to go at this from is via debate given that we've been talking about that already. So as I mentioned before, you can imagine one debater is like, "Well, I just know that breaking down this physics problem in terms of conservation of energy is more likely to get the right answer than this approach you're suggesting where we break it down in terms of all of the forces. And I don't have a logical argument of why that's true, it's just I've tried a bunch of problems and that's what happened to work better." And the other debater is like, "Oh, well, I don't believe you. I've tried a bunch of problems and the forces approach has worked better."

**Beth Barnes:**
And then, well, how are the debaters supposed to justify that? And the standard debate approach would be like, "Oh, well, they can claim that it worked well in the first half of the examples and it worked well in the second half of the examples and if I'm lying, then you can challenge one of these", but this is impractical. So what imitative generalization does is rather than having to go over every single training example every time one of these kinds of claims comes up, you just do one pass over all your data and you get out an object that captures all of that knowledge and an amplified human - so a human amplified using IDA or a human interacting with the debate - is able to interact with that object and use all of the information.

**Beth Barnes:**
So what this algorithm actually looks like... First of all, let's say the version that if you had as many humans as you wanted and you could use a bunch of their time, what you would just do is have you search over some space of hypotheses, which are sort of full distributions over everything you think about how the world is. And for each of these, you ask... You have your human-supervised debate about what the prior probability of that hypothesis should be and for each data point in your training data, you have the human supervisor debate about what is the likelihood of that training data point given the hypothesis. So then for each hypothesis, you've got a prior and likelihood score. So then you just search over as many as possible of the hypotheses in your distribution until you find the best ones. And this isn't intended to be like a MAP, a point estimate, this is intended to give you just the best distribution. But yeah, obviously, in fact, we can't-

**Daniel Filan:**
And here the hypotheses are, like you said, they're these ways of reasoning about how everything will pan out?

**Beth Barnes:**
Yeah. So like concretely, but impractically, you could think of them as just a very long text that explains a bunch of rules for how to think about things or ways the world is. Or it could be various physical constants and laws of physics and principles that are useful for understanding history and things like this that don't have any explanation behind them but are just what fits the data. And then if you imagine this giant wall of text, obviously that's too long for the human to read and engage with all of it. So that's why you have something like debate. So for any given hypothesis, you'd have one debater who's like, "Oh, this part should mean the prior is lower because this is unreasonable for this reason." or something like that.

**Daniel Filan:**
So you told us how we could kind of do this if you have as many humans as you want with as much time as you want?

**Beth Barnes:**
Yeah. I realized that is not in fact what exactly I gave you because once we've introduced debate, we can say the same things we said about debate before, which is... One thing I said before is you don't in fact have to use the human every time. You can just train a reward model from the human and then do your debates against that reward model. And the other thing is once you've trained your debaters, you don't have to do whole debates, you just ask them to answer. You make them think they're about to do a debate, you ask them for an answer and they give the sort of answer that they could support in a debate, which is hopefully truthful. Because I introduced debate already. So you don't actually need...

**Daniel Filan:**
Yeah. But that still seems like you're doing this process over every big, long string of - big, long, wall of text, right? And there are lots of big-

**Beth Barnes:**
Yes.

**Daniel Filan:**
... possible treatises.

**Beth Barnes:**
There are a lot of difficulties with this. So to be clear, I don't think we have anything in mind here that actually seems like it would work. It just seems like the right sort of direction or you can describe versions of it that are completely impractical that would sort of do the right thing. And so to make this practical, you'd have to have some reasonable way to represent these hypotheses such that the humans can still interact with them, but they're feasible to optimize or feasible to explore the space. So you might also want to do something like you have humans demonstrate, modifying the hypothesis to make it more likely or modifying it to make it support the data better and then train your ML to imitate that human exploration process.

**Beth Barnes:**
There's various other things you could fool around doing with trying to make this actually tractable. And it's not clear whether that'll all work out or whether one of the modifications that you'd need to make will actually break everything. And the core hard problem does seem to be something about how do you represent something that is competitive with representing it as a big neural net in terms of ease of optimization, but still has this property where the human can actually engage with it and understand what it means?

**Daniel Filan:**
Yeah. So is the idea that as the human, if somebody's saying, "Oh, here's a heuristic that I've learned that has worked really well in the past and it's going to serve me well in the future and it says this." Is the idea that the human is going to check, "Oh, that's actually a crazy heuristic." or, "Oh, that doesn't really support what you say it supports." or, "That didn't really work well with the past"? And in a way that we couldn't just do mechanistically?

**Beth Barnes:**
I guess I'm not quite sure what your question is, but-

**Daniel Filan:**
Yeah. I guess the question is what's the benefit of this over just machine learning?

**Beth Barnes:**
The other way to approach why you would need this is basically machine learning generalization is kind of scary. We have no particular reason to believe that implicit in ML architectures is a good prior. And in particular, the worry is something that performs really well on your training data is an agent that reasons about the process it's embedded in and reasons about how to get a high score on the training data and then who knows what that does later on. And we think there's not that good a reason to believe that the ML prior would favor something that's directly a model of the world that will continue giving you sensible answers and is doing what you expect over something that's reasoning about the training process, especially because if you ever have mistakes or quirks in your training data, you are going to favor the thing that's an agent reasoning about the training process because it'll get those things right versus something that's trying to be an honest model of the world because it will get those things wrong.

**Beth Barnes:**
And in particular, we're always going to have to do this generalization between questions that we know the answers to or questions that we can sort of produce training data for versus questions that we can't supervise at all. So the way something like imitative generalization is helping with this is you are using the human prior rather than the neural net prior. So you only ever use ML generalization for things that are basically in the same distribution or you can check all of your ML generalizations and the sort of uncheckable generalization is informed by the human prior.

**Beth Barnes:**
This is kind of - it's confusing to explain. Sorry. Okay. So just to give a sense of the algorithm. Because I think this doesn't really give a sense of why it's useful - well, not that much anyway - to give a sense of the algorithm and why it helps with generalization, imagine that you're doing every ML researcher's favorite task, which is labeling pictures of dogs. And you have some training set that's a bunch of breeds of dogs that are labeled, and a test set where you don't have any labels but you want to label the dogs correctly. And further, imagine that you have various human contractors but they don't know anything about dog breeds and for whatever reason, there aren't any books or things to learn about dog breeds. All you have is just this one labeled training set.

**Beth Barnes:**
The standard ML approach is just train your model on the training set and then just run it on the test set and hope for the best. But we know that there are various things about the ML prior that can make this go wrong. For example, if in your training set, all of your pictures of huskies were on a snowy background, maybe if in your test set there's a picture of a husky on grass, your model thinks it's not the husky. So you could think about this as your neural net prior doesn't favor the hypothesis "husky is a big, fluffy dog that's kind of like a wolf" over "a husky is an image with a lot of white and blue pixels, has a lot of snow in it". So what imitative generalization would do here is... So your hypothesis space is a sort of instruction book about how to label the images, which could contain different possible instructions like if there is snow, label it as husky, or if the dog is gray and fluffy with pointy ears and kind of wolf-like, label it as husky.

**Daniel Filan:**
Okay. And really the hypothesis space is any of these books is one hypothesis and the space is all of the possible books?

**Beth Barnes:**
Yes. So yeah. All of the possible instruction sets for what dog breeds look like. So then you give these instructions to your humans and get them to label the dogs in the training set and see how well they do. And you optimize the instructions to make the humans do well at labeling the dogs where you know what the correct label is. But this wouldn't give you a distinction between the reasonable description of the husky and the description of the husky as an image with snow. So where you get that is you also give these instructions to the humans and ask them to say a priori whether this is reasonable. And for the purposes of this example, we assume that humans vaguely know what animals and dogs are, but don't know specific dog breeds.

**Beth Barnes:**
So then the humans would say that... Basically they'd favor any description of husky which is about characteristics of the dog as more likely than the description that a husky is an image with lots of snow in it. So then you optimize the guidebook. Hopefully you get one that has sensible and correct descriptions of how to label dogs. And then can give that to the humans and have them label your test set and then you train your ML model to imitate the result of the humans labeling the test set and now you can make arbitrary predictions on your test set and you can always check them with the humans if you want. And you have on distribution labels for the task you will do.

**Daniel Filan:**
Like you know you've moved to a new distribution and then you get human labels on that distribution-

**Beth Barnes:**
Yeah.

**Daniel Filan:**
... and can sort of generalize from the human labels that were generated from learning on the previous thing.

**Beth Barnes:**
Yes. Yeah. Yeah.

**Daniel Filan:**
So bringing that back to the debate context, right? Am I right that one example of where you might want to use this is, one debater says it's going to be easier and better to use a breakdown of the energies in this problem to solve this physics thing. Another debater says it's actually going to be easier and better to do the breakdown of the forces. Is the idea that using imitative generalization, the reason that's going to be good is that we're going to have a good human prior over which one of those is better?

**Beth Barnes:**
The reason this is hard in debate is partly just because it's sort of intractable to do within a debate. You could still imagine having a debate about whether using an argument based on forces is better a priori or using an argument based on energy conservation is better a priori. So the difference from debate isn't necessarily about the prior, but the difference from standard ML and the difference from debate is just that it's more efficient and more of a reasonable set up. So if you did the IG [imitative generalization] thing with debate, instead of every time you have a question that relates to something that's justified by intuition from all the data, instead of having to go over your data again, you just look at the hypothesis that IG produces and what does this say about that? So in this case, you'd look at the object you've got and be, "What does it say about when you should use arguments? For what sort of physics questions are arguments based on forces more likely to give the correct answer than arguments based on energy?"

**Daniel Filan:**
Okay. So you would sort augment debate with we're going to use imitative generalization, just on the side on a ton of stuff and we're going to use that to inform some kind of heuristic questions?

**Beth Barnes:**
Yeah. I usually in fact more think of it as like using debate on top of imitative generalization as in then...

**Daniel Filan:**
As opposed to the other way around.

**Beth Barnes:**
I don't think it matters that much, but what you think of as... you get this object out of imitative generalization that encodes everything a reasonable ML model could have learned from this dataset and that's interpretable to humans and is a reasonable sort of model. It's not an agent that's just trying to tell you the right thing. Sorry, trying to tell you what you want to hear. And then, you can have debates that bottom out, either in some sort of reasoning of "this implies that", or they bottom out in "stuff we learned from data says this".

**Daniel Filan:**
Okay. And am I right in thinking that this is sort of a line of work to make something like imitative generalization work? Or is it now we basically know what it's going to look like?

**Beth Barnes:**
Oh yeah. I think we really don't know. I think Paul is vaguely optimistic that something will work, but it definitely doesn't feel like we have something sort of reasonable yet. And I think I can say a bit more about the difficulties and what I think the hard core is.

**Daniel Filan:**
Yeah. Two that jump out are, it seems like there are a lot of long treatises that you have to deal with and... In my head that was two difficulties. But I guess that's just one.

**Beth Barnes:**
Yeah. So I think one other way to see, why this is kind of tricky is maybe one hypothesis that does really well on the training set is just copy the output of this big neural net.

**Daniel Filan:**
Yep.

**Beth Barnes:**
That does great. It gets all the answers right. But then you're now back in the same place as before, is we've just got to trust this - when we run that on the test set, will it give us reasonable answers? We have no idea. So you need the human prior to downweight "just believe this big neural network". But then it's kind of unclear - So we want something like, if there is any sort of additional structure that could be pulled out of that neural net and exposed to the human, the hypothesis that instead does that will beat the one that's like "just trust this big black box."

**Beth Barnes:**
Which seems sort of doable, but it's very confusing. How do you represent things in a way that gives the opportunity for that? But we also - if you imagine representing everything that AlphaFold knows in text. That's just going to be horribly inefficient to try and optimize that and get the humans to do it. And also the human's going to have no idea of what's going on.

**Daniel Filan:**
Yeah.

**Beth Barnes:**
How do you represent that session in such a way that the human has meaningful understanding and it's reasonably efficient? And this I think ends up being pretty close to the sort of hard problems of interpretability., and like Chris Olah's ideas about microscope AI. Like the original statement of this sort of problem, or what we want to do, where Chris says, one way to have capabilities of AI safely is, if we're able to do sufficiently good interpretability that we just look inside the model and we see all the things that it knows, all of the things it extracted and understood from the world, and then we could just use that knowledge to do stuff that we think is important. And obviously the difficulty there is how are we going to understand what it is the model knows? Or how are we going to come up with interpretability techniques that turn a gazillion weights into "Ah, now I have this insight about the world."

**Beth Barnes:**
And one way you can imagine setting up IG, imitative generalization, and the way you could represent the hypotheses is your hypothesis is a neural net with a bunch of annotations and you optimize the neural net weights and the annotations jointly such that that combination of weights and annotations is plausible to the human. And when the human assumes that the annotations and the net are correct, they use it to make good predictions. This then ends up looking quite a lot like you train a model for interpretability and your standard for whether something is a correct interpretation of what the model knows is does it cause the human to get correct answers on the training set?

**Beth Barnes:**
And you can imagine something going on with the annotations, looking a bit like [circuits](https://distill.pub/2020/circuits/) that Chris Olah and co have produced for models we have now. Which is the human would look at this annotation - in the husky case, they'd see "here's the bit that's a fur detector. And this feeds into this pointy edge detector. And here's the fur detector and the curved line detector, which detects a tail or something." And if they saw that the husky detector didn't use any of those features and just used a snow detector, they'd think that was suspicious. That isn't how it should be structured. And the reason you'd think that the annotations you end up with when you optimize do correctly reflect the knowledge is well, if you've labeled something as a pointy ear detector versus a floppy ear detector but it's actually a boat detector versus an airplane detector or something, this will not make your humans get the right answer when they're trying to label dogs.

**Daniel Filan:**
Okay. I guess that actually gets to another question I had. Supposed it's the case that - you're a listener, right? So some listeners to this are thinking "man, this line of work is pretty exciting", pretty pumped for it, but for whatever reason, this listener thinks, "I don't want to do it directly. I don't think that's something that's for me". But maybe there are complements, things people could do that aren't exactly working on this, but are really useful for working on this. Yeah. I'm wondering, what do you think the complements of your work are?

**Beth Barnes:**
Yeah. To be clear, I'm not exactly working on this either. I'm currently just doing miscellaneous stuff, but yes. Really excited about people just doing a bunch more interpretability. There are a bunch of benefits to this and several different ways that it ties into both this stuff in particular and just AI safety in general. As I was saying, it seems like particularly Chris Olah-style microscope AI type interpretability is trying to solve the same more specific problem that we're kind of running into here. Yeah. They're focusing on the same thing of how do we represent knowledge in a way that a human - maybe an assisted human, either assisted with debate and whatever interpretability tools we can build and just whatever else we can build to help the human, such that they can actually engage with stuff the model knows. And I think more broadly, I am just excited about more interpretability. So I think that on the margin, just doing more interpretability and raising the bar or the kind of expectation of how much we understand our models before we deploy them and do stuff with them would be really good. Because I think it's just going to be every bit more that we know about our models and have some insight into what they're doing is going to be helpful to safety, I think. I guess sometimes I think there's some kind of window for a treacherous turn where there's the time between when a model is smart enough to think about deception and when it's good enough to get away with it.

**Beth Barnes:**
And the more interpretability you do, you increase your probability that you'll be able to see that your model is thinking about this kind of thing, or considering doing things you don't like rather than having to see it actually happen in practice or something. Which pushes out, it's going to be a lot harder to make it invisible that you're thinking about deception, as opposed to avoid trying to take over stuff or avoid behaving in the way that's obviously bad. I guess also there's the way this ties into the sort of Paul-style alignment, where we want to have a story of how are we going to know everything the model knows. And I think even if you think that's just way too demanding and we're never going to be able to do anything like that, it's still useful to have a bit more interpretability.

**Beth Barnes:**
Even if we can just do very crude things like "Is this model just thinking about some category of stuff it really shouldn't be thinking about" or "Is this structured in a way that suggests we've got an agent or it's doing a bunch of search and planning, where that wasn't what we wanted" or even, "We know we've got something that's an agent and we want to try and take parts of it that we can use without using the whole agent. Can we sort of figure out which bits are the world model and which are the agent and do some very crude kind of lobotomy model surgery". I just think even having a vague idea of which bits are doing what. I think this - this idea is due to Richard Ngo by the way.

**Beth Barnes:**
It is interesting that in humans, in fact it does seem like frontal lobe lobotomies actually do do the thing of mostly removing agency and mostly preserving world knowledge. I don't know very much about this and would be interested in someone looking into that more in particular. Whether lobotomy patients who are extremely passive can still model or understand the sort of agency or actions of others. That is very much a tangent. But anyway, so one of the things I'm excited about is interpretability for various reasons, but, I think it also particularly ties close to this. What are other things I'm excited about people doing? I am excited about people just generally building more of the infrastructure for having these very human-in-the-loop training procedures. So if we are going to want something where we have a large number of humans who are carefully and correctly following some moderately complicated instructions, and we need them to make a judgement and we maybe also need them to be reflective of broad values in some reasonable sense.

**Beth Barnes:**
What are all the logistical challenges of doing that? You could do several iterations of trying to get humans to follow instructions while they interact with something that tries to persuade them not to or something like that. What do you have to do to be confident that you humans will actually follow the instructions you've given, or how are you going to set up layers of oversight and monitoring such that you can make sure your large number of humans are doing the correct thing. Something else I've been thinking about recently is how does this work if you do, if you want your humans - in the debate example, it'd be the debate judges - to reflect broad values. How would you do that?

**Beth Barnes:**
And how much convergence is there between people who have different object level ideas about morality? Do they agree more about higher-level principles, like what is good reasoning about ethics, or who are the right sort of people to trust, or what other sort of things you do to get a better answer to an ethical question? I feel like this is something maybe people who think about coherent extrapolated volition assume that people pretty widely agree that you will make a better ethical decision if you'd thought about it for longer and had more information. But I'm not quite so sure whether that's true across all cultures or whether people would say something like "it's more valuable if you do it just based on faith" or something, or "the first thing you should do is to lock in your faith and make sure you don't get corrupted or tempted away from it by arguments."

**Beth Barnes:**
On the other hand, I think it could be useful for reducing races, or a sense of people wanting their side to develop AGI first if we can show that people in fact do converge and agree a lot. Or if you can construct some kind of example system, here's this thing kind of works and it does seem like there are a bunch of ways people can sort of converge and that it would reasonably represent everyone's interests in the way they're happy with.

**Daniel Filan:**
Okay, cool.

**Beth Barnes:**
Yeah. So I feel like the obfuscated argument post is we kind of took that work to a point where we eyeballed it and were like "we're sufficiently convinced that this is a limitation that we're going to move on to some other things". But I think it's really not very solid.

**Beth Barnes:**
It would be really nice for someone to both try to experimentally validate that obfuscated arguments exist and that people can't distinguish obfuscated decompositions from honest decompositions and try that. Although I do think a negative result with humans might not be that convincing because it seems like maybe humans are bad at coming up with obfuscated arguments because that's not usually how they think, but if you train them all to do debate, they'll get really good at coming up with obfuscated arguments. But anyway, I think it would be worth investigating that a bit more. And also there's some theoretical CS proofs that turned out to be a bit of a headache about Merlin-Arthur protocols being equivalent to the formal version of debate in a certain set up that, I wish I had been able to do well but kind of give up on, but I can imagine someone who had the right background might be able to just do that much more nicely because I think we are still not quite sure whether we've got all of that right.

**Beth Barnes:**
It's relatively straight forward in the case of where we're talking about a big argument tree, the argument is all conjunctive, and the question is whether there's a flaw somewhere and then if the debaters have no idea where the flaw is, it's relatively obvious there that you could have a different protocol which is just, you only have one model, and you just have your other model just randomly choose which branch to take such that if your previous model was only choosing randomly anyway then those are obviously equivalent, but when you have things like the argument isn't actually conjunctive, parts of it are disjunctive. It's a bit less clear what the sort of simple equivalent is. And there's lots of stuff there for people who have been interested in things that like..

**Daniel Filan:**
So in terms of this like general approach. How... so I sort of see this as saying we're going to come up with debate. We're going to find some flaws with debate. We're going to come up with a new thing to fix the flaws. So there's imitative generalization, which is an idea for fixing one of the flaws. How many other fixes do you think there are left that need to be come up with?

**Beth Barnes:**
There's maybe kind of a few different categories of things. It's like the thing I was talking about before, which is actually getting the logistics of all of this training process set up and making sure that humans are doing what you expect them to do is a whole thing. That's going to be tricky, and there's various tweaks to the mechanism and the structure to make it better.

**Beth Barnes:**
I don't know. Maybe I'm being over confident and too inside-view here, it does feel like maybe there aren't additional problems that are as big and of a similar type to this knowledge that's only justified via intuition. It feels plausible that the pieces of IDA slash debate plus imitative generalization, capture all of the ways of knowing things, but there could easily be something else. I'm not that confident. It doesn't feel like there are going to be loads more pieces of that size, I think. I guess something else that I'll just say here, because I haven't mentioned it yet and it's maybe kind of important, is just I, and Paul I think, mostly don't think that we will actually use debate to train ML systems and that the reason that it's worth thinking about, at least for me, it's just easier to understand and think about. And it's a lot more practical to do human experiments with than IDA.

**Beth Barnes:**
But has basically the same set of limitations. And I think that's kind of an analogy that, if there is a winning dishonest strategy in debate that corresponds to a problem that you might have in IDA. And if there is no winning dishonest, or no drawing dishonest strategy in debate, then your IDA should work right. So I imagine that what we're actually going to do looks a lot more like amplification, but we're just thinking about things in terms of debate. I think for what it's worth Geoffrey Irving, who's the actual original creator of debate, maybe thinks more that we will actually use something that looks like debate and doesn't see it as just being - I mean, he definitely sees it as analogous to IDA and that's how he came - he came up with it while he was trying to understand IDA I think...

**Daniel Filan:**
I'm not sure if we've explained exactly what IDA is in general. But now that we've said that, that's the thing we're actually going to do. What is IDA and what does it stand for again?

**Beth Barnes:**
Yeah. So it stands for iterated distillation and amplification. So I think I mentioned HCH before. This is the idea that one way you might solve a problem or answer a question, or represent, have a more formalized model of humans sort of thinking for a long time, or humans during all the computation they can, is having humans who give sub-tasks to other humans and then use the results of those sub-tasks to answer some part of their own task and pass that on.

**Daniel Filan:**
Yep.

**Beth Barnes:**
So IDA is a proposal for approximately how to train and how to practically train an ML system that gives the same results as this HCH tree. So with IDA... One way you can think about is first you train a model that just imitates the human doing some task.

**Beth Barnes:**
You have the human do a range of tasks. You train models to imitate that. Fine, relatively safe. Then you have the human do tasks using that model to help them do sub-tasks. So this is now a depth-2 tree, where you have a human and then they send queries to this model. So now you can train another model to imitate that. So you're training a model to imitate a depth-2 tree. And that little tree can hopefully do more than a human alone could do, because, the human's able to ask the model for help.

**Daniel Filan:**
Gotcha. And what's distillation and amplification? I can sort of see what's being iterated here.

**Beth Barnes:**
The amplification step is when you give the model to the human, to assist them or amplify their ability do stuff. And the distillation step is when you train the model to imitate that team of humans and models.

**Daniel Filan:**
Okay. So, the thought is that's what we're actually going to do, and all of these experiments about debate are just sort of to inform us on how well that works. It seemed like the detailed rules were pretty important in debate. I'm wondering, do you think that's... How are we going to transfer that knowledge to doing this IDA protocol?

**Beth Barnes:**
I think that there's some way in which the rules are basically totally irrelevant. They were just us figuring out which things are real problems and which things are deep problems and which things are contingent problems. So, we fiddle around, we find a problem, we fiddle around, we fix the rules, and then, eventually we find a problem that we can't seem to fix by fiddling around with the rules. And then we think about it and we're like "ah yes, this is a deep problem. But I think parts of the ideas and rules will still apply. My sense is this is actually not something I've thought about in that much detail, I guess. Because I've either been in the space of "okay, let's just try and make debate work concretely" or over in the space of "okay, let's think about the deep problems and, how they relate to each other."

**Beth Barnes:**
And it feels like we're generally learning useful things or something. I'm not actually sure that there'd be a direct, that many direct analogies between this particular set and stuff we do in IDA. I don't know. There's also various ways you can blend debate and IDA. So for example, if, instead of training your model to imitate this little tree, you could train your model to maximize the reward given by this little tree. So your model does some stuff gives some answers and then the human with the assistance of a previous version of that model assesses how good the answers and things were. So, you can run something that's half and half, you have a little debate and then the debate is judged by the amplified human or something like that. So, there's sort of a continuum between these, you can kind of interleave them.

**Daniel Filan:**
Okay. And one thing I'm just realizing that might be closely related to that is it seems to me - I think at the start we were like, "yeah, the point of debate is that we have a machine learning system and it knows some stuff. And we want to know, if we have a question and it knows the answer, we want to know what answer it knows." Whereas IDA to me sounds like the problem of "I'm a human, I want to solve some tasks." It has kind of a different flavor. Is the idea that when we're doing IDA, we're doing it on the human's trying to figure something out about a model, or how - can you say more about that correspondence?

**Beth Barnes:**
Yeah. So you can totally imagine that the top level question in IDA is this model has suggested this plan, is this suspicious and dodgy or is it a good plan? Same as debate. There's a sort of - you can move between planning and actions in the world, and just language and question answering. You can do questions about whether something is a good plan or whether some model should be trusted to take some sequence of actions or whatever. In that case with IDA, we want it to be able to answer every - We have some model and we're trying to get it to do things. And we also want it to tell us, correct answers to questions. And the way we know that it's telling us correct answers is that we've trained it to imitate the answers that humans get after they think about it and pass off all these sub-tasks and aggregate all these sub-tasks.

**Beth Barnes:**
So like I said before, maybe you'd have a debate about whether some model is going to be deceptive. And one debater would point out that this part of the plan would have this consequence, and this would involve the model getting lots of power and this would be bad. You might, similarly, the IDA equivalent is this implicit, equivalent HCH tree would be a bunch of humans being like "is this part of the plan reasonable? What are the possible consequences of this action, what are the possible consequences of this other thing that's going to happen?" And eventually you get some answer that's like "Ah yes, at step 20 when this lever is pressed, this happens and then something something". Yeah. I mean, I don't know, sometimes I worry, I personally play too fast and loose with "Oh, everything's analogous to everything else and you can move between them." And maybe I'm missing out on some details that are important, but I do tend to think you can turn lots of these things into other things.

**Daniel Filan:**
So I guess my last question on this type of work. So as you might know, in NeurIPS they recently required broader impact statements. And in particular, they asked people to imagine "OK, the work you're doing, what are possible ways that it could turn out to be bad?" And I kind of like this question. I think people working in the AI x-risk space rarely ask it of themselves. So if all this work on debate and imitative generalization, if this line of work turned out to have negative consequences and they don't get to be like, "Oh, it didn't pan out. So there was an opportunity cost", something bad happens because of this research. What's the bad thing?

**Beth Barnes:**
Okay. There's a few different things I can come up with here. One is something like stuff is all kind of looking like it's good in theory land and in human experiments land. But like, in fact it just is kind of impractical and what we should have been thinking about the whole time was more in the weeds, hacking together ML stuff to be incrementally more aligned and patching various problems. And we're sitting here in our ivory tower speculating about how we know everything, the model knows. And in fact, we should have been just like, "Oh, is there some trick we can use that makes it a bit more likely that we'll catch things of this type?" I guess there's also something else.

**Beth Barnes:**
Maybe it's not exactly that that's the problem, it's just the fact that you know, that there's some fundamental flaw with things like debate. And it all looks like it's working. We're like, "Great! We'll use it to build the aligned AGI." And then, it turns out it's actually not, and it's all terrible. I don't know if there's... I mean, I could go on brainstorming. There's probably things related to, maybe alignment isn't the only framing that's an important part of risk and looking like you have more of a solution to alignment makes people more excited to build powerful AI. And then there's some, not quite alignment-related thing that goes badly that, I don't know, something about misuse or something that people would have been less likely to do if no-one had been saying "Oh, we think we have a relatively good alignment solution". If everyone was like, "Oh yeah, whatever you try to build a powerful model, it just... This will always backfire on you" or something. Maybe this is actually a better world, if... I don't know, this is pretty speculative, I don't really believe that

**Daniel Filan:**
That's fair enough. Yeah. So closing out, you mentioned that you were no longer working on at least debate stuff, and maybe you said you weren't working on imitative generalization? What are you doing these days?

**Beth Barnes:**
I'm trying to figure out what exactly I should be doing. Currently, I'm being a sort of miscellaneous safety strategy person at OpenAI, and writing a lot of Google docs and gently doing a little bit of ML experiments that are kind of related to imitative generalization. Yeah. Not quite sure what I'll be doing in the future.

**Daniel Filan:**
Sure. If people are interested in following you or your research, or exciting things you're putting out, how should they do that?

**Beth Barnes:**
Yeah. Most of the things will probably be [alignment forum](https://www.alignmentforum.org/) posts.

**Daniel Filan:**
All right. Well, thanks for appearing on the show and to the listeners. I hope you join us again.

**Beth Barnes:**
Thanks for having me.

**Daniel Filan:**
This episode was edited by Finan Adamson.
