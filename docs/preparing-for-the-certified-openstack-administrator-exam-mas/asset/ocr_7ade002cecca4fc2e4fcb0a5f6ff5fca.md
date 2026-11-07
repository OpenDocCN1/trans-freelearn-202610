To view all the individual services in the catalog, run the following command:

```
$ openstack service list
```

To view the URL endpoints that make up the list, use this:

```
$ openstack endpoint list
```

Let's now imagine that our engineers give us the following API URL information about the newly installed Trove service listening on port 8779 in our environment:

+   - **RegionOne: public endpoint:**
+     http://192.168.56.56:8779/v1.0/$(tenant_id)s
+   - **RegionOne: internal endpoint:**
+     http://192.168.56.56:8779/v1.0/$(tenant_id)s
+   - **RegionOne: admin endpoint:**
+     http://192.168.56.56:8779/v1.0/$(tenant_id)s

![](bbox=[0.06, 0.5, 0.16, 0.62])

The Region in the service catalog represents a full Openstack deployment, including its own public, internal, and admin API endpoints. Different Regions can share one set of Keystone and Horizon services, to provide access control and a Web interface. All services in our environment use one region: RegionOne.

Let's begin our advertisement of Trove by creating a listing for the service. This will include the name of the service, a description, and a type:

```
$ openstack service create --name trove --description "OpenStack Trove" database
```

Let's now advertise the provided URLs so our OpenStack users will know how to contact the Trove API. We will advertise the public, internal, and admin API endpoints:

```
$ openstack endpoint create --region RegionOne database public
http://192.168.56.56:8779/v1.0/
$ openstack endpoint create --region RegionOne database internal
http://192.168.56.56:8779/v1.0/
$ openstack endpoint create --region RegionOne database admin
http://192.168.56.56:8779/v1.0/
```

Confirm that the Trove service and endpoints have been added to the catalog:

```
$ openstack catalog list
```