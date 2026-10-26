```
myOnnxSession.loadModel("./onnx_model.onnx")
.then(() => {
    // Step 1: create an input
    // Step 2: run and update the output div
});
</script>
</body>
</html>
```

This HTML file first loads the downloaded JavaScript file (onnx.js). It then creates another JavaScript to create a new ONNX inference session and loads the downloaded ONNX model (onnx_model.onnx). After the model is loaded (asynchronously), it performs the following two steps.

Step 1: This step creates an example input as an ONNX Tensor with the same data as in the previous section ([2, 3, 4, 5]):

```
const inferenceInputs = new onnx.Tensor(
  [2, 3, 4, 5],
  'float32', [1, 4]
);
```

Step 2: This step takes the prepared input in the previous step and passes it to the inference session to perform the prediction asynchronously. Once the execution generates the output object, it then takes the output data and updates innerText of the output div:

```
myOnnxSession.run([inferenceInputs])
.then((output) => {
  const outputTensor = output.values().next().value;
  const msg = `model output tensor: ${outputTensor.data}`;
  document.getElementById('output').innerText = msg;
});
```

The following code snippet creates a helper method (expose_content) that allows exposing the content of a web server running on a given port to an iframe and embeds the generated iframe into the current Google Colab output cell:

```
from IPython.display import Javascript
def expose_content(port, height=200):
    display(Javascript("""
(async () =>{
```