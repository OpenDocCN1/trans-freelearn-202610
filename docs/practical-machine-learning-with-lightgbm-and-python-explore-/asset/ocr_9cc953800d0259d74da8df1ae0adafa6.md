models with different performances. Some error functions can also be used as loss functions— for example, **mean squared error (MSE)**.

+   The second property is that a loss function must be differentiable, specifically in the context of gradient descent. An example of an error function that's not differentiable is the F1 score. The F1 score may produce a numeric value of the model's performance, but it is not differentiable and cannot be used as a loss function.

The process for gradient descent can then, be defined as follows. Suppose we have a loss function $L$ defined for parameters $x$. For an initial set of parameters, the loss is calculated as $L(x_0)$. Gradient descent proceeds iteratively to minimize the loss function:

```
$L(x_{n+1}) < L(x_n)$
```

To update the parameters, we take a step in the direction of the negative gradient of $L$. We can specify the gradient descent update rule as follows:

```
$x_{n+1} = x_n - \gamma_n \nabla L(x_n)$
```

Here, $\gamma_n$ is the **learning rate**, which defines the step size, and $\nabla L(x_n)$ is the gradient of $L$ at $x_n$.

The graph in Figure 2.3 illustrates the gradient descent process:

![](bbox=[0.26, 0.475, 0.74, 0.765])

Figure 2.3 – Graph showing the gradient descent process to find the minimum of a function

Choosing an appropriate learning rate is critical to gradient descent’s success. If the learning rate is too low, the optimization will be very slow, potentially not reaching a minimum in the allowed number of iterations. A small learning rate could also lead to the process getting stuck in a local minimum: