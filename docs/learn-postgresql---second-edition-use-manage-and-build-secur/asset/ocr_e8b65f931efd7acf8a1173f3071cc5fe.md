Once you have selected the configuration that best fits your workload, you can export such a configuration as a postgresql.conf file or as a set of ALTER SYSTEM statements to be executed as a SQL interactive script so that you can apply the configuration to your cluster:

![](bbox=[0.12, 0.17, 0.87, 0.64])

Figure 16.2: An example of an automatically generated configuration

As you can see, the result is a bunch of configuration parameters that you can copy and paste into a “blank” configuration file. The idea is to start from this configuration and continue tuning on top of it.