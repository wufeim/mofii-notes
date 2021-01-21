RAFT: Recurrent All-Pairs Field Transforms for Optical Flow
=====================================

| **Year:** May 2020
| **Authors:** Zachary Teed, Jia Deng
| **Affiliations:** Princeton University

Optical flow is the task of estimating per-pixel motion between video frames. The best systems are limited by difficulties including fast-moving objects, occlusions, motion blur, and texturelss surfaces.

Optical flow has traditionally been approached as an optimization problem which defines a trade-off between a **data term** which encourages the alignment of visually similar image regions and a **regularization term** which imposes priors on the plausibility of motion. Recently, deep learning has been shown as a promising alternative to traditional methods and achieves comparable performance while being significantly faster.

In this paper, the authors introduce Recurrent All-Pairs Field Transforms (RAFT), a new deep network architecture for optical flow. It achieves SOTA results on KITTI and Sintel, and displays strong cross-dataset generalization and high efficiency.

.. image:: figures/raft-1.png
   :width: 600pt

Approach
-------------------------------------

Given a pair of RGB images, :math:`I_1`, :math:`I_2`, we estimate a dense displacement field :math:`(\mathbf{f}^1, \mathbf{f}^2)`, which maps each pixel :math:`(u, v)` in :math:`I_1` to its corresponding coordinates :math:`(u', v') = (u + f^1(u), v + f^2(v))` in :math:`I_2`.

RAFT consists of a feature encoder, a correlation layer, and a recurrent GRU-based update operator, where all stages are differentiable and composed into an end-to-end trainable architecture.

Feature Extraction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Feature are extracted from the input images using a convolutinoal network. The encoder :math:`g_\theta` outputs features at 1/8 resolution with :math:`D = 256`: :math:`g_\theta: \mathbb{R}^{H \times W \times 3} \mapsto \mathbb{R}^{H/8 \times W/8 \times D}`. There is an additional context network :math:`h_\theta` that extracts features only from the first image :math:`I_1`.

Computing Visual Similarity
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Given input features :math:`g_\theta(I_1) \in \mathbb{R}^{H \times W \times D}` and :math:`g_\theta(I_2) \in \mathbb{R}^{H \times W \times D}`, the correlation volume is formed by taking the dot product between all pairs of feature vectors:

.. math::

   \mathbf{C}(g_\theta(I_1), g_\theta(I_2)) \in \mathbb{R}^{H \times W \times H \times W}

**Correlation Pyramid:** The authors construct a 4-layer pyramid :math:`\{\mathbf{C}^1, \mathbf{C}^2, \mathbf{C}^3, \mathbf{C}^4\}` by pooling the last two dimensions. Thus :math:`\mathbf{C}^k \in \mathbb{R}^{H \times W \times H/2^k \times W/2^k\}`. The set of volumes gives high resolution information about both large and small displacements.

**Correlation Lookup:** The authors define a lookup operator :math:`L_\mathbf{C}` which generates a feature map by indexing from the correlation pyramid. Given a current estimate of optical flow :math:`(\mathbf{f}^1, \mathbf{f}^2)`, they map each pixel :math:`\mathbf{x} = (u, v)` to its estimated correspondence :math:`\mathbf{x}' = (u + f^1(u), v + f^2(v))`. Then they define a local grid around :math:`\mathbf{x}'`:

.. math::

   \mathcal{N}(\mathbf{x}')_r = \left\{ \mathbf{x}' + \mathbf{dx} \mid \mathbf{dx} \in \mathbb{Z}^2, \lVert \mathbf{dx} \rVert_1 \leq r \right\}

They use the local neighborhood :math:`\mathcal{N}(\mathbf{x}')_r` to index from the correlation volume.

**Efficient Computation for High Resolution Images:** Although all pairs correlation scales :math:`O(N^2)`, there is an equivalent implementation that scales :math:`O(NM)` by exploiting the linearity of the inner product and average pooling.

Iterative Updates
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The update operator estimates a sequence of flow estimates :math:`\{\mathbf{f}_1, \dots, \mathbf{f}_N\}` from an initial starting point :math:`\mathbf{f}_0 = 0`. In each iteration, we have :math:`\mathbf{f}_{k+1} = \Delta \mathbf{f} + \mathbf{f}_k`.

The update operator takes flow, correlation, and a latent hidden state as input, and outputs :math:`\Delta \mathbf{f}` and an updated hidden state.

**Inputs:** Given current flow :math:`\mathbf{f}^k`, we retrieve correlation features from the correlation pyramid. The correlation feature are then processed by 2 convolutional layers. Another 2 convolutional layers are applied to the flow estimate for flow features. The input feature map is then taken as the concatenation of the correlation, flow and context features.

**Update:** A core component of the update operator is the gated activation unit based on the GRU cell:

.. math::

   z_t & = \sigma(\text{Conv}_{3 \times 3}(h_{t-1}, x_t], W_z)) \\
   r_t & = \sigma(\text{Conv}_{3 \times 3}(h_{t-1}, x_t], W_r)) \\
   \tilde{h}_t & = \text{tanh}(\text{Conv}_{3 \times 3}([r_t \bigodot h_{t-1}, x_t], W_h)) \\
   h_t & = (1 - z_t) \bigodot h_{t-1} + z_t \bigodot \tilde{h}_t

**Flow Prediction:** The hidden state outputted by the GRU is passed through two convolutional layers to predict the flow update :math:`\Delta \mathbf{f}`.

Supervision
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

They supervise the network with the L1 distance between the full sequence of predictions and the ground truth flow with exponentially increasing weights:

.. math::

   \mathcal{L} = \sum_{i=1}^N \gamma^{N - i} \lVert \mathbf{F}_{gt} - \mathbf{f}_i\rVert_1
