# Bash and shell

sections:

1. **Basic Bash/Shell Scenarios (1–10)** – covering fundamentals & quick automation.
2. **Intermediate Scenarios (11–20)** – involving loops, functions, file parsing, automation.
3. **Advanced/Real-time DevOps Scenarios (21–30)** – production-grade scripts, monitoring, AWS/Linux integration, CI/CD.

***

### **1. Basic Scenarios (1–10)**

**Q1.**\
**Scenario:** You are given a log file `application.log`. You need to extract only error lines containing the keyword `"ERROR"` and save them into `error.log`.\
**Answer:**

```bash
grep "ERROR" application.log > error.log
```

_Real-time use case:_ Filtering errors during troubleshooting without opening huge logs.

***

**Q2.**\
**Scenario:** You want to check if a directory `/opt/data` exists. If it does, print “Directory exists”, else create it.\
**Answer:**

```bash
if [ -d "/opt/data" ]; then
    echo "Directory exists"
else
    mkdir -p /opt/data
fi
```

_Real-time use case:_ Preparing folders for app deployments in automation scripts.

***

**Q3.**\
**Scenario:** You want to find all `.log` files in `/var/log` that are larger than 50MB.\
**Answer:**

```bash
find /var/log -type f -name "*.log" -size +50M
```

_Real-time use case:_ Identifying large logs consuming disk space.

***

**Q4.**\
**Scenario:** Print the current date in the format `YYYY-MM-DD` in a script.\
**Answer:**

```bash
echo $(date +%F)
```

_Real-time use case:_ Naming backup files with date stamps.

***

**Q5.**\
**Scenario:** Write a script that checks if a service `nginx` is running, and if not, start it.\
**Answer:**

```bash
if ! systemctl is-active --quiet nginx; then
    systemctl start nginx
    echo "Nginx started"
fi
```

_Real-time use case:_ Self-healing scripts for production services.

***

**Q6.**\
**Scenario:** List the 10 most CPU-consuming processes.\
**Answer:**

```bash
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -n 11
```

_Real-time use case:_ Quick diagnosis of CPU spikes.

***

**Q7.**\
**Scenario:** Convert all `.txt` files in `/data` to `.bak` extension.\
**Answer:**

```bash
for file in /data/*.txt; do
    mv "$file" "${file%.txt}.bak"
done
```

_Real-time use case:_ Batch renaming during migrations.

***

**Q8.**\
**Scenario:** Find the number of failed SSH login attempts from `/var/log/auth.log`.\
**Answer:**

```bash
grep "Failed password" /var/log/auth.log | wc -l
```

_Real-time use case:_ Security monitoring for brute-force attacks.

***

**Q9.**\
**Scenario:** Write a one-liner to delete empty files from `/tmp`.\
**Answer:**

```bash
find /tmp -type f -empty -delete
```

_Real-time use case:_ Cleaning up temp files to save disk space.

***

**Q10.**\
**Scenario:** Append system uptime to a file `/var/log/uptime_report.log`.\
**Answer:**

```bash
uptime >> /var/log/uptime_report.log
```

_Real-time use case:_ Collecting uptime history for performance tracking.

***

### **2. Intermediate Scenarios (11–20)**

**Q11.**\
**Scenario:** Create a script that takes a filename as input and counts the number of lines in it.\
**Answer:**

```bash
#!/bin/bash
file=$1
if [ -f "$file" ]; then
    wc -l < "$file"
else
    echo "File not found"
fi
```

_Real-time use case:_ Quick metrics gathering from log files.

***

**Q12.**\
**Scenario:** Write a script to monitor free memory, and if it's less than 500MB, send an alert.\
**Answer:**

```bash
free_mem=$(free -m | awk '/Mem:/ {print $4}')
if [ "$free_mem" -lt 500 ]; then
    echo "Low memory alert: ${free_mem}MB free" | mail -s "Memory Alert" admin@example.com
fi
```

_Real-time use case:_ Automating infra monitoring.

***

**Q13.**\
**Scenario:** Archive `/var/log/*.log` files into a single tar.gz file with today’s date.\
**Answer:**

```bash
tar -czf logs_$(date +%F).tar.gz /var/log/*.log
```

_Real-time use case:_ Daily log archival for compliance.

***

**Q14.**\
**Scenario:** Rename all `.jpg` files in a directory with a sequential number format.\
**Answer:**

```bash
count=1
for file in *.jpg; do
    mv "$file" "image_${count}.jpg"
    ((count++))
done
```

_Real-time use case:_ Standardizing file names before uploading to a web server.

***

**Q15.**\
**Scenario:** Create a script that monitors if a given URL is accessible.\
**Answer:**

