# Building the Consul cluster

Start by changing directories to the root directory of ch2. This is where we will find the Terraform scripts for building our cluster. You can do this by running the following command:

```
$ cd $github_root/Consul-Up-and-Running/ch2/
```

Much like when building our Packer image, we have a few files in this directory to be aware of. Don't be concerned about the files directory just yet, as we'll get into that as we evaluate the Terraform script for building our cluster.

If you take a look at our variables.tf file, this is where the variables that Terraform requires are defined. I always get confused between variables being defined and where they are set. Anyhow, for each of the five variables, there is a description (always a good idea) and a reasonable default (well, reasonable from my perspective). Have a look at the following code snippet:

# variables.tf

```
variable "aws_region" {
  type        = string
  default     = "us-east-1"
  description = "Region for AWS Components"
}
variable "owner" {
  type        = string
  default     = "consul-demo"
}
variable "consul_region" {
  type        = string
  description = "Region of consul server (not AWS Region)"
  default     = "global"
}
variable "vpc_cidr" {
  type        = string
  description = "CIDR of the VPC"
  default     = "192.168.100.0/24"
}
variable "instance_type" {
```