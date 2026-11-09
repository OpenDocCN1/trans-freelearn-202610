Best Practices – Technical

+   - The value of the `this` variable should not be changed.
+   - The reusability should be maximized. E.g. rather than repeating lines of code at different places, a single method can be written so that changes in the method can be reflected at all the places where this method is used.
+   - There should be only one successful return point (except in switch statements) so that object deletion, etc. can be ensured.
+   - A method should perform a single well-defined task and be named according to the task performed.

# Text Constant Standards

All the text used in Dynamics AX is supposed to be in a label file irrespective of its use e.g. user interface or error or success message. The use of text constants can be classified into two broad categories i.e. user interface text and system-oriented text.

The text used in the user interface should follow the following best practices:

+   - Modify property values on the extended data types or base enums in the application.
+   - Never create duplicate label files i.e. the same text (in the same language) but a different label file.
+   - New label files can be created when customizing the text, but it is always recommended to reuse the standard labels. However, it may offer a disadvantage – all the changes made to the SYS layer label files will be gone whenever an upgrade occurs. So the decision of customizing an existing label file or creating new label file should be taken carefully.
+   - User interface text (labels files) should be used in double quotes.

System-oriented text constants must be in single quotes.

# Exception Handling

The principle uses of exception handling include freeing system resources (e.g. memory through object deletion, closing database connection, etc.) and providing constructive information in the Infolog so that the user can prevent such erroneous conditions. The following are a few recommended best practices related to exception handling:

+   - A `try` or `catch` deadlock or `retry` loop should always be created around database transactions that can cause deadlocks.
+   - In the `retry` clause the values of transient variables should be set back to the values before `try`.