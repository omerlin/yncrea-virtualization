# Docker basics and a bit more

Virtualbox required under Windows7 
not under windows10 if you use the native virtualization

## Installation
Depends on the OS.

* Windows:	https://docs.docker.com/docker-for-windows/
* Mac:      https://docs.docker.com/docker-for-mac/
* ==Linux==:    https://docs.docker.com/engine/install/


!!! Tips 
    If under Linux you don't have the right to execute Docker under your user account
    In that case, you need this:
    ````
      sudo usermod -aG docker $( whoami )
    ````

When docker is installed, a good starting point is this command:
````
docker info
````
This give you details around version and embedded plugins

## Image and runtime

Dockerfile
```
FROM ubuntu
RUN apt-get update && apt-get install -y nginx
COPY index.html /var/share/nginx/html
CMD nginx -g "daemon off";
```


## Network

There are several network drivers :

* ==bridge==: This is the default non routable network - adress 172.17.0.2. Used for standalones application
* host : The container is using the host network
* ==overlay==: Virtual Embedded network added to the host network allowing containers communication (used in Kubernetes)
* None: Deactivated network

### Docker Network LABS

* List the networks on your machine
```
docker network ls
```

* What is the default network ?

One method:

To see the default network, type a command like this one :
```
docker run -ti nginx bash -c "hostname -i"
```

```
docker run -d --name=nginx nginx:alpine
```
Then you can inspect it, looking at the {==Networks==} entries

```
docker inspect nginx
```

* Launch another container

```
docker run --rm -it --name=alpine alpine
```

   - check the network
   - ping the nginx container

To call remotly Nginx:

```
wget -O- 172.17.0.2
```

* Test internet access ?

```
wget -O- wttr.in/Moon
```

* Try a container with no network interface

```
docker run --rm -it --name=alpine_none --net=none alpine
```

Could you check it has no network connectivity ?

* Container with Host network

```
docker run --rm -it --name=alpine_host --net=host alpine
```

Could you check it has access to host network ?  
Why using {==Host network==} ?  

  - Typical use case is a Jenkins CI that must access to host network resources ...

Under the docker container:

```
docker run --rm -it --name=alpine_host --net=host alpine
{ echo -e 'HTTP/1 200 OK\r\n'; echo "Marvelous !"; } | nc -v -l -p 80
```

Outside on the Host:

```
wget -O- localhost
```

* Isolating docker networks by creating another one

```
docker network create another_net
docker run --rm -it --name=alpine_custom --net=another_net alpine
```

Could you check it's really isolated ?

* Externet access

We clean everything

```
docker rm -f $(docker ps -qa)
```

We will use PAT (Port Adress Translation)

```
docker run -d --name=nginx --publish=80 nginx:alpine
```

  - What is the port ?  
  - How do i get it ?  
  - check by calling nginx ...

Luckily you can choose the mapping port

```
docker run -d --name=nginx --publish=8080:80 nginx:alpine
```

### to go further: docker swarm

This Url allows to play with swarm with [play with docker](https://labs.play-with-docker.com/)

TODO: give details here

## Data Storage

![DOCKERLAYER](../files/virtualization/docker_image_layers.png "Docker image layers")

The most common storage drivers are AUFS, Overlay/Overlay2, Devicemapper, Btrfs, and ZFS. 
All storage drivers can be categorized into three different types:

Storage driver category     | Storage drivers 
----------------------------|-----------------
Union filesystems           | AUFS, Overlay, Overlay2
Snapshotting filesystems    | Btrfs, ZFS
Copy-on-write block devices | Devicemapper

![DOCKERLAYER](../files/virtualization/docker_container_images.png "Docker image layers")

### Bind mounts

A simple and traditional way for data sharing between a host and guest :
```
docker run  --rm -ti --volume /home/ubuntu/foo:/foo alpine sh
```

!!! Warning
    Security leak as we are accessing the host volume 
    Not portable

Better option, binding mounts using Volumes

````
docker run -d --name foo -ti --volume /foo alpine sh
docker run -d --name bar -ti --volumes-from foo alpine sh
````

We can see the volume with these commands :

````
docker info|grep "Docker Root Dir"
Docker Root Dir: /var/snap/docker/common/var-lib-docker
sudo ls -als /var/snap/docker/common/var-lib-docker

docker volume ls
````

## Public registry
[Docker Hub](https://hub.docker.com/) is the default public registry.
There are other public registry free for reading image (Redhat http://quay.io for example)

!!! Note
    Don't hesitate to create your own free registry at Docker Hub.

## Private registry

!!! Warning
    Don't confuse between Saas private registry (so managed registry) and company private registry.
    Even at Docker you can have private registry (one free per account)

Private registries are used to manage your company artifacts.
Some common private registry : 

* [Sonatype Nexus](https://www.sonatype.com/nexus/repository-oss) 
* [Jfrog Artifactory](https://jfrog.com/artifactory/)

To use a private registry you need to deploy some private certificate on your Docker client.
Some commands may be restricted - it depends on the server configuration ( "docker search" forbidden for instance)

When you are in a corporate company, your Docker build may rely on remote internet registries
you will need a proxy to build new images.
Several options in front of you:

* Transparent proxy usage
* Add proxy options to your build like
```
--build-args PROXY=proxy.fr.myCompany:8080
```  
* Proxy in the daemon docker configuration
* Or (best option) your private registry is doing the proxy for you

!!! TODO
    Add a schema !!!

## Build image - Slimming stage

### Code to deploy

helloworld.go :

    :::go
    package main
    import "fmt"
      func main() {
      fmt.Println("Hello, world!")
    }

Build :
```
env GOOS=linux GOARCH=amd64 go build -o helloworld .
```

!!! important
    you need to install golang before   
    ```
       # on ubuntu
       sudo apt install golang-go 
    ```

### Beginner

Dockerfile:
```
FROM ubuntu:xenial
COPY hello hello
ENTRYPOINT ["./hello"]
```

What is the issue there ?

### Intermediary level

Dockerfile:
```
FROM alpine:3.8
COPY hello hello
ENTRYPOINT ["./hello"]
```

Much better ...

### Seasoned integrator

Dockerfile using “stage” builds

```Dockerfile
# Step 1
FROM golang:alpine3.7 as build-env
COPY main.go hello/main.go
WORKDIR hello
RUN env GOOS=linux GOARCH=amd64 go build

# Step 2
FROM scratch
COPY --from=build-env /go/hello/hello /go/bin/hello
ENTRYPOINT ["/go/bin/hello"]
```

* My Dockerfile becomes **includes a Continuous Integration tool stage** as the build part is included 
* ... and the size is reduced to the max !

![DOCKERSLIMMING](../files/virtualization/docker_images_slimming.png "Docker image slimming")

## For fun, ask dockerfile for a Go helloworld to chatGPT

[ChatGPT](https://chat.openai.com/chat)