How can the script decide whether the grid has been added intentionally or it is a bug that occurs from a change in the code? It simply cannot do so. The script runs automatically; however, it requires a human interaction to pass the test; this is what we call a semi-automated test. Sometimes, (small) visual changes are made intentionally, and sometimes, they can be produced by bugs. It is impossible to automatically decide which change is intended.

In a good data visualization setup, we would generate a set of screenshots and provide pictures with the absolute difference from the previous version to the developer whenever he/she commits a change to the source code repository. To facilitate focusing on the most important changes, the script can only show images to the developer that has changed more than a certain threshold of pixels.

# ## Unit tests

Unit tests (or specs) aim to test the smallest units of your code for correctness; and hence, they provide an executable documentation of all the units of your code. While it is debatable what the smallest unit of your code is, in my opinion the smallest unit is a single function.

In data visualizations, it is a good technique to use a functional approach to design your components in such a way that every function receives an input, modifies it, and returns an output. A good strategy would be to write tests for all these functions.

Units test should only affect the code from the tested unit and not require any other units, so everything besides the tested unit should be mocked. Mocks are the copies of objects created specifically for testing that (synchronously) return a predefined result. Let's think about `XMLHttpRequest` in one of our functions. We can simply create a Mock that synchronously returns a predefined result for this request. The benefit is clear. It is much faster as we don't have to create a real request. It's also repeatable and doesn't depend on the real result of the `XHR`.

This brings us to the second most important statement about unit tests: they should run locally after each change in the source code and therefore be super-fast. Thus, we most often run the unit tests in a headless browser (without the graphics), such as **PhantomJS**, on our local development environment.

# ## Integration tests

Integration tests (or e2e) aim to test the integration of components from the position of a user interacting with the component. This means that they test the interplay of multiple components in a real browser for user interactions.