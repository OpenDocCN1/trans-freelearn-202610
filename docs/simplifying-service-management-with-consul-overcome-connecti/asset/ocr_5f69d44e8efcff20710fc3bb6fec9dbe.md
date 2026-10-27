```
type          = string
description   = "machine instance type"
default       = "t3.micro"
}
```

If you would rather use your own values for the necessary variables, don’t worry—I won’t be offended. These variables are set within the terraform.tfvars file. Have a look at the following code snippet:

# Terraform.tfvars

```
owner         = "rjackson"
aws_region    = "us-east-2"
instance_type = "t2.small"
```

OK—now that we’ve set our variables to our liking, it’s time to fire up our infrastructure! If you’ve never used Terraform before, don’t worry—it’s quite easy, and all of the code has been built. Again, I never claimed to be a software developer, so please don’t laugh at my code! First, we’re going to kick off our infrastructure build and then dissect what we built. To get started, Terraform needs to download any provider-specific data necessary. This is performed via the following command:

```
$ terraform init
```

Of course, it’s never good to build something without examining what you build, so next, we’re going to plan our build. All of the output from this command should be green plus signs (+), identifying which infrastructure is going to be created. Note that many of these items are known after apply operation as they require assignment from AWS, but it might be fun to peruse what you are about to build. If not, don’t worry—I’ll review the details after we kick off the build.

Now, run the following command:

```
$ terraform plan
```

Next, we’re going to apply our infrastructure build. This is done using—you guessed it: terraform apply. This kicks off another plan and shows you what you are about to build, but since we’ve already done that, let’s live dangerously and auto-approve. Have a look at the following code snippet:

```
$ terraform apply --auto-approve
```