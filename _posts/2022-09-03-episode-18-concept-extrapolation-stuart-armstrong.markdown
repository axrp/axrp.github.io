---
layout: post
title: "18 - Concept Extrapolation with Stuart Armstrong"
date: 2022-09-03 16:00 -0700
categories: episode
---

[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/YmE3NDc2ZmYtMmZlMy00YmEzLThjODQtNTBiYzYwMjQ0NGVj)

Concept extrapolation is the idea of taking concepts an AI has about the world - say, "mass" or "does this picture contain a hot dog" - and extending them sensibly to situations where things are different - like learning that the world works via special relativity, or seeing a picture of a novel sausage-bread combination. For a while, Stuart Armstrong has been thinking about concept extrapolation and how it relates to AI alignment. In this episode, we discuss where his thoughts are at on this topic, what the relationship to AI alignment is, and what the open questions are.

Topics we discuss:
- [What is concept extrapolation](#what-is-concept-extrapolation)
- [When is concept extrapolation possible](#when-is-concept-extrapolation-possible)
- [A toy formalism](#toy-formalism)
- [Uniqueness of extrapolations](#uniqueness-of-extrapolations)
- [Unity of concept extrapolation methods](#unity-of-concept-extrapolation-methods)
- [Concept extrapolation and corrigibility](#concept-extrapolation-and-corrigibility)
- [Is concept extrapolation possible?](#is-concept-extrapolation-possible)
- [Misunderstandings of Stuart's approach](#misunderstandings-of-stuarts-approach)
- [Following Stuart's work](#following-stuarts-work)

**Daniel Filan:**
Hello, everybody. In this episode, I'll be speaking with Stuart Armstrong. Stuart was previously a senior researcher at the Future of Humanity Institute at Oxford, where he worked on AI safety and x-risk, as well as how to spread between galaxies by disassembling the planet Mercury. He's currently the head boffin at Aligned AI, where he works on concept extrapolation, the subject of our discussion. For links to what we're discussing, you can check the description of this episode, and you can read the transcript at axrp.net. Well, Stuart, welcome to the show.

**Stuart Armstrong:**
Thank you.

**Daniel Filan:**
Cool.

**Stuart Armstrong:**
Good to be on.

## What is concept extrapolation <a name="what-is-concept-extrapolation"></a>

**Daniel Filan:**
Yeah, it's nice to have you. So I guess the thing I want to be talking about today is your work and your thoughts on concept extrapolation and model splintering, which I guess you've called it. Can you just tell us: what is concept extrapolation?

**Stuart Armstrong:**
Model splintering is when the features or the concepts on which you built your goals or your reward functions break down. Traditional examples are in physics when (for instance) the ether disappeared. It didn't mean when the ether disappeared that all the previous physics that had been based on ether suddenly became completely wrong. You had to extend the old results into a new framework. You had to find a new framework and you had to extend it. So model splintering is when the model falls apart or the features or concepts fall apart and concept extrapolation is what you do to extend the concept across that divide.

**Daniel Filan:**
Okay.

**Stuart Armstrong:**
Like there was a concept of energy before relativity, and there's a concept of energy after relativity. They're not exactly the same thing, but there's a definite continuity to it.

**Daniel Filan:**
Cool. So you mentioned that at some point we used to think there was ether, and now we think there isn't. What's an example of a concept or something that splintered when we realized there wasn't an ether anymore, just to get a really concrete example.

**Stuart Armstrong:**
Maxwell's equations - Maxwell's non-relativistic equations are based on a non-constant speed of light. Maxwell's equations are not relativistic, though they have a relativistic formulation.

**Daniel Filan:**
Hang on. I thought they were. Isn't that why you get the constant speed of light out of them?

**Stuart Armstrong:**
Okay. If that's uncertain, then let's try another example.

**Daniel Filan:**
We could do energy, once you discovered general relativity, or...

**Stuart Armstrong:**
Energy, inertial mass, for example: those concepts needed a Newtonian universe to make sense, when it wasn't so much the absence of ether, but it was the surprisingly constant speed of light that broke those. So when you measure the speed of light to be the same no matter what your own movement and acceleration is, this breaks a lot of the Newtonian concept.

**Daniel Filan:**
And is the idea that there were potentially many ways you could have generalized them or reformed them to work in a relativistic universe?

**Stuart Armstrong:**
No, not really.

**Daniel Filan:**
Okay.

**Stuart Armstrong:**
This is part of the interesting thing about concept extrapolation: sometimes there's only one real inheritor of the old concept, and sometimes there can be multiple ones. Like temperature - our modern concept of temperature, as the derivative of energy with respect to entropy, for example, is one of the direct descendants of, well, 'it feels hot today', versus 'it feels cold today'. But our neuroscience, qualia, how we feel things is another direct descendant of that old concept.

**Stuart Armstrong:**
Physics might lead us a bit astray because extrapolations tend to be clear and definite and only one, though maybe that might be because of the choice that we've made. There might have been multiple ones that were discarded along the way. But in general, sometimes there's a clear extrapolation, sometimes there are multiple routes you can go down and sometimes there's a mix between the two.

**Daniel Filan:**
And is concept extrapolation the kind of thing that you do when you learn something new about the way the world works, or am I also going to talk about it in the case where I just go to a new place that I haven't been before and things are slightly different there, but it's not like the laws of physics are fundamentally different or anything?

**Stuart Armstrong:**
All of these are on a continuum. If you look at how we've tried to define model splintering, the smallest of changes can be captured in the formalism... You basically have ontology changes at one end and you have 'is a car that is painted a new color still a car' at the other end.

**Daniel Filan:**
And I guess the idea is that you're going to try to think about all of these with one framework?

**Stuart Armstrong:**
Yes. And the car that is painted of a slightly different color is an example of one where there's a trivial single extrapolation - yes, it's a car! And the ontology changes or extreme ontology changes can be ones where it's really not clear what to do.

**Daniel Filan:**
So now that I think we've got a good sense of what concept extrapolation is: how do you see it as relating to AI alignment or preventing doom from AI?

**Stuart Armstrong:**
In a sense, I see it as the very center of it. [Nate Soares](https://www.so8r.es/) has recently published [something](https://www.lesswrong.com/posts/GNhMPAWcfBCASy8e6/a-central-ai-alignment-problem-capabilities-generalization) about the 'left turn' or the 'unexpected left turn' as I believe he's formulated it. The idea being that at some point, it'll become much easier to generalize capabilities than to generalize values, goals, those kind of objects. One way of thinking of capability increases is that this is a changing of (and presumably an improvement of) your world model. So you get a better ability to influence the world as your model of it changes, as you understand different approaches and that sort of thing. Then, if you just change the models there, and your goals are naively defined in the way they were originally defined, then this is going to go hideously wrong. But if the goals change as the world model changes, then you are naturally - well not naturally, *artificially* - solving part of the alignment problem.

**Stuart Armstrong:**
An example that I had was a sort of toy model of wire heading, where you had images of smiling people and non-smiling people, and the smiling people had a big 'happy' written on the image, and the non-smiling ones had 'sad' written on the image. The AI's reward was initially defined on the images, which were given the label of 'happy' or 'sad'; and then it developed the ability to change the text on images that it was producing in whatever way; and this led to non-smiling images with 'happy' written on them and the opposite.

**Stuart Armstrong:**
So this is model splintering. The AI's model of the world previously was that the expression and the text were perfectly correlated, so there's no real need to distinguish between the two. Now its model of the world has changed and it can see that these features are no longer correlated. They can be different. And we've published [a benchmark](https://github.com/alignedai/HappyFaces) recently, and the benchmark essentially is: upon realizing that the model has splintered, that the correlated features are no longer correlated, the AI generates multiple candidates for what its reward information could extrapolate to.

**Stuart Armstrong:**
So in a sense, it has gained capabilities or knowledge about the world. Either way is a good way of modeling what's happened. And now as a consequence, it must extrapolate its reward or generate different candidates to deal with its increase in power or its increase in its world model.

**Daniel Filan:**
So I guess the idea is that: presumably we'd have to be able to get two models out of that, because it's not obvious whether we want to be categorizing things based on the smiley faces or based on the red text at the bottom of the screen, right?

**Stuart Armstrong:**
Yes. The fact that it's only two models is a human judgment of what features are important. And there's actually a rather interesting philosophical computer science insight that has emerged from looking at this. Can I try a very simple example?

**Daniel Filan:**
Yep. Hit us.

**Stuart Armstrong:**
This is double MNIST, one of the simplest possible classification tasks imaginable. You have a zero on top of another zero, that is labeled 'zero'. And you have a one on top of another one and that is labeled one. And humans instinctively see that there are two features here. The digit on top and the digit on the bottom. However, when you zoom into these images, there's actually hundreds of features. There's the upper left curve of the zero, the upper right curve of the zero. Every piece of every digit is its own feature that can be used quite successfully as the feature on which you do the classification task.

**Stuart Armstrong:**
So intrinsically, this data has many, many different features and many, many different extrapolations, but the ones that you consider or that it is useful to consider depend on how the world model of the AI changes, and on the data that it sees. If it sees zero one and one zero, and if it sees a lot of those, then the two useful features are the top digit versus bottom digit. That is as expected. But if it sees things where you get the left half of the top digit and the right half, and those are changed, and that kind of thing happens, then there, you would hone in on different features. So what this means is that the world model or the unlabeled data in this case play the role of telling you which are the possible extrapolations, what breaks, what are the features that break apart and what are the features that don't break apart.

**Stuart Armstrong:**
And I just wanted to say that we started with image classification, not because image classification is particularly relevant to this task, but because we had got to the point where the theory seemed sufficiently 'done' that we needed to get some practical experience and practical results on what extrapolation is and how it can work, to feed back into the theory, ultimately. So that bit that I told you about how the unlabeled data or the world model can determine which are the features that come apart and which are the ones that don't, this is an insight that is constructed from the practical experience of doing the image classification task. We're aiming to extend it to RL and other environments at the moment as well. But yes, the practical and theoretical benefits of sitting down to do this seem to have been borne out - at least, they're starting to bear fruit.

## When is concept extrapolation possible <a name="when-is-concept-extrapolation-possible"></a>

**Daniel Filan:**
And can you give us a sense of the theory that's being developed here? First of all, what does the theory tell you?

**Stuart Armstrong:**
It tells you how to accomplish the task of extrapolating beyond the training data as your world model changes, and how this might be done in a human-safe way. In essence, the core of the alignment problem in our view is always going to be some version of concept extrapolation, in that what we are trying to do is do a safe survivable flourishing world via AI. And none of those concepts are defined with any degree of rigor across the potential futures that we encounter. So how these can be extrapolated is going to be a critical part of it. If we want flourishing, we have to define what flourishing is and get that definition to extend at least adequately across all possible weird futures that the AI could put us in.

**Stuart Armstrong:**
In a sense, there's no theory that says that this is doable and there's no theory that says that it has to fail. What we are as humans is... We exist in a variety of environments, both real and imagined, and we have pieces of values and preferences defined across these environments. And it is not surprising that, say, there's huge contradictions between the different parts of our values when we try to formalize them or extend them. But you can extend a lot of human values in relatively decent ways, more or less complicated depending on your preferences. So it's not like there is a true essence of human value out there that we are trying to get to. If there was, then in a sense concept extrapolation has to succeed or some alignment method has to succeed: in theory, it's just a question of reaching that goal (so it may fail in practice). But since there isn't that, we can't say that alignment has to work in theory. But similarly, we can't say that alignment has to fail in theory, because you might make the argument that if there is this ideal thing out there, and that if we can't reach it, then it has to fail. But since there isn't that, it doesn't have to fail. And we've seen examples of concept extrapolation in physics, in morality, that can be more or less good, but tend to be non-disastrous.

**Stuart Armstrong:**
One of my favorite examples of that is how you can go from the sentiments expressed when it was written 'We hold it self-evident that all men are created equal', and get to the suffragettes movements and votes for women. It seems to be a slight contradiction or huge contradiction between what was written, what was intended, and the ultimate goal, but there is a relatively straight line that you can follow through history and morality that goes from one to the other. So that seems to be a successful extrapolation of a concept of a moral value to a new environment. So it can be done in practice, and it can fail in practice. We also have examples of that. So this seems to mean that we need a lot more experience as to how these things succeed and fail in practice.

**Daniel Filan:**
I guess part of what you're saying there is just related to the fact that to some degree, this is inevitable and the thing you're aligning to, I guess, is also concepts that don't have a super clear extrapolation. I guess I'm wondering: you mentioned some idea of a theory of concept extrapolation. What are the relevant objects in this theory? Can you give us a sense for what the results are in this theory, or what the relevant properties are of things, such that if you have this property, then you can extrapolate well or poorly or something?

**Stuart Armstrong:**
Let me give you an example. It is my strong belief that symbol grounding can be solved in part by concept extrapolation. An example that I give for this is: imagine if you had an AI trained on videos of happy people, and maybe the negative videos of sad people with lower reward. Now, the standard alignment failure mode here is that it fills the universe with videos of happy people. The reason I am saying that symbol grounding may be solvable by concept extrapolation is that even though the wireheaded ideal is the best explanation for what's going on, for the training data - the wireheading is always the best explanation for the training data because it fits perfectly - the hypothesis of, 'well, there are actual humans out in the world, they are sometimes sad and sometimes happy that correspond to these clusters of something or other (reactions, hormones...)'. This is not a theory that is all that complicated to consider if you have a good world model. So just having a practical model of the world would suggest that the symbol grounded version of these are not too hard to find. They're below the wireheaded one in say probability, or simplicity, or fit, but it's not random. Fitting this data to actual humans being happy is a lot easier than fitting this data to stock prices or the movements of the moons of Jupiter and things of that nature.

**Stuart Armstrong:**
This is my strong belief at the moment. I have not proven this result. But this is the kind of thing that would be very valuable to have a formulation of - results both theoretical and experimental. And it would make the alignment problem harder or easier depending on what the result is. Because in one view, at least if we don't change the world too much, you have the wireheading solution, and you can actually rule that out relatively easily. And the next strongest candidate is something that corresponds pretty much to what we want. And this would allow us to define humans and human happiness by 'here's some humans, this is what happiness looks like', rather than constructing very, very complicated definitions of what these are.

**Stuart Armstrong:**
There's essentially a trade off that: if concept extrapolation works fantastically (to a level that I don't expect that it will), you can kind of get good outcomes by just pointing vaguely at good outcomes in the world. Now, however, if it doesn't work quite as well as that, you need to put more training data into it. But in any case, the ultimate aim is that you do it without having perfect training data because you will never have perfect training data.

**Daniel Filan:**
So I guess that's the aim, and there's some sense that you can probably prove things based on which theories of the world make more sense or are easier to handle or something. I guess part of what I'm wondering is: are there existing results or theoretical constructs yet, or is this a work in progress?

**Stuart Armstrong:**
There are theoretical constructs, as in my first attempt of the mathematical formulation of model splintering, which I imported into category theory, which was quite interesting mathematically. Now the problem with it is that it is sufficiently universal, that it is relatively easy to generate toy examples where concept extrapolation succeeds and ones where it fails. The failure mainly being there are too many options and the consequences of bad decisions are too great that it just cannot be solved. And the easy ones are things closer to physics, or 'this is a car of a different color' kind of approaches. Basically, if you want to do a model of the world where concept extrapolation is easy, define fundamental reality with these concepts and then add approximations to it.

**Stuart Armstrong:**
And then the extrapolation will be easy because the extrapolation is already there, in a sense. And it turns out that in physics, the extrapolations are already there in most cases - general relativity and energy being a potential exception. But what is the description of our value concepts, our preferences? Because they're clearly not pieces of a single, well-defined full human utility function, it hasn't been built that way. But they are of a maximal complexity. Take the complexity of all human brains: the estimates of every human being on every potential realistic, short environment that they could encounter that doesn't break their brain - the amount of information in that is finite. And can that be extended in a safe way to extreme capabilities and extreme new environments? I think it can. But how easy is it, how hard it is... This is far more an experimental question than it is a theoretical one, I feel.

**Daniel Filan:**
So would a summary of that be: there's some formalism, but it's not expressive enough to ... or the real gains are in just trying things and seeing what happens? Or checking how reality actually is?

**Stuart Armstrong:**
It's testing how reality actually is. It's also testing how the tools that we use might interact with that reality. When you start extrapolating, there are choices that you can make. You can rely heavily on human feedback: you can try and idealize human feedback along the lines of what humans themselves would consider idealization. You can rule out wireheading solutions and try and give a syntactic definition of what wireheading consists of. You can use the modal solution at each point, in a sense. So that's more self-directed and simplicity-based. You can try low-level human values and extrapolate them and then try and fit them within, say, human meta-preferences. Or you can try the meta-preferences first and extend the low-level preferences to meet them up. I'm mentioning all these because we don't really know which ones will be better. And some of them may break immediately. Some of them may lead to theorems as to the conditions under which they break or don't break, and that would be very useful to have.

## A toy formalism <a name="toy-formalism"></a>

**Daniel Filan:**
So you mentioned some kind of formalism that was maybe connected to category theory, where you could come up with these examples and counter examples. Can you give us a flavor of what that is so that we can get a sense of concretely, what's going on?

**Stuart Armstrong:**
Okay. Bear in mind that I am not sure that this formalism is the ideal one. It may be too general. It has some universality properties. But universality properties in maths are cheap and easy. But the basic idea is that you start with your features and you have probability distributions over the possible values that the features can take, and this defines your world model. And for example, you could have the feature of pressure and temperature and volume, and the ideal gas laws would give you a distribution of how these are related. Then you might move to a different model of, say, atoms bouncing around. And then you can maybe ... In this case, you can at least, statistically, port the ideal gas laws if you cross. But maybe, actually, there are different types of atoms in your new model. And that means that the ideal gas laws are no longer accurate. You have, say, the van der Waals gas laws. So you relate 'this is my description of the universe according to this feature', 'this is my probability distribution', and 'this is my translation of this set of features to this set of features'. And then there is a comparison between what the probability distributions are saying.

**Stuart Armstrong:**
So if you just have one type of atom, you can get the ideal gas laws as a statistical version of the deterministic 'atoms bouncing around' model. If you have different types of atoms, then the two distributions may or may not correspond exactly depending on the different types of atoms, where they're located, and how you analyze them. So when you translate between 'ideal gas laws' and 'atoms bouncing around', you can check whether (say) your more advanced one, the atoms bouncing around, does the probability distribution there give you exactly the probability distribution from the ideal gas laws? In that case, this is just a pure improvement. You've refined your understanding of what's going on. But what'll generally happen is that it's slightly different. You've not only refined your understanding, but you've found errors in your previous understanding. And so the category theory comes in when you move from one universe of potential features to another universe of potential features. And you can port the probability distributions back and forth, and then you can compare what they look like on the other universe. And if they correspond perfectly, that means you've done a pure refinement. You are either gaining knowledge in one direction or you are statistically grouping them together in the other direction, and that forms a category. But there are weaker correspondences where you can learn or approximate or the things don't quite match up, that (say) correspond more to how you went from Newtonian physics to relativity. So, you can have a distance metric for how well they correspond in typical environments.

**Stuart Armstrong:**
So this is a formalism with which you can discuss how model splintering works. I am not yet sure whether it is a useful one. I did it so that I knew that there was a theoretical construct out there that everything could be formulated in terms of, if I needed to. That I wasn't going in an area where the objects fundamentally didn't speak to each other or were of a completely different nature. So now I don't have to fear any ontology change, because any ontology change, including some of the weirdest ones you can do, can be formulated in this formalism. But, as I say, whether it is useful except as a thinking tool, I don't know. This is going to be something that will be determined more experimentally.

## Uniqueness of extrapolations <a name="uniqueness-of-extrapolations"></a>

**Daniel Filan:**
So when we're thinking about concept extrapolation, do you have a sense of what it looks like when there's only one way for a concept to extrapolate? Is there something you can say about what has to be true for there to be essentially a unique or maybe just a preferred extrapolation?

**Stuart Armstrong:**
The easiest way, as I said, to get that is to start with your ultimate concept and have the lower level or the messy ones as just approximations of that ideal one. And that is physics, basically, where it turns out that the underlying model of reality is surprisingly simple. It could have not turned out that way; in which case, the extrapolation across concepts would've been more complicated. We could go back to the ancient Greeks and the conception of the four elements or five elements and how they determined the physics of the time. There is no real extrapolation of what is air and fire from that era to nowadays. It's all 'air is something that goes up and is moist' - or is it dry - it doesn't really make much sense. But the predictions you could make from that - that if you tossed a rock into a pond, it would fall - that extrapolated.

**Daniel Filan:**
Yeah, and I still talk about air and fire. It's not like those concepts have gone away. I think we would agree about, for the most part, what stuff I run into in my every day is air and what stuff is water.

**Stuart Armstrong:**
Maybe. What about earth? And if you are in another planet, is that air?

**Daniel Filan:**
I'm not though, you know?

**Stuart Armstrong:**
Okay, but we're having a discussion here about how these concepts kind of extrapolate, they do in some circumstances, they don't do in others. So the challenge here is that we absolutely want the concepts to extrapolate well because we're using them as not just concepts, but as our goals. So we want it to do well. And what is the definition of 'well', which itself is a judgment based on our values - which again, it has to extrapolate itself. So let's be a bit more practical: fundamental human rights, for example, have turned out to be relatively easily extendable or extrapolatable in the modern world because there is a relatively clear division between what's human and what's not. Where you get into ambiguous areas like fetuses and embryos, that is where you get some of the biggest fighting on them. But if there were increased-intelligence chimpanzees that could cope in modern society, say, or had good language and there was a large community of them and that kind of thing - Not human. Definitely distinct from human. Possibly stupider in many ways, if you wanted to ensure that they were different. But their existence would mean the definition of what is and what, isn't a human, becomes a lot more complicated... No, that's a bad example, but a better example is: what if the Neanderthals were still around in large numbers? That you had a differently intelligent humanoid species. Probably stupider in most ways, would be my guess. But maybe smarter in some ways and different. And there, the concepts of human rights would become more complicated because maybe there are some things that we value that they don't value at all. Or vice versa, especially maybe in the social arenas.

**Stuart Armstrong:**
So a lot of the ease of extending the concept of human rights or the concept of legal equality has been practical - that it turns out that this makes sense, it can be defined. And then you might think, 'Well, here are examples - Neanderthals, intelligent chimpanzees - that undermine it'. Maybe we can either draw the boundary large or we can say, let's not create or not allow the existence of these edge cases that would undermine the central principle. But I fear this is getting a bit away from your question. Legal equality is a powerful concept that is a distillation of a lot of social urges and preferences that humans have. We have innate equality feelings, but they are very... Not necessarily incoherent, but they're very situational. They point in lots of different directions. You pick a story and depending on how you present it, you can make pretty much any type of behavior the one to be desired or avoided on grounds of equality or other similar sentiments. However, we have come up with this concept of legal equality and a lot of inequality that is allowed within legal equality. So this gave us a distillation of a lot of our equality fairness values and left other ones aside. So this is a more or less successful distillation of something that has a lot of weird shards in human values if you zoom into it.

**Stuart Armstrong:**
Basically, what I'm saying is if you tossed away the concept of laws and legal equality and then took the human urges towards equality and fairness and rebuilt something from that, we may not find something that is all that close to what we had initially. It may be that there are other concepts of equality that may have emerged, and there may have been more that you can organize society around. But we have got to legal equality. It is a distillation of a lot of human values and a relatively low information one compared with the amount of information that human preferences to do with fairness have. So it is possible to extrapolate very successfully from lots and lots of very incoherent things towards something that didn't exist before. It wasn't that there was a legal system and a concept of legal equality, and then we got pieces of that, that were implanted into tribal humans, and then we've just rebuilt the thing there. It is a construction, a generalization, an extrapolation, and one that is reasonably close to its origins.

**Daniel Filan:**
Okay. And I guess this is an example of something where there are potentially multiple different successful extrapolations. And if you want to extrapolate, you've got to pick one and go with it?

**Stuart Armstrong:**
Not necessarily. One can always choose to become conservative across the different extrapolations. This is not unusual. A useful, practical feature of humans is that we tend to have diminishing returns. This allows us to combine different preferences in ways that might otherwise be impossible. So what is the value of the Mona Lisa in terms of human lives is an undefined question. I would say that for people who have not sat down to figure it out, it is undefined and there are multiple possible answers for them. But most of those human answers do not lead to a world without art or to a world without living humans in it.

## Unity of concept extrapolation methods <a name="unity-of-concept-extrapolation-methods"></a>

**Daniel Filan:**
So if I think about both that and the [happy face dataset and challenge](https://github.com/alignedai/HappyFaces), it seems like for both of them, we're using this idea of concept extrapolation or model splintering. If I'm thinking how much I want to invest in the concept of concept extrapolation, so to speak, it seems like one thing I'm going to want to know is how universal a phenomenon, at least the relevant fixes might be. So if we're working on this happy face dataset, do you expect that working on concept extrapolation in that setting is going to tell you much about extrapolating the concept of legal equality, for example? And also, do you think for each application, are there going to be new things we need to think about in terms of how concepts extrapolate or will we eventually just solve concept extrapolation for all the cases of splintering we expect to run into?

**Stuart Armstrong:**
We might. This is a practical question rather than a theoretical one. If I already knew the answer to it, we would have a decent theory for how to go about it. There are already some insights that are gained from even the very simplistic image classifier. The example I gave you is of what role the world model or the unlabeled data is doing in disambiguating what the possible extrapolations are. Now, you can connect it with philosophical work and computer science work that has already been done. But this was, to me, at least, an "oh yes, of course, but I'd never thought of it that way." So it has already been a generator of insights; and the fact that we can do this using relatively dull, gradient descent methods - we're getting around to writing up the method and publishing it - but the fact that we can do it using relatively standard gradient descent methods means that it is possible to do this in the standard computer science approach, and it wasn't clear that that would be the case at all.

**Daniel Filan:**
It seems like you must think that there's some degree of generalization for how to think about concept extrapolation - otherwise, you wouldn't form a word for it and work on this image thing, when I gather your heart's desire is not actually to classify happy and sad faces, right?

**Stuart Armstrong:**
Really? Oh. Yes, we started with that because it was easiest, basically. We might have started on a [CoinRun](https://openai.com/blog/quantifying-generalization-in-reinforcement-learning/) example, that was another one that we considered, though we think that the methods here can be used for the CoinRun example. The CoinRun (to explain) was a very simple Mario-ish platform game in which an agent would bounce around and then find the coin all the way on the right, and that would be the victory condition. And if you trained agents on this, [it turned out](https://arxiv.org/abs/2105.14111) that, generically, they didn't learn about the coin. They learned about 'go all the way to the right'. And so if you put the coin somewhere else, they would ignore it and they would go to the right. So they've failed to extrapolate what the goals reasonably could be from the initial data. But we started with image classification just because it turned out to be easier or it seemed to be an easier place to start, with some potential commercial applications, which is relevant to the for-profit and 'getting these methods out into the world' angle of the company.

## Concept extrapolation and corrigibility <a name="concept-extrapolation-and-corrigibility"></a>

**Daniel Filan:**
Speaking of things this might be related to, how much do you see concept extrapolation as related to this idea of corrigibility, where agents are supposed to allow themselves to be amended if they misgeneralize in new environments or if they do something it turns out the creator didn't actually want?

**Stuart Armstrong:**
Oh. I've just understood one of Nate Soares' [points](https://www.lesswrong.com/posts/3pinFH3jerMzAvmza/on-how-various-plans-miss-the-hard-bits-of-the-alignment) because of your question.

**Daniel Filan:**
Oh, excellent. Okay.

**Stuart Armstrong:**
Or, one of the potential confusions there. I think it is both not related at all and very related. The relation is that I think that corrigibility itself is the extrapolation of the concept of the 'well-intentioned assistant'. That is the basic concept that we have examples of for humans, and that we are trying to push to the superhuman AI level. So, if you have a good model of concept extrapolation, then this would allow you to push corrigibility itself upwards. But I don't tend to think of the agents as corrigible as in, they extend their world model and then they change their preferences according to, say, human feedback. It is a lot more that they have a process for how their values should be extended as their environment changes, but this is compatible with Bayesianism. Once you've gone to the end of the process, say once you know everything there is to know about the universe or every possible universe, you can then work backwards from your value system to what information about the world tells you about that value system. So when is human feedback reliable? That is very easy to do if you have the real, in quotes, "Human Value Function." Then human feedback is reliable exactly when it points towards the real Human Value Function, and is unreliable when it points away from it.

**Stuart Armstrong:**
But, you don't want to have that way of thinking from the beginning; but, you want to be compatible with that, as in, you want to behave in a way that is compatible with a Bayesian agent gaining information, because, if you are not... Well, it breaks. There's a lot of theorems on this. This is where things go badly. And I feel that corrigibility, in many ways how it's conceived, is a way to try and avoid the Bayesian full expected utility maximizer approach. You're changing the utility function. You're rooting, tearing out, the previous one and replacing it with a new one that is more adapted to the situation. And most approaches, including [mine](https://www.fhi.ox.ac.uk/wp-content/uploads/2015/03/Armstrong_AAAI_2015_Motivated_Value_Selection.pdf) that I've done with [various indifference methods](https://arxiv.org/abs/1712.06365), are intrinsically non-Bayesian. Whereas concept extrapolation in general is trying to extend it in a way that could be Bayesian in retrospect. So, this is a very convoluted way of saying that I see corrigibility itself as the extrapolation of a specific concept. But, that I see the approach of corrigibility as quite different to what concept extrapolation itself is. Corrigibility seems to be 'change utilities and avoid the badness that that typically comes from doing this' Though, I'm pretty sure that the people who are looking at corrigibility at the advanced level are thinking in a lot more of a Bayesian way. But, I think most people's conception of what corrigibility is is different from what value extrapolation is.

**Daniel Filan:**
All right. But, it seems like the thing you're saying is that corrigibility is an extrapolated version of the concept of deference. But also, a lot of corrigibility approaches involve ripping out some preferences and installing a new one. Whereas, you think of concept extrapolation as more like, "Oh, you're updating something in a way that could be rationalized in some formalism as Bayesian updating." But, you don't necessarily already have the prior or whatever installed. That's what I got from your answer.

**Stuart Armstrong:**
Okay. That is about a 10th of the length of my answer, and much better.

## Is concept extrapolation possible? <a name="is-concept-extrapolation-possible"></a>

**Daniel Filan:**
The next thing I want to ask is, is there anything that you wish I, or other people, would ask you about concept extrapolation and your work on it that I have not yet asked?

**Stuart Armstrong:**
Let's think. The main thing that I think I would like people to get is that a lot of things that people seem to think about concept extrapolation are wrong. So the best questions would be, "Do you believe blah?" Or, "Does concept extrapolation do blah?" To which I could say, "No, not at all."

**Daniel Filan:**
I can try a few of those questions. Here's one that I have. So, the world has a lot of clocks in it, right? So for instance, the Bitcoin blockchain just keeps on getting longer and every day it's longer, or, on the vast, vast majority of days it's longer than it's ever been. And if you know that, there might be some worry that concepts are just always going to be splintering. Because for any concept you ever see, you could imagine extrapolating it, the quote unquote "normal" way, when the Bitcoin blockchain is one block longer. Or, flipping it when the blockchain is one block longer. Does this mean that model splintering is just always constantly happening?

**Stuart Armstrong:**
Yes. Model splintering is constantly happening, and the space of potential concept extrapolations is always huge and growing. The space of useful and acceptable ones is not. So I'm not trying to get every possible extrapolation of human values that could ever exist. We'll get some in the family of adequate, and survivable, ones from the human perspective.

**Daniel Filan:**
Okay. I'm wondering if the Bitcoin blockchain example poses some sort of impossibility result. Right? Because, there's a whole bunch of different ways concepts could extrapolate when you add this new feature. And adding new features to the world, or stuff you haven't previously heard of, can make things... Either it can be totally irrelevant, as in the Bitcoin blockchain case, or it can be extremely relevant - like, we turned the consciousness off on everyone, or something. So I'm wondering if that poses some sort of impossibility result? Where, when you add a new feature, things could either basically not have important differences or have extremely important differences?

**Stuart Armstrong:**
Part of the difficulty is that human judgment is very clear as to what is irrelevant and what isn't irrelevant here. So the examples you've given me, obviously one is a lot more relevant than the other, but we're using our human judgment for that. One thing that you can do is take some diminishing return mix of the possible extrapolations. Generally, this will turn out to be quite acceptable according to most human evaluations. No one really knows what the exact balance between liberty and absence of pain is across the cosmos, but the range... Oh, I have to be careful here, because I'm talking about the range of the possible trade-off between the two. There is a vast amount of different trade-offs you can do, which result in very good worlds...

**Stuart Armstrong:**
Physics is maybe a good example, not necessarily the laws of physics but the 'atoms in this room' kind of physics. You could say, "if I add more atoms, the entropy goes up, the combinatorics explodes." But we can deal with that. So, we can simplify or we can take statistical combinations of different approaches. And being conservative and combining the different values or preferences or possible preferences in a conservative way - take a log of them and sum them, for example - these tend to work, at least roughly. And so no, I don't see the combinatorial explosion of possible concepts as providing all that much of a problem. We can cope with this in the physical world, and there's no reason that we can't cope with it in the moral world.

**Daniel Filan:**
So in the case of conservatively combining different extrapolations of values, it seems like an issue is... So take the Bitcoin blockchain example, and let's say there's some value. Suppose it's something very simple, like the balance in my bank account: I want it to be high. It seems like you could have diametrically opposed different extrapolations. Where one extrapolation is, "I always want it to be high." And another is, "Well, I want it to be high until the blockchain is a certain length and then I actually want it to be low." Then once the blockchain is that length, you can't really conservatively combine those two different extrapolations because there's nothing they agree on as good, right? One's just like a sign-flipped version of the other in this new domain. And it seems like, unless you restrict what sorts of extrapolations you're allowed to throw into the conservative combination, you're going to potentially run into these issues where you can't neatly trade things off in the way you'd want to.

**Stuart Armstrong:**
I want to specify first that conservative combination is more a placeholder than a full method. I'm not saying that this would always work. I'm using it as an example that can be built on. The other argument is if your true preferences are to have the Bitcoin to this point, and then it be as low, this should leave some traces in your preferences, in your behavior... The conservative combination and other more Bayesian approaches don't really work with values that are strictly antagonistic, which is why, say, S-risk utility functions are particularly bad.

**Daniel Filan:**
What are S-risk utility functions?

**Stuart Armstrong:**
Suffering risk utility functions. If there's a utility function that values extreme suffering in a positive way, it is very difficult to combine that with one that wants human flourishing and human happiness. Whereas, if you had a paper clipper utility function, that is easy to combine with one that wants flourishing - you get more human flourishing and you get more paperclips, everybody wins. But when they're antagonistic like that, it is very difficult. But if they are antagonistic in terms of values... Let's take the, "You want your Bitcoin to go up until a certain date and then afterwards to go down." Where is the evidence for that? If nothing changes apart from the date. If you have not gone around saying, "Oh, I want to cash out at this date." Or, "Actually Bitcoin is evil. I want to destroy all my value." Or... If there is-

**Daniel Filan:**
It's just my bank account or...

**Stuart Armstrong:**
Or, your bank account. If there is literally nothing changed, or if there is literally nothing that seems relevant changed, then I say basically ignore the 'grue and bleen' type examples, i.e., the ones where a concept extends and then for some inexplicable reason it flips, in the same way that we do that with empirical problems. If however, there are hints pointing that this might flip there, then we have evidence for it. Then that's a different situation. But in most cases of extrapolation, it is a question of trading off various possible values or extrapolations rather than a question of dealing with, "Maybe it's the exact opposite of what the evidence has suggested so far."

**Daniel Filan:**
So, this would say something like, "Look, if you can value the same features in the same way, even when there's a new feature by default, unless there's some indication that you shouldn't do that."

**Stuart Armstrong:**
There's an example, I believe from [Eliezer Yudkowksy](https://en.wikipedia.org/wiki/Eliezer_Yudkowsky) a few years back, about diamonds: if you want more diamonds, how do you define this across all the future? How you extrapolate the concept of diamonds is (there are many different ways of doing it) but the basic idea is that if you go back to the environment on which people collected diamonds initially, the values there, or the preferences there, should be clear. And if there's no sudden flipping at that point, there's no reason to have a sudden flipping of the extrapolation further on.

**Daniel Filan:**
I guess the next question I have about the 'blockchain length' versus 'consciousness on or off' examples is, it seems like the difference between those things is that one of those features is incredibly important to humans and the other is not. And therefore, you should behave differently with respect to the change in blockchain length. Compared to how you would deal with consciousness turning off.

**Stuart Armstrong:**
If we are thinking of concept extrapolation, a lot of what you're saying makes sense. We are thinking mostly of value extrapolation.

**Daniel Filan:**
But that's not the same thing?

**Stuart Armstrong:**
Value extrapolation is a subset. Value extrapolation is when you are aiming to extrapolate the concepts on which your values, or your reward functions, or your preferences are based. So it means that we don't care about the extrapolation of the concept of blockchain because no one cares, or very few people care, intrinsically about blockchains. So we don't have to extrapolate every single concept. We only are extrapolating things which human values are closely related to.

**Daniel Filan:**
But, you still have to extrapolate how to deal with my bank account balance when the blockchain gets longer. Right?

**Stuart Armstrong:**
That seems like it's mostly an empirical question. I just need to figure out what you want from your bank account balance, or more precisely what you want from your ability to access that. What is the values that drive you to have a certain bank account? And if I can figure those out, then the connection between the bank account and the blockchain, and all that, is just empirical.

**Daniel Filan:**
I guess it's... You might think that it's not totally empirical. So partially because you don't know how those values that relate to how I deal with the bank account change with a longer blockchain. And also, I can't simulate an environment where the Bitcoin blockchain is longer, because it requires solving difficult computational problems. So I can't give examples of what the world would be like if the blockchain were longer to you, at least not fully fleshed out ones.

**Stuart Armstrong:**
Not fully fleshed out ones. But you have used this description, and I understand it perfectly. Well, or, I understand it. I don't see what the blockchain is doing here. We can replace the blockchain with, say, any chaotic process or any complicated process pretty much.

**Stuart Armstrong:**
What the algorithm is fundamentally trying to extrapolate is your values and your preferences. There does not seem to be much evidence that human preferences are tied to complicated objects like the blockchain. And that maybe if they are - maybe it's a noise in the neurons which is relevant to whether we swerve left or right on a particular moral point - then treating it as noise or as stochastic seems to be sufficient.

**Daniel Filan:**
I'm not saying that human values actually depend on the length of the blockchain. I'm just using that as an example of something where you're regularly reaching values that you've never seen before. And if you couldn't handle extrapolating across that, things would be quite bad, and there's one correct extrapolation.

**Stuart Armstrong:**
There's one correct extrapolation?

**Daniel Filan:**
Yep. The correct extrapolation is, "I don't actually care." Like, if I wanted my bank account balance to go up before the blockchain was so long, I still want it to go up.

**Stuart Armstrong:**
Oh. Yes. But I think there are too many things that are entangled in that analogy.

**Daniel Filan:**
For the record, my actual bank account balance is with a normal US dollar bank.

**Stuart Armstrong:**
Could you give us the account number and maybe a sample of your signature and your mother's maiden name on this podcast?

**Daniel Filan:**
Maybe in the bit after the recording, perhaps.

**Stuart Armstrong:**
But the Bitcoin basically goes through a hash function, and the content of the hash also depends on a lot of decisions across the world. If you say that your behavior depends on the length of the blockchain, or on a particular feature of the blockchain, you have defined a relatively simple function defining your preferences.

**Daniel Filan:**
I'm not saying that.

**Stuart Armstrong:**
I know. This is not something where I have a particular answer that I'm working towards. This is where I'm sort of thinking aloud on what you were saying.

**Daniel Filan:**
So for any preference, or for any type of behavior, if you let it branch off the length of a blockchain, you could make it more complicated by doing some complicated thing before it was so long. And then doing some different complicated thing after it was so long. Right?

**Stuart Armstrong:**
It can, but: first of all, you have a complexity cost, but I'm not counting on complexity costs to be the things that save us. But what I am counting more on is the fact that most reasonable accounts of human values, both empirically and theoretically, do not have those kind of features. If you told me that this is what future Stuart would do, and that there was no particular reason for this, I would say, "Okay, this is a failure." But I could imagine a world in which our preferences were more often structured like that. It just seems that it isn't the world that we live in. So part of the analysis of human preferences and meta-preferences, and getting that information into the extrapolation process, would rule out those kind of examples.

**Daniel Filan:**
Yeah, I'm asking how would they rule them out?

**Stuart Armstrong:**
Okay, this goes back to my [research agenda, Synthesizing a Human's Preferences](https://www.alignmentforum.org/posts/CSEdLLEkap2pubjof/research-agenda-v0-9-synthesising-a-human-s-preferences-into). Now, we have the fact that human preferences do not in anyone's judgment

**Stuart Armstrong:**
behave like that: how does this fact get inserted into the extrapolation process? Well, typically there's two ways you could imagine it going. The first one is the explicit way, that all human meta-preferences are included in this extrapolation process and therefore that rules it out directly. But it's not so much that it rules it out as that the human meta-preferences point towards how preferences are supposed to behave, and this thing is not compatible with the directions that they're pointing in.

**Stuart Armstrong:**
But the other way is the human programmer's decisions on part of the thing that they define inside their function. You can rule out a lot of clickbait by saying, okay, if it's a listicle, it's likely to be a clickbait - that's a feature. But you're actually making a value judgment on that. You're making the value judgment that the sort of things that are listicles are clickbait. And the reason there's a value is because clickbait is intrinsically a value definition. It is things that attract people's attention - that's a behavior - but it's not good for them that their attention is attracted - that's a preference. But people do things like 'if it's a list, it's more likely to be clickbait' without realizing the values that they are injecting into the process. So, I expect that some of the choices made when saying 'this is how you extrapolate' may encode a lot of human knowledge about our preferences without necessarily realizing it.

**Daniel Filan:**
But if we're talking about, I don't know, either concept or value extrapolation: I guess in my head, I'm imagining we're building some AI system. It gets to look at the whole world, or at least it has the ability to infer a whole bunch of stuff about the world. So, I wouldn't have thought that 'just ensure that this thing doesn't have access to this random feature like the Bitcoin blockchain length that you don't care about preference-wise' - I would have thought that wouldn't be an option.

**Stuart Armstrong:**
I think you may have misunderstood what I was saying.

**Daniel Filan:**
Yeah. I understood you to be saying something like, if there's some distractor feature like blockchain lengths, that you don't want to cause splintering, then just build your system so that that's not one of the features.

**Stuart Armstrong:**
Yes, but this is at the meta level. If I was to say that a property of extrapolations of human values is that they don't suddenly flip to their opposites for no reason whatsoever, what I was just saying is that ideally this would be a meta-preference that was... Okay, this is something that I've thought about but not formulated extremely clearly. I am hoping that there is a syntactic definition of wireheading, that you can define wireheading in a way that does not require looking into values much. That the concept of capturing the measurement of your own reward - so affecting a simple part of the universe - whereas the actual reward function affects a much larger part of the universe - that this can be defined in a syntactic way, i.e. there might be a formula for it. I give it, say, 40% odds that there's a formula that defines wireheading sufficiently clearly, that we can set it aside. It would be lovely if that was the case.

**Stuart Armstrong:**
If I do stumble upon this formula, I'm going to add it to the definition of value extrapolation, because wireheading is something we don't want. Now, I have used my own value judgment to add a syntactic piece of the definition of the process that makes it more value-aligned with human preferences in general. This is a way that our values can be implicitly encoded. I mean, it would be explicitly, because I'd say that this was that. But if this is the case, this implicit encoding of the values is not inferior to another approach which is, say, more value extrapolation or concept extrapolation-y on the example of wireheading. It would be equally valid or more valid if it worked. So, if I add something along the lines (by hand) of 'don't assume that the values randomly reverse for no reason', then whether this works or not is something we can test, and we can compare it with our other values or outcomes in various situations.

**Daniel Filan:**
But how do you operationalize that? If I'm writing my AI that gets to walk around in the world, how do you write code that says you're allowed to think that values depend on some things but not on the length of the Bitcoin blockchain?

**Stuart Armstrong:**
It's allowed to think anything.

**Daniel Filan:**
Yeah, it's allowed to think stuff, but if I'm programming it, how do I...

**Stuart Armstrong:**
The most obvious way of extrapolating it would be some energy style where you have a variety of different criteria, like simplicity, like 'not randomly reversing itself', like compatibility with a whole load of estimated, extrapolated meta-preferences, and that you would do an energy minimization on that.

**Daniel Filan:**
Okay. But when you say not randomly reversing itself, what do you mean by that in terms of operationalizable concepts?

**Stuart Armstrong:**
Operationalizable concepts, okay. So, if you just take all of the extrapolations, say, complexity-weighted, and became conservative across them, you would tend to get rid of most of these cases, because for the one that starts caring about the opposite of the blockchain, there's the one that starts caring twice as strongly, for example. And there is no particular reason to prefer one over the other. So, a lot of these are just going to get canceled out by that aspect of it.

**Daniel Filan:**
Because you're averaging them or...?

**Stuart Armstrong:**
Yes, because you're averaging them or even with a logarithm normalization. But we do that in... This movement here may have caused a disastrous tsunami in three years time.

**Daniel Filan:**
For listeners' reference, Stuart just waved his arm while holding a mug.

**Stuart Armstrong:**
Or it may have prevented it. We don't consider all these options mainly because there's no reason to think that it goes one way or the other. And when doing extrapolations, similarly, if there is this "odd" behavior, I put odd in quotes but actually it is odd, a lot of the odd behaviors or the pathological behaviors can be defined as, 'Yes, it might go this way, but it might go completely the opposite way as well'. So, if we went back to the model that I was discussing before with the different features and the probability distributions, and you looked specifically at a reward function or candidate reward functions, you could port those back and forth between, say, your simplistic model and your more sophisticated model. And there, you can start having them pay a complexity cost in a way that is pretty close to the way that you can do it for physics.

**Daniel Filan:**
Okay. Just in terms of this averaging out thing, why would that not mean... Say, we're wondering how much I value human happiness and flourishing, tomorrow versus today. Well, one way to extrapolate it would be I evaluate it the same way tomorrow as today. One way to extrapolate it would be, well, I value it the opposite tomorrow as today. There's a minus sign flip. And therefore, if you average those out, I stop valuing things at all. I'm totally neutral about things, right? Because that's the average of plus X and minus X. It's zero, basically. If there's some averaging out, why does it not average out to zero?

**Stuart Armstrong:**
Because especially if you have more than one day - if you have always valued human life roughly the same - then I would argue that the departure of 'suddenly you don't value human life' or 'suddenly you value human life twice as much' are equally valid extensions at that point, and those are the ones that tend to cancel out.

**Daniel Filan:**
I agree those are equally valid and unlikely. I'm wondering, how does that get modeled? How does that show up without someone just saying, 'well, those are two equally valid extrapolations'?

**Stuart Armstrong:**
This again seems to be exactly analogous to the problem of induction in empirical situations.

**Daniel Filan:**
Yeah. For the problem of induction, you can have a prior. So, I guess maybe what we're doing is we're having some kind of prior over potential concepts.

**Stuart Armstrong:**
There's two places that I would appeal to get that prior. The first is complexity, which helps here. Though, as I say, I don't want to rely too much on complexity. Then there's a stronger version of complexity, which is all the different pieces of human values. None of them really exhibit this kind of behavior, so it would be even more surprising if just this aspect did.

**Daniel Filan:**
Exhibit what behavior?

**Stuart Armstrong:**
The suddenly inverting itself for no reason.

**Daniel Filan:**
Well, because the blockchain got longer. Not for no reason.

**Stuart Armstrong:**
The other thing is the human meta-preferences where, if we had a discussion about what we value and you would ask people would they prefer to suddenly completely value the opposite when the blockchain gets longer, for example, people would tend to say no. Now, I know that you can't go from empirical observations to values. [I wrote a paper on that](https://arxiv.org/abs/1712.05812). But given some assumptions about how human statements connect with human values and from which you build the rest, this then becomes evidence that we don't want our preferences to exhibit this kind of behavior. So, this is a value-related reason for the extrapolation of these lower-level values to not exhibit that behavior.

## Misunderstandings of Stuart's approach <a name="misunderstandings-of-stuarts-approach"></a>

**Daniel Filan:**
All right. So, I guess we could talk about that much longer, but our time is limited. Earlier you said there were basically misunderstandings that people had that you wanted to clear up. Are there any more of those that you'd like to talk about?

**Stuart Armstrong:**
Yes. Part of it is that a lot of people seem to think that we're relying on human feedback to solve the ambiguity between different possible extrapolations. It's a method we use now with much dumber AIs than humans, but this is ultimately not going to work. There are a variety of things that can be done. One is to generalize human feedback in an idealized way, so itself a form of extrapolation. You can become conservative. There are various ways for choosing how you extend the concept or the reward, and we're going to be analyzing them. But 'ask the human' is more of a practical stopgap for current systems. It's not a big aim of the approach. And in fact, in a sense, we feel that the biggest challenge, or one of the bigger challenges here is not choosing the right extrapolation, but having the right extrapolation as a candidate. So, just to go back to the early example, if we have an AI that is hesitating between 'do we want videos of happy humans or do we want actual happy humans', if we've got to that, well, then fantastic. Either we can let it become conservative between the two, or we can tell it that it's the second. Most of the battle is already won at that point, which is why so much of our early focus is getting the extrapolations, or getting the good extrapolations, rather than choosing amongst them.

**Daniel Filan:**
Although even in that case, it seems like you have to filter out the crazy extrapolations, right?

**Stuart Armstrong:**
Oh yeah, this is a simplified outcome with just those two. A lot of people are focusing on how do you choose from amongst these extrapolations. In practice in a lot of ML, machine learning stuff, they don't generate any candidates except for one, or ensemble methods generate a few more. Bayesian methods generate a huge amount, but they already have a prior ahead of time, which is a bit of a cheat in this case. But yes, let's start by getting some reasonable candidates before we worry too much about the process of selecting amongst them. And 'ask a human' is just the current stopgap for the current practical problems.

**Daniel Filan:**
So, that's one misunderstanding people have. They think that you're focusing on choosing between extrapolations.

**Stuart Armstrong:**
Yes. The other one is that we think that we can build up from solving image classification to solving AI alignment. In a sense, it is the opposite. We have seen the advanced form of value extrapolation, concept extrapolation as the way that we plan to solve AI alignment. And we're using image classification as a toy version of this to understand how this approach works. What are the features? What is the theory that we should be building here? And so, it's not so much a question of building up as applying the ideas to the simplest non-trivial problem that we can apply it to.

**Daniel Filan:**
Yeah, I guess there the worry would be - I think if I inhabit that critique or that misunderstanding, I would think, well, okay, you're trying it on this really simple example, and then you're learning some things and making some tweaks. Well, maybe if you're trying to align some crazy AGI, that will also have some new tweaks or new things to learn that weren't already contained in the image classification case.

**Stuart Armstrong:**
Oh, I see the point. Yes, the learning approach that we have on image classification is not the approach of how you would align a dangerous superintelligence. It's that we can tweak and we can experiment and we can play around with it precisely because it's not an AGI that we're dealing with. This is, in a sense, practical theory building, if that makes sense. If we have a system to deploy on an AGI of great power, then it would not be because, 'Well, it works with images so it'll probably work with this AGI'. It would be because our work with images and other things have led to the development of a theoretical framework that we think is sufficiently solid, that it can work with an AGI.

**Daniel Filan:**
So the idea is you have some proto-theory, you play with image classifiers, you come up with a theory. You think about, 'Okay, is this theory solid? Do I think this theory covers what it has to cover?' And then, you use it on an AGI?

**Stuart Armstrong:**
Mm-hmm. And toy examples have been very useful in both philosophy and in physics in the past. Relativity came from ideas of 'what if you were on a cannonball that was falling through the earth' and that kind of thing. You can get a lot from toy examples when it comes to building theory. In fact, a lot of the examples and counterexamples in alignment at the moment are essentially toy examples.

## Following Stuart's work <a name="following-stuarts-work"></a>

**Daniel Filan:**
So, I guess my final question is: if people are interested in following your work and more stuff about concept extrapolation or whatever else you might work on, how should they do so?

**Stuart Armstrong:**
The two easiest ways are to go to our website, [buildaligned.ai](https://www.aligned-ai.com/), and sign up for a periodic newsletter, or look at [LessWrong](https://www.lesswrong.com/) or [the Alignment Forum](https://www.alignmentforum.org/) to see some of the things that we post there. Currently, there's also [a benchmark and a challenge](https://github.com/alignedai/HappyFaces) which is, can you disambiguate features better than what we've done? And if you can, I would love you, and please get in contact about that.

**Daniel Filan:**
Okay. And how can people find you on LessWrong or the Alignment Forum?

**Stuart Armstrong:**
[Stuart Armstrong](https://www.alignmentforum.org/users/stuart_armstrong) is the username.

**Daniel Filan:**
Okay, great. Well, thanks for coming on the show.

**Stuart Armstrong:**
Thank you for inviting me here. It's been interesting, and I think I have developed some of the ideas in the course of this conversation.

**Daniel Filan:**
Excellent. To the listeners. I hope it was interesting for you also.

**Daniel Filan:**
This episode is edited by Jack Garrett, and Amber Dawn Ace helped with transcription. The opening and closing themes are also by Jack Garrett. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
