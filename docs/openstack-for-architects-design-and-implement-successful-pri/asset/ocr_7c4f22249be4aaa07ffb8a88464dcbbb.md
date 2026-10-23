![](bbox=[0.12, 0.13, 0.18, 0.22])

For PackStack installations, the file path looks similar to /usr/lib/python2.7/site-packages/ceilometer-2014.2-py2.7.egg-info/entry_points.txt.

While this example was only for Nova, there are preconfigured api_audit_map.conf files for other services such as Glance, Ceilometer, Cinder, Neutron, and so on at https://github.com/openstack/pycadf/tree/master/etc/pycadf. By following the preceding steps and using these api_audit_map.conf files, you can enable all core OpenStack services for CADF eventing.

Since the events are stored to messaging, and events, by default are sent to Ceilometer, this is where you will find your events stored. In order to query Ceilometer for Keystone events, you should first execute a Keystone action that would cause an event, for example:

```
$ openstack user create test_user --os-identity-api-version 3 --os-auth-url http://10.0.2.15:5000/v3 --os-default-domain default
```

In order to get the event that this created out of Ceilometer, you should do the following:

```
$ ceilometer event-list --query
event_type=identity.user.created
```

Which will result in the following type of output:

```
+-------------------------------+-------------------------------------------------------------------
| Event Type                    | Traits                                                            |
+-------------------------------+-------------------------------------------------------------------
| identity.user.created         | +--------------------------------+-----------------------------------
|                               | |        name        | type |
|                               | +--------------------------------+-----------------------------------
| asd123j123312yf34212339e6e424a7f | | initiator_id | string |
| ae7e3k14k202042j22f19bc25c8c09f0 | | project_id | string |
| 22a21766c478927349e3fda7ee0b712b | | resource_id | string |
| identity.openstack.example.com | | service | string |
```