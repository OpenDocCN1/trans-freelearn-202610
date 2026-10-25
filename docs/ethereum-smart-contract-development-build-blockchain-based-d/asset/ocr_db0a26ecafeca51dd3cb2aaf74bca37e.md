```
pragma solidity ^0.4.11;

contract numberLoop {
    uint number; // unsigned integer is positive integer
    //constructor function with default value
    function numberLoop() public {
        number = 100;
    }
    function myFirstLoop() public returns (uint) {
        for (uint i = 1; i < 10; i++) {
            number = number + i;
        }
        return number;
    }
}
```

Figure 3.18: Debugging mode to identify states of execution

At this point, I hope you are quite comfortable playing with smart contracts using the remix IDE in your browser. However, what I have shown is just a happy-path scenario. The development community for Ethereum is highly agile and is rolling out newer changes very quickly. By the time you start reading this book, there might be slight or major modifications in the IDE. Once you start coding, you will realize the nuances and even capture many peculiar bugs down the line. The next section is all about what you can do when you are stuck with a problem or a code bug.