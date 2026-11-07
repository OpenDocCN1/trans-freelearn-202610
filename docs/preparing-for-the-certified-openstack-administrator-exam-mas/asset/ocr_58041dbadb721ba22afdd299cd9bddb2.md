![](bbox=[0.09, 0.1, 0.18, 0.22])

One great way to see the raw API calls being made behind the scenes is by using the `--debug` option. Append it to any `python-openstackclient` command to see calls to the OpenStack APIs!

For your convenience, Figure 3.20 shows a comprehensive list of available quotas, their definitions, default values, and the service responsible for managing the quota.

| Quota Name | Definition | Quantity/Size | Service |
|---|---|---|---|
| Fixed IPs | Max # of fixed ip addresses within the | -1 | Neutron |
| Floating IPs | Max # of floating ip addresses within the | 50 | Neutron |
| Keypairs | Max # of SSH key pairs within the | 100 | Nova |
| Injected File Content | Max file size (bytes) that can be injected | 10240 | Nova |
| Injected Files | Max # of files that can be injected into | 5 | Nova |
| Injected Path Size (KB) | Max size (kilobytes) of the injected file | 255 | Nova |
| Instances | Max # of virtual machines that can | 10 | Nova |
| Metadata Items | Max # of metadata key-value pairs that within the | 128 | Nova |
| Networks | Max # of networks within the project. | 10 | Neutron |
| Ports | Max # of ports within the project. | 50 | Neutron |
| RAM | Max size (MB) of all RAM assigned to all | 51200 | Nova |
| RBAC Policy | Max # of RBAC policies rules within | 10 | Neutron |
| Routers | Max # of routers within the project. | 10 | Neutron |
| Security Groups | Max # of security groups within the | 10 | Neutron |
| Security Group Rules | Max # of security group rules within | 100 | Neutron |
| Server Groups | Max # of server groups within the | 10 | Nova |
| Server Group Members | Max # of server group members | 10 | Nova |
| Subnets | Max # of subnets within the project. | 10 | Neutron |
| Subnet pools | Max # of subnets pools within the | -1 | Neutron |
| Total Size of Volumes and | Max size (GB) of all volumes and | 1000 | Cinder |
| VCPUs | Max # of cores assigned to all | 20 | Nova |
| Volumes | Max # of volumes that can be created | 10 | Cinder |
| Volume Size | Max size (GB) of each volume within | -1 | Cinder |
| Volume Backups | Max # of volume backups within the | 10 | Cinder |
| Volume Backup Size | Max size (GB) of all volume backups | 1000 | Cinder |
| Volume Snapshots | Max # of volume snapshots within the | 10 | Cinder |
| Volumes | Max # of volumes within the project. | 10 | Cinder |

Figure 3.20: A full list of all available quotas. The -1 value means 'no limit'