```
{
    StartCoroutine("Heal");
}
}
```

5. The `IEnumerator Heal()` coroutine is responsible for increasing our Spider's `hitPoints` in time:

```
isHealing = true;
Enemy enemyScript = GetComponent<Enemy>();

while (enemyScript.hitPoints < (float)enemyScript.initialHitPoints)
{
    enemyScript.hitPoints += healingSpeed * Time.deltaTime;
    yield return null;
}

enemyScript.hitPoints = enemyScript.initialHitPoints;
isHealing = false;
```

6. Assign the script to the Spider character.

7. Play the game and press the H key to start the healing action. Our character needs to be in the SpiderIdle state (the public string healinAllowedState is set to Base Layer.SpiderIdle in the Inspector).

# How it works...

To check the Animator state our character is currently in, we first use the GetCurrentAnimatorStateInfo(int layerIndex) function to get the state the character's Animator is in. We need to call this function on the Animator component; thus, we need to have a variable storing a reference to it (anim in our example). Then we call the IsName(string name) function on the state we got from the GetCurrentAnimatorStateInfo(int layerIndex) function. The IsName(string name) function compares the Animator state name to the name we put as the parameter.

![](bbox=[0.12, 0.78, 0.2, 0.86])

We need to specify the full name of the state. This name needs to be in this format: <Layer Name>.<State Name>. For instance, our SpiderIdle state is in the Base Layer, so we need to put Base Layer.SpiderIdle as the name parameter in the IsName() function.