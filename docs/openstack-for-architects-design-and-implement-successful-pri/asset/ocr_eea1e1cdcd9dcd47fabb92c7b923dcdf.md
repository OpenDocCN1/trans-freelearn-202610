# Requirements

The host system will need to meet the following requirements prior to deployment:

+   - Red Hat Enterprise Linux 7 (or CentOS 7)
+   - Network Manager must be disabled
+   - Network interfaces must be configured as per the Network Addressing section in /etc/sysconfig/network-scripts
+   - The RDO OpenStack repository must be enabled (from https://rdoproject.org/)

To enable the RDO repository, run the following command as the root user on your system:

```
yum install -y https://rdoproject.org/repos/rdo-release.rpm
```

# Installing OpenStack

Assuming that we've correctly configured our host machine as per our deployment plan, the actual deployment of OpenStack is relatively straightforward. The installation instructions can either be captured in an additional section of the deployment plan or they can be captured in a separate document-the Installation Guide. Either way, the installation instructions should be immediately followed by a set of tests that can be run to verify that the deployment went correctly.

# Installation instructions

To install OpenStack, execute the following command as the root user on the system designated in the deployment plan:

```
# yum install -y openstack-packstack
```

This command will install the packstack installation utility on the machine. If this command fails, ensure that the RDO repository is correctly enabled using the following command:

```
# rpm -q rdo-release
```