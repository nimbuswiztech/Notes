# Multi stage docker file

### **What are Docker Multi-Stage Builds?** <a href="#undefined" id="undefined"></a>

Docker multi-stage builds are a powerful feature that allows you to use multiple **`FROM`** statements in a single Dockerfile. Each **`FROM`** instruction begins a new stage in the build process, enabling you to create optimized, production-ready images by separating build dependencies from runtime requirements.

**Key Benefits:**

* **Reduced Image Size**: Eliminate build tools and dependencies from final image (50-70% size reduction)
* **Enhanced Security**: Minimize attack surface by excluding unnecessary components
* **Improved Build Efficiency**: Leverage Docker's layer caching and parallel execution
* **Simplified Workflow**: Single Dockerfile instead of multiple files

### **How Multi-Stage Builds Work** <a href="#undefined" id="undefined"></a>

In traditional single-stage builds, everything (build tools, dependencies, source code) ends up in the final image. Multi-stage builds solve this by:

1. **Build Stage**: Uses full-featured base image with all build tools
2. **Runtime Stage**: Uses minimal base image and copies only necessary artifacts

Each stage can copy artifacts from previous stages using **`COPY --from=<stage-name>`**.

### **Real-Time Examples and Demos** <a href="#undefined" id="undefined"></a>

## Multistage Dockerfile for Building and Running hello-world <a href="#multistage-dockerfile-for-building-and-running-spm" id="multistage-dockerfile-for-building-and-running-spm"></a>

This Dockerfile performs the following steps in distinct stages:

1. **Clone** the Git repository.
2. **Build** the project using Maven.
3. **Package** the generated WAR into an Apache Tomcat base image.
4. **Expose** the HTTP port and **run** Tomcat.

Save this as `Dockerfile` in an empty host folder and build with `docker build -t spm-hello-world .`.

```
# Use a base image that supports Java and Apache Tomcat
FROM maven:3.9.1-eclipse-temurin-17 AS builder

# Install git
RUN apt-get update \
    && apt-get install -y --no-install-recommends git \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app   

# Clone the repository
RUN git clone https://github.com/nimbuswiztech/spm-hello-world.git

# Set working directory
WORKDIR /app/spm-hello-world

# Build the WAR
RUN mvn clean install -DskipTests

# Stage 2: Package into Tomcat
FROM tomcat:10.1-jdk17 AS final

# Remove the default Tomcat webapps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the webapp.war file to the webapps directory of Tomcat
COPY --from=builder /app/spm-hello-world/webapp/target/webapp.war /usr/local/tomcat/webapps/ROOT.war

# Expose the default Tomcat port (8080)
EXPOSE 8080

# Start Tomcat when the container starts
CMD ["catalina.sh", "run"]




```

### Usage <a href="#usage" id="usage"></a>

1.  Build the image:

    ```
    docker build -t spm-hello-world .
    ```
2.  Run the container:

    ```
    docker run -d -p 8080:8080 --name hello-world-app spm-hello-world
    ```
3. Access the application in your browser at `http://localhost:8080/`.

***

This multistage Dockerfile keeps the final image minimal by discarding build tools and source code, copying only the generated `WAR` into Tomcat.
