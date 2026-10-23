# Slot types

Slots can see a variable passed via an utterance. By default, Amazon provides many slot types, such as number, city, and country, that are recognized by Alexa. You can also create a custom slot type and process it at your convenience.

An example of a slot type can be found in a weather skill when you enquire about the weather in a particular city.

In your WeatherByCity intents, you don't need to add an utterance for each city in the world, and you can pass it as a variable:

# Slot Types

![](bbox=[0.138, 0.322, 0.865, 0.494])

Figure 12.18 – Alexa skill Slot Types page

# Endpoint

**Endpoint** will receive the JSON payload request. We can leave the default here as we are using an AWS Lambda function.