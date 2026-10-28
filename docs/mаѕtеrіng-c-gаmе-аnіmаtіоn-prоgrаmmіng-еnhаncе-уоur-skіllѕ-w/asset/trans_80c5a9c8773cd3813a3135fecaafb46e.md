

3. 接下来，我们在渲染器中创建一个新的私有方法 `checkForLevelCollisions()`。我们首先获取实例设置并跳过空实例：

```
void OGLRenderer::checkForLevelCollisions() {
    for (const auto& instance :
        mModelInstCamData.micAssimpInstances) {
        InstanceSettings instSettings =
            instance->getInstanceSettings();
        if (instSettings.isInstanceIndexPosition == 0)
            continue;
    }
```

4. 然后，我们只需使用实例的包围盒查询三角形八叉树：

```
std::vector<MeshTriangle> collidingTriangles =
    mTriangleOctree->query(instance->getBoundingBox());
```

5. 仅仅知道我们有碰撞是不够的，但我们想要存储碰撞数据以供进一步操作，例如对碰撞做出反应或绘制调试线。因此，我们将 `MeshTriangles` 向量存储在实例设置中：

```
instances.at(i)->setCollidingTriangles(
    collidingTriangles);
}
}
```

方法 `checkForLevelCollisions()` 被调用到渲染器的 `draw()` 方法中，在实例渲染之后、在 UI 绘制之前。现在所有实例在每一帧都会被检查是否与场景几何体发生碰撞，如果发现碰撞三角形，我们将其存储在实例的 `InstanceSettings` 中。

尽管你可以相信碰撞检测的通用功能是可行的，但眼见为实。我们现在为各种场景数据添加额外的调试线，允许我们将可行信息作为覆盖在屏幕上的线条绘制出来。

## 绘制调试线

如果我们想实时在屏幕上看到碰撞，我们可以在场景几何体中突出显示受影响的三角形。为了实现这种高亮，我们将遵循以下步骤：

- 1. 我们为渲染器添加一个新的私有成员 `mLevelCollidingTriangleMesh`，用于存储碰撞三角形网格：

```
std::shared_ptr<OGLLineMesh>
mLevelCollidingTriangleMesh = nullptr;
```

- 2. 然后，在 `checkForLevelCollisions()` 中，我们清空网格：

```
mLevelCollidingTriangleMesh->vertices.clear();
```

- 3. 接下来，我们遍历所有碰撞三角形，将顶点成对添加到网格存储中，为三角形的每一边创建一个线段：

```
for (const auto& tri : collidingTriangles) {
OGLLineVertex vert;
vert.color = glm::vec3(1.0f, 0.0f, 0.0f);
vert.position = glm::vec4(tri.points.at(0), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(1), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(1), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(2), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(2), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(0), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
}
```

- 4. 现在我们已为所有实例保存了检测到的三角形的轮廓。将三角形渲染到屏幕上可以通过一个简单的线条绘制调用完成：

```
mLineVertexBuffer.uploadData(*mLevelCollidingTriangleMesh);
if (mLevelCollidingTriangleMesh->vertices.size() > 0) {
```

```
mLineShader.use();
mLineVertexBuffer.bindAndDraw(GL_LINES, 0,
    mLevelCollidingTriangleMesh->vertices.size());
}
```

运行带有实例/场景碰撞和调试绘制的应用程序，结果的图片类似于图 12.1：

![](img/3cfb9832c943f5f7366fd104b5255971_411_0.png)

图 12.1：实例与场景几何体的碰撞

在图 12.1 中，实例与地面和多面墙发生碰撞。所有三角形都是通过其包围盒检测到的，因此如果实例仍距离三角形本身有一段距离时三角形被高亮显示，请不要惊慌。在这种情况下，三角形的包围盒被击中，其覆盖的区域比三角形本身更大。但检测到的碰撞中误报的数量很少，可能只会对碰撞检测的性能产生很小的影响。

为了让实例对与场景几何体的碰撞做出适当的反应，我们将下一步扩展节点树。

