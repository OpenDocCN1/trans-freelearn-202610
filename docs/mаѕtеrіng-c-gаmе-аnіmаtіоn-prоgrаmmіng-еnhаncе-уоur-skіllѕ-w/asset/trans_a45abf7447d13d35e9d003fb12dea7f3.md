

## 第13章

如导航网格部分所述，创建网格至少是部分手动工作，具体取决于用于创建游戏地图的编辑器。某些编辑器可以根据地图元素创建导航网格，但在大多数情况下，生成的网格必须手动修正。导航网格必须与地图其余的关卡数据存储在同一张地图中，或存储在单独的数据文件中。

为了同时支持地图内和独立的导航网格，寻路在可行走地面网格方面保持模块化。例如，如果你的导航网格以特殊名称保存在地图文件中，你可以将导航多边形及其邻接属性导入寻路类。你还必须将导航三角形导入单独的三角形八叉树，并进行额外的射线到三角形相交，以找到地面层三角形和导航网格三角形。A* 寻路算法也已实现为单独的方法，允许你轻松添加其他算法或不同的启发式函数。

本章示例代码中，我们将使用与地面层检测相同的思路，并使用每个网格三角形的法线来决定其是否可行走。此方法将导致导航网格中包含所有向上朝向的三角形，即使这些三角形可能无法被任何实例到达。但为了演示游戏地图中寻路和导航的一般思想，从地图的地面三角形创建导航网格已足够，并能提供合理的结果。

我们将使用欧几里得距离来计算节点之间的距离以及启发式函数，因为地图中的三角形很可能不是按矩形网格排列的。为了加快距离计算，我们将扩展网格三角形数据结构。

MeshTriangle 结构体在 OpenGL 的 OGLRenderData.h 文件中定义，在 Vulkan 的 VkRenderData.h 文件中定义。在 MeshTriangle 结构体的末尾，我们添加两个新数组，edges 和 edgeLengths：

```
struct MeshTriangle {
    std::array<glm::vec3, 3> edges{};
    std::array<float, 3> edgeLengths{};
};
```

在 edges 数组中，我们存储每个三角形的三个边。我们按与原始三角形相同的缠绕顺序排列边。由于在计算任意相邻三角形时需要边的长度，我们将每条边的长度存储在对应的 edgeLengths 元素中。

现在我们可以直接进入寻路类的实现。

### 添加寻路类

寻路类（命名为 PathFinder）将驻留在 tools 文件夹中，该文件夹存储了在前几章中创建的所有其他辅助类，如 AABB 或 IKSolver。为了保持命名一致，头文件命名为 PathFinder.h，实现将放在 PathFinder.cpp 文件中。

在 PathFinder.h 头文件的顶部，在所有 #include 指令之后，我们添加两个结构体条目。第一个新结构体命名为 NavTriangle：

```
struct NavTriangle {
    int index;
    std::array<glm::vec3, 3> points{};
    glm::vec3 center{};
    glm::vec3 normal{};
    std::unordered_set<int> neighborTris{};
};
```

我们在这里不复用 MeshTriangle，因为我们需要一些不同的变量。虽然 index、points 数组和法线向量相同，但我们还需要将每个三角形的中心世界位置存储在 center 变量中，并将周围三角形存储在 neighborTris 中。实例将从一个三角形中心导航到下一个三角形中心，而 neighborTris 中存储的三角形用于找到距离目标最近的三角形。

对于邻居三角形，选择 std::unordered_set 而非普通的 std::vector，以便自动移除重复条目。

第二个结构体称为 NavData，包含 A* 算法的数据：

```
struct NavData {
    int triIndex;
    int prevTriIndex;
    float distanceFromSource;
    float heuristicToDest;
    float distanceToDest;
};
```

在 triIndex 变量中，我们存储对应 NavTriangle 的三角形索引。通过使用索引，我们可以简单查找以找到三角形数据，如位置或邻居三角形。一旦我们访问了三角形的邻居节点，我们就会将迄今为止最短路径的三角形的索引添加到 prevTriIndex 中，从而允许我们在寻路运行结束时回溯最短路径。

其余三个变量（distanceFromSource、heuristicToDest 和 distanceToDest）是 A* 算法的核心。在这里我们存储从源到当前节点的累计距离、当前节点与目标之间距离的启发式函数结果，以及两者的总和。通过比较所有邻居节点的 distanceToDest 值，A* 选择通向目标节点的下一个节点。

