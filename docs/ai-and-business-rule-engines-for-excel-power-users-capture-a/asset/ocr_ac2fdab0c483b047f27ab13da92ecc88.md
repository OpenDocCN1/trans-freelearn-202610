+   2. The dropdown gives us a choice of forms to trigger our flow – select the form we created in the previous section.

+   3. Add another action immediately after this trigger – **Get response details**. Form Id should be the same form, and **Response Id** can be obtained via the dynamic content popup – it should look like *Figure 8.24*:

![](bbox=[0.132, 0.219, 0.852, 0.356])

Figure 8.24 – Getting a handle to the form information

If we forget this step, the form IDs don’t pop up as suggestions when we try to select values.

+   4. Add another new action immediately after this step – **Initialize variable** – as in *Figure 8.25*:

![](bbox=[0.135, 0.478, 0.851, 0.649])

Figure 8.25 – Getting country of residence from the form

+   5. We name the variable `Country`, and set its type to `String`. The value we set (using the **Dynamic Content** popup) is **Country of residence** for our form.

+   6. Note, we’ve renamed the name of this (and other actions) to make things clearer – you don’t have to do this, but you’ll be glad later that you did!

+   7. We update the step where we initialize our payload, as in *Figure 8.26*. It's renamed `Payload_template`. Note that the value is also slightly different – with a `__cor__` marker in the `Country of residence` field – we’ll find and replace this step later. There is nothing special about this value – it’s just some text that is easy to understand, yet unlikely to occur naturally in the file (to prevent accidental matches).