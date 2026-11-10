
- **Transition Offset**: This offsets the target animation in time. The value is in percentage of the animation. Setting it to 0.3 means that the target animation will start from 30 percent of its length instead of the beginning.
- **Interruption Source**: This setting tells Unity whether the transition can be interrupted by other transitions. You can set it to:
  - **None**: the transition cannot be interrupted by any other transition.
  - **Current State**: The transition can be interrupted by transitions from the state we are trying to transition from.
  - **Next State**: The transition can be interrupted by transition from the state we are trying to transition to.
  - **Current State then Next State**: The transition can be interrupted by the transitions of the state we are trying to transition from or by the transitions of the state we are trying to transition to. The transitions from the state we are trying to transition from take the priority.
  - **Next State then Current State**: The transition can be interrupted by the transitions of the state we are trying to transition from or by the transitions of the state we are trying to transition to. The transitions from the state we are trying to transition to take the priority.
  - **Ordered Interruption**: If set to false, this lets the transition be interrupted by other transitions independently of their order. If set to true, the order of the transitions matters.

## There's more...

When you create an Animator Controller, you can see three more nodes apart from your animation states:

- **Entry and Exit**: This node is used when you transition between state machines (Animator Controllers or substate machines). We will discuss it in detail in the Using Sub-State Machines in Animator Controller recipe in Chapter 6, Handling Combat.
- **Any state**: This node can be used as a helper to make a transition from any state to a given state. It is used when you have an animation that can be played anytime, for instance, a hit animation. We will discuss it in detail in the Using transitions from any state to play hit reactions recipe in Chapter 6, Handling Combat.

## Using parameters to control the animation flow

You can define a set of parameters in an Animator Controller and use them to control the transitions between animation states in Mecanim. In this recipe, we will show how to use parameters for transition conditions and use scripts to set values of those parameters in runtime.

## Getting ready

Before we start, you should prepare an Animator Controller with at least one transition between animation states. The controller should be assigned to a character (its Animator component) placed in a scene. You can also use the provided example Unity project and go to the Chapter 01 Working with animations\Recipe 05 Using parameters to control the animation flow directory. You will find an Example.unity scene there. There is a Warrior game object in the scene's Hierarchy. If you run the game and press the space bar, the Warrior will make a wave gesture. You can select the Warrior and open his Animator Controller. If you click on the Idle | Wave transition, you will be able to see the transition condition.

## How to do it...

To use parameters for controlling state transitions, follow these steps:

- 1. Open the Animator Controller asset.
- 2. Find the Parameters tab in the upper left corner of the Animator window and click on it.
- 3. Click on the plus icon in the Parameters tab to add a new parameter.
- 4. Choose the Trigger type for the parameter.
- 5. Type a name of the newly created parameter (in the provided example, the name of the parameter is Wave).
- 6. Click on the transition between states you want to use parameters for. In the provided example, it is the transition between Idle and Wave animation states. Idle is the default state.
- 7. Go to the Inspector tab and find the Conditions section.
- 8. Click on the plus icon to add a new condition. If you have only one parameter, it will be chosen as the condition automatically. If you have more parameters, you need to choose the proper one from a drop-down list.
- 9. If you want to make an immediate transition between your animation states, make sure to disable the Has Exit Time option, found above the Settings foldout.
- 10. Your transition will take place only when its conditions are met. You need to set the parameter using scripts.
- 11. To create a new C# script, click on the right mouse button in the Project View and select Create | C# Script. Name the script as you wish (in the provided example, it's called Wave, the same as the parameter it sets).
- 12. Open the script and write the following:

![](img/616a75b593bef4212f618360fceea442_45_0.png)

```csharp
using UnityEngine;
using System.Collections;

public class Wave : MonoBehaviour {
//The anim variable is used to store the reference
//to the Animator component of the character.
private Animator anim;
void Start () {
//We get the component and assign it to
//the anim variable when the game starts
anim = GetComponent<Animator>();
}
void Update () {
//We check if player pressed the spacebar
if (Input.GetKeyDown(KeyCode.Space))
{
/*We call the SetTrigger() function on the
Animator component stored in the anim
variable. The function requires one
parameter - the name of the trigger
parameter set in our Animator Controller
("Wave" in our example). Make sure to match
it with the name of the parameter you've
created in your Animator Controller*/
anim.SetTrigger("Wave");
}
}
}
```

- 13. Make sure your class name is the same as the file name of the script, as it won't compile otherwise.
- 14. Assign the script to the character, to the same Transform that has the Animator component with your Animator Controller attached. Play the game and press the space bar; you should see your character switch to the next animation state.

## How it works...

You can use several types of parameters and corresponding script functions to set them:

- **Trigger**: This is the simplest parameter. It is set to true with the SetTrigger(string name) function called on the Animator component object. It is reset by the Animator Controller after it is consumed (used) by a transition. The string name parameter of the function has to match your trigger parameter name set in the Animator Controller.
- **Int**: This is an integer parameter. When you use it, you have to specify a logical comparison in the condition. The transition will only occur if the value of the parameter meets the comparison condition with a given number. You can use the Equal, Greater, Less, and Not Equal options to compare the value of your parameter with the given number. Integer type parameters are set with the SetInteger(string name, int value) function. The string name parameter needs to match the parameter name set in the controller. The int value parameter is the value to set the controller parameter to.
- **Float**: This is a float parameter. It works the same as the integer type, but uses floating point numbers instead of integers. It is set using the SetFloat(string name, float value) function.
- **Bool**: This is a Boolean parameter. The condition can check if the parameter is true or false. The value of the parameter is set with the `SetBool(string name, bool value)` function.

## There's more...

- You can add more than one condition to a state transition by clicking on the plus icon in the **Conditions** section in the transition **Inspector**. For the transition to occur, all its conditions have to be met. It works as logical *AND* for the conditions.
- You can also create more than one transition between the same states. To do this, right-click on the state you want to transition from and choose the **Make Transition** option, and then select the state you already have a transition to. A multi-transition is marked with three arrows instead of one. If the conditions of any of the transitions are met, the transition will occur. You can use it as logical *OR* for transition conditions.
- If you have more than one transition between states, you can only edit one of them at a time. To edit a transition, select it in the **Transitions** section of the **Inspector**.

![](img/616a75b593bef4212f618360fceea442_47_0.png)

- If you want to remove a transition, select it in the **Inspector** and click on the minus icon, or select it in the **Animator Controller** and press *Delete* on the keyboard. Pressing *Delete* removes all the transitions.
