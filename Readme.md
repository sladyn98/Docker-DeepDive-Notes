# Docker Deep Dive Notes


To understand docker we need to deep dive into the docker engine and its modularized components. Let us learn more about it

## Tools that Docker Engine is modularized into


* **Container runtime specification** : TO-DO

* **containerd** : TO-DO

* **runc** : It is a CLI tool for spawning and running containers according to something called as the OCI specification. It has a single purpose in life and that is to create containers. containerd provides runc with a valid OCI bundle for runc to make a container of


# Docker networking

Every container has its own IP address. If you start a container and inspect it using the `docker inspect` command you should see something like this : 

```
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "a7b8d2171e11af6fed0c95caebe28716dc5476bb9460a4dbb71b13e3c7e57c03",
                    "EndpointID": "",
                    "Gateway": "",
                    "IPAddress": "172.168.1.13",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }
```

In the above output the IPAddress key shows the address on which the container is being run on. If you execute a `curl` on this IPAddress it will output the result of the api call. 

Each docker container uses a network and by default this network is `bridge`.You can have a look at the available networks by using the following command.

* `docker network ls` : This command lists out the networks avaiable or technically lists all the networks the Engine `daemon` knows about. Example output can be seen here : 

```

NETWORK ID          NAME                DRIVER              SCOPE
a7b8d2171e11        bridge              bridge              local
4257f48825f8        host                host                local
000a4584959d        none                null                local

```
    
As you can see there are three networks here: host, bridge and none. The scope of each network is local.


## Publishing docker Ports

By default, when you create or run a container using docker create or docker run, it does not publish any of its ports to the outside world. To make a port available to services outside of Docker, or to Docker containers which are not connected to the container’s network, use the --publish or -p flag. This creates a firewall rule which maps a container port to a port on the Docker host to the outside world. Here is an example

* ` docker run -it -P image_name` : This command assigns the exposed port on the container to any random port on the host machine.

* `docker run -it -p 8080:80` : This command assigns the exposed port 8080 on the container to the port 80 on the host machine. 

* `docker run -it --network=host image_name` : This command exposes all of the servies on `localhost`.

* `docker run -it --network=none image_name` : This command choses the `none` network ie no network is chosen.

# Storage in Docker

There are three types of methods to store/share information between the host and the container 

* Volumes

* Bindmounts

* tmpfs

Let us look at each of these options in depth and when would be the best case at using each one of them. 


### Bindmounts 

A bind mount is a file or folder stored anywhere on the container host filesystem, mounted into a running container.


### Volumes

* Decoupling container from storage
* Share volume among different containers
* Attach volume to containers
* On deleting container volume does not delete


TO-DO

a) Complete the architecture deep dive

b) Complete the storage commands

c) Tagging and other docker nuances