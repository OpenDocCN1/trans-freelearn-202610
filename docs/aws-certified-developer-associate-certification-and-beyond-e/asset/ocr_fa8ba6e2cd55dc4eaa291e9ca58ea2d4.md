# Project Task 9.4 – Creating a CloudFront Distribution

In this task, you will create a CloudFront distribution that will be used as the entry point to your website and application. Amazon CloudFront will help distribute your static and dynamic content over high-speed networks through AWS’s globally distributed network of edge locations and proxy servers, which cache content and reduce latency:

+   1. Ensure you are logged into your AWS Development account and working in the N. Virginia region.
+   2. Use the search bar at the top of the management console page to search for and navigate to the Amazon S3 web console.
+   3. Click the bucket that hosts your **Todo List** static website.
+   4. From the **Properties** tab, make a note of the static website hosting URL. This is the URL you use to browse your frontend website.
+   5. Next, from the search bar at the top of the web page, search for `CloudFront` and navigate to its web console page in another browser tab.
+   6. From the right-hand pane, click the **Create a CloudFront distribution** button.
+   7. Paste the S3 bucket’s static website URL in the search box under **Origin domain**. If you click on the search box, it will list all possible endpoints in your account that could be used for configuring a CloudFront distribution. However, for Amazon S3 static website hosting, you must use a website endpoint and not simply a bucket endpoint.
+   8. Provide a name in the **Name** text box of your choice if you are not satisfied with the default name chosen for you.
+   9. Ensure that **Public** is selected under **Origin access**.
+   10. Next, scroll down till you reach the **Web Application Firewall (WAF)** section. Here, select the **Do not enable security protection** option. For the purposes of this lab, you are not configuring any WAF security for your application.
+   11. Next, scroll to the bottom of the page and click the **Create distribution** button. The distribution will take a few minutes to deploy fully, as per the following screenshot:

![](bbox=[0.145, 0.67, 0.87, 0.85])

Figure 9.24: The CloudFront distribution deployment status