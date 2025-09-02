# Architecture of Docker

##

Docker makes use of a client-server architecture. The Docker client talks with the docker daemon which helps in building, running, and distributing the docker containers. The Docker client runs with the daemon on the same system or we can connect the Docker client with the Docker daemon remotely. With the help of REST API over a  UNIX socket or a network, the docker client and daemon interact with each other.&#x20;

<figure><img src="https://media.geeksforgeeks.org/wp-content/uploads/20221205115118/Architecture-of-Docker.png" alt="Docker Architecture"><figcaption><p> </p></figcaption></figure>

### What is Docker Daemon?

Docker daemon manages all the services by communicating with other daemons. It manages docker objects such as images, containers, networks, and volumes with the help of the API requests of Docker.

### Docker Client

With the help of the docker client, the docker users can interact with the docker. The docker command uses the Docker API. The Docker client can communicate with multiple daemons. When a docker client runs any docker command on the docker terminal then the terminal sends instructions to the daemon. The Docker daemon gets those instructions from the docker client withinside the shape of the command and REST API's request.

The main objective of the docker client is to provide a way to direct the pull of images from the docker registry and run them on the docker host. The common commands which are used by clients are **docker build, docker pull,** and **docker run.**

### Docker Host

A Docker host is a type of machine that is responsible for running more than one container. It comprises the Docker daemon, Images, Containers, Networks, and Storage.

### Docker Registry

All the docker images are stored in the docker registry. There is a public registry which is known as a [**docker hub**](https://www.geeksforgeeks.org/devops/what-is-docker-hub/) that can be used by anyone. We can run our private registry also. With the help of **docker run** or **docker pull** commands, we can pull the required images from our configured registry. Images are pushed into configured registry with the help of the **docker push** command.

### Docker Objects

Whenever we are using a docker, we are creating and use images, containers, volumes, networks, and other objects. Now, we are going to discuss docker objects:-

<figure><img src="https://media.geeksforgeeks.org/wp-content/uploads/20230419173522/Docker-objects.webp" alt="Docker objects" width="1000"><figcaption><p> </p></figcaption></figure>

### Docker Images

An image contains instructions for creating a docker container. It is just a **read-only template**. It is used to store and ship applications. Images are an important part of the docker experience as they enable collaboration between developers in any way which is not possible earlier.

### Docker Containers

Containers are created from docker images as they are ready applications. With the help of Docker API or CLI, we can start, stop, delete, or move a container. A container can access only those resources which are defined in the image unless additional access is defined during the building of an image in the container.

### Docker Storage

We can store data within the writable layer of the container but it requires a storage driver. [Storage driver ](https://www.geeksforgeeks.org/cloud-computing/data-storage-in-docker/)controls and manages the images and containers on our docker host.&#x20;

<figure><img src="https://media.geeksforgeeks.org/wp-content/uploads/20221130192716/BlackBlueModernTutorialYoutubeThumbnail2.jpg" alt="Docker Storage"><figcaption></figcaption></figure>

### Types of Docker Storage&#x20;

1. **Data Volumes:** Data Volumes can be mounted directly into the filesystem of the container and are essentially directories or files on the Docker Host filesystem.
2. **Volume Container**: In order to maintain the state of the containers (data) produced by the running container, Docker volumes file systems are mounted on Docker containers. independent container life cycle, the volumes are stored on the host. This makes it simple for users to exchange file systems among containers and backup data.
3. **Directory Mounts:** A host directory that is mounted as a volume in your container might be specified.&#x20;
4. **Storage Plugins:** Docker volume plugins enable us to integrate the Docker containers with external volumes like Amazon EBS by this we can maintain the state of the container.

### Docker Networking&#x20;

[Docker networking](https://www.geeksforgeeks.org/devops/basics-of-docker-networking/) provides complete isolation for docker containers. It means a user can link a docker container to many networks. It requires very less OS instances to run the workload.

#### Types of Docker Network&#x20;

1. **Bridge:** It is the default network driver. We can use this when different containers communicate with the same docker host.
2. **Host:** When you don't need any isolation between the container and host then it is used.
3. **Overlay:** For communication with each other, it will enable the swarm services.
4. **None:** It disables all networking.
5. **macvlan:** This network assigns MAC(Media Access control) address to the containers which look like a physical address.

\
