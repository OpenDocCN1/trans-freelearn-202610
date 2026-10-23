![](bbox=[0.117, 0.094, 0.852, 0.509])

Figure 8.15 – Asset configuration

> **Important Note**
> 
> Details for finding your API key for Document Understanding can be found at `https://docs.uipath.com/document-understanding/docs/api-key`.

Lastly, with our project settings set and assets defined, the last setting we need to define before venturing into building the use case is setting our main file. If you look at the project files of our newly created project, you'll notice two `.xaml` files:

+   - `Main-ActionCenter.xaml`
+   - `Main-Attended.xaml`

The reason for the two main files is that they each have small variances for unattended and attended executions. The `Main-ActionCenter.xaml` file contains Action Center activities that assist with validation for unattended scenarios. These actions aren't necessary in attended executions, since we can display the Validation Station to the user without the need for Action Center.