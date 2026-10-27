While t-SNE visualizes feature embeddings, **explainable AI** techniques such as *activation-map* visualization reveal what CNN layers learn, motivating the CNN Brain Viewer Dashboard developed next.

# CNN brain viewer dashboard

A common criticism of deep learning systems is their apparent lack of interpretability. Although convolutional neural networks achieve remarkable classification accuracy, understanding what information is captured by intermediate layers remains challenging. One useful strategy is to visualize activation maps generated throughout the network.

**From Edges to Semantics:** CNNs learn hierarchical representations:

| Layer Depth | Typical Features Learned |
|---|---|
| Early layers | edges, corners |
| Intermediate layers | textures, patterns |
| Deep layers | object parts |
| Final layers | semantic concepts |

*Table 12.1: Hierarchical representations with CNNs*

This hierarchy resembles processing in the human visual cortex, where increasingly abstract representations emerge in deeper regions.

**ResNet18 Feature Visualization:** The dashboard uses a pretrained ResNet18 model. Forward hooks are attached to selected layers.

```
def hook_fn(name):
    def hook(module, input, output):
        activations[name] = output.detach()
    return hook

model = models.resnet18(weights=models.ResNet18_Weights.DEFAULT).to(DEVICE)
model.eval()
activations = {} # hooks storage
model.conv1.register_forward_hook(...) # register layers
model.layer1.register_forward_hook(...)
model.layer2.register_forward_hook(...)
model.layer3.register_forward_hook(...)
model.layer4.register_forward_hook(...)
```