# # Sensors and InputControllers

Sensors are a common way to control games on smartphones. They work fine
when the only controls in the game are left and right (like a car racing game). If
you plan to also move up and down, you need to ask the player to do a
calibration at the beginning of the game to make it usable. Note that, when you
are using only one axis, such calibration is not necessary.

In addition to this, the up and down movement tends to interfere with the
sensorLandscape orientation. So, the use of sensors is not a very good idea for
YASS.

# ## Note

Sensors are good controls only in certain cases.

You also have to consider that, while sensors are a replacement for directions,
you still need to place the action buttons on the screen—in our case, the fire
button.

We are not going to use sensors for YASS but, if you want to make a game that
uses them, we will cover the basics.

You need to register a listener for the accelerometer and another one for the
magnetic field. You should only listen to the sensors while the game is running,
so we will override the life cycle methods to register and unregister sensors
accordingly:

```
private void registerListeners() {
    SensorManager sm = (SensorManager)
    mActivity.getSystemService(Activity.SENSOR_SERVICE);
    sm.registerListener(mAccelerometerChangesListener,
    sm.getDefaultSensor(Sensor.TYPE_ACCELEROMETER),
    SensorManager.SENSOR_DELAY_FASTEST);
    sm.registerListener(mMagneticChangesListener,
    sm.getDefaultSensor(Sensor.TYPE_MAGNETIC_FIELD),
    SensorManager.SENSOR_DELAY_FASTEST);
}
```

```
private void unregisterListeners() {
    SensorManager sm = (SensorManager)
```