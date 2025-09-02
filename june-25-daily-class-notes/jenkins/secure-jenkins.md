# Secure jenkins

To **secure Jenkins**, follow a layered approach that protects the application, system, credentials, and users. Below is a **comprehensive checklist** with real-time DevOps context and best practices.

***

### 🔐 1. **Enable Global Security in Jenkins**

#### ✅ Steps:

* Go to `Manage Jenkins → Configure Global Security`
* Enable **“Enable Security”**
* Choose authentication method:
  * Jenkins own user database
  * LDAP / Active Directory
  * GitHub OAuth / Google SSO (via plugin)

***

### 🧑‍🔒 2. **Use Role-Based Access Control (RBAC)**

#### ✅ Best Practice: Use Matrix-based or Role Strategy plugin

* Install **Role-based Authorization Strategy** plugin
* Define roles like:
  * **admin**: all permissions
  * **developer**: limited to job creation and build
  * **viewer**: read-only

#### 🔒 Disable Anonymous Access

* Remove all permissions from “anonymous” user.

***

### 🔐 3. **Use Credentials Plugin Securely**

#### ✅ Do:

* Store secrets (API keys, tokens, SSH keys) in **Jenkins Credentials Store**
* Use `withCredentials` block in Jenkinsfile

```groovy
withCredentials([string(credentialsId: 'my-token', variable: 'TOKEN')]) {
    sh 'curl -H "Authorization: Bearer $TOKEN" https://api.prod.com/deploy'
}
```

#### ❌ Don’t:

* Never hardcode secrets in pipeline scripts or environment variables.

***

### 🌐 4. **Secure Jenkins URL and Network**

#### ✅ Use HTTPS:

* Run Jenkins behind **Nginx/Apache with SSL**
* Use **Let’s Encrypt** or internal SSL certificate

#### ✅ Firewall Rules:

* Only allow Jenkins access from trusted IPs or VPN
* Block public access to port **8080**

***

### 📦 5. **Restrict Script and Plugin Execution**

#### ✅ Sandbox Groovy Scripts:

* Enforce **Groovy sandbox** for non-admins
* Review scripts in **In-Process Script Approval**

#### ✅ Update Plugins Regularly:

* Go to `Manage Jenkins → Plugin Manager → Updates`
* Uninstall unused plugins

***

### 📈 6. **Monitoring, Auditing & Backups**

#### ✅ Install:

* **Audit Trail Plugin** → tracks UI actions
* **Monitoring Plugin** → view JVM memory, threads
* **ThinBackup Plugin** or backup `/var/lib/jenkins/`

#### ✅ Log Important Events:

* Configure `jenkins.log`
* Monitor who logs in, what builds are run, config changes, etc.

***

### ⚙️ 7. **Disable Unused Features**

| Feature                  | Disable via                            |
| ------------------------ | -------------------------------------- |
| CLI                      | Global Security → Uncheck “Enable CLI” |
| Remoting/Agent Protocols | `jenkins.args` or security settings    |
| Script Console           | Restrict to admin only                 |

***

### 🔒 8. **Harden Jenkins on OS Level**

| OS Hardening Step          | Description                               |
| -------------------------- | ----------------------------------------- |
| Run as a **non-root** user | Use dedicated `jenkins` user              |
| Limit file permissions     | Jenkins home should not be world-readable |
| Secure `jenkins.service`   | Use systemd to restrict system access     |

***

### 🔁 9. **Regular Maintenance & Alerts**

* Set up Slack or Email notifications for failed builds or login attempts
* Rotate credentials regularly
* Periodically audit users and permissions

***

### ✅ Summary Checklist

| Task                                        | Status |
| ------------------------------------------- | ------ |
| \[ ] Enable authentication and RBAC         | ✅      |
| \[ ] Disable anonymous access               | ✅      |
| \[ ] Use credentials plugin (no hardcoding) | ✅      |
| \[ ] Set up HTTPS and reverse proxy         | ✅      |
| \[ ] Install Audit Trail & Monitoring       | ✅      |
| \[ ] Regularly update Jenkins/plugins       | ✅      |
| \[ ] Limit who can run Groovy/Script        | ✅      |
| \[ ] Backup Jenkins (e.g., ThinBackup)      | ✅      |
