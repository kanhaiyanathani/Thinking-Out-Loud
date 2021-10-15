# Paper review

I would like to proceed by summarizing an interesting research paper that I have read recently. It's an Award winner paper published in the CVPR 2019 conference. The paper titled “**A General and Adaptive Robust Loss Function**” by Jonathan T. Barron, presents a general loss function with robustness as a free parameter(trainable parameter).

As a statistician, we always want to prefer a robust loss(like L1) over the non-robust loss(L2) while training any model. But that comes at the cost of non-convexity i.e. the training will be more prone to getting stuck at a local minima than the global one. So, how about having a model which is convex in the starting but could smoothly transition into less convexity and increased robustness, while the training continues?. This paper provides a tool to do exactly the same with its theoretical validity.

They introduced a general loss function:

![](.gitbook/assets/0)

![](.gitbook/assets/1)

The above fig. shows the loss function and its gradient for different robustness values(α parameter). Clearly, as α decreases, it penalizes the loss and its derivative for outliers. So, we can manually decrease the α, as the training continues. Instead of the overhead of manual tuning, we would prefer a free parameter that could be trained as the rest of the parameters. But this will not work because the training will just cheat and change the shape of the loss function to minimize everything. So, to tackle this, the paper also presents an adaptive loss:

Where , act as normalization constant for the **distribution p(x|α)**. Calculation of this partition function has also been discussed but we will not go into the details.

![](.gitbook/assets/2)

In the above figure, we can clearly see that as α decreases, it not only reduces the loss for outliers but also increases the loss for inliers. “During training, optimization will have to choose between reducing α, thereby getting “discount” on large errors at the cost of paying a penalty for small errors, or increasing α, thereby incurring a higher cost for outliers but a lower cost for inliers”\[1]. So, the training can’t cheat here because of this inherent trade-off property of the adaptive loss function. The training will do the right thing, all without having to do any hyper-parameter tuning.

They have demonstrated the results on the existing models by just swapping in with this adaptive loss. They have shown enhanced performances in the task of “VAE for Image synthesis” and “self-supervised monocular depth estimation”.

**How did I arrived at this paper?** During my leisure time a few days back, I was thinking about different fundamental changes that can be done in the training procedures of any deep learning models to make it fast and accurate towards finding the global minima. I thought through different techniques such as the uses of 2nd derivative in the gradient update equation (derived from the taylor expansion of the loss function) to make the convergence fast. Another way I was thinking was to sample from the conditional probability distribution(made from loss) of a parameter(weight of network) to find the best estimate which maximizes the distribution (or minimizes the loss), similar to Monte Carlo Markov Chain (MCMC) techniques of estimating the expectation.

While searching for these I arrived at this paper, I am basically impressed by its underlying concept of “flexible” loss function which can smoothly transition between a more convex to less convex shapes, to simultaneously make the model more robust as well as more accurate towards finding the global minima.

**In my view,** we can do a similar thing with the activation functions used in hidden layers of any neural network. Instead of fixed activations, we can have a general activation function that can smoothly transition from a linear to nonlinear functions. The non-linearly parameter can be coupled with the robustness parameter of the adaptive loss function discussed in the paper so that an increase in convexity in the loss function will ensure less non-linearity in the activation function and vice-versa.

1. [https://arxiv.org/abs/1701.03077](https://arxiv.org/abs/1701.03077) ↑
