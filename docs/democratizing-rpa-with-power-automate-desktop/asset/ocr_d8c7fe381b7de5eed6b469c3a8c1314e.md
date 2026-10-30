Figure 4.12 – The guessing game implementation

Here are the steps explained in detail:

+   1. The flow starts with the definition of two variables, Counter (a number, initialized with 0) and GuessRight (a Boolean value initialized with False, storing the status of the user’s guess), and a Generate random number action. To make it a bit easier for the user, a number between 1 and 20 will be generated and stored in the RandomNumber variable.

+   2. Next, the Loop condition action will be used. The first operand in the actions parameter dialog will be the GuessRight variable, which we want to check against the fixed value of False. This means as long as the value of GuessRight is false (the user did not enter the right number), the loop will continue.

+   3. Next, we want to display an input screen that allows the user to enter a number. We use the Display input dialog action for this, which is also located in the Message boxes actions group. We only have to enter a title and a message here. The input value will be stored in a variable called UserInput.