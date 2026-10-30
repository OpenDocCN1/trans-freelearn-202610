

## 使用 PyGame 构建游戏

在这里，我们将学习如何构建这样一个游戏。游戏的基本逻辑是：有一个移动的矩形，我们已知其前导点的坐标。这将作为我们的蛇。它将由四个方向键控制。食物会在屏幕上随机初始化。由于我们在每个时间点都知道蛇的位置，因此我们会检查矩形是否撞到了边界墙壁或自身。如果撞到了，程序就会退出。现在，让我们逐节查看代码；每一节代码之后都会给出相应的解释：

```python
from pygame.locals import *
import pygame
import random
import sys
import time

pygame.init()

fpsClock = pygame.time.Clock()

gameSurface = pygame.display.set_mode((800, 600))
pygame.display.set_caption('Pi Snake')

foodcolor = pygame.Color(0, 255, 0)
backgroundcolor = pygame.Color(255, 255, 255)
snakecolor = pygame.Color(0, 0, 0)
textcolor = pygame.Color(255, 0, 0)

snakePos = [120, 240]
snakeSeg = [[120, 240], [120, 220]]
foodPosition = [400, 300]
foodSpawned = 1
Dir = 'D'
changeDir = Dir
Score = 0
Speed = 5
SpeedCount = 0
```

现在，我们将了解每一块代码的作用，但为了简洁起见，非常基础的内容将被跳过，因为我们在前面的章节中已经学习过它们。

`finish()` 函数之前的前几行代码会初始化 PyGame 并设置游戏参数。`pygame.time.Clock()` 函数用于跟踪游戏内的时间，这主要用于帧率（每秒帧数，即 FPS）。虽然看起来有点微不足道，但 FPS 非常重要，而且可以进行调节。我们可以通过提高或降低 FPS 来控制游戏速度。继续看后面的代码，我们可以选择一些选项，例如屏幕尺寸、蛇的颜色、起始位置、起始速度等。`snakePos` 列表变量存储了蛇头的位置，`snakeSeg` 将以嵌套列表的形式包含蛇身体各段的初始坐标。第一个元素包含蛇头的坐标，第二个元素包含蛇尾的坐标。这段代码还定义了食物的初始位置、食物的状态、初始方向、初始速度和玩家的初始得分：

```python
def finish():
    finishFont = pygame.font.Font(None, 56)
    msg = "Game Over! Score: " + str(Score)
    finishSurf = finishFont.render(msg, True, textcolor)
    finishRect = finishSurf.get_rect()
    finishRect.midtop = (400, 10)
    gameSurface.blit(finishSurf, finishRect)
    pygame.display.flip()
    time.sleep(5)
    pygame.quit()
    exit(0)
```

前面的代码块定义了游戏的结束流程。正如我们将在后面的代码中看到的，当蛇撞到墙壁或自身时，就会调用 `finish()`。在这个过程中，我们首先指定想要显示的消息及其属性（例如字体），然后将最终得分添加到其中。接着，我们通过 `render()` 函数渲染消息，该函数作用于 `finishFont` 变量。然后，我们通过 `get_rect()` 获取一个矩形，最后通过 `blit()` 函数将它们绘制出来。在使用 PyGame 时，`blit()` 是一个非常重要的函数，它允许我们将一个图像绘制在另一个图像之上。在我们的场景中，这非常有用，因为它允许我们在游戏结束时显示的消息之上绘制一个边界矩形。最后，我们通过 `display.flip()` 函数将消息渲染到屏幕上，并在延迟 5 秒后退出游戏：

```python
while 1:
    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            exit(0)
        elif event.type == KEYDOWN:
            if event.key == ord('d') or event.key == K_RIGHT:
                changeDir = 'R'
            if event.key == ord('a') or event.key == K_LEFT:
                changeDir = 'L'
            if event.key == ord('w') or event.key == K_UP:
                changeDir = 'U'
            if event.key == ord('s') or event.key == K_DOWN:
                changeDir = 'D'
            if event.key == K_ESCAPE:
                pygame.event.post(pygame.event.Event(QUIT))
                pygame.quit()
                exit(0)

    if changeDir == 'R' and not Dir == 'L':
        Dir = changeDir
    if changeDir == 'L' and not Dir == 'R':
        Dir = changeDir
    if changeDir == 'U' and not Dir == 'D':
        Dir = changeDir
    if changeDir == 'D' and not Dir == 'U':
        Dir = changeDir

    if Dir == 'R':
        snakePos[0] += 20
    if Dir == 'L':
        snakePos[0] -= 20
    if Dir == 'U':
        snakePos[1] -= 20
    if Dir == 'D':
        snakePos[1] += 20
```

