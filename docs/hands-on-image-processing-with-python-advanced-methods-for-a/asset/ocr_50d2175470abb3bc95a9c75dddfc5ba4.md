In summary, the recognition pipeline converts faces into embeddings, compares them using cosine similarity, and assigns the identity of the nearest prototype. Fine-tuning with Siamese-style metric learning or using a pretrained face-recognition model such as FaceNet can further improve recognition performance.

# Bayesian neural networks for image classification

Conventional neural networks learn a single deterministic value for each weight. In contrast, a Bayesian Neural Network (BNN) models each weight as a probability distribution, allowing the network to estimate not only a prediction but also its uncertainty. This is particularly important in safety-critical applications such as medical imaging, autonomous driving, and remote sensing, where knowing *how confident* a prediction is can be as important as the prediction itself. Intuitively, instead of asking “What is the best weight?”, a Bayesian network asks “What range of weights are plausible given the data?”

**Bayesian Neural Networks:** In a deterministic network, weights are fixed: $W = w$ whereas in a Bayesian network each weight is represented by a distribution:

$$W \sim \mathcal{N}(\mu, \sigma^2)$$

where $\mu$ and $\sigma$ are learned during training. The predictive distribution is obtained by marginalizing over all possible weights:

$$p(y|x, D) = \int p(y|x, W),\,p(W|D),\,dW$$

where $D$ denotes the training data. Since this integral is intractable for deep networks, approximate methods such as **Variational Inference**, **Monte Carlo Sampling**, and **MC Dropout** are commonly used.

**Bayesian Linear Layer:** The key idea is to replace *deterministic weights* with *Gaussian-distributed weights*.

```
```python
class BayesianLinear(nn.Module):
    def forward(self, x):
        weight_std = torch.exp(0.5 * self.weight_logvar)
        weight = self.weight_mu + weight_std * torch.randn_like(weight_std)
        return F.linear(x, weight, bias)
```
```

During every *forward* pass, a *new weight* sample is drawn from the learned distribution. Consequently, two evaluations of the same image may produce slightly different predictions. Instead of committing to a single model, the network effectively evaluates many plausible models and combines their predictions.

**Variational Learning and KL Regularization:** Training optimizes both classification accuracy and uncertainty estimation. The loss combines cross-entropy and a KL-divergence penalty:

$$L = L_{CE} + \beta D_{KL}(q(W)\|p(W)),$$

where $L_{CE}$: classification loss, $q(W)$: learned posterior, $p(W)$: prior distribution, and $D_{KL}$: complexity penalty. In the implementation:

```
```python
ce = F.cross_entropy(out_bnn, y_batch)
kl = bnn.kl_loss() / len(train_dataset)
loss_bnn = ce + 0.01 * beta * kl
```
```