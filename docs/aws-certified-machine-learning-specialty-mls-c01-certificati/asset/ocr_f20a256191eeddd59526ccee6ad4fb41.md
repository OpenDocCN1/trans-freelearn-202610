Next, we will create a co-occurrence matrix, which will count the number of occurrences of each pair of words, according to the pre-defined context window. Let's see this in action. Consider the following text: "I will pass this exam, you will see. I will pass it".

The context window of the first word "pass" would be the ones in bold: "I will pass this exam, you will see. I will pass it". Considering this logic, let's see how many times each pair of words appears in the context window:

| | I | Will | Pass | This | Exam | You | See | It |
|---|---|---|---|---|---|---|---|---|
| I | 0 | 3 | ... | ... | ... | ... | ... | ... |
| Will | 3 | 0 | ... | ... | ... | ... | ... | ... |
| Pass | ... | ... | 0 | ... | ... | ... | ... | ... |
| This | ... | ... | ... | 0 | ... | ... | ... | ... |
| Exam | ... | ... | ... | ... | 0 | ... | ... | ... |
| You | ... | ... | ... | ... | ... | 0 | ... | ... |
| See | ... | ... | ... | ... | ... | ... | 0 | ... |
| It | ... | ... | ... | ... | ... | ... | ... | 0 |

Figure 3.26 – Co-occurrence matrix

As you can see, the pair of words "I will" appears three times when we use a context window of size 2:

+   1. *I will* pass this exam, you will see. I will pass it.
+   2. I will pass this exam, you *will* see. I will pass it.
+   3. I will pass this exam, you will see. *I will* pass it.

Looking at the preceding table, the same logic should be applied to all other pairs of words, replacing "..." with the associated number of occurrences. You now have a numerical representation for each word!