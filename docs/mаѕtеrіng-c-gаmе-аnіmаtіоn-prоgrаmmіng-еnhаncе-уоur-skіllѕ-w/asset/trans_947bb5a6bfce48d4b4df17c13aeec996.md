

对于用户界面，我们应禁用对默认自由摄像机的名称更改，当摄像机实例 0 被选中时，通过在名称字段周围使用 ImGui::BeginDisabled() 和 ImGui::EndDisabled() 的调用来实现。

图 6.1 显示了摄像机部分的 resulting 用户界面：

![](img/3cfb9832c943f5f7366fd104b5255971_189_0.png)

图 6.1：新的摄像机设置

在摄像机之间切换现在变得和选择新的基础模型或新的动画片段一样简单。除了组合框之外，还添加了两个箭头，允许我们直接选择上一个和下一个摄像机。

对于真正的摄像机管理，缺少两个功能：创建新摄像机和删除现有摄像机，除默认摄像机外。

## 添加和删除摄像机

要创建摄像机，可以采用两种路径：

- 在原点添加摄像机，使用默认值
- 克隆当前选中的摄像机及其所有设置，除了名称

强迫用户返回原点并将新摄像机移动到虚拟世界中的期望位置感觉有些不妥。更好的解决方案是能够克隆当前选中的摄像机，因为用户很可能希望使虚拟世界的当前视图持久化。

克隆和删除功能都由新的回调处理，将检查负担和所有工作从用户界面转移到渲染器。新的渲染器方法，名为 cloneCamera() 和 deleteCamera()，简短而简单，因此我们在此跳过其列表。

然而，处理摄像机名称需要一些额外的注意。组合框中重复的名称会令人困惑，因此我们在克隆摄像机时必须找到一种解决方案来创建唯一名称。创建新名称的一种简单方法是向名称附加一个数字，并在进一步克隆时将数字递增。

处理新名称的方法称为 generateUniqueCameraName()，定义在渲染器类中。该方法只有一个参数，即摄像机基本名称。

首先，我们复制基本名称，因为我们在 while 循环期间如果摄像机名称已被使用会调整名称，并定义一个名为 matches 的字符串，包含从零到九的所有数字：

```
std::string camName = camBaseName;
std::string matches("0123456789");

while (checkCameraNameUsed(camName)) {
```

在循环中，我们检查摄像机名称是否已有数字作为后缀。如果没有，我们简单地附加一个 1：

```
const auto iter = std::find_first_of(camName.begin(),
    camName.end(), matches.begin(), matches.end());
if (iter == camName.end()) {

    camName.append("1");
} else {
```

如果我们找到一个数字，那么我们将不带尾随数字的摄像机名称保存到 cameraNameString 中，将现有的摄像机数字转换为 int，将数字递增 1，将新数字转换回字符串，并组合原始摄像机名称和新数字：

```
std::string cameraNameString = camName.substr(0,
    std::distance(camName.begin(), iter));
std::string cameraNumString = camName.substr(
    std::distance(camName.begin(), iter));
int cameraNumber = std::stoi(cameraNumString);
camName = cameraNameString +
    std::to_string(++cameraNumber);
}
}
```

这样，我们在克隆现有摄像机时可以创建唯一但仍然可理解的摄像机名称。

generateUniqueCameraName() 中的 while 循环使用另一个名为 checkCameraNameUsed() 的新方法。检查摄像机名称是否已被使用是通过遍历现有摄像机并将摄像机名称与建议的新名称进行比较来完成的：

```
bool OGLRenderer::checkCameraNameUsed(std::string
  cameraName) {
  for (const auto& cam : mModelInstCamData.micCameras) {
    if (cam->getCameraSettings().csCamName == cameraName) {
      return true;
    }
  }
  return false;
}
```

相同的 checkCameraNameUsed() 方法也将在用户界面中使用，以在摄像机重命名时检测重复名称。如用户界面中的大多数地方一样，使用回调到渲染器来执行名称检查，将工作转移到渲染器类。

在图 6.2 中，克隆和删除摄像机的新按钮显示了出来，以及多次按下 Clone Current Camera 按钮的结果：

![](img/3cfb9832c943f5f7366fd104b5255971_191_0.png)

