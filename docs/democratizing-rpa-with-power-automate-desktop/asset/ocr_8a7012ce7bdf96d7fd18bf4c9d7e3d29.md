![](bbox=[0.18, 0.08, 0.84, 0.33])

Figure 6.13 – Updating MS Access with invoice information

This whole procedure seems a bit cumbersome at first glance, but it reveals the following:

+   - PAD is fully capable of interacting with applications and confirming dialogs
+   - It does not matter whether it is an application of Microsoft or not, because the concept of UI elements is universal
+   - PAD can extract data from other applications and use them with UI elements to create further actions
+   - If the task presented here has to be performed once or even several times a day (which could well be the case), then the integration and automation shown here pay off several times more

In addition to these important points, I would like to make the following comments:

+   - As already mentioned, there is also a billing function in the database. However, before an invoice can be created, further conditions must be fulfilled, which I have ignored for the benefit of this example.
+   - When handing over the amount, we have not performed any currency conversion for the sake of simplicity.