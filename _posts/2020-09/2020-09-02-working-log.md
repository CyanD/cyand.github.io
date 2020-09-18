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

*

## Week 1 Summary

* Compared the auto-regressive decoder and none auto-regressive one. Both of them could converge.
* The duration model couldn't be well trained with only 1000 utterances, and reduced the quality of the generated speech.
* Change the input format of model from concatenation of phoneme-tone-prosody embedding, into skip encoder, and then into sum. The sum of phoneme-tone-prosody embedding performed the best, since there's no information loss and the tone/prosody information was better fused into the phoneme embeddings.
* Prepare for the oral defense.

# Week 2

## 2020/09/07 Mon.

* Sum of phoneme tone and prosody embedding still generated bad speech

## 2020/09/08 Tue.

* Analysis the reason why generated speech is not good.

## 2020/09/09 Wed.

* Start to review the espnet code

## 2020/09/10 Thu.

* Fix the PTTS custom op integration bug

## 2020/09/11 Fri.

* OP integration bug

## Week 2 Summary

* Pause the progress of DurationTacotron model, and turn to use the espnet
* Fixing the custom OP integration error

# Week 3

## 2020/09/14 Mon.

* Test r=1
* Apartment purchase contract is delivered to bank
* The shape generation paper

## 2020/09/15 Tue.

* Train model with r=1, and concatenation of inputs. This produced good result.
* Improve GMM attention with duration as mu.

## 2020/09/16 Wed.

* Target user recording data preprocess
* Adaptive training on target speakers

## 2020/09/17 Thu.

* Adaptive training experiment on target speakers: Good result before adaptation, but poor result after.
* Patent writing.

## 2020/09/18 Fri.

* Analysis the potential problem in the data of target speakers

## Week 3 Summary

* Experiment r=1; Change input format into concatenation
* Process three male and three female target speaker data
* Adaptive training experiment on target speakers
