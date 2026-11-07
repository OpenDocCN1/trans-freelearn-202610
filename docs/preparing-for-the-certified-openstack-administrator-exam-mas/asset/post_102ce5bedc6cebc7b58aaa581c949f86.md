
- About Keystone 61
  - Authentication or authorization? 62
  - Keystone architecture 62
  - Keystone concepts 63
  - Keystone in your virtual appliance 68
- Authentication in action 69
  - Project scope via the dashboard 71
- The admin life 72
  - The Identity panel 73
- Keystone and the OpenStack CLI 73
- Project scope via CLI 76
  - Service-based clients versus OpenStack client 77
- Exam objectives 77
  - Exam objective - adjusting quotas 79
    - Horizon dashboard 80
    - CLI 83
  - Exam objective - managing users 87
    - Horizon dashboard 87
    - CLI 90
  - Exam objective - creating domains, projects, groups, and roles 91
    - CLI 91
  - Exam objective - managing the service catalog - services and endpoints 93
    - Horizon dashboard 96
    - CLI 97
- Summary 100
## Chapter 4: Glance Image Service 101
- What is a cloud image? 101
  - Building cloud images 102
  - Image file formats 103
  - Glance architecture 104
  - Glance on the Horizon dashboard 105
    - Glance image properties versus Glance metadata definitions 106
- Exam objectives 109
  - Exam objective - creating Glance images 110
    - Horizon dashboard 111
    - CLI 114
  - Exam objective - downloading images 118
    - CLI 118
  - Exam objective - sharing images with specific projects 118
    - CLI 118
- Exam objective - setting Glance image properties and metadata definitions 119
  - Horizon dashboard 120
  - CLI 124
- Summary 125
## Chapter 5: Nova Compute Service 127
- Nova - the compute orchestrator 128
- Nova architecture 128
- Hypervisor types 129
- Exam objectives 131
- Exam objective - managing key pairs 132
  - Horizon dashboard 132
  - CLI 133
- Exam objective - managing flavors 135
  - Horizon dashboard 136
  - CLI 138
- Exam objective - launching instances 138
  - Horizon dashboard 139
  - CLI 144
- Exam objective - getting access to your instance 145
  - Horizon dashboard 146
  - CLI 149
- Exam objective - creating instance snapshots 150
  - Horizon dashboard 151
  - CLI 152
- Exam objective - managing instance actions 153
  - Horizon dashboard 155
  - CLI 155
- Summary 157
## Chapter 6: Neutron Networking Service 159
- About Neutron 160
- Neutron architecture 160
- LinuxBridge plugin 162
- Neutron concepts 162
- Exam objectives 164
- Exam objective - managing tenant networks 164
  - Horizon dashboard 165
  - CLI 167
- Exam objective - managing tenant subnets 169
  - Horizon dashboard 170
  - CLI 175
- Exam objective - managing security groups and rules 175
  - Horizon dashboard 176
  - CLI 182
