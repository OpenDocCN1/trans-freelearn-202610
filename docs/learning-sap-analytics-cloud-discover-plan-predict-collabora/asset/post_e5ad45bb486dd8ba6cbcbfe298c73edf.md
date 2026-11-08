
left side of the screen.

By default, the timeline shows all the events and tasks for the next ten days or less. You can modify both the number of days and the specific categories that you want to see in the timeline.

- 23. In the Timeline view, click on the Preferences icon.
- 24. Set the number of days to show from the Number of Days From Today list (for example, 5).
- 25. Select the categories that you want to see, for example, Financial Planning.

Click on OK to apply your settings. The timeline view displays only the selected categories that fall within the specified number of days. Note that these steps will yield the result only when you have multiple events in different categories, spanning different timelines.

![](img/c69568baeee565fe02397867c05883e1_315_0.png)

If you click on the task entry (A) in the Calendar or Gantt view, you see a section labeled Assignees & Status (B). This section displays the name of the assignee (C) and the status of the task, which should currently be displaying Pending (D). In the next section, we will see how the assignee views the task and event assigned to him.

![](img/c69568baeee565fe02397867c05883e1_316_0.png)

When the assignee logs in, the notifications area (A) alerts him that he has a couple of messages about the event and task assigned to him. Clicking on the notifications icon displays the messages. When he clicks on the event notification, the event screen pops up, where he can see the event’s details. Among other details, he can find the task assigned to him in the Related Tasks section, which requires him to Update Q1 Data for West US in the Bocage model, as shown in the following screenshot:

![](img/c69568baeee565fe02397867c05883e1_316_1.png)

When he opens the task by clicking on the link in the Related Tasks section or by selecting it in the Calendar or Gantt views, he gets the following information in the Details panel. The two buttons at the bottom of this panel allow him to accept the task or decline it. The link in the Related Content area shows the name of the story he needs to update. After completing the assigned task, he clicks on the Accept Task button in this panel, followed by the Done button.

![](img/c69568baeee565fe02397867c05883e1_317_0.png)

When the event and task creator logs in, he gets three notifications, as illustrated in the following screenshot. These messages inform him that the task was accepted and then completed by the assignee.

![](img/c69568baeee565fe02397867c05883e1_317_1.png)

The completed event and the corresponding task are now presented in a dark color, and the statuses of both the event and the task are also marked as completed in the Details panel:

![](img/c69568baeee565fe02397867c05883e1_318_0.png)

## Events toolbar

You have already utilized some of the options available in the toolbar. Here are the briefs of the remaining options:

![](img/c69568baeee565fe02397867c05883e1_319_0.png)

**Export Event (A)**: When you select an event in the Calendar or Gantt view and click on this toolbar option, the event is exported as a `.ics` file. A file with the ICS file extension is an `iCalendar` file. It is a plain text file that includes calendar event details. In the current scenario, the file contains event details as illustrated in the following screenshot. You can open this file in Microsoft Outlook, Windows Live Mail, and IBM Notes, as well as in calendar programs, such as Google Calendar for web browsers, Yahoo! Calendar, Apple Calendar for iOS mobile devices and Macs, Mozilla Lightning Calendar, and VueMinder.

```
Update Q1 Data, event from SAP BusinessObjects Cloud
• Link - https://eu1.sapbusinessobjects.cloud/sap/fpa/ui/#:view_id=em;eventId=E85BFF58EF
• Description -
• assignees
  • Admin
• Tasks
  • Update Q1 data for West US in the Bocage model
    • Description -
    • Duedate - 2017-04-29
    • Reminder - reminder 3 days before
```

- **Edit Event/Maintain Task (B)**: This toolbar option can be used to modify the details of an event and the task as well. When you select an event in the Calendar or Gantt view and click on this icon, the Edit Event dialog box appears. When you select a task and click on this option, you see the Edit Task dialog box.
- **Delete Event/Task (D)**: Similar to the edit option, this one also toggles to delete the selected event or task.
- **Edit Reminders (E)**: This option is used to modify the reminders you have set for your events and tasks.
- **Pin to Home (F)**: You can pin your events and tasks to your Home screen using this option. For further details, refer to the *Pinning Story Objects* section in Chapter 7, *Working*

