
## Working with Open Asset Import Library

- All data that will be drawn on the screen is stored in aiMesh structs. Every mesh consists of a number of so-called faces, usually triangles. For every vertex of every face, important data like the vertices, normals, and texture coordinates are stored directly in aiMesh. The drawing order of the vertices is stored separately as indices in the other structures, allowing features like space-saving reuse of vertices.

- For animations, the model “skeleton” consists of bones, stored in aiBone structs. Here, the offset matrix defines the offset between the position in mesh space and the bind pose in bone space (in the glTF file format, this is the “inverse bind matrix”). Additionally, several value pairs are stored for every vertex, each containing a node index and a weight value. In every pair, the weight value specifies the fraction of the nodes movement that is applied to the vertex.

- The position of every vertex in the Assimp meshes can be bound to up to four bones, and the influence of each of these four bones on the final vertex position can be controlled by a weight value between 0 and 1. The weight is used as a scaling factor for the transformations of the specified bone – a value 1 means that the vertex makes the same transformations as the bone, and for a value 0, the vertex ignores the transforms of the bone.

- On the right side of Figure 1.20, the data for animations is shown. The aiAnimation struct contains the animation channels for bones and meshes, the overall duration of the specific animation, and the number of frames per second for this animation.

- As an example for the animations, we will look at the aiNodeAnim struct. This struct consists of key frames with rotations, translations, or scales to apply to a specific node. The node name in the aiNode struct is used to find the corresponding bone to animate.

Currently, more than 50 different file formats are known to Assimp. Some notable examples are listed here:

- Autodesk 3D Studio (.3ds), AutoCAD (.dxf), and FBX (.fbx)

- Collada (.dae) and glTF (.gltf and .glb), managed by the Khronos Group

- Wavefront vertices (.obj plus materials (.mat))

- STL files, mostly known from 3D printing (.stl)

- Game engine formats, i.e., from Quake (.md3/.md5), or Half-Life (.mdl)

Even though the number of formats is impressive, it needs to be stated that not all models you find can be imported into the application by using Assimp.

Several of these file formats are reverse-engineered from closed source applications, and only a subset of versions work. Other formats are open source, like Collada or glTF, but some of those formats are also constantly evolving. Also, not all new features have yet been implemented in Assimp. So, even with a versatile asset import library like Assimp, you may end up with a model that fails to be imported, produces “vertex garbage” on screen, or is missing some of its properties. Nevertheless, Assimp is currently the best open source solution to load many different game and non-game 3D models into your own application.

Loading a model file with Assimp boils down to importing the aiScene object of a file, checking for the existence of other data types, and importing that data into the application. In the next section, we will do a short walk-through of the steps to load a model file with Assimp.

The complete source code for the application can be found in the folder chapter01, the subfolder 01_opengl_assimp for OpenGL, and the subfolder 02_vulkan_assimp for Vulkan.

## Loading a model file

To load a model file with Open Asset Import Library, we must include the following three headers in the implementation file of the AssimpModel model loading class in the model folder:

```
#include <assimp/scene.h>
#include <assimp/Importer.hpp>
#include <assimp/postprocess.h>
```

Next, we create an Assimp::Importer instance in the loadModel() method, and use the importer to load a file from disk:

```
Assimp::Importer importer;
const aiScene *scene = importer.ReadFile(modelFilename,
  aiProcess_Triangulate | aiProcess_GenNormals);
```

We hand over the file name of the asset file we want to load, plus the two values aiProcess_Triangulate and aiProcess_GenNormals as optional postprocessing flags.

The first flag, aiProcess_Triangulate, instructs Assimp to triangulate all polygons with more than three vertices, if those polygons exist in the file. Since our basic renderer only understands triangles, a polygon with more than three vertices would cause graphical errors.

Using aiProcess_GenNormals as the import flag ensures that all triangles have normal vectors. Default normal vectors pointing upward from the surface of the triangle will be created only if no normal vector is found. Existing normals are not changed by the flag.

Next, we must check if the import was successful:

```
if(!scene || scene->mFlags & AI_SCENE_FLAGS_INCOMPLETE ||
    !scene->mRootNode) {
    return false;
}
```

We assume a failed import if the scene pointer itself is a nullptr, the scene is marked as incomplete by the importer, or the scene has no root node defined.

After the model file has been loaded successfully, we will scan the model for embedded textures.

### Loading embedded textures

Some model formats can embed the textures into the object file. If the function call to HasTextures() returns true, we loop over all textures of the model file.

```
if (scene->HasTextures()) {
    unsigned int numTextures = scene->mNumTextures;
    for (int i = 0; i < scene->mNumTextures; ++i) {
```

Inside the loop, we extract the texture name, height, and width, as well as the pixel data of the texture. The texture name is only for information purposes, since the data is embedded into the model file, but having the name is helpful while debugging problems:

```
std::string texName =
    scene->mTextures[i]->mFilename.C_Str();
int height = scene->mTextures[i]->mHeight;
int width = scene->mTextures[i]->mWidth;
aiTexel* data = scene->mTextures[i]->pcData;
```

Now, we create a shared pointer and try to import the texture data. If the import fails with an unexpected error, we stop loading the model:

```
std::shared_ptr<Texture> newTex =
    std::make_shared<Texture>();
if (!newTex->loadTexture(texName, data,
    width, height)) {
    return false;
}
```

In the mNodeList, all nodes are stored in a flat but hierarchical order, guaranteeing us to access any of the nodes before its child nodes. This way, the mNodeList is fast when it comes to updates that need to be done in all nodes. We can just iterate from start to end through the vector.

At the end of processNode(), we check for child nodes, and process any child nodes we find in a recursive manner:

```
unsigned int numChildren = aNode->mNumChildren;
for (unsigned int i = 0; i < numChildren; ++i) {
    std::string childName =
        aNode->mChildren[i]->mName.C_Str();
    std::shared_ptr<AssimpNode> childNode =
        node->addChild(childName);
    processNode(childNode, aNode->mChildren[i],
        scene, assetDirectory);
}
```

After processing all nodes in the hierarchy, we have collected all meshes, textures, and skeletal data from the model.

### Adding vertex buffers for the meshes

Back in the loadModel() method, we create a combined vertex and index buffer object for every mesh, upload the extracted vertex data, and store the buffer in a std::vector called mVertexBuffers:

```
for (auto mesh : mModelMeshes) {
    VertexIndexBuffer buffer;
    buffer.init();
    buffer.uploadData(mesh.vertices, mesh.indices);
    mVertexBuffers.emplace_back(buffer);
}
```

To draw the imported model, we can now simply iterate over the mModelMeshes vector and use the drawIndirect() call of the VertexIndexBuffer class to draw all triangles of this specific mesh with only a single draw command. The method is called “indirect” because Assimp stores the model data internally as vertices plus indices, and we draw the triangles in the indirect mode, via the indices.
