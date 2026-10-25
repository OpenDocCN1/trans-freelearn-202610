# Considering test automation as a software project

If you see automated testing as a software project and apply well-known principles, then you will save on maintenance costs and increase the reliability of tests.

The **Don't Repeat Yourself (DRY)** principle is a great example. Under time pressure, it is tempting to cut and paste similar tests from one area of the code base to another, don't. Projects evolve bending the shape of the code base; the tests need to be reusable to adapt to that change. Fragile tests push up maintenance costs. If you separate the code into pages, then when the workflow between pages changes, most of the testing code remains intact.

The **Keep It Simple Stupid (KISS)** principle implies keeping every aspect of the project as simple as possible. For example, it is possible to use real browsers for automated functional tests or the HtmlUnit framework to simulate a browser. The second choice avoids the need to set up an in-memory X server or VNC (http://en.wikipedia.org/wiki/Virtual_Network_Computing) and will also keep track of browser versioning. These extra chores decrease the reliability of running a Jenkins job, but do increase the value of the tests. Therefore, for small projects, consider starting with `HtmlUnit`. For larger projects, the extra effort is worth the cost.

Consider if you need a standalone integration server or if you can get away with using a Jetty server called during the integration goal in Maven.