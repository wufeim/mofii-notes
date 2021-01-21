Representation Learning with Contrastive Predictive Coding
=====================================

| **Year:** Jul 2018
| **Authors:** Aaron van den Oord, Yazhe Li, Oriol Vinyals
| **Affiliations:** DeepMind

One of the most common strategies for unsupervised learning has been to predict future, missing or contextual information. The idea of predictive coding is one of the oldest techniques in signal processing for data compression. **The authors hypothesize that this approach is fruitful partly because the context from which we predict related values are often conditionally dependent on the same shared high-level latent information. And by casting this as a prediction problem, we automatically infer these features of interest to representation learning.**

In this work, the authors propose a universal unsupervised learning approach to extract useful representations from high-dimensional data, called Contrastive Predictive Coding. The key insight is to learn such representations by predicting the future in latent space by using powerful autoaggressive models.

The authors demonstrate that their approach is able to learn useful representations achieving strong performance on four distinct domains: speech, images, text, and reinforcement learning in 3D environments.

Motivation and Intuitions
-------------------------------------

The main intuition behind this model is to learn the representations that encode the underlying shared information between different parts of the (high-dimensional) signal. At the same time it discards low-level information and noise that is more local.

Images may contain thousands of bits of information while the high-level latent variables such as the class label contain much less information (10 bits for 1024 categories). This suggests that modeling :math:`p(x \mid c)` directly may not be optimal for the purpose of extracting shared information between :math:`x` and :math:`c`.

Whey predicting future information, they encode the target :math:`x` (future) and context :math:`c` (present) into a compact distributed vector representations in a way that maximizes the mutual information defined as

.. math::

   I(x; c) = \sum_{x, c} p(x, c) \log \frac{p(x \mid c)}{p(x)}

By maximizing the mutual information between the encoded representations, we extract the underlying latent variables the inputs have in common.
