```
function getTweet(uint tweetId) constant returns (string tweetString, uint timestamp) {
    // returns two values
    tweetString = _tweets[tweetId].tweetString;
    timestamp = _tweets[tweetId].timestamp;
}

function getLatestTweet() constant returns (string tweetString, uint timestamp, uint numberOfTweets)
{
    // returns three values
    tweetString = _tweets[_numberOfTweets - 1].tweetString;
    timestamp = _tweets[_numberOfTweets - 1].timestamp;
    numberOfTweets = _numberOfTweets;
}
```

Figure 9.7: Important tweet retrieval functions

The following Figure 9.8 summarizes the remaining functions of the account contract. getNumberOfTweets() returns the count of tweets by the account owner. getOwnerAddress() returns the account owner's blockchain address. The function adminRetrieveDonations() can be further developed to allow the account owner to retrieve any donations they have received from other users. So, this contract can then behave as a donation wallet that can tweet messages. We have muted this functionality to keep the contract simple. The function adminDeleteAccount() kills his tweeter account and retrieves all donations received:

```
function getNumberOfTweets() constant returns (uint numberOfTweets) {
    return _numberOfTweets;
}

function getOwnerAddress() constant returns (address adminAddress) {
    return _adminAddress;
}

function adminRetrieveDonations(address receiver) {
    if (isAdmin()) {
        receiver.transfer(this.balance);
    }
}

function adminDeleteAccount() {
    if (isAdmin()) {
        selfdestruct(_adminAddress);
    }
}
```

Figure 9.8: Miscellaneous functions

But to have a new tweeter account, we need user registration. This action is achieved by the TwtRegistry smart contract that is described in the next section.