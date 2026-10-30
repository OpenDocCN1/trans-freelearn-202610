+   6. Save the role as `language-translation-from-image`.
+   7. The next immediate step is to create a Lambda function. Navigate to **Lambda > Functions > Create Function**.
+   8. Name the function `language-detection-from-image`.
+   9. Set the runtime to `Python 3.6`.
+   10. Use an existing role; that is, `language-detection-from-image-role`.
+   11. Paste the code available at https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/blob/master/Chapter-2/Amazon%20Translate%20Demo/lambda_function/lambda_function.py and click **Deploy**. We will use the `translate_text` API to translate the input text.
+   12. The next step is to create a bucket called `language-translation-from-image`.
+   Create a folder named `image`. Then, navigate to **Properties > Events> Add notification**.
+   13. Fill in the required fields, as shown here, and click on **Save** (please make sure you select `.jpg` as the suffix; otherwise, it will trigger the lambda function for any object creation process):
+   14. Navigate to **Amazon S3>language-detection-image>input-image**. Upload the `sign-image.jpg` image into the folder. This file is available in this book's GitHub repository: https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-2/Amazon%20Translate%20Demo/input_image.

+   - **Name:** *translate-language-image*
+   - **Events:** `All object create events`
+   - **Prefix:** `image/`
+   - **Suffix:** `.jpg`
+   - **Send to:** `Lambda Function`
+   - **Lambda:** `language-translation-from-image`