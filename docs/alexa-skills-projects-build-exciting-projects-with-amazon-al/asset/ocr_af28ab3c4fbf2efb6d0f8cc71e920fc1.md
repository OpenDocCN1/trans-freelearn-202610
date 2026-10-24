The temperature data is in the following format:
ObjectLSB:ObjectMSB:AmbientLSB:AmbientMSB,. For extracting the data, we have created a separate class called Utilities. Add a separate Utilities class to the project and introduce the following extractAmbientTemperature method in it:

```
public static double
extractAmbientTemperature(BluetoothGattCharacteristic c) {
    int offset = 2;
    return shortUnsignedAtOffset(c, offset) / 128.0;
}
```

The extractAmbientTemperature method makes use of the shortSignedAtOffset method, hence we will also add that to the Utilities class:

```
/**
 * Gyroscope, Magnetometer, Barometer, IR temperature
 * all store 16 bit two's complement values in the awkward
format
 * LSB MSB, which cannot be directly parsed as
getIntValue(FORMAT_SINT16, offset)
 * because the bytes are stored in the "wrong" direction.
 *
 * This function extracts these 16 bit two's complement values.
 * */
private static Integer
shortSignedAtOffset(BluetoothGattCharacteristic c, int offset)
{
    Integer lowerByte = c.getIntValue(FORMAT_UINT8, offset);
    Integer upperByte =
    c.getIntValue(BluetoothGattCharacteristic.FORMAT_SINT8, offset
    + 1); // Note: interpret MSB as signed.
    return (upperByte << 8) + lowerByte;
}
```

The preceding code snippet has been sourced from http://processors.wiki.ti.com/.