# Tutorial

# WALKTHROUGH

# STATIC WARNING!

EEPROMs can be easily damaged by static electricity, so try to ground yourself before handling them. Ideally, wear an anti-static wrist strap, or at the very least leave that nylon sweater in the wardrobe.

You should see a table displaying the devices found on your I²C bus. Assuming you have nothing else connected, all but one of the entries should be empty, as represented by 2 dashes: - -. Normally, your EEPROM should show up in the leftmost column (o) as the number 50, next to the 50 on the horizontal axis. If you don’t see this or the command returns an error (‘No such file or directory’), try the other bus:

```
i2cdetect -y 0
```

If your EEPROM still isn’t showing up, check your connections and that you have I²C support enabled. Now you need some software. Clone the HATs reference material and tools from GitHub (magpi.cc/10AHAgK). It’s well worth reading this documentation, as it explains the thinking behind the HAT specification in lots of detail. You’re also going to use the handy EEPROM tools to flash your chip. First of all, compile the eepmake tool:

```
cd hats/eepromutils
make
```

Then modify the eepflash script, which assumes that the EEPROM is on the first I²C bus; if yours is, then you can obviously skip this step. Copy the eepflash.sh and then modify with your favourite text editor:

```
cp eepflash.sh eepflash1.sh
nano eepflash1.sh
```

Change all mentions of i2c-0 to i2c-1, and i2c-0/0-0050 to i2c-1/1-0050, then save the file. Now you need to modify the supplied template with your own settings. Open the eeprom_settings.txt file in your favourite editor and modify the various fields. Most are self-explanatory and you can read more about each one in the specification. However, the UUID is particularly important and must comply to RFC 4122, so that every HAT can be uniquely identified and can therefore be used as a per-board serial number. It also allows HATs to be stacked. Once that’s done, you need to convert the human-readable text file into binary data that can be written to the EEPROM.

```
./eepmake eeprom_settings.txt eeptest.eep
```

Then dump this data to the EEPROM – this is the actual flashing bit and you’ll see a warning asking you if you’re sure you want to go ahead:

```
./eepflash1.sh -w -f=eeptest.eep -t=24c32
```

Make sure you use the version of the script we modified earlier. If you’re using a different type of EEPROM, change the -t parameter accordingly.

Run the prototype from the breadboard – use the code in the code listing to test it

![](bbox=[0.07, 0.535, 0.95, 0.94])