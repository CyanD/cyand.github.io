---
layout: post
title: August, 2020
---

# Week i

## 2020/08/07
To make the training and inference phrase of duration-tacotron model is more matching, the input of decoder input at each timestep is changed to use the predicted mel features rather than the teacher forcing method (target mel). 

Why it's possible to use the predicted mel? In duration-tacotron, the attention is not required to be learnt as it's given by duration of phoneme, which simplify the objective of the network. 

However, the experiment result shows that the loss is higher, and the synthesized speech have some pronunciation problem, especially at the end of the speech.

# Week i+1

## 2020/08/10
Even though the loss of duration-tacotron is low enough, the synthesized speech of many speakers are not correct, and only a few samples are correct. 

## 2020/08/11

* Analysis the reason why duration-tacotron generates bad speech.
* Sougou TTS slides