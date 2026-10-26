# Device plugin

Device plugin is really helpful in getting all the device related hardware and software information. Device objects are available in global scope, but only after the device is ready.

Ionic Native supports this plugin with devices and it has six static members to access data:

+   - `Device`: Returns Object with Cordova, model, platform, UUID, and version, for example, `Device.uuid`
+   - `cordova`: Returns string with specific Cordova versions
+   - `Model`: Returns string with the device's model name
+   - `platform`: Returns string with the operating system name
+   - `uuid`: Returns string with the device's **Universally Unique Identifier** (**UUID**)
+   - `version`: Returns string with the operating system version
+   - `isVirtual`: Checks whether the device is running on a simulator or not
+   - `serial`: Fetches the hardware serial number
+   - `manufacturer`: Fetches the device's manufacturer

```
import { Device } from '@ionic-native/device';

constructor(platform: Platform, private device: Device)
{
  this.platform.ready().then(() => {
    console.log(this.device.version);
  });
}
```