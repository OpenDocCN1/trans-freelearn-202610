# # Detecting the current Android version

We can determine the current version of Android using the static variables of the Build.Version class, SDK_INT, and we can determine whether it is newer than a specific version by comparing it to that version's appropriate variable, Build.VERSION_CODES, as shown in the following code snippet. If this explanation was a bit mouthful, just look at how we determine whether the current version is equal to or newer (greater) than Lollipop:

```
```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
    // Lollipop or newer code goes here
} else {
    // Code for devices older than lollipop here
}
```
```

Now, let's see how to make some noise with Android devices that are newer and older than Lollipop.