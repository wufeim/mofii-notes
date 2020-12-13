Focal Loss for Dense Object Detection (RetinaNet)
=====================================

| **Authors:** Tsung-Yi Lin, Priya Goyal, Ross Girshick, Kaiming He, Piotr Dollar
| **Affiliations:** Facebook AI Research

In this paper, the authors discover that the central cause why one-stage detectors have trailed the accuracy of two-stage detectors is the extreme foreground-backgruond class imbalance encountered during training of dense detectors. They propose **Focal Loss** that focuses training on a sparse set of hard examples and prevents the vast number of easy negatives from overwhelming the detector during training.

To evaluate the effectiveness of the loss, they design and train a simple dense detector called **RetinaNet**. Experiments on the COCO dataset show that RetinaNet trained with focal loss achieves SOTA speed and accuracy.
