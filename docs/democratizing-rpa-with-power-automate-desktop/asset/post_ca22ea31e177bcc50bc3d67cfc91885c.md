
![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_58_0.png)

# Figure 2.24 – Dragging the browser action onto the canvas

3. At this point, you might be requested to install the browser extension for PAD:

# You need the Power Automate extension.

+   1. To use web automation in your flows, select 'Get Extension' to download and install to the selected browser.
+   2. After installing, be sure to enable the extension in the browser.

# Figure 2.25 – Browser extension installation dialog

4. This will only happen once. Get the extension and make sure that it is activated.

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_60_0.png)

Figure 2.27 – The recorder button

At this point, the PAD designer will disappear, and a smaller recorder window will appear instead. It should look like this:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_60_1.png)

Figure 2.28 – The recorder window

Now, the funny part begins. Start the recording by either clicking the button in the middle of the screen or at the top. The recorder window will change and display an empty area under **Recorded actions**. Use your mouse and hover over different elements on the website, such as the book picture. You will see that PAD tries to identify the different elements on the website and frames them accordingly. In the following screenshot, I hovered over the book image:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_61_0.png)

Figure 2.29 – Detecting web elements

For the recorder to recognize the web page elements correctly, it sometimes takes a bit of dexterity. It also helps to move the mouse out of the desired area and back in again to get the right mark. Once the correct element has been highlighted, click the right mouse button to be able to select the information we want to extract from this element. A context menu for this element will appear so that we can select different entries:

Figure 2.31 – A recorded action in the recorder

## Important note – remove additional actions

You might have other actions that have been recorded by the recorder, depending on where you clicked and what keys you might have pressed. For our example, there must be only one action in the recorder. Use the **Delete** button (trashcan symbol) to remove any additional actions.

Now, repeat the same process for the title of the book. Use your mouse to hover over the elements and the title and click the right mouse button:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_64_0.png)

Figure 2.32 – Extracting data from a web element

This time, we want to extract the title of the element. With that, we have captured two elements on the web page, which are represented in the recorder window like this:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_65_0.png)

Figure 2.33 – Extracted data in the recorder

There is an important difference here: PAD already recognized that we captured two pieces of information from one record on the web page. Our next step will be to go to the next element, and this is where some magic starts. If you repeat the first step and mark the next book image and extract the Src information again, PAD will recognize this pattern and assume that you want to do the same as with the first element. We can see this on the web page because PAD has marked the corresponding elements with a green dotted line:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_66_0.png)

Figure 2.34 – Pattern recognition of the recorder

## Pattern recognition

Please notice that it sometimes takes capturing more than two structures before the recorder recognizes the pattern. So, please proceed to the next element if PAD does not recognize the pattern immediately. Recognition

also depends on the underlying structure of the HTML. After the third or fourth element at the latest, the pattern will be recognized.

This means that PAD will capture all the elements on this page now. But what about the next page of books? Let’s scroll down so that we can see the pagination control on that website:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_67_0.png)

Figure 2.35 – Incorporating navigation

We can now use the single arrow button to load the next page. Right-click on the element shown in the following screenshot and choose Set this element as pager:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_68_0.png)

Figure 2.36 – Using a pager in a recording

The element now gets a little blue dotted border, indicating the pager functionality. Looking at the recorder window, just one element should be seen here:

Help

Done

Cancel

# Figure 2.37 – Recorded web scraping actions

If other elements have been captured by mistake, just delete them. You can now stop the recording by clicking the Done button. Now, the recorded actions get transferred to the main PAD design window:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_70_0.png)

# Figure 2.38 – Transferring recorded actions to the designer

There are also comments in the list of actions describing the start and end of the generated actions by the recorder. Let’s investigate the Extract data from web page action a bit more by double-clicking it:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_71_0.png)

Figure 2.39 – Revealing the web data extraction

The interesting parts here are **Extract data from** and **Store data mode**. After the browser has been loaded by our first action in the flow, this action extracts all the data from the web page unless we don’t tell it otherwise. While on some occasions this might be useful, for our test, we only want to extract the first 10 pages. Click on the dropdown for the **All available** entry and change this to **Only the first**. Now, another text box will appear, where we can enter the number of web pages to process.

The last setting for this example is **Store data mode**. So far, the data that is captured is stored in a variable (`DataFromWebPage`, in my example). But of course, there are other ways to work with this data now. For example, you could take this variable and use it as input for another action after the web data has been extracted, or you could choose to store this in Excel. Therefore, click the dropdown for **Store data mode** and change the value from **Variable** to **Excel spreadsheet**:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_72_0.png)

Figure 2.40 – Modifying the web data extraction parameters

Now, the web data extraction will only run for the first 10 pages, take the data, launch Excel with a new spreadsheet, and paste the captured data in there. Save the dialog now and let the show begin. On my machine, it took 45 seconds to extract 120 records and paste them into Excel.

## Summary

Installing PAD is straightforward, just like installing any other Microsoft tool. Once you have signed up for a Microsoft account, you can log in to the program and start creating your UI flows right away.

It is recommended to try things out and use the tool even to automate simple tasks. This will make you more familiar with the program, as I explained in this chapter. The recorder can save you a lot of work and it can be used for new or existing flows. Especially if web scraping is a task, this tool can be very handy.

In the next chapter, we will continue our journey and look at the editing and debugging capabilities of PAD.

## Further reading

To learn more about the topics that were covered in this chapter, take a look at the following resources:

- *Install Power Automate Browser extensions:* https://learn.microsoft.com/en-us/power-automate/desktop-flows/install-browser-extensions
- *Record desktop flows:* https://learn.microsoft.com/en-us/power-automate/desktop-flows/recording-flow

Figure 3.2 – Action kebab menu options

We have the following options:

- **Edit**: This pops up the **Parameter** dialog for the specific action. This dialog can change depending on the action we are using. A **Launch Outlook** action has other settings than **Get files in folder**.
- **Run from here**: If we use this command (or *Alt+F5*), the flow will start to execute from this action onward. All previous actions will not be executed. If there are other actions before, which are necessary for our selected action to be executed, this might lead to an error. But nevertheless, it could also be a handy function if the flow ran into an error previously, and you just want to start right there.
- **Undo / Redo / Cut / Copy / Paste**: These are well-known editing functions from other programs, such as Office.
- **Move up / Move down**: Instead of clicking and dragging an action, we could use these commands or use shortcuts.
- **Disable action**: This is also a very useful feature when designing a flow. It allows you to disable an action so that it will not be executed.

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_76_0.png)
