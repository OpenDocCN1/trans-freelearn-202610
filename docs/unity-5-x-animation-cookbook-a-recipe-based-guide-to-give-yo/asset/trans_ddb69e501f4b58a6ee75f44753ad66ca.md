
19. 创建一个新的 C# 脚本并将其命名为 `Elevator.cs`。编写以下代码：

```
using UnityEngine;
using System.Collections;

public class Elevator : MonoBehaviour
{
    // Update is called once per frame
    void Update ()
    {
        /*When the player presses the E key, we are setting the
        Move trigger on the Animator component. We are assuming
        the Animator component is present on the game object our
        script is attached to*/
        if (Input.GetKeyDown (KeyCode.E)) {
            GetComponent<Animator> ().SetTrigger ("Move");
        }
    }
}
```

20. 将 `Elevator.cs` 脚本分配给 **Elevator** 游戏对象。

21. 选择 **ElevatorFrame** 游戏对象并为其添加一个 **Mesh Collider** 组件。

22. 选择 **Lift** 游戏对象并通过右键菜单选择 **3D Object | Cube** 为其添加三个 **Cube** 游戏对象。使用这些立方体来封装 **Lift** 的地板和两个屏障（具体情况下立方体的数量可能会有所不同）。请参阅以下截图：

![](img/616a75b593bef4212f618360fceea442_87_0.png)

23. 从立方体中移除 **Mesh Renderer** 和 **Mesh Filter** 组件，只保留 **Box Collider** 组件。为立方体赋予适当的名称，例如 `FloorCollider`、`LeftCollider` 和 `RightCollider`。
24. 再次选择 **Lift** 对象。为其添加一个 **Rigidbody** 组件并将其设置为 **Is Kinematic**。
25. 向 **Lift** 游戏对象添加 `Platform.cs` 脚本。您可以在 *为物体的世界位置设置动画 – 创建移动平台配方* 的 *如何操作* 部分第 13 步中找到该脚本。
26. 这个简单的电梯适用于使用 **Rigidbody** 组件的角色。您可以使用 Unity 标准资源包中的 **ThirdPersonCharacter** 预制体，或使用提供的 Unity 示例项目中的预制体。
27. 运行游戏并按下键盘上的 *E* 按钮来触发电梯。

## 它是如何工作的...

本配方有几个关键要素使其发挥作用：

- **动画化的 Lift 游戏对象**：**Lift** 游戏对象使用动画从上到下移动。
- **Lift 作为子游戏对象**：**Lift** 游戏对象是 **Elevator** 游戏对象的子对象，这使得可以复制 **Elevator** 并在场景中的多个位置使用它。
- **Platform.cs 脚本**：我们使用的是 *为物体的世界位置设置动画 – 创建移动平台配方* 中相同的 `Platform.cs` 脚本。该脚本将角色设为 **Lift** 的父级。这样角色就会被迫随 **Lift** 移动，从而防止抖动（您可以尝试从 **Lift** 游戏对象中移除 `Platform.cs` 脚本并在 **Elevator** 中向下移动：角色会上下跳动）。
- **玩家输入**：我们使用玩家输入来设置 **Elevator** 的 Animator Controller 中的 `Move` 触发参数。这会播放动画（**ElevatorGoingUp** 或 **ElevatorGoingDown**，取决于 Elevator 当前所处的状态）。

## 还有更多...

目前，**Elevator** 会在每次玩家输入后移动，无论玩家是否站在 **Lift** 中。您可以使用设置为 **Is Trigger** 的 **Box Collider** 组件并编写脚本来检查玩家是否在 **Lift** 中，只有在这种情况下才允许玩家使用输入。以下是可以实现此功能的示例 `LiftCheck.cs` 脚本（您可以在提供的示例 Unity 项目的 `Chapter 02 Working with the animation view\Recipe 07 Creating an elevator triggered by player input\Scripts` 目录中找到该脚本）：

