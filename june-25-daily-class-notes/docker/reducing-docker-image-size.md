# Reducing Docker Image Size

***

### 1. Choose a Minimal Base Image <a href="#id-1-choose-a-minimal-base-image" id="id-1-choose-a-minimal-base-image"></a>

Using a lean starting point removes unnecessary components from day one.

* **Alpine Linux**\
  – Alpine images are typically 5–7 MB versus hundreds of MB in Debian/Ubuntu.\
  – Caveat: some binaries rely on GNU libc; you may need Alpine’s `glibc` package or choose an Alpine-compatible build.
* **Distroless or Scratch**\
  – `scratch` starts from an empty image; ideal for statically compiled Go, Rust, or C++ binaries.\
  – Google’s Distroless images include only your language runtime and application code, omitting shell and package managers.

***

### 2. Leverage Multi-Stage Builds <a href="#id-2-leverage-multi-stage-builds" id="id-2-leverage-multi-stage-builds"></a>

Break your Dockerfile into **build** and **runtime** stages to discard build-time dependencies.

```
# Build stage
FROM golang:1.21-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o myapp

# Runtime stage
FROM scratch
COPY --from=builder /app/myapp /myapp
ENTRYPOINT ["/myapp"]
```

* Only the final compiled binary is copied; all Go toolchain and source files are left behind.

***

### 3. Minimize Layers and Combine Commands <a href="#id-3-minimize-layers-and-combine-commands" id="id-3-minimize-layers-and-combine-commands"></a>

Every `RUN`, `COPY`, or `ADD` instruction creates a new layer. Fewer layers → smaller image.

*   **Combine related operations:**

    ```
    RUN apt-get update && \
        apt-get install -y --no-install-recommends \
          ca-certificates \
          curl \
        && rm -rf /var/lib/apt/lists/*
    ```
* Clean caches and lists in the same `RUN` step to avoid leaving them in intermediate layers.

***

### 4. Clean Up Package Manager Caches <a href="#id-4-clean-up-package-manager-caches" id="id-4-clean-up-package-manager-caches"></a>

Remove unnecessary files immediately after installation:

* **APT (Debian/Ubuntu):**\
  `rm -rf /var/lib/apt/lists/*`
* **YUM (CentOS/RHEL):**\
  `yum clean all && rm -rf /var/cache/yum`
* **apk (Alpine):**\
  `apk --no-cache add <packages>`

***

### 5. Exclude Unneeded Files via .dockerignore <a href="#id-5-exclude-unneeded-files-via-dockerignore" id="id-5-exclude-unneeded-files-via-dockerignore"></a>

Use a `.dockerignore` file to prevent copying source control metadata, docs, tests, and local configs into the build context.

Example `.dockerignore`:

```
.git
node_modules
*.md
*.log
tests/
```

***

### 6. Use Language-Specific Best Practices <a href="#id-6-use-language-specific-best-practices" id="id-6-use-language-specific-best-practices"></a>

Tailor cleanup to your language ecosystem:

* **Node.js:**
  * Install only production dependencies:\
    `npm ci --production` or `yarn install --production`
  * Prune devDependencies:\
    `npm prune --production`
* **Python:**
  * Use a requirements file with only needed packages.
  * Use `pip install --no-cache-dir -r requirements.txt` to skip pip cache.
* **Java:**
  * Build a “fat” JAR outside the image, then use `FROM openjdk:jre` instead of `jdk`.
  * Consider tools like JLink or GraalVM Native Image to produce a minimal runtime.

***

### 7. Remove Build-Only Tools and Files <a href="#id-7-remove-build-only-tools-and-files" id="id-7-remove-build-only-tools-and-files"></a>

In multi-stage builds, ensure compilers, build caches, and documentation are left in earlier stages:

* Delete source archives, `.o` files, and intermediate artifacts.
* Avoid copying the entire source directory into the final image.

***

### 8. Analyze and Optimize Image Contents <a href="#id-8-analyze-and-optimize-image-contents" id="id-8-analyze-and-optimize-image-contents"></a>

Use Docker’s built-in tools to inspect and prune:

* `docker image ls` and `docker history <image>`
* `docker image inspect <image>`
* [Dive](https://github.com/wagoodman/dive): interactive exploration of each layer’s contents.

Identify large files or directories that can be excluded or compressed.

***

### 9. Compress or Archive Assets <a href="#id-9-compress-or-archive-assets" id="id-9-compress-or-archive-assets"></a>

Where appropriate, bundle static assets (e.g., web assets) into compressed archives and extract at runtime, or serve them via an external CDN to avoid embedding heavy assets in the image.

***

### 10. Automate Image Slimming <a href="#id-10-automate-image-slimming" id="id-10-automate-image-slimming"></a>

Integrate regular image audits and size checks into your CI pipeline:

* Fail builds if image exceeds a size threshold.
* Report layer sizes and trending growth over time.

\
