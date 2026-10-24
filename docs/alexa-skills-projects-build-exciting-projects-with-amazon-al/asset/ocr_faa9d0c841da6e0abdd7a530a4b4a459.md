With the current count of **Games/Trivia**-oriented skills in the Alexa Skills store being around 6,000, there is a lot more room to accommodate other **Games/Trivia** skills, which is one of the major reasons that we decided to introduce our readers to the Factly Skill in this chapter.

# # Designing the Factly Skill

We hope that the discussion in the last section should have given you a fair idea of the motivation behind the Factly Alexa Skill. Now, we shall level up and design the actual Factly Skill. It is always better to design before coding anything because it gives us a clarity about the end goals of the software and the finer details, which we may otherwise miss when we actually code the software.

The Factly Alexa Skill is a simple quiz skill. At the code level, the Factly Skill/Lambda will contain a bank of questions that will be presented to the user one by one. The skill will further perform depending on the user's reply.

Factly will work via the following steps:

+   1. The user will launch the Factly Skill via a standard Alexa Skill launch utterance.
+   2. Alexa will confirm the launch and provide a brief introduction to the skill and play method:

![](bbox=[0.308, 0.565, 0.692, 0.725])

Figure 4.7: Step 1 – Factly, launch

The skill that we will design will consist of three trivia questions and each question will have four answer choices, with only a single choice being the correct one.