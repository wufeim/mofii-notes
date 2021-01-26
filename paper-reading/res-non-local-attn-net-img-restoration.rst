Residual Non-Local Attention Networks for Image Restoration
=====================================

| **Year:** Mar 2019
| **Authors:** Yulun Zhang, Kunpeng Li, Kai Li, Bineng Zhong, Yun Fu
| **Affiliations:** Northeastern Universiyt, Huaqiao University

Image restoration aims to recover high-quality (HQ) images from their corrupted low-quality (LQ) observations.

Previous CNN-based image restoration methods have three limitations:

- the receptive field size is relatively small
- distinctive ability is limited: noise removal is eaiser in the plain area, so the model should focus on textual area more
- all channel-wise features are treated equally

In this paper, the authors propose the residual non-local attention network (RNAN) for high-quality image restoration. They design residual local and non-local attention blocks to extract features that capture the long-range dependencies between pixels and pay more attention to the challenging parts.

Experiments demonstrate that this method obtains comparable or better results compared with recently leading methods quantitatively and visually.

Framework
-------------------------------------

Denote :math:`I_L` and :math:`I_H` as the low-quality and high-quality images. The reconstructed iamges :math:`I_R` can be obtained by

.. math::

   I_R = H_{RNAN} (I_L)

The framework of RNAN is shown in the figure below. The first and last convolutional layers are shallow feature extractor and reconstruction layer. They propose residual local and non-local attention blocks (RAB and RNAB) to extract hierarchical attention-aware features.

The goal of training RNAN is to minimize the L2 loss function:

.. math::

   L = \frac{1}{N} \sum_{i=1}^N \lVert H_{RNAN}(I_L^i) - I_H^i \rVert_2

.. image:: figures/res-non-local-attn-net-img-restoration-1.png
   :width: 400pt

Residual Non-Local Attention Block
-------------------------------------

Each attention block is divided into two parts: :math:`q` residual blocks (RBs) in the beginning and end of attention block, and two branches in the middle part: trunk branch and mask branch.

.. image:: figures/res-non-local-attn-net-img-restoration-2.png
   :width: 320pt

**Trunk branch.** The trunk branch includes :math:`t` residual blocks (RBs). They adopt the simplified RB from [1]. Feature maps from trunk branches of different depths serve as hierarchical features.

**Mask branch.** The mask branch learn mixed attention maps in channel- and spatial-wise simultaneously. To grasp information of larger scope, the authors choose to use large-stride convolution and deconvolution to enlarge receptive field size.

**Non-local mixed attention.** Non-local blocks (NLBs) are incorporated into the mask branch to obtain non-local mixed attention. The non-local operation is defined as:

.. math::

   y_i & = \left( \sum_{\forall j} f(x_i, x_j)g(x_j) \right) / \sum_{\forall j} f(x_i, x_j) \\
   f(x_i, x_j) & = \exp(u(x_i)^\top v(x_j)) = \exp((W_ux_i)^\top W_vx_j) \\
   g(x_j) & = W_g x_j

.. image:: figures/res-non-local-attn-net-img-restoration-3.png
   :width: 320pt

**Residual non-local attention learning.** One form of attention residual learning was proposed in [1]:

.. math::

   H_{RNA}(x) = H_{trunk}(x) (H_{mask}(x) + 1)

which is found not suitable for image restoration tasks. Instead the authors use:

.. math::

   H_{RNA}(x) = H_{trunk}(x) H_{mask}(x) + x

which can preserve more low-level features.

Reference
-------------------------------------

**[1]** Lim, B., Son, S., Kim, H., Nah, S., & Mu Lee, K. (2017). Enhanced deep residual networks for single image super-resolution. In *Proceedings of the IEEE conference on computer vision and pattern recognition workshops* (pp. 136-144).
