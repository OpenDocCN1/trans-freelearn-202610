# BUILDING THE PROJECT

This is a pretty straightforward project to build. There are just three components on the breadboard. As well as the electronics, you will also need to find a way to attach the pump to your water container and, if it is in the open, keep the rain out of the pump.

# >STEP-01
Solder leads
onto the pump

Bell wire or speaker cable is just fine. To make it easier to push the loose ends into the breadboard, tin them with solder so that they don’t bunch up when you try to insert them into the breadboard.

![](bbox=[0.68, 0.21, 0.93, 0.35])

# >STEP-02
Build the breadboard

When inserting the components into the breadboard, make sure that you get the transistor and diode the correct way around. The transistor has one curved side that should be facing to the left, and the diode has a stripe on one end that needs to be towards the top of the breadboard. The resistor can be connected either way around. To prevent accidental shorts between the resistor and diode leads, it is a good idea to shorten one or both of the leads.

![](bbox=[0.68, 0.36, 0.93, 0.5])

# >STEP-03
Connect the breadboard

Attach male-to-male jumper wires to the screw terminals of the barrel jack adaptor. Use red and black leads, and make sure that you attach the red lead to the screw terminal marked '+'. Fit the other end of the jumper wires into the breadboard.

The wires to the pump motor should also now be fitted to the breadboard, and the female-to-male jumper wires used to connect the breadboard to GND and 18 on the Raspberry Pi GPIO header.

![](bbox=[0.43, 0.68, 0.93, 0.85])

![](bbox=[0.05, 0.1, 0.42, 0.2])

Above Controlling the waterer from a browser

connected to the breadboard using a DC barrel socket to screw adaptor and two male-to-male jumper wires.

The tubing and fittings that we used came from a low-cost supermarket as a ‘Micro Irrigation System’. This packet contained a few metres of tubing as well as small posts for anchoring the pipes, and small joining sections that were perfect for connecting the pump to the tubing.

The breadboard, jumper wires, and resistor are probably best bought as an electronics starter kit. The Monk Makes Electronic Starter Kit for Raspberry Pi includes these parts. Most starter kits for the Raspberry Pi will include the breadboard, jumper wires, and some resistors.

![](bbox=[0.05, 0.42, 0.42, 0.64])

Above Peristaltic pump

# Peristaltic pumps

Peristaltic pumps use a motor to drive a gearbox that in turn drives a pair of rollers that squeeze the flexible but strong pipe to push the water through the pump.

These pumps have the advantage that they are ‘self-priming’. That is, you do not need to get the air out of the intake tube before they start pumping; they will eventually pull the water through. The other advantage they have is that when the motor stops, it effectively seals the pipe, so no water will flow through it even if the water is under a bit of pressure.

# Building your waterer

As with all projects, it is a good idea to test the project out and get everything working while the parts are all still in your workspace. Once you know all is well, you can install the project in its final location.