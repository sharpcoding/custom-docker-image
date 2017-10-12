# Custom docker image
Scripts and documentation facilitating creation of customized Docker images and sending them to Kubernetes. All focused on simplicity.

A [minimalistic Docker image of Ubuntu](http://phusion.github.io/baseimage-docker) has been found. 

With the following Dockerfile:
```
FROM phusion/baseimage:latest
RUN apt-get update
RUN apt-get -y install iputils-ping
RUN apt-get -y install mc
```
there are *ping*, *curl* and *mc* commands available, with Midnight Commander mc as a kind of luxurious extension for instances used for development purposes. The baked image weights 323 MB:

```
REPOSITORY   TAG SIZE
tiny-ubuntu  v1  323MB
```

## Baking a Docker image

Start shell/cmd and run the following in the same directory where the [Dockerfile](/Dockerfile) is placed:
```
docker-compose build
```

See that the image has been created successfully:
```
docker images --all
```
the command above should result in something like:
```
REPOSITORY   TAG    IMAGE ID      CREATED       SIZE
ubuntu       latest b47cb2d60bbe  39 hours ago  122MB
tiny-ubuntu  v1     a77509368a8d  4 months ago  225MB
```

Run the image (which will result in creating a container):
```
docker run tiny-ubuntu:v1
```

View created container:
```
docker ps
```

It should display something like:
```
CONTAINER ID  IMAGE            COMMAND         CREATED        STATUS        PORTS NAMES
8d9736464db0  tiny-ubuntu:v1   "/sbin/my_init" 5 minutes ago  Up 5 minutes        nervous_shaw
```
From right now we will use a number of 8d9736464db0 - the container ID, which definitely will be different on your machine. Run shell on a container:
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