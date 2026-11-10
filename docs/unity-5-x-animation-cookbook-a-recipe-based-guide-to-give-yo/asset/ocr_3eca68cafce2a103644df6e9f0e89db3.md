```
(GameObject) Resources.Load(effectResourceName);
if (effectResource != null)
{
    GameObject.Instantiate(effectResource,
    transform.position, Quaternion.identity);
}
}
```

25. Assign the script to our character and play the game to see the effect.

# How it works...

We are using one important feature of Animation Events in this recipe: the possibility to pass a string, int, or float parameter to our script's functions. This way we can create one function for playing all the sound effects associated with our character and pass the clips' names as string parameters from the Animation Events. The same concept is used for spawning the Dust effect.

The Resources folder is needed to get any resources (prefab, texture, audio clip, and so on) with the Resources.Load(string path) function. This method is convenient for loading assets using their names.

# There's more...

Our Dust effect has the AutoDestroy.cs script attached to make it disappear after a certain time. You can find that script in the Shared Scripts folder in the provided example Unity project.

# Creating camera shakes with the Animation View and the Animator Controller

In this recipe, we will use a simple, but very effective method for creating camera shakes. Those effects are often used to emphasize impacts or explosions in our games.