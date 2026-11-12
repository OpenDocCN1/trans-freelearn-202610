# A testing strategy for visualizations

Now, we have enough information to build a solid testing strategy for responsive visualizations. Note that this is a very opinionated setup based on my experience, and feel free to exchange any layer or tool with your favorite ones. However, keep the general ideas for your workflow.

First, we will use the Chrome browser on the desktop PC while developing. We either use a normal desktop window or we switch to a mobile view in the device mode. This depends on the target screen size and resolution. Personally, I prefer to work in a desktop Chrome and debug and test on mobiles.

While developing, we need to regularly do some manual testing to make sure our visualization looks and feels nice. Running emulators for different operating systems locally is a pain; that's why I prefer syncing a real mobile phone and a tablet via Browsersync to my current Chrome window. If you don't have access to real devices, you can also use Browserstack on your local project.

For unit tests, we need to start a Karma unit runner before we start making changes. This will watch source files for changes, fire up a PhantomJS instance, and run the complete test suite whenever a file is saved.

Once we push a commit to Github, TravisCI automatically runs all unit tests locally and integration tests on different mobile devices using Sauce Lab. This will also create different images for the visual regression test and show them to the developer.

Finally, a feature or fix can be merged into the master when all tests have passed, and the developer accepts the different images of the visual regression.

# Summary

This chapter gives a solid overview on different testing strategies for testing responsive visualization components. In the first section, we spoke about the testing strategies, costs, and benefits of testing and the difference between local, remote, manual, and automated tests.

Then, we go through different testing strategies and understand how they can be implemented for the visualization. We cover manual testing using Browsersync and Browserstack, as well as automated testing using TravisCI, Karma, Protractor, and Selenium. Finally, we will build our perfect testing strategy for visualization components.

In the next chapter, you will learn about detecting and solving cross-browser issues when creating responsive visualizations in SVG.