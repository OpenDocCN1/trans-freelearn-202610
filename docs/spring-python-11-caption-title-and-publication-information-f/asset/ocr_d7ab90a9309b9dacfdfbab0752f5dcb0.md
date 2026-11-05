Here we have slightly altered index, so that it prints our 'welcome' but also offers a hyperlink to logout. We have also added a /login link that either attempts to log the user in, or displays an HTML form so the user can attempt to login.

+   4. With these changes in place, let's restart the app, and open up the browser.

![](bbox=[0.21, 0.24, 0.79, 0.43])

As you can see, we are now looking at a very simple login screen. If we look at the shell in which our updated application is running, we can see what has happened.

![](bbox=[0.21, 0.51, 0.79, 0.71])

We pointed our browser at the root URL, but Spring Python's security filter noticed we had no credentials, so it redirected us to /login.