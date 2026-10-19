Our self-driving car example highlights two more benefits of the rules-based approach:

+   - The rules we are applying are pretty clear – they’re not hidden in obscure code or buried in the weightings of a neural network. This clarity extends to bias. If there is bias in our rules (they are written by humans after all), we can at least examine and challenge them. This is not always true for other types of AI, especially if they are trained on biased data.
+   - We can almost guarantee that certain business rules will apply in certain situations. The *almost* but not certainly being able to guarantee the behavior is due to the interplay of different business rules. But we can test for this and modify our business rules as appropriate.

Both these strengths address the weaknesses of the neural network AI approach in particular. This is not a book written for Google or Tesla AI engineers, but if you are one, consider using rules-based AI as a safety net underneath the systems you are developing!

As great as the rules-based approach to AI is, we still need something to run these rules (we’ll talk more about rule engines shortly). While we haven’t yet addressed some of the problems we highlighted previously (for example, change tracking), we’ll see later how the tools around a rule engine will help us solve that problem as well. Before that, it’s worth looking more at what a business rule is in more detail, as it’s core to this approach to AI.

# Practical artificial intelligence and business rules

The preceding business rule examples deliberately used a when . . . then format. This was no accident; the format gives clarity about the knowledge we are trying to share with the AI system.

# The when ... then format

Let’s highlight the key format for writing business rules:

```
When "something is true" then "take some action"
```

This is a very simple concept, so simple that if you’re an Excel power user (or have programmed in other languages), this is something that might trip you up.

In Excel, the flow is important – one cell references another, which might reference a third cell – but the order in which the values will be calculated is guaranteed. Likewise, in computer languages such as Java or VBA, statements are executed strictly in the order they are written (with options to loop and call functions).

A business rule can activate any time the "when" part is true – there is no guarantee of the order the rule statements will fire in.

The reminder is that we use when not if – when the first part of the rule is true, we should try to ensure that the then action is taken. Most programmers will struggle with this subtle difference – they are used to controlling if the code should branch, instead of trusting the rule engine to decide when something is true and that it should be executed on their behalf.