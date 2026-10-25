# # Singleton Components

As mentioned previously, Static Classes have difficulty interfacing with Unity-related functionality, and cannot directly make use of `MonoBehaviour` features, such as event callbacks, Coroutines, hierarchical design, and Prefabs. Also, since there's no object to select in the **Inspector** window, we lose the ability to inspect the a Static Class' data at runtime through the **Inspector** window, which can make debugging difficult. These are features that we may wish to make use of in our global classes.

A common solution to this problem is to implement a Component that acts like a Singleton- -it provides `static` methods to grant global access, and only one instance of the `MonoBehaviour` is ever allowed to exist at any given time.

The following is the definition for a `SingletonComponent` class:

```
using UnityEngine;

public class SingletonComponent<T> : MonoBehaviour where T :
SingletonComponent<T> {
private static T __Instance;

protected static SingletonComponent<T> __Instance {
get {
if(!__Instance) {
T [] managers = GameObject.FindObjectsOfType(typeof(T)) as T[];
if (managers != null) {
if (managers.Length == 1) {
__Instance = managers[0];
return __Instance;
} else if (managers.Length > 1) {
Debug.LogError("You have more than one " +
typeof(T).Name +
" in the Scene. You only need " +
"one - it's a singleton!");
for(int i = 0; i < managers.Length; ++i) {
T manager = managers[i];
Destroy(manager.gameObject);
}
}
}
GameObject go = new GameObject(typeof(T).Name, typeof(T));
__Instance = go.GetComponent<T>();
DontDestroyOnLoad(__Instance.gameObject);
}
return __Instance;
}
}
```