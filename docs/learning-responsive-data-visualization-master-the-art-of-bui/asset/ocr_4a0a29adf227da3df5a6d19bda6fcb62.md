The preceding example works better because a new transition gets started whenever an old one has stopped. However, the nested callback tree looks really ugly and bears potential errors for your productive code.

One step towards fully chainable transitions is to add a little extension to D3 that allows Promised-based state transitions. Promises are asynchronous representations for future values and have a very nice property – they can be chained together in a flat structure instead of a nested tree structure.

Here is the helper function for Promised-based transitions, which uses an ES6 Promise polyfill to work in all browsers (you can read more about polyfills and their purpose in the last chapter of this book):

```
function add$transition(selection) {
    selection.$transition = function(animationFn) {
        var getPromiseTransition = function() {
            return new Promise(function(resolve, reject) {
                selection.transition()
                .call(animationFn)
                .each("end", function() {
                    resolve(selection);
                });
            });
        });

        if (selection.$progress == undefined) {
            selection.$progress = getPromiseTransition();
        }
        else {
            selection.$progress = selection.$progress
            .then(function() {
                return getPromiseTransition();
            });
        }

        return selection;
    }
}
```

Let's go through the preceding code. First, we add a new .transition(animationFn) method to the D3 selection, which allows us to create Promise-based async transitions. Next, we jump to the line where we check if there already exists a $progress property on the selection; this property will always store the most recent Promise. If it doesn't exist, we initialize it with a new transition Promise; otherwise, we return a new transition Promise from the .then() method, which gets executed after the Promise is resolved. Then, we override the $progress property with the result that will be returned from the .then() method, which is the most recent Promise.