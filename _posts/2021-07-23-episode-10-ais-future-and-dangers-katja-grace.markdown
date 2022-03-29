---
layout: post
title: "10 - AI's Future and Impacts with Katja Grace"
date: 2021-07-23 15:00 -0700
categories: episode
---

[Google Podcasts link](https://podcasts.google.com/feed/aHR0cHM6Ly9heHJwb2RjYXN0LmxpYnN5bi5jb20vcnNz/episode/MDQ5OGFmYzgtMWUzNi00NTVkLWI3NjAtYWQ4ZmEyMDRkNTNh)

When going about trying to ensure that AI does not cause an existential catastrophe, it's likely important to understand how AI will develop in the future, and why exactly it might or might not cause such a catastrophe. In this episode, I interview Katja Grace, researcher at AI Impacts, who's done work surveying AI researchers about when they expect superhuman AI to be reached, collecting data about how rapidly AI tends to progress, and thinking about the weak points in arguments that AI could be catastrophic for humanity.

Topics we discuss:
- [AI Impacts and its research](#about-ai-impacts)
- [How to forecast the future of AI](#forecasting-ai)
- [Results of surveying AI researchers](#survey-results)
- [Work related to forecasting AI takeoff speeds](#takeoff-speeds)
  - [How long it takes AI to cross the human skill range](#skill-range)
  - [How often technologies have discontinuous progress](#discontinuous-progress)
  - [Arguments for and against fast takeoff of AI](#fast-takeoff-arguments)
- [Coherence arguments](#coherence-arguments)
- [Arguments that AI might cause existential catastrophe, and counter-arguments](#catastrophe-arguments)
  - [The size of the super-human range of intelligence](#super-human-intelligence-range)
  - [The dangers of agentic AI](#agentic-ai-dangers)
  - [The difficulty of human-compatible goals](#difficulty-human-compatible-goals)
  - [The possibility of AI destroying everything](#possibility-ai-destroying-everything)
- [The future of AI Impacts](#future-ai-impacts)
- [AI Impacts vs academia](#ai-impacts-vs-academia)
- [What AI x-risk researchers do wrong](#x-risk-researcher-mistakes)
- [How to follow Katja's and AI Impacts' work](#following-katja-ai-impacts)

**Daniel Filan:**
Hello everybody. Today, I'll be speaking with Katja Grace, who runs [AI Impacts](https://aiimpacts.org/), a project that tries to document considerations and empirical evidence that bears on the longterm impacts of sophisticated artificial intelligence. We'll be talking about her paper, ["When Will AI Exceed Human Performance? Evidence From AI Experts](https://arxiv.org/abs/1705.08807), coauthored with John Salvatier, Allan Dafoe, Baobao Zhang, and Owain Evans, as well as AI Impacts' other work on what the development of human level AI might look like and what might happen afterwards. For links to what we're discussing you can check the description of this episode, and you can read a transcript at [axrp.net](https://axrp.net/). Katja, welcome to AXRP.

**Katja Grace:**
Hey, thank you.

## AI Impacts and its research <a name="about-ai-impacts"></a>

**Daniel Filan:**
Right. So I guess my first question is, can you tell us a bit more about what AI Impacts is and what it does?

**Katja Grace:**
Yeah, I guess there are basically two things it does where one of them is do research on these questions that we're interested in, and the other is try to organize what we know about them in an accessible way. So the questions we're interested in are these high level ones about what will the future of, well humanity, but involving artificial intelligence in particular, look like? Will there be an intelligence explosion? Is there a huge risk of humanity going extinct? That sort of thing. But also those are questions we know about. It seems like there's a vaguer question of, what does the details of this look like? Are there interesting details where if we knew exactly what it would look like we'd be like, "Oh, we should do this thing ahead of time." Or something.

**Katja Grace:**
We're interested in those questions. They're often pretty hard to attack head-on. So what we do is try to break them down into lower level sub-questions and then break those down into the lower level sub-questions again until we get to questions that we can actually attack, and then answer those questions. So often we end up answering questions like, [for cotton gin technology were there fairly good cotton gins just before Eli Whitney's cotton gin](https://aiimpacts.org/effect-of-eli-whitneys-cotton-gin-on-historic-trends-in-cotton-ginning/) or something. So it's very all over the place as far as subject matter goes, but all ultimately hoping to bear on these other questions about the future of AI.

**Daniel Filan:**
And how do you see it fitting in to the AI existential risk research field? If I'm someone else in this research field, how should I interact with AI Impacts outputs?

**Katja Grace:**
I think having better answers to the high-level questions I think should often influence what other people are doing about the risks. I think that there are a lot of efforts going into avoiding existential risk and broadly better understanding what we're up against seems like it might change which projects are a good idea. It seems like we can see that for the questions that are most clear, like is there likely to be a fast intelligence explosion? Well, if there is, then maybe we need to prepare ahead of time for anything that we would want to happen after that. Whereas if it's a more slow going thing that would be different.

**Katja Grace:**
But I think there's also just an intuition that if you're heading into some kind of danger and you just don't really know what it looks like at all, it's often better to just be able to see it better. I think the lower level things that we answer, I think it's harder for other people to make use of except to the extent that they are themselves trying to answer the higher level questions and putting enough time into that to be able to make use of some data about cotton gins.

**Daniel Filan:**
I guess there's this difference between the higher level and the lower level things with the higher level of things perhaps being more specific, or more relevant rather, but in terms of your output, if somebody could read a couple things on the website or just a few outputs, I'm wondering specifically what work do you think is most relevant for people trying to ensure that there's no existential catastrophe caused by AI?

**Katja Grace:**
I think the bits where we've got closest to saying something at a higher level, and so more relevant, is we have [a page of arguments for thinking that there might be fast progress in AI at around the point of human level AI](https://aiimpacts.org/likelihood-of-discontinuous-progress-around-the-development-of-agi/). Sort of distinct from an intelligence explosion, I think often there's a thought that you might just get a big jump in progress at some point, or you might make a discovery and go from a world similar to the world we have now to one where fairly impressive AI is either starting an intelligence explosion at that point, or just doing really crazy things that we weren't prepared for.

**Katja Grace:**
So we tried to make a list of all the arguments we've heard around about that and the counter arguments we can think of. I think that's one that other people have mentioned being particularly useful for them. I guess, relatedly, we have [this big project on discontinuous progress in history](https://aiimpacts.org/discontinuous-progress-investigation/), so that's feeding into that, but I guess separate to the list of arguments we also have, just historically when have technologies seen big jumps? What kinds of situations cause that? How often they happen, that sort of thing.

**Daniel Filan:**
Well we will be talking about that later, so I'm glad it's relevant. So it seems like you have a pretty broad remit of things that are potentially items that AI Impacts could work on. How do you, or how does AI Impacts choose how to prioritize within that and which sub questions to answer?

**Katja Grace:**
I think at the moment the way we do it is we basically just have a big spreadsheet of possible projects that we've ever thought of that seemed like they would bear on any of these things. And I guess the last time we went to a serious effort to choose projects from this list, what we did was first everyone put numbers on things for how much they liked them, intuitively taking into account how useful they thought they would be for informing anything else and how well equipped to carry them out they thought we were, or they were in particular. And then we had a sort of debate and we looked at ones where someone was excited and other people weren't or something like that and discussed them, which was quite good I think.

**Katja Grace:**
I would like to do more of that. I think it was pretty informative. And in the end we're basically going with an intuitive combination of this is important, we think it's tractable for someone currently at the org to do it well. And maybe some amount of either we have some obligation to someone else to do it, or we know that someone else would like us to do it. Sometimes people give us funding for something.

**Daniel Filan:**
What do you think the main features are that predict your judgments of importance of one of these sub-questions?

**Katja Grace:**
I think how much it might change our considered views on the higher level questions.

**Daniel Filan:**
If you had to extract non-epistemic features. So an epistemic feature, it might be that this would change my opinions, and a non-epistemic feature, it might be that this involves wheat in some capacity.

**Katja Grace:**
My sense is that projects are better when they're adding something empirical to the community's knowledge. I think you can do projects that are sort of more vague and what are the considerations about this? And I think that can be helpful. I think it's harder for it to be helpful or especially if it's, and what do I think the answer is to this given all the things? Because I think it's harder for someone else to trust that they would agree with your opinions about it. Whereas if you're like, "Look, this thing did happen with cotton gins and other people didn't know that and it's relevant." Then however it is that they would more philosophically take things into account, they have this additional thing to take into account.

**Daniel Filan:**
I guess that's kind of interesting - so you said that the thing that is most relevant perhaps for outsiders is [this big document about why we might or might not expect fast takeoff](https://aiimpacts.org/likelihood-of-discontinuous-progress-around-the-development-of-agi/). And yeah, it's mostly a list of arguments and counter arguments. I'm wondering what do you think about that relationship?

**Katja Grace:**
I think considering that as a project I wouldn't class it with the unpromising category of more vague, open-ended, non-empirical things. So I guess my thing is not empirical versus not. I think the way that these arguments seem similar to an empirical thing is they're in some sense a new thing that the other people reading it might not have known and now they have it and can do what they want with it. It's somehow like you've added a concrete thing that's sort of modular and people can take away and do something with it rather than the output being somehow more nebulous, I guess.

**Katja Grace:**
I guess the thing that I try to do with AI Impacts at least that I think is less common around in similar research is the pages are organized so that there's always some sort of takeaway at the top, and then the rest of the page is just meant to be supporting that takeaway, but it should be that you can basically read the takeaway and do what you want with that, and then read the rest of the page if you're curious about it.

## How to forecast the future of AI <a name="forecasting-ai"></a>

**Daniel Filan:**
So when you're trying to figure out what the future of AI holds, one thing you could do is you could think a lot about progress within AI or facts about the artificial intelligence research field. And another thing you could do is you could try to think about [how economic growth has accelerated over time](https://aiimpacts.org/precedents-for-economic-n-year-doubling-before-4n-year-doubling/), or [how efficient animals are at flying](https://aiimpacts.org/are-human-engineered-flight-designs-better-or-worse-than-natural-ones/), or [progress in cotton gin technology](https://aiimpacts.org/effect-of-eli-whitneys-cotton-gin-on-historic-trends-in-cotton-ginning/). I'm wondering, these feel like different types of considerations one might bring to bear, and I'm wondering how you think we should weigh these different types?

**Katja Grace:**
I guess thinking about how to weigh them seems like a funny way to put it. I guess there's some sort of structured reason that you're looking at either of them. There was some argument for thinking that AI would be fast, it would be dangerous or something. And I guess in looking at that argument, if you're like, "Oh, is this part of it correct?" Then like in order to say, "Is it correct?" There are different empirical things you could look at. So if a claim was, "AI is going to go very fast because it's already going very fast." Then it seems like the correct thing to do is to look at is it already going very fast? What is very fast? Or something. Whereas if the claim is it's likely that this thing will see a huge jump, then it seems like a natural way to check that is to be like, "All right, do things see huge jumps? Is there some way that AI is different from the other things that would make it more jumpy?"

**Katja Grace:**
So there, I think it would make sense to look at things in general and look at AI and the reason to look at things in general, as well as AI, I guess maybe there are multiple reasons. One of them would just be there's a lot more of other things. Especially if you're just getting started in AI, in the relevant kind of AI or something, it's nice to look at broader things as well. But there I agree that looking at AI is pretty good.

**Daniel Filan:**
I guess maybe what I want to get at is I think a lot of people when trying to think about the future of AI, a lot of people are particularly focused on details about AI technology, or their sense of what will and won't work, or some theory about machine learning or something, whereas it seems to me that AI Impacts does a lot more of this latter kind of thing where you're comparing to every other technology, or thinking about economic growth more broadly. Do you think that the people who are not doing that are making some kind of mistake? And what do you think the implicit, I guess disagreement is? Or what do you think they're getting wrong?

**Katja Grace:**
I think it's not clear to me. It might be a combination of factors where one thing where I think maybe I'm more right is just, I think maybe people don't have a tendency when they talk about abstract things to check them empirically. I guess a case where I feel pretty good about my position is, well maybe this isn't AI versus looking at other things, it's more like abstract arguments versus looking at a mixture of AI and other things. But it seems like people say things like probably AI progress goes for a while and then it reaches mediocre human level, and then it's rapidly superhuman. And that also this is what we see in other narrow things. As far as I can tell, it's not what we see in other narrow things. You can look at lots of narrow things and see that it often takes decades to get through the human range. And I haven't systematically looked at all of them or something, but at a glance it looks like it's at least quite common for it to take decades.

**Katja Grace:**
Chess is a good example where as soon as we started making AI to play chess it was roughly at amateur level and then it took, I don't know, 50 years or something to get to superhuman level, and it was sort of a gradual progress to there. And so as far as I can tell people are still saying that progress tends to jump quickly from subhuman to superhuman and I'm not quite sure why that is, but checking what actually happens seems better. I don't know what the counter argument would be. But I think there's another thing which is, I'm not an AI expert in the sense that my background is not in machine learning or something. I mean I know a bit about it, but my background is in human ecology and philosophy. So I'm less well equipped to do the very AI intensive type projects or to oversee other people doing them. So I think that's one reason that we tend to do other more historical things too.

## Results of surveying AI researchers <a name="survey-results"></a>

**Daniel Filan:**
So now I'd like to talk a little bit about this paper that you were the first author on. It's called, [When Will AI Exceed Human Performance? Evidence From AI Experts](https://arxiv.org/abs/1705.08807). It came out in 2017 and I believe it was something like the 16th most discussed paper of that year according to some metric.

**Katja Grace:**
Yeah. [Altmetric](https://www.altmetric.com/details/20475804/#score).

**Daniel Filan:**
So yeah, that's kind of exciting. Could you tell us a bit about what's up with this? Why this survey came into existence and what it is and how you did it? Let's start with the what. What is this survey and paper?

**Katja Grace:**
Well the survey is a survey of, I think we wrote to - I think it was everyone who presented at [NIPS](https://nips.cc/Conferences/2015) and [ICML](https://icml.cc/2015/) in 2015 we wrote to, so I believe it would have been all the authors, but I can't actually remember, but I think our protocol is just to look at the papers and take the email addresses from them. So we wrote to them and asked them a bunch of questions about when we would see human level AI described in various different ways and using various framings to see if they would answer in different ways, like how good or bad they thought this would be, whether they thought there would be some sort of very fast progress or progress in other technologies, what they thought about safety, what they thought about how much progress was being driven by hardware versus other stuff. Probably some other questions that I'm forgetting.

**Katja Grace:**
We sort of randomized it so they only got a few of the questions each so that it didn't take forever. So the more important questions lots of people saw and the ones that were more like one of us thought it would be a cool question or something fewer people saw. And I guess we got maybe - I think it was a 20% response rate or something in the end? We tried to make it so they wouldn't see what the thing was about before they started doing it, just in terms that it was safety and long-term future oriented to try and make it less biased.

**Daniel Filan:**
And why do this?

**Katja Grace:**
I think knowing what machine learning experts think about these questions seemed pretty good I think both for informing people, informing ourselves about what people actually working in this field think about it, but also I think creating common knowledge. If we publish this and they see that yeah, a lot of other machine learning researchers think that pretty wild things happening as a result of AI pretty soon is likely, that that might be helpful. I think another way that it's been helpful is often people want to say something like this isn't a crazy idea for instance. Lots of machine learning researchers believe it. And it's helpful if you can point to a survey of lots of them instead of pointing to [Scott Alexander's blog post](https://www.lesswrong.com/posts/LTtNXM9shNM9AC2mp/superintelligence-faq) that several of them listed or something.

**Daniel Filan:**
So there's a paper about the survey and I believe [AI Impacts also has a webpage about it](https://aiimpacts.org/2016-expert-survey-on-progress-in-ai/). And one thing that struck me reading the webpage was first of all, there was a lot of disagreement about when human level AI might materialize. But it's also the case that a lot of people thought that basically most people agreed with them, which is kind of interesting and suggests some utility of correcting that misconception.

**Katja Grace:**
Yeah. I agree that was interesting. So I guess maybe the survey didn't create so much common knowledge, or maybe it was more like common knowledge that we don't agree or something. Yeah. And I guess I don't know if anyone's attempted to resolve that much beyond us publishing this and maybe the people who were in it seeing it.

**Daniel Filan:**
The first question I think people want to know is, when are we going to get human level AI? So when did survey respondents think that we might get human level AI?

**Katja Grace:**
Well, they thought very different things depending on how you asked them. I think asking them different ways, which seemed like they should be basically equivalent to me just get extremely different answers. Two of the ways that we asked them, we asked them about high-level machine intelligence, which is sort of the closest thing to the question that people have asked in previous surveys, which was something like when will unaided machines be able to accomplish every task better and more cheaply than human workers?

**Katja Grace:**
But we asked them this in two ways, one of them was for such and such a year, what is the probability you would put in happening by that year? And the other one was for such and such a probability in what year would you expect it? So then we kind of combine those two for the headline answer on this question by sort of making up curves for each person and then combining all of those. So doing all of that and the answers that we got from combining those questions were a 50% chance of AI outperforming humans in all tasks in 45 years. But if you ask them instead about when all occupations will be automated, then it's like 120 years.

**Daniel Filan:**
I think these are years from 2016?

**Katja Grace:**
That's right. Yeah.

**Daniel Filan:**
Okay. So subtract five years for listeners listening in 2021. So there's this difference between the framing of whether you're talking about tasks that people might do for money versus old tasks. You also mentioned there was a difference in whether you asked probability by a certain year versus year at which it's that probability that the thing will be automated. What was the difference? What kind of different estimates did you get from the different framings?

**Katja Grace:**
I think that one was interesting because we actually tried it out beforehand on MTurkers and also we did it for all of the different narrow task questions that we asked. So we asked about a lot of things, like when will AI be able to fit Lego blocks together in some way or something. And this was a pretty consistent effect across everything. And I think it was much smaller than the other one. I think it usually came out at something like a decade of difference.

**Daniel Filan:**
Does anyone know why that happens?

**Katja Grace:**
I don't think so.

**Daniel Filan:**
Which one got the sooner answers?

**Katja Grace:**
If you say, "When will there be a 50% chance?" That gets the sooner answer than if you say, "What is the chance in 2030?" or something. My own guess about this is that people would like to just put low probabilities on things. So if you say, "What is the chance in any particular year?" They'll give you a low probability. Whereas if you say, "When will there be a 90% chance?" They have to give you a year.

**Daniel Filan:**
And they don't feel an urge to give large numbers of years as answers.

**Katja Grace:**
Also somehow it seems unintuitive or weird to say the year 3000 or something. That's just speculation though. I'd be pretty curious to know whether this is just a tendency across lots of different kinds of questions aren't AI related. It seems like probably since it was across a lot of different questions here.

**Daniel Filan:**
Yeah. You have these big differences in framing. You also have differences in whether the respondents grew up in North America or Asia right?

**Katja Grace:**
I can't remember if it was where they grew up, but yeah. Roughly whether they're...

**Daniel Filan:**
It was where their undergraduate institution was I think.

**Katja Grace:**
Yes. It was like a 44 year difference. Where for those in Asia it was like 30 years. Asking about each HLMI again, for [those in North America] I guess it was 74.

**Daniel Filan:**
Here as well do you have any guesses as to why there are these very large differences between groups of people? It's pretty common for people to have their undergraduate in one continent and go to a different continent to study. So it's not as though word hasn't reached various places. Do you have any idea what's going on?

**Katja Grace:**
I think I don't. I would have some temptation to say, "Well, a lot of these views are more informed by cultural things than a lot of data about the thing that they're trying to estimate." I think to the extent that you don't have very much contact with the thing you're trying to estimate, maybe it's more likely to be cultural. But yeah, I think as you say, there's a fair amount of mixing.

**Katja Grace:**
Yeah. I think another thing that makes me think that it's cultural is just that opinions were so all over the place anyway, I think. I guess I can't remember within the different groups, how all over the place they were, but I think I would be surprised if it wasn't that both cultural groups contain people who think it's happening very soon and people who think it's happening quite a lot later. So it's not like people are kind of following other people's opinions a great deal.

**Daniel Filan:**
So given that there are these large framing differences and these large differences based on the continent of people's undergraduate institutions, should we pay any attention to these results?

**Katja Grace:**
I mean, I do think that those things should undermine our confidence in them quite a lot. I guess things can be very noisy and still some good evidence if you kind of average them all together or something. If we think that we've asked with enough different framings sort of varying around some kind of mean, maybe that's still helpful.

**Daniel Filan:**
So perhaps we can be confident that high-level machine intelligence isn't literally impossible.

**Katja Grace:**
There are some things that everyone kind of agrees on like that, or maybe not everyone, but a lot of people. I think also there are more specific things about it. You might think that if it was five years away it would be surprising if almost everyone thought it was much further away. You might think that if it's far away people probably don't have much idea how many decades it is, but if it was really upon us they would probably figure it out, or more of them would notice it. I'm more inclined to listen to other answers in the survey. I guess I feel better about asking people things they're actually experts in. So to the extent can ask them about the things that they know about, and then turn that into an answer about something else that you want to know about. That sort of seems better.

**Katja Grace:**
So I guess we did try to do that with human level AI in a third set of questions for asking about that, which were taken from [Robin Hanson's previous idea of asking how much progress you had seen in your field, or your subfield during however many years you've been in your subfield, then using that to extrapolate how many years it should take to get to 100% of the way to human level performance in the subfield](https://www.overcomingbias.com/2012/08/ai-progress-estimate.html). That's the question. For that kind of extrapolation to work you need for there not to be acceleration or deceleration too much. And I think in Robin's sort of informal survey a few years earlier there wasn't so much, whereas in our survey a lot of people had seen acceleration perhaps just because the field had changed over those few years. So harder to make clear estimates from that.

**Daniel Filan:**
I'm wondering why that result didn't make it into the main paper because it seems like relatively compelling evidence, right?

**Katja Grace:**
I think maybe people vary in how compelling they find it. I think also the complication with acceleration maybe it makes it hard to say much about. It seems like you can still use it as a bound. If you did this extrapolation it led to numbers much closer than the ones Robin had got. I think part of the reason we included it, or part of the reason it seemed interesting to me at least was it seemed like it led to very different numbers. If you ask people, "When will it be HLMI?" They're like, "40 years." Or something. Whereas if you say, "How much progress have you seen in however many years?" And then extrapolate, then I think it was ending up hundreds of years in the future in Robin's little survey. So I was like, "Huh, that's interesting." But I think here it sort of came out reasonably close to the answer for just asking about HLMI directly. Though if you ignored everyone who hadn't been in the field for very long, then it still came out hundreds of years in the future if I recall.

**Daniel Filan:**
Yeah. I'm wondering why you think - because it seemed like there was a bigger seniority bias, or a seniority difference in terms of responses in that one. And I'm wondering, what do you think was going on there?

**Katja Grace:**
I don't know. I guess-

**Daniel Filan:**
Acceleration could explain it, right? If progress had been really fast recently then the progress per year for people who have not been there very long would be very high, but progress per year for people who've been there for ages would be low.

**Katja Grace:**
In the abstract that does sound pretty plausible. I have to check the actual numbers to see whether that would explain the particular numbers we had. But yeah, I don't have a better answer than that. I could also imagine more psychological kinds of things where the first year you spend in an exciting field it feels like you're making a lot of progress, whereas after you've spent 20 years in an exciting seeming field, maybe you're like, "Maybe this takes longer than I thought."

**Daniel Filan:**
So is it typically the case that people are relatively calibrated about progress in their own field of research? How surprised should we be if AI researchers are not actually very good at forecasting how soon human level machine intelligence will come?

**Katja Grace:**
I don't know about other experts and how good they are forecasting things. My guess is not very good just based on my general sense of people's ability to forecast things by default in combination with things like ability to forecast how long your own projects will take, I think it was clearly not good. Speaking for myself, I make predictions in a spreadsheet and I label them as either to do with my work or not. And I'm clearly better calibrated on the not work related ones.

**Daniel Filan:**
Are the not work ones about things to do with your life or about things in the world that don't have to do with your life?

**Katja Grace:**
I think they're a bit of both, but I think that the main difference between the two categories is the ones to do with work are like, "I'm going to get this thing done by the end of the week." And the ones not to do with work are like, "If I go to the doctor, he will say that my problem is blah." So is it to do with my own volition.

**Daniel Filan:**
Yeah. So I've done a similar thing and I've noticed that I'm much less accurate in forecasting questions about my own life. Including questions like at one point I was wrong about what a doctor would diagnose me with. I lost a lot of prediction points for that, but I think part of the effect there is that if I'm forecasting about something like how many satellites will be launched in the year 2019 or something I can draw on a bunch of other people answering that exact same question. Whereas there are not a lot of people trying to determine what disease I may or may not have.

**Katja Grace:**
I guess I almost never forecast things where it would take me much effort to figure out what other people think about it to put into the prediction I think. I don't tend to do that. So I think it's more between ones where I'm guessing.

**Daniel Filan:**
Yeah, that seems fair. A different thing about your survey is that you ask people about the sensitivity of AI progress to various inputs, right? So if you'd halved the amount of computation available, what that would do to progress, or if you'd halved the training data available, or the amount of algorithmic insights. And it seems to be that one thing some enterprising listener could do is check if that concords with work done by people like Danny Hernandez on scaling laws or something.

**Daniel Filan:**
I guess the final thing I'd like to ask about the survey, perhaps two final things. The first is aside from the things that I've mentioned, what do you think the most interesting findings were that didn't quite make it into the main paper, but are available online?

**Katja Grace:**
I guess the thing that we've talked about a little bit, but that I would just emphasize much more as a headline finding is just that the answers are just very inconsistent and there are huge framing effects. So I think it's both important to remember that when you're saying, "People say AI in however many years." I think the framing that people usually use for asking about this is the one that gets the soonest answers out of all the ones we tried, or maybe out of the four main ones that we tried. So that seems sort of good to keep in mind. I think another important one was just that the median probability that people put on outcomes in the vicinity of extinction level of bad is 5%, which, I don't know, seems pretty wild to me that among sort of mainstream ML researchers the median answer is 5% there.

**Daniel Filan:**
Does that strike you as high or low?

**Katja Grace:**
I think high. I don't know, 5% that this project destroys the world, or does something similarly bad, it seems unusual for a field.

**Daniel Filan:**
They also had a pretty high probability that it would be extremely good. Right? It's unclear if those balance out.

**Katja Grace:**
Yeah. That's fair. I guess as far as should we put attention on trying to steer it toward one rather than the other, it at least suggests there's a lot at stake, or a lot at risk, and more support for the idea that we should be doing something about that than I think I thought before we did the survey.

## Work related to forecasting AI takeoff speeds <a name="takeoff-speeds"></a>

**Daniel Filan:**
So now I'd like to move on to asking you some questions about AI Impacts' work on, roughly, takeoff speeds. So takeoff speeds refers to this idea that in-between the time when there's an AI that's about as good as humans perhaps generally, or perhaps in some domain, and the time when AI is much, much smarter or more powerful or more dominant than humans, that might take a very long time, which would be a slow takeoff, or it might take a very short time, which would be a fast takeoff. AI Impacts has done, I think, a few bits of work relevant to this that I'd like to ask about.

### How long it takes AI to cross the human skill range <a name="skill-range"></a>

**Daniel Filan:**
So the first that seemed relevant to me is this question about how long it takes for AI to cross the human skill range of various tasks. So there's [a few benchmarks I found on the site](https://aiimpacts.org/?s=human+range). So for classification of images on the ImageNet dataset, [it took about three years](https://aiimpacts.org/time-for-ai-to-cross-the-human-performance-range-in-imagenet-image-classification/) to go from beginner to superhuman. For English draughts [i.e. checkers] [it took about 38 years](https://aiimpacts.org/time-for-ai-to-cross-the-human-range-in-english-draughts/) to go from beginner to top human, [21 years](https://aiimpacts.org/time-for-ai-to-cross-the-human-range-in-starcraft/) for StarCraft, [30 years](https://aiimpacts.org/time-for-ai-to-cross-the-human-performance-range-in-go/) for Go, and [30 years](https://aiimpacts.org/time-for-ai-to-cross-the-human-performance-range-in-chess/) for chess. And in Go, it's not just that the beginners were really bad. Even if you go from what I would consider to be an amateur who's good at the game to the top human, that was about 15 years roughly. Why do you think it takes so long for this to happen? Because there's this intuitive argument that, look. Humans, they're all pretty similar to each other relative to the wide range of cognitive algorithms one could have. Why is it taking so long to cross the human skill range?

**Katja Grace:**
I'm not sure overall, but I think the argument that humans are all very similar so they should be in a similar part of the range. I think that doesn't necessarily work because I guess if you have, consider wheelbarrows, how good is this wheelbarrow for moving things? Even if all wheelbarrows are basically the same design, you might think there might be variation in them just based on how broken they are in different ways.

**Katja Grace:**
So if you think of humans as being, I don't know if this is right, but if you think of them as having sort of a design and then various bits being more or less kind of broken relative to that in the sense of having mutations or something, or being more encumbered in some way, then you might expect that there's some kind of perfect ability to function that nobody has, and then everyone just has going down to zero level of function, just different numbers of problems. And so if you have a model like that, then it seems like even if a set of things basically have the same design, then they can have an arbitrarily wide range of performance.

**Daniel Filan:**
I guess there's also some surprise that comes from the animal kingdom. So I tend to think of humans as much more cognitively similar to each other than to all of the other animals. But I would be very surprised if there's a species of fish that was better than my friend at math, but worse than me, or if there was some monkey that had a better memory than my friend, but worse memory than the guy who is really good at memory. Do you think this intuition is mistaken? Or do you think that despite that we should still expect these pretty big ranges?

**Katja Grace:**
I guess it seems like there are types of problems where other animals basically can't do them at all and then humans often can do them. So that's interesting. Or that seems like one where the humans are clearly above, though - I guess chess is one like that perhaps, or I guess I don't know how well any animal can be taught to play chess. I assume it's quite poorly.

**Daniel Filan:**
Aren't there chickens that can play checkers apparently? I have heard of this. I don't know if they're good.

**Katja Grace:**
My impression is that as soon as we started making AI to play, I think checkers or chess, that it was similar to amateur humans, which sort of seems like amateur humans are kind of arbitrarily bad in some sense, but maybe there's some step of do you understand the rules? And can you kind of try a bit or something? And both the people writing the first AIs to do this and the amateurs who've just started doing it have got those bits under control by virtue of being humans. And, I don't know, the fish don't have it or something like that. I think it's interesting that, like you mentioned, what was it, ImageNet was only three years or something.

**Katja Grace:**
I think that's very vague and hard to tell by the way, because we don't have good measures for people's performance at ImageNet that I know of. And we did some of it ourselves and I think it depends a lot on how bored you get looking at dogs or something. But yeah, it seems like it was plausibly pretty fast. And it's interesting that ImageNet or recognizing images is in the class of things where humans were more evolved for it. And so most of them are pretty good, unless they have some particular problem, and I'm not sure what that implies, but it seems like it wouldn't surprise me that much if things like that were different. Whereas chess is a kind of thing where by default we're not good at it. And then you can sort of put in more and more effort to be better at it.

**Daniel Filan:**
So the first thing I wanted to say is that as could probably be predicted from passing familiarity with chickens, they're able to peck a checkers board perhaps to move pieces, but they are not at all good at playing checkers. So I would like to say that publicly.

**Daniel Filan:**
Maybe the claim of short, small human ranges is something like that there are people who are better or worse at chess, but like a lot of that variance can just be explained by, some people have bothered to try to get good at chess. And some people might have the ability to be really good at chess, but they've chosen something better to do with their lives. And I wonder if there's instead a claim of, look, if everybody tried as hard as the top human players tried to be really good at chess, then that range would be very small and somehow that implies a short.

**Katja Grace:**
I think that we probably have enough empirical data to rule that out for at least many activities that humans do where there are enthusiasts at them, but they never reach anywhere near peak human performance.

**Daniel Filan:**
Yeah, and with the case of Go I think maybe good at Go - for Go enthusiasts, I mean like the rank of [1d](https://en.wikipedia.org/wiki/Go_ranks_and_ratings) - I think good at Go is roughly the range where maybe not everybody could get good at Go, or I wouldn't expect much more out of literally everybody, but it still takes 15 years to go from that to top human range. It still seems like an extremely confusing thing to me.

**Katja Grace:**
I was going to ask why it would seem so natural for it to be the other way that it's very easy to cross the human range quickly?

**Daniel Filan:**
Something like just the relative - humans just seem more cognitively similar to each other than most other things. So I would think well, it can't be that much range. Or human linguistic abilities. I think that there's no chicken that can learn languages better than my friend, but worse than me.

**Katja Grace:**
Also chickens can't learn languages at all, or I assume pretty close to nothing. So it seems like for the language range or something, depending on how you measured it, you might think humans cover a lot of the range at least up until the top humans. And then there are a bunch of things that are at zero.

**Daniel Filan:**
I mean, there still are these African grey parrots or something, or this monkey can use sign language, or this ape I guess.

**Katja Grace:**
There was [GPT-3](https://arxiv.org/abs/2005.14165), which is not good in other ways.

**Daniel Filan:**
Yeah. It still seems to me that like basically every adult can, or perhaps every developmentally normal adult, it seems to me can use language better than those animals, but I'm not certain. I wonder if instead my intuition is that humans are just - the range in learning ability is not so big? But then that's further away from the question you want to predict, which is how long until AIs can't destroy us until they can destroy us.

**Katja Grace:**
Assuming that when there are AIs that are smarter than any human they can destroy us, which seems pretty non-obvious. But yeah.

**Daniel Filan:**
No, I'm not assuming that. I think the reason that people are interested in this argument, or one part of this argument that I'm trying to focus on is how long until you have something that's about a smart as a human, which can probably not destroy the human race, until you're some degree of smartness at which you can just destroy all of humanity, whether or you might want to.

**Katja Grace:**
Because it's super, super human type thing.

**Daniel Filan:**
Yeah. I still find this weird, but I'm not sure if I have any good questions about it.

**Katja Grace:**
I agree it's weird. I guess at the moment we're doing some more case studies on this and I'm hoping that at the end of them to sit down and think more about what we have found and what to make of it.

**Daniel Filan:**
Yeah. What other case studies are you doing? Because the ones that have been looked at there's ImageNet, which is image classification, then there's a bunch of board games and one criticism of this has been, well, maybe that's too narrow a range of tasks. Have you looked at other things?

**Katja Grace:**
Yeah. I forget which ones are actually up on the website yet. I think [we do have StarCraft up](https://aiimpacts.org/time-for-ai-to-cross-the-human-range-in-starcraft/) probably. And maybe that was like 20 years or something.

**Daniel Filan:**
21 years.

**Katja Grace:**
We have some that are further away from AI, like clock stability. How well can you measure a period of time using various kinds of clocks or your mind. Where that one was about 3000 years, apparently according to our tentative numbers.

**Daniel Filan:**
3000 years between...

**Katja Grace:**
For our automated time measuring systems to go between the worst person at measuring time and the best, according to our own, somewhat flawed efforts to find out how good different people are at measuring time. I think it wasn't a huge sample size. We just got different people to try and measure time in their head, and we tried to figure out, I think, how good professional drummers were at this or something.

**Daniel Filan:**
That seems shockingly long.

**Katja Grace:**
Well, this all happened quite a long time ago when everything was happening more slowly.

**Daniel Filan:**
When did we hit superhuman timekeeping ability?

**Katja Grace:**
I think in the 1660s when we got well adjusted pendulum clocks, according to my notes here, but we haven't finished writing this and putting it up. So I'm not sure if that's right.

**Daniel Filan:**
So you've looked at timekeeping ability. Are there any other things that we might expect to see on the website soon?

**Katja Grace:**
Frequency discrimination in sounds where it tentatively looks like that one's about 30 years. Speech transcription, which is, I think quite hard because there aren't very good comparable measures of human or AI performance.

**Daniel Filan:**
I will say with my experience with making this podcast and using AI speech transcription services, it seems to me to be that the commercially available ones are not yet at the Daniel quality range, although they do it much faster.

**Katja Grace:**
There are also things that we haven't looked at, but it's not that hard to think about, like robotic manipulation it seems has been within the range of not superhuman at all sorts of things, but probably better than some humans at various things for a while I think, though I don't know a huge amount about that. And I guess creating art or something sort of within the human range.

**Daniel Filan:**
Yeah. It could be that we're just not sophisticated enough to realize how excellent AI art truly is. But yeah, my best guess is that it's in the human range.

### How often technologies have discontinuous progress <a name="discontinuous-progress"></a>

**Daniel Filan:**
I'd next like to talk about discontinuities in technological progress and this question of arguments for and against fast takeoff. So how common are discontinuities in technological progress and what is a discontinuity?

**Katja Grace:**
[We were measuring discontinuities](https://aiimpacts.org/discontinuous-progress-investigation/) in terms of how many years of progress at previous rates happened in one go, which is a kind of vague definition. It's not quite clear what counts as one go, but we're also open to thinking about different metrics. So if it was like, "Ah, it wasn't one go, because there were like lots of little gos, but they happened over a period of 10 minutes." Then we might consider the metric of looked at every 10 minutes did this thing see very sudden progress. We're basically trying to ask when is there a massive jump in technology on some metric? And we're trying to explicate that in a way that we can measure. At a high level, how often do they happen? It's sort of not never, but it's not that common.

**Katja Grace:**
We didn't do the kind of search for them where you can easily recover the frequency of them. We looked for ones that we could find by asking people to report them to us. And we ended up finding 10 that were sufficiently abrupt and clearly contributed to more progress on some metric than another century would have seen on the previous trend. So 100 years of progress in one go, and it was pretty clear and robust. Where there were quite a few more where maybe there were different ways you could have measured the past trend or something like that.

**Daniel Filan:**
Are there any cases where people might naively think that there was really big, quick progress or something where there wasn't actually?

**Katja Grace:**
I guess it's pretty hard to rule out there being a discontinuity of some sort, because we're looking for a particular trend that has one in. So if you're like, the printing press, usually the intuition people have is this was a big deal somehow. So if we're like, "Ah, was it a discontinuity in how many pages were printed per day or something?" And if it's not it might still be that it was in some other nearby thing. I think a thing that was notable to me was Whitney's [cotton gin](https://en.wikipedia.org/wiki/Eli_Whitney#Cotton_gin). [Our best guess](https://aiimpacts.org/effect-of-eli-whitneys-cotton-gin-on-historic-trends-in-cotton-ginning/) in the end was that it was a moderate discontinuity, but not a huge one. And it didn't look to me like it obviously had a huge effect on the American cotton industry in that the amount of cotton per year being produced was already shooting up just before it happened. So that still does count as a discontinuity, I think. But yeah, it looks like much less of a big deal than you would have thought, much more continuous.

**Katja Grace:**
I think maybe penicillin for syphilis didn't seem discontinuous in terms of the trends we could find. And one reason it didn't, I think, there were some things where we could get clear numbers for how many people were dying of syphilis and that sort of thing. And there it's kind of straightforward to see it doesn't look like it was a huge discontinuity. It sounds like it was more clearly good on the front of how costly was it to get treatment. It seems like for the previous treatment, people just weren't even showing up to get it because it was so bad.

**Daniel Filan:**
Bad in terms of side effects?

**Katja Grace:**
Side effects. Yeah. But interestingly, it was nicknamed the magic bullet because it was just so much better than the previous thing. Over the ages syphilis has been treated in many terrible ways, including mercury and getting malaria because malaria, or I guess the fever, helps get rid of the syphilis or something.

**Daniel Filan:**
And those are bad ways to treat syphilis?

**Katja Grace:**
I think they're both less effective and also quite unpleasant in themselves or they have bad side effects. So the thing prior to penicillin was kind of already quite incredible in terms of it working and not being as bad as the other things I think. And so then even if penicillin was quite good on that front comparably it didn't seem like it was out of the distribution for the rate at which progress was already going at that sort of thing.

**Daniel Filan:**
So I guess the reason that you would be interested in this is you want to know are big discontinuities the kind of thing that ever happens? And if they are the kind of thing that ever happens, then it shouldn't take that much evidence to convince us that it'll happen in AI. And so in assessing this I could look at the list of things that you guys are pretty sure were discontinuous on some metric. I think there's [this additional page which is things that might be discontinuities that you haven't checked yet](https://aiimpacts.org/incomplete-case-studies-of-discontinuous-progress/). And there are a lot of entries on that page, it kind of looked like to me. I'm wondering if you could forecast, what's your guess as to the percentage of those that you would end up saying, "oh yeah, this seems quite discontinuous."

**Katja Grace:**
I guess among the trends that we did look into, we also carefully checked that some of them didn't seem to have discontinuities in them. So I guess for trying to guess how many there would be in that larger set, I think maybe something like that fraction, where [inaudible 00:48:14] you're looking at these very big robust discontinuities, we found 10 of them in 38 different trends where some of those trends were for the same technology, but there were different ways of measuring success or something like that. And some of the trends, they could potentially have multiple discontinuities in, but yeah so it's roughly something like 10 really big ones in 38 trends. Though that's probably not quite right for the fraction in the larger set, because I think it's easier to tell if a thing does have a big discontinuity than to show that it doesn't probably.

**Katja Grace:**
Although, well, maybe that's not right. I think showing that it does is actually harder than you might think, because you do have to find enough data leading up to the purported jump to show that it was worse just beforehand, which is often a real difficulty. Or something looks like a discontinuity, but then it's not because there were things that just weren't recorded that were almost as good or something. But yeah, I guess you could think more about what biases there are in which ones we managed to find. I think the ones that we didn't end up looking into, I think are a combination of ones that people sent in to us after we were already overwhelmed with the ones we had, and ones where it was quite hard to find data, if I recall. Though some of this work was done more in like 2015. So I may not recall that well.

**Daniel Filan:**
I think my takeaway from this is that discontinuities are the kind of thing that legitimately just do happen sometimes, but they don't happen so frequently that you should expect them. For a random technology you shouldn't necessarily expect one, but if you have a decent argument it's not crazy to imagine there being a discontinuity. Is that a fair...

**Katja Grace:**
I think that's about right. I'm not quite sure what you should think over the whole history of a technology, but there's also a question of suppose you think there's some chance that there's a discontinuity at some point, what's the chance of it happening at a particular level of progress. It seems like that's much lower.

**Daniel Filan:**
Yeah that seems right. At least on priors.

### Arguments for and against fast takeoff of AI <a name="fast-takeoff-arguments"></a>

**Daniel Filan:**
I guess I'd like to get a bit into these arguments about fast takeoff for artificial intelligence, and in particular this argument that it won't take very long to hit the human range, to go through the human range to very smart. Somehow something about that will be quite quick. And my read of [the AI impact page about arguments about this](https://aiimpacts.org/likelihood-of-discontinuous-progress-around-the-development-of-agi/) is that it's skeptical of this kind of fast takeoff. Is that a fair summary of the overall picture?

**Katja Grace:**
I think that's right. Yeah. I think there were some arguments where they didn't seem good and some where it seemed like they could be good with further support or something. And we don't know of the support, but maybe someone else has it, or it'd be a worthwhile project to look into it and see if we could make it stronger.

**Daniel Filan:**
I'd like to discuss a few of the arguments that I think I, and maybe some listeners might have initially found compelling and we can talk about why they may or may not be. I think one intuitive argument that I find interesting is this idea of recursive self-improvement. So we're going to have AI technology, and the better it is, the higher that makes the rate of progress of improvement in AI technology, and somehow this is just the kind of thing that's going to spiral out of control quickly. And you've got humans doing AI research and then they make better humans doing better AI research. And this seems like the kind of thing that would go quickly. So I'm wondering why you think that this might not lead to some kind of explosion in intelligence?

**Katja Grace:**
I do think that it will lead to more intelligence and increasing intelligence over time. It seems like that sort of thing is already happening. I'd say we're sort of already in an intelligence explosion that's quite slow moving, or these kinds of feedback effects are throughout the economy. And so there's a question of, should you expect this particular one in the future to be much stronger and take everything into a new regime? And so I think you need some sort of further argument for thinking that that will happen. Whereas currently we can make things that make it easier to do research and we do. And I guess there's some question of whether research is going faster or slower or hard to measure perhaps the quality of what we learn, but this kind of feedback is the norm. And so I guess I think you could make a model of these different things, a quantitative model, and say, "Oh yeah. And when we plug this additional thing in here it should get much faster." I haven't seen that yet. But maybe someone else has it.

**Daniel Filan:**
One reason that this seems a little bit different from other feedback loops in the economy to me is that with AI it seems like, I don't know, one feedback loop is you get a bit better at making bread, then everybody grows up to be a little bit stronger. And everyone's a little bit more healthy, or the population's a little bit bigger and you have more people who are able to innovate on making bread. And that seems like a fairly circuitous path back to improvement. Whereas with machine learning, it seems like making machine learning better is somehow very closely linked to intelligence. Somehow. If you can make things generally smarter then somehow that might be more tightly linked to improving AI development than other loops in the economy, and that's why this might - this argument is less compelling now that it's out of my head than when it was in it. But I'm wondering if you have thoughts about that style of response.

**Katja Grace:**
All things equal, it seems like tighter loops like that are probably going to go faster. I think, I guess, I haven't thought through all of that, but I think also that there are relatively tight ones where for instance, people can write software that immediately helps them write better software and that sort of thing. And I think you could make a fairly similar argument about softwarey computery things that we already have. And so far that hasn't been terrifying. Perhaps, but then you might say, and we've already seen the start of it and it seems like it's a slow takeoff so far. I don't necessarily think that it will continue to be slow or I think maybe continuing economic growth longterm has been speeding up over time. And if it hadn't slowed down in the '50s or something I guess, [maybe we would expect to see a singularity around now anyway](https://aiimpacts.org/historical-growth-trends/). So maybe yeah, the normal feedback loops in technology and so on, eventually you do expect them to get super fast.

**Katja Grace:**
I think there's a different question of whether you expect them to go very suddenly from much slower to much faster. I guess I usually try to separate these questions of very fast progress at around human level AI into fast progress leading up to, I guess maybe Nick Bostrom calls it crossover or something? So some point where the AI becomes good enough to help a bunch with building more AI. Do you see fast progress before that? And do you see fast progress following that where the intelligence explosion would be following that?

**Katja Grace:**
And the discontinuity stuff that we've done is more about before that, where I think a strong intelligence explosion would be a sufficiently weird thing that, I wouldn't just say, "Oh, but other technologies don't see big discontinuities." I think that would be a reasonable argument to expect something different, but for an intelligence explosion to go from nothing to like very fast that seems like there has already been a big jump in something. It seems like AI's ability to contribute to AI research kind of went from meh to suddenly quite good. So part of the hope with the discontinuity type stuff is to address should we expect an intelligence explosion to appear out of nowhere or to more gradually get ramped up?

**Daniel Filan:**
Yeah. And I guess that the degree of gradualness matters because you sort of have a warning beforehand.

**Katja Grace:**
And maybe the ability to use technologies that are improving across the board perhaps to make the situation better in ways.

**Daniel Filan:**
Yeah. Whereas if it were just totally out of the blue then on any given day, could be tomorrow, so we just better be constantly vigilant. I guess I'd like to talk a bit about the second argument that I'm kind of interested in here, which is sort of related to the human skill range discussion. So as is kind of mentioned, on an evolutionary timescale it didn't take that long to get from ape intelligence to human intelligence. And human intelligence seems way better than ape intelligence. At least to me. I'm biased maybe, but we build much taller things than apes do. We use much more steel. It seems better in some objective ways or more impactful.

**Daniel Filan:**
And you might think - from this, you might conclude, it must be the case that there's some sort of relatively simple, relatively discrete genetic code or something. There's this relatively simple algorithm that once you get it, you're smart. And if you don't get it, you're not smart. And if there is such a simple intelligence algorithm that is just much better than all of the other things then maybe you just find it one day and on that day, before you didn't have it and you were like 10 good. And now you do have it. And you're like 800 good or something, or 800 smart. I'm wondering, so this is sort of related, it's maybe exactly the same thing as one of the considerations that you have that there are counter-arguments too, but I'm wondering if you could say what seems wrong to you about that point of view?

**Katja Grace:**
I guess I haven't thought about this in a while, but off the top of my head at the moment, a thing that seems wrong is, I guess there's kind of an alternate theory of how it is that we're smart, which is more like we're particularly good at communicating, and building up cultural things and other apes aren't able to take part in that, and more gave us an ability to build things up over time, rather than the particular mind of a human being such that we do much better. And I guess I'm not sure what that says about the situation with AI.

**Daniel Filan:**
Perhaps it means that you can't make strong inferences of the type humans developed evolutionary quickly therefore there's a simple smartness algorithm. Yeah. So it sounds like to the extent that you thought that human ability was because of one small algorithmic change, if it's the case that it was actually due to a lot of accumulated experience and greater human ability to communicate and learn from other people's experience that would block the inference from human abilities to quick takeoffs.

**Katja Grace:**
That seems right. It seems like it would suggest that you could maybe quickly develop good communication skills, but if an AI has good communication skills it seems like that would merely allow it to join the human giant pool of knowledge that we share between each other. And if it's a bit smarter than humans or even if it's a lot smarter, I think on that model maybe it can contribute faster to the giant pool of knowledge, but it's hard for it to get out ahead of all of humanity based on something like communication skills.

**Katja Grace:**
But I also have a different response to this, which is you might think that whatever it is that the way that humans are smart is sort of different from what monkeys are trying to do or something. It wasn't like evolution was pushing for better building of tall buildings or something. It seemed more like if we have to fight on similar turf to monkeys, doing the kinds of things that they have been getting better at, fighting in the jungle or something, it's just not clear that individual humans are better than gorillas. I don't know if anyone's checked. So you might have a story here that's more like, well, we're getting gradually better at some cognitive skills. And then at some point we sort of accidentally used them for a different kind of thing.

**Katja Grace:**
I guess an analogy is a place I do expect to see discontinuous progress is where there was some kind of ongoing progress in one type of technology and it just wasn't being used for one of the things it could be used for. And then suddenly it was. If after many decades of progress in chess someone decides to write a program for shmess, which is much like chess but different, then you might see shmess progress to suddenly go from zero to really good. And so you might think that something like that was what happened with human intelligence to the extent that it doesn't seem to be what evolution was optimizing for. It seems like it was more accidental, but is somehow related to what monkeys are doing.

**Daniel Filan:**
But does that not still imply that there's some like core insight to doing really well at shmess that you either have or you don't?

**Katja Grace:**
Maybe most of doing well at shmess is the stuff you got from doing well at chess. And so there's some sort of key insight in redirecting it to shmess, but if the whole time you're trying, if you're starting from zero and trying to just do shmess, it would have also taken you about as long as chess. There, I guess in terms of other apes, it would be a lot of what is relevant to being intelligent they do have, but somehow it's not well oriented toward building buildings or something.

**Daniel Filan:**
So, one thing that I'm interested about with this work is it's relationship to other work. So I think the most previous, somewhat comprehensive thing that was trying to answer this kind of question is [Intelligence Explosion Microeconomics](https://intelligence.org/files/IEM.pdf). It's unfortunate in that it's trying to talk very specifically about AI, but it was published I think right before deep learning became at all important, but I'm wondering if you have thoughts about that work by Eliezer Yudkowsky and what you see as the relationship between what you're writing about and that?

**Katja Grace:**
I admit that I don't remember that well what it says at this point, though I did help edit it as a, I guess, junior MIRI employee at the time. What I mostly recall is that it was sort of suggesting a field of looking into these kinds of things. And I do think of some of what AI Impacts does at least as hopefully answering that to some extent, or being work in that field that was being called for.

**Daniel Filan:**
I guess a related question that you are perhaps in a bad position to answer is that one thing that was notable to me is that you published this post, and I think a lot of people still continued believing that there would be fast progress in AI, but it seemed like the obvious thing to do would have been to write a response, and nobody really seemed to. I'm wondering, am I missing something?

**Katja Grace:**
I guess I am not certain about whether anyone wrote a response. I don't think I know of one, but there are really a lot of written things to keep track of. So that's nice. Yeah. I think I don't have a good idea of why they wouldn't have written a response. I think maybe at least some people did change their minds somewhat as a result of reading some of these things. But yeah, I don't think people's minds change that much probably overall.

## Coherence arguments <a name="coherence-arguments"></a>

**Daniel Filan:**
So I guess somewhat related to takeoff speeds, I guess I'd like to talk about just existential risk from AI more broadly. So you've been thinking about this recently a bit, for instance, you've written [this post on coherence arguments](https://www.lesswrong.com/posts/DkcdXsP56g9kXyBdq/coherence-arguments-imply-a-force-for-goal-directed-behavior) and whether they provide evidence that AIs are going to be very goal directed and trying to achieve things in the world. I'm wondering what have you been thinking about recently, just about the broader topic of existential risk from AI?

**Katja Grace:**
I've been thinking a bunch about this specific sort of sub-sub-question of, is this a reason to think that AI will be agentic where it's not super clear that it needs to be agentic for there to be a risk, but in the most common kind of argument, it being agentic plays a role. And so I guess things I've been thinking about there are, I'm pretty ignorant about this area, I think other people who are much more expert, and I'm just sort of jumping in to see if I can understand it and write something about it quickly for AI Impacts basically. But then I got sidetracked in various interesting questions about it to me which seemed maybe resolvable.

**Katja Grace:**
But I guess one issue is I don't quite see how the argument goes that says that if you're incoherent then you should become coherent. It seems like logically speaking if you're incoherent, I think it's a little bit like believing a false thing. You can make an argument that it would be good to change in a way to become more coherent. But I think you can also sort of construct a lot of different things being a good idea once you have circular preferences or something like that. So I think when I've heard this argument being made, it's made in a sort of hand-wavy way, that's like, "Ah." I don't know if listeners might need more context on what coherence arguments are.

**Daniel Filan:**
Yeah let's say that. What is a coherence argument?

**Katja Grace:**
Yeah. Well, I think a commonly mentioned coherence argument would be something like, a way that you might have incoherent preferences is if your preferences are circular, say. If you want a strawberry more than you want a blueberry, and you want a blueberry more than you want a raspberry, and you want a raspberry more than you want a strawberry. And the argument that that is bad is something like, "Well, if you have each of these preferences, then someone else could offer you some trade where you pay some tiny sum of money to get you one more, and you will go around in a circle and end up having spent money. And so that's bad, but the step where that's bad, I think is kind of relying on you having a utility function that is coherent or something. Or I think you could equally say that having lost money there is good due to everything being similarly good or bad basically.

**Daniel Filan:**
Well, hang on. But if you value money, it seems like this whole chain took you from some state and having some amount of money to the same state, but having less money. So doesn't that kind of clearly seem just bad if your preferences are structured such that at any state you'd rather have more money than less?

**Katja Grace:**
I guess I'm saying that if your basic preferences say are only these ones about different berries relative to each other, then you might say, "All right. But you like money because you can always buy berries with money." But I'm saying yeah, you could go that way and say, "Money equals berries." But you can also say, "Oh, negative money is the same as this cycle that I like." Or whatever. I guess I've been thinking of it as, well you have indifference curves across different sets of things you could have. And if you have incoherent preferences in this way, it's sort of like you have two different sets of indifference curves and they just cross and hit each other. And then it's just a whole web of things that are all equivalent, or there's some way of moving around the whole web. But these are somewhat incoherent thoughts. Which is like, "What have I been thinking about lately?"

**Katja Grace:**
It seems like in practice, if you look at humans, say, I think they do become more coherent over time. They do sort of figure out the logical implications of some things that they like, or other things that they now think they like. And so I think probably that does kind of work out, but it would be nice to have a clearer model of it. So I guess I've been thinking about what is a better model for incoherent preferences since it's not really clear what you're even talking about at that point.

**Daniel Filan:**
Do you have one?

**Katja Grace:**
A tentative one, which is, I guess it sort of got tied up with representations of things. So my tentative one is, instead of having a utility function you have something like a function that takes representations of states, it takes a set of representations of states and outputs an ordering of them. And so the ways you can be incoherent here are, you don't recognize that two representations are of the same state, and so you sort of treat them differently. Also because it's just from subsets to orderings, it might be that if you had a different subset your response to that would not be coherent with your responses to other subsets, if that makes sense.

**Daniel Filan:**
And then in, in this model why would you become more coherent? Or would you?

**Katja Grace:**
I guess the kind of dynamic I'm thinking of where you become more coherent is - you're continually presented with options and some of your options affect what your future representation to choice function is. Or I guess it doesn't really matter whether you call it yours or some other creature's in the future, but you imagine there are some choices that bear on this. And so if I'm deciding what will I do in this future situation? And I'm deciding it based on - if I currently have a preference, say currently I like strawberries more than raspberries or something, and I see that in the future when I'm offered strawberries, raspberries, or bananas, I'm going to choose raspberries instead. That if I can do the logical inference that says this means I'm going to get raspberries when I would have wanted strawberries or something.

**Katja Grace:**
I guess there's some amount of work going on where you're making logical inferences that let you equate different representations with each other, and then having equated them when you have a choice to change your future behavior, or your future choice function, then some of the time you'll change it to be coherent with your current one, or your one where you're making a different set of choices. And so very gradually I think that would bring things into coherence with one another is my vague thought.

**Daniel Filan:**
It's relatively easy to see how that should iron out dynamic inconsistency, where your future self wants different types of things than your current self. One thing that seems harder to me is, suppose right now if you say, "Which do you prefer a strawberry or blueberry?" And I say, "I'd rather have a strawberry." And then you say, "Okay, how do you feel about strawberries, blueberries, bananas?" And then I say, "Blueberries are better than strawberries, which are better than bananas." Then when I'm imagining my future self it's not obvious what the force is, pushing those to be the same. Right?

**Katja Grace:**
I think I was imagining that when you're imagining the future thing, there are different ways you can represent the future choice. And so you're kind of randomly choosing one between it. So sometimes you do see it as, "oh, I guess I'm choosing strawberry over raspberry here." Or whatever it was. Or sometimes you represent them in some entirely different way. Sometimes you're like, "Oh, I guess I'm choosing the expensive one. Is that what I wanted?" And so sometimes you randomly hit one where the way you're representing it, your current representations of things, or I guess, sorry, they're all your current ones, but when you're choosing what your future one will be, and you're representing the future choice in a different way to the way you would have been at the time, then you change what it would be at the time, or change what the output of the function is on the thing that it would be at the time, which I think has some psychological realism. But yeah, I don't know if other people have much better accounts of this.

## Arguments that AI might cause existential catastrophe, and counter-arguments <a name="catastrophe-arguments"></a>

**Katja Grace:**
But I guess you originally asked what I've been thinking about lately in the area of arguments about AI risk, I guess this is one cluster of things. Another cluster is what is the basic argument for thinking that AI might kill everyone? Which is quite a wild conclusion, and is it a good argument? What are counter arguments against it? That sort of thing. So I could say more about that if you're interested.

**Daniel Filan:**
Yeah. I am looking to talk about this a bit more, so yeah. What are your thoughts on why AI might or might not do something as bad as killing everyone?

**Katja Grace:**
I take the basic argument that it might to be something like, it seems quite likely that you could have superhuman AI, AI that is quite a lot better than any human. I could go into details. I guess I could go to details in any of these things, but I'll say what I think the high level argument is. It seems pretty plausible that we could develop this relatively soon. Things are going well in AI. If it existed it's quite likely that it would want a bad sort of future in the sense that there's a decent chance that it would have goals of some sort. There's a decent chance its goals would not be the goals of humans, and goals that are not the goals of humans are sort of bad often. Or that's what we should expect if they're not the same as our goals that we would not approve.

**Katja Grace:**
And so I guess I would put those, it has goals, they're not human goals. Non-human goals are bad, all sort of under superhuman AI would by default want a future that we hate. And then also if it wants a future that we hate, it will cause the future to be pretty bad, either via some sort of short-term catastrophe where maybe it gets smart very fast, and then it's able to take over the world or something, or via longer term reduction of human control over the future. So that would be maybe via economic competition or stealing things well, but slowly, that sort of thing.

### The size of the super-human range of intelligence <a name="super-human-intelligence-range"></a>

**Katja Grace:**
So arguments against these things, or places this seems potentially weak. I think superhuman AI is possible. I guess in some ways it seems pretty clear, but I think there's a question of how much headroom there is in the most important tasks. There are clearly some tasks where you can't get much better than humans just because they're pretty easy for us or something. Tic Tac Toe is an obvious one, but you might think, all right, well if things are more complex though, obviously you can be way better than humans. I think that just sort of seems unclear for particular things, how much better you can be in terms of value that you get from it or something.

**Daniel Filan:**
I guess it depends on the thing. One way you could get evidence that there would be headroom above present human abilities is if humans have continued to get better at a thing and aren't stopping anytime soon. So for instance, knowing things about math, that seems like a case where humans are getting better and it doesn't look like we've hit the top.

**Katja Grace:**
It seems like there you could distinguish between how many things can know, and how fast can you make progress at it or something, or how quickly can you accrue more things? It seems pretty likely you can accrue more things faster than humans. Though you do have to compete with humans with whatever kind of technology it is to use in some form, but not in their own brains potentially. AI has to compete with humans who have calculators or have the best software or whatever in a non-agentic form.

**Daniel Filan:**
Yeah. And I guess another task at which I think there's been improvement is, how well can you take over the world against people at a certain level of being armed? Or maybe not the whole world, I think that's unusual, but at least take over a couple of countries. That seems like the kind of thing that happens from time to time in human history. And people have kind of got better armed, and more skilled at not being taken over, I think in some ways. And yet people still keep on occasionally being taken over, which to me suggests a degree of progress in that domain that I'm not sure is flattening out.

**Katja Grace:**
That seems right. I definitely agree that there should be a lot more tech progress. It seems like, again, there's a question of how much total progress is there ever, and there's how fast - the thing that humans are doing in this regard is adding to the tech. And so there's a question of how much faster than a human can you add to the tech? Seems pretty plausible to me that you can add to it quite fast, though again compared to humans who also have various non-agentic technology it's less clear, and then it's not clear how much of the skills involved in taking over the world or something are like building better tech. I guess you might think that just building better tech alone is enough to get a serious advantage, and then using your normal other skills maybe you could take the world. Yeah. And it seems plausible to me that you can knock out this counter argument, but it just sort of needs better working out I think, or it's a place I could imagine later on being like, "Oh, I guess we were wrong about that. Huh?"

**Daniel Filan:**
Yeah. I guess maybe moving on. So the first step in the argument was we can have smarter than human intelligences.

**Katja Grace:**
I guess the next one was we might do it soon. But yeah, I guess that was just "might" anyway, but I don't have any particularly interesting counter arguments to that except maybe not.

### The dangers of agentic AI <a name="agentic-ai-dangers"></a>

**Katja Grace:**
But yeah, the next one after that was, if superhuman AI existed it would by default threaten the whole future where that is made up of: it would by default want a future that we hate and it would get it, or it would destroy everything as a result. And so I guess there there's "it has goals". I think that's one where again, I could imagine in 20 years being like, "Oh yeah, somehow we were just confused about this way of thinking of things."

**Katja Grace:**
In particular, I don't know, it seems like as far as I know we're not very clear on how to describe how to do anything without the destroying the world. It seems like if you describe anything as maximizing something maybe you get some kind of wild outcome. It seems like in practice that hasn't been a thing that's arisen that much. Or, you know, we do have small pigs or something, and they just go about being small pigs and it's not very terrifying. You might think that these kinds of creatures that we see in the world, it's possible to make a thing like that that is not aggressively taking over the world.

**Daniel Filan:**
I think people might be kind of surprised at this claim that it's very hard to write down some function that maximizing it doesn't take over the world. Can you say a little bit more about that?

**Katja Grace:**
I guess one thing to say is, "Well, what would you write down?"

**Daniel Filan:**
I don't know, like make me a burrito, please. It seems like a thing I might want a thing to do. It doesn't seem naively perhaps likely to destroy the world. I often ask people to make burritos for me, and maybe they're not trying hard enough but...

**Katja Grace:**
Maybe the intuition behind this being hard would be something like, well, if you're maximizing that in some sense, are you making it extremely probable that you make a burrito? Or making a burrito very fast? Or very many burritos or something? Yeah. I'm not sure what the strongest version of this kind of argument is.

**Daniel Filan:**
Make the largest possible burrito maybe. I guess it sort of depends how you code up what a burrito is.

**Katja Grace:**
I guess maybe if you tried to write it as some sort of utility function, is it one utility if you have a burrito and zero otherwise or something? Then it seems like maybe it puts a whole bunch of effort into making really sure that you have the burrito.

**Daniel Filan:**
Yeah. To me, that seems like the most robust case where utility functions always love adding more probability. It's always better.

**Katja Grace:**
It seems like maybe in practice building things is more like, you can just say, "Give me a burrito." And it turns out to be not that hard to have a thing get you a burrito without any kind of strong pressure for doing something else. Although you might think, "Well, all right, sure. You can make things like that. But surely we want to make things that are more agent-like." Since there's at least a lot of economic value in having agents it seems like, as in people who act like agents often get paid for that.

**Daniel Filan:**
As long as they don't destroy the world.

**Katja Grace:**
Maybe even if they do slowly they can be paid for that. You could imagine a world where it's actually quite hard to make a thing that's a strong agent in some sense. And that really deep down, everything is kind of like a [sphex](https://en.wikipedia.org/wiki/Sphex#Uses_in_philosophy) that's just responding to things. It's like, yep. When I see a stoplight, then I stop. And when I see a cookie on a plate, I put my hand out to fit it in my mouth. And you can have more and more elaborate things like that that sort of look more and more agent-like, and you can probably even make ones that are more agent-like than humans. And that is destructive in ways or dangerous, but it's not like they're arbitrarily, almost magically seeming agent-like where they think of really obscure things instantly and destroy the world. It's more like every bit of extra agenty-ness takes effort to get, and it's not perfect.

**Daniel Filan:**
Although on that view you could still think, "Well, we make things that are way more agent-like than people. And they have the agenty problems way more than people have them." I guess in that case, maybe you just think that it's quite bad, but not literally destroying the world. And then you move on to something else that might destroy the world actually.

**Katja Grace:**
Yeah. Something like that. Or once you're in the realm of this is a quantitative question of what it's like and how well the forces that exist in society for dealing with things like that can fight against them, then it's not like automatically the world gets destroyed at least. If looking back from the future I'm like, "Oh, the world didn't get destroyed. And why was that?" I think that's another class of things. The broader class being something like, I am just sort of confused about agency and how it works and what these partial agency things are like, and how easy it is to do these different things.

**Daniel Filan:**
So, yeah, it seems to be that one inside view reason for expecting something like agency in AI systems, is that the current way we build AI systems is we are implicitly getting the best thing in a large class of models at achieving some task. So you have neural nets that can have various weights, and we have some mechanism of choosing weights that do really well at achieving some objective. And you might think, "well, okay, it turns out that being an agent is really good for achieving objectives." And that's why we're going to get agents with goals. I'm wondering what you think about that.

**Katja Grace:**
Do you think there is a particular reason to think that agents are the best things for achieving certain goals?

**Daniel Filan:**
Yeah. I mean, I guess the reason is, what do you do if you're an agent? You sort of figure out all the ways you could achieve a goal and do the one that achieves it best.

**Katja Grace:**
Yeah. That seems right.

**Daniel Filan:**
And that's kind of a simple to describe algorithm, both verbally in English and in terms of if I had to write code to do it. It's not that long. But to hard code the best way of doing it would take a really long time. It's easier to program something like a very naive for-loop over all possible ways of playing Go, and say, "The best one, please." I think that's easier than writing out all the if statements of what moves you actually end up playing.

**Katja Grace:**
I mean, I'm not really an expert on this, but it seems like there's a trade-off where for the creature running in real time it takes it a lot longer to go through all of the possibilities and then to choose one of them. So if you could have told it ahead of time what was good to do, you might be better off doing that. In terms of maybe you put a lot of bits of selection initially into what the thing is going to do, and then it just carries it out fast. And I would sort of expect it to just depend on the environment that it's in, where you want to go on this.

**Daniel Filan:**
Yeah. Well, I mean, in that case, it's like you're-

**Katja Grace:**
Maybe you're right, even if I was right here there would be a lot of environments probably where the best thing is the agent-like one. Or maybe many more where it's some sort of intermediate thing where there're some things that get hard-coded because they're basically always the same. You probably don't want to build something that as soon as you turn it on, it just tries to figure out from first principles what the world is or something. If you just already know some stuff about the world, it's probably better to just tell it.

**Daniel Filan:**
Yeah. And you definitely don't want to build a thing that does that every day when it wakes up.

**Katja Grace:**
But you might think then that within the things that might vary, it's good for it to be agentic and therefore it will be.

**Daniel Filan:**
Yeah. Or also I kind of think that when you're looking over possible specific neural networks to have, it's just easier to find the relatively agent-like ones, because they are somehow simpler, or there are more of them roughly. And then once you get an agent-like thing you can tweak it a bit. Or more of them for a given performance level maybe. It's a bit hard to be exact about this without a actual definition of what an agent is.

**Katja Grace:**
Yeah. That seems right. So you were just saying that maybe agents are just much easier to find in program space than other things.

**Daniel Filan:**
Yeah. I kind of think that.

**Katja Grace:**
That seems plausible. I haven't given it much thought. It seems like you would want to think more about it for me.

**Daniel Filan:**
And then I think the next step in the argument was that the agent's goals are typically quite bad or did we already talk about that?

### The difficulty of human-compatible goals <a name="difficulty-human-compatible-goals"></a>

**Katja Grace:**
No, we were just talking about it has goals and then they're quite bad, which I was organizing it as it's goals are not human goals, and then by default non-human goals are pretty bad. Yeah, I guess I think it's sort of unclear. Human goals are not a point. There's sort of some larger cloud of different goals that humans have. It's not very clear to me how big that is compared to the space of goals. And it seems like in some sense, there are lots of things that particular humans like having for themselves. It seems like if you ask them about utopia and what that should be like it might be quite different. At least to me, it's like pretty unclear whether for a random person, if they got utopia for them, whether that would clearly be basically amazing for me, or whether it has a good chance of being not good.

**Katja Grace:**
I guess there's a question of AI, if you tried to make it have human goals, how close does it get, is it basically within that crowd of that cluster of human things, but not perfectly the human you are trying to get? Or is it far away such that it's much worse than anything we've ever dealt with? Where if it's not perfectly your goals, if we were trying to have it be your goals, but it's way closer than my goals are to your goals, then you might think that this is a step in a positive direction as far as you're concerned. Or maybe not, but it's at least not much worse than having other humans, except to the extent that maybe it's much more powerful or something and it isn't exactly anyone's goals.

**Daniel Filan:**
So I think the idea here is twofold. Firstly, there's just some difficulty in specifying what do I mean by human goals? If I'm trying to like load that into an AI, what kind of program would generate those? Okay there are three things. One of the things is that. The second thing is some concern that look, if you're just searching over a bunch of programs to see which program does well on some metric, and you think the program you're getting is doing some kind of optimization, there are probably a few different metrics that fit well with succeed. But if you have objective A, you would probably have objectives B, C, D and E that also motivate you to do good stuff in the couple of environments that you're training your AI on. But perhaps B, C, D and E might imply very different things about the rest of the future. For instance, one example of a goal B would be play nice until you see that it's definitely the year 2023, in which case go wild. I think the claim has to be firstly, that there is such a range.

**Katja Grace:**
Sorry, such a range?

**Daniel Filan:**
A range of goals such that if an optimizer had that goal it would still look good when you're testing it. Which is basically how we're picking AI algorithms. And you have to think that range is big, and that it's much larger than the range you'd want, than an acceptable range of within human error tolerance or something. Yeah. I think there, listeners can refer back to [episode 4 with Evan Hubinger](https://axrp.net/episode/2021/02/17/episode-4-risks-from-learned-optimization-evan-hubinger.html) for some discussion of this, but I'm wondering what do you think about that line of argument?

**Katja Grace:**
So it sounds like you're saying, "All right, there are sort of two ways this could go wrong, where one of them is we try to get the AI to have human goals, and we do it sort of imperfectly so then it's bad." Which is more like what people were concerned about further in the past. And then more recently there's Evan's line of argument that's, "Well, whatever goals you're even trying to give it, it won't end up with those goals because you can't really check which goals it has." It will potentially end up deceiving you and having some different goals.

**Daniel Filan:**
I see those as more unified than they are are often presented as actually. Now you're interviewing me and I get to go on a rant. I feel like everyone keeps on talking about, "Oh, everybody's totally changed their mind about how AI poses a risk" or something. And I think some people have, or I don't know, I've changed my mind about some things, but both of the things there seem to me that they fit into like the difficulty of writing a program that gets an agent to have the motivations that you want it to have.

**Katja Grace:**
That seems right.

**Daniel Filan:**
There's this problem of specification. Part of the problem of specification is you can't write it down, and part of the problem of specification is there are a few blocks in learning it. Like, firstly, it's hard to come up with a formalism that works on-distribution, and then it's hard to come up with a formalism that also works in worlds that you didn't test on. To me these seem more unified than they're typically presented as. But when lots of people agree with me often I'm the one who's wrong.

**Katja Grace:**
To me it seems like they're unified in the sense that there's a basic problem that we don't know how to get the values that we like into an AI, and they're maybe different in that well, we considered some different ways of doing it and each one didn't seem like it would work. One of them was somehow write it down or something. I guess maybe that one is also try to get it to learn it without paying attention to this other problem that would arise. Yeah. I don't know. They seem like sort of slightly different versions of a similar issue I would say. In which case maybe I'm mostly debating the non-mesa optimizer one, and then maybe the mesa-optimizer one, being Evan's one.

**Daniel Filan:**
That being the one about optimizers found by search.

**Katja Grace:**
Yeah. Originally you might think, "All right, if you find an optimizer by search that looks very close to what you want, but maybe it's not exactly what you want then is it that bad?" If you make [a thing that can make faces](https://thispersondoesnotexist.com/), do they just look like horrifying things that aren't real human faces once you set them out making faces? No, they basically look like human faces except occasionally I guess. So that's a counter argument.

**Daniel Filan:**
I mean, sometimes they have bizarre hats, but that doesn't seem like too bad.

**Katja Grace:**
I think sometimes they're kind of melting and green or something, but I assume that gets better with time. But that's like, "Oh, you're missing the mark a little bit. And how bad is it to miss the mark a little bit." Where I guess there's some line of argument about value is fragile and if you might miss the mark a little bit it'll be catastrophic. Whereas the mesa-optimizer thing is more like, "No, you'll miss the mark a lot because in some axis you were missing it a little in that it did look like the thing you wanted, but it turns out there are things that are just extremely different to what you wanted that do look like what you wanted by your selection process or something." And I guess that seems like a real problem to me. I guess, given that we see that problem I don't have strong views on how hard it is to avoid that and find a selection process that is more likely to land you something closer to the mark.

**Daniel Filan:**
Yeah. I should say, we haven't seen the most worrying versions of this problem. [There have been cases where we train something to play Atari and it sort of hacks the Atari simulator](https://www.youtube.com/watch?v=meE5aaRJ0Zs). But there have not really been cases where we've trained something to play Atari, and then it looks like it's playing Atari, and then a month later it is trying to play chess instead or something like that. And yeah, so it's not quite the case that, "Oh this, I'm just talking about a sensible problem that everyone, that we can tell exists."

**Katja Grace:**
If it's intrinsically a hard-to-know-it-exists problem, though. If the problem is like your machine would pretend it was a different thing to what it really is, it's sort of naturally confusing.

**Daniel Filan:**
I guess we've covered... We've now gone into both the questions of AI having different goals to humans and slight differences being extremely bad.

### The possibility of AI destroying everything <a name="possibility-ai-destroying-everything"></a>

**Katja Grace:**
Yeah, then perhaps there's the other prong here of the AI being terrible, which is, if it wanted a future that we hate, it would destroy everything.

**Daniel Filan:**
Or somehow get a future that we hate.

**Katja Grace:**
Where maybe I have more to say on the slow, getting a future that we hate... Where I guess, a counter argument to the first one is it's just not obvious whether you should expect it to just rapidly be extremely amazing, but perhaps there's some reason there's some chance for that. But as far as counter arguments to the other thing goes: it seems like in the abstract, it kind of rests on a simple model that's, more competent actors tend to accrue resources either via economic activity or stealing.

**Katja Grace:**
And that seems true, but it's all things equal and there are lots of other things going on in the world. And I think just a very common error to make in predicting things is you see that there is a model that is true, and you sort of don't notice that there are lots of other things going on. I think in the world at the moment, it doesn't seem the case that the smartest people are just far and away winning at things. I mean they're doing somewhat better often, but it's very random and there are lots of other things going on.

**Daniel Filan:**
It does basically seem to me that people who are the most competent, not all of them are succeeding amazingly, but it seems like they have a way higher rate of amazing success than everyone else. And I don't know, I hear stories about very successful people and they seem much more competent than me or anybody I know. And some people are really amazingly, seem to me to be really amazingly successful.

**Katja Grace:**
I don't know if that matches my experience? But also I guess it could be true. And also just a lot of very competent people also, aren't doing that well, where yeah, it does increase your chance, but it's such that the people who are winning the most, everything has gone right for them. They're competent and also they have good social connections and things randomly went well for them. Where still, each of these, is not just going to make you immediately win.

**Daniel Filan:**
Yeah. But I mean, if you think that there's some sort of relationship here and that we might get AI that's much more competent than everyone else. Then shouldn't we just follow that line and be, "Ah, well it might just get all the stuff?"

**Katja Grace:**
Okay. It seems like maybe that's what the model should be, but there are sort of other relationships that it might be. You might imagine that you just have to have a certain amount of trust and support from other people in order to be allowed to be in certain positions or get certain kinds of social power. And that it's quite hard to be an entity who no one is willing to extend that sort of thing to, who is quite smart and still to get very far.

**Daniel Filan:**
Yeah. I mean, the hope is that you can trick people. Right?

**Katja Grace:**
That's true. It seems like for smart people trying to trick people in our world, I think they often do quite badly or, I mean, sometimes they do well for a while-

**Daniel Filan:**
Yeah, I guess.

**Katja Grace:**
We do try to defend against that sort of thing. And that makes it... it's not just like the other things are not helping you do well, but people's lack of trust of you is causing them to actively try to stop you.

**Daniel Filan:**
Yeah. I mean, maybe this gets to the human range thing. To me, it seems like the question... the analogy that seems closer to me is not the most competent humans compared to the least competent humans; but the most competent humans compared to dogs or something, it's not actually very hard to trick a dog. I'm not that good at tricking people and I've tricked dogs, you know?

**Katja Grace:**
Yeah.

**Daniel Filan:**
I mean, dogs are unusually trusting animals. But I don't know, I think other animals can also be tricked.

**Katja Grace:**
That's fair. I guess when we were talking about the human range thing, it also seemed reasonable to describe some animals' cognitive abilities of some sorts as basically zero or something on some scales. So I somewhat wonder whether yeah, you can trick all manner of animals that basically can't communicate and don't have much going on in terms of concepts and so on. And you can trick humans also, but it's not just going to be trivially easy to do so. I mean, I think at some level of intelligence or something, or some level of capability and having resources and stuff, maybe it is. But also if you're hoping to trick all humans or something, or not get caught for this... I guess clearly humans can trick humans pretty often, but if you want it to be like a long run, sustainable strategy-

**Daniel Filan:**
Yeah, you need a combination of tricking some and out running the others, right?

**Katja Grace:**
Something like that. I agree that it might be that with intelligence alone, you can quite thoroughly win. But I just think it's less clear than it's often thought to be or something, that the model here should have a lot more parts in it.

**Daniel Filan:**
And so candidate parts are something like the relationship between people trying to not have all their stuff be stolen, and your agent that's trying to steal everyone's stuff and amass all the resources, or something.

**Katja Grace:**
I think I'm not quite sure what you meant by that one.

**Daniel Filan:**
Oh. Agents that are trying to trick everyone to accrue resources versus people who don't like to be tricked sort of.

**Katja Grace:**
Yeah. So I guess maybe I think of that as there are a bunch of either implicit or explicit agreements and norms and stuff that humans have for dealing with one another. And if you want to do a thing that's very opposed to existing property rights or expectations or something, that's often a lot harder than doing things that are legal or well looked upon by other people. I think, within the range of things that are not immediately going to ring alarm bells for people, it's probably still easier to get what you want, if you're more trusted or have better relationships with different people or are in roles where you're allowed to do certain things.

**Katja Grace:**
I'm not sure how relevant, or it seems like AI might just be in different roles that we haven't seen so far, or treated in different ways by humans, but I think for me, even if I was super smart and I wanted to go and do some stuff, it would sort of matter a lot how other people were going to treat me. Or on what things they were just going to let me do them and what things they were going to shoot me, or just pay very close attention or ask me to do heaps of paperwork or-

**Daniel Filan:**
Yeah. I think the argument has to be something like, look, there are some range of things that people will let you do, and if you're really smart, you can find really good options within that range of things. And the reason that that's true is that, somehow when human norms are constructing the range of things that we let people do, we didn't think of all the terrible things that could possibly be done. And we aren't just only allowing the things that we are certain are going to be fine.

**Katja Grace:**
Yeah. That seems right. I guess it seems partly we do sort of respond dynamically to things. If someone finds a loophole, I think we're often not just, oh, well I guess you're right, that was within the rules. Often we change the rules on the spot or something is my impression, but you know, you might be smart enough to get around that also to come up with a plan that doesn't involve anyone ever managing to stop you.

**Daniel Filan:**
Yeah. You somehow need to go very quickly from things looking like you haven't taken over the world to things looking like you have.

**Katja Grace:**
Or at least very invisibly in-between or something. And maybe that's possible, but it seems like a sort of quantitative question. How smart you have to be to do that and how long it actually takes for machines to become that smart. And I guess, different kind of counter argument here, or at least a different complication... I think the similar kind of model of, if you're more competent, you get more of the resources and then you take over the world, it was sort of treating share of resources as how much of the future do you get, which is not quite right.

**Daniel Filan:**
So yeah, the question is, if you have X percent of the stuff, do you get X percent of the future or something?

**Katja Grace:**
It seems a way that's obviously wrong is, suppose you're just the only person in the universe. And so it's sort of all yours in some sense, and you're just sitting on earth and this asteroid coming toward you, you clearly just don't have much control of the future. So there's also, you could then maybe model it as, oh yeah, there's a bunch of control of the future that's just going to no one. And maybe you can kind of get more or something like that. It seems like it would be nice to be clearer about this model. It seems like also in the usual sort of basic economics model or the argument for say, having more immigrants and that not being bad for you or something... It's sort of like, yeah, well you'll trade with them and then you'll get more of what you wanted.

**Katja Grace:**
And so you might want to make a similar argument about AI. If it wasn't going to steal stuff from you, if it was just going to trade with you and over time it would get more wealth, but you would get more wealth as well, it would be a positive sum thing. And you'd be like, "well, how could that possibly be bad?" Let's say it never violates your property rights or anything. It's more like, "well, there was a whole bunch of the future that was going to no one, because you were just sitting there probably going to get killed sometime, and then you both managed to get more control of the future, wasn't that good for you?" And then it seems, I guess the argument would be something like, "Ah, but you were hoping to take the whole future at some point. You were hoping that you could build some different technology that wasn't going to also have some of the future." But yeah, and maybe that works out, but I guess this just seems like a more complicated argument that it seems good to be clear on.

**Daniel Filan:**
Yeah. I mean, to me the response is - so there's two things there: firstly, there's a question of... I guess they're kind of both about widening the pie, to use the classic economist metaphor here. So one thing I think is that, suppose I have all of the world's resources, which are, a couple of loaves of bread and that's all the world has sadly, then I'm not going to do very well against the asteroid. Right? Whereas right now, even though I don't have all the world's resources, I'm going to do better because there's sort of more stuff around. Even I personally have more stuff. So in the AI case, yeah, I guess the thing is if AI technology in general has way more resources than me and is not cooperative, then it seems the case that that's clearly going to be worse than a world in which there's a similar amount of resources going around, but I, and my friends have all of it instead of them being under the control of an AI system.

**Daniel Filan:**
Yeah, and as to the the second thing, I don't know, my take is that the thing we should worry about is AI theft. I don't know. I don't have a strong reason to believe that these terrible AI systems are going to respect property rights, I don't know. Some people have these stories where they do and it's bad anyway, but I don't know. It seems to me they'd just steal stuff.

**Katja Grace:**
I guess you might think about the case where they respect property rights either because you are building them. So there's some chance you'll succeed at some basic aspect of getting them to act as you would want. Where respecting the law is a basic one, that being our usual way of dealing with creatures who are otherwise not value-aligned.

**Daniel Filan:**
I mean, that's not how we deal with dogs, right? Or it a little bit is, but they're treated very differently by the law than most things. And if I think about what's our control mechanism for dangerous animals or something, it mostly isn't law. I guess maybe the problem is well, they just can't understand law. But it seems to me that, "obey the law", in my mind, once you've solved "get a thing that robustly obeys the law", you'll have solved most to all of the problem

**Katja Grace:**
That seems plausible. It's not super obvious to me.

**Daniel Filan:**
And in particular, that seems like a hard thing to me.

**Katja Grace:**
I agree it seems plausibly hard. I guess the other reason it seems maybe worth thinking about is just that, if you did get to that situation, it still seems like you might be doomed. I don't know that I would be happy with a world where we trade with the AIs and they are great. And we trade with them a lot and they get a lot of resources and they're really competent at everything. And then they go on to take over the universe and we don't do much of it. Maybe I would prefer to have just waited and hoped to build some different technology in the future or something like that.

**Daniel Filan:**
Yeah. I guess there's this question of, is it a thing that's basically like human extinction and even out of the AI outcomes that are not basically human extinction, perhaps some can be better than others and perhaps we should spend some time thinking about that.

**Katja Grace:**
That's not what I was saying, but yeah, that seems potentially good.

**Daniel Filan:**
Oh, to me, that seems almost the same as what you were saying. Because in the outcome where we have these very smart AIs that are trading with us and where we end up slightly richer than we currently are, to me, that doesn't seem very much like human extinction.

**Katja Grace:**
I was thinking that it does, depending on how we... Or if we end up basically on one planet and we're a bit richer than we currently are, and we last until the sun kills us. Maybe I wouldn't describe it as human extinction in that we didn't go extinct. However, as far as how much of the possible value of the future did we get supposing that AI is doing things that we do not value at all, it seems like we've basically lost all of it.

**Daniel Filan:**
Yeah, that seems right. I think this is perhaps controversial in the wider world, but not controversial between you and I. I'm definitely in favor of trying to get AI systems that really do stuff that we think is good and don't just fail to steal from us.

**Katja Grace:**
Nice.

**Daniel Filan:**
Yeah. I think maybe part of my equanimity here is thinking, if you can get "obey the law", then maybe it's just not too much harder to get, "be really super great for us" rather than just merely lawful evil.

**Katja Grace:**
I really have to think more about that to come to a very clear view. I guess I am more uncertain. I guess we were talking earlier about the possibility that humans are mostly doing well because of their vast network of socially capable culture accumulators. I think in that world, it's sort of less clear what to think of super AIs joining that network. It seems like the basic units then are more big clusters of creatures who are talking to each other. It's sort of not clear what role super AI plays in it. Why doesn't it just join our one? Is there a separate AI one that's then fighting ours? I don't know why would you think that?

**Katja Grace:**
It's just kind of - a basic intuition I think people have is like, ah, well, so far there have been the humans, the basic unit, and then we're going to have this smarter thing, and so anything could happen. But if it's more like, so far the basic unit has been this giant pool of humans interacting with each other in a useful way. And there've been like more and fewer of them and they have different things going on and now we're going to add a different kind of node that can also be in a network like this.

**Katja Grace:**
I guess it's just like the other argument, I feel like it doesn't... Or the other intuition, doesn't go through for me at least.

## The future of AI Impacts <a name="future-ai-impacts"></a>

**Daniel Filan:**
I think I'm going to move on now. Yeah. So I guess I'd like to talk a bit more about you and AI Impacts' research. Yeah, sort of circling back. Hopefully AI Impacts - I don't know if you hope this, I hope that AI Impacts will continue for at least 10 more years. I'm wondering, what do you think the most important question is for AI Impacts to answer in those 10 years? And a bit more broadly, what would you like to see happen?

**Katja Grace:**
I guess on the most important question, I feel like there are questions that we know what they are now: when will this happen? How fast will it happen? And it seems like if we could become more certain about timelines, say, such that we are, ah it's twice as likely to happen in the next little while than we thought it was. And it's probably not going to happen in the longer term. As far as guiding other people's actions, I feel like it's maybe not that helpful in that you're sort of giving one bit of information about, what should people be doing? And I think that's kind of true for various of these high level questions, except maybe, is this a risk at all? Where if you manage to figure out that it wasn't a risk, then everyone could just go and do something else instead, which sounds pretty big.

**Katja Grace:**
But I feel like there are things where it's less clear what the question is, where it's more like maybe there's details of the situation where you could just see better. Like how exactly things will go down, where it's currently quite vague or currently people are talking about fairly different kinds of scenarios happening: are the AIs all stealing stuff? Is there just one super AI that immediately stole everything? Is it some kind of long-term AI corporations competing one another into oblivion or something? It seems just getting the details of the scenarios down, that would be pretty good. Yeah. I guess this isn't my long-term well considered answer. It's more like right now, which thing seems good to me.

**Daniel Filan:**
And I guess another question, what work do you think is the best complement to AI Impacts' research? So what thing that is really not what AI Impacts does, is the most useful for what AI Impacts does?

**Katja Grace:**
It's hard to answer the most useful, but a thing that comes to mind is sort of similar questions or also looking around for empirical ways to try to answer these high level questions, but with more of a AI expertise background. I think that's clearly a thing that we are lacking.

## AI Impacts vs academia

**Daniel Filan:**
All right. Sort of a meta question, is there anything else I should have asked about AI Impacts and your research and how you think about things?

**Katja Grace:**
You could ask how it differs from academia? What is this kind of research?

**Daniel Filan:**
I guess being in academia, that was very obvious to me.

**Katja Grace:**
I'm curious how that seems different to you then?

**Daniel Filan:**
Oh, well, to me it seems like academics - so probably there's the deliverable unit, right? There are some pages on this website or [one page in particular](https://aiimpacts.org/resolutions-of-mathematical-conjectures-over-time/) that I'm just remembering that was about the time to solve various math conjectures. And the title is, how long does it take to solve mathematical conjectures? And the body is, we used this estimator on this dataset, here's the graph, the answer is this. And that's the page. It seems to me that academics tend to like producing longer pieces of work that are sort of better integrated, that are fancier in some sense.

**Katja Grace:**
Yeah. That seems right.

**Daniel Filan:**
In machine learning, people love making sure that you know that they know a lot about math.

**Katja Grace:**
I'm curious what you meant by better integrated? You mean into the larger literature?

**Daniel Filan:**
Yeah. It seems to me AI Impacts has a bunch of pages and they're kind of related to each other. But an academic is going to write a bunch of papers that are relatively, I think, more closely related to each other. Whereas, the kind of clusters of related stuff... It seems like there's going to be more per cluster than maybe AI Impacts does. I think AI Impacts seems to me to be happier-

**Katja Grace:**
Jumping around-

**Daniel Filan:**
To just answer one question and then move on to something totally different.

**Katja Grace:**
Yeah. I think that's right.

**Daniel Filan:**
It does occur to me that maybe... Normally in these shows, I don't normally tell people what their research is.

**Katja Grace:**
That's helpful though, because I mean, I think it's true that AI Impacts is different in those ways. And that is intentional, but I haven't thought about it that much in several years. So it's good to be reminded what other things are like.

**Daniel Filan:**
I guess there's also the people who do it. Academics tend to like research being done or supervised by people who have done a PhD in a relevant field and spend a lot of time on each thing. Whereas AI Impacts has more, I guess maybe more generalist is a way to summarize a lot of these things. More of it seems to be done by philosophy PhD students taking a break from their programs, which sound terrible to me [to be clear, I'm saying the programs sound terrible, not the students or their practice of taking a break to work at AI Impacts].

**Katja Grace:**
That seems right. Yeah. Again, I think part of the premise of the project is something like, there is a bunch that you can figure out from kind of a back of the envelope calculation or something often that is not being made use of. And so often what you want to add value to a discussion is not an entire lifetime of research on one detailed question or something. And so it's sort of, okay, having done a back of the envelope calculation for this thing, the correct thing to do is, now move on to another question. And if you ever become so curious about that first question again, or you really need to be sure about it or to get a finer-grained number or something, then go back and do another level of checking.

**Katja Grace:**
Yeah. I think that was part of it. I think I don't quite understand why it is that writing papers take so long compared to writing blog posts or something. It seems like in my experience it does, even with a similar level of quality in some sense. So I was partly hoping to just do things where at least the norm is something like, okay, you're doing the valuable bit of some sort of looking things up or calculation, then you're writing that down and then you're not spending six months, whatever else it is I do when I spend six months straight writing a paper nicely.

**Daniel Filan:**
Yeah. Yeah. It does seem easier to me. I think maybe I care a lot more about the... Let's see, when I write a paper, I don't know how it is in your subfield, but in my subfield in machine learning, papers sort of have to be eight pages. They can't be eight pages and one line. So there's some amount of time being spent on that. Yeah. When I write blog posts, they're just worse than the papers I write and I just spend 30 minutes writing the first thing I thought of. And this is especially salient to me now because a few days ago I published [one](https://www.lesswrong.com/posts/m5frrcYTSH6ENjsc9/challenge-know-everything-that-the-best-go-bot-knows-about) that got a lot of pushback that seems to have been justified. Yeah. There does seem to be something strange.

**Katja Grace:**
I think another big difference is that we're often trying to answer a particular question and then we're using just whatever method we can come up with, which is often a bad method, because there's just no good way of answering the question really well. Whereas, I think academia is more methods oriented. They know a good method and then they find questions that they can answer with that method. At least it's my impression.

**Daniel Filan:**
This is not how it's supposed to work. My advisor Stuart Russell, has the take that, look, the point of a PhD, and I think maybe he would also say the point of a research career, is to answer questions about artificial intelligence and do the best job you can. And it's true that people care more... I think people do care more about good answers to bad questions than bad answers to good questions. Stuart's also a little bit unusual in his views about how PhD theses should be.

**Katja Grace:**
I'm probably also being unfair. It might be that in academia, you have to both - you're at least trying to ask a good question and answer it well, whereas we're just trying to answer a good question.

**Daniel Filan:**
Yeah. I think there's also this thing of, who's checking what? In academia, most of the checking is, did you answer the question well? When your paper goes through peer review, nobody says, the world would only be $10 richer if you answered this question. They sometimes talk about interest, which is kind of a different thing. So those are some differences between AI Impacts and academia. Do more come to mind?

**Katja Grace:**
I guess maybe one that is related to your saying it's not well integrated, I guess I think of academia as being not intended to be that integrated in that it's sort of a bunch of separate papers that are kind of referring to each other maybe, but not in any particular - there's not really a high level organizing principle or something. Whereas, I guess with AI Impacts, it somewhat grew out of a project that was to make really complicated [Workflowys](https://workflowy.com/) of different arguments for things.

**Daniel Filan:**
What's a Workflowy?

**Katja Grace:**
Oh, it's software for writing bulleted lists that can sort of go arbitrarily deep in their indenting.

**Daniel Filan:**
The levels of indentation.

**Katja Grace:**
Right. And you can sort of zoom in to further-in levels of indentation, and just look at that. So you can just have a gigantic list that is everything in your life and so on, but I guess Paul Christiano and I were making what we called structured cases that were supposed to be arguments for, for instance, AI is risky or something, or AI is the biggest problem or something. And the idea was there'd be this top level thing, and then there'll be some nodes under it. And it would be yeah, if you agree with the nodes under it, then you should agree with the thing above it. And so you could look at this and be, oh, I disagree with the thing at the top, which of the things underneath do I disagree with? Ah, it's that one. All right, what do I disagree with under that, and so on.

**Katja Grace:**
This is quite hard to do in WorkFlowy somehow. And it was very annoying to look at. And so, yeah, I guess one idea was to do a similar thing where each node is a whole page, where there's some kind of statement at the top and then the support for it is a whole bunch of writing on the page. Instead of that just being a huge jumble of logically, carefully related statements. And so I guess, AI Impacts is still somewhat like that. Though not super successfully at the top in that it's missing a lot of the top level nodes that would make sense of the lower level nodes.

**Daniel Filan:**
Is that roughly a comprehensive description of the differences between AI Impacts and academia? AI Impacts is much smaller than academia.

**Katja Grace:**
Also true. I imagine it's not a comprehensive list, but it'll do for now.

## What AI x-risk researchers do wrong <a name="x-risk-researcher-mistakes"></a>

**Daniel Filan:**
All right. So I guess the final question that I'd like to ask, oh, I forgot about one... Another one is, by working on AI Impacts you sort of look in to a bunch of questions that a lot of people, I guess, have opinions on already. What do you think people in AI x-risk research are most often, most importantly wrong about?

**Katja Grace:**
I'm pretty unsure, and places where it seems like people are wrong, I definitely don't have the impression that, obviously they're wrong rather than, I'm confused about what they think. I think we have a sort of meta disagreement maybe where I think it's better to write down clearly what you think about things than many people do, which is part of why I don't know exactly what they think about some things. Yeah. I mean, it's not that other people don't ever write things down clearly, but I guess it's a priority for me to try and write down these arguments and stuff, and really try and pin down whether they're sloppy in some way. Whereas, it seems like in general, there's been less of that than I would have expected overall, I think.

**Daniel Filan:**
I mean, I think people write a lot of stuff. It's just about sort of taking a bunch of things for granted.

**Katja Grace:**
Or maybe the things that were originally laying out the things that are being taken for granted, were not super careful, more evocative, which is maybe the right thing for evoking things. Maybe that was what was needed. But yeah, I guess I'm more keen to see things laid out clearly.

## How to follow Katja's and AI Impacts' work <a name="following-katja-ai-impacts"></a>

**Daniel Filan:**
All right. So if people are interested in following you and your research and AI Impacts' research, how can they do so?

**Katja Grace:**
Well, there's the [aiimpacts.org](https://aiimpacts.org/) website is a particularly good way to follow it. I guess we have a [blog](https://aiimpacts.org/category/blog/) which you can see on the website. So you could subscribe to that if you want to follow it, I guess otherwise we just sort of put up [new pages](https://aiimpacts.org/tag/reference/) sometimes some of which are interesting or not, which you can also subscribe to see if you want. If you just want to hear more things that I think about stuff. I have a blog, [worldspiritsockpuppet.com](https://worldspiritsockpuppet.com/).

**Daniel Filan:**
All right, great. Thanks for appearing on the show. And to the listeners, I hope you join us again.

**Katja Grace:**
Thank you for having me.

**Daniel Filan:**
This episode is edited by Finan Adamson. The financial costs of making this episode are covered by a grant from the [Long Term Future Fund](https://funds.effectivealtruism.org/funds/far-future). To read a transcript of this episode, or to learn how to support the podcast, you can visit [axrp.net](https://axrp.net/). Finally, if you have any feedback about this podcast, you can email me at <feedback@axrp.net>.
