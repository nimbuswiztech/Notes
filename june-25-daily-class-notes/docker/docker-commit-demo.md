# Docker Commit Demo

## Docker Commit Demonstration: Creating Custom Images with Java and Tomcat <a href="#docker-commit-demonstration-creating-custom-images" id="docker-commit-demonstration-creating-custom-images"></a>

This comprehensive guide demonstrates how to use Docker commit to create a new image from a running container after installing Java and Tomcat on Ubuntu.

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Before starting, ensure Docker is installed on your system. You can verify this by running:

```
docker --version
```

### Step 1: Pull the Ubuntu Base Image <a href="#step-1-pull-the-ubuntu-base-image" id="step-1-pull-the-ubuntu-base-image"></a>

First, pull the latest Ubuntu image from Docker Hub:

```
docker pull ubuntu:latest
```

This downloads the base Ubuntu image that we'll use as our starting point.

### Step 2: Run Ubuntu Container in Interactive Mode <a href="#step-2-run-ubuntu-container-in-interactive-mode" id="step-2-run-ubuntu-container-in-interactive-mode"></a>

Start an interactive Ubuntu container that allows you to execute commands inside it:

```
docker run -it --name ubuntu-java-tomcat ubuntu:latest /bin/bash
```

The flags used here are:

* **`-i`**: Keeps standard input open for interaction
* **`-t`**: Allocates a pseudo-terminal for command-line interface
* **`--name ubuntu-java-tomcat`**: Assigns a memorable name to the container
* **`/bin/bash`**: Starts the Bash shell inside the container

### Step 3: Update the Package Repository <a href="#step-3-update-the-package-repository" id="step-3-update-the-package-repository"></a>

Once inside the container, update the package lists:

```
apt update
apt upgrade -y
```

This ensures you have access to the latest package versions.

### Step 4: Install Java (OpenJDK) <a href="#step-4-install-java-openjdk" id="step-4-install-java-openjdk"></a>

Install the Java Development Kit, which is required for Tomcat:

```
apt install -y default-jdk
```

Verify the Java installation:

```
java -version
```

You should see output similar to OpenJDK version information.

### Step 5: Create Tomcat User and Group <a href="#step-5-create-tomcat-user-and-group" id="step-5-create-tomcat-user-and-group"></a>

For security purposes, create a dedicated user for running Tomcat:

```
# Create tomcat group
groupadd tomcat

# Create tomcat user with specific settings
useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat

# Create the Tomcat directory
mkdir -p /opt/tomcat
```

### Step 6: Download and Install Apache Tomcat <a href="#step-6-download-and-install-apache-tomcat" id="step-6-download-and-install-apache-tomcat"></a>

Navigate to the temporary directory and download Tomcat:

```
cd /tmp
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.26/bin/apache-tomcat-10.1.26.tar.gz
```

Extract Tomcat to the `/opt/tomcat` directory:

```
tar -xzf apache-tomcat-10.1.26.tar.gz -C /opt/tomcat --strip-components=1
```

Set appropriate permissions:

```
chown -R tomcat: /opt/tomcat
chmod -R u+x /opt/tomcat/bin
```

### Step 7: Configure Tomcat (Optional) <a href="#step-7-configure-tomcat-optional" id="step-7-configure-tomcat-optional"></a>

You can modify Tomcat's default port from 8080 to another port by editing the server configuration:

```
# Edit the server.xml file
nano /opt/tomcat/conf/server.xml
```

Look for the Connector element and modify the port attribute:

```
xml<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000" redirectPort="8443" />
```

### Step 8: Test Tomcat Installation <a href="#step-8-test-tomcat-installation" id="step-8-test-tomcat-installation"></a>

Start Tomcat to verify the installation:

```
/opt/tomcat/bin/startup.sh
```

You should see startup messages indicating Tomcat is running. By default, Tomcat runs on port 8080.

