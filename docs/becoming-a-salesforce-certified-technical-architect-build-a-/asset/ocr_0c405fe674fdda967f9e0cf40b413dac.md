Monolithic applications typically have three environments (development, testing, and production). In Salesforce, you can create more layers to provide an even more organized development and release cycle. The typical arrangement is development, CI environment (also known as the build environment), QA, staging, and production. Additional layers can be added, depending on the project's nature, such as integration testing, hotfix, and a dedicated training environment.

The following table can be used to help you select the right combination of environments for each task:

| Sandbox type | Developing new features | Testing | Integration testing/performance testing | Staging and UAT |
|---|---|---|---|---|
| Scratch | Excellent fit | Good (unit test, functional tests, UI automated test) | Not a good fit | Not a good fit |
| Developer | Excellent fit | Good (unit test, functional tests, UI automated test) | Not a good fit | Not a good fit |
| Developer pro | Good fit | Excellent fit for feature testing; data storage can be used for limited regression testing | Limited usage for integration testing; not a good fit for performance testing | Not recommended |
| Partial copy | Ensure that the data subsets are fine to be shared with the developers | Excellent fit for feature testing; data storage can be used for limited regression testing and production debugging (data subset) | Good fit for integration testing (limited data subset); not a good fit for performance testing | Good fit (limited data subset) |
| Full copy | Unusual; ensure it is fine to share data with the developers | Best used for production debugging | Excellent fit for integration testing; ideal for performance testing (considering that it has the same infrastructure as the production org) | Excellent fit |

Table 10.2 – Fitness description of different sandbox types for different tasks