# Messaging

Given that our site's focus is to gather people together so that they can interact with each other, we need a way for them to interact. The primary way for people to interact with one another is through direct communication. We will build a system to perform this communication in a way that is similar to sending an email from a standard email client such as Outlook or Hotmail.

We will provide a way for our users to create and send messages via the Xinha WYSIWYG editor.

![](bbox=[0.282, 0.481, 0.72, 0.708])

Once a user can create and send messages, we will then create a way for other users to receive and read those messages. The emails will queue up in their inbox, and from there they will be able to read their messages.

Once we have this messaging subsystem in place we can hook up our other features in a way that they too can send messages — such as when a user accepts a friend request we can show the acceptance of that request in the friend's Inbox.