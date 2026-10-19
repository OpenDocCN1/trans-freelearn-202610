10. Maven will automatically download the testing libraries needed and run the tests. Your build success method now includes the number of tests run and their status, as in Figure AB.9.

```
[INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
[INFO]
[INFO] -------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] -------------------------------------------------------------
[INFO] Total time: 01:42 min
[INFO] Finished at: 2022-12-13T20:45:46Z
[INFO] -------------------------------------------------------------
```

Figure AB.9 – Successful test run

11. If you need more information on the success or failure of each test, look in the newly created target folder. A subfolder within that, surefire-reports, will contain details of each test.

While it’s great to have tests running in our project, we still don’t have Scenario Simulation tests running yet. Let’s fix that now.

# Running scenario testing in VSCode

Setting up the scenario testing to be included in these automatic tests requires some final small steps:

+   1. Our decision models (dmn files) are currently in the root folder of the project. To be included in our Maven testing, they need to be in the sample-kogito/src/main/resources folder. Drag and drop the files across to this location.

> Moving the decision model files is important
> If you don’t move the decision files, most of the following steps will appear to work, but the actual tests won’t run correctly.

+   2. To let the automatic tests know that they should run Scenario Simulation, create the KogitoScenarioJunitActivatorTest.java file in the sample-kogito src/test/testscenario folder.

+   3. Open this new file in the editor and put the following contents in – this will automatically pick up and run any scesim (Scenario Simulation) tests:

```
package testscenario;

@org.junit.runner.RunWith(org.kogito.scenariosimulation.runner.KogitoJunitActivator.class)

public class KogitoScenarioJunitActivatorTest {}
```