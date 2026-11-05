+   **The Spring Triangle—Portable Service Abstractions** 107
+   **Programmatic transactions** 108
+     Configuring with the IoC container 108
+     Configuring without the IoC container 109
+     @transactional versus programmatic 110
+   **Making new functions play nice with existing transactions** 110
+     How Spring Python lets us define a transaction's ACID properties 113
+   **Applying transactions to non-transactional code** 115
+   **Testing your transactions** 117
+   **Summary** 118
+   **Chapter 6: Securing your Application with Spring Python** 119
+     **Problems with coding security by hand** 120
+     **Building web applications ignoring security** 122
+       Looking at our web application from 10,000 feet 130
+     **Handling new security requirements** 131
+       Authentication confirms "who you are" 131
+       Authorization confirms "what you can do" 132
+     **Time to add security to our application** 133
+     **Accessing security data from within the app** 141
+     **Testing application security** 142
+     **Configuring SQL-based security** 143
+     **Configuring LDAP-based security** 144
+     **Using multiple security providers is easy** 146
+       Migrating from an old security solution to a new one 147
+       Supporting multiple user communities 148
+       Providing redundant security access 148
+     **Coding our own security extension** 150
+       Coding a custom authentication provider 150
+     **Some of the challenges with Spring Python Security** 152
+     **Summary** 153
+   **Chapter 7: Scaling your Application Across Nodes with Spring Python's Remoting** 155
+     **Introduction to Pyro (Python Remote Objects)** 156
+     **Converting a simple application into a distributed one on the same machine** 157
+       Fetching the service from an IoC container 158
+       Creating a client to call the service 159
+       Making our application distributed without changing the client 159
+       Is our example contrived? 163
+       Spring Python is non-invasive 163