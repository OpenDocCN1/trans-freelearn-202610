```
```solidity
pragma solidity ^0.4.11;
contract simpleVotingDapp {
    /*
    The key of the mapping is candidate name stored as type bytes32 and value is
    an unsigned integer to store the vote count
    */
    mapping (bytes32 => uint8) public votesReceived;
    /*
    We use an array of bytes32 instead to store the list of candidates
    */
    bytes32[] public candidateList;
    /* This is the constructor which will be called once when we
    deploy the contract to the blockchain. When we deploy the contract,
    we will pass an array of candidates who will be contesting in the election
    e.g.["Tom","Dick","Harry"]
    */

    function simpleVotingDapp(bytes32[] candidateNames) {
        candidateList = candidateNames;
    }

    // This function returns the total votes a candidate has received so far
    function totalVotesFor(bytes32 candidate) returns (uint8) {
        if (validCandidate(candidate) == false) revert();
        return votesReceived[candidate];
    }

    // This function increments the vote count for the specified candidate. This
    // is equivalent to casting a vote
    function voteForCandidate(bytes32 candidate) {
        if (validCandidate(candidate) == false) revert();
        votesReceived[candidate] += 1;
    }

    function validCandidate(bytes32 candidate) returns (bool) {
        for(uint i = 0; i < candidateList.length; i++) {
            if (candidateList[i] == candidate) {
                return true;
            }
        }
        return false;
    }
}
```
```

Figure 5.8: Simple votingDapp.sol

![](bbox=[0.146, 0.598, 0.855, 0.787])

Figure 5.9: VotingDapp on private blockchain with candidate array as a constructor parameter