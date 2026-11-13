

public void removeFromGameEngine (GameEngine gameEngine) {
   gameEngine.removeGameObject(this);
}

当我们在复合项上覆盖这些方法时，必须记住调用父类方法来添加和删除对象。

## 制作良好的粒子系统

顺便说一句，粒子系统非常强大，但对它们进行微调和设计并非易事。

粒子系统的实现细节很简单，如我们所见。但要制作一个看起来真实且良好的粒子系统则是完全不同的事情。

微调它们的关键在于反复调整参数并查看效果，一次又一次，直到我们对结果满意为止。

为了揭示微调粒子系统这门晦涩艺术的一些奥秘，我们将举几个例子。你可以自己尝试粒子和参数，看看微小的变化能带来多大的不同。

使用粒子系统有两种方式：一次性发射和连续发射器。让我们分别举几个例子。

## 一次性发射

使用一次性发射时，我们让 ParticleSystem 一次性发射所有粒子。

在这种情况下，我们不需要将 ParticleSystem 添加到或从 GameEngine 中移除，因为 ParticleSystem 的 onUpdate 方法不需要被调用（它仅用于发射新粒子）。

使用一次性发射时，用与我们计划发射的粒子数量相同的粒子数来初始化粒子池是合理的。

ParticleSystem 类的 oneShot 方法如下：

```
public void oneShot(GameEngine gameEngine, double x, double y,
    int numParticles) {
    mX = x;
    mY = y;
    mIsEmitting = false;
    for (int i=0; !mParticlePool.isEmpty() && i<numParticles; i++) {
        activateParticle(gameEngine);
    }
}
```

我们设置发射的 x 和 y 坐标，然后将 isEmitting 设置为 false。虽然只有在将 ParticleSystem 添加到 GameEngine 时才需要更新 isEmitting，但我们还是这样做以保险起见。在这种情况下，将 isEmitting 设置为 false 会使 onUpdate 什么也不做。

参数设置完成后，我们从池中获取粒子并激活它们。请记住，我们在解释 ParticleSystem 类时已经了解过 activate 方法。作为激活的一部分，粒子会被添加到 GameEngine 中。

我们将使用这种粒子系统来制作小行星和玩家的爆炸效果。

## 小行星爆炸

对于小行星爆炸，我们将使用带有三个小岩石碎片的粒子，这样看起来就像小行星破碎成多个碎片。为了增强这种效果，我们将让粒子向任何方向旋转和移动，但不会飞太远。

我们将 ParticleSystem 的创建和配置作为 Asteroid 创建的一部分。这样可以确保每个小行星都有自己独立的粒子池：

```
public Asteroid(GameController gameController, GameEngine gameEngine) {
 super(gameEngine, R.drawable.a10000, BodyType.Circular);
 mSpeed = 200d*mPixelFactor/1000d;
 mController = gameController;
 mExplosionParticleSystem = new ParticleSystem(gameEngine,
 EXPLOSION_PARTICLES, R.drawable.particle_asteroid_1, 700)
 .setSpeedRange(15, 40)
 .setFadeOut(300)
 .setInitialRotationRange(0, 360)
 .setRotationSpeedRange(-180, 180);
}
```

```
public void explode(GameEngine gameEngine) {
 mExplosionParticleSystem.oneShot(gameEngine, mX + mWidth / 2, mY + mHeight / 2, EXPLOSION_PARTICLES);
}
```

粒子系统的配置如下：

- 粒子将存活 700 毫秒。
- 它们会向各个方向生成，速度在每秒 15 到 40 个单位之间变化。
- 在最后 300 毫秒，它们会有一个 alpha 修改器以平滑淡出。
- 粒子将具有任意的初始旋转。请注意，粒子不是对称的；这一点非常重要，可以防止粒子看起来是静止的。
- 最后，每个粒子的角旋转速度将在每秒 -180 到 180 度之间。
- EXPLOSION_PARTICLES 是小行星的一个常量。我们将其设置为 15。

explode 方法会从小行星中心触发 oneShot，使所有粒子运动起来。

最后的连接点是触发 explode 方法。这发生在子弹与小行星碰撞或玩家与小行星碰撞时。Bullet 类中的实现如下：

