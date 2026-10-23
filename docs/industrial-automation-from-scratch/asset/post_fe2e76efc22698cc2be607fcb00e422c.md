
Wiring and programming a VFD (VAT20) to use a push button or switch to run a three-phase motor in a forward/reverse direction and to use a potentiometer to set the frequency (speed)

## An overview of a VFD

A VFD is a device used to vary the speed of Asynchronous or Synchronous (AC) electric motors by varying the frequency and voltage supplied. A VFD can also be referred to as an Adjustable Speed Drive (ASD) or inverter. It is the most effective way of controlling the speed of an induction motor. The principle of operation of a VFD is based on the fact that the speed of an induction motor is directly proportional to the frequency of the voltage supplied to the motor. When frequency increases, speed also increases, and when frequency reduces, the speed will also reduce. A synchronous motor runs at synchronous speed (the speed of the rotating magnetic field of the stator) while an asynchronous (induction) motor’s speed is slightly less than synchronous speed. In the previous chapter, it was mentioned that synchronous speed is determined by the frequency of the voltage source and the number of poles, as shown by the following formula:

Synchronous speed = 120f/P

Here, f = frequency in Hz and P = the number of poles.

From the preceding equation, it can be seen that a change in frequency will cause a change in speed. If an application requires a varying speed, a VFD can be used to increase or reduce the speed of the motor, and it does so by varying the frequency and voltage. Some of the companies in the VFD industry include Siemens, Rockwell Automation, Yaskawa Electric Corporation, and General Electric. This book focuses on the General Electric VFD.

The following image shows a VFD manufactured by General Electric (VAT 20-U20N0K4S):

![](img/b10cb271ed113c7ba5bfded0a75d13e0_149_0.png)

Figure 5.1 – A VFD from General Electric (VAT 20-U20N0K4S)

The credit for this photo goes to *Showlight Technologies Limited at* www.showlight.com.ng.

You should now have an understanding of what a VFD does, the relationship between the speed of an induction motor and the frequency, and the various companies in the VFD industry. Your knowledge here will help in understanding other sections in this chapter. Let’s learn how a VFD works in the next section.

## Understanding how a VFD works

The principle of operation of a VFD can be explained using the three basic stages that make it up, as shown in the following block diagram:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_150_0.png)

Figure 5.2 – A block diagram of a VFD

Let’s have a look at the three stages to understand how a VFD works:

- **Converter:** This is the first stage of a VFD. It converts the AC input power to DC power. This stage can also be referred to as the rectifier stage. It can consist of diodes or an array of fast switches.

The following circuit diagram shows the converter/rectifier stage of a VFD that is made up of diodes:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_150_1.png)

Figure 5.3 – The converter/rectifier stage

- **DC link:** Power from the rectifier is fed to the DC link stage. The DC link stage smoothens the DC from the converter/rectifier stage. It consists of a capacitor and may also include an inductor that remove ripples from the rectified signal to provide clean DC power for the next stage.

The following circuit diagram shows the DC link stage of a VFD:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_151_0.png)

Figure 5.4 – The DC link stage of a VFD

- **Inverter:** This is the third stage, which converts the DC power from the DC link into a three-phase AC output. This stage comprises of a power-switching device such as a **Metal Oxide Semiconductor Field Effect Transistor (MOSFET)**, or an **Insulated Gate Bipolar Transistor (IGBT)**, or a thyristor. The output of an inverter is not a pure sine wave but an approximation, based on the principle of Pulse Width Modulation (PWM), which is the major inverter technology.

The following circuit diagram shows the inverter stage of a VFD:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_152_0.png)

Figure 5.5 – The inverter stage of a VFD

Having understood how a VFD works, let’s have a look at the advantages in the next section.

## Advantages of a VFD

A VFD offers so many advantages, which account for its use in many industries. Some of the advantages of VFD include the following:

- A VFD extends motor life and reduces maintenance costs because it starts the motor by ramping it up to the operating speed, rather than starting it abruptly as a direct online starter would do. This starting method reduces mechanical and electrical stress that would have shortened the life of the motor or increased the maintenance costs of both the motor and the driven equipment.

- A VFD reduces energy consumption. Running a motor at full speed consumes a lot of energy, which increases energy costs. Some applications do not require a motor to run at full speed. A VFD can be used in such cases to match the speed of the motor to the process requirement. This will greatly reduce electricity costs.

- A VFD has built-in safety features that provide protection for both the motor and the VFD itself. The VFD can automatically stop operation when a fault occurs. The built-in safety features eliminate the cost of additional safety components.

- A VFD offers flexible control of AC motors. With VFD, various options are available for controlling a motor. It can use push buttons, selector switches, and a potentiometer, and can even be operated through a **Programmable Logic Controller (PLC)** or other form of industrial controller.

- A VFD reduces disturbances in the power line. When AC motors are started across the line, they can demand up to 600% of the full load current, depending on the capacity of the motor. This can cause a significant disturbance or a short-duration reduction in voltage (voltage sag) in the power system connected to it. Sensitive equipment connected to the same power distribution system can be interrupted due to the low voltage that occurs as a result of the disturbances. A VFD eliminates voltage sag or disturbance in a power line since it starts at 0V and ramps up to the required speed.

Let’s discuss some disadvantages of a VFD as well.

## Disadvantages of a VFD

Despite the many advantages that make VFDs useful for many applications in the industry, they also have some disadvantages that need to be known, understood, and mitigated. Some of the disadvantages include the following:

- **Harmonics**: Harmonics are the most common issue that has to be dealt with when using a VFD. They are high-frequency signals that are superimposed on the fundamental waveform, creating a distorted waveform. They can cause overheating of motors and transformers, premature failure of circuit breakers, incorrect operation of electronic instruments, and other issues. A remedy for harmonics caused by a VFD is the use of an AC line reactor (choke) or filter.

- **High initial cost**: Depending on how large a system is, the initial cost of purchasing a VFD could be a disadvantage. However, the cost issue can be neglected by the energy saving and protection offered by the built-in safety features.

This section explains some of the disadvantages of a VFD along with a way to mitigate each challenge. Let’s proceed to the next section, where we will learn how to select a VFD.

## Selecting a VFD

Before going too far into the selection of VFD, it is important to first know and understand basic motor information, which can be found on a motor nameplate. Here are some motor information that can be found on a nameplate:

- **Horsepower**: This is a measure of the output power of a motor. The output power of a motor can sometimes be given in watts, where 1HP = 746 watts. A 10HP motor is 7.46KW.

- **Full Load Amps (FLA)**: This is the maximum current expected to be drawn by a motor when operating at maximum torque and HP.

- **Voltage**: This is the voltage required to operate a motor for optimal performance. Operating the motor below or above the rated voltage will affect its performance.
