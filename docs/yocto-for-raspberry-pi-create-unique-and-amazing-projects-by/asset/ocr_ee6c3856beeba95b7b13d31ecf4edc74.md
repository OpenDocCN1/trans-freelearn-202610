+   - For example, in our example (raspberry-pack-image.bb), we used several packages through IMAGE_FEATURES, such as splash and ssh-server-dropbear.

We have also used this variable (in Chapter 5 , Creating, Developing, and Deploying on the Raspberry Pi) in conf/local.conf in order to add the package-management package (with EXTRA_IMAGE_FEATURES).

Another example of IMAGE_FEATURES is if we want to work with the integration of the Eclipse IDE, we have to add the following package to the conf/local.conf file:

```
IMAGE_FEATURES += "eclipse-debug"
```