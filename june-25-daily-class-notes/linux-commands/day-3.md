# Day 3

#### 🔍 `grep` Command with Real-Time Use Cases

**54. `grep -i "devops" f1`**\
📘 _Case-insensitive search for "devops" in file `f1`_\
✅ **Use Case:** While reviewing log files, you're not sure if the word “DevOps” appears as “DevOps”, “devops”, or “DEVOPS”. This command helps find all such occurrences regardless of case.

***

**55. `grep -in "pattern" filename`**\
📘 _Search "pattern" with line number, ignore case_\
✅ **Use Case:** Searching for a specific function name in a case-insensitive way across a large script and needing the exact line numbers to modify them later.

***

**56. `grep -ic "pattern" filename`**\
📘 _Count how many times the pattern appears (case-insensitive)_\
✅ **Use Case:** You want to count how many times the word "error" appears in a production log to measure failure rates.

***

**57. `grep -iw "devops" f1`**\
📘 _Match only the exact word “devops”, case-insensitive_\
✅ **Use Case:** You're checking a documentation file to see if “devops” is mentioned as a standalone term (not as part of words like “devopsengineer”).

***

**58. `grep -li "devops" *`**\
📘 _List filenames (not content) where “devops” is found (case-insensitive)_\
✅ **Use Case:** You have multiple config files in a directory and want to know which files mention “devops” without printing the full match.

***

**59. `grep -ilr "devops" *`**\
📘 _Recursive search for files containing “devops”, print file names only_\
✅ **Use Case:** Searching a full codebase (subfolders too) to find all files that reference "devops" for documentation or audit purposes.

***

**60. `grep -ie "devops" -e "teams" f1`**\
📘 _Search for multiple patterns: “devops” or “teams”_\
✅ **Use Case:** While checking a resume file, you want to highlight if either “devops” or “teams” is mentioned to verify team-oriented DevOps experience.

***

**61. `grep -i "devops" file`**\
📘 _Case-insensitive search for “devops” in file_\
✅ **Use Case:** Similar to #54 — helpful when parsing logs or documentation files for “devops” usage regardless of case.

***

**62. `grep -in "devops" file`**\
📘 _Same as above, but includes line numbers_\
✅ **Use Case:** While editing a README, you want to quickly locate all lines that talk about DevOps to restructure or remove duplicates.

***

**63. `grep -ic "devops" file`**\
📘 _Count occurrences of “devops”, ignore case_\
✅ **Use Case:** Metrics collection — How many times “devops” is promoted or explained in a given tutorial file.

***

**64. `grep -iw "devops" file`**\
📘 _Matches exact word “devops”, case-insensitive_\
✅ **Use Case:** You want to exclude substrings like “devops123” or “fullstackdevops” and focus only on “devops” as a keyword.

***

**65. `grep -il "devops" *`**\
📘 _List files that contain the word “devops”, ignore case_\
✅ **Use Case:** Same as #58 but written again — maybe as a review pattern. Useful when hunting through scripts/configs.

***

**66. `grep -ilR "devops" *`**\
📘 _Recursive search, case-insensitive, list filenames_\
✅ **Use Case:** You're onboarding to a project and want to know which files in the repo have “devops” in any directory or subdirectory.

***

**67. `grep -ie "pattern1" -ie "pattern2" -ie "pattern3" file`**\
📘 _Search for multiple patterns in one go_\
✅ **Use Case:** Checking logs for multiple keywords like "ERROR", "FAIL", "CRITICAL" to quickly triage issues from one file.

***



**68. `chown user:group file`**\
📘 _Changes owner and group of a file_\
✅ **Use Case:** You deployed a file as `root` but it needs to be accessed by the `jenkins` user. Use `chown jenkins:jenkins config.xml`.

***

**69. `chown user filename`**\
📘 _Changes only the owner (not the group)_\
✅ **Use Case:** After creating a file using `sudo`, you want to hand over ownership to a developer account (e.g., `chown devuser report.txt`).

***

***

#### 🔧 `sed` – Stream Editor (for editing file contents)

**70. `sed`**\
📘 _Stream editor for parsing and transforming text_\
✅ **Use Case:** General-purpose tool. Used for automated text replacement, line deletion, or insertion in config files, logs, or scripts.

***

**71. `sed 's/pattern/change_pattern/g' filename`**\
📘 _Replaces all occurrences of `pattern` with `change_pattern` in each line_\
✅ **Use Case:** Replace all instances of `http` with `https` in a list of URLs stored in a file.

