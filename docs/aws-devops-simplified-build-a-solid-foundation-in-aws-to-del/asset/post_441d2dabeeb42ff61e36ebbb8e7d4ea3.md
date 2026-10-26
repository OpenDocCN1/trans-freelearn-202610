
## Understanding the target architecture of the DevSecOps pipeline

A pipeline definition in AWS CodePipeline consists of at least two stages, and corresponding actions within each stage. We covered the constructs of AWS CodePipeline in detail in Chapter 5, Rolling Out a CI/CD Pipeline. Typical stages that come to our mind when we think of the entire life cycle of software delivery are source, build, test, and deploy. In the exercise that follows, we will mainly focus on the first two stages and understand how DevSecOps practices could be introduced in a Docker image delivery workflow, as an example.

We developed and deployed a Python-based To-Do list manager application in the previous chapters. We will reuse the application code and respective Docker manifest file. Our main focus will be on introducing security assessments that can be integrated into the build phase of this application, leading to the creation of a secure Docker image. Figure 9.2 highlights the different types of security scans we will run:

![](img/ecd265dfca9bb64161408ba8a944a4d9_232_0.png)

Figure 9.2 – Different stages of the DevSecOps pipeline

Let’s begin by outlining the steps that are executed in each stage of the pipeline:

- **1. New code changes**: Almost every CI/CD pipeline has an automated trigger for an event of interest. In this case, we are interested in new commits being pushed by a developer into the CodeCommit repository that contains the application code.
- **2. CodePipeline execution**: Soon after a git commit is registered in the CodeCommit repository, the pipeline execution begins. As shown in the preceding diagram, Source is the first stage, and this is where CodePipeline pulls the most recent version of the code changes and makes them available in subsequent stages and actions.
- **3. Application validation**: The second stage in the pipeline defines the actions that assess the code and its dependencies against security vulnerabilities. This stage further consists of two *actions* that execute in parallel:

  - I. **Software Composition Analysis (SCA)**: This is a CodePipeline action within the application validation stage. We leverage CodeBuild to provide a temporary compute environment that can be used to install the **Dependency-Check** utility from **OWASP**. Post installation, we validate the Python code dependencies defined in the `requirements.txt` file. If you have not worked with Python before, the `requirements.txt` file lists all the dependencies – that is, the libraries and packages used by a Python project.

  Dependency-check is an SCA tool that works with various programming languages and detects publicly disclosed vulnerabilities of the project dependencies. This is achieved by assessing the presence of a **Common Platform Enumeration (CPE)** identifier for a specified dependency. In the event of discovery, a comprehensive report will be generated, containing links to the corresponding CVE entries.

  - II. **Static Application Security Testing (SAST)**: In this CodePipeline action, we use the **Bandit** utility, which scans Python code to bring security issues to the surface. Bandit creates an **abstract syntax tree (AST)** from all the Python code files it scans and then runs security validation plugins against this AST. The output is a report that highlights security issues, along with the suggested actions for the developers.

- **4. Dockerfile validation**: This is the third stage of the pipeline and is triggered after the application code scans are green. As you might notice, we are following the principle of *inside-out*, by first scanning the application itself, and then tackling the abstractions on top of it, one level at a time. This also helps with optimized usage of pipeline resources since there is no point in testing the Docker image before ensuring a good security posture for the code that will be added to it.

  For Dockerfile linting and security analysis, we make use of `hadolint`. In addition to providing basic linting capabilities, hadolint also allows security professionals to enforce certain things. For example, you can define the trusted registries these Dockerfiles can be based on or suppress false positives. All validations from hadolint can be accessed at https://github.com/hadolint/hadolint#rules.

- **5. Dockerfile build**: This is the last and final stage of the pipeline where a Docker image is built and then published to an **Elastic Container Registry (ECR)** repository for use in deployment activities.

The workflow we are deploying can be easily extended with specific deployment actions for any other software. In the ideal scenario, you would have additional stages that consume the artifacts (Docker images) from this pipeline and deploy them onto an environment that supports running Docker containers, such as **Elastic Container Service (ECS)** or **Elastic Kubernetes Service (EKS)**.

Having understood the overall flow and what we are trying to achieve with the DevSecOps pipeline, let’s outline the two components of the code base that we will be working with.

## Understanding the code base

