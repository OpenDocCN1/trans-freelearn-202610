# Lifecycle events and Nav Guards

Lifecycle events have been there in Ionic 1, and Native platforms also have a well structured lifecycle, which allows us to perform actions during various stages of navigation.
There are six Page events and two Nav Guards available:

+   - ionViewDidLoad: When the page has loaded
+   - ionViewWillEnter: When the page is about to enter
+   - ionViewDidEnter: When the page is fully entered
+   - ionViewWillLeave: When the page is about to leave
+   - ionViewDidLeave: When the page has finished leaving
+   - ionViewWillUnload: When the page is about to be destroyed
+   - ionViewCanEnter: Runs before the view can enter
+   - ionViewCanLeave: Runs before the view can exit

![](bbox=[0.152, 0.345, 0.848, 0.667])

ionViewCanEnter and ionViewCanLeave are two Nav Guards that help control views leaving and entering. One such example is if we are filling up a form and accidentally click some button or menu item, it will immediately clear the data that the user has filled. So to secure the data we can apply ionViewCanLeave, like we did on our support page:

```
// src/pages/support/support.ts

// If the user enters text in the support question and
// then navigates
// without submitting first, ask if they meant to leave
// the page
ionViewCanLeave(): boolean | Promise<boolean> {
// If the support message is empty we should just
// navigate
if (!this.supportMessage ||
this.supportMessage.trim().length === 0) {
return true;
}

return new Promise((resolve: any, reject: any) => {
```