```bash
url="https://example.com"
if curl -s --head "$url" | grep "200 OK" > /dev/null; then
    echo "Website is up"
else
    echo "Website is down"
fi
```

_Real-time use case:_ Basic uptime monitoring for websites.

***

**Q16.**\
**Scenario:** Write a script to extract the IP address of the current machine.\
**Answer:**

```bash
hostname -I | awk '{print $1}'
```

_Real-time use case:_ Automatically detecting the IP in cluster configurations.

***

**Q17.**\
**Scenario:** Automate the deletion of log files older than 7 days in `/var/logs`.\
**Answer:**

```bash
find /var/logs -type f -mtime +7 -exec rm {} \;
```

_Real-time use case:_ Preventing disk overflow in production.

***

**Q18.**\
**Scenario:** Print only unique lines from a file `data.txt`.\
**Answer:**

```bash
sort data.txt | uniq
```

_Real-time use case:_ Removing duplicates in configuration lists.

***

**Q19.**\
**Scenario:** Write a script to read usernames from `users.txt` and create them on the system.\
**Answer:**

```bash
while read user; do
    sudo useradd "$user"
done < users.txt
```

_Real-time use case:_ Bulk user creation for onboarding.

***

**Q20.**\
**Scenario:** Write a script to backup `/etc` directory every night via cron.\
**Answer:**

```bash
tar -czf /backup/etc_$(date +%F).tar.gz /etc
```

_Real-time use case:_ Config backup for disaster recovery.

***

### **3. Advanced DevOps Scenarios (21–30)**

**Q21.**\
**Scenario:** Script to check disk usage of `/` and alert if usage is more than 80%.\
**Answer:**

```bash
usage=$(df / | awk 'NR==2 {print $5}' | tr -d '%')
if [ "$usage" -gt 80 ]; then
    echo "Disk usage is ${usage}%" | mail -s "Disk Alert" admin@example.com
fi
```

_Real-time use case:_ Proactive disk monitoring.

***

**Q22.**\
**Scenario:** Script to tail logs and trigger an action if “OutOfMemoryError” is found.\
**Answer:**

```bash
tail -Fn0 /var/log/app.log | while read line; do
    echo "$line" | grep -q "OutOfMemoryError"
    if [ $? = 0 ]; then
        systemctl restart app
    fi
done
```

_Real-time use case:_ Auto-recovery from JVM crashes.

***

**Q23.**\
**Scenario:** Script to deploy code from Git to `/var/www/html`.\
**Answer:**

```bash
cd /var/www/html
git pull origin main
systemctl restart apache2
```

_Real-time use case:_ Simple CI/CD for web apps.

***

**Q24.**\
**Scenario:** Script to upload a file to AWS S3 bucket.\
**Answer:**

```bash
aws s3 cp /path/to/file s3://mybucket/
```

_Real-time use case:_ Automated backups to S3.

***

**Q25.**\
**Scenario:** Script to parse JSON using `jq`.\
**Answer:**

```bash
cat data.json | jq '.name'
```

_Real-time use case:_ Processing API responses in automation.

***

**Q26.**\
**Scenario:** Script to restart a Kubernetes pod if it’s in CrashLoopBackOff.\
**Answer:**

```bash
pod=$(kubectl get pods | grep CrashLoopBackOff | awk '{print $1}')
[ -n "$pod" ] && kubectl delete pod "$pod"
```

_Real-time use case:_ K8s health check automation.

***

**Q27.**\
**Scenario:** Monitor SSL certificate expiry for a domain.\
**Answer:**

```bash
expiry=$(echo | openssl s_client -connect example.com:443 2>/dev/null | openssl x509 -noout -dates | grep 'notAfter' | cut -d= -f2)
echo "Expiry date: $expiry"
```

_Real-time use case:_ Preventing SSL downtime.

***

**Q28.**\
**Scenario:** Script to watch a folder and trigger a build when files change.\
**Answer:**

```bash
inotifywait -m /path/to/code -e modify,create,delete |
while read path action file; do
    ./build.sh
done
```

_Real-time use case:_ Local CI pipeline.

***

**Q29.**\
**Scenario:** Script to read CPU load and store it every 5 minutes.\
**Answer:**

```bash
while true; do
    uptime >> /var/log/cpu_load.log
    sleep 300
done
```

_Real-time use case:_ Continuous performance monitoring.

***

**Q30.**\
**Scenario:** Script to check if a given TCP port is open.\
**Answer:**

```bash
host="example.com"
port=443
if nc -zv "$host" "$port" 2>/dev/null; then
    echo "Port $port is open"
else
    echo "Port $port is closed"
fi
```

_Real-time use case:_ Network troubleshooting for deployments.
