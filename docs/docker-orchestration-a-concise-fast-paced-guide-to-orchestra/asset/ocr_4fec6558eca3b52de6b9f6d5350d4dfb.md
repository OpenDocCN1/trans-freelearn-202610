# # Node components

Node components run on the worker nodes. They control running containers, networking, and other services. They are as follows:

+   - **kubelet**: This is the main processing daemon on each node. It is responsible for starting containers, mounting volumes, downloading secrets, and keeping the node part of the cluster.
+   - **kube-proxy**: This is responsible for maintaining the network rules and forwards network requests to the correct location.

# # Installing Kubernetes

Kubernetes can be installed on most cloud services or locally on your own hardware or virtual machines. Unsurprisingly, since Kubernetes is a Google project, it runs best on GCE and new features tend to be supported there first.

In addition, Google has rolled out a service called **Google Container Engine (GKE)**, which is a pre-rolled Kubernetes service. Google maintains the Kubernetes master and makes it easy to add new nodes to the cluster. You only need to worry about the containers. This is the easiest way to get started with Kubernetes.

For local installations, there are several options. The first is the new `kubeadm` tool which was released as a beta tool with Kubernetes 1.4. Second, there is the `kube-up.sh` script which is part of the Kubernetes release package which can install Kubernetes on a variety of platforms.

There are two other options worth considering. RHEL has a `kubernetes` package available which is installable with `yum`. The Rancher container management suite has an option to automatically deploy Kubernetes environments.

# # Installing with kubeadm

The `kubeadm` tool was introduced in Kubernetes 1.4. The goal is to provide an easy way to install Kubernetes on any platform. For now, it only supports CentOS 7 and Ubuntu 16.04. Installation using `kubeadm` is still a little messy but it is an exciting start. For more details, refer to http://kubernetes.io/docs/getting-started-guides/kubeadm/.