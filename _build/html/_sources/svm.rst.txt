[ESL] 12 Support Vector Machines and Flexible Discriminants
=====================================

12.1 Introduction
-------------------------------------

In this chapter we describe generalizations of linear decision boundaries for classification.

12.2 The Support Vector Classifier
-------------------------------------

Our training data consists of :math:`N` pairs :math:`(x_1, y_1), \dots, (x_N, y_N)`, with :math:`x_i \in \mathbb{R}^p` and :math:`y_i \in \{-1, 1\}`. Define a hyperplane by

.. math::

   \{x: f(x) = x^\top \beta + \beta_0 = 0\}

where :math:`\beta` is a unit vector: :math:`\lVert\beta\rVert = 1`. A classification rule induced by :math:`f(x)` is

.. math::

   G(x) = \text{sign}[x^\top\beta + \beta_0]

The signed distance from a point :math:`x` to the hyperplane is :math:`f(x) = x^\top\beta + \beta_0`.

If the classes are separable, we can find the hyperplane that creates the biggest margin by solving the optimization problem

.. math::

   \max_{\beta, \beta_0, \lVert \beta \rVert = 1} \; & M \\
   \text{subject to} \; & y_i(x_i^\top\beta + \beta_0) \geq M, \;\;\; i = 1, \dots, M
