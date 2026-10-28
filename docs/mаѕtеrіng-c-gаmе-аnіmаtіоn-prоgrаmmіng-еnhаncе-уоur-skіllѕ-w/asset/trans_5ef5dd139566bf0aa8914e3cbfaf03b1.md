

如果脚节点链为空，我们会立即继续，因为这里没有需要计算的内容。但如果我们有一个有效的脚节点链，我们会向后遍历节点链，并在每个节点链中向前遍历所有实例：

```
for (int index = nodeChainSize - 1; index > 0; --index) {
    for (size_t i = 0; i < numberOfInstances; ++i) {
```

循环的顺序——先脚，然后节点链，最后实例——是有原因的：这样处理其中一个脚节点链为空或脚节点链大小不同的情况会容易得多，因为我们可以跳过整个循环或使用不同的迭代次数。向后遍历节点链是必要的，因为脚链向量包含从末端执行器到根节点的节点顺序，但世界位置更新以根节点作为第一个节点开始。

在内部循环中，我们从脚节点链中提取当前节点和前一个节点的节点 ID：

```
int nodeId =
    modSettings.msFootIKChainNodes[foot].at(index);
int nextNodeId =
    modSettings.msFootIKChainNodes[foot]
    .at(index - 1);
```

通过使用这两个节点 ID，我们提取节点世界位置：

```
glm::vec3 position = Tools::extractGlobalPosition(
    mWorldPosMatrices.at(i) *
    mShaderBoneMatrices.at(i * numberOfBones +
    nodeId) *
    model->getInverseBoneOffsetMatrix(nodeId));
glm::vec3 nextPosition =
    Tools::extractGlobalPosition(
    mWorldPosMatrices.at(i) *
    mShaderBoneMatrices.at(i * numberOfBones +
    nextNodeId) *
    model->getInverseBoneOffsetMatrix(nextNodeId));
```

在乘以骨骼偏移节点的逆矩阵、当前节点位置和实例世界位置之后，我们得到脚节点链的两个相邻节点的世界位置。

利用这两个位置，我们计算归一化的 toNext 向量：

```
glm::vec3 toNext = glm::normalize(nextPosition - position);
```

要计算节点的期望旋转，我们需要找到一个四元数，将骨骼从当前世界旋转旋转到 FABRIK 算法计算出的旋转。

## 创建新的世界位置

我们提取 FABRIK 计算的相同节点的新的世界位置，计算新的世界位置之间的归一化 toDesired 向量，并将 toNext 和 toDesired 向量之间的旋转角度计算为四元数 nodeRotation：

```
int newNodePosOffset = i * nodeChainSize + index;
glm::vec3 toDesired = glm::normalize(
mNewNodePositions.at(foot)
.at(newNodePosOffset - 1) -
mNewNodePositions.at(foot)
.at(newNodePosOffset));
glm::quat nodeRotation = glm::rotation(toNext, toDesired);
```

此时，我们获得了骨骼当前位置与模型骨架相同骨骼新位置之间的世界级旋转。

现在我们提取当前节点世界级旋转并计算将匹配节点世界级旋转所需的局部旋转：

```
glm::quat rotation = Tools::extractGlobalRotation(
mWorldPosMatrices.at(i) *
mShaderBoneMatrices.at(i * numberOfBones + nodeId) *
model->getInverseBoneOffsetMatrix(nodeId));
glm::quat localRotation = rotation * nodeRotation * glm::conjugate(rotation);
```

最后，我们读取节点的当前局部旋转，拼接新的旋转，并将旋转存储回 TRS 数据中：

```
glm::quat currentRotation = mTRSData.at(i * numberOfBones + nodeId).rotation;
glm::quat newRotation = currentRotation * localRotation;
mTRSData.at(i*numberOfBones + nodeId).rotation = newRotation;
}
```

通过运行 mAssimpMatrixComputeShader 并上传更新后的 TRS 数据，脚节点链中所有节点的新的节点矩阵将被重新计算。作为最后一步，我们读取 mShaderBoneMatrixBuffer 的内容，以便在脚节点链的下一个节点中使用新的节点矩阵。

我们距离一个可用的逆运动学实现仅一步之遥。现在剩下的是检测脚位置与地面三角形之间的碰撞。让我们处理本章的最后一部分。

## 检测脚到地面的碰撞

检测实例脚与地面碰撞的总体思路与实例相同。我们使用射线到三角形检查来查找射线与定义脚节点下方或上方三角形相交。此交点随后用作 FABRIK 求解器的目标位置。

对于每个实例的两个脚，我们使用与 FABRIK 求解器代码最后一步相同的方法来提取世界位置：

