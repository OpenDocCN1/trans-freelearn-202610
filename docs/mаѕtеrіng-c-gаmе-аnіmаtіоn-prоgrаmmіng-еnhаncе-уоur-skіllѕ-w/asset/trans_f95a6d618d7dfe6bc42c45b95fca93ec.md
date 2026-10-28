
通常，你会看到以下格式的关卡：

- Khronos Group glTF（.glTF/.glb）：开源 glTF 格式不仅可以用于我们各章中使用的动画 Woman 和 Man 模型等模型，还可以将整个关卡导出为 glTF 文件。
- Collada（.dae）：Collada 是一种古老但完全开放的基于 XML 的文件格式。Collada 也由 Khronos Group 管理，并且该文件格式甚至已经制定了 ISO 标准。
- Wavefront（.obj + .mtl）：许多关卡都采用 Wavefront 文件格式。Wavefront 格式的文件是纯文本（没有二进制组件），该格式文档完善且被广泛支持。
- 通用场景描述（.usd/.usdz）：与其他文件格式相比，**通用场景描述（USD）** 格式相当新。USD 也是开源的且文档完善，但由于文件格式的复杂性，Assimp 中的支持仍处于实验阶段。
- Autodesk FBX（.fbx）：“Filmbox” 格式是专有的，并且大多没有文档，但 Blender 和 Assimp 等工具可以读写这种文件格式。使用 FBX 或多或少需要自担风险，因为可能只有特定版本能按预期工作。

如果这些格式都不符合你的需求，你可能需要扩展某种文件格式，甚至构建自定义文件格式。

## 扩展现有格式或创建自定义格式

从头创建新的游戏关卡或修改现有游戏关卡，可能需要原始文件格式中不存在的额外信息，例如烘焙光照贴图、树木数据、导航网格或细节层次数据。更多信息请参见 *关卡数据可能包含额外数据* 一节。

glTF 等文件格式内置了创建扩展的能力，而其他文件格式可能很难扩展，甚至无法在不破坏现有导入器的情况下扩展。在这种情况下，你可以从头发明自己的文件格式，或者将某种标准文件格式用于关卡数据，并添加一种自定义格式来存储额外数据。

本书前面在 *第 5 章* 的 *添加 YAML 解析器* 一节中创建了一种自定义文件格式：用于存储模型、实例、摄像机、碰撞检测和节点树的所有设置的 YAML 配置文件。虽然我们依赖标准的文本格式将数据存储到磁盘上，但文件的内容是针对我们示例应用的需求而定制的。

将相同的数据存储为二进制格式也是可行的，例如，当解码文本信息耗时过长时。

但创建一种全新的文件格式应该是最后的手段，因为你需要编写所有读写数据的代码，在读写操作期间跟踪文件格式的不同版本，甚至可能还要支持不同的操作系统和硬件架构。维护这样一种自然演化的文件格式可能成为一场噩梦。

更好的方法是使用标准格式，并将所有文件打包到一个存档中，例如 ZIP 文件。当关卡分发给玩家或其他开发者时，你不必担心文件缺失，同时也不需要重新发明轮子去创建一种包罗万象的新文件格式。

这类存档比你想象的更常见。例如，原版 DOOM 中的 WAD 格式和 Quake 系列的 PAK/PK2/PK3 格式，都是为了将全部游戏数据收集到单个文件中而创建的，而且这些文件类型甚至支持补丁更新，因为新存档中的文件会替换旧存档中的同名文件。

## 构建自己的关卡

如果你在互联网上找不到合适的游戏关卡地图怎么办？你仍然可以选择自己创建一个小地图，例如使用 Blender。创建地图超出了本书的范围，但你可以在互联网上找到合适的教程和视频。在“其他资源”一节中可以找到两个示例视频的链接。

在探讨了在应用中分离模型数据和关卡数据的原因之后，我们现在将实现用于加载和处理关卡数据的新代码。

## 导入游戏地图

作为加载关卡的第一步，我们将添加一个名为 AssimpLevel 的新 C++ 类。你可以将 AssimpLevel 类视为 AssimpModel 和 AssimpInstance 这两个类的混合体，它包含模型类的静态顶点数据，以及实例类的位置、旋转或缩放等动态属性。

