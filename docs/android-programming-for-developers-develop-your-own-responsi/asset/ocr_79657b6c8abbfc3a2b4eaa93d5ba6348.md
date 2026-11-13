logical NOT, like this:

```
if (!wavingWhiteFlag){
  // not surrendering so check everything else
  if(isComingOverTheBridge && friendlyTroops >= enemyTroops){
    // shoot them
  }else if(isComingOverTheBridge && friendlyTroops < enemyTroops) {
    // blow the bridge
  }
}else{
  // this is the else for our first if
  // Take prisoners
}
// Holding position
```

This demonstrates that we can nest if and else statements inside of one another to create quite deep and detailed decisions.

We could go on making more and more complicated decisions with if and else but what we have seen is more than sufficient as an introduction. It is probably worth pointing out that very often there is more than one way to arrive at a solution to a problem. The *right* way will usually be the way that solves the problem in the clearest and simplest manner.

Let's look at some other ways to make decisions in Java and then we can put them all together in an app.