```
int footNodeId = modSettings.msFootIKChainPair.at(foot).first;
glm::vec3 footWorldPos = Tools::extractGlobalPosition(
	mWorldPosMatrices.at(i) *
	mShaderBoneMatrices.at(i * numberOfBones + footNodeId) *
	model->getInverseBoneOffsetMatrix(footNodeId));

```

我们还计算脚节点相对于地面的偏移量：

```
float footDistAboveGround = std::fabs(
    instSettings.isWorldPosition.y - footWorldPos.y);
```

这个 footDistAboveGround 偏移量是必要的，以便让脚在斜坡地面三角形上方悬浮，与脚在平面地面三角形上时的距离相同。然后，我们使用实例的 AABB 来计算包围盒的完整高度和半高度：

```
AABB instanceAABB = model->getAABB(instSettings);
float instanceHeight = instanceAABB.getMaxPos().y - instanceAABB.getMinPos().y;
float instanceHalfHeight = instanceHeight / 2.0f
```

利用这两个高度值，我们创建一个射线来检测与地面三角形的相交。接下来，我们为交点的最终 hitPoint 设置默认值并遍历碰撞三角形：

```
glm::vec3 hitPoint = footWorldPos;
for (const auto& tri :
    instSettings.isCollidingTriangles) {
    std::optional<glm::vec3> result{};
```

然后我们检查每个碰撞三角形是否与从实例半高度开始、最多向下指向整个实例高度的射线相交：

```
result = Tools::rayTriangleIntersection(
    footWorldPos +
    glm::vec3(0.0f, instanceHalfHeight, 0.0f),
    glm::vec3(0.0f, -instanceHeight, 0.0f), tri);
```

使用射线的上、下限有助于防止误检测附近的任何三角形。接下来，我们检查是否找到了交点：

```
if (result.has_value()) {
    hitPoint = result.value() +
    glm::vec3(0.0f, footDistAboveGround, 0.0f);
}
```

```
mIKSolvedPositions.begin(),
mIKSolvedPositions.end();
```

此时，脚位置已准备好进行来自“检测脚到地面碰撞”部分的节点矩阵和 TRS 更新。

在加载关卡后启用并配置逆运动学将向屏幕渲染类似于图 12.14 的图像：

![](img/3cfb9832c943f5f7366fd104b5255971_438_0.png)

图 12.14：右实例的右脚在斜坡地面上的相交减少了

正如你在图 12.14 中右实例所看到的，右脚不再完全与地面三角形相交。此外，右膝的角度已调整，以允许脚放置在地面三角形较高部分上。

左脚的脚趾仍有一些相交。通过使脚趾与地面坡度对齐来修复剩余的相交是“实践环节”部分中的一个任务。但即使有这种最小的相交，观看实例在山丘上上下移动也更像真实人类，令人非常有趣。

## FABRIK 的局限性

使用 FABRIK 求解器进行逆运动学创建的动画通常看起来不错，但该算法有一些局限性需要解决。

最大的问题出现在节点旋转导致不自然骨骼位置时。例如，人类不能向前弯曲膝盖，但我们屏幕上的实例可以做到。如果不检查和限制节点的旋转，实例的骨骼最终可能处于几乎任何位置。*实践环节*部分中的一个任务通过限制节点旋转来处理这个问题。

另一个问题出现在目标距离末端执行器太远，从而无法到达时。FABRIK 通过将整个骨骼链从根节点拉伸到末端执行节点形成直线来解决无法到达的目标，导致实例像机器人一样腿部僵硬地行走。由于我们可以检测到目标是否无法被末端执行器到达，因此处理这些情况比无限节点旋转更容易。*实践环节*部分中有一个任务用于解决无法到达目标的问题。

## 总结

在本章中，我们增强了关卡的碰撞检测。首先，我们为关卡数据添加了单独且专门的八叉树，使我们能够快速检查实例与关卡几何体之间的碰撞。然后，我们为虚拟世界添加了简单的重力，并调整了碰撞检测以允许实例在关卡地面上行走。最后，我们为实例的脚添加了逆运动学，通过避免脚悬浮在地面上方或嵌入关卡的地面三角形中，使斜坡地面的脚和腿运动更自然。

在下一章中，我们将为关卡数据添加最终扩展并添加简单的导航。我们将首先简要回顾游戏中实现导航的几种方法。接下来，我们将探讨 A* 路径查找算法并在应用程序中实现它。然后，我们将向地图添加路径点，作为 A* 计算的目标。最后，我们将为实例实现路径点导航，使实例能够移动到随机路径点或在定义的一组路径点之间巡逻。

