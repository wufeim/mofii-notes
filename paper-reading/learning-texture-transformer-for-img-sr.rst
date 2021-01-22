Learning Texture Transformer Network for Image Super-Resolution
=====================================

| **Year:** Jun 2020
| **Authors:** Fuzhi Yang, Huan Yang, Jianlong Fu, Hongtao Lu, Baining Guo
| **Affiliations:** Shanghai Jiao Tong University, MSRA

Image super-resolution (SR) is the task of recovering realistic textures from a low-resolution (LR) image. Recent progress has been made by taking high-resolution images as references (Ref), so that relevant textures can be transferred to LR images.

Existing SR approaches neglect to use attention mechanisms to transfer high-resolution (HR) textures from Ref images. In this work, the authors propose a novel Texture Transformer Network for Image Super Resolution (TTSR). It consists of four closely-related modules optimized for image generation tasks:

- a learnable texture extractor
- a relevance embedding module
- a hard-attention module and a soft-attention module

Experiments show that TTSR achieves significant improvements over SOTA approaches.

Texture Transformer
-------------------------------------

LR, LR :math:`\uparrow`, and Ref represent the input image, the 4x bicubic-upsampled input image, and the reference image. We obtain Ref :math:`\downarrow\uparrow` by downsampling and then upsampling the reference image, which is dominant-consistent with LR :math:`\uparrow`. The architecture overview is shown below.

.. image:: figures/learning-texture-transformer-for-img-sr-1.png
   :width: 360pt

**Learnable Texture Extractor.** The authors use a learnable texture extractor to encourage a joint feature learning across teh LR and Ref image.

.. math::

   Q & = \text{LTE}(LR\uparrow) \\
   K & = \text{LTE}(Ref\downarrow\uparrow) \\
   V & = \text{LTE}(Ref)

**Relevance Embedding.** Relevance embedding aims to embed the relevance between the LR and Ref iamge by estimating the similarity between :math:`Q` and :math:`K`. The authors first unfold :math:`Q` and :math:`K` into patches

.. math::

   & q_i, \; i \in [1, H_{LR} \times W_{LR}] \\
   & k_j, \; j \in [1, H_{Ref} \times W_{Ref}]

Then the relevance :math:`r_{i, j}` is estimated by the normalized inner product:

.. math::

   r_{i, j} = \left\langle \frac{q_i}{\lVert q_i \rVert}, \frac{k_j}{\lVert k_j \rVert} \right\rangle

**Hard Attention.** The hard-attention module transfer teh HR texture features :math:`V` from the Ref image. To avoid blurry results, only features from the most relevant position in :math:`V` are transferred. The hard-attention map :math:`H` is calculated by:

.. math::

   h_i = \text{argmax}_j r_{i, j}

The transferred HR texture features :math:`T` is simply selected from the :math:`h_i` position in :math:`V`.

**Soft Attention.** The soft-attention module synthesize features from the transferred HR texture features :math:`T` and the LR features :math:`F`. The soft-attention map :math:`S` is computed by:

.. math::

   s_i = \max_j r_{i, j}

The fusion operation is represented as:

.. math::

   F_{out} = F + \text{Conv}(\text{Concat}(F, T)) \odot S

Cross-Scale Feature Integration
-------------------------------------

The texture transformer can be further stacked in a cross-scale way with a cross-scale feature integration module (CSFI).

Stacked texture transformers output the synthesized features for three resolution scales (1x, 2x, and 4x). A CSFI module is applied each time the LR feature is up-sampled to the next scale.

.. image:: figures/learning-texture-transformer-for-img-sr-2.png
   :width: 400pt

Loss Function
-------------------------------------

The loss function is the sum of the reconstruction loss, the adversarial loss, and the perceptual loss:

.. math::

   \mathcal{L} = \lambda_{rec}\mathcal{L}_{rec} + \lambda_{adv}\mathcal{L}_{adv} + \lambda_{per}\mathcal{L}_{per}

The reconstruction loss is the L1 loss which is sharper for performance and easier for convergence:

.. math::

   \mathcal{L}_{rec} = \frac{1}{CHW} \left\lVert I^{HR} - I^{SR} \right\rVert_1

The adversarial loss is introduced and the authors adopt WGAN-GP [1] which proposes a penalization of gradient norm to replace weight clipping, resulting in more stable training and better performance:

.. math::

   \mathcal{L}_D & = \mathbb{E}_{\tilde{x} \sim \mathbb{P}_g} [D(\tilde{x})] - \mathbb{E}_{x \sim \mathbb{P}_r} [D(x)] + \lambda \mathbb{E}_{\hat{x} \sim \mathbb{P}_{\hat{x}}} \left[ (\lVert \nabla_{\hat{x}} D(\hat{x}) \rVert_2 - 1)^2 \right] \\
   \mathcal{L}_G & = - \mathbb{E}_{\tilde{x} \sim \mathbb{P}_g} [D(\tilde{x})]

The perceptual loss is used to improve visual quality. The key idea is to enhance the similarity in feature space between the prediction image and the target image.

.. math::

   \mathcal{L}_{per} = \frac{1}{C_iH_iW_i} \left\lVert \phi_i^{vgg} (I^{SR}) - \phi_i^{vgg} (I^{HR}) \right\rVert_2^2 + \frac{1}{C_jH_jW_j} \left\lVert \phi_i^{lte}(I^{SR} - T\right\rVert_2^2

References
-------------------------------------

**[1]** Gulrajani, I., Ahmed, F., Arjovsky, M., Dumoulin, V., & Courville, A. C. (2017). Improved training of wasserstein gans. In *Advances in neural information processing systems* (pp. 5767-5777).
