Other messaging services used with OpenStack such as ZeroMQ, ActiveMQ, or Qpid may have different strategies and configurations for achieving H/A and horizontal scalability. For these services, refer to the documentation to determine the optimal architecture.

# Compute, storage, and network agents

The compute, storage, and network components in OpenStack have a set of services, that perform the work which is scheduled by the API services. These services register themselves with the schedulers on start up over the message bus. The schedulers are responsible for determining the health of the services and scheduling work to active services. The compute and storage services are all designed to be run Active/Active but the network services need some extra consideration.

Each hypervisor in an OpenStack deployment runs the nova-compute service. When this service starts up, it registers itself with the nova-scheduler service. A list of currently available nova services is available via the nova service-list command. If a compute node is unavailable, its state is listed as down and the scheduler skips it when performing instance actions. When the node becomes available, the scheduler includes it in the list of available hosts.

For KVM or Xen-based deployments, the nova-compute service runs once per hypervisor and is not made highly available. For VMware-based deployments though, a single nova-compute service is run for every vSphere cluster. As such, this service should be made highly available in an Active/Passive configuration. This is typically done by virtualizing the service within a vSphere cluster and configuring the virtual machine to be highly available.

Cinder includes a service known as the volume service or cinder-volume. The volume service registers itself with the Cinder scheduler on startup and is responsible for creating, modifying, or deleting LUNs on block storage devices. For backends which support multiple writers, multiple copies of this service may be run in Active/Active configuration. The LVM backend (this is the reference backend) is not highly available, though, and may only have one cinder-volume service for each block device. This is because the LVM backend is responsible for providing iSCSI access to a locally attached storage device.

![](bbox=[0.12, 0.78, 0.2, 0.86])

For this reason, highly available deployments of OpenStack should avoid the LVM Cinder backend and instead use a backend that supports multiple cinder-volume services.