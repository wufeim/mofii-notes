End-To-End Optimized Image Compression
=====================================

| **Authors:** Johannes Balle, Valero Laparra, Eero P. Simoncelli
| **Affiliations:** New York University, Universitat de Valencia

The authors propose an image compression method, consisting of a nonlinear analysis transformation, a uniform quantizer, and a nonlinear synthesis transformation.

For **lossy compression problem**, one must trade off two competing costs: the entropy of the discretized representation (**rate**) and the error arising from the quantization (**distortion**). Joint optimization of rate and distortion is difficult.

.. note::

   **Transform Coding**

   Most existing image compression methods operate by linearly transforming the data vector into a suitable continuous-valued representation, quantizing its elements independently, and then encoding the resulting discrete representation using a lossless **entropy code**. This scheme is called **transform coding** due to the central role of the transformation. Typically, the three components of transform coding methods - transform, quantizer, and entropy code - are separately optimized.

The authors propose a framework for end-to-end optimization of an image compression model based on nonlinear transforms. Specifically, a generalized divisive normalization (GDN) joint nonlinearity is used, followed by uniform scalar quantization. The compressed image is reconstructed from the quantized values using an approximate parametric nonlinear inverse transform. For any desired point along the rate-distortion curve, the parameters of both analysis and synthesis transofrms are jointly optimized using SGD.

Experiment results show improvement in visual quality measured by MS-SSIM for all images at all bit rates.

Choice of Forward, Inverse, and Perceptual Transforms
=====================================


