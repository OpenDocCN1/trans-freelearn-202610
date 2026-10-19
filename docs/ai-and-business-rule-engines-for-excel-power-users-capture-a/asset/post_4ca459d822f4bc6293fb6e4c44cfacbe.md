
![](img/48815d6f06dc8c7aeaa3963d289fb1e7_56_0.png)

## Figure 2.1 – Drools home page on GitHub

We’ve simplified this screenshot of https://github.com/kiegroup/drools as there are a lot more files and folders on the home page of GitHub (the folder icons at the middle left of the screen). These files allow you to modify and build the Drools/Kogito rule engine completely from the source if you want. We are not going to be doing this in this book, so let’s look for the information on this page that we need to use:

- At the top right is an About section giving a one-line summary of the project. Since it describes the project as a rule engine, we know we’re in the right place. It also links to the main project site (drools.org), which we’ll explore in a couple of minutes, and gives mailing lists for user support.
- At the bottom left is the readme.md – a more detailed description of the project. It repeats the link to the website but also shares more links to the documentation for the project.
- At the top left are tools for collaboration (such as Code and Branches or Main). We won’t be going into this in detail, but if you do want to know more about how hundreds of people across the world can use these tools to work together on a complex product, take a look at the Packt book by George Lomidze, Git and GitHub: The Complete Git and GitHub Course, available at https://www.packtpub.com/product/git-and-github-the-complete-git-and-github-course-video/9781800204003.
- On the right of the page are measures of project activity, such as people watching, stars, forks, and releases. Not surprisingly, this is a very active project – we wouldn’t ask you to invest your time in anything less.
- This is not shown, but in the bottom right are the contributors to the project. At the time of writing, there are more than 200 contributors, which makes it one of the most active projects on GitHub. It’s good to see there are *real people* behind the project.

We’ll come back and use GitHub in more detail as a place to save our business rules in *Chapter 5*. There, we’ll have the chance to get familiar with the standard GitHub page layout.

## The best way to ask for help in the open source world

While it’s easy to find *where* to ask for help, I don’t want to leave you with the impression that these 200 contributors are just sitting there waiting for a call from you. While they are passionate about the KIE project, these are busy people, all with a day job (like you). Knowing *how* to ask for help will get you better results. In one line, a little humility and a lot of trying to solve the problem first will go a long way.

> **Try it on your colleagues**
> The following *how to ask for help* steps are equally valid when it comes to working with your colleagues on the IT helpdesk as when reaching out to the open source community.

The following are the steps to follow when you run into an issue. It is possible that you’ve already asked for help on Excel community forums in the past. If so, none of the following steps should appear unfamiliar to you:

