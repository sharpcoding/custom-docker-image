# Custom docker image
This repository contains scripts and documentation focused on creating customized Docker images and sending them to Kubernetes. This is a guide for developers, focused on simplicity.

In the first step a [minimalistic Docker image of Ubuntu](http://phusion.github.io/baseimage-docker) was found.

## Details regarding the image

With the following Dockerfile:
```
FROM phusion/baseimage:latest
RUN apt-get update
RUN apt-get -y install iputils-ping
RUN apt-get -y install mc
```
there are *ping*, *curl* and *mc* commands available, with Midnight Commander mc as a kind of luxurious extension for instances used for development purposes. The image weights 323 MB:

```
REPOSITORY   TAG SIZE
tiny-ubuntu  v1  323MB
```

## Baking it yourself

Start shell/cmd and run the following in the same directory where the [Dockerfile](/Dockerfile) is placed:
```
docker-compose build
```

Verify that the image has been created successfully:
```
docker images --all
```
it result in something like:
```
REPOSITORY   TAG    IMAGE ID      CREATED       SIZE
ubuntu       latest b47cb2d60bbe  39 hours ago  122MB
tiny-ubuntu  v1     a77509368a8d  4 months ago  225MB
```

Start the image (which will result in creating a container):
```
docker run tiny-ubuntu:v1
```

View the created container:
```
docker ps
```

It should display something like:
```
CONTAINER ID  IMAGE            COMMAND         CREATED        STATUS        PORTS NAMES
8d9736464db0  tiny-ubuntu:v1   "/sbin/my_init" 5 minutes ago  Up 5 minutes        nervous_shaw
```
From right now we will use a kind of random number 8d9736464db0 - this is the container ID, which will be different in your session. Run the Linux shell in container:
```
docker exec -it 8d9736464db0 bash
```

Verify the ping command is available:
```
root@e5a676c2ff47:/# ping -c 4 windity.com
PING windity.com (139.162.210.148) 56(84) bytes of data.
64 bytes from li1373-148.members.linode.com (139.162.210.148): icmp_seq=1 ttl=37 time=0.429 ms
64 bytes from li1373-148.members.linode.com (139.162.210.148): icmp_seq=2 ttl=37 time=0.825 ms
64 bytes from li1373-148.members.linode.com (139.162.210.148): icmp_seq=3 ttl=37 time=0.746 ms
64 bytes from li1373-148.members.linode.com (139.162.210.148): icmp_seq=4 ttl=37 time=0.825 ms

--- windity.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3131ms
rtt min/avg/max/mdev = 0.429/0.706/0.825/0.164 ms
root@e5a676c2ff47:/#
```

Kill the container:
```
docker container kill 8d9736464db0
```