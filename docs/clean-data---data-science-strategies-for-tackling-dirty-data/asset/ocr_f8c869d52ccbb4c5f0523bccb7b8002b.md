+   - https://community.docusign.com/docusign/rss/board?board.id=Java
+   - https://community.docusign.com/docusign/rss/board?board.id=php_api
+   - https://community.docusign.com/docusign/rss/board?board.id=dev_other
+   - https://community.docusign.com/docusign/rss/board?board.id=Ask_A_Development_Question_Board

Visit each URL in that list in your web browser (or just one, if you are pressed for time). The file is RSS, which will look like semi-structured text with tags, similar to HTML. Save the RSS as a file on your local system and give each one a .rss file extension. At the end of this process, you should have at most seven RSS files, one for each preceding URL shown.

Inside each of these RSS files is metadata describing all the discussion threads on the forum, including the one piece of data that we really want at this stage: the URL for each particular discussion thread. Open one of the RSS files in a text editor and you will be able to spot an example of a URL we are interested in. It looks like this, and inside the file, you will see that there is one of these for each discussion thread:

```
<guid>http://community.docusign.com/t5/Misc-Dev-Archive-READ-ONLY/Re-Custom-CheckBox-Tabs-not-marked-when-setting-value-to-quot-X/m-p/28884#M1674</guid>
```

Now, we can write a program to loop through each RSS file, look for these URLs, visit them, and then extract the reply times we are interested in. The next section breaks this down into a series of smaller steps, and then shows a program that does the entire job.

# Step two – Extract URLs from RSS; collect and parse HTML

In this step, we will write a program that will do the following:

+   1. Open each RSS file that we saved in Step 1.
+   2. Every time we see a <guid> and </guid> tag pair, extract the URL inside and add it to a list.
+   3. For each URL in the list, download whatever HTML file is at that location.
+   4. Read that HTML file and extract the original post time and the reply time from each message.
+   5. Calculate how long it took to send a reply with mean, median, and standard deviation, like we did in Part 1.