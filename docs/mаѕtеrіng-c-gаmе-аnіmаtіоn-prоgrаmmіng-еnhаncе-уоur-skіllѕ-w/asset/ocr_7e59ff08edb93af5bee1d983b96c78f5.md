We will use the number of bones in the model as x dimension, simplifying the remaining part of the shader code:

```
uint numberOfBones = gl_NumWorkGroups.x;
```

Locating the correct index in the buffer is done by combining the number of bones, the shader instance (invocation), and the node we will work on:

```
uint index = node + numberOfBones * instance;
```

The main logic for the compute shader multiplies the translation, rotation, and scaling matrix in the TRS order, and saves the result in the buffer for the TRS matrices, at the same index of the node transforms:

```
trsMat[index] = getTranslationMatrix(index) *
    getRotationMatrix(index) * getScaleMatrix(index);
}
```

In GLM, matrices are multiplied from right to left, a fact that may be confusing at first. So, despite the name of the matrix being “TRS,” the multiplications are performed in reverse order of the name: The model scaling is applied first, then the rotation, and then the translation comes last. Other math libraries or different matrix packings may use a different order of multiplication. Two extensive matrix tutorials are listed in the Additional resources section.

Saving the TRS matrix on the same spot as the node transforms retains the order of nodes in a model and the order of nodes in all model instances.

To trigger the shader execution, we call glDispatchCompute() for the OpenGL renderer, adding a memory barrier that waits for the SSBO:

```
glDispatchCompute(numberOfBones,
    std::ceil(numberOfInstances / 32.0f), 1);
 glMemoryBarrier(GL_SHADER_STORAGE_BARRIER_BIT);
```

The memory barrier makes sure the CPU waits for a specific state of the GPU. In this case, we must wait until all SSBO writes have finished, so we set the bit for the shader storage buffers. The call to glMemoryBarrier() simply blocks execution, returning only after the GPU has reached the desired state.