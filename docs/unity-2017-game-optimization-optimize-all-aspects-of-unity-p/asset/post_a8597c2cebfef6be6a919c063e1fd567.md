
Note that this means we don't need to implement `static` methods in a Singleton Component class definition. For example, we could simply call `EnemyManagerSingletonComponent.Instance.KillAll()` to access the `KillAll()` method.

Note that it is possible to place an instance of a `SingletonComponent` in a **Hierarchy** window since it derives from `MonoBehaviour`. Although, be warned that the `DontDestroyOnLoad()` method would never be called, which would prevent the Singleton Component's `GameObject` from persisting when the next Scene is loaded. We will perhaps need to call `DontDestroyOnLoad()` in the derived class' `Awake()` callback to make this work, unless, of course, we actually want destructible Singletons. Sometimes it makes sense to allow such Singletons to be destroyed between scenes so that it can start fresh each time; it all depends on our particular use cases.

In either case, shutdown of a Singleton Component can be a little convoluted because of how Unity tears down scenes. An object's `OnDestroy()` callback is called whenever it is destroyed during runtime. The same method is called during application shutdown, whereby every Component on each `GameObject` has its `OnDestroy()` callback called by Unity. The same activities take place when we end Play Mode in the Editor, thus returning to Edit Mode. However, destruction of objects occurs in a random order, and we cannot assume that the `SingletonComponent` object will be the last object destroyed.

Consequently, if any object attempts to do anything with the Singleton Component in the middle of their `OnDestroy()` callback, then they may be calling the `SingletonComponent` object's `Instance` property. However, if the Singleton Component has already been destroyed prior to this moment, then a new instance of the `SingletonComponent` will be created in the middle of application shutdown. This can corrupt our Scene files, as instances of our Singleton Components will be left behind in the Scene. If this happens, then Unity will throw the following error message:

"Some objects were not cleaned up when closing the scene. (Did you spawn new GameObjects from OnDestroy?)"

The obvious workaround is to simply never call into a `SingletonComponent` object during any `MonoBehaviour` Component's `OnDestroy()` callback. However, there are some legitimate reasons we may wish to do so: most notable is that Singletons are often designed to make use of the Observer design pattern. This design pattern allows other objects to register/deregister with them for certain tasks, similar to how Unity latches onto callback methods, such as `Start()` and `Update()`, but in a more strict fashion.

With the Observer design pattern, objects will typically register with the system when they are created, will make use of it during runtime, and then either deregister from it during runtime when they are finished using it or deregister during their own shutdown for the sake of cleanup. We will see an example of this design pattern in the upcoming section, A global Messaging System, but if we imagine a MonoBehaviour making use of such a system, then the most convenient place to perform shutdown deregistration would be within an OnDestroy() callback. Consequently, such objects are likely to run into the aforementioned problem, where a new GameObject for the SingletonComponent is accidentally created during application shutdown.

To solve this problem, we will need to make three changes. Firstly, we need to add an additional flag to the SingletonComponent, which keeps track of its active state and disable it at the appropriate times. This includes the Singleton's own destruction, as well as application shutdown (OnApplicationQuit() is another useful Unity callback for MonoBehaviours, which is called during this time):

```
private bool _alive = true;
void OnDestroy() { _alive = false; }
void OnApplicationQuit() { _alive = false; }
```

Secondly, we should implement a way for external objects to verify the Singleton's current state:

```
public static bool IsAlive {
 get {
  if (__Instance == null)
   return false;
  return __Instance._alive;
 }
}
```

Finally, any object that attempts to call into the Singleton during its own OnDestroy() method must first verify the state using the IsAlive property before calling instance, as follows:

```
public class SomeComponent : MonoBehaviour {
 void OnDestroy() {
  if (MySingletonComponent.IsAlive) {
   MySingletonComponent.Instance.SomeMethod();
  }
 }
}
```

This will ensure that nobody attempts to access the Singleton instance during destruction. If we don’t follow this rule, then we will run into problems where instances of our Singleton object will be left behind in the Scene after returning to Edit Mode.

The irony of the `SingletonComponent` approach is that we are using a `Find()` call to determine whether or not one of these `SingletonComponent` objects already exists in the Scene before we attempt to assign the `__Instance` reference variable. Fortunately, this will only happen when the Singleton Component is first accessed, which is usually not a problem if there aren’t too many `GameObjects` in the Scene, but it’s possible that the initialization of the Singleton Component may not necessarily occur during Scene initialization and can, therefore, cost us a performance spike at a bad moment during gameplay when an instance is first acquired and `Find()` gets called. The workaround for this is to have some God Class confirm that the important Singletons are instantiated during Scene initialization by simply accessing the `Instance` property on each one.

Another downside to this approach is that if we later decide that we want more than one of these Singletons executing at once or we wish to separate out its behavior to be more modular, then there would be a lot of code that needs to change.

The final approach we will explore will attempt to solve many of the problems revealed by the previous solutions and provide a way to gain all of their benefits, by combining ease of implementation, ease of extension, and strict usage that also reduces the likelihood of human-error during configuration.

## A global Messaging System

The final suggested approach to solve the problem of interobject communication is to implement a global Messaging System that any object can access and send messages through to any object that may be interested in listening to that specific type of message. Objects can send messages or listen for them (sometimes both!), and the responsibility is on the listener to decide what messages they are interested in. The message sender can broadcast the message without caring at all who is listening, and a message can be sent through the system regardless of the specific contents of the message. This approach is by far the most complex and may require some effort to implement and maintain, but it is an excellent long-term solution to keep our object communication modular, decoupled, and fast as our application gets more and more complex.

The kinds of message we wish to send can take many forms, including data values, references, instructions for listeners, and more, but they should all have a common, basic definition that our Messaging System can use to determine what the message is and who it is intended for.

The following is a simple class definition for a `Message` object:

```
public class Message {
    public string type;
    public Message() { type = this.GetType().Name; }
}
```

The `Message` class' constructor caches the message's `type` in a local `string` property to be used later for cataloging and distribution purposes. Caching this value is important, as each call to `GetType().Name` will result in a new string being allocated, and we've previously learned that we want to minimize this activity as much as possible.

Any custom messages can contain whatever superfluous data they wish so long as they derive from this base class, which will allow it to be sent through our Messaging System. Take note that despite acquiring the `type` from the object during its base class constructor, the `name` property will still contain the name of the derived class, not the base class.

Moving on to our `MessagingSystem` class, we should define its features by what kind of requirements we need it to fulfill:

- It should be globally accessible
- Any object (`MonoBehaviour` or not) should be able to register/deregister as listeners to receive specific message types (that is, the Observer design pattern)
- Registering objects should provide a method to call when the given message is broadcasted from elsewhere
- The system should send the message to all listeners within a reasonable time frame, but not choke on too many requests at once

### A globally accessible object

The first requirement makes the Messaging System an excellent candidate for a Singleton object, since we would only ever need one instance of the system. Although, it is wise to think long and hard if this is truly the case before committing to implementing a Singleton.
