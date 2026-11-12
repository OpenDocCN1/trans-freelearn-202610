+   • Read XPS With OCR: This option enables a user to read all the characters of an XPS file using an OCR engine. This activity is similar to the **Read PDF with OCR** activity.

Now that you have an initial idea of the activities available in the PDF package, we will see how we can use some of these activities. In the next section, we will focus on using some of the activities that have been discussed to generate some output.

# # Extracting text from a PDF using the Extract PDF Text and Extract PDF with OCR activities

Data extraction from PDF files can be done in columns of text or on individual elements. First, we will look at how we can extract all of the data from a page. Using the **Read PDF Text** and **Read PDF with OCR** activities enables a user to read large text files. A page of a PDF file might contain computer-generated text and also text in image format. Once the file is open in Adobe Reader, you can easily highlight the computer-generated text individually, whereas the text in image format gets highlighted as a section. Find a PDF file that contains such text and try to use it for our assignment. Let's try to build a simple workflow with the previously mentioned activities by going through the following steps:

+   1. Open the UiPath Studio solution that you created in the previous section.
+   
+   2. Open the `Main.xaml` workflow file to start editing the workflow.
+   
+   3. Drag and drop the **Read PDF Text** activity from the **Activities** panel by searching for it.
+   
+   4. Select the activity to view the properties of the activity.
+   
+   5. Provide the full file path for the **FileName** property of the activity.
+   
+   6. Create a string variable in the **Output Text** property to hold the extracted text by pressing `Ctrl + K`.
+   
+   7. By default, the **Range** property is set to `All`. The default value of `All` means that it extracts text from all pages of the document. However, if you only want to extract data from a specific page or a range of pages, you can define that using this property. If you are going to provide a particular page, you can enter the page number. If it is a range of pages, you can provide the starting page and the ending page separated by a "-", for example, `12, 15, 20-24`. For our scenario, we will use the default value.