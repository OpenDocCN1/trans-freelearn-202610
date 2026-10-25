The response from the web service is also captured by Wireshark. The following is a screenshot of the response:

![](bbox=[0.125, 0.15, 0.875, 0.59])

# Afterword

Web service integration is just as easy as the database integration shown in the previous section. Again, DWR and the user interface do not know the implementation of the remoted Java classes, so the web service integration is very transparent to users.

# Integrating with a Messaging System

The idea behind the messaging example is that our application will send an order form to the backend system using JMS. The backend system here is a Python script that accesses the JMS messaging server, ActiveMQ.