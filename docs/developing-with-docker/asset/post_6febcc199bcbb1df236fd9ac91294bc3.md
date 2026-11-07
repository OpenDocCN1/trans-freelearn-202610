
## Awesome Docker

Apart from the official, very comprehensive documentation, there is an awesome (it's even named that way – Awesome Docker) list of Docker resources available on GitHub https://github.com/veggiemonk/awesome-docker.

The list of resources has a good structure and begins with the basics like tutorials explaining what Docker actually is and how to install it on various platforms. The list is divided into main areas, which, among others, include:

- **Useful articles**: This contains a list of articles related to various subjects. Docker networking will list articles related to Docker's networking internals. The Image optimizations section will tell you how to create the smallest possible images and how to create Dockerfiles with the image size in mind. The security section lists resources related to security best practices, auditing, improving container security, and so on. Performance related articles will let you improve the performance of the containers. The Good Tips section is what I found the most useful. It contains a list of tips and tricks, coming from the people who use it in real life – these are very valuable.
- **Tools**: This contains references to a lot of developer and devops tools that are related to Docker. It will include tools for doing continuous integration or continuous delivery, monitoring, and logging; the list is endless, you can find something useful for sure.
- **Slides/Videos**: This holds a list of additional resources available as slides for presentations and a list of videos, webinars, and so on.
- **People, twitter accounts and communities**: And last but not least, a catalogue of people important in the Docker world. You will be able to find a list of blogs, and twitter accounts worth following and also a list of Docker related communities you can join.

The Awesome Docker list is updated very often and has a lot of contributors. You will always find a lot of interesting material here. For example, you will find links to the most up-to-date Docker Tips articles and blogs, a list of useful Docker third-party developer tools and recipes for integration with many continuous integration/continuous delivery providers. If you need to host your Docker container, you can get the information about almost all available Docker hosting providers. It's really hard to imagine that you wouldn't be able to find something you are looking for if it's related to Docker. There are even links for running Docker on Raspberry Pi. My advice is to clone the repository to be able to update in the future to find more interesting links.

![](img/0c87321919fa245e22f056c67ec70003_249_0.png)

You can clone the Awesome-Docker list and find the latest changes when doing a Git pull.

If you find something interesting you would like to share with others, you are more than welcome to contribute to the Awesome-Docker-list; the authors appreciate anyone who would like to add something to the list.

![](img/0c87321919fa245e22f056c67ec70003_249_1.png)

To contribute to the Awesome-Docker list, create a pull request to the repository.

Another resource I would like to mention is the Docker Jumpstart, by Andrew Odewahn. It's also available on the GitHub in the form of mark-up documentation. You can get it at the https://github.com/odewahn/docker-jumpstart/ address. It's very concise and complete, good for refreshing essential Docker skills.

## Training

If you prefer watching videos with webinars, the http://training.docker.com/ site contains a lot of courses to improve and gain knowledge about Docker. Some of them are self-paced training videos (available at http://training.docker.com/self-paced-training). They are free to watch and I highly recommend watching them. Each of the series is a one hour long video webinar. The subjects include:

- Introduction to Docker
- Docker fundamentals
- Docker operations

If free resources are not enough, you can always sign-up to instructor-led training from the Docker team. They have a lot of courses available (paid, of course) at http://training.docker.com/instructor-led-training. These include specific subjects like:

- Docker administration and operations
- Docker datacentre training series
- Deploying Docker datacenter
- Managing container services with universal control plane

and many others. The paid courses cover more advanced and specialized topics. They contain tips which you will probably not find anywhere else but they are not mandatory for working successfully with Docker on a daily basis.

There are a lot of Docker related resources available on the Internet; it is impossible to fit them all in here. Most of them are free to access, so you can further extend your Docker knowledge. But the best way of learning is doing – start experimenting with your Docker images: pull, create, build and publish. If you introduce Docker into your daily routine, you will quickly wonder how you worked without Docker in the past. I hope that working with Docker will be as pleasant an experience for you, as it is for me.

## Index

### A
ADD instruction 129, 130, 131
  examples 130
Amazon
  URL 38
Angular.js application
  packaging 228, 229, 230, 231, 233, 234, 235, 236
API list
  reference link 209
arbitrary commands
  executing, with exec command 167, 168
ARG instruction 148, 149
Artifactory
  reference link 60
AUFS layers
  reference link 52
Augmented File System (AUFS) 45
automated build
  about 195
  build trigger 201, 202, 203
  setting up 195, 196, 197, 198, 199, 200, 201
### B
Boot2Docker utility 12
bridged network 68, 69
build context 195
busybox 56
### C
Captain Hook
  reference link 208
CMD instruction 133, 134, 135
  CMD ["executable"] 133
CMD command parameter1 parameter2 133
overriding 165, 166

commands
  overriding, from Dockerfile 165
Completely Fair Scheduler (CFS) 184
constraints
  executing, on resources 181
  memory constraints 181, 182, 183
  processor 183, 184, 185
  updating, on Docker container 185, 186
Consul
  URL 77
  container 53, 54, 55
Container Network Model (CNM) 64
  endpoint 64
  network 64
  sandbox 64
container
  changes, saving 55, 56, 57, 58
  executing, in network 70, 71, 72, 73
  identifying 162, 163
  linking 88, 89, 90
  PID namespace mode, settings 163, 164
  UTS namespace mode, settings 164, 165
containerization
  about 8
  versus virtualization 7
COPY instruction 132, 133
curl command
  URL, for download link 202
### D
data, in container
  reference link 94
default networks 65, 66
detached mode
  about 160, 161
  executing 160
digest 162
Docker container 8
events 171,172
inspecting 172,173,174
logs, viewing 169,170,171
monitoring 168
restart policy 175,176
statistics 174,175
Docker Distribution toolset
  reference link 194
Docker Hub
  about 103,104,105,119,120,121,122
  account 105,106,107
  accounts 104
  automated builds 104
  collaborators 108,109
  image repositories 104
  integration source code repositories 104
  logging into 110,111,112,113
  organization account 107,108
  organization teams 107,108
  organizations 104
  private repositories 109
  REST API 104
  URL 103
  web hooks 104
Docker ID
  reference link 105
Docker image repository
  reference link 198
Docker index 59,60,61,62
Docker Jumpstart
  reference link 239
Docker registry 59,60,61,62
Docker registry REST API
  about 119,120,121,122
  reference link 120
Docker repository 59,60,61,62
Docker resources
  reference link 238
Docker Toolbox 11
  references 22
Docker, credential helpers
  references 112
docker-client
  reference link 209
docker-squash
  reference link 51
Docker.DotNet
  reference link 209
Docker
  about 7
  APIs 11
  benefits 9
  bridged network 68
  bridged networking 69
  busybox image, executing without network 66
  compose tool 14
  container, executing in network 70,71,72,73
  containers, linking 88,89,90
  default networks 65,66
  engine 12
  engine client 12
  Git 16
  hardware requisites 19,20
  host network 68
  immutable infrastructure 10
  installing, on EC2 Cloud 37,38,39,40,41,42
  installing, on Linux 34,35,36,37
  installing, on Mac OS 29,30,31,32
  installing, on Windows 20,21,22,23,24,25,27,28
Kitematic tool 14
machine 12,13
multi-host networking, creating 74
network, creating 69,70
networking 64,65
networking plugins 91
Oracle VirtualBox 15
portable build environment 9,10
ports, exposing 83,84,85,86,87
ports, mapping 83,84,85,86,87
reproducible build environment 9,10
resources 237,238,239,240
size 9
speed 9
swarm mode 187
tools 11
tools, overview 11
URL, for guidance 239
