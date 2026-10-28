```
for (int i = mNodePositions.size() - 2; i >= 0; --i) {
    glm::vec3 boneDirection =
        glm::normalize(mNodePositions.at(i) -
        mNodePositions.at(i + 1));
    glm::vec3 offset = boneDirection * mBoneLengths.at(i);
    mNodePositions.at(i) =
        mNodePositions.at(i + 1) + offset;
}
}
```

8. The last method, solveFARBIK(), is used to control the iterations of FABRIK plus the comparison of the effector and target positions. We start with a simple check for an empty node matrix vector. In this case, we return an empty vector:

```
std::vector<glm::vec3> IKSolver::solveFARBIK(
    std::vector<glm::mat4>& nodeMatrices,
    glm::vec3 targetPos) {
    if (nodeMatrices.size() == 0) {
        return std::vector<glm::vec3>{};
    }
}
```

9. Then, we resize and fill the mNodePositions vector:

```
mNodePositions.resize(nodeMatrices.size());
for (int i = 0; i < nodeMatrices.size(); ++i) {
    mNodePositions.at(i) =
        Tools::extractGlobalPosition(
        nodeMatrices.at(i));
}
```

10. In the helper method extractGlobalPosition(), the translational part of the node matrix is extracted and returned. Now we can calculate the bone lengths, and we store the position of the root node for the backward part of FABRIK:

```
calculateOrigBoneLengths();
glm::vec3 rootPos =
    mNodePositions.at(mNodePositions.size() - 1);
```

11. We need to store the root node position because we alter the mNodePositions vector during the forward calculations. Without saving the original root node, the backward part would be impossible to solve.