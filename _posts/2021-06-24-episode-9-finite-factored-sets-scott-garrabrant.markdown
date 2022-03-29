---
layout: post
title: "9 - Finite Factored Sets with Scott Garrabrant"
date: 2021-06-24 15:00 -0700
categories: episode
---

[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/NjhiNGU4ZGQtODRlZi00NmZmLWI1ZDMtN2ZiNDJlMzRjZmY2)

Being an agent can get loopy quickly. For instance, imagine that we're playing chess and I'm trying to decide what move to make. Your next move influences the outcome of the game, and my guess of that influences my move, which influences your next move, which influences the outcome of the game. How can we model these dependencies in a general way, without baking in primitive notions of 'belief' or 'agency'? Today, I talk with Scott Garrabrant about his recent work on finite factored sets that aims to answer this question.

Topics we discuss:
- [Finite factored sets' relation to Pearlian causality and abstraction](#pearl-diff)
- [Partitions and factors in finite factored sets](#partitions-factors)
- [Orthogonality and time in finite factored sets](#orthogonality-time)
- [Using finite factored sets](#using-ffs)
- [Why not infinite factored sets?](#why-finite)
- [Limits of, and follow-up work on, finite factored sets](#ffs-limits)
- [Relevance to embedded agency and x-risk](#embedded-agency-x-risk-relevance)
- [How Scott researches](#what-is-it-like-to-be-a-scott)
- [Relation to Cartesian frames](#cartesian-frames-relation)
- [How to follow Scott's work](#following-scotts-work)

**Daniel Filan:**
Before we begin, a note about this episode. More than other episodes, it assumes knowledge of the subject matter during the conversation. So, although we'll repeat some basic definitions, before listening there's a good chance that you'll want to watch or read something explaining the mathematics of finite factored sets. The description of this episode will contain links to resources that I think do this well. Now, on to the interview.

**Daniel Filan:**
Hello everybody. Today, I'll be speaking with Scott Garrabrant. Scott is a researcher at the [Machine Intelligence Research Institute](https://intelligence.org/) or MIRI for short. And prior to that, he earned his PhD in Mathematics at UCLA studying combinatorics. Today, we'll be talking about his work on [finite factored sets](https://www.alignmentforum.org/posts/N5Jm6Nj4HkNKySA5Z/finite-factored-sets). For links to items that we'll be discussing, you can check the description of this episode, and you can read a transcript at [axrp.net](https://axrp.net/).

## Relation to Pearlian causality and abstraction <a name="pearl-diff"></a>

**Daniel Filan:**
Scott, welcome to AXRP. I guess we're going to start off talking about the finite factored sets work, but to start off that starting off, you've kind of compared this to... Or I think it's sort of meant to be somehow in the same vein of Judea Pearl's work on causality, where you have this [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) of nodes and arrows. And the nodes are things that might happen and the arrows are one thing causing another kind of. So I'm wondering, what's good about Pearlian causality? Why does it deserve to be developed on. Let's just start with that. What's good about Pearlian causality?

**Scott Garrabrant:**
What's good about Pearlian causality. So specifically I want to draw attention to the fact that I'm talking about kind of earlier Pearlian stuff, like Pearl has a bunch of stuff. I'm talking about the stuff that you'll find in chapter two of the book [Causality](http://bayes.cs.ucla.edu/BOOK-2K/) specifically. I mean, so basically it's a framework that allows you to take statistical data and from it infer temporal structure on the variables that you have. Which is just really useful for a lot of concepts, there are a lot of purposes. It's a framework that allows you to kind of just go from pure probabilities to having an actual structure as to what's going on and causality, which then lets you answer some questions about interventions possibly and things like that.

**Daniel Filan:**
So if it's so great, why do we need to do any more work? Why can't we just all read this book and go home?

**Scott Garrabrant:**
So my main issue is kind of a failure to work well with abstraction. And so we have these situations, possibly coming from decision theory, where we want to model agents that are making some choice and they have some effect on the world. And it makes sense to model these kinds of things with causality, which is not directly using Pearlian causal inference, but using just kind of the general framework of causality, where we kind of draw these directed acyclic graphs with arrows, that kind of represent effects that are happening.

**Scott Garrabrant:**
And you run into these problems where if you have an agent and, for example, it's being simulated by another agent, then there's this desire to put multiple different copies of the same structure in multiple different places in your causal story. And to me it feels like this is really pointing towards needing the ability to have some of your nodes, some of your variables in your causal diagrams be abstract copies of other nodes and variables. And there's an issue, the Pearlian paradigm doesn't really work well with being able to have some of your variables be abstract copies of others.

**Daniel Filan:**
So what's an example of a place where you'd want to have like multiple copies of the same structure in different places. If you could spell that out.

**Scott Garrabrant:**
Yeah, I can give more specific direct examples that kind of are made up examples, but really I want to claim that you can see a little bit of this any time you have any agent that's making a decision. Agents will make decisions based on their model of the world. And then they'll make that decision - based on their model of the consequences of their actions, and they'll make a decision and they'll take an action. And then those consequences of their action will actually take place in the real world. And so you can kind of see that there's the agent's model of what will happen that is kind of causing the agent's choice, which kind of causes what actually happens. And there's this weird relationship between the agent's model of what will happen and what actually happens that could be well-described as the agent's model is kind of an abstract version of the actual future.

**Daniel Filan:**
Okay. And why can't we have abstractions in Pearlian causality?

**Scott Garrabrant:**
So the problem, I think, lies with what happens when you have some variables that are kind of deterministic functions of others. And so if you have an abstract refined version - if you have a refined version of a variable and you have another coarse abstract version of the same variable, you can kind of view the coarse version that has less detail as a deterministic function of the more refined variable. And then Pearl has some stuff that allows for determinism in the structure. But the part that I really like doesn't really have a space for having some of your variables be deterministically or partially deterministically related. And in the parts of Pearl where some of your variables can be deterministically related, the ability to do inference is much worse. The ability to infer causality from the statistical data.

**Daniel Filan:**
Yeah. I guess to what degree are we dealing with strict determinism here? Because guesses can sometimes be wrong, right? If I'm thinking about the necessity of abstraction here as "I have models about things", it's not really the case that my model is a deterministic function of reality, right?

**Scott Garrabrant:**
Yeah. This is right. I mean, there's a story where you kind of can have some real deterministic functions where you have multiple copies of the same algorithm in different places in space time or something but... Yeah, I feel I'm kind of dancing around my true crux with the Pearlian paradigm. And I don't know, I'm not very good at actually pointing at this thing. But my true crux feels something like variable non-realism, where in the Pearlian world, we have all these different variables and you have a structure that kind of looks like this variable listens to this variable. And then this variable listens to this other variable to kind of determine what happens. And in my world, I'm kind of saying that...

**Scott Garrabrant:**
I'm kind of in an ontology in which there's nothing real about the variables beyond their information content. And so if you had two variables, one of which is a copy of the other, it wouldn't really make sense to talk about like... Yeah, if X and X' were copies of each other, it wouldn't really make sense to ask is Y looking at X or looking at X' if they're actually the same information content. And so kind of philosophically, I think that the biggest difference between my framework and Pearl's framework is something about denying the realism of the variables. Yeah, that didn't really answer your question about, do we really get determinism? I mean, I think that systems that have a lot of determinism are useful for models. We have systems that don't have real determinism, but we also don't actually analyze our systems in full detail.

**Scott Garrabrant:**
And so I can have a high level model of the situation. And while this calculator is not actually deterministically related to this other calculator, relative to my high level model it kind of is. And so even if we don't get real determinism in the real world, in high-level models it feels still useful to be able to work okay with determinism. But I don't know, I think that focusing... I don't know, in some sense, I want to say determinism is kind of the real crux, but in another sense I want to say that it's distracting from the real crux. I'm not really sure.

**Daniel Filan:**
It also seems like one issue - let's say I'm playing Go. And I'm thinking about what's going to happen when I make some move. And then I play the move and then that thing happens. Well, if we say that my model of what's happening is an abstraction of what actually happens, it's a function of what actually happens, then it's the case that there's sort of an arrow from what actually happens to what I think is going happen. And then there's an arrow from that to what I do, because that's what caused me to make the decision and then there's an arrow from what I do to what actually happens because that's how normal things work. But then you have a loop which you're sort of not allowed to have in Pearl's framework. So that seems like kind of a problem.

**Scott Garrabrant:**
Yeah. I think that largely my general, or a large part of my research motivation for the last, I'm not sure how long, I think at least three years has been a lot towards trying to fix this problem where you have a loop. Thinking about decision theory in ways that people were talking about decision theory in, I don't know, around 2016 or something. There was stuff that involved, well, what happens when you take DAGs [directed acyclic graphs], but you have loops and stuff like this. And I had this glimmer of hope around, well, maybe we can not have loops, when I realized that in a lot of stories like this, the loop is kind of caused by wanting to have... by conflating a variable with an abstract copy of the same variable. And that's not what you did. What you did was you drew an arrow from the thing that actually happens to your model.

**Scott Garrabrant:**
And I think that... So in my framework there aren't going to be arrows, but to the extent that there are arrows like this, it makes more sense to draw the arrow from the coarse model to what actually happens. And to the extent that the coarse model is like a noisy approximation of what actually happens, you kind of won't actually get an arrow there or something. But in my world, the coarser descriptions of what's going on will kind of necessarily be no later than a more refined picture of the same thing. And so I think that I more want to say, you don't want to draw that same kind of arrow between the real world and the model. And kind of, if I really wanted to do all this with graphs, I would say you should at least draw an undirected arrow.

**Scott Garrabrant:**
That's kind of representing their logical entanglement. That's not really causal or something like that. That's not the approach that I take. The approach that I take is to kind of throw all the graphs away. But yeah, so I largely gained some hope that these weird situations that felt like they were happening all over the place and setting up decision theory problems and agents that trust themselves and think about themselves and do all sorts of reasoning about themselves. I gained some hope that all of the weird loopy stuff that's going on there might be able to be made less loopy via somehow combining temporal reasoning with abstraction. And that's largely a good description of a lot of what I've been working on for many years.

**Daniel Filan:**
All right. I guess if I think concretely about this coarse versus abstract thing. Imagine I'm like... So basically think about the Newcomb scenario. So Newcomb's problem is, there's this really smart agent called Omega. And Omega is simulating me, Daniel Filan. And Omega gives me the choice to just take one box that has an unknown amount of money to me in it, unknown to me. And two boxes where I take the unknown amount of money and also a box that contains $1,000. And I can just see that it definitely has $1,000. And Omega is a weird type of agent that says, "I figured out what you would do. And if you would've taken one box, I put a lot of money in the one box, but if you would have taken both then I put almost none in it."

**Daniel Filan:**
And then I take one or the other. So what should I think of... It seems like in your story, there's an abstract variable, maybe in a non-realistic kind of way, because we're variable non-realists, but there's some kind of abstract variable that's Omega's prediction of what I do, and then there's what I actually do. And if it's the case that Omega just correctly guesses, always correctly predicts whether I'm going to one box or two box. What should I think of... In what way is this abstraction lossy? What extra information is there when I actually take the one or the two?

**Scott Garrabrant:**
So I guess if Omega is just completely predicting you correctly. Then I don't want to say that... I kind of want to say, well, there's a variable-ish thing that is what you do. And it goes into Omega filling the boxes. And it also goes into you choosing the boxes. And it's kind of at one point in logical time or something like that. I think that the need for actual abstraction can be seen more in a situation where you, Daniel, can also partially simulate Omega and learn some facts about what Omega is going to predict about you. So maybe... Yeah, Omega is doing some stuff to predict you and you're also simulating Omega. And in your simulation of Omega, you can see predictions about stuff that you're currently doing or about to do and stuff like that.

**Scott Garrabrant:**
And then there's this weird thing where now you have these weird loops between your action and your action. So in the situation where Omega was kind of opaque to you, the intuition goes against what we normally think of as time, but we didn't necessarily get loops, but in the intuition where you're able to kind of see Omega's prediction of you, things are kind of necessarily lossy because you could kind of diagonalize against predictions of you and kind of because it doesn't fit inside you. So, let's see. If you're looking at a prediction of yourself and you have some program trace, which is your computation, and you're working with an object that is a prediction of yourself and maybe it's a very good prediction of yourself. You're not actually going to be able to fully simulate every little part of the program trace because it's kind of contained inside your program trace.

**Scott Garrabrant:**
And so there's a sense in which I want to say, there's some abstract facts that may be our predictions or proofs about what Daniel will do, and that can kind of live inside Daniel's computation. And then Daniel's actual program trace, is this more refined picture of the same thing. And so, I think the need for actually having different levels of abstraction that are at different times is coming more from situations that are kind of actually loopy as opposed to in the Newcomb problem you described. The only reason that it feels loopy is because we have this logical time, then we also have physical time and they seem to go in different directions.

## Partitions and factors <a name="partitions-factors"></a>

**Daniel Filan:**
So now we've gotten a bit into the motivation. What is a finite factored set?

**Scott Garrabrant:**
Okay. So there's this thing - I don't know, I guess I first want to recall the definition of a partition of a set. So a partition of a set is a set of subsets of that set. So we'll start with an original set S and a partition of S is a set of subsets of S, such that each of the sets is non-empty and the sets are pairwise disjoint, so they don't have any common intersection. And when you union all the sets together, you get your original set S. So it's kind of a way to take your set and view it as a disjoint union.

**Daniel Filan:**
Yeah. I kind of think of it as like dividing up a set into parts and that way of dividing it up is a partition.

**Scott Garrabrant:**
Yeah. And so I introduced this concept called a factorization, which can be thought of as a multiplicative version of a partition where you kind of... In the partition story, you put the sets next to each other and you union them together to get the whole thing. And in a factorization, I instead want to kind of multiply your different sets together. And so the way I define a factorization of a set S is it's a set of non-trivial partitions of S such that for each way of choosing a single part from each of these partitions, there will be a unique element of S that's in the intersection of those parts. And so the same way that you can view partition as a disjoint union, you can view a factorization as a... or sorry, a partition is a way to view S as a disjoint union, a factorization of S is a way to view S as a product.

**Daniel Filan:**
Okay. And so to make that concrete, an example that I like to have in my head is suppose we have points on a 2D plane, and we imagine the points have an X coordinate and a Y coordinate. So one partition of the plane is I can divide the plane up into sets where the X coordinate is zero, sets where the X coordinate is one, sets where the X coordinate is two. And those look like lines that are perpendicular to the X axis. And none of those lines intersect. And every point has some X coordinate.

**Daniel Filan:**
So it's this set of lines that together cover the plane, that's one partitioning, the X partitioning. And there's another one for values of Y, right? Which look like horizontal lines that are various amounts up or down. And so once you have the X partitioning and the Y partitioning, any point on the plane can be uniquely identified by which part of the X partitioning it is and which part of the Y partitioning it is because it just tells you how much to the right of the origin are you, how much above the origin are you, that just picks out a single point. I'm wondering, do you think that's a good kind of intuition to have.

**Scott Garrabrant:**
Yeah, I think that's a great example. To say a little bit more there. So your original set S in that example you just gave is going to be the entire Cartesian plane, the set of all ordered pairs, like (X coordinate, Y coordinate). And then your factorization is going to be a set B, which is going to have just two elements. And the two elements are the partition according to what is the Y coordinate, and the partition according to what is the X coordinate. You can kind of view partitions as questions. And so in general, if I have a set like the Cartesian plane, and I want to specify a partition, one quick way to do that is I can just ask a question. I can say, what is the X coordinate? And that question kind of corresponds to the partition that breaks things up according to their X coordinate.

**Daniel Filan:**
Okay. And the one slightly misleading thing about that example is that there are an infinite number of points in the X, Y plane. But of course, we're talking about finite factored sets. So S only has a finite number of points.

**Scott Garrabrant:**
Yeah. So we're talking about finite factored sets. So in general, I'll want to work with a pair (S, B). Where S is a set, a finite set, and B is a factorization of S.

**Daniel Filan:**
Why choose the letter B, for a factorization?

**Scott Garrabrant:**
It's for basis.

**Daniel Filan:**
Mmm.

**Scott Garrabrant:**
Yeah. It's for basis. Largely, while I'm thinking of the elements of B as partitions of S, I'm also kind of just thinking of them as elements, just out on their own that are kind of representing the different basis elements. Yeah, so it actually looks a lot like a basis, because for any point in S you can kind of uniquely specify it by specifying its value on each of the basis partitions.

**Daniel Filan:**
Okay. So yeah, this gets into a question I have which is, how should I think about the factors here? Like these finite factored sets, I guess they're supposed to represent what's going on in the world. How should I think about factors in general or partitions, I can think about as questions. These factors in B, how should I think about those?

**Scott Garrabrant:**
Yeah, I think I didn't explicitly say, but the elements of B, we'll call factors. We use the word factor for the elements of B. So I almost want to say the factors are kind of a preferred basis for... Yeah, the factors kind of form a preferred basis for your set of possibilities. And so if I consider the set of all bit strings of length five, right. There are 32 elements there. And if I wanted to specify an element, I can do so in lots of different ways. But there's something intuitive about this choice of breaking my 32 elements into what's the first bit, what's the second bit, what's the third bit, what's the fourth bit, what's the fifth bit. So it's kind of... It's a set of questions that I can ask about my element, that uniquely specify what element it is.

**Scott Garrabrant:**
And also for any way of answering the questions, there's going to be some element that works with those answers. And so factorization is kind of just like, it's a combinatorial thing that could be used for many different things. But one way to think about it is kind of, you're making a choice that is a preferred basis, a preferred set of variables to break things up into and you're thinking of those as kind of primitive and then other things as built up from that. So properties like do the first two bits match is then thought of as built up from what is the first bit and what is the second bit. And so it's kind of a choice of what comes first, a choice of what are the primitive variables in your structure.

**Daniel Filan:**
Okay. So if I'm trying to think about maybe some kind of decision problem where I'm going to do something, and then you're going to do something and then another thing's going to happen. And I want to model that whole situation with a finite factored set. Instead of thinking about modeling which thing is going to happen to me today, if I want to model an evolving situation, how should I think about what the set S is and what the factors should be?

**Scott Garrabrant:**
Yeah. So factorization is very general and actually, I use the word factorization in multiple different contexts when talking about this kind of thing. But in the question that you're asking, to say some more background stuff. So I'm going to introduce this theory of time that has a background structure that looks like a factored set rather than a background structure that looks like a DAG, looks like a directed acyclic graph, like in the Pearlian case. And so specifically if I'm using a factored set and I am using that to interpret... I'm using that to kind of describe some causal situation, I am not going to have nodes. I'm not going to have factors that kind of correspond to the nodes that you would have in the Pearlian world. Instead, I am mostly going to be thinking of the factors as independent sources of randomness.

**Scott Garrabrant:**
I'm hesitant there, because a lot of my favorite parts of the framework aren't really about probability. But if I'm thinking about it in a temporal inference setting where I'm like getting the statistical distribution of... Where I'm getting a statistical distribution, then I'm thinking of the factors as basically independent sources of randomness. And so if we have some variable X, and then we have some later variable Y that can take on some values and partially is going to be a function of X, then we won't have a factor for X and a factor for Y, we'll have a factor for maybe that which kind of goes into what X is. And then we'll have another factor that's like the extra randomness that went into the computation of Y once you already knew X. And when we think of factored sets as related to probability, we're actually going to always want our factors to be independent. And so the factors can't really be put on things like X and Y when there's a causal relationship between X and Y.

**Daniel Filan:**
So it almost sounds like the factors are supposed to be somehow initial data, like the problem set-up or something, the specification of what's going on, but kind of an initial specification?

**Scott Garrabrant:**
Yeah, indeed, if you take my theory of time. So I have way of taking a factored set and taking an arbitrary partition on your set S. And then I'm going to be able to specify time, specify when some partitions are before or after other partitions. The factors will be partitions with the property that you can't have anything else that's strictly before it besides deterministic things. And so there's a sense in which the factors are initial.

**Scott Garrabrant:**
Yeah, the factors are basically the initial things in the notion of time that I want to create out of factored sets. But also intuitively, it feels like they're initial. It feels like they're the primitive things that came first and then everything else was built out of them.

**Daniel Filan:**
Yeah, I think primitive is a better word than initial.

**Scott Garrabrant:**
Yeah, they're - In the [poset](https://en.wikipedia.org/wiki/Partially_ordered_set) of divisibility, right? I wanted to say that one is initial, not the primes. But the primes have the property that you can't really have anything else before them. Yeah, I don't know, primitive is like prime is... Yeah.

**Daniel Filan:**
Yeah. So in the sense they're like when you're dividing numbers, whole numbers, you get to primes and then you get nowhere else, and they're primitive in that sense.

**Scott Garrabrant:**
Right.

## Orthogonality and time <a name="orthogonality-time"></a>

**Daniel Filan:**
Yeah. So you talk about the history of variables as all the initial factors that you need to specify what a variable is. And then you have this definition of orthogonality. And people can read the paper. I don't know if they'll be able to read the paper by the time this goes live, but they'll be able to read something to learn about what orthogonality is.

**Scott Garrabrant:**
Yeah, the [talk](https://www.youtube.com/watch?v=fvj8fi5azTM) and the [transcript of the talk](https://www.alignmentforum.org/posts/N5Jm6Nj4HkNKySA5Z/finite-factored-sets) that, for example, appears on [the MIRI blog](https://intelligence.org/2021/05/23/finite-factored-sets/) or on [LessWrong](https://www.lesswrong.com/posts/N5Jm6Nj4HkNKySA5Z/finite-factored-sets), it has all of the statements of everything that I find important. And so you shouldn't have to wait for a paper. I think that, modulo the fact that you'd have to prove things yourself, that has all the important stuff.

**Daniel Filan:**
So, people, I think for me at least, it's easy to read the definition of orthogonality and still not really know how to think about it, right? So how should people think about what's orthogonal?

**Scott Garrabrant:**
So in the temporal inference thing where we're going to be connecting up these combinatorial structures with probability distributions, orthogonality, it's going to be equivalent to independence. And so, one way to think of orthogonality is... I kind of took out a combinatorial fragment of independence, where I'm not actually working with probabilities but I am working with a thing that is representing independence. Another way to think of it is like when two partitions are orthogonal, you should expect that if you come in and tweak one of them, it will have no effect on the other one, they're separated. And specifically in the factored set framework, orthogonality means they do not have a common factor. Where these factors can be thought of as sources of randomness or sources of something. If they do not share a common factor, then they're in some sense separated.

**Daniel Filan:**
Yeah. So I'll just say one example that helped me understand it is this XY-plane example where the factors were, you're partitioning up according to the X coordinate. And you can also partition up according to what the Y coordinate is. So you can have one different partition, that's are you on the left-hand side or the right-hand side of the Y-axis. That's a partition of the plane. It's like a variable. It's like are you on the left, or are you on the right. And there's a second variable, that's are you above the X-axis, or are you below the X axis. Right? That also partitions the plane up. And my understanding is that in this factored set, those two partitions, or you could think of them as variables, are orthogonal, and that hopefully gives people a sense. It's also kind of nice because they're literally - if you think about the dividing lines, they're literally orthogonal in that case, and that's maybe not a coincidence.

**Scott Garrabrant:**
Yeah. So historically when I was developing factored set stuff, I was actually working with things that looked like I have two partitions and there's something nice when it's the case that they're both kind of mutually... Or sorry, for any way of choosing a value of this X partition and also choosing the value of the Y partition, those two parts will intersect. So in your example, right, the point is that all four quadrants exist, and this is like there's a sense in which this is saying that the two partitions aren't really stepping on each other's toes.

**Scott Garrabrant:**
If we specify the value of this X partition, it doesn't stop us from doing whatever we want in terms of specifying the value of the Y partition. And largely orthogonality is a step more than that, where a consequence of being orthogonal, this is they're not going to step on each other's toes. But I have this extra thing, which is really just the structure of the factorization, but there's this extra thing beyond just not stepping on each other's toes, which is coming from some sort of theory of intervention or something. You can view the factored set as a theory of intervention because the factored set basically allows you to take your set and view its elements like tuples. And when your elements are like tuples, you can imagine going in and changing one value and not the others. And so, orthogonality looks like it's not just X and Y are compatible, like compatible in all ways and assign values to each of them, but it's also, when you mess with one, it doesn't really change the other.

**Daniel Filan:**
Cool. So another question about orthogonality. So in the talk that listeners can [watch](https://www.youtube.com/watch?v=fvj8fi5azTM) or [read a transcript of](https://www.alignmentforum.org/posts/N5Jm6Nj4HkNKySA5Z/finite-factored-sets), you sort of say, "Okay, we have this definition of orthogonality and this definition of conditional orthogonality, which is a little bit more complicated but kind of similar." You then talk about inference in the real world. So we sort of imagine that you're observing things that are roughly like... You don't observe the underlying set, but you observe things that are roughly like these factors. And somehow you get evidence about what things are orthogonal to each other. And from that, you go on and infer a whole bunch of stuff, or you can sometimes. And you give an example of how that might work. How would I go about gathering this orthogonality data of what things in the world are orthogonal to what other things?

**Scott Garrabrant:**
So the default is definitely passing through this thing that I call the fundamental theorem. So the fundamental theorem says that conditional orthogonality is equivalent to, for all probability distributions that you can put on your factored set which respect the factorization, your variables are conditionally independent. I don't know, I phrased that as for all probability distributions, but you can quickly jump that for a probability distribution in general position.

**Scott Garrabrant:**
And so the basic thing to do is if you have access to a distribution over the things, over the elements of your set or over something that's a function of the elements of your set, if you have access to a distribution, it's kind of a reasonable assumption to say that if you have a lot of data and it looks like these two variables are independent, then you assume that they are orthogonal in whatever underlying structure produced that distribution. And if they're not independent, then you see they're not orthogonal.

**Scott Garrabrant:**
And it's just - the default way to get these things is via taking some distribution, which could be coming from a bunch of samples, or it could be a Bayesian distribution. But I think that orthogonality is something that you can basically observe through its connection to independence versus time is not as much.

**Daniel Filan:**
I guess this works when my probability distribution has this form where the original factors in your set B, like the primitive variables, have to be independent in your probability distribution. And if they're independent in that distribution, then conditional independence is conditional orthogonality. How would I know if my distribution had that nice structure?

**Scott Garrabrant:**
You can just interpret all of the independence as orthogonality and see whether it contradicts itself, right? It's like you might have a distribution that can't really be well-described using this thing. And one thing you might do is you might develop an orthogonality database where you keep track of all of the orthogonalities that you observed. And then you notice that there's some orthogonalities that you observed that are incompatible with coming from something like this. Yeah, I'm not sure I fully understand the question.

## Using finite factored sets <a name="using-ffs"></a>

**Daniel Filan:**
I guess what I'm asking is imagine I'm in a situation, right? And I don't already have the whole finite factored set structure of the world. I'm wondering how I go about getting it. Especially if the world is supposed to be my life or something. So I don't get tons of reruns. Maybe this isn't what it's supposed to do.

**Scott Garrabrant:**
Basically the answer you'd give here is similar to the answer you'd give to the same question about Pearlian Causality. And I think that largely the temporal inference story makes the most sense in a context that's very like you have a repeated trial that you can repeat an obnoxious number of times, and then you can get a bunch of data. And you're trying to tell a story about this trial that you repeated. And yeah, so the story that I tell makes the most sense in a situation that's like that.

**Scott Garrabrant:**
I'm excited for a lot of things about factored sets that are not about just doing temporal inference from a probability distribution. And those feel like they play a lot more nicely with... Sorry, the applications that feel like they're about embedded agents to me are different from the applications that feel like they're about temporal inference. Because it feels like you need something like this frequentist, lots of repetition to be able to get a distribution in order to be able to do a lot of stuff with temporal inference. Or at least doing it the naive way, maybe you can build up more stuff.

**Daniel Filan:**
So embedded agency is your term for something being an agent in a world where your thinking processes are just part of the physical world and can be modified and modeled and such. Is that a fair summary?

**Scott Garrabrant:**
Yeah.

**Daniel Filan:**
Okay, so the applications of the finite factored set framework to embedded agency, are you thinking of that more as a way to model things?

**Scott Garrabrant:**
Yeah, I'm thinking of it mostly like basically, there's a lot of ways in which people model agents using graphs where the edges in the graph represent information flow or causal flow or something, that all feel they're entangled with this Pearlian causality story. And often I think that pictures like this fail to be able to handle abstraction correctly, right? I have a node that represents my agent. And I don't really have room for another node that represents a coarser version of my agent because if I did, which one gets the arrow out of it and such. And so largely, my hope in embedded agency is just all the places where we want to draw graphs, instead, maybe we can draw a factored set and this will allow things to play more nicely with abstraction and playing nicely with abstraction feels like a major bottleneck for embedded agency.

## Why not infinite factored sets? <a name="why-finite"></a>

**Daniel Filan:**
Okay. Yeah, I'll ask more about that a bit later. Yeah, so I guess I want to ask more questions about the finite factored set concept itself. Why is it important that it's finite?

**Scott Garrabrant:**
So I can give an example where you should not expect the fundamental theorem to hold in the cases where it's infinite. So the thing where independence exactly corresponds to orthogonality, in the infinite case, one shouldn't expect that to hold. And it might be that you can save it by saying, "Well, now we can't take arbitrary partitions. We can only take petitions that have a certain shape."

**Daniel Filan:**
Sort of like measurability criteria?

**Scott Garrabrant:**
Sort of like measurability criteria, but measurability is actually not going to suffice for this. I can give an example. To give an example, if you imagine the infinite factored set that is countable bit strings.

**Daniel Filan:**
So this is just like infinite sequences of ones and zeros, the set of all of them?

**Scott Garrabrant:**
Yeah. So you have the set of all infinite sequences of ones and zeros, and you have the obvious factorization on this set, which is you have one factor for each bit. And then there's a partition that is asking, "Is the infinite bit string, the all zero string?" And there's the partition that's asking, "Is the infinite bit string, the all one string?" These are two partitions, let's call them X and Y.

**Daniel Filan:**
Okay.

**Scott Garrabrant:**
And it turns out that for any probability distribution you can put on this factored set that respects the factorization. At least one of this partitions is going to have to be - either it's going to be the case that the all zero string is probability zero or the all one string is probability zero.

**Daniel Filan:**
Yep.

**Scott Garrabrant:**
Because all of the bits have to be independent. And then you'd be able to conclude that the question, "Is it the all zero string?" And "Is it the all one string?" Those two questions will have to be independent in all distributions on the structure, but it really doesn't make sense to call them orthogonal.

**Daniel Filan:**
Why doesn't it?

**Scott Garrabrant:**
Why doesn't it make sense to call them orthogonal? It doesn't make sense to call them orthogonal because all of our factors go into the computation of that fact. And so, if you're thinking of orthogonality as, they can be computed using disjoint collections of factors, you can't really compute whether something's the all zero string or whether something's all the one string without seeing all the factors. I mean, you can't compute it because the first time you see a one, you can say, "All right, I'll stop looking." But in my framework, that doesn't count. You have to specify upfront all the bits you're going to use. And so, I don't know, I think there's some hope to being able to save all of this. And I haven't done that yet. And there's another obstacle to infinity, which is even the notion of thinking about the history of a partition, that's not even going to be well-defined in the infinite case.

**Daniel Filan:**
Okay, and so the history of a partition in the finite case, it was just the smallest set of factors that determined - if I have a partition with elements like X1 through Xk or something, the history of the partition is just all of the basic factors, all the things in my set B where if you think of the things in the set B, its variables, if I know the value for all the variables, it's the smallest set of variables, such that if I know that the value for all of them, I can tell what partition element I'm in for the thing that I'm looking for the history of. So it's the smallest set of, the smallest amount of initial information or something, that's specifying the thing I'm interested in. Is that what a history is?

**Scott Garrabrant:**
Yeah, that's right.

**Daniel Filan:**
And it's a bit worrisome because usually there's not a smallest set of sets of... Right? That's not obviously well-defined.

**Scott Garrabrant:**
Yeah. So, specifically smallest in the subset ordering. And so the history of a partition is a set of factors. And if you take any set of factors that would be sufficient to determine the value of that partition, the answer to that question, what part of it's in, if you have any set of factors that were sufficient to compute the value of X, then necessarily that set of factors must be a superset of the history. And so it's not smallest by cardinality, it's smallest by the subset ordering. And showing that this is well-defined involves basically just showing that sets of factors that are sufficient to determine the value of X are closed under intersection. So if I have two different sets of factors and it's possible to compute the value of X with either of those sets of factors, then it's possible to compute the value of X using their intersection.

**Scott Garrabrant:**
But actually this is only true for finite intersections. And so if I have a partition and you're able to compute the value from either of these two sets of factors, then you're able to compute the value from their intersection. But if I have an infinite class of things then I can't necessarily compute the value from their intersection. To see an example of this, if you, again, look at infinite bit strings with the obvious factorization and you look at the partition, are there finitely many ones? If you take any infinite tail of your bit string that's sufficient to determine, are there finitely many ones? But if you take the intersection of all of these infinite tails, you get the empty set.

**Daniel Filan:**
Yeah, so one way I'm now thinking of this is the problem with infinite sets is that they have these things that are analogous to tail events or something in normal probability theory where you depend on all of these infinite number of things, the limit of it. But the limit is actually zero, but you can't exactly-

**Scott Garrabrant:**
Yeah, there's this - actually, what I think is a coincidence.

**Daniel Filan:**
Okay.

**Scott Garrabrant:**
But you could do this naive thing where you say, "Well, just take the intersection and call that the history." And then the history of the question, "Are there finitely many ones?", would then be the empty set and then a lot of properties would break. But an interesting coincidence here or something, I don't know, I don't think it's a coincidence, but I don't like this definition. I don't like the definition of generalizing to the infinite case by just defining the history to be the intersection. But if you were to do that, then you would get that the question, "Are there finitely many ones?", is orthogonal to itself because it has empty history. And what kind of partitions are orthogonal to themselves? They're deterministic ones where one of the parts has probability one and all the others have probability zero.

**Scott Garrabrant:**
And the [Kolmogorov zero-one law](https://en.wikipedia.org/wiki/Kolmogorov%27s_zero%E2%80%93one_law) says that properties like the question, "Are there finitely many ones?", if all of the individual things are independent, are necessarily probability zero or one. And so there's this thing where if you define history in this way, if you naively extend history to the infinite case by just taking the intersection, even though it's not closed under intersection, you actually get something that feels like it gives the right answer because of the Kolmogorov zero-one law.

**Daniel Filan:**
Yeah, but it's going through some weird steps to get the right answer.

**Scott Garrabrant:**
Yeah.

**Daniel Filan:**
By the way, I don't know, there are a variety of these things called [zero-one laws](https://en.wikipedia.org/wiki/Zero%E2%80%93one_law) in probability theory, and if listeners want to think about knowledge and changing over time, I don't know. Some of these zero-one laws are fun to mull over and think about how they apply to your life or something. Oh, do you have comments on that claim?

**Scott Garrabrant:**
No, I think Kolmogorov's zero-one law is really interesting and I would recommend it to people who like interesting things.

## Limits of, and follow-up work on, finite factored sets <a name="ffs-limits"></a>

**Daniel Filan:**
All right. So back on the finite factored sets, they're sort of a way of modeling some types of worlds. Or some sorts of ways the world can be. Are there any worlds that can't be modeled by finite factored sets?

**Scott Garrabrant:**
Yeah.

**Daniel Filan:**
I guess infinite ones, but ignoring that for this second.

**Scott Garrabrant:**
So there's this issue that's similar to an issue in Pearl where we kind of - when you look at distributions that are coming from a finite factored set or from a DAG, we're looking at probabilities in general position. And so it doesn't really make sense to have a probability one half or one fourth. Although-

**Daniel Filan:**
What do you mean by probability in general position?

**Scott Garrabrant:**
So in both my world and Pearl's world, we want to specify a structure.

**Daniel Filan:**
Yep.

**Scott Garrabrant:**
And then we have all of the probability distributions that are compatible with that structure. And these give you a manifold or something of different probability distributions. And then some measure zero subset of them have special coincidences. And when I say in general position, I mean, you don't have any of those special coincidences. And so an example of a special coincidence is any time you have a probability of one half or one fourth or something like that, that's a special coincidence. But to a Bayesian, that might happen because of principle of indifference or something. Or to a limited Bayesian that kind of doesn't really know what... It feels like principle of indifference advises having probabilities that are rational numbers, but probabilities that are rational numbers lead to coincidences in independence that don't arise from orthogonality. And so there's a sense in which my framework and the Pearlian framework don't believe in rational probabilities as something that just happens, or something.

**Daniel Filan:**
Yeah. I mean, it's even more concerning because if I think that I'm a computer and I think I assign probabilities to things, well, the probabilities I assign will be numbers that are the output of some computation. And there are a countably infinite number of computations, but there are an uncountably infinite number of real values. So somehow, if I'm only looking at things in general position, I'm ruling out all of the things that I actually could ever output.

**Scott Garrabrant:**
Yeah, so I have a little bit of a fragment of where I want to go with trying to figure out how to deal with the fact that my system and Pearl's system don't believe in rational probabilities, which is to define something, and this is going to be informal. Well, it'll be formal but wrong. To define something that's like, you take a structure that is a factored set together with a group of symmetries on the factored set that allows you to maybe swap two of the parts within a partition or swap two of the partitions with each other, or swap two of the factors with each other, right? So you can have some symmetry rules. So if you, for example, considered bit strings of length five again, you could imagine a factored set that it separates into the five bit locations, is this bit zero or one, but then it also has the symmetry of you can swap any of the digits with each other that can also do swapping zero with one. But for now, I'll just think about swapping any of the digits with each other.

**Scott Garrabrant:**
And then it's subject to the structure that this is this factored set and the swapping rule for this group of symmetries, then the set of distributions that are compatible with that structure will not be just things in which the bits are independent from each other, it'll be any distribution in which the bits are IID, so independent and also identical. And so you could do that. So you could say, "Well, now my model of what might happen, the thing that I'm going to try to infer from my probability distribution is a factored set together with a group of symmetries on that factored set." And I mean, you're not going to at least naively get the fundamental theorem the same way. And so I'm not sure what happens if you try to do inference on something like this, but if you want to allow for things like rational probabilities, then maybe something like that would be helpful.

**Daniel Filan:**
All right, so I'd like to ask a question about the form of the framework. So when I think of models of causality or of time, the two prior ones that I think of are, we talked about Pearl's work on directed acyclic graphs and do-operators and such, where you can draw a graph, and also Einstein's work on special and general relativity, where you have this space time thing that's very geometric, very curved and you have this time direction, which is special and kind of different from the spatial directions. Those were all really geometric and included some nice pictures. Finite factored sets does not have many pictures. Why not? I really liked pictures.

**Scott Garrabrant:**
Yeah. I mean, I think it has something to do with the variable non-realism, where it feels like the points or nodes in your pictures or something - if I take a Pearlian DAG and there are 10 nodes in it and even if I assume that they're all just binary facts, then now I have, well, you take 1024 different ways that the world can be. And then you take Bell number of 1024 different possible variables that I can define on this, which is obnoxiously huge and then there's not as much of a useful interpretation of the arrows that connect them up or something, it's something to do with variable non-realism, where there's a sense in which Pearl's kind of starting from a collection of variables, which is a way to factor the world into some small object. And because I'm not starting with that, my world is kind of a lot larger.

**Scott Garrabrant:**
I don't know. Another thing that I'd call a theory of time is people talk about time with entropy. That's another example that doesn't feel as visual.

**Daniel Filan:**
Yeah, that's true.

**Scott Garrabrant:**
And I think that that's a lot more variable free and that's maybe part of why.

**Daniel Filan:**
Yeah. It's also the case that once you have variables, they have these relations in terms of their histories and such, you could draw them in a DAG or something.

**Scott Garrabrant:**
Yeah. It's like the structure of an underlying finite factored set is very trivial or something. It's - Pearl has a DAG, and if you wanted to draw a finite factored set as a DAG, it would just be a bunch of nodes that are not connected at all that each have their own independent sources of randomness. And then if you wanted to, you could maybe draw an arrow from these nodes to all the different things that you can compute using them. Or if you wanted to say, oh, let's just have the basic variables then it's just these nodes.

**Daniel Filan:**
Yeah. But I guess somehow if you want to talk about the structure that lets you talk about variables, but you don't want to talk about variables. I guess that's less amenable to pictures perhaps.

**Scott Garrabrant:**
Yeah. I mean, I don't feel like physics and Pearl have pictures for necessarily the exact same reason or something, and I'm kind of just, graphs got lucky in the way that they're easy to visualize or something like that.

**Daniel Filan:**
Yeah, that might be right. It's also true that graphs are not - simple graphs are easy to visualize, but there are a lot of non-planar graphs that are kind of a pain to draw. Yeah. So, a related question complaint I have is a lot of this work seems like it could be category theory.

**Scott Garrabrant:**
Yeah. It could be category theory.

**Daniel Filan:**
Yes. So partitions are basically functions from a set to a different set and the parts are just all the things that have the same value of the function?

**Scott Garrabrant:**
Yeah. Partitions is kind of the information content of a function out that you get by kind of ignoring the target and only looking at the source.

**Daniel Filan:**
Yeah. And it seems like there are probably nice definitions of factors and such and... You know, there's lots of duality and category theory has pictures. It's also a little bit nice in that I have to admit, looking at sets of sets of sets of sets can be a little bit confusing after a while in the way that categories can have some nice language for that. So why isn't everything category theory, even though category theory is objectively great?

**Scott Garrabrant:**
Yeah. I mean, it goes further than that. I actually know most of the category theory story and I've worked out a lot of it and kind of went with the combinatorialist aesthetic with the presentation anyway. So what are my reasons? One reason is because I kind of trust my category theory taste less and I kept on changing things or something in a way that I was not getting stuff done in terms of actually getting the product out or something by working in category theory. And so it was kind of oh yeah, I'll punt that to the future.

**Scott Garrabrant:**
Another reason is because it feels the system just really doesn't have prerequisites and by phrasing everything in terms of category theory, you're kind of adding artificial prerequisites that maybe make the thing prettier, but you actually, you know what a set is, you can kind of go through all the proofs or something. That's not entirely true, but it's because I'm working in a system that has very few prerequisites, the extra cost of prerequisites is higher. The marginal cost of adding prerequisites is higher.

**Scott Garrabrant:**
Another reason was I was just really shocked by [the sequence that counts the number of factorizations](https://oeis.org/A338681) not showing up on [OEIS](https://oeis.org/). So yeah, if you take an n-element set and you count how many factorizations there are on the n-element set, you get a sequence and there's this Online Encyclopedia of Integer Sequences that has 300,000 sequences and it does not have this sequence in spite of having a bunch of lower quality sequences.

**Scott Garrabrant:**
And I was very surprised by this fact, and it feels like a very objective test. I'm not a particularly scholarly person. It's hard for me to figure out what people have already done. And I was just pretty blown away by the fact that this thing didn't show up on OEIS. And so I kind of stuck with the combinatorialist thing because it had that objective thing for the purpose of being able to do an initial sell or something.

**Daniel Filan:**
Okay.

**Scott Garrabrant:**
Yeah. Those are most of my reasons. I think that - I haven't worked out all the category theory, but I think it will end up being pretty nice. In fact, I think that just even the definition of conditional orthogonality, I think can be made to look relatively nice categorically, and it's via a path that is pretty unclear from the definition that I give in [the talk](https://www.youtube.com/watch?v=fvj8fi5azTM) or [the post](https://www.alignmentforum.org/posts/N5Jm6Nj4HkNKySA5Z/finite-factored-sets) but there's an alternative definition that kind of looks like if you want to do orthogonality and you want to condition on some fact about the world, the first thing you do is you take your original factored set and you kind of take the minimal flattening of it. So that the thing you want to condition on is kind of rectangular in your factored set and then you - where by flattening I mean, you merge some of the factors together.

**Daniel Filan:**
Okay.

**Scott Garrabrant:**
And if you take the minimal factoring and then you ask whether your partitions are orthogonal in the minimal factoring, that corresponds to conditional orthogonality. And so I think that categorically there's a nice definition here. But I definitely agree about the category theory aesthetic and I think that it actually is a good direction to go here that I may or may not try to do myself, but if somebody was super interested in trying to convert everything to category theory, I could talk to them about it.

**Daniel Filan:**
So speaking of that, I'm wondering, what follow-up work are you excited about being done here? And do you think that this kind of development is going to look more like showing nice things within this framework - making it categorical or showing the decidability of inference in finite factored sets? Or do you think it's going to look more like iterating on some of the definitions and tweaking the framework a little bit until it's the right framework?

**Scott Garrabrant:**
Yeah. I mean the category theory thing a little bit does fall into tweaking the framework until it's the right framework. Although it's a little different, I have applications I'm excited about in both spaces.

**Scott Garrabrant:**
Yeah, if I were to list applications that I expect that I'm not personally going to do, that seem like projects that would be interesting for people to pick up, one of them would be converting everything to category theory. One of them would be figuring out all the infinite case stuff and looking at applications to physics. I think that there's a non-trivial chance of some pretty good applications in physics that would come out of figuring out all the infinite case stuff. Because I think that factored sets are actually a lot closer to being able to give you something like continuous time then the Pearlian stuff. Yeah.

**Scott Garrabrant:**
So one would be the physics thing. One would be basically trying to do computational stuff in terms of, I kind of just have a couple proofs of concept of how to do temporal inference. And I think you said, showing decidability of temporal inference is a thing, where really it's... I think that somebody should be able to actually search over the space of a certain flavor of proof and be able to actually come up with examples of temporal inference that come from this where you take in some orthogonality data and are able to infer time from them. And I think that there's a computational question here that I think, I might be wrong, might be able to at least be able to produce some good examples, even if it's not actually doing temporal inference in practice, and so I'd be excited about something that.

**Scott Garrabrant:**
I would be excited about somebody trying to extend to symmetric finite factored sets, which is the thing I was talking about earlier, about dealing with rational probabilities. I think that of these that I listed, the one that I'm most likely to want to try to work on myself is the symmetric factored sets thing. Because I think that could actually have applications to the kind of embedded agency type stuff I'd want to work with. But for the most part I'm expecting to myself think in terms of applications, as opposed to think in terms of extending the theory, and all the things that I kind of said, were all forms of extending the theory either by tweaking stuff or by kind of putting stuff on top of it. I think it's mostly just putting stuff on top of it.

**Scott Garrabrant:**
I think I don't say that much. Sorry, I think that there aren't that many knobs to twiddle with the basic thing. You could have some new orientation on it, but I think it will be basically the exact same thing. I think that the parts... The way that I defined it, there was only one factorization on a zero element set. Maybe it would be nicer if there were infinitely many factorizations on zero. The definitions might be slightly different, but I think it's basically the same core thing.

**Scott Garrabrant:**
I think that I'm mostly thinking that the baseline I have is kind of correct enough for the kind of thing that I want to do with it, that I don't expect it to be a whole new thing. I expect it to be built on top, and there's different levels of built on top.

## Relevance to embedded agency and x-risk <a name="embedded-agency-x-risk-relevance"></a>

**Daniel Filan:**
So I guess I'd now like to pivot into a bit of a more general discussion about your research and your research taste. How do you see the work on finite factored sets as contributing to reducing existential risk from artificial intelligence? If you see it as doing that?

**Scott Garrabrant:**
I think that a lot of it factors through trying to become less confused about agency and embedded agency, which I don't know, I have opinions in both directions about the usefulness of this. Sometimes I'm feeling like, yeah this isn't going to be useful and I should do something else. And then sometimes I'm just interacting with questions that are a lot more direct and noticing how a lot of the kind of questions that I'm trying to figure out for embedded agency actually feel like bottlenecks to be able to say smart things about things that feel more direct.

**Daniel Filan:**
Can you give an example of that?

**Scott Garrabrant:**
I don't know. Evan says some things about [myopia](https://www.alignmentforum.org/posts/LCLBnmwdxkkz5fNvH/open-problems-with-myopia).

**Daniel Filan:**
That's Evan Hubinger.

**Scott Garrabrant:**
Right. And that feels a lot more direct, trying to get a system that's kind of optimizing locally and not looking far ahead and stuff like that.

**Scott Garrabrant:**
And I feel like, in wanting to think about what this even means, I notice myself wanting to have a better notion of time and better notion of things about the boundaries between agent and environment and all of this stuff. And so I don't know... That's an example of something that feels kind of more direct, myopia feels like something that could be very useful if it could be implemented correctly and could be understood correctly.

**Scott Garrabrant:**
And when I try to think about things that are more direct than embedded agency, I feel like I hit the same kind of cruxes and that working with embedded agency feels like it's more directed at the cruxes, even though it's less directed at the actual application of the thing in a way that I expect to be useful.

**Daniel Filan:**
In the myopia example, I think the first-pass solution would be look, there's just physical time [that] basically exists. And we're just going to say, okay, I want an AI system. I want it to care about what's going to happen in the next 10 seconds of physical time and not things that don't happen within the next 10 seconds of physical time. Do you think that's unsatisfactory?

**Scott Garrabrant:**
Yeah. So, I mean, I think that you can't really look at a system and try to figure out whether it's optimizing for the next 10 seconds or not. And I think that - the answer that I actually gave with the myopia thing was a little off because I was actually remembering a thought about myopia, but it wasn't about time. It was about - just time was the thing that I said in that thing.

**Scott Garrabrant:**
It was more about counterfactuals and more about the boundary between where the agent is or something like that. But I don't know. I still think the example works. I mean, I think that it comes down to you want to be able to look at a system and try to figure out what it's optimizing for. And if you have the ability to do that, you can check whether it's optimizing for the next 10 seconds, but in general, you don't really have the ability to do that.

**Scott Garrabrant:**
Figure out what it's trying to do or something like that. And I think that... Yeah, how do I get at the applications? Okay. So one thing I think is that in trying to figure out how the system works, it is useful to try to understand what concepts it's using and stuff like this. And I think that the strongest case I can kind of make for factored sets is that I think that there's a sense in which factored sets is also the theory of conceptual inference. And I think that this could be helpful for looking at systems or trying to do oversight of systems that you want to be able to look at a thing and figure out what it's optimizing for.

**Daniel Filan:**
In what ways would you say it's a theory of conceptual inference?

**Scott Garrabrant:**
Well, one way to look at the diff between factored sets and Pearl is that we're kind of not starting from a world factored into variables instead we're inferring the variables ourselves. And so there's a sense in which if you try to do Pearlian style analysis on a collection of variables, but you messed it up, and instead of having a variable for what number this... I have a number and it's either zero or one and it's also either blue or green. And I can also invent this concept called grue, which is a green zero or a blue one. And instead of thinking in terms of what's the number and is it blue, you can think of what's the number and is it grue, and maybe if you're working in the latter framework, you're kind of using the wrong concepts and you will not be able to pull out all the useful stuff you'd be able to if you were using the right concepts.

**Scott Garrabrant:**
And factored sets kind of has a proof of concept towards being able to distinguish between blue and grue here, where the point is, in this situation, if the number is kind of independent of the color and you're working with the concept of number and the concept of grue-ness, you have this weird thing where it looks there's a connection between number and grue-ness, but it also is the case that if I invent the concept of number [xor](https://en.wikipedia.org/wiki/Exclusive_or) grue-ness, I kind of invent color, and color lets me factor the situation more and see that maybe you should think of it as the number and the color are primitive properties like we were saying before, and grue-ness is a derived property.

**Scott Garrabrant:**
And so there's a sense in which earlier things are more primitive, and it's not just earlier things, I think there was more than just that. But there's a sense in which because I'm not taking my variables or my concepts as given, I am also doing some inferring which concepts are good.

**Daniel Filan:**
So somehow it strikes me that inferring which concepts are good, is a related, but different problem to inferring which concepts a system is using.

**Scott Garrabrant:**
I don't know, there's stuff that you like to think about that involve kind of having separate neurons as part of it. And I think there's a sense in which it might be that we're confused when we're looking at a neural net because we're thinking of the neurons as more independent things, when really they could be a transform similar to the blue/grue thing from some other thing that is actually happening and being able to have objective notions of what's going on there - being able to have a computation and having there be a preferred basis that causes things to be able to factor more or something feels... Yeah, so I guess I'm concretely pointing at the picture of factorization into neurons in the result of a learned system might be similar to grue.

**Daniel Filan:**
Yeah, it's interesting in that people have definitely thought about this problem, but all the work on it seems kind of hacky to me. So for instance, so I know Chris Olah and collaborators now or formerly at OpenAI, have done a lot of stuff on using non-negative matrix factorization to kind of get out the linear combinations of neurons that they think are important. And the reason they use non-negative matrix factorization, as far as, I might be getting this wrong, but as far as I can tell it's because it kind of gets good results sort of, rather than a theory of non-negativity or something.

**Daniel Filan:**
Or a similar thing is there's [some](https://arxiv.org/abs/1312.6199) [work](https://arxiv.org/abs/1704.05796) about exactly trying to figure out whether the concepts in neural networks are on the neurons or whether they're these linear combinations of neurons, but the way they do it, which again, I'm going to sound critical here. It's a good first pass, but a lot of this work is, okay, we're going to make a list of all of the concepts. And now we're going to test if a neuron has one of the concepts which I've decided really exists, and we're going to check random combinations of neurons and see if they have concepts, which I've decided exist and which does better.

**Daniel Filan:**
Yeah. There's definitely something unsatisfying about this. Maybe I'm not aware of more satisfying work. Yeah. It does seem there's some problem there.

**Scott Garrabrant:**
And again, I think that you're not going to be directly applying the kind of math that I'm doing, but it feels I kind of have a proof of concept for how one might be able to think of blueness as a statistical property, blueness versus grue-ness as a statistical property. It's something that you can kind of get from raw data.

**Scott Garrabrant:**
And I don't know, I feel there's a lot of hope in something that. But that's also not my main motivation. That was a side effect of trying to do the embedded agency stuff. But it's kind of not a side effect because I think that the fact that I'm trying to do a bunch of embedded agency stuff and then I... I was trying to figure out stuff related to time and related to decision theory and agents modeling themselves and each other. And I feel like I stumbled into something that might be useful for identifying good concepts, like blue.

**Scott Garrabrant:**
And I think that that stumbling is part of the motivation. I don't know, that stumbling is part of the reason why I'm thinking so abstractly. That's not a motivation for thinking about embedded agency. That's a motivation for thinking as abstractly as I am, because you might get far reaching consequences out of the abstraction.

## How Scott researches <a name="what-is-it-like-to-be-a-scott"></a>

**Daniel Filan:**
All right, so I guess a few other questions to kind of get at this. What do you do? What is a day of Scott researching look like?

**Scott Garrabrant:**
I mean, recently it's been thinking about presentation of factored set stuff. Often it involves thinking in [Overleaf](https://www.overleaf.com/) or something where I'm just writing some stuff up and then I have thoughts as a consequence of the writing. Often it looks like talking to people about different formalisms and different weird philosophy. Yeah, I don't know.

**Daniel Filan:**
So you're thinking about presentation of this work? What are you trying to get right or not get wrong? What are the problems that you're trying to solve in the presentation?

**Scott Garrabrant:**
I think that a large part of the presentation thing is I want to wrap everything up so that it feels like something that can just be used without thinking about it too much or something that.

**Scott Garrabrant:**
Part of the presentation is some hope that maybe it can have large consequences to the way that people think about structure learning. But mostly it's kind of having it be a basic tool that I can then kind of... I've kind of locked in some of the formalism such that I don't have to think about these details as much and I can think about the things that are built on top of them.

**Scott Garrabrant:**
I don't know, I think that in thinking about this presentation or something, it's not where the interesting work is done. I think that the interesting... The part that had a lot of interesting meat in terms of actually how research is done was a lot of the stuff that I did late last year, which was kind of, okay I finally wrapped up [Cartesian frames](https://www.alignmentforum.org/posts/BSpdshJWGAW6TuNzZ/introduction-to-cartesian-frames). What is it missing? And it largely was - I had this orientation that was Cartesian frames kind of feel like they're doing the wrong thing similar to... Or sorry, like... All right. So here's a story that I can kind of tell, which is I was looking at Cartesian frames, which is some earlier work from last year. And part of the thing was you viewed this world as a binary function from an agent's choice and an environment's choice, or an agent's way of being, or an agent's action to the environment's way of being... Sorry, cross the environment's way of being to the full world state. And a large part of the motivation was around taking some stuff that was kind of treated as primitive and making it more derived. In particular I was trying to make time more derived and some other things, but I was trying to make time feel more derived so that I can kind of do some reductionism or something.

**Scott Garrabrant:**
And at the end of Cartesian frames, I was unsatisfied because it felt like the binary function... The function from A cross E to W was itself derived, but not treated that way.

**Scott Garrabrant:**
When I look at a function from A cross E to W, I don't want to think of it as a function. I want to think of it as like, well, there's this object A cross E, and there's this object W, and there's a relation between them. And then there's that relation kind of satisfies the axioms of a function, which is for each way of choosing an A cross E there exists a W. But then I also wanted to say, well, it's not just a function from A cross E to W, where A cross E is a single object.

**Scott Garrabrant:**
There's this other thing, which is I have this space, A cross E, and I'm specifically viewing it as a product of A and E. And what was going on there was, it felt like in my function from A cross E to W, I did not just have it's a function not a relation, I also have this system of kind of interventions, where I could imagine tweaking the A bit, and tweaking the E bit independently. And the product A cross E as an object, A cross E, it has the structure of a product. And I was trying to figure out what was going on there in a way that - the same way that you can view the function as just a relation that satisfies some extra conditions.

**Scott Garrabrant:**
I wanted to view the product as some extra conditions. And those extra conditions were basically what kind of grew into me being really interested in understanding the combinatorial notion of orthogonality. And so, I was dissatisfied with something being not quite philosophically right or not quite derived enough or something, and I double clicked on that a bunch.

**Daniel Filan:**
Okay, so another question that I want to ask is, so you work at MIRI, the Machine Intelligence Research Institute, and, I think, among people who are trying to reduce existential risks from AI, as a shorthand, people often talk about the MIRI way of viewing things and the thoughts that MIRI has, or something. I also work at [CHAI](https://humancompatible.ai/), so CHAI is the Center for Human Compatible AI and sometimes people talk about the CHAI way of doing things and that always makes me mad because I'm an individual, damn it. How do you think, if people are modeling you as just one of the MIRI people, basically identical to [Abram Demski](https://www.alignmentforum.org/users/abramdemski), but with a different hairstyle, what do you think people will get wrong?

**Scott Garrabrant:**
Yeah, so I've been doing a lot of individual work recently, so it's not like I'm working very tightly with a bunch of people, but there is still something to be said for, well, even if people aren't working tightly together, they have similar ways of looking at things.

**Daniel Filan:**
Maybe in a world where they really understand Abram and the rest of the people.

**Scott Garrabrant:**
Yeah, I can point at concrete disagreements or differences in methodology or something. I think that Abram and I have some disagreements about time. I think that there's a thing where Abram, I think Abram more than anybody else, is taking [logical induction](https://arxiv.org/abs/1609.03543) seriously and kind of doing a bunch of work in the field that is generated by and exemplified by logical induction. And I look at logical induction and I'm like, "You're just putting all this stuff on top of time and I don't know what time is yet. I need to go back and reinvent everything, because it's built on top of something and I don't like what it's built on top of."

**Scott Garrabrant:**
And so, I end up being a lot more disconnected and a lot more pushing towards... I don't know, I think that Abram's work will tend to be more on the surface feel directed towards the thing that he's trying to do or something. And I will kind of just keep going backwards into the abstract or something. I also think that there are a lot of similarities between the way of thinking that people in MIRI share, and also some large subset of people in AI safety in general, that they're just a bunch of people that I can kind of predictably expect, if I come up with a new insight and I want to communicate it to them, it'll go kind of well and quickly, not just because they're smart, because they're on the same background, there's less inferential gap. But yeah, people are individuals.

**Daniel Filan:**
That's true. Yeah, so speaking of this desire to make things derived, like where does time come from and such? What do you think you're happy to see as just primitive?

**Scott Garrabrant:**
I don't think it's a what. I think it's something like taking something that you're working with that you think is important and doing reductionism on it, is a useful tool when you have something that is both critical, like you need to understand this thing and there's actual mutual information between this thing that I'm holding and stuff that I care about. And also, it feels like this thing that I'm holding has all these mistakes in it, or has all these inconsistencies, right?

**Scott Garrabrant:**
It's like, why be interested in something like decision theory? Well, decisions are important and also, if you zoom in at them and look at the edge cases, you can kind of see they're built on top of something that feels kind of hacky. And then a thing that you can do is you can say, "Well, what are they built out of?" Yeah, you can try to do some sort of reductionism. And so, it's more a move for when things aren't clicking together nicely. I don't think of reductionism as get down to the atoms, I think of reductionism as the pieces don't fit together correctly, go down one more step and see what's going on or something.

**Daniel Filan:**
So, a related question, it might be too direct, but suppose a listener wants to develop an inner Scott, they want to be able to, "What would Scott say or think about such and such topic?" Just restricted to the topic of reducing existential risk from AI. What do you think the most important opinions and patterns of thought are to get right, that you haven't already explicitly said?

**Scott Garrabrant:**
So, it depends on whether they want an inner Scott for predicting Scott or whether they want an inner Scott for just generally giving them useful ideas or something, in the space of being a thing to bounce things off of and say, "I want to understand X more." One question is, "What would Scott say about X?" And it's not actually important that it matches and it's more important, does it generate useful thoughts? Which is generally what I do with my models of people. I have inner people and then sometimes I find out that they don't exactly match the outer people. And I don't care that much, because their main purpose is to give me thoughts, so I want to make it better, but it's not for prediction, it's for ideas.

**Scott Garrabrant:**
So, I have an inner Scot and my inner Scott is kind of a little bit being rewritten now, because a large part of my inner Scott was kind of identifying with logical induction. And I actually do this for a lot of people. I think about their thought patterns as in relation to things that they've developed. And so, if I were to tell that story, I would say things like part of the thing in logical induction is that you don't make the sub-agents have full stories. A large part of what's going on in logical induction is, it's a way of ensembling different opinions where you don't require that each individual opinion can answer all the questions. You allow them to specialize and you allow them to fail to be able to model things in some domains. And you want them to be able to track what they fail to be able to model. And so, I have a lot of that going on, where I just have kind of boxed fake frameworks in my head where I'm just very comfortable drawing analogies to all sorts of stuff.

**Scott Garrabrant:**
And I don't know. I, for example, wrote [a blog post on what does the Magic: The Gathering color wheel say on AI safety](https://www.alignmentforum.org/posts/9CKBtxWtjvminNTmC/how-the-mtg-color-wheel-explains-ai-safety) or something. I do that kind of thing, where I'm just like, "Here's a model, it's useful for me to be able to think with" or something. And I'm not trusting it in these ways, but I'm kind of trusting it as being generative in certain ways. And I keep on working with it as long as it's generative. Yeah, what am I saying? I'm saying that I tend to think that if something is fruitful and creating good ideas, but also being wrong in lots of ways, I wouldn't say don't mess with it, but if messing with it breaks it, undo that messing with it and let it be wrong and still be fruitful or something. And so, I tend to work with obviously wrong thoughts or something like that.

**Daniel Filan:**
Okay. Another question about intellectual production is, there's this idea of complements to something, or complements to some production process or things that are not exactly, maybe I partially mean inputs, or inputs to the process or things separate from the process, that make the process better. So, in the case of Scott Garrabrant doing research, what are the best complements to it?

**Scott Garrabrant:**
I think that isolation has been pretty good actually. Does that count? Is that a complement? Is that the type of complement?

**Daniel Filan:**
I just realized that I've been kind of confused about what complement is, but it's at least an input. What's been good about isolation?

**Scott Garrabrant:**
I don't know. I think I've just largely been thinking by myself for the last year, as opposed to thinking with other people and it felt like it was good for me for this year or something. I might want to go back to something else. And why? I mean, I think there is a thing where I have in the past made mistakes of the form, trying to average myself with the people around me in terms of what to think about things, and this is dampening, just because of [the law of large numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers), I guess.

**Daniel Filan:**
It's [the heat equation](https://en.wikipedia.org/wiki/Heat_equation), right? Everyone averages everyone else, and things become uniform.

**Scott Garrabrant:**
Right. There's a sense in which working with other people is grounding in that it keeps on giving feedback on things, but I don't know, grounding has good things and has bad things associated with it. And one of the ways in which it has bad things associated with it is that, I don't know, it's like things can flow less or something.

**Daniel Filan:**
Yeah, it's funny, I don't exactly know what grounding is in the social sense. I recently read [a good blog post about it](http://agentyduck.blogspot.com/2021/05/staying-grounded.html), but I totally forgot. I mean, there is one sense in which, so if I think about literal physical grounding in electronics, the point of that is to equalize your electrical potential with the electrical potential of the ground, so that you don't build up this big potential difference and then have someone else touch it and touch the ground and have some crazy thing happen. But as long as you have the same potential as the ground, it means that there's not a net force for charges to move from the ground to you or vice versa, but it does mean that things can sort of move in both ways.

**Daniel Filan:**
And, I don't know, I think maybe there's an analogy here of something about being averaged with a bunch of people, I guess, it sort of forces you to develop a communication protocol or common language or something that somehow facilitates flow of ideas or whatever. And just directly, because other people have ideas and you average them into you as part of like some kind of average, maybe literal averaging is not right. I'm kind of blabbering on. Does any of that resonate?

**Scott Garrabrant:**
I'm not sure.

**Daniel Filan:**
Okay, so we can move on. Is there anything else that I should have asked?

**Scott Garrabrant:**
Yeah. I mean, I have a large space of thoughts, which I don't even know what exactly I'd say next or something, about how I plan to use factored sets, I think, because I think it actually does differ quite a bit from the use case in the paper slash [video](https://www.youtube.com/watch?v=fvj8fi5azTM) slash whatever blog post. That's one thing that comes to mind. Let me keep thinking for more. Yeah, I guess that's the main thing that comes to mind.

**Daniel Filan:**
Okay, how do you plan to use it?

**Scott Garrabrant:**
I mean, so one piece of my plan is that, I talk a bunch about probability and I don't really plan on working with probability very much in the future. A large part of the thing is I'm kind of pulling out a combinatorial fragment of probability, or combinatorial fragment of independence, so that I can avoid thinking of things with probability or something like that. I don't really talk about probability in Cartesian frames and a lot of the stuff in Cartesian frames I think can, slash, I hope to, port over to factored sets. It's largely, there are lots of places where I'd want to draw a DAG, but I'd want to never mention probability. Or maybe I could mention probability, maybe I could think of things as sometimes being grounded in probability sometimes not, but I want to draw DAGs all over the place and I have this suspicion that, well, maybe places where I'm drawing DAGs, I could instead think in terms of factored sets.

**Scott Garrabrant:**
Although, I have to admit, DAGs are still useful, I have this example where I can infer some time in factored sets that's not the one that I give in the talk. And when I think about it, I have a graph in my head, that's like the Pearlian picture, which maybe has something to do with the fact that you're saying that Pearl can be visualized, but it definitely feels like I haven't fully ported my head over to thinking in factored set world, which seems like a bad sign, but it also doesn't seem like that strong a bad sign, because it's new.

**Daniel Filan:**
Yeah. I mean, graphs do have this nice, somehow if you want to understand dependence, it's just so easy to say this thing depends on this thing, which depends on these three things and it's very nice to draw that as a graph.

**Scott Garrabrant:**
Yeah. I mean, I'm more thinking in terms of screening off. Screening off is a nice picture where you imagine getting in on a path and you kind of block the path and you kind of condition on something on the path and then information can't flow across anymore.

**Daniel Filan:**
Yeah, so a variable screens something off. Yeah, can you just say what screening off means?

**Scott Garrabrant:**
I mean, the way I'm using it here, I'm mostly just saying X screens off Y from Z, if Y and Z are orthogonal, given X, where orthogonal could mean many different things. It could mean graphs, it could mean independence, it could mean the thing in factoring sets.

**Daniel Filan:**
Yeah. Yeah, I guess the idea of paths and graphs, gives you this kind of nice way of thinking about screening off.

**Scott Garrabrant:**
Yeah. So, I do feel like I can't really picture conditional orthogonality as well as I can picture D-separation, even though I can give a definition that's shorter than D-separation that captures a lot of the things.

## Relation to Cartesian frames <a name="cartesian-frames-relation"></a>

**Daniel Filan:**
So yeah, speaking of Cartesian frames, so [Cartesian frames](https://www.alignmentforum.org/posts/BSpdshJWGAW6TuNzZ/introduction-to-cartesian-frames) is I guess, a framework you worked on, as you said last year, and one thing that existed in Cartesian frames was it had this notion of [sub-agency](https://www.alignmentforum.org/posts/nwrkwTd6uKBesYYfx/subagents-of-cartesian-frames) where it was, if you had an agent in an environment, you'd kind of talk about what it meant to view it as like somehow a collection or a composition of sub-agents.

**Daniel Filan:**
So yeah, this question, we're just going to assume that listeners basically get the definition of that and you can skip ahead to the last question if you want to look that up or don't want to bother with that. But I'm wondering, so these finite factored sets, it's kind of easy to see the world being a product of the agent and the environment, that's kind of like this factorization thing. I'm wondering how you think the notion of the sub-agents thing goes into it? Because that was the thing I was kind of... It was kind of the most interesting part of Cartesian frames.

**Scott Garrabrant:**
Yeah, so I think that I actually do have to say something about the definition of sub-agent to answer this, which is in Cartesian frames, I gave multiple definitions of sub-agent and one of them was kind of opaque, but very short. And I kind of mutually justified things as like, "Ah, this is pointing to something you care about, because it agrees with this other definition." But it's really carving it at the joints, because it's so simple.

**Scott Garrabrant:**
So, it shouldn't be clear why this is a sub-agent, but in Cartesian frames, I had a thing that was like C is a sub-agent of D if every morphism from C to ⊥ factors through D. And when you think about ⊥, there's a sense in which you can kind of think of ⊥ as like the world, because ⊥ is the thing where the agent kind of just gets to choose a world and the environment doesn't do anything. And so, you can view this thing as saying every morphism from C to ⊥ factors through D and I think this translates pretty nicely. It's not symmetric in the Cartesian frame thing, but I think it translates pretty nicely to, D screens off C from the world. C is a sub-agent of D, means that D screens off C from the world. In the Cartesian frame thing, it's not a symmetric notion. When I convert to factored sets, it becomes a symmetric notion and maybe there's something lossy there.

**Daniel Filan:**
And by screening off, you mean just conditional orthogonality?

**Scott Garrabrant:**
I mean conditional orthogonality. I'm saying "factoring through", saying a function factors through an object is similar to a screening off notion. And the way that I define sub-agency in factored sets looks like this. So, I can say more about what I say about the world. The world is maybe some high level world model that we care about, so we have some partition of our finite factored set W, which is kind of representing stuff that we care about. And we have some partition that's maybe like, you have some partition D which corresponds to the super-agent and the choices made by the super-agent. And we also have some partition C, which corresponds to the choices made by our sub-agent. And so, you can think of maybe D as a large team and C is like one sub part of that team.

**Scott Garrabrant:**
And if you imagine that the large team has this channel through which it interacts with the world, and that D kind of represents the output of the large team to the world, but then internally, it has some internal discussions, but those internal discussions never kind of leave the team's internal discussion platform or whatever. Then there's a sense in which, if I want to, if I know, if the team is a very tight team and C doesn't really have any interaction with the world besides through the official channels that are D, then if I want to know about the world, once I know about the output of D, learning more about C doesn't really help, which is saying that C is orthogonal to the world, given D.

**Daniel Filan:**
How is that symmetric, because normally, if X is orthogonal to Y given Z, it's not also the case that Y is orthogonal to Z given X, right?

**Scott Garrabrant:**
Sorry, it's symmetric with respect to... You're not replacing the given.

**Daniel Filan:**
Oh, okay.

**Scott Garrabrant:**
By symmetric, it's symmetric with respect to swapping... Yeah, when I said symmetric, obviously I should've meant, it's symmetric with respect to swapping C with W and D is in the middle, which, what does it mean to swap C with W?

**Daniel Filan:**
That's kind of strange.

**Scott Garrabrant:**
It's capturing something about being a sub-agent means that the interface of the super-agent is kind of screening off all of your stuff. And one way to see this is if we weren't working with, I was thinking of this in terms of like W is everything we care about. But if we weren't thinking about W is everything we care about, if we just took any partition X and any other partition Y and we let C be equal to X, and we let D be equal to the common refinement of X and Y, then D screes off C from the world.

**Scott Garrabrant:**
So if you take any two choices, any two partitions, and you just put them together, you get a super-agent under this definition. And you can kind of combine any partitions that are kind of representing some choices or something maybe, and you can combine them and you get super-agents. But that's super-agent with respect to the whole world. And then as you take a more restricted world, now you can have sub-agents that are not just one piece of many pieces, but instead, maybe the sub-agent can have some internal thoughts that don't actually affect the world and are not captured in the super-agent, which is maybe only capturing some more external stuff.

**Daniel Filan:**
Yeah. I guess one thing that comes to mind is that... Yeah, so we have this weird thing where the definition of a sub-agent you could swap out the sub-agent with the rest of the world, because we were thinking of an agent as like a partition, probably not all, just the choice of an agent, maybe, as a partition, but probably not all partitions, not all variables, should get to count as being an agent, right? And I'm wondering if there's some restrictions you could place on what counts as an agent at all, that would break that symmetry?

**Scott Garrabrant:**
Yeah, you could. I don't actually have a good reason to want to here, I think. I think that part of what I'm trying to build up is to not have to make that choice of what counts as an agent and what doesn't. I don't know, I can define this sub-agent thing and I can define things like this partition observes this other partition, so embedded observation, which I haven't explained. And I feel like it's useful that I can give these definitions and they extend to these other partitions that we don't want to think of as agentic either. I actually feel a little more confused in the factored set world about how to define agents than I did before the factored set world, because if I'm trying to define agency my kind of go to thing is agency is time travel. It's this mechanism through which the future affects the past through the agent's modeling and optimization.

**Scott Garrabrant:**
And now I'm like, well, part of the point of factored sets is I was trying to actually understand the real time, such that time travel doesn't make sense as much anymore. And one hope that I have for saving this definition and thinking about what is an agent in the factored set world, is the factored set world leads to multiple different ways of defining time. And so, just like we want to say there's some sort of internal to the agent notion of time, where it feels like the fact that I'm going to eat some food causes me to drive to the store or something. So like in internal to the agent there's some time and then there's also the time of physics. And so, one way you could think of agency is where there's kind of different notions of time that disagree.

**Scott Garrabrant:**
And there's a hope for having a good system of different notions of time in factored sets that comes from the fact that we can just define conditional time, the same way we define conditional orthogonality. We can just imagine taking a factored set, taking some condition, and now we have a new structure of time in the conditioned object and it might disagree. And so, you might be able to say something like agents will tend to have different versions of their time disagree with each other, and this might be able to be made formal. I don't know, this is a vague hope.

## How to follow Scott's work <a name="following-scotts-work"></a>

**Daniel Filan:**
All right, cool. Maybe that gives people ideas for how to extend this or for work to do. So yeah, I guess the final question I would like to ask is, if people have listened to this and they're interested in following you and your work, how should they do so?

**Scott Garrabrant:**
Yeah, so specifically for finite factored sets, everything that I've put out so far is on [LessWrong](https://www.lesswrong.com/users/scott-garrabrant). And so, you could Google some combination of my name, Scott Garrabrant and LessWrong and finite factored sets. And I intend for that to be true in the future. I intend to keep posting stuff on LessWrong related to this and probably related to future stuff that I do. Yeah, I tend to have big chunks of output rarely. Yeah, I haven't posted much on LessWrong since posting Cartesian frames and I'm currently planning on posting a bunch more in the near future related to factored sets and that'll all be on [LessWrong](https://www.lesswrong.com/users/scott-garrabrant).

**Daniel Filan:**
All right, Scott, thanks for being on the podcast and to the listeners. I hope you join us again.

**Scott Garrabrant:**
Thank you.

**Daniel Filan:**
This episode is edited by Finan Adamson. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
