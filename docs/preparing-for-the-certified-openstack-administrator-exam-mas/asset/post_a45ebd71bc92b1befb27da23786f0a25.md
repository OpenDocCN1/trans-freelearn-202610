
## Question 17

Create a new Swift container within the auto project called stuff.

Upload a file called object.txt to the stuff container. The container content should be readable/downloadable by the public but only writable by lisaw in the accounting project.

## Question 18

Scoped to the trade project, use the CLI to download the awesome-cirros image to the /tmp directory.

## Question 19

Create a Heat stack within the accounting project from the following template:

```
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
      image: { get_param: Image }
      flavor: { get_param: Flavor }
      networks:
      - network: { get_param: Net }
  volume1:
    type: OS::Cinder::Volume
    properties:
      name: { get_param: VolumeName }
      size: 1

  volumeattach1:
    type: OS::Cinder::VolumeAttachment
    properties:
      volume_id: { get_resource: volume1 }
      instance_uuid: { get_resource: server1}
```

The stack name should be called test-accounting-stack. Use the Cirros image, m1.tiny flavor, and attach to the tenant-network1 network. The instance that gets created should be called heat-instance. The volume should be called volume1.

## Question 20

Within the auto project, set the object.txt file in the stuff container to expire on 1596822650 (Friday, August 7, 2020, 5:50:50 PM).

## Question 21

Create a new domain called zevotoys.

## Question 22

Create a new user within this domain called maryw with the password set to password.

## Question 23

Create a project within the zevotoys domain called marketing.

## Question 24

Create a new group within the zevotoys domain called students. Add maryw to this group.

## Question 25

Assign the _member_ role to the students group on the marketing project.

## Question 26

Share the awesome-cirros image (in the trade project) with the quality-control project.

## Question 27

Create a new provider network and subnet within the admin project with the following details:

- **Network Name**: provider-network-practice-test
- **Provider Network Type**: Flat
- **Physical Network**: public-test
- **External**: true
- **Subnet Name**: provider-subnet-practice-test
- **Subnet-Range**: 172.16.2.0/24
- **Gateway**: 172.16.2.1
- **DHCP**: Disabled

## Question 28

Create a snapshot of volume1 in the auto project. Call the snapshot volume1-snapshot. It should have this description: COA Prep volume1-dashboard snapshot.

## Question 29

Within the procurement project, attempt to create another security group called default. You should receive an error with a request ID. Find any information regarding this error in the neutron log files and document the information you find.

## Question 30

Adjust the allocation pool for tenant-subnet1 in the accounting project to 192.168.5.55-192.168.5.60.

## Question 31

Create a bootable volume from the existing volume1-dashboard volume within the human-resources project.

## Question 32

From the accounting project, back up the volume1 volume to a container called accounting-volume-backups.

## Question 33

SSH into instance1 (inside the accounting project) via its floating IP, 172.16.0.99. Open up any necessary security group rules if you can't connect to the instance. Bonus points if you can SSH into the instance without a password by using the private key generated in Question 3.

## Question 34

Update the test-accounting-stack in the accounting project to use the image, instance1-snapshot (created in Question 12). Be sure to not destroy the existing stack and only update the image. After the image has been successfully updated, delete the entire stack.

## Question 35

Within the accounting project, create another tenant network called tenant-network2. It should have a subnet called tenant-subnet2 with the following details:
- CIDR: 192.168.6.0/24
- DHCP: Enabled
- Allocation range: 192.168.6.50 to 192.168.6.60

After the network and subnet have been created, boot a new instance within the accounting project called instance2 with the m1.tiny flavor. It should use the Cirros image and should be attached to tenant-network2. Once the instance has been booted, use the Horizon dashboard console to ping instance1 via its fixed IP address. You may need to create a router to allow traffic between tenant-network1 and tenant-network2.

## Index

### A

- actions, OpenStack services
  - access and security 25
  - admin 25
  - identity 25
  - images 25
  - instances 24
  - NETWORK 25
  - OBJECT STORE 25
  - ORCHESTRATION 25
  - volumes 25
