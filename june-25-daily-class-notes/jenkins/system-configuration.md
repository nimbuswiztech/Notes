# System Configuration

### **System Configuration**

#### 1. **Home Directory**

* **Path:** `/var/lib/jenkins`
* Where Jenkins stores:
  * Job configs
  * Builds
  * Plugins
  * Logs
  * Credentials (in encrypted form)

***

#### 2. **System Message**

* Text box to display a **global banner** on the Jenkins homepage.
* Useful for maintenance notifications, downtime alerts, or announcements.

***

#### 3. **Maven Project Configuration**

**Global `MAVEN_OPTS`:**

*   Java options for all Maven builds (e.g., memory limits, proxy):

    ```
    -Xmx1024m -Djava.net.useSystemProxies=true
    ```

**Local Maven Repository:**

* You can override default `~/.m2/repository`
* Useful for caching or sharing repo location in agents.

***

#### 4. **Executors**

* `# of Executors`: Set how many parallel jobs this master node (or agent) can run.
* **2** means Jenkins master can run 2 jobs at once.

***

#### 5. **Labels**

* Tags assigned to the node for **job targeting** (e.g., `linux`, `docker`, `gpu`)

***

#### 6. **Usage**

* Set to “Use this node as much as possible” (default behavior for job scheduling)

***

#### 7. **Computer Retention Check Interval**

* Internal setting — how often (in seconds) Jenkins checks for idle agents for potential disconnection (default: 60s)

***

#### 8. **Quiet Period**

* Wait time (in seconds) before starting a job after it's triggered.
* **5** = Wait 5 seconds before triggering a build.

***

#### 9. **SCM Checkout Retry Count**

* If source code checkout fails, Jenkins will retry this many times.
* **0** = no retries.

***

#### 10. **Restrict Project Naming**

* Used to limit naming formats for new jobs. If enabled, only allowed patterns can be used.

***

### 🌍 **Jenkins Location**

#### Jenkins URL:

* Used in:
  * Emails
  * Webhooks
  * CLI interactions
* **Current**: `http://34.230.89.21:8080/`

#### System Admin E-mail:

* Used as the **"from" address** for Jenkins notifications.
* **Not configured**: Set to `nobody@nowhere` by default.

***

### 🗃️ **Global Properties**

* Define:
  * **Environment variables**
  * **Custom tool paths**
  * These apply to all jobs and agents.

***

### ⚙️ **Tool Locations**

* Override automatic tool detection by specifying paths (e.g., Git, Java, Maven, Python)

***

### 📉 **Disk Space Monitoring Thresholds**

* Configures warning levels when disk space is low

***

### 📦 **Pipeline Speed / Durability**

* Controls how Jenkins saves pipeline state:
  * **Default** = high durability (safe but slower)
  * Can be tuned for performance vs resilience.

***

### 📊 **Usage Statistics**

* Sends anonymous usage data to Jenkins developers to improve the tool.

***

### 🌐 **HTTP Proxy Configuration**

* Configure proxy (host, port, username) to allow Jenkins to access the internet (plugins, updates).

***

### ⏳ **Global Build Time Out**

* Sets a maximum timeout for **all** jobs unless overridden per job.

***

### 📸 **Develocity (Formerly Gradle Enterprise)**

* Enterprise build analysis and metrics integration.
* Only used if Develocity is in use.

***

### 🕒 **Timestamper**

* Format timestamps for console output logs.
  * System clock format
  * Elapsed time format
  * Useful for debugging long pipelines

***

### 🔎 **Administrative Monitors**

* Warnings and health checks about Jenkins:
  * Disk full
  * Plugin updates
  * Security issues
* Can disable individual alerts if not relevant.

***

### ♻️ **Global Build Discarders**

* Cleanup old build data **globally** to save disk space.
* E.g., delete builds older than 30 days or keep only last 10.

***

### 🔔 **Notifications**

#### Default Notification URL:

* URL to notify other systems (usually used in integrations)

#### GitHub / GitHub Enterprise Servers:

* GitHub API tokens, server URLs, webhooks

***

### 📚 **Global Shared Libraries**

#### Trusted:

* Pipeline libraries with full script access (no sandbox)
* Used for reusable shared logic (e.g., `@Library('devops-lib')`)

#### Untrusted:

* Libraries run in sandboxed mode (restricted)

***

### 🧪 **Build-timeout Plugin**

* Add timeouts at **build step level**, not just the whole job

***

### 🔧 **Git Plugin**

* Set global Git config for all jobs:
  * `user.name`
  * `user.email`
* Options for commit summaries, tag actions, credential masking, etc.

***

### 🐚 **Shell**

* Path to shell executable (e.g., `/bin/bash`)
* Used for "Execute Shell" steps in freestyle or scripted jobs.

***

### 📧 **Extended E-mail Notification**

* Advanced email settings:
  * SMTP server and port
  * Default suffix (e.g., `@example.com`)
  * Headers like `Precedence: bulk`
  * Default content and templates
  * Ability to test email configuration

***

### ✅ **Buttons: Save / Apply**

* **Save**: Saves and closes the configuration
* **Apply**: Saves without leaving the page
