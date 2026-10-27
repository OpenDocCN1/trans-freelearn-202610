
## Preface

Have you ever spent hours troubleshooting a network, only to realize the network addresses for your applications changed? Or perhaps you've had to deflect flying coffee mugs from irate project managers because it is taking weeks to update a firewall or load balancer. Maybe your engineering teams are struggling with end-to-end encryption between their services, reinventing the wheel with every project, or leaving security up to the fictitious perimeter surrounding the network. If these situations sound familiar, HashiCorp Consul just might be the answer for your woes!

Consul provides a platform-independent solution for service discovery, network automation, and secure service communications. This book walks through the foundation of the Consul architecture, explaining how the servers and clients operate. We'll build our own Consul system in Amazon Web Services, and secure the system against crawlers and eavesdroppers. With an established cluster, we'll start deploying services into the system, and show how Consul discovers and monitors these services. Multiple methods of sharing service information will be discussed. However, the continuing focus is on how Consul utilizes this information to dynamically adjust network infrastructure and facilitate secure connections among the registered services. Finally, we'll do a quick walk-through of HCP Consul, a Software as a Service solution offered on HashiCorp Cloud Platform.

## Who this book is for

Any solution architects or DevOps engineers interested in the challenges and solutions of network management in a dynamic hybrid-cloud environment. A background in basic networking is helpful as we discuss device communication and connectivity, as well as the layers of the network stack. Knowledge of Terraform will be beneficial, as we'll be using Terraform to create and work with our system, but it is not required.

## What this book covers

*Chapter 1, Consul Overview – Operation and Use Cases,* starts the book with a short overview of Consul and a high-level understanding of the three primary Consul use cases.

*Chapter 2, Architecture – How Does It Work?,* shows us how the servers and agents operate and communicate, now that we have a basic understanding of "why" Consul.

*Chapter 3, Keep It Safe, Stupid, and Secure Your Cluster!,* looks at securing the entire system, now that we understand how Consul operates. We'll learn how to apply Zero Trust to all of our applications.

*Chapter 4, Data Center (Not Trade) Federation,* looks at how Consul facilitates the extension of the cluster across multiple environments for world domination, now that, within the global and everchanging world, our applications must extend beyond the geographical borders of data centers.

*Chapter 5, Little Bo Peep Lost Her Service,* looks at how Consul not only discovers services – the foundation of Consul's operation being service discovery, but within those services, it also monitors and shares service status information.

*Chapter 6, Connect Four or More,* looks at, with knowledge of our services and their location(s), using Consul to identify and secure the communication among our services.

*Chapter 7, Animate Me,* covers how to simplify our networking infrastructure management with Consul's service information, with an entire world outside of Consul that can make use of its dynamic service discovery.

*Chapter 8, Where Do We Go Now?,* provides some pathways to continue your Consul education and operation. We introduce the HashiCorp certification program for Consul Associate Certification for further knowledge. Additionally, we'll show you how to sign up for HashiCorp's hosted Consul solution enabling the adoption of Consul without the overhead of managing the cluster itself.

## To get the most out of this book

Throughout this book, we'll be using Terraform to build and manage our Consul system, and although knowledge of Terraform would be beneficial, it isn't required. We'll walk through the Terraform code and what it performs in order to create and manage the Consul system. Much of our operations within the book utilize the bash command line. Familiarity with common bash operations and the use of emacs or vi for editing text documents will be useful.

| Software/hardware covered in the book | OS requirements |
|---|---|
| Consul | Windows, Linux, MacOS |
| Terraform | Windows, Linux, MacOS |
| Text editor of choice | |

## Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: `https://static.packt-cdn.com/downloads/9781800202627_ColorImages.pdf`.

## Download the example code files

You can download the example code files for this book from GitHub at `https://github.com/PacktPublishing/Simplifying-Service-Management-with-Consul`.

In case there's an update to the code, it will be updated on the existing GitHub repository. We also have other code bundles from our rich catalog of books and videos available at `https://github.com/PacktPublishing/`. Check them out!

