# # Whitelisting

Domain whitelisting is a security model that controls the access to external domains. Cordova provides a configurable security policy to define which sites to access. For Cordova 4.0 and newer, we should use `cordova-plugin-whitelist` as it provides better security and configuration settings. Currently `cordova-whitelist-plugin` Supports Android higher than 4 and for our platforms we have to configure security in the `config.xml` file: `<access origin="*" />`

While creating a project, by default all the domains are whitelisted, which means you will have access to all domains. Also, domain whitelisting is not available on and below Android API 10. So this is another important thing to note that you set your `min-target-sdk` above 10. Also, Ionic itself supports above Android version 4.4 . It will work on old devices and sdk, but it may have performance and other issues. Also, we don't have a big market share of devices with less than Android 4.4. So for building applications for the future we can ignore some share of market to make your application secure and good in performance. With new technologies coming, such as the BLE support application, even many native applications are only supporting latest versions.

We have already covered the whitelist plugin in [Chapter 3, *Ionic Native and Plugins*] so you already have an idea of how we can easily configure various setting and whitelist resources. The new content security policy in the plugin gives you control over which network requests (images, XHRs, and so on) are allowed to be made (via web view directly):

```
<!-- Allow everything but only from the same origin and foo.com -->
<meta http-equiv="Content-Security-Policy" content="default-src 'self' foo.com">
```

```
<!-- Allows XHRs only over HTTPS on the same domain. -->
<meta http-equiv="Content-Security-Policy" content="default-src 'self' https:">
```

```
<!-- Allow iframe to https://cordova.apache.org/ -->
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; frame-src 'self' https://cordova.apache.org">
```

You can find various other examples of configurations such as the previous ones in the official documentation of the whitelist plugin: https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-whitelist/.