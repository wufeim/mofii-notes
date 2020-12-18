Deep Generative Video Compression
=====================================

| **Authors:** Jun Han, Salvator Lombardo, Christopher Schroers, Stephan Mandt
| **Affiliations:** Dartmouth College, Disney Research

In the context of image compression, deep learning approaches have already shown promising results. Motivated by these successes, the authors propose a step towards innovating beyond block-based hybrid codec by framing video compression in a deep generative modeling context.

They propose an unsupervised deep learning approach to encoding video that learns the optimal transformation of the video to a lower-dimensional representation and a powerful predictive model that assigns probabilities to video segments, allowing us to efficiently entropy-code the discretized latent representation into a short code length.

In this work, the authors propose an end-to-end deep generative modeling approach to compress temporal sequences with a focus on video. The approach jointly learns to transform the original sequence into a lower-dimensional representation as well as to discretize and entropy code this representation according to predictions of a sequential VAE.

Rate-distortion evaluations show that this model yields competitive results when trained on generic video content. Extreme compression performance is achieved when training the model on specialized content.
