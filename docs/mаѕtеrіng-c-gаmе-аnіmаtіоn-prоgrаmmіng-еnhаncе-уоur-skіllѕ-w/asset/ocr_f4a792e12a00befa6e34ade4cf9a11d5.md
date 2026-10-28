12. Then, we start a loop with the maximum number of iterations:

```
for (unsigned int i = 0; i < mIterations; ++i) {
    glm::vec3 effector = mNodePositions.at(0);
    if(glm::length(targetPos - effector) <
        mCloseThreshold) {
        return mNodePositions;
    }
}
```

At the start of the loop, we compare the positions of the effector node and the desired target. If the positions are close together, we return the current node positions as solutions.

13. Now comes the FABRIK solving part. We call the forward solver method with the target position and then the backward solver method with the saved root node position:

```
solveFABRIKForward(targetPos);
    solveFABRIKBackwards(rootPos);
}
```

14. After both solving parts are finished, we continue with the next iteration. Finally, we return the positions FABRIK found for the nodes:

```
return mNodePositions;
}
```

It’s possible that the effector node is unable to reach the target position, i.e., because the node chain is too short. In this case, the best position FABRIK could find will be returned.

Before we can continue with the last part of the FABRIK, we need to make sure to configure the root and effector nodes of the model. Here, we will use only the feet to create a natural appearance while walking up or down a hill.

# Defining the node chain for the instance’s feet

To be able to configure the nodes for the feet, two new members of the ModelSettings struct in the ModelSettings.h file are added:

```
std::array<std::pair<int, int>, 2> msFootIKChainPair{};
    std::array<std::vector<int>, 2> msFootIKChainNodes{};
```

In the msFootIKChainPair array, we store the node IDs of the effector and the root node for the left foot and right foot. We only need this pair for the user interface since the contents of the msFootIKChainNodes array will be calculated from the effector and root node IDs.