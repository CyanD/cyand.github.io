---
layout: post
title: Some deep learning for TTS basics
---

Some basic techniques about deep learning especially for TTS.

# 1. Highway network

The advantage of a highway network is that solves or prevents partially the vanishing gradient problem, thus leading to easier to optimize the network. 

Equation

![img](https://wikimedia.org/api/rest_v1/media/math/render/svg/74407197b19e4998cb1230ff6412769ef18c9584)

Code

``` Python
class HighwayNet:
  def __init__(self, units, name=None):
    self.units = units
    self.scope = 'HighwayNet' if name is None else name
    self.H_layer = tf.layers.Dense(units=self.units, activation=tf.nn.relu, name='H')
    self.T_layer = tf.layers.Dense(units=self.units, activation=tf.nn.sigmoid, name='T')
  def __call__(self, inputs):
    with tf.variable_scope(self.scope):
      H = self.H_layer(inputs)
      T = self.T_layer(inputs)
      return H * T + inputs * (1. - T)
```

# Reference

1. https://en.wikipedia.org/wiki/Highway_network
2. xx
