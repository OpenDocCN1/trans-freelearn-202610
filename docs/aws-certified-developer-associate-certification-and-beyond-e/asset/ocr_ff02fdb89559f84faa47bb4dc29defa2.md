20. You will be taken back to the Specify permissions page, which should be similar to Figure 10.22:

![](bbox=[0.21, 0.135, 0.81, 0.485])

Figure 10.22: Configuring an inline policy to access the secret in AWS Secrets Manager

21. Scroll to the bottom of the page and click Next.
22. Provide a policy name such as Todo-EB-Secret-IAM-Policy.
23. Click Create policy at the bottom of the page.
24. Your inline policy is created and added to the IAM role you just defined.

With the IAM role and policies now defined, you can proceed to create your Elastic Beanstalk application and environment.