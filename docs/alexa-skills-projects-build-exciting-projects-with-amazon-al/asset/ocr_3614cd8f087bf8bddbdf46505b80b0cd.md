16. We are done with our implementation and now the `onCharacteristicChanged` method should be called for every temperature value update on the SensorTag. As a final step, we can update the `onCharacteristicChanged` method to display live temperature updates to the user. Also, we can set up and include the data upload to the Firebase backend so that our app can keep uploading periodic data updates to the remote server. Our final code for the UI updates and upload to the backend is as follows:

```
```java
@Override
public void onCharacteristicChanged(BluetoothGatt gatt,
BluetoothGattCharacteristic characteristic) {
    super.onCharacteristicChanged(gatt, characteristic);
    if
    (characteristic.getUuid().equals(UUID_CHARACTERISTIC_TEMPERATURE_DATA)) {
        final double ambient =
        Utilities.extractAmbientTemperature(characteristic);
        //Upload to Firebase Backend
        FirebaseDatabase database = FirebaseDatabase.getInstance();
        DatabaseReference myRef =
        database.getReference("Temperature");
        myRef.setValue(ambient);
        //Update the UI
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                ((TextView)
                findViewById(R.id.textView).setText("Temperature: " + ambient
                + "\\u00b0" + "C");
            }
        });
    }
}
```
```

Please note that we still need to set up Firebase on both the client and server sides, due to which the preceding code may fail to compile, as we are referring to Firebase but have not yet set it up. We will set it up in the next step.