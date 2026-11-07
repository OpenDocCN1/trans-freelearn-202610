# Limiting the length of each column

In the previous examples, we always saw the full contents of each column, as each column had the number of characters within the limit defined by $cfg['LimitChars']. This is a limit enforced on all non-numeric fields. If this limit was lower (say 10), the display would be as follows:

![](bbox=[0.18, 0.25, 0.82, 0.35])

This would help us see more columns at the same time (at the expense of seeing less of each column).

# Display options

In order to see the full texts, we will now make use of the Options slider, which reveals some display options. The option that concerns us at the moment is the Partial Texts/Full Texts pair; we can choose Full Texts to see all of the text that was truncated. Even if we elect not to change the $cfg['LimitChars'] parameter, there will be a time when asking for full texts will be useful (when we work with TEXT field type—more on this in Chapter 6).

# Browsing distinct values

There is a quick way to display all distinct values and the number of occurrences for each value for each field. This feature is available on the Structure page. For example, we want to know how many different authors we have in our book table and how many books each one wrote. On the line describing the field we want to browse (here author_id) we click the Browse distinct values icon or link.

![](bbox=[0.18, 0.68, 0.83, 0.8])