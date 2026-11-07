The ENV Dockerfile instruction sets the environment variable <key> to the value <value>. It has two forms:

+   - The first form, ENV <key> <value> will set a single variable to a value. The entire string after the first space will be treated as the <value>. This will include any character, also spaces and quotes. Consider the following example: ENV JAVA_HOME /var/lib/java8
+   - The second form is ENV <key>=<value> ..., and allows setting multiple environment variables at once. The difference is the equal to sign. If you need to provide spaces in the values, you will need to use quotes. If you need quotes in the values, use backslashes: ENV CONFIG_TYPE=file CONFIG_LOCATION="home/Jarek/my \ app/config"

Note that you can use ENV to update the PATH environment variable, and then CMD parameters will be aware of that setting. This will result in a cleaner form of CMD parameters in the Dockerfile. Consider the following example:

```
ENV PATH /var/lib/tomcat8/bin:$PATH
```

Setting this will ensure that CMD ["startup.sh"] works, because it will find the startup.sh file in the system PATH. You can also use ENV to set the often modified version numbers so that upgrades are easier to handle, as shown in the following example:

```
ENV TOMCAT_VERSION_MAJOR 8
ENV TOMCAT_VERSION 8.5.4
RUN curl -SL
http://apache.uib.no/tomcat/tomcat-$TOMCAT_VERSION_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz | tar zxvf apache-tomcat-$TOMCAT_VERSION.tar.gz -c /usr/Jarek/apache-tomcat-$TOMCAT_VERSION
ENV PATH /usr/Jarek/apache-tomcat-$TOMCAT_VERSION/bin:$PATH
```

In the preceding example, Docker will download the version of Tomcat specified in the ENV variable, extract it to the new directory with that version in its name and also set up the system PATH to make it available for running.

The environment variables set using ENV will persist when a container is run from the resulting image. The same as with labels created with LABEL, you can view the ENV values using the docker inspect command. The ENV values can be also overridden just before the start of the container, using docker run --env <key>=<value>.