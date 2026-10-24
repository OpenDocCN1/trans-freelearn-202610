# Example 1 – string extraction

Here is the first paragraph from the python_cheat_sheet_v1.pdf PDF file that we want to extract (the PDF file can be found in this chapter’s GitHub repository):

![](bbox=[0.15, 0.19, 0.82, 0.38])

Figure 9.1: First paragraph of text from the python_cheat_sheet_v1.pdf file

To extract the first paragraph, as seen in Figure 9.1, the regex should be applied to page1_source, and the content of the first paragraph is returned using Python’s list(), in this case, with an index of 0 - python_definition[0]:

```
python_definition = re.findall(r"^(.*?)Check\s*out", page1_source)
python_definition[0]
"Python is a beautiful language. It's easy to learn and fun, and its syntax is simpleyet elegant. Python is a popular choice for beginners, yet still powerful enough toto back ...... Whatever the goal, Python's design makes the programming experiencefeel almost as natural as writing in English."
```

In this example, we have learned how to identify content and apply regex to collect the desired content.

# Example 2 – tabular content

In this example, we will extract the tabular data found on page 3, as seen in Figure 9.2, of the GeoBase_NHNC1_Data_Model_UML_EN.pdf file (the PDF file can be found in this chapter’s GitHub repository), and create a CSV file: