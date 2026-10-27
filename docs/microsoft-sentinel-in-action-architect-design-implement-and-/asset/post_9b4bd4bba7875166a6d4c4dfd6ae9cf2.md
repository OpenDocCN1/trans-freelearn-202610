
Let's now look at the different fields under the context menu in detail:

- If you select **Create new incident**, the new incident blade will open, as follows.
- This will allow you to create a new incident based on the selected bookmark:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_358_0.png)

Figure 11.18 – Creating an incident from bookmark

These fields are the same as any other incident, so they will not be covered here. Refer to Chapter 9, Incident Management, for more information on incidents. Click Create to create a new incident from this bookmark.

- Clicking on **Add to existing incident** will open a new blade, allowing you to choose one or more incidents with which to associate this bookmark, as shown in the following screenshot. Select the incident with which you wish to associate this bookmark, and then click Add:

## Using Microsoft Sentinel notebooks

Sometimes, just using KQL queries against logs does not give enough information to assist with properly performing hunting activities. In cases such as this, you can use the Jupyter notebook, hosted in the Microsoft Azure Machine Learning (AML) service, to perform additional work. The Jupyter Notebook combines text with code to provide an overall view of your threat-hunting activities. The code can be written in Python, PowerShell, and other languages, so threat hunters can work with a language they are most likely already familiar with.

> **Important Note**
> The full scope of the Jupyter Notebook is beyond the scope of this book. For more information, go to https://jupyter.org/.

Click on Notebooks in the Microsoft Sentinel navigation area to go to the Notebooks page, which will look like the following screenshot:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_360_0.png)

Figure 11.20 – Notebooks page overview

Each of the areas on this page is described in more detail in the following sections.

## The header bar

The header bar allows you to work with the notebooks and is shown in the following screenshot:

For each notebook, the name of the notebook will be shown with the authoring company underneath it. The last update time, with the type of notebook underneath it, will be shown on the right. There are currently four different options for type: Getting started, Configuration, Hunting, and Investigation. More may be added later.

## The notebook details pane

Selecting a notebook will show the notebook details blade, as shown in the following screenshot. This will show the notebook's name at the top of the page. Under that is the name of who created it and the last time it was updated.

Beneath that is the description of the notebook, which will tell you what the notebook is trying to find. Under that is the Required data types field, which refers to the Microsoft Sentinel logs that it will be querying. Below that is a listing of the data sources (not shown) that are used to populate the data types and, finally, one or more images pertaining to the notebooks:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_362_0.png)

Figure 11.23 – Notebook details pane

At the bottom of this blade is the **Save notebook** button. This will save the selected notebook. You have the option to overwrite an existing notebook if you have saved this template already. If you do not choose to overwrite an existing notebook and one is present, then nothing will happen.

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_363_0.png)

Figure 11.24 – Saving a notebook template

After you click on the **Save notebook** button, unless you cancel the action, it will change to **Launch notebook**. If you click the button now, the notebook will then be opened in the AML workspace. If you select a different notebook and return to this one, the button will revert to **Save notebook**.

> **Important Note**
> 
> If you do not have an Azure Machine Learning workspace created, there will be a message stating that when you try to save the notebook, you will need to create the workspace first. See the following *Creating a workspace* section ahead for more information.

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_365_0.png)

Figure 11.26 – Microsoft Azure Machine Learning page

From here, you can select your notebook and start working with it. The actual usage of the notebooks is outside the scope of this book.

Azure notebooks will allow you to perform queries outside Microsoft Sentinel. They can incorporate text and code and can include data internal and external to Microsoft Sentinel. They can also make use of graphics packages that are not available to Microsoft Sentinel to create graphs that you would not be able to create in Microsoft Sentinel.

Next, we will look at the mechanics of how to perform a threat hunt. This is going to focus on the tasks you will perform and not on how to do it inside Microsoft Sentinel.

As you can see from the preceding diagram, there are various steps to performing an investigation. Each step is described in further detail next.

## Developing a premise

In this step, you need to determine what it is you are trying to find or prove. What is it you are trying to find out? Is it to determine whether a malicious IP address has been found in your environment? Did a new user account perform actions it should not have? Is someone from a foreign entity trying to gain access to your system?

When you develop a premise, you are specifying what it is you are trying to find. It may be that you are told what it is you are looking for, or it may be that you are looking for something that you think may have happened. Utilize tools such as threat intelligence sources as premises to being your threat hunt.

Keep in mind that threat hunting is a gap measure to ensure coverage of newly reported threats until analytics rules are available.

## Determining data

In this step, you will determine which data you need to start your investigation. Which data will you need to look at to work your hypotheses? Is it all in Log Analytics, or will it be found elsewhere? What do you already know about your environment and what do you need to learn?

It may be that during your investigation, you determine that there is additional information that you may need, so do not feel that you are locked into just the data you have acquired in this step.

You may also find that you do not have all the data needed to work on your premise. It may be that there is data you need that is not being logged, in which case you will need to see whether you can start gathering that information. In any event, if you cannot obtain the needed data, you may need to go back and revise your premise.

Once you start finding specific types of activities in the various logs you have, it will make sense to keep track of these to make it easier to find the data in the future. The following screenshot shows the beginning of one method to track information: using Microsoft Excel to keep track of the log name, the type of data being queried, and whether the data is found in the log. If the data is found, the name of the column holding the data is entered:

## Executing an investigation

In this step, you will execute the queries that you obtained in the previous step, whether you wrote them or found ones that were already written that will work. You may be performing queries in Microsoft Sentinel as well as, or instead of, in a notebook.
Wherever you run the queries, once you start looking at the results, you may find that you need to revise your premise or determine that you need more data, so you may need to go back to a previous step (refer to Figure 10.24).

## Responding

Now that you have the information you need, you can respond to the results of the investigation. You may just have to plug the security gap you found, or you may need to escalate to another team and present what you found. This may be just your team, or in the event of a major breach, it could be the Chief Information Security Officer (CISO), Chief Information Officer (CIO), or even the board.

If the response is simple, such as blocking a port in a firewall or blocking an IP address range, most likely it is just a matter of notifying the appropriate people, updating the ticket, and moving on.

However, there will most likely be cases where you need to do a full presentation on your findings. This could be just to your team, but it could also be to a different audience. How you do the presentation is up to your company and its corporate culture; just keep thinking while you are doing your work, Would this information be useful in a presentation?, and think about how you would present it. A table of information may be enough when presenting to your team, but others who are not as familiar with the data may need graphs and charts.

At the very least, your findings and how you got to your results should be presented within your team if it is new information, so that everyone is kept up to date with the latest findings and can take the most appropriate action.

## Monitoring

Develop new analytic queries if possible; otherwise, continue to periodically run the investigation to validate findings. Refer to Chapter 7, Creating Analytic Rules, for a refresher on creating analytic queries.
