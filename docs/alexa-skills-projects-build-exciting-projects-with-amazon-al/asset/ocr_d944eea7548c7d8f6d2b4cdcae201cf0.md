5. The getWelcomeResponse method is responsible for formulating the launch response and posing the first question to the user:

```
var ANSWER_COUNT = 4;
var GAME_LENGTH = 3;

function getWelcomeResponse(callback) {
    var sessionAttributes = {},
        speechOutput = "Factly will ask you " +
GAME_LENGTH.toString()
            + " questions, try to get as many right as you can. Just say the number of the answer. Let's begin. ",
        shouldEndSession = false,

        gameQuestions = populateGameQuestions(),
        currentQuestionIndex = 0,
        correctAnswerIndex =
correctAnswerSlots[currentQuestionIndex],
        answers = populateAnswers(gameQuestions, 0),

        spokenQuestion =
Object.keys(questions[gameQuestions[currentQuestionIndex]])[0],
        repromptText = "Question 1. " + spokenQuestion + " ",

        i;

    for (i = 0; i < ANSWER_COUNT; i++) {
        repromptText += (i+1).toString() + ". " + answers[i] +
". "
    }
    speechOutput += repromptText;
    sessionAttributes = {
        "speechOutput": repromptText,
        "repromptText": repromptText,
        "currentQuestionIndex": currentQuestionIndex,
        "correctAnswerIndex": correctAnswerIndex + 1,
        "questions": gameQuestions,
        "score": 0
    };
    callback(sessionAttributes,
        buildSpeechletResponse(speechOutput, repromptText,
shouldEndSession));
}
```