+   3. After this, write the entry point code that sets up the URL (for this exercise, we will use the text version of the **National Public Radio (NPR)** website) that should be used for your demo and the filename that should be used to write the PDF content to. Once that has been set, the code calls the `generate_pdf()` method to generate the content:

```
```python
if __name__ == '__main__':
    url = 'http://text.npr.org'
    pdf_file = 'demo_page.pdf'
    generate_pdf(url, pdf_file)
```
```

+   4. Now, to see the code in action, run the following command:

```
```bash
python3 pdf_demo.py
```
```

Once the command finishes executing, you will have a new PDF file named `demo_page.pdf` that has been saved in the same directory where the command was executed. When you open the file, it should resemble what you can see here:

![](bbox=[0.121, 0.396, 0.846, 0.842])

Figure 13.7: Web page converted into a PDF using weasyprint