- 1. Always assume that it is your mistake, not a problem in the underlying project you are using. Ninety percent of the time, when you work through it step by step, you’ll find a spelling error, a problem with an internet connection, or some simple misunderstanding. You will lose a lot of credibility if you approach the project developers with something that turns out to be your fault and is a very simple error.
- 2. **Read the Fantastic Manual (RTFM)** is age-old advice, with the addition of **Search the Fantastic Web (STFW)**. While the material we are working through in this book is new to us, plenty of people have tried similar things before. Take your time to read this book and the online documentation and double-check any other instructions you are working through.
- 3. Google any error messages. Error messages make for great search terms as they are often very specific. The search engine will suggest plenty of pages with areas worth investigating to help solve your problem.
- 4. Turn it off and on again. It is only half a joke that your IT helpdesk will ask you to restart your laptop to resolve a hard-to-fix problem. A less radical approach is to close and reopen the individual pieces of software (for example, the web browser, or restarting the Docker desktop service). Modern software is complex with many moving parts that are easy to get out of sync with each other. Reopening them gives them a fresh start, and makes it more likely all the different pieces will line up correctly.
- 5. Make sure you are looking in the right place for help and information. Remember that this book combines several different projects and technologies (Excel, Drools, Docker, Azure, Power Automate and OpenShift, Visual Studio, and others). Asking an Excel-specific question on a Drools mailing list is unlikely to return a response.
- 6. Look for similar problems. When you are searching mailing lists and the web, be open to reading about problems that are similar but not an exact match for your current issue. Some of the steps taken to solve that other project may also help you.
- 7. Two pairs of eyes are better than one. I've lost count of the number of times a colleague spotted the issue within a couple of minutes of looking at my project. This is also true where they don't have expertise in that technology – a fresh approach is sometimes enough. At the very least, having to talk through what you are doing and the Q&A that follows will unlock several suggestions that will get you nearer to a solution.
- 8. Make your question standalone. Very few people will have the exact same setup as you do. The more that you can remove special setup steps when asking your question, the bigger the pool of people that will be able to help. It's even better if you can include steps to reproduce the problem (as a step 1, 2, 3 format or a code sample).
- 9. Make your question smart. You're asking for help, and the community is under no obligation to respond to you. Taking the time to make your question clear and concise will increase your chances of success. A well-known guide in asking smart questions, by Eric Raymond, can be found here: https://github.com/selfteaching/How-To-Ask-Questions-The-Smart-Way.
- 10. Check again. Before you ask the question, go through all the preceding steps and document the steps you have already taken. This will give you credibility in asking your question, and avoid the frustration of getting back suggestions that you've already attempted.
- 11. Now, and only now, post your question and wait. Since you're looking for community support, you need to be prepared for a delay in receiving an answer (if at all). You may not receive a complete answer, but steps toward a solution. For some people, this is fine. But if responsive support is a must for you, it might be worth looking at the commercial support options from Red Hat and other vendors.

You'll notice that we haven't (yet) supplied you with the actual locations to ask questions. We'll share those shortly. The open source community is friendly but might have been worn down by lazy questions in the past. So with a bit of thought, your question will stand out and get answered.

When you find a solution (or part of one), share it back via the mailing lists or help forum for that community. While this is good karma, on a more practical level, it will help bolster your reputation in the community and make it more likely that you will get help in the future.

Now that you know how to ask questions, let’s look at one of the places we can ask questions related to the Kogito/Drools project.

## KIE, Kogito, and Drools resources

We’ve touched on a lot of aspects of the Kogito/Drools project already. We’ve seen the various components of the project, alongside the Drools rules engine. We’ve seen the dual nature of the open source and commercially supported projects from Red Hat and IBM. We’ve also seen the Drools GitHub site as a jumping-off point to learn more about the project and ask for help.

While this book will walk you through everything you need to get started with business rules, we want to give you resources to learn more, explore, and ask questions about the KIE/Drools rules engine.

Before you follow the upcoming links, remember that the documentation is written for people already familiar with business rules and the underlying technologies. It will become more accessible to you as you work your way through this book. Just don’t be overwhelmed by the complexity and detail of information being shared on these sites now:

- As we saw in our previous look at the GitHub site for Drools, the main Drools site is https://www.drools.org/.
- The navigation bar at the top of this site contains links to the overall umbrella community (KIE). Many of these links (but not all) are relevant to the business rules engine – for example, the Kogito project home page at https://kogito.kie.org/.
- More information on the corporate version of this product, Red Hat Decision Manager, can be found at https://developers.redhat.com/products/red-hat-decision-manager/overview, which is especially useful when making the business case to colleagues for using a rules-based solution.

These three links are the most likely to stay relevant, while the following links might change over time. If any of them are broken, you should be able to search for the updated one:

- Since the starting point for readers is KIE Sandbox, the resources listed on the main KIE page are probably best to look at first: https://kogito.kie.org/get-started/. This lists the main guides, examples, and technical documentation for the project.
- The Kogito documentation is updated with every release and goes into more detail about the features we highlight in this book. It’s available online at https://docs.kogito.kie.org/latest/html_single/.
- Remember that underneath, Kogito uses the Drools rules engine to power the decision services. There is a lot of rule engine documentation on the site at https://www.drools.org/learn/documentation.html. Over time, this documentation is becoming more business user friendly, but some of it may be more suitable for technical developers.
