Support Vector Machine (SVM)
=======================================

SVM是一种二分类模型，求解的是划分数据集并最大化几个间隔的超平面。

.. image:: figures/svm-1.jpg
   :width: 200pt

给定数据集 :math:`T = \{(x_1, y_1), \dots, (x_N, y_N)\}`，其中 :math:`x_i \in \mathbb{R}^n, y_i \in \{-1, +1\}`。对于 :math:`(x_i, y_i)`，它到超平面 :math:`y = wx + b` 的距离可以定义为

.. math::

   \gamma_i = y_i\left( \frac{w}{\lVert w \rVert} \cdot x_i + \frac{b}{\lVert w \rVert} \right)

因此，想要求解SVM定义的最大间隔超平面的问题可以表述为

.. math::

   \max_{w, b} \; & \gamma \\
   \text{subject to } & y_i\left( \frac{w}{\lVert w \rVert} \cdot x_i + \frac{b}{\lVert w \rVert} \right) \geq \gamma

等价于

.. math::

   \max_{w, b} \; & \gamma \\
   \text{subject to } & y_i\left( \frac{w}{\lVert w \rVert\gamma} \cdot x_i + \frac{b}{\lVert w \rVert\gamma} \right) \geq 1

令 :math:`w = \frac{w}{\lVert w \rVert \gamma}`， :math:`b = \frac{b}{\lVert w \rVert \gamma}`，我们有

.. math::

   \max_{w, b} \; & \gamma \\
   \text{subject to } & y_i(w \cdot x_i + b) \geq 1

注意到最大化 :math:`\gamma` 等价于最大化 :math:`\frac{1}{\lVert w \rVert}`，又等价于最小化 :math:`\frac{1}{2}\lVert w \rVert^2`，从而SVM定义的优化问题被转化为

.. math::

   \min_{w, b} \; & \frac{1}{2}\lVert w \rVert^2 \\
   \text{subject to } & y_i(w \cdot x_i + b) \geq 1

利用拉格朗日乘子，我们得到无约束的拉格朗日目标函数：

.. math::

   L(w, b, \alpha) = \frac{1}{2}\lVert w \rVert^2 - \sum_{i=1}^N \alpha_i (y_i(w \cdot x_i + b) - 1)

以上的推导都是基于数据集是线性可分的，即所有的线性约束条件都可以被满足。对于线性不可分的数据集，我们引入“软间隔”的概念，即允许一部分点不满足约束条件。通过Hinge Loss，原优化问题可以写为：
