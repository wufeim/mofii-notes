Probabilistic Anchor Assignment with IoU Prediction for Object Detection
=====================================

| **Authors:** Kang Kim, Hee Seok Lee
| **Affiliations:** XL8 Inc., Qualcomm Korea YH

In this paper the authors propose a novel anchor assignment strategy that adaptively separates anchors into positive and negative samples for a ground truth bounding box according to the model's learning status. Moreover, they propose to predict the IoU of detected boxes as a measure of localization quality to reduce the gap between the training and testing objectives. Experiments results verify the effectiveness of the proposed methods on the MS COCO dataset.

Probabilistic Anchor Assignment Algorithm
-------------------------------------

When devising an anchor assignment strategy, the authors take three key considerations into account:
    - the strategy should measure the quality of an anchor based on how likely the model is to find evidence to identify the target object with that anchor
    - the separation of anchors should be adaptive so that it does not require a hyperparameter
    - the strategy should be formulated as a likelihood maximization for a probability distribution in order for the model to be able to reason about the assignment in a probabilistic way
In this respect, they design an anchor scoring scheme and propose an anchor assignment that brings the scoring scheme into account.

One intuitive way to define the score of an anchor is:

.. math::

   S(f_\theta (a, x), g) = S_{cls} (f_\theta(a, x), g) \times S_{loc}(f_\theta(a, x), g)^\lambda

Here :math:`S_{loc}` is the IoU of a predicted box with its GT box:

.. math::

   S_{loc}(f_\theta(a, x), g) = IoU(f_\theta(a, x), g)