## Sharing a story with colleagues and granting access privileges

After completing a story, you save it and share it with other users. Sharing a story is a part of collaboration. The main purpose of sharing a story is to request others to provide their feedback, and based on their input further refine the story. You went through the saving process at the end of Chapter 5, *Extending Stories with KPI, Filters and other Handy Objects* in the *Save Your Story* section. To remind you, there are three options to save and share your work with colleagues:

- **Public**: You can share your stories by saving them to the `Public` folder, which makes them available to everyone
- **Teams**: This option grants the access of your story to specified teams
- **Private**: If you select the Private option, your story will be available only to users with whom you share it

You have a number of options to share your story with others. Here's the simplest route:

- 26. In your story, click on the Share icon, and select Share Story (A) from the menu:

![](img/c69568baeee565fe02397867c05883e1_321_0.png)

The Share Story dialog box pops up. You can send the link (B) in this dialog box to your colleagues to share the story with them.

- 27. To share the story with specific users or teams, click on the Add Usernames icon, which renders the Select Users and Teams dialog box from where you can select teams (C) or individual users (D).
- 28. From the Access list, you can provide the Read, Update, or Full Control rights to the selected people. For this example, provide the Read access right.

![](img/c69568baeee565fe02397867c05883e1_322_0.png)

When the other users log in, they get a notification (E) about the share. Clicking on the notification opens up the story with limited access, in which they can see and utilize a subset of options in the main toolbar (F). The available options allow them to save the story with a new name, apply filters, and create their own formulas.

![](img/c69568baeee565fe02397867c05883e1_322_1.png)

The other option in the Share menu is Sharing Settings. When you click on this option, you see the Edit Sharing Settings dialog box, as illustrated in the following screenshot. Here, you can adjust your story access privileges according to the users. As you can see in the screenshot, the dialog box allows you to grant full, read, update, and delete access to the selected members and groups. You can use different combinations for different people. For example, you can provide Update Access to some members so they can update the story, if required, and grant Read Access to others to prevent story updating. You can also add more users and teams using the Add Users and Teams button.

### Edit Sharing Settings

| User/Team | Full Access | Read Access | Update Access | Delete Access |
|---|---|---|---|---|
| All Users | ☐ | ☑ | ☐ | ☐ |

Add Users and Teams

Save

Cancel

## Collaborating via discussions

You can collaborate with colleagues using the discussions feature of SAP Analytics Cloud. The discussions feature enables you to connect with other members in real time. Here are the steps to utilize this feature:

- 29. Click on the Collaboration icon on the main toolbar to access the Discussions panel.
- 30. Click on the New Discussion icon. Use the left arrow if you want to switch back.
- 31. In New Discussion, click on the + icon to add participants
- 32. In the Select Users dialog box, select the users you want to add to the discussion.

![](img/c69568baeee565fe02397867c05883e1_324_0.png)

After inviting the participants, you can begin the discussion by clicking on the New icon (A) at the bottom of the Discussions panel. The button provides you with some collaborating options. If you choose the Link Story option (B), your story is added to the discussion panel. The New Task option (C) invokes the corresponding window you went through earlier in this chapter to create a new task. You can also send a text message by typing a message (D) and clicking on the send icon (E). The messages you send to the participants are also added to the discussion panel (F).

Clicking on the Manage icon (G) delivers a menu (H), which allows you to manage related content, manage users, archive the discussion for later reference, and delete the discussion. If required, you can access the archived discussion using the Discussions panel menu (I). The participants receive alerts about the discussion in the notification area. They can join the discussion by clicking on the relevant message in the notification area. Clicking on the collaboration icon shows a similar discussions screen, as illustrated in the following screenshot, from where they can access and respond to the shared content:
