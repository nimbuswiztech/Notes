# Shell Scripting & Bash Commands

### Real-Time Scenario-Based Interview Questions (Shell Scripting & Bash Commands)

### 1. **Scenario:**

You need to monitor disk space usage on a Linux server and send an email alert if disk usage exceeds 85%.\
**Question:**\
Write a shell script to automate this.

**Answer:**

```
#!/bin/bash
threshold=85
usage=$(df -h / | grep '/' | awk '{print $5}' | sed 's/%//')
if [ "$usage" -ge "$threshold" ]; then
  echo "Disk space on $(hostname) is above 85% ($usage%)." | mail -s "Disk Space Alert" user@example.com
fi
```

* _Explanation:_ Checks disk usage, and if above threshold, sends an email.

### 2. **Scenario:**

You have a log file growing in size every minute.\
**Question:**\
How would you only display new lines as they are appended (like real-time monitoring)?

**Answer:**

* Use: `tail -f /path/to/logfile.log`
* _Explanation:_ The `-f` flag follows the log and updates output as lines are added.

### 3. **Scenario:**

A file has thousands of records; you want to extract lines 101 to 200.\
**Question:**\
How would you do this using Bash?

**Answer:**

* Use: `sed -n '101,200p' filename.txt`
* _Explanation:_ Prints only lines 101 to 200.

### 4. **Scenario:**

You want to rename all `.txt` files in a directory by appending the date.\
**Question:**\
Provide a Bash command/script for this.

**Answer:**

```
for file in *.txt; do
  mv "$file" "${file%.txt}_$(date +%Y%m%d).txt"
done
```

* _Explanation:_ Loops through `.txt` files, renames with date appended.

### 5. **Scenario:**

You need to find duplicate lines in a large file.\
**Question:**\
Which Bash command helps here?

**Answer:**

* Use: `sort filename | uniq -d`
* _Explanation:_ Lists lines occurring more than once.

### 6. **Scenario:**

You want to schedule a backup script to run at 3:30AM daily.\
**Question:**\
How do you set up a cron job for this?

**Answer:**

* Command:\
  `30 3 * * * /path/to/backup.sh`
* _Explanation:_ Adds to the crontab to run script at the specified time.

### 7. **Scenario:**

You need to replace all occurrences of "dev" with "production" in a file.\
**Question:**\
Which command would you use?

**Answer:**

* Use: `sed -i 's/dev/production/g' filename`
* _Explanation:_ In-place replacement of all matching words.

### 8. **Scenario:**

Extract email addresses from a file using shell tools.\
**Question:**\
Provide a command line for this task.

**Answer:**

* Use: `grep -E -o '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' filename`
* _Explanation:_ Finds all email-like patterns using regex.

### 9. **Scenario:**

List running processes that contain "java".\
**Question:**\
What command do you use?

**Answer:**

* Use: `ps aux | grep java | grep -v grep`
* _Explanation:_ Shows only process lines including "java".

### 10. **Scenario:**

You have a directory with 1 million files.\
**Question:**\
How to efficiently count the number of files?

**Answer:**

* Use: `find . -type f | wc -l`
* _Explanation:_ Counts all files recursively.

### 11. **Scenario:**

Write a script to check if a service (like nginx) is running, and restart it if it isn’t.\
**Question:**\
How can you automate this?

**Answer:**

```
#!/bin/bash
if ! systemctl is-active --quiet nginx; then
  systemctl restart nginx
fi
```

* _Explanation:_ Checks if nginx is active, restarts if not.

### 12. **Scenario:**

Extract the IP address assigned to `eth0` using shell commands.\
**Question:**\
How will you do this?

**Answer:**

* Use: `ip addr show eth0 | grep 'inet ' | awk '{print $2}' | cut -d/ -f1`
* _Explanation:_ Extracts the IP address portion for eth0.

### 13. **Scenario:**

A task requires creating a compressed archive every Monday at midnight.\
**Question:**\
Script/cron combination to achieve this?

**Answer:**

*   Script `backup.sh`:

    ```
    tar -czf /backup/data_$(date +%Y%m%d).tar.gz /data
    ```
* Cron job:\
  `0 0 * * 1 /path/to/backup.sh`
* _Explanation:_ Cron runs script weekly at midnight.

### 14. **Scenario:**

You want to know total memory used by all "python" processes.\
**Question:**\
Give the Bash one-liner.

**Answer:**

* Use: `ps -C python -o rss= | awk '{sum+=$1} END {print sum/1024 " MB"}'`
* _Explanation:_ Summarizes memory usage (RSS in KB, converted to MB).

### 15. **Scenario:**

You need a menu-driven shell script to start/stop/status/restart a service.\
**Question:**\
Provide an outline.

**Answer:**

```
echo "1. Start 2. Stop 3. Status 4. Restart"
read -p "Choice: " choice
case $choice in
 1) systemctl start nginx ;;
 2) systemctl stop nginx ;;
 3) systemctl status nginx ;;
 4) systemctl restart nginx ;;
 *) echo "Invalid" ;;
esac
```

* _Explanation:_ Basic menu structure using `case`.

### 16. **Scenario:**

How to extract the 2nd column from a space-delimited file?\
**Question:**\
Give the command.

**Answer:**

* Use: `awk '{print $2}' filename`
* _Explanation:_ Prints only the 2nd column.

### 17. **Scenario:**

You are handed a `.tar.gz` file.\
**Question:**\
How do you extract it?

**Answer:**

* Use: `tar -xzf filename.tar.gz`
* _Explanation:_ Decompresses and extracts files.

### 18. **Scenario:**

How do you check if a directory exists via shell script?

**Answer:**

```
if [ -d /path/to/dir ]; then
  echo "Exists"
else
  echo "Does not exist"
fi
```

* _Explanation:_ The `-d` test checks for directories.

### 19. **Scenario:**

How to reverse the order of lines in a file?\
**Question:**\
Share the command.

**Answer:**

* Use: `tac filename`
* _Explanation:_ `tac` prints lines in reverse order.

### 20. **Scenario:**

Write a script to ping a host every 5 seconds and log the date/time and result.

**Answer:**

```
#!/bin/bash
while true; do
  dt=$(date '+%Y-%m-%d %H:%M:%S')
  if ping -c 1 google.com &>/dev/null; then
    echo "$dt SUCCESS" >> ping.log
  else
    echo "$dt FAILURE" >> ping.log
  fi
  sleep 5
done
```

* _Explanation:_ Infinite loop pings host, writes result to a log file every 5 seconds.
