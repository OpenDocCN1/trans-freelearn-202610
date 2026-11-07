Let me see if I can create an alert popup. To accomplish this, I will input the following into the input field:

```
```html
<script>alert("Your site is not protecting against XSS!Please fix ASAP!!")</script>
```
```

When I hit Submit button, I can see the URL has my script in it passed as a variable. I can also see that the XSS worked, as I can see the alert pop up with the message that I specified:

![](bbox=[0.3, 0.24, 0.7, 0.36])

# Stored XSS attack

The other type of cross-site scripting attack is the stored XSS attack. For these, the script is injected into fields which are stored on the target servers. These include items like a guestbook, message forum, visitor logs, or databases. The victim will then retrieve the information as well as the script, and the malicious script will then run. Stored XSS, or persistent XSS, tend to be not as common, but they also tend to be more devastating.

To attempt a stored XSS attack, I will try and see how the form works. I will fill out the form and submit to verify all is working:

![](bbox=[0.17, 0.58, 0.83, 0.71])