图 6.2：克隆和删除的新按钮，以及一些新摄像机

作为摆脱单一摄像机的最后一步，我们必须更改基于 YAML 的配置文件以反映新的摄像机配置。

## 调整摄像机配置的加载和保存

为了加载和保存新的摄像机设置，我们必须在 tools 文件夹中的 Yam1Parser 类中更改 YAML 解析和发射。

解码 YAML 摄像机节点可以与解码 glm::vec3 或 InstanceSettings: 类似，通过在头文件 Yam1ParserTypes.h 中新的 convert 模板块中添加新的 decode 方法来实现：

```
Template<
  struct convert<CameraSettings> {

    static bool decode(const Node& node,

        CameraSettings& rhs) {

      CameraSettings defaultSettings = CameraSettings{};

      rhs.csCamName =
        node["camera-name"].as<std::string>();

      try {

        rhs.csWorldPosition =

          node["position"].as<glm::vec3>();

      } catch (...) {

        rhs.csWorldPosition =

          defaultSettings.csWorldPosition;

      }

      try {

        rhs.csViewAzimuth =
          node["view-azimuth"].as<float>();
```

```
} catch (...) {
    rhs.csViewAzimuth = defaultSettings.csViewAzimuth;
}

try {
    rhs.csViewElevation =
        node["view-elevation"].as<float>();
} catch (...) {
    rhs.csViewElevation =
        defaultSettings.csViewElevation;
}
```

在解析节点时处理抛出的异常可确保在格式错误或缺少键的情况下尽可能恢复摄像机配置。无法解析摄像机名称时仍会跳过整个摄像机以避免命名问题。

我们还添加 encode 方法以使模板完整实现。有了 decode 函数，我们只需将 CameraSettings 类型传递给 .as<>() 调用即可读取包含摄像机设置的节点：

```
... camNode.as<CameraSettings>();
```

对于向保存文件发射摄像机配置，我们创建一个序列并为每个可用摄像机添加一个映射：

```
...
mYamlEmit << YAML::BeginMap;
mYamlEmit << YAML::Key << "cameras";
mYamlEmit << YAML::Value;
mYamlEmit << YAML::BeginSeq;
for (const auto& cam : modInstCamData.micCameras) {
    CameraSettings settings = cam->getCameraSettings();
    mYamlEmit << YAML::BeginMap;
    mYamlEmit << YAML::Key << "camera-name";
    mYamlEmit << YAML::Value << settings.csCamName;
...
}
```

```
mYamlEmit << YAML::EndSeq;
 mYamlEmit << YAML::EndMap;
```

在保存的 YAML 配置文件中，将为每个摄像机出现一个新的序列，存储所有摄像机设置作为键/值对：

```
cameras:
  - camera-name: FreeCam
    position: [2.9061296, 11.1587305, 64.1114578]
    view-azimuth: 289.300262
    view-elevation: -34.4999695
```

由于我们对配置文件进行了基本更改，我们应该通过新的版本号来反映新的磁盘格式。

## 提升配置文件版本号

调整发射器中的配置文件版本号很容易；我们只需将版本号字符串从 1.0 提高到类似 1.1 或 2.0。从现在开始，所有保存的配置文件都将具有新的版本号。

加载配置文件现在变得有点复杂。如果我们想支持旧文件格式，我们必须保留用于解析先前文件版本内容的所有方法。在读取要解析的文件的版本号后，我们必须决定是解析多个摄像机的新序列样式，还是获取单一摄像机的设置并将这些设置应用于始终可用的默认自由摄像机。保存配置可以在新版本中完成，从而将配置文件从旧版本迁移到新版本。

通过支持读取旧版本来更新配置是实践课程部分中的任务之一。

我们现在可以创建一堆自由浮动的摄像机，将摄像机定位在我们的虚拟世界中，并通过用户界面在摄像机之间切换。听起来还是有点无聊，不是吗？让我们通过添加不同类型的摄像机来进一步增强应用程序。

## 创建不同类型的摄像机

在虚拟世界中添加多个摄像机的能力是添加一些预定义摄像机设置的一个良好基础。不同的摄像机类型在视图移动和摄像机本身移动方面表现不同。

