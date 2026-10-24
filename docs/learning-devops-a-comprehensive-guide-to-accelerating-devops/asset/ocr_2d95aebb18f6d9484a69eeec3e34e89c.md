# # 8 Deploying Infrastructure as Code with CI/CD Pipelines

In the first part of this book, we learned a lot about **Infrastructure as Code (IaC)**, its advantages, and tools such as Terraform, Packer, and Ansible, and their command lines. Then, in the second part, we discussed Git and dedicated a chapter to the Continuous Integration and Continuous Deployment of applications.

However, we should not neglect to also put CI/CD into practice as IaC, which will allow us to orchestrate and automate the whole process of provisioning the infrastructure.

It is in this second edition of this book that I wanted to add this chapter to reuse all that we have learned in the previous chapters and show how to implement IaC with a CI/CD pipeline, which will be composed of Packer, Terraform, and Ansible.