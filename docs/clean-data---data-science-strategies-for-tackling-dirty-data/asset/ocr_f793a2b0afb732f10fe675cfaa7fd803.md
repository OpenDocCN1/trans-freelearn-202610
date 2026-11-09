+   2. Empty the cleaned_posts_code, cleaned_posts_urls, and cleaned_comments_urls tables as follows:
+   3. Edit the urlExtractor.php and codeExtractor.php scripts to SELECT from the posts table rather than the test_posts table. These queries can be edited as follows:
+   4. Re-run the urlExtractor.php and codeExtractor.php scripts so that they will repopulate the clean code and URL tables we emptied (truncated) earlier.

```
TRUNCATE TABLE cleaned_posts_code;
TRUNCATE TABLE cleaned_posts_urls;
TRUNCATE TABLE cleaned_comments_urls;
```

```
SELECT Id, Body FROM posts
```

At this point, we have the cleaned code and URL tables ready to be analyzed and visualized. Take your time when performing these steps, and know that many of these queries and scripts will likely take a long time to finish. The posts table is quite large and many of the queries we are writing are selected against a text column using wildcards.

# Summary

In this project, we posed a few questions about the prevalence of URLs on Stack Overflow, specifically those related to paste sites like http://www.Pastebin.com and http://www.JSFiddle.net. To get started answering these questions, we downloaded data from the Stack Overflow postings (and other Stack Overflow data as well) from the Stack Exchange public file release. We built a MySQL database and eight tables to hold this data. We then created smaller 1,000-row versions of each of those tables for testing purposes, populated with a randomly selected sample of the data. From these test tables, we extracted the URLs mentioned in each question, answer, and comment, and saved them to a new clean table. We also extracted the source code found in the questions and answers, and saved those snippets to a new table as well. Finally, we were able to build some simple queries and visualizations to help us answer the questions we posed at the beginning.

Despite its modest results, from a data cleaning perspective, our dinner party was a success. We were able to make a coherent plan, and take methodical steps to put the plan into action and alter it when needed. We are now ready for our final project, and a completely different dinner party menu.

In the next chapter, we will collect and clean our own version of a famous Twitter dataset.