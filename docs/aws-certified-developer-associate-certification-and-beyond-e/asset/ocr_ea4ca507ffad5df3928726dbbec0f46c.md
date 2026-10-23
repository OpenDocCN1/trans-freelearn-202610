```
iaasacademycreator@IaasS-MacBook-Air sam-app % aws s3 sync website/ s3://todolistapp.com
upload: website/.DS_Store to s3://todolistapp.com/.DS_Store
upload: website/assets/img/logo.png to s3://todolistapp.com/assets/img/logo.png
upload: website/assets/.DS_Store to s3://todolistapp.com/assets/.DS_Store
upload: website/assets/img/stay2.jpg to s3://todolistapp.com/assets/img/stay2.jpg
upload: website/index.html to s3://todolistapp.com/index.html
upload: website/assets/img/fav.png to s3://todolistapp.com/assets/img/fav.png
upload: website/assets/js/main.js to s3://todolistapp.com/assets/js/main.js
upload: website/assets/css/style.css to s3://todolistapp.com/assets/css/style.css
upload: website/assets/img/steve-jobs.png to s3://todolistapp.com/assets/img/steve-jobs.png
upload: website/assets/img/bg.jpg to s3://todolistapp.com/assets/img/bg.jpg
upload: website/assets/img/head-1.png to s3://todolistapp.com/assets/img/head-1.png
upload: website/assets/img/banner-bg.png to s3://todolistapp.com/assets/img/banner-bg.png
iaasacademycreator@IaasS-MacBook-Air sam-app %
```

Figure 14.18 – The aws s3 sync command to upload website content to an S3 bucket

+   9. Now that the website files have been uploaded, navigate to the CloudFront management console in your AWS account. Click on the distribution ID created by CloudFormation.
+   10. Make a note of the distribution domain name and paste the URL into another browser tab.
+   11. You should find your new to-do list application coming soon website up and running, as per the following screenshot:

![](bbox=[0.13, 0.46, 0.85, 0.9])

Figure 14.19 – To-do list frontend static website – coming soon page