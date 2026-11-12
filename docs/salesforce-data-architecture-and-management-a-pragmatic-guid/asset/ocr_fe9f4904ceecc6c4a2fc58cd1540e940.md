Let's now work on the Lightning flow and set up the same process. Here is what our Lightning flow definition looks like:

![](bbox=[0.4, 0.15, 0.6, 0.45])

Figure 6.32 – Lightning Flow that fetches the Account record ID and then updates the Type field when the Opportunity record is set to Closed Won

Now, let's do an update on an Opportunity record, but before that, make sure that the workflow rule we created earlier has been deactivated and the debug logs are turned on. Following the same process as we did for the workflow rule, we see the following results:

![](bbox=[0.13, 0.585, 0.86, 0.785])

Figure 6.33 – The result showing the time it takes for the Lightning flow to update the Type field on the associated Account record

The results are astonishing: the workflow rule took 17.71 ms to complete, whereas Lightning Flow took 411.84 ms to give us the same result. Lightning Flow took approximately 23x longer to update a simple field on the Account object.