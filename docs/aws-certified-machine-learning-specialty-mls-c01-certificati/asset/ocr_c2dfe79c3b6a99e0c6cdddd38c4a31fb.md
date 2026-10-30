+   - Extracting text from documents with Amazon Textract
+   - Creating chatbots on Amazon Lex

Let's get started!

# Technical requirements

All you will need for this chapter is an AWS account.

You can download the code examples for this chapter from GitHub at https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-2.

# Analyzing images and videos with Amazon Rekognition

If you need to add powerful visual analysis to your applications, then Amazon Rekognition is the service to opt for. Rekognition Image lets you easily build powerful applications to search, verify, and organize millions of images. Rekognition Video lets you extract motion-based context from stored or live stream videos, and helps you analyze them. Rekognition Video also allows you to index metadata such as objects, activities, scene, celebrities, and faces, making video searches easy. Rekognition Image uses deep neural network models to detect and label thousands of objects and scenes in your images. It helps you capture text in an image, a bit like Optical Character Recognition (OCR). A perfect example is a T-shirt with quotes on it. If you were to take a picture of one and ask Amazon Rekognition to extract the text from it, it would be able to tell you what the text says. You can also perform celebrity recognition using Amazon Rekognition. I am not a celebrity, so I won't be using the celebrity recognition API for my face; instead, I will use the face comparison API.

The official documentation, available at https://aws.amazon.com/rekognition/faqs/, states the following:

> "With Rekognition Image, you only pay for the images you analyze and the face metadata you store. You will not be charged for the compute resources if, at any point of time, your training fails."

Some common uses of Amazon Rekognition include the following:

+   - Image and video analysis
+   - Searchable image library