+   11. Like our previous example, the first time this is run, a lot of the supporting libraries will be downloaded into the image. You will get an update as each of the tests runs and passes (which is what we’d expect).
+   
+   12. You will get a summary of the multiple tests that ran, and details (in the **Surefire** reports files, which will be saved into a newly created Target folder).

As expected, there was nothing dramatic. Kogito executed our combined model using the parameters in the test, and the output matched what we expected for each line of our scenario.

> **No surprises here**
> 
> The KIE team runs these tests automatically before releasing the code, so it would have been a surprise if any of the tests had failed.
> 
> However, there have been a couple of instances where Codespaces upgrades to the latest version of Maven before the KIE team does. This shouldn’t, but occasionally does, cause a Maven build error. If you’re unlucky enough to encounter this problem, your options are as follows:
> 
> - To wait. The teams will resolve the issue once they move to the latest version of Maven. Normally, this is quite fast – days rather than weeks.
> - To set up VS Code on your laptop. While this is easy to do, and we give hints in *Appendix B* on how to do this, you will probably need technical support.
> - To change the version of Maven used by Codespaces. Typically, you will want a slightly older version than the one currently installed. Instructions are at `https://maven.apache.org/install.html`, but again we suggest support from a more technical colleague.

# What next from here – exploring other models and building your own

Now that we’re more comfortable in a slightly more technical rule-editing environment, you may want to read the following article, which gives more details on the example we just ran: `https://blog.kie.org/2021/01/how-to-use-a-pmml-file-in-dmn-editor-vscode.html`.

You may also wish to explore the test project further. There are other decision models and unit tests in this folder that are worth exploring. Finally, it’s worth taking a machine learning model that you’ve trained yourself and following the example to get KIE and Kogito to execute it yourself.

We promised two easier ways of executing the models that might suit other readers that don’t want to use VS Code. Let’s touch quickly on Business Central, which is very similar to the example we just ran.