```
{
    "SecurityGroupRuleId": "sgr-0b38ce903b21e47cb",
    "GroupId": "sg-06320d12095603e68",
    "GroupOwnerId": "643998996127",
    "IsEgress": false,
    "IpProtocol": "tcp",
    "FromPort": 8080,
    "ToPort": 8080,
    "CidrIpv4": "95.157.36.247/32"
}
]
```

With the AMI and security group ready, let's move onto creating the EC2 instance.

# Creating the test EC2 instance

+   1. With all prerequisites covered, we can proceed with creating the instance:

```
aws-devops-simplified:~/environment/chapter-3/packer $ aws ec2 run-instances --instance-type t3.micro --security-group-ids sg-06320d12095603e68 --image-id ami-0834874e33262e665 --region eu-central-1
{
    "Groups": [],
    "Instances": [
        {
            "AmiLaunchIndex": 0,
            "ImageId": "ami-0834874e33262e665",
            "InstanceId": "i-00f089cfe084be59a",
            "InstanceType": "t3.micro",
            "LaunchTime": "2022-11-25T10:51:15+00:00",
            "Monitoring": {
                "State": "disabled"
            },
            "Placement": {
                "AvailabilityZone": "eu-central-1c",
            ...
```

+   2. Once the instance has been created, you can extract the public IP address to test the web application in your browser (Figure 3.5):

```
aws-devops-simplified:~/environment/chapter-3/packer $ aws ec2 describe-instances --region eu-central-1 --profile aws-devops-simplified
...
```