

最后要说明的是——getArea 只是复用一个 Rect 对象，将其设置为四个象限的值。

```java
private Rect getArea(int area) {
    int startX = mArea.left;
    int startY = mArea.top;
    int width = mArea.width();
    int height = mArea.height();
    switch (area) {
    case 0:
        mTmpRect.set(startX, startY,
                startX + width / 2, startY + height / 2);
        break;
    case 1:
        mTmpRect.set(startX + width / 2, startY,
                startX + width, startY + height / 2);
        break;
    case 2:
        mTmpRect.set(startX, startY + height / 2,
                startX + width / 2, startY + height);
        break;
    case 3:
        mTmpRect.set(startX + width / 2, startY + height / 2,
                startX + width, startY + height);
        break;
    }
    return mTmpRect;
}
```

为了了解四叉树的优化程度，让我们假设屏幕上有 100 个对象，并且它们仅分布在两个象限中。这是该算法提供最佳优化的情况之一。

![](img/35d01831c19688a44e50bce7c489007a_1385_0.png)

如果不使用四叉树技术，需要评估的碰撞数量为 50,000。对列表进行两次迭代会产生 n²/2 次碰撞检查。

当我们将空间分为四个部分时，第一和第四象限中检查的碰撞数量为 1,250（50²/2），其他象限中为零。我们需要额外增加 400 次操作，用于确定每个对象所在的区域（四个象限中有 100 个对象）。

对我们来说，检查对象所在的象限与检查碰撞的复杂度相近，但对于 3D 碰撞，碰撞检查的代价要高得多。

### 重复碰撞

使用空间分区时，有一个特殊情况需要注意：某些碰撞可能会被检测到两次。

当两个位于两个象限交点上的对象之间发生碰撞时，这两个对象都同时在这两个象限中；当算法检查碰撞时，它会在每个象限中被检测到。最坏的情况下，当对象位于四个象限的交点上时，它会被检测到四次。

为解决此问题，我们将保存已检测到的碰撞列表，并且仅在碰撞未被之前检测到时才处理它。

Collision 类非常简单。它只包含两个 ScreenGameObject 实例、一个 equals 方法，以及几个用于处理其对象池的静态方法：

```java
public class Collision {
    private static List<Collision> sCollisionPool = new ArrayList<Collision>();

    public static Collision init(ScreenGameObject objectA,
    ScreenGameObject objectB) {
        if (sCollisionPool.isEmpty()) {
            return new Collision(objectA, objectB);
        }
        Collision c = sCollisionPool.remove(0);
        c.mObjectA = objectA;
        c.mObjectB = objectB;
        return c;
    }

    public static void release(Collision c) {
        c.mObjectA = null;
        c.mObjectB = null;
        sCollisionPool.add(c);
    }

    public ScreenGameObject mObjectA;
    public ScreenGameObject mObjectB;

    public Collision(ScreenGameObject objectA, ScreenGameObject
    objectB) {
        mObjectA = objectA;
        mObjectB = objectB;
    }

    public boolean equals (Collision c) {
        return (mObjectA == c.mObjectA && mObjectB == c.mObjectB)
            || (mObjectA == c.mObjectB && mObjectB == c.mObjectA);
    }
}
```

我们将已检测碰撞的列表作为参数添加到 checkCollisions 方法中，并在 GameEngine 上再次检查之前确保将其清理。GameEngine 上更新后的 checkCollisions 如下：

```java
private List<Collision> mDetectedCollisions = new ArrayList<Collision>();
```

```java
private void checkCollisions() {
    // Release the collisions from the previous step
    while (!mDetectedCollisions.isEmpty()) {
        Collision.release(mDetectedCollisions.remove(0));
    }
    mQuadTreeRoot.checkCollisions(this, mDetectedCollisions);
}
```

请注意，由于我们使用了对象池，不能简单地清空列表；需要逐个释放每个元素。

最后，我们通过在四叉树中检查来验证该碰撞之前未被检测到。

```java
public void checkCollisions(GameEngine gameEngine, List<Collision>
detectedCollisions) {
    int numObjects = mGameObjects.size();
    if (numObjects > MAX_OBJECTS_TO_CHECK && sQuadTreePool.size() >=
4) {
        // Split this area in 4
        splitAndCheck(gameEngine, detectedCollisions);
    }
    else {
        for (int i = 0; i < numObjects; i++) {
            ScreenGameObject objectA = mGameObjects.get(i);
            for (int j = i + 1; j < numObjects; j++) {
                ScreenGameObject objectB = mGameObjects.get(j);
                if (objectA.checkCollision(objectB)) {
                    Collision c = Collision.init(objectA,
objectB);
                    if (!hasBeenDetected(detectedCollisions, c)) {
                        detectedCollisions.add(c);
                        objectA.onCollision(gameEngine, objectB);
                        objectB.onCollision(gameEngine, objectA);
                    }
                }
            }
        }
    }
}
```

请注意，整个过程中都需要碰撞列表，并且它必须作为计算状态的一部分传递给递归方法。

## 总结

现在我们的游戏真的可以玩了，可以检测子弹何时接触小行星并将其销毁；此外，我们必须小心，因为我们的飞船也可能被它们摧毁。

我们学习了如何使用矩形和圆形物体进行简单的碰撞检测，为每个特定精灵调整物体。

我们还学习了一些碰撞检测优化技术，并实现了一种利用空间分区的技术。

我们路线图中的下一步是为游戏增加一些“多汁感”（juiciness）。你难道没有注意到小行星缺少一些爆炸效果吗？在下一章中，我们将讨论如何为特效创建粒子系统。

## 第 5 章 粒子系统

粒子系统是视频游戏中广泛使用的一种技术，用于模拟用其他方法难以渲染的现象。其典型用途包括爆炸、烟花、烟雾、火焰、水等。它们通常是高度混乱的系统。

它们的基础是使用大量称为粒子的小型精灵。它们的行为是参数化的，因此每个粒子都有一组不同的伪随机值。这使得粒子系统的每次使用都不同，但看起来仍然相似。

大多数游戏引擎都包含实现粒子系统的方法。我们的引擎也将包含粒子系统。

我们的代码将基于免费软件项目 Leonids，这是一个用于在标准 Android 界面中显示粒子系统的库。我们将对其进行调整，以供我们的引擎使用。

在解释粒子系统的基础知识后，我们将实现几个一次性粒子系统示例来模拟爆炸，以及一些连续发射器来模拟小行星的拖尾和飞船引擎的烟雾。

### 一般概念

我们将以一种模仿 Leonids 粒子系统库（http://plattysoft.github.io/Leonids/）的方式来制作我们的粒子系统。这个项目是我为在标准 Android 界面中使用粒子系统而制作的免费软件库。

![](img/35d01831c19688a44e50bce7c489007a_1390_0.png)

Leonids 库的演示版可在 Google Play 上获取。

因此，Leonids 实现了自己的更新和绘制线程，以及一个简化的 GameView。我们已经拥有所有这些，因此将调整代码以满足我们的需求。

本章将使用的概念如下：

- **粒子**：被渲染的每个精灵
- **粒子系统**：负责生成、更新和跟踪粒子的实体
- **初始化器**：根据某些参数为粒子设置值的类（在激活之前）
- **修改器**：根据经过时间更新粒子值的类（在激活期间）

在我们的游戏中，我们将只使用少量几个初始化器和修改器。如果需要扩展粒子系统的功能，可以访问 Leonids 的 GitHub 页面以获取灵感。

