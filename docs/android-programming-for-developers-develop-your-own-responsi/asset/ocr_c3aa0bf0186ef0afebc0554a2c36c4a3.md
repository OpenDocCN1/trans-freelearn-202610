# Performance myths – avoid interfaces

On the older versions of Android, before the JIT compiler was introduced,
accessing methods via an interface instead of the exact type was slightly more
efficient. In these versions, it made sense to declare a variable of ArrayList
instead of the generic List interface to access the class directly.

In the modern versions of Android, however, there is no difference between
accessing a variable via an interface and doing it directly. So, for the sake of
generality, we will be using the generic interface instead of the class, as seen
inside the GameEngine:

```
private List<GameObject> mGameObjects = new ArrayList<GameObject>
();
```