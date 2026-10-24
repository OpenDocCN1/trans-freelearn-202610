3. By default, you are given the space to specify two choices, but you can include additional choices by clicking **Add another choice**. For our Good Eatin' site, we are allowing visitors to select between Breakfast, Lunch, Dinner, and Desert.

4. The **Poll status** controls whether or not visitors can enter new votes into the system. If you want to manually manage the duration of the poll, you can simply close the poll by editing the poll and setting the **Poll status** to Closed when the poll is over.

5. The **Poll duration** can be used to automatically close a poll after a specified interval. The poll can be kept open for various intervals from 1 day to 1 year.

6. When you click **Save**, Drupal will add the poll to the web site, and allow you to vote in it, as shown in the following screenshot:

![](bbox=[0.264, 0.416, 0.793, 0.679])

7. The final step to setting up the poll is making sure that the users can submit answers to the poll. This is controlled via the **Permissions Manager**, which is accessed by selecting **User management** and then **Permission**, from the **Administer** menu.

8. There are several permissions available to control access to the polls, as shown in following screenshot:

| Permission | anonymous user | authenticated user | VIP user |
|---|---:|---:|---:|
| **poll module** |  |  |  |
| cancel own vote | ☑ | ☑ | ☑ |
| create poll content | ☐ | ☐ | ☐ |
| delete any poll content | ☐ | ☐ | ☐ |
| delete own poll content | ☐ | ☐ | ☐ |
| edit any poll content | ☐ | ☐ | ☐ |
| edit own poll content | ☐ | ☐ | ☐ |
| inspect all votes | ☐ | ☐ | ☐ |
| vote on polls | ☑ | ☑ | ☑ |