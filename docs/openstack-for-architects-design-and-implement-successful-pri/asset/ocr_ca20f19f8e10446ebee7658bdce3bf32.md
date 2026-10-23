To begin, start by installing a fresh copy of the operating system on the four servers that we outlined in the hardware table in the deployment plan. Each system needs to have the network interfaces specified in the network table configured before deployment and all of the requirements specified in the deployment plan will also need to be met(that is, Network Manager needs to be disabled and the RDO repository needs to be enabled).

Execute the following command on the cloud controller (controller1) to generate an answer file. We'll use this as a template for our deployment configuration:

```
# packstack --gen-answer-file=packstack-answers.txt
```

Next, edit the packstack-answers.txt file, updating the following parameters:

| Parameter | Example | Description |
|---|---|---|
| CONFIG_CONTROLLER_HOST | 192.168.0.10 | IP address of the cloud controller on the management network |
| CONFIG_COMPUTE_HOSTS | 192.168.0.100, 192.168.0.101 | IP addresses of the compute nodes on the management network |
| CONFIG_NETWORK_HOSTS | 192.168.0.10 | IP address of the cloud controller on the management network |
| CONFIG_AMQP_HOST | 192.168.0.10 | IP address of the cloud controller on the management network |
| CONFIG_MARIADB_HOST | 192.168.0.10 | IP address of the cloud controller on the management network |