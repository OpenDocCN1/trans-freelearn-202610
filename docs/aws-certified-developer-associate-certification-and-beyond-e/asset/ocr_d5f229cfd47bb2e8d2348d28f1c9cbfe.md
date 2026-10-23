13. Next, scroll down to the User data – optional section. In the User data textbox, you must paste a Bash script (user-data.sh) provided to you in our GitHub repository. See the following screenshot for a sample:

```
1  #!/bin/bash
2
3  AWS_REGION=us-east-1
4  SECRET_NAME=[Secret Name]
5  DB_HOST=[Database Endpoint]
6  DB_PORT=3306
7
8  yum update -y
9  aws s3 cp s3://[S3 Bucket Name]/todo-plus-nodejs.zip /var
10 unzip /var/todo-plus-nodejs.zip -d /var
11 cd /var/todo-plus-nodejs
12 npm install
13 npm run build
14
15 echo "export AWS_REGION=$AWS_REGION" >> ~/.bashrc
16 echo "export SECRET_NAME=$SECRET_NAME" >> ~/.bashrc
17 echo "export DB_HOST=$DB_HOST" >> ~/.bashrc
18 echo "export DB_PORT=$DB_PORT" >> ~/.bashrc
19 source ~/.bashrc
20 pm2 start dist/main.js+
```

Figure 7.23 – Sample user data script

14. Ensure that you populate the secret name that you created in Chapter 6, Managing Multiple Datasets with AWS Relational and Non-Relational (NoSQL) Databases.

15. Populate the DB_HOST placeholder value with the endpoint address of your RDS database. You will find this in the RDS dashboard, under Databases. Click on the database you deployed in Chapter 6, Managing Multiple Datasets with AWS Relational and Non-Relational (NoSQL) Databases, to view its Connectivity and security settings. You will find the Endpoint address provided, which you can copy and paste into the code replacing the placeholder value.

16. Finally, replace the placeholder for the Amazon S3 bucket name that you created earlier in Project Task 7.1.

17. Next, click the Create launch template button.

18. You should receive a notification stating that the launch template has been successfully created.