```
@Override
public void onCollision(GameEngine gameEngine, ScreenGameObject otherObject) {
    if (otherObject instanceof Asteroid) {
        removeFromGameEngine(gameEngine);
        Asteroid a = (Asteroid) otherObject;
        a.explode(gameEngine);
        a.removeFromGameEngine(gameEngine);
    }
}
```

就这么简单。是不是很简单？由于 ParticleSystem 使用的是 oneShot，因此不需要将其添加到或从 GameEngine 中移除。

![](img/35d01831c19688a44e50bce7c489007a_1409_0.png)

爆炸粒子和粒子系统细节。

## 飞船爆炸

对于玩家爆炸，我们将采用不同的方法。我们希望它看起来更具戏剧性。速度值将比小行星更大，以给人一种更具侵略性且范围更广的爆炸印象。我们还将使用两种不同颜色的粒子，而不是一种带有多个碎片的粒子。

由于每个 ParticleSystem 都设计为只持有一种类型的粒子（一张图像），我们将在 Player 类中拥有两个 ParticleSystem 实例，并在构造函数中初始化：

```
mExplosionParticleSystem1 = new ParticleSystem(gameEngine,
EXPLOSION_PARTICLES, R.drawable.particle_ship_explosion_1, 600)
    .setSpeedRange(30, 150)
    .setInitialRotationRange(0, 360)
    .setFadeOut(200);
mExplosionParticleSystem2 = new ParticleSystem(gameEngine,
EXPLOSION_PARTICLES, R.drawable.particle_ship_explosion_2, 600)
    .setSpeedRange(30, 150)
    .setInitialRotationRange(0, 360)
    .setFadeOut(200);
```

在这种情况下，每个粒子都很小且对称，因此不需要旋转速度。由于它们比之前快得多，存活时间也短一些，淡出也更快速。

在这种情况下，EXPLOSION_PARTICLES 设置为 20，因此总共有 40 个粒子。

当 Player 对象爆炸时（作为 onCollision 的一部分），我们会在两个 ParticleSystem 实例上触发 oneShot：

```
mExplosionParticleSystem1.oneShot(gameEngine, mX + mWidth / 2,
mY + mWidth / 2, EXPLOSION_PARTICLES);
mExplosionParticleSystem2.oneShot(gameEngine, mX+mWidth/2,
mY+mWidth/2, EXPLOSION_PARTICLES);
```

如你所见，如果现在运行，爆炸的感觉与小行星的爆炸完全不同：

- 粒子 1
- 粒子 2

![](img/35d01831c19688a44e50bce7c489007a_1411_0.png)

## 发射器

使用粒子系统的另一种方式是将它们配置为发射器。这意味着你的系统在活跃时每秒会发射一定数量的粒子。

计算合适的池大小非常简单。这只是一个基于粒子存活时间和你希望每秒发射粒子数量的公式。例如，使用每秒 20 个粒子和 500 毫秒的存活时间，你只需要一个 10 个粒子的池，因为粒子死亡后会被返回到池中。

重要的是要记住，作为发射器工作的 ParticleSystem 需要添加到或从 GameEngine 中移除，因为需要 onUpdate 方法来检查更多粒子的生成。ParticleSystem 的 onUpdate 方法如下：

```
@Override
public void onUpdate(long elapsedMillis, GameEngine gameEngine) {
    if (!mIsEmitting){
        return;
    }

    mTotalMillis += elapsedMillis;
    // We have to make sure that we have to keep emitting
    while ( !mParticlePool.isEmpty() && // We have particles in the
pool
        mActivatedParticles < mParticlesPerMillisecond*mTotalMillis) {
    // and we are under the number of particles that should be
launched
        // Activate a new particle
        activateParticle(gameEngine);
    }
}
```

代码非常简单。首先我们检查系统是否正在发射。如果系统不在发射，则不需要更新任何内容，因此可以直接返回。

如果系统正在发射，我们会检查是否需要激活一个粒子。为此，我们将已激活的粒子数量与应该已激活的粒子数量（particlesPerMillisecond*totalMilliseconds）进行比较。我们还需要确保我们

