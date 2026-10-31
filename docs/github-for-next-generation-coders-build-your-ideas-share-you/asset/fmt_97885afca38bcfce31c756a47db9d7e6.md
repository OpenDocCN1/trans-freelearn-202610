
## View and adjust permissions
In the screenshot shown in Figure 10.2, you can see that you can also invite friends by their GitHub usernames. For each friend, you can set their repo access level or remove access. A personal GitHub repository has only two permissions:
* `Ownership`, which gives full control of the repository.
* `Collaborators`, which applies to users who can read the contents of the repository and write to it by making pull requests.

If you need more granular access to your repository, you should convert the repository to an organization that has many more roles.

## Teams and more (organization account repo)
If you’re working with a lot of people, you can create teams, such as a design team or a content team, and give each one a different key. Click on `Create team` to start creating new teams.

![Figure 10.3 – Creating new teams](image/B21847_10_3.jpg)
Figure 10.3 – Creating new teams

### What are access controls good for?
By setting the access control on your repo, you ensure that only the right people can make changes.
Access control also helps you keep a project organized and know who’s responsible for what task in it. It will give you peace of mind to know that your friends helping you on the project won’t accidentally remove the main section of your website.

Now that you’ve learned how to manage who can do what in your website project, let’s take the next step to learn how to keep your site even more secure. We’ll look into how to set up 2FA for your portfolio website GitHub repository. This is like adding a double lock on your door, making sure that only the right people can get in, even if they have a key.

# Setting up 2FA for your portfolio website
You’ve built a cool one-page portfolio website and stored it on GitHub, a place where you can keep all your website files safe and sound. However, just like how you’d want a good lock on your house to keep your treasures secure, you also want to make sure your GitHub account is extra-protected. That’s where 2FA comes in.

## What is 2FA?
Think of 2FA as a double-check process to make sure it’s really you trying to access your GitHub account. It’s like having a door with two locks. The first lock opens with your key (your password), and the second lock opens with a special code (the second factor), which changes every time.

### Why use 2FA for your website repo?
Being extra safe has no disadvantages, and it’s the same case when it comes to using 2FA. Some of its benefits are:
* 2FA makes it much harder for someone else to sneak into your GitHub account and mess with your website project
* You can be more relaxed knowing your website’s code and content are well-protected

## How to set up 2FA
To set up 2FA on GitHub, follow these steps:
1. Go to the settings of your GitHub account: After logging into GitHub, click on your profile picture in the top-right corner and select `Settings`. It’s like going into the control panel of your account.
![Figure 10.4 – GitHub account settings](image/B21847_10_4.jpg)
Figure 10.4 – GitHub account settings
2. Find the security section: On the sidebar in the `Settings` menu, look for `Password and authentication`. This is where you can manage your account’s defenses.
![Figure 10.5 – GitHub’s Password and authentication section](image/B21847_10_5.jpg)
Figure 10.5 – GitHub’s Password and authentication section
3. Start the 2FA process:
   1. Find the option for `Two-Factor Authentication` and click on `Enable two-factor authentication`. It’s like deciding to add that second lock to your door.
   2. Scan the QR code, which will be displayed to you by your mobile device’s app, such as Microsoft Authenticator. After you have scanned the QR code, the app will generate a six-digit code that you can enter on GitHub.
![Figure 10.6 – Setting up the authenticator app](image/B21847_10_6.jpg)
Figure 10.6 – Setting up the authenticator app
4. Choose your 2FA method:
![Figure 10.7 – Choose your 2FA method](image/B21847_10_7.jpg)
Figure 10.7 – Choose your 2FA method
* `SMS/Text message`: You’ll get the second code sent to your phone as a text message. It’s straightforward but requires your phone to be handy.
* `Authenticator app`: Use an app such as Google Authenticator, Microsoft Authenticator, or Authy. The app generates the code for you. It works even if your phone isn’t connected to the internet.
5. Follow the setup instructions: GitHub will guide you through the setup process. It involves verifying your password (your first lock) and then setting up how you’ll receive your 2FA codes (your second lock).
6. Backup codes: During setup, GitHub gives you backup codes. These are like spare keys. Click `Download` to download your recovery codes to your device. Save them somewhere safe but accessible, just in case you can’t use your phone or authentication app.
7. After saving your two-factor recovery codes, click `I have saved my recovery codes` to enable 2FA for your account.
8. Configure 2FA using GitHub Mobile: You can have multiple options for 2FA, one of which is using the GitHub Mobile app when signing into your GitHub account.
![Figure 10.8 – The 2FA method using GitHub Mobile](image/B21847_10_8.jpg)
Figure 10.8 – The 2FA method using GitHub Mobile
You just have to enable 2FA and install the GitHub mobile app from your mobile app store to sign in to your GitHub account.

Setting up 2FA is more like adding a high-tech security system to your house. It’s a bit more effort to get in the door, but it means your website project – and all the hard work you’ve put into it – is much safer. Next, we will learn about another GitHub feature that will help you add security.

# Keeping your website safe – vulnerability scanning with Dependabot
To make your website run smoothly and look good, you can use various tools and libraries to develop it, kind of like using different ingredients to bake a cake. But what if one of those ingredients was spoiled? It could ruin your cake, right? In the world of web development, spoiled ingredients are vulnerabilities in the tools and libraries you use, which could make your website unsafe. That’s where vulnerability scanning and Dependabot come into play on GitHub.

## What is vulnerability scanning?
`Vulnerability scanning` with `Dependabot` is like having a gadget that scans all your ingredients to make sure they’re safe to use. On GitHub, it scans the tools and libraries (called `dependencies`) that your website uses to find any known vulnerabilities.

### Why is it important?
Just like you wouldn’t want to use spoiled ingredients in a cake, you also don’t want to use vulnerable `dependencies` that could make your website an easy target for bad actors. It ensures your website remains safe for everyone who visits it.

## And what’s Dependabot?
`Dependabot` is like a helpful robot in your kitchen that not only tells you when an ingredient is spoiled but also automatically replaces it with a fresh one. On GitHub, `Dependabot` automatically updates your `dependencies` if it finds vulnerabilities.

### How does it work?
If `Dependabot` finds a vulnerability in one of your website’s `dependencies`, it will alert you and can automatically create a `pull request` to update it to a newer, safer version. Creating a `pull request` in this scenario would be like swapping out the vulnerable dependency for a safe one.

### Setting up Dependabot
You enable `Dependabot` in your website’s GitHub repository settings. It’s like turning on the automatic ingredient checker in your kitchen. To navigate to the Dependabot settings, go to `Code`, and then `Code security and analysis`. You can refer to Figure 10.9 to check whether you are on the right screen.
![Figure 10.9 – Setting up Dependabot](image/B21847_10_9.jpg)
Figure 10.9 – Setting up Dependabot
