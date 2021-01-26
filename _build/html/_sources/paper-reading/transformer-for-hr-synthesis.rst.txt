Taming Transformers for High-Resolution Image Synthesis
=====================================

| **Year:** Dec 2020
| **Authors:** Patrick Esser, Robin Rombach, Bjorn Ommer
| **Affiliations:** Heidelberg University

In contrast to CNNs, Transformers contain no inductive bias that prioritizes local interactions. The authors show how to (i) use CNNs to learn a context-rich vocabulary of image constituents, and in turn (ii) utilize transformers to efficiently model their composition within high-resolution images.

The authors apply this approach to conditional synthesis tasks, where both non-spatial information and spatial information can control the generated image. The authors present the first results on semantically-guided synthesis of megapixel images with transformers at `https://git.io <https://git.io/JLlvY>`_.
