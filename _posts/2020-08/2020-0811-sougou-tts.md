---
layout: post
title: Sougou Text-to-Speech Technique
---

This post is based on the sougou public speech [1].

They referred 5 papers [2, 3, 4, 5, 6].

# Sougou StyleTTS

![sougou style tts](/assets/image/public_speech/ccf/sougou_style_tts.png)

The whole architecture of styleTTS is based the encoder-decoder structure. Some sub-modules are added to enhance the prosody of speech, including duration prediction, speaker/language embedding, VAE, and GRL. The encoder module is shared with duration prediction.

The attention mechanism in the seq2seq structure is replace by duration, which could improve the stability of the model. The speaker/language embedding is used to support multi-speaker and multi-language synthesis. The VAE learns the style and residual information, which makes the model focus on the common parts between different speakers and languages. As the different speaker in the multi-speaker TTS corpus may have different coverage of phoneme, especially the Erhua and English words, the gradient reverse layer (GRL) is used to make the encoder module learn common linguistic feature rather than the speaker-dependent information.

## With Reference Audio

When modifying the source speech to have a target style, they use style encoder to extract the reference style features from the reference speech.

## Without Reference Audio

If there's no reference speech, they add a style prediction module to predict the style from encoder hidden states.

![image](/assets/image/public_speech/ccf/no_reference_audio.png)

The loss the style prediction is from a GAN model, rather than directly a simple classification loss.

# High Sampling Rate Synthesis

Humans are sensitive to the sampling rate change. The 32k sampling rate speech can give people better auditory experience than 16k. Sogou introduced their method for high sampling rate synthesis.

![image](/assets/image/public_speech/ccf/high-sr-tts.png)

The acoustic model is baed on the autoregressive seq2seq, in which the attention is replace by duration prediction. The duration model is trained using the distillation technique.

The vocoder model is based on multi-band MelGAN model [7].

# Many-to-One Voice Conversion

Although style TTS can synthesis given style and accent speech, there are still some limitation to synthesize a speech of the target speaker satisfying any fine-level controlling. They introduced their voice conversion method to transfer the source speech to a target speaker while keeping the linguistic content unchanged and remaining the high expressiveness. 

![image](/assets/image/public_speech/ccf/vc.png)

The model is based on the ASR-TTS architecture. The VAE is a reference encoder used to extract prosody features of source speech. The ASR model extract the content (linguistic) features, followed by a bottleneck feature layer to further eliminate the speaker related information. Speaker embedding is added to extract the characteristic of the target speaker. And vocoder is WaveRNN.

The voice conversion method is also used to transfer accent, for example from Mandarin Chinese to Sichuan/Taiwan accent.

# Personalized TTS

Personalized TTS want to do voice cloning using just 3-5 minutes data recorded by user in a noisy environment.

![image](/assets/image/public_speech/ccf/ptts.png)

## Challenge

* noisy in user recoded speech
* data size is small, only 3-5 minutes or even less
* user recording has accents
* adaptation time limit

# Reference

1. 搜狗AI交互技术、语音识别、多模态合成技术的研究进展及应用丨CCF语音对话与听觉专业组. https://www.bilibili.com/video/av841742753/
2. Style Tokens: Unsupervised Style Modeling, Control and Transfer in End-to-End Speech Synthesis. https://arxiv.org/abs/1803.09017
3. Disentangling Correlated Speaker and Noise for Speech Synthesis via Data Augmentation and Adversarial Factorization. https://research.google/pubs/pub47834/
4. Learning to Speak Fluently in a Foreign Language: Multilingual Speech Synthesis and Cross-Language Voice Cloning. https://arxiv.org/abs/1907.04448
5. Lei He, Neural Text-to-Speech @Microsoft, CCF 2019
6. Zhiyong Wu, One-shot voice conversion, CCF 2019
7. Yang G , Yang S , Liu K , et al. Multi-band MelGAN: Faster Waveform Generation for High-Quality Text-to-Speech[J]. 2020.
