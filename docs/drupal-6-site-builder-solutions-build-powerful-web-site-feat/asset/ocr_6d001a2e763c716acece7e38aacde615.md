# Automatically moderating content based on keywords

Good Eatin' Goal: Add the automatic moderation of posts, based on the content entered within a post.

Additional modules needed: Actions (core), Trigger (core).

# Basic steps

An alternative to forcing all posts into moderation is to allow posts to be automatically published, but to force specific posts to be moderated if they do not meet your guidelines. This can help you cut down on your moderation time and allow most posts to be available immediately after they are written.

+   1. We will begin by creating a new action which will unpublish a post. Select site configuration and then Actions, from the Administer menu to open the Actions manager.

+   2. Select a new action of Unpublish post containing keyword(s) and click Create.

![](bbox=[0.21, 0.45, 0.68, 0.5])

Drupal will now display a form on which you can enter the keywords that a post should contain if it is to be held for moderation. Enter the words separated by commas. If a term that you want to filter on includes a comma, you must enclose the entire term in quotes.

![](bbox=[0.06, 0.6, 0.83, 0.9])