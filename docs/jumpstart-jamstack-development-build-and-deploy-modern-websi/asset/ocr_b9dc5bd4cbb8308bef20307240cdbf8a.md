The code for this chapter is available at `https://github.com/PacktPublishing/Jumpstart-Jamstack-Development/tree/chapter_twelve`.

# Alexa skill life cycle

A user activates a particular skill by saying the trigger word *Alexa* followed by the skill name and the intent (command) to be executed. For example, *Alexa, ask jumpstart jamstack to tell me my upcoming events.*

The Amazon Echo device will send the request to the Alexa console, which will then process the request by contacting our Sanity application and will send the results back to the Amazon Echo device, prompting a message to the user.

To build an Alexa skill, we will use the Alexa Developer console. The two components of an Alexa skill are the skill interface and the skill service.

# Skill interface

The **Skill interface** is the software provided by Amazon where the user can develop an Alexa skill through a nice and clean UI. We will use the skill interface to create and modify each component of the skill.

# Skill service

The **Skill service** is where the business logic of an Alexa skill resides. The code will execute the intent and return a JSON response to the interface that will give the user an understandable human sentence as a response.

In this chapter, our service will be executed in Node.js inside an AWS Lambda function.

Now that we know what the skill interface and skill service are, we will move on to creating the skill for Alexa.

# Creating the skill

As mentioned earlier, we are going to create a skill through the **Alexa Developer Console**, which will retrieve five upcoming events from our event website.