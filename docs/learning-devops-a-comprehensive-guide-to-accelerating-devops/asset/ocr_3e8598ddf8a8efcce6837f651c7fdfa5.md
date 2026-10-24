For the connection to LaunchDarkly, we need to use the SDK key that was provided
when the project was created. Then, in the preceding code, we connect the user who
is connected to the application to the feature flag that we created previously in the
portal.

+   5. Finally, in the Home/Index.cshtml view, we add the following code to add a
+   condition that will display the image, depending on the value of the feature flag:
+   ```
+   <div class="text-center">
+   @if(Model.ShowBoxHome) {
+   <div><img src="img/test.png"></div>
+   }
+   </div>
+   ```

+   6. Finally, we deploy and execute the application. As illustrated in the following
+   screenshot, the home page shows the central image because the feature flags are set
+   to true by default:

![](bbox=[0.117, 0.437, 0.852, 0.639])

Figure 15.14 – LaunchDarkly feature flag demonstration application