## Conventions used

There are a number of text conventions used throughout this book.

`Code in text`: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "Within the Terraform output, you'll see a value for `CONSUL_HTTP_ADDR`."

A block of code is set as follows:

```
Apply complete! Resources: 23 added, 0 changed, 0 destroyed.

Outputs:

CONSUL_HTTP_ADDR = "http://18.218.165.4:8500"
Consul_Server_IPs = [
  "3.139.233.155",
```

## Share Your Thoughts

Once you've read *Simplifying Service Management with Consul*, we'd love to hear your thoughts! Please click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we're delivering excellent quality content.

## Section 1:
Consul Use Cases
and Architecture

Welcome to the consoling world of Consul. If you've ever built a structure, you know that the foundation is the most important part. If you don't believe me, just look at the leaning tower of Pisa! This section covers the foundational aspects of the Consul system, briefly reviewing why Consul is such a critical component in today's dynamic world, as well as how the Consul system operates to support various use cases.

This part of the book comprises the following chapters:

- Chapter 1, Consul Overview – Operation and Use Cases
- Chapter 2, Architecture – How Does It Work?
- Chapter 3, Keep It Safe, Stupid, and Secure Your Cluster!
- Chapter 4, Data Center (Not Trade) Federation

## 1
Consul Overview – Operation and Use Cases

Welcome to the wonderful world of HashiCorp Consul, a versatile utility to manage, automate, and securely connect all of the services within your network. Thanks for joining me on this ride, and I hope we'll both learn a lot throughout the process. Within this chapter, we're going to be reviewing Consul at a high level just to understand its basics and applications; essentially, we'll be digging the foundation and pouring the concrete to create a stable base on which to form your Consul structure. And if you've ever looked at the leaning tower of Pisa, you know that a solid foundation is critical for any important structure! Within this chapter, we'll be learning about the following:

- The Consul system – what are the components and how do they live together in harmony?
- Consul service discovery – touching on the foundational use case on which all of the other Consul functionality builds
- Consul service mesh – the basics of Consul service mesh to securely enable service-to-service communication
- Network automation – a high-level view of the application of service discovery to automate your network infrastructure

We won't be getting into any code snippets quite yet, but rest assured they are coming. If you're familiar with the concepts and use cases of Consul, go ahead and skip to those chapters – I promise I won't be hurt (too much). However, this would be an excellent chapter to hand your manager when they ask, *what in the world are you doing with this newfangled invention?*

## The Consul system – servers and clients

Sometimes, in order to understand exactly how something works, we need to understand the components involved. After all, within every form of communication, there is a transmitter and a receiver, and if we aren't familiar with either one it can be difficult to understand the message. Consul, at its core, is a communications engine. It provides a new structure to facilitate the communications of network services, not just network devices. If you've spent any time in the networking field, you'll have heard terms such as IP address, subnet mask, and gateway address. These are all critical components involved in the communications of your network devices. However, in the glorious world of the cloud, microservices, and dynamic adjustments to the network, these components will change as the network waxes and wanes. Throughout this painful process, we've learned that the addressing system we've grown to know and love was simply a means to an end. That end is the connection and efficient communication of our services. So how does Consul do that? We are about to find out!

## Consul servers – the master of their domain

Within almost any operational system or structure, there is a brain. This might not be the Brain who, along with his sidekick Pinky, attempts to take over the world day after day. We're focusing here on the brain that makes the decisions. The brain that determines whether to have Indian food or Thai food for dinner. The brain that figures out what to wear on a particular day. The brain that decides which employee gets the biggest raise. Now, at this point, you might be saying *computers and applications don't have brains; they only do what we tell them to do.* You are absolutely correct, but that doesn't mean that the machines don't have the logic and intelligence, even if artificial, to make decisions. In some ways, they are superior, as machines and applications are able to focus on logic and less on ego and emotion. Right, Mr. Spock? *That is correct, you subservient and primitive mammal.*
