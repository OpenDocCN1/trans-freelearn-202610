Note that the `private` access specifiers shown in the preceding code are redundant keywords in C#, since fields and methods default to `private` unless specified otherwise. However, it is often best practice to be explicit about the intended access level.

Looking at this Component in the **Inspector** window reveals three values, initially given default values of `0`, or `null`, which can be modified through the Editor interface:

![](bbox=[0.2461, 0.2578, 0.752, 0.3609])

We can drag-and-drop a Prefab reference from the **Project** window into the **Enemy Prefab** field revealed in the **Inspector** window.

Note how Unity automatically takes a camel-cased field name and creates a convenient **Inspector** window name for it. `_numEnemies` becomes **Num Enemies**, `_enemyPrefab` becomes **Enemy Prefab** and so on.

Meanwhile, the `_enemyManager` field is interesting because it is a reference to a specific `MonoBehaviour` class type. If a `GameObject` is dragged-and-dropped into this field, then it will refer to the Component on the given object as opposed to the `GameObject` itself. Note that if the `GameObject` does not contain the expected `MonoBehaviour`, then nothing will be assigned to the field.

A common usage of this Component reference technique is to obtain references to other Components attached to the very same `GameObject` a Component is attached to. This is an alternative means of caching Components with zero cost, as discussed in the section entitled _Cache Component references_ earlier in this chapter.

There is some danger to using this method. Much of our code would assume that a Prefab is assigned to a field that is used like a Prefab and a `GameObject` is assigned to a field that refers to an instance of a `GameObject`. However, since Prefabs are essentially `GameObjects`, any Prefab or `GameObject` can be assigned to a serialized `GameObject` reference field, which means we could assign the wrong type by accident.