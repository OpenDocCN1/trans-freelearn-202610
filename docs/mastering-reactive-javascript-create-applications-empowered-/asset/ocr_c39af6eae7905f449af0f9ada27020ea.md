Let's see how we can use an observable to test a transducer. To do so, let's first create a transducer to map a number to it plus 1 and then filter to keep only even numbers; this is the same transducer we already use in previous examples, so there is no need to explain each step:

```
var mapAndFilter = t.comp(
    t.map((i)=>i+1),
    t.filter((i)=>i%2===0)
);
```

Now we can create a unit test for it using an observable:

```
var assert = require('assert');

describe('Transducer test', function() {
    it('should return empty when running for empty observables',
    function(done) {
        var found = false;
        Rx.Observable.empty()
            .transduce(mapAndFilter)
            .subscribe(
                function(){
                    found = true;
                },
                function noop(){},
                function(){
                    assert.equal(false,found);
                    done();
                }
            );
        });
    });
```

So far, we have only one test; this test checks if the transducer returns empty if the source of data is empty. To implement this, we first create a flag variable called found; this flag will be turned to true is we propagate any content in this observable. We add an empty() function to our error listener, and we check if our flag is still false and then finish the test when we complete the observable.