
- LegUp.R（并且 **LegUpTarget.R** 以它为父级）
- Leg.L（并且 **LegTarget.L** 以它为父级）
- Leg.R（并且 **LegTarget.R** 以它为父级）
- Foot.L（并且 **FootTarget.L** 以它为父级）
- Foot.R（并且 **FootTarget.R** 以它为父级）
- ArmUp.L（并且 **ArmUpTarget.L** 以它为父级）
- ArmUp.R（并且 **ArmUpTarget.R** 以它为父级）
- Arm.L（并且 **ArmTarget.L** 以它为父级）
- Arm.R（并且 **ArmTarget.R** 以它为父级）
- Hand.L（并且 **HandTarget.L** 以它为父级）
- Hand.R（并且 **HandTarget.R** 以它为父级）

16. 有时我们的 **ImportedRig** 结构不同，例如它可能有更多的脊柱骨骼（三根或更多）。在这种情况下，我们应该在 **Copy Transform** 约束中选择最合适的骨骼，或添加两个 **Copy Rotation** 约束并通过设置 **Influence** 滑块来插值两个相邻骨骼的最终旋转：

![](img/616a75b593bef4212f618360fceea442_308_0.png)

Control rig with Copy Transform constraints

17. 现在我们需要为脚设置 IK。选择 **FinalRig** 并进入 **Edit Mode**。

18. 复制两个脚骨骼并将其名称更改为 **footTarget.L** 和 **footTarget.R**。

19. 移除它们的父级并取消选择 **Deform** 选项（它们不应变形网格）。

20. 进入 **Pose Mode**。
21. 选择 **shin.L** 骨骼并添加一个 **Inverse Kinematics** 约束。将 **Chain Length** 设置为 2，并选择 **FinalRig** 作为 **Target**，**footTarget.L** 作为 **Bone**。
22. 选择 **foot.L** 骨骼并为其添加 **Copy Rotation** 约束。选择 **FinalRig** 作为 **Target**，**footTarget.L** 作为 **Bone**。
23. 对右腿重复步骤 21 和 22。
24. 选择 **hips** 骨骼并为其添加 **Copy Rotation** 和 **Copy Location** 约束。在两个约束中，均将 **Target** 设置为 **HipsTarget** 空对象。
25. 在 **Copy Location** 约束中选择 **Offset** 选项，并在 **Pose Mode** 中移动 **hips** 骨骼，使其大致匹配场景中 **HipsTarget** 空对象的位置。
26. 选择 **footTarget.L** 骨骼。为其添加 **Copy Location** 和 **Copy Rotation** 约束。在两个约束中，均选择 **FootTarget.L** 空对象作为 **Target**。
27. 在 **Copy Location** 约束中选择 **Offset** 选项，并在 **Pose Mode** 中移动 **footTarget.L** 骨骼，使其大致匹配场景中 **footTarget.L** 空对象的位置。
28. 对右脚目标重复步骤 26 和 27。
29. 为每个其他骨骼（包括具有 **Inverse Kinematics** 约束的骨骼）添加一个 **Copy Rotation** 约束。将每个此类约束的 **Target** 设置为相应的空对象（对于 **shin.L** 骨骼，选择 **LegTarget.L** 空对象，以此类推）。
30. 你应该会看到 **ImportRig** 的运动被转移到 **FinalRig**。此外，你可以移动 **hips** 骨骼以及 **footTarget.L** 和 **footTarget.R** 骨骼来调整动画。
31. 如果网格上出现任何错误，尤其是角色的四肢以奇怪的方式旋转，你可以旋转目标空对象来修复此问题。
32. 你也可以旋转目标空对象来轻松更改角色的整体姿势。
33. 要完成重定向过程，请将文件另存为新文件。在 **Pose Mode** 中选择 **FinalRig** 中的所有骨骼，按空格键，输入 **Bake Action**，然后按 *Enter*。
34. 在 **Bake Action** 窗口中，选择 **Visual Keying** 和 **Clear Constraints**。你也可以为新创建的动作提供名称。如果点击 **OK**，所有约束将被移除，最终动画将被烘焙。
35. 记得在 **Action Editor** 中选择该动作附近的小 **F** 符号（这将在 rig 不使用它时仍保存数据）。

