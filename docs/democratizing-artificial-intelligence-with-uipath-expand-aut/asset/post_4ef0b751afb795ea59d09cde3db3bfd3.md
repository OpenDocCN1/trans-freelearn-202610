
6. Drag a **Move Outlook Mail Message** activity into the workflow, setting the following parameters:

- MailMessage: email
- MailFolder: "Inbox\Email Classification\Spam"
- The parameters are shown in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_306_0.png)

Figure 9.31 – For Each Spam Email

With the for loop, adding each spam email to our data table, and moving each spam email into the Spam folder, we can add the same steps for the not-spam emails:

1. Drag a **For Each** activity into the workflow, looping through each email in in_listNotSpamEmails.
2. Within the **For Each** activity, drag an **Add Data Row** activity into the workflow, setting the following parameters:
   - ArrayRow: {email.Body, "ham"}
   - DataTable: retrain_DT
3. Drag a **Move Outlook Mail Message** activity into the workflow, setting the following parameters:
   - MailMessage: email
   - MailFolder: "Inbox\Email Classification\Invoices"

With both email folders traversed through, added to retrain_DT, and moved into their intended folders, we can now focus on writing retrain_DT to a CSV file and uploading the CSV file to our AI Center training dataset:

1. Drag a **Write CSV** activity into the workflow, setting the following parameters:
   - FilePath: "retrain.csv"
   - DataTable: retrain_DT
   - **Add Headers**
2. Drag an **Upload File** activity into the workflow, setting the following parameters:
   - **Project**: Email Classification
   - **Dataset**: Email Classification
   - **Path into dataset**: "train/"
   - **File**: "retrain.csv"

The process is illustrated in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_308_0.png)

Figure 9.32 – Uploading retrain_DT to our training dataset

With Retrain_Model.xaml complete, we can focus on connecting it with Main.xaml and fetching emails from the Retrain-Spam and Retrain-NotSpam folders:

1. Open Main.xaml and open the **End Process** sequence.
2. Drag a **Get Outlook Mail Messages** activity into the workflow, setting the following parameters:
   - **MailFolder**: "Inbox\Email Classification\Retrain-Spam"
   - **Top**: -1
   - **Output**: list_retrainSpamEmails
3. Drag another **Get Outlook Mail Messages** activity into the workflow, setting the following parameters:
   - **MailFolder**: "Inbox\Email Classification\Retrain-NotSpam"
   - **Top**: -1
   - **Output**: list_retrainNotSpamEmails
   With our emails extracted, we can now pass them to our newly created Retrain_Model workflow, but we only need to run this workflow if we have any emails.
4. Drag an **If** activity into the workflow, setting the condition to (list_retrainSpamEmails.Count > 0) OR (list_retrainNotSpamEmails.Count > 0).
5. Within the **If** activity, drag an **Invoke Workflow File** activity into the workflow, setting the following parameters:
   - **Workflow file name**: "Retrain_Model.xaml"
   - **in_listSpamEmails**: list_retrainSpamEmails
   - **in_listNotSpamEmails**: list_retrainNotSpamEmails

And that's it! Within Figure 9.33, we can see what our End Process sequence should resemble. In this section, we successfully added retraining capabilities to our automation project by first building a full pipeline to retrain our ML package every 7 days and then building an automation workflow to extract validated emails and add the validation to our training pipeline. With the development and testing portions complete, in the next section, we will venture into deploying the automation into UiPath Assistant.

## Deploying with the end-user experience in mind

With the initial development and testing of the use case complete, let’s venture into deploying the automation into a production environment. In this section, we will work on deploying the automation into UiPath Assistant so that it can be run in an attended scenario.

### Deploying the project

In a production setting, we shouldn’t run our automation directly from UiPath Studio, nor should we expect our end users to have access to UiPath Studio. Thus, we must deploy our automation into UiPath Orchestrator to allow others to run our automation from UiPath Assistant.

> **Important Note**
> More information on deploying automation can be found at the following link:
> `https://docs.uipath.com/studio/docs/about-publishing-automation-projects`

For this use case, we're going to quickly publish the automation into our own personal workspace within UiPath Assistant. Follow the next steps:

1. Within the `Email Classification` project, click the **Publish** button in the **Design** ribbon of UiPath Studio.
2. Check that the **Publish to** field is set to **Orchestrator Personal Workspace Feed**, as illustrated in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_312_0.png)

Figure 9.34 – Publishing the automation

3. Click **Publish**.

Once published, you should be able to see the automation within UiPath Assistant. It should look like this:

![](img/05268897c8ac5d80eb3fead1cc8a822a_313_0.png)

Figure 9.35 – Project deployed to UiPath Assistant

Clicking the **Play** button to the right of `Email.Classification` will install the automation on your machine and start execution.

Once deployed, you are free to run the automation directly from UiPath Assistant. Any changes to the code can be made in UiPath Studio and then republished following the steps just discussed. In this section, we completed the use case build by deploying the `Email Classification` project into UiPath Assistant—great job!

## Summary

In this chapter, we covered building a cognitive automation use case with *UiPath AI Center*. In the construction of the use case, we learned how to create a project in AI Center, load training and evaluation datasets, build training and evaluation pipelines, and—finally—deploy an ML skill. We also learned how to leverage the ML skill within our UiPath project and learned how to close the feedback loop with UiPath's **Upload File** activity, allowing us to upload training data back into the training dataset of AI Center. With these foundations, you can now venture off and build your own cognitive automation.

In the next chapter, we will take another hands-on approach by working together to build the last of three use cases with UiPath and Druid. We will learn how to integrate chatbots and UiPath.

# 10
Use Case 3 –
Chatbots with Druid

We can bridge conversational artificial intelligence (AI) and UiPath automation with intelligent chatbots to modernize operations and enhance the user experience (UX). With chatbots, we can integrate intelligent communication tools into messaging apps or chat windows, giving end users an almost immediate resolution to their inquiries. With chatbots and UiPath, we can go one step further in fulfilling the requests of customers—not only can we respond to customer inquiries using intelligent chatbots, but we can also launch automation processes to provide end-to-end (E2E) fulfillment of customer requests.

In this chapter, we will work together to build a chatbot that can interact with human users and launch UiPath automations. As we build the use case, we will follow a similar development life cycle as outlined in the previous chapters. We will start with understanding the current state of our automation opportunity, and then design the future state of the use case with automation involved. Once the future state is designed, we will build the chatbot with Druid and the automation with UiPath Studio.

In this chapter, *Use Case 3 – Chatbots with Druid*, we will cover the following topics:

- Understanding the current state of the use case
- Creating a future state design
- Building a solution with Druid
- Building an automation solution
- Testing to ensure stability and improve accuracy
- Considerations for production

Once complete, we will have built a chatbot with Druid with a connection to UiPath automation.

## Technical requirements

All code examples for this chapter can be found on GitHub at the following link:

```
https://github.com/PacktPublishing/Democratizing-Artificial-Intelligence-with-UiPath/tree/main/Chapter10.
```

Working with UiPath is very easy, and with the Community version, we can get started for free.

For this section, you will need the following:

- UiPath Enterprise Cloud
- UiPath Studio 2021.4+
- Druid Oxygen v1.57.0+

> **Important Note**
> Trial licenses for Druid can be acquired at https://www.druidai.com/license-activation.

Check out the following video to see the Code In Action at: https://bit.ly/3rt4C5x
