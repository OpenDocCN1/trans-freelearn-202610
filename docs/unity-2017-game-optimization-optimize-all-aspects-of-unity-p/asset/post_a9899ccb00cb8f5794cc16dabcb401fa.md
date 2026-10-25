

void SpawnObject(GameObject prefab, List<GameObject> list) {
    GameObject obj = PrefabPoolingSystem.Spawn(prefab,
    5.0f *
    Random.insideUnitSphere,
    Quaternion.identity);
    list.Add(obj);
}

void DespawnRandomObject(List<GameObject> list) {
    if (list.Count == 0) {
    // Nothing to despawn
    return;
    }

    int i = Random.Range(0, list.Count);
    PrefabPoolingSystem.Despawn(list[i]);
    list.RemoveAt(i);
}

Once we spawn more than five instances of any of the Prefabs, it will need to instantiate a new one in memory, costing us some memory allocation. However, if we observe the **Memory Area** in the **Profiler** window, while we only spawn and despawn instances that already exist, then we will notice that absolutely no new allocations take place.

## Prefab Pooling and Scene loading

There is one important caveat to this system that has not yet been mentioned: the `PrefabPoolingSystem` class will outlast Scene lifetime since it is a Static Class. This means that when a new Scene is loaded, the Pooling System's dictionaries will attempt to maintain references to any pooled instances from the previous Scene, but Unity forcibly destroys these objects regardless of the fact that we are still keeping references to them (unless they were set to `DontDestroyOnLoad()`), and so the dictionaries will be full of `null` references. This would cause some serious problems for the next Scene.

We should, therefore, create a method in `PrefabPoolingSystem` that resets the Pooling System in preparation for this likely event. The following method should be called before a new Scene is loaded so that it is ready for any early calls to `Prespawn()` in the next Scene:

```
public static void Reset() {
    _prefabToPoolMap.Clear();
    _goToPoolMap.Clear();
}
```

Note that if we also invoke a garbage collection during Scene transitions, there's no need to explicitly destroy the `PrefabPool` objects these dictionaries were referencing. Since these were the only references to the `PrefabPool` objects, they will be deallocated during the next garbage collection. If we aren't invoking garbage collection between Scenes, then the `PrefabPool` and `PooledPrefabData` objects will remain in memory until that time.

## Prefab Pooling summary

This Pooling System provides a decent solution to the problem of runtime memory allocations for `GameObject`s and Prefabs, but, as a quick reminder, we need to be aware of the following caveats:

- We need to be careful about properly resetting important data in respawned objects (such as `Rigidbody` velocity)
- We must ensure that we don't prespawn too few, or too many, instances of a Prefab
- We should be careful of the order of execution of `Spawned()` and `Despawned()` methods on `IPoolableComponent` and not assume that they will be called in a particular order
- We must call `Reset()` on `PrefabPoolingSystem` when loading a new Scene in order to clear any `null` references to objects, which may no longer exist

There are several other features that we could implement. These will be left as academic exercises if we wish to extend this system in the future:

- Any `IPoolableComponent` added to the `GameObject` after initialization will not have their `Spawned()` or `Despawned()` methods invoked since we only collect this list when the `GameObject` is first instantiated. We could fix this by changing `PrefabPool` to keep acquiring `IPoolableComponent` references every time `Spawned()` and `Despawned()` are invoked at the cost of additional overhead during spawning/despawning.
- Any `IPoolableComponent` attached to children of the Prefab's root will also not be counted. This could be fixed by changing `PrefabPool` to use `GetComponentsInChildren<T>` at the cost of additional overhead if we're using Prefabs with deep hierarchies.
- Prefab instances that already exist in the Scene will not be managed by the Pooling System. We could create a Component that needs to be attached to such objects and that notifies the `PrefabPoolingSystem` class of its existence in its `Awake()` callback, which passes the reference along to the corresponding `PrefabPool`.
- We could implement a way for `IPoolableComponent` to set a priority during acquisition and directly control the order of execution for their `Spawned()` and `Despawned()` methods.
- We could add counters that keep track of how long objects have been sitting in the Inactive list relative to total Scene lifetime and print out the data during shutdown. This could tell us whether or not we're prespawning too many instances of a given Prefab.
- This system will not interact kindly with Prefab instances that set themselves to `DontDestroyOnLoad()`. It might be wise to add a Boolean to every `Spawn()` call to say whether the object should persist or not and keep them in a separate data structure that is not cleared out during `Reset()`.
- We could change `Spawn()` to accept an argument that allows the requestor to pass custom data to the `Spawned()` function of `IPoolableObject` for initialization purposes. This could use a system similar to how custom message objects were derived from the `Message` class for our Messaging System in _Chapter 2, Scripting Strategies_.

## IL2CPP optimizations

Unity Technologies have released a few blog posts on interesting ways to improve the performance of IL2CPP in some circumstances, but they can be difficult to manage. If you're using IL2CPP and need to eke out the last little bit of performance from our application that we can, then check out the blog series at the following links:

- https://blogs.unity3d.com/2016/07/26/il2cpp-optimizations-devirtualization/
- https://blogs.unity3d.com/2016/08/04/il2cpp-optimizations-faster-virtual-method-calls/
- https://blogs.unity3d.com/2016/08/11/il2cpp-optimizations-avoid-boxing/

## WebGL optimizations

Unity Technologies have also released a number of blog posts covering WebGL applications, which includes some crucial information about memory management that all WebGL developers should know. These can be found at the following links:

- https://blogs.unity3d.com/2016/09/20/understanding-memory-in-unity-webgl/
- https://blogs.unity3d.com/2016/12/05/unity-webgl-memory-the-unity-heap/

## The future of Unity, Mono, and IL2CPP

When the first edition of this book was written, IL2CPP was being teased in a variety of blog posts, and Unity was still running a very old version of Mono. Well, the good news is that IL2CPP has finally landed, but the bad news is Unity is still running a very old version of Mono. This is an unfortunate consequence of how various elements of the Mono Framework are licensed, meaning that Unity Technologies has only been able to update Mono on an infrequent basis.

After many years, Unity is still only able to make use of the .NET 3.5 library functionality, which is about a decade old at this point. This limits what kinds of .NET library classes we can use, restricts certain C# language features, and limits performance since many enhancements would have been made to the library during this time. However, to the great relief of many Unity developers, an experimental upgrade to a more recent version of Mono has been in beta for a while and is now available in Unity 2017 by switching the Edit | Project Settings | Player | Configuration | Scripting Runtime Version option to Experimental (.NET 4.6 Equivalent). This setting upgrades the Mono runtime and allows us to make use of the .NET 4.6 functionality, which is only a couple of years old at the time of writing.

This feature is still experimental, however. This could lead to obscure bugs and crashes until it's been deemed stable. At which point, the Mono upgrade will become permanent so that all users can enjoy it. For the time-being, we should only use this option if we need it for a vital bug fix or feature that we cannot work around. Also, there is never a guarantee that the version fixing your bug isn't going to introduce an even worse bug. However, at the very least, hope is on the horizon that Unity is serious about making the upgrade happen sooner rather than later, as there are regular bug fixes around this area each update.

