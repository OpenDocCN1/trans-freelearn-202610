# Step two – try pasting the copied data into a text editor

The following screenshot shows how the copied text looks when it is pasted into Text Wrangler, our text editor:

```
1  38 Prepare students to be productive
2  members of the workforce
3  27 Prepare young people to be responsible
4  citizens
5  21 Ensure that all qualified students have
6  4-year 4-year 2-year Private Public Private/Public
7  23 28 47 48 31 14 16 24 25
8  For profit
9  63 8 20
10 143
11  equal access to a college education
12 4 Contribute to the economic development 2582
13 of their region or locality
14 4 Conduct research to help solve medical, 5831
15 scientific, social, and other national
16 problems
17 2 Provide continuing education for adults 2113
18 of all ages
19 3 None is "very important"/No answer 4432
```

Clearly, this data is not in any sensible order after copying and pasting it. The page number is included, the numbers are horizontal instead of vertical, and the column headers are out of order. Even some of the numbers have been combined; for example, the final row contains the numbers 4,4,3,2; but in the pasted version, this becomes a single number 4432. It would probably take longer to clean up this data manually at this point than it would have taken just to retype the original table. We can conclude that with this particular PDF file, we are going to have to take stronger measures to clean it.

![](bbox=[0.21, 0.65, 0.26, 0.72])

> We should note at this point that the other areas of this PDF file do clean up nicely. For example, the Preface, a text-only section located on page 3 of the file, copies out just fine using the preceding technique. With this file, it is only the actual tabular data that is a problem. You should experiment with all parts of a PDF file—including text and tabular data—before deciding on an extraction technique.