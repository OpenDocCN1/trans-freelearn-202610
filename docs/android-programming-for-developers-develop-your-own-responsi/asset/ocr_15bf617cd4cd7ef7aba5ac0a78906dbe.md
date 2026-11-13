```
public void nested(View v){
    Log.i("message:","In nested method");

    // a nested for loop
    for(int i = 0; i < 3; i ++){

        for(int j = 3; j > 0; j --){

            // Output the values of i and j
            Log.i("i = " + i,"j=" + j);
        }
    }
}
```

We will be able to call this method we have just written, from the appropriately labeled button.

Now, let's run the app and start tapping buttons. If you tap each of the buttons once from top to bottom, this is the console output you will see:

```
message:: In countUp method
x =: 1
x =: 2
x =: 3
message:: In countDown method
x =: 3
x =: 2
x =: 1
message:: In nested method
i =0: j=3
i =0: j=2
i =0: j=1
i =1: j=3
i =1: j=2
i =1: j=1
i =2: j=3
i =2: j=2
i =2: j=1
```

We can see that the countUp method does exactly that. The int x variable is initialized to zero, an infinite while loop is entered, and x is incremented with the increment ++ operator. Fortunately, on each iteration of the loop, we test for x being equal to 3 with if (x == 3) and break when this is true.