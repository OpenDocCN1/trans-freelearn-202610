![](bbox=[0.12, 0.09, 0.85, 0.37])

Figure 12.5 – Combined machine learning and decision model

5. Note that there are several other models in this folder, but we’ve chosen KiePMMLRegression.dmn as it’s the closest example to the model we trained in Chapter 11. The decision service has three input fields and then calls the embedded machine learning model to a prediction based on it.

> **Differences between VS Code and KIE Sandbox**
> Running a decision model in VS Code is different from KIE Sandbox. For example, there is no **Run... as Form** button to try out the model. Instead, we can either write Java code to call our model (which we will show one example of using the Red Piranha project later in this chapter) or we can use the scenario testing tool that we met in Chapter 9 and Appendix B.

6. Scenario testing is the easier option, especially since there is a scenario already set up in the KiePMMLRegressionTest.scesim file. You can either navigate through the folder structure on the left-hand side (test/resources/), or you can press Ctrl + P and start typing the filename to quickly jump to the file. The scenario test is pretty straightforward – the first three columns in light blue are input values, and the fourth column is a prediction of the value that we expect. That’s shown in Figure 12.6: