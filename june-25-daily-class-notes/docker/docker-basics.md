# Docker basics

### Docker Teaching Materials

### 1. Introduction to Docker

* **Concept:** Docker is a platform to develop, ship, and run applications in lightweight, portable containers.
* **Key Points:** Containers isolate applications; they share the host OS kernel but run as if on their own machine.
* **Difference vs VMs:** VMs virtualize hardware, containers virtualize OS. Containers are faster and use fewer resources.
* **Use cases:** Simplifying deployment, CI/CD, microservices.

### 2. Docker Fundamentals

* **Docker Architecture:**
  * Docker daemon runs on your machine handling container lifecycle.
  * Docker CLI is the command tool you interact with.
  * Images are the blueprint, containers are running instances.
* **Registries:** Docker Hub is a public registry to find and share images.

### 3. Setting Up Docker Environment

* **Installation:** Guide links or instructions to install Docker on Windows/Mac/Linux.
* **First command:** `docker run hello-world` - shows Docker installed correctly.
* **Container management:**
  * Start: `docker start <container>`
  * Stop: `docker stop <container>`
  * Remove: `docker rm <container>`

### 4. Working with Docker Images

* **Images & Layers:** Images are made of layers stacked; each command in a Dockerfile adds a layer.
* **Pull from Docker Hub:** `docker pull <image>`
* **Build your image:**
  *   Dockerfile example:

      ```
      textFROM ubuntu:20.04
      RUN apt-get update && apt-get install -y nginx
      CMD ["nginx", "-g", "daemon off;"]
      ```
  * Build: `docker build -t my-nginx .`
* **Push to Hub:**
  * Tag: `docker tag my-nginx username/my-nginx:tag`
  * Push: `docker push username/my-nginx:tag`

### 5. Docker Containers Deep Dive

* **Managing containers:**
  * List running: `docker ps`
  * Execute command: `docker exec -it <container> bash`
  * View logs: `docker logs <container>`
* **Volumes for data:**
  * Create volume: `docker volume create myvolume`
  * Run container with volume: `docker run -v myvolume:/data myimage`
* **Networking:** Expose ports with `-p hostPort:containerPort`.

### 6. Docker Compose

*   **Compose file example:**

    ```
    textversion: "3"
    services:
      web:
        image: nginx
        ports:
          - "8080:80"
      db:
        image: mysql
        environment:
          MYSQL_ROOT_PASSWORD: example
    ```
* **Commands:**
  * `docker-compose up` to start
  * `docker-compose down` to stop

### 7. Advanced Docker Concepts

* **Custom networks:** Create bridge networks with `docker network create mynetwork`
* **Data management:** Use volumes and bind mounts for persistent data
* **Security:** Run as non-root user, scan images for vulnerabilities
* **Orchestration overview:** Brief mention of Docker Swarm and Kubernetes

### 8. Hands-on Projects

* Guide to build a multi-container app with web and database using Docker Compose.
* Instructions to optimize images by minimizing layer count and clearing caches.

### 9. Troubleshooting & Best Practices

* Common errors: Port conflicts, permission issues, out of disk space.
* Cleanup: `docker system prune`

### 10. Review & Q\&A

* Recap commands, concepts
* Encourage questions and pointers to official Docker docs and tutorials.

***

If you want, I can prepare detailed slides, lab exercises, or cheat sheets to accompany these materials. Let me know your preference!

docker run commanddocker run command

The **`docker run` command** is used to create and start a new container from a specified Docker image. Its basic syntax is:

```
 docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

At minimum, you must specify an **image**; options and command arguments allow customization.

### Common Examples <a href="#common-examples" id="common-examples"></a>

*   **Start a container with Ubuntu interactively:**

    ```
    docker run -it ubuntu:latest bash
    ```

    _`-it`_ starts an interactive session with a pseudo-terminal, and `bash` runs inside the container.
*   **Run container in the background (detached mode):**

    ```
    docker run -d nginx
    ```

    _`-d`_ will keep your terminal free by running the container in the background.
*   **Assign a name to the container:**

    ```
    docker run --name mynginx nginx
    ```

    _`--name`_ sets a custom name for the container, making management easier.
*   **Expose container ports to host:**

    ```
    docker run -p 8080:80 nginx
    ```

    _`-p 8080:80`_ maps host port 8080 to container port 80.
*   **Remove container after exit:**

    ```
    docker run --rm ubuntu echo "Hello World"
    ```

    _`--rm`_ automatically removes the container when it exits.

### How the `docker run` Command Works <a href="#undefined" id="undefined"></a>

* Checks if the image exists locally, pulls if not.
* Creates a container, assigns resources and namespaces.
* Starts the container and executes the specified command (or the image default).
* Optionally attaches your terminal if using interactive mode.

### Important Options <a href="#important-options" id="important-options"></a>

* **`-it`**: Interactive and allocates a terminal.
* **`-d`**: Detached/background mode.
* **`--name`**: Set container name.
* **`-p`**: Port mapping.
* **`-v`**: Mount volumes.
* **`--rm`**: Auto-remove container after exit.
* **`-e`**: Set environment variables.
* **`--entrypoint`**: Override the default entrypoint.
* **`--network`**: Attach to specified network.

More options can be listed by running:

```
docker run --help
```

This command is foundational for working with Docker containers in all application scenarios



To expose Nginx running in a Docker container to your local machine, use the **`docker run -p hostPort:containerPort nginx`** command. For example, to map your machine’s port 8080 to port 80 inside the container (where Nginx listens by default), run:

```
bashdocker run -d --name nginx-server -p 8080:80 nginx
```

* **`-d`** runs Nginx in detached mode.
* **`--name nginx-server`** gives your container a custom name.
* **`-p 8080:80`** maps host port 8080 to container port 80, exposing Nginx to `http://localhost:8080` on your machine.

If you want to use the standard HTTP port (80), replace 8080 with 80. You can confirm Nginx is running and accessible by browsing to `http://localhost:8080` (or the port you mapped).

You can run multiple containers by mapping each one to a different host port, like:

```
bashdocker run -d --name nginx-alt -p 8081:80 nginx
```

This approach ensures your local machine can access Nginx via the mapped port.

\
