Now that we’ve decided that workflow and business rules make a good end-to-end solution, let’s meet Power Automate and understand why we should choose it as our workflow tool.

# What is (and why) Power Automate?

Microsoft Power Automate (formerly known as Flow) is one of the hundreds of workflow offerings on the market – do a search for Workflow Engine if you want to look for yourself. So why does this book choose it, especially when Kogito also has a workflow engine built in?

+   - As a Microsoft product, Power Automate is highly integrated with Office, including Excel. The hosted version is available online, without setup, to most Office users.
+   - Power Automate is aimed at Excel Power users – a good crossover with the readership of this book. Meanwhile, Kogito’s workflow engine is more complex and is targeted at developers. And Power Automate strikes a nice balance – while easy to use, it is still more powerful than consumer-orientated services such as IFTTT.com.
+   - Power Automate follows the Business Process Model Notation (BPMN) standard, so many of the concepts you learn about in this chapter will be applicable to other graphical workflow engines. We’ll see this when we take a quick look at the Kogito workflow engine (jBPM) at the end of this chapter.

So, what is Power Automate? It’s a way of taking the workflow (as in Figure 8.1), adding the specific actions we want to carry out at each step, and then triggering (or starting) the workflow when specific conditions are true – for example, to start the workflow when we receive an email.

Because it is cloud-based, the workflows will continue to run when your laptop is turned off, even when you go on holiday. So, let’s take a look at Power Automate and create our first flow.

# Getting started with Power Automate online

Power Automate is linked from several areas of Microsoft Office, but the easiest place to start is the home page at https://powerautomate.microsoft.com/. After logging in (or creating a trial account if you need one), you’ll be greeted by a home/welcome page with a lot of sample flow templates, and pointers to learning materials on each of them.

It’s worth exploring these resources (now or later) – there are a lot of other Power Automate features outside the scope of this chapter. Instead, we’ll take a quick look around the screen before starting to build our first flow.

Click on My flows on the left-hand side of the home screen. You’ll see a screen similar to Figure 8.2, although obviously, you may not have any flows listed there (yet). This will be our main area for working with flows, so it’s worth taking a look around the screen.