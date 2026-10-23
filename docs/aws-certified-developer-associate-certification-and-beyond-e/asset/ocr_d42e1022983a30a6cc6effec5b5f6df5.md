```
```
[ec2-user@ip-10-0-1-141 bin]$ cd /
[ec2-user@ip-10-0-1-141 /]$ ls
bin boot dev etc home lib lib64 local media mnt opt proc
[ec2-user@ip-10-0-1-141 /]$ cd home
[ec2-user@ip-10-0-1-141 home]$ ls
ec2-user ssm-user
[ec2-user@ip-10-0-1-141 home]$ 
```
```

Figure 11.18: Navigating to the home directory on the server

+   III.  Next, type in the `cd ec2-user` command and press Enter. You will be taken into the `ec2-user` directory.

+   IV.  Next, you will need to download the updated Todo List backend application, which contains all of the APIs to perform all backend operations. This download will also include the Docker file you will use to build your Docker image. While still in the `ec2-user` directory, type in the `wget` command and press Enter.

+   V.  This `wget` command downloads the ZIP file from the GitHub repository directly to your `ec2-user` directory.

+   VI.  Next, type in the `ls` command and press Enter. You will see that the `todo-plus-nodejs-updated.zip` file has been downloaded.

+   VII.  Type in the `unzip todo-plus-nodejs-updated.zip` command to unzip the contents of the file. This will create a directory called `todo-plus-nodejs` containing all the files you need to build the Docker image. Navigate into this folder by typing in `cd todo-plus-nodejs` and pressing Enter.

+   VIII.  Next, type in `ls` and press Enter, and you should see the contents of this folder, as shown in Figure 11.19:

```
```
[ec2-user@ip-10-0-1-141 ~]$ ls
todo-plus-nodejs  todo-plus-nodejs-updated.zip
[ec2-user@ip-10-0-1-141 ~]$ cd todo-plus-nodejs/
[ec2-user@ip-10-0-1-141 todo-plus-nodejs]$ ls
Dockerfile docker-compose-mysql.yml package-lock.json prisma test tsconfig.json
README.md nest-cli.json package.json src tsconfig.build.json user-data.sh
[ec2-user@ip-10-0-1-141 todo-plus-nodejs]$
```
```

Figure 11.19: Unzip the todo-plus-nodejs-updated.zip folder

+   IX.  The unzipped folder contains a Dockerfile. Open the file in your favorite editor to review it.