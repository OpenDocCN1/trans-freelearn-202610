

This is not ideal for Prefabs we need immediately at the start of the game, but all future Prefabs are good candidates for asynchronous loading if we're willing to create systems that manage this behaviour.

## Keep previously loaded serialized objects in memory

As previously mentioned, once a serialized object has been loaded into memory, then it remains there and can be copied if we need it again later, such as instantiating more copies of a Prefab. We can free this data later with explicit calls to `Resources.Unload()`, which will release the memory space to be reused later. But, if we have a lot of surplus memory in the application's budget, then we could choose to keep this data in memory, which would reduce the need to reload it again from disk later. This naturally consumes a lot of memory with more and more serialized data, making it a risky strategy for memory management, and so we should only do this when necessary.

## Move common data into ScriptableObjects

If we have a lot of different Prefabs with Components that contain a lot of properties that tend to share data, such as game design values like hit points, strength, speed, and so on, then all of this data will be serialized into every Prefab that uses them. A better approach is to serialize this common data in a `ScriptableObject` which they load and use instead. This reduces the amount of data stored within the Serialized File for the Prefab and could significantly reduce the loading time of our scenes by avoiding too much repetitive work.

## Load scenes additively and asynchronously

Scenes can be loaded either to replace the current Scene or can be loaded additively to add its contents to the current Scene without unloading the preceding one. This can be toggled via the `LoadSceneMode` argument of the `SceneManager.LoadScene()` family of functions.

Another mode of Scene loading is to complete it either synchronously or asynchronously, and there are good reasons to use both. Synchronous loading is the typical means of loading a Scene by calling `SceneManager.LoadScene()` where the main thread will block until the given Scene completes loading. This normally results in a poor user experience, as the game appears to freeze as the contents are loaded in (whether as a replacement or additively). This is best used if we want to get the player into the action as soon as possible, or we have no time to wait for Scene objects to appear. This would normally be used if we're loading into the first level of the game or returning to the main menu.

For future Scene loading, however, we may wish to reduce the performance impact so that we can continue to keep the player in action. Loading a Scene can take a lot of work, and the larger the Scene, the longer it will take. However, the option of asynchronous additive loading offers a huge benefit; we can let the Scene gradually load in the background without causing significant impact on the user experience. This can be accomplished with `SceneManager.LoadSceneAsync()` combined with passing in `LoadSceneMode.Additive` for the loading mode argument.

It's important to realize that scenes do not strictly follow the concept of a game level. In most games, players are normally trapped in one level at a time, but Unity can support multiple scenes being loaded simultaneously through additive loading, allowing each Scene to represent a small chunk of a level. Ergo, we could initialize the first Scene for the level (`Scene-1-1a`), and as the player nears the next section, asynchronously and additively load in the next (`Scene-1-1b`), and repeat this continuously as the player travels through the level.

![](img/da79484c9b30dc915216dbc5454a9bbf_115_0.png)

Unity Technologies restructured the Scene system in a mid-release of Unity 5 into a global `SceneManager` class, although this was more of a naming convention change to make it clear that levels and scenes are not the same thing. The exact same features are available in older releases of Unity, just with a slightly different API through the `Application` class.

Exploiting this feature would require a system that either constantly checks the player's position in the level until they get close or uses Trigger Volumes to broadcast a message that the player is nearing the next section and begin asynchronous loading at the appropriate time. Another important consideration is that the Scene's contents won't immediately appear since asynchronous loading effectively spreads the loading out over a handful of frames in order to cause as little visible impact as possible. We need to make sure that we trigger asynchronous Scene loading with more than enough time to spare so that the player won't see objects popping into the game.

Scenes can also be unloaded to clear them out of memory. This will save some memory or runtime performance in the form of removing any Components making use of `Update()` that we no longer need. Again, this can be done both synchronously and asynchronously with `SceneManager.UnloadScene()` and `SceneManager.UnloadSceneAsync()`. This can be an enormous performance benefit because we're only using what we need due to the player's location in the level, but note that it is not possible to unload small chunks of a monolithic Scene. If the original Scene file was enormous, then unloading it would unload everything. The original Scene would have to be broken up into smaller scenes and then loaded and unloaded as needed. Similarly, we should only begin to unload a Scene if we're certain the player can no longer see its constituent objects, otherwise they would witness objects disappearing out of nowhere. One last consideration is that Scene unloading would trigger the destruction of many objects, which is likely to free up a lot of memory and trigger the garbage collector. An efficient use of memory is also important when making use of this tip.

This approach would require a significant amount of Scene redesign work, script writing, testing, and debugging, which is not to be underestimated, but the benefits of improving user experience are exceptional. Having seamless transitions between areas in a game is a benefit that is often praised by players and critics, because it doesn't take the player out of the action. If we use it appropriately, it can save a significant amount of runtime performance, improving the user experience further.

## Create a custom Update() layer

Earlier in this chapter, in the _Update, Coroutines and InvokeRepeating_ section, we discussed the relative pros and cons of using these Unity Engine features as a means of avoiding excessive CPU workload during most of our frames. Regardless of which of these approaches we might adopt, there is an additional risk of having lots of `MonoBehaviours` written to periodically call some function, which is having too many methods triggering in the same frame simultaneously.

Imagine thousands of `MonoBehaviours` that initialized together at the start of a Scene, each starting a Coroutine at the same time that will process their AI tasks every 500 milliseconds. It is highly likely that they would all trigger within the same frame, causing a huge spike in its CPU usage for a moment, which settles down temporarily and then spikes again a few moments later when the next round of AI processing is due. Ideally, we would want to spread these invocations out over time.

The following are the possible solutions to this problem:

- Generating a random time to wait each time the timer expires or Coroutine triggers
- Spread out Coroutine initialization so that only a handful of them are started at each frame
- Pass the responsibility of calling updates to some God Class that places a limit on the number of invocations that occur each frame

The first two options are appealing since they’re relatively simple and we know that Coroutines can potentially save us a lot of unnecessary overhead. However, as we discussed, there are many dangers and unexpected side effects associated with such drastic design changes.

A potentially better approach to optimize updates is to not use `Update()` at all, or more accurately, to use it only once. When Unity calls `Update()`, and in fact, any of its callbacks, it crosses the aforementioned Native-Managed Bridge, which can be a costly task. In other words, the processing cost of executing 1,000 separate `Update()` callbacks will be more expensive than executing one `Update()` callback, which calls into 1,000 regular functions. As we witnessed in the _Remove empty callback definitions_ section, calling `Update()` thousands of times is not a trivial amount of work for the CPU to undertake, primarily because of the Bridge. We can, therefore, minimize how often Unity needs to cross the Bridge by having a God Class `MonoBehaviour` use its own `Update()` callback to call our own custom update-style system used by our custom Components.

In fact, many Unity developers prefer implementing this design right from the start of their projects, as it gives them finer control over when and how updates propagate throughout the system; this can be used for things such as menu pausing, cool time manipulation effects, or prioritizing important tasks and/or suspending low priority tasks if we detect that we’re about to reach our CPU budget for the current frame.

All objects wanting to integrate with such a system must have a common entry point. We can achieve this through an Interface Class with the `interface` keyword. Interface Classes essentially set up a contract whereby any class that implements the Interface Class Class must provide a specific series of methods. In other words, if we know the object implements an Interface Class, then we can be certain about what methods are available. In C#, classes can only derive from a single base class, but they can implement any number of Interface Classes (this avoids the _deadly diamond of death_ problem that C++ programmers will be familiar with).

