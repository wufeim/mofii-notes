Optimizer
=====================================

The framework of the optimization is:

1. Calculate the current gradient:

   .. math::

      g_t = \nabla f(w_t)

2. Estimate the first and second-order momentum:

   .. math::

      m_t & = \phi(g_1, \dots, g_t) \\
      V_t & = \psi(g_1, \dots, g_t)

3. Calculate the update rule:

   .. math::

      \eta_t = \frac{\alpha}{\sqrt{V_t}} \codt m_t

4. Update the weights:

   .. math::

      w_{t+1} = w_t - \eta_t

Various optimizers differ in how to estimate the update rule.

SGD
-------------------------------------

The stochastic gradient descent is given by:

.. math::

    \eta_t = \alpha \cdot g_t

**Disadvantages:** frequent updates; cannot escape local minima/saddle point.

SGD with Momentum
-------------------------------------
