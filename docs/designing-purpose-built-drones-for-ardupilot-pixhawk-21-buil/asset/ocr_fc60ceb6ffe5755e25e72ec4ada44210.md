Here is where we can set what every button, stick, dial, and slider on our physical joystick will control.

Once every axis is set on our main controls for **Roll, Pitch, Throttle,** and **Rudder**, you'll need to make sure that they control the green bars in the correct direction. If they don't, just tick the checkbox in the **Reverse** column.

Setting an axis or button is relatively easy. Just click on the **Auto Detect** button, and an alert will pop up telling you to click on **OK**. Don't move the joystick until you do. Once you click on **OK**, the software will begin listening for movement on the game controls. As soon as you move an axis or press a button, it will register that as the desired control.

You may notice that several buttons have a -1 selected in their **Control Axis** dropdowns. This is a designation which means that the button is unassigned. As with the axis, the **Auto Detect** buttons can automatically assign the physical joystick button to that control.

The next dropdown is the type of control that button is assigned to. You may notice that several are set to **Change Mode**. That's because they set the drone into various flight modes. You can set the flight mode using the **Settings** button next to the dropdown. It will open another window, with yet another dropdown in it.

Similarly, you'll find the `Do_Set_Servo` type very useful. It can do all sorts of things, from setting servo-plug controlled gimbal modes, to controlling camera functions via a servo-plug, to raising and lowering retractable landing gear. Basically, it can set a servo value for just about anything controlled via servo-plug. All you have to do is open the **Settings** dialog, and select the output port on Pixhawk, then tell it what you want that PWM value of the control to be (remember, the PWM value we were talking about when we set up flight modes on the controller). You're just mimicking what you'd like it to be as if you flipped a switch on a standard RC remote. On our two `Do_Set_Servo` functions, we're using a toggle switch on the joystick. One direction sets the PWM to 0 (lowered landing gear), and the other direction sets the PWM to 1,800 (raised landing gear).

Once you've finished configuring a joystick, all you really have to do is click on **Save** at the top of the interface, and then click on **Enable** before closing the window.