
To enable Dependabot in your repository, you should enable **Dependabot alerts** and **Dependabot security updates**, as shown in Figure 10.10.

![Figure 10.10 – Enabling Dependabot alerts](image/B21847_10_10.jpg)

Figure 10.10 – Enabling Dependabot alerts

Dependabot helps you take your security up a notch in the following ways:

* **Scanning for vulnerabilities**: Dependabot regularly checks the dependencies your website uses. When it finds a problem, it alerts you by sending an email to you and a notification on GitHub, kind of like a smoke detector going off when it detects something wrong.

![Figure 10.11 – Dependabot alerts in action](image/B21847_10_11.jpg)

Figure 10.11 – Dependabot alerts in action

* **Automatically fixing issues**: If Dependabot finds a safer version of a problematic dependency, it will automatically prepare an update. It’s as if it’s not only telling you the milk is spoiled but also going to the store to get fresh milk for you.

![Figure 10.12 – Fixing issues found by Dependabot](image/B21847_10_12.jpg)

Figure 10.12 – Fixing issues found by Dependabot

* **Reviewing and merging**: You can review the update (the pull request) that Dependabot made. If everything looks good, you merge it into your project, updating the dependency. It’s like checking the quality of new milk and then putting it into the fridge for use.

![Figure 10.13 – Merging fixed issues](image/B21847_10_13.jpg)

Figure 10.13 – Merging fixed issues

Regular scans and updates mean your website is less likely to have vulnerabilities that could be exploited. Automating the update process saves you the hassle of manually checking and updating each dependency. Knowing that Dependabot is keeping an eye on things lets you focus on other aspects of your website or new projects.

You’ve learned how to keep your website safe by using vulnerability scanning with Dependabot on GitHub. This tool helps you identify and fix security issues in your project’s dependencies, making your website more secure. Next, we’ll talk about protecting your one-page website with `CODEOWNERS` on GitHub. This feature allows you to designate specific people of your team to review and approve changes to certain parts of your project, ensuring that the right team members oversee the code. This helps maintain the quality and security of your website.

# Protecting your one-page website with `CODEOWNERS`

As you’re working on your one-page portfolio website with your friends, you’ve got different parts of the website you’re each responsible for. Now, you want to make sure that any changes to these parts are checked by the right person before they become part of the main website. This is where something called `CODEOWNERS` comes into play on GitHub.

## What is `CODEOWNERS`?

Think of `CODEOWNERS` as a list you put up that says who’s in charge of what. For your website, it’s a file you add to your project that lists who oversees different parts of your site. So, if someone wants to change something, the right person gets to review it first.

Creating the `CODEOWNERS` file is easier than you think; you can do it in a few easy steps:

1. First, you create a new file named `CODEOWNERS` and put it in the root of your repository or `.github` folder in your project. It’s like putting that list in a place where everyone can see it.

2. The next thing you have to do is to add names to specify who is responsible for what. Inside the `CODEOWNERS` file, you write down which parts of the website each person is responsible for. For example, if your friend Alex is good at styling, you might say that Alex is the owner of the CSS file.

It might look something like this – `*.css @Alex`. This means that Alex is the boss of the `styles.css` file.

![Figure 10.14 – The CODEOWNERS file](image/B21847_10_14.jpg)

Figure 10.14 – The CODEOWNERS file

## How does `CODEOWNERS` help?

`CODEOWNERS` adds automatic reviews, meaning that when someone makes a change to a part of the website listed in `CODEOWNERS`, GitHub automatically knows to ask the right person (such as Alex for the CSS file) to review the changes before they’re added to the main project.

`CODEOWNERS` makes things safe, helping to prevent mistakes or unwanted changes from slipping through because the person who knows a certain part best gets to check it first.

This way, you can organize who looks after what, making sure everyone’s working together smoothly without stepping on each other’s toes.

![Figure 10.15 – File protected by CODEOWNERS](image/B21847_10_15.jpg)

Figure 10.15 – File protected by CODEOWNERS

Let’s say you’re great at writing documentation, so you’re the CODEOWNER for the documentation on the website. Anytime someone wants to update the website’s documentation, you’ll be asked to review it. It ensures that the website’s message stays clear and true to what you envisioned.

You’d add a line to the `CODEOWNERS` file such as `*.md` or `*.txt @YourUsername`, indicating that you’re in charge of the content directory.

By using `CODEOWNERS`, you’re putting up a friendly *guard* on different parts of your website project. It’s like saying, “*Hey, I’ve got this part covered, but I’m happy to look over your ideas before we make it official.*” It keeps your project safe and ensures that changes are always checked by someone who knows the ropes.

So, setting up `CODEOWNERS` is like creating a safety net for your one-page portfolio website, ensuring that every change is given the thumbs up by the right person, keeping your project on track and looking its best.

Now that you’ve set up `CODEOWNERS` to make sure the right people review changes to our website, let’s focus on making your site even safer. Next, we’re going to learn about applying secure coding practices. This is like learning the best ways to lock up our website at night, making sure it’s safe from any unwelcome visitors. You’ll make sure your website isn’t just well-organized but also well-written and secure.

# Applying secure coding practices

After you’ve built your one-page portfolio website and are using GitHub to manage and share your code, you’ll want to keep your personal information safe online. It’s essential to protect your website’s code from potential threats.

Building a one-page portfolio website on GitHub is exciting, but ensuring it’s secure is crucial. It’s like inviting guests into your digital home; you want to make sure it’s safe and secure for everyone.

Let’s dive into how you can apply secure coding practices using GitHub features.

## Why secure development matters

Think about your website like a diary with a lock. You wouldn’t want strangers reading your secrets, right? Similarly, when developing software, you need to protect it from unwanted access or changes. This means making sure only the right people can see or modify your website’s code and content.

## Planning a secure development strategy

We’re going to look at several key practices that act as layers of security, from keeping sensitive data out of public view to regulating who can make changes to our project:

* **Protecting information using `.gitignore`**: Just like you wouldn’t leave your diary open for everyone to read, you need to keep your website’s data safe from those who shouldn’t see it. Imagine you have a key to a secret page where you keep sensitive information away from prying eyes. In your GitHub repository, the `.gitignore` file acts like this secret page, telling Git which files or directories to ignore and not upload to GitHub. This could include configuration files with passwords or API keys. This file can be created in the root of your repository to tell Git which files and directories to ignore when you make a commit. Figure 10.16 shows `.gitignore` in action.

![Figure 10.16 – The .gitignore file](image/B21847_10_16.jpg)

Figure 10.16 – The .gitignore file
