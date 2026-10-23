
In the beginning of this chapter, we defined security zones. These zones of networks and resources, along with a proper physical or logical network architecture, provide essential network and resource isolation of the different zones of an OpenStack cloud. For example, zones can be isolated with physical LAN or VLAN separation.

## Assessing risk

With this best practice in mind, an administrator will first need to decide where the threat vectors are in their organization and where they are most vulnerable. One example would be the clouds that have public security zone services such as Horizon open to the Internet. Since these endpoints are exposed to networks and users that are outside the organization's control, this endpoint is an obvious choice for high-security measures. However, securing the public endpoints may not be the only encryption needed on OpenStack endpoints. Depending on the organization's network architecture, threats could come from internal networks that may be connected to more secure OpenStack zones (management or private zones). As a result, it is insufficient to simply rely on OpenStack's default security zone separation in developing encryption security policy. While adding encryption everywhere would certainly add an additional protection to all traffic, simply adding SSL/TLS encryption will provide no protection if an endpoint host is compromised. Therefore, endpoint encryption should simply be a part of a larger network and host-based security strategy.

## Best practices for endpoint security

In today's standard OpenStack deployments, the default traffic over the public network is secured with SSL. While this is a good first step, best security practices dictate that all internal traffic should be treated with the same secure practices. Furthermore, due to recent SSL vulnerabilities, SSL/TLS is the only recommended encryption method unless an organization absolutely needs legacy compatibility.

TLS secures endpoints with the Public Key Infrastructure (PKI) framework. This framework is a set of processes that ensure messages are being sent securely based on verification of the identities of parties involved. PKI involves private and public keys that work in unison in order to create secure connections. These keys are certified by a Certification Authority (CA) and most companies issue their own for internal certificates. Public facing endpoints must use well known public certificate authorities in order to have the connections recognized as secure by most browsers. Therefore, with these certificates being stored in the individual hosts, host security is paramount in order to protect the private key files. If the private keys are compromised, then the security of any traffic using that key is also compromised.

Within the OpenStack ecosystem, support for TLS currently exists through libraries implemented using OpenSSL. We recommend using only TLS 1.2 as 1.1 and 1.0 are vulnerable to attack. SSL (v1-v3.0) should be avoided altogether due to many different public vulnerabilities (Heartbleed, POODLE, and so on). Most implementations today are actually TLS since TLS is the evolution of SSL. Many people call TLS 1.0 SSL 3.1, but it is common to see the newest HTTPS implementations called either SSL/TLS or TLS/SSL. We will be using the former in this document.

It is recommended that all API endpoints in all zones be configured to use SSL/TLS. However, in certain circumstances performance can be impacted due to the processing needed to do the encryption. In use cases of high traffic, the encryption does use a significant amount of resources. In these cases, we recommend using hardware accelerators as possible options in order to offload the encryption from the hosts themselves.

The most common use case to use SSL/TLS for OpenStack endpoints is using an SSL/TLS proxy that can establish and terminate SSL/TLS sessions. While there are multiple options for this, such as Pound, Stud, nginx, HAproxy, or Apache httpd, it will be up to the administrator to choose the tool they would like to use.

## Examples

The following examples will show how to install SSL/TLS for service endpoints using Apache wsgi proxy configuration for Horizon and configuring native TLS support in Keystone. These examples assume that the administrator has already secured certificates from an internal CA source. If you will be publicly offering Horizon or Keystone, it is important to secure certificates from a global CA. These certificates should be recognized by all browsers and have the ability to be validated across the Internet.

Let's start with Keystone:

Now that we have our certificates, let's put them in the right place and give them adequate permissions:

```
# chown keystone /etc/pki/tls/certs/keystone.crt
# chown keystone /etc/pki/tls/private/keystone.key
```

Now we can start the serious security work. We start by adding a new SSL endpoint for Keystone and then delete the old one. First, find your current endpoints:

```
# keystone endpoint-list | grep 5000
```

Now create new ones:

