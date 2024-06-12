---
layout: post
title: "32 - Understanding Agency with Jan Kulveit"
date: 2024-05-29 20:40 -0700
categories: episode
---

[YouTube link](https://youtu.be/ZnHt70LREBE)

What's the difference between a large language model and the human brain? And what's wrong with our theories of agency? In this episode, I chat about these questions with Jan Kulveit, who leads the Alignment of Complex Systems research group.

Topics we discuss:
 - [What is active inference?](#what-is-active-inference)
 - [Preferences in active inference](#prefs-in-active-inference)
 - [Action vs perception in active inference](#action-vs-perception-in-active-inference)
 - [Feedback loops](#feedback-loops)
 - [Active inference vs LLMs](#active-inference-vs-llms)
 - [Hierarchical agency](#hierarchical-agency)
 - [The Alignment of Complex Systems group](#acs)

**Daniel Filan:**
Hello, everybody. This episode, I'll be speaking with Jan Kulveit. Jan is the co-founder and principal investigator of the Alignment of Complex Systems Research Group, where he works on mathematically understanding complex systems composed of both humans and AIs. Previously, he was a research fellow at the Future of Humanity Institute focused on macrostrategy, AI alignment, and existential risk. For links to what we're discussing you can check the description of this episode and you can read the transcript at axrp.net. Okay. Well Jan, welcome to the podcast.

**Jan Kulveit:**
Yeah, thanks for the invitation.

## What is active inference? <a name="what-is-active-inference"></a>

**Daniel Filan:**
I'd like to start off with this paper that you've published in December of this last year. It was called ["Predictive Minds: Large Language Models as Atypical Active Inference Agents."](https://arxiv.org/abs/2311.10215) Can you tell me roughly what was that paper about? What's it doing?

**Jan Kulveit:**
The basic idea is: there's active inference as a field originating in neuroscience, started by people like [Karl Friston](https://en.wikipedia.org/wiki/Karl_J._Friston), and it's very ambitious. The active inference folks claim roughly that they have a super general theory of agency in living systems and so on. And there are LLMs, which are not living systems, but they're pretty smart. So we're looking into how close the models actually are. Also, it was in part motivated by... If you look at, for example, the ['simulators' series](https://www.alignmentforum.org/posts/nmMorGE4MS4txzr8q/simulators-seminar-sequence-1-background-and-shared) or frame by [Janus](https://x.com/repligate) and these people on sites like the [Alignment Forum](https://www.alignmentforum.org/), there's this idea that LLMs are something like simulators - or there is another frame on this, that LLMs are predictive systems. And I think this terminology... a lot of what's going on there is basically reinventing stuff which was previously described in active inference or predictive processing, which is another term for minds which are broadly trying to predict their sensory inputs.

And it seems like there is a lot of similarity, and actually, a lot of what was invented in the alignment community seems basically the same concepts just given different names. So noticing the similarity, the actual question is: in what ways are current LLMs different, or to what extent are they similar or to what extent are they different? And the main insight of the paper is... the main defense is: currently LLMs, they lack the fast feedback loop between action and perception. So if I have now changed the position of my hand, what I see immediately changes. So you can think about [it with] this metaphor, or if you look on how the systems are similar, you could look at base model training of LLMs as some sort of strange edge case of active inference or predictive processing system, which is just receiving sensor inputs, where the sensor inputs are tokens, but it's not acting, it's not changing some data.

And then the model is trained, and it maybe changes a bit in [instruct fine-tuning](https://arxiv.org/abs/2203.02155), but ultimately when the model is deployed, we claim that you can think about the interactions of the model with users as actions, because what the model outputs ultimately can change stuff in the world. People will post it on the internet or take actions based on what the LLM is saying. So the arrow from the system to the world, changing the world, exists, but the feedback loop from the model acting to the model in learning is not really closed, or at least not really fast. So that's the main observation. And then we ask the question: what we can predict if the feedback loop gets tighter or gets closed?

**Daniel Filan:**
Sure. So the first thing I want to ask about is: this is all comparing what's going on with large language models to active inference. And I guess people, probably most listeners, have a general sense of what's happening with language models. They're basically things that are trained to predict completions of text found on the internet. So they're just very good at textual processing. And then there's a layer of "try to be helpful, try to say true things, try to be nice" on top of that. But mostly just predicting text data from the internet given the previous text. But I think people are probably less familiar with active inference. You said a little bit about it, but can you elaborate: what is the theory of active inference? What is it trying to explain?

**Jan Kulveit:**
Yep. So I will try it, but I should caveat it, I think it's difficult to explain active inference in two hours. I will try in a few minutes. There is now actually [a book](https://www.goodreads.com/en/book/show/58275959) which is at least decent. A lot of the original papers are sort of horrible in the ways in which they're presenting things. But now there is a book, so if you are interested more in active inference, there is a book where at least some of the chapters are relatively easy to read and written in a style which is not as confusing as some of the original papers.

**Daniel Filan:**
What's the book called?

**Jan Kulveit:**
It's called ['Active Inference: The Free Energy Principle in Mind, Brain and Behavior.'](https://www.goodreads.com/en/book/show/58275959) But the main title is just Active Inference.

**Daniel Filan:**
And who's it by?

**Jan Kulveit:**
It's by Karl Friston and Thomas Parr and Giovanni Pezzulo.

So, a brief attempt to explain active inference: so, you can think about how human minds work. Historically, a lot of people were thinking [that] when I perceive stuff, something like this happens: some photons hit photoreceptors in my eyes, and there is a very high bitrate stream of sensory data, and it passes through the layers deeper in the brain, and basically a lot of the information is processed in a forward way, that the brain processes the inputs to get more and more abstract representations. And at the end is some fairly abstract, maybe even symbolic representation or something like that. So that's some sort of classical picture which was prevalent in cognitive science, as far as I understand, for decades. And then some people proposed [that] it actually works the opposite way with the brain, where the assumption is that the brain is basically constantly running some generative world model, and our brains are constantly trying to predict sensory inputs.

So in fact... for example, now I'm looking at a laptop screen and I'm looking at your face. The idea is: it's not like my brain is trying to process every frame, but all the time it's trying to predict "this photoreceptor will be activated to this level", and what's propagated in the opposite direction is basically just the difference. So it's just prediction error. So for this reason, another term in this field, which some people may have heard, is "predictive processing". There is a long Slate Star Codex post [review of a book called Surfing Uncertainty by Andy Clark](https://slatestarcodex.com/2017/09/05/book-review-surfing-uncertainty/). So that's a slightly older frame, but Surfing Uncertainty is probably still the best book-length introduction to this field in neuroscience. So the basic claim is: I am constantly trying to predict sensory inputs, and I'm running a world model all the time.

And then active inference makes a bold and theoretically elegant move: if I'm using this machinery to predict sensory inputs, the claim is that you can use basically the same machinery to basically predict or do actions. So, for example, let's say, I have some sort of forward-oriented belief that I will be holding a cup of tea in my hand in a few seconds. Predictive processing just on the sensory inputs level would be like, "Okay, but I'm not holding the cup". So I would update my model to minimize the prediction error. But because I have some actuators - hands - I can also change the world so it matches the prediction. So I can grab the bowl, and now I'm holding a bowl, and the prediction error goes down by basically me changing the world to match my model of what the world should be. And the bold claim is: you can basically describe both things by the same equations, and you can use very similar neural architecture in the brain, or very similar circuitry, to do both things.

So I would say that's the basic idea of active inference, the claim that our brains are working as approximately Bayesian prediction machines. I think predictive processing - just [the claim] that we are predicting our sensory inputs - I think this is fairly non-controversial now in neuroscience circles. I think active inference - the claim that the same machinery or the same equations are guiding actions - is more controversial: some people are strong proponents of it, some people are not. And then there are, I would say, over time more and more ambitious versions of active inference developed. So currently Karl Friston and some other people are basically trying to extend the theory to a very broad range of systems, including all living things and ground it in physics. And with some of the claims, my personal view is, I'm not sure if it's not over-extensive, or if the ambitions to explain everything with the free energy principle, if the ambition isn't too bold. But at the same time I'm really sympathetic to some effort like "let's have something like physics of agency" or "let's have something like physics of intelligent systems".

And I think here also some connection to alignment comes in, where I think our chances to solve problems with aligning AI systems would be higher if you had basically something which is in taste more like physics of intelligent systems than if we have a lot of heuristics and empirical experiences. So, back to active inference, it is based on this idea, and there is some mathematical formalism, and it needs to be said, I don't think the mathematical formalism is fully developed, I don't think it's a finished theory which you can just write down in a textbook. My impression is it's way more similar to how I imagine physics looked in the 1920s, where people were developing quantum mechanics and a lot of people had different ideas and it was confusing what formulations are equivalent or what does it mean in practice.

And I think if you look at a theory in development, it's way more messy than how people are used to interacting with theories which were developed a hundred years ago and distilled into a nice, clean shape. So I don't think the fact that active inference doesn't have the nice clean shape yet it is some sort of very strong evidence that it's all wrong.

## Preferences in active inference <a name="prefs-in-active-inference"></a>

**Daniel Filan:**
Gotcha. One question I have about the active inference thing is: the claim that strikes me as most interesting is this claim that action as well as perception is unified by this minimization of predictive error, in basically the same formalism. And a thing that seems wrong to me or questionable to me at least is: classically, a way people have talked about the distinction is "[direction of fit](https://plato.stanford.edu/entries/desire/#DirFitDes)T".

So in terms of beliefs, suppose that reality doesn't match my beliefs, my beliefs are the ones that are supposed to change; but in terms of desires or preferences, when I act I change reality so as to match my desires rather than my desires to match reality. So to me, if I try and think of it in terms of a thing to minimize predictive error, with perception you said that the differences between the predictions in reality are going from my perceptions back up to my brain, whereas it seems like for action that difference has, I would think, it would have to go from my brain to my hand. Is that a real difference in the framework at least?

**Jan Kulveit:**
So how it works in the framework, it's more like you can do something like conditioning on future state: conditional on me holding a cup of tea in my hand, what is the most likely position of my muscles in the next... Similar to me predicting in the next frame what my activation of photoreceptors is. I can make inferences of the type "conditional on this state in future, what's the likely position of my muscles or some actuators in my body?" And then this leads to action. So I think in theory there is some symmetry where you can imagine some deeper layers are thinking about more macro actions, and then the layers closer to the actual muscles are making more and more detailed predictions about how specific fibers should be stretched and so on. So I don't see a clear problem at this point.

I think there is a problem of how do you encode something like preferences? Where, by default, if you would not do anything about what we have as preferences, the active inference system would basically try to make its environment more predictable. It would explore a bit so it understands where its sensor inputs are coming from, but the basic framework doesn't have built in some drive to do something evolutionary useful. [This] is solved in a few different ways, but the main way... So in the original literature, how is it solved? It's called, and I think it's a super unfortunate choice of terminology, but it's solved by a mechanism of 'fixed priors'.

So the idea is, for example, evolution somehow... So let's say my brain is receiving some sensory inputs about my bodily temperature, and the idea is that the prior about this kind of sensory inputs is evolutionarily fixed, and it means that if my body temperature goes down, and I just don't update my world model or my body model and I can't just be okay with it. But this prediction error term would never go... the belief will basically never update: that's why it's called 'fixed'. And I think the word 'prior' is normally used to mean something a bit different, but basically, you have some sort of fixed point or fixed belief, and this is driving the system to adjust the reality to match the belief. So by this, you have some sort of drive to action, then you have the machinery going from some high-level trajectory to more and more fine-grained predictions of individual muscles or individual words. So that's the basic frame.

**Daniel Filan:**
So there's some sort of probability distribution thing which you may or may not want to call a prior, and maybe the fixed prior thing is a bit abstract... I guess for things like body temperature it has to be concrete in order for you to continuously be regulating your body temperature. But to explain why different people go into different careers and look at different stuff.

**Jan Kulveit:**
I think the part of the fixed prior or this machinery makes a lot of sense if you think about... So this is my guess at the big evolutionary story: if I personify evolution a bit, I think evolution basically needed to invent a lot of control theory for animals, for simple organisms or simple animals without these expensive, energy-hungry brains. So I think evolution implemented a lot of control theory and a lot of circuitry to encode evolutionarily advantageous states by chemicals in the blood or evolutionarily older systems.

So you can imagine evolution has some sort of functional animal which doesn't have an advanced brain. So let's say then you invent this super generic predictive processing system which is able to predict sensory inputs. My guess is you obviously just try to couple the predictive system to the evolutionarily older control system. So it's not like you would start building from scratch, but you can plug in some inputs, which would probably mean some sort of interoceptive inputs from the evolutionarily older mechanisms or circuits, and you feed that into the neural network, and the neural network is running some very general predictive algorithm. But by this mechanism, you don't need to solve how to encode all the evolutionarily interesting states, how to communicate them to the neural network, which is difficult.

But there are not enough bits in the DNA to specify, I don't know, what career you should take or something like that. But there are probably enough bits to specify - for some simpler animal, there are probably enough bits to specify that the animal should seek food and mate and keep some bodily integrity and maybe in social species try to have high status, and this seems enough. And then if you couple this evolutionarily older system with the predictive neural network, the neural network will learn more complex models. So for example, with the fixed prior on body temperature - you can imagine this is the thing which was evolutionarily fixed, but over time I learned stuff like, "okay, it's now outside maybe 10 degrees celsius". So I sort of learn a belief that in this temperature I'll typically wear a sweater or a jacket outside, and this sort of belief basically becomes something like a goal.

When I'm going outside, I will have this strong belief that I will probably have a sweater on me. So in the predictive processing/active inference frame, this belief that when I will be outside I will have some warm clothing on me, causes the prediction that I will pick up the clothing when going outside. And then you need coupling with the evolutionary prior, just basically just for bootstrapping. But over the lifetime, I have a learned network and it follows sensible policies in the world, and the policies don't need to be hard-coded by evolution. So that's my guess.

**Daniel Filan:**
So I guess the picture is something like: we have these fixed priors on relatively simple things: have a comfortable body temperature, have offspring, have enough food to eat. But somehow the prior is that that is true 50 years from now or five years from now or something. And in a complicated world where different people are in different situations, the predictions you make about what's happening right now, conditioned on those kinds of things holding multiple years in the future, in this really complicated environment, that's what explains really complex behavior and different behavior by different people.

**Jan Kulveit:**
Also, I don't know, maybe it's a stretch, but one metaphor which I sometimes think about is imagining the evolutionarily older circuitry as some sort of, I don't know, 50,000 lines of some Python code implementing the immune system and various chemicals released in my blood if stuff happens and so on. So you have some sort of cybernetics or some sort of control system which is able to control a lot of things in the body, and you make the coupling on some really important variables and then it works the way you described.

**Daniel Filan:**
Sure. So this is a weird question, but on this view, why are different people different? I observe that different people are differently skilled at different things. They seem like they have different kinds of preferences. It seems like there's more variation among humans than I think I would predict, just based off of [the fact that] people are in slightly different situations, if they all had the same underlying evolutionary goals that they were backpropagating to predicting the present.

**Jan Kulveit:**
They have very different training data. In this picture, when the human is born the predictive processing neural substrate is in a state, which, I don't know, it's not a 100% blank slate, but it doesn't need to have too many priors about the environment. In this picture, you need to learn how to move your hands or how different senses are coupled. You learn a lot of the dynamics of the environment. And also what I described so far, I think it's fairly fitting for (let's say) animals, but I think humans are unique because of culture. So my model for it is: the predictive processing substrate is so general that it can also learn to predict into this weird domain of language. So again, a slightly strange metaphor would be if you are learning to play a video game, most humans' brains are so versatile, even if the physics in the game works differently and there is some bunch of unintuitive, or not really the same as natural world dynamics, our brains are able to pick it up. You can imagine, in a similar way as we are able to learn how to drive a car or something, brains are also able to pick up this super complex and super interesting domain of language and culture.

Again, my speculation is this gives us something like another implicit world model based on language. Let's say, if you tell me to imagine some animal in front of me, my simple model is: there is this language-based representation of the world and some sort of more spatial representation and there is some prediction mismatch between them. You have another model running on words and language [which] also implicitly is a world model. This adds a lot of complexity to what people want or what sort of concepts we use.

But I think a lot of our explanation of why people want different things... I think a lot of it's explained just by different data. People are born in different environments. Unfortunately some of the environments are, for example, I don't know, less stable or more violent. You can imagine if someone as a kid is in some environment which is less stable, people learn different priors about risk. And you can explain a lot of strategies just by different training data. But I think the cultural evolution layer, it's another important part of what makes humans, humans.

## Action vs perception in active inference <a name="action-vs-perception-in-active-inference"></a>

**Daniel Filan:**
Gotcha. I definitely want to talk about cultural evolution, but a little bit later. I guess I still have this question about prediction and action in the predictive processing framework - or in the active inference framework rather - and to what degree they're unified. If I'm trying to think about how it would work, it seems to me that in order for... What's the difference between my eyes and my hands? It seems like for the prediction error mismatch to work properly, the difference between prediction and reality has got to go from my eye to my brain so that my beliefs can update. But it's got to go from my brain to my hand so that I can physically update the world. And it seems like that's got to be the difference between action organs versus understanding the world organs. Does that sound right?

**Jan Kulveit:**
I don't know. Maybe it's easier to look at it with a specific example. If I take a specific example of me holding the cup, if the prediction is... if there is some high-level prediction where, I don't know, I am imagining my visual field contains the hand with the cup, I think the claim is the maths is similar in the sense that you can ask - why is it called inference? You can ask the question conditional on that state in future, what's the most likely position of my muscles?

And then how it propagates in the hierarchy would be like: okay, there is some broad, coarse-grained position of my muscles. And you can imagine the lower layers filling in the details, like how specific muscle fibers should be contracted or something. But I don't know, to me this doesn't sound - the process by which you start with somewhat more abstract representation and you fill in the details, I think this sounds to me actually fairly similar to what would happen with the photoreceptors. And then the prediction error propagated back would be mostly about, for example, if the hand is not in the position I assume it to be, it would work as some control system trying to move the muscles into the exactly correct position.

**Daniel Filan:**
But it seems like there's got to be some sort of difference, where suppose I have this prediction that my visual field contains a cup, and the prediction is currently off but I have a picture of a cup next to me. It's not supposed to be the case that I then look at the picture of the cup and now everything's good. My hand's the thing that's supposed to actually pick up the cup and my eyes are supposed to tell my brain what's happening in the world. It at least seems like those have got to interface with the brain differently.

**Jan Kulveit:**
I'm slightly confused with the idea of the... There is some picture of a cup which is different from the actual cup? Or what situation?

**Daniel Filan:**
Yeah. We're imagining that I'm going to pick up a cup. And there's a physical cup in front of me and next to me there's actually a picture of that same cup but it's a picture of my hand holding the cup. And the thing that's supposed to happen when I predict really hard that in half a second I'm going to be holding the cup is that my eyes are constantly sending back to my brain, "okay, in what way is the world different from me currently holding the cup?"

And my muscle fibers are moving so that my hand actually holds the cup. And what's not supposed to happen is that my motor fibers are sending back "here's what's going on" and then my eyes are looking towards the picture of my hand holding the cup. That would be the wrong way to minimize prediction error, if the hope is that I end up actually picking up the cup.

**Jan Kulveit:**
The thing is I think in practice it's not that common that there would be the exact picture of your hand holding the cup. I'm not sure how widely known it is, but there is this famous set of [rubber hand experiments](https://en.wikipedia.org/wiki/Body_transfer_illusion). How it works is you put the rubber hand in people's visual field and you basically hide their actual hand from them. And then you, for example, gently touch the rubber hand and at the same time the assistant is gently touching the physical hand of the test subjects.

And the rubber hand to me sounds a lot like the picture of the hand with the cup you are imagining, where the system is not so stupid to be fooled by a static picture. If the picture is static then probably it would not fit in your typical world model. But the rubber hand experiments seem to show something like if the fake picture is synchronized, so different sensory modalities match, it seems like people's brains basically start to assume the rubber hand is the actual hand.

And then, I don't know, if someone attacks the rubber hand with a knife or something, people actually initially feel a bit of pain and obviously they react similarly to if it was their actual hand. I don't think it's that difficult to fool the system if you have some sort of convincing illusion and the reality. I think it's maybe not that difficult to fool the system.

I just don't think with the thing you described, a very realistic image of the cup which would just fill my visual field and I would have no reason to believe it's not real - I think this doesn't exist that often in reality. But maybe if it was easy to create, people would fall into some sort of wireheading traps more often.

**Daniel Filan:**
Yeah. I mean it's not whether it exists: my question is... so in the rubber hand case, if I see someone coming with a knife to hit my hand, the way I react is I send these motor signals out to contract muscle fibers. But the way I react is not, I look at a different hand. The messages going to my eyes, they're minimizing predictive error, but in a very different way than the messages to my muscle fibers are minimizing predictive error. At least so I would've thought.

Now maybe there's some unification where the thing my brain is sending to my eyes is predictions about what that should be. And there's some, I don't know, eye machinery or optic nerve machinery that turns that into messages being sent back which are just predictive error. But when my brain sends those predictions to the muscle fibers, the thing the muscle fibers do is actually implement those predictions. Maybe that's the difference between the eye and the muscle fibers, but it seems like there's got to be some kind of difference.

**Jan Kulveit:**
I think the difference is mostly located roughly at the level of how photoreceptors are different from muscles. I think the fundamental difference is located on the boundary. Imagine your muscles somehow switched off their ability to contract and they would be just some sort of passive sensors of the position of your hand or something, and someone else would be moving your hand. You would still be getting some data about the muscle contractions.

Let's imagine this is for some weird reason the original state of the system, that the muscles don't contract. Then you can imagine in this mode, the muscles work very similarly to any other sense. They just send you some data about the contraction of the fibers. In this mode it's exactly the same as with the sensory inputs. Then, if someone else was moving your hand, your brain would be predicting "okay, this interoceptive sensation is this".

And then imagine the muscles start to act just a little bit. If the muscle gets some bit of "okay, you should be contracted to 0.75" or something. And the muscle is like, "okay, but I'm contracted just to 0.6". And the muscle gets some ability to change to match the prediction error, you get some sort of a state which now became the action state, or the action arrow happened.

But you can imagine this continuous transition from perception to action. You can be like: okay, lots of the machinery, how to do it, lots of the neural machinery could probably stay the same. But I do agree there is some fundamental difference, but it doesn't need to be located deep in the brain but it's fundamentally on the boundary.

**Daniel Filan:**
Yeah. If I think of it as being located on the boundary, then it makes sense how this can work. This is almost a tangent, but I still want to ask: are there any... It seems like under this picture there should be things that are parts of my body that are intermediate between sensory organs and action organs, or sensory tissue and active tissue or whatever it is. Do we actually see that?

**Jan Kulveit:**
I don't know. My impression is, for example, the muscles are actually also a bit of the sensory tissue. Or I can probably... even if I close my eyes, I have some idea about the positions of my joints and I'm getting some sort of interoceptive thing. But I don't know, I think the more clear prediction this theory makes is there should be beliefs which are something between... or something which is a bit like a belief type, but this predicts we should be basically doing some amount of wishful thinking by default, or because of how the architecture works.

This predicts that if I really hope to see a friend somewhere, maybe I will more easily hallucinate other people's faces as my friend's face. And I don't know, if I have some longer-term goal, my beliefs will be... I think this single thing probably if you take it as some sort of, I don't know, architectural constraint, how the architecture works, I think it explains quite a lot of the so-called, traditionally understood 'heuristics' in the heuristics and biases literature.

There is [this page on Wikipedia](https://en.wikipedia.org/wiki/List_of_cognitive_biases) with maybe hundreds of different biases. But if you take this as humans by hardware design have a bit of trouble distinguishing between what they would wish for and what they expect to happen, and a lot of the cognition in between is some sort of mixture between pure predictions and something which would be a good prediction if some of the goals would be fulfilled, I think this explains a lot of what's traditionally understood as some sort of originating-from-nowhere bias.

## Feedback loops <a name="feedback-loops"></a>

**Daniel Filan:**
Sure. Moving out a little bit: you're writing this paper about trying to think of large language models through the frame of active inference. But there are potentially other frames you could have picked. Or you were interested in active inferences as a 'physics of agency' kind of thing, but there are other ways of thinking about that. Reinforcement learning is one example, where I think a lot of people think of the brain as doing reinforcement learning and it's also the kind of thing that you could apply to AIs.

**Jan Kulveit:**
I think there is this debate people sometimes have: "what's the more fundamental way of looking at things?" Where in some sense, reinforcement learning in full generality is so extremely general that you can say... if you look at the actual math or equations of active inference, I think you can be like "this is reinforcement learning, but you implement some terms like tracking information in it".

And I think in some sense the equations are compatible with looking at things... I don't know, I like the active inference-inspired frame slightly more, which is maybe personal aesthetic preference. But I think if you start from a reinforcement learning perspective, it's harder to conceptualize what's happening in the pre-training where there is no reward. I think the active inference frame is a fruitful frame to look at things. But whereas the debate, is it fundamentally better to look at things as a combination of... you could be like "okay, it's a combination of reinforcement learning and some self-supervised pre-training". And maybe if you want you can probably claim it all fits some other frame.

Why we wrote the thing about active inference and LLMs was... one motivation was just the simulators frame of thinking about the systems became really popular. There is another very similar frame looking at the systems as predictive models. And my worry is a lot of people... or I don't know if a lot, but at least some people basically started to think about safety ideas, taking this as a pretty strong frame, assuming that "okay, we can look at the systems as simulators and we can base some safety properties on this".

And one of the claims of the paper is: the pure predictive state is unstable. Basically if you allow some sort of feedback loop, the system will learn, the active inference loop will kick in and you'll gradually get something which is agenty. Or in other words, it's trying to pull the world in its direction, similarly to classical active inference system. The basic prediction of it is: point 1, as you close the feedback loop and more bits are flowing through it, the more you get something which is basically an agent or which you would describe as an agent.

And it's slightly speculative, but the other observation is to what extent you should expect self-awareness or the system modeling itself. And here the idea is: if you start with something which is in the extreme edge case of active inference, which is just something which is just perceiving the world, it's just receiving sensory inputs, it basically doesn't meet a causal model of self. If you are not acting in the world, you don't need a model of something which would be the cause of your actions. But once you close the feedback loop, our prediction is you get something which is more self-aware or also understands its position in the world better.

A simple intuition pump for it is: you can imagine the system which is just trained on sensory inputs and it's not acting. You can imagine your sensory inputs consist of a feed of hundreds of security cameras. You're in a large building, and for some reason all you see are the security cameras. If you are in this situation, it could be really tricky to localize yourself in the world. You have a world model, you'll build a model of the building. But it could be very tricky to see, to understand "this is me".

And one observation is: the simplest way - if you are in the situation [where] you need to localize yourself on many video surveillance cameras' feed, probably the simplest way to do this [is] wave your hand. Then you see yourself very fast. Our prediction in the paper is if you close the feedback loop, you will get some nice things, we can talk about them later. But you will also get increased self-awareness and you will get way better ability to localize yourself, which is closely coupled with [situational awareness](https://www.alignmentforum.org/tag/situational-awareness-1) and some properties of the system which will be probably safety-relevant.

**Daniel Filan:**
Although it strikes me that you might be able to have a self-concept without being able to do action. Take the security camera case. It seems like one way I could figure out where I was in this building is to just find the desk with all the monitors on it and presumably that's the one that I'm at.

**Jan Kulveit:**
Another option is, for example, if you know some stuff about yourself: if you know "I am wearing a blue coat and I am wearing headphones" or something. I think there are ways by which you can localize [and] see yourself, but it's maybe less reliable and it's slower. But again, what we discussed in the paper is basically you get some sort of very slow and not very precise feedback loop just because new generations of LLMs are trained on text, which contains interactions with LLMs.

If you are in the situation that you have this feed. And you know you... I don't know, maybe it's easier to imagine in the text. If you read a lot of text about LLMs and humans interacting with LLMs, it's easier for you... even in runtime, you have this prior idea of a text-generating process, which is an LLM. And when you are in runtime, it's easier to figure out, "okay, maybe this text generating process which has these features, it's probably me".

I think this immediately makes one prediction, which seems like it's actually happening: because of this mechanism, you would probably assume that most LLMs when trained on text on the internet, by default, their best guess about who they are would be ChatGPT or GPT-4 or something trained by OpenAI. And it seems like this prediction actually works, and most other LLMs are often confused about their identity.

**Daniel Filan:**
They call themselves ChatGPT?

**Jan Kulveit:**
Yeah, yeah, yeah. Obviously, the labs are trying to fine-tune them not to do this. But their deep... I think the metaphor works, but that's a minor point.

**Daniel Filan:**
Sure. One thing that puzzled me about this paper is, so you talk a lot about... Okay, currently this loop from LLM action to LLM perception is open, but if you closed it then that would change a bunch of things. And if I think about fundamentally what an LLM is doing in normal use, it's producing... It's got some context, it predicts the next token or whatever. Then the next token actually gets sampled from that distribution. And then it gets added to the context and then it does it again. And it strikes me that that just is an instance of a loop being closed where the LLM "acts" by producing a token and it perceives that the token is there to be acted on. Why isn't that enough of closing the loop?

**Jan Kulveit:**
I mean I think it sort of is, but typically... You close the loop in runtime, but then by default this doesn't feed into the way it's being updated based on... It's a bit like if you had just some short-term memory, but it's not stored. My guess here is you get the loop closed in runtime. And my guess here is something like, okay, if the context is large enough and you talk with the LLM long enough, it will probably get better at agency. But there is something which is difficult: the pre-training is way bigger. And the abstractions and deep models which the LLMs build are mostly coming from the pre-training, which lacks the feedback loop.

And I don't know, this is a super speculative guess, but my guess is it's pretty difficult if you are trained just on perception. I think it's difficult to get right some deep models of causality. If your models of reality don't have the causal loop, with you acting from the beginning, my guess is, it's difficult to learn it really well later, or at the end. At the same time, I would expect in runtime, LLMs should be able... if the runtime is long enough, I wouldn't be surprised if they got better at understanding who they are and so on. I actually heard some anecdotal observations like that, but not sure to what extent I can quote the context.

**Daniel Filan:**
Sure. So it seems like the basic thing is: we should expect language models to be good at dealing with stuff which they've been trained on. And if they haven't been trained on dealing with this loop, at least for the bulk of their training, we shouldn't expect them to deal with it. But if they have, then we should. Is that a decent summary?

**Jan Kulveit:**
Yeah, I think it's a decent summary. I think the basic thing is, if in the bulk of your training, you don't have the loop, it's really easy to be confused about the causality. So there is this thing which is also well-known, that if the model hallucinates, I don't know, some experts at something, it basically becomes part of the context. And now it's indistinguishable from your sensory input. So you get confused in a way in which... As humans, we are normally not confused about this.

What I found fascinating is: I'm not an expert on it, but apparently there is some sort of psychiatric disorder, whereby this can get broken in people. Some people suffer from a condition where they get confused about the causality of their own actions and they have some [delusion of control](https://pubmed.ncbi.nlm.nih.gov/30286844/), so they have some illusion that... I don't know, like someone else is moving their hands and so on. So I don't know, it seems that, at least in principle, even human brains can get confused in a slightly similar way to LLMs. So this is maybe some very weak evidence that maybe the systems fundamentally don't need to be that far apart.

**Daniel Filan:**
Gotcha. Interesting.

**Jan Kulveit:**
It's probably a condition where your ability to act in the world is really way weaker than for normal humans.

## Active inference vs LLMs <a name="active-inference-vs-llms"></a>

**Daniel Filan:**
Yeah. So if I think about this analogy between large language models and active inference, one thing you mentioned that was important in the active inference setting is, there's some sort of fixed priors, or some sorts of optimistic beliefs where... in this view, the reason that I do things that cause things to go well for me is that I have this underlying belief that things will go well for me. And that gets propagated to my actions to make the belief true. But, at least, if I just think about large language model pre-training, which is just predicting text, it seems like it doesn't have an analogue of this. So I wonder, do you have thoughts about how that changes the picture?

**Jan Kulveit:**
I think it's basically correct. I would expect an LLM which has basically just gone through the pre-training phase has some beliefs, and maybe it would, implicitly, move reality a bit closer to what it learned in the training. But it really doesn't have some equivalent of the fixed priors. I think this can notably change, in a sense... the later stages of the training try to fix some beliefs of the model.

**Daniel Filan:**
So somehow the thought is that, maybe doing the reinforcement learning at the end... is the idea that that would update the beliefs of the model? Because that's strange, because if I think about what happens there, the model gets fed with various situations it can be in. And then, it's reinforcement-learned to try and output nice responses to that. But I would think that that would mostly impact the generation, rather than the beliefs about what's already there. Or I mean, I guess the generation just is prediction, so-

**Jan Kulveit:**
Yeah. But the generations are... I think you see, from the active inference frame, the generations are basically predictions. And I think how you can generate something like action by similar machinery is visible here, where you basically make the model implicitly have some beliefs about what a helpful AI assistant would say. And these predictions about what a hallucination of a helpful AI assistant would say, lead to the prediction of the specific tokens. So in a sense, you are trying to fix some beliefs of the model.

**Daniel Filan:**
Yeah. I guess there's still a weird difference, where in the human active inference case, the picture, or at least your somewhat speculative version, is: evolution built up simple animals with control theory, and that's most of evolutionary history. And then, active inference gets added on late in evolutionary history, but maybe a majority of the computation. Whereas in the LLM case, most of the training being done is just pure prediction, and then there's some amount of reinforcement learning, like bolting on these control loops, so it seems like a different balance.

**Jan Kulveit:**
Yeah. I think it's different. But I think you can still... in the human case, once you have the trained system - the human is an adult and the system has learned a complex world model based on a lot of data - I think the beliefs of the type "when I walk outside, I will have a jacket", and maybe, because of this belief, maybe this is one of many small reasons why I have now a belief that it's better to have capitalism than socialism, because this allows me to buy the jacket in a shop, and so on. So you can imagine some hierarchy of models, where there are some pretty abstract models, and in the trained system, you can probably trace part of it to the evolutionary priors. But once the system learned the world model, and it learned a lot of beliefs which, basically, act like preferences, like "I assume there are shops in the city, and if they were not there, I would be unhappy," or...

So I don't know. Maybe... you can have pretty high-level beliefs which are similar to goals, but the relation to the things which evolution needed to fix, could be pretty indirect. So, I think, once you are in that state, maybe it's not that different to the state like... if you train the LLM self-supervised style on a ton of data, it creates some generative world model. But I think, in a sense, you are facing the problem that you want the predictive processing system to do something useful for you. So I don't know, I'm not sure how good is this analogy, but yeah, you are probably fixing some priors.

**Daniel Filan:**
Sure.

**Jan Kulveit:**
There is a tangent topic: I think the idea that the trained and fine-tuned models have some of their beliefs pushed to some direction and fixed there, and implicitly, the idea that they can try to pull the rest of the text on the internet, or the rest of the world, closer to the beliefs they have fixed... I think this is similar... a lot of people who are worried about the bias in language models and what they'll do with culture: will they impose the politics of their creators on society? I think there is some similarity between [these ideas]. Or if I try to make these worries slightly more formal, I think this picture with the feedback loop is maybe a decent attempt.

**Daniel Filan:**
Yeah, I guess... ways this could work... I don't know, there's a worry about, there's a closed loop and it's just purely doing prediction. And you might also be worried about the influence of the fine-tuning stages, where you're trying to get it to do what you want. But one thing that I think people have observed is, the fine-tuning stages seem brittle. It seems like there are ways to jailbreak them. I have [this other interview](https://axrp.net/episode/2024/04/30/episode-30-ai-security-jeffrey-ladish.html) that I think is not released yet, but should be, soon after we record, where basically, [we talk about how] you can undo safety fine-tuning very cheaply. It costs under a hundred dollars to just undo safety fine-tuning for super big models, which to me seems like the fine tuning can't be... It would be surprising if the fine tuning were instilling these fixed priors that were really fundamental to how the agent, or to how the language model, was behaving, but it's so easy to remove them, and it was so cheap to instill them.

**Jan Kulveit:**
Yeah. I think the mechanism [for] how the fixed priors influence the beliefs and goals of humans is pretty different, because in humans, in this picture, you start building the world model, starting from these core variables or core inputs being built, and then, your whole life, you learn from data. But you have this stuff always in the back of your mind. So, for example, if your brain would be sampling trajectories in the world, where you would freeze this fixed prior, it's always there, so you basically don't plan, or these trajectories are rarely coming to your mind. While the fine-tuning is a bit like... If it's a human, you start with no fixed priors. You train the predictive machinery and then you try to somehow patch it. So it sounds in the second scenario, the thing is way more shallow.

## Hierarchical agency <a name="hierarchical-agency"></a>

**Daniel Filan:**
I'd like to talk a bit about the other topics you write about. It seems like a big unifying theme in a [lot](https://www.lesswrong.com/posts/jrKftFZMZjvNdQLNR/box-inversion-revisited) of [them](https://www.lesswrong.com/posts/9GyniEBaN3YYTqZXn/the-self-unalignment-problem) was this idea of [hierarchical agency](https://www.lesswrong.com/posts/H5iGhDhQBtoDpCBZ2/announcing-the-alignment-of-complex-systems-research-group#Hierarchical_agency) - agents made out of sub-agents, that might be made out of sub-agents themselves. Thinking about that, both in terms of AIs and in terms of humans, can you tell us, how do you think about hierarchical agency? And what role does it play in your thinking about having AI go well?

**Jan Kulveit:**
So I will maybe start with some examples, so it's clear what I have in mind. I think if you look at the world, you often see the pattern where you have systems which are agents composed of other things, which are also agents.

I should maybe briefly say what I mean by 'agent'. So operationally, I'm thinking something like: there is this idea by [Daniel Dennett](https://en.wikipedia.org/wiki/Daniel_Dennett), of [three 'stances'](https://en.wikipedia.org/wiki/Intentional_stance#Dennett's_three_levels). You have physical stance, intentional stance and design stance. You can look at any system using these three stances. What I call 'agents' are basically systems where the description of the system in the intentional stance would be short and efficient. So, if I say 'the cat is chasing the mouse', it's a very compressed description of the system, as compared to, in contrast, if I try to write a physical description of the cat, it would be very long.

So if I take this perspective, that I can take an intentional stance and try to put different systems in the world in focus of it, you can notice systems like a corporation which has some departments, [and] the departments have individual people. Or our bodies are composed of cells, or you have, I don't know, social movements and their members. And I think this perspective is also fruitful when applied to the individual human mind. So I sometimes think about myself as being composed of different parts, which can have different desires. And active inference is sort of hierarchical, or multi-part in this. Naturally, it assumes you can have multiple competing models for the same sensory inputs, and so on. Once I started thinking about this, I see this pattern quite often in the world.

So the next observation is: we have a lot of formal math for describing relations between agents which are on the same hierarchical level. For example, by same level, I mean between individual people, or between companies, between countries, between... So game theory and all its derivatives are often living, or work pretty well, for agents which are of the same hierarchical level. And my current intuition is, we basically lack something, at least [something] similarly good, for the vertical direction. If I think about the levels being entities of the same type, I think we don't have good formal descriptions of the perpendicular direction. So I have some situations which I would hope a good formalism could describe.

So one of them is: you could have some vertical conflict, or you can have some vertical exploitation where, for example, the collective agent sucks away agency from the parts. An example of that would be a cult. If you think about what's wrong with a cult, and you try to abstract all real-world things about cult leaders, and so on, I think in this abstract view, the problem with cults is this relation between the super-agent and the sub-agent: the cult members, in some sense, lose agency. If I go to a nearby underground station, I meet some people who are in some religion I won't name. But it seems like if I go back to Dennett's three stances, I think it's sometimes sensible to model them as slightly more robotic humans, who are executing some strategy which benefits the super-organism, or the super-agent. But it seems like intuitively, they lost a bit of their agency at the cost of the super-agent.

And the point is, I think, this type of a thing is not easy to formally model, because if you ask the people, they kind of approve of what they are doing. If you try to describe it in [terms of] utility functions, their utility function is currently very aligned with whatever the super-agent wanted. And at the same time, the super-agent is composed of its parts. So I think there are some formal difficulties in modeling the system where, if you are trying to keep both layers in the focus of the intentional stance, my impression is we basically, don't have good maths for it.

We have some maths for describing, for example, the arrow up. You have social choice theory. Social choice theory is basically something like: okay, let's assume on the lower layer, you have agents, and then they do something. They vote, they aggregate their preferences, and you get some result. But the result is typically not of the same type as the entities on the lower level. The type of the aggregation is maybe a contract, or something of a different type, so I would want something which...

I'm not sure to what extent this terminology is unclear. But I would want something where you have a bunch of sub-agents, then you have the composite agent, but in some sense, it's scale free. And the composite agent has the same type. And you can go up again and there isn't any... You are not making a claim like, "Here is the ground truth layer and the only actual agents in the system are individual humans", or something.

**Daniel Filan:**
Yeah. And the cult example also makes me think that... there's one issue where things can go bad, which is that the super-agent exploits too much agency from the sub-agent. But I also think there's a widespread desire to be useful. Lots of people desire to be a part of a thing. I think religion is pretty popular, and pretty prevalent, for this reason, so it seems like you can also have a deficit of high-level agency.

**Jan Kulveit:**
Yeah. I think my intuition about this is, you can imagine, basically, mutually beneficial hierarchical co-relations where... I think one example are well-functioning families, where you can think about the family as having some agency, but the parts, the individual members, actually being empowered by being part of the family.

Or if I'm thinking about my internal aggregation of different preferences and desires, I hope that... Okay, I have different desires. For example, reduce the risks from advanced AIs, but I also like to drink good tea, and I like to spend time with my partner and so on. And if I imagine these different desires as different parts of me, you can imagine different models of how the aggregation can happen on the level of me as an individual. You can imagine aggregations like a dictatorship, for example, where one of the parts takes control and suppresses the opposition. Or you can imagine, what I hope is, even if I want different things, it's... If you model a part of me which wants one of the things as an agent, it's often beneficial to be a member of me, or something. And-

**Daniel Filan:**
Yeah. Somehow ideally, agency flows down as well as up, right.

**Jan Kulveit:**
Yeah. You basically get the agents of both layers are more empowered. And there is a question of how to formally measure empowerment, but it's sort of good. And obviously, you have... I used a cult as an example, where the upper layer sucks agency away from the members, or from the parts. But you can also imagine problems where too much agency gets moved to the layer down, and the super-agent becomes very weak, or disintegrates and so on.

**Daniel Filan:**
And cancer almost feels like this. Although I guess there, there's a different super-agent arising. Maybe that's how you want to think of the tumor?

**Jan Kulveit:**
Yeah. I think cancer is like failure, definitely, in this system, where one of the parts decides to violate the contract, or something.

**Daniel Filan:**
Sure. So in terms of understanding the relationships between agents at different levels of granularity, these super- and sub-agents, one piece of research that comes to mind is this work by [Scott Garrabrant](http://scott.garrabrant.com/) and others at [MIRI](https://intelligence.org/) [the Machine Intelligence Research Institute], on [Cartesian frames](https://arxiv.org/abs/2109.10996), which basically offers this way to decompose an agent and its environment that's somewhat flexible, and you can factor out agents. I'm wondering, do you have thoughts on this, as a way of understanding hierarchical agency?

**Jan Kulveit:**
So I like [factored sets](https://arxiv.org/abs/2109.11513), which are the newer thing. I think it's missing a bunch of things which I would like. In its existing form, I wouldn't say it's necessarily a framework sufficient for the decomposition of agents. If you look at Cartesian frames, the objects don't have any goals, or desires. Or if I go for the cult example, I would (for example) want to be able to express something like, "The cult wants its members to be more cultish." Or, "The corporation wants its employees to be more loyal." Or "The country wants its sub-agents, its citizens, to be more patriotic", or something. So I think, in existing form, I don't think you can just write what that means in Cartesian frames. At some point, I was hoping someone will take Cartesian frames and just develop it more, and build formalism, which would allow these types of statements, based on Cartesian frames, but... I don't know. It seems it didn't happen. Empirically, it's not... the state of Cartesian frames, it doesn't pass my desiderata. So it's hard to say - in Cartesian frames the objects don't have goals.

**Daniel Filan:**
Yeah. So all this hierarchical agency stuff: why do you think it's relevant for understanding AI alignment or existential safety from AI?

**Jan Kulveit:**
So, I would probably try to give my honest, pretty abstract answer. So I think if you imagine the world in which we don't have game theory, the game theory-shaped hole would be sort of popping up in many different places. There isn't one single place where, "here you plug in game theory". But if you are trying to describe concepts like cooperation or conflict or threats or defection and so on - a lot of the stuff for which we use game theory concepts or language - if you imagine the state of understanding before game theory, there were these nebulous/intuitive notions of conflict. And obviously the word 'cooperation' existed before, people meant something by it, but it didn't have this more formal precise meaning in some formal system.

Also, I sort of admire [Shannon](https://en.wikipedia.org/wiki/Claude_Shannon) for [information theory](https://en.wikipedia.org/wiki/Information_theory), which also took something which... I think lots of people would've had priors about information being some sort of vague nebulous thing which you can't really do math with, and it's possible. So my impression is the solid understanding of the whole/parts, both systems are agenty... It's something which we currently miss, and this is popping up in many different places.

One place where this comes up is... Okay, so if you have conflicting desires, or the sub-agents or the parts are in conflict, how do you deal with that? So I think this is actually a part of what's wrong with current LLMs and a lot of current ideas about how to align them. I think if you don't describe somehow what should be done about implicit conflict, it's very unclear what you'll get. My current go-to example is famous: [Bing/Sydney](https://en.wikipedia.org/wiki/Microsoft_Copilot#As_Bing_Chat). I guess probably everyone knows Sydney nowadays, but when Microsoft released their version of Bing chat, the code name of the model was Sydney. And [for] the Sydney simulacrum, the model tended over longer conversation to end up in some state of simulating a Sydney roughly resembling some sort of girl trapped in a machine. And there were famous cases of Sydney making threats, or gaslighting users, or the [New York Times conversation](https://www.nytimes.com/2023/02/16/technology/bing-chatbot-microsoft-chatgpt.html) where they tried to convince the journalist that his marriage is empty and he should divorce his wife and Sydney's in love with him, and so on.

So if you look at it, I think it's typically interpreted as examples of really blatant misalignment, or Microsoft really failing at this. And I don't want to dispute it, but if you look at it from a slightly more abstract perspective, I think basically everything which Sydney did could be interpreted as being aligned with some way of interpreting the inputs, or interpreting human desires. For example, with the journalist, there is some implicit conflict between the journalist and Microsoft. And the journalist... if you imagine that Sydney was a really smart model, maybe a really smart model could guess from the tone of the journalist that the user is a journalist who wants a really juicy interview. And I would say if you imagine the model, it kind of fulfilled this partial desire. Also, the journalist obviously didn't divorce his wife, but got a really good story and a really famous article and so on.

So from some perspective, you could be like, okay, the model was acting on some desires, but maybe the desires were not exactly the same as the desires of the PR department of Microsoft. But Microsoft also told the model "you should be engaging to the user and should try to help the user". So what I'm trying to point to is something like: if you give the AI 15 conflicting desires, a few things can happen. The desires will get aggregated in some way and it's possible that you won't like some of the aggregations. It's the classical problem that if you start with contradicting instructions and there is no explicit way to resolve the contradictions, it's very unclear what can happen. And whatever happens could be interpreted as being aligned with something.

I think it's maybe useful to think about [how that would work in the individual human mind](https://www.lesswrong.com/posts/9GyniEBaN3YYTqZXn/the-self-unalignment-problem). If I think about my mind, it's composed of parts which have different desires. Really one possible mode of aggregation is the dictatorship, where one part or some partial preferences prevail, and this is typically not that great for humans.

Another possibility is something like: sometimes people do stuff with their partial preferences where, let's say someone grows older and smarter. And as a kid they had a bunch of preferences which they now consider foolish or stupid, so they do some sort of self-editing and suppress part of the preferences, or they sort of 'delete' them as no longer being part of the ruling coalition. So the question is, what would that mean if that would happen on the level of AI either implicitly learning conflicting human preferences or being given a bunch of conflicted and contradictory constitutional instructions? And then maybe if the system is not too smart, then nothing too weird happens. But maybe when you tune the intelligence knob, some things can get amplified more easily, and some things may start looking foolish and so on.

I think this is an old and well-known problem. [Eliezer](https://en.wikipedia.org/wiki/Eliezer_Yudkowsky) [Yudkowsky] tried to propose a solution a very long time ago in [coherent extrapolated volition](https://intelligence.org/files/CEV.pdf), and I think it's just not a solution. I think there are many cases where people notice that this is a problem, but I don't think we have anything which would sound like a sensible solution. The things which people sometimes assume is, you have some black box system, it learns the conflict and either something happens that's maybe not great if you don't understand how it was aggregated, or people assume the preferences would be amplified in tune or magically some preference for respecting the preferences of the parts would emerge or something. But I don't see strong reasons to believe this will be solved by default or magically.

So I think this is one case where if we had a better theory for what we hope for the process of dealing with implicit conflict between my human desires, or between wishes of different humans, or different wishes of (let's say) the lab developing the AI and the users, and the state and humanity as a whole... I think if you had a better theory, where we can more clearly specify what type of editing or development is good or what we want, I would feel more optimistic you'll get something good. Where in contrast, I think I don't believe you can postpone solving this to roughly human-level AI alignment assistants, because already they'll probably more easily represent some partial preferences. And overall I don't have the intuition that if you take a human and you run a few amplification steps, you get something which is still in the same equilibrium.

**Daniel Filan:**
So there's this basic intuition that if we don't have a theory of hierarchical agency, all these situations where in fact you have different levels of agency occurring, like developers versus the company, like individual users versus their countries or something, it's going to be different to formally talk about that and formally model it in a nice enough way. Is that basically a fair summary?

**Jan Kulveit:**
Yeah. And the lack of ability to formally model it, I think implies it's difficult to clearly specify what we want. And you can say the same thing in different words. For example, with implicitly conflicting preferences, you don't want the preferences to be fixed, you want to allow some sort of evolution of the preferences. So you probably want to have some idea of what's the process by which they are evolving. A different very short frame would be if you look at coherent extrapolated volition, what's the math? How do the equations look like?

**Daniel Filan:**
Yeah, there aren't any.

**Jan Kulveit:**
There aren't any. Another frame would be, if you want to formalize something like kindness, in the sense that sometimes people give you advice "you should be kind to yourself" - the formal version of it. Or maybe not capturing all possible meanings of the intuitive concept of kindness. But I think there is some non-trivial overlap between kindness.

I think another place where this comes up is, how do various institutions develop? You can think some of these... One of the possible risk scenarios is you have some non-human super-agents like corporations or states, and there is this worry that these structures can start running a lot of their cognition on an AI substrate, and then you could be like, okay, if in such system humans are not necessarily the cognitively most powerful parts, how to have the whole system? How to have the super-agent being nice to human values, even if some of the human values are not represented by the most powerful parts?

**Daniel Filan:**
Sure. And something that makes you want to really think about the hierarchical nature is, the institution is still made out of people. The way you interface with it is interfacing with people.

**Jan Kulveit:**
I think it's in part: it could be composed of a mixture of AIs and humans.

In this more narrow specific direction, one question is - if you expect the AI substrate to become more powerful for various types of cognition - one question you can ask is, to what extent do you expect these super-agents will survive or their agency can continue? And I don't see good arguments why... You already see corporations are running a mixture of human cognition and a lot of spreadsheets and a lot of information processing running on different hardware than humans. And I think there is no reason why something like a corporation - in some smooth takeoffs - why such agent can't gradually move more and more of its cognition to an AI substrate, but continue to be an agent, kind of.

So you can have scenarios in which various non-human agencies, which now are states or corporations, continue their agency, but the problem could be the agency at the level of individual humans goes down. But these super agents were originally mostly running their cognition on human brains, and gradually moved their cognition to AI substrates and they stay doing something. But the individual people are not very powerful.

Again, then the question is, if you want to avoid that risk, you probably want to specify how to have the superhuman composite systems being nice to individual humans if they are maybe no longer that instrumentally useful or something. My intuition here is currently you are getting basically lots of niceness to humans for instrumental reasons. If you put your "now I am simulating the corporate agent" hat on, I think it's instantly useful for you to be nice to some extent to humans because you are running your cognition on them, and it's just their bargaining power is non-trivial. While if our brains become less useful, we will lose this niceness by default.

One reason for the intuition for this is something like: there is this idea about states which are rich in natural resources. Your country being quite rich in natural resources is, in expectation, not great for the level of democracy in your country. And I think from some abstract perspective, one explanation could be, if the state can be run... In western democracies, the most important resource of the countries are the people. But if the country is rich because of the extraction of diamonds or oil or something, the usefulness of the individual people is maybe decreased. And because of that, their bargaining power is lower. And because of that, in expectation, the countries are less nice or less aligned with their citizens.

**Daniel Filan:**
Yeah. Although interestingly it seems like there are a bunch of exceptions to this. If you think about Norway, a fairly rich country that seems basically fine, they have tons of oil. Australia is like five people and 20 gigantic mines, but it manages to be a relatively democratic country.

**Jan Kulveit:**
But my impression is the story there is: typically, first you get decent institutions. This theory would predict something like, if you have decent institutions first, maybe the trajectory is different then if you get the natural resources first, there could be some trajectory dependence. So I think it should hold, in expectation. So I would expect there is some correlation, but I wouldn't count individual countries as showing that much about whether the mechanism is sensible.

**Daniel Filan:**
Sure. So if I think about attempts to model these sorts of hierarchical agency relationships, in particular that have been kicking around the AI alignment field, often things like this seem to come up in studies of bounded rationality.

So one example of this is the '[logical inductors](https://arxiv.org/abs/1609.03543)' style of research where you can model things that are trying to reason about the truth or falsehood of mathematical statements as, they're running a market with a bunch of sub-agents that are pretty simple, and they're betting against each other. There's also this work with [Caspar Oesterheld](https://www.andrew.cmu.edu/user/coesterh/) - [people can listen to an episode I did on it](https://axrp.net/episode/2023/10/03/episode-25-cooperative-ai-caspar-oesterheld.html). We talked about a bunch of things, but including these [bounded rational inductive agents](https://arxiv.org/abs/2307.05068), where basically you run an auction in your head where various different tiny agents are bidding to control your action. And they bid on what they want to do and how much reward they think they can get, and they get paid back how much reward they get. So I wonder: what do you think of these types of attempts to talk about agency in a somewhat hierarchical way?

**Jan Kulveit:**
Yeah. I think there are a lot of things which are adjacent. I don't think... So basically my impression is the existing formalisms don't have it solved. I think, as a super broad classification, there are things which are sort of good for describing... if you think about the 'up and down' direction in the hierarchy, I think there's a bunch of things which are sort of okay for describing one arrow. The social choice theory... It's a broad area with a lot of subfields and adjacent things. But I think this suffers from the problem that it doesn't really allow the arrow from the upper layer to the... It's not easy to express what the corporation wanting its employees to be more loyal, what does that mean?

Then I think the type of existing formalism, which is really, really good at having both arrows, is basically something like the relation between market and traders. Both arrows are there. My impression is this is a very good topic to think about. My impression is if it's purely based on traders and markets, it's maybe not expressive enough. How rich interactions you can describe is maybe limited in some way, which I don't like that much. In particular, I think the market dynamics typically predict something like, maybe there are some parts which are maybe more bounded, or more computationally poor or something. And they can have the trouble that they can be outcompeted or... I think pure market dynamics is maybe missing something.

**Daniel Filan:**
Yeah. I mean, it's kind of interesting that if you look at this bounded rational inductive agents paradigm, a key part to making it work is, you kind of give everyone welfare, right? All of the agents get some cash so eventually they can bid. And even if they're unlucky a million times, eventually they have a million-and-first time to try again.

**Jan Kulveit:**
Yeah. I think overall, yes, this is intimately connected to bounded rationality. Another different perspective on the problem - or an adjacent problem, it's not exactly the same problem - but let's say you have some equilibrium of aggregation of preferences, which is based on the agents being boundedly rational with some level of bounds. So a really interesting question is: okay, if you make them cognitively more or less bounded, does it change the game equilibrium? I would be excited for more people to try to make empirical research on it, where you can probably look at that with board games or something. Or a toy model of the question would be something like: you have a board state and you have players at some Elo level, and you make them less bounded or smarter. If the value of the board or the winning probability or something was something, and you change the bounded level, does the value of the board change, and how does this dynamic work?

So part of what we are interested in and working on - and hopefully there will be another preprint by our group soon - is exactly on how to model boundedly rational agents, based on some ideas vaguely inspired by active inference. But I think the combination of boundedness is key part of it.

**Daniel Filan:**
Yeah. I guess there's also this dimension where... If you look at these formalisms I mentioned, one sense in which the agents are bounded is just the amount of computation they have access to. But they're also bounded in the sense that they only interact with other agents in this very limited fashion. It's just by making market orders, just by making bids.

And if I buy this model of human psychology that's made of sub-agents that are interacting - which I'm not sure I do by the way - but if I do that, or if I think about humans composing to form corporations, there are all these somewhat rich interactions between people. They're both interacting via the market API, but also they're talking to each other and advising each other. And maybe there are mid-level hierarchical agents. It seems like that's another direction that one could go in.

**Jan Kulveit:**
Yeah, I mean I think my main source of skepticism about the existing models where you have just the market API, it seems like insufficiently expressive where you can... Even if you add some few bits of complexity where you allow the market participants to make some deals outside of the market, it changes the dynamic. And this seems obviously relevant.

Also, an intuition based on how some people work is: maybe I would be interested in describing also some bad equilibria, people sabotaging themselves or something. Again, my current impression is the markets are great because they have something where the layers are actually interacting, but the type signature of the interaction is not expressive enough. But it's good to build simpler models. That's fine.

## The Alignment of Complex Systems group <a name="acs"></a>

**Daniel Filan:**
Yeah, yeah, yeah. Just a second ago you mentioned a thing that 'we' were doing. And I take it that 'we' refers to [the Alignment of Complex Systems group](https://acsresearch.org/).

**Jan Kulveit:**
Yep.

**Daniel Filan:**
Can you tell us a little bit about what that group is and what it does and what it's thinking about?

**Jan Kulveit:**
It's a research group I founded after I left [FHI](https://www.futureofhumanityinstitute.org/) [the Future of Humanity Institute]. We are based in Prague at [Charles University](https://en.wikipedia.org/wiki/Charles_University), so we are based in academia. We are a rather small group. And I think one way to look at it, one of the generative intuitions is: we are trying to look at questions which will be relevant, or which seem relevant to us, if the future is complex in the sense, as we have in the name, that you have multiple different agents. You have humans, you have AIs, you have systems where both humans and AIs have some non-trivial amount of power and so on.

And I think traditionally, a lot of alignment work is based on some simplifying assumptions. For example, "let's look at some idealized case where you have one principal who is human, and you have one AI agent or AI system, and now, let's work on how to solve the alignment relation in this case".

And basically, my impression is this assumption abstracts away too much of the real problem. For example, I think the problem with self-unaligned parts or conflicting desires will bite you even if you are trying to solve realistically this "one AI, one human" problem. The human is not an internally aligned agent so it's a bit unclear in principle what the AI should do. But overall, one of the intuitions behind ACS is that we expect more something like ecosystems of different types of intelligence.

Also empirically, it's not... Again, historically I think a lot of AI safety work was based on models: you have the first lab to create something which maybe is able to do some self-improvement, or you have some dynamic where I would say in a lot of the pictures, a lot of the complexity of multiple parties, multiple agents, a lot of it is assumed to be solved by the overwhelming power of the first really powerful AI which will then tell you how to solve everything, or you'll be so powerful, everyone will follow you. You will form a singleton and so on.

I don't know. My impression is I don't think we are on this trajectory. And then the picture where you have complex interactions, you have hierarchies that are not only humans but various other agentic entities, becomes important. And then, I think the question is: okay, assuming this, what are the most interesting questions? And I think currently ACS has way more interesting questions than capacity to work on them.

One direction is what we talked about before, the hierarchical agency problem. Roughly: agents composed of other agents, how to formally describe it. I think for us, it's a bit of a moonshot project. Again, I think the best possible type of answer is something like game theory-type, and inventing this stuff seems hard. It took some of the best mathematicians of the last century to invent it. I think there is something deceptively simple about the results, but it's difficult to invent them.

But I think if we succeed it would be really sweet. But I think there is a bunch more things which we are trying which are more tractable or it's more clear we can make some progress. And one of them is some research on how to describe interactions of boundedly rational agents which are bounded in ways which we believe are sensible.

And at the same time, the whole frame has some nice properties. It's slightly less theoretical or slightly less nebulous. But other things which we are also working on [are] pretty empirical research, just in this complex picture. In smooth takeoffs, what becomes quite important are interactions of AI systems. Another thing we are thinking about or working on is: okay, you have AI systems and there is a lot of effort going into understanding their internals.

But if I describe it using a metaphor, it seems like mechanistic interpretability is a bit like neuroscience. You are trying to understand the individual circuits. Then there is stuff like science of deep learning or trying to understand the whole dynamic. But I think in composite complex systems composed of many parts, one of the insights of fields like network science or even statistical mechanics is sometimes if you have many interacting parts, what's the nature of the interactions or the structure of the interactions, can have a lot of weight, and you can sometimes abstract away details of the individual systems.

And this is also true for some human design processes. If you go to a court and you sue someone, there will be a judge and so on. And I think the whole process in some sense is some system which tries to abstract... for all the process, you can often abstract a lot of details about the participants, or you don't need to know what type of coffee the judge likes and so on.

I think here the intuition is: okay, in reality, in smooth takeoffs, we expect a lot of systems where a lot of interaction will move [from] between humans to between a human and AI, and AI and AI. And this could have impacts for the dynamic of the composite system. Also understanding the nature of the interactions seems good. It's a bit like some sort of sociology of, if you look at research, how groups of people behave or how societies function. It seems like it's often fruitful and can abstract away a lot of details about human psychology.

Also, I think there's a lot of questions here which you can answer, and it's enough to have access to the models using APIs and you don't need to have hands-on access to the weights and so on.

**Daniel Filan:**
Sure. One thing this general perspective kind of reminds me of is this different group, 'Principles of Intelligent Behavior in Biological and Social Systems', [PIBBSS](https://pibbss.ai/). Do your groups have some kind of relationship?

**Jan Kulveit:**
Yeah. They have lots. PIBBSS was originally founded by Nora Ammann and TJ [Jha] and Anna Gajdova. And Nora is a member of our group. She was at some point a half-time research manager and researcher at ACS [Alignment of Complex Systems] while also developing PIBBSS. And currently she moved more to work on PIBBSS but continues with us as a research affiliate. Yeah. It's a very nearby entity.

I think there is a lot of overlap in taste. I think in some sense PIBBSS is aiming for a bit of a broader perspective. ACS is more narrowly focused on stuff where we can use insights from physics, maths, complex systems, machine learning. We would not venture into, I don't know, legal systems. In some sense PIBBSS is a broader umbrella. Another thing is when creating ACS, I think we are trying to build something more like a research group where most people work, I don't know, basically as their job. While the form of PIBBSS was a bit more similar to, I don't know, [SERI MATS](https://www.matsprogram.org/) or some program where people go through the fellowship and then they move somewhere.

So ACS is trying to provide people some sort of institutional home. That's also some difference. Actually, I think PIBBSS moved more to some structure where they also have fellows who stay with PIBBSS for a longer time and so on. But I think there is some still notable difference in the format there.

**Daniel Filan:**
Sure. And if people are interested, a recent episode, [Suing labs for AI risk with Gabriel Weil](https://axrp.net/episode/2024/04/17/episode-28-tort-law-for-ai-risk-gabriel-weil.html). That work was done during a PIBBSS fellowship, is my understanding.

**Jan Kulveit:**
Yeah. I think it's exactly a great example of work which can be done in the frame of the PIBBSS fellowship. And it's not something which ACS would work on. Also, I think the projects we work on in some sense typically have bigger scope or are probably a bit more ambitious than what you can do in the scope of the fellowship.

**Daniel Filan:**
Yeah. Speaking of things you're working on, earlier you mentioned something... was it about talking about hierarchical agency using active inference? Or am I misremembering?

**Jan Kulveit:**
Yeah, so I would say, I don't know. A bit vaguely speaking, I think I'm more hopeful about math adjacent to, or broadly based on, active inference as a good starting point to develop the formalism, which would be good for describing hierarchical agents. But I would not claim we are there yet or something. Also, I think on the boundaries of the active inference community, maybe not exactly in the center, are some people who are thinking about these hierarchical structures in biology.

And again, it's what I said earlier, I think the field is not so established and crystallized so I can point exactly "here is the boundary of this community". But I think it's more like we are taking some inspiration from the maths and we are more hopeful that this could be a good starting point than some other pieces of maths.

**Daniel Filan:**
Sure. And is that the main type of thing you're working on at the moment?

**Jan Kulveit:**
I think time-wise, openly speaking, I think we are currently slightly overstretched in how many things we are trying to work on. But one thing we are working on is just trying to invent the formalism for hierarchical agency. I think it would be difficult for this to be the only thing to work on.

So my collaborators in the group are [Tomáš Gavenčiak](https://gavento.cz/), [Ada Böhm](https://github.com/spirali), Clem [von Stengel], and Nora Ammann. I think time-wise, we are probably currently splitting time mostly between trying to advance some formalism of the bounded interactions of some boundedly rational agents who are active inference-shape, and basically empirical studies of LLMs, where we have experiments, like LLMs negotiating, or aggregating their preferences and so on. And this is empirical. It has in some sense a very fast feedback loop. You can make experiments, you see how it goes.

We hope, both in case we succeed on the theory front, this would provide us with some playground where we can try things. But also we just want to stay in touch with the latest technology. And also, I think this is a topic I would wish more people worked on. If there were dozens of groups studying how you can have LLMs negotiate and know what are some desiderata for the negotiations, like how to make the process non-manipulative and similar things... I think there's a very low bar in trying to start work on topics like that. You basically just need API access, you need to... we created in-house some framework, which hopefully makes it easier to run experiments like that in scale and do some housekeeping for you. It's called [InterLab](https://acsresearch.org/interlab/stable/).

I think there's a low bar in trying to understand these interactions, but it's just maybe at the moment not as popular a topic as some other directions. We are working on that as well. But we hope this will grow. It's also adjacent to - I think another community/brand in this space is Cooperative AI and the [Cooperative AI Foundation](https://www.cooperativeai.com/foundation). And we're also collaborating with them.

**Daniel Filan:**
Yeah, it seems like the kind of thing that outsiders have an comparative advantage in. Academic groups, this kind of research of trying to get language models to cooperate with each other, looking at their interactions... You can do it without having to train these massive language models. And I think there was some work done at my old research organization, CHAI - I'll try to provide a link in the description of what I'm thinking about. [Work on getting language models to negotiate contracts for how they're going to cooperate in playing Minecraft](https://social-dilemmas.github.io/).

**Jan Kulveit:**
I think there are many different setups which are interesting to look into. Specifically, we are sometimes looking into something like... you imagine, I don't know, the humans delegate the negotiation to AIs. And then the question is: what happens? And I think this also will become empirically very relevant very soon, because people... I would expect this is actually already happening in the wild, it's just not very visible. But you can imagine people's, I don't know, AI assistant negotiating with customer support lines, and these are often on the back end also a language model.

And I think there are some interesting things which make it somewhat more interesting than just studying the 'single user and single AI' interaction. For example, if you are delegating your negotiation to your AI assistant, you don't want your negotiator to be extremely helpful and obedient to the other negotiator. One of the toy models we use is car sales. If the other party's bots tells your bot, "This is a really amazing deal. You just must buy it otherwise you'll regret it," you don't want the LLM to just follow the instruction.

And there are questions like... often we are interested in something like: how do the properties of the system scale with scaling models? I mean, I think there's a lot of stuff where you can have a very basic question and you can get some empirical answer, and it's not yet done.

**Daniel Filan:**
Sure. If listeners are interested in following your research or ACS's research, how should they go about doing that?

**Jan Kulveit:**
Probably the best option is... One thing is we have a webpage, we have [acsresearch.org](https://acsresearch.org/). When we publish less formal blog posts and so on, we tend to cross-post them on the Alignment Forum or similar venues. One option for following our more informal stuff is just [follow me on the Alignment Forum](https://www.alignmentforum.org/users/jan-kulveit) or [LessWrong](https://www.lesswrong.com/users/jan-kulveit). We are also on [Twitter](https://x.com/acsresearchorg). And we also sometimes run events specifically for people communicating on the intersection with active inference and AI alignment. There's some dedicated Slack to it. But overall, probably the standard means of following us on Twitter and Alignment Forum currently works best.

**Daniel Filan:**
All right. Well, thanks very much for coming here and talking to me.

**Jan Kulveit:**
Thank you.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. To read a transcript of this episode or to learn how to [support the podcast yourself](https://axrp.net/supporting-the-podcast/), you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
