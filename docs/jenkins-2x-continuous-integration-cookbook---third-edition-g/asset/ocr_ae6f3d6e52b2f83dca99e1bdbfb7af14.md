# Feature Person Repository

# Scenario Person Creation

# Steps

Given an empty repository 088ms

When I create a new Person named 'George' with the system 003ms

Then I should have Person named 'BadName' in the repository 000ms

```
java.lang.AssertionError
  at org.junit.Assert.fail(Assert.java:86)
  at org.junit.Assert.assertTrue(Assert.java:41)
  at org.junit.Assert.assertNotNull(Assert.java:712)
  at org.junit.Assert.assertNotNull(Assert.java:722)
  at com.damienfremont.blog.StepDefinitions.lambda$new$3(StepDefinitions.java:22)
  at *.Then I should have Person named 'BadName' in the repository(person-repository.feature:6)
```

# Scenario Outline Person Creation Examples

# Scenario Outline Person Creation Examples

14. The following graph shows the Steps Statistics for this build. The following list is based on the results:

# Steps Statistics

The following graph shows step statistics for this build. Below list is based on results. step does not provide information about result then is not listed below. Additionally @Before and @After are not counted because they are part of the scenarios, not steps.

| Implementation | Occurrences | Duration | Average | Ratio |
| StepDefinitions.java:14 | 1 | 088ms | 088ms | 100.00% |
| StepDefinitions.java:16 | 3 | 000ms | 000ms | 100.00% |
| StepDefinitions.java:18 | 4 | 003ms | 000ms | 100.00% |
| StepDefinitions.java:21 | 4 | 000ms | 000ms | 75.00% |
| 4 | 12 | 091ms | 007ms | Totals |

15. The Trends Statistics graph shows Features, Scenarios, and Steps for a period of time: