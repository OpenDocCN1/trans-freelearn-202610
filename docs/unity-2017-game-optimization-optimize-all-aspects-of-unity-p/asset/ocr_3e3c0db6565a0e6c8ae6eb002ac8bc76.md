# # This gives us the following result:

```
```
public void CreateEnemies(int numEnemies) {
	GameObject enemyManagerObj = GameObject.Find("EnemyManager");
	EnemyManagerComponent enemyMgr =
	enemyManagerObj.GetComponent<EnemyManagerComponent>();
	string[] names = { "Tom", "Dick", "Harry" };

	for(int i = 0; i < numEnemies; ++i) {
		GameObject enemy = (GameObject)GameObject.Instantiate(_enemyPrefab,
																			5.0f * Random.insideUnitSphere,
																			Quaternion.identity);
		enemy.name = names[Random.Range(0, names.Length)];
		enemyMgr.AddEnemy(enemy);
	}
}
```
```

If this method is called during the initialization of the Scene, and we’re not overly concerned with loading time, then we can probably consider ourselves finished with our optimization work.

However, we will often need new objects that are instantiated at runtime to find an existing object to communicate with. In this example, we want new enemy objects to register with our `EnemyManagerComponent` so that it can do whatever it needs to do to track and control the enemy objects in our Scene. We would also like the `EnemyManager` to handle all enemy-related behavior, so that objects calling its functions don’t need to perform work on its behalf. This will improve the _Coupling_ (how well our codebase separates related behavioru) and _Encapsulation_ (how well our classes prevent outside changes to the data they manage) of our application. The ultimate aim is to find a reliable and fast way for new objects to find existing objects in the Scene without unnecessary usage of the `Find()` method, so that we can minimize complexity and performance costs.

There are multiple approaches we can take to solving this problem, each with their own benefits and pitfalls:

+   - Assign references to preexisting objects
+   - Static Classes
+   - Singleton Components
+   - A global Messaging System