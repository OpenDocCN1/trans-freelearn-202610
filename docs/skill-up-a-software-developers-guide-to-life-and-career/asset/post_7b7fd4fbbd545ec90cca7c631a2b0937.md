
of?". Lean on their experience to help guide your evaluation. Many times, bad legacy code isn't so much the result of an incompetent developer, but of poor project management and deadline-driven developer pressure.

## What are your thoughts on refactoring a code base Skill Up: A Software Developer's Guide to Life and Career starting over from scratch?

Resist the need to rewrite everything so it's perfect. We've all inherited some nasty codebases and if we're going to be honest about it, we've all been that culprit more than once in our careers. But don't rewrite for the sake of rewriting. That's irresponsible.

If you're going to re-write from scratch, make sure you're doing it for the right reasons. We all want to work on brand new projects using the most recent versions of every new technology framework. It's much more enjoyable.

Sometimes it's not the best thing for the project and the client though. Make sure your decision is justified by more than "you wanting to do it". Sloppy code can be cleaned up. Tests can be added. Versions can be upgraded. Old code with poor app architecture riddled with brittle, unstable features and a failure to use any general best practices or third-party tools can be a good justification for rewriting. Lack of understanding of a confusing app is not a justifiable reason.

## When is the best time to work on fixing poorly written code?

Just like re-writing an application from scratch, it's irresponsible to leave messy code that every developer on the team is going to touch. There's a time to let bad code be. But not when you're tripping it over it every time you work on the app.

## Tips for taking over a legacy application

While there are a number of techniques you need to apply to work with a legacy application, the first should be building a comprehensive test suite.

## Creating a test suite

No matter what language or framework that you work in, you will be able to create automated tests that capture the functionality of the application. So, in the legacy application I was working on I started creating tests for each model.

I began with basic unit tests and then started branching out to integration tests that ensured that the various elements of the codebase were communicating properly with each other. Going through this process had the added bonus that I became more familiar with the structure of the app and I could refactor the code as I implemented the tests.

## Adding new features via TDD

Once the test suite was built, I started building all new features via the TDD (test-driven development) method, which ensured that the test suite was up to date. By utilizing this process, it also made it possible to ensure that the new features that I added wouldn't break pre-existing functionality. This is called regression testing.

## Breaking out specific features into microservices

The further I got into the codebase, I started to notice that the app had become bloated with features, and many of the components didn't need to be included in the core application.

Therefore, I slowly started creating microservice applications that handled isolated pieces of functionality. Some examples were: creating a microservice that managed the user notification system and building an app that processed the reporting engine. After creating the microservices, I could get rid of significant portions of the legacy code and then simply wire up the legacy application with the new microservices so they communicated properly.

## DRY up the codebase

In many legacy applications, you'll run into duplicate code that causes a number of problems, including the issue of having to make one change in multiple places in the codebase.

An example of this was how the application I was working on dealt with view templates. There were a number of view files with identical HTML code. I could refactor these components into partials that could be shared across the application, which allowed me to make a single code change that would populate throughout the app.

The topic of taking over a legacy application is important to understand, not only for the reason of being prepared for what steps you need to take to work on a legacy app, but also so you will have a better idea of how to build applications from scratch.

Remembering the Eventbrite application that I mentioned earlier, that application was easy to work with and add features to because it had been built from day one using each of the techniques mentioned in this chapter.

If you develop an application from scratch using these best of breed techniques, you will make it easier on yourself when you're adding features in the future. It will have the added benefit that any new developers that may work on the application in the future will be able to start adding new features easily and they'll appreciate the extra work you put into the development process.

## Summary

I hope that this has been a helpful guide for taking over a legacy application and that you can apply it on the projects that you're working on.

## Starting over versus refactoring

When I was a young and naive freelancer I dreaded the idea of refactoring a legacy application. The second that the client brought up the possibility of starting over from scratch, I jumped on it!

However, the more projects I work on, the more my mindset has changed. Over the years I've put together a system to help me decide between starting over versus refactoring a project. And that's what I'm going to walk through here. The steps I follow are:

- 1. Removing the fear factor.
- 2. Analyzing the 80/20 principle.
- 3. Building an automated bug list.
- 4. Becoming the client.

## #1 – removing the fear factor

One of the reasons why inexperienced freelancers tend to opt for starting a project over is because of fear. And fear is rarely a good reason to make any decision (unless you're running away from a wild animal or something like that). So, before I make a decision, the first task I perform is taking fear out of the equation. I'll ask myself:

> "If you weren't afraid of the unknown issues with this codebase, what would your decision be?"

Once fear has been removed I can look at the project from an unbiased viewpoint.

## #2 – analyzing the 80/20 principle

The 80/20 principle has a number of practical ramifications. You may have heard it being used to say that 20% of the people make 80% of the wealth. Or that 20% of a customer base generates 80% of a company's revenue. However, I've also seen that the 80/20 principle can work well for deciding between starting over versus refactoring.

## When should you start over?

So far, this chapter has heavily favored refactoring an application. However, there are times when starting over from scratch is a better approach. Some of the rationales that make starting over a wise decision for the client are:

- **A complete change in architecture**: I have had clients who requested that I migrate an application from being a monolith (a large single application) to becoming microservice-based (a number of applications that each perform a single feature). In cases like this, it wouldn't make any sense to try to keep the legacy application since the core application structure would have to change.
- **Moving to a different language/framework**: Over the years, I've had a number of clients who had old ASP .NET and PHP applications approach me to rebuild their systems in Ruby. When it comes to changes languages or web frameworks it wouldn't be possible to retain the legacy application.

Notice how both these key reasons had nothing to do with bugs or messy code? The only time I'd recommend for a client to start over from scratch is if it's literally impossible to retain the legacy application.

## Summary

In summary, the key to remember is to go through the system of checks whenever you're asked to decide between starting over versus refactoring. The more experienced you get as a developer, the more you'll realize that it's incredibly rare that a functioning legacy application cannot be saved.
