

When the **urgent** label is added to a card, post the `@board this card needs urgent attention!` comment.

This is a quick way to tag everyone on the board and make them aware of something that’s urgent. Selecting a label, moving a card, or even clicking a button is much faster than typing a comment out, and it’s definitely faster than copying a card link and posting a Slack message!

## 向 __ 发送电子邮件通知

Even though sending a comment will trigger an email notification, if you only want the email and don’t need the content documented on the card, or if you just want to make sure everyone sees regardless of their notification settings, you can send an email notification with this action. You can trigger it to send it to a specific email address (regardless of whether they’re a Trello user or not) or send an email to everyone on the card or everyone on the board.

You can then specify the content of the subject and message. This is where knowing **automation variables** is most handy because that’s how you can reference the specific card and details about it inside the body of the email.

Real-world example

When a new card is added to the board, send an email notification to every member of the board with the subject **New card : {cardname}** and the message **A new card has been added to the board! View it here {cardlink}**

If you have trouble getting people on your team to check Trello, try using this action to send email notifications. You can even do this for cards people are not subscribed to, which is a perfect way to help more people get on board—literally.

## Get/post/put to URL

If you have an API endpoint that can receive and store data, then you can use this rule to get data from that API or send data to it. This is pretty advanced stuff, but if you want to integrate with another tool, this is likely what you’ll want to use.

You’ll need to be familiar with the API you’re working with and what’s returned to be able to best use this, but essentially you can reference whatever is returned in your next step. You can learn more about HTTP requests with automation in the official docs here: support.atlassian.com/trello/docs/issuing-http-requests/.

Real-world example

When a card is moved into the **Get data** list, get the URL https://api.thecatapi.com/v1/images/search?breed_ids=beng&include_breeds=true and post {httpresponse.image} as a comment on the card.

_You know, in case you want to put images of cats as comments on your cards. Who doesn’t?_

## Summary

We’ve covered so much ground, and these are just the “basic” actions you can automate with Trello. You can now move cards around, add or remove attributes such as labels or links, adjust due dates, create checklists, add members, and more.

You’ll never remember all of these off the top of your head, and that’s totally okay. Even I have to open up the **Automation** section sometimes to remember everything that is possible. At first, it’s also not always intuitive which sections you’ll find the actions in, but the more you use them, the more familiar they’ll become.

But if you thought that was all, we’re only getting started. You can still automate even more actions in Trello, so in the next chapter, we’ll talk through some advanced automation actions.

## Advanced Actions

We’ve already covered a lot of ground and you are hopefully feeling extremely inspired by how much you can do in Trello. But for some workflows, you’ll need to use even more than basic automation actions. For instance, maybe you can’t find an action that suits what you need for counting how many cards have been through a workflow, or you need to find a similar card and make an update on _that_ card.

In this chapter, you’ll learn about more advanced actions such as the following:

- Clearing and setting custom field values
- Sorting lists by attributes such as dates, labels, or fields
- Finding one or more cards and performing actions
- Sending Slack messages with Trello

Now, grab some coffee because there’s a lot to go through here!

## Field actions

If you want to do any automation of updating or clearing custom fields, this is your section!

Custom fields only available for Trello Standard or higher plans

If you aren’t using them, you can skip right past this section and come back one day when you need it. But if you are using custom fields (or if you aren’t but you’ve thought about it), you have a lot of options for automating those fields, so let’s dive in!

## Select an Action

Move Add/Remove Dates Checklists Members Content Fields Sort Cascade Jira Bitbucket Slack

clear custom field Field name +

set custom field Field name to New value +

check custom field Field name +

increase the number in custom field Field name by 1 +

set date custom field Field name to now +

move the date in custom field Field name to the same day next week +

Unlike setting the date, moving it respects the existing time (e.g. move an appointment to another day).

### Figure 8.1 – Field actions

In [Chapter 3], we learned about how powerful custom fields were, and we didn’t even talk about automation! While they’re pretty nifty just sitting around on cards, the real potency comes from being able to tie automations to them.

If you think your use case is so unique that Trello probably can’t handle it, then I’d say you probably haven’t used custom fields to their full potential. Although we’ve discussed a lot of actions for dates, members, checklists, and more, custom field actions allow you to do almost everything else. So, if you were disappointed to not find something in the previous rules, then buckle up because you can find a way to make it work with custom fields. You might just have to get a bit creative!

## Clear custom field __

Pick a custom field and clear it—great for when you need to reset a card.

Real-world example

When a card is moved to **Backlog**, clear custom field **Prioritized**.

Clearing custom fields is helpful for resetting a card, such as when it’s deprioritized.

## Set custom field __ to __

Use this to update a custom field to a specific value. You can make it a single, static value that will apply every time. Or you can make it dynamic by using **automation variables**. For instance, you can set a custom field to be the current date, the current list name, or anything else!

Real-world example

When a card is added to the board, set custom field **Type** to `{cardlistname}`.

This is a great rule for keeping track of where a card originated. I use this in my recipe board to keep track of the type of meal a recipe falls under (like Meatless, Quick, or New). I’ll queue up meals to make in my **Meal Plan** list, and then when I’m done, I use another automation rule to move the card back to its meal type list, by referencing the value in that `Type` custom field.

## Check/uncheck custom field

If you’re using a checkbox custom field, this will allow you to set it to “true” or “false” (checked or unchecked).

Real-world example

When a card is moved to **Ready for Review** list, check the custom field **Drafted**.

Some people prefer using custom fields instead of checklists, which is understandable as they are at the top of the card, and for a while, there were more options for automating custom fields than checklist items.

## Increase/decrease the number in custom field __ by __

If you have a number type custom field, you can use this rule to adjust that number by specific increments. This is useful if you want to count the number of times something happens, as every time that thing happens, you can increase the number of a card by one.

### Real-world example

When a comment is added to a card, increase the number in custom field **Comments** number by 1.

This rule is specifically helpful if you want to keep a log of the number of comments on a card. Perhaps your team could agree that if a card reaches 20 comments, it’s time to have a quick sync to discuss to make a decision together. This automation would make it easy to quickly count comments, and you could even add another automation that notified you when that custom field reaches 20 (or whatever number you set).

## Set date custom field __ to __

This is the automation for date fields. You can set it to a variety of options, very similar to what you’ve seen in the **Dates** automation section.

### Real-world example

When a card is moved to **Done**, set the date for **Completed Date** to **Now**.

While you could set the due date, using a custom field for tracking the completed date allows you to see actual versus estimated dates.

## Move the date in custom field __ to __

