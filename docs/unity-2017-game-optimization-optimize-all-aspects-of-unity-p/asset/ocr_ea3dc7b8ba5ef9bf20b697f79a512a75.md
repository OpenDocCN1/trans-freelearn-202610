```
} else {
    // instantiate a new object
    GameObject newGO = GameObject.Instantiate(prefab, position, rotation)
    as GameObject;
    data = new PoolablePrefabData();
    data.go = newGO;
    data.poolableComponents = newGO.GetComponents<IPoolableComponent>();
}

data.go.SetActive (true);
data.go.transform.position = position;
data.go.transform.rotation = rotation;

for(int i = 0; i < data.poolableComponents.Length; ++i) {
    data.poolableComponents[i].Spawned ();
}
_activeList.Add (data.go, data);

return data.go;
}
```

# # Instance prespawning

Since we are using `GameObject.Instantiate()` whenever the `PrefabPool` has run out
of despawned instances, this system does not completely rid us of runtime object
instantiation, hence heap memory allocation. It's important to prespawn the expected
number of instances that we will need during the lifetime of the current Scene so that we
minimize or remove the need to instantiate more during runtime.

Note that we shouldn't prespawn too many objects. It would be wasteful to prespawn 100
explosion Particle Effects, if the most we will ever expect to see in the Scene at any given
time is three or four. Conversely, spawning too few instances will cause excessive runtime
memory allocations, and the goal of this system is to push the majority of allocation to the
start of a Scene's lifetime. We need to be careful about how many instances we maintain in
memory so that we don't waste more memory space than necessary.

Let's define a method in our `PrefabPoolingSystem` class that we can use to quickly
prespawn a given number of objects from a Prefab. This essentially involves spawning N
objects and then immediately despawning them all:

```
public static void Prespawn(GameObject prefab, int numToSpawn) {
    List<GameObject> spawnedObjects = new List<GameObject>();

    for(int i = 0; i < numToSpawn; i++) {
        spawnedObjects.Add (Spawn (prefab));
    }
}
```