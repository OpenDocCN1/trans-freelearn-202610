# Token systems

These are related to behavioral design patterns. Token systems are analogous to the template method, where the skeleton of an algorithm is defined as an abstract class, allowing the sub-classes to provide concrete behavior.

In the previous chapters, we discussed how the ERC20 standard is implemented for Ethereum-based token systems for compatibility. ERC20 is not a piece of code, software, or technology. It is a technical specification.

Figure 11.3 depicts a sample ERC20 standard contract interface with the recommended function definitions:

```
contract ERC20 {
    function totalSupply() constant returns (uint totalSupply);
    function balanceOf(address _owner) constant returns (uint balance);
    function transfer(address _to, uint _value) returns (bool success);
    function transferFrom(address _from, address _to, uint _value) returns (bool success);
    function approve(address _spender, uint _value) returns (bool success);
    function allowance(address _owner, address _spender) constant returns (uint remaining);
    event Transfer(address indexed _from, address indexed _to, uint _value);
    event Approval(address indexed _owner, address indexed _spender, uint _value);
}
```

Figure 11.3: ERC20 token system