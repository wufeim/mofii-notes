Logistic Regression
=====================================

Let the training data be :math:`D = \{(x_i, y_i)\}` where :math:`x_i \in \mathbb{R}^n` and :math:`y_i \in \{0, 1\}`. Since the linear model :math:`wx^\top` ranges from :math:`-\infty` to :math:`\infty`, we use the sigmoid function to map the output of the linear model to the range :math:`(0, 1)`:

.. math::

   h_w(x) = g(w^\top x), \;\;\; g(x) = \frac{1}{1 + e^{-x}}

And we assume that

.. math::

   P(y = 1 \mid x; w) & = h_w(x) \\
   P(y = 0 \mid x; w) & = 1 - h_w(x)

With maximum likelihood, we have

.. math::

   L(\theta) = P(D \mid \theta) = \prod P(y \mid x; \theta) = \prod g(\theta^\top x)^y(1- g(\theta^\top x))^{1-y}

and the log likelihood is given by

.. math::

   l(\theta) = \sum y\log g(\theta^\top x) + (1-y)\log(1 - g(\theta^\top x))

In machine learning, we minimize the loss function. Here, we define the loss function :math:`\mathcal{L}` as

.. math::

   \mathcal{L} = \frac{1}{N} \sum - y\log g(\theta^\top x) - (1-y)\log(1 - g(\theta^\top x))

To solve this optimization problem, we may use stochastic gradient descent, Newton's Method, conjugate gradient descent, and etc.