```
using UnityEngine;
using System.Collections;

public class LiftCheck : MonoBehaviour
{
    Elevator elevatorScript;
    // Use this for initialization
    void Start ()
    {
        /*We try to find the Elevator script on the root
        transform (we are assuming this is the Elevator game
        object)*/
        elevatorScript = transform.root.GetComponent<Elevator> ();
    }
    // This function is called when our character
    enters the trigger
    void OnTriggerEnter (Collider other)
    {
        /*We check if we've found the Elevator script*/
        if (elevatorScript != null) {
            /*We check if the object which entered
            the trigger is the Player*/
            if (other.gameObject.CompareTag ("Player")) {
                /*We enable the Elevator script
                (and enable the input)*/
                elevatorScript.enabled = true;
            }
        }
    }
    // This function is called when our character
    exits the trigger
    void OnTriggerExit (Collider other)
    {
        /*We check if we've found the Elevator script*/
        if (elevatorScript != null) {
            /*We check if the object which exited the
            trigger is the Player*/
            if (other.gameObject.CompareTag ("Player")) {
                /*We disable the Elevator script (and
                disable the input)*/
                elevatorScript.enabled = false;
            }
        }
    }
}
```

## 3

## 2D 和用户界面动画

本章将涵盖以下主题：

- 从 3D 软件包导出 2D 精灵动画
- 使用 Animation View 创建逐帧精灵动画
- 使用 Animation View 创建 2D 精灵木偶动画
- 使用 Animator Controller 播放精灵动画
- 使用 Animation View 创建淡出-淡入过渡
- 使用 Animation View 创建滑动过渡
- 使用填充图像创建动画进度条
- 使用 Mecanim 状态为 UI 按钮状态设置动画

## 简介

在上一章中，我们使用的是 Animation View，它是 Unity 中制作动画的基本工具。本章解释了 **2D 和用户界面动画管线**。在本章中，我们将继续使用 Animation View 创建各种 2D 和 UI 动画。

## 从 3D 软件包导出 2D 精灵动画

绘制动画 2D 精灵可能具有挑战性。一些艺术家在 3D 软件包中创建角色并将其导出为 2D 精灵，以使动画过程更简单。在本第一个配方中，我们将创建这样一个角色并将其动画导出为精灵表。

## 准备工作

开始之前，您需要在所选的 3D 软件包中拥有一个动画模型（我们使用的是 Blender）。您也可以下载提供的示例；在 Unity 中打开项目；转到 `Chapter 03 2D and user interface animation\Recipe 01 Exporting a 2d sprite animation from a 3d package` 文件夹。您将在那里找到一个名为 `Example.unity` 的场景，其中包含一个动画硬币精灵。在 `Animation` 目录中，您可以找到导出的帧和包含它们的精灵表。您还可以找到一个包含硬币模型的 `*.blend` 文件；设置为渲染出动画。

## 如何操作...

要从 3D 软件包导出 2D 精灵动画并将其导入 Unity，请按照以下步骤操作：

1. 在您选择的 3D 软件包中创建一个动画对象。
2. 设置相机以渲染动画对象。相机可以是透视或正交，取决于您想要实现的效果。在此示例（旋转硬币）中，它设置为透视以更好地显示动画。尝试将对象的中心放在画面的中心。
3. 将动画渲染为具有透明背景的单独帧（通常每秒 12 帧足以获得高质量的精灵动画）。使用合理的命名约定（例如 `frame_01.png`、`frame_02.png` 等）。所有帧的像素尺寸应相同。
4. 在所选的图像编辑软件中创建一个新图像（本配方中使用 GIMP）。图像应具有透明背景。
5. 统计所有渲染的帧，并将新创建的图像大小设置为足够大，以容纳所有渲染的帧，彼此相邻（按列和行排列）。例如，大小为 512 × 512 像素的图像最多可容纳 8 个 256 × 256 像素大小的帧、16 个 128 × 128 像素大小的帧或 32 个 64 × 64 像素大小的帧。
6. 将所有帧按列和行排列在新创建的图像上，从第一帧开始，如以下屏幕截图所示：
7. 将图像导出为具有透明背景的 *.png 文件。
8. 将图像导入 Unity（将其拖放到 Project View 或转到 Assets | Import New Asset 并在资源管理器中选择图像文件）。

![](img/616a75b593bef4212f618360fceea442_93_0.png)

