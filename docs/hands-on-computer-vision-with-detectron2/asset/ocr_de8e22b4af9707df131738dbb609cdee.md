Let us use the traced model and perform a prediction on the X input generated previously:

```
with torch.no_grad():
    y = traced_model(X)
    print(y)
```

This code snippet gives the following output:

```
tensor([[ -0.1100]],device='cuda:0')
```

Observably, this is a different output compared to the prediction of the original PyTorch model because the execution path is for the dummy input (dummy_X) and not this current X input. Therefore, in this case, the scripting approach should be used.

# Exporting PyTorch models using the scripting method

Continuing with the code in the previous section; the following code snippet exports the PyTorch model to TorchScript using the scripting method:

```
scripted_model = torch.jit.script(pt_model)
print(scripted_model.code)
```

This snippet should display the TorchScript code for the exported model as follows:

```
def forward(self,
    x: Tensor) -> Tensor:
    linear = self.linear
    y = (linear).forward(x, )
    if bool(torch.gt(torch.sum(y), 0)):
        _0 = y
    else:
        _0 = torch.neg(y)
    return _0
```

Observably, this is different from the tracing approach as the TorchScript code of the exported model now contains the control flow section. Additionally, the following code snippet should display the parameters of the exported model (as 0.01 for all parameters):

```
for p in scripted_model.parameters():
    print(p.data)
```