First, we get the instance and calculate the view direction from the camera to the instance position:

```
std::shared_ptr<AssimpInstance> instance =
    mCamSettings.csInstanceToFollow.lock();
glm::vec3 viewDirection = instance->getWorldPosition() -
    mCamSettings.csWorldPosition;
```

Next, we set the elevation angle of the view to the cosine of the angle between the view direction to the instance and the upward-pointing mWorldUpVector:

```
mCamSettings.csViewElevation = 90.0f -
    glm::degrees(
        glm::acos(
            glm::dot(
                glm::normalize(viewDirection), mWorldUpVector
            )
        )
    );
```

Using the dot product between the view direction and the upward-pointing vector has been used in first- and third-person cameras in a very similar way.

Then, we map the same view direction vector to two dimensions and calculate the angle between the flattened view direction vector and a vector pointing toward the negative z axis:

```
float rotateAngle =
glm::degrees(
    glm::acos(
        glm::dot(
            glm::normalize(
                glm::vec3(viewDirection.x, 0.0f, viewDirection.z)),
            glm::vec3(0.0f, 0.0f, -1.0f)
        )
    )
);
```

Finally, we extend the azimuth angle to a full 360 degrees:

```
if (viewDirection.x < 0.0f) {
    rotateAngle = 360.0f - rotateAngle;
}
```