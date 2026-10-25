# # Verifying script count

If we’re looking at our Profiler data and note that a certain `MonoBehaviour` method is being executed more times than expected, or is taking longer than expected, we might want to double-check that it only occurs as many times in the Scene as we expect it to. It’s entirely possible that someone created the object more times than expected in the Scene file, or that we accidentally instantiated the object more than the expected number of times from code. If so, the problem could be due to conflicting or duplicated method invocations generating a performance bottleneck. We can verify the count using the same shortlist method used in the *Best approaches to performance analysis* section.

If we expected a specific number of Components to appear in the Scene, but the shortlist revealed more (or less!) than this, then it might be wise to write some initialization code that prevents this from ever happening again. We could also write some custom Editor helpers to display warnings to any level designers who might be making this mistake.

Preventing casual mistakes such as this is essential for good productivity, since experience tells us that if we don’t explicitly disallow something, then someone, somewhere, at some point, for whatever reason, will do it anyway. This is likely to cost us a frustrating afternoon hunting down a problem that eventually turned out to be caused by human-error.

For example, typing `t:MyTestMonoBehaviour` (note that it is not case-sensitive) into the **Hierarchy** textbox will show a shortlist of all `GameObjects` that currently have at least one `MyTestMonoBehaviour` script attached as a Component.

Note that this shortlist feature also includes any `GameObjects` with Components that *derive* from the given script name.

We should also double-check that the `GameObjects` they are attached to are still enabled, since we may have disabled them during earlier testing since someone or something may have accidentally deactivated the object.

![](bbox=[0.133, 0.184, 0.206, 0.248])