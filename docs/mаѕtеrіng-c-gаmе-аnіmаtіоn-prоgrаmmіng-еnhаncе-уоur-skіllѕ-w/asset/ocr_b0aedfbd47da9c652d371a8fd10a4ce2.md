But, in addition, we also check if the currently selected instance is different compared to the new mCurrentInstance pointer:

```
if (mCurrentInstance !=
    modInstData.miAssimpInstances.at(
    modInstData.miSelectedInstance)) {
    mSavedInstanceSettings = settings;
    mCurrentInstance =
    modInstData.miAssimpInstances.at(
    modInstData.miSelectedInstance);
}
}
```

Whenever the current instance changes, we store the new selected instance in the mCurrentInstance pointer, and we also save the instance settings we just retrieved in the mSavedInstanceSettings variable.

The mSavedInstanceSetting variable enables us now to revert the settings of the instance, undoing any changes. To toggle the setting revert, we add a new button named Reset Values to Previous below the existing Reset Values to Zero button:

```
ImGui::SameLine();
if (ImGui::Button("Reset Values to Previous")) {
    settings = mSavedInstanceSettings;
}
```

To undo any changes, we copy the saved instance settings back to the current settings variable. At the end of the CollapsingHeader Instances, the values in settings are saved to the currently selected instance. Et voilà, we have reverted the settings to the state when we selected the instance.

Applying the changes just does the copy operation the other way around:

```
ImGui::SameLine();
if (ImGui::Button("Apply Changes")) {
    mSavedInstanceSettings = settings;
}
```

Here, the saved settings are overwritten by the current settings of the instance, making the current settings the new default for an undo operation.