# Chapter 8 example

1. Email

Enter your answer

2. Customer Number

The value must be a number

3. Country of residence

Select your answer

Submit

Figure 8.21 – Setting up a form to gather information

8. At the top right of the screen is a Send button. This provides a sharable link similar to https://forms.office.com/r/ETQ900ed1q7T (yours will be different). Make a note of this link, as we’ll need it to trigger our Power Automate flow later.

Note that we’re not adding a date of birth or the other inputs to our decision service, as we can add these later. For the moment, we want to keep the example simple.

If you’ve used Forms before, you know that it will automatically capture the responses for you. We want to do something more dynamic, so let’s add a Power Automate flow to our form.

# Updating our Power Automate flow

The flow we are aiming to create, mimicking the customer service flow at the start of the chapter, is in Figure 8.22. This flow has the same steps as earlier, but mapped to Power Automate actions: