# Sonnar qube installation

Here are step-by-step instructions to install SonarQube on an AWS EC2 instance running  Amazon Linux 2

### Step 1: Launch EC2 Instance

* Log in to AWS Management Console.
* Launch an EC2 instance:
  * Choose Amazon Linux 2 or Ubuntu Server.
  * Select instance type (e.g., t2.medium or better recommended for SonarQube).
  * Configure security groups to allow inbound ports:
    * SSH (port 22) for access
    * HTTP (port 9000) for SonarQube UI

### Step 2: Connect to EC2 Instance

*   Use SSH to connect to your instance from your terminal:

    ```
    textssh -i /path/to/your-key.pem ec2-user@<EC2-PUBLIC-IP>
    ```

### Step 3: Update System Packages

For Amazon Linux 2:

```
textsudo yum update -y
```

Or for Ubuntu:

```
textsudo apt update && sudo apt upgrade -y
```

### Step 4: Install Java (SonarQube requires Java 11 or higher)

Amazon Linux 2:

```
sudo amazon-linux-extras install java-openjdk11 -y
```

Ubuntu:

```
sudo apt install openjdk-11-jdk -y
```

Verify Java installation:

```
java -version
```

### Step 5: Create SonarQube User

Create a dedicated user to run SonarQube:

```
sudo useradd sonar
sudo passwd sonar
```

### Step 6: Download SonarQube

Download the latest SonarQube Community Edition from the official site:

Navigate to /opt (or you can choose a directory of your preference):

```
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.0.0.60804.zip
```

(Note: Replace the link with the current version from the SonarQube download page.)

### Step 7: Unzip SonarQube

```
sudo yum install unzip -y   # For Amazon Linux
sudo apt install unzip -y   # For Ubuntu
sudo unzip sonarqube-10.0.0.60804.zip
sudo mv sonarqube-10.0.0.60804 sonarqube
sudo chown -R sonar:sonar sonarqube
```

### Step 8: Configure SonarQube

Edit SonarQube config file:

```
sudo vi /opt/sonarqube/conf/sonar.properties
```

* Set database connection if you plan to use an external database (by default it uses embedded H2 for testing).
* (Optional) Edit `sonar.web.host=0.0.0.0` to make it accessible on all interfaces.\
  Save and exit.

### Step 9: Install and Configure PostgreSQL (Optional, for production)

SonarQube recommends PostgreSQL or MySQL. For testing, embedded DB is fine.

If you want PostgreSQL installed:

```
sudo amazon-linux-extras enable postgresql13
sudo yum install postgresql postgresql-server postgresql-devel postgresql-contrib postgresql-docs -y
sudo service postgresql initdb
sudo service postgresql start
sudo -i -u postgres psql
```

Then create database and user for SonarQube.

### Step 10: Start SonarQube

Switch to sonar user:

```
sudo su - sonar
```

Start SonarQube:

```
cd /opt/sonarqube/bin/linux-x86-64
./sonar.sh start
```

### Step 11: Access SonarQube Web Interface

*   Open your browser and navigate to:

    ```
    http://<EC2-PUBLIC-IP>:9000
    ```
*   Login with default credentials:

    ```
    Username: admin
    Password: admin
    ```

### Step 12: Optional - Configure SonarQube as a service

Create systemd service so SonarQube starts on boot:

Create file `/etc/systemd/system/sonarqube.service` with content:

```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonar
Restart=always

[Install]
WantedBy=multi-user.target
```

Reload daemon and enable service:

```
sudo systemctl daemon-reload
sudo systemctl enable sonarqube
sudo systemctl start sonarqube
```

***

Let me know if you want me to adjust this for a different OS or to cover production settings with an external database.

\
