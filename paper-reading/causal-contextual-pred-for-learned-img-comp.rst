Causal Contextual Prediction for Learned Image Compression
=====================================

| **Year:** Nov 2020
| **Authors:** Zongyu Guo, Zhizheng Zhang, Runsen Feng, Zhibo Chen
| **Affiliations:** University of Science and Technology of China

To capture spatial dependencies in the latent space, prior works exploit hyperprior and spatial context model to build an entropy model, which estimates the bit-rate for end-to-end rate-distortion optimization. However, this entropy model is suboptimal from two aspects:

- It fails to capture global-scope sptial correlations among the latents.
- Cross-channel relationships of the latents remain unexplored.

In this paper, the authors propose the concept of separate entropy coding to leverage a serial decoding process for causal contextual entropy prediction in the latent space. The model is composed of a *causal context model* and a *causal global prediction model*.

Experimental results demonstrate that this model outperforms standard VVC/H.266 codec on Kodak dataset in terms of both PSNR and MS-SSIM.
