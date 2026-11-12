# Using shared network storage for Docker volumes

One of the biggest challenges with running a Docker cluster is figuring out what to do with the data. Docker supports using **volumes** to store data. By default, the volumes store data on the Docker host. What happens if that container is started on another host? None of the data is available. For most applications, this is a problem.

Docker solved this problem by creating a pluggable storage system. Volumes can be created that connect to a wide variety of storage systems. Those systems can be anything from traditional network storage such as a **Network File System (NFS)** and **Internet Small Computer System Interface (iSCSI)** to cloud services such as Amazon S3 or clustered storage systems such as Gluster or Ceph.

Each storage option has its pros and cons. Some may be better suited to some workloads than others. Fortunately, you can choose the best solution on a per-container basis. A public web server may run in Amazon EC2 and need storage there while a local file server connects to a local Ceph cluster.

A volume is created using the `docker volume create` command. The following example creates a volume named `vol1` using a fictional driver named `foo`:

```
$ docker volume create --driver foo --name vol1 --opt bar=1 --opt baz=2
```

The driver a volume uses is defined by the `--driver` flag. Each driver has its own set of options that are set adding one or more `--opt` flags. Be sure to read the documentation for each storage driver you choose to use.

# Introducing Ceph

Ceph is a distributed, clustered storage solution developed by Red Hat. It provides an object store, block storage, and a shared filesystem. Ceph can also provide compatibility with Amazon S3 and OpenStack Swift with the RADOS Gateway. Multiple storage pools and storage tiering are also possible. It can also self-heal when there are problems in the cluster making it an excellent choice for providing storage for network services.