---
layout: post
title: "38.5 - Adrià Garriga-Alonso on Detecting AI Scheming"
date: 2025-01-19 16:25 -0800
categories: episode
---

[YouTube link](https://youtu.be/3A7CK4-1OFo)

Suppose we're worried about AIs engaging in long-term plans that they don't tell us about. If we were to peek inside their brains, what should we look for to check whether this was happening? In this episode Adrià Garriga-Alonso talks about his work trying to answer this question.

Topics we discuss:
 - [The Alignment Workshop](#alignment-workshop)
 - [How to detect scheming AIs](#how-to-detect-scheming-ais)
 - [Sokoban-solving networks taking time to think](#sokoban-solving-nets-taking-time-to-think)
 - [Model organisms of long-term planning](#model-organisms-of-long-term-planning)
 - [How and why to study planning in networks](#how-and-why-study-planning-networks)

**Daniel Filan** (00:09):
Hello, everyone. This is one of a series of short interviews that I've been conducting at the Bay Area [Alignment Workshop](https://www.alignment-workshop.com/), which is run by [FAR.AI](https://far.ai/). Links to what we're discussing, as usual, are in the description. A transcript is, as usual, available at [axrp.net](https://axrp.net/), and as usual, if you want to support the podcast, you can do so at [patreon.com/axrpodcast](https://www.patreon.com/axrpodcast). Let's continue to the interview. Adrià, thanks for chatting with me.

**Adrià Garriga-Alonso** (00:30):
Thank you for having me, Daniel.

**Daniel Filan** (00:31):
For people who aren't familiar with you, can you say a little bit about yourself, what you do?

**Adrià Garriga-Alonso** (00:35):
Yeah. My name is Adrià. I work at FAR.AI. I have been doing machine learning research focused on safety for the last three years and before that, I did a PhD in machine learning. I've been thinking about this for a while, and my current work is on mechanistic interpretability and specifically how we can use interpretability to detect what a neural network wants and what it might be scheming towards.

## The Alignment Workshop <a name="alignment-workshop"></a>

**Daniel Filan** (01:04):
Before I get into that too much, we're currently at this alignment workshop being run by FAR.AI. How are you finding the workshop?

**Adrià Garriga-Alonso** (01:10):
It's really great. I have had lots of stimulating conversations and actually, I think my research will change at least a little bit based on this. I'm way less sure of what I am doing now. I still think it makes sense, but I think I need to steer it somewhat, but I will still tell you what I've been working on if you ask.

**Daniel Filan** (01:33):
Sure, sure. Has the stuff that's changed your mind about... has it just been chatting with people? Has it been presentations?

**Adrià Garriga-Alonso** (01:39):
Yeah, mostly talking to people. I think I've been thinking a lot more about what scheming would actually look like and in which conditions it would emerge. I think, without going too deeply into it now (but maybe later we will), I was thinking of scheming as a thing that requires a lot of careful consideration about what futures might look like and perhaps an explicit prediction of what the future will look like under various things the AI could do, and then thinking of how good they are for the AI.

(02:16):
But actually, it might be a lot more reactive. It might just be the AI sees that... The chain of reasoning from "I want to do thing X. If I don't show myself to be aligned, I won't be able to do thing X, therefore let's do whatever it seems like in the moment to seem aligned." -- it's not a very long one that requires a lot of careful consideration. Perhaps the mechanisms I was hoping to catch it with won't work.

## How to detect scheming AIs <a name="how-to-detect-scheming-ais"></a>

**Daniel Filan** (02:49):
Wait, before we get into... You said you want to do mechanistic interpretability to find scheming. What sorts of things do you mean by scheming?

**Adrià Garriga-Alonso** (02:58):
Specifically talking about when an AI that has internalized some goals - so it's an agent, it wants something - is taking actions to get more of that goal that we wouldn't approve of if we knew of, or if we knew why it's doing them. It's hiding intentions from, in this case, developers or users.

**Daniel Filan** (03:29):
Okay. Hiding intentions from developers or users for a reason: it seems like that can encompass tons of behavior or tons of things you could be thinking about. Maybe you can say just a little bit about how you've been tackling it in your existing research.

**Adrià Garriga-Alonso** (03:53):
Yes, that's a very good question. I've been thinking about scheming specifically towards a goal or towards a long-term goal. The generator here is the AI wants something that can be accomplished better by (say) having more resources. That's the reason that we are worried about a loss of control.

(04:15):
Conversely, if there is no long-term thing that the AI wants and needs more resources for, then maybe we shouldn't be worried about loss of control too much. Maybe this long-term wanting of things is really the key characteristic that can make AI dangerous in an optimized misalignment kind of way. Targeting it seems useful. I've been approaching this by coming up with toy models or... At the start, they're toy, so they're progressively less toy, but now we're in fairly early days with this.

(04:54):
Coming up with models that display these long-term wants and behavior, that try to get the long-term wants and then figure out how are they thinking about this and by what mechanism are these wants represented, by what mechanism are these translated into action? If we can understand that in smaller models, maybe we can understand that in somewhat larger models and quickly move on to the frontier LLMs and maybe try and understand if they have a want machinery somewhere and if they take actions based on that.

## Sokoban-solving networks taking time to think <a name="sokoban-solving-nets-taking-time-to-think"></a>

**Daniel Filan** (05:29):
Concretely, what kinds of models have you actually trained and looked at?

**Adrià Garriga-Alonso** (05:35):
The models I'm training now are game-playing agents. We've trained this recurrent neural network that plays [Sokoban](https://en.wikipedia.org/wiki/Sokoban). Sokoban is a puzzle game that requires many... The goal of it is there's a maze-like or a grid world with some walls and then some boxes and some places where you should push the boxes to, and then you've got to push the boxes on the targets. If you don't put them in the correct sequence, then it's very easy to get stuck and you can't solve the level anymore-

**Daniel Filan** (06:11):
Right, because you can only push them, you can't pull them. If they get in a corner, you can't get them out.

**Adrià Garriga-Alonso** (06:15):
Yes, that's right. Yes, exactly. In a corner, you can't get them out, you can't push two boxes at a time. It has to be only one box. It's easy to inadvertently block your next box with the previous box or yes, get into a corner and now you can only move in some directions. For example, if you get it into a wall, then you can only move along that wall. The geometry of the levels gives you complicated actions that you may need to take.

(06:46):
We started from the point of view of a 2019 DeepMind paper by [Arthur Guez](https://www.gatsby.ucl.ac.uk/~aguez/) and others, called ["An investigation of model-free planning"](https://arxiv.org/abs/1901.03559). In that paper, they did the setup that we replicated here. The setup is: they just train a neural network with reinforcement learning, just by giving it a large positive reward if it solves the level and then a small negative reward for every step it takes so that it goes quickly.

(07:16):
They just train it to play this game, and it plays decently well on a bunch of random levels that they generate, and then they found that if you give this neural network more time to think at the start, then it is better able to solve these levels. 5% of levels that it previously was not able to solve, now it solves-

**Daniel Filan** (07:36):
Sorry, when you say "more time to think at the start"...?

**Adrià Garriga-Alonso** (07:39):
In this case, this is analogous to giving language models chain of thought. The specific mechanism is... The recurrent neural network has a latent state it brings from one timestep to the next, so they just process the initial input, the image of the puzzle, how it's set up at the first step, many times.

**Daniel Filan** (08:01):
Okay. So that is this setup that you're replicating.

**Adrià Garriga-Alonso** (08:06):
Yes.

**Daniel Filan** (08:06):
What do you do with that?

**Adrià Garriga-Alonso** (08:07):
We got that, and we've been poking at it in various ways. Some of the ways were about trying to better understand: in what circumstances does more thinking time help solve the levels? Or why does this happen from a behavioral point of view? What happens to the levels...

(08:28):
Actually, I slightly misspoke earlier: I said 5% of levels that were not solved are now solved. That's not actually true. 5% more levels get solved, but some of the ones that were solved before now are not and some of the ones that were not solved before now are. These don't cancel out. It ends up being a net gain.

(08:48):
We looked at a bunch of these levels and then we looked at how the neural network seem to behave naturally. Why might it have this capability to generalize, to just if you give it more time perform better? The original impetus there was: maybe because you give it more time to think and then it does better, it's doing some iterative algorithm on the inside that can use more computation to maybe consider more possible solutions and then see if one of them is the good one.

(09:22):
The things that we found there were pretty interesting for a model this small. It seems that the neural network has learned a meta-strategy in training of when it needs more time to think because a level is complicated, just taking time to think. The way it takes time to think is just it paces around the level without pushing any boxes. This does not change the game state in an irreversible way, but it just gives more processing time to the network. We found many instances of this. When the neural network paces around, we measured this with the proxy of how often does it end up in a state that it previously wasn't. It must have moved in some direction, then came back.

(10:08):
This happened overwhelmingly at the beginning of the level. You might think that it's using this to plan out the whole level, how it's going to do it, and then it's going to do it. Also if we substitute these cycle steps by just processing the current input without taking any action, which is again out of distribution from training because it always has to take an action, then these cycles disappear in the next few seconds. You can substitute for the cycles with just thinking time. Perhaps the thinking time is the main reason that the cycles are there.

**Daniel Filan** (10:42):
That's interesting. If I think about stories of AI being scary, it's AI gaining resources, and one of the resources AIs can gain is cognitive resources, just time to think.

**Adrià Garriga-Alonso** (10:55):
That's a good point.

**Daniel Filan** (11:00):
You can have these grid worlds where an AI has to get money in order to do a thing, and you can use reinforcement learning to train the AI to get the money. I'm not aware of previous work showing reinforcement learning on neural networks resulting in networks learning to give themselves more time to think about stuff. Is there previous work showing this?

**Adrià Garriga-Alonso** (11:24):
I actually don't know. I don't know of any either. The literature research we did mostly found things like architectures that explicitly try to train in variable amounts of thinking time. I believe the [Neural Turing Machine](https://arxiv.org/abs/1410.5401) -

**Daniel Filan** (11:41):
And even AlphaGo: didn't AlphaGo do this to some degree?

**Adrià Garriga-Alonso** (11:45):
Getting more time to think?

**Daniel Filan** (11:48):
In [the Lee Sedol games](https://en.wikipedia.org/wiki/AlphaGo_versus_Lee_Sedol), it didn't have a constant time for each move.

**Adrià Garriga-Alonso** (11:51):
Oh. Maybe that was pre-programmed after the fact: "search until you get enough value" or "you get a move that puts the probability of winning to a high enough amount". I don't actually know. I really think I should look this up now. This was not the main focus of the work, so I didn't claim that this was novel or anything, I was like, "okay, this is an interesting behavior of this neural network".

## Model organisms of long-term planning <a name="model-organisms-of-long-term-planning"></a>

**Daniel Filan** (12:18):
Okay, so what was the main focus of the work?

**Adrià Garriga-Alonso** (12:21):
The main focus is: train this neural network and now we have a model of... Or this is evidence that internally, it really has this goal, and it's perhaps thinking about how to go towards it. The goal was: now we can use this as a model organism to study planning and long-term goals in neural networks.

(12:43):
This is a useful model organism to start with because it is very small. It is only 1.29 million parameters, which is much, much smaller than any LLMs, but it still displays this interesting goal-directed behavior. Maybe we have a hope of just completely reverse-engineering it with current interpretability techniques, and now we know what planning looks like in neural networks when it's learned naturally, and maybe we can use this as a way to guide our hypotheses for how larger models might do it.

(13:14):
We took [some initial steps in this direction](https://tuphs28.github.io/projects/interpplanning/) with the help of a team of students at Cambridge as well, including [Thomas Bush](https://tuphs28.github.io/) as the main author. I advised this a little bit. They trained probes that could predict the future actions of this neural network, many steps in advance, by very simple linear probes that are only trained on individual "pixels", so to say, of the neural network's hidden state, because the hidden state has the same geometry as the input.

(13:52):
We also replicated this. We can also train these probes that predict the actions many steps in advance, and then some of these are causal in the sense that if you change the plan that is laid out by... These probes give you a set of arrows, let's say, on the game state that says, "the agent is going to move over this trajectory" or "the boxes will move in this trajectory and this one will go here, that one will go here, that one will go here". We can also write to this and then that actually alters the trajectory that the agent takes. We can put a completely new plan on the hidden state of the neural network and then it will execute it, which leads us to believe that the way in which it comes up with actions really is coming up with these plans first and then executing them.

**Daniel Filan** (14:44):
Okay. This actually sounds kind of similar to [this work](https://www.alignmentforum.org/posts/gRp6FAWcQiCWkouN5/maze-solving-agents-add-a-top-right-vector-make-the-agent-go) that [Alex Turner](https://turntrout.com/welcome) has done on activation steering.

**Adrià Garriga-Alonso** (14:54):
The mazes and the cheese.

**Daniel Filan** (14:55):
The mazes and the cheese, yeah. Interestingly, I think Alex's interpretation is that agents are a bit more... Without putting words in his mouth, this is my interpretation of his interpretation, but I think he thinks that this shows that agents are a bit more... They have these [shards](https://turntrout.com/shard-theory) and they have this "I want to find the cheese" shard versus "I don't want to find the cheese" shard and whichever shard is activating stronger-

**Adrià Garriga-Alonso** (15:22):
-Is the one that determines the behavior now.

**Daniel Filan** (15:23):
Yeah, I think he has a relatively non-unitary planning interpretation of those results. It seems like you have a different interpretation of yours. Do you think it's because there's an important difference between the results?

**Adrià Garriga-Alonso** (15:34):
That's a good question. Initially for this project, we also targeted... Okay, I think there's some difference. Here's the difference I think there is. Initially for this project, we also targeted maze-solving neural networks. We trained a specific kind of algorithmic recurrent network that [a NeurIPS paper](https://arxiv.org/abs/2202.05826) - by [Arpit Bansal](https://arpitbansal297.github.io/), I think it is, I can give the citation later - came up with, and then we looked at how it solved mazes.

(16:05):
It was a very simple algorithm that could be said to be planning but does not generalize very much. The algorithm was [dead-end filling](https://en.wikipedia.org/wiki/Maze-solving_algorithm#Dead-end_filling). You can look it up on Wikipedia. It's a way to solve simple mazes, which are those that don't have cycles. I think these are also the mazes that Alex's work used. And it just works by: you start with your maze as a two-color image, let's say it's black and white, and then the walls are black and then the spaces where you can go are white, and then for every square of the maze, you see if it has three walls next to it, adjacent to it, and then if so, you also color it black.

(16:48):
You can do this, and then you can see the hallways of the maze fill up until only the path from agent to goal is left. That's what we observed this neural network doing very clearly. I think this is an interesting planning algorithm, but the state space of this is small enough that it fits completely in the activation of the neural network. It's a planning algorithm that only very specifically works for this problem.

(17:20):
I thought that perhaps a more complicated environment where the state space is much larger and doesn't completely fit in the activations of the neural network... because here the neural network has an activation for every location in the maze and then the states are also just locations in the mazes because there's only one agent, so this is just the XY position. It could perfectly represent this and think about all the possible states and then plan.

(17:47):
I think that, plus the fact that you can also just go up and right and just get to the cheese, and if you get it somewhat wrong, you can correct it, makes it so that the neural network is less strongly selected to be able to do this long-term planning and also the long-term planning that you might find uses a lot less machinery and would be less generalizable. I think there's a big difference in the environment here.

(18:19):
I guess the other thing I would say though is I do think his theory is broadly right, or maybe more right than wrong, in that I do think it's highly likely that neural networks that you actually train are not just one coherent agent that only wants to maximize this one thing, but it might have other mechanisms in there, such as "I just like going right", or "I like this other goal". Also those might play a role. And I think we might find this in this agent, but I think we selected it pretty strongly towards having this one goal of solving this level and the levels are solved with planning, so it probably does that a whole bunch.

**Daniel Filan** (19:07):
You mentioned your paper, a paper by some Cambridge people: what are the names of these papers so that people can look?

**Adrià Garriga-Alonso** (19:13):
[The Cambridge one](https://tuphs28.github.io/projects/interpplanning/) isn't published yet, but we will soon. The main author is Thomas Bush, and the title is something like An Example of Planning in Model-Free Reinforcement Learning. I will have to look this up and send it to you.

**Daniel Filan** (19:34):
Hopefully that's enough for people to Google it.

**Adrià Garriga-Alonso** (19:37):
And then our paper is called ["Planning in a recurrent neural network that plays Sokoban"](https://arxiv.org/abs/2407.15421).

## How and why to study planning in networks <a name="how-and-why-study-planning-networks"></a>

**Daniel Filan** (19:44):
Going back up the stack a little bit, I guess the reason you were interested in this work is because it sounded like you were interested in understanding agents that were scheming, they're deceptively aligned, they're trying to deceive you and do some tricky stuff in order to achieve some goal, and that's why you're studying goal orientation and how it gets implemented.

**Adrià Garriga-Alonso** (20:07):
That's right.

**Daniel Filan** (20:12):
You mentioned that you had some reservations about that approach, which I understood to be something like: maybe it doesn't take tons of planning to do this sort of scheme behavior.

**Adrià Garriga-Alonso** (20:24):
Yeah, maybe you don't need to have a goal that you explicitly evaluate, you explicitly think of futures. Yeah, I think you're broadly right. I just wanted to expand a bit.

**Daniel Filan** (20:33):
I guess there are two types of scheming behavior, right? There's one type which is: you are being trained and somehow you know you're being trained, and so you're like "okay, I'm just going to play along because I can't really do anything else. Nothing else is particularly useful right now". That seems like it is not super cognitively complex, right?

**Adrià Garriga-Alonso** (20:54):
Yeah.

**Daniel Filan** (20:54):
But then there's a second thing which is: you're an AI, you finish being trained, you're being deployed, but there are humans around, they don't want you to execute your super devious plan, so you've somehow got to get around them. I might think that actually maybe it does need a lot of planning to execute the "actually do nasty stuff" part.

**Adrià Garriga-Alonso** (21:18):
Right, right. That's a good point. If the safeguards are complicated enough and there's many things that need to go right, then it would perhaps need a lot of planning. I guess I agree with this. Maybe the takeaway here is more that my initial hypothesis as to what planning would look like is wrong rather than studying planning is not useful.

**Daniel Filan** (21:48):
Okay. What was the initial hypothesis?

**Adrià Garriga-Alonso** (21:50):
I had this model of: the reason that we can get at planning in neural networks, even if we don't solve all of interpretability, is that planning needs... Okay, I say this now, but I'm not sure I entirely believe it, but for a second, let's put this hat on.

**Daniel Filan** (22:09):
Bracketing it.

**Adrià Garriga-Alonso** (22:11):
Yeah, yeah, exactly. Planning needs a mechanism that thinks about possible outcomes or possible ways that the world can go from now on. I guess you need a world model, but I'm also abstracting over this. I'm just saying "okay, there's just going to be several possible futures depending on what I do, and I'm going to think about a bunch of possible things I might do, and then what will the world look like in a while".

(22:38):
The neural network or the planning algorithm would evaluate all of these possible outcomes according to some internal criterion, which is basically "how much do I like these worlds that we end up in?", and then it would basically look at all the evaluations and then pick the best one, and then execute that. I was like: okay, we might not understand what's the abstract representation of the future outcomes. That seems complicated, especially if the agent is very intelligent.

(23:05):
We might also not understand exactly how does it get translated into an evaluation. It might have lots of intuitive things, and we might not know what each thing represents. But what we can definitely see, even if we can't do interpretability on most of everything else, we can see that there's a number at the end that is somewhere in the activations of the neural network. There's a bunch of these and the best one gets picked, and that is what gets executed.

(23:31):
If we would find this computational signature of this mechanism, which picks the best number out of many, and that determines the action, that is a planning-like thing that would be happening. I think this will happen also in a bunch of other circumstances, but perhaps we can just catalog the, I don't know, thousands of these instances in the neural network and if they all coherently do something, maybe we would know when the neural network is expecting to get a big reward, or maybe we would be able to ablate these to zero to prevent any long-term goals from happening.

**Daniel Filan** (24:03):
Although I guess just having a bunch of actions that you score doesn't say that the goal is long-term, right? If I'm like: I really like to wave my left arm, I'm going to consider actions that I'm going to take and which one of them involves waving my left arm, I'm going to do +1000 to that, and I'm going to pick that action like that.

**Adrià Garriga-Alonso** (24:21):
Yeah, I guess my intuition here is also that goals that are fairly short-term will be more efficiently expressed as kind of cached skills or something, or things that the neural network can access. Or you're not thinking explicitly about which muscles you're moving when you're raising your left arm. That kind of-

**Daniel Filan** (24:44):
Don't presume things about how I think about my muscles, please.

**Adrià Garriga-Alonso** (24:46):
Okay, okay. I don't know what it's like to be Daniel Filan.

**Daniel Filan** (24:51):
Most of the time when I'm lifting my left arm, I'm not explicitly thinking about muscles, but from time to time it's fun, yeah.

**Adrià Garriga-Alonso** (24:57):
Or I suppose if you're trying to gain a new skill of exactly how to move your left arm... I don't know, if you do dancing, then you might have some technique that you need to move your arm explicitly in a particular way you haven't done before.

**Daniel Filan** (25:12):
Then I would normally not think about muscle contractions, I guess. I don't know. I guess you'd visualize the thing. I think that thing where I would think about muscle contractions is maybe in a weightlifting context.

**Adrià Garriga-Alonso** (25:22):
Good point, or physical therapy.

**Daniel Filan** (25:24):
Yeah. Sorry, you were saying something and then I brought in diversions.

**Adrià Garriga-Alonso** (25:28):
No, no, no, no. I think I was finished with my point, which is that most of the time short-term goals wouldn't require all this explicit outcome planning and would just use this other mechanism.

(25:39):
And we would also get some short-term goals there. I was hoping it would be less, or maybe we would be able to tell them apart in some way, or I guess I also hope that now that we have located these goals, maybe we can trace them backwards a little bit: what in the neural network produces this evaluation? Maybe we can understand a little bit about that. Even if we don't understand it perfectly, we can get some broad strokes of what it is.

(26:09):
It's difficult for the neural network to modify this. Even if it's a very intelligent agent, it wouldn't necessarily be able to fool our best classifiers by changing the way that its internal brain is structured. That seems pretty difficult.

**Daniel Filan** (26:28):
Okay. Cool. We're about out of time. I want to give you time to enjoy the rest of the workshop. Thanks very much for chatting with me.

**Adrià Garriga-Alonso** (26:36):
Thank you very much for having me.

**Daniel Filan** (26:38):
This episode was edited by Kate Brunotts, and Amber Dawn Ace helped with transcription. The opening and closing themes are by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with patrons such as Alexey Malafeev. To read a transcript of the episode, or to learn how to support the podcasts yourself, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
