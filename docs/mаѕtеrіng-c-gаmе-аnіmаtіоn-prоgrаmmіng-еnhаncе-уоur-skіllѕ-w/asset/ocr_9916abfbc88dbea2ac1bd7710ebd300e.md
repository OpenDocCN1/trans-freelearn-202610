The addModel() method for the OpenGL renderer looks like this:

```
bool OGLRenderer::addModel(std::string modelFileName) {
    if (hasModel(modelFileName)) {
        return false;
    }
}
```

First, we check if the model has been already loaded. To avoid confusion, a model file can be loaded only once. Having the exact same model file loaded twice (or more times) makes little sense.

Now, we try to load the specified model file:

```
std::shared_ptr<AssimpModel> model =
    std::make_shared<AssimpModel>();
if (!model->loadModel(modelFileName)) {
    return false;
}
```

When the loading fails, the model also returns false to the caller, even though the return value is not used currently.

If the model file can be loaded, it will be placed into a std::vector of models:

```
mModelInstData.miModelList.emplace_back(model);
```

From this list, the UI generates the combo box with the currently loaded model.

We also add an instance of the model and return the information about a successfully loaded model to the caller:

```
addInstance(model);
return true;
```

For a loaded model to be helpful, we need to create at least one instance, so we have something to render in the world. We use the already implemented way of creating a first instance, instead of having a separate solution.

The addInstance() method is also only a couple of lines long:

```
std::shared_ptr<AssimpInstance>
OGLRenderer::addInstance(std::shared_ptr<AssimpModel>
    model) {
```