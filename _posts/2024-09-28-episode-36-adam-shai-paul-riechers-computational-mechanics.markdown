---
layout: post
title: "36 - Adam Shai and Paul Riechers on Computational Mechanics"
date: 2024-09-28 22:20 -0700
categories: episode
---

[YouTube link](https://youtu.be/sUpAssKC-L0)

Sometimes, people talk about transformers as having "world models" as a result of being trained to predict text data on the internet. But what does this even mean? In this episode, I talk with Adam Shai and Paul Riechers about their work applying computational mechanics, a sub-field of physics studying how to predict random processes, to neural networks.

Topics we discuss:
  - [What computational mechanics is](#what-comp-mech-is)
  - [Computational mechanics vs other approaches](#comp-mech-vs-other-approaches)
  - [What world models are](#what-world-models-are)
  - [Fractals](#fractals)
  - [How the fractals are formed](#how-the-fractals-are-formed)
  - [Scaling computational mechanics for transformers](#scaling-comp-mech-for-transformers)
  - [How Adam and Paul found computational mechanics](#how-adam-and-paul-found-comp-mech)
  - [Computational mechanics for AI safety](#comp-mech-for-ai-safety)
  - [Following Adam and Paul's research](#following-adam-and-pauls-research)

**Daniel Filan:**
Hello, everybody. This episode I'll be speaking with Adam Shai and Paul Riechers, who are co-founders of [Simplex](https://www.simplexaisafety.com/). Simplex is a new organization that takes a physics of information perspective on interpretability, aiming to build a rigorous foundation for AI safety. Paul has a background in theoretical physics and computational mechanics, while Adam has a background in computational and experimental neuroscience. For links to what we're discussing, you can check the description of the episode and a transcript is available at axrp.net. Paul and Adam, welcome to AXRP.

**Adam Shai:**
Yeah, thanks for having us.

**Paul Riechers:**
Thank you.

## What computational mechanics is <a name="what-comp-mech-is"></a>

**Daniel Filan:**
You guys work on doing computational mechanics for AI safety type things. What is computational mechanics?

**Paul Riechers:**
I'm happy to give a bit of a background. Computational mechanics has basically grown within the field of physics, mostly, out of chaos theory, information theory. For what purpose? Well, I mean physics has always been concerned with prediction, that we want to write down some equations that tell us maybe if we know the state of the world, what does that imply for the future? Some planets move around and all these things.

And then there became a pretty serious challenge to that - being able to predict physical systems - that came from chaos theory, where even if you have deterministic equations of motion, there's a finite horizon to how much you can predict. And so I don't know if I'd say that physics was thrown into turmoil, but it was a serious challenge for what are the limits of predictability and how would you go about doing that prediction as well as possible.

I'd say that's the history out of which computational mechanics has grown. And so, it's now a very diverse set of results and ideas, ways of thinking about things and dynamics. But really at its core is: what does it mean to predict the future as well as possible? As part of that, what does it mean to generate dynamics and how is generating different than predicting? If you have some generative structure, is it harder to predict? A lot of this stuff's been quantified.

And a lot of the work that's been done is at this ultimate limit of what's possible. But there's also a lot of work that's been done, and more and more work now, of if you have some resource constraints, then what does that imply for your ability to predict? And you're not asking yet, but I'd also be happy to share how that's relevant to AI/ML, I'll maybe just throw it in there.

**Daniel Filan:**
Sure.

**Paul Riechers:**
Maybe this is obvious to your listeners, but we're now training these AI models to predict future tokens from past tokens as well as possible. And while people are poking around trying to understand what's going on, there's this theory that was developed specifically to address that. And so a lot of the mathematical framework maps on surprisingly easy, where we've been able to come away with some results that we've been just actually surprised that it worked out so well. And it's a great framework to then build upon. I wouldn't say computational mechanics is the answer to understanding all of interpretability and AI safety, but it's really a great foundation and something that helps us to make better questions and see where research should go.

**Daniel Filan:**
Sure. I guess a bit more specifically, if I hear this pitch of like, "Oh, yeah, we're interested in understanding how to predict the future as well as possible." Someone might think, "Hey, that's Bayesian statistics. We got Bayes' Theorem, like, 100 years ago. What else is there?" I'm sure there's more, but what is it?

**Paul Riechers:**
What are you doing Bayesian inference over?

**Daniel Filan:**
Yeah, I don't know, stuff. You're going to have some prior and you're going to have some likelihood and then you're just done. What else is there?

**Paul Riechers:**
No, exactly. But I think that's my point: computational mechanics helps us to understand what are the things that you're trying to do updates over? And I think there's some question of... From the start, I'm saying, "Okay, we're applying this theory because models are predicting the future as well as possible," but actually what we're doing is we're training them on this next token cross-entropy loss.

Well, what does that mean? People have argued about whether they can have some world model, and it's unclear what people even mean by a world model, or if they're stochastic parrots, and it's not even clear what people mean by that. One of the advantages is that: let's just take this seriously, what are the implications of doing well at next token prediction? And there's a theorem - it's actually a corollary from [a paper](https://arxiv.org/abs/cond-mat/9907176) from I think 2001 from [Cosma] [Shalizi](https://www.stat.cmu.edu/~cshalizi/) and [Jim] [Crutchfield](https://csc.ucdavis.edu/~chaos/) - which if you translate it into modern language, says that to do as well as possible at next token prediction implies that you actually predict the entire future as well as possible. Okay, then computational mechanics comes into play.

It's more than Bayesian inference because what? Well, you want to know what about the past you need to predict the future. And if you had no resource limitations, you can just hang on to all of the past. And there would be maybe a mapping from this past to this future, but with some number of your token alphabet, the number of paths would be growing exponentially with longer length paths.

And so that's intractable. You somehow need to compress the past. And what about the past should be coarse-grained? Basically there's this obvious mantra, but when you take it seriously, again, it has interesting mathematical implications, that for the purpose of prediction, don't distinguish pasts that don't distinguish themselves for the purpose of prediction. That means if a past induces the same probability distribution over the entire future, then just clump those pasts together. And also if you want to say about lossy prediction, then you can look in that space of probability distributions over the future and you can see what histories are nearby in that space.

**Daniel Filan:**
Sure. Maybe a thing that would help my understanding is if I just say what I've gathered the interesting objects, interesting things in computational mechanics are. And you can tell me where I'm wrong, or perhaps more likely what interesting things I'm missing. Does that sound good?

**Adam Shai:**
Yeah.

**Daniel Filan:**
Cool. Here's what I see computational mechanics as doing, just from a brief glance. It seems like it's really interested in stochastic processes. Kind of like time series inference of a bunch of things happen at... you've got this thing and then the next thing and then the next thing and then the next thing. And you want to predict the future. As opposed to forms of Bayesian inference where you're inferring the correct neural network that's underlying this IID, this independent identically distributed thing as opposed to various other things you could do with Bayesian inference.

And in particular, it's interested in hidden Markov models. I see a lot of use of hidden Markov models where basically, there's some underlying thing. The underlying thing is transitioning from state to state in some kind of lawful way. And these transitions are associated with emitting a thing that you can see. Each time a transition happens, you see a thing emitted. And basically in order to do well at predicting, you've got to understand the underlying hidden Markov model, understand what state is the hidden Markov model in. And if you could understand what state the hidden Markov model was in, then you do a really good job of prediction.

I then see this construction of a thing called an epsilon-machine, which as far as I can tell is the ideal hidden Markov model of a certain stochastic process, where basically, the states of the epsilon-machine are just somehow the sufficient statistics. Like you were saying, grouping together all pasts that lead to the same future things, that's just the states in the epsilon-machine. And there's some sort of minimality of, those are precisely the states you need to distinguish between. If you had any more states, it would be too many. If you had any fewer states, it would be too few to actually understand the process.

So you have stochastic processes, there's hidden Markov models, there's epsilon-machines. Then I think there's something about these inference hidden Markov models where if I'm seeing the outputs of a stochastic process, if I'm seeing just a sequence of heads and tails or tokens or whatever, there's some process where I am coming to have probability distributions over what state the thing might be in. I update those distributions. And that itself works kind of like a hidden Markov model where the states are my states of knowledge and the tokens of transitions are like, I see a thing. I move from this state of knowledge to this state of knowledge.

I also see this construction of going from underlying processes of generating to the process of inferring things. And I guess there are some interesting similarities and differences between the inference process and the generation process. My impression of computational mechanics is it's taking that whole thing, these stochastic processes, these hidden Markov models, these epsilon-machines, these inference hidden Markov models... You probably have a better name for them than that, but that's what I came up with.

**Paul Riechers:**
Maybe "mixed-state presentation".

**Daniel Filan:**
Mixed-state presentation. And then there's this question of, okay, how do I calculate interesting things about these processes? How do I actually find, I don't know, stationary distributions, or how do I get observables or higher order statistics out of that? This is what I've gathered from computational mechanics. I'm wondering, does that seem right? Am I missing stuff?

**Adam Shai:**
That was pretty good. I think there's a lot there. I'll just go at it without any particular order. But importantly, you brought up the epsilon-machine. For instance, one extra thing we can add on to what you said is that there's a distinction between minimal for the purposes of predicting versus minimal for the purposes of generating. And so the epsilon-machine is the minimal machine for the purposes of prediction. In general, you can get smaller machines.

**Daniel Filan:**
Where a "machine" is a hidden Markov model?

**Adam Shai:**
Yeah. I think it can be a little bit more general than that, but yeah, I think it's fine to think of it as an HMM. Yeah. The mixed-state presentations are HMMs in this framework, and the epsilon-machine is one. And you notice they actually both generate the same data set. You can think of, you have data. The data (for instance) that you train a transformer on, all of your training data. And these can even be - in theory not for a transformer in practice - but in theory these can even be infinite strings.

If you want to make an HMM that generates that data, there's an arbitrary choice a lot of the times. There are many, many HMMs that can generate that data, an infinite number of them in fact. And in comp. mech., they call these different presentations. And Paul, in [one of his papers](https://pubs.aip.org/aip/cha/article/28/3/033115/684965), in the intro, makes a really great point, and this is a deep conceptual view that I really love about comp. mech., which is that for different questions about a process and for the structure of the data, different presentations allow you to answer those questions with different amounts of ease.

If you want to ask questions about the predictive structure and what information you need to predict, then an epsilon-machine or a mixed-state presentation is a very good way to answer that question. Do you have some question that's different about generation or about pairwise correlations? Then different presentations will be more useful for that. Just this notion that the way that you present the data, the choice that you have of exactly which machine you use to generate the data with, even though it's the same exact data, with the same structure in some sense, allows you to answer different questions. That's just a few thoughts.

**Paul Riechers:**
Yeah, I guess you had a lot to unpack there in what you said. There are a few things I wanted to at least comment on. The first one is I think a common misconception, I think it's natural that you were saying, "Oh, it seems that computational mechanics is about HMMs." And I guess that's a common misconception. I'd say that actually HMMs turn out to be the answer to a particular question you can ask in computational mechanics.

The data-generating structure could be essentially anything. You could think through [the Chomsky hierarchy](https://en.wikipedia.org/wiki/Chomsky_hierarchy) or other types of ordinary differential equations or whatever that generates the thing. And then you ask the question of, again, what is it about the past that I need to remember to predict the future as well as possible? And then you have these minimal sufficient statistics, which are these coarse-grainings of the past. And then you look at what's the meta-dynamic among those belief states about the world, like you mentioned.

And then that thing turns out to be describable always as a hidden Markov model, if we allow ourselves infinitely many states. Because you can think about this basically via Bayesian updating, that given basically your knowledge of the world up to some point, even if you know the data generating process, you don't know the hidden state of the world. You just wake up in the morning and you have this impoverished view. You need to take in data and synchronize to the state of the world. You know something, you have something like a model of the world. Your data so far induces some belief state.

And then as you take in more information, how do you update your belief? Of course, just Bayes' rule, Bayesian updating. And so from a particular belief state, there is a unique answer of where you go to, given the new data. That induces a hidden Markov model. In fact, it induces a particular type of hidden Markov model, which in the lingo of comp. mech., computational mechanics, is "unifilar". But you can also think in the theory of automata, this is called deterministic. That's a little confusing because we're talking about stochastic deterministic automata. "Unifilar" is a better name. You're following a single thread through the states of this belief structure.

**Daniel Filan:**
Right. Just to clarify, it's stochastic in the sense that the states of knowledge are probabilities over what's going to happen in the future, but it's deterministic in that if you see a thing, there's just a deterministic next state of knowledge you go to?

**Paul Riechers:**
There's a deterministic way to update your state of knowledge. There's typically some stochasticity in terms of what in the world comes at you. And so, if you look at an ensemble of realizations, there would be randomness, stochasticity in terms of the transition rates between belief states. But in any realization, there's a thing you do to update your beliefs.

And then just a few of the things I wanted to engage with were: what's the role of the epsilon-machine here? I think especially for making a connection with understanding neural networks, I think the epsilon-machine isn't as important. I think this mixed state presentation, this updating beliefs is the primal thing that we care about. And it doesn't even require stationarity or anything like that.

If you do have stationarity, then there will end up being some recurrent structure in the belief state update. And then those recurrent belief states are the causal states of this epsilon-machine. I think it's a bit of the history of where comp. mech. came from that the epsilon-machine was important. And I think for the current way in which comp. mech. is useful, the epsilon-machine is not as important. But it is still interesting to say what are the recurrent states of knowledge that I would come back to for something stationary? Which maybe is relevant if you're reading a long book or something.

**Daniel Filan:**
Okay. Can I try my new understanding?

**Paul Riechers:**
Okay.

**Daniel Filan:**
And see if I've gotten any novel misconceptions. Here's my new understanding. You've got the stochastic processes. Stochastic processes are just things that generate sequence data, like the probability distribution over books that are going to be written - and a book is a really long sequence of words in this frame - or sequences of the state of some chaotic process maybe - just sequences of stuff. You want to understand this probability distribution over these sequences. What's going to happen in the future given the past?

And it turns out that hidden Markov models are a universal way of describing these stochastic processes. If you have a stochastic process, you can create a HMM, hidden Markov model, that generates it. In fact, you can create a bunch of hidden Markov models that generate it. There's also this process of updating your beliefs given some data that you've seen in this process. And that also looks kind of like a hidden Markov model, maybe not exactly because things are coming in instead of going out, but something like that.

And computational mechanics as I currently understand it is something like, "Okay, how do we relate these stochastic processes to hidden Markov models?" Which hidden Markov... You can construct a bunch of hidden Markov models that correspond to the same stochastic process. Which one do we want to play with and which one is most useful for various mathematical goals we might have for understanding these sequences?

If we want to understand the inference process, we can think of that in terms of some kind of hidden Markov model-like thing. And then there's just this process of understanding, okay, what actually happens? How? Maybe... you haven't said this, but I imagine there might be questions like, okay, how many states are there? What are the dynamics of belief updating? Where do you go to? That's what I now hypothesize computational mechanics is. How does that sound? What am I missing now?

**Paul Riechers:**
I'd say you've hit on a lot of relevant things. I think maybe even more interesting topics would come up if we're thinking about: what does any of this have to do with neural networks? Because like I said, computational mechanics is a very rich field. There's semester-long courses on this, so I don't think we could do it justice right away.

But I think we can get to a lot of neat ways in which the theory is being used and extended by us and our colleagues, as we're saying, "Okay, how can we utilize this to...?" Well, what we have done so far is basically, we have this new angle at understanding internal representations of neural networks, and also something which I think will become more clear in our upcoming work, the behaviors of models, like what to anticipate in terms of model behavior, in-context learning... There's really an amazing number of ways now to adapt the theory that we see, which is getting us to answer questions that I'd say weren't obvious to even ask before.

**Adam Shai:**
Yeah. I had a thought just because of something you said: one of the things we've been talking about recently is: we've been thinking about a lot of the work that's been going on in interpretability that uses the concept of "world models", and what the form of that work is, and trying to get closer and closer to "what is a world model? What is the type of stuff people do when they talk about world models in interpretability work?"

And this comes back to this issue of where HMMs fit in comp. mech., and are we assuming an HMM or something? One of the things I've been thinking about: even if you don't have stochastic data, if you have some other data structure - and there's a lot of data structures that one could have - you have some implicit or maybe explicit structure to the world in which you want to ask, does this transformer represent somehow in its internals?

Well, no matter what that data structure is, at the end of the day, if you're going to train the transformer on it, you have to turn it into sequential data. It takes sequences of tokens. And so, you can go in and you can probe the model internals for structures associated with this original data structure in your head that might not have anything to do with sequences. That's a fair game to play, and I think people sometimes do that. Maybe you conceptualize Othello as not really being about sequences, but being about some other type of data structure, about states of the board where the top left corner is black, white, or empty. And then you can go in and [you can probe](https://www.neelnanda.io/mechanistic-interpretability/othello) your Othello-GPT to see if that's [linearly represented in the residual stream](https://arxiv.org/abs/2310.07582).

But when you train the Othello-GPT, you are transforming the game of Othello to a specific tokenization scheme that gives you specific sequences. And those sequences are naturally thought of as an HMM. And then - this work isn't done, so this is more thoughts and things we hope to do - but we should be able to take results like that, reframe it from a comp. mech. point of view, and then run our analysis and apply our framework to it to even make hopefully theoretical claims explaining the results that they've gotten. That certain ways, certain probes for properties of the game state are linearly represented, and others are non-linearly represented.

If we take our findings seriously that this belief state geometry... I guess we haven't spoken too much about that yet, but if that belief state geometry is the thing that the transformer is trying to represent, then hopefully we can explain a bunch of these other results that people have been finding.

**Daniel Filan:**
I think first, I just want to make sure that I really understand what this theory is before I move on there. But I'm chomping at the bit to get to that later.

Okay. Here's a really basic question. Why is it called computational mechanics? Does it have something to say about computation or mechanics?

**Paul Riechers:**
Yeah. It's a kind of interesting historical note. I think it makes more sense when you realize it's called computational mechanics in distinction to statistical mechanics, which, okay, this makes some sense. What's statistical mechanics? In physics there is this theory of thermodynamics, of course, which is this high-level... you just look at these macro states and how they evolve.

Thermodynamics doesn't require a microscopic description, but then it turned out that, oh, actually, if you believe in atoms - I mean this is kind of why people started re-believing in atoms - then you can actually explain some of the thermodynamic behaviors, and you can figure out material properties and stuff. How? Just basically by saying how random something is. We have entropy that quantifies that. And by saying how random something is, we can derive all of these properties.

But it's a static view of what's going on. You just look at the statistics. Okay. When the question shifted to say: what are the dynamic aspects? What are the computational aspects? Somehow there's... nature's intrinsically computing. You think of [Shannon's channel](https://dennisdjones.wordpress.com/wp-content/uploads/2013/04/shannon-info2.gif) that information would go through. You can also think of the present as a channel that the past must go through to get to the future. If you take this on, there's some sense in which nature's naturally computing the future from the past.

That's the idea of computational mechanics. And yeah, it's also taken and built upon a lot of ideas in computer science and information theory. Unfortunately, it also has the same name as something which is quite different, which is... I don't even know exactly what it is, but it's like-

**Adam Shai:**
How to use computers to model Newtonian mechanics.

**Paul Riechers:**
Something like that. That's a bit confusing. But yeah, the computational mechanics that we're talking about here is in distinction to statistical mechanics.

**Daniel Filan:**
Got it. I guess the final thing that I want to ask is, I think you mentioned a bit offhandedly this idea of: how can you do prediction under some sort of resource constraints? Maybe it was not being able to distinguish certain states of the world, or maybe it was something else. I'm really interested in that because I think this is a question that Bayesian statistics doesn't have an amazing answer for just off the shelf. It's a question that is obviously really relevant to AI. Can you tell us a little bit?

**Paul Riechers:**
Yeah. We're also really interested in this. I mentioned that some work has been done in comp. mech. on this, but I'd say it's underdeveloped. But [Sarah Marzen](https://www.sarahmarzen.com/) and other colleagues have [worked](https://arxiv.org/abs/1412.2859) on, thinking of rate-distortion theory, [how does it apply](https://journals.aps.org/pre/abstract/10.1103/PhysRevE.95.051301) in this dynamic framework of computational mechanics when maybe your memory is limited?

**Daniel Filan:**
What is rate-distortion theory?

**Adam Shai:**
Conceptually, you can imagine a plot where on the x-axis, you have your memory capacity, and on the y-axis, you have your accuracy at predicting the future, and there's some kind of Pareto optimal curve you can get. So, for a certain amount of memory constraint, you can only get so far in accuracy, and there's some curve that describes the optimal trade-off there.

**Daniel Filan:**
So, shape of the curve, I guess, probably how you get to the curve. And so, you were saying there was some work by Sarah Marzen and others on taking some sort of rate-distortion theory view and applying it to, I think, dynamical processes or something?

**Paul Riechers:**
Yeah. So, say, instead of the causal states, instead of these ideal belief states, how would you compress those to still predict as well as possible given the memory constraints. So related to information bottleneck methods, there's this causal bottleneck stuff. So, that's one way in which you can think of a constraint. There's memory constraints. But there can be all sorts.

There's an interesting avenue on the physics side, where, actually, if you allow yourself something like quantum states, quantum information can somehow compress the past better than classical information could. So, there's this interesting branch of computational mechanics which has said, actually, we can use quantum states to produce classical stochastic processes with less memory than otherwise classically necessary. So that's interesting. Sounds very tangential, but a bit of a teaser. Actually, we think that neural nets do this. So, that's a bit wild, and again, something you just probably wouldn't think to look for if you weren't building off this theory.

## Computational mechanics vs other approaches <a name="comp-mech-vs-other-approaches"></a>

**Daniel Filan:**
Sure. I guess finally, to just triangulate computational mechanics a little bit more, I want to compare it to two other things that sound kind of similar. So, the first is, people who have followed the AI safety space, people who have listened to [a few episodes back in my podcast](https://axrp.net/episode/2024/05/07/episode-31-singular-learning-theory-dan-murfet.html) might be familiar with singular learning theory. Right? So, this is the most recent really math-y way of thinking about Bayesian statistics that has taken the alignment world by storm.

How do you see computational mechanics as contrasting with singular learning theory, perhaps, both in just what the theory even is or is trying to do and how it might apply to AI systems?

**Paul Riechers:**
I have a quick comment, maybe, then Adam [maybe] wants to elaborate, but at a very high level, computational mechanics is a model-agnostic theory of prediction. Whereas, I think SLT takes on very seriously that, if you have a particular model, in that you have a particular parameterization of the model, then, what's learning like?

Singular learning theory, SLT is a Bayesian theory. I think sometimes the way in which it's applied to talk about actual learning with, maybe, stochastic gradient descent confuses me a little bit, but I think it's a genuine mathematical theory that, I think, likely will say something useful about inference. Maybe it can be complementary to comp. mech. in addressing some of those aspects. Do you have further thoughts on that?

**Adam Shai:**
Yeah, I mean, first of all, I'm not an expert at all on SLT, although I've been highly inspired by them and the entire group at [Timaeus](https://timaeus.co/). I think they ask a lot of questions that are similarly motivated. So, for instance, I think they really do a good job at talking about their motivations about the computational structure of these systems and how can we get a mathematical and theoretical handle on those things?

I think their tactic and framing of it, and this might be more of a vibes thing than... again, I'm not an expert in the math at all, so I really can't speak to that. But I think, naturally, they take a view that has more to do with learning and the learning process, and I think they want to, at the end, say something about the structure of that learning process and the geometry of the local landscape, and how that relates to the computational structure at a particular weight setting. Whereas, I think comp. mech. most naturally, at least, takes a view that really has to do with the structure of that inference process directly.

But then, of course, one can think of extensions: Paul's already done some work extending that to the learning process a little bit. So I think they're trying to get to a very similar set of questions, and a lot of these I consider deep conceptual questions at the heart of how do AI systems work? What does it mean to talk about structure of computation? What does it mean to talk about the structure of training data, but from different starting points. The learning process versus the inference process.

**Daniel Filan:**
Right. In some ways, they seem sort of dual, right? Like figuring out the weight-updating process versus fixed weights: where should it go?

The other thing I want to ask to triangulate - again, a [somewhat recent episode of AXRP](https://axrp.net/episode/2024/05/30/episode-32-understanding-agency-jan-kulveit.html) - is, when I hear people talk about, "oh, yeah, we've got a really nice mathematical Bayesian theory of updating based on evidence and we're going to use it to understand intelligence", one place my mind goes to is active inference, this [Karl Friston](https://en.wikipedia.org/wiki/Karl_J._Friston)-style work.

I'm wondering if there's any interesting connections or contrasts to be brought out there.

**Adam Shai:**
I think, again, this will also be a little vibes-y. I think the "active" part of active inference is talking about, as far as I understand it, an agent taking actions in the world. There's a part of comp. mech. that has to do with interacting systems called transducers, and that is so far not what we've applied to neural networks. So, that part's a little different if you just take the "not transducer" part of comp. mech., at least, and compare it to it.

Also, the way that I view active inference - again, as a non-expert - is it takes seriously... I guess it matters what form of active inference. So, often, it starts with just "we think the brain's doing Bayes" or "an agent is doing some Bayesian thing", and then, you can get it down to an actual formula by kind of assuming... it's hard to do Bayes in full, so let's find some kind of mathematical way that you can approximate it, and they get to a specific set of equations for that Bayesian updating, and...

Yeah, so it kind of assumes a certain method of approximating Bayesian inference where, again, we're pretty agnostic to all that stuff in comp. mech.. So those are just some thoughts.

**Daniel Filan:**
Sure. I guess, I am limited in my ability to wait until I dive into your paper and your interesting work. But is there anything you would like to say just about computational mechanics as a theory before I dive into that?

**Paul Riechers:**
Yeah, I guess just to re-emphasize two things: One, it's, as it is currently, not the answer to everything, but has been really useful. But also, I think it's a great springboard to continue on. So, we've recently founded this research organization, [Simplex](https://www.simplexaisafety.com/), because we believe that we can really build on this work in useful ways. So just that contextualization might help.

## What world models are <a name="what-world-models-are"></a>

**Daniel Filan:**
Sure. Let's talk about building on it. You've recently published this paper, ["Transformers represent belief state geometry in their residual stream"](https://arxiv.org/abs/2405.15943) by Adam, [Sarah Marzen](https://www.sarahmarzen.com/), Lucas Teixeira, [Alex Gietelink Oldenziel](https://sites.google.com/view/afdago/home), and Paul. What did you do in this paper?

**Adam Shai:**
We were motivated by this idea of: what is a "world model"? What is, fundamentally, the structure that we're trying to get... That we're training into these transformers when we train them on next token prediction, what is the relationship of that structure to a world model? How can we get a handle on these questions?

I guess one of my own personal... I don't know if it's a pet peeve, but something like a pet peeve is it feels like, often, there's just this never-ending discussion that happens in the AI community at large about: these things, do they really understand the world? Do they have a world model? Do they not? And these things, these are slippery concepts and it feels like the same conversation has been happening for a while now. And our hope is that, if we can get to some more concrete handle, then, we can actually know what everyone's talking about. We don't have to keep on talking past each other. We can answer some of these questions.

So, the tactic that we take in this paper is to start with a ground truth data-generating process. So, we know the hidden structure of the world, so to speak, that generates the data that we train a transformer on. And then, we can go in and we can, first, make predictions using the theory of computational mechanics about what structure should be inside of the transformer. Then, we can go and we can see if it is there.

So, we need a way to figure out what we should look for in the transformer. And so, we need a way to go from this hidden data-generating structure, which takes the form of an HMM in our work, to a prediction about the activations in the transformer. So, we just ask the question: what is the computational structure that one needs in order to predict well given finite histories from this data-generating process, if we want to predict the next token.

That, in computational mechanics, takes the form of something called a mixed-state presentation, which describes how your belief about which state the data-generating process is in, how that belief gets updated as you see more and more data.

That, by itself, doesn't give you a prediction for what should be inside the transformer because that's just another hidden Markov model. But there's a geometry associated with that, and that geometry is given by virtue of the fact that your beliefs over the states of the generative process are, themselves, probability distributions. Probability distributions are just a bunch of numbers that sum to one, and you can plot them in a space where each thing they have a probability for - in this case, it's the different states of the generative process - are a different axis.

And so, the points lie in some geometry in that space. And in one of the first examples we used in the paper, and it's featured in [that blog post on LessWrong](https://www.lesswrong.com/posts/gTZ2SxesbHckJ3CkF/transformers-represent-belief-state-geometry-in-their), even though it's a very simple generative structure, you end up getting this very cool-looking fractal. So, it feels like a highly nontrivial geometry to kind of expect and predict should be in the transformer.

And what we find is that when we actually go and when we try to find a linear plane inside of the residual stream, that when you project all the activations to it, you get the fractal, you can find it. And not only can you find it, you can perform that analysis over training, and you can kind of see how it develops and refines to this fractal structure.

**Paul Riechers:**
And that it's linearly represented also.

**Adam Shai:**
And it's linear. So we were very excited about that. It means that we have some kind of theoretical handle about: given the structure of our training data, what we should expect geometrically in the activations of a transformer trained on that data. So, it gives us a handle on model internals and how it relates to training data.

**Daniel Filan:**
Gotcha. When I'm thinking about this, one thing I'm paying attention to is, like you said, what does it mean for a thing to have a world model? And I guess part of the contribution of computational mechanics is just distinguishing between the dynamics of generating some sequence, and the dynamics of what inference looks like, what states you go through with inference.

And would it be right to say that a big deal about your paper is just saying: here's the dynamics of the inference that we think Bayesian agents should go through when they're inferring what comes next, what comes next... Or even just inferring the whole future. And saying: hey, we found the dynamics of inference encoded in the neural network.

**Paul Riechers:**
Yeah. I guess one of the points that we hope is obvious in hindsight - but I think a lot of good results are obvious in hindsight, so it's hard to feel proud of yourself - but it's just this thing of "what does it mean to have a world model?" And we just took this question pretty seriously: what's the mathematics of it? And then, there's these speculations, can next token prediction do this? And then, the answer is concretely yes.

In fact, not only must neural nets learn a generative model for the world, but they also must be learning a way to do effectively Bayesian updating over those hidden states of the world. And I guess, what's more, is it's not just tracking next token probability distributions, actually, the model will differentiate basically states of knowledge that could have identical probability distributions over the next token, which seems a little odd if you just think about, "I'm just trying to do well at next token prediction."

But again, in hindsight, it kind of makes sense that if there will be a difference down the road and you just merged those states early on, then, even if your loss at this time step or at this context position is just as good, eventually, as you move through context, you'll have lost the information needed to distinguish what's coming at you. So, that's this thing about predicting the whole future: it's just this iterative thing. If you do next token prediction on and on and on, eventually, you want to get the whole future right.

And not only is there this causal architecture, but as Adam said, there's this implied geometry, which is pretty neat. And I think there's all sorts of hopes. We've had a lot of good community engagement. People have talked about, well, maybe this means we can back out what a model has learned if we can somehow automate the process of this simplex discovery. There's a lot of, I think, directions to go that we're excited about and we think the community can also help out.

**Adam Shai:**
Just to add one thing: to reiterate the point that you made about the structure of inference versus the structure of generation, I do think that at least for me, it was a super core conceptual lesson coming from a neuroscience background. Because in the section of neuroscience I was in, at least, we would talk a lot about "the cortex's role is to do this, to implement this predictive model". But then, we would always instantiate that in our minds as generative models.

If you asked any particular neuroscientist in that field, including me just a few years ago, "but don't you have to use the generative model to do prediction?" You'd be like, "yeah, of course you have to do inference over it", but it'd be kind of a side thing. Once you have the generative model, it's obvious how to do inference, and there's not much to it. That would be the implication.

But, actually, the structure of inference is enormous, right? From this three-state HMM - and this is a simple example - we get an infinite-state inference structure that has this fractal thing. So the structure of inference itself is not something to just shrug away. In some sense, if you're trying to do prediction, it is the thing. And its structure can be quite complicated and interesting.

And so, I actually think the neuroscientists could learn a lot from this, too. Although, obviously, we applied it to neural networks.

**Paul Riechers:**
And there's maybe one other idea that you could usefully latch onto here, which is something like thinking of predator and prey. You can have a fox and mouse or something like that, and the mouse can be, maybe, hard to predict (or eat) with a simple generative structure. It just does something. And all the fox has to do is predict what it's doing. But it's not sufficient for a fox to be able to act like a mouse. It has to actually predict it.

And that's maybe some evolutionary pressure for it to have a bigger brain and things like this. And I think this is... is this a weird analogy? But I think it's actually quite relevant: how smart should we expect AI models to be? It's not just that they need to be able to act like a human, they need to be able to predict humans. And that's a vastly greater capability.

We can't do that with each other too well. I mean, we do a little bit.

**Daniel Filan:**
But it's hard.

**Paul Riechers:**
But it's a superhuman task, actually. Yeah.

**Daniel Filan:**
Yeah. Have you played the [token prediction game](http://rr-lm-game.herokuapp.com/)?

**Paul Riechers:**
I've played various versions of this. I don't know if it's the one you're referring to.

**Daniel Filan:**
Yeah. It's just a web app that... Basically, the game is to literally do what these transformers have to do. So, you see just a prefix, or actually, I think you start with zero prefix. You just have to guess what the next token is.

I think it even shows you four possibilities or something, and you have to just guess, yeah, what is the next thing going to be? And then, it gives it to you. And then, you have to guess, okay, what is the next thing going to be?

And it's really hard. Yeah, I don't know. Somehow, I'd never made this comparison before, but it's much harder than writing something.

**Paul Riechers:**
Yeah. For sure. And there's also something interesting in that meta-dynamic of doing the prediction, which is, as you get more context, you'll probably be better at predicting what's next. Okay, now, I have some context, right?

And so, this is some quantitative stuff that, just, again, once we take these ideas seriously, we can say "as you move through context, your entropy over the next token, if you're guessing, should decrease". How does it decrease? Actually, that's one of the things I worked on during my PhD in comp. mech.: there's this way to calculate this via the spectral structure of this belief state meta-dynamic and all that.

But there's something to this of: that model, since they're predicting, they should and they will converge to users in context. What is that? That is a definite type of in-context learning. It's a bit, I think, our task to show whether or not that's the same of what other people call in-context learning, but that's a very concrete prediction that, especially with non-ergodic data, which is the case for us, that, in general, you're going to have this power law decay of next token entropy, which is borne out empirically.

## Fractals <a name="fractals"></a>

**Daniel Filan:**
Sure. So, there's tons to talk about here. First, I want to ask kind of a basic question. Just aesthetically, if I weren't trying to read your paper, one of the coolest things about it would be this nice colorful fractal that you guys have, right? Why is it a fractal?

**Paul Riechers:**
So, I guess one way to think about that is, what is a fractal in general? Well, a fractal is something like iterating a simple rule over and over, right? That's a self-similar structure you get out of that. Well, what's the similar rule?

Well, we have a simple generative structure, and then, you're doing Bayesian updating over that. There's different types of outputs that it could give. Each output gives a different way of doing Bayes updates over whatever your distribution of the states is.

So, you're doing the same thing over and over, and the geometric implication of that is fractal. Is that helpful?

**Daniel Filan:**
Yeah. I think this maps onto what my guess was.

**Adam Shai:**
I'll also point out that after the LessWrong post we made, John Wentworth and David Lorell made [a follow-up post explaining that iterative game structure](https://www.lesswrong.com/posts/mBw7nc4ipdyeeEpWs/why-would-belief-states-have-a-fractal-structure-and-why). And then, there's also been [academic work](https://arxiv.org/abs/2008.12886) explaining the same thing from the point of view of iterative functions, from [Alexandra Jurgens](https://csc.ucdavis.edu/~ajurgens/). So, those are two resources.

**Daniel Filan:**
Cool. The next thing I want to ask is: picking off a thing you mentioned very briefly of potential follow-up work being to try and find this fractal structure as a way of understanding what the network has learned.

Is there something to say about... So, you have these fractals of belief states from this mixed-state presentation or something. Is there some way of going from "here's the fractal I found" to "here are the belief dynamics"?

**Paul Riechers:**
Yeah. I think this is getting at an important point that the fractal... those are the beliefs; they're not the dynamic among the beliefs.

**Daniel Filan:**
Yep. They're not the updates.

**Paul Riechers:**
Yeah. And I think an obvious thing that we'd like to do is: now that we find this thing, is it now natural to do some mech. interp. sort of thing of finding a Bayesian updating circuit? That would be nice.

It's not totally clear how this works out, but it's a natural sort of thing for getting at, hopefully, a thought process of the machine, right?

I mean, "what is its internal dynamics among these beliefs?" is a great question that we are pursuing. A lot of this is empirical, because now we have this theoretically-grounded thing, but we also need to work with the actual implementation of transformer architecture. How does that actually instantiate the thing?

**Daniel Filan:**
Yeah. I guess I also have a theoretical question of: suppose you had this fractal, somehow. You had it to infinite precision. Is it possible to back out the belief HMM or the stochastic process? Or are there multiple things that go to the same fractal?

**Paul Riechers:**
So, there would be multiple choices. There's degenerate ways in which you can choose to represent a generative structure, but it's all the same stochastic process. And that's the thing that matters: the world model that it has. So there really shouldn't be that much degeneracy at all.

In fact, in the current toy models that we've used to say what happens in the simplest framework, then, we have to project down, we have to find a projection in the residual stream where this fractal exists.

Actually, probably, in frontier models, the residual stream is a bottleneck. So, you're not going to have to project down. It's going to try to use all of its residual stream. So, in fact, it maybe is easier in a sense, but then, it's also doing this probably lossy representation that we're also trying to get a handle on.

**Daniel Filan:**
Yeah. I guess I wonder, if one fractal does correspond to one generative process, maybe we don't even need to do mechanistic interpretability, and we can just say, "oh, yeah, here's the geometric structure of the activations. Bam, this is the thing that it learned."

**Adam Shai:**
Yeah. I don't want to overstate what we're claiming here. The way that I would think about what this belief state geometry is, is not... Maybe it'll be useful to try to think about features, the way people talk about, and what they are and what the relationship is between features and these belief states, even just philosophically.

I think it would be a mistake to claim that the belief states are the features. I think of features, at the moment, at least - and they're kind of ill-defined - but I think of them as the kind of computational atoms that the neural network will be using in the service of building up the belief state geometry and the dynamics between those belief states.

Of course, how a neural network builds the belief state geometry up probably will be highly, highly dependent on the exact mathematical form of the network they're using. So, in transformers, the fact that you have a residual stream with an attention mechanism, the attention mechanism is, literally, the only place the transformer can bring information from the past to the current token position.

And it has a certain mathematical form... Strangely, a lot of it is linear, not totally obviously. But that mathematical form puts constraints over what information can be brought from the past to the present, and it puts strong constraints over exactly the manner in which you can build up this belief state dynamic.

So, yeah, it would be really cool to be able to even... It's like a version of comp. mech. that somehow takes into account the kind of mathematical constraint that the attention head puts on you in your ability to bring information from the past to the present. Which we very much don't have, but which you could vaguely imagine.

**Paul Riechers:**
Although Adam has done some preliminary experiments where you look at basically this factor we were talking about; how that's built up through the layers and in the different... from attention, from MLPs. I mean, it's surprisingly intuitive and beautiful. So encouraging at least.

**Adam Shai:**
So for instance, if you take the fractal example in the Mess3 process, it's a vocab size of three, so it only speaks in As, Bs and Cs. So it's just strings of As, Bs and Cs. And then you put it through the embedding and you see just three points. So it's like three points in a triangle, one for each of the tokens. That makes sense. And then that's the first place that you're in the residual stream. Then you split off and you go into the first attention mechanism. And what you see happen in the first attention mechanism is it spreads out the points to a filled-in triangle.

And then when you add in that triangle back into the residual stream, you're adding in the triangle to these three points in a triangle. And what you get is three triangles in the shape of a triangle and then so on and so forth. And then actually it goes into the MLP and you can see it stretch out these three stacked triangles to look more like the simplex geometry. And then every time you go through another layer, it adds more holes and more triangular structure. And you very nicely see a very intuitive feeling for how every part of the transformer mechanistically puts in extra structure in order to get to the belief state geometry.

**Paul Riechers:**
And this is one example of how we hope that computational mechanics can be something like a language for bridging between mechanism and behavior. A lot of mech. interp. has been great, but it's pretty low level. There's just results here and there. So one thing computational mechanics can do is create a language to tie this together to unify the efforts, but also where is this leading? And so hopefully there's a little bit more of a bridge between the mechanism and behavior.

## How the fractals are formed <a name="how-the-fractals-are-formed"></a>

**Daniel Filan:**
Sure. So there's this question about what we learned, and we've talked about that a little bit. We've learned that these fractals, they're represented, you can get this mixed-state presentation in the residual stream. Another result that comes up in the paper... well, one result is that it's linear. Even in retrospect, are you surprised by that or do you think you have a good story for why it should be linear?

**Adam Shai:**
I don't have a good story, and in fact, computational mechanics, as far as I know, it does not get... Right? It just says that the mixed-state presentation should be there. It doesn't say how or where.

**Paul Riechers:**
Yeah. I think when we decided to do these experiments, based on this theoretically-informed thing: somehow this geometry should be in there. These belief states should be in there. Even this geometry should be in there, but at least it's going to be stretched out or something. Right? But it's like, okay, there's still definitely something to explain. I think I've seen titles and abstracts of papers that I feel like maybe help us to understand why things are linearly represented, but I don't yet understand that well enough.

It's too good to be true, but hey, good for us.

**Daniel Filan:**
Yeah. Maybe this has a similar answer, but another result that comes out is that at least in one of these processes that you train a transformer on, there's not one layer in the residual stream that you can probe and get this nice simplex. You have to train a probe on all of the layers. Right?

**Adam Shai:**
Yeah.

**Daniel Filan:**
Is there a story there of just like, "Well, one layer would've been too good to be true," or is there something deeper?

**Adam Shai:**
Actually in that case, there is a deep reason that computational mechanics gives for that finding. Although I should say we were not looking for that finding. I think one of the nice things about having a theoretical foundation for the way that you go about running your experiments is that sometimes phenomena jump out at you and then in hindsight you realize that the theory explains it. So in this particular process, this is the random random XOR process where if you start from a particular state, you generate a bit, a 0 or 1, then you generate another bit 0 or 1, and then you take the XOR of the previous two, and then you generate a bit, you generate a bit, XOR, so on and so forth.

So that's your data-generating process. It has five states in its minimal generative machine, and that means that the belief state geometry lives in a 4D space. And actually unlike the fractal thing, you actually get 36 distinct states. It's also quite a beautiful structure. Maybe there's a way to splash that on the screen.

**Daniel Filan:**
Probably. We have a low budget.

**Adam Shai:**
Okay. But it kind of looks like an X-ray crystallography pattern, and so it's aesthetically pleasing, but importantly, multiple of those belief states, even though they are distinct belief states, they give literally the same next token prediction.

So what does that mean? The transformer... all it needs to do, if we just think of the last layer of the transformer and how it actually gets made into a prediction for the next token, you just read off of the residual stream with the unembedding and convert it to a probability distribution. But that means that at the last layer, you actually don't need to represent distinctions [other than] in next token predictions.

And in fact, not only relative to the local thing of doing the next token predictions, but there's no attention mechanism after that. So even if you wanted to represent it there, it could never be used there because it can never interact and send information into the future.

**Paul Riechers:**
Not of the next token, but everything else.

**Adam Shai:**
Yeah. But still, comp. mech. says the structure, these distinctions should be there. They don't need to be in the last layer, but they do need to be somewhere. You see them in earlier layers. So that's an explanation for why it's not in the last layer.

**Paul Riechers:**
I think this hints at maybe how the transformer is using the past construction of the belief states; that if it's earlier on, then you can leverage those across contexts a little better. So I think there's strong interplay here between theory and experiment that there's still a lot for us to figure out.

**Daniel Filan:**
Sure. Sorry, just a very basic conjecture I have after hearing you say that, is just that the parts that are relevant for next token prediction are in the final layer, the parts that are relevant for the token-after-that prediction are in the second-to-last layer...?

**Paul Riechers:**
Well, I don't think it's quite that clean, because if that's the case, then you'd only be able to model Markov order N processes where N is the number of layers. So it's not going to be quite that clean. But something like at the end, you really only need the last token distribution. But then also the theory says that somewhere in the model, somewhere in the residual stream, there should be these representations of the full distribution over the future. And it does seem that to be able to... for a certain context position to look back and utilize that, it would serve a purpose for those to come about early on.

It's still not clear to me: why bother shedding them? They could persist as far as I'm aware, but there's not a pressure for them to persist. So I'd like to understand that better.

**Daniel Filan:**
Sorry to get hung up on fractals, but I have another fractal question. So this random random XOR belief state thing didn't form a fractal. Is that just because the fact that one in three bits is deterministic - does that just snap you to one of these relatively well-defined states? Is that what's going on?

**Paul Riechers:**
I wouldn't classify it that way, but the distinction, I think... One way to anticipate whether or not you're going to have a fractal structure is whether the minimal generative mechanism is this thing I alluded to earlier, about whether it's unifilar or not, whether it has these deterministic transitions between hidden states. So for the random random XOR, the minimal generative mechanism is a five-state thing, and it's also a five-state unifilar thing. So it is the recurrent belief states. So you have these 36 belief states in general that, as you go through resolving these different types of uncertainty, you'll eventually get to these five states.

So okay, that's simple. Whereas if you have even a simple generative mechanism, let's say you can have as small as a two-state hidden Markov model that generates the thing, but it's non-unifilar, in the sense that if you know the current state of the machine and you know the next token, it still doesn't tell you exactly where you end up. So the number of probability distributions induced can be infinite and generically is.

So in that setting, again, you're folding your beliefs over and over through Bayesian updating. So in general, if you have a minimal generative model which is non-unifilar, Bayesian folding will give you a fractal, and if the minimal generative structure is unifilar, then you won't. So that's also kind of nice that we can expect that.

**Daniel Filan:**
Sure. Cool. Again, speaking of fractals, in the paper you basically say, "Hey, here's the fractal we expect to see. Here's the fractal we get." And it's pretty close in mean squared error. It's way closer than if you had a randomly-initialized network or way closer than it was during the start of training. And my understanding is you're measuring this in mean squared error terms, right? A similar question I could ask is, "Okay. Suppose that instead of the actual mixed-state presentation of the ideal process, I just had this thing I got from the network, how good a job would I do at prediction?" I'm wondering: is that a question that there's a nice answer to? And do you know what the answer is? And if you do know what the answer is, please tell me.

**Paul Riechers:**
So it's a little unclear to me what you're asking, but you might be asking something like: can we do something with the activations if we don't know the ground truth of the generative structure? Is that right?

**Daniel Filan:**
Or instead of measuring the distance between activations and ground truth by mean squared error of this probe, one thing you could do is say, "Okay, imagine this is the mixed-state presentation I have, how good a job am I going to do at predicting the underlying the actual thing?" Somehow the thing you've probed for seems like a lossy version of the underlying fractal. How much does that loss show up in prediction as opposed to reconstruction error?

**Adam Shai:**
The one thing you should be able to do is - and this is not an infinitely general answer to your question - but if you think of the fractal that is the ground truth mixed-state presentation, and then you think of coarse-graining that fractal to a certain amount, Sarah Marzen has [work](https://journals.aps.org/pre/abstract/10.1103/PhysRevE.95.051301) going through the math of the implication for predictive accuracy. And that intuitively (I think) makes sense.

And in general, I think it should be possible to work out given any particular coarse-graining - so what does that mean in this particular case? It means if you take just kind of some volume in the simplex and you just kind of say, "I know I'm in this area, but I don't know where exactly, I don't know exactly which belief state I'm in, but I know I'm somewhere there" - you should also be able to calculate the consequences of that and its implications for next token predictive accuracy or cross-entropy loss. What that looks like in terms of the visual of the fractal is exactly the fractal just getting fuzzier.

**Paul Riechers:**
Yeah. Nearby points in the simplex do make similar predictions. So there's some sense in which... Yeah, if it looks fuzzy, then you can almost visually say how that's going to turn out for prediction and we can quantify that.

**Daniel Filan:**
Right. Am I right that this is because the simplex is distributions over hidden states and therefore being close by in the simplex is just close by in your whole-future predictions rather than just next token predictions?

**Paul Riechers:**
You can think of: how do you calculate probability of future tokens from a distribution over the model? And it's basically linearly in terms of the distribution over the model. And so if you slightly reweight, there's a continuity there. You can talk about closeness and a type of distance induced, but it's not exactly the natural Euclidean one.

## Scaling computational mechanics for transformers <a name="scaling-comp-mech-for-transformers"></a>

**Daniel Filan:**
Gotcha. I guess the next thing I want to ask is just how to generalize from these results. My understanding is that both of the processes could be represented by hidden Markov models with five or fewer states. In general, of course, processes can have a bunch of states in their minimal hidden Markov model. How scared should I be of "maybe things are really different if you have a hundred or infinity states that you need to keep track of"?

**Adam Shai:**
You should be excited, not scared.

**Daniel Filan:**
Okay.

**Adam Shai:**
We can just go to the full thing. What's the minimal generative structure for all of the data on the internet? That's a large thing. It's a very large thing. It is way larger than the number of dimensions in the residual stream of even the biggest model. It's not even close.

So the natural kind of bottlenecks that exist in the strongest versions of artificial intelligence systems that exist today do not have the capacity to - in a straightforward way at least - represent the full simplex structure that this thing predicts, which means that there is at least one extra question about... Some compression must be going on. What is the nature of that compression? This is something we're very excited [about] and looking at now.

I think the main thing that I get from our original work, although all of these things about fractals and finding fractals and transformers, all that is true and these are good paths to go on... But the more general point, and the thing that gets us excited, is that it gives us evidence that this framework that we're using to go about thinking about the relationship between training data structure, model internals and model behavior works.

And now we can start to ask questions like "what is the best way to compress into a smaller residual stream than you really have? And so now we don't have space to represent the full structure, how do we do that?" And we don't have an answer to that at the moment. But it's definitely, I think, one of the things we're very excited to get a handle on.

**Paul Riechers:**
And at least the experiments become clear once you have the theoretical framework: now we know where the simple picture should break down and we can look there. So we are very excited to see how things start to break down and how the theory generalizes to understand more towards frontier models.

**Daniel Filan:**
Yeah. I guess you've maybe already talked about this a little bit, but I'm wondering: in terms of understanding these mixed-state presentations in transformers or just generally applying computational mechanics to neural nets, what are the next directions you're most excited by?

**Paul Riechers:**
There's an incredible amount that we're excited by. We currently have this team of the two of us and collaborators in academia and also more generally in the AI safety interpretability community. But we're really excited to create more opportunities for collaborations and scaling up because it's a bit too much. But let's get specific. There's some obvious next projects: the AI safety world is a little obsessed by [SAEs](https://arxiv.org/abs/2309.08600) (sparse autoencoders), right? And features. What even are they? How do we know if we're doing a good job? There's no ground truthiness to it.

And so one thing we're excited to do is just say, "Well, let's benchmark, in the cases where we know what the machine is trying to do, that these are the optimal belief states: how do belief states correspond to features?" And Adam alluded to this earlier, but one working hypothesis could be "features are these concise, minimal descriptors that are useful for pointing at belief states." Okay. So let's test that out. So we're doing a collaboration right now where we're trying to make this comparison. I think it's going to be pretty empirically driven for now, but I think that should be quite useful broadly to know what we're doing with features.

There's a lot that I'm doing right now in terms of understanding internal representations more generally in a few ways. A lot of what we've done right now is, what do you expect to happen once the model has been well-trained? And then it's like, "Okay, it should have this structure at the end." But how should that structure emerge over training? And there's a few ways in which we have a handle on that, and it's something I'm very excited about. So I'm working actively on that, and we have a lot more. Do you have some favorites you want to talk about?

**Adam Shai:**
Yeah. It's kind of a problem in that... For instance, just last week there was [this Simons Institute workshop](https://simons.berkeley.edu/homepage) that had to do with AI and cognitive psychology. I went and I watched a few of the talks, and this is something increasingly that's been going on with me - not to get too personal - but every time I see a talk about AI, often my reaction is like, "Ah, I think we can get a handle on this using our framework."

So I just have a list of papers that keeps on growing about things I would like to explain from a comp. mech. point of view. But just to name a few of them: there's always in-context learning, which I think is a big one. You can start to think of a way to get a handle of that using non-ergodic processes. So what that means is, in the examples that we've talked about in our public-facing work so far, the generative models are all single parts. There's one hidden Markov model. You go recurrently between all the states.

You can also imagine processes where you just take a bunch of those and put them side by side. In addition to the beliefs having to see more data and try to figure out in which state in the process you're in, you also have to figure out which process you're in. So there's this extra meta-synchronization process going on.

**Paul Riechers:**
And it's natural also for the type of data that you'd be training on, because you have this whole hierarchical structure of "what language, what genre, what mood", and there are these recurrent components, but many different recurrent components.

**Adam Shai:**
And you can imagine that dynamic being quite different for sets of processes which overlap in their internal structures more or less. And that might have implications for which kind of abstract structures you use and take advantage of in in-context learning. So that's one thing I think we're quite excited about.

Another one is related to that: just talking more generally about the capabilities of these systems and getting a handle on "What is a capability? What are the different types of computational structure a model can take?" Because one of the things that I really love about computational mechanics is it gives you a handle on "what does it even mean to talk about structure in these systems?" Structure in training data, structure in model internals, structure in behavior.

So for instance - and this is probably coming from my neuroscience background a little - but what do we do in neuroscience when we want to study some cognitive capacity like abstraction or transfer learning or generalization? Well, we want to run our experiments on model organisms like rats and stuff like that. So what we try to do is we try to find the simplest toy example of abstraction, something that has the flavor of abstraction in its minimal form that we can train a rat to do.

Then we can go into the rat's brain while it's doing this abstraction task and we can do the neuroscience equivalent of mechanistic interpretability on it. So that was my life for a decade. If we can find a form of an abstraction task in the form of these HMMs - and I think I have an idea for one - now we can say... For instance, let's say you have two generative structures next to each other, and they have the same hidden structure except they just differ in the vocabularies they speak in.

So let's say one speaks in As and Bs and one speaks in Xs and Ys. Now, when we look at these as humans, it's very obvious that they have the same hidden structure. They have the same abstract structure, but they speak in two different vocabularies. What would it mean for a transformer to understand that these two processes have the same abstract structure?

So if you train a transformer on this set of tasks, one thing it could do is learn them both optimally, but not understand that they have the same structure. Another thing it can do is learn them both optimally, but understand they have the same structure. Behaviorally, you could figure out the difference between those two cases if you test them on a held out prediction task, which is combining histories of As, Bs, Xs and Ys together, just holding the abstract structure constant.

And if they can still do the optimal prediction task now, combining past histories of the two different vocabularies that they've never experienced together before, then you can say they've abstracted. In addition, because we have a theory for understanding how the model internals relate to any kind of behavior, we can make a prediction of what would underlie that abstract structure. For each of the processes, for each of the generative structures, there should be a simplex geometry that's represented in the residual stream of the transformer. And the prediction would be that if the network is able to abstract and understand this abstract structure, then these simplex structures will align in space. Whereas if not, they might lie in orthogonal subspaces.

And now all of a sudden you start to think of the relationship between compression: if the residual stream doesn't have enough space to represent them in orthogonal subspaces, maybe it has to put them in overlapping subspaces, and that you start to realize that there might be a story in general. Like we were talking about before, in the real case, we don't have enough space to represent the full structure, so you have to do some compression. And because of that, you might have to... The network is forced to take what should be separate simplex structures and kind of overlap them. And that might be the thing that gives rise to out-of-distribution generalization and abstraction and stuff like that. So that's another thing that I'm quite excited about.

## How Adam and Paul found computational mechanics <a name="how-adam-and-paul-found-comp-mech"></a>

**Daniel Filan:**
Sure. I'd like to move on a bit to the story of how you guys got into this, if that's okay?

**Paul Riechers:**
Sure.

**Daniel Filan:**
Maybe I'll start with Adam. So my understanding is that you have a neuroscience background. Based on what I know about neuroscience, it's not predominantly computational mechanics, and it's also not predominantly saving the world from AI doom. How did that happen for you?

**Adam Shai:**
Just last year even, I was towards the end of a postdoc in neuroscience and experimental neuroscience. Like I was saying before, I was training rats on tasks and I was studying the role of expectations and predictions and sensory processing in the visual cortex of these rats. Actually, I remember running the experiments on these rats, and it was like a week after ChatGPT came out and I was talking to ChatGPT and just really shocked (I think is the right word) by its behavior. And just had a moment of... everything... All of my beliefs and intuitions for what I thought must underlie intelligent behavior were just proven to be wrong. I guess, I had all these intuitions that came from my neuroscience background about recurrence and complicated dendritic structures and all kinds of details, and I was like, "There's some secret sauce in neuroscience that has to underlie intelligent behavior." Meanwhile, I'm studying these rats on these very simple tasks that are basically not much more than left, right. That's simplifying it, I'm straw-manning it a little bit, but they're not super complicated linguistic tasks that humans perform.

ChatGPT... after learning about the transformer structure and being like, "these things are feedforward. They're weirdly linear. They're not totally linear, obviously, and that's important, but they're much more linear in structure..." Basically to whittle it down as I was like, "The architecture is not interesting enough to give rise to these interesting dynamics, given what I think about the relationship between mechanism and intelligent behavior." That was a shocking thing. And then, also realizing that the existence of the system pushed more on my intuitions about intelligence than basically any paper I had read in neuroscience in a while.

It was actually emotionally a little difficult, I have to say. But, yeah, that point is when I started thinking about that transition. I'm like, "If my intuitions are wrong about this, then what that means for the future..." GPT-4 came out not long after that, and that was another like, "Whoa. Okay. Apparently, it wasn't just this one thing. Apparently, it can get better faster."

So, yeah. That's when I started going... And there's always some tension here when you realize the safety implications for the future. And then, on the other hand, it's also just actually interesting, literally just a very interesting... just intellectually, it's super cool. And there's this tension between those two things that I feel quite often. I'm pretty sure I'm not the only one in the AI safety community that feels that.

But even from my academic interests, I'm interested in intelligence. I'm interested in the mechanisms that underlie intelligence. I'm interested in the way that networks of interacting nodes can give rise to interesting behavior. So I guess that's the way that I found myself in AI safety. I guess the other relevant thing is, for a long time, eight years ago or something, I had randomly run into comp. mech. as a neuroscientist, just reading one of the papers. It just struck me - not that I understood it at all; I did not have, and I still don't really have, the mathematical acumen to grok all of its complexities - but something about the framework really captured exactly what I was interested in getting at in neuroscience, which had to do [with] "what is the relationship between a dynamical process and the computation it performs?"

And so for a while, I would annoy all my neuroscience lab mates and stuff about comp. mech., and I probably sounded like a crazy person, but I was never able to really figure out how to apply it in any useful way. So I'm super excited to start working on these neural networks and applying it, actually getting somewhere with it, it's an amazing feeling. I'm very lucky to have met Paul and the other comp. mech. folks that are helping me with this.

**Daniel Filan:**
[Paul,] first of all, how did you get interested in computational mechanics to begin with?

**Paul Riechers:**
That goes back a long time. I don't know if I can remember. I have a background in physics. I've done theoretical physics. I also did a master's in electrical and computer engineering, but then was back to theoretical physics for a PhD. I was interested in emergent behavior, complexity, chaos, all this stuff. Who isn't? But I think early on in (let's say) 2005, I had [Jim Crutchfield](https://csc.ucdavis.edu/~chaos/) as an undergrad teacher at UC Davis, and I just thought the stuff he did was super cool.

**Daniel Filan:**
And he does computational mechanics?

**Paul Riechers:**
Yeah. He's been the ringleader. It's been Jim and colleagues building up computational mechanics. So I guess from that point, I realized what Jim did. I had some idea of computational mechanics, and I was going on a trip with my buddy. My buddy, Seth, is like, "Well, what would you do if you could do anything?" I was like, "Oh, well, I'd probably do this computational mechanics stuff, but I don't know if I'm smart enough" or something. But then it turns out that if you believe in yourself a little bit, you can do lots of things that you're excited about.

So, I ended up, long story short, doing a PhD in Jim's group. So I did a lot of computational mechanics during my PhD and have done a bit afterwards. But in physics, I've worked on all sorts of things, it's been a big mixed bag of quantum information, ultimate limits of prediction and learning, which is where the comp. mech. comes in, non-equilibrium thermodynamics, but trying to understand just what is reality like? And so my interest in comp. mech. was because it was very generally, "what are the limits of prediction and learning?" That seemed fascinating to me. Maybe you have this follow-up question of, "How did I get into some AI safety stuff?" Right?

**Daniel Filan:**
Truly a master of sequence prediction.

**Paul Riechers:**
Yeah. I felt very unsettled a year or more ago with what I was seeing in AI, like generative images, ChatGPT, this stuff. I was very unsettled by it, but I also didn't think I had actually anything to contribute. Actually, when we were talking in Jim's group about neural networks, even back in 2016 or something, it was just like, "This stuff is so unprincipled. They're just cramming data into this architecture. They don't know what they're doing. This is ridiculous." It was always just like, "Let's do the principled thing. Let's do comp. mech."

I think somehow, this was just in my mind: neural nets are the unprincipled thing, and I do the principled thing, but now these unprincipled things are becoming powerful: well, [redacted], what do we do? And it was out of the blue that this guy, [Alexander Gietelink Oldenziel](https://sites.google.com/view/afdago/home) reached out to me and he's like, "Oh, comp. mech. is the answer to AI safety." And I was like, "Well, no. I know comp. mech., you don't, you sound like a crazy person", but I already was concerned with AI safety, so I'm like, "OK, I'll hear you out."

And so I explained to him why it wasn't going to be useful: for example, that these architectures were, as far as I understood them, feed-forward, computational mechanics was about dynamics. But we kept talking and actually he did convince me that there's something about thinking about the dynamics through context which is really important. And I don't think Alexander quite made the case for how they're important but somehow I realized, "oh, this will be important".

And actually at that point I started talking to Adam, this was almost a year ago I guess, where Adam then started helping me to understand the details of transformer architectures and this stuff. And we were talking with [Sarah Marzen](https://www.sarahmarzen.com/) and others, and started to realize "OK, actually this will help us to understand model internals that people just aren't thinking about, it's going to help us to understand behaviours people just aren't thinking about".

And so then when I realized that "oh, there's actually something here", it felt both research-wise super exciting, but also a moral imperative: I care about this, I need to do something about it. So I started just reaching out to people at Google, whatever connections I had, and be like "Hey, I feel like I have something really important to contribute here, how should I go about this?

And the broad community of interpretability, in both industry and outside, people were very helpful for directing me towards, "hey, there's this thing [MATS](https://www.matsprogram.org/), which I don't know, maybe it's too junior for you..." but actually for me, it was great. So I was a MATS scholar beginning of this year. For me, it was great just being surrounded by people that really understood the ML side, because that hasn't been my strength, but now I've grokked a lot of that. And so I'm really grateful that there has been a community that just cares about this stuff. And so many people - like with [PIBBSS](https://pibbss.ai/), Nora [Ammann] and everyone, so many people have just been asking, "How can we help you?" And that's been really cool.

So Adam and I have been talking for a while. We've been working on this stuff, and I think earlier in the year we were saying, "This stuff's going really well. It seems like we have a lot to contribute. At some point, we should maybe do an organization on this, a research organization." And so we were open to that idea, and then things fell in place where it was like, "Oh, I guess we can do that now."

**Adam Shai:**
Yeah. I'll second the thing that Paul said about how supportive the broad AI safety community has been. For the last six months, I've been a PIBBSS affiliate, and that's another AI safety organization. And they've been really helpful to both of us, I think, supporting us in starting Simplex and the research and everything.

**Daniel Filan:**
Sure. I think I understand your individual journeys. One thing that I still don't quite understand is: how did you guys meet and sync up?

**Paul Riechers:**
It's [this Alexander guy](https://sites.google.com/view/afdago/home) again?

**Adam Shai:**
Yeah, it is Alexander.

**Daniel Filan:**
This is [the second episode](https://axrp.net/episode/2024/05/07/episode-31-singular-learning-theory-dan-murfet.html) where Alex cold-emailing people has played a crucial role in someone's story.

**Paul Riechers:**
He's some wizard in the background.

**Adam Shai:**
Well, I met Alexander... it's probably almost two years now ago at some rationality event thing that I actually wasn't even invited to, I was just tagging along, I think. We started talking about rats and minimum description length things, and I was like, "Oh, that sounds like a less principled version of comp. mech.," and I started ranting about comp. mech., and then me and Alexander kept on talking about that and trying to think if there was a way to apply it to neural networks. I had exactly the same reaction [to Paul], I'm like, "I don't know. These aren't recurrent, so I don't really know how they would be relevant to transformers."

And then I think a little while after that, Alexander reached out to Paul, and I was having a very frustrating day with the rats, I think. And I was like, "Ah, I really want to do comp. mech.." I messaged Sarah Marzen and we had a meeting. It was really great. And then eventually, all four of us got together and started trying to think of ways to apply comp. mech. to neural networks.

**Paul Riechers:**
So me and Adam were still both in academia. I was actually about to take on something that would've led to a tenured role at a university in Spain. We were looking forward to sangria, all this stuff. But we were starting to do this research on the side, and this thing, it was starting to go well. And I realized "Oh, okay, I need to dedicate myself to this."

So I think for me, the first engagement of "let's apply comp. mech. to neural nets" has been in collaboration with Adam. And so it's been really a great, I think, complementary set of skillsets coming together. And I think we have a lot of shared priorities and vision.

**Adam Shai:**
It's been super fun.

**Paul Riechers:**
Yeah.

## Computational mechanics for AI safety <a name="comp-mech-for-ai-safety"></a>

**Daniel Filan:**
Awesome. So I have a question about the safety application. So my understanding is that basically the story is: have a principled understanding of what neural nets might be doing, get some tools to have a more principled way of saying what's going on there, and hopefully just use that to feed into making them safe, designing them the way we want them to be. Is that right? Or is there a different plan you have?

**Paul Riechers:**
Yeah, I mean, that's an aspect of it, for sure. But I think for me, another important aspect is enabling better decision-making. People talk a lot about "what's your P(doom)?" or probability of this or that. I feel like we understand these things so little. We don't even know what the support is, probability over what? We don't know what's going to happen. We don't know the mechanisms, everything. We don't even have a shared language. There isn't science being done the way that I think it needs to be and at the scale that it needs to be.

So I think computational mechanics can - maybe optimistically - help to figure out what are the things that we're most confused about? How do we go about that? And build up some infrastructure. And some of that'll be at a low level, some of that'll be at a high level, and probably going back and forth between those.

So I feel like understanding is very important in general to make good decisions. One [reason] is, if you understand well, then you could have new interventions. So that's one thing. And then the other [reason] is you might understand well that this will go very poorly in any case, because now we have a more mechanistic understanding of why or how things would go good or bad. And that's this other part that I think's really important.

**Adam Shai:**
A few other things that are related to that. So just to take a very concrete example: why don't we have rigorous benchmarks for SAEs? Why don't we have a data set, or transformer, or a combination of both, where we know exactly what the correct features are so we can tell whether or not a specific form of SAE, with specific architecture and losses and stuff, whether it works better or worse than other things?

And I think it's because we don't have an understanding for a lot of the basic concepts with which we talk about AI-safety-relevant things like features (in this particular case, it's features.) And the hope is, if we can get a better understanding, that we can even do things like build benchmarks: a pretty simple thing. I think this also carries over to things like evals, where if we had a better understanding of what fundamentally a capability was - what are the different types of capabilities? - we could hopefully also build better evals, and we can relate any particular eval that exists right now to things going on inside of the transformer.

This also relates to things like out-of-distribution behavior. If we can relate the behavior in general of some internal structure in a transformer to all of the things it can do as it spits out tokens at the end, then we will know the space of out-of-distribution behavior. And nothing comes for free. It's an enormous amount of work, and it's not a simple thing to go from this theoretical understanding we have right now to the application to the largest models that exist. That's not the statement, but at least we have some footing here. So that's, I think, kind of the idea.

**Paul Riechers:**
And just one more thing I want to add on this is there's a lot of power in comp. mech. in that the theory is model-agnostic. I mean, most of the results aren't actually about neural nets - I was surprised it applied at all. And it's not specific to transformer architectures, whereas maybe some mechanistic interpretability stuff would be architecture-dependent. So I think that's powerful: for any particular architecture, you can then back up the implications. But as architectures change, this learning should be able to change, be adapted with us. So I do think that's important.

**Daniel Filan:**
Yeah, so that's one story about how computational mechanics could be useful. One obvious way to help push this forward is work on computational mechanics, join Simplex or look at their list of projects and work on them. I wonder if there are any synergies with other approaches in AI safety that you think work really well with comp. mech., that are good complements to it?

**Adam Shai:**
These connections haven't been made formally. I think there's an obvious connection to mech. interp. in general, which is kind of a big field. But for any kind of mechanistic understanding one has from the standard toolset of mech. interp. - so these are things like looking at attention head patterns, causal interventions of different forms - one can ask now how these relate to the task of carrying out the mixed-state presentation.

I'm hoping that there are synergies with a more [dev. interp.](https://devinterp.com/) point of view. I think there's already a bunch of pieces of evidence that [suggest] that that will work out. We can see this kind of belief state geometry forming over training. Paul has theoretical work talking about the learning process of a parameterized model from the point of view of comp. mech..

So hopefully we can also connect this thing to the training process itself, which is kind of a dev interp, maybe SLT-flavored way of looking at things. I mentioned evals before... how many different AI safety approaches are there?

**Daniel Filan:**
Unclear to me. So if I'm thinking about how computational mechanics does apply to safety, does help things being safe: we've talked a bit about the theory, and it seems like it's mostly a theory of doing good inference about stochastically-generated processes that... I'm imagining [that] someone else is generating this process, I'm inferring it, and then, computational mechanics is saying, "Okay, what's going on there? What's going on in my head?"

But if I think about AI, one of the crucial, important features of it is that it involves things acting in the world and changing the world. Not just inference, but this whole loop of inference and action. And I'm wondering, are there developed computational mechanics ways of talking about this? Or is this an open area? I think Adam mentioned, I forgot what word it was, but there was something about computational mechanics with two systems that were maybe influencing each other? Is that enough? Do we need more?

**Paul Riechers:**
Yeah, I guess I'd say there's some basically preliminary work in computational mechanics about not just what it's like to predict or to generate, but really putting this together in an agentic framework. Instead of epsilon-machines, there's [epsilon-transducers](https://arxiv.org/abs/1412.2690), which is: what's the minimal structure and memoryful structure for an input/output device?

So that, I think, is really relevant work. Also, we can probably look at model internals and things like that. But I think there's a totally different level at which computational mechanics can be applied that we're excited for, but just don't have the bandwidth for right now, in terms of what to expect with interacting agents at a high level. There's a lot of ways to go about this. There's theory work that's been done with POMDPs, but I think that there is [work](https://www.nature.com/articles/s41534-016-0001-3) that has been done in terms of basically these memoryful input/output agents. There's a lot to do that I indeed feel that's important for understanding, if there's any hope for understanding what the emergent behaviors of interacting agents would be. Sounds hard. I'm curious about that. Things need to be developed a lot more.

**Adam Shai:**
Yeah. Just to add onto that, some of the conceptual things that would be exciting to try to get a handle on, would be things like, "what does it mean for an agent to understand some structure in the world? What does it mean in terms of its internal states?" And then going a level beyond that, "What does it mean for an agent to have a model of itself, inside of itself, and what is that computational structure that subserves that?" So I think these are pretty fundamental questions that the work hasn't been done [for] yet, but one could kind of imagine using and extending the framework of transducers to get at that, which would be super exciting.

## Following Adam and Paul's research <a name="following-adam-and-pauls-research"></a>

**Daniel Filan:**
Listeners have now heard the exciting promise of computational mechanics, cool things they can do, cool things that work with it, cool potential threads of using it to understand agency. If people want to follow your guys' research, your writings, how should they go about doing that?

**Adam Shai:**
I think the main way is [simplexaisafety.com](https://www.simplexaisafety.com/), our website. You can contact us. We're very excited to collaborate and work with other people. So if anyone is interested, they should certainly feel free to contact us through there.

**Paul Riechers:**
Yeah, for sure. I'd say that people that feel like there's a natural connection, if you're feeling inspired to make our mission go well, or you see a natural collaboration, please feel free to reach out, because we feel like this work's really important and we're just a bit bottlenecked. So I think working with good people and growing a vision together would be really nice.

**Daniel Filan:**
Great. Well, thanks very much for coming here today. It was great talking.

**Paul Riechers:**
Yeah. Thanks so much.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. Filming occurred at [FAR Labs](https://far.ai/labs/). Financial support for this episode was provided by the [Long-Term Future Fund](https://funds.effectivealtruism.org/funds/far-future), along with [patrons](https://patreon.com/axrpodcast) such as Alexey Malafeev. I'd also like to thank Joseph Miller for helping me set up the audio equipment I used to record this episode. To read a transcript of this episode, or to learn how to support the podcast yourself, you can visit [axrp.net](https://axrp.net). Finally, if you have any feedback about this podcast, you can email me, at <feedback@axrp.net>.
