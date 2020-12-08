Bridging the Gap Between Anchor-Based and Anchor-Free Detection via Adaptive Training Sample Selection
=====================================

| **Authors:** Shifeng Zhang, Cheng Chi, Yongqiang Yao, Zhen Lei, Stan Z. Li
| **Affiliations:** CBSR NLPR CASIA, SAI UCAS, AIR CAS, BUPT, Westlake University

The authors argue that the essential difference between anchor-based and anchor-free detection is how to define positive and negative training samples, no matter regressing from a box or a point. Then they propose an Adaptive Training Sample Selection (ATSS) to automatically select positive and negative samples according to statistical characteristics of object. Experiments show that ATSS can achieve 50.7% AP on MS COCO.

Difference Analysis of Anchor-Based and Anchor-Free Detection
-------------------------------------

Without loss of generality, the representative anchor-based RetinaNet and anchor-free FCOS are adopted to dissect their differences:

- **The number of anchors tile per location:** RetinaNet tiles several anchor boxes per location, while FCOS tiles one anchor point per location.
- **The definition of positive and negative samples:** RetinaNet resorts to IoU for positives and negatives, while FCOS utilizes spatial and scale constraints to select samples.
- **The regression starting status.** RetinaNet regresses the object bounding box from preset anchor box, while FCOS locates the object from the anchor point.

By removing inconsistencies between the two methods, the authors compare the performance of RetinaNet (#A = 1) and FCOS on the MS COCO minival set, and the results are reported below.

.. image:: figures/atss-1.png
   :width: 280pt

**The results in each column demonstrate that the definition of positive and negative samples is an essential difference between anchor-based and anchor-free detectors. The results in each row indicate that the regression starting status is an irrelevant difference.**

Adaptive Training Sample Selection
-------------------------------------

The authors propose the ATSS method that automatically divides positive and negative samples according to statistical characteristics of object almost without any hyperparameter.

----

| **Algorithm: Adaptive Training Sample Selection (ATSS)**
|   **Inputs:**
|     :math:`\mathcal{G}` is a set of ground-truth boxes on the image
|     :math:`\mathcal{L}` is the number of feature pyramid levels
|     :math:`\mathcal{A}_i` is a set of anchor boxes from the :math:`i` th pyramid levels
|     :math:`\mathcal{A}` is a set of all anchor boxes
|     :math:`k` is a quite robust hyperparameter with a default value of 9
|   **Outputs:**
|     :math:`\mathcal{P}` is a set of positive samples
|     :math:`\mathcal{N}` is a set of negative samples
|   **Algorithm:**
|       **for** :math:`g \in \mathcal{G}` **do**
|           build an empty set for candidate positive samples of :math:`g`: :math:`\mathcal{C}_g \leftarrow \emptyset`;
|           **for** :math:`i \in [1, \mathcal{L}]` **do**
|               :math:`\mathcal{S}_i` :math:`\leftarrow` select :math:`k` anchors from :math:`\mathcal{A}_i` whose centers are closest to the center of :math:`g` based on L2 distance;
|               :math:`\mathcal{C}_g = \mathcal{C}_g \cup \mathcal{S}_i`;
|           **end for**
|           compute IoU between :math:`\mathcal{C}_g` and :math:`g`: :math:`\mathcal{D}_g = IoU(\mathcal{C}_g, g)`;
|           compute mean of :math:`\mathcal{D}_g`: :math:`m_g = Mean(\mathcal{D}_g)`;
|           compute standard deviation of :math:`\mathcal{D}_g`: :math:`v_g = Std(\mathcal{D}_g)`;
|           compute IoU threshold for :math:`g`: :math:`t_g = m_g + v_g`;
|           **for** :math:`c \in \mathcal{C}_g` **do**
|               **if** :math:`IoU(c, g) \geq t_g` and center of :math:`c` in :math:`g` **then**
|                   :math:`\mathcal{P} = \mathcal{P} \cup c`;
|               **end if**
|           **end for**
|       **end for**
|       :math:`\mathcal{N} = \mathcal{A} - \mathcal{P}`;
|       return :math:`\mathcal{P}`, :math:`\mathcal{N}`;

----

Here are some motivations behind this method:

- **Selecting candidates based on the center distance between anchor box and object.**
- **Using the sum of mean and standard deviation as the IoU threshold.** This helps to adaptively select enough positives for each object from appropriate pyramid levels in accordance of statistical characteristics of object. The figure below illustrates this method.

  .. image:: figures/atss-2.png
     :width: 360pt

- **Limiting the positive samples' center to object.** Althought the IoU of candidates is not a standard normal distribution, the statistical results show that each object has about :math:`0.2 \times k\mathcal{L}` positive samples, invariant to its scale, aspect ratio, and location.
- **Keeping almost hyperparameter-free.** Experiments show that results are quite insensitive to the variations of :math:`k`.
