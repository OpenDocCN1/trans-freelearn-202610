+   - **Project managers**: Responsible for the entire team delivering the entire system on time, on spec, and on budget.

+   - **System or cloud administrators**: Responsible for the infrastructure to run the system in the development, test, and production phases. Often, they will be the first to flag issues that happen when the system is running *in real life*.

It is possible for several of these roles to be combined into smaller projects. Alternatively, larger organizations may have teams that split these roles. Either way, your role as a business knowledge expert is crucial to the entire team and a good business expert is invaluable to delivering a system that allows the organization to run effectively.

# What software architects do

Just like architects design houses, enterprise software architects design enterprise software. Both are great jobs, with many highly skilled and very opinionated individuals. Some software architects may be upset by the one-size-fits-all enterprise solution I have presented in the following diagram.

![](bbox=[0.3, 0.44, 0.7, 0.65])

Figure 2.2 – One-size-fits-all enterprise software diagram

*Figure 2.2* is simple but shows what most systems do: take information from a user, process it in some way, then store it somewhere. Splitting the design into three layers like this makes it easier to understand what each layer does:

+   - The **user interface layer** communicates to and from a *real* person. Normally, this is a web page. But if we wanted to change to a voice interface (such as Amazon Alexa) or a 3D avatar, we would only have to change this layer.

+   - The **business logic layer** is where we do something with the user information. It might use the dates selected to suggest flights, update your bank balance, or carry out any other process you need to run the business.