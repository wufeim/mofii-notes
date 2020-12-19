Image-to-Image Translation with Conditional Adversarial Networks (Pix2Pix)
=====================================

| **Authors:** Phillip Isola, Jun-Yan Zhu, Tinghui Zhou, Alexei A. Efros
| **Affiliations:** Berkeley AI Research Laboratory

The authors define **automatic image-to-image translation** as the task of translating one possible representation of a scene into another, given sufficient data. Whatever the taks is, the setting is always the same: predict pixels from pixels.

In this work, the authors develop a cGAN (conditional GAN) framework sufficient to achieve good results for all these problems. The model conditions on an input image and generates a corresponding output image.

.. image:: figures/pix2pix.rst
   :width: 520pt

Method
-------------------------------------

The objective of a conditional GAN can be expressed as

.. math::

   & \min_G\max_D \mathcal{L}_\text{cGAN}(G, D) \\
   & \mathcal{L}_\text{cGAN}(G, D) = \mathbb{E}_{x,y}[\log D(x, y)] + \mathbb{E}_{x,z}[\log(1 - D(x, G(x, z))]

Previous approaches have found it beneficial to mix the GAN objective with a more tranditional loss. The authors use L1 distance rather than L2 distance to encourage less blurring. The final objective is:

.. math::

   \min_G\max_D \mathcal{L}_\text{cGAN}(G, D) + \lambda \mathcal{L}_{L1}(G)

In some GANs, Gaussian noise :math:`z` is provided as an input to the generator. The authors didn't find this strategy effective (the noise is ignored by the generator), and instead provide noise in the form of dropout.

The authors note that despite the dropout noise, they observe minor stochasticity in the output. Designing conditional GANs that produce highly stochastic output, and thereby capture the full entropy of the conditional distributions they model, is an important question left open by the present work.

The figure below gives an overview of the architecture.

.. image:: figures/pix2pix.png
   :width: 400pt
