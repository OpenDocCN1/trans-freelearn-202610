```
version: 0.2
phases:
  build:
    commands:
      - npm i && npm run build
      # Please change the s3 bucket URI
      - aws s3 cp --recursive dist/ s3://todo-plus-frontend/
      # Please change the distribution id
      - aws cloudfront create-invalidation --distribution-id E2A4WHGXZQDJJ8 --paths "/*"
```

Figure 12.6 – Sample buildspec.yml file

You will note that not all the phases in the buildspec.yml file need to be included. In the sample file in Figure 12.6, the commands instruct AWS CodeBuild to run a build task with the available source code, copy the artifact to the Amazon S3 bucket hosting the application’s frontend component, and invalidate the existing CloudFront distribution to ensure that the new application is made live and accessible via CloudFront.

If the build process is successful, AWS CodeBuild will deploy your code to your target environment. This process ensures that your code is deployed consistently and reliably.

Throughout the build and deployment process, you can monitor progress and view build logs in the AWS CodeBuild console, as per Figure 12.7. This allows you to stay up to date on your code’s status and quickly address any issues that arise during the build process.

![](bbox=[0.12, 0.43, 0.85, 0.64])

Figure 12.7 – Interface of AWS CodeBuild