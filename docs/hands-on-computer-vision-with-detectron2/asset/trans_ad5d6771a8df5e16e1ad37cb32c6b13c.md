
值得注意的是，我们选择的损失函数是均方误差（MSE）。下面的代码片段创建了一个名为GD（梯度下降）的优化器，用于在给定学习率和梯度的情况下执行参数更新：

```
class GD():
    def __init__(self, lr):
        self.lr = lr

    def backward(self, theta, gradients):
        theta = theta - self.lr*(gradients)
        return theta
```

接下来，我们必须创建一个函数，在给定X和Y数据（无论是批次还是整个轮次）的情况下执行一次训练步骤：

```
def train_step(X, Y, theta, optimizer):
    # forward
    Y_hat = forward(theta, X)
    loss = mse(Y_hat, Y)
    dLdTheta = gradient(X, Y, Y_hat)
    theta = optimizer.backward(theta, dLdTheta)
    return theta, loss, dLdTheta
```

现在我们已经知道了如何在每次迭代（批次或轮次）中执行训练，下面的代码片段可以帮助我们执行训练循环：

```
def train(X, Y, theta0, optimizer, num_epochs):
    losses = []
    thetas = []
    dLdThetas = []
    theta = theta0
    for epoch in range(num_epochs):
        theta, loss, dLdTheta=train_step(X, Y, theta,optimizer)
        thetas.append(theta)
        losses.append(loss)
        dLdThetas.append(dLdTheta)
    return losses, thetas, dLdThetas
```

此外，下面的代码片段创建了一个函数，帮助我们可视化损失在训练迭代中的变化情况：

```
```python
def plot_losses(losses):
    plt.plot(losses)
    plt.ylim([-1, 20])
    ax = plt.gca()
    msg = f'Final loss: {round(losses[-1], 3)}'
    plt.text(0.99, 0.1, msg,
        horizontalalignment='right',
        verticalalignment='top',
        transform = ax.transAxes)
    plt.xlabel("iteration")
    plt.ylabel("loss")
    plt.show()
```
```

进一步地，下面的代码片段还可视化了训练参数（theta）在训练迭代中的变化情况：

```
```python
def plot_thetas(thetas, losses):
    grid_size = 100
    thetas = np.array(thetas)
    theta1s = np.linspace(0, 4, grid_size)
    theta2s = np.linspace(1, -7, grid_size)
    Z = np.zeros((grid_size, grid_size))
    for i in range(grid_size):
        for j in range(grid_size):
            theta = np.array([[theta1s[i]], [theta2s[j]]])
            Y_hat = forward(theta, X)
            loss = mse(Y_hat, Y)
            Z[i,j] = loss
    # some simple plotting code lines are removed for space eff.
```
```

接下来，我们必须随机初始化训练参数的值，并设置一些训练超参数：

```
```python
theta0 = np.random.rand(2, 1)
num_epochs = 500
lr = 0.001
```
```

下面的代码片段使用梯度下降方法执行训练并可视化结果：

```
```python
optimizer = GD(lr = lr)
losses, thetas, dLdthetas = train(X, Y, theta0,
optimizer=optimizer, num_epochs=num_epochs)
plot_losses(losses)
plot_thetas(thetas, losses)
```
```

*图6.8* 显示了使用梯度下降方法时，训练迭代中的损失和theta值变化：

![](img/0e7994c850a84b7a53d34c7f943c7edd_149_0.png)

可以观察到，损失和theta值变化得逐渐且平滑，因为训练步骤是在整个数据集上执行的。然而，训练过程在500次迭代后没有收敛（X标记表示收敛点）。

另一方面，下面的代码片段准备了一种使用随机梯度下降方法执行训练过程的方法：

```
```python
def train_mini_batch(X, Y, theta0, optimizer, num_epochs,
batch_size):
    # some simple initialization code lines are removed here
    for e in range(num_epochs):
        # shuffle
        np.random.shuffle(indices)
        for i in range(batches):
            start = i*batch_size
            stop = start + batch_size
            indices_b = indices[start:stop]
```
```

## 动量

梯度下降仅基于损失函数当前导数的值来更新权重。另一方面，动量方法除了考虑当前梯度的影响外，还考虑过去梯度的累积。打个比方，我们可以将随机/梯度下降方法视为在路上推一个箱子。如果我们推它，它会移动；如果我们停止推，它由于高摩擦而不会移动。另一方面，我们可以将动量方法视为踩自行车。如果我们踩踏板，自行车就会移动。然而，当我们停止踩踏板时，由于之前踩踏板产生的动量，它还会继续移动一段距离。

