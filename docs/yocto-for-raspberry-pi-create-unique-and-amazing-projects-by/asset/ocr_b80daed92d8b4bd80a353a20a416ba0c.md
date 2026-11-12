# # Dependencies

In order to satisfy dependencies, the recipes must declare what they need to have available during the build process.

BitBake use a special mechanism that allows us to list the build-time dependencies and then checks whether all of the rules are satisfied before the build step. For example, if you work with canutils ( http://pengutronix.de/software/socket- can/download/canutils/ ), you have to set this following variable:

```
DEPENDS = "libsocketcan"
```

In this example, **CANUTILS** needs libsocketcan; therefore, BitBake will start by building the libsocketcan package (and installing the headers into rootfs) before building canutils and linking.

When an application depends on something to run, it is called a runtime dependency (these are packages necessary on the target in order to guarantee proper functioning). In this case, we don't need to set the DEPENDS variable but the RDEPENDS variable in a recipe in order to inform BitBake.