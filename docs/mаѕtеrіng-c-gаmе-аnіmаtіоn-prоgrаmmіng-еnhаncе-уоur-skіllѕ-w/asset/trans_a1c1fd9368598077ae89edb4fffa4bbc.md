


## 创建最终的节点矩阵

在开始下一个计算着色器之前，我们必须绑定在着色器运行期间会使用到的所有缓冲区。我们总共有四个 SSBO：

```cpp
mAssimpMatrixComputeShader.use();
mShaderTRSMatrixBuffer.bind(0);
modelType.second.at(0)->getModel()
    ->bindBoneParentBuffer(1);
modelType.second.at(0)->getModel()
    ->bindBoneMatrixOffsetBuffer(2);
mShaderBoneMatrixBuffer.bind(3);
```

由于所有数据都已驻留在 GPU 上，因此这里不需要任何上传操作。

第二个计算着色器本身名为 `assimp_instance_matrix_mult.comp`，可以在着色器文件夹中找到。着色器代码再次从版本和局部大小定义开始：

```glsl
#version 460 core
layout(local_size_x = 1, local_size_y = 32, local_size_z = 1) in;
```

使用局部大小 32 是因为代码是在一台 NVIDIA GPU 上开发的。对于 AMD GPU，你应该使用局部大小 64，如“Calculating the node transforms in a shader”一节中所述。

与第一个计算着色器类似，其后是 SSBO：

```glsl
layout (std430, binding = 0) readonly restrict
buffer TRSMatrix {
    mat4 trsMat[];
};
layout (std430, binding = 1) readonly restrict
buffer ParentMatrixIndices {
    int parentIndex[];
};
layout (std430, binding = 2) readonly restrict
buffer BoneOffsets {
    mat4 boneOffset[];
};
```

前面的代码行可能会让你皱眉，因为我们似乎违反了 GLSL 着色器代码的基本规则之一：循环的大小必须在编译时已知。幸运的是，该规则不适用于 while 循环。我们可以自由地在循环体内部修改循环控制变量，从而创建各种长度的循环。

然而，这段代码可能会影响着色器性能，因为 GPU 经过优化以在每个线程上执行相同的指令。你可能需要在不同的 GPU 上检查着色器代码，以确保看到预期的加速。

还要注意，无意中创建无限循环可能会导致系统锁定，因为着色器代码永远不会将 wave 或 warp 归还到池中。确保在 CPU 端为 while 循环提供有效的退出条件是个好主意，因为 GPU 锁定可能只能通过强制重启计算机来解决。

只要父节点列表中没有错误或循环，我们就会在每个节点上结束于最后一个块：

```cpp
if (parentNode == -1) {
    nodeMat[index] = nodeMatrix * boneOffset[node];
}
```

在这里，我们将包含根节点以上所有矩阵的 resulting 节点矩阵与节点的骨骼偏移矩阵相乘，并将结果存储在可写的 NodeMatrices 缓冲区中。

启动计算的方式与第一个着色器完全相同。对 OpenGL 运行 `glDispatchCompute()`，然后跟随 `glMemoryBarrier()`：

```cpp
glDispatchCompute(numberOfBones,
    std::ceil(numberOfInstances / 32.0f), 1);
glMemoryBarrier(GL_SHADER_STORAGE_BARRIER_BIT);
```

对于 Vulkan，使用 `vkCmdDispatch()`：

```cpp
vkCmdDispatch(commandBuffer, numberOfBones,
    std::ceil(numberOfInstances / 32.0f), 1);
```

此时，NodeMatrices 缓冲区包含所有节点的 TRS 矩阵，接近我们在第 1 章 CPU 版本代码中 `updateAnimation()` 调用后得到的结果——除了实例的模型根矩阵之外。

## 完成计算迁移

因此，让我们将缺失的矩阵计算添加到顶点蒙皮着色器中。首先，我们在遍历模型的所有实例时收集包含世界位置的矩阵：

```cpp
mWorldPosMatrices.resize(numberOfInstances);
for (unsigned int i = 0; i < numberOfInstances; ++i) {
    ...
    mWorldPosMatrices.at(i) =
        modelType.second.at(i)->getWorldTransformMatrix();
}
```