让我们重写随机/梯度下降方法的权重更新公式，使其看起来像这样：

$$\theta_t = \theta_{t-1} - \eta g_t$$

这里，$\theta_t$ 和 $g_t$ 分别是时间 $t$ 时的训练参数和梯度，$\eta$ 是学习率。因此，如果没有梯度（$g_t = 0$），那么参数就不会更新。另一方面，动量方法分为两步。在每次迭代中，它记录一个变量 $v_t$（速度），然后使用它通过以下两个方程更新训练参数：

$$v_t = \mu v_{t-1} + \eta g_t$$

$$\theta_t = \theta_{t-1} - v_t$$

这里，$\mu$ 通常是一个小于且接近1.0的值，例如0.9、0.95或0.99。为 $\mu$ 选择这些值之一，使得当 $g_t = 0$ 时，经过几次迭代后，$v_t$ 也会变为零，并且不会更新权重。正如这些公式所示，通常，使用动量方法的主要优点是可以加快训练时间（收敛时间）。

动量有几种变体，前面列出的是Sutskever的实现。还有另一个来自Nesterov的流行实现，略有不同：

$$v_t = \mu v_{t-1} + g_t$$

$$\theta_t = \theta_{t-1} - \eta v_t$$

让我们继续使用前面章节中介绍的代码和可视化方法来阐述这些概念。下面的代码片段准备了一个动量优化器：

```
class Momentum():
    def __init__(self, lr, moment, nesterov=False):
        self.lr = lr
        self.moment = moment
        self.first_step = True
```

```
```python
self.nesterov = nesterov

def backward(self, theta, gradients):
    if self.first_step:
        self.v = np.zeros_like(theta)
        self.first_step = False

    if not self.nesterov:
        self.v = self.moment*self.v + self.lr * gradients
        theta = theta - self.v
    else:
        self.v = self.moment*self.v + gradients
        theta = theta - self.lr * self.v

    return theta
```
```

下面的代码片段随后使用动量执行训练并可视化结果：

```
```python
optimizer = Momentum(lr = lr, moment=0.9)
losses, thetas, dLdThetas = train_mini_batch(X, Y, theta0,
optimizer=optimizer, num_epochs=num_epochs-300, batch_size = 5)
plot_losses(losses)
plot_thetas(thetas, losses)
```
```

*图6.10* 显示了这段代码的输出。正如我们所见，由于动量的作用，损失和训练参数的变化更加平滑。此外，最终的损失和theta值表明，这种方法的表现甚至优于没有动量的随机梯度下降方法。Nesterov实现动量的方法也有类似的结果。因此，为了节省篇幅，我们在此没有包含代码和结果（你可以参考本书GitHub仓库中的代码文件）：

![](img/0e7994c850a84b7a53d34c7f943c7edd_153_0.png)

图6.10：使用动量时训练迭代中的损失和theta值变化

在许多情况下，当神经网络尚未训练好时，使用较高的学习率可能会导致梯度爆炸问题。同样，当接近最优点时，最好使用较小的学习率，这样变化不会在最优点左右来回波动。然而，这并不意味着我们应该始终选择较小的学习率，因为这样会让训练过程更长。因此，可变学习率方法可能会产生更好的结果。

## 可变学习率

可变学习率有多种实现方式。由于篇幅限制，本节只介绍Detectron2在其随机梯度下降实现中所使用的方法。具体来说，在训练开始时，可以选择执行预热步骤。该步骤使用一个名为 `warmup_iters` 的超参数。从训练开始，学习率逐渐线性增加（也可以使用正弦波方法），使其在该阶段结束时达到基础学习率。

此外，还有另一组超参数，称为 steps 和 gamma。这些是学习率按 gamma 因子变化的步骤。这些超参数有助于在训练过程接近结束时逐步降低学习率。下面的代码片段实现了具有可变学习率的随机梯度下降方法：

```
```python
class VariableLR():
    def __init__(self, lr, moment,
        nesterov =False,
        warmup_iters = 0,
        steps      = None,
        gamma      = 0.5):
        # codes removed: self.<inputs> = <inputs>
        self.iter = 0
```
```

