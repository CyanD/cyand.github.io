---
layout: post
title: September, 2020
---

# Week 1

## 2020/09/01 Tue.

* The generated speech from skip encoder is good at the begin, but bad the the end.
* None auto-regressive training did not produce good result.
* Prepare slides for ranking promotion

## 2020/09/02 Wed.

* Prepare slides for ranking promotion
* Try replace skip encoder with sum of phoneme, tone and prosody embedding

## 2020/09/03 Thu.

* Oral defense
* Add preprocess to dump extracted mel to disk to save CPU memory

##  2020/09/04 Fri.

## Week 1 Summary

* Compared the auto-regressive decoder and none auto-regressive one. Both of them could converge.
* The duration model couldn't be well trained with only 1000 utterances, and reduced the quality of the generated speech.
* Change the input format of model from concatenation of phoneme-tone-prosody embedding, into skip encoder, and then into sum. The sum of phoneme-tone-prosody embedding performed the best, since there's no information loss and the tone/prosody information was better fused into the phoneme embeddings.
* Prepare for the oral defense.
