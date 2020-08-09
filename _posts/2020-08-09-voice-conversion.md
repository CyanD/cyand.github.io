---
layout: post
title: A simple overview about some voice conversion papers
---

# Voice Conversions
Voice conversion (VC) is a technique to modify the speech from source speaker to make it sound like being uttered by a target speaker while keeping the linguistic content unchanged.
Many methods of VC require the target speaker has many speech data for training. However, one-shot VC just need one utterance from the target speaker.  
According to the number of source and target speakers, VC system can be classified as one-to-one, one-to-many, many-to-one, and many-to-many systems. 

## A Compact Framework for Voice Conversion Using Wavenet Conditioned on Phonetic Posteriorgrams
Paper [2] is from professor Zhiyong Wu, Tsinghua University, China.
There are many common structures to their following paper [1].  The WaveNet vocoder, PPG network, speaker encoder, and the idea of using conditioning are nearly the same.
### Background
Many approaches  of VC systems segregate the training procedure of conversion and vocoder module, with different optimization objectives, which may lead to the difficulty in model tuning and coordination.
### Contribution
Paper [2] proposed a compact framework to unify the conversion and vocoder parts.
### Methods
The WaveNet vocoder is employed to generate the speech of target speaker, conditioning on the intermediate representation of PPGs. 
The PPGs representation is extracted by multi-head attention structure and BLSTM. 
The method to unify the conversion function and the vocoder is:
1. Extract speaker independent linguistic features from the source speaker’s speech;
2. Encode the linguistic features and the f0 using self-attention structures and BLSTM;
3. Up-sampling the encoded features into the desired resolution of WaveNet to generate the target speaker’s waveform.
### Experiment
Baseline is a separately trained conversion module and WaveNet vocoder. 
The baseline method [3] uses phonetic posterior-grams to do many-to-one voice conversion, who architecture is depicted in the following figure:
![ppgs for vc](https://raw.githubusercontent.com/CyanD/cyand.github.io/master/assets/image/voice_conversion/ppg_vc.png)
### Details
Phonetic posterior-grams (PPGs) are generally considered as speaker independent features containing linguistic information. A PPG is a time sequence representing the posterior probability of each [senone]([What are the senones in a Deep Neural Network? - Cross Validated](https://stats.stackexchange.com/q/342497)) for each time frame of an utterance. The number of frames each senone lasts reveals the duration information. The PPGs can be extracted from utterance using ASR method. The PPGs can be easily converted into conrrespoding phoneme or even word sequence, but commonly it’s bettor not to do that for two reasons: 1) PPGs can capture the composition of different senons and its temporal changes, which can obtain more accurate estimates of acoustic parameters; 2) The duration information is damaged when mapping posterior probability into phoneme sequence [2].

### Findings
The proposed method can achieve betters results in naturalness and similarity.
## One-shot Voice Conversion with Global Speaker Embeddings
Paper [1] is from  professor Zhiyong Wu,Tsinghua University, China.
### Background
Building a VC system for a new target speaker requires a large amount of speech data from the target speaker.
### Contribution
Paper [1]  proposed a method to build a VC system using just one utterance from an arbitrary target speaker, and no need any adaptation training. 
### Methods
They proposed to use global speaker embeddings (GSE), which is inspired from global style tokens (GST), to control the converted targets speech. Besides that, speaker-independent phonetic  posterior-grams (PPG) are employed as the local conditions.
To generate the waveform of the target speaker, the conditional WaveNet synthesizer is used. The GSE and PPG are the global and local conditioning of the WaveNet. 
To get the embeddings  of target speakers, the utterance of target speaker is firstly fed into a speaker encoder network to generate the reference embedding, which is then employed as attention query to the GSEs  to produce the speaker embedding. The procession is exactly to the GST vectors.
### Experiment
Baseline method is an adaptation training baed any-to-any VC system.
### Findings
The proposed method performs equally well or better in both speech naturalness and speaker similarity. 
The proposed method has higher flexibility than the baseline.
### Details


# References
1. One-shot Voice Conversion with Global Speaker Embeddings, https://www.isca-speech.org/archive/Interspeech_2019/pdfs/2365.pdf
2. A Compact Framework for Voice Conversion Using Wavenet Conditioned on Phonetic Posteriorgrams. https://ieeexplore.ieee.org/document/8682938
3. Phonetic posteriorgrams for many-to-one voice conversion without parallel data training. https://ieeexplore.ieee.org/document/7552917
