Entropy
=====================================

The **entropy** of variable :math:`X` is given by:

.. math::

   H(X) = - \sum_{x \in X}p(x) \log p(x)

The **conditional entropy** of variable :math:`Y` given :math:`X` is:

.. math::

   H(Y \mid X) = \sum_{x \in X}p(x)H(Y \mid X = x)

----

In InfoGAN, the authors used the **mutual information** between :math:`X` and :math:`Y`, :math:`I(X; Y)`, to incentivize the conditional GAN to learn from the input conditions. It measures the "amount of information" learned from knowledge of random variable :math:`Y` about the other ranodm variable :math:`X`, and is given by:

.. math::

   I(X; Y) = H(X) - H(X \mid Y) = H(Y) - H(Y \mid X)

Finally, the InfoGAN solves the information-regularized minimax game:

.. math::

   \min_G \max_D V_I(D, G) = V(D, G) - \lambda I(c; G(z, c))
