```
$ docker-compose up -d
Unable to find image 'docker/compose:1.5.0' locally
1.5.0: Pulling from docker/compose

f5f17db9ef7c: Pull complete
1a5d7604b84c: Pull complete
edb9562a1a85: Pull complete
83f27339e1ae: Pull complete
fe338db44539: Pull complete
fd10e3d2c1a4: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:146e95775e3e1a79e0e31632d3b1842f84debed41a222188eb73d6c95163175b
Status: Downloaded newer image for docker/compose:1.5.0
dockercouchbase41_couchbase1_1 is up-to-date
Starting fa82e35b4f_dockercouchbase41_couchbase3_1
Starting dockercouchbase41_couchbase2_1
```

# Check if Docker processes are running

We will then check if Docker processes are running. docker ps shows the status of Docker processes, and docker-compose ps shows the status of docker-compose processes:

```
$ docker ps
$ docker-compose ps
```

```
$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS
96e137dfc3cc   arungupta/couchbase  "/entrypoint.sh /opt/"  3 minutes ago    Up 3 minutes    8091-8093/tcp, 11207/tcp, 11210-11211/tcp, 18091-18
02/tcp          dockercouchbase41_couchbase2_1
ca9790431358   arungupta/couchbase  "/entrypoint.sh /opt/"  3 minutes ago    Up 3 minutes    0.0.0.0:8091-8093->8091-8093/tcp, 11207/tcp, 11211/
tcp, 0.0.0.0:11210->11210/tcp, 18091-18092/tcp   dockercouchbase41_couchbase3_1
6bb9483dff68   arungupta/couchbase  "/entrypoint.sh /opt/"  3 minutes ago    Up 3 minutes    8091-8093/tcp, 11207/tcp, 11210-11211/tcp, 18091-18
02/tcp          dockercouchbase41_couchbase1_1
$ docker-compose ps
Name                           Command                State   Ports
-------------------------------------------------------------------------------
dockercouchbase41_couchbase1_1  /entrypoint.sh /opt/couchb ...  Up      11207/tcp, 11210, 11211/tcp,
18091/tcp, 18092/tcp, 8091/tcp,
8092/tcp, 8093/tcp
dockercouchbase41_couchbase2_1  /entrypoint.sh /opt/couchb ...  Up      11207/tcp, 11210/tcp, 11211/tcp,
18091/tcp, 18092/tcp, 8091/tcp,
8092/tcp, 8093/tcp
dockercouchbase41_couchbase3_1  /entrypoint.sh /opt/couchb ...  Up      11207/tcp, 0.0.0.0:11210->11210/tcp,
11211/tcp, 18091/tcp, 18092/tcp,
0.0.0.0:8091->8091/tcp,
0.0.0.0:8092->8092/tcp,
0.0.0.0:8093->8093/tcp
$
```

```
$ docker inspect --format '{{ .NetworkSettings.IPAddress }}' dockercouchbase41_couchbase1_1
172.17.0.3
$ docker inspect --format '{{ .NetworkSettings.IPAddress }}' dockercouchbase41_couchbase2_1
172.17.0.5
```