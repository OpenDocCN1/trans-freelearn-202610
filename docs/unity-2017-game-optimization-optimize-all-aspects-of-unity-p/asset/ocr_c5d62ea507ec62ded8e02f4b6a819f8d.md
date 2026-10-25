First, we verify that the object is being managed by the pool and then we grab the
corresponding PoolablePrefabData in order to access the list of IPoolableComponent
references. Once Despawned() is invoked on all of them, we deactivate the object, remove
it from the active list, and push it into the inactive queue so that it can be respawned later.

# # Prefab pool testing

The following class definition allows us to perform a simple hands-on test with the
PrefabPoolingSystem class; it will support three Prefabs, and prespawn five instances of
each during application initialization. We can press the 1, 2, 3, or 4 keys to spawn an
instance of each type, and then press Q, W, E and R to despawn a random instance of each
type, respectively:

```
```
public class PrefabPoolingTestInput : MonoBehaviour {
    [SerializeField] GameObject _orcPrefab;
    [SerializeField] GameObject _trollPrefab;
    [SerializeField] GameObject _ogrePrefab;
    [SerializeField] GameObject _dragonPrefab;

    List<GameObject> _orcs = new List<GameObject>();
    List<GameObject> _trolls = new List<GameObject>();
    List<GameObject> _ogres = new List<GameObject>();
    List<GameObject> _dragons = new List<GameObject>();

    void Start() {
        PrefabPoolingSystem.Prespawn(_orcPrefab, 11);
        PrefabPoolingSystem.Prespawn(_trollPrefab, 8);
        PrefabPoolingSystem.Prespawn(_ogrePrefab, 5);
        PrefabPoolingSystem.Prespawn(_dragonPrefab, 1);
    }

    void Update() {
        if (Input.GetKeyDown(KeyCode.Alpha1)) {SpawnObject(_orcPrefab, _orcs);}
        if (Input.GetKeyDown(KeyCode.Alpha2)) {SpawnObject(_trollPrefab,
_trolls);}
        if (Input.GetKeyDown(KeyCode.Alpha3)) {SpawnObject(_ogrePrefab,
_ogres);}
        if (Input.GetKeyDown(KeyCode.Alpha4)) {SpawnObject(_dragonPrefab,
_dragons);}
        if (Input.GetKeyDown(KeyCode.Q)) { DespawnRandomObject(_orcs); } }
        if (Input.GetKeyDown(KeyCode.W)) { DespawnRandomObject(_trolls); } }
        if (Input.GetKeyDown(KeyCode.E)) { DespawnRandomObject(_ogres); } }
        if (Input.GetKeyDown(KeyCode.R)) { DespawnRandomObject(_dragons); } }
    }
}
```
```