At its most basic level, performance optimization is just another form of problem-solving, and overlooking the obvious while problem-solving can be an expensive mistake. Our goal is to use benchmarking to observe our application looking for instances of problematic behavior, then use instrumentation to hunt through the code for clues about where the problem originates. Unfortunately, it’s often very easy to get distracted by invalid data or jump to conclusions because we’re being too impatient or missed a subtle detail. Many of us have run into occasions during software debugging where we could have found the root cause of the problem much faster if we had simply challenged and verified our earlier assumptions. Hunting down performance issues is no different.

A checklist of tasks would be helpful to keep us focused on the issue, and not waste time chasing so-called _ghosts_. Of course, every project is different, with its own unique challenges to overcome, but the following checklist is general enough that it should be able to apply to any Unity project:

+   • Verifying that the target script is present in the Scene
+   • Verifying that the script appears in the Scene the correct number of times
+   • Verifying the correct order of events
+   • Minimizing ongoing code changes
+   • Minimizing internal distractions
+   • Minimizing external distractions

# # Verifying script presence

Sometimes, there are things we expect to see, but don’t. These are usually easy to spot because the human brain is very good at pattern recognition and spotting differences we didn’t expect. Meanwhile, there are times where we assume that something has been happening, but it didn’t. These are generally more difficult to notice, because we’re often scanning for the first kind of problem, and we’re assuming that the things we don’t see are working as intended. In the context of Unity, one problem that manifests itself this way is verifying that the scripts we expect to be operating are actually present in the Scene.

Script presence can be quickly verified by typing the following into the **Hierarchy** window textbox:

```
`t:&lt;monobehaviour name&gt;;`
```