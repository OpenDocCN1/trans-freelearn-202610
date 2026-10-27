
Within Kubernetes, we have our own server cluster to manage all of the **nodes** (machines) and applications being orchestrated. Today, we have the benefit of the cloud providers offering their own managed Kubernetes services, dramatically simplifying the (often severe) burden of managing the cluster. The cluster can have any number of **nodes**, which provide the compute resources for the applications to be deployed. Each application being deployed and managed is packaged as a **container**. This brings us to the Pod, which is a set of one or more containers that live, operate, and tie together. The Kubernetes Pod often shares various attributes among the containers within the Pod. These attributes include things such as storage, and . . . wait for it . . . networking!

![](img/52479dafaefba00517c48a137e037b7e_193_0.png)

Figure 6.14 – Kubernetes at a high level

When deploying applications to Kubernetes, we use a command called `helm`, which is accompanied by a helm chart that defines how the application is to be deployed. Consul provides its own helm chart for this purpose and is available publicly on GitHub (`https://github.com/hashicorp/consul-k8s`). Utilizing this helm chart, Consul can be installed alongside the relevant Pod(s) within the Kubernetes cluster, facilitating a secure network connection to the Pod. Much like the mesh example outside of Kubernetes, within Kubernetes the Consul Connect feature utilizes the Envoy proxy server as the connection point to the application. The primary difference is that instead of the Consul agent running as an individual process, it runs as a **sidecar** process within the cluster configured with Kubernetes **annotations**. This process is the same regardless of whether the application is the destination for the mesh connection or the source for the mesh connection with an upstream service.

Again, when the topic of service mesh arises, it usually relates to the concept of orchestrators such as Kubernetes. However, I hope I've been able to show you how the applicability extends far beyond the Kubernetes cluster. Kubernetes is an advanced topic in itself and, in my opinion, can be very confusing, and it is exactly that level of complexity that requires technologies such as the Consul service mesh.

## Summary

Congratulations! If you've been able to follow this chapter and deal with my horrible jokes, you have a foundational view of the Consul service mesh. The topic of service mesh is a very lengthy one, and I truly hope that this has provided a decent start to that messy journey! We discussed (well, I wrote, and you read) a bit about traditional networking and why the service mesh is such a valuable tool in today's multi-cloud world. We were able to see a bit of how the mesh worked, connecting our services securely, and how to ensure some services did *not* connect. We had a brief overview of how to use the mesh principles to connect data centers, and how to support services outside of the mesh. Finally, we had a very high-level view of how Consul operates within the world of Kubernetes.

If the topic of service mesh really gets you tingly, I strongly recommend you continue your education. HashiCorp has several tutorials on the topic, and as the need for the mesh has grown, countless blogs and videos have been produced. The topic is far too broad for a single chapter, but I do hope that I was able to provide some clarity on the subject. Next, we're going to look at how Consul can help us minimize, or eliminate, those dastardly tickets for network changes through automation!

## 7 Animate Me

We've already talked at great length about the Consul cluster's awareness of the services and applications deployed within the system, along with their corresponding health. Why should Consul keep all of this information to itself? A big part of the successful existence of any network, human or machine, is the sharing of information. Throughout this chapter, we're going to look at a few ways that Consul is able to share that information with third-party components in order to improve our lives through automation:

- Key/value (KV) store – Consul utilizes the distributed KV store to share information among Consul nodes, but this can also be used by humans and machines to share data and directives throughout the cluster.
- Watches and handlers – In Chapter 5, Little Bo Peep Lost Her Service, we saw how Consul watches and handlers are able to announce specific changes to the service status. In this chapter, we'll see how this can be used to distribute information to third-party components.
- Consul and Terraform – Although this isn't a book on Terraform, we have been using it to create and manage our infrastructure. The final section of this chapter is going to show how Consul works with Terraform to automate nearly any infrastructure components based on Consul's application and service awareness.

So, whether the title of this chapter conjures up fears of the singularity overtaking humans, or the sounds of an incredible song from the band Rush, please join me as we journey into the world of a more intelligent and self-reliant network.

The following topics will be covered in this chapter:

- The need for automation
- Distributed configuration with the KV store
- Watching your handlers
- Floating with Consul-Terraform-Sync

### Technical requirements

Within this section, we'll be deploying a full Consul cluster to **Amazon Web Services (AWS)** utilizing Terraform. If you've already gone through *Chapter 2, Architecture – How Does It Work?*, set up your AWS account, downloaded Terraform, and cloned the repository, you're in great shape!

Within the `ch7` folder of the repository, we have a similar structure as for the `ch2` folder, providing the code for creating the image, and of course, creating the Consul cluster. If you would like to follow along, use `ch7` to create a new Consul cluster, which includes some of the service configuration files you'll utilize within this chapter.

### The need for automation

Within the world of information technology, we live by tickets. Thankfully not speeding tickets or violations of other laws, but methods in which work can be requested and addressed. Entire product lines and organizations have been built around these processes, and rightfully so. I have a colleague who raves about a book called *The Phoenix Project*. If you haven't read or listened to it, I would strongly recommend it. The entire fictional story focuses on the process problems with IT and how technology might be all well and good, but the *process* of making changes often dictates the success or failure of any project.

As we look at our networking and security equipment, the importance of following this ticketing system increases by orders of magnitude. It's not like ordering the wrong virtual machine size or the wrong operating system. The slightest misstep can disrupt entire organizations and cause millions of dollars in damage and lost revenue. Therefore, any changes to this equipment need to be officially requested, evaluated, tested, and tracked throughout the change management process. You don't have to work in this field to have felt the challenge of DNS settings taking down entire networks and preventing our kids from attending virtual classes!

### Distributed configuration with the key/value store

As we've read, one of the fundamental protocols of the Consul system is the gossip protocol, enabling each Consul agent within the system to share information with all others. The information is centrally located and managed by the Consul server cluster, but all agents (who are properly authorized if required) have access to that data. Through a very simple KV interface, Consul provides this ability to distribute intelligence to all entities interacting with the system. Although this provides a very easy way to distribute information, Consul isn't a fully featured distributed data store. Objects within the KV system have a maximum size of 512 KB, and the data only persists to the local data center. However, this doesn't prevent us from using it to do some pretty cool stuff.

To see the benefit of applying the Consul KV system for configuration adjustments, consider the situation of troubleshooting a distributed application. After all, nearly everything we deploy these days consists of a set of services, as opposed to a giant monolithic application. When problems occur, which they always do, we'll find that the log data that we need is only available at elevated levels. From my horrid recollection of doing this in the past, the process usually goes something like this:

1. Acquire the IP addresses for each machine/component that needs to be adjusted.
2. Open a bunch of terminal windows on my machine.
3. SSH into the first machine, usually using some shared PEM key that everybody on the team has.
4. Edit the configuration file to change the log level.
5. Reload the service and check the logs to make sure that the service is running again with the new log level.
6. Log out of the machine and proceed to the next one in the list.

Once the debugging session is over, we have to go back into the machines and revert the changes we've made. Configuration management tools such as Ansible, Puppet, or Chef can help with these changes as well, but those changes often require updates to extensive scripts, which brings in additional risk.