```
void generateGroundTriangles(OGLRenderData& renderData,
std::shared_ptr<TriangleOctree> octree,
BoundingBox3D worldbox);
std::vector<int> findPath(int startTriIndex,
int targetTriIndex);
```

通过调用 generateGroundTriangles()，所有三角形八叉树中向上朝向的三角形将被定位，并为每个“可通行”三角形创建邻居信息。地面数据准备就绪后，findPath() 可用于从起始三角形找到到目标三角形的路径。findPath() 的结果是 A* 找到的路径，作为要踩上的三角形索引向量，按从起始三角形到目标三角形的顺序排列；如果不存在有效路径，则返回空向量。

PathFinder 类中还有一个私有成员，名为 mNavTriangles：

```
std::unordered_map<int, NavTriangle> mNavTriangles{};
```

我们将计算出的地面三角形存储在 mNavTriangles 映射中。索引与三角形数据之间的映射用于通过三角形索引快速访问，该索引也作为 NavData 元素的一部分存储。

让我们接下来逐步介绍地面三角形生成代码。

### 生成地面三角形

地面三角形是通过利用渲染器生成的三角形八叉树生成的。由于我们将对三角形八叉树进行大量请求，它将作为 generateGroundTriangles() 方法的第二个参数传入。

在清除之前生成的任何导航三角形后，我们通过使用作为第三个参数给出的世界边界进行查询，从八叉树获取所有三角形：

```
mNavTriangles.clear();
std::vector<MeshTriangle> levelTris =
octree->query(worldbox);
```

然后我们遍历所有关卡三角形以找到向上朝向三角形的子集：

```
std::vector<MeshTriangle> groundTris{};
NavTriangle navTri;
for (const auto& tri: levelTris) {
if (glm::dot(tri.normal,
glm::vec3(0.0f, 1.0f, 0.0f)) >=
std::cos(glm::radians(
renderData.rdMaxLevelGroundSlopeAngle))) {
groundTris.emplace_back(tri);
}
}
```

将三角形的法线与向上朝向向量点积，与 rdMaxLevelGroundSlopeAngle 值的余弦进行比较，这一点在第12章的碰撞检测中已提到。如果当前三角形通过检查，我们将其添加到 groundTris 向量中。

接下来，除了 groundTris 中的地面三角形外，我们用最小数据填充名为 navTri 的 NavTriangle 并将其添加到 mNavTriangles 映射中：

```
navTri.points = tri.points;
navTri.normal = tri.normal;
navTri.index = tri.index;
navTri.center = (tri.points.at(0) +
tri.points.at(1) + tri.points.at(2)) / 3.0f;
mNavTriangles.insert(std::make_pair(tri.index, navTri))
```

我们在这里在 groundTris 向量和 mNavTriangles 映射中使用不同数据集，因为对三角形八叉树的查询返回 MeshTriangles 向量，但我们为地面三角形维护一个更合适的 NavTriangles 映射。

```
for (const auto& tri : groundTris) {
    std::vector<MeshTriangle> nearbyTris =
    octree->query(tri.boundingBox);
}
```

此查询效果良好，因为在第12章中，我们必须在渲染器类 OGLRenderer 或 VkRenderer 的 generateLevel0tree() 方法中将每个三角形的边界框稍微调大，以避免三个维度中任一维度的大小为零。通过这种最小尺寸更改，关卡中相邻三角形的边界框现在会相交，三角形八叉树将返回所有邻居三角形。

然后我们获取 mNavTriangles 映射中同一地面三角形的引用，并遍历八叉树查询报告的所有三角形：

```
NavTriangle& navTri = mNavTriangles.at(tri.index);
for (const auto& peer : nearbyTris) {
```

为地面三角形使用引用很重要，因为我们将直接在映射中的 NavTriangle 对象上更新邻居三角形。

即使这两个嵌套循环看起来很糟糕，总体计算时间仍然很小，因为八叉树查询只报告少量邻居三角形。我们甚至可以通过简单检查排除更多三角形：

```
if (tri.index == peer.index) {
    continue;
}
if (glm::dot(peer.normal,
    glm::vec3(0.0f, 1.0f, 0.0f)) <
    std::cos(glm::radians(
    renderData.rdMaxLevelGroundSlopeAngle))) {
    continue;
}
```

