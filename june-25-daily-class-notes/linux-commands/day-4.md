# Day 4

#### **xargs & find Command**

**`xargs`**

* **Purpose**: Converts input from `stdin` into arguments for a command.
* **Use Case**: Works well with `find` and `grep` when the result is a long list.

**`find . -type f -mtime +2 | xargs rm`**

* **Finds**: Files modified more than 2 days ago.
* **Action**: Deletes them using `xargs rm`.
* **Real-Time Use**: Clean up old logs: `find /var/logs -type f -mtime +7 | xargs rm`

**`find . -type f -mtime +365 | xargs rm`**

* **Finds**: Files older than 365 days.
* **Action**: Deletes them.
* **Use Case**: Archiving or cleaning up year-old files.

**`find . -type f -iname "test" -maxdepth 1`**

* **Finds**: Files named "test" (case-insensitive) in the current directory only.

**`find . -type d -iname "test" -maxdepth 2`**

* **Finds**: Directories named "test" within current directory and its immediate subdirectories.

**`find . -iname "test" -maxdepth 1`**

* **Finds**: Files or directories named "test" in the current directory (case-insensitive).

***

#### 🔗 **Links: Soft and Hard**

**`ln -s test1 softlink`**

* **Creates**: A symbolic (soft) link named `softlink` pointing to `test1`.

**`ln -s /home/test/test1 softlink`**

* **Creates**: A softlink to a full path, works across partitions.

**`ln /home/test/test1 hardlink`**

* **Creates**: A hardlink. Both `test1` and `hardlink` share same inode (actual data).
* **Note**: Cannot hardlink across different filesystems.

***

#### 🖥️ **System Info and User Commands**

**`uname -a`**

* **Shows**: Kernel name, version, system name, architecture, etc.

**`cat /etc/os-release`**

* **Shows**: OS details (like Ubuntu 20.04, Amazon Linux, etc).

**`hostname`**

* **Shows**: Current system's hostname.

**`who | wc -l`**

* **Counts**: Number of logged-in users.

**`whoami`**

* **Shows**: Currently logged-in user.

***

#### 🛠️ **Background Jobs & Process Monitoring**

**`nohup ./script.sh &`**

* **Runs**: `script.sh` in background, immune to hangups.
* **Output**: Goes to `nohup.out` by default.

**`top`**

* **Interactive**: Shows CPU, memory usage, process list in real-time.

***

#### 🔐 **SSH & File Transfers**

**`ssh username@server`**

* **Connects**: To remote server using username.

**`ssh -i demo.pem user@server`**

* **Connects**: Using key file (EC2 / Linux remote access).

**`ssh -i demo.pem ubuntu@54.26.118.12`**

* **Example**: Connect to a specific IP using key-based auth.

**`scp file_name user@server:/tmp`**

* **Copies**: File to `/tmp` on remote server.

**`scp file_name user@server:/home/ubuntu`**

* **Copies**: File to user's home.

**`scp -r directory user@server:/path`**

* **Copies**: Directory recursively to remote server.

**`rsync file user@server:/home/temp`**

* **Efficient Transfer**: Synchronizes file to remote, supports resume, delta transfer.

***

#### 🌐 **Network Commands**

**`ping ip_address / ping hostname`**

* **Checks**: Network reachability.

**`telnet ip / dns_name`**

* **Checks**: TCP connectivity to a host on a given port.

**`netstat -na | grep 8080`**

* **Checks**: If any process is listening or connected on port 8080.

***

#### 📊 **Disk, Memory & File Utilities**

**`df -h .`**

* **Shows**: Disk space usage of current directory's partition in human-readable form.

**`free -g` / `free -m`**

* **Shows**: Memory usage in GB/MB.

**`uniq file`**

* **Removes**: Duplicate adjacent lines from `file`.

**`sort file`**

* **Sorts**: Lines of the file alphabetically.

**`sort file | uniq`**

* **Removes**: Duplicate lines from unsorted file (first sorts, then removes dups).

**`sort -r file | uniq`**

* **Reverse sort** and then remove duplicates.

***

#### 🔒 **File & Directory Permissions (chmod)**

| **Command**                   | **Meaning**                                 |
| ----------------------------- | ------------------------------------------- |
| `chmod 777 filename`          | Full permission to everyone                 |
| `chmod 777 directory`         | Full permission to all on directory         |
| `chmod 644 dir1 dir2 dir3`    | Owner: rw-, Others: r--                     |
| `chmod 755 file1 file2 file3` | Owner: rwx, Others: r-x                     |
| `chmod -R 777 folder1`        | Recursively set 777 for folder and contents |
| `chmod a+w file`              | Add write to all (user, group, others)      |
| `chmod a+rwx folder`          | Give full access to all                     |
| `chmod u+rw file`             | Add read/write to owner                     |
| `chmod g+w directory`         | Add write to group                          |
| `chmod o+r file`              | Others can read file                        |

***

#### 🎯 **Permission Bits Reference**

| rwx Combination | Numeric |
| --------------- | ------- |
| rwx             | 7       |
| rw-             | 6       |
| r--             | 4       |
| r-x             | 5       |
| --x             | 1       |
| ---             | 0       |

E.g. `chmod 754` = rwx (7) r-x (5) r-- (4)

***

#### 🧩 **umask – Default File Permissions**

* **Formula**:\
  `Final Permission = Default Permission - umask`

| umask | Resulting Default Permission |
| ----- | ---------------------------- |
| `000` | `rwx rwx rwx` (full access)  |
| `777` | `--- --- ---` (no access)    |
| `641` | `--x -wx rw-`                |

***

#### 🗑️ **Remove/Delete Files & Folders**

