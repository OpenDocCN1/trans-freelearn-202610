Figure 7.2 breaks down the anatomy of a Heat template.

# Anatomy of a Heat Template

```
```yaml
heat_template_version: 2013-05-23
description: Test Stack

parameters:
  ServerName:
    type: string
    description: A name for the server
  Image:
    type: string
    description: Image ID used to boot the server
  Flavor:
    type: string
    description: Flavor of server
  Net:
    type: string
    description: Network ID for the server
  Volume:
    type: string
    description: A volume ID to attach to the server

resources:
  server1:
    type: OS::Nova::Server
    properties:
      name: { get_param: ServerName }
      image: { get_param: Image }
      flavor: { get_param: Flavor }
      networks:
        - network: { get_param: Net }

  volumeattach:
    type: OS::Cinder::VolumeAttachment
    properties:
      volume_id: { get_param: Volume }
      instance_uuid: { get_resource: server1 }

outputs:
  myinstance_ip:
    description: IP address of the first instance
    value: { get_attr: [server1, first_address] }
```
```

# Template Header

+   - version (corresponds with OpenStack Version)
+     2013-05-23 = Grizzly, Havana, and Icehouse
+   - description (optional)

# Parameters

+   - specify any input parameters (optional)

# Resources

+   - OpenStack resources you are creating (mandatory)

# Outputs

+   - Information provided to a user after the stack has been created, i.e. IP addresses, URLs, UUIDs, passwords, etc.

Figure 9.2: Anatomy of a Heat template