By using simple pseudo-code, FABRIK can be melted down to this procedure:

```
NodePositions := OrigNodePositions
StoreOriginalBoneLengths
RootNodePos := NodePositions[LastPosition];
For NumIterations; Do
    EffectorPos := NodePositions[0]
    If TargetIsCloseToEffector
        Return
    SolveForward(TargetNodePos)
    SolveBackwards(RootNodePos)
EndFor
```

The SolveForward and SolveBackwards methods are similar, so we can look only at pseudo-code for the forward-solving part of FABRIK:

```
NodePositions[0] = TargetNodePos
For i = 1; i < NodePositions.Size; i += 1
    BoneDirection = NodePositions[i] - NodePositions[i-1]
    NewOffset = BoneDirection * BoneLengths[i - 1]
    NodePositions[i] = NodePositions[i-1] + NewOffset
EndFor
```

The backward-solving part just starts with the saved root node position and iterates over the bones in the opposite direction.

As you can see, the number of steps is small and the actions in these steps are simple. Armed with the new knowledge, let’s add a FABRIK solver.

# Implementing the FABRIK inverse kinematics algorithm

The inverse kinematics solver using the FABRIK algorithm is implemented by the following steps:

+   1. For the FABRIK inverse kinematics solver, we create a new class, IKSolver, by adding the IKSolver.h and IKSolver.cpp files to the tools folder.
+   2. Next to the constructor and a setter for the number of iterations, a public method named solveFABRIK() is added:

```
std::vector<glm::vec3> solveFARBIK(
    std::vector<glm::mat4>& nodeMatrices,
    glm::vec3 targetPos);
```