# Extending Camera Handling

We are using an overloaded getSSBODataMat4() method to read a single glm::mat4 at the position of the selected bone of the selected instance. To be able to read more than one matrix from an SSBO, getSSBODataMat4() returns a std::vector<glm::mat4>. But since we only need a single matrix, we extract the first matrix of the vector by using .at(0).

The camera offset matrix is calculated with a simple glm::translate() call:

```
glm::mat4 offsetMatrix = glm::translate(glm::mat4(1.0f),
	camSettings.csFirstPersonOffsets);
```

Finally, a slightly monstrous call is used to calculate the position of the bone:

```
cam->setBoneMatrix(
	mWorldPosMatrices.at(selectedInstance) *
	boneMatrix * offsetMatrix *
	glm::inverse(modelType.second.at(0))
	->getModel()->getBoneList().at(selectedBone)
	->getOffsetMatrix()));
```

By using the correct matrix multiplication order (depending on the library, GLM uses right to left), the following operations are done:

+   1. Get the offset matrix of the selected bone from the model and calculate the inverse of the offset matrix. The offset matrix of a bone contains the offset of that bone between the skinning position and the default T-pose.
+   
+   2. Multiply the inverse of the bone offset matrix by the camera position offset matrix, moving the camera slightly away from the bone offset.
+   
+   3. Multiply the final bone TRS matrix by the previous product of the bone offset and camera matrices, moving the camera to the correct position of that bone relative to the model root.
+   
+   4. Multiply the world position offset of the selected instance, moving the camera to the selected bone of our selected instance.

After the bone matrix has been calculated, we need to update the camera and re-upload the view and projection matrix to the GPU:

```
cam->updateCamera(mRenderData, deltaTime);
	mViewMatrix = cam->getViewMatrix();
	std::vector<glm::mat4> matrixData;
	matrixData.emplace_back(mViewMatrix);
```