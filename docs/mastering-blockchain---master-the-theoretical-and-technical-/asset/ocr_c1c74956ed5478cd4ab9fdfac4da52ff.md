# Circuit

As shown in the following image, the positive leg (long leg) of the LED is connected to pin number 21 of the GPIO and the negative (short leg) is connected to the resistor, which is then connected to the **ground (GND)** pin of the GPIO. Once the connections are set up the ribbon cable can be used to simply connect to the GPIO connector on the Raspberry Pi.

![](bbox=[0.332, 0.235, 0.667, 0.404])

Once the connections are set up correctly and the Raspberry Pi has been updated with the appropriate libraries and geth, the next step is to develop a simple smart contract that expects a value. If the value provided to it is not what it expects it does not trigger an event; otherwise, if the value passed matches the correct value, the event triggers which can be read by the client JavaScript programme running via Node.js. Of course, the solidity contract can be very complex and can also deal with the ether sent to it and if the amount of ether is equal to the required amount then the event can trigger; but in this example the aim is to demonstrate the usage of smart contracts to trigger events that can then be read by JavaScript programmes running on Node.js, which then in turn can trigger actions on IoT devices using various libraries.

The smart contract source code is shown as follows:

```
```solidity
pragma solidity ^0.4.0;
contract simpleIOT {
    uint roomRent=10;
    event roomRented(bool returnValue);
    function getRent (uint8 x) returns (bool)
    {
        if (x==roomrent)
        {
            roomRented(true);
            return true;
        }
    }
}
```
```

![](bbox=[0.332, 0.687, 0.667, 0.86])