然后，我们继续看无限 while 循环，它包含了游戏的大部分逻辑。另外，如前所述，pygame.event.get() 函数会获取事件的类型；根据按下的按键，它会改变一些参数的状态。例如，按下 Esc 键会导致游戏退出，而按下方向键会改变蛇的方向。之后，我们会检查新方向是否与旧方向完全相反，并且只会在不是相反方向时才改变方向。在这种情况下，changeDir 只是一个中间变量。然后，我们根据所选方向来改变蛇的位置。每次位置移动都代表屏幕上 20 像素的位移：

```python
snakeSeg.insert(0, list(snakePos))
if snakePos[0] == foodPosition[0] and snakePos[1] == foodPosition[1]:
    foodSpawned = 0
    Score = Score + 1
    SpeedCount = SpeedCount + 1
else:
    snakeSeg.pop()

if SpeedCount == 5:
    SpeedCount = 0
    Speed = Speed + 1

if foodSpawned == 0:
    x = random.randrange(1, 40)
    y = random.randrange(1, 30)
    foodPosition = [int(x * 20), int(y * 20)]
    foodSpawned = 1

gameSurface.fill(backgroundcolor)
for position in snakeSeg:
    pygame.draw.rect(gameSurface, snakecolor, Rect(position[0], position[1], 20, 20))
pygame.draw.circle(gameSurface, foodcolor, (foodPosition[0] + 10, foodPosition[1] + 10), 10, 0)
pygame.display.flip()
if snakePos[0] > 780 or snakePos[0] < 0:
    finish()
if snakePos[1] > 580 or snakePos[1] < 0:
    finish()
for snakeBody in snakeSeg[1:]:
    if snakePos[0] == snakeBody[0] and snakePos[1] == snakeBody[1]:
        finish()
fpsClock.tick(Speed)
```

需要记住的是，在此时，屏幕上还没有渲染任何内容。我们只是在实现游戏逻辑，只有当我们完成这些逻辑之后，才会在屏幕上渲染内容。这将通过 pygame.display.flip() 函数完成。另一件重要的事情是，还有一个名为 pygame.display.update() 的函数。这两者之间的区别在于，update() 函数只更新表面的特定区域，而 flip() 函数则更新整个表面。但是，如果我们不给 update() 函数提供任何参数，那么它也会更新整个表面。

现在，既然我们已经改变了蛇头的位置，就必须更新 `snakeSeg` 变量，以反映蛇身上的这一变化。为此，我们使用 `insert()` 方法，并传入要追加的对象位置以及新的坐标。这样就能将蛇头的新坐标添加到 `snakeSeg` 变量中。接下来是有趣的部分：我们检查蛇是否到达了食物所在的位置。如果是，我们就增加分数，将 foodSpawned 状态设置为 False，并将 SpeedCount 增加 1。因此，一旦速度计数达到 5，速度就会增加 1 个单位。如果不是，我们就用 `pop()` 方法移除最后一个坐标。这很有趣，因为如果蛇吃到了食物，那么它的长度就会增加；因此，`else` 语句中的 `pop()` 方法不会执行，蛇的长度就会增加 1 个单位。

在下一段代码中，我们检查食物是否已经生成；如果没有，我们会在考虑屏幕尺寸的情况下随机生成它。random 包中的 `randrange()` 函数正好可以帮助我们做到这一点。

最后，我们进入实际渲染的部分。渲染不过是一个术语，指的是在屏幕上生成并显示某些内容所需的过程。第一条语句用我们选择的背景颜色填充屏幕，以便屏幕上的所有其他内容都清晰可见：

```python
for position in snakeSeg:
    pygame.draw.rect(gameSurface, snakecolor,
                     Rect(position[0], position[1], 20, 20))
```

前面的代码块会遍历 `snakeSeg` 变量中存在的所有坐标，并用初始化代码中为我们的蛇指定的颜色填充它们之间的空间。rect 函数接收三个输入：游戏运行的窗口名称、矩形的颜色，以及由 Rect 函数给出的矩形坐标。Rect 函数本身接收四个参数：矩形的 x 和 y 位置以及高度和宽度。这意味着，对于 `snakeSeg` 变量中包含的每个坐标，我们都会绘制一个尺寸为 20 x 20 像素的矩形。因此，我们可以看到，我们不需要把蛇作为一个整体来跟踪；只需要跟踪描述蛇的坐标即可。

接下来，我们使用 PyGame 包中 draw 模块的 `circle()` 方法来绘制食物。该方法接收五个参数：窗口名称、颜色、圆心、半径以及圆的宽度。圆心由一个元组给出，该元组包含我们之前选择的 x 和 y 坐标。下一条语句 `pygame.display.flip()` 会实际显示我们刚刚绘制的内容。