## 扩展节点树以支持场景几何体碰撞

得益于前几章建立的良好基础，添加一种新的碰撞类型只需几分钟，按照以下步骤操作即可：

- 1. 首先，我们在 `Enums.h` 文件中的 `nodeEvent` 枚举中添加新的事件类型：
```
enum class nodeEvent : uint8_t {
...
instanceToLevelCollision,
NUM
};
```
- 2. 我们还在渲染器 `init()` 方法期间向 `micNodeUpdateMap` 添加一些文本，以便在节点树中添加名称：
```
mModelInstCamData.micNodeUpdateMap[
nodeEvent::instanceToLevelCollision] =
"Inst to Level collision";
```
- 3. 然后，在 `checkForLevelCollisions()` 中，如果我们至少有一个碰撞三角形，则触发新的事件：
```
if (collidingTriangles.size() > 0) {
 mModelInstCamData.micNodeEventCallbackFunction(
 instSettings.isInstanceIndexPosition,
 nodeEvent::instanceToLevelCollision);
}
```
- 4. 该事件通知实例的节点树发生了与场景几何体的碰撞。作为示例，我们可以让实例转身 180 度，如图 12.2 所示：

![](img/3cfb9832c943f5f7366fd104b5255971_413_0.png)

图 12.2：一个对实例/场景碰撞做出反应的事件节点

在图 12.2 中的事件节点中，实例在与场景发生碰撞时会立即转身。还添加了 250 毫秒的冷却时间。使用冷却时间应给予实例足够的时间远离受影响的场景几何体，以避免立即重新触发场景碰撞。

但现在，碰撞检测有一个巨大的缺点：实例保持在同一高度行走，从而穿过山丘或在地形变化时在空中行走。

为了让实例保持在地面上，让我们为应用程序添加一种简单的重力形式。

## 使用重力使实例保持在地面上

重力在我们周围无处不在，将物体向下加速，直到它们撞到其他不可移动的物体，例如地面。我们在行走或静止时大多意识不到重力的作用；我们只在某物掉落并且很可能在撞到地面时破碎时才意识到其威力。

在添加重力之前，我们需要找到一种方法来检测场景几何体中的三角形是否属于地面。

## 在场景数据中查找地面三角形

对于真实世界的场景，可以使用额外属性来标记场景的区域是否可行走。由于我们希望尽可能通用地保持地面级别技术，我们将使用不同的方法并使用三角形法线来检查我们周围的区域是否属于地面或墙壁，以及斜坡区域可以行走的最大角度。

我们将每个三角形的法线作为 3 元素向量存储在 `MeshTriangle` 结构体内部，与其他三角形数据一起存储：

```
struct MeshTriangle {
    ...
    glm::vec3 normal;
    BoundingBox3D boundingBox;
};
```

在 `generateLevelOctree()` 中生成三角形八叉树时，会采用三角形的第一个顶点的法线：

```
tri.normal = glm::normalize(normalMat *
    glm::vec3(mesh.vertices.at(
    mesh.indices.at(i)).normal));
```

由于场景数据的索引结构，使用三角形的顶点之一的法线可能会导致因同一顶点被不同三角形重用而产生的伪影。在这种情况下，我们可以更改法线的计算方式，使其为每个三角形的两条边的叉积：

```
glm::vec3 edge1 = glm::vec3(mesh.vertices.at(
    mesh.indices.at(i + 1)).position -
    mesh.vertices.at(mesh.indices.at(i)).position);
glm::vec3 edge2 = glm::vec3(mesh.vertices.at(
    mesh.indices.at(i + 2)).position -
    mesh.vertices.at(mesh.indices.at(i)).position);
tri.normal = glm::normalize(normalMat *
    glm::cross(edge1, edge2));
```

为了能够在用户界面中控制地面坡度，我们在 `OGLRenderData` 结构体中添加一个名为 `rdMaxLevelGroundSlopeAngle` 的新变量：

```
float rdMaxLevelGroundSlopeAngle = 0.0f;
```

