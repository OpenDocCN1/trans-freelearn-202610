# Integrating with meta-packt_rpi

Now, we will prepare our environment so that all of our development is Yocto compliant.

Here is our custom integration layer for the Nunchuck recipe:

```
$ tree meta-packt_rpi/
meta-packt_rpi/
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
├── recipes-core
│   └── images
│       └── raspberry-packt-image.bb
├── recipes-custom
│   ├── gpio-packt
│   │   ├── fix.patch
│   │   ├── gpio_example.c
│   │   └── gpio-packt_0.1.bb
│   ├── nunchuck
│   │   ├── nunchuck
│   │   │   └── nunchuck.c
│   │   └── nunchuck_0.1.bb
├── recipes-example
│   └── example
│       └── example-packt-0.1
│           ├── example.patch
│           ├── helloworld.c
│           └── example-packt_0.1.bb
├── recipes-example-bbappend
└── example-bbappend
    └── example-packt-0.1
        └── example.patch
        example-packt_0.1.bbappend
```

The nunchuck_0.1.bb file will contain the recipe for implementing the Nunchuck binary in our rootfs.