We use mFired only to change the color of the output pin from white to green, signaling in the editor window that the wait node has notified any node connected to the output pin.

Once mAcive is set to true, the update() method starts decrementing the wait time in mCurrentTime:

```
void WaitNode::update(float deltaTime) {
    if (!mActive) {
        return;
    }
    mCurrentTime -= deltaTime;
```

After the wait time is below 0, trigger signals for both the input and the output pin are sent out, the wait time is reset, the node is deactivated, and the color of the output pin changes:

```
if (mCurrentTime <= 0.0f)
    fireNodeOutputTriggerCallback(mOutId);
    fireNodeOutputTriggerCallback(mInId);
    mCurrentTime = mWaitTime;
    mActive = false;
    mFired = true;
}
```

The reason for triggering the input pin was discussed in the section Creating graph node classes: if the wait node is a child of a sequence node, the parent sequence node needs to know that the wait node is no longer active.

Finally, exporting and importing data is done by the two methods, exportData() and importData():

```
std::optional<std::map<std::string, std::string>>
WaitNode::exportData() {
std::map<std::string, std::string> data{};
data["wait-time"] = std::to_string(mWaitTime);
return data;
}
void WaitNode::importData(
    std::map<std::string, std::string> data) {
mWaitTime = std::stof(data["wait-time"]);
mCurrentTime = mWaitTime;
}
```