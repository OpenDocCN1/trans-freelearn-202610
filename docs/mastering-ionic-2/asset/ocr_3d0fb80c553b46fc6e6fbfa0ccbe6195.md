# # Ionic deploy

The Ionic deploy service gives you access to live deployments, which helps you publish web assets such as HTML, CSS, and JS directly to your users without uploading to the Play Store every time. You will have many advantages of using this approach for live deployments:

+   - Real-time Updates on demand
+   - Save app stores approval time
+   - A/B testing by uploading different snapshots to different channels

Before we get started, let's install the deploy plugin, which will update your application on Android and iOS devices:

```
`$ ionic cordova:plugin add ionic-plugin-deploy -save`
```

We will be injecting the `Deploy` method inside our `app.component.ts`, so every time when the application is opened it checks for new updates: // src/app/app.component.ts

```
```typescript
import { AlertController } from 'ionic-angular';
import { Deploy } from '@ionic/cloud-angular';

constructor(
public deploy: Deploy,
public alertCtrl: AlertController
) {

// checks if new snapshot available
this.deploy.check().then((snapshotAvailable: boolean) => {
if (snapshotAvailable) {
let alert = this.alertCtrl.create({
title: 'Update Available !',
message: 'Do you want to update the application now ?',
buttons: [
{
text: 'Later',
role: 'cancel',
handler: () => {
console.log('Cancel clicked');
}
},
{
text: 'Update',
handler: () => {
this.deploy.download().then(() => {
```
```