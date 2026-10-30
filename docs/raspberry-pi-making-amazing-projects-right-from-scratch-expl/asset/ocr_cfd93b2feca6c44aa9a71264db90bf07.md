First, we start by importing the required libraries, as follows:

```
import turtle
import random
```

We need the random library for the randint() function, which returns a random integer in the provided range. This is needed to make our generated tree seem different every time. Then, we will define a function to draw a part of the tree recursively:

```
def fractal_tree(b_len,t):
    if b_len > 5:
        temp=random.randint(1, b_len)
        temp_angle = random.randint(1, 25)
        t.forward(temp)
        t.right(temp_angle)
        fractal_tree(b_len-10,t)
        t.left(2 * temp_angle)
        fractal_tree(b_len-10,t)
        t.right(temp_angle)
        t.backward(temp)
```

In the preceding program, we are calling the same function twice in order to draw the further branches of the tree. If the b_len parameter is less than or equal to 5, then it will be a leaf (which means that the function will not be called again); else, the recursion will continue. We are randomizing the angle and length of the movement of the turtle while drawing the branches here; otherwise, the tree will be symmetrical, which is very unlikely in real life. The combination of t.forward(), t.backward(), t.left(), and t.right() ensures that at the end of each function call, the turtle cursor is at the same position as where it started.

Finally, we write the routine to call this recursive function:

```
t=turtle.Turtle()
disp=turtle.Screen()
t.left(90)
t.up()
t.backward(100)
t.down()
t.color("green")
fractal_tree(120,t)
disp.exitonclick()
```