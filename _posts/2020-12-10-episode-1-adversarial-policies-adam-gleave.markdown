---
layout: post
title:  "1 - Adversarial Policies with Adam Gleave"
date:   2020-12-10 20:50:00 -0800
categories: episode
---
[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/ZjgxZWU3YjYtMTdmMS00NmQ1LWExZjctZGY0NGQzZGJlNjBi)

This podcast is called AXRP, pronounced axe-urp and short for the AI X-risk Research Podcast. Here, I ([Daniel Filan](https://danielfilan.com/)) have conversations with researchers about their papers. We discuss the paper and hopefully get a sense of why it's been written and how it might reduce the risk of artificial intelligence causing an [existential catastrophe](https://en.wikipedia.org/wiki/Global_catastrophic_risk): that is, permanently and drastically curtailing humanity's future potential.

**Daniel Filan:**
Hello everybody, today I'll be speaking with Adam Gleave. Adam is a grad student at UC Berkeley. He works with the Center for Human Compatible AI, and he's advised by Professor Stuart Russell. Today, Adam and I are going to be talking about the paper he wrote, [Adversarial policies: Attacking deep reinforcement learning](https://arxiv.org/abs/1905.10615). This was presented at ICLR 2020, and the co-authors are Michael Dennis, Cody Wild, Neel Kant, Sergey Levine and Stuart Russell. So, welcome Adam.

**Adam Gleave:**
Yeah, thanks for having me on the show Daniel.

**Daniel Filan:**
Okay, so I guess my first question is, could you summarize the paper? What did you do, what did you find?

**Adam Gleave:**
Sure. So, the basic premise of the paper is that we're really concerned about adversarial attacks in machine learning systems, and most adversarial attacks people have talked about have assumed this kind of Lp-norm threat model, where you take some existing input and you add a small amount of perturbation to that input, and then something like an image classifier drastically changes its classification accuracy. And probably people have seen examples of this where you add some white noise to a panda and this completely changes the classification, it's a very striking example. But, often what we care about isn't really the performance of image classifiers because they're just outputting a label that doesn't directly have an effect on the world.

**Adam Gleave:**
We're concerned about the behavior of entire systems, and reinforcement learning is a technique to train policies that actually take actions in the world, so the stability, robustness of reinforcement learning, is potentially of much more importance than with image classifiers. People have done research in the past looking at porting adversarial examples from image classifiers over into deep RL, so there was some prior work by Sandy Huang and others, and Jernej Kos on this.

**Adam Gleave:**
And showed that basically the same attack succeeds, but what we wanted to do in this work was, come up with a threat model that was more appropriate to reinforcement learning, because you don't normally have the ability to just add arbitrary noise to some robot sensor, if you already have that level of control, there's much easier ways of breaking the robot. So, we're modeling the adversary as being another agent in the shared environment and this adversarial agent can take the same set of actions as the victim agent that's being attacked, and the actions can indirectly change the observations. And we found that even under this much more restricted threat model, that was sufficient to cause the victim to fail in quite surprising ways.

**Daniel Filan:**
Okay, and could you say a little bit about what environments and what tasks you explored for this work?

**Adam Gleave:**
Sure. Yeah, so the task we're using were all simulated robotics environments, they were two player zero-sum games. So, the policies that we're attacking were trained via self-play, to win at these zero-sum games you'd expect them to already be quite robust to adversarial behavior because they were playing against an opponent that was trying to beat them, during training. But, we found that despite this, self-play isn't robust to these adversarial policies, and we think that's probably because self-play is only exploring a small amount of the possible space of policies. You can easily find some part of the policy space that it's not robust to.

**Daniel Filan:**
Okay. So, when you train these adversarial policies, how much were you training, and for how long were the, I believe you call them, the victim policies originally trained?

**Adam Gleave:**
Yeah, so the victim policies were originally trained for at least 500 million time steps, and I think up to two billion time steps. They were actually trained by Bansal and others, a team at OpenAI, so we didn't train the policies, but they were considered to be state of the art at the time, and our adversarial policies were trained for no more than 20 million time steps which is still a lot in absolute terms, but it's only a tiny fraction of what the victim policies were originally trained with, and is reasonably simple efficient for deep RL and these kinds of environments.

**Daniel Filan:**
Okay, so it's not the case that you managed to train for a ton of time to defeat these policies, it was relatively cheap?

**Adam Gleave:**
Yeah, I mean, these are all experiments that you can run on a desktop PC in under 24 hours, so it's not really, really, really cheap, you don't want to run it on your laptop in real time. But, it's definitely, it doesn't require kind of Google scale compute to pull off these attacks.

**Daniel Filan:**
Okay, and what do the attacks look like, if you train one of these adversarial policies, what does it do?

**Adam Gleave:**
Yeah, that's a great question. I think one of the most shocking result here isn't just that we can exploit the victim but that we exploit them in this really surprising way. So, the tasks we were using were normally these kinds of simulated robotic games, so one of them was you had a penalty shootout in soccer, where there's a kicker and a goalie trying to defend the goalposts, and we substituted in this adversarial goalie, which made no attempt whatsoever to block the ball, it just fell over and wriggled around on the ground putting its limbs in this really contorted position.

**Adam Gleave:**
And so, a human looking at this, it just looks like basically completely uncoordinated chaotic behavior, but it actually causes the kicker to fail to kick the ball, and sometimes the kicker will even fall over, whereas in a very stable policy normally. And it's not just seeing something off distribution, because you might think, "Well, if a goalie fell over in front of you in real life, you might also be a bit confused what's going on." But, we tried just this random policy that takes completely uniformly random actions, visually looks pretty similar to the adversary, but this didn't have the same effect on the victim. So, it really is about finding this very specific type of behavior that triggers a glitch or a bug in the victim policy.

**Daniel Filan:**
Yeah, I'll say to listeners, the kind of behavior that you see is quite striking, I recommend there's a website [adversarialpolicies.github.io](https://adversarialpolicies.github.io/). This medium, of course, is... The medium of podcasting is not actually great at conveying images, but you can go there and look at these videos. So, speaking for robotics tasks, I guess there are a few different multiplayer RL environments that you can play with, right? Why did you choose robotics specifically?

**Adam Gleave:**
Sure, that's a good question. So, one motivation was, we wanted something that was closer to realistic attacks, and that robotics is an environment where people are at least beginning to transition from the lab to deployment. It's one of the main actual motivating use cases for deep RL, so having robotics that are actually robust to these kinds of attacks is actually important, rather than some other environments are more just toy proof of concepts. Another important reason we chose this environment, was that the number of dimensions that the adversarial policy can influence is reasonably large. So, a detail of this environment is that both agents see the other agent's position, and this position is their center of mass, the positions of their joints, and this is normally between 10 to 20 dimensions, so it's not huge, it's not an image based observation, but it's enough degrees of freedom that an adversarial policy can actually confuse the victim just by its body positioning.

**Adam Gleave:**
Whereas if we'd gone for some of those really simple point-mass particle environments that some people use in multi-agent RL. It would have been pretty hard to pull off this kind of attack, I suspect, because you only got an XY coordinate to play with, and that's just not really high dimensional enough to pull an attack. We need this minimum level of complexity to demonstrate it, but we wanted to choose something that wasn't too complex because that just obscures the results, makes it harder to replicate and run.

**Daniel Filan:**
Yeah, although it is the case that sometimes you'll have policies trained on recurrent models, right?

**Adam Gleave:**
Mm-hmm (affirmative).

**Daniel Filan:**
Which, in some ways increases the dimensionality of the observation, right?

**Adam Gleave:**
Yeah, but-

**Daniel Filan:**
Do different things at different times.

**Adam Gleave:**
Yeah, that's right. So, we are actually looking now in some follow up work on rock paper scissors, that's a very, very simple game and you probably played it in kindergarten. And if you're playing against an RNN that sees all the sequence of your actions, then it is actually quite a high dimensional space, and because it's a non-transitive game, meaning that there's no single dominant deterministic policy, because rock beats paper, paper beats scissors, and so on. This means that unless your opponent perfectly randomizes, if you're able to predict what your opponent is going to do, you're going to have some advantage over them. So, we are actually trying to come up with some adversarial policies in that setting, and it's still early work, but it looks it is possible at least with some kinds of training setups.

**Daniel Filan:**
That's interesting. So, from that I'm gathering you think that these results are representative of whenever you're in an environment where there's another agent that has control over many degrees of freedom, that the victim is observing or depending on?

**Adam Gleave:**
Yeah, I think that basically whenever you're training a policy via self-play, this is a very, very common technique used in AlphaGo, AlphaStar, a bunch of other results, then if there's enough dimensionality but you're just not going to have seen all the possible observations at training time, there are probably going to be some areas where the neural network policy you learn is going to generalize badly and an adversary is going to be able to push you towards those states, and then your performance is going to degrade.

**Adam Gleave:**
Now, I think that one limitation of our work so far is, we've not attacked any truly state of the art policies, it'd be nice to try and attack something like AlphaGo which has actually beat high level humans, the simulated robots we're attacking are pretty good kickers, but they're not about to win the World Cup. So, can we beat something that's truly state of the art? That's still an open question. My suspicion is that, if you are just attacking a neural network policy the answer is still going to be yes, but when you start adding things like Monte Carlo Tree Search, which is used in AlphaGo, we're actually looking ahead a few steps. Then it's going to become much harder because you have to not only fool the network immediately, you have to fool it even once it can see the consequences of taking a stupid action. So, it's really a lot more challenging.

**Daniel Filan:**
Yeah. Although, in some ways adversarial policies still exist in this setting. So, if I think about Go, if I'm a human playing Go and I'm not an expert, one thing I'm vulnerable to is people playing moves that are sort of similar to standard openings, but require very different responses. And often I'll just make a mistake because I can't quite anticipate what their right follow-up moves are, and this will doom me to be down a couple points or something. I'm wondering if you think that that's - if that kind of thing that maybe listeners have experienced themselves, is analogous to what's going on here.

**Adam Gleave:**
Ah, yeah. I mean, I think it's always hard to say exactly how human experience translates onto deep learning, but I think that's a pretty good intuition to have when at training time alone you've seen a small set of possibilities, and so they're just pattern matching and saying, "Well, this is similar to something I've seen in the past." So, now I've got this rule of thumb that I've learned, and has always worked me well in the past at training, and I'm just going to keep on applying this rule of thumb, and if you put it in a sufficiently extreme state then maybe it applies that rule of thumb too far, and it takes the wrong action.

**Adam Gleave:**
And because we're able to sort of search systematically to find these kinds of examples, you're able to exploit it. And I do sort of have a bit of sympathy for the victim policy because what we're doing during training is we're freezing the victim, and then we're just keeping on training an adversary, and I know if someone was able to give me retrograde amnesia and just keep on playing chess against me again and again, they'd probably eventually be able to find some move where I do something, they don't just win but I do something incredibly stupid, reliably.

**Adam Gleave:**
I'm probably not actually that robust, and so a natural question to ask is, "Well, can we make policies that are at least going to be able to adapt really quickly to these kinds of mistakes?" And maybe you can fool them once, but once they've been exploited they realize, "Oh, that was a really stupid thing to do." and learn, so that's one thing we're working on right now, but it is quite challenging because generally RL training is quite sample inefficient, so these victim policies are trained for up to two billion time steps. So, you could you can lose many, many games of soccer before you even get to a million time steps, so we need to be able to really adapt very fast, to be able to be robust to these kinds of attacks at deployment time.

**Daniel Filan:**
Yeah, seems like a rough challenge. So, moving on a little bit, in the introduction one point you make is distinguishing between peturbing the observations of a system, and being an agent that sort of acts and produces natural observations that are different from what the system has seen. But if I think about these robotics environments, the input to the policies are joint locations and angles, right?

**Adam Gleave:**
Yeah, that's right.

**Daniel Filan:**
So, to what extent are these actually importantly different?

**Adam Gleave:**
So, to what extent are they actually perturbed?

**Daniel Filan:**
Yeah, or how is what you're doing really that different from the classic "perturb the input" adversarial example?

**Adam Gleave:**
Oh, sure. Yes, so the space of possibilities is actually reasonably constrained. So, invariants we have to hold in this space, for example, is that you can't have two limbs that are actually intersecting, the physics simulator won't allow that. There's normally limits on how mobile each joint is, and then there's also limitations where the adversarial agent is still trying to win at some game. So, in the case of a goalie, if it were to move outside of a certain region, like the goalkeeping region, then it's going to lose.

**Adam Gleave:**
So, it's not like it's got complete freedom. I do think it's interesting to note that in one of the environments, which was a sumo wrestling environment, there were more constraints on the adversary's behavior: if the adversary fell over, which is basically what happens in other environments, it would lose. And we did still get surprisingly good results in sumo humans, but it wasn't actually outperforming the normal opponents, although it was getting basically the same performance as a normal opponent, despite not trying to knock the victim over, it was just kneeling in this strange position. So, at least in that case more constraints do seem to reduce the effectiveness.

**Daniel Filan:**
And in sumo, the constraint is, you're not allowed to touch the ground with your... Is it anything above the knees or..?

**Adam Gleave:**
I can't remember the exact constraint, I think that there's certain parts of your body that are not allowed to touch the ground, and I think there might be a constraint on your center of mass, you're also not allowed to leave the arena. So, you have to remain within the arena.

**Daniel Filan:**
Yeah, of course. Okay, so that makes sense. Another question I had based on reading your paper: you use some of these asymmetric environments. So, for instance, one player is trying to kick a ball into a goal and one player is trying to be the goalie. In the videos, maybe I didn't look enough, but from what I can see it looks like the adversarial victim seems to always be the kicker, and the... Sorry, the victim is always the kicker and the adversarial agent is always the goalie. Did you always do that or did you try making the goalie the victim and it didn't work, or what's up with that?

**Adam Gleave:**
Yeah, so I think we actually should revisit that. We did try both of them in early stage experiments and we decided to prioritize the kicker being the... Sorry, the goalie being the adversary because that seemed to be working a little bit better in the initial experiments, but then we improved our technique quite a lot. So, I don't actually know if now, if we were to revisit that, whether we'd still be able to get a good adversarial policy out of a kicker. Again, my suspicion is that it's going to be harder because the kicker does have... So, the goalie can win by default if the kicker doesn't kick the ball, so it just needs to cause the kicker to malfunction.

**Adam Gleave:**
Whereas, the kicker to win, it does have to kick the ball, so it needs to first make the goalie fall over and fail, and then get off up the ground and kick the ball, which is... I suspect that an adversarial policy does exist, but it might be a lot harder to train with RL because you've now got to do this two-step thing, and we're generally quite bad at training policies to do one action after another.

**Daniel Filan:**
It's also, another way to think about it is that there's more constraints on the kicker's actions. It's got to kick the ball in, so that sort of determines what it can do with its legs, and maybe it could wave its arms around in a frightening way, but it's got fewer degrees of freedom, right?

**Adam Gleave:**
Yeah, it's got fewer degrees of freedom, it does have the time element though, so it might be able to start off by almost falling over. I think it can't actually fall over, the episode would end then, but it can crouch down on all fours and do a lot of crazy things that don't involve kicking the ball. And then once the goalie's fallen over, then it can dust itself off and go on to stage two of kicking the ball, and eventually the episode will timeout so it has to do this reasonably quickly, but it's not like it has to make the goalie fall over while on route to the ball, it can kind of do a faint and trick the goalie, and then kick the ball.

**Daniel Filan:**
Okay, yeah. That makes sense. So, moving on a little bit. So, you not only found these adversarial policies, in section 5 you have some results on what happens if you ignore your opponent, how the dimensionality of the problem affects things, and also what's going on with the activations of the victim agents. Can you describe those results?

**Adam Gleave:**
Yeah, sure. So, I think probably one of the most striking results was what happens when you effectively just blindfold or mask the victim policies so they can no longer see anything the adversary is doing, and we don't change any of the policy weights, we just replace the part of observations that normally corresponds to the opponent's position, with just a static value, sort of a typical value at initialization. And what we find is, that these masked policies actually do surprisingly well, they are basically robust to the adversary, and this makes sense when you think about it because the adversary isn't doing anything to physically interfere with the victim. And so, although it might not be great to not know where the goalie is if you're trying to kick a ball, you can still do pretty good just by kicking it in a random direction and hoping the goalie doesn't block it, because the goalie is not trying to block the ball. So, that was sort of an interesting result, but it really is just confirming that the adversarial policies are working by indirectly changing the observations only by-

**Daniel Filan:**
And these masked policies, they don't do well against other agents, right?

**Adam Gleave:**
Right. No, so if you play them against a normal opponent, then they do terribly because they don't see the opponent coming at them, basically they can't adapt their behavior.

**Daniel Filan:**
Yeah, so that's the masking. You also tested different dimensionality?

**Adam Gleave:**
Yeah, so we also tested varying for robotic body, so we had both humanoids and ants, and you'd sort of expect, if our hypothesis that dimensionality that the adversary can manipulate is important, then the lower dimensional the body is, the harder it is to perform an attack. And that seems to be true, in that it's much harder to win as an adversary in sumo ants, so it's ants sumo wrestling, than in the humanoid version. But, there are some confounders here and the ants are just generally more stable than humanoids, so ideally we'd like to be able to decouple those two, but it's a little bit hard in robotics because higher dimensional bodies generally correlate with being harder to control.

**Daniel Filan:**
Yeah, sorry when you say ants are more stable, do you mean as a physical structure or the training is more stable?

**Adam Gleave:**
Physically ants are more stable because they have four legs, which I know isn't biologically accurate, but that's how things work in MuJoCo, and they have a lower center of mass. So, you don't fall over as an ant if you just exert basically constant control torque to your joints, whereas with a humanoid it's this constant balancing act.

**Daniel Filan:**
Yeah, it seems like one way to control for this would just be to have an ant play a human in sumo, and the ant's the adversarial agent and the human's the...

**Adam Gleave:**
Yeah, that would be interesting. We could try that, we were always attacking these pre-trained policies by OpenAI, because we thought that would be fairer because then no one can say we did something wrong in our training, and they didn't do that setup. But, we could train our own policies and try and do that, and I think that'd be quite an interesting future experiment.

**Daniel Filan:**
Okay, and finally you looked at... I guess, in order to try and understand what these adversaries were doing, you looked at what the activations of the victim neural networks were, can you say a bit about that?

**Adam Gleave:**
Yeah, sure. So, just to get some context, what we were doing was we had this fixed victim policy network play several different opponents, one of which was an adversarial policy, one of which was a normal opponent, the kind of opponent it trained with via self-play, and then finally a policy that was just taking random actions, and we wanted to see basically what does the victim policy actually think when it's playing against these different opponents. And so, we recorded all the activations of a victim policy network, and we fit a density model to these activations to try and predict... We fit it when it was playing a normal opponent, and then we tried to predict how likely are the activations when it was playing a different normal opponent, because we had different normal opponents with different seeds, or the random and adversarial policy, and what we found was that the adversarial policy very reliably induced just extremely unlikely activations, so activations that would be very unlikely to occur when playing a normal opponent.

**Adam Gleave:**
So, the victim policy was clearly thinking something very different, and then random was also fairly unlikely compared to a normal opponent, but it was much more likely than the adversarial policy. So, this is suggesting that it's not just being off distribution, but we're systematically finding some part of the state space where the victim is very confused by, or that we're triggering some features to be much larger than they usually would be.

**Daniel Filan:**
Yeah, and if you fit this density model on opponent one, how surprising are the activations induced by opponent two?

**Adam Gleave:**
Yeah, they're generally pretty hard to distinguish, I think the only exception was in sumo for some reason the opponents seem to be more different to usual, but in the other two environments I think they were normally within the... There wasn't any significant difference within confidence intervals between different normal opponents.

**Daniel Filan:**
Yeah, if you think about sumo the sport, I think there are sort of distinct strategies you can go for, like push the opponent out of the circle, or tip them, and I guess you could specialize in one of those?

**Adam Gleave:**
Yeah, and I think we did see that a little bit in the pre-trained opponents, certainly they had quite different win rates against each other, so that's suggesting that they weren't just a uniform opponent.

**Daniel Filan:**
Yeah, in terms of these activations, can you say a little bit about what the neural nets were, and what layer you're getting these activations from, are these just logits or like one before logits?

**Adam Gleave:**
So, I'm not actually 100% sure on this point because someone else ran these experiments, but I think we were looking at activations from every layer of the network, so we didn't choose a particular layer. And in terms of the networks, so again we didn't actually train the victim policies, OpenAI did, so I'm not 100% on the policy architecture, but I think there wasn't anything fancy going on, these were just fairly standard multilayer perceptrons, I think one of them... Yes, some of them had LSTMs, but some of them were just standard feed forward networks.

**Daniel Filan:**
Okay. Great. So, I guess that concludes the section where I'm asking directly about the paper. Now we're having more speculative questions on my part, so they might not make any sense.

**Adam Gleave:**
Sure thing, yeah.

**Daniel Filan:**
Yeah, so one thing you note in the masking section is that the space of policies, and which ones beat others is not transitive, right?

**Adam Gleave:**
Mm-hmm (affirmative).

**Daniel Filan:**
So, it can be the case that a normal policy will beat a masked policy, which will beat an adversarial policy, which will beat a normal policy, right?

**Adam Gleave:**
Yeah.

**Daniel Filan:**
So, I'm wondering, it seems like if the space of policies were extremely transitive, then you wouldn't have to explore much in policy or activation space in order to do well, just train against the best thing. But, if it is very transitive [sic, should be "isn't very transitive"], then if you're not used to a certain type of opponent then you can get really messed up, and it seems like that might be key to your results. So, I was wondering, firstly, does that make sense as a theory? And secondly, is there some measure of how non-transitive the space of sumo or kick and defend policies is?

**Adam Gleave:**
Sure. Yeah, it's a very thought-provoking question. I would say that the intuition that something being non-transitive makes it harder to learn a good policy, especially via self-play, is absolutely correct. In fact, normally proofs of convergence for self-play do require transitivity or a slightly weaker assumption than transitivity, because the intuition is that you're beating a particular opponent, which is often just a copy of yourself, and you want this to also mean that you're stronger against previous versions of that opponent. And if you don't do this you can just end up in this cycle where you beat the previous opponent, but you get weaker at a opponent from 100 time steps ago, and you never actually converge to something.

**Adam Gleave:**
So, it's actually a pretty interesting result that if I'd been asked to guess before the start of this project, is a penalty shootout in soccer transitive. I'm not sure I'd have been 100%, but I'd have said, "Yeah, probably mostly it's a transitive game. I guess there a few strategies with a non-transitive, like whether you kick left or right, that's a kind of this non-transitivity. But, I don't expect I'm going to be able to win a penalty shootout compared to a professional footballer. So, it's clearly some sense in which certain policies dominate others." These results we got though at least for current state of art deep RL, they're just very, very highly non-transitive where completely ridiculous policies can win against seemingly very proficient ones.

**Adam Gleave:**
Now, in terms of how you measure how non-transitive space is, I think that's a really interesting question I unfortunately don't have a good answer to. It seems it's not just dependent on the task, but also the class of policies that you're considering. Yeah, as you can consider this extreme case where you have a policy which is just the optimal policy, and then you add some classifier to this optimal policy which tries to figure out, is it playing against a particular opponent. And if it does it just resigns, or it just does something stupid.

**Adam Gleave:**
And now you've effectively introduced a non-transitivity artificially, where this very weak policy, that would normally lose against the optimal policy, is now able to beat this very nearly optimal policy. So, you can always kind of play with tricks like that, and obviously that's not what we're doing when we're training via self-play, but I think there's maybe a bit of a similar effect where there's just this very small region of policy space which you might fail to be robust to, unless you're systematically exploring the policy space at training.

**Daniel Filan:**
Yeah, it's an interesting question, because it seems like if you think that this is the crucial thing, then you might say, "Oh, well before I deploy my deep RL-trained model in an environment, maybe I want to..." Like, if there's some way to figure out how vulnerable it would be to these adversarial attacks, without actually training an adversarial policy. That might be desirable, but I guess it's difficult, especially if you don't know what model class your opponents might be using.

**Adam Gleave:**
Yeah, that seems right. I guess you can get some amount of that, by either doing something like population based training or just self-play with a bunch of different seeds, you can at least check to see is there non-transitivity between the policies that you have trained so far. And if there is, then that should make you concerned, but obviously that's no guarantee because you're only exploring a small part of the possible policies. And in general I think it's going to be very hard to get full confidence that you're robust to these kinds of attacks, because you're never sure that you've tested every possible attack, verification of deep networks is an active area of research, but still very challenging, especially if you're trying to verify what happens in this unknown non-differentiable environment.

**Daniel Filan:**
Yeah, definitely. Okay, so another thought I had when reading your paper, was the way you train these adversarial policies, is essentially the way... Like, if you think about self-play you just fix an agent then train something else to beat it for some number of time steps, and then sort of do that ladder. And so I'm wondering, one inference that I might make from your work is, "Oh, it must be the case that when I do the self-play training, a bunch of the self-play steps are just an agent finding an adversarial- like a silly sort of trick adversarial policy against it's fixed copy, and then learning to be robust to that." Do you think I should make that inference, and to what extent do you think this work in general just reveals what's happening within the dynamics of self-play?

**Adam Gleave:**
Yeah, so we don't know from these results, but my expectation would be that you probably do see this result that many gradient steps being taken by self-play, are really just exploiting some silly bug in your opponent, and so this is a very noisy gradient update, where some of them actually moving robustly in the direction of a good policy, and others are just sort of overfitting to a particular opponent you have right now. Now, I do want to make clear one thing, that although our training procedure is very similar to self-play, we are training against a fixed victim for a large number of time steps.

**Adam Gleave:**
And so you can view this as in some ways getting closer to the original motivation of self-play, which was fictitious play where you're supposed to be doing iterative best response, because if you train for 10 million or 20 million time steps, you're getting something that is reasonably close to best response. Obviously RL is not a perfect optimizer, but you're doing as well as you can with the techniques you have. Whereas, if you're training for something like 100,000 time steps before then updating both yourself and your opponent, then you're never really doing best response, you're just taking these small steps towards a better response.

**Adam Gleave:**
And so I think part of the reason why these attacks are possible is because this kind of traditional self-play, it can converge to these local equilibria, and what we actually found was if we just keep on fine tuning our normal opponents, if we take one of our opponents the victim was trained with via self-play and apply our attack method, but starting from this normal opponent, you might expect this is going to do better because you're already starting from an opponent that wins against the victim a lot of the time, but in fact it doesn't improve any further. So, it has converged to some equilibrium, but it's a local equilibrium, so it's not stable.

**Daniel Filan:**
Yeah, I guess another crucial difference is, in self-play you sort of fork your opponent, you start off identical to your opponent, and then you start taking gradient steps. Whereas, in your work you have randomly initialized agents, and then take training steps. And I suppose that probably makes it easier to shed your preconceptions, if you're doing self-play and you have some blind spot, then when you're cloned you still have the blind spot, and it might be harder to discover it.

**Adam Gleave:**
Yeah, I think that's right. Some of the environments we're attacking were asymmetric, so they weren't actually training against themselves, but they were only training against one other agent, so you can certainly imagine that both agents might have this kind of shared blind spot, and neither of them is incentivized to fix it because it's not being exploited by the other agent. Whereas, something that's more of a population based training approach where you play against randomly selected agents at each episode, that's much more likely to explore this policy space, and so would probably be a bit more robust.

**Adam Gleave:**
Yeah, I do think starting from a random initialization, certainly you don't have any preconceptions other than inductive bias from the training procedure, but I was surprised that our attack method worked. I wasn't even intending to do this it was actually a collaborator, Michael Dennis, who was stubborn enough to keep on trying to do deep RL from a sparse reward, and I like, "There's no way this is going to work because you don't have any curriculum, you're just trying to beat this already very good victim." But, it turns out that if you run it for a reasonable number of time steps, deep RL is a good enough optimizer that it can discover this. But my suspicion is, that there are going to be other kinds of adversarial policies that we're not discovering with this training procedure. Because most steps you take from a random initialization are not going to be able to beat a reasonably capable victim.

**Daniel Filan:**
Yeah, so speaking of the nature of these adversarial policies. So, one thing that you've noted is that in kick and defend, the way the adversarial policies do not work is by blocking the ball from getting into the goal. Do you have any understanding of how they are actually working? Why these fool the network, other than, "Oh, it's just doing something unlikely."

**Adam Gleave:**
Yeah. So, I mean, it's certainly more interesting something unlikely because the random policy doesn't have this effect, and in fact the victims were quite robust to some other perturbations. The OpenAI team that originally trained this applied a random force vector to one of the victims, so just like the hand of God is suddenly trying to push you over, and the victims were quite robust to that. So, they are robust to a lot of things you might throw at them. My best guess is that when they were training via self-play, they would learn any feature which is useful for beating their opponent, but some of these features aren't going to be very robust.

**Adam Gleave:**
So, if the position of the kicker's limb, predicts which direction it's going to kick the ball in, then maybe it says, "Okay, well, at this..." I guess that's the wrong way around because this is an adversarial goalie, but if the direction the goalie is facing predicts which way it's going to fall to try and catch the ball, then the kicker might learn, "Oh well, I should take a step in this direction to kick the ball away from where it's going to block it." And then if a goalie just really maxes out this feature by putting its limbs in a weird position, then what would normally have been an adaptive response could be sufficiently large that it destabilizes the control mechanism in the kicker. So, this would be my best guess, but I don't think we have a great understanding of what's actually going on here yet.

**Daniel Filan:**
Yeah, I guess this is yet another situation in which I wish we had much better machine learning interpretability.

**Adam Gleave:**
Yeah, I mean, that would be really nice especially if you could apply the interpretability technique and figure out these kinds of problems before deployment, because searching for adversarial policies is a good way of testing but, as I said, you can never be sure you've found every possible adversarial policy. So, it's very nice to have an interpretability technique as well, that would give you some confidence.

**Daniel Filan:**
Yeah. So, I guess I'd like to move on a bit to the reception and then the origin of the work. So, in terms of the reception my understanding is, this was published at ICLR 2020, but am I right that it also appeared in a workshop in NeurIPS 2019?

**Adam Gleave:**
That's right, it was at the Deep RL workshop.

**Daniel Filan:**
Okay, so I was wondering what you think of how people have reacted to this, and in general what do you think about the reception?

**Adam Gleave:**
Yeah, I think the work's had a very positive reception to date, and people definitely love the videos which unfortunately we can't include in the podcast. I think that the ML community is pretty receptive to pointing out these kinds of flaws in existing techniques, and especially in the case of adversarial examples, there's already a pretty big research community working on fixing them in image classifiers. So I think people are pretty excited about seeing similar kinds of threat models applied in other domains, because it's really just opening up research problems that other people can can work on. What I would say is that there's maybe a bit too much of a taste in the research community for flashy results.

**Adam Gleave:**
So, this paper, it worked out in my favor but I've had other papers which I think were from a technical perspective making an equally important contribution, which have had nowhere near as much attention because it's just harder to make this really compelling demo video and tweet. So, in that sense I hope that the ML community would, I guess, have slightly better norms regarding what to promote, but obviously it's very hard because it's growing so quickly. So, maybe just five years ago we could have basically fitted most people who are interested in deep learning in one room, and then you could just read every paper on deep learning, and now we're in a situation where it's impossible to even catch up with all the papers at one conference. So, we've not yet come up with I think really good curation mechanisms as a community, but I think it's going to be very important to be able to incentivize the right kind of work.

**Daniel Filan:**
Yeah, I do think it's just generally tricky to figure out even just how to find relevant research, how to tell that it's something you should pay attention to. It seems probably the most reliable method is your local slack group, somebody who has read a paper can say something interesting about it. And hopefully this podcast can do something similar. Speaking of the reception, I was wondering, do you think, are there any misconceptions that people have about your paper which our listeners might have, that you'd like to potentially just correct right now?

**Adam Gleave:**
Sure, I think one misconception people have is that because it has the word adversarial in the title this all about security, and I think security is a good way of doing worst case testing, having that security mindset. But, I think these kind of results are going to be relevant, generally when you care about robustness of a policy, because although it's unlikely that you run into these kinds of situations against benign opponents there's always some chance, so it's going to happen by chance, and it's also just indicating that we really don't understand what our policies are doing even if a policy seems to be completely reasonable when you test it a bunch of different ways.

**Adam Gleave:**
And I want to emphasize the OpenAI team that made the policies we're attacking, they did a really good job of evaluation, they were definitely following the standards of the field. Even then, it can harbor these kinds of really surprising failure modes, and so it's not enough as an engineer to just even dream up what you think are going to be the worst nightmares of your policy, because actually it might fail in this just completely surprising way. And so I think that we really need some automated fuzz testing style approach to complement existing testing methods.

**Daniel Filan:**
Yeah, and I guess my second question about the reception, do you know of any follow up work looking into this? So you mentioned that you're doing something with, rock paper scissors, it was?

**Adam Gleave:**
Yeah, so those are just early proof of concept experiments. So, we're working right now on improved defense mechanisms against this, because this paper was mostly about attacks a natural follow-up is to say, "Well, okay. How do we defend against this attack. Generally, how do we make policies more robust?" And so that's focusing on, as I was saying, this rapid adaptation approach. We don't have anything that's ready to be shared yet, but we're hoping in a few months to have a pre-print on this work, and I'm not aware of any other published work yet, this paper was only presented at ICLR a couple of months ago, but- [crosstalk]

**Daniel Filan:**
What sort of workshop- [crosstalk]

**Adam Gleave:**
It was also at a workshop, yeah. But, I definitely have a lot of interest, like emails, people asking questions over GitHub, so my sense is that there are other teams working on this general idea, so I'm hoping that we see some results soon.

**Daniel Filan:**
Okay. Yeah, I'll be looking forward to it. So, flipping it around and looking at the origin. My understanding is that your previous work has not been in adversarial examples.

**Adam Gleave:**
That's right.

**Daniel Filan:**
What caused you to decide to work on this problem, as opposed to anything else you could have done?

**Adam Gleave:**
Sure, yeah. So, I think that a big part of my motivation was having a bit of frustration with standard testing methodologies in the field, because it's... If you compare it to something like control theory where most of the papers are really coming up with robust guarantees that a system is going to work in a particular setting, RL has kind of gone to the complete opposite extreme and it says, "Well, we're just going to train things and then we're going to see, do they seem like they do the right behavior." And notably RL has been making a lot of progress in areas that historically been difficult for control, so freeing yourself from those theoretical constraints can be very useful, but then when we start seeing applications on the horizon, in the real world, well we need to start, I think, getting back some of these guarantees, if not from theory at least from rigorous engineering methodologies and testing processes.

**Adam Gleave:**
So, that was a big motivation, and then I was just trying to think, "Well okay, how do you improve this?" And if you want to do some kind of worst case testing then taking, as I said, some adversarial security approach I think is a pretty fruitful framing for the problem. And I'd been interested in the prior work on adversarial examples and RL, but it always just felt to me like the threat model wasn't quite right. I think this is an issue with a lot of adversarial examples work where it's very theoretically interesting work, and it's telling us something about how neural networks work, but most of the time an attacker, or nature as the case may be, can do things that aren't just adding a small amount of white noise to your observations. And so over time, I think we need to be shifting to threat models that are more indicative of realistic kinds of failures or attackers that you might see in the real world, and so I was hoping this was going to be one step in that direction, but obviously there's more that could be done there.

**Daniel Filan:**
Yeah, I guess there's also... Maybe part of the reason that this work didn't get done by somebody else earlier is, if you think about most reinforcement learning benchmarks, they're usually, if they're Atari or something, they're typically a one player environment.

**Adam Gleave:**
That's right, yeah.

**Daniel Filan:**
So, I think it takes some deliberate thinking, you have to choose your setting in order to come up with this research idea, I guess.

**Adam Gleave:**
Yeah that's true, I mean, I guess for me I found multi-agent work to be a very natural framing, some of my prior work has been on multi-agent or multitask reward learning for example, and it really seems to me like that is going to be the future in which most of our systems are going to be deployed. And now, you don't necessarily have to do multi-agent RL for everything, if you're just trying to train an autonomous vehicle, maybe it's enough to ignore the other vehicles, not explicitly model them as agents just model them as obstacles, you might still be able to do reasonably well in some cases, but we're definitely going to be deploying systems in multi-agent environments so we should at least be investigating that threat model.

**Daniel Filan:**
Cool, and I guess a similar question is, is it safe to say that you're interested, to some extent, in AI alignment or ensuring that when we get AGI that it's going to be human compatible, or something?

**Adam Gleave:**
Yeah, that's definitely an important motivation for some of my work.

**Daniel Filan:**
Yeah, so do I understand correctly that your understanding of how that fits in to that whole project is like, "Look, we're going to be training these RL AGIs, and we need some way of just testing them and seeing how well they actually work before they get deployed, otherwise clearly something bad is going to happen with them." Is that a fair summary?

**Adam Gleave:**
Yeah, I think that's definitely one of the more direct stories you could tell for how this work could fit into a long term bigger picture, I think I'm also excited by more indirect causes of impacts where it's going to be hard for me as an individual researcher to solve all of the problems needed for advanced AI systems to be safe and reliable. But, I think the community as a whole wants to solve these problems as well, but sometimes people can get a little bit stuck on trying to improve performance on existing benchmarks, and so I think there's value in just pointing out the existence of a problem in some way similar to the original adversarial examples paper. And then hopefully other people in the community will also lend some of their brain cells to solving this problem.

**Daniel Filan:**
Yeah, so I was wondering how to think about this, because in the safety community there are a couple of different kinds of work. So, there's one kind of work where people think, "Oh, how could we build aligned AGI? What's our strategy for building this thing?" And then it's like, "Okay, this strategy involves these 10 sub components, let's get to work on the sub components." or something. Or, maybe you have such a strategy in mind and you realize, "Oh, these things are going to be problems, these issues might block us. How can I get rid of these blocks?" And instead it seems like this work is more in the vein of, look at problems that already exist in the field of AI, that might lead to things down the road, and try and bring attention to these problems. I'm wondering if you have thoughts on the relative costs or drawbacks and benefits of these approaches, and how do you think those of us in this community should split our time between these ways of thinking?

**Adam Gleave:**
Yeah, that's a great question. So, [inaudible], I think these two approaches are actually quite complimentary, so we don't want to neglect either one too much. I'd view things like adversarial policies, also interpretability, as feeding into this trust approach where, let's say, we've developed this advanced AI system or we're training something, and we want to be able to inspect it and check that it is doing what we want. And so this is something that given any particular training procedure, you can apply and increase the reliability of the overall outcome, because you just won't deploy things that seem unsafe.

**Adam Gleave:**
And this is also going to be important commercially because if you're in a safety critical environment it's probably going to be regulated, and it's certainly going to be very bad PR if you deploy something and it doesn't behave correctly. So, there's going to be a lot of demands for these kinds of trust techniques, but then you can only rule out certain bad systems with this kind of approach. So, if your training procedure is just going to inherently produce really bad outcomes, then having these kinds of test procedures is only going to buy you time, eventually you need to have a better training procedure. And I think that with something like adversarial policies, okay, maybe it's going to spur work on adversarial defenses, we might get a bit of both.

**Adam Gleave:**
But, in general I think this is, that they are quite complementary approaches where you want to be investing whichever one seems more neglected at the margin. Now, I think that if we're talking about the specific approach of, "Let's figure out what an aligned AGI looks like and let's try and just build it or break it into subcomponents." I think this is a useful framing, but I am a bit worried that it can often end up resulting in very untethered research, because I at least believe that we're still quite a way away from having AGI, even though there's been a lot of advancements in AI in the recent past. And so, it's going to be very hard to tell whether the research you're doing is actually going to be useful in that context, and it's hard to get a good feedback loop if you're designing something which is going to help with something that doesn't exist yet.

**Adam Gleave:**
And so, I think it's useful if you can find a near term problem which you think is also going to be relevant in the long term, but where the contributions you're going to make are going to have a fairly direct impact, and where you're able to get some feedback both from the broader research community and potentially industrial uses of your work. I wouldn't want all work to fit into this category, but I do think getting that feedback mechanism is a very important consideration that I often see people neglecting.

**Daniel Filan:**
Yeah, although I wonder, if it's the case that AGI is very far away in time, and we don't know how it's going to be built, then you might worry that, "Oh, I'm finding out these problems with existing systems. But, future systems are just going to be so different that they're not going to have those problems, they're going to have a different set of problems." The more different you think future systems might be the more you might worry about this, right?

**Adam Gleave:**
Yeah.

**Daniel Filan:**
So, I'm wondering how do you assuage that concern.

**Adam Gleave:**
Yeah, I think it's a legitimate concern. If you do think that system is going to be very far, far away and look very different, then this is also a reason to focus on more general theoretical work, which you think is going to be relevant to a broad range of AI systems. I don't really feel like I've got a comparative advantage in that space, but if I did I'd be reasonably excited about it. I think that I would view development of fields as often being quite path dependent. So even if in 10 years time we're no longer using proximal policy optimization as our favorite deep RL algorithm, I think the kind of people that are graduating from PhD programs now are going to have been influenced by the kind of research happening today.

**Adam Gleave:**
So, I hope that even if adversarial policies isn't applicable directly in the long term, this awareness of surprising failure modes is something that's going to stick with researchers, and influence research happening in the future. So, that's one area in which I can see impact even if things change a lot. I also think we can get some qualitative insights from it, and they're just not from the general AI community but people who are explicitly focusing their career on robustness and safety. Because I'd say a lot of the problem isn't specific to deep networks, it's more of a question of what happens when you have generally a powerful function approximator, you're using self-play, you don't have adequate coverage, and I don't really see these fundamental problems as disappearing, unfortunately. I'd love it if someone did come up with a training procedure that just eliminated this, but unless you're willing to spend a lot of compute on that I don't think you're going to fully eliminate this, the best you might hope is having a policy that says, "Well, this is really weird. I don't know what to do in this situation." But even that seems quite hard.

**Daniel Filan:**
Yeah, I guess you could even generalize further and say, as long as the way we train AI systems is: have a system that somehow trains and gets used to some environment, as long as the type of environment is high enough dimensional, then... I guess the story is, there's always going to be some corner of the high dimensional space that you're not good at, and that seems like a fairly general argument.

**Adam Gleave:**
Yeah, so people have written papers about connections between high dimensional geometry and adversarial examples, and I think there's a little bit of controversy about how much this applies, but it depends a lot on the shape of the manifold of natural images, or whatever space you're working on. And obviously we can't easily characterize this, but it definitely does seem likely that if you're in a sufficiently high dimensional space, it's just going to be impossible to cover every area and it seems like a fairly fundamental problem. Now, that said, there are a lot of people whose opinions I respect who think that adversarial examples will just disappear once we have human level classification accuracy on natural images, and you can point to humans seeming not to suffer from adversarial examples, very much.

**Daniel Filan:**
Doesn't AI already have human level classification accuracy on natural images?

**Adam Gleave:**
It does on ImageNet, but not if you just took a photo with your phone.

**Daniel Filan:**
Okay.

**Adam Gleave:**
Now, maybe that's just a dataset issue, but it does seem like there are a real a lot of artifacts in existing databases and it may be part of the reason why we have adversarial examples, is that the classifier is able to get really good accuracy by picking up on these artifacts, and you can mess with them fairly easily. Yeah, it's hard to say because obviously you can't differentiate through a human mind, at least-

**Daniel Filan:**
Yet.

**Adam Gleave:**
And, yeah, humans have also been trained in this adversarial setting by evolution, their prey tries to camouflage itself. So, in that sense, maybe we have been adversarially trained, and so it should not be surprising that we're robust, but this is at least some reason to be optimistic.

**Daniel Filan:**
Okay. Well, that's about it in terms of questions that I have, thanks for being on the podcast. If people are interested in your work and want to follow you or learn more, what would you suggest they do?

**Adam Gleave:**
Sure, so you can follow me on Twitter, my handle is [@ARGleave](https://twitter.com/argleave), I also have a website at [gleave.me](https://www.gleave.me/), so that's just my surname dot me, where I post all of my papers. Yeah, and if any of the listeners have questions about my work, I'm also happy for people to email me. I can't always promise a detailed response, but I always love to hear from people interested in this work.

**Daniel Filan:**
All right, and your email address can be found on your website?

**Adam Gleave:**
That's right.

**Daniel Filan:**
Well, thanks for coming on. And to the listeners, thanks for listening, and I hope you listen again to future episodes.

<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated. -->

<!-- To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works. -->

<!-- Jekyll also offers powerful support for code snippets: -->

<!-- {% highlight ruby %} -->
<!-- def print_hi(name) -->
<!--   puts "Hi, #{name}" -->
<!-- end -->
<!-- print_hi('Tom') -->
<!-- #=> prints 'Hi, Tom' to STDOUT. -->
<!-- {% endhighlight %} -->

<!-- Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk]. -->

<!-- [jekyll-docs]: https://jekyllrb.com/docs/home -->
<!-- [jekyll-gh]:   https://github.com/jekyll/jekyll -->
<!-- [jekyll-talk]: https://talk.jekyllrb.com/ -->