- Admin Panel Group 72
- admin user
  - about 72
- Identity panel, using 73
- Advanced Message Queueing Protocol (AMQP) 30
- all-in-one Newton environment
  - about 48
  - hardware requisites 50
- Amazon
  - about 12
  - drawbacks 14
- APIs
  - used, for checking component status 293
- appliance
  - executing, on Linux 56
  - executing, on Windows 56
  - importing 54
  - on mac 56
- OpenStack connectivity, verifying with Horizon dashboard 58
- OpenStack service connectivity, verifying with
  - CLI 57
  - SSH, using 56
- architecture, Cinder
  - cinder-api 228
  - cinder-backup 229
  - cinder-scheduler 229
  - cinder-volume 229
- architecture, Heat
  - heat-api 272
  - heat-api-cfn 272
  - heat-engine 272
- architecture, Neutron
  - neutron-dhcp-agent 161
  - neutron-l3-agent 161
  - neutron-linuxbridge-agent 161
  - neutron-metadata-agent 161
  - neutron-server 161
- architecture, Nova
  - nova-compute 128
  - nova-conductor 128
  - nova-consoleauth 128
  - nova-novncproxy 128
  - nova-scheduler 128
- architecture, Swift
  - swift-account-auditor 254
  - swift-account-reaper 254
  - swift-account-replicator 254
  - swift-account-server 254
  - swift-container-auditor 254
  - swift-container-replicator 254
  - swift-container-server 253
  - swift-container-sync 254
  - swift-container-updater 254
  - swift-object-auditor 253
  - swift-object-expirer 253
  - swift-object-replicator 253
  - swift-object-server 253
  - swift-proxy-server 253
- authentication
  - about 62, 69
  - project scope, selecting via dashboard 71
- authorization 62

### B

- bootable 235
- booting 21

### C

- Certified OpenStack Administrator exam
  - about 32
  - benefits 33
  - URL 34
- Certified OpenStack Administrator, exam objectives
  - about 39
  - block storage 41
  - dashboard operation, verifying 42
  - identity management 40
  - image management 40
  - instance actions, computing 40
  - networking 41
  - object storage 42
  - OpenStack 40
  - orchestration 42
  - troubleshooting 43
- Certified OpenStack Administrator
  - certification 34, 35, 37, 38
  - exam objectives 39
  - references 35
  - URL, for requisites 78
- characteristics, elastic cloud
  - API emphasized 13
  - cloudy development 14
  - horizontal scaling 13
  - open source 14
  - standard hardware 13
- characteristics, enterprise virtualization
  - expensive hardware 11
  - GUI emphasized 11
  - proprietary 12
  - traditional software development 12
  - vertical scaling 11
- Cinder, exam objectives
  - backup, creating 249
  - backups, managing 247, 249
  - CLI 235, 237
  - horizon dashboard 233, 238, 242, 244
  - instance, creating 242
  - snapshot, creating 245
  - snapshots, managing 243
  - volumes, managing 232
- Cinder
  - about 25, 226, 228
  - architecture 228
  - backend software 231
  - backup 231
  - exam objectives 232
  - hardware drivers 231
  - iSCSI (Internet Small Computer Systems Interface) 229
  - LVM (Logical Volume Manager) 229
  - snapshot 231
  - technologies 229
  - volume 231
- Cirros 105
- Classless Inter-Domain Routing (CIDR) 171
- CLI help 302
- cloud image
  - about 101
  - building 102
  - glance architecture 104
  - Glance image properties, versus Glance metadata definitions 106, 109
  - glance, on Horizon dashboard 105
  - hashicorp packer 102
  - image file formats 103
  - OpenStack disk image builder project 102
  - Oracle VirtualBox 102
- cloud
  - Amazon 12
  - Amazon, drawbacks 14
  - enterprise virtualization, origin 10
  - history 9
  - NASA 15
  - predicament, of software developers 9
  - Rackspace 15
- command-line interface (CLI)
  - about 62
  - debugging 289
  - project scope 76
  - service-based clients, versus OpenStack client 77
