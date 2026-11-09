You’ll be doing all of this while enjoying the benefits of trusted, secure, Salesforce-approved tools that offer a precise insight into how customers are interacting with your application. The road might seem challenging, but with AppExchange App Analytics as your compass, you’re more than equipped to steer the ship of your application’s success.

# Enabling AppExchange App Analytics

Whether you’re just setting foot on your AppExchange journey or are well on your way, one of the smartest moves you can make is to request AppExchange App Analytics. “Why the haste?”, you ask. Well, the sooner you request it after your application passes the security review, the quicker you start accumulating valuable information about your app’s usage.

Keep in mind that some logs are collected exclusively from your customers’ orgs, so initially, your data repository might seem like an empty canvas. But remember—Rome wasn’t built in a day. With time and increased usage, the canvas will be filled with a spectrum of data, offering you a wealth of information to analyze and act upon.

To activate access to AppExchange App Analytics package usage logs and subscriber snapshots, run SF CLI command:

```
sf package update --package "Your Package Alias" --enable-app-analytics
```

You can anytime deactivate these logs by SF CLI command:

```
sf package update --package "Your Package Alias" --no-enable-app-analytics
```