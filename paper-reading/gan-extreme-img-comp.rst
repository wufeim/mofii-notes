Generative Adversarial Networks for Extreme Learned Image Compression
=====================================

| **Authors:** Eirikur Agustsson, Michael Tschannen, Fabian Mentzer, Radu Timofte, Luc Van Gool
| **Affiliations:** ETH Zurich

Deep compression systems are typically optimized for traditional distortion metrics such as peak signal-to-noise ratio (PSNR) or multi-scale structural similarity (MS-SSIM). For very low bitrates, these distortion metrics lose significance as they favor pixel-wise prreservation of local (high-entropy) structure over preserving texture and global structure. To further advance deep image compression it is therefore of great importance to develop new training objectives , such as adversarial losses.

The authors prpose a principled GAN framework for full-resolution image compression and use it to realize an extreme image compression system, targeting bitrates below 0.1 bpp. The authors consider two modes of operation, namely
  - **generative compression (GC):** preserving the overall image content while generating structure of different scales
  - **selective generative compression (SC):** completely generating parts of the image from a semantic label map

For GC, a comprehensive user study shows that the proposed compression system yields visually considerably more appealing results than BPG and AEDC.

Deep Image Compression
-------------------------------------

To compress an image :math:`\mathbf{x} \in \mathcal{X}`, one learns an encoder :math:`E`, a decoder :math:`G`, and a finite quantizer :math:`q`. The encoder :math:`E` maps the image to a latent feature map :math:`\mathbf{w}`, whose values are then quantized to :math:`L` levels to obtain a representation :math:`\hat{\mathbf{w}} = q(E(\mathbf{x}))`. A differentiable relaxation of :math:`q` can be used to allow backpropagation.

The rate-distortion trade-off between reconstruction quality and bitrate is

.. math::

   \mathbb{E}[d(\mathbf{x}, \hat{\mathbf{x}}] + \beta H(\hat{\mathbf{w}})

Since the entropy is bounded by

.. math::

   H(\hat{\mathbf{w}}) \leq \text{dim}(\hat{\mathbf{w}}) \log_2(L)

It is also valid to set :math:`\beta = 0` and control the maximum bitrate through the bound. While potentially leading to suboptimal bitrates, this avoids to model the entropy explicitly as a loss term.
