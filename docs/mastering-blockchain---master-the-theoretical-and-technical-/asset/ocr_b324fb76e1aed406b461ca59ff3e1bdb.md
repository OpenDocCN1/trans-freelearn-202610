The next function is isAlreadyHashed, which checks whether the idea is already hashed. This takes the input parameter idea of type string, also declared as a constant, which means that it cannot change the state of the contract and returns either true or false based on the outcome of the execution of the function named alreadyHashed. This function then calls the alreadyHashed function described earlier to check from the hashes map whether the hash is already stored there. This would mean that the same string (idea) has already been hashed and stored (patented):

```
function isAlreadyHashed(string idea) constant returns (bool)
{
    var hashedIdea = HashtheIdea(idea);
    return alreadyHashed(hashedIdea);
}
```

Finally, the HashtheIdea function is shown here, which takes the idea variable of type string and is of constant type, which means that it cannot change the state of the contract. It is also declared as private as there is no need to expose this function publicly because it is used only internally in the contract. This function returns the bytes32 type value:

```
function HashtheIdea(string idea) constant private returns (bytes32) {
    return sha3(idea);
}
```

This function calls solidity's built-in function sha3 and passes a string to it in a variable idea. This function returns the sha3 hash of the string. The sha3 function is an alias for the keccak256() function available in solidity, which computes the Keccak-256 hash of the string passed to it. Note that this is not NIST standard SHA-3; instead, it is Keccak-256, which is the original proposal to NIST for the SHA-3 standard competition. It was later modified slightly and standardized as the SHA-3 standard by NIST. The actual SHA-3 standard hash function will return a different hash compared to Keccak-256 (Ethereum's sha3 function).

The complete contract source code is shown as follows:

```
pragma solidity ^0.4.0;
contract PatentIdea
{
    mapping (bytes32 => bool) private hashes;
    bool alreadyStored;
    event ideahashe d(bool);
    function saveHash(bytes32 hash) private
    {
        hashes[hash] = true;
    }
    function SaveIdeaHash(string idea) returns (bool)
```