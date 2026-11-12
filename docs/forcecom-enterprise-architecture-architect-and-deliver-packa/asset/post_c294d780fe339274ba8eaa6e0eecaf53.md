
- Releasing from Source Control
- Automated regression testing

## Development workflow and infrastructure

A development workflow describes the day-to-day activities of a developer in order to obtain the latest build, make changes, and submit them back to a repository that maintains the current overall state of the source code. Having a solid well-defined developer workflow, supporting tools, and infrastructure is critical to the efficiency of any development team.

The first thing a developer will want to know is where the source code is, basically a source control system, such as Git. The next would be how to get it into a form so that they can start executing it. Salesforce provides a toolkit known as the Migration Toolkit that helps write scripts to deploy to a Salesforce org using the popular developer scripting language known as Ant. We will be building such a script in this chapter. Such scripts are also used by Continuous Integration build servers described later in this chapter.

The Migration Toolkit also provides a means to extract from Salesforce org source that has all the components needed to represent your application, not just the Apex and Visualforce source files, but XML files describing Custom Objects, fields, tabs, and practically any other component that can be packaged. We will use this facility to extract the current state of our packaging org in order to initially populate a Git repository with all the component files needed to maintain further changes to the source code outside of the packaging org that we have been developing within so far.

## Salesforce Developer Experience (DX)

At the time of writing, Salesforce announced Salesforce DX. It's a new developer experience focused on enterprise developer needs. The goal is to provide additional tools and APIs that better facilitate source control-based development with a full development life cycle:

- Scratch orgs: These are special types of orgs that can be created and deleted on demand. Thus it is not necessary to clean an org (think of this as your workspace in the cloud). You can also provide JSON configuration file to set various configurations for the org, such as multi-currency and even namespace (despite your packaging org also using the same).
- Heroku flow: The Heroku command line has been extended with additional facilities, such as the creation of Scratch orgs and the ability to synchronize the contents of your local folder structure with that of an attached org.

Details are limited to what was announced at the annual Salesforce Dreamforce event. The preceding points are aimed at giving you an idea of some of the things they announced and that will start to arrive over the next few years.

Meanwhile the following sub-sections provide you with a means to achieve source control development with today's tools, both from Salesforce and the community. It is also likely that some of the approaches here will still be needed as Salesforce DX continues to mature over several releases.

## Packaging org versus sandbox versus developer org

As mentioned previously, it can rapidly become impractical to have a number of developers within one org. They might accidentally overwrite each other's changes, make Custom Object or setup changes that others might not want to see, and consume daily API limits for that org more rapidly, eventually blocking everyone from making changes.

When it comes to the packaging org there is another more important reason, and that is the ability to know exactly what changes have been applied to your application code between one version and the next. This is something you and your customers want to have confidence in being able to know at any given time.

There is also the ability to be able to control the release of features or rollback changes, which becomes harder if those changes have already been made in the packaging org. For these reasons, it is typically a good idea to limit access to your packaging org to those responsible for making releases of your packages, thus treating your packaging org conceptually as a warehouse only.

Sandbox orgs are often attributed to development of custom solutions within an Enterprise organization, where the sandbox has been initialized with a copy of the production org. This can include data, installed packages, and other customizations. At the time of writing, sandbox orgs are available from Partner Portal-provisioned Enterprise Edition orgs (maximum of five). A sandbox can be used as an alternative to a developer org. One advantage is the ability to configure the parent org to a default blank state and then leverage the sandbox refresh process to effectively clean the org ready for a new build deployment. Note that the upcoming Salesforce DX scratch orgs are similar to sandbox orgs in some of these aspects. Note that if you are using Field Sets, sandbox orgs do not currently allow the definition of **Available Fields**.

## Creating and preparing your developer orgs

In the *Required and recommended organizations* section of *Chapter 1, Building, Publishing, and Supporting Your Application*, I recommended that you create a developer org. If you have not done already, visit the Partner Portal or `https://developer.salesforce.com/` and obtain a new developer org (or DE org for short) or Salesforce DX scratch org if you have that facility available to you.

Since this application utilizes Lightning Components and FlexiPages that references these components, you will need to enable **My Domain** on your Development Org for the deployments in this chapter to succeed.

Once you have this, you might want to take some time to further clean out any sample packages, Apex code, or configuration that Salesforce has put in such orgs, as these components will be merged with your own components later in the developer workflow process and you would want to avoid committing them to Source Control, or worse, getting them added to your package. Salesforce DX scratch orgs are empty by design.

Cleaning DE orgs is not something that Salesforce natively supports, yet most developers are familiar with the ability to clean their working environment (their DE orgs in the case of Force.com development) before taking new builds from Source Control. This avoids unwanted classes, objects, and fields building up during the course of development, for example when implementation strategies change or functionality is redefined, causing previously committed components to be renamed or deleted. This can leak back into your code base accidentally. Fortunately, there is an open source solution that leverages the Migration Toolkit to clean a DE org, which we will explore later. Salesforce DX scratch orgs have the same problem, but can more easily be deleted and replaced with a new org.

> Due to the overhead of creating and managing installation of packages. Typically, DE orgs are also used by test engineers and other members of the team to test or preview features being developed. That being said, it can be worth considering the creation of a beta package from a stable build of your code at various intervals throughout your release development cycles, for additional sanity and smoke testing ahead of your final upload process. The uploading and installation of packages can be automated via the Tooling and Metadata APIs.

In this chapter, we are going to extract our application's components from the packaging org as files that we can later upload into a GitHub repository to allow tracking and auditability of changes in a more robust way than is permitted within a Salesforce org. We will then use this repository to populate the DE org created, make some changes, and commit those changes back to the repository. This is known as developer workflow.

## The developer workflow

There are many options when it comes to choosing an appropriate Source Control system to store your application code within. In this book, we will use GitHub as a remote Source Control store and local Source Control repository.
