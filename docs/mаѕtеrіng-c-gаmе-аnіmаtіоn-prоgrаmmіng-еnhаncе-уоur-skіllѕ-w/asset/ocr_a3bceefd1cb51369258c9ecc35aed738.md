If the foot node chain is empty, we continue immediately since we don’t have anything to calculate here. But if we have a valid foot node chain, we iterate backward over the node chain, and in each node chain, we iterate forward over all instances:

```
for (int index = nodeChainSize - 1; index > 0; --index) {
    for (size_t i = 0; i < numberOfInstances; ++i) {
```

The order of the loops – foot first, then the node chain, and instance last – has a good reason: it is much easier to handle cases where one of the foot node chains is empty or the foot node chains have different sizes, as we can skip an entire loop or use a different number of iterations. Iterating the node chain backward is necessary because the foot chain vector contains the nodes in order from the effector to the root node, but the world position update starts with the root node as the first node.

In the inner loop, we extract the node IDs of the current node and the previous node from the foot node chains:

```
int nodeId =
    modSettings.msFootIKChainNodes[foot].at(index);
int nextNodeId =
    modSettings.msFootIKChainNodes[foot]
    .at(index - 1);
```

By using the two node IDs, we extract the world position of the nodes:

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