Before we start the implementation, let’s look at the states and transitions of the state machine in Figure 7.4:

# States of mAnimState

![](bbox=[0.16, 0.2, 0.82, 0.6])

Figure 7.4: State machine of the mAnimState variable in the AssimpInstance class

These five states and the transitions between the states can be explained as follows:

+   1. We start at the state machine at playIdleWalkRun. Here, the default, instance-speed-based blending between the movement states, idle, walk, and run, is played.
+   2. Once an action is requested, we check whether source (current state) and destination (action state) are in the msAllowedStateOrder map. If yes, we prepare the directional clips and change to the transitionFromIdleWalkRun state.
+   3. The transitionFromIdleWalkRun state is needed to smooth the transition from idle/walk/run to the requested action. Most animation clips start from a similar pose of body, arms, and legs. By blending the current idle, walk, or run animation to the starting point of the same animation clip, the instance is adjusted back to its starting pose. After reaching the starting pose, we advance to the transitionToAction state.