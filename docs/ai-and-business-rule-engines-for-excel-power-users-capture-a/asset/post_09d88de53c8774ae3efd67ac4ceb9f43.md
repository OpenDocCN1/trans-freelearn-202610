
www.packtpub.com/product/openshift-for-the-absolute-beginner-hands-on-video/9781838559090.

OpenShift has a free layer called the Developer Sandbox (not to be confused with the KIE Sandbox). This allows you to try out the deployment with the option to upgrade to a paid-for subscription later. Obviously, the free offering has some limitations, which we will touch on at the end of the section. Broadly speaking, the Developer Sandbox is good enough for developing business rules, but you probably want to upgrade to the full version if you’re using it with paying customers.

Let’s walk through signing up for the Developer Sandbox, accessing the Sandbox once it is set up, linking the Sandbox to the Developer Sandbox, and deploying our decision models to it.

## Signing up for the OpenShift Developer Sandbox

Signing up for the OpenShift Developer Sandbox is a similar process to joining a popular website such as Gmail or Facebook. You go to the website, provide some personal details, verify your email or mobile number, and then you’re logged into the site. We’ll walk through these steps one by one to get you started:

- 1. Open the main Developer Sandbox page at https://developers.redhat.com/developer-sandbox.
- 2. While this page has a lot of information, there is a lot here we don’t need. Click on the Start your sandbox for free button:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_127_0.png)

Figure 5.1 – Signing up for the OpenShift Sandbox

- 3. Enter your email to begin the process of setting up your OpenShift account.

You will be asked for basic details, including a password. When you’ve filled in all the necessary fields click on the button to Create My Account.

- 4. During the account creation process, you will be asked to confirm your mobile number. That’s there to make it more difficult for people to sign up for multiple accounts. You will have to enter the code that gets sent to your mobile.

- 5. You’ll also be prompted to choose a project name during the setup process—this is something you can’t change later, so pick one that you’d be happy sharing with your business colleagues.

- 6. Finally, after confirming your email address by clicking on the link in an email that gets sent to you, you should see the You’re ready to get started! message:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_128_0.png)

Figure 5.2 – Successful OpenShift sandbox setup

During the signup process, you may see wording in the emails that get sent that you don’t quite understand. For example, see the following:

```
Notice: Your Developer Sandbox for Red Hat OpenShift account is provisioned
```

Sandboxes are a bit like renting an apartment condo. We don’t own it, but we’re the only ones that can access that piece of the building during the rental period. We expect it to be fully clean with no trace of the previous occupants when we check in, and we expect all the features listed (beds, coffee makers, and so on) to be ready to use. Provisioned means that this setup process is complete.

A better comparison

Since sandboxes are completely torn down and rebuilt after we use them, a better comparison would be German-style rentals where the apartment is stripped back to nearly bare walls, and each person renting must install their own appliances and furniture.

Another reminder in the email is like so:

```
Your account will be active for 30 days. At the end of the active period, your access will be deactivated and all your data on the Developer Sandbox will be deleted.
```

Red Hat allows us to use their cloud for free, but they have to draw the line somewhere. 30 days is plenty of time to try out the ideas in this book, and you can sign up again (using the same contact details) for several more 30-day periods.

In general, we’ll always have a local copy of our decision models, and we don’t store data once the decision model has finished its run, but it’s a useful reminder!

## Accessing your OpenShift Developer Sandbox

Now that we have our account set up, go to `https://developers.redhat.com/developer-sandbox` to start using our new cloud environment. The website should remember that you have logged in (but will give you the option to do so if needed), and you may have to click on **Start Using your Sandbox** to view the main OpenShift console like in *Figure 5.3*:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_129_0.png)

Figure 5.3 – The OpenShift console

