# Severity

The severity should be assigned as per the impact of the issues from a technical perspective. The following are the recommended severity levels in the IBI specifications for Dynamics AX:

+   - **Severity 1 (Must Fix)**: These are fixes related to system crash or information loss. A few examples of this category are application crash, product instability, major test blockage, high security risk, broken builds, or failed build verification tests, application security failures and trustworthy computing failures, data loss, corruption, or misrepresentation, memory leaks, etc.
+   - **Severity 2**: This severity is assigned when a major part or the entire feature is unusable, or a small part of the feature is unusable but the workaround is very complex. This may also be a severity level where a bug can affect customer or testing significantly. A few examples are:
+     - A telephone number field (which it is mandatory to fill) can accommodate only 5 characters while real-life phone number at customer locations are 7 digits.
+     - Functionality to encrypt sensitive information such as credit card numbers is missing.
+   - **Severity 3**: This severity level should be assigned when a minor feature problem exists and has workaround. The customer or test should not be affected significantly due to the bug.
+     - A field accepts more than two characters for ISD code, but the ISD code for the customer's country is single digit. The workaround may be to put 00X rather than X.
+   - **Severity 4**: Very minor issues such as typo errors (without giving wrong information), incorrect tab order, etc.

Some people are confused between severity and priority and often consider that a bug of high priority is also of high severity, which is not true. For example:

+   - The help file is missing for a newly added feature:
+     High Priority (a user cannot use the functionality as the help file is the only way to know about the new feature).
+     Low severity (it is not a information loss or functionality missing; from a technical perspective it is not so damaging).
+   - Interest calculation is wrong in the year 2100 due to incorrect leap year calculation at the end of a century:
+     Low priority (Customer is not affected today).
+     High severity (the calendar algorithm may need a change).