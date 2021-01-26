Taming Transformers for High-Resolution Image Synthesis
=====================================

| **Year:** Dec 2020
| **Authors:** Patrick Esser, Robin Rombach, Bjorn Ommer
| **Affiliations:** Heidelberg University

In contrast to CNNs, Transformers contain no inductive bias that prioritizes local interactions. The authors show how to (i) use CNNs to learn a context-rich vocabulary of image constituents, and in turn (ii) utilize transformers to efficiently model their composition within high-resolution images.

The authors apply this approach to conditional synthesis tasks, where both non-spatial information and spatial information can control the generated image. The authors present the first results on semantically-guided synthesis of megapixel images with transformers at `https://git.io <https://git.io/JLlvY>`_.

Approach
-------------------------------------

High-resolution image synthesis requires a model that understands the global composition of images, enabling it to generate locally realistic as well as globally consistent patterns. The authors represent images as a composition of perceptually rich image constituents from a codebook, which can significantly reduce the description length of compositions and allows us to model the globl interrelations with a transformer.

Learning an Effective Codebook of Image Constituents
-------------------------------------