There is a lot in this image showing the power of the OpenShift cloud offering. We won’t do our normal tour of the screen; instead, we’ll take it as confirmation that we have the Developer Sandbox set up correctly. If you’re curious, take a look around, but we’ll focus on getting two key pieces of information to allow us to link our local KIE Sandbox with this piece of the cloud.

## Two vital pieces of link information

Before we leave the OpenShift console, let’s make a note of two key pieces of information that we’ll need to link to our OpenShift instance later. *Figure 5.4* shows the menu under the project name on the top right of the console. Your project name will be different, but **Copy login command** will be the same.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_129_1.png)

Figure 5.4 – Copying login details

After you select **Copy login command**, you will be shown a page similar to *Figure 5.5*. There is a lot going on during this process, and it’s worth walking through it step by step:

- 1. Since this is accessing sensitive information, you may be asked to log in again. You’ll see a simple screen that reads **Display Token**. Clicking on this link will two pieces of information.
- 2. Look for the line starting with **Your API token is sha256~_oNI_uw-qyD6A~-EI-Qhg1-sddfFezCFZ-gZxLxFZUo**. Note that *your token will be different as it is unique to you and will change daily*.
- 3. Copy the text starting from **sha256** through to the end of the line.
- 4. It’s probably better to save it to a text file (using Notepad) as we need to copy a second value from this page. We’ll use this token instead of a username and password so OpenShift will know it’s us when we connect to it from KIE Sandbox.
- 5. Our second value is the URL (web location) of where to find our OpenShift instance in the cloud. This value is on the next line starting **oc login**. You need to take the text immediately after **server=** through to the end of the line, as highlighted in *Figure 5.5*. Again, your values are unique to you and will be different from the image.

```
oc login --token=sha256~_oNI_uw-qyD6A~-EI-Qhg1-sddfFezCFZ-gZxLxFZUo
server=https://api.sandbox.x8i5.p1.openshiftapps.com:6443
```

Figure 5.5 – Copying server details

- 6. Save this server URL somewhere—we’re about to use it to link to our KIE Sandbox.

## Linking KIE Sandbox and your OpenShift instance

If you don’t already have it open, go to your local KIE Sandbox at https://sandbox.kie.org/. We use the word local as up until now, all the decision models have been saved in our web browser on our local laptop.

> **KIE Sandbox is constantly evolving**
> The KIE team has the sandbox under constant development, so it likely the screenshots will differ slightly from what is presented here. The basic concept of providing the token via the settings menu to link the KIE Sandbox to both OpenShift and GitHub will remain the same.

We move to the local cloud as we link KIE Sandbox to our new OpenShift cloud instance:

- 1. Click on the settings icons on the top right of the KIE Sandbox screen.
- 2. Click on **OpenShift** in the pop-up screen that appears and you should see the settings screen like in *Figure 5.6*:

### Settings

GitHub   OpenShift   KIE Sandbox Extended Services

Configure through the guided wizard instead→

Namespace (project) * ?

Host * ?

Token * ?

Connect

Figure 5.6 – Entering server details

3. While it’s possible to enter the details directly here, it’s easier the first time to click on Configure through the guided wizard instead.

The first step of the connection wizard asks for the project name. If you’ve forgotten what it was, go back to OpenShift; it’s on the top right of the screen. Ours is crt-openshift-dev yours will be different and must match your OpenShift settings exactly. Figure 5.7 shows a sample. When this is done, click Next.

### Settings

GitHub   OpenShift   KIE Sandbox Extended Services

- 1. Create your instance
- 2. Set credentials
- 3. Connect

In order to create your Developer Sandbox instance:
1. Go to the Get Started page
2. Follow the steps to launch your instance. You will be asked to log in with your Red Hat account.
3. Once your instance is up and running, inform the namespace (project) in your cluster you want to have the Decision Model deployed to.

Namespace (project)   crt-openshift-dev

This information is necessary for deploying your Decision Model into the right project namespace.

Next   Back   Cancel

Figure 5.7 – Connection wizard step 1
