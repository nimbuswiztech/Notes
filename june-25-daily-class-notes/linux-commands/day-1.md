---
description: Basic Linux commands
---

# Day 1

#### Navigation & Directory Management

1. **`pwd`** – Prints the current working directory.\
   &#xNAN;_&#x55;seful to confirm the current path, especially in scripts or nested environments._
2. **`ls`** – Lists files/directories in the current directory.
3. **`ls -l`** – Lists files with detailed information (permissions, owner, size, etc.).
4. **`ls -lt`** – Lists files sorted by modification time (latest first).
5. **`ls -lrt`** – Lists files sorted by modification time in reverse (oldest first).\
   &#xNAN;_&#x43;ommonly used to see the oldest logs or files in `/var/log` or build artifacts._
6. **`cd ..`** – Move up one directory level.
7. **`cd ../..`** – Move up two directory levels.
8. **`cd /home/user/path`** – Go to a specific absolute path.

***

#### Creating Files & Directories

9. **`mkdir <directory_name>`** – Create a single directory.
10. **`mkdir dir1 dir2 dir3`** – Create multiple directories at once.
11. **`mkdir -p d3/dir1/dir2`** – Create nested directories in one go.\
    &#xNAN;_&#x46;requently used in scripts to ensure directory structure exists._
12. **`touch <file_name>`** – Create an empty file or update the timestamp.
13. **`touch f1 f2 f3`** – Create multiple files at once.
14. **`touch directory_name/filename`** – Create a file inside a specific directory.
15. **(Duplicate of 11)** – Still useful for emphasis on nested creation.

***

#### Editing & Viewing Files

16. **`vi file_name`** – Open file in `vi` editor.

* Press `i` to enter **insert** mode.
* Press `Esc :wq!` to **save and exit**.
* Press `Esc :q!` to **exit without saving**.\
  &#xNAN;_&#x49;deal when editing config files like `nginx.conf`, `.env`, `crontab`, etc._

17. **`cat filename`** – View the contents of a file.\
    &#xNAN;_&#x51;uick way to read logs, YAML files, shell scripts, or Jenkinsfile content._
