```
]
},
"Action": "sts:AssumeRole"
}
]
}
```

13. Your trust relationships should now look like this:

![](bbox=[0.13, 0.25, 0.87, 0.39])

Figure 14.10 – IAM role creation – Updated trust relationship

When Amazon Lookout for Metrics will try to read the datasets you just uploaded in S3, it will request permissions from IAM by using the role we just created. The policy you defined to give access to your S3 bucket is what IAM will use to authorize Amazon Lookout for Metrics to access your dataset. On the other hand, the trust relationship you defined will allow other services (such as Amazon SageMaker) to run Amazon Lookout for Metrics APIs for you. You may have noticed that we also added the SNS service to the trust relationships.

Later in this chapter, you will use Amazon Simple Notification Service (SNS) to create a notification when severe anomalies are detected. We will use this trust relationship to give Amazon Lookout for Metrics the ability to call Amazon SNS APIs.

You are now ready to connect your dataset to Amazon Lookout for Metrics.