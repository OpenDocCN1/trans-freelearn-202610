In theory, the error messages should be generic and let you know the page isn't there, but not divulge any other information, similar to the page below from google.com. Now we still get a 404 error, so we know that page doesn't exist, but it is not giving us any additional information which is better than the above example:

![](bbox=[0.21, 0.2, 0.79, 0.41])

# Session management

Making sure others can't hijack your session while using a web application is important. Otherwise, they would be able to not only see what you are doing, but be able to buy items with your information. It is important for web applications to have safeguards in place to prevent session hijacking from occurring. Sessions can come in all types. For example, they can use cookies, or within the URL, or even with a hidden field. No matter the session type, they should remain unique to a user, and not be able to be stolen or seen by another user.

Using some of the following utilities, I will test some of the web applications for any session hijacking issues.

# Burp suite with intercept

Burp suite is a pretty comprehensive product, with many functions that do a ton of different things. The functions are listed at the top, and include items like Proxy, Spider, Intruder, Repeater etc. Since we are talking about manipulating sessions, we will use the Proxy and Intercept functions. This will allow us to try man-in-the-middle connections and steal session information. The other functions allow for other, very powerful ways to test your web applications as well, but are beyond the scope of this session management section. For information on those sections, you can check out the burp suite help page: https://portswigger.net/burp/help/suite_burptools.html.