We shall first see the simpler of the two, which is AMAZON.StopIntent, raised once the user decides to prematurely end the skill's execution by saying a phrase such as "Alexa, Stop Factly." This causes handleFinishSessionRequest to be called:

```
function handleFinishSessionRequest(intent, session, callback)
{
    callback(session.attributes,
        buildSpeechletResponse("Factly will Exit. Good bye!",
"", true));
}
```

This says a goodbye phrase to the user before the Factly Skill exits.

The Answer intent request is a significantly more complicated one, as it has multiple information flow paths and is handled by the handleAnswerRequest method:

```
function handleAnswerRequest(intent, session, callback) {
    var speechOutput = "";
    var sessionAttributes = {};
    var gameInProgress = session.attributes && session.attributes.questions;
    var answerSlotValid = isAnswerSlotValid(intent);
    var userGaveUp = intent.name === "DontKnowIntent";

    {
        var gameQuestions = session.attributes.questions,
            correctAnswerIndex = parseInt(session.attributes.correctAnswerIndex),
            currentScore = parseInt(session.attributes.score),
            currentQuestionIndex = parseInt(session.attributes.currentQuestionIndex),
            correctAnswerText = session.attributes.correctAnswerText;

        var speechOutputAnalysis = "";

        if (answerSlotValid && parseInt(intent.slots.Choice.value) == correctAnswerIndex) {
            currentScore++;
            speechOutputAnalysis = "correct. ";
        } else {
            if (!userGaveUp) {
                speechOutputAnalysis = "wrong. ";
            }
            speechOutputAnalysis += "The correct answer is . .
```