***

**72. `sed -i 's/pattern/change_pattern/g' filename`**\
📘 _Same as above, but edits the file in-place_\
✅ **Use Case:** Updating environment URLs in `.env` files before deployment from staging to production.

***

**73. `sed '2s/unix/linux/g' filename`**\
📘 _Replaces "unix" with "linux" only in line 2_\
✅ **Use Case:** You want to correct a known typo on a specific line (like in README or config instructions).

***

**74. `sed '5,$s/pattern/change_pattern/g' filename`**\
📘 _Replace pattern from line 5 to the end of the file_\
✅ **Use Case:** If logs have a header and the actual content starts from line 5, you may want to replace keywords only in the content part.

***

**75. `sed '2s/development/devops/2' f2`**\
📘 _On line 2, replace the second occurrence of "development" with "devops"_\
✅ **Use Case:** You're updating a templated email where the second mention of "development" needs to be tailored differently.

***

**76. `sed -n '4p' f2`**\
📘 _Prints only line 4 from file f2_\
✅ **Use Case:** Extracting specific metadata or a version number from a known fixed line in a config or script.

***

**77. `sed -n '144p' f2`**\
📘 _Prints only line 144 from f2_\
✅ **Use Case:** Log analysis – You were told error happened at line 144, and you want to read just that line.

***

**78. `sed '3d' f2`**\
📘 _Deletes line 3 from file f2_\
✅ **Use Case:** Removing an incorrect or unwanted configuration entry at a known position.

***

**79. `sed '30d' f2`**\
📘 _Deletes line 30 from file f2_\
✅ **Use Case:** Same as above, used when you need to clean up corrupted or deprecated entries from scripts or structured text.

***

**80. `sed -n -e '10p' -e '25p' filename`**\
📘 _Prints lines 10 and 25_\
✅ **Use Case:** Extracting log summaries or known headers from standard positions in monitoring logs.

***

***

#### 📄 `cp` – Copy Files

**81. `cp -i f2 f4`**\
📘 _Copy file f2 to f4 with prompt before overwrite_\
✅ **Use Case:** While backing up or overwriting a config file, the `-i` prevents accidental overwrites in production.

***

***

#### ✂️ `cut` – Extract fields from lines

**82. `cut -d " " -f4 checklist`**\
📘 _Extracts 4th field from each line using space as delimiter_\
✅ **Use Case:** From a list like `date status user command`, extract the `command` column from a process checklist.

***

**83. `cut -d " " -f1 checklist`**\
📘 _Extracts 1st field (e.g., date)_\
✅ **Use Case:** Getting only dates from a log file for plotting or auditing.

***

**84. `cut -d " " -f2,4 checklist`**\
📘 _Extracts 2nd and 4th fields_\
✅ **Use Case:** Get `status` and `command` columns to identify failed operations in logs.

***

***

#### 🧮 `awk` – Pattern scanning and processing

**85. `awk -F " " '{print $2}' checklist`**\
📘 _Prints second column of each line (space-delimited)_\
✅ **Use Case:** Extracting usernames or status codes from a structured checklist/log.

***

**86. `awk -F " " '{print $4}' checklist`**\
📘 _Prints fourth column_\
✅ **Use Case:** Fetching commands or services executed for analysis.

***

**87. `awk -F " " '{print $2,$4}' checklist`**\
📘 _Prints 2nd and 4th columns_\
✅ **Use Case:** Listing username and command to correlate who executed what action in a system audit.

***

***

#### ⚙️ `ps` and `kill` – Process Management

**88. `ps -ef | grep -i "service_name"`**\
📘 _Search for process by name (case-insensitive)_\
✅ **Use Case:** You want to check if a `tomcat` or `jenkins` process is running and see its PID and user.

***

**89. `ps -ef | grep -ie "jenkins" -e "tomcat"`**\
📘 _Search for either “jenkins” or “tomcat” processes_\
✅ **Use Case:** In a CI/CD setup, quickly verify if both Jenkins and Tomcat services are up and running.

***

**90. `ps -u "user_name"`**\
📘 _Lists all processes owned by a specific user_\
✅ **Use Case:** See what all background jobs are running under the `deploy` user to track app-related processes.

***

**91. `kill -9 "PID"`**\
📘 _Forcefully terminates a process with a specific PID_\
✅ **Use Case:** A deployment script hangs due to a zombie process. Use this to force kill the stuck process.

***
