# Adding NServiceBus to MVC

We will next be extending our `BasicPaymentClient` folder and projects into a `SagaPaymentClient` folder and projects with the addition of the bus. The differences will be as follows:

+   - We will refactor the WCF client out of the MVC controller and move it into a new message handler.
+   - We will create a database that keeps track of the state of the message. The saga data will be saved in the `nservicebus` table. Ensure that it is created when running the saga code.
+   - We will create a new message handler and put the WCF client code in it. This code from the WCF client to the WCF server will be kept separate from the frontend code as far as possible, to keep the `PayMessage` class completely separate from the frontend.
+   - We will create a new message handler in the MVC that will update the DB with the message state as it receives the new state from the endpoints. We will look at the code for the message and saga handlers as we test them in the next section.
+   - `NServiceBus.Testing` offers testing by sending messages through message handlers and sagas. This includes anything that a message handler and saga can do, including header manipulation and dependency injection. Refer to http://docs.particular.net/NServiceBus/unit-testing for some basic examples. For the source code of `NServiceBus.Testing`, go to https://github.com/Particular/NServiceBus/tree/develop/src/.

The very basis of starting unit testing is to create a unit testing project in Visual Studio by adding a new unit testing project to an existing solution. Visit http://msdn.microsoft.com/en-us/library/hh598957.aspx and hyperlink it. The **Add New Project** window is as shown here: