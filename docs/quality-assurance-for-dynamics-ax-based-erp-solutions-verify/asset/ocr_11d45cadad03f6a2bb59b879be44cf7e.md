# Classification for Security Bugs

The security bugs are classified as per STRIDE and hence it is critical to have correct categorization of bugs:

+   - **Spoofing:** If a user pretends to be another entity or person. Other terms for spoofing are impersonation, masquerading, or identity theft.
+   - **Tampering:** If there is an unauthorized modification of data by using techniques such as SQL Injection. Other terms for it may be (data) integrity.
+   - **Repudiation:** If a user denies having performed an action, and there is insufficient evidence to prove the contrary.
+   - **Information disclosure:** If information is accessed by an unauthorized person.
+   - **Denial of service:** If people misuse the functionality with an intention of creating stressed conditions so that other real users cannot use the system comfortably.
+   - **Elevation of privilege:** If a user gets more privileges than entitled.
+   - **Attack surface reduction:** If code access is reduced for users with least privileged such as anonymous users.

The defect management tool must have an additional option 'Not a Security Bug' to declare that an issue is not a security-related issue.

# Root Cause Analysis for Security Bugs

Root cause analysis must be carried out for all security bugs. Based on their origin, security bugs could be classified in one of the following types:

+   - Not a security bug.
+   - **Buffer overrun or underrun:** Occurs when the size of a variable is not large enough to contain a given value and the memory buffer is overwritten with extra data. Buffer overruns are one of the most common and damaging security risks. While designing or coding ensure that variables declared have sufficient size or precision to handle the maximum or minimum possible values. A few types of buffer overruns are heap overruns, stack-based buffer overruns, V-table and function pointer overwrites, and exception handler overwrites. The buffer overrun may result in access violation, instability, and code injection for malicious purposes.