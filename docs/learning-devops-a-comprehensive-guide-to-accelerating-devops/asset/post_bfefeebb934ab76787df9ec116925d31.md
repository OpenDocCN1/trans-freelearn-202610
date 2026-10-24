

Finally, when choosing a tool, you must make sure that it protects all the data that is captured, that the dashboards the tools present are understandable enough by all team members, and that it is integrated into a DevOps process.

We have explained that monitoring is a practice that must be integrated into a DevOps culture, taking into account some points of good practice that can improve communication between Dev and Ops and improve product quality for the end users.

After reviewing DevOps best practices with automation, CI/CD pipelines, and monitoring, we will take an overview of DevOps practices for project management and team organization.

## Evolving project management

We have previously discussed some good DevOps practices to apply to projects, but all this can only be implemented and realized with a change in the way that projects are managed and teams are organized.

Here are some good practices that can facilitate the implementation of a DevOps culture in project management within companies.

First of all, it should be remembered that a DevOps culture only makes sense with the implementation of development and delivery practices that will allow applications to be delivered in short deployment cycles. Therefore, in order to be applicable, projects must also be managed with short cycles. To achieve this, one of the most suitable project management methods to apply a DevOps culture that has proven its worth in recent years is the agile method, which uses sprints (short cycles of 2 to 3 weeks) with incremental, iterative deployments and strong collaboration between developers.

A DevOps culture just extends the agile methodology by promoting collaboration between several domains (Dev/Ops/security/testers).

> **Note**
> To learn more about the agile method and its different frameworks (Scrum and Extreme Programming (XP), for example), I recommend `http://agilemethodology.org/`, which provides a lot of documentation.

In addition, for a better application of DevOps implementations, it is important to change your organization by no longer having teams organized by areas of expertise, such as having a team of developers, another team of operations, and a team of testers. The problem with this organizational model is that the teams are compartmentalized, resulting in a lack of communication (which we saw in Chapter 1, The DevOps Culture and Infrastructure as Code Practices, with the wall of confusion). This means that different teams have different objectives, which slams the brakes on applying good practices for a DevOps culture.

One of the models that allow for better communication is feature team organization with multidisciplinary project teams that are composed of people from all fields. In a team, we have developers, operational staff, and testers, and all these people work with the same objective.

If you want to know more about Microsoft's DevOps transformation, I suggest you watch the presentation by Donovan Brown at https://www.agilealliance.org/resources/sessions/microsoft-devops-transformation-donovan-brown/, which explains how Microsoft has changed its organization to adapt to a DevOps culture to continuously improve its products while taking into account user needs.

We have just seen that to implement a DevOps culture in companies, organizational changes are required, including agile project management and the composition of multidisciplinary teams.

## Summary

In this final chapter, we have seen that the implementation of a DevOps culture within projects requires the use of best practices regarding the automation of all manual tasks, the proper choice of tools, a less-monolithic project architecture, and the implementation of monitoring.

On a large scale, for the organization of teams and the company as a whole, we have seen that the agile method, as well as multidisciplinary teams, contribute strongly to the implementation of a DevOps culture.

To finish this book, my advice to all you readers who are adopting DevOps practices is to implement and monitor them on small projects and to start by using the tools that are most familiar and accessible to you. Then, as soon as your DevOps process is working properly, you can extend this to larger projects.

## Questions

- 1. What are the advantages of deployment automation?
- 2. Is it necessary to use Terraform to do IaC?
- 3. What needs to be done to improve security in DevOps processes?
- 4. Does monitoring only concern monitoring of the condition of the infrastructure?
- 5. Which good practice should be implemented in the application architecture?
- 6. In a DevOps organization, how are teams constituted?

## Further reading

If you want to know more about DevOps best practices, here are some articles to aid you in this:

- *16 Best Practices Of CI/CD Pipeline To Speed Test Automation*: `https://www.lambdatest.com/blog/16-best-practices-of-ci-cd-pipeline-to-speed-test-automation/`
- *How To Implement Continuous Testing In DevOps Like A Pro?_: `https://www.lambdatest.com/blog/how-to-implement-continuous-testing-in-devops-like-a-pro/`
- *Secure DevOps_: `https://www.microsoft.com/en-us/securityengineering/devsecops`
- *9 Pillars of Continuous Security Best Practices_: `https://devops.com/9-pillars-of-continuous-security-best-practices/`
- *Top 5 Best Practices for DevOps Monitoring_: `https://devops.com/top-5-best-practices-devops-monitoring/`
- *10 Pitfalls to Avoid when Implementing DevOps_: `https://opensource.com/article/19/9/pitfalls-avoid-devops`

## Assessments

## Chapter 1: The DevOps Culture and Infrastructure as Code Practices

- 1. DevOps is a contraction that is formed from the words *Development* and *Operations*.
- 2. DevOps is a term that represents a culture.
- 3. The three axes of DevOps culture are collaboration, process, and tools.
- 4. The objective of continuous integration is to get quick feedback on the quality of the code archived by team members.
- 5. The difference between continuous delivery and continuous deployment is that the triggering of the deployment in production is done manually for continuous delivery, whereas it is automatic for continuous deployment.
- 6. Infrastructure as Code consists of writing the code of the resources that make up an infrastructure.

## Chapter 2: Provisioning Cloud Infrastructure with Terraform

- 1. The language used by Terraform is HashiCorp Configuration Language (HCL).
- 2. Terraform’s role is as an Infrastructure as Code tool.
- 3. No. Terraform is not a scripting tool.
- 4. The command that allows you to display the installed version is `terraform version`.
- 5. The name of the Azure object that connects Terraform to Azure is the Azure Service Principal.
- 6. The three main commands of Terraform are `terraform init`, `terraform plan`, and `terraform apply`.

## Chapter 3: Using Ansible for Configuring IaaS Infrastructure

- 1. The role of Ansible, as detailed in this chapter, is to automate the configuration of a VM.
- 2. No. We cannot install Ansible on a Windows OS.
- 3. The two artifacts studied in this chapter that Ansible needs to run are the inventory and the playbook.
- 4. The option is --check.
- 5. The name of the utility used to encrypt and decrypt Ansible data is Ansible Vault.
- 6. When using dynamic inventory in Azure, the script is based on VM tags that are used to return the list of VMs.

## Chapter 4: Optimizing Infrastructure Deployment with Packer

- 1. The two ways to install Packer are manually or via a script.
- 2. The mandatory sections of a Packer template that are used to create a VM image in Azure are builders and provisioners.
- 3. The command used to validate a Packer template is packer validate.
- 4. The command that is used to generate a Packer image is packer build.
- 7. The Terraform command that allows us to destroy resources is terraform destroy.
- 8. We add the --auto-approve option to the terraform apply command.
- 9. The purpose of the Terraform state file is to keep the resources and their properties throughout the execution of Terraform.
- 10. No, it is not a good practice to store a Terraform state file locally; it must be stored in a protected remote backend.

