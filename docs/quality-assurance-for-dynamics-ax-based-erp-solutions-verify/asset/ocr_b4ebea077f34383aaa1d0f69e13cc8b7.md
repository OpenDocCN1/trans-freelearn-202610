# Stabilization Phase

This is the stage when software is almost in a stable condition (i.e. functionally complete) and can be reviewed as a system. At this stage the security push goes beyond the security reviews done in earlier stages. Conducting code reviews at this stage offers an opportunity to not only review the new code, but also the modified code and core dynamics AX code in new perspective of entirety.

In Dynamics AX the following activities are recommended by Microsoft:

+   - Review of features and functions specifically designed to improve the trustworthiness i.e. reliability, security, and privacy. Risk analysis done in design phase of the security development life cycle can be useful to identify such features and functions.
+   - Review of overall code in entirety: i.e. both new code and updated code. Impact on existing code should also be a consideration.

# Release Phase

The primary activity in release phase is conducting the Final Security Review (FSR). This activity is conducted a couple of months before implementing or shipping the product. The FSR should be conducted by an independent security team at a stage when non-security issues are minimum. The FSR should not only consider the security health of the product in scope, but also previously reported issues. The previously reported issues give a clue to the pattern of security issues and the other areas where this kind of issues are still not discovered. It may also help to understand the root causes and take corrective and preventive actions.

# Support/Servicing Phase

A software application cannot be 100% secure — and even if it is100% secure today, it will definitely be vulnerable tomorrow due to discovery of new kinds of attacks. A support system should be in place, which can respond to newly discovered security vulnerabilities. The response should include:

+   - Evaluating vulnerability and issueing advisories and/or updates.
+   - Conducting a post mortem of the vulnerabilities and conducting an analysis of the chances of similar kinds of vulnerabilities in the product and taking preventive action.
+   - Reporting newly identified vulnerabilities to the security team so that suitable test cases can be added for future release of the same or other products.