Meanwhile, if our Scene contains thousands of MonoBehaviours with these empty
Update() definitions, then we would be wasting a lot of CPU cycles every frame,
potentially causing havoc on our frame rate.

Let's prove all of this with a simple test. Our test Scene should have GameObjects with two
types of Component, EmptyClassComponent with no methods defined at all and
EmptyCallbackComponent with an empty Update() callback defined:

```
public class EmptyClassComponent : MonoBehaviour {
}

public class EmptyCallbackComponent : MonoBehaviour {
void Update () {}
}
```

The following are the test results for 30,000 Components of each type. If we enable all
GameObjects with attached EmptyClassComponents during runtime, then nothing
interesting happens under the CPU Usage Area of the Profiler. There will be a small
amount of background activity, but none of this activity will be caused by the
EmptyClassComponents. However, as soon as we enable all objects with
EmptyCallbackComponent, we will observe a huge increase in CPU usage:

![](bbox=[0.104, 0.494, 0.895, 0.808])