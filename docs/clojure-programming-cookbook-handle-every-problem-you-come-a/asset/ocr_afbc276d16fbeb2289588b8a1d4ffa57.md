```
main" >> /etc/apt/sources.list.d/docker.list
```

```
# install docker
apt-get update
apt-get install docker-engine
```

```
# configure docker for remote access
vi /lib/systemd/system/docker.service
# update line below with a tcp binding:
# ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
```

```
# reload sytemd deamon, to read the just changed docker.service file
systemctl daemon-reload
# restart docker
systemctl restart docker
```

Enough Docker setup for now. Let's create the target Clojure application.

# Preparing the Clojure application

While there are many templates completely ready for us in Clojure, the application used in this recipe will be a compojure app created from scratch.

Create a new project with the following:

```
lein new withdocker
```

Now let's modify the project.clj file a bit:

```
(defproject withdocker "0.1.0-SNAPSHOT"
  :dependencies [[org.clojure/clojure "1.8.0"]
                  [compojure "1.5.1"]
                  [ring/ring-jetty-adapter "1.3.2"]
                  [ring/ring-defaults "0.2.1"]]
  :main withdocker.handler
  :plugins [[lein-ring "0.9.7"]]
  :ring {:handler withdocker.handler/app}
  :profiles {:uberjar {:aot :all}
             :dev {:dependencies
[[javax.servlet/servlet-api "2.5"]
```