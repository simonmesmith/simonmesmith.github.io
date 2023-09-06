---
layout: single
title: "PyLitSense: An easy way to try biomedical sentence embeddings"
tags: biomedical-research embeddings sentence-embeddings
---

[Retrieval augmented generation](https://arxiv.org/abs/2005.11401) can ground large language models to improve their response accuracy, recency, and referenceability. This can be particularly important in biomedical research, as you want up-to-date, non-hallucinated, referenced information.

For example, ask ChatGPT something like "Does metformin reduce COVID severity?" Many of the articles on this topic were published after its knowledge cutoff. So to perform best, it needs to search and then use the results to inform its response. And since we don't only want keyword-based results (for example:  we want to know if metformin "lessens," "minimizes," or has other effects like "reduce"), we need to use [sentence embeddings](https://en.wikipedia.org/wiki/Sentence_embedding).

Unfortunately, creating these embeddings on a large number of sentences can be expensive and time-consuming. And there are _billions_ of sentences in biomedical papers. Fortunately, the US National Center for Biotechnology Information created [LitSense](https://www.ncbi.nlm.nih.gov/research/litsense/) to help. It allows you to query against hundreds of millions of sentences from PubMed abstracts, and some full-text articles.

I think this is an underutilized resource. So, to help people explore its potential, I've created the [pylitsense](https://pypi.org/project/pylitsense/) Python package as a wrapper around the LitSense API. Here's how to use it:

## Install

```bash
pip install pylitsense
```

## Use

```python
from pylitsense.pylitsense import PyLitSense

# Initialize
pls = PyLitSense()

# Query
results = pls.query("your query here")

# Print results
for result in results:
    print(result.text, result.score)
```

Try it out, and add any issues or feature requests to the [GitHub repo](https://github.com/simonmesmith/pylitsense).
