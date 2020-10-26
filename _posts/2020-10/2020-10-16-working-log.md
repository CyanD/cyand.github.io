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

# Week 3

## 2020/10/19 Mon.

* Process the Chinese-English mixed ASR dataset
* Experiment on Weight constrain: limit the fine-tuned parameters to be close to the base model's parameters

## 2020/10/20 Tue.

* Process the Chinese-English mixed ASR dataset: build the relationship of ASR phoneme and TTS phoneme

## 2020/10/21 Wed.

* Optimize the load procedure of PTTS: Only fine-tuned parameters are exported into the pb model

## 2020/10/22 Thu.

* Both source and fine-tuned model are exported to tf pb format. 
* Only restore and overwrite fine-tuned parameters and keep others untouched when switching ptts model
* Ptts model size 1MB, base model size 80MB; base model loading time 630ms, ptts model loading time 90ms.

## 2020/10/23 Fri.

* Loading pb model in C++ code.

# Week 4

## 2020/10/16 Fri.

* [ ] HSBC bank account
    - [ ] China
    - [ ] HK
    - [ ] Ca
    - [x] Bank Statement
* [ ] Stock
* [x] Credit Card Payment
    - [x] HSBC
    - [x] CITIC
    - [x] ICBC
* [ ] Patent
* [ ] Paper
* [ ] English Learn
    - [ ] Vocabulary
    - [ ] Reading
* [ ] Pb Model loading and switching in C++
    - [ ] Load source model
    - [ ] Load ptts model
    - [ ] Overwrite fine-tuned parameters
