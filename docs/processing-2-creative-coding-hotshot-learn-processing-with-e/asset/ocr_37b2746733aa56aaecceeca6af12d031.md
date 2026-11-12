Knowing how to interface sliders, buttons, and knobs with a computer, enables us to optimize our workspace and workflow exactly the way we need and want it.

# ## Your Hotshot Objectives

Our current mission consists of four tasks. We are starting with a simple "hello world" circuit and extending it until we have a working controller board for our Smile-O-Mat sketch, and then building a housing for it. The task list for our current mission is as follows:

+   - Connecting your Arduino
+   - Building your controller
+   - Changing your face
+   - Putting it in a box

# ## Mission Checklist

To build the controller, we will need an Arduino board, a solderless breadboard, and a handful of electronic components, as follows:

+   - Arduino
+   - Breadboard
+   - Wires
+   - A push button
+   - A resistor (10 kOhm)
+   - Three variable resistors (100 kOhm)

In the final task of this mission, we will create a permanent housing for the controller using a box and some fancy knobs. I will also provide a list of materials that I have used in this section, but similar to the robots in _Project 1, Romeo and Juliet_, the housing is very much a matter of taste.

# ## Connecting your Arduino

Say hello to your Arduino. The first task of our current mission is to write a sketch that runs on Arduino and reads the value of a variable resistor. Since this is a book about Processing, we are going to write a Processing sketch that receives the resistor values from our Arduino board and prints the value.