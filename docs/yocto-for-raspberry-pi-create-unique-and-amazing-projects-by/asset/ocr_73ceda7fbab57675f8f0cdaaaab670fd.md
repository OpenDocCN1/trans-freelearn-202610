# Setting up a customized package list

First, you need to add a new BitBake file ( .bb) named packagegroup-pakt-iot.bb:

```
$ mkdir recipes-core/images/packagegroups
$ touch packagegroup-pakt-iot.bb
```

A typical package list will look something like this:

```
$ cat packagegroup-pakt-iot.bb
DESCRIPTION = "Package list for pakt-iot-image"
LICENSE = "MIT"
inherit packagegroup
RDEPENDS_${PN} += " \\
pkg1 \\
pkg2 \\
pkg3 \\
"
```