+   16. Click on the **Save** button to save and close the **UI Explorer** window.

+   17. Configure the **Output Value** of the **Get Text** activity by creating and providing a new string variable.

+   18. Add a **Write Line** activity below the **Get Text** activity and configure it to print the value in the string variable that you created.

+   19. Run the process to see whether the robot can extract the invoice number. You should be able to see the extracted number in the **Output** panel.

Use the same concept and extract other document values to become more familiar with document data extraction. Understanding the selectors and best practices for using selectors is very important. Using the knowledge gained in Chapter 10, *Exploring User Interfaces and User/Robot Interactions*, try finding the best selectors to capture the values. As you explore, you might realize that you cannot build a reliable selector to extract some values. In the next section, we will describe what approach you can take in scenarios where you cannot build a reliable selector to extract values from the document.

# Extracting individual values using the Anchor Base activity

We were able to build a reliable selector for the document because the word "invoice" was always there in the document. However, if you look at the values, such as **Subtotal**, **Credit**, and **Grand total**, they only contain a number that could eventually change. The selector extracted for the **Subtotal** field is shown in Figure 11.16:

```
<wnd app='acrord32.exe' cls='AcrobatSDIWindow' title='Invoice #38645 - Adobe Acrobat Reader DC (32-bit)' />
<wnd aaname='Document Pane' cls='AVL_AVView' title='AVScrolledPageView' />
<wnd cls='AVL_AVView' title='AVPageView' />
<ctrl name='$40.00 USD ' role='text' />
```

Figure 11.16 – The selector for the SubTotal field

As you can see in the sample selector, it does not contain a field that we can rely on to locate the correct field. We have an easy workaround to address such scenarios. We can use an **Anchor Base** activity to find an anchor (that is, an additional field that we can rely on) to extract the respective value. The **Anchor Base** activity consists of two sections; the **Anchor** section and the **Value** section:

+   • The **Anchor** section is dedicated to looking for a suitable UI element with a reliable selector next to the value that needs to be extracted. The **Anchor** section of the **Anchor Base** activity is configured with the **Find Element** activity to find the anchor element.