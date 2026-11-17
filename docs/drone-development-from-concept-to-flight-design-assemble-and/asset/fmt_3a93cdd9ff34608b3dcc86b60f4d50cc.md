
We have now got the current consumption of the drone from the above charts. The total current consumption will always be 5% higher due to other avionics and environmental effects such as wind, so let's keep the current consumption at `14.68 + 5% = 15.414` amperes, which means that the total current consumption of the drone can be considered as `15.414` amperes. We will use this as a base for all calculations.

The following are things to keep in mind when selecting a battery:

- **Weight**: As we have a flying system, it is always advisable to keep the weight as low as possible. We should always look for the battery that has a high energy density and a low weight, which would give us better efficiency and higher endurance.

- **C ratings**: As per battery university, the charge and discharge rates of a battery are governed by C ratings. C ratings determine the safe discharge of the battery, i.e., the number of amperes that a battery can safely give the system without getting damaged. It is mentioned as `5C`, `15C`, `22C`. It is calculated as follows: let us say that the capacity of the battery is `16Ah`, that is, `16000mAh`, the C rating is `5`, and safe discharge is equal to `Ah x C`, so `16 x 5 = 80 amps`. Thus, the battery can give `80 amps` continuously without getting damaged, though our application should be consuming less than that.

We can choose any battery chemistry from among lithium polymer, lithium ion, and solid-state lithium polymer if they satisfy the above ratings considering the weight and energy density of the battery.

# Calculating the flight time (endurance)

The flight time is calculated using a simple fundamental equation: energy stored divided by energy consumed.

For our application, the total current consumption came out to be `15.5 Amps` (rounded off). If we select a battery of `16 AH`, it would satisfy our endurance requirement with minimal weight.

## Formula to calculate flight time

The flight time (T) of a multicopter can be estimated using the following simplified formula:

```
Flight Time = ( Capacity(Ah) × 60) / Ampere Draw
```

where flight time is to be calculated, capacity in Ah = `16Ah` or `16000mAh`, and ampere draw is `15.5`.

Putting the values into the formula gives `61.93 minutes`.

Hence, we have calculated the flight time of the `4 kg` drone to be `61.93 minutes`.

This is how we can select the right propulsion train and calculate the flight time of the system. Note that the flight time is an approximate, ideal flight time. In reality, flight time is impacted by temperature, humidity, flying altitude, wind, and many more factors due to which endurance can vary.

# Designing and developing the airframe

An airframe is the skeleton of the drone that holds the avionics and other components together tightly and stiffly. It's the structure that actually moves into the air and gets the effect of the environment; hence, an airframe should be rigid, strong, and lightweight. We can either design our own airframe using the different software available and following the composite manufacturing process or we can use the airframes available on the market.

Whether we are designing the airframe or getting the airframe, we need to consider the following points that would be helpful in giving us an idea of how much load the drone can carry and how maneuverable it can be:

- **Professionally engineered**: When an airframe is developed in a laboratory, certain materials are selected, such as carbon fiber, glass fiber, and so on, and have to go through the manufacturing process of cutting drilling. The material, that is, carbon fiber, is laid up in molds using resin with certain layer sequences and kept for curing. This makes the fiber hard and makes it take the shape of the mold. Hence, the desired shape comes out and is then engineered using machining.

    This is the process of building an airframe if you have designed it in-house and have some unique concepts regarding it. However, for a beginner build, there are quadcopter/hexacopter airframes available on the market to start the build.

    An airframe should be smoothly finished internally and externally without any rough surface and equivalently sized across all axes.

- **Configuration and size**: The general term of measurement of an airframe is known as the wheelbase. The wheelbase is known as the diagonal center-to-center distance between two motor mounts. The propellers should perfectly fit into the airframe without colliding with any parts of the airframe or themselves in the air. There shouldn't be any loose ends in the airframe. The airframe should also be balanced under all configurations with a proper center of gravity and center of mass.

- **Mounts and fixtures**: Mounts and fixtures are the hardware on which avionics such as motors, `ESC`, and other parts are installed on the drone in the desired manner. These mounts and fixtures for the motors are designed and developed with holes and fixtures for the placement of `PDBs`, `ESCs`, and flight controllers. Payload mounts and spaces have to be maintained for free movement of the payload. The details for mounting hardware, such as screws, glues, nuts, and bolts, should be proper.

- **Material**: The material for manufacturing the airframe should be lightweight and sturdy with good strength across all axes. Composite materials such as carbon fiber and glass fiber or Kevlar are widely used for manufacturing airframes due to their lightweight and excellent strength properties. These materials should also be passive towards the environmental conditions and not be subject to wear and tear in the long run.

- **Strength**: The airframe should have adequate strength to handle the load of force produced by the motors and gravity. Vulnerable areas such as arm joints and motor mount joints must be strong and able to take the load mid-air under sharp maneuvers.

# Developing the control system

In this section, we will cover control system development.

## Electronic speed controllers and flight controllers

An `electronics speed controller` (`ESC`) is a small device that comes between the `flight controller` (`FC`) and the `BLDC` motors. An `ESC`, as the name suggests, controls the speed at which the `BLDC` motor rotates as per the signals received from the `flight controller`. Each motor requires one `ESC` to control its speed and is connected to the `flight controller`. The `FC` simultaneously has `pulse with modulation` (`PWM`) pins to which the `ESC` is connected. The `FC` gives the `PWM`/`DShot` signal to the `ESC` as per the commands given by the `GCS` `RC` to run the motors. The `ESC` gets power from the battery and signals from the `flight controller`. The following diagram illustrates this setup:

![Figure 4.4 – The workings of an ESC](image/B19566_04_4.jpg)

Figure 4.4 – The workings of an ESC

The `ESC` is connected to the battery and supplies the battery voltage to the motors. The `ESC` is a kind of oscillator or pulse generator that passes a variable duty cycle and is commonly used as a speed controller for motors. This is controlled by the `FC` via the `PWM` signals. These `PWM` signals generated by the `FC` to operate a `BLDC` motor are fed into the `ESC`. The value of a `PWM` signal varies between `1000` to `2000` microseconds, where `1000` represents the lowest voltage level and `2000` is the highest voltage level that has to be supplied to the motors. This voltage value depends on the value of the `PWM` being supplied to the `ESC`. The `flight controller` has special motor driving algorithms due to which roll pitch and yaw are controlled based on the speed of the motors.

The factors to be considered while choosing the `ESC` for the drone are as follows:
