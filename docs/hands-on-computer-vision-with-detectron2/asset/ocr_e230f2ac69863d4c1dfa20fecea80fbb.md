Specifically, it has the code from the `forward` method of the original model in TorchScript format. Next, we are ready to perform the prediction using the traced model on the same input (X) created previously:

```
```python
with torch.no_grad():
    y = traced_model(X)
    print(y)
```
```

The preceding code snippet should display the result exactly the same as the prediction of the original PyTorch model:

```
```text
tensor([[0.1100],
        [0.0000]], device='cuda:0')
```
```

The following code snippet saves the traced model, loads it again, then uses it to perform prediction on the same input to make sure that we can export and load the model in a different environment:

```
```python
traced_model.save("traced_model.pt")
loaded_model = torch.jit.load("traced_model.pt")
for p in loaded_model.parameters():
    print(p)
print(loaded_model(X))
```
```

This code snippet should save the traced model to the hard drive. After loading, the loaded model should have the same parameters as the PyTorch model. Finally, the loaded model should produce the same prediction on the X input as the original model.

If you run this notebook on Google Colab, you can save this model into your local system for future use with the following statements:

```
```python
from google.colab import files
files.download('traced_model.pt')
```
```

Congratulations! Using the tracing method, you should have mastered the steps to export PyTorch models into TorchScript. The following section describes the limitation of the tracing method and introduces the scripting approach to perform the same task.