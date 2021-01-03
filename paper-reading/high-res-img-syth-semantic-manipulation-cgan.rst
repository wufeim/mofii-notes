High-Resolution Image Synthesis and Semantic Manipulation with Conditional GANs (Pix2PixHD)
=====================================

| **Authors:** Ting-Chun Wang, Ming-Yu Liu, Jun-Yan Zhu, Andrew Tao, Jan Kautz, Bryan Catanzaro
| **Affiliations:** NVIDIA Corporation, UC Berkeley

In this paper, the authors discuss a new approach that produces high-resolution (2048 x 1024) images from semantic label maps. They address two main issues of previous SOTA methods:

- the difficulty of generating high-resolution images with GANs
- the lack of details and realistic textures

They propose to generate 2048 by 1024 visually appealing results with a novel adversarial loss, as well as new multi-scale generator and discriminator architectures.

They further extend the method in two directions:

- enable flexible object manipulation using instance-level object segmentation information
- generate diverse results given the same input label map

This method outperforms previous approaches regarding quantitative evaluations and human perception studies.

Improving Photorealism and Resolution
-------------------------------------

The authors improve the Pix2Pix framework by using a coarse-to-fine generator, a multi-scale discriminator architecture, and a robust adversarial learning objective function.

.. image:: figures/high-res-img-syth-semantic-manipulation-cgan-1.png
   :width: 520pt

**Coarse-to-fine generator:** The authors decompose the generator into two sub-networks: the global generator network :math:`G_1` and the local enhancer :math:`G_2`. The global generator network outputs at a resolution of 1024 x 512, and the local enhancer outputs with a resolution of 2048 x 1024. Additional local enhancer networks could be utilized to synthesize images at an higher resolution.

The global generator is built on the architecture proposed in **[1]**. It consists of 3 components: a convolutional front-end :math:`G_1^{(F)}`, a set of residual blocks :math:`G_1^{(R)}`, and a transposed convolutional back-end :math:`G_1^{(B)}`.

The local enhancer network also consists of 3 components: a convolutional front-end :math:`G_2^{(F)}`, a set of residual blocks :math:`G_2^{(R)}`, and a transposed convolutional back-end :math:`G_2^{(B)}`. The input to :math:`G_2^{(R)}` is the element-wise sum of the :math:`G_2^{(F)}` and :math:`G_1^{(B)}`.

During training, they first train :math:`G_1` on lower resolution images, and then train the two networks jointly on high resolution images.

**Multi-scale discriminators:** The authors use 3 discriminators that have an identical network structure but operate at different image scales, referred to as :math:`D_1`, :math:`D_2`, and :math:`D_3`. *Without the mutli-scale discriminators, the authors observe that many repeated patterns often appear in the generated images.*

**Improved adversarial loss:**

References
-------------------------------------

**[1]** Johnson, J., Alahi, A., & Fei-Fei, L. (2016, October). Perceptual losses for real-time style transfer and super-resolution. In European conference on computer vision (pp. 694-711). Springer, Cham.
