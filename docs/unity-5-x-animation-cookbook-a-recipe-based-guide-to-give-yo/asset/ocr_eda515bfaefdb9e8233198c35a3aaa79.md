+   - **Bool**: This is a Boolean parameter. The condition can check if the parameter is true or false. The value of the parameter is set with the `SetBool(string name, bool value)` function.

# There's more...

+   - You can add more than one condition to a state transition by clicking on the plus icon in the **Conditions** section in the transition **Inspector**. For the transition to occur, all its conditions have to be met. It works as logical *AND* for the conditions.
+   - You can also create more than one transition between the same states. To do this, right-click on the state you want to transition from and choose the **Make Transition** option, and then select the state you already have a transition to. A multi-transition is marked with three arrows instead of one. If the conditions of any of the transitions are met, the transition will occur. You can use it as logical *OR* for transition conditions.
+   - If you have more than one transition between states, you can only edit one of them at a time. To edit a transition, select it in the **Transitions** section of the **Inspector**.

![](bbox=[0.102, 0.545, 0.896, 0.735])

+   - If you want to remove a transition, select it in the **Inspector** and click on the minus icon, or select it in the **Animator Controller** and press *Delete* on the keyboard. Pressing *Delete* removes all the transitions.