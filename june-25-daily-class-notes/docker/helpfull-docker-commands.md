# Helpfull docker commands

<table><thead><tr><th width="165">Category</th><th width="272">Command</th><th width="373">Description</th><th width="288">Example</th><th width="369">Use case</th></tr></thead><tbody><tr><td>Basic Info</td><td>docker --help</td><td>Display all available Docker commands and options</td><td>docker --help</td><td>Getting started - see all available commands</td></tr><tr><td>Basic Info</td><td>docker version</td><td>Show Docker version information for client and server</td><td>docker version</td><td>Check installed Docker version</td></tr><tr><td>Basic Info</td><td>docker info</td><td>Display system-wide Docker information</td><td>docker info</td><td>Get detailed Docker system information</td></tr><tr><td>Image Management</td><td>docker images</td><td>List all Docker images on your system</td><td>docker images</td><td>See all downloaded/built images</td></tr><tr><td>Image Management</td><td>docker pull</td><td>Download an image from Docker registry (Docker Hub)</td><td>docker pull ubuntu:latest</td><td>Download images before using them</td></tr><tr><td>Image Management</td><td>docker build</td><td>Build an image from a Dockerfile</td><td>docker build -t myapp:latest .</td><td>Create custom images from Dockerfile</td></tr><tr><td>Image Management</td><td>docker rmi</td><td>Remove one or more images</td><td>docker rmi image_id</td><td>Delete unwanted images to save space</td></tr><tr><td>Image Management</td><td>docker image ls --filter "dangling=true"</td><td>List dangling images (untagged images)</td><td>docker image ls --filter "dangling=true"</td><td>Find images that need cleanup (from our history)</td></tr><tr><td>Image Management</td><td>docker image prune</td><td>Remove dangling images</td><td>docker image prune</td><td>Clean up unused dangling images (from our history)</td></tr><tr><td>Container Management</td><td>docker run</td><td>Create and start a new container from an image</td><td>docker run -d -p 8080:80 nginx</td><td>Launch applications in containers</td></tr><tr><td>Container Management</td><td>docker ps</td><td>List running containers</td><td>docker ps</td><td>See what containers are currently running</td></tr><tr><td>Container Management</td><td>docker ps -a</td><td>List all containers (running and stopped)</td><td>docker ps -a</td><td>See all containers including stopped ones</td></tr><tr><td>Container Management</td><td>docker start</td><td>Start a stopped container</td><td>docker start container_name</td><td>Resume a previously stopped container</td></tr><tr><td>Container Management</td><td>docker stop</td><td>Stop a running container</td><td>docker stop container_name</td><td>Gracefully shutdown a running container</td></tr><tr><td>Container Management</td><td>docker restart</td><td>Restart a container</td><td>docker restart container_name</td><td>Restart container after configuration changes</td></tr><tr><td>Container Management</td><td>docker rm</td><td>Remove one or more containers</td><td>docker rm container_name</td><td>Delete stopped containers to free space</td></tr><tr><td>Container Interaction</td><td>docker exec -it</td><td>Execute commands inside a running container</td><td>docker exec -it container_name /bin/bash</td><td>Access container shell for debugging/configuration</td></tr><tr><td>Container Interaction</td><td>docker logs</td><td>Fetch logs from a container</td><td>docker logs -f container_name</td><td>Debug issues by viewing container logs</td></tr><tr><td>Container Interaction</td><td>docker inspect</td><td>Get detailed information about a container or image</td><td>docker inspect container_name</td><td>Get detailed configuration and status info</td></tr><tr><td>Container Interaction</td><td>docker stats</td><td>Display live resource usage statistics</td><td>docker stats</td><td>Monitor container resource consumption</td></tr><tr><td>Container Interaction</td><td>docker cp</td><td>Copy files between container and host</td><td>docker cp file.txt container_name:/path/</td><td>Transfer files to/from containers</td></tr><tr><td>Network Management</td><td>docker network ls</td><td>List all Docker networks</td><td>docker network ls</td><td>See available networks for containers</td></tr><tr><td>Network Management</td><td>docker network create</td><td>Create a new network</td><td>docker network create mynetwork</td><td>Create custom networks for container communication</td></tr><tr><td>Volume Management</td><td>docker volume ls</td><td>List all Docker volumes</td><td>docker volume ls</td><td>See persistent storage volumes</td></tr><tr><td>Volume Management</td><td>docker volume create</td><td>Create a new volume</td><td>docker volume create myvolume</td><td>Create persistent storage for containers</td></tr><tr><td>Cleanup</td><td>docker container prune</td><td>Remove all stopped containers</td><td>docker container prune</td><td>Clean up stopped containers (from our history)</td></tr><tr><td>Cleanup</td><td>docker system prune</td><td>Remove unused data (containers, networks, images)</td><td>docker system prune</td><td>General cleanup of unused Docker resources</td></tr><tr><td>Cleanup</td><td>docker system df</td><td>Show Docker disk usage</td><td>docker system df</td><td>Check how much disk space Docker is using</td></tr></tbody></table>

### Essential Command Patterns to Practice <a href="#essential-command-patterns-students-should-practic" id="essential-command-patterns-students-should-practic"></a>

**Basic Workflow Commands:**

```
bash# Pull, run, and manage containers
docker pull nginx
docker run -d --name webserver -p 8080:80 nginx
docker ps
docker logs webserver
docker stop webserver
docker rm webserver
```

**Debugging and Inspection:**

```
bash# Access container shell and inspect
docker exec -it container_name /bin/bash
docker inspect container_name
docker stats
```

**Cleanup and Maintenance:**

```
bash# Regular cleanup routine
docker container prune
docker image prune
docker system prune
docker system df
```
