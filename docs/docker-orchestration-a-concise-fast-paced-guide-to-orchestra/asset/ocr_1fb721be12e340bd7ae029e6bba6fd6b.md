# Installing Rancher agents

Once Rancher has been started, it can create new hosts using any upstream provider that is supported by `docker-machine`. Log in to the web UI, go to **Hosts** in the INFRASTRUCTURE menu, and click on **Add Host**. A wizard will walk you through adding the host to the cloud environment:

![](bbox=[0.103, 0.234, 0.895, 0.604])

It is also possible to add the agent manually. Select the **Custom** option in **Add Host**. There will be a Docker command that will need to be run on each Docker host that is being added to the cluster. It will look something like the command listed here:

```
sudo docker run -d --privileged -v
/var/run/docker.sock:/var/run/docker.sock -v
/var/lib/rancher:/var/lib/rancher rancher/agent:v1.0.2
http://172.17.8.101:8080/v1/scripts/781E07FE518B7181D563:1477180800000:N3gz
UQgoo3mFPcGkqEimBoNExFC
```