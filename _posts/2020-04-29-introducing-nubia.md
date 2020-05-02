# Introducing NUBIA

# Introducing NUBIA: A new way to evaluate text. 

### [View Paper](https://arxiv.org/abs/2004.14667) | [Repo](https://github.com/wl-research/nubia) | [Notebook](https://colab.research.google.com/drive/1_K8pOB8fRRnkBPwlcmvUNHgCr4ur8rFg) | [FAQ](https://github.com/wl-research/nubia/blob/master/FAQ.md) | [Back](https://wl-research.github.com/blog/)

## TLDR: 

We have designed a fully neural paradigm to build evaluation metrics for text generation tasks (image captioning, machine translation, summarization). NUBIA is interpretable, modular, largely outperforms currently used metrics and slightly exceeds/matches state-of-the-art as far as correlation with human judgment. It's a continuously improving metric and we believe it can greatly aid progress in text generation tasks.

![](https://wl-research.github.io/blog/images/birds-nubia.png)


## Why: 

When we first started this project, around spring 2019, the premise was whether one could compare two documents by comparing their summaries. So to figure out whether documents were similar, could you first summarize them and then apply a similarity metric on their summaries? We quickly realized that the bottleneck to this was getting a useful metric for text similarity.  

We didn't think that was a big deal. After all, word vectors, paragraph vectors, bi-directional LSTMs, sentence encoders and, more recently, Transformers have all made substantial amounts of progress when it came to capturing meaning from text. We assumed all it would take was picking a pre-trained architecture, exposing its hidden state after passing the two summaries, computing cosine similarity between the hidden states and voilà! What we had not anticipated was that we would need to design an entire paradigm to build evaluation metrics ourselves.

So we surveyed NLP literature to get up to speed and learn about current metrics used to assess summaries and machine translation. The far majority of the literature relied on BLEU and ROUGE, two metrics based on n-grams overlap that are almost 20 years old. These [metrics](https://en.wikipedia.org/wiki/BLEU) claimed to correlate with human judgement; but the dataset used to measure this correlation was not readily available. 

This is a serious bottleneck. Part of the problem is that there is no real protocol to assess and evaluate metrics for text generation tasks. Why is that important?

Unlike traditional machine learning tasks like classification and regression, text generation (i.e. machine translation, summarization, image captioning) is extremely nuanced and the gold standard here is human evaluation. But we can’t afford that for most models so instead, proxies in the form of machine learning models were designed to approximate human judgment of quality. A consequence of this unique setup is that until we get quasi-human judgment of quality (which we’re still far from) the metrics themselves have to be upgraded every couple of years to reflect the dynamic progress of the field. However, that has not happened and, instead, we’re stuck with metrics that haven't changed since 2002. Given their popularity and simplicity, it has been virtually impossible to dislodge them. 

The question of coming up with a systematic protocol to benchmark metrics became the focus of the team and was the subject of a [paper](https://openreview.net/forum?id=S1xkQac9LB) we presented at the NeurIPS 2019 Document Intelligence workshop. On this one, we were joined by a brilliant NLP Researcher from Boğaziçi University in Turkey who played a huge role in fleshing out this metric scorecard and laying the foundation for NUBIA.

After digging a bit, we also found out that other researchers raised [red flags](https://www.aclweb.org/anthology/P16-1182/) about the need to seriously vet evaluation metrics for language generation tasks. However, the community did not sufficiently pay attention to this problem and still uses ROUGE and BLEU while [complaining](https://twitter.com/ylecun/status/1161900853349048320?lang=en) about it. 

After jotting down thoughts and criteria on how to assess evaluation metrics, we realized that those criteria almost automatically gave a formulation for what a sound metric would be. So we went ahead and built it. 

What is NUBIA? Think of it like an AI system that does one particular job for now (sorry to disappoint AGI folks): given two sentences, reference sentence A and candidate sentence B, it outputs a score between 0 and 1 expressing how much it believes that A is interchangeable with B. 

It does this by comparing pairs of sentences along 3 axes: 

How semantically similar are they?
Is the candidate sentence in logical agreement (Reference implies Candidate), neutral (the candidate sentence can’t be deduced from the reference sentence either because they’re unrelated or the candidate sentence adds extra information not contained in the reference sentence) or in contradiction with the reference sentence?
How legible, or grammatically coherent are the two sentences, individually?

After scoring the sentence pairs along those dimensions, NUBIA computes a “holistic score” between 0 and 1 that captures its impression. We go into a lot more detail about how in our [paper](https://arxiv.org/abs/2004.14667). We've also released our [repo](https://github.com/wl-research/nubia) and this [demo notebook](https://colab.research.google.com/drive/1_K8pOB8fRRnkBPwlcmvUNHgCr4ur8rFg) where you can see many examples and try some of your own.

One thing that continues to surprise us is that even with the *many* criticisms of BLEU and ROUGE, very sophisticated proposals to replace them keep failing to get traction (see the related work section of our paper). The rapid pace of progress in NLP is, in some areas, both currently bottlenecked by language generation evaluation metrics and at the same time offering promising techniques with the potential to overcome these bottlenecks.

We’re excited to contribute to this conversation by releasing our work. 

If you’re an NLP researcher working on machine translation, summarization or image captioning, we’d love to discuss how to incorporate NUBIA into your work. We’re still deeply thinking about the question of how to properly assess/diagnose evaluation metrics and will share more in the coming months. While we think NUBIA is a step in the right direction, we also know it is far from perfect.

We'd love to hear your feedback/questions. We kept this blog post intentionally short on technical details to convey the big picture/motivation behind this work but we suspect that the answers to your questions are in the [paper](https://arxiv.org/abs/2004.14667) or the [repo](https://github.com/wl-research/nubia). You can contact us by email [here](mailto:hassanmohamed@alum.mit.edu) or by opening an issue in our repo.

Thanks for reading!