AssimpLevel 类由两个新文件组成：AssimpLevel.h 和 AssimpLevel.cpp。这两个文件都位于 model 文件夹中，但我们将借用 AssimpModel 和 AssimpInstance 这两个类的大部分方法和成员。

让我们简要地浏览一下 AssimpLevel 类。

## 添加一个 C++ 类来保存关卡数据

由于关卡数据与模型/实例数据非常相似，我们可以重用 AssimpModel 和 AssimpInstance 类中已有的部分功能，例如加载模型文件或执行矩阵运算。

对于静态数据，我们可以从 AssimpModel 类中复制以下方法和成员，并将名称中的 model 部分替换为 level，以与类名保持一致：

- loadModel() 方法，但不包括骨骼、动画和查找表创建
- processNode() 方法，同样不包括骨骼相关部分
- 整个 draw() 方法
- getTriangleCount()、getModelFileName() 和 getModelFileNamePath() 方法
- setModelSettings() 和 getModelSettings() 方法
- mTriangleCount 和 mVertexCount 成员变量
- mRootNode、mNodeList、mRootTransformMatrix、mModelMeshes、mVertexBuffers 和 mModelSettings 成员变量，用于存储可行的关卡数据
- mTextures 和 mPlaceholderTexture 成员变量，用于纹理

对于动态数据，可以从 AssimpInstance 类中复制以下方法和成员，同样将名称中的 model 替换为 level：

- updateModelRootMatrix() 和 getWorldTransformMatrix() 方法
- mLocalTranslationMatrix、mLocalRotationMatrix、mLocalScaleMatrix、mLocalSwapAxisMatrix、mLocalTransformMatrix 和 mModelRootMatrix 成员变量

为了将所有可变的关卡数据放在一个位置，我们在 model 文件夹中创建了 LevelSettings.h 文件，其中包含 LevelSettings 结构体：

```
struct LevelSettings {
    std::string lsLevelFilenamePath;
    std::string lsLevelFilename;
    glm::vec3 lsWorldPosition = glm::vec3(0.0f);
    glm::vec3 lsWorldRotation = glm::vec3(0.0f);
    float lsScale = 1.0f;
    bool lsSwapYZAxis = false;
};
```

如你所见，这些关卡设置也是部分取自模型设置（文件名）和实例设置（位置、旋转、缩放、轴交换）的数据的混合体。LevelSettings 数据类型将用于简化用户界面中的关卡设置，并用于保存和加载关卡相关数据。

我们还通过添加一个名为 micLevels 的 AssimLevel 共享指针向量，以及一个保存向量中当前所选关卡的 micSelectedLevel 整数，使已加载的关卡数据可用于应用的其他部分：

```
std::vector<std::shared_ptr<AssimLevel>> micLevels{};
int micSelectedLevel = 0;
```

管理 micLevels 向量中的 AssimLevel 对象的主要工作将由 renderer 类处理，因此我们第二步将添加方法和回调。

## 添加回调和渲染器代码

新的关卡功能与现有模型功能之间的相似性也延续到了回调中。必须在 Callbacks.h 文件中添加三个新的回调定义：levelCheckCallback、levelAddCallback 和 levelDeleteCallback。

```
using levelCheckCallback = std::function<bool(std::string)>;
using levelAddCallback = std::function<bool(std::string)>;
using levelDeleteCallback =
    std::function<void(std::string)>;
```

对于模型，我们有一组相同的回调。第一个回调 levelCheckCallback 用于检查是否已加载了同名的关卡文件，另外两个回调 levelAddCallback 和 levelDeleteCallback 用于从文件加载新关卡和删除现有关卡对象。

与大多数回调一样，我们也会在 ModelInstanceCamData.h 的 ModelInstanceCamData 结构体中提供这些函数：

```
levelCheckCallback micLevelCheckCallbackFunction;
levelAddCallback micLevelAddCallbackFunction;
levelDeleteCallback micLevelDeleteCallbackFunction;
```
