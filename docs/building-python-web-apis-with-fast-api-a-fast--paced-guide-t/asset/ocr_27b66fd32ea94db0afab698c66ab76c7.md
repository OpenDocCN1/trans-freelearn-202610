Let’s check the coverage report for routes/events.py. Click on it to display it.

![](bbox=[0.255, 0.11, 0.745, 0.52])

Figure 8.19 – Coverage report showing executed code in green and untouched code in red

# Summary

In this chapter, you have successfully tested the API by writing tests for the authentication routes and the CRUD route. You have learned what testing is and how to write tests with pytest, a fast testing library built for Python applications. You also learned what pytest fixtures are and used them in creating reusable access tokens and database objects, as well as preserving the application instance throughout the testing session. You were able to assert the responses of your API HTTP requests and verify the behavior of your API. Finally, you learned how to generate a coverage report for your tests and distinguish the blocks of code run during the testing session.

Now that you have been equipped with the knowledge of testing web APIs, you are ready to publish your application to the World Wide Web through a deployment channel. In the next and final chapter, you’ll learn how to containerize your application and deploy your locally using Docker and docker-compose.