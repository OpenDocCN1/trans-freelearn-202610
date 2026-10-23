# ▼ Container – 1 Info

![](bbox=[0.63, 0.12, 0.83, 0.13])

### Container details
Specify a name, container image and whether the container should be marked as essential. Each task definition must have at least one essential container.

+   * **Name**: todo-list-app
+       * Up to 255 letters (uppercase and lowercase), numbers, hyphens, and underscores are allowed.
+   * **Image URI**: 441639934710.dkr.ecr.us-east-1.amazonaws.com/to...
+       * Up to 255 letters (uppercase and lowercase), numbers, hyphens, underscores, colons, periods, forward slashes, and number signs are allowed.
+   * **Essential container**: Yes

### Private registry Info
Store credentials in Secrets Manager, and then use the credentials to reference images in private registries.

![](bbox=[0.14, 0.36, 0.37, 0.38])

### Port mappings Info
Add port mappings to allow the container to access ports on the host to send or receive traffic. For port name, a default will be assigned if left blank.

+   * **Container port**: 80
+   * **Protocol**: TCP
+   * **Port name**: container-port-1
+   * **App protocol**: HTTP
+   * **Remove**

Figure 11.29: Container – 1 configured with a name and the ECR image URI

+   11. Next, ensure that **Container port** is set to **80**.
+   12. Next, you will need to create some environmental variables. Ensure that you open the Amazon RDS and Secrets Manager consoles in separate tabs so that you can easily access the relevant details. You will need to create four environmental variables, as follows:

+       I. `Key = AWS_REGION, Value type = Value, Value = us-east-1`
+       II. `Key = SECRET_NAME, Value type = Value, Value = <the name of your secret from Secrets Manager you created in Chapter 5>`
+       III. `Key = DB_HOST, Value type = Value, Value = <the endpoint name of your RDS database you created in Chapter 6>`
+       IV. `Key = DB_PORT, Value type = Value, Value = 3306`