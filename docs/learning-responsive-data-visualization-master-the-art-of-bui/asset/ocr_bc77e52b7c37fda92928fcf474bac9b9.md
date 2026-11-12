If we start the tests, it runs and hopefully finishes without errors like in the following figure:

```
ckoeerner@koerner-ThinkPad-W550s:~/Dropbox/msc-private/Projekte/Responsive Data Visualizations with Bootstrap and D3.js/chapter09/codes$ protractor protractor-conf.js
[11:27:24] I/direct - Using the selenium server at http://localhost:4444/wd/hub
[launcher] Running 1 instances of WebDriver
Started
..
1 spec, 0 failures
Finished in 1.605 seconds
[launcher] 0 instance(s) of WebDriver still running
[launcher] chrome #01 passed
ckoeerner@koerner-ThinkPad-W550s:~/Dropbox/msc-private/Projekte/Responsive Data Visualizations with Bootstrap and D3.js/chapter09/codes$
```

Passing all the tests in Protractor

Now would be a good point to create Integration test files that contain a single visual component as well as tests for this component. Try to add scrolling, hovering, and clicking to break the visualization.

> You can find more information about Protractor, its configuration, and API on the Angular website at https://angular.github.io/protractor.

# Running your tests in the cloud

Now comes the master class; you can write a script to run your complete Integration tests on every push via TravisCI. Take a look at the steps in https://docs.travis-ci.com/user/languages/javascript-with-nodejs to get started with Travis and JavaScript.

However, instead of running your browsers in the virtual machine of Travis, you can add a service such as Browserstack to run the tests on multiple devices using multiple resolutions in the cloud.

I want to mention another great Service for automated device testing, which is a great alternative to Browserstack and free for open source projects—Sauce Labs.

> You can find more information about Sauce Labs on their website at https://saucelabs.com/.