```
# keystone endpoint-create --publicurl https://openstack.example.com:5000/v2.0 --internalurl https://openstack.example.com:5000/v2.0 --adminurl https://openstack.example.com:5000/v2.0 --service keystone
```

Then delete the old one:

```
# keystone endpoint-delete <endpoint-id-from-above-endpoint>
```

Edit your /etc/keystone/keystone.conf file to contain these lines:

```
[ssl]
enable = True
certfile = /etc/pki/tls/certs/keystone.crt
keyfile = /etc/pki/tls/private/keystone.key
```

Now restart the Keystone service and ensure there are no errors:

```
# service openstack-keystone restart
```

Change your environment variables in your keystonerc_admin or any other project environment files to contain the new endpoint by changing OS_AUTH_URL to:

```
OS_AUTH_URL=https://openstack.example.com:35357/v2.0/
# source keystonerc_<user>
export OS_CACERT=/path/to/certificates
```

Give it a quick test:

```
keystone endpoint-list
```

## Securing the Cloud

Assuming the command didn't display any errors, you can now configure the rest of your OpenStack services to connect to Keystone using SSL/TLS. The configurations are all very similar. Here is an example of Nova. You will need to edit the `/etc/nova/nova.conf` file to look like this:

```ini
[keystone_authtoken]
auth_protocol = https
auth_port = 35357
auth_host = openstack.example.com
auth_uri = https://openstack.example.com:5000/v2.0
cafile = /path/to/ca.crt
```

Cinder, Glance, Neutron, Swift, Ceilometer, and so on are all the same. Here is a list of configuration files you will need to edit:

- `/etc/openstack-dashboard/local_settings` (Horizon access to Keystone)
- `/etc/ceilometer/ceilometer.conf`
- `/etc/glance/glance-api.conf`
- `/etc/neutron/neutron.conf`
- `/etc/neutron/api-paste.ini`
- `/etc/neutron/metadata_agent.ini`
- `/etc/glance/glance-registry.conf`
- `/etc/cinder/cinder.conf`
- `/etc/cinder/api-paste.ini`
- `/etc/swift/proxy-server.conf`

Once these or any other service configuration files have been modified, restart all of your services and test your cloud:

```bash
# openstack-service restart
# service httpd restart
# openstack-status
```

You should see all of your services running and online. You have now encrypted traffic between your OpenStack services and Keystone, a very important step in keeping your cloud secure. If not, check your services log files for errors.

Now we configure Horizon to encrypt connections to clients using SSL/TLS.

On the controller node, add the following (or modify the existing configuration to match) in `/etc/apache2/apache2.conf` on Ubuntu and `/etc/httpd/conf/httpd.conf` on RHEL:

```
<VirtualHost <ip address>:80>
    ServerName <site FQDN>
    RedirectPermanent / https://<site FQDN>/
</VirtualHost>
<VirtualHost <ip address>:443>
    ServerName <site FQDN>
    SSLEngine On
    SSLProtocol +TLSv1 +TLSv1.1 +TLSv1.2
    SSLCipherSuite HIGH:!RC4:!MD5:!aNULL:!eNULL:!EXP:!LOW:!MEDIUM
    SSLCertificateFile      /path/<site FQDN>.crt
    SSLCACertificateFile    /path/<site FQDN>.crt
    SSLCertificateKeyFile /path/<site FQDN>.key
    WSGIScriptAlias / <WSGI script location>
    WSGIDaemonProcess horizon user=<user> group=<group> processes=3 threads=10
    Alias /static <static files location>
    <Directory <WSGI dir>>
        # For http server 2.2 and earlier:
        Order allow,deny
        Allow from all

        # Or, in Apache http server 2.4 and later:
        # Require all granted
    </Directory>
</VirtualHost>
```

On the compute servers, the default configuration of the `libvirt` daemon is to not allow remote access. However, live migrating an instance between OpenStack compute nodes requires remote `libvirt` daemon access between the compute nodes. Obviously, unauthenticated remote access is not allowable in most cases; therefore, we can set up `libvirtd` TCP socket with SSL/TLS for the encryption and X.509 client certificates for authentication.
