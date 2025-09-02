# Docker Networking

<figure><img src="https://miro.medium.com/v2/resize:fit:1280/0*COnaKh6TSeynIUEa.png" alt="" height="348" width="640"><figcaption></figcaption></figure>

### **Docker Network** <a href="#e823" id="e823"></a>

* Docker networking is primarily used to establish communication between Docker containers and the outside world via the host machine.
* Docker Networks are used to provide complete isolation for Docker containers.
* Docker uses Linux’s [Namespace](https://medium.com/@BeNitinAgarwal/understanding-the-docker-internals-7ccb052ce) for resource isolation, which includes network resources.
* Docker isolates the network through Network Namespace and provides an independent network environment, including network cards, routing, Iptable rules, and so on.
* When Docker is installed, a default bridge network named `docker0` is created. Each new Docker container is automatically attached to this network unless a custom network is specified.
* If you do an ifconfig on the Docker Host, you will see the Docker Ethernet adapter.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*B5FjMavJ1pBPreu61uYzSw.png" alt="" height="263" width="700"><figcaption></figcaption></figure>

Also, check if you need to know about [docker0 and etho](https://stackoverflow.com/questions/37536687/what-is-the-relation-between-docker0-and-eth0).

Docker supports networking for its containers via network drivers. These drivers have several network drivers.

* Bridge
* Host
* Overlay
* Macvlan
* None

### The Bridge Driver <a href="#id-214e" id="id-214e"></a>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*VDNvPBLuTnXc9cZF.png" alt="" height="653" width="700"><figcaption></figcaption></figure>

* It is a private default network created on the host.
* Containers linked to this network have an internal IP address through which they communicate with each other easily.
* The Docker server (daemon) creates a virtual ethernet bridge docker0 that operates automatically, by delivering packets among various network interfaces.

**Let’s see in action**

```
$ docker network ls
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*YuNcF4GdCh4VeacRGPckPg.png" alt="" height="95" width="700"><figcaption></figcaption></figure>

We will create two nginx containers and see which network it attaching to.

```
$ docker run -itd --name nginx1 nginx$ docker run -itd --name nginx2 nginx$ docker ps $ docker network inspect bridge |grep Name
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*Zb3eRIb8K6ZJl4yg-LMPeQ.png" alt="" height="205" width="700"><figcaption></figcaption></figure>

Here, both Nginx containers are attached to the bridge network.

### The Host Driver <a href="#id-7f0a" id="id-7f0a"></a>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*JwZ2n9HIbDncu2-2.png" alt="" height="436" width="700"><figcaption></figcaption></figure>

* It is a public network.
* The container will use the host’s IP and port to run services inside the container.
* It removes network isolation between the container and the host machine where Docker is running
* For example, If you run a container that binds to port 80 and uses host networking, the container’s application is available on port 80 on the host’s IP address.
* One limitation with the host driver is that it does work on Linux hosts but not on Windows and Mac.
* It does not require network address translation (NAT).

```
$ docker network ls
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*u0SKHIstBCSWzFXLcbGzeQ.png" alt="" height="116" width="700"><figcaption></figcaption></figure>

Create an httpd container in host networking

```
$ docker run -itd --net host --name host_web httpd
```

To check the container created within-host network

```
$ docker network inspect host |grep Name
```

The IP address of the container within-host network is null.

```
$ docker network inspect host |grep IPv4Address
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*mmcgBBymh9CiZO5DJ9BXSg.png" alt="" height="189" width="700"><figcaption></figcaption></figure>

Access the web container with Public IP.

<figure><img src="https://miro.medium.com/v2/resize:fit:978/1*TtUZhlHulbtaWjuH5TCbAg.png" alt="" height="104" width="489"><figcaption></figcaption></figure>

## Overlay Driver <a href="#id-6bb6" id="id-6bb6"></a>

<figure><img src="https://miro.medium.com/v2/resize:fit:1146/0*1xmsjESIUD4wGe32.png" alt="" height="299" width="573"><figcaption></figcaption></figure>

* Overlay allows containers across the host to communicate with each other without worrying about the setup.
* It is for multi-host network communication, as with Docker Swarm or Kubernetes.
* It creates an internal private network that spans across all the nodes participating in the swarm cluster
* Think of an overlay network as a distributed virtualized network that’s built on top of an existing computer network.

```
$ docker network ls
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*k_S2Ad9gulJHFVSJ24pLdA.png" alt="" height="79" width="700"><figcaption></figcaption></figure>

You don’t see any overlay network driver created by docker and can’t create it for the common use of docker container.

Let try to create it.

```
$ docker network create -d overlay myStack1
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*9v2NFuiWt3FFAs1yp-Bk4A.png" alt="" height="60" width="700"><figcaption></figcaption></figure>

In order to use the Overlay network, we have to use the Swarm service. we will see the Docker Swarm section.

## Macvlan Driver <a href="#d814" id="d814"></a>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*LRqQVdswEDKMp5p8.png" alt="" height="713" width="700"><figcaption></figcaption></figure>

* Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network.
* The Docker daemon routes traffic to containers by their MAC addresses.
* Using the `macvlan` driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network.
* It is suitable when a user wants to directly connect the container to the physical network rather than the Docker host.

```
docker network ls
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*k_S2Ad9gulJHFVSJ24pLdA.png" alt="" height="79" width="700"><figcaption></figcaption></figure>

You don’t see any macvlan network driver created by docker.

Create MACVLAN network “mvnet” bound to eth0 on the host.

```
$ docker network create -d macvlan --subnet 192.168.0.0/24 --gateway 192.168.0.1 -o parent=eth0 mvnet
```

Create two containers C1 and C2 on the “mvnet” network and ping the C1 from C2 with the container’s C1 IP address.

```
$ docker run -itd --name C1 --net mvnet --ip 192.168.0.3 busybox sh
$ docker run -it --name C2 --net mvnet --ip 192.168.0.4 busybox sh
$ ping 192.168.0.3
```

In the above example, we have created a macvlan network `eth0` on the host and also attach two containers to the macvlan network and show that they can ping between themselves with the container’s IP.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*U5OawhtJGlaF_TeV-_NqUg.png" alt="" height="248" width="700"><figcaption></figcaption></figure>

## None Driver <a href="#bbd7" id="bbd7"></a>

<figure><img src="https://miro.medium.com/v2/resize:fit:600/0*TTU1P5TmtIVUYQVG.png" alt="" height="229" width="300"><figcaption></figcaption></figure>

* In this kind of network, containers are not attached to any network.
* It does not have any access to the external network or other containers.
* This network is used when you want to completely disable the networking stack on a container and, only create a loopback device.

```
$ docker network ls
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*k_S2Ad9gulJHFVSJ24pLdA.png" alt="" height="79" width="700"><figcaption></figcaption></figure>

## Basic Docker Networking Commands <a href="#id-8321" id="id-8321"></a>

**List down the Networks associated with Docker**

```
$ docker network ls
```

**Creating a Network**

```
$ docker network create mynetwork
```

**Disconnecting a Container from the Network**

```
$ docker network disconnect mynetwork 0f8d7a833f42
```

**Displays detailed information on one or more networks.**

```
$ docker network inspect mynetwork 
```

**Remove all Unused Networks**

```
$ docker network prune
```

[Docker Networking](https://medium.com/tag/docker-networking?source=post_page-----bdebb478781f---------------------------------------)[\
](https://medium.com/tag/docker-network?source=post_page-----bdebb478781f---------------------------------------)
