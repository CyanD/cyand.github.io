---
layout: post
title: Improve GMM Attention with phoneme duration information for multi-speaker synthesis
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

GMM attention is a type of location-sensitive attention structure, which predicts the alignment location by depending on the current location and conditioning, and the content information has little impacts on the location prediction. In my experiment, GMM Attention mechanism could help Tacotron model get better stability and robustness in long sentence synthesis.

However, when use gmm attention for multi-speaker synthesis, the seq2seq model could generate wrong alignment which leads to skip and repeat error in the synthesized speech. One possible reason is that different speakers have different style to read the same input text, which makes the attention model more difficult to be learnt.

To simplify the attention learning task, lots of papers have proposed some methods, for example, use guided attention loss to limit the alignment range. 

This post proposes a new simple method (called **Dur-GMM Attention**), which leverages the phoneme duration information to regulate the attention model. 

The Dur-GMM attention method has some advantages as follows:

1. The Dur-GMM attention is more stable especially for multi-speaker synthesis, as the duration is used as a regulation.
2. Duration controllable.
3. The Dur-GMM attention could generate better speech in prosody and naturalness.
4. The Dur-GMM attention have less parameters than the normal GMM attention model.

Even though the Dur-GMM attention has the above advantages, we should realize there's one disadvantage of Dur-GMM attention, that a duration prediction model is required.

# Method

## GMM attention

Let's first look at the normal GMM attention, which could be described in the following formula:

$$
\alpha_{i, j} = \sum_{k=1}^K \frac{ \omega_{i, k}}{Z_{i, k}} exp(-\frac{(j- \mu_{i, k})^2}{2( \sigma_{i, k})^2}) 
$$

$$
\mu_i = \mu_{i-1} +  \Delta_i
$$

This approach uses $$K$$ Gaussians to produce the attention weights $$\alpha_i$$. The mean of each Gaussian component is computed using the recurrence relation, which makes the mechanism location-relative and potentially monotonic if $$\Delta_i$$ is constrained to be positive.

And the mixture parameters and intermediate parameters $$( \omega_i, \Delta_i, \sigma_i)$$ are computed from the attention state as follows:

$$
( \widehat{\omega_i}, \widehat{\Delta_i}, \widehat{\sigma_i}) = V tanh (Ws_i+b)
$$

where 

$$
Z_i =  \sqrt{2\pi\widehat{\sigma}_i^2} 
$$

$$
\omega_i = S_{max}(\widehat{\omega_i})
$$

$$
\Delta_i = S_+( \widehat{\Delta}_i)
$$

$$
\sigma_i = S_+( \widehat{\sigma}_i)
$$

In which, the $$s_i$$ is the query vector, and the $$S_+(.)$$ is the soft-plus function, and the S_{max} is the soft-max function. 

For multi-speaker models, the mean of each Gaussian component $$\mu_{i, k}$$ is hard to predict, as the speaker style variance in each speaker. However, the fail of $$\mu_{i, k}$$ prediction would lead to the mis-pronunciation of generated speech including skip or repeat words.

## Dur-GMM Attention

# Tool and Reference

1. Latex editor: http://www.sciweavers.org/free-online-latex-equation-editor
2. Method to enable MathJax rendering: 

``` html
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```

3. LOCATION-RELATIVE ATTENTION MECHANISMS FOR ROBUST LONG-FORM SPEECH SYNTHESIS. https://arxiv.org/pdf/1910.10288.pdf
