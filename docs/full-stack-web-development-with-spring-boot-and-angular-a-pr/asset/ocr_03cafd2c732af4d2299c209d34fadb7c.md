The workflow also has two jobs: test and deploy. The test job’s steps are to check out the code, build the application using Java 17, and run the test. On the other hand, the deploy job’s steps are to check out the code and use the Heroku deploy action, which requires a Heroku API key, the application’s name, and the Heroku account’s email.

+   3. For the Heroku API key, you need to go to the Account settings menu of your profile on your Heroku dashboard:

![](bbox=[0.37, 0.245, 0.645, 0.525])

Figure 17.11 shows the Account settings menu under my profile on the Heroku dashboard.

+   4. Click Account settings, then go to API Key and generate an API key:

![](bbox=[0.158, 0.656, 0.857, 0.717])

Figure 17.12 shows where you can generate a Heroku API key.

Copy the Heroku API key since you will need to create a new Actions secret for GitHub Actions.