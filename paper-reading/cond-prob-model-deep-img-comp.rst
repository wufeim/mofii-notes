Conditional Probability Models for Deep Image Compression
=====================================

| **Year:** Jan 2018
| **Authors:** Fabian Mentzer, Eirikur Agustsson, Michael Tschannen, Radu Timofte, Luc Van Gool
| **Affiliations:** ETH Zurich

Recently, DNNs trained as image auto-encoders led to promising results in image compression. A key challenge in training such systems is to optimize the bitrate :math:`R` of the latent image representation in the auto-encoder. After discretization, we measure the bitrate :math:`R` with the entropy :math:`H` of the resulting symbols. Since discretization is non-differentiable, this presents challenges for gradient-based optimization methods.

In this paper, the authors propose a new method based on leveraging context models as an entropy term in the optimization. Experiments show that this approach yields SOTA results when measured in MS-SSIM.

Method Overview
-------------------------------------

Given a set of training images :math:`\mathcal{X}`, we wish to learn a compression system which consists of an encoder, a quantizer, and a decoder. The encoder :math:`E: \mathbb{R}^d \to \mathbb{R}^m` maps an image :math:`\mathbf{x}` to a latent representation :math:`\mathbf{z} = E(\mathbf{x})`. The quantizer :math:`Q: \mathbb{R} \to \mathcal{C}` discretizes the coordinates of :math:`\mathbf{z}` to :math:`L = \lvert \mathcal{C} \rvert` centers, obtaining :math:`\hat{\mathbf{z}}` with :math:`\hat{z}_i = Q(z_i) \in \mathcal{C}`. The decoder :math:`D` then forms the reconstructed image :math:`\hat{\mathbf{x}} = D(\hat{\mathbf{z}})`.

We want the encoded representation :math:`\hat{\mathbf{z}}` to be compact, while at the same time we want the distortion :math:`d(\mathbf{x}, \hat{\mathbf{x}})` to be small. This results in the rate-distortion trade-off

.. math::

   d(\mathbf{x}, \hat{\mathbf{x}}) + \beta H(\hat{\mathbf{z}})

This system is realized by modeling :math:`E` and :math:`D` as the encoder and decoder of a CNN auto-encoder.

Quantization
-------------------------------------

Given centers :math:`\mathcal{C} = \{c_1, \dots, c_L\} \subset \mathbb{R}`, we use the nearest neighbor assignments to compute

.. math::

   \hat{z}_i = Q(z_i) = \text{argmin}_j \lVert z_i - c_j \rVert

but rely on (differentiable) soft quantization

.. math::

   \tilde{z}_i = \sum_{j=1}^L \frac{\exp(-\sigma \lVert z_i - c_j \rVert)}{\sum_{l=1}^L \exp(-\sigma \lVert z_i - c_j \rVert)}c_j

to compute gradients during the backward loss.
