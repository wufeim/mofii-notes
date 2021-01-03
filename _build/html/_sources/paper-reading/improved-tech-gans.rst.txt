Improved Techniques for Training GANs
=====================================

| **Year:** Jun 2016
| **Authors:** Tim Salimans, Ian Goodfellow, Wojciech Zaremba, Vicki Cheung, Alec Radford, Xi Chen
| **Affiliations:** OpenAI

Training GANs requires finding a Nash equilibrium of a non-convex game with continuous, high-dimensional parameters. GANs are typically trained using SGD to minimize a cost function. When used to seek a Nash equilibrium, they may fail to converge.

In this work, the authors introduce several techniques intended to encourage convergence of the GANs. They lead to improved semi-supervised learning performance and improved sample generation.
