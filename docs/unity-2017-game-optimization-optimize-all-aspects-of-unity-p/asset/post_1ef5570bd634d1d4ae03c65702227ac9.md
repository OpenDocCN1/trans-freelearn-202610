

As you can see, the `GetComponent<T>()` method is only a tiny fraction faster than `GetComponent(typeof(T))`, whereas `GetComponent(string)` is significantly slower than the alternatives. Therefore, it is pretty safe to use either of the type-based versions of `GetComponent()` because of the small performance difference. However, we should ensure that we never use `GetComponent(string)` since the outcome is identical, and there is no benefit for the costs incurred. There are some very rare exceptions, like if we were writing a custom debug console for Unity, which might parse a user-inputted `string` to acquire a Component. In any case, these would be used for debugging and diagnostics situations only, where performance isn't too important. For a production-level application, the use of `GetComponent(string)` is just a needless waste.

## Remove empty callback definitions

The primary means of scripting in Unity is to write callback functions in classes derived from `MonoBehaviour`, which we know Unity will call when necessary. Perhaps the four most commonly used callbacks are `Awake()`, `Start()`, `Update()`, and `FixedUpdate()`.

`Awake()` is called the moment a `MonoBehaviour` is first created, whether this occurs during Scene initialization or when a new `GameObject` containing the `MonoBehaviour` is instantiated at runtime from a Prefab. `Start()` will be called shortly after `Awake()` but before its first `Update()`. During Scene initialization, every `MonoBehaviour` Component's `Awake()` callback will be called before any of their `Start()` callbacks are.

After this, `Update()` will be called repeatedly, each time the Rendering Pipeline presents a new image. `Update()` will continue to be called provided the `MonoBehaviour` is still present in the Scene, it is still enabled, and its parent `GameObject` is active.

Finally, `FixedUpdate()` is called just prior to when the Physics Engine updates. Fixed Updates are used whenever we want activity similar in behavior to `Update()` but that isn't tied directly to the render frame rate and is called more consistently over time.

Refer to the following page in the Unity documentation for an accurate picture of when various Unity callbacks are called: https://docs.unity3d.com/Manual/ExecutionOrder.html.

Whenever a MonoBehaviour is first instantiated in our Scene, Unity will add any defined callbacks to a list of function pointers, which it will call at key moments. However, it is important to realize that Unity will hook into these callbacks even if the function body is empty. The core Unity Engine has no awareness that these function bodies may be empty and only knows that the method has been defined and, therefore, that it must acquire it and then call it when necessary. Consequently, if we leave empty definitions of these callbacks scattered throughout the codebase, then they will waste a small amount of CPU due to the overhead cost of the engine invoking them.

This can be a problem since any time we create a new MonoBehaviour script file in Unity, it will automatically generate two boilerplate callback stubs for us for Start() and Update():

```
// Use this for initialization
void Start () {
}

// Update is called once per-frame
void Update () {
}
```

It can be easy to accidentally leave these empty definitions on scripts that don't actually need them. An empty Start() definition is liable to cause any object to initialize a little slower, for no good reason. This effect may not be particularly noticeable for a handful of MonoBehaviours, but as development on the project continues and we populate our scenes with thousands of custom MonoBehaviours with lots of empty Start() definitions, it could start to become a problem, causing slow Scene initialization and wasting CPU time whenever a new Prefab is created via GameObject.Instantiate().

Such calls typically happen during key gameplay events; for instance, when two objects collide, we might spawn a Particle Effect, create some floating damage text, play a sound effect, and so on. This can be a critical moment for performance, because we've suddenly requested that the CPU make a lot of complicated changes, but with only a finite amount of time to complete them before the current frame ends. If this process takes too long, then we would experience a frame drop as the Rendering Pipeline isn't allowed to present a new frame until all of the Update() callbacks (counted across all MonoBehaviours in the Scene!) have finished. Ergo, a bunch of empty Start() definitions being called at this time is a needless waste and could potentially cut into our tight time-budget at a critical moment.

Meanwhile, if our Scene contains thousands of MonoBehaviours with these empty Update() definitions, then we would be wasting a lot of CPU cycles every frame, potentially causing havoc on our frame rate.

Let's prove all of this with a simple test. Our test Scene should have GameObjects with two types of Component, EmptyClassComponent with no methods defined at all and EmptyCallbackComponent with an empty Update() callback defined:

```
public class EmptyClassComponent : MonoBehaviour {
}

public class EmptyCallbackComponent : MonoBehaviour {
void Update () {}
}
```

The following are the test results for 30,000 Components of each type. If we enable all GameObjects with attached EmptyClassComponents during runtime, then nothing interesting happens under the CPU Usage Area of the Profiler. There will be a small amount of background activity, but none of this activity will be caused by the EmptyClassComponents. However, as soon as we enable all objects with EmptyCallbackComponent, we will observe a huge increase in CPU usage:

![](img/da79484c9b30dc915216dbc5454a9bbf_66_0.png)

It’s hard to imagine a Scene with more than 30,000 objects in it, but keep in mind that `MonoBehaviour`s contain the `Update()` callback, not `GameObject`s. A single `GameObject` can contain multiple `MonoBehaviour`s at once, and each of their children can contain even more `MonoBehaviour`s, and so on. A few thousand or even a hundred empty `Update()` callbacks will inflict a noticeable impact on frame rate budget, for zero potential gain. This is particularly common with Unity UI Components which tend to attach a lot of different Components in a very deep hierarchy.

The fix for this is simple: delete the empty callback definitions. Unity will have nothing to hook into, and nothing will be called. Finding such empty definitions in an expansive codebase may be difficult, but if we use some basic regular expressions (known as _regex_), we should be able to find what we’re looking for relatively easily.

All common code-editing tools for Unity, such as MonoDevelop, Visual Studio, and even Notepad++, provide a way to perform a regex-based search on the entire codebase. Check out the tool’s documentation for more information, since the method can vary greatly depending on the tool and its version.

The following regex search should find any empty `Update()` definitions in our code:

`void\\s*Update\\s*\\((\\s*?)\\s*?\\n*?\\{(\\n*?\\s*?)*?\\}`

This regex checks for a standard method definition of the `Update()` callback, while including any surplus whitespace and newline characters that can be distributed throughout the method definition.

Naturally, all of the above is also true for the non-boilerplate Unity callbacks, such as `OnGUI()`, `OnEnable()`, `OnDestroy()`, and `LateUpdate()`. The only difference is that only `Start()` and `Update()` are defined automatically in a new script. Check out the `MonoBehaviour` Unity Documentation page for a complete list of these callbacks at `http://docs.unity3d.com/ScriptReference/MonoBehaviour.html`.

It might also seem unlikely that someone generated so many empty versions of these callbacks in our codebase, but never say never. For example, if we use a common base class `MonoBehaviour` throughout all of our custom Components, then a single empty callback definition in that base class will permeate the entire game, which can cost us dearly. Be particularly careful of the `OnGUI()` method, as it can be invoked multiple times within the same frame or UI event.

![](img/da79484c9b30dc915216dbc5454a9bbf_67_0.png)

