```
done();
}
)
});
```

Here we changed our filter() function, to return true for even numbers and false for odd numbers; we also store the element that we found in a variable and, as the only even number in this array is the number 2, we expect the counter to be equal to 1 and the element found to be equal to 2.

Now that we have our tests we are ready to implement our filter() function using only flatMap(); a good practice is to always fail your tests before trying the real implementation, so let's try the following implementation for our filter() function:

```
var Rx = require('rx');
module.exports = function(observable,filter){
return observable;
};
```

On our first attempt to implement the filter() function we are just returning the own observable, so some of our tests must fail. If we run our tests this is what happens:

![](bbox=[0.12, 0.45, 0.83, 0.82])