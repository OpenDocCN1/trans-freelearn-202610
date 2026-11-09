# Blanks

Be aware that " " (two double quotes with a space in between, sometimes called a blank but more appropriately called a **space**) is not necessarily the same thing as "" (two double quotes right next to each other, sometimes also called a blank but more appropriately called **empty**). Consider the difference between these two MySQL INSERT statements:

```
-- this SQL has an empty for Sally's favoriteColor and a space for Frank's
INSERT INTO people (firstName, birthdate, faveoriteColor, salary)
VALUES ("Sally","1971-09-16","",129000),
       ("Frank","1975-10-23"," ",76000);
```

In addition to spaces, other invisible characters that sometimes get accidentally interpreted as empty or blank include tabs and carriage returns / line feeds. Be careful of these, and when in doubt, use some of the tricks introduced earlier in this chapter to uncover invisible characters.

# Null

I know that if you look up null in the dictionary, it might say that it means zero. But do not be fooled. In computing, we have a whole host of special definitions for NULL. To us, NULL is not nothing; in fact, it is the absence of even nothing.

How is this different than empty? First, empty can be equal to empty, as the length of the empty string will be 0. So, we can imagine a value existing, against which we can perform comparisons. However, NULL cannot be equal to NULL, and NOT NULL will not be equal to NOT NULL either. I have heard it suggested that we should all repeat as a mantra that *NULL is not equal to anything, even itself.*

We use NULL when we legitimately want no entry of any kind for the data value. We do not even want to put the pot on the stove!

# Why is the middle name example "empty" and not NULL?

Good question. Recall from the stew pot analogy that if we ask the question and the answer is empty, it is different from never getting an answer (NULL). If you asked me what my middle name was and I told you I did not have one, then the data value is empty. But if you just do not know whether I have a middle name or not, then the data value is NULL.