All Druid chatbots are based on this methodology, whereby a user initiates a conversation with a chatbot (an utterance). Based on that utterance, the chatbot best matches with an intent that then starts a dialog, which could contain free text, buttons, or other inputs.

With this understanding, we can design the flows of our chatbot. By default, a chatbot should have the following flows configured:

+   - **Welcome flow**: A flow to welcome the user once they initiate the chatbot
+   - **Fallback flow**: A flow for the chatbot to fall back on if the bot does not know the answer or response to a user's input

In addition to leveraging the Welcome and Fallback flows, we will need to add one additional flow for our automation. This flow will be called UiPath_Flow, where we will provide a set of buttons for the user to choose from. Depending on the button the user chooses, Druid will trigger an automation with an argument to either reset their password or to upgrade their application license.

That concludes our future state design. In this section, we discussed chatbot basics, created a diagram to resemble our future state design, and introduced the three flows we will build in our chatbot. In the next section, we will get started with development by building the Druid chatbot.

# Building a solution with Druid

As we venture into building our defined use case, we'll start with building our chatbot component with Druid. For this use case, we'll start out with Druid's **Druid Starter** template to set the foundation of our chatbot. This template comes with a definition of system entities and common flows that every chatbot should leverage.