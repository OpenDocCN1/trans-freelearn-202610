# Adding Simple Navigation

```
std::make_pair(navTriIndex, navPoint));
}
```

As the last step of the code block, we add the new navigation data to the navPoints map that contains data about all currently known nodes.

```
} else {
NavData& navPoint = navPoints.at(navTriIndex);
}
```

Here, we get a reference to the navigation data to be able to update the information in place. For the existing navigation point, we calculate the new estimated distance from the known distance to the source and the heuristic value to the target:

```
NavData possibleNewPrevNavPoint =
navPoints.at(currentIndex);
float newDistanceFromSource =
possibleNewPrevNavPoint.distanceFromSource +
glm::distance(currentTriPoint, navTriPoint);
float newDistanceToDest = newDistanceFromSource +
navPoint.heuristicToDest;
```

If the new path through this node is shorter than the previously known path, we update the navigation data to reflect the new, shorter path:

```
if (newDistanceToDest < navPoint.distanceToDest) {
navPoint.prevTriIndex = currentIndex;
navPoint.distanceFromSource =
newDistanceFromSource;
navPoint.distanceToDest = newDistanceToDest;
}
```

After all neighboring nodes have been checked, we add the current node to the closed list, marking it as fully explored:

```
navClosedList.insert(currentIndex);
```