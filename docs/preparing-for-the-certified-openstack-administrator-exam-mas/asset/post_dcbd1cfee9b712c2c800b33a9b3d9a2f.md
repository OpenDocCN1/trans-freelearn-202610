
![](img/da864e0670c65edf6f5ab861ef9906b3_46_0.png)

Figure 1.6: python-openstackclient brings the traditional service-based clients into a single shell with a uniform command structure

You can also view help commands by running the following:

```
$ openstack help
```

![](img/da864e0670c65edf6f5ab861ef9906b3_46_1.png)

The OpenStack clients will be discussed in more detail in upcoming chapters. As of September 2017, the COA exam tests on the Newton version of OpenStack. The exam will provide access to python-openstackclient and all service-based clients. A majority of the exam objectives can be completed with python-openstackclient, except where noted in this book.

## OpenStack daemon communication

Now that we've discussed the various ways in which an OpenStack operator interacts with OpenStack via the API, let's discuss internal communication among the core OpenStack services. In Figure 1.7, you can see the color code in the top left that shows the service and daemon. The service is simply the name of the OpenStack service, while the daemon represents the program that is actually running to bring the service to life.

To avoid tight coupling, communication amongst the daemons within a specific service is achieved via an Advanced Message Queueing Protocol (AMQP). In a typical OpenStack environment, this can be software such as RabbitMQ or Qpid.

Recall the cloudy development methodology that we discussed in the history portion of this chapter. While OpenStack encourages its software developers to follow this methodology when deploying applications on it, the actual OpenStack infrastructure follows the exact same principles: highly available, scalable, and loosely coupled.

For example, when a user sends a request to `nova-api` to boot a server, `nova-api` will publish this message on the AMQP bus. This message will then be read by `nova-scheduler`. Now let's visualize an OpenStack cloud that often receives hundreds of requests to boot instances. To solve this problem you would deploy more servers in the infrastructure, install more `nova-scheduler` daemons, and simply point them to the OpenStack environment's AMQP server. That's Horizontal scaling in action!

![](img/da864e0670c65edf6f5ab861ef9906b3_47_0.png)

Figure 1.7: OpenStack daemons within a specific service use the AMQP bus to communicate among themselves

## OpenStack API communication

While the OpenStack service daemons rely on AMQP for communication among daemons within their specific service, the APIs are used both by OpenStack users wishing to create virtual resources and for service-to-service communication.

![](img/da864e0670c65edf6f5ab861ef9906b3_48_0.png)

Figure 1.8: An example of API communication amongst OpenStack services

Envision a user sending a request to nova-api to boot an instance. In order to boot that server, quite a few internal API calls would need to take place. Let's look at an example sequence of API calls that would take place when booting a virtual machine instance.

An OpenStack user would first send a POST API call to the `nova-api` daemon with details about what sort of virtual machine they'd like. That message would get put on the AMQP message bus and consumed by the `nova-scheduler` daemon. The `nova-scheduler` daemon would then choose the best fit for the virtual machine using its scheduling algorithm. The `nova-compute` daemon residing on the compute node consumes this message. Before the server can be booted, the following must occur (see Figure 1.8):

- (1): The `nova-compute` daemon sends an API request to Keystone to retrieve a scoped token.
- (2): `nova-compute` gets back a 200 HTTP response along with a scoped token. Now `nova-compute` has a scoped token and can freely communicate with other OpenStack services.
- (3): `nova-compute` now sends a request to `neutron-server` to verify whether the specified virtual network on which to boot the instance is available.
- (4): `nova-compute` receives a response that the network is available and capable of attaching to the instance.
- (5): `nova-compute` must then send an API call to `glance-api` to retrieve the operating system image.
- (6): The `glance-api` responds back with the image that will reside on the compute node.

Now our instance will be booted! That was a lot of work!

![](img/da864e0670c65edf6f5ab861ef9906b3_49_0.png)

Don't worry! You don't need to understand all the magic occurring behind the scenes in OpenStack to be successful on the exam. This is for educational purposes only!

## About the Certified OpenStack Administrator exam

Now that you've learned a bit about OpenStack, let's get down to why you're really here:

To pass the Certified OpenStack Administrator exam!

## Benefits of passing the exam

Ask anyone about getting started in the IT world and they may suggest looking into industry-recognized technical certifications. IT certifications measure competency in a number of areas and are a great way to open doors to opportunities. While they certainly should not be the only determining factor in the hiring process, achieving them can be a measure of your competence and commitment to facing challenges.

## If you pass...

Upon achieving a passing grade, you will receive your certificate (see Figure 1.9). Laminate, frame, or pin it to your home office wall or work cubicle—it’s proof that you have met all the requirements to become an official OpenStack administrator. The certification is valid for three years from the pass date, so don’t forget to renew.

![](img/da864e0670c65edf6f5ab861ef9906b3_50_0.png)

Figure 1.9: A certificate you receive when you pass the Certified OpenStack Administrator exam

In addition to the certification, a COA badge will appear next to your name in the official OpenStack Community Directory, available at https://www.openstack.org/community/members/.

![](img/da864e0670c65edf6f5ab861ef9906b3_51_0.png)

Figure 1.10: Upon passing the exam, you will also receive an official COA badge next to your name in the OpenStack Foundation Member Directory

![](img/da864e0670c65edf6f5ab861ef9906b3_51_1.png)

The OpenStack Foundation has put together a great tool for helping employers verify the validity of COA certifications. Check out the Certified OpenStack Administrator Verification Tool (https://www.openstack.org/coa/coa-verify/).

## Seven steps to becoming a Certified OpenStack Administrator

Let's begin by walking through some steps to become a Certified OpenStack administrator.

## Step 1 - study!

Practice. Practice. Practice. Use this book and the included OpenStack All-in-One Virtual Appliance as a resource as you begin your Certified OpenStack Administrator journey. If you still find yourself struggling with the concepts and objectives in this book, you can always refer to the official OpenStack documentation—or even seek out a live training class at https://www.openstack.org/marketplace/training/.

## Step 2 - purchase

Once you feel that you're ready to conquer the exam, head to https://www.openstack.org/coa/ and click on Get Started. After signing in, you will be directed to checkout to purchase your exam. The OpenStack Foundation accepts all major credit cards, and costs $300.00 USD as of September 2017 (subject to change, so keep an eye on the website). You can also get a free retake within 12 months of the original exam purchase date if you do not pass on the first attempt.

![](img/da864e0670c65edf6f5ab861ef9906b3_52_0.png)

To encourage students in academia to get their feet wet with OpenStack technologies, the OpenStack Foundation is offering the exam for $150.00 (50% off the retail price) with a valid student ID. Check out https://www.openstack.org/coa/student/ for more info.

## Step 3 - COA portal page

Once your order is processed, you will receive an email with access to the COA portal. Think of the portal as your personal COA website, where you can download your exam receipt and keep track of your certification efforts. Once you take the exam, you can come back to the COA portal to check your exam status and score, and even download certificates and badges for your website or business cards.

## Step 4 - hardware compatibility check

The COA exam can be taken from your personal laptop or desktop, but you must ensure that your system meets the exam's minimum system requirements. A link on the COA portal page will present you with the compatibility check tool, which will run a series of tests to ensure you meet the requirements. It will also assist you in downloading a Chrome plugin for taking the exam. At this time, you must use the Chrome or Chromium browser and have access to reliable internet, a webcam, and microphone.
