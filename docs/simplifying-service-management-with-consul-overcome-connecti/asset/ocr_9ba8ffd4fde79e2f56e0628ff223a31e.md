Next, we have the instance identity information and associated roles. This doesn't have any direct association with Consul but is required to enable the communication paths we need within AWS. The code is illustrated in the following snippet:

```
```hcl
resource "aws_iam_instance_profile" "instance_profile" {
  name_prefix = var.owner
  role        = aws_iam_role.instance_role.name
}

resource "aws_iam_role" "instance_role" {
  name_prefix        = var.owner
  assume_role_policy = data.aws_iam_policy_document.instance_role.json
}

data "aws_iam_policy_document" "instance_role" {
  statement {
    effect  = "Allow"
    actions = ["sts:AssumeRole"]

    principals {
      type        = "Service"
      identifiers = ["ec2.amazonaws.com"]
    }
  }
}

resource "aws_iam_role_policy" "metadata_access" {
  name   = "metadata_access"
  role   = aws_iam_role.instance_role.id
  policy = data.aws_iam_policy_document.metadata_access.json
}

data "aws_iam_policy_document" "metadata_access" {
  statement {
    effect = "Allow"
    actions = [
      "ec2:DescribeInstances",
    ]
    resources = ["*"]
  }
}
```
```