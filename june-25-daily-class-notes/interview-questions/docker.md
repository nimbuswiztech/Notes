# Docker

scenario-based Docker questions with answers designed to help you understand practical Docker usage and prepare for interviews:

1. **Scenario: You need to create a Docker image for a Python web application. How would you do it?**\
   &#xNAN;_&#x41;nswer:_ Write a Dockerfile that uses an official Python base image, copies the application code, installs dependencies using `pip`, and sets the command to run the web app. Then build the image using `docker build` and run it with `docker run`.
2. **Scenario: How would you share data between a Docker container and the host machine?**\
   &#xNAN;_&#x41;nswer:_ Use Docker volumes or bind mounts to map a directory from the host into the container, ensuring persistent storage and data sharing.
3. **Scenario: Your Docker container is using too much memory. How would you diagnose and fix it without restarting?**\
   &#xNAN;_&#x41;nswer:_ Use `docker stats` to monitor resource usage and `docker exec` to get shell access inside the container to check running processes. Limit memory usage using Docker run options like `--memory` and optimize the app to reduce memory consumption.
4. **Scenario: You need to run multiple containers that depend on one another. How would you manage this setup?**\
   &#xNAN;_&#x41;nswer:_ Use Docker Compose to define multi-container applications, specifying services, networks, volumes, and dependencies in a single `docker-compose.yml` file.
5. **Scenario: How do you ensure sensitive data such as passwords are secure in Docker containers?**\
   &#xNAN;_&#x41;nswer:_ Use Docker Secrets or environment variables injected at runtime and avoid hardcoding secrets in images or Dockerfiles.
6. **Scenario: Your team works across multiple environments (dev, staging, prod) using Docker. How would you handle environment-specific configs?**\
   &#xNAN;_&#x41;nswer:_ Use environment variables, `.env` files, or separate Docker Compose override files for each environment to manage environment-specific settings.
7. **Scenario: You want to update your Dockerized application without downtime. How can you achieve this?**\
   &#xNAN;_&#x41;nswer:_ Use rolling updates with orchestrators like Docker Swarm or Kubernetes to deploy new container versions while keeping the service available.
8. **Scenario: How would you troubleshoot a Docker container that fails to start?**\
   &#xNAN;_&#x41;nswer:_ Check container logs with `docker logs`, inspect container configuration with `docker inspect`, and try interactive shell access with `docker exec` to debug.
9. **Scenario: You want to scale a web application using Docker. How?**\
   &#xNAN;_&#x41;nswer:_ Use Docker Swarm or Kubernetes to scale container replicas horizontally using `docker service scale` or deploying multiple pods.
10. **Scenario: You need persistent storage for a database running in a Docker container. What approach would you use?**\
    &#xNAN;_&#x41;nswer:_ Use Docker volumes to store data outside the container filesystem to persist data across container restarts and recreations.
11. **Scenario: How do you optimize Docker images for size?**\
    &#xNAN;_&#x41;nswer:_ Use multi-stage builds to remove build dependencies from the final image and choose minimal base images like Alpine Linux.
12. **Scenario: You have multiple microservices; how do you manage their communication securely inside Docker?**\
    &#xNAN;_&#x41;nswer:_ Use Docker networks to isolate and connect containers securely, leveraging network policies and custom bridges.
13. **Scenario: How do you handle logging for Docker containers in production?**\
    &#xNAN;_&#x41;nswer:_ Configure centralized logging with tools like ELK stack, Fluentd, or Docker’s built-in logging drivers for monitoring and analysis.
14. **Scenario: What is the use of health checks in Docker containers?**\
    &#xNAN;_&#x41;nswer:_ Health checks allow Docker to monitor container’s state and restart or alert if the application inside is not healthy.
15. **Scenario: You want to make your Docker containers more secure. How?**\
    &#xNAN;_&#x41;nswer:_ Run containers as a non-root user, limit container capabilities, use read-only filesystems, scan images for vulnerabilities, and use Docker Content Trust for image verification.
16. **Scenario: How would you link two containers in Docker without using legacy links?**\
    &#xNAN;_&#x41;nswer:_ Use Docker networks to connect containers and allow them to discover each other by container name.
17. **Scenario: How do you perform zero-downtime deployments with Docker?**\
    &#xNAN;_&#x41;nswer:_ Use orchestration platforms to perform rolling updates, gradually replacing containers with the new version.
18. **Scenario: How do you manage configuration changes in containers without rebuilding images?**\
    &#xNAN;_&#x41;nswer:_ Use environment variables, config files mounted as volumes, or Docker Configs in Swarm.
19. **Scenario: Your container image depends on several layers; how do you reduce build time?**\
    &#xNAN;_&#x41;nswer:_ Optimize Dockerfile commands to cache layers efficiently, minimize layer count, and avoid unnecessary files in the build context.
20. **Scenario: What would you do if a container leaks sensitive information in its logs?**\
    &#xNAN;_&#x41;nswer:_ Avoid printing secrets in logs, configure logging drivers that mask or exclude sensitive data, and implement centralized log access controls.
21. **Scenario: How can you rollback a faulty container update?**\
    &#xNAN;_&#x41;nswer:_ Use orchestrator features to redeploy the previous known good image version or manually run the prior image.
22. **Scenario: How do you handle persistent configuration data for containers in a distributed setup?**\
    &#xNAN;_&#x41;nswer:_ Use shared volumes, distributed storage solutions, or Docker Configs and Secrets for storing configs accessible by multiple containers.
23. **Scenario: Your containerized app needs to communicate across different Docker hosts. How do you set this up?**\
    &#xNAN;_&#x41;nswer:_ Use overlay networks in Docker Swarm or Kubernetes services to enable cross-host container communication securely.
24. **Scenario: How would you monitor and alert on Docker container health in production?**\
    &#xNAN;_&#x41;nswer:_ Use monitoring tools like Prometheus, Grafana, or Docker’s built-in health checks and alerts integrated with notification systems.
25. **Scenario: You need to run a batch job inside a container periodically. How would you implement this?**\
    &#xNAN;_&#x41;nswer:_ Use cron jobs on the host or orchestrator’s scheduled jobs (e.g., Kubernetes CronJobs) to run containers periodically.
26. **Scenario: How do you handle Docker image versioning and rollout in CI/CD?**\
    &#xNAN;_&#x41;nswer:_ Tag images with semantic versions or build numbers and configure CI/CD pipelines to deploy specific tags.
27. **Scenario: You want to reduce build time by caching dependencies in your Dockerfile. How?**\
    &#xNAN;_&#x41;nswer:_ Separate dependency installation into a separate Dockerfile layer before copying application code, so only changed files bust the cache.
28. **Scenario: How do you recover data from a deleted Docker container?**\
    &#xNAN;_&#x41;nswer:_ If volumes were used, data persists outside containers. Otherwise, container data is lost unless backed up.
29. **Scenario: How do you perform multi-stage builds in Docker and why?**\
    &#xNAN;_&#x41;nswer:_ Use multiple FROM statements in Dockerfile to separate build stages, copying only necessary artifacts to the final image, reducing size and attack surface.
30. **Scenario: How would you design a microservices application using Docker containers and Docker Compose?**\
    &#xNAN;_&#x41;nswer:_ Create individual Dockerfiles for each microservice, define services with their dependencies and networks in `docker-compose.yml`, and use shared volumes and networks for inter-service communication.
