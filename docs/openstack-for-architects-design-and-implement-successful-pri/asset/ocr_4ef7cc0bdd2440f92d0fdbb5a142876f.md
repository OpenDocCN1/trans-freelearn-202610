# 4

# Building the Deployment Pipeline

We often tell customers we work with that OpenStack is not **installed**, it is **deployed**. While the difference in words might seem subtle, it can really be a revolutionary change within an organization. Most enterprise infrastructure teams are used to the following process in the deployment of a new infrastructure platform:

+   1. Install the platform.
+   2. Configure and integrate the platform.
+   3. Run the platform.
+   4. Upgrade the platform.

Installing and configuring the platform can take months or years and once it's installed, the platform is expected to run for years. Upgrades to the platform happen every 3 to 5 years and are large 6-12 month projects. Red Hat has structured the release of our Enterprise Linux operating system around these cycles – there were 3 years between the release of RHEL 5 and RHEL 6 and almost 4 years between RHEL 6 and RHEL 7. Each release is supported for 10 years and conservative infrastructure teams will wait at least a year after the release of a new version to start their 6-12 month upgrade project.

This kind of conservative approach to platform deployment has worked extremely well in the operating system, virtualization platform, and application platform space. However, if the most important business driver for your private cloud project is increased agility, you are unlikely to achieve your goal with a 12 month roll-out and a 5 year life cycle.