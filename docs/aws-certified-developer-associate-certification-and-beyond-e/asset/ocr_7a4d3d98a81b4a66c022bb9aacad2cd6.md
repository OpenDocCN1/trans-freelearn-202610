```
"ecr:CompleteLayerUpload",
"ecr:UploadLayerPart",
"ecr:InitiateLayerUpload",
"ecr:BatchCheckLayerAvailability",
"ecr:PutImage"
],
"Resource": "arn:aws:ecr:[REGION NAME]:[Account ID]:repository/[ECR IMAGE NAME]"
},
{
"Sid": "VisualEditor1",
"Effect": "Allow",
"Action": "ecr:GetAuthorizationToken",
"Resource": "*"
}
]
}
```

> **Note:**
> You can download this policy sample from our GitHub repository which is mentioned in the technical requirements section

+   6. Click **Next**.
+   7. Provide a name for your policy such as todolist-backend-policy-ecr.
+   8. Click the **Create policy** button.
+   9. Next, navigate back to the AWS CodeBuild console. Click on the **Start build** button, as per Figure 12.50:

![](bbox=[0.14, 0.64, 0.87, 0.81])

Figure 12.50 – Start build to test the build