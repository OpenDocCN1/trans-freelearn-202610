+   12. Lastly, before running the test, make Cypress skip the simple test we wrote earlier by calling skip, like so:

```
it.skip("should display login page", () => {
```

The preceding code changed the simple test from run to don't run.

The entire code for the anti-heroes.cy.ts spec file can be found at https://github.com/PacktPublishing/Spring-Boot-and-Angular/blob/main/Chapter-15/superheroes/cypress/e2e/anti-heroes.cy.ts.

Now, we can run the anti-heroes.cy.ts spec file to see whether everything will pass, as shown in the following figure:

![](bbox=[0.213, 0.322, 0.77, 0.55])

Figure 15.8 – Passing tests

Figure 15.8 shows that the should display login page test was skipped, while the rest of the tests passed.