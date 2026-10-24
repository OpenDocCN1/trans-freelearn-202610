Unit testing is a test that is typically done by the developer of the Digital Twin application.
Unit-testing scenarios for the Power BI-coded solution that was introduced in _Chapter 6_,
_Building the Digital Twin Prototype_, requires the developer to test each functional area of
the Digital Twin. The functional areas for the example solution used in that chapter are
shown in the following screenshot. Each block on the diagram is tested individually for
scenarios where the code would most likely break, starting with ADT, then the event grids,
and finally, the Power BI **user interface (UI)**, as shown here:

![](bbox=[0.127, 0.248, 0.8438, 0.5762])

Figure 7.6 – Unit testing a coded Digital Twin

A black-box test, in contrast, is done by the QA team, who evaluate the expected output
without understanding the code behind the solution. This would, for example, be
evaluating the results on the Power BI end-user interface by varying input values with
both expected and unexpected input values.

A Digital Twin solution that is created in a no-code environment follows a similar
functional-testing approach. The major difference is that we cannot perform unit testing
at the code level but rather at the functional blocks in the no-code environment.

# ## Testing a no-code Digital Twin platform solution

A no-code application development platform provides an end-user configuration
environment with limited coding capabilities. This is aimed at SMEs who understand
the business requirements of a Digital Twin application but do not have software
development skills.