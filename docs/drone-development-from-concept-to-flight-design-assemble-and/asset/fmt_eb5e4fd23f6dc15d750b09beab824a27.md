
# PID Controllers in Drones

## Importance of Attitude Control

Attitude control is essential for maneuvers, maintaining a straight flight path, and responding to pilot inputs accurately.

## Position Hold and Waypoint Navigation

**Use**: PIDs enable drones to maintain a fixed position in GPS coordinates (position hold) and follow predefined flight paths (waypoint navigation). They adjust motor speeds and control surfaces to counteract external factors such as wind.

**Importance**: Position hold and waypoint navigation are vital for applications such as aerial photography, surveying, and search and rescue, where precise positioning is necessary.

## Response to Environmental Disturbances

**Use**: PIDs help drones respond to environmental disturbances, such as wind or turbulence, by making immediate corrections to maintain stability.

**Importance**: Environmental disturbances can impact a drone’s flight. PIDs help counteract these effects to ensure smooth and controlled flight.

## Smooth Flight

**Use**: PIDs contribute to smoother flight by reducing oscillations and overshooting. They provide gradual corrections to maintain a stable flight path.

**Importance**: Smooth flight is essential for capturing high-quality images and videos, especially in aerial photography and videography.

## Safety

**Use**: PIDs play a critical role in ensuring safe drone operations. They prevent erratic movements and help avoid dangerous situations, reducing the risk of crashes.

**Importance**: Safety is a paramount concern in drone operations, both for the drone and the people and property it interacts with.

## Error Correction

**Use**: PIDs continuously monitor the error between the desired and actual drone state. They adjust control surfaces and motor speeds to minimize this error.

**Importance**: Error correction is essential for maintaining precision in flight and meeting the operator’s intentions.

## PID Components

### Proportional (`P`)

The proportional component of the `PID` controller measures the current error between the desired and actual state of the drone. In the context of drones, this could involve the difference in orientation (roll, pitch, yaw) or position (latitude, longitude, altitude). The `P` term calculates how much and in which direction the drone should adjust to minimize the error. If the error is large, the correction applied by the proportional component is significant, and if it’s small, the correction is minor.

**Use**: The proportional term calculates a corrective action proportional to the error. If the drone deviates from the desired state, the `P` term applies a correction that is directly proportional to the magnitude of the error.

**Importance**: The `P` term’s primary role is to provide an immediate response to errors, helping the drone maintain the desired orientation, position, or flight path.

### Integral (`I`)

The integral component considers the accumulation of past errors over time. It is responsible for addressing any long-term errors that the proportional component alone cannot fix. For drones, this can help correct slow, persistent disturbances, such as wind or minor imbalances in the drone’s structure.

**Use**: The `I` term addresses long-term or persistent errors. It helps correct slow, continuous disturbances or errors that the `P` term alone cannot resolve. For instance, it compensates for slight imbalances or biases in the drone’s system.

**Importance**: The `I` term is critical for fine-tuning and eliminating steady-state errors that may not be immediately addressed by the `P` term.

### Derivative (`D`)

The derivative component accounts for the rate of change of error. It helps prevent overshooting or oscillations around the desired state. In drone control, the derivative component can reduce the impact of rapid changes in orientation or position, such as abrupt maneuvers.

**Use**: The `D` term helps dampen the drone’s response to rapid changes in error. It mitigates the potential for overshooting or oscillations that can occur during quick maneuvers or corrections.

**Importance**: The `D` term is essential for stabilizing the drone and ensuring that it does not exhibit erratic behavior or excessive oscillations, especially during aggressive flight or when responding to sudden disturbances.

## Methods to Detect Bad PIDs in Drones and How to Fix Them

We need to know from continuous flights whether we need to tune our drone or not. Here is a list of observations that we can make from the flight behavior and what solutions we can utilize for them:

### Excessive Vibrations and Oscillations

**Symptoms**: If your quadcopter exhibits pronounced vibrations and oscillations, especially during hover or steady flight, it may indicate issues with the `PID` values. You might see rapid back-and-forth movements or jittery behavior.

**Solution**: Check and adjust the `P`, `I`, and `D` values to reduce vibrations and achieve smoother flight.

### Slow Response to Commands

**Symptoms**: A quadcopter that responds slowly to pilot inputs, particularly during maneuvers or directional changes, may signal suboptimal `PID`s.

**Solution**: Increase the `P` values for more responsiveness. Adjusting the `P` term makes the quadcopter react more quickly to commands.

### Overshooting and Bouncing

**Symptoms**: If the quadcopter overshoots its desired position and then bounces back, it could indicate that the `P` term is set too high.

**Solution**: Decrease the `P` value until the overshooting stops while maintaining stability. Fine-tune it to achieve a balance.

### Inaccurate Altitude Hold

**Symptoms**: Altitude fluctuations during hovering or visible deviations in height may indicate the `I` term requires adjustment.

**Solution**: Increase the `I` value to address altitude drift or decrease it if the drone tends to fluctuate. Find the point where the quadcopter maintains a consistent altitude.

### Yaw Drift

**Symptoms**: Yaw drift occurs when the quadcopter’s heading slowly changes without pilot input, often due to incorrect `D`-term tuning.

**Solution**: Modify the `D` value to reduce yaw drift. Increasing `D` helps reduce oscillations and maintain heading stability.

## Rate Controllers in a Drone and Tuning PIDs

Drones use a sophisticated control system to maintain stable flight and respond to pilot commands or autonomous instructions. This control system includes various controllers, loops, and rate controllers. Tuning individual gains and parameters within these controllers, especially in `PID` controllers, is crucial for optimizing drone performance. Here’s a detailed explanation of these components and the process for tuning gains and parameters:

### Attitude Controller

**Role**: The attitude controller is responsible for maintaining the drone’s orientation, which includes roll, pitch, and yaw angles. It ensures the drone remains level and responds to user inputs or autopilot commands.

**Architecture**: The attitude controller is often implemented as a `PID` controller. The input to this controller consists of the desired attitude (from user or autonomous commands) and the current attitude measured by onboard sensors, such as accelerometers and gyroscopes.

**Gains**: The primary gains for attitude control include `P` (Proportional), `I` (Integral), and `D` (Derivative) gains for roll, pitch, and yaw control. They are usually denoted as `Roll_P`, `Roll_I`, `Roll_D`, `Pitch_P`, `Pitch_I`, `Pitch_D`, `Yaw_P`, `Yaw_I`, and `Yaw_D`.

**Tuning gains**: Adjust `Roll_P`, `Pitch_P`, and `Yaw_P` to control the drone’s sensitivity to attitude changes. Higher values make the drone react more aggressively to level deviations.
