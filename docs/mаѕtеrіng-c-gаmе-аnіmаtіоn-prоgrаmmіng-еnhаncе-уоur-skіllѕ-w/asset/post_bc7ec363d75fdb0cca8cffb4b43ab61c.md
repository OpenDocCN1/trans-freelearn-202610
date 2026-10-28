
Usually, you will find levels in the following formats:

- Khronos Group glTF (.glTF/.glb): The open source glTF format can not only be used for models like the animated Woman and Man models we are using in the chapters, but also entire levels can be exported as glTF files.
- Collada (.dae): Collada is an old but fully open XML-based file format. Collada is also managed by the Khronos Group, and even an ISO standard for the file format has been created.
- Wavefront (.obj + .mtl): Many levels can be found in the Wavefront file format. Files in Wavefront format are pure text (no binary components) and the format is well documented and widely supported.
- Universal Scene Description (.usd/.usdz): The **Universal Scene Description (USD)** format is quite new compared to the other file formats. USD is also open source and well documented, but the support in Assimp is still experimental due to the complexity of the file format.
- Autodesk FBX (.fbx): The “Filmbox” format is proprietary and mostly undocumented, but tools such as Blender and Assimp can read and write this file format. Using FBX is more or less at your own risk as it is possible that only specific versions are working as expected.

If none of these models fits your needs, you may have to extend a file format, or even build a custom file format.

## Extending existing formats or creating a custom format

Creating new game levels from scratch or modifying existing game levels may require additional information that is not available in the original file format, such as baked-in light maps, tree data, navigation meshes, or level-of-detail data. See the *Level data may contain additional data* section for more information.

File formats such as glTF have the built-in ability to create extensions, while other file formats may be hard or impossible to extend without breaking existing importers. In such a case, you could invent your own file format from scratch or use a combination of one of the standard file formats for level data and add a custom format to store additional data.

A custom file format was created earlier in this book in the *Adding a YAML parser* section of *Chapter 5*: the YAML configuration file to store all settings of the models, instances, cameras, collision detection, and node trees. Although we are relying on a standard textual format to store the data on disk, the contents of the file are tailored to fit the needs of our example application.

Storing the same data in a binary format would also be possible, for instance, when decoding the text information will take too long.

But creating an entirely new file format should the last resort since you will have to write all the code to read and write the data, keep track of different versions of the file format during read and write operations, and maybe even support different operating systems and hardware architectures. Maintaining such an organically grown file format can become a nightmare.

A better way is to use standard formats and bundle all files into an archive, for instance, into a ZIP file. You don’t have to care about missing files when the level is distributed to players or other developers, but at the same time, you do not need to reinvent the wheel by creating a new, all-encompassing file format.

Such archives are more common than you might think. Formats such as WAD from the original DOOM and PAK/PK2/PK3 for the Quake series were created to collect all game data in a single file, and these file types even support patching as files in new archives replace files of the same name in old archives.

Building your own levels

What if you can’t find a suitable game level map on the internet? You still have the option to create a small map by yourself, for instance, with Blender. Creating a map is out of the scope of this book, but you will find suitable tutorials and videos on the internet. You can find links to two example videos in the Additional resources section.

After we explored the reasons why separating model and level data in the application, we will now implement new code for loading and processing level data.

## Importing a game map

As the first step to load a level, we will add a new C++ class named AssimpLevel. You can think of the AssimpLevel class as a mix of the two classes, AssimpModel and AssimpInstance, containing the static vertex data of the model class and dynamic properties such as the position, rotation, or scale of the instance class.

The AssimpLevel class consists of two new files, AssimpLevel.h and AssimpLevel.cpp. Both files are in the model folder, but we will borrow most of the methods and members from the two classes, AssimpModel and AssimpInstance.

Let’s do a brief walkthrough of the AssimpLevel class.

## Adding a C++ class to hold the level data

Since the level and model/instance data are so similar, we can reuse parts of the functionality we already have in the AssimpModel and AssimpInstance classes, such as loading the model file or doing matrix operations.

For the static data, we can copy the following methods and members from the AssimpModel class, exchanging the model part in the name with level to be consistent with the class name:

- The loadModel() method, but without bones, animations, and lookup table creation
- The processNode() method, again without the bone-specific part
- The entire draw() method
- The getTriangleCount(), getModelFileName(), and getModelFileNamePath() methods
- The setModelSettings() and getModelSettings() methods
- The mTriangleCount and mVertexCount member variables
- The mRootNode, mNodeList, mRootTransformMatrix, mModelMeshes, mVertexBuffers, and mModelSettings member variables to store the viable level data
- The mTextures and mPlaceholderTexture member variables for the textures

For the dynamic data, the following methods and members can be copied from the AssimpInstance class, again replacing the word model with level in the names:

- The updateModelRootMatrix() and getWorldTransformMatrix() methods
- The mLocalTranslationMatrix, mLocalRotationMatrix, mLocalScaleMatrix, mLocalSwapAxisMatrix, mLocalTransformMatrix, and mModelRootMatrix member variables

To have all the variable level data in one location, we create the LevelSettings.h file in the model folder, containing the LevelSettings struct:

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

As you can see, these level settings are also a mix of data taken partially from model settings (filenames) and instance settings (position, rotation, scale, axis swap). The LevelSettings data type will be used to simplify the level settings in the user interface and to save and load level-related data.

We also make the loaded level data available to other parts of the application by adding a vector of AssimLevel shared pointers called micLevels and the micSelectedLevel int that holds the currently selected level from the vector:

```
std::vector<std::shared_ptr<AssimLevel>> micLevels{};
int micSelectedLevel = 0;
```

The main work to manage the AssimLevel objects in the micLevels vector will be handled by the renderer class, so we add methods and callbacks as the second step.

## Adding callbacks and renderer code

Similarities between the new level functionality and existing model functions also continue in the callbacks. Three new callback definitions named levelCheckCallback, levelAddCallback, and levelDeleteCallback must be added to the Callbacks.h file:

```
using levelCheckCallback = std::function<bool(std::string)>;
using levelAddCallback = std::function<bool(std::string)>;
using levelDeleteCallback =
    std::function<void(std::string)>;
```

For the models, we have the same set of callbacks. The first callback, levelCheckCallback, is used to check if a level file with the same name is already loaded, and the other two callbacks, levelAddCallback and levelDeleteCallback, are used to load a new level from a file and to delete an existing level object.

Like for most of the callbacks, we will also make the functions available in the ModelInstanceCamData struct in ModelInstanceCamData.h:

```
levelCheckCallback micLevelCheckCallbackFunction;
levelAddCallback micLevelAddCallbackFunction;
levelDeleteCallback micLevelDeleteCallbackFunction;
```
