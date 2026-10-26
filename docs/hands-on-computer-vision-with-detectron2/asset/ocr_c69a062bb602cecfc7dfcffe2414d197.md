# Momentum

Gradient descent updates the weight based on the value of the current derivative of the loss function only. On the other hand, the momentum approach also considers the accumulation of the past gradients besides the impact of the current gradient. Analogically, we can consider the stochastic/gradient descent approach as pushing a box on the road. If we push, it will move, and if we stop pushing, it will not move due to having high friction. On the other hand, we can consider the momentum approach as pedaling a bicycle. If we pedal, then the bike moves. However, it moves for a bit more when we stop pedaling, thanks to the momentum generated from previous pedalings.

Let’s rewrite the stochastic/gradient descent approach weight update formula so that it looks like this:

$$\theta_t = \theta_{t-1} - \eta g_t$$

Here, $\theta_t$ and $g_t$ are the training parameter and the gradient at time $t$, and $\eta$ is the learning rate. Therefore, if there is no gradient ($g_t = 0$), then the parameter isn’t updated. On the other hand, the momentum approach takes two steps. At every iteration, it records a variable, $v_t$ (velocity), and then uses it to update the training parameter using the following two equations:

$$v_t = \mu v_{t-1} + \eta g_t$$

$$\theta_t = \theta_{t-1} - v_t$$

Here, $\mu$ typically has a value that is less than and closer to 1.0, such as 0.9, 0.95, or 0.99. One of these values is chosen for $\mu$ so that when $g_t = 0$, after a few iterations, $v_t$ also becomes zero and will not update the weight. As these formulas indicate, generally, the main advantage of using the momentum approach is that you can speed up the training time (time to convergence).

Momentum has several variations, and the previously listed one is Sutskever’s implementation. There is another popular implementation from Nesterov, which is a little different:

$$v_t = \mu v_{t-1} + g_t$$

$$\theta_t = \theta_{t-1} - \eta v_t$$

Let’s continue with the code and visualization approach we introduced in the previous sections to illustrate these concepts. The following code snippet prepares a momentum optimizer:

```
class Momentum():
    def __init__(self, lr, moment, nesterov=False):
        self.lr = lr
        self.moment = moment
        self.first_step = True
```