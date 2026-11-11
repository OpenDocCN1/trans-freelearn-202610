# Format the Users will Use when They Use Your Search

While this question does not directly affect keyword behavior, it will make you think about what the user has to put in. This question essentially asks what is the entire SMS message the user must send to 411Sync. The first word should be the name of the keyword. If there are any variables, they should be named and enclosed in curly brackets.

Later, we will see we need one variable to identify the geographic region we are interested in. Therefore when you create a service, make sure this question is filled in with the keyword and a variable name in curly brackets. In the example, I created a mobile search with the chpreports {regionCode}.

# HTTP Location of the XML Data

This question asks where your RSS file will be. This should point to the script we are going to create. Note that if the user needs to enter in a variable in order to use your search, you need to manually set up the URL here to handle a variable. For example, in the previous question we identified one variable, regionCode. (That we need. "regionCode" is purely informational!) In the script, we do not access it as $_GET['regionCode']. 411Sync will take whatever extra parameters it sees and just append it to this URL.

In creating my URL, I set this question to:

```
http://www.shuchow.com/mashups/ch4/feed.php?rid=
```

As parameters are just passed to the URL, we catch it here. We set up one URL and at the end, we start the query string parameter. In this case, it is named rid. When 411Sync is activated and it sends a parameter and GET request to the service, that parameter will get stuck at the end of this URL. Then, our script will be able to access it using $_GET['rid'].