36. 移除所有空对象和 **ImportedRig**（你可能还需要手动移除其动作）。保存文件。
37. 将文件导入 Unity 并将 rig 设置为 Humanoid。动画应按预期工作。你也可以选择先将文件导出为 FBX。如果遇到任何问题，请尝试使用 6.1 版本的 FBX 格式，这可能会有所帮助：

![](img/616a75b593bef4212f618360fceea442_310_0.png)

FinalRig on the left and ImportedRig and the empties (control rig) on the right

## How it works...

Problems with importing motion capture data can be very frustrating. The most common errors are connected with an inappropriate **T-POSE** of the **BVH** rig. This T-Pose depends on the software the animation was recorded in and exported from. Additionally, BVH rigs can have arbitrary bone hierarchies, with more than two spine bones for instance. A proper Unity Humanoid rig hierarchy was covered in the *Configuring Generic and Humanoid rigs* recipe in *Chapter 1, Working with animations.*

In Blender, the Rigify rig has a proper T-Pose. Thus, the best solution is to retarget the animation from the **ImportRig** to the **FinalRig** (the **Rigify** one). In theory, we could omit creating the control rig (the empties) but those give us more control. The (...)**Target** empties are not constrained to the **ImportRig** bones—they are only parented to the empties that are constrained. This way we can rotate or move the (...)**Target** empties to correct any rotation errors or adjust a pose.

We also use **Inverse Kinematics** for the feet in this setup. This, combined with the **Offset** option in the **Copy Location** constraints (set for the **footTarget.L**, **footTarget.R**, and **hips** bones), allows us to adjust the feet and hips positions. This way we can move the character to the center of the scene quite easily. We can even make it crouch while moving,

## See also

If you are on a budget and want to use motion capture data, you can find a lot of free motions on the Carnegie Mellon University's website at http://mocap.cs.cmu.edu/. Another good option is to invest in the Perception Neuron solution (https://neuronmocap.com/)—it is a project successfully founded on Kickstarter. Perception Neuron seems affordable for most indie studios or even individuals. It is a suit with a set of sensors (gyroscopes, magnetometers, and accelerometers) that the actor puts on. It works similar to standard motion capture solutions based on markers. I must say the quality of the recorded motions is mostly very good (it has some problems with rapid motions though). You also have to watch out for large metal objects, speakers, and other sources of magnetic fields in the room. But if you can't afford a motion capture studio session and want to use mocap for your project, it can be a good bet.

## Adding behaviors to Mecanim states

In this last recipe, we will cover behaviors. These are scripts that can be attached to Mecanim states. You can use them to turn standard Animator Controllers into logic graphs such as AI trees, quests, and so on.

## Getting ready

We are going to create this recipe from scratch and we don't need any special assets. You can download the example project and go to the Chapter 10 Miscellaneous\Recipe 05 Adding behaviors to Mecanim states directory. Open the Example.unity scene there and play the game. If you play the game, you can press the space bar to change the states in the Animator Controller attached to the Controller game object. The cycle starts in the NoBehaviors state—this is simply an empty state with no additional behavior. If we press the space bar, we transition to Light1Random. This state changes the color of the Light1 light in the scene. If we press the space bar again, the state will change to WaitAndSwitch. This state waits for a given time and switches to the next state: Light2Random. This one changes the color of the Light2 light. If we press the space bar again, we will transition to the RotateObject state, which rotates an object visible in the scene. Pressing the space bar yet again transitions the controller to the Light1Random state again. See the following screenshot for reference:
