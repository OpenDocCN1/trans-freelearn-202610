After you select **Copy login command**, you will be shown a page similar to *Figure 5.5*. There is a lot going on during this process, and it’s worth walking through it step by step:

+   1. Since this is accessing sensitive information, you may be asked to log in again. You’ll see a simple screen that reads **Display Token**. Clicking on this link will two pieces of information.
+   2. Look for the line starting with **Your API token is sha256~_oNI_uw-qyD6A~-EI-Qhg1-sddfFezCFZ-gZxLxFZUo**. Note that *your token will be different as it is unique to you and will change daily*.
+   3. Copy the text starting from **sha256** through to the end of the line.
+   4. It’s probably better to save it to a text file (using Notepad) as we need to copy a second value from this page. We’ll use this token instead of a username and password so OpenShift will know it’s us when we connect to it from KIE Sandbox.
+   5. Our second value is the URL (web location) of where to find our OpenShift instance in the cloud. This value is on the next line starting **oc login**. You need to take the text immediately after **server=** through to the end of the line, as highlighted in *Figure 5.5*. Again, your values are unique to you and will be different from the image.

```
oc login --token=sha256~_oNI_uw-qyD6A~-EI-Qhg1-sddfFezCFZ-gZxLxFZUo
server=https://api.sandbox.x8i5.p1.openshiftapps.com:6443
```

Figure 5.5 – Copying server details

+   6. Save this server URL somewhere—we’re about to use it to link to our KIE Sandbox.

# Linking KIE Sandbox and your OpenShift instance

If you don’t already have it open, go to your local KIE Sandbox at https://sandbox.kie.org/. We use the word local as up until now, all the decision models have been saved in our web browser on our local laptop.

> **KIE Sandbox is constantly evolving**
> The KIE team has the sandbox under constant development, so it likely the screenshots will differ slightly from what is presented here. The basic concept of providing the token via the settings menu to link the KIE Sandbox to both OpenShift and GitHub will remain the same.

We move to the local cloud as we link KIE Sandbox to our new OpenShift cloud instance:

+   1. Click on the settings icons on the top right of the KIE Sandbox screen.
+   2. Click on **OpenShift** in the pop-up screen that appears and you should see the settings screen like in *Figure 5.6*: