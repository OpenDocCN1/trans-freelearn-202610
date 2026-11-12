
## Use with sharing Apex classes

Apex code, by default, runs in system mode, which means it has access to _everything_: it can access all objects and their fields, and field-level security and sharing rules don't apply. Check to make sure that your classes are using the `with sharing` keyword because that respects field-level security and sharing rules. The net result of this is that smaller result sets are returned, which can help improve performance.

## Devices and browsers

As discussed earlier in this chapter, the Octane score is a good measure of the browser's performance. The higher the score, the better the performance is expected to be. Since the Octane score takes into consideration the user's device as well, it is important to confirm that the device supports the minimum requirements required for the smooth operation of Salesforce. If the user's device is running at the minimum recommended hardware specifications, upgrade the device with additional memory and processing power.

Another thing is to confirm that the device is charged or is not being throttled in some way. Devices usually switch to low-power mode when they are close to running out of power or users may have inadvertently changed the settings, causing the device to operate in a low-powered mode.

If you determined in your initial investigation that the issue is specific to a user, check their browser. Currently, Chrome is the most optimal browser for running Salesforce Lightning. I highly recommend not using Internet Explorer with Salesforce LEX as your users will run into many problems. If the browser is Chrome, then check for its version and whether the version is supported by Salesforce.

Disabling any extensions or plugins in the browser can also help with performance significantly as they actively consume memory and processing resources. If nothing works, then restarting the browser can also help with performance-related issues as resources that are tied up by previous processes are freed.

## Visualforce pages

You should have standards on where most development is going to happen, be it Visualforce or Lightning. But considering that Visualforce is the older UI and a lot of custom development has been done in Visualforce, I suggest revisiting the previous release notes and determining whether the custom functionality provided by the Visualforce page is now available through out-of-the-box functionality or by using declarative tools.

When you have to resolve a performance-related issue on an immediate basis for a Visualforce page, I suggest reviewing **View State**. **View State** is an encrypted string that stores a collection of values comprising field values, controller state, and components’ state in a hidden form element. Very large view states require large amounts of processing time because the string has to be serialized/deserialized and encrypted/decrypted depending on the type of request.

If you notice the issue is with the size of **View State**, you can reduce the number of variables that are included in **View State** by declaring them as `transient`. Doing this will not include these variables in **View State**, but make sure these are variables that are not needed for maintaining the page state. If **View State** is large because controllers and controller extensions have a large number of fields, consider filtering out any extraneous fields that might not be needed.

### Debug mode

Salesforce uses the production mode by default that optimizes and minifies JS code to reduce its size. This means that the JS code in the browser is obfuscated, and this only applies to framework code – custom code is never minified and obfuscated. This debug flag should always be turned off in production because otherwise, it can significantly slow down performance for the users it has been turned on for.

### Page redesign

Having many fields on the page can also impact performance, so you want to make sure that only fields that are actually used are available on the page. Use tools such as **Field Trip**, for example, to identify fields that are not frequently used and remove them from the page and eventually from your org.

Keep a minimal number of fields that are deemed most important on the primary page and less used fields on another tab. Do the same for components, placing them on second or even third tabs if they are not the most important. The reason is that components that are on the primary page are rendered at the time of initial page load, whereas other components are loaded on demand.

If you notice many related lists on the primary page, move them to other tabs. Salesforce recommends having fewer than three related lists on a tab. For the primary page, place the most important related lists on it.

## Solution tool selection

Not every tool is created equal and that means you must be cognizant of the tool that you are using to deliver your solution. The performance of a Workflow Rule versus a Lightning Flow may be different and can be a factor in deciding which tool to use. This is a design-time consideration, but it can break your business process down the road as volumes increase or more automation is put in place.

I want to explain this point in depth because usually, we don't pay much attention to how the choice of our tool is going to impact performance, and our decision making in most cases is limited to whether the solution choice is declarative versus being code-based. To illustrate my point, let's use an example of simple automation that is used to update an **Account** record when an opportunity has been won. We will update the **Type** field from **Prospect** to **Customer – Direct**. As our tool of choice for this example, we will define **Workflow Rule** and **Lightning Flow** and measure the performance using the **Developer Console**.

Before we set up the workflow rule, let's first ensure that our debug log levels are set up appropriately. Follow the next steps to turn on debug logs:

- 1. Navigate to **Setup | Environments | Logs | Debug Logs**.
- 2. Set up the debug level details as shown in **Figure 6.26**. Once you have set up the debug level, make sure to turn on debugging by updating the start and stop times and ensuring that you have debugging turned on for the correct user:

![](img/782d45700e093b3d82b348c0ccc8ccba_264_0.png)

Figure 6.26 – Note the level of detail for each debug element; by default, other categories are turned on as well, but we want to focus on performance results, so we turned on Database, Workflow, and Apex Profiling

Here is how the workflow rule is set up. There is one action associated with the workflow rule that updates the Type field on the associated Account record to Customer – Direct. Figure 6.27 shows the setup of the workflow rule:

![](img/782d45700e093b3d82b348c0ccc8ccba_265_0.png)

Figure 6.27 – Workflow Rule setup

In the following screenshot, we show how the Field Update action has been set up:

![](img/782d45700e093b3d82b348c0ccc8ccba_265_1.png)

Figure 6.28 – The setup of our Field Update action

Debug logs are on, and the workflow rule has been set up, so go ahead and update the opportunity to Closed Won. Check the Account record to ascertain that the Type field has been correctly updated. Once that has been confirmed, go to Developer Console. In Developer Console, click on the Logs tab, as seen in the following screenshot:

![](img/782d45700e093b3d82b348c0ccc8ccba_266_0.png)

Figure 6.29 – The transaction has been logged

Double-click the log record, which will open the Execution Log page (not shown here). Once the log has opened, go to the Debug menu, click on Switch Perspective, and then click on the Analysis (Predefined) option, as shown in the following screenshot:

![](img/782d45700e093b3d82b348c0ccc8ccba_266_1.png)

Figure 6.30 – The Switch Perspective and Analysis (Predefined) menu options

On the Analysis (Predefined) perspective, click on the Performance Tree tab:

![](img/782d45700e093b3d82b348c0ccc8ccba_267_0.png)

Figure 6.31 – The results from our transaction

You can see that the total process time was 104 milliseconds (ms), while the Workflow Rule tool took 17.71 ms to execute. There were no DMLs executed as part of the process, so the Limits tab (second half of the page) has no information.
