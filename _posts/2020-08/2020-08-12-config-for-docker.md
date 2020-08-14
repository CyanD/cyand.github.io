---
layout: post
title: Config commands for new docker
---

My docker is hosted in a CloudML platform. Sometimes the machine is restarted and all of my configuration is lost. Here are some common-used commands for configurations from scratch.

Following this configuration, one can build a environment with: 

* PyTorch-1.6.0

The purpose of build these environment is to run the following public projects:

* https://github.com/descriptinc/melgan-neurips

``` bash
# config for pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

# apt-get install
apt-get update -y; apt-get install -y libsndfile1-dev
apt-get install -y tmux 
apt-get install -y htop

# pip install
pip install librosa

# install cuda 10.1 based pytorch
pip install torch==1.6.0+cu101 torchvision==0.7.0+cu101 -f https://download.pytorch.org/whl/torch_stable.html
```
