---
layout: post
title: Some basic signal processing methods for TTS and ASR
---

Many papers and code implementations usually use some signal processing methods to enhance the performance TTS and ASR. Here are some must-known algorithms. One may be familiar with these terminology names as they are commonly mentioned in the papers and occurred in codes, but unfamiliar with the their formula and details, and when why how do they work. 

1. MLPG

2. pre-emphasis

``` python
x = librosa.core.load(wav_path, sr=16000)[0]
signal.lfilter([1, -hparams.preemphasize], [1], x)
```

3. de-emphasis

``` python
x = signal.lfilter([1], [1, -hparams.preemphasize], x)
```

4. MFCC

5. 
