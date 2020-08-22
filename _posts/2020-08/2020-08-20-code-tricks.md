---
layout: post
title: Some Tricks about TensorFlow PyTorch Python and C++
---

Here I share some tricks about TensorFlow PyTorch Python and C++ to speedup coding and developing.

# TensorFlow 1.x

## 1. Print variables and shape of weight in the saved checkpoint

``` BASH
python -m tensorflow.python.tools.inspect_checkpoint --file_name <model ckpt>
```