| Command                  | Description                               |
| ------------------------ | ----------------------------------------- |
| `rm filename`            | Delete a file                             |
| `rm -rf directory_name`  | Force delete a directory and contents     |
| `rm f1 f2 f3`            | Delete multiple files                     |
| `rm -rf folder1 folder2` | Delete multiple folders recursively       |
| `rm directory/dir1/file` | Delete a specific file inside nested dirs |
| `rm -rf /dir1/dir2/dir3` | Delete full path recursively              |

#### 🔐 **103. `chown new_name file`**

* **Changes ownership** of a file to the specified `new_name` (user).
* ✅ **Example**: `chown ubuntu report.txt`
* 🔧 **Use Case**: When you copy files as `root` and want to hand over ownership to another user.

***

#### 🔐 **104. `chown new_name:new_grp file`**

* **Changes** both **owner and group** of the file.
* ✅ **Example**: `chown ubuntu:devops report.txt`
* 🔧 **Use Case**: Change owner and group in shared environments.

***

#### 🧠 **105. `sed` – Stream Editor**

* **Text processing tool** to search, replace, delete, insert lines from a file or stream.
* ⚡ **Use Case**: Mass editing, automation scripts, or CI/CD transformations.

***

#### 🔄 **106. `sed 's/old_pattern/new_pattern/g' filename`**

* **Replaces** all instances of `old_pattern` with `new_pattern` in each line.
* ✅ **Example**: `sed 's/error/ok/g' logs.txt`

***

#### 🔄 **107. `sed 's/samsung/sony/g' newfile`**

* **Replaces** all occurrences of “samsung” with “sony” in `newfile`.

***

#### 📝 **108. `sed -i 's/Testing/development/g' file2`**

* `-i`: **Edit file in-place** (no output to stdout).
* ✅ Replaces **all "Testing" → "development"** in the actual file.
* ⚠️ Useful for config changes in pipelines or scripts.

***

#### 🔡 **109. `sed 's/testing/development/gi' file2`**

* `g`: Replace all matches in line
* `i`: Case-insensitive
* ✅ Replaces "testing", "TESTING", etc., with "development".

***

#### 🔢 **110. `sed '2s/testing/development/gi' file2`**

* Only replaces text on **line 2**.
* ✅ Useful when editing a specific line in a file.

***

#### 🔢 **111. `sed '10,30s/testing/development/gi' file2`**

* Replaces **only in lines 10 through 30**.

***

#### 🔚 **112. `sed '30,$s/testing/development/gi' file2`**

* Replace from **line 30 to the last line** (`$`).
* ✅ For changing all lines below a certain point.

***

#### 📍 **113. `sed -n '98p' file`**

* `-n`: Suppress default output
* `'98p'`: **Print only line 98**
* ✅ For extracting a specific line from a large file.

***

#### 📍 **114. `sed -n '2p' file2`**

* Prints **line 2 only** from file2.

***

#### 📍 **115. `sed -n '10,20p' file`**

* Prints lines from **10 to 20**.
* ✅ Like `tail | head`, but more efficient.

***

#### ❌ **116. `sed '2d' file2`**

* **Deletes** line 2 from `file2`.
* Output printed to stdout by default.
* ✅ For ignoring header lines, specific rows, etc.

***

#### ❌ **117. `sed '100d' file`**

* **Deletes** line 100 from `file`.

***

#### ✅ Bonus Tip: Save Changes In-Place

To make `sed` delete or replace lines **in the file**, add `-i` like:

```bash
bashCopyEditsed -i '2d' file
```

#### 📁 **File Movement Commands – `mv`**

**118. `mv old_name New_name`**

* **Renames** a file or directory.
* ✅ Example: `mv report.txt report_backup.txt`
* 🔧 Use Case: Rename a file after processing.

**119. `mv file dir1/file`**

* **Moves file** to another directory with the same or new name.
* ✅ Example: `mv app.log /var/logs/app.log`

**120. `mv dir2/f1 .`**

* **Moves `f1` from `dir2` to current directory**.

***

#### 📄 **File Copy Commands – `cp`**

**121. `cp file file2`**

* **Copies** `file` to `file2`.
* ✅ Use Case: Backup original file before changes.

**122. `cp file dir1/dir2/file`**

* Copies file to a nested directory.
* ⚠️ All intermediate directories must exist.

**123. `cp -i file file2`**

* **Interactive copy**. Prompts before overwrite.
* ✅ Good for preventing accidental overwrites.

**124. `cp -i test dir1/test`**

* Same as above, copying file `test` to directory with prompt if it already exists.

**125. `cp -r dir2 dir3`**

* **Recursive copy**: Copies directory and its contents.
* ✅ Example: `cp -r /etc/nginx /tmp/backup_nginx`

**126. `cp -r dir1 dir2/dir3/dir4`**

* Copies entire `dir1` into a deep directory path.

**127. `cp -i dir1/file dir2/dir3/dir4/`**

* Copies file with overwrite prompt to deeply nested location.

***

#### 📂 **List Matching Files – `ls` with Wildcards**

**128. `ls Devops*`**

* Lists files/directories **starting with "Devops"**.

**129. `ls test*`**

* Lists files/directories **starting with "test"**.

**130. `ls *`**

* Lists **all** files and directories in the current path.

***

#### 🔍 **Search Within Files – `grep` with Regex Patterns**

**131. `grep -i "^Devops" file_name`**

* Finds lines starting with `Devops` (case-insensitive) in `file_name`.
* `^` = beginning of line.

**132. `grep -i "^t" file_name`**

* Matches lines starting with `t` or `T`.

**133. `grep -i "dev$" file_name`**

* Finds lines **ending** with `dev` (case-insensitive).
* `$` = end of line.
