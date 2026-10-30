11. Navigate to the AWS S3 console page. Create a bucket, for example, rekognition-test-baba, as shown in the following screenshot:

![](bbox=[0.13, 0.15, 0.87, 0.45])

Figure 2.3 – AWS S3 console page

12. Click on Create folder and name it images. Click Save.

13. Click the Properties tab of our bucket.

14. Scroll to Events for that bucket.

15. Inside the Events window, select Add notification and set the following properties:

+   - Name: rekognition_event
+   - Events: All object create events
+   - Prefix: images/
+   - Send to: Lambda Function
+   - Lambda: lambda-rekognition: