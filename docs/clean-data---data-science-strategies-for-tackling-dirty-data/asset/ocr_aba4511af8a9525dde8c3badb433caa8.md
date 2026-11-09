If we view the HTML source for the web page, we can locate a few lines of the nearly 1,000 lines of HTML code that comprise the data and layout instructions for the browser to show that particular weather table:

```
806 <div class="wx-temperature-label">FEELS LIKE
807 <span itemprop="feels-like-temperature-fahrenheit">43</span>&deg;</div>
808 </div>
809 <div class="wx-data-part">
810 <div class="wx-temperature">43<span class="wx-degrees">&deg;</span></div>
811 <div class="wx-temperature-label">HIGH AT 1:25 PM</div>
812 </div>
813 <div class="wx-data-part">
814 <div class="wx-temperature">28<span class="wx-degrees">&deg;</span></div>
815 <div class="wx-temperature-label">LOW</div>
816 </div>
```

Unstructured indeed! Of course, it is technically possible to pull the data value 43 (for the temperature in Fahrenheit) from this page, but it's not going to be a fun process, and we have no guarantee that our method to do that will be the same tomorrow or the day after that, as http://weather.com could change the source code for the site at any moment. Nonetheless, there is a ton of data on the Web, so in Chapter 5, Collecting and Cleaning Data from the Web, we cover a few strategies to extract data from web-based, unstructured files.

# Archiving and compression

When is a text file also a binary file? When it's been compressed or archived, of course. What are archiving and compression? In this section, we'll learn what file archives and compressed files are, how archiving and compression work, and the various standards for each.

This is an important section as a lot of real-world data (especially, delimited data) will be compressed when you find it out in the real world. What are the most common compression formats that you as a data scientist are likely to run into? We will definitely find the answer to this question. You might also want to compress your data when you share it with others. How can you figure out which compression method is the best choice?

# Archive files

An archive file is simply a single file that contains many files inside it. The files inside can be either text or binary or a mixture of both. Archive files are created by a special program that takes a list of files and changes them into a single file. Of course, the archive files are created in such a way that they can be expanded back into many files.