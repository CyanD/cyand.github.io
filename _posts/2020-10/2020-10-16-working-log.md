---
layout: post
title: October, 2020
---

# Week 1

## 2020/10/09 Fri.

* Simplify the durtaco model to cnn-lstm-cnn-lstm structure: more noise in the samples
* Remove BLSTM layer in durtaco to support streaming synthesis

## 2020/10/10 Sat.

* Test the No_blstm durtaco model performance on the target speaker: cpu rtf in [0.2, 0.27] using tf checkpoint

# Week 2

## 2020/10/12 Mon.

* As the most simplified model produced noise result, let's simplify the model gradually.

## 2020/10/13 Tue.

* Compare the quality between the models with and without postnet: there's no obvious difference in the quality of synthesized speech.
* Export the no_blstm model to tf pb format and test it's performance on CPU: rtf=0.065.

## 2020/10/14 Wed.

* Modify the no_blstm code to support streaming inference.
* Test the no_blstm model in a streaming mode, found that it's rtf=0.16, almost 2.5 times than the none-steaming mode.

## 2020/10/15 Thu.

* Statistic the model inference performance. None-streaming mode: rtf=0.065, first frame latency=250ms; Streaming mode: rtf=0.15, latency=30ms.
* Process the test text and generate the speech for MOS test.
* Regulate the finetuned parameters to be close to the source model: add l2 loss of the finetuned parameters.
* English word pronunciation is not good as there's little English in the training data.

## 2020/10/16 Fri.

* Check quality of the ASR dataset
