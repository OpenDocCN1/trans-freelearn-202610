# Detectron2 utilities for exporting models

Detectron2 provides a wrapper class called TracingAdapter, which helps wrap a Detectron2 model and supports exports using the tracing method. Detectron2 models take rich inputs and also produce rich outputs (i.e., they can be dictionaries or objects). Conversely, the tracing function (torch.jit.trace) takes tensors as inputs/outputs. Therefore, this adapter class helps flatten the inputs of Detectron2 models as tensors so they are traceable. Additionally, it also provides the utilities to reconstruct model outputs as rich outputs from flattened results as tensors. This class takes the following input parameters:

+   - model: This is a Detectron2 model to be exported.
+   - inputs: This can be rich inputs (e.g., dictionaries or objects) and flattened by this class to produce tensors for tracing.
+   - inference_func: This is an optional callable that can be used to replace the model inference if needed.
+   - allow_non_tensor: This dictates whether inputs/outputs can contain non-tensor objects. It filters out non-tensor objects to make the model traceable but cannot reconstruct the rich inputs/outputs if enabled. The default value for this input parameter is False.

It has the forward method that wraps the original model forward method. This method provides flattened outputs that can be converted to rich outputs using the schema (outputs_schema) recorded by this class. It also provides a data member (flattened_inputs) to store the flattened inputs. The following is an example usage of this class:

```
# adapter can use rich inputs
adapter = TracingAdapter(model, inputs)
# trace with flattened inputs
traced = torch.jit.trace(adapter, adapter.flattened_inputs)
# produces flattened outputs
flattened_outputs = traced(*adapter.flattened_inputs)
# convert flattened outputs to rich outputs
outputs = adapter.outputs_schema(flattened_outputs)
```

The TraceAdapter class supports the tracing and ONNX (more on this in the next chapter) methods of exporting.