We will need to match the name of each device found with the name of the SensorTag, and the NAME_TAG variable will help us do exactly that. Once the correct match is found, we initiate a connection by calling the connectGatt method and indicate that the connection request has been made by updating the state of the connected variable.

10. The call to the connectGatt method takes three arguments. The last argument is a callback method, which can be used for:

+   - Monitoring connection-state changes
+   - Service discovery
+   - Characteristic changes

We will initially implement this callback in the MainActivity class for listening to connection-state changes:

```
protected BluetoothGattCallback gattCallback = new
BluetoothGattCallback() {
    @Override
    public void onConnectionStateChange(BluetoothGatt gatt, int status, int newState) {
        super.onConnectionStateChange(gatt, status, newState);
        if (newState == BluetoothGatt.STATE_CONNECTED) {
            Log.i(TAG, "onConnectionStateChange() - STATE_CONNECTED");
            gatt.discoverServices();
        }
    }
};
```

As per the preceding implementation, if the connection succeeds, then we initiate service discovery.

11. We are interested in two services: IR_TEMPERATURE_SERVICE and HUMIDITY_SERVICE. We first define their UUID(s) in the MainActivity class:

```
private static final String UUID_IR_TEMPERATURE_SERVICE =
"f000aa00-0451-4000-b000-000000000000";
private static final String UUID_HUMIDITY_SERVICE =
"f000aa20-0451-4000-b000-000000000000";
```