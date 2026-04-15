+++
title="Preprint: In-Place Test-Time Training"
date=2026-04-15
draft=false
[taxonomies]
tags=["blog", "preprint", "ai", "machine intelligence", "llms", "inference", "In-Place-TTT"]
+++

# A new milestone?

## Introduction

This post is about the following preprint: [In-Place Test-Time Training](https://doi.org/10.48550/arXiv.2604.06169)

However, this is not a "review" type of post. I don't really believe in those things -- though I will outline some points that I personally find interesting. My underlying rationale is that each of us is unique and, though having the summary/write-up by someone else helps; it conveys a personal perspective which through my numerous readings lead me to conclude that they are often too "flawed/biases" with respect to the original authors of any piece of work -- one can also see this as a game of minimizing/maximizing mutual information between one/other.

Maybe the best example to further outline my argument is through *Precision/Personalized Medecine*; the **only** interesting thing about *generics* are their democratic prices. This is where AI's potential can make things happen that were not possible before. On a **conceptual** and **potential** level this may echo something like: [Personal SuperIntelligence](https://www.meta.com/superintelligence/)

## Core of the Matter

I write these few lines because I am concerned and work (among other things) by the whole *AI Safety / Alignment* thing. I called it *thing* because, we kind of feel what these are supposed to be without being able to articulate a proper/formal definition(s) about them.

Anyhow, in the great scheme of things, I believe we have hit a new milestone with this paper.
What do I mean by that?

A number of persons have said it but I'll refer to Kai-Fu Lee saying something about the change in magnitude that AI can be the new [*electricity*](https://www.goodreads.com/author/quotes/1549627.Kai_Fu_Lee).

### Breath in Breath Out

One of the main points developed in the paper is that the LLM ecosystems are ''fixed'' or *static*. This is a notion that I have been pondering upon since GPT-2. I believe this paper (and possibly others before?) paves the way to a new power dynamic. I like to think of it from switching to DC to AC. The idea came to my mind when seeing the following device: a *mercury arc rectifier* -- what matters are the concepts.

[![Mercury Arc Rectifier](https://img.youtube.com/vi/Z-YW8Sxj4_8/0.jpg)](https://www.youtube.com/watch?v=Z-YW8Sxj4_8)

This new *power dynamics* which is dynamical in the sense that the authors propose to use one of the Transformer-neural architecture(TNA) component i.e. the last MLP blocks precisely for dealing with transient information/memory.

Another analogy would perhaps be from computer communication networks i.e. moving from *simplex* to *half-duplex* channel communications.?

It also interesting to note that this evolution in TNA follows the development of Large Language Reasoning models i.e. in a way to push forward their capabilities.

## Some Technical Elements

The *static* aspect can roughly be described as follows: LLMs are trained on a substantial amount of data, they acquire consequential knowledge that we find it very useful and productive to interact with/use. This knowledge is mainly stored as weights, where reasoning may happen or/and through the use of Chain-Of-Thought. Though, I would like to draw the *attention* that the architecture *matters*and has weight too.

Incidentally,

- I believe the following reference for understanding multi-token prediction is interesting: [Better & Faster Large Language Models via Multi-token Prediction](https://doi.org/10.48550/arXiv.2404.19737)

- The notion of *context parallelism* described in the paper is of interest.


## Conclusion


Please do note that I purposely don't get into the granularities and intricacies of what might or might not be going with the training of LLMs/LRMs. I started the project: [Emergent Cognition](https://emergentcognition.org/) to investigate this.

Finally, I would like to say that there may very well be prior work(s) on providing a way to ''unfreeze'' or render LLMs/LRMs more dynamic but I happen to came upon this one recently (and didn't perform a systematic review) and the timing *feels right*.


## References

Vizuara AI Labs: [Build DeepSeek from Scratch](https://www.youtube.com/watch?v=QWNxQIq0hMo&list=PLPTV0NXA_ZSiOpKKlHCyOq9lnp-dLvlms)

[Chain of Thought Monitorability: A New and Fragile Opportunity for AI Safety](https://doi.org/10.48550/arXiv.2507.11473)
