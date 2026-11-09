*Best Practices – Trustworthy Computing*

+   - Maintainable: It is easy to configure and manage.
+   - Resilient: It works despite changes in conditions.
+   - Recoverable: It is easily restored without adverse impact.
+   - Proven: It operates in a variety of environments.

The above qualities can be achieved with a multipoint strategy, which not only ensures higher reliability, but also continuous improvement in it:

+   - Designing for greater reliability such as interoperable design, fail-safe design etc.
+   - Engineering to offer more reliability such as code analysis for superior coding logic and removal of memory leakage
+   - Features to understand where the system fails or how the customer wants to use the system, etc. Windows error reporting is one such example

# Microsoft Security Development Life Cycle

To develop, deploy, and use software applications in a trustworthy way Microsoft recommends a Security Development Life Cycle. This lifecycle is based on SD3+C methodology.

# SD3+C Methodology

Microsoft has developed a methodology to combat real-world security issues. It consists of four pillars:

+   - Secure by design: Any ERP solution based on Dynamics AX should be designed, architected, and implemented in such a way that it can protect itself as well as the information being processed. Following are a few points that make for secure design:
+     - The ERP solution should be designed in such a way that it only accepts the user inputs that are supposed to be the expected inputs.
+     - Dangerous APIs should be handled in a secured way (as prescribed by Microsoft).
+     - Data or control commands should be validated at trust boundaries.
+     - Overall architecture should be secure, which can be achieved by threat modeling at design stage.