然后，世界位置矩阵被上传到一个 SSBO，并绑定到顶点蒙皮着色器：

```cpp
mAssimpSkinningShader.use();
mAssimpSkinningShader.setUniformValue(
    numberOfBones);
mShaderBoneMatrixBuffer.bind(1);
mShaderModelRootMatrixBuffer.uploadSsboData(
    mWorldPosMatrices, 2);
```

在顶点蒙皮着色器本身中，引入了新的缓冲区：

```glsl
layout (std430, binding = 2) readonly restrict
    buffer WorldPosMatrices {
    mat4 worldPos[];
};
```

最后，我们从世界位置和顶点蒙皮矩阵创建一个组合矩阵，并使用新矩阵来计算顶点和法线的位置：

```glsl
mat4 worldPosSkinMat = worldPos[gl_InstanceID] * skinMat;
gl_Position = projection * view * worldPosSkinMat *
    vec4(aPos.x, aPos.y, aPos.z, 1.0);
...
normal = transpose(inverse(worldPosSkinMat)) *
    vec4(aNormal.x, aNormal.y, aNormal.z, 1.0);
```

现在让我们添加更多模型——大量模型。假设我们总共添加了 4,000 个示例模型的实例。你机器上的 resulting 矩阵生成时间可能与图 2.6 中的数字相似：

![](img/3cfb9832c943f5f7366fd104b5255971_104_0.png)

图 2.6：使用 4,000 个实例的计算着色器版本

即使实例数量增加了 2.5 倍，计算着色器代码的平均矩阵生成时间仍约为 CPU 版本的一半。你甚至可能在更强大的 GPU 上看到更大的非线性性能提升。最近的 GPU 不仅有数千个核心并行处理矩阵乘法，而且下一个更大的型号几乎使核心数量翻倍，从而带来更多并行化。

我们可以将实例数量扩展得更多或处理更复杂的模型，同时仍保持更低的矩阵生成时间。在某个任意的实例数量下，应用程序的帧率仍会降至 60 FPS 以下。根据你的系统，这可能在达到图 2.6 中的 4,000 个实例之前发生，也可能要晚得多。

如果你将分析器附加到应用程序，你会发现我们计算的新瓶颈：`AssimpAnimChannel` 类中方法 `getRotation()` 结尾处的四元数 SLERP：

```cpp
glm::quat rotation =
    glm::normalize(glm::slerp(mRotations.at(timeIndex),
    mRotations.at(timeIndex + 1), interpolatedTime));
```

```cpp
buffer, bufferVector.data();
glBindBuffer(GL_SHADER_STORAGE_BUFFER, 0);
```

SSBO 的内容可以与在 CPU 上完成的相同计算结果进行比较。逐步地、缓冲区地缩小问题范围，直到找到错误。

回读 SSBO 可能不是进行计算着色器调试的明显解决方案，但这里任何一点帮助都是受欢迎的。但是，根据着色器的复杂性，你可能不得不退回到手动遍历代码。还尝试使用简单的数据集来简化调试。

## 总结

在本章中，我们将大部分计算从 CPU 迁移到 GPU 上的计算着色器。在简要回顾了并发代码执行的历史之后，我们制定了将节点变换计算迁移到 GPU 的计划，并最终执行了该计划。在本章结束时，我们检查了 resulting 应用程序所获得的加速。

在下一章中，我们将研究为模型视图应用程序添加视觉选择的方法。能够创建数千个模型实例是很好的，但目前在所有其他实例中定位一个特殊实例几乎是不可能的。我们将讨论两种不同的方法并实现其中一种。

## 实践环节

你可以对代码做一些补充：

- 添加“可编程顶点拉取”到代码中。

    使用可编程顶点拉取，顶点数据将不再通过顶点缓冲区推送。相反，顶点数据将被上传到 GPU 上的 UBO 或 SSBO，并且顶点着色器用于从该缓冲区为每个顶点提取所有数据。

- 将 `mix()` 和 `slerp()` 从 `AssimpAnimChannel` 移动到 GPU。

    当从通道向量中提取平移、旋转和缩放的两个数据值的定时后，需要对平移和缩放进行线性插值，对旋转进行 SLERP。两种插值类型每帧都会被调用数千次——也许 GPU 更快。

