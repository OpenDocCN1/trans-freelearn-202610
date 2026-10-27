You can read the device model by running the following command:

```
ideviceinfo | grep ModelNumber
```

# Identifying the iOS version:

To identify what version of iOS the device is running, you can run the following command:

```
ideviceinfo | grep ProductVersion
```

# Identifying the device serial number:

The device's serial number can be parsed by running the following command:

```
ideviceinfo | grep SerialNumber
```

# Identifying the phone number:

Finally, to identify the phone number related to the SIM card in the device, run the following command:

```
ideviceinfo | grep PhoneNumber
```

# Deciding the best acquisition method

Once the device has been seized, it has been correctly isolated, and you have identified the hardware model and iOS version, the next step in the examination process is assessing what the best acquisition method is to extract the richest dataset from the device while preserving data integrity.