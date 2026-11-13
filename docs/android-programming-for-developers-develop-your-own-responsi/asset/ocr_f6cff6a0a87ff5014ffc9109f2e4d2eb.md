Now, we output the contents of the array using a for loop and our Random object. Note how we've ensured that although the question is random, we can always pick the correct answer. Add this next code after the previous code:

```
/*
Now we know that the country is stored at element 0
The matching capital at element 1
Here are two variables that reflect this
*/
int country = 0;
int capital = 1;

// A quick for loop to ask 3 questions
for(int i = 0; i < 3; i++){
    // get a random question number between 0 and 4
    questionNumber = randInt.nextInt(5);

    // and ask the question and in this case just
    // give the answer for the sake of brevity
    Log.i("info", "The capital of "
    +countriesAndCities[questionNumber][country]);

    Log.i("info", " is "+countriesAndCities[questionNumber][capital]);
} // end of for loop
```

Run the example remembering that nothing will happen on the screen, as all the output will be sent to our logcat console window in Android Studio. Here is the output:

```
info: The capital of USA
info: is Washington
info: The capital of India
info: is New Delhi
info: The capital of United Kingdom
info: is London
```

What just happened? Let's go through this chunk by chunk so we know exactly what is going on.

We make a new object of the Random type called randInt, as shown in the following code, that is ready to generate random numbers later in the program:

```
Random randInt = new Random();
```