With our first question, we wanted to find counts of the different paste sites mentioned by URL in posts and comments. The q1.php script and bar graph we made to visualize the data show that, at least in the test data, JSFiddle was the most commonly referenced of the six paste site URLs we looked at.

The second question was about whether paste site URLs were more prevalent in questions or answers. Our queries show that paste site URLs were about twice as likely to occur in questions as opposed to answers, but the numbers for both were very small, at least in our test set.

For the third question, we wanted to look for whether people were actually heeding the advice of Stack Overflow and posting code in addition to a paste site URL. In our test set, the queries show that 25 postings (out of 37) include both a paste site URL and the recommended accompanying source code. This is a rate of about 68 percent compliance.

There are many additional questions we could ask and answer at this point, and many exciting ways we could extend this simple study into something that could be even more interesting. For now though, we will focus on the storage and cleaning procedures needed to extend this project to use the full dataset.

# Moving from test tables to full tables

At the beginning of this project, we made a set of test tables so that we could develop our project in a stress-free environment using tables with only 1,000 rows each. Using small tables with manageable numbers of rows is important in cases where we are not sure that our queries will work as we want them to, or where we want to experiment with tricky joins, subqueries, weird regular expressions, and so on. At this point, though, if we feel good about the queries and scripts we have written, it is time to rewrite our procedures to use the full-size tables.

Here are the steps we will follow to move the project over to full-size tables:

+   1. DROP the test tables:

```
DROP TABLE IF EXISTS test_badges;
DROP TABLE IF EXISTS test_comments;
DROP TABLE IF EXISTS test_posts;
DROP TABLE IF EXISTS test_post_history;
DROP TABLE IF EXISTS test_post_links;
DROP TABLE IF EXISTS test_tags;
DROP TABLE IF EXISTS test_users;
DROP TABLE IF EXISTS test_votes;
```