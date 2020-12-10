Pooling Layer
=====================================

- imcrease the receptive field
- translation/rotation/scale invariance (doubtful)
- easier to optimize
- prevent the model from overfitting
- robust to noise

Mean Pooling, Max Pooling, Stochastic Pooling, and Global Average Pooling
-------------------------------------

- **Mean Pooling:** keep context information; make the image blurry
- **Max Pooling:** keep texture information; widely-used
- **Stochastic Pooling:** improve the generalization of the model
- **Global Average Pooling:** used to substitute the fully-connected layers in a FCN; decrease the total number of parameters; prevent the model from overfitting; not as "black-box" as the fully-connected layers
