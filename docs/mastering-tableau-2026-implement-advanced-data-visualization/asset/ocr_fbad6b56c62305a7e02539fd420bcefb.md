For a proper understanding of the integration, let's also look at the Tableau/R workflow as an example. The terms used in the following diagram, which you may be unfamiliar with, will be explained throughout this chapter:

![](bbox=[0.16, 0.14, 0.89, 0.47])

Figure 14.2: Tableau/R workflow

The preceding screenshot can be used likewise for Python by replacing R with Python and Rserve with Tabpy. Let's begin with R since it was the first available integration.

# R installation and integration

To adequately understand how Tableau and R work together, it's important to grasp the big picture. To facilitate that understanding, we'll cover high-level concepts and information in this section before delving into calculated fields and R scripting details.

While installing R is straightforward, the integration process requires specific configuration beyond the standard installation. To successfully connect Tableau with R, you will need to make sure that permissions are correctly set and that various components—some required and some that are just nice to have—are correctly implemented. We will cover the basics, review a couple of the typical challenges faced during installation, and provide troubleshooting guidance.

Perform the following steps to install R:

+   1. Download R by visiting http://www.r-project.org/, clicking on the **download R** hyperlink, and choosing a CRAN mirror. Note that R works best in a Linux or Unix environment; however, to learn R and begin working with Tableau/R functionality to complete the exercises in this chapter, installing the Windows version is adequate.