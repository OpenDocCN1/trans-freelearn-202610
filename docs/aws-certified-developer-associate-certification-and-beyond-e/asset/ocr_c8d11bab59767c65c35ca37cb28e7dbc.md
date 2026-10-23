```
"Resource": "arn:aws:s3:::REPLACE_WITH_BUCKET_NAME_HERE/*"
}
]
}
```

+   1. In the Amazon S3 console, click on **Buckets** from the left-hand menu.
+   2. Click your bucket name in the right-hand pane to access its configuration details.
+   3. Select the **Permissions** tab from the right-hand pane, as shown in the screenshot:

![](bbox=[0.13, 0.27, 0.85, 0.46])

Figure 3.13 – The Permissions tab of the Amazon S3 bucket

+   4. Click the **Edit** button under the **Bucket policy** section.
+   5. Replace any default policies with the preceding policy, ensuring that you use the correct ARN for the bucket.
+   6. Click **Save changes** at the bottom of the page.
+   7. You should get a success message confirming the application of the new bucket policy.

# Project Task 3.4 – Configure Static Website Hosting

In this task, you will configure static website hosting for your Amazon S3 bucket. This will allow your web application to be hosted and accessed publicly from the bucket.

+   1. While in the configuration pane of the Amazon S3 bucket, click on the **Properties** tab.
+   2. Scroll toward the bottom till you get to the **Static website hosting** section.
+   3. Click **Edit**.