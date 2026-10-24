12. As already pointed out, the temperature data characteristic supports notifications, and we can enable these notifications for periodic data updates for temperature data. Once we have successfully read the temperature data characteristic, we can enable notifications for this characteristic while overriding the `onCharacteristicRead` method in the `BluetoothGattCallback` implementation:

```
```java
@Override
public void onCharacteristicRead(BluetoothGatt gatt,
BluetoothGattCharacteristic characteristic, int status) {
    super.onCharacteristicRead(gatt, characteristic, status);
    if
    (characteristic.getUuid().equals(UUID_CHARACTERISTIC_TEMPERATURE_DATA)) {
        //Enable local notifications
        gatt.setCharacteristicNotification(characteristic, true);
        //Enabled remote notifications
        BluetoothGattDescriptor desc =
        characteristic.getDescriptor(CONFIG_DESCRIPTOR);
        desc.setValue(BluetoothGattDescriptor.ENABLE_NOTIFICATION_VALUE
        );
        gatt.writeDescriptor(desc);
    }
}
```
```

Once **Notifications** are enabled, whenever the temperature data is updated on the SensorTag, the app will receive the updated value in the `onCharacteristicChanged` method.

13. As already pointed out in the last step, we will override the `onCharacteristicChanged` method so that we can receive the updated temperature value in order to parse and print it on the console:

```
```java
@Override
public void onCharacteristicChanged(BluetoothGatt gatt,
BluetoothGattCharacteristic characteristic) {
    super.onCharacteristicChanged(gatt, characteristic);
    if
    (characteristic.getUuid().equals(UUID_CHARACTERISTIC_TEMPERATURE_DATA)) {
        double ambient =
        Utilities.extractAmbientTemperature(characteristic);
    }
}
```
```