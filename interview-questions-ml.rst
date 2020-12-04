Interview Questions (ML)
=====================================

1. **What's the trade-off between bias and variance?**

2. **What is the difference between supervised and unsupervised learning?**

   监督学习需要有label的数据，包括分类和回归。无监督学习不需要label，从没有label的数据中发掘潜在的结构。

3. **How is KNN different from k-means clustering?**

   KNN是监督学习的一种，根据:math:`x`附近的:math:`k`个点投票预测。

   :math:`k` -means是无监督学习的一种，通过聚类的方式分析数据。

4. **Explain how a ROC curve works.**

   ROC全称receiver operating characteristic curve，描述真阳性率（TP/P）和假阳性率（FP/N）在各种阈值下的关系。

   .. image:: figures/roc_curve.png
     :width: 320pt

   如图所示，ROC曲线自左向右表示阈值从1降到0的过程中TPR和FPR的变化。我们往往关注曲线下的面积，面积为0.5时为随机分类，面积越接近1分类能力越强。

5. **Define precision and recall.**

   精准度（precision）定义为TP/(TP+FP)，从结果角度衡量预测出的正例中有多少是真实的正例。

   召回率（recall）定义为TP/(TP+FN)，从真实结果角度出发，描述真实正例中有多少被预测正确。

   在阈值从0升到1的过程中，FP减少从而精准度上升，而于此同时FN上升导致召回率下降，可以以此绘制P-R曲线。通过计算P-R曲线下方的面积可以得到AP（Average Precision）指标。

   .. image:: figures/pr_curve.png
     :width: 320pt
