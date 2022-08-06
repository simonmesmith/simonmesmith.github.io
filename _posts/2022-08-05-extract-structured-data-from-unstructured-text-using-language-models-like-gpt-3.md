---
layout: post
title:  Extract structured data from unstructured text using language models like GPT-3
tags: coding machine-learning language-models
---

# Extract structured data from unstructured text using language models like GPT-3

Recently I faced a common challenge: extracting structured information from millions of unstructured text documents.

Neither regular expression extraction nor part of speech tagging would scale due to having multiple categories of content and inconsistent phrasing within them. We would have had to write tailored regular expressions or part of speech extractions for every new paragraph topic, and account for a long tail of edge cases.

Having experimented with large language models like [GPT-3](https://beta.openai.com), I was curious as to whether we could simply train one to extract the information we wanted into a structured format like JSON. Then we could validate the JSON and load it directly into BigQuery.

I was thrilled to see how well this work, and if you have access to GPT-3 you can immediately try it yourself. For example, first, enter this one-shot training example:

``` 
John bought a bag of peanuts for $8. He thought they were delicious.
{“person”: “John”, “product”: “peanuts”, “cost”: ”$8”, “sentiment”: “positive”}
```

Then enter some similar examples and see how well GPT-3 manages them.

Example 1:

> “When she arrived at the store, Sarah purchased a bottle of water. It cost $4.50. She was pissed that it was so expensive!” 

GPT-3’s response:

```json
{“person”: “Sarah”, “product”: “water”, “cost”: ”$4.50”, “sentiment”: “negative”}
```

Example 2: 

> “After a long day at work, Frank went shopping for some new clothes. He bought a suit and tie. It cost $1,500. He didn’t mind, as he considered it a cost of doing business.”

GPT-3’s response:

```json
{“person”: “Frank”, “product”: “suit and tie”, “cost”: ”$1,500”, “sentiment”: “neutral”}
```

As you can seem from these two examples, GPT-3 generalizes extremely well.

There’s more work to do to scale this up. But so far it’s quite exciting to find a new way to solve such a common challenge.