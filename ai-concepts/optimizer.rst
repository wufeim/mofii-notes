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

      \eta_t = \frac{\alpha}{\sqrt{V_t}} \cdot m_t

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

Momentum is introduced in the SGD and we have

.. math::

   m_t = \beta_1 \cdot m_{t-1} + (1 - \beta_1) \cdot g_t

Usually we pick :math:`\beta_1 = 0.9`, which means the current gradient only slightly adjusts the update direction.

**Disadvantages:** uniform udpate speed; cannot update parameters based on their importance.

SGD with Nesterov Acceleration
-------------------------------------

NAG changes the way we estimate the graident:

.. math::

   g_t = \nabla f(w_t - \frac{\alpha}{\sqrt{V_{t-1}}} \cdot m_{t-1})

.. note::

   **What's next?**

   The idea is that we want the learning rate is adaptive. For frequently-updated parameters, we'd like to slow down the learning rate. For rarely-updated parameters, we'd like a bigger learning rate.

AdaGrad
-------------------------------------

Let the second-order derivative be

.. math::

   V_t = \sum_{\tau=1}^t g_{\tau}^2

Then the update rule is

.. math::

   \eta_{ti} = \frac{\alpha}{\sqrt{V_{ti} + \varepsilon}} \cdot m_{ti}

**Disadvantages:** The denominator of the update rule builds up and the learning rate would converge to zero.

RMSProp
-------------------------------------

In case the learning rate drop rapidly, RMSProp changes the way we calculate the second-order derivative

.. math::

   V_t = \beta_2 \cdot V_{t-1} + (1 - \beta_2)g_t^2

Usually we choose :math:`\beta_2 = 0.9` and :math:`\alpha = 0.001`.

Adam
-------------------------------------

Adam = Adaptive + Momentum:

.. math::

   m_t & = \beta_1 \cdot m_{t-1} + (1 - \beta_1)\cdot g_t \\
   V_t & = \beta_2 \cdot V_{t-1} + (1 - \beta_2)\cdot g_t^2

Nadam
-------------------------------------

Nadam = Adam + Nesterov Acceleration.
