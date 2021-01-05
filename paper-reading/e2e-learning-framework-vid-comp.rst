An End-to-End Learning Framework for Video Compression
=====================================

| **Year:** Apr 2020
| **Authors:** Guo Lu, Xiaoyun Zhang, Wanli Ouyang, Li Chen, Zhiyong Gao, Dong Xu
| **Links:** `IEEE Xplore <https://ieeexplore.ieee.org/document/9072487>`_

Existing methods try to employ DNN for video compression [1]. However, most work only replace one or two modules in the traditional framework instead of optimizing the video compression system in an end-to-end fashion.

There are two challenges for building an end-to-end optimized video compression system:
    - it is very difficult to build a learning based video compression system because of the complicated coding procedure
    - it is necessary to design a scheme to generate and compress the motion information that is tailored for video compression

Inspired by the previous work [2], the authors propose the first end-to-end deep video compression (DVC) framework. All components in video compression are implemented with the end-to-end neural networks, and are jointly optimized based on the rate-distortion trade-off through a single loss function. The framework is also very flexible and two variants, :code:`DVC_Lite` and :code:`DVC_Pro`, are proposed for speed/efficiency priority. The adaptive quantization layer for the DVC framework significantly reduces the number of parameters for variable bitrate coding.

Expreimental results show that the proposed approach can outperform the widely used video coding standard H.264 and be even on par with the latest standard H.265.

Notations
-------------------------------------

- :math:`\mathcal{V} = \{x_1, \dots, x_{t-1}, x_t, \dots \}`: current video sequences
- :math:`\bar{x}_t`: predicted frame
- :math:`\hat{x}_t`: reconstructed frame
- :math:`r_t` (transformed to :math:`y_t`): residual (error) between the original frame :math:`x_t` and the predicted frame :math:`\bar{x}_t`
- :math:`\hat{r}_t`: residual (error) between the original frame :math:`x_t` and the reconstructed/decoded frame :math:`\hat{x}_t`
- :math:`v_t` (transformed to :math:`m_t`): motion vector or optical flow value
- :math:`\hat{v}_t`: reconstructed version

Hybrid Coding Framework of Video Compression
-------------------------------------

The input frame :math:`x_t` is split into a set of blocks. The traditional video compression algorithm is summarized as follows,
    1. Block based motion estimation.
    2. Motion compensation.
    3. Transform and quantization.
    4. Inverse transform.
    5. Entropy coding.
    6. Frame reconstruction.

.. image:: figures/e2e-learning-framework-vid-comp-1.png
   :width: 320pt

Refer to [3, 4] for more details.

The Proposed End-to-End Deep Video Compression Framework
-------------------------------------

Motion, residual and entropy bits for hybrid coding are all learned by networks in the proposed framework. All these functional modules are jointly optimized in an end-to-end way using a single rate-distortion loss. The differences between the proposed method and traditional video compression codecs are summarized below.

**Step 1. Motion estimation and compression.** The authors use a CNN model to estimate the optical flow [5], which is considered as the motion information :math:`v_t`. An auto-encoder network with quantization is used to compress the optical flow in a lossy way.

**Step 2. Motion compensation.** A pixel-wise motion compensation approach is implemented using a neural network.

**Step 3-4. Transform, quantization and inverse transform.** The linear transform is replaced by a highly non-linear residual encoder-decoder network.

**Step 5. Entropy coding.** At the training stage, a bit rate estimation net is used to obtain probability distribution of each symbol.

**Step 6. Frame reconstruction.** Reconstructed frame :math:`\hat{x}_t` is generated based on predicted frame :math:`\bar{x}_t` and the reconstructed residual :math:`\hat{r}_t`.

.. image:: figures/e2e-learning-framework-vid-comp-2.png
   :width: 320pt

Reference
-------------------------------------

**[1]** Liu, D., Li, Y., Lin, J., Li, H., & Wu, F. (2020). Deep learning-based video coding: A review and a case study. ACM Computing Surveys (CSUR), 53(1), 1-35.

**[2]** Lu, G., Ouyang, W., Xu, D., Zhang, X., Cai, C., & Gao, Z. (2019). Dvc: An end-to-end deep video compression framework. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (pp. 11006-11015).

**[3]** Wiegand, T., Sullivan, G. J., Bjontegaard, G., & Luthra, A. (2003). Overview of the H. 264/AVC video coding standard. IEEE Transactions on circuits and systems for video technology, 13(7), 560-576.

**[4]** Sullivan, G. J., Ohm, J. R., Han, W. J., & Wiegand, T. (2012). Overview of the high efficiency video coding (HEVC) standard. IEEE Transactions on circuits and systems for video technology, 22(12), 1649-1668.

**[5]** Ranjan, A., & Black, M. J. (2017). Optical flow estimation using a spatial pyramid network. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (pp. 4161-4170).
