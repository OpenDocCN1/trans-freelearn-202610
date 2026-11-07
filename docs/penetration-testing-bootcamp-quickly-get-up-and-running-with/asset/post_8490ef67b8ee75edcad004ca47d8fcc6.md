
In theory, the error messages should be generic and let you know the page isn't there, but not divulge any other information, similar to the page below from google.com. Now we still get a 404 error, so we know that page doesn't exist, but it is not giving us any additional information which is better than the above example:

![](img/095df2cd1930baecbcecaa51ee3417aa_208_0.png)

## Session management

Making sure others can't hijack your session while using a web application is important. Otherwise, they would be able to not only see what you are doing, but be able to buy items with your information. It is important for web applications to have safeguards in place to prevent session hijacking from occurring. Sessions can come in all types. For example, they can use cookies, or within the URL, or even with a hidden field. No matter the session type, they should remain unique to a user, and not be able to be stolen or seen by another user.

Using some of the following utilities, I will test some of the web applications for any session hijacking issues.

## Burp Suite with intercept

Burp Suite is a pretty comprehensive product, with many functions that do a ton of different things. The functions are listed at the top, and include items like Proxy, Spider, Intruder, Repeater etc. Since we are talking about manipulating sessions, we will use the Proxy and Intercept functions. This will allow us to try man-in-the-middle connections and steal session information. The other functions allow for other, very powerful ways to test your web applications as well, but are beyond the scope of this session management section. For information on those sections, you can check out the Burp Suite help page: https://portswigger.net/burp/help/suite_burptools.html.

Now, let's play around with the Proxy function next. For using the Proxy and intercept features, we first need to record a valid session. How you get the traffic to Burp Suite is really up to you. I will be using a fake proxy server setting to have the client come through me before reaching the ultimate destination. This will allow me to see their session IDs, and hopefully, replay them back to the destination application and get in without logging in. To accomplish this, I will assume that the proxy server setup is already complete.

I will go to my simulated client machine, and log into a website for testing. I will make sure that I have intercept turned on within Burp Suite. You can see that option below:

![](img/095df2cd1930baecbcecaa51ee3417aa_209_0.png)

I will then go to my client machine, and log into the website that I will be testing against in my lab:

![](img/095df2cd1930baecbcecaa51ee3417aa_209_1.png)

While browsing from the client machine, if Intercept is turned on, you will need to hit forward for each request.

Once I am at the login page, I will log in and get to the main login page.

![](img/095df2cd1930baecbcecaa51ee3417aa_210_0.png)

Once logged in, I will hit refresh on the page to verify that I am still logged in, and then grab the PHPSESSID. I will copy the ID, and paste it into a text editor of my choosing. Remember, you will have to hit forward on any of the connections, including the refresh.

![](img/095df2cd1930baecbcecaa51ee3417aa_210_1.png)

Once that is completed, I will open up my local browser which is also going through the proxy. This will be the hacker computer. I will hit that same website, but making sure I am going directly to the page that will display after I login, and in this case, it's `index.php` based on the `GET` command above:

![](img/095df2cd1930baecbcecaa51ee3417aa_211_0.png)

Now, since I am intercepting this as well, I will look at the RAW output. Before I hit forward, I will change the `PHPSESSID` that I get from my new connection to the one I grabbed above:

![](img/095df2cd1930baecbcecaa51ee3417aa_211_1.png)

I will then delete that ID, and paste the one from above. The following screenshot shows that the old session ID has replaced my new one:

![](img/095df2cd1930baecbcecaa51ee3417aa_212_0.png)

Once all set, I will hit forward, and my browser will go right to the index.php page without me having to enter any credentials, since I was able to steal the session from another user who already logged in.

## Using XSS for cookie retrieval

When we talked about cross-site scripting in the last section, I had the inserted script just output an alert message. But, technically you could add the script to other things. One option is for retrieving other user session information by having the script grab the current session ID, and send it off to the hacker. This way, they are grabbing all the session IDs as users are logging into the application.

In my example, I will just be outputting the session ID to the screen via an alert box so that we can verify that the session ID is there, but with some slight modification, you could have it sent to the hacker.

To accomplish this, I will use the following to access that session ID information:

```
<script>alert(document.cookie)</script>
```

Now, when that script is run within the client browser, I can see that the user's session ID popup within that alert menu:

![](img/095df2cd1930baecbcecaa51ee3417aa_213_0.png)

This is a very easy way to grab and use other users' session information so that you don't have to login to these applications yourself.

## Summary

We are now done with day 8, so let us recap. We talked about various OWASP categories for web application security, including infrastructure and design weaknesses, identity-based testing, validating data/error handling and logic, session management, and finally, manipulation with client-side testing. I used various tools in my lab to search web applications that had issues located in each of these categories. These tools all ran within Kali Linux, and provided large amounts of information for the issues they disclosed.

In the next chapter, I will talk about how to get out of the environment and clean-up since the penetration test is coming to an end. This is an important task, as we don't want to leave any findings or information behind. Plus, we want to make sure we leave the environment how we found it prior to the penetration testing.

I will touch on how to clean up the trails we leave behind, as well as covering our tracks and destroying any equipment that we may need to. A lot of this will ultimately depend on the initial engagement discussed with the stakeholders.

## 9

## Cleaning Up and Getting Out

With us coming toward the end of our engagement with the stakeholders, and all the tests having been performed, it is time for us to do any necessary housekeeping we need to do and then get out of the environment.

It is important to make sure we leave the environment just like it was before we started the penetration test. The software should be the same as prior to the testing as well as the hardware. There should be nothing new that we introduced into the environment that we don't undo. Our job is to hand over the findings and documentation to the stakeholders and have them perform any remediation that they feel is necessary based on their schedule and priorities.

The following topics will be discussed in this chapter:

- Covering your tracks
- Cleaning up any trails left behind
- Destroying equipment

## Cleaning up any trails left behind

One of the most important things we can do, as penetration testers, is to leave the environment in the same shape it was prior to the engagement. This means we need to make sure anything we had on the systems has been removed. This list is just some of the things that we need to do as responsible penetration testers:

- Remove any user or system accounts that were created for any tests or procedures. We don't want to leave any unmonitored accounts.
- Remove any exploits, backdoors, rootkits, or malware that we may have left behind as part of our testing.
