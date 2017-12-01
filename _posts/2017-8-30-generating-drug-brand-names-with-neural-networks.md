---
layout: post
title: Generating drug brand names with neural networks
tags:
  - Artificial intelligence
---

Creating new drug brand names is hard. Apart from the general challenge of branding, there's the added challenge of needing a name that physicians, patients and pharmacists won't confuse with existing names—and that won't imply claims not supported by a drug's label. But we live in the age of artificial intelligence. Could it help?

It's a long, laborious, and expensive process to get to an approvable drug name. Drug branding agencies typically generate up to 5,000 names to get to one that's approvable. And the FDA rejects about 20% to 35% of those that are proposed.

Inspired by the work of research scientist and neural network hobbyist [Janelle Shane] (http://lewisandquark.tumblr.com/) (who's used neural networks to generate names for everything from guinea pigs to craft beers), I wanted to find out if AI could make this process any easier.  So I took a stab at training a neural network on drug brand names and letting it loose to create its own.

## Have you taken your Latuna today?

Here are some of my favorites, in alphabetical order, from a list of 100 generated in less than five seconds:

* Amendreve
* Astentin
* Delazan
* Delina
* Elbriz
* Enivir
* Famzero
* Harconta
* Keprofol
* Laiglentia
* Lumamzla
* Lutana
* Riviro
* Sontralin
* Tiblio
* Torivion
* Trevidexre
* Vinapra

## How it works

The names were generated through the use of a recurrent neural network (more specifically: [LSTM](https://keras.io/layers/recurrent/) using [Keras](https://keras.io/) with [Tensorflow](https://www.tensorflow.org/)). This type of neural network can learn sequences of characters, and then predict the probability of a character following other characters. For example, when it sees "queen," "quart," "question," and other words that start with "q," it learns that the letter "u" tends to follow the letter "q." If you then provide it with the letter "q" and ask it to predict the next letter, it would give you "u" with high probability. By doing this repeatedly, it can generate words and sentences. For the list above, I provided it with a list of existing drug brand names, from which it learned generalities about their character sequences.

## Some future ideas to explore

The work to generate this list is a proof of concept, with many limitations. But I hope you agree that it demonstrates the power of neural networks to automate many aspects of the drug commercialization process.

Here are two things I would consider to build upon this work in future:

* __Generate brand names based on generic name associations__. For the list above, the neural network only learned brand names in the absence of generic names. If it could also learn how brand names relate to their generic names, you could provide it with a generic name and have it generate candidate brand names that are more relevant to the generic name.
* __Automate evaluation and improvement of generated names__. For the list above, I selected 18 names from a list of 100. The rest were pretty much garbage. So, while fast, it produces a low ratio of quality names to garbage names. But fear not! For a future step, we could (1) use Amazon Mechanical Turk to evaluate names or (2) better yet, create an adversarial system in which one AI (the generator, used to generate the list above) tries to fool a second AI (the discriminator) with generated names, the discriminator rejects those that don't seem real enough, and the generator learns from that feedback to continuously improve its performance.

So there you have it, the ability to generate hundreds or thousands of drug brand names instantaneously, and a path towards improving the results over time.

_This post originally appeared on the [Klick Health blog](https://www.klick.com/health/news/blog/strategy/generating-drug-brand-names-with-neural-networks/)._
