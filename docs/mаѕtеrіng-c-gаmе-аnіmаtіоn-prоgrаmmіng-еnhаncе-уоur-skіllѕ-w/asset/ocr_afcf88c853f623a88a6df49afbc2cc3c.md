```
std::unordered_set<int> neighborTris =
    currentTri.neighborTris;
```

For every loop iteration, we extract the current triangle from the mNavTriangles map and get the center point in world coordinates and the neighbor triangles.

Next, we loop over all neighbor triangles and extract the center point of the triangle:

```
for (const auto& navTriIndex : neighborTris) {
    NavTriangle navTri = mNavTriangles.at(navTriIndex);
    glm::vec3 navTriPoint = navTri.center;
```

If the neighboring node is not fully explored and not even visited yet, we continue inside the two if conditions and add the node to the open list:

```
if (navClosedList.count(navTriIndex) == 0) {
    if (navOpenList.count(navTriIndex) == 0) {
        navOpenList.insert(navTriIndex);
```

As a reminder, the closed list contains all fully explored nodes, and the open list contains all known neighbors but not yet fully explored nodes of any of the nodes (even already closed nodes). At this point, we know that this is a new node to visit, so we create new navigation data:

```
NavData navPoint{};
navPoint.triIndex = navTriIndex;
navPoint.prevTriIndex = currentIndex;
```

For the distance calculation, we get the distance from the current index (our parent node) and add the distance between the two nodes to the distance from the source:

```
NavData prevNavPoint =
    navPoints.at(navPoint.prevTriIndex);
navPoint.distanceFromSource =
    prevNavPoint.distanceFromSource +
    glm::distance(currentTriPoint, navTriPoint);
navPoint.heuristicToDest =
    glm::distance(navTriPoint, targetPoint);
navPoint.distanceToDest =
    navPoint.distanceFromSource +
    navPoint.heuristicToDest;
navPoints.emplace(
```