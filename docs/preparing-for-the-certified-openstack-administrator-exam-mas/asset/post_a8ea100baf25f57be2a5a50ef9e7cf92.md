
![](img/da864e0670c65edf6f5ab861ef9906b3_290_0.png)

Figure 9.1: High-level view of Heat architecture

## Heat templates

Heat templates (heat orchestration templates or HOT, for short) are templates that heat-api can consume to create virtual resources in the OpenStack environment. They describe all the desired resources and their associated parameters. After the stack has been created, the OpenStack resources are up and running. Templates are extremely convenient because they allow you to check them into a version control system such as GitHub to easily track changes and collaborate with the team. If problems occur after deploying a Heat template, simply restore to a previous version of the template.

Figure 9.2 breaks down the anatomy of a Heat template.

## Anatomy of a Heat Template

```yaml
heat_template_version: 2013-05-23
description: Test Stack

parameters:
  ServerName:
    type: string
    description: A name for the server
  Image:
    type: string
    description: Image ID used to boot the server
  Flavor:
    type: string
    description: Flavor of server
  Net:
    type: string
    description: Network ID for the server
  Volume:
    type: string
    description: A volume ID to attach to the server

resources:
  server1:
    type: OS::Nova::Server
    properties:
      name: { get_param: ServerName }
      image: { get_param: Image }
      flavor: { get_param: Flavor }
      networks:
        - network: { get_param: Net }

  volumeattach:
    type: OS::Cinder::VolumeAttachment
    properties:
      volume_id: { get_param: Volume }
      instance_uuid: { get_resource: server1 }

outputs:
  myinstance_ip:
    description: IP address of the first instance
    value: { get_attr: [server1, first_address] }
```

### Template Header

- version (corresponds with OpenStack Version)
  - 2013-05-23 = Grizzly, Havana, and Icehouse
- description (optional)

### Parameters

- specify any input parameters (optional)

### Resources

- OpenStack resources you are creating (mandatory)

### Outputs

- Information provided to a user after the stack has been created, i.e. IP addresses, URLs, UUIDs, passwords, etc.

Figure 9.2: Anatomy of a Heat template

1. **Template header:** The template version is mandatory and the description is optional. Additional features and capabilities are released in every new version of OpenStack. This means that newer templates that users create may reference features not found in older versions, therefore making the templates incompatible with older environments. Here is a list of Heat template versions and the OpenStack versions they are compatible with:

| Heat template version | OpenStack version |
|---|---|
| 2013-05-23 | Grizzly, Havana, Icehouse, and higher |
| 2014-10-16 | Juno and higher |
| 2015-04-30 | Kilo and higher |
| 2015-10-15 | Liberty and higher |
| 2016-04-08 | Mitaka and higher |
| 2016-10-14 | Newton and higher |

2. **Parameters:** These are optional and can be any particular value you'd like the user to be able to insert. These values get passed in by the user at the time of deploying or “creating” the stack.

3. **Resources:** These are mandatory and represent any OpenStack resource you want to create.

4. **Outputs:** Information provided to the user after stack creation. This could be anything from an IP address to a special registration code or password.

## Heat resources

Resources are mandatory and are a reference to any OpenStack resources you want to create. OpenStack resources can be anything from Nova virtual machine instances to Neutron security groups. Refer to Figure 9.3. You can get a complete list of compatible resources on the Horizon dashboard by navigating to the Resource Types panel from the Project-Orchestration panel group.

![](img/da864e0670c65edf6f5ab861ef9906b3_293_0.png)

Figure 9.3: A list of available Heat resources on the Horizon dashboard

## Backward compatibility with AWS

You may have noticed that some of the Heat resources have a prefix AWS::. These resources are used to make CloudFormation templates compatible with OpenStack Heat. As you know, AWS and OpenStack have many different features so I do not recommend attempting to create stacks with templates that have been designed to work with AWS environments. You would be much better off refactoring your CloudFormation templates with Heat resources. But for the curious, here are a few AWS Heat resources and what they do:

| AWS Heat Resource | Action |
|---|---|
| AWS::EC2::Instance | OS::Nova::Server (Creates a Nova instance) |
| AWS::EC2::SecurityGroup | OS::Neutron::SecurityGroup (Creates a Neutron security group) |
| AWS::EC2::VPC | OS::Neutron::Network (Creates a Neutron tenant network) |
| AWS::EC2::Volume | OS::Cinder::Volume (Creates a Cinder volume) |
| AWS::IAM::AccessKey | OS::Nova::KeyPair (Generates a public/private key for a specified user) |
| AWS::S3::Bucket | Similar to OS::Swift::Container (Creates a Swift container) |

## Exam objectives

Let's take a look at the Heat exam objectives.

Figure 9.4 breaks down various Heat-related objectives you should be responsible for knowing in the COA exam. As you can see, all of these tasks can be completed on both the Horizon dashboard and CLI. Remember that time is of the essence during the exam, and in many cases, it can be much faster to tackle a task on the dashboard.

![](img/da864e0670c65edf6f5ab861ef9906b3_294_0.png)

Figure 9.4: Heat exam objectives can be performed on the Horizon Dashboard or CLI

## Exam objective - managing stacks

When you create a heat template, you are creating a stack. The stack is owned by the project to which the user was scoped at the time of creation. Creating a stack is fairly simple on the Horizon dashboard and CLI. Let's try the dashboard first.

This chapter assumes you have performed the exam objective exercises in Chapter 6. If you have not completed Chapter 6, *Neutron Networking Service*, go back and ensure that you have created a **tenant-network1-dashboard** and **tenant-network1-cli**. A user must have the `heat_stack_owner` role to manage stacks. The admin user that ships with the included virtual appliance should already have this role.

### Horizon dashboard

- 1. From the OpenStack overview page, scope yourself to **logistics** by selecting the top dropdown.
- 2. Select the **Stacks** panel from the **Project-Orchestration** panel group.
- 3. Select **Launch Stack**.

![](img/da864e0670c65edf6f5ab861ef9906b3_295_0.png)

Figure 9.5: Launching a stack from the Horizon dashboard

4. See Figure 9.6. Template Resource (required): You can specify how you wish to provide the template you are going to use.

- **File**: Allows you to select a template on your local filesystem.
- **Direct Input**: Copy and paste a template into a text field.
- **URL**: Provide a URL to a publicly accessible template hosted on a site or GitHub.

5. Paste the following template. This template creates a brand new instance with our specified flavor and image, connects it to an existing tenant network, and then attaches a brand new 1 GB block volume. Be careful with the spacing when pasting into the Horizon dashboard. A copy of the template is available inside the virtual appliance and official book GitHub repo.

![](img/da864e0670c65edf6f5ab861ef9906b3_296_0.png)

YAML (YAML Ain't Markup Language) is a superset of JSON and makes setting values easier for humans. Tabs are not allowed in YAML so always be sure to use spaces. All properties and lists must be indented with 1 or more space. If you struggle with the template, find an example Heat template online or play around with an online YAML parser at http://yaml-online-parser.appspot.com/.

```yaml
heat_template_version: 2013-05-23
description: Test Stack

parameters:
  ServerName:
    type: string
    description: A name for the server
  Image:
    type: string
    description: Image ID used to boot the server
  Flavor:
    type: string
    description: Flavor of server
  Net:
    type: string
    description: Network ID for the server
  VolumeName:
    type: string
    description: A name for the volume

resources:
  server1:
    type: OS::Nova::Server
    properties:
      name: { get_param: ServerName }
```
