This imports the dependencies required for the speech output. Using espeak from Python is also as easy as:

```
>>> espeak.synth("I am the weatherman")
```

The synth method in the espeak module accepts a string and produces speech based on that. It's not the best-sounding voice, but it works!

To add this to the weather station, we can add the following statement inside our while loop:

```
espeak.synth("The current temperature is : " + str(temp))
```

```
espeak.synth("Humidity level is : " + str(hum) + "percent")
```

This works because Python supports string concatenation. This means that we can combine two strings just by adding a + sign between them. However, the temp and hum variables are not string variables. So we need to convert them to strings by the str() method. The Str() method takes any type of variable and converts it into a string.

We can also use the espeak library to tell us many different things about our environment by getting the state from the Grove Sensors.

# Summary

In this chapter, we learned how to use Python to communicate with Twitter by using an open source library, Tweepy. We learned how to use Tweepy to send tweets. We also learned how to create a database, save and update data in it, and get some data from an existing database. Toward the end, we extended the capabilities of the weather station we built in Chapter 9, Grove Sensors and the Raspberry Pi and added tweeting and data-saving functionalities to it. We also added speech capabilities to it to make it easier to use.

In the next chapter, we will be exploring the interesting topic of parallel computing. We will be using multiple Raspberry Pis to create a cluster of computers that perform computations faster by working in tandem towards a single task. A master Pi will control other slave Pis and outsource its computations to them, so that the task is performed faster. We will use this setup to perform an n-body simulation, which is by itself a very computationally challenging task.