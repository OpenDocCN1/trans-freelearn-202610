# # TwtAccount.sol

This solidity file models a single Twitter account on a blockchain. *Figure 9.3* summarizes the important functions present in this twitter account contract:

```
contract TwtAccount{

    function TwtAccount()

    function getOwnerAddress() constant returns(address adminAddress)
    function adminDeleteAccount()
    function adminRetrieveDonations(address receiver)

    function getLatestTweet() constant
    returns(string tweetString,uint256 timestamp,uint256 numberOfTweets)

    function isAdmin() constant returns(bool isAdmin)

    function getTweet(uint256 tweetId) constant
    returns(string tweetString,uint256 timestamp)

    function getNumberOfTweets() constant returns(uint256 numberOfTweets)
    function tweet(string tweetString) constant returns(int256 result)
}
```

Figure 9.3: TwtAccount contract summary

Now, let us understand explicitly what each function in this smart contract does. *Figure 9.4* denotes the start of the account contract where `TwtAccount()` is a constructor function. Tweet denotes the data structure of a single tweet. The mapping denotes an array of all tweets to a particular tweet, account, which helps in retrieving each tweet by their `tweetID`. The other two variables denote the owner of the account and the total count of tweets by the particular owner:

```
pragma solidity ^0.4.0;
contract TwtAccount {

    struct Tweet {
        uint timestamp;
        string tweetString;
    }
    mapping (uint => Tweet) _tweets;
    uint _numberOfTweets;
    address _adminAddress;

    function TwtAccount() {
        _numberOfTweets = 0;
        _adminAddress = msg.sender;
    }
}
```