### Exam objective - managing routers - east/west traffic 183
- Horizon dashboard 184
- CLI 196
### Exam objective - manage provider networks 198
- Horizon dashboard 198
- CLI 202
### Exam objective - manage provider subnets 203
- Horizon dashboard 204
- CLI 207
### Exam objective - managing routers - north/south traffic 208
- Horizon dashboard 209
- CLI 213
### Exam objective - managing floating IPs 214
- Horizon dashboard 214
- Bonus exercise 221
- CLI 221
- Before proceeding... 223
### Summary 223
## Chapter 7: Cinder Block-Storage Service 225
### About Cinder 226
- Cinder architecture 228
- Cinder behind the scenes 229
- Cinder backend software and hardware drivers 231
- Cinder concepts 231
### Exam objectives 232
### Exam objective - managing Cinder volumes 232
- Horizon dashboard 232
- CLI 235
- Horizon dashboard 238
- CLI 242
### Exam objective - managing Cinder snapshots 243
- Horizon dashboard 244
- CLI 245
### Exam objective - managing Cinder backups 247
- Horizon dashboard 247
- CLI 249
- Note 249
### Summary 250
## Chapter 8: Swift Object-Storage Service 251
### About Swift 251
- Swift architecture 253
- Swift concepts 255
- Exam objectives 256
- Exam objective - managing containers and objects 256
- Horizon dashboard 257
- CLI 260
- Exam objective: managing container ACLs 262
- CLI 262
- Exam objective: settings objects to expire 268
- CLI 268
- Summary 270
## Chapter 9: Heat Orchestration Service 271
- About Heat 271
- Heat architecture 272
- Heat templates 273
- Heat resources 276
- Backward compatibility with AWS 277
- Exam objectives 277
- Exam objective - managing stacks 278
- Horizon dashboard 278
- CLI 283
- Exam objective - updating stacks 285
- Horizon dashboard 285
- CLI 287
- Summary 288
## Chapter 10: Troubleshooting 289
- Debugging the CLI 289
- Managing OpenStack daemons 292
- Using the APIs to check the component status 293
- Important OpenStack service directories 294
- Exam objective - analyzing log files 295
- Managing MySQL 296
- Summary 298
## Chapter 11: Final Tips and Tricks 299
- Project scope 300
- Time management 301
- CLI help 302
- External monitors 304
- Issues during the exam 305
- Diving deeper into OpenStack 305
- Summary 306
## Chapter 12: Practice Exam 307
- Before you begin... 307
- Instructions 307
- Question 1 307
- Question 2 308
- Question 3 308
- Question 4 308
- Question 5 308
- Question 6 308
- Question 7 309
- Question 8 309
- Question 9 309
- Question 10 309
- Question 11 309
- Question 12 310
- Question 13 310
- Question 14 310
- Question 15 310
- Question 16 310
- Question 17 311
- Question 18 311
- Question 19 311
- Question 20 312
- Question 21 312
- Question 22 312
- Question 23 312
- Question 24 312
- Question 25 313
- Question 26 313
- Question 27 313
- Question 28 313
- Question 29 313
- Question 30 313
- Question 31 314
- Question 32 314
- Question 33 314
- Question 34 314
- Question 35 314
- Index 315

Chapter 6, Neutron Networking Service, dives into the world of OpenStack networking by providing an overview of Neutron and covers exam objectives including managing tenant and provider networks, subnets, security groups, routers, and floating IPs.

Chapter 7, Cinder Block-Storage Service, explains Cinder persistent block storage and exam objectives including creating volumes, attach/detaching volumes, volume snapshots, and creating volume types.

Chapter 8, Swift Object-Storage Service, discusses the birth of AWS S3, Swift object storage, and exam objectives, including setting Access Control Lists (ACLs) on containers and setting objectives to expire at specific times.

Chapter 9, Heat Orchestration Service, explains OpenStack Heat and exam objectives including creating and updating stacks.

Chapter 10, Troubleshooting, shows you how to perform debugging on the OpenStack CLI, manage OpenStack daemons, analyze OpenStack log files, and perform basic operations on an OpenStack database.

Chapter 11, Final Tips and Tricks, gives you some useful tips and tricks to keep in mind before taking the COA exam.

Chapter 12, Practice Exam, is a mock COA exam that you can use to put your OpenStack skills to test!

## What you need for this book

Please download all materials from the official book repository at https://github.com/PacktPublishing/Preparing-for-the-Certified-OpenStack-Administrator-Exam. The repository also contains an OpenStack virtual appliance that can be imported into VirtualBox to perform all exam objective exercises.

## Who this book is for

This book targets IT professionals, system administrators, DevOps engineers, and software developers with basic Linux command line and networking knowledge.

## Conventions

In this book, you will find a number of text styles that distinguish between different kinds of information. Here are some examples of these styles and an explanation of their meaning.

Code words in text, database table names, Linux programs, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles are shown as follows: "You can fulfill majority of the exam objectives with `python-openstackclient`."

A block of code is set as follows:

```
export OS_AUTH_URL=http://192.168.56.56:5000/v3
export OS_REGION_NAME=RegionOne
export OS_PROJECT_DOMAIN_NAME=default
export OS_USER_DOMAIN_NAME=default
export OS_USERNAME=admin
export OS_PASSWORD=admin
export OS_PROJECT_NAME=logistics
export OS_TENANT_NAME=logistics
```

Any command-line input or output is written as follows:

```
$ openstack help
```

New terms and **important words** are shown in bold. Words that you see on the screen, for example, in menus or dialog boxes, appear in the text like this: "From the VirtualBox menu bar, select **Preferences**."

![](img/da864e0670c65edf6f5ab861ef9906b3_20_0.png)

Warnings or important notes appear in a box like this.

![](img/da864e0670c65edf6f5ab861ef9906b3_20_1.png)

Tips and tricks appear like this.
