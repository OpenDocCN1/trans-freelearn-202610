
## Interpreting commands and initiating actions

Now that the system can both hear and speak, you'll want to provide the robot with the ability to respond to your speech and execute commands based on the speech input. Next, you're going to configure the system to respond to simple commands.

In order to respond, we're going to edit the continuous.c code in the /home/pi/src/programs directory. We could create our own C file but this file is already set up in the make system and is an excellent starting point. You can save a copy of the current file as continuous.c.old so that you can always go back to the starting program if required. Then, you need to edit the continuous.c file. It is very long and a bit complicated. However, you are looking for a specific section in the code, which is shown in the following screenshot. Look for the /* Exit if the first word spoken was GOODBYE */ comment line:

![](img/324d126a7e2da696837a3e066326bd61_160_0.png)

In this section of the code, the word has already been decoded and is held in the hyp variable. You can add the code here in order to make your system do things based on the value associated with the word that we decoded. Firstly, let's try to add the ability to respond to hello and goodbye commands to see whether or not we can get the program to respond to these commands. You need to make changes to the code in the following manner:

- 1. Find the /* Exit if the first word spoken was GOODBYE */ comment
- 2. In the statement if (strcmp(word, "goodbye") == 0), change word to hyp and goodbye
- 3. Insert brackets around the break; statement and add the system("espeak \"good bye\\\""); statement just before the break; statement
- 4. Add the other else if statement to the clause by typing else if (strcmp(hyp, "hello") == 0). Add brackets after the else if statement and, inside the brackets, type system("espeak \"hello\\\"");

The file should now look as follows:

![](img/324d126a7e2da696837a3e066326bd61_161_0.png)

## Adding Voice Recognition and Speech – A Voice Activated Robot

Now you need to rebuild your code. As the make system already knows how to build the pocketsphinx_continuous program, it will rebuild the application if you make a change to the continuous.c file at any point. Simply type sudo make and the file will compile and create a new version of pocketsphinx_continuous. To run your new version, type ./pocketsphinx_continuous. Make sure that you type ./ at the start. Again, I had to type sudo ./pocketsphinx_continuous, allow it to fail, and then type ./pocketsphinx_continuous to make it work.

If everything was set up correctly, saying "hello" should result in a response of "hello" from your Raspberry Pi. Saying "goodbye" should elicit a response of "goodbye" and also shut down the program. Note that the system command can be used to run any program that runs from a command line. Now you can use this program to start and run other programs based on the commands. In this case, you need to change the code shown to call your Python code to issue the commands to the robot, as shown in the following screenshot:

![](img/324d126a7e2da696837a3e066326bd61_162_0.png)

In this case, you hook up only two of the very many commands that your robot can respond to; you can add the rest of the commands to your continuous.c file by using the same technique. Now, you can give your robot voice commands and it will obey them! Using the directions from the earlier section of this chapter, you can also control your robot remotely using single character commands and add a webcam. You have your very own robotic servant!

## Summary

In this chapter, you've learned about the basics of hacking a toy robot using a Raspberry Pi. Feel free to experiment; you can see how easily you can play all sorts of games with your new toys. In the next chapter, you'll learn how to build a remote control vehicle that can go into a room and display what it sees back to a central location.

## 7
### Adding Raspberry Pi Zero to an RC Vehicle

You've now built robots that can roll, walk, and respond to voice commands. In this chapter, you'll modify an RC car to add Raspberry Pi Zero to control the vehicle. Additionally, you'll add a webcam so that you can see what your car is seeing remotely.

In this chapter, you'll learn the following topics:

- How to modify an Xmods RC car using Raspberry Pi Zero
- How to set break into the control circuitry of the car and use the Raspberry Pi to control it
- How to use wireless communication to add remote control to the car
- How to add a webcam so you can control your car via **First Person View (FPV)**

## Configuring and controlling an RC car with Raspberry Pi Zero

In this project, you’ll be working on a simple RC car, similar to the one shown here:

![](img/324d126a7e2da696837a3e066326bd61_166_0.png)

This particular car is an Xmods car, originally sold by Radio Shack. However, the best place to get them now is eBay. The advantage of this particular set is that the inputs to the drive train and steering are very easy to access.

The following image shows the car with its center control mechanism exposed:

![](img/324d126a7e2da696837a3e066326bd61_167_0.png)

There are two connections that you will want direct access to. The first is the drive motor, and the second is the steering mechanism. For this particular model of RC car, the drive mechanism is in the rear. What you are normally looking for is two wires that will directly drive the DC motor of the car. In this system, there is a connector in the rear of the car; it looks similar to the following image:

![](img/324d126a7e2da696837a3e066326bd61_167_1.png)

In the main control section of the car, you can see that there is a connector that plugs into these two wires, in order to control the speed of the car, as shown here:

![](img/324d126a7e2da696837a3e066326bd61_168_0.png)

Remove this plug and these wires; you'll use Raspberry Pi Zero and a motor controller to provide the voltage to the drive system of the car. The motor will run faster or slower based on the voltage that is applied to these wires and the polarity of the voltage will determine the direction. Raspberry Pi Zero will need to provide a positive or negative 6-volt signal to control the speed and direction of the car.

You'll also need to replace the control signals that go to the front of the car for the steering. This is a bit more difficult. The following is the connector that goes to the front of the car:

![](img/324d126a7e2da696837a3e066326bd61_169_0.png)

The five-pin connector that comes from the control module is shown in the following image:

![](img/324d126a7e2da696837a3e066326bd61_169_1.png)

![](img/324d126a7e2da696837a3e066326bd61_171_0.png)

You'll also need access to the rear wheel compartment of your car to drive the two rear wheels. The following is how the access will look:

![](img/324d126a7e2da696837a3e066326bd61_171_1.png)

## Adding Raspberry Pi Zero to an RC Vehicle

You'll also need to connect the battery power to Raspberry Pi Zero. Here is the modified connection to get the battery power from the car:

![](img/324d126a7e2da696837a3e066326bd61_172_0.png)

To control the car, you'll need to provide each of the control signals. The + or - 6-volt signals cannot be sourced directly by Raspberry Pi Zero. You'll need some sort of a motor controller to source the signal to control the rear wheel drive of the car and the turning mechanism of the car. The simplest way to provide these signals is to use a motor shield, an additional piece of hardware that is installed on top of Raspberry Pi Zero and can source the voltage and current to power both of these mechanisms. The RaspiRobot Board V2 you used in Chapter 4, Building and Controlling a Simple Wheeled Robot, can provide these signals, as shown in the following image:

![](img/324d126a7e2da696837a3e066326bd61_172_1.png)
