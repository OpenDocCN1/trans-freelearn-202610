It is achieved by animating the public variables of a script.

![](bbox=[0.11, 0.16, 0.88, 0.42])

Animated fog

# How to do it...

To animate public script variables, follow these steps:

+   1. Create a new script and call it `FogAnimator.cs`.
+   2. Create two public variables in this script: `public float fogDensity` and `public Color fogColor`.
+   3. In this script's `Update()` function, we call the **ChangeFog** Trigger in the controller when player presses the space bar. We also set the `RenderSettings.fogColor` and `RenderSettings.fogDensity` parameters using our public variables. We also adjust the **Main Camera**'s background color to match the fog color:

```
```csharp
if (Input.GetKeyDown(KeyCode.Space))
{
    anim.SetTrigger("ChangeFog");
}
RenderSettings.fogColor = fogColor;
RenderSettings.fogDensity = fogDensity;

Camera.main.backgroundColor = fogColor;
```
```