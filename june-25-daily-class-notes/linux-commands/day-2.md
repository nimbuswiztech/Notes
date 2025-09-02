---
description: Linux commnds
---

# Day 2

#### **18. `cat filename`**

* Displays the content of a file on the terminal.
* Example: `cat notes.txt` shows all text in `notes.txt`.

***

#### **19. `:/pattern`**

* Used in **vi/vim** editor to search for a `pattern`.
* Example: `:/error` will search for the first occurrence of "error".

***

#### **20. `:%s/Testing/development/g`**

* In **vi/vim**, substitutes **all** occurrences of `Testing` with `development` **in the entire file**.
* `%` refers to all lines, `g` is for global (all matches per line).

***

#### **21. `:1 s/Testing/dev/g`**

* In **vi/vim**, replaces all instances of `Testing` with `dev` in **line 1** only.

***

#### **22. `:5,$ s/Testing/dev/g`**

* In **vi/vim**, replaces `Testing` with `dev` from **line 5 to the end of file**.

***

#### **23. `1,5 s/Testing/dev/g`**

* In **vi/vim**, replaces `Testing` with `dev` in **lines 1 to 5**.

***

#### **24. `mkdir -p dir1/dir2/dir3`**

* Creates nested directories at once.
* `-p` ensures parent directories are created if they don’t exist.

***

#### **25. `wc filename`**

* Stands for "word count".
* Shows number of lines, words, and bytes in the file.

***

#### **26. `wc -l f1`**

* Counts the number of **lines** in file `f1`.

***

#### **27. `wc -w f1`**

* Counts the number of **words** in file `f1`.

***

#### **28. `wc -c f1`**

* Counts the number of **bytes** in file `f1`.

***

#### **29. `wc -m f1`**

* Counts the number of **characters** in file `f1`.

***

#### **30. `echo "statement"`**

* Prints the string **statement** to terminal.
* Example: `echo "Hello DevOps"` → prints `Hello DevOps`.

***

#### **31. `echo -e "Hi \nThis is devops class \nThird line"`**

* Prints the string with **interpreted escape characters** like  for newline.

***

#### **32. `>` \[Redirect]**

* Redirects output to a file (overwrites existing content).
* Example: `echo Hello > file.txt`

***

#### **33. `>>` \[Append]**

* Appends output to a file without overwriting.
* Example: `echo World >> file.txt`

***

#### **34. `du filename`**

* Shows disk usage of a file in blocks.

***

#### **35. `du -sh filename`**

* `-s`: summarize, `-h`: human-readable (KB/MB/GB).
* Shows readable size of file.

***

#### **36. `du -sh directory_name`**

* Shows human-readable size of a directory.

***

#### **37. `du -sh *`**

* Displays sizes of **all files and directories** in current directory.

***

#### **38. `head filename`**

* Displays the **first 10 lines** of a file by default.

***

#### **39. `head -3 f1`**

* Displays **first 3 lines** of `f1`.

***

#### **40. `head -20 f1`**

* Displays **first 20 lines** of `f1`.

***

#### **41. `tail f1`**

* Displays the **last 10 lines** of `f1`.

***

#### **42. `tail -2 f1`**

* Displays the **last 2 lines** of `f1`.

***

#### **43. `tail -20 f1`**

* Displays the **last 20 lines** of `f1`.

***

#### **44. `|` (pipe)**

* Passes output of one command as input to another.
* Example: `ls -l | wc -l`

***

#### **45. `ls -l | wc -l`**

* Lists files and **counts** how many (like a file count in long listing).

***

#### **46. `head -10 file | tail -1`**

* Shows the **10th line** of a file.
  * First 10 lines → pipe to tail to show the last (i.e., 10th).

***

#### **47. `grep`**

* Searches for a pattern in a file.

***

#### **48. `grep -i "devops" f1`**

* Case-insensitive search for "devops" in `f1`.

***

#### **49. `grep -in "pattern" filename`**

* `-i`: case-insensitive, `-n`: show line numbers of matches.

***

#### **50. `grep -ic "pattern" filename`**

* `-i`: case-insensitive, `-c`: count of matching lines.

***

#### **51. `grep -iw "devops" f1`**

* Searches for **exact word** match, ignoring case.

***

#### **52. `grep -li "devops" *`**

* Lists all filenames in current directory where "devops" is found (case-insensitive).

***

#### **53. `grep -ilr "devops" *`**

* Recursively (`-r`) and case-insensitively searches and lists files with match for "devops".

***

#### **54. `grep -ie "devops" -e "teams" f1`**

* Searches for **multiple patterns** ("devops" or "teams") in `f1`.

***

#### **55. `head -8 f1 | tail -1 | wc -w`**

* Gets the **8th line** of `f1` and **counts the number of words** in it.

***

#### **56. `df -h`**

* Shows disk space usage in **human-readable** format.

***

#### **57. `free -g`**

* Shows memory usage in **gigabytes**.