### Step 9: Exit the Container <a href="#step-9-exit-the-container" id="step-9-exit-the-container"></a>

Once you've completed the installation and configuration, exit the container:

```
exit
```

### Step 10: Commit Changes to Create New Image <a href="#step-10-commit-changes-to-create-new-image" id="step-10-commit-changes-to-create-new-image"></a>

Now comes the key step - using `docker commit` to create a new image from your modified container:

```
docker commit -a "Your Name" -m "Ubuntu with Java and Tomcat installed" ubuntu-java-tomcat my-ubuntu-tomcat:v1.0
```

The commit command options are:

* **`-a "Your Name"`**: Sets the author of the image
* **`-m "message"`**: Adds a descriptive commit message
* **`ubuntu-java-tomcat`**: The container name to commit
* **`my-ubuntu-tomcat:v1.0`**: The new image name and tag

### Step 11: Verify the New Image <a href="#step-11-verify-the-new-image" id="step-11-verify-the-new-image"></a>

Check that your new image was created successfully:

```
docker images
```

You should see `my-ubuntu-tomcat` listed among your local images.

### Step 12: Run Container from New Image with Port Exposure <a href="#step-12-run-container-from-new-image-with-port-exp" id="step-12-run-container-from-new-image-with-port-exp"></a>

Now run a container from your newly created image, exposing Tomcat's port to the host:

```
docker run -d -p 8080:8080 --name tomcat-server my-ubuntu-tomcat:v1.0 /opt/tomcat/bin/catalina.sh run
```

The flags used are:

* **`-d`**: Runs the container in detached mode (background)
* **`-p 8080:8080`**: Maps host port 8080 to container port 8080
* **`--name tomcat-server`**: Assigns a name to the running container
* **`/opt/tomcat/bin/catalina.sh run`**: Starts Tomcat in the foreground

### Step 13: Verify Tomcat is Running <a href="#step-13-verify-tomcat-is-running" id="step-13-verify-tomcat-is-running"></a>

Check if your container is running:

```
docker ps
```

Access Tomcat through your web browser by navigating to:

```
thttp://localhost:8080
```

You should see the Tomcat welcome page, confirming that your custom image is working correctly.

### Additional Docker Commit Options <a href="#additional-docker-commit-options" id="additional-docker-commit-options"></a>

The `docker commit` command supports several useful options:

| Option          | Description                                        |
| --------------- | -------------------------------------------------- |
| `-a, --author`  | Set the author name for the image                  |
| `-m, --message` | Add a commit message describing the changes        |
| `-p, --pause`   | Pause the container during commit (default: true)  |
| `-c, --change`  | Apply Dockerfile instructions to the created image |

### Best Practices and Considerations <a href="#best-practices-and-considerations" id="best-practices-and-considerations"></a>

**Security Considerations**:

* Always run Tomcat with a dedicated user account, never as root
* Set appropriate file permissions for Tomcat directories
* Consider changing the default port for additional security

**Docker Commit vs Dockerfile**:

* Docker commit is useful for quick prototyping and debugging
* For production environments, prefer Dockerfiles for better reproducibility
* Committed images can become large; clean up unnecessary files before committing

**Port Management**:

* Exposed ports are metadata that document which ports the application uses
* Published ports actually map container ports to host ports
* Use `-p` flag to publish ports when running containers

### Troubleshooting Common Issues <a href="#troubleshooting-common-issues" id="troubleshooting-common-issues"></a>

**Container Exits Immediately**:

* Ensure you provide a command that keeps the container running
* For Tomcat, use `catalina.sh run` instead of `startup.sh` in detached mode

**Permission Denied Errors**:

* Run Docker commands with `sudo` if you encounter permission issues
* Ensure your user is in the docker group for non-root access

**Port Already in Use**:

* Check if port 8080 is already occupied by another service
* Use a different host port mapping (e.g., `-p 8090:8080`)
