In this case, we're calling some custom `ProcessAI()` subroutine every single frame. This may be a complex task, requiring the AI system to check some grid system to figure out where it's meant to move or determine some fleet maneuvers for a group of spaceships or whatever our game needs for its AI.

If this activity is eating into our frame rate budget too much, and the task can be completed less frequently than every frame with no significant drawbacks, then a good trick to improve performance is to simply reduce the frequency that `ProcessAI()` gets called:

```
private float _aiProcessDelay = 0.2f;
private float _timer = 0.0f;
void Update() {
	_timer += Time.deltaTime;
	if (_timer > _aiProcessDelay) {
		ProcessAI();
		_timer -= _aiProcessDelay;
	}
}
```

In this case, we've reduced the `Update()` callback's overall cost by only invoking `ProcessAI()` about five times every second, which is an improvement over the previous situation, at the expense of code that can take a bit of time to understand at first glance, and a little extra memory to store some floating-point data. Although, at the end of the day we're still having Unity call an empty callback function more often than not.

This function is a perfect example of a function we could convert into a Coroutine to make use of their delayed invocation properties. As mentioned previously, Coroutines are typically used to script a short sequence of events, either as a one-time, or repeated, action. They should not be confused with threads, which would run on a completely different CPU core in a concurrent manner, and multiple threads can be running simultaneously. Instead, Coroutines run on the main thread in a sequential manner such that only one Coroutine is handled at any given moment, and each Coroutine decides when to pause and resume via `yield` statements. The following code is an example of how we might rewrite the above `Update()` callback in the form of a Coroutine:

```
void Start() {
	StartCoroutine(ProcessAICoroutine());
}
```