# Httpd

## Apache HTTPD Practical Session <a href="#delivering-an-engaging-apache-httpd-practical-sess" id="delivering-an-engaging-apache-httpd-practical-sess"></a>

### 1. Brief Introduction

* **Apache HTTPD** is one of the most widely used web servers.
* Purpose: Serve web content to clients over HTTP/HTTPS.
* Emphasize common, real-world applications: hosting websites, proxies, securing web traffic, handling custom domains.

### 2. Setting Up Apache HTTPD

* **Installation** (typical Linux system example):
  * `sudo apt install apache2` or `sudo yum install httpd`
* **Starting the service**:
  * `sudo systemctl start apache2` _or_ `sudo systemctl start httpd`
* **Verifying installation**:
  * Open a browser and visit `http://localhost` – you should see "It works!" or the Apache default page.

### Critical Apache Configuration Files <a href="#critical-apache-configuration-files" id="critical-apache-configuration-files"></a>

| File Name                   | Purpose                                                                         | Typical Location                     |
| --------------------------- | ------------------------------------------------------------------------------- | ------------------------------------ |
| httpd.conf                  | Main server configuration file; sets core directives (e.g., server root, ports) | /etc/httpd/conf/httpd.conf           |
| apache2.conf / apache.conf  | Alternative main config (varies by OS, like Ubuntu/Debian)                      | /etc/apache2/                        |
| ports.conf                  | Defines which ports Apache will listen on                                       | /etc/apache2/ports.conf              |
| conf.d/ or sites-available/ | Directory (or files) for extra config (modular settings, virtual hosts)         | /etc/httpd/conf.d/                   |
| .htaccess                   | Per-directory configuration; overrides in a specific web directory              | Project root/web folder              |
| mime.types                  | Maps file extensions to MIME types                                              | /etc/httpd/conf/mime.types           |
| ssl.conf                    | SSL/TLS configuration for HTTPS                                                 | /etc/httpd/conf.d/ssl.conf           |
| error\_log/access\_log      | Log files for errors and access, path set via config                            | /var/log/httpd/ or /var/log/apache2/ |

### Key Directives in `httpd.conf` <a href="#undefined" id="undefined"></a>

* **ServerRoot**: Path to Apache’s core files/logs.
* **Listen**: Defines the port (default 80 for HTTP).
* **DocumentRoot**: Directory containing website files (default `/var/www/html`).
* **ServerName**: Hostname or IP address for the server.
* **Directory**: Block to set rules (permissions, options) for specific folders.
* **ErrorLog/CustomLog**: Log file paths for debugging and access monitoring.
* **Include/IncludeOptional**: Add other configuration files for modular settings.
* **VirtualHost**: Support multiple websites/domains on one server.

### `.htaccess` Files

* Used for configuration at the directory level when you don’t have access (or don’t want) to edit `httpd.conf`.
* Common use cases: URL rewrites, access restriction, custom error pages, forcing HTTPS, password-protection.

### Example: Running a Practical Real-Time Scenario <a href="#example-running-a-practical-real-time-scenario" id="example-running-a-practical-real-time-scenario"></a>

### Demonstration Steps

1. **Edit httpd.conf**
   * Set or check `ServerName localhost:80`, adjust `DocumentRoot` if desired.
   * Change `Listen 80` to another port (e.g., 8080) and restart to see the result.
2. **Create a Simple Website**
   * Create an `index.html` inside your `DocumentRoot` and customize content.
3. **Set Up a Virtual Host**
   * Add a `<VirtualHost *:80>` block to serve content for a custom domain.
   * Test by adjusting `/etc/hosts` on your local machine to simulate multiple domains.
4. **Apply `.htaccess` Configuration**
   * In your web directory, create a `.htaccess` file with directives for URL rewriting or deny/allow policies.
5. **Test Logging**
   * Access your server and view the error and access logs. Try visiting invalid URLs and inspect error responses.
6. **Implement Security Features**
   * Restrict access to certain directories or enable SSL with self-signed certificates for HTTPS (for advanced demonstrations).

### Scenario-Based Questions for Student Engagement <a href="#scenario-based-questions-for-student-engagement" id="scenario-based-questions-for-student-engagement"></a>

1. **Configuration Debugging**
   * If Apache isn’t serving your test website after editing `httpd.conf`, which files/logs would you check and why?
2. **Port Conflicts**
   * You want Apache to run alongside another application on port 80. How can you change Apache’s port? Which config directives and files would be involved?
3. **Access Control**
   * How can you limit access to a specific directory, making it available only to users from a certain IP address? Demonstrate using `.htaccess` or `<Directory>` directives.
4. **Custom Error Pages**
   * How would you configure Apache to display a custom page for 404 errors? Which file and directive would you use?
5. **Virtual Host Setup**
   * You need to host two internet domains on the same server. Describe the changes needed in your configuration to implement virtual hosting.
6. **MIME Type Issues**
   * Users report that downloads from your server have the wrong file type. How would you troubleshoot and fix this?
7. **SSL/HTTPS Enablement**
   * Your site needs to enforce secure connections. What configuration changes and files are required to enable HTTPS?
8. **Performance Optimization**
   * Discuss why excessive use of `.htaccess` may impact performance and how to optimize Apache configuration accordingly.\
