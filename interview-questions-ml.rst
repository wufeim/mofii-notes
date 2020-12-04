Interview Questions (ML)
=====================================

1. **What's the trade-off between bias and variance?**

2. **What is the difference between supervised and unsupervised learning?**

   监督学习需要有label的数据，包括分类和回归。无监督学习不需要label，目的是从没有label的数据中发掘潜在的结构。

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

6. **What is Bayes' Theorem? How is it useful in a machine learning context?**

   贝叶斯定理是：

   .. math::

     P(A \mid B) = \frac{P(B \mid A)P(A)}{P(B)}

   可以很容易的从条件概率的定义推导出来。

7. **Why is "Naive" Bayes naive?**

   朴素贝叶斯算法通过贝叶斯定理计算条件概率。但它是“朴素的”，原因在于计算过程中进行了如下近似：

   .. math::

     P(Y=1 \mid X_1=x_1, X_2=x_2) = \frac{P(X_1=x_1 \mid Y=1) \cdot P(X_2=x_2 \mid Y=1)}{P(y=3)}
   
   相当于假设了 :math:`X_1` 和 :math:`X_2` 间是相互独立的。这个算法的时间复杂度是 :math:`O(kd)` ，其中 :math:`k` 是label类别数而 :math:`d` 是特征维度，因此相比起KNN等一些算法会快很多。

8. **Experience the difference between L1 and L2 regularization.**

   对于线性回归问题中，我们通过增加L1正则化或者L2正则化来优化最小二乘。最小二乘往往bias较低variance较高，适当增加bias可以减少variance，从而模型预测的结果更加稳定。
   
   如果加入L2范数，我们会得到ridge regression：

   .. math::

     \hat{\beta}^\text{ridge} = \text{argmin}_\beta \left\{ \sum_{i=1}^N (y_i - \beta_0 - \sum_{j=1}^p x_{ij})^2 \beta_j + \lambda \sum_{j=1}^p \beta_j^2 \right\}

   如果加入的是L1范数，我们会得到lasso regression:

   .. math::

     \hat{\beta}^\text{ridge} = \text{argmin}_\beta \left\{ \sum_{i=1}^N (y_i - \beta_0 - \sum_{j=1}^p x_{ij})^2 \beta_j + \lambda \sum_{j=1}^p \lvert\beta_j\rvert \right\}

9. **What is your favorite algorithm, could you explain it to me in less a minute?**

10. **What is the difference between Type I and Type II error?**

    统计学上我们把原假设 :math:`H_0` 为真而拒绝原假设称为第一类错误（Type I error），把原假设 :math:`H_0` 为假而没有拒绝原假设称为第二类错误（Type II error）。

11. **What's a Fourier transform?**

    傅立叶变换是一种线性积分变换，将信号在时域和频域间变换。一般的傅立叶变换可以写作：

    .. math::

      F(w) = \int_{-\infty}^\infty f(t)e^{-iwt} dt

    对于周期函数，我们可以通过傅立叶级数表示为：

    .. math::

      f(t) = \sum_{n=-\infty}^\infty F_ne^{i2\pi nt/T}
    
    对于实值函数，我们有：

    .. math::

      f(t) = \frac{a_0}{2} + \sum_{n=1}^\infty \left[a_n\cos\left( \frac{2\pi nt}{T} \right) + b_n\sin\left( \frac{2\pi nt}{T} \right)\right]

12. **What's the difference between probability and likelihood?**

13. **What is deep learning, and how does it contrast with other machine learning algorithms?**

    深度学习是机器学习的一个子集，通过链式法则反向传播梯度。

14. **What's the difference between a generative and discriminative model?**

    判别模型（经过训练）对数据进行分类或回归，而生成模型（根据latent code）生成符合某种性质对特定数据。

15. **What cross-validation technique would you use on a time series dataset?**

16. **How is a decision tree pruned?**

17. **Which is more important to you? Model accuracy, or model performance?**

    模型准确度仅仅是模型性能的一个部分，例如上面的precision，recall，和AP指标都用来更客观的衡量模型的性能。

18. **What's the F1 score? How would you use it?**