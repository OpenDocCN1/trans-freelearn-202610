# # A random diversion

Sometimes in our apps, we will want a random number. There are many possible uses for random numbers, for example, when our app wants to show a random tip of the day, a game that has to choose between scenarios, or a quiz that asks random questions.

The Random class is part of the Java API and is fully compatible with our Android apps.

Let's have a look at how we can create random numbers, and later in the chapter, we will put it to practical use. All the hard work is done by the Random class. First, we need to create an object of the Random type:

```
Random randGenerator = new Random();
```

Then, we use our new object's nextInt method to generate a random number between a certain range.

This line of code generates a random number using our Random object and stores the result in the ourRandomNumber variable:

```
int ourRandomNumber = randGenerator.nextInt(10);
```

The number that we enter for the range starts from 0. So, the preceding line will generate a random number between 0 and 9. If we want a random number between 1 and 10, we just use this:

```
ourRandomNumber ++;
```

We can also use the Random object to get other types of random numbers using nextLong, nextFloat, and nextDouble.

We will put the Random class to practical use later in the chapter with a quick geography quiz app.