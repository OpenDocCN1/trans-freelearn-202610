If the `found` flag still has the value `false`, this means that no element was propagated. If we run this test we will see the following message in the console, showing that our test passed:

![](bbox=[0.17, 0.17, 0.86, 0.31])

Let's add one more test using an observable containing some values:

```
```js
it('should return propagates 2,4 when running for an observable containing 1,2,3,4', function(done) {
  Rx.Observable.of(1,2,3,4)
  .transduce(mapAndFilter)
  .toArray()
  .subscribe(
    function(value){
      assert.equal(2,value.length);
      assert.equal(2,value[0]);
      assert.equal(4,value[1]);
    },
    function noop(){},
    function(){
      done();
    }
  );
});
```
```

In this new test we add a test case using an observable containing the values 1, 2, 3, and 4, we then apply the transducer to it and the operator `toArray()` on the result observable to create a new observable that propagates only one array containing all the values from this observable. We know the propagated value must be the array [ 2, 4 ], and to check this we must add three assertions; the first ensures that we have an array with two elements, the second ensures that the first element is 2, and the third ensures that the second element is 4. We finally finish the test when the observable completes.