Similar to other chapters, the code for the exercise that follows can be found in the project directory, `chapter-9/`, inside your Cloud9 IDE. Two subfolders contain the code for both the Python application and the infrastructure for rolling out a DevSecOps CI/CD pipeline. Let’s walk through the contents of both:

- **Python Flask application:** This folder includes the code required to run a Python-based **To-Do list manager** application in the form of a Docker container. In addition to the `app.py` file, a `Dockerfile` manifest lists all the instructions you will need to create a Docker image.
- **DevSecOps CI/CD pipeline constructs:** We will be leveraging **AWS CDK** constructs to deploy the components of our CI/CD pipeline and see what a typical DevSecOps workflow looks like. All the resource constructs we use in the CDK stack are AWS-managed services, so that avoids the need for any foundational infrastructure elements such as VPCs, subnets, internet gateways, and so on.

Before we can commit the code for the Python application and assess it for any security risks, we need to deploy the CDK stack and provision the pipeline components.

## Deploying the CDK stack in an AWS account

To deploy the stack resources, we first need to fire the `npm install` command so that all the project dependencies can be downloaded. After the installation completes, you can use the `cdk synth` command to check the CloudFormation template, if you like.

At this point, we are all set to deploy the stack in the AWS account, which will set up all the stages and corresponding actions of the pipeline using the **AWS CodePipeline** service:

```
aws-devops-simplified:~ /environment/chapter-9/devsecops-cicd-pipeline
$ cdk deploy
...
...
✨  Synthesis time: 12.65s
DeliveryPipelineStack:  start: Building 46e40c3e0f5b985e8b4b1bbc5c4eb-24cacee4d46e40c3e0f5b985e8b4b1bbc5c4eb24cacee4d04d15ecbcbecbc4839d8f-9dlcf:current_account-current_region
...
Do you wish to deploy these changes (y/n)? y
DeliveryPipelineStack: deploying... [1/1]
...
DeliveryPipelineStack: creating CloudFormation changeset...
DeliveryPipelineStack
Deployment time: 202.82s
```

After the CDK application has been successfully deployed, you can access the CodePipeline service to see all the stages and corresponding actions that have been provisioned. You will see something similar to Figure 9.3:

![](img/ecd265dfca9bb64161408ba8a944a4d9_235_0.png)

Figure 9.3 – Different stages of the CI/CD pipeline

As you can notice, the first stage reports a failure, which is expected since there is no code in the freshly provisioned CodeCommit repository. So, let’s go ahead and commit the application code into the repository. CodeCommit recommends using the git-remote-codecommit plugin for authenticating against the HTTPS (GRC) endpoint. This GRC endpoint can be retrieved from the CodeCommit service page for your repository, in the AWS web console. The git-remote-codecommit plugin is already included in the Cloud9 IDE that you’ve installed in your AWS account.

One last step before we can see the pipeline in execution is to initialize a local git repository and set up the git remote so that it points to our CodeCommit repository in the AWS account. We just need to push the application code, so let’s switch the working directory as well:

```
aws-devops-simplified:~/environment/chapter-9 $ cd chapter-9-flask-app/
aws-devops-simplified:~/environment/chapter-9/chapter-9-flask-app $ git init .
aws-devops-simplified:~/environment/chapter-9/chapter-9-flask-app $ git checkout -b master
aws-devops-simplified:~/environment/chapter-9/chapter-9-flask-app $ git add .
aws-devops-simplified:~/environment/chapter-9/chapter-9-flask-app $ git commit -m "Sample application for DevSecOps pipeline"
```

After files have been added to Git’s staging area, we can push them to the CodeCommit repository:

```
aws-devops-simplified:~/environment/chapter-9/chapter-9-flask-app $ git remote add origin codecommit::eu-central-1://image-delivery-pipeline
aws-devops-simplified:~/environment/chapter-9/chapter-9-flask-app $ git push origin master
...
```

As soon as the commits are registered in CodeCommit, the pipeline execution begins. CodePipeline can now source the code from the CodeCommit repository on the configured branch (master). Let’s dive into the result of security assessments that are configured in the pipeline.

## Checking the result of security assessments

Every stage of the pipeline validates some or the other aspect of the application against security risks. We follow the same *inside-out* principle and start from the application itself, before moving on to Docker validations.
