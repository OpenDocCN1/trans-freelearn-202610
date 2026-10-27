Also, as part of this build, I've created a very simple template file and placed it in the `/tmp` directory. This template file is a very simple text file and utilizes the information that we've populated within our KV store that identifies the gift of the Seventh Son of a Seventh Son:

# SeventhSon.tpl

```
Seventh Son of a Seventh Son is a story about a young boy born
with the gift of {{ key "/son/seventh/gift" }}. It has been
popularized in literary form by Orson Scott Card, and musically
by Iron Maiden through their epic album of the same title.
```

Within the template, we can see where Consul is directed to insert the value for the `son/seventh/gift` key.

In order to utilize the template and create a file, consul-template can be run manually. However, we're talking about automating as much as we can through Consul's intelligence. Why would we now resort to manual operations? So, instead of manually running consul-template, we kicked off the application as a background operation as part of the `remote-exec` operation performed when we created the infrastructure:

```
nohup sudo consul-template -consul-addr=\"${self.private_ip}\"
-template \"/tmp/SeventhSon.tpl:/etc/consul/seventh_son.txt\",
```

This command instantiates the consul-template application, identifying the IP address to use to connect to the Consul service. For our infrastructure, the consul-template function is running on the same machine as Consul, but it can live elsewhere and still make use of Consul's data. We are telling consul-template to load the template file `tmp/SeventhSon.tpl` and create a new text file (`/etc/consul/seventh_son.txt`) with that template. As the application is running in the background, consul-template will monitor the value for the identified key, and when that value is changed, consul-template will update the target file as well. We can try this in our own little environment.

If you've been playing along, you've already set a value of `clairvoyance` for the `son/seventh/gift` key. If consul-template is working as designed, we should already have our file created. To validate SSH to one of the clients that are available, execute the following command:

```
$ cat /etc/consul/seventh_son.txt
```