A similar check exists in the SingleInstanceBehavior copy constructor; we also have to add the FaceAnim node type here to bind the node action callback:

```
if (node->getNodeType() == graphNodeType::instance ||
    node->getNodeType() == graphNodeType::action ||
    node->getNodeType() == graphNodeType::faceAnim {
    newNode->setNodeActionCallback(
        mBehaviorData->bdNodeActionCallbackFunction);
}
```

In addition to a manipulation of the InstanceSettings variables when changing the face animation settings, we add two new setters to the AssimpInstance class for simplified access to the new InstanceSettings variables.

# Enabling instance and renderer to react to face animation changes

Updating the InstanceSettings by reading all data first plus writing all data back at the end is good if we need to adjust more than multiple values. For a single change, separate setters are easier to use. We add the two new public methods setFaceAnim() and setFaceAnimWeight() to the AssimpInstance class:

```
void setFaceAnim(faceAnimation faceAnim);
void setFaceAnimWeight(float weight);
```

Both methods update the two values in the InstanceSettings data of the instance, plus a bit of extra logic to handle the none value of the faceAnimation enum.

As the last step for the new node, the updateInstanceSettings() method of the renderer class, OGLRenderer.cpp or VkRenderer.cpp, needs to know what to do when an instance wants to change the face animation settings.

To do that, in the switch block for the node type, a new case block for the new faceAnim node type must be added:

```
switch (nodeType) {
    ...
    case graphNodeType::faceAnim:
```