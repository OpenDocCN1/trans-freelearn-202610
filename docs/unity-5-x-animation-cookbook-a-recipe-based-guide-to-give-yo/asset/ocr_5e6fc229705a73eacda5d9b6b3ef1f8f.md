```
if (Input.GetKeyDown(KeyCode.Space))
{
/*We cal the SetTrigger() function on the
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
```

13. Make sure your class name is the same as the file name of the script, as it won't compile otherwise.
14. Assign the script to the character, to the same Transform that has the Animator component with your Animator Controller attached. Play the game and press the space bar; you should see your character switch to the next animation state.

# How it works...

You can use several types of parameters and corresponding script functions to set them:

+   - **Trigger**: This is the simplest parameter. It is set to true with the SetTrigger(string name) function called on the Animator component object. It is reset by the Animator Controller after it is consumed (used) by a transition. The string name parameter of the function has to match your trigger parameter name set in the Animator Controller.
+   - **Int**: This is an integer parameter. When you use it, you have to specify a logical comparison in the condition. The transition will only occur if the value of the parameter meets the comparison condition with a given number. You can use the Equal, Greater, Less, and Not Equal options to compare the value of your parameter with the given number. Integer type parameters are set with the SetInteger(string name, int value) function. The string name parameter needs to match the parameter name set in the controller. The int value parameter is the value to set the controller parameter to.
+   - **Float**: This is a float parameter. It works the same as the integer type, but uses floating point numbers instead of integers. It is set using the SetFloat(string name, float value) function.