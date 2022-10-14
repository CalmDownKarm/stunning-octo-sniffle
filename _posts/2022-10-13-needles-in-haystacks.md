---
toc: False
layout: post
description: automatically constructing narratives from news articles
categories: [research]
title: Needles in Haystacks
---
I'm working on research that tries to tie together narratives from news articles. At the outset, this sounds like a relatively simple problem, but like all problems, there's more to it than is immediately apparent.

A baseline assumption is that narratives in the news are characterized by a series of events; set to a chronological timeline. Then, we can tie individual events to each other and the broader narrative once we define a relationship.

So the first question becomes, what is an event - and how do we identify that an event has occurred? Initially, we were drawn to the event extraction task, as it's a well-known task in the NLP world, which implies a lot of prior work in this area. However, something that quickly becomes obvious is that there are a lot of different definitions of events, and with each definition, the extraction pipeline changes quite dramatically. In our specific case, it seems helpful to think of events as how laypeople would think of events - something happening in the real world. How the news talks about events usually involve a significant amount of repetition of specific terminology. Thus we can characterize events by certain linguistic signatures that the news uses to talk about these events. So in our thus simplified case, the first problem reduces to extracting these signatures of events from the news articles.

For example, most people are aware of the news about The Mueller investigation of possible collusion between Russia and Trump in the 2016 election. A linguistic signature of that event could be "Trump-Russia inquiry"; it could equally be "Inquiry on Russia and Trump Allies," and it's likely not "Trump campaign ties to Moscow" even though those are terms that are closely related. A quick and easy way to find these sorts of terms is to find words that co-occur in the corpus of articles and calculate a normalized co-occurrence probability of the words across all the pieces in the corpus. By doing this, we can extract all the most common phrases from the corpus of documents. However, this leads us to a specific problem - the candidate phrases will have some distribution. Some ubiquitous phrases will be related to the event; others may not be related to the event but may be widespread phrases that occur in those articles because they are signatures of a closely related event. Lastly, the event-to-phrase mapping isn't 1:1, so there could be multiple phrases that map to the same event. 

So given that we can extract all of these candidate phrases, the next step is to collapse/compress these candidate phrases into specific linguistic signatures of an event and filter out linguistic representations that belong to a particular event. Something we're finding helpful in this step is the idea of semantic similarity/paraphrase mining. Encode the phrases using an appropriate sbert model and see which phrases are the most similar using cosine similarity. This small set of final phrases can be human-annotated for quality. Then we can brute force extract every candidate phrase from the corpus and match each phrase from the signature set against the sentences to find a set of articles that are all closely related to that event. This final set of articles thus represents the coverage of a particular event in the news. 

Once we have the articles for the event, that may solve this task's first problem. Of course, it completely ignores the fact that, at the moment, we have no way of estimating recall - we don't know how many linguistic signatures we're missing because we don't know what we don't know and have no idea how many linguistic signatures are present in the corpus. A solution to that problem would be to annotate a subset and pick the subset reasonably where we can believe that the samples are representative of the corpus at large, but that has its own issues.

However, once we have the article set, we embark upon a much more complex problem - how to structure the events into a narrative.








