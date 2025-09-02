# Linux Interview Questions

_In some questions I am encouraging to search online because it will help you practice for searching more complicated questions in the future_&#x20;

1\. When you login you get “$” prompt, what is the prompt for root?&#x20;

\#

2\. Explain the difference between grep and egrep?&#x20;

Search online&#x20;

3\. What is the port # for DNS, NTP and NFS?&#x20;

53,123 and 111/2049&#x20;

4\. What is the configuration file name of DNS and where is it located?&#x20;

/etc/named.conf&#x20;

5\. How many new directories will be created after running the following command

mkdir {a..c}{1..3}&#x20;

9&#x20;

6\. Your PC is configured with a DNS server address but not the default gateway. Can the PC access internet?

No&#x20;

7\. What is the difference between IP and Gateway?&#x20;

Search online&#x20;

8\. Can you assign one static IP to 2 computers, if not then why?&#x20;

No because it will create IP conflict&#x20;

9\. How to change IPs address to static?&#x20;

ifconfig x.x.x.x&#x20;

10\. You are trying to ping a server by hostname and you get an error message, “ping: unknown host …”. What could be the reason and how to solve the problem so you can ping it by hostname?&#x20;

Check for /etc/hosts or DNS to see if it has hostname to IP entry&#x20;

11\. Explain the difference between relative and absolute path?&#x20;



Absolute path starts from / where relative path is your current directory&#x20;

12\. List 3 different methods of adding user?&#x20;



Search online&#x20;

13\. What is the command to change file/directory ownership and group?&#x20;

chown and chgrp&#x20;

14\. List any 3 type of filesystem?&#x20;

\


ext4,NTFS and FAT&#x20;

\


15\. When you login you get a message on the screen. What is the name of that file and where is it located?&#x20;

\


/etc/motd&#x20;

16\. What is /bin directory used for?&#x20;

\


Search online&#x20;

17\. What are the different types of DNS Server&#x20;

\


Master and secondary&#x20;

18\. How to change a user password?&#x20;

\


passwd username&#x20;

19\. What is the version of Redhat Linux you have experience with?&#x20;

\


7.4&#x20;

20\. List any 4 linux distributions?&#x20;

\


Redhat, CentOS, Ubuntu and SUSE&#x20;

21\. How to logoff from the system?&#x20;

\


exit&#x20;

22\. Give any 3 examples of operating systems?&#x20;

\


Windows, Linux and MAC&#x20;

23\. How to create a directory?&#x20;

\


mkdir&#x20;

24\. Where are the zone files located for DNS service?&#x20;

\


/var/named/zonefiles&#x20;

25\. How to check kernel version?&#x20;

\


uname –a&#x20;

26\. Which directory has all the configuration files?&#x20;

\


/etc&#x20;

27\. How to become root user from a regular user?&#x20;

\


su –&#x20;

28\. How many mega bytes in 1 giga bytes?&#x20;

\


Search online&#x20;

29\. What is the purpose of having different network ports?&#x20;

\


So the communication of each application goes through a dedicated port&#x20;

30\. How to display first column of a file?&#x20;

\


cat filename | awk ‘{print $1}’&#x20;

31\. What is the name of DNS rpm package?&#x20;

\


bind&#x20;

32\. What is the difference between nslookup and dig commands?&#x20;

\


Search online&#x20;

33\. How to check your user id and group id?&#x20;

\


id&#x20;

34\. How to check a file’s permission?&#x20;

\


ls –l&#x20;

35\. What is the difference between “kill” and “kill -9” command?&#x20;

\


Search online&#x20;

36\. What is subnet?&#x20;

\


Search online&#x20;

37\. You are troubleshooting an issue with Redhat support and they have asked you to send the contents of /etc directory. How and which method you will use to transfer the contents?&#x20;

\


tar (compress) the entire /etc directory and ftp&#x20;

38\. What is root home directory?&#x20;

\


/root&#x20;

39\. What is rsyslogd deamon and its purpose?&#x20;

\


Search online&#x20;

40\. Your company has terminated a server administrator. What is first thing as an administrator you should do to enhance the security?&#x20;

\


Change root password&#x20;

41\. How to check the computer name or host name in Linux?&#x20;

\


hostname&#x20;

42\. Which permission allows a user to run an executable with the permissions of the owner of that file?&#x20;

\


First 3 bits should have x&#x20;

43\. What is the command to untar a tarred file?&#x20;

\


untar&#x20;

44\. What is /proc directory used for?&#x20;

\


Search online&#x20;

45\. What is the purpose of nsswitch.conf file&#x20;

\


It tells the system where to go to resolve hostnames&#x20;

46\. List 3 basic commands to navigate the filesystem?&#x20;

\


cd, pwd and ls&#x20;

47\. Which service/daemon should be running on the server that allows you to connect remotely?&#x20;

\


sshd&#x20;

48\. What is the purpose of firewall?&#x20;

\


Search online&#x20;

49\. List any 3 IT components?&#x20;

\


Hardware, OS and Applications&#x20;

50\. Which directory has all the commands we use, e.g. ls, cd etc.?&#x20;

\


/usr/bin or /bin&#x20;

\


51\. What is the difference between memory, virtual memory and cache?&#x20;

\


Search online&#x20;

52\. Which of the following is correct?&#x20;

a. Hardware  Operating System  Users&#x20;

b. Operating System  Users  Hardware&#x20;

c. Database  Hardware  Users&#x20;

\


53\. Which of the following is a communication command?&#x20;

o grep&#x20;

o mail&#x20;

o touch&#x20;

o cd&#x20;

\


54\. How to rename a file or directory?&#x20;



mv&#x20;

55\. How to change a hostname in Linux?&#x20;

\


Search online&#x20;

56\. How to check network interfaces in Linux?&#x20;

\


ifconfig&#x20;

57\. Why is “tail –f logfilename” command used most often and what does it do?&#x20;

\


It will output all incoming logs in real time&#x20;

58\. What type of hardware have you worked on?&#x20;

\


You should get yourself familiar with Dell, HP and UCS hardware by going online and check the vendor websites&#x20;

59\. How to sort a file in reverse order?&#x20;

\


cat filename | sort –r&#x20;

60\. What is the name of operating system that runs Unix?&#x20;

\


Solaris, HP-UX etc.&#x20;

61\. List all byte sizes from smallest to largest?&#x20;

\


Search online&#x20;

62\. How to check the total number of partition in Linux?&#x20;

\


fdisk -l&#x20;

63\. How to access a linux system from a linux system?&#x20;

\


ssh&#x20;

64\. Explain the procedure of bonding 2 NICs or interfaces together?&#x20;

\


Search online&#x20;

65\. What is the exact command syntax to list the 5th column of a file and cut the first 3 letters?&#x20;

\


cat filename | awk ‘{print $5}’ | cut –c1-3&#x20;

66\. What is /etc/hosts file used for?&#x20;

\


To resolve hostnames with IP address&#x20;

\


67\. List any 3 options of ‘df’ command and what they are used for?&#x20;

\


Search online&#x20;

68\. What is the command to change file/directory permissions?&#x20;

\


chmod&#x20;

69\. What is the purpose of pipe (|)?&#x20;

\


To combine multiple commands&#x20;

70\. What is /etc directory used for?&#x20;

\


For configuration files&#x20;

71\. Which command is used to list files in a directory?&#x20;

\


ls –l&#x20;

72\. There is a command which gives you information about other commands, please explain that command and what is it used for?&#x20;

\


man&#x20;

73\. How to delete a file and a directory?&#x20;

\


rm filename and rmdir dirname&#x20;

74\. What is the difference between “tail” and “tail -10”?&#x20;

\


None&#x20;

75\. List 4 commands to display or read a file contents?&#x20;

\


cat, more, less, vi&#x20;

76\. Which command is used to read the top 5 lines of a file?&#x20;

\


head -5 filename&#x20;

77\. What are the different commands or methods to write to a file?&#x20;

\


echo > filename and vi filename&#x20;

78\. What is swap space and how to check swap space?&#x20;

\


Search online&#x20;

79\. What is inode and how to find an inode of a file?&#x20;

\


Search online&#x20;

80\. Which file to edit for kernel tuning?&#x20;

\


Search online&#x20;

81\. What is the latest version of Redhat?&#x20;

\


Search online&#x20;

82\. Name the command to find specific word from a file?&#x20;

\


grep word filename&#x20;

83\. You have scheduled a job using crontab but it does not run at the time you specified, what could be the reason and how would you troubleshoot?&#x20;

\


Check your system time&#x20;

Check your crontab entry&#x20;

Check /var/log/messages&#x20;

84\. How to check system hardware information?&#x20;

\


dmidecode&#x20;

85\. How to check network interface MAC address?&#x20;

\


ifconfig&#x20;

86\. If I don’t want others to read my file1, how to do that?&#x20;

\


Remove r from the last 3 bits of file permission&#x20;

87\. What is the purpose of “uniq” and “sed” command?&#x20;

\


Search online&#x20;

88\. Which command is used to list the contents of a directory in the most recent time and in reverse order, meaning the most updated file should be listed on the bottom?&#x20;

\


ls –ltr&#x20;

89\. What is the difference between tar, gzip and gunzip?&#x20;

\


Search online&#x20;

90\. What are the different ways to install and OS?&#x20;

\


DVD, DVD iso and network boot&#x20;

91\. How to view difference between two files?&#x20;

\


diff file1 and file2&#x20;

92\. You noticed that one of the Linux servers has no disk space left, how would you troubleshoot that issue?&#x20;

\


If running LVM then add more disk and extend LVM&#x20;

If not running LVM then add more disk, create a new partition and link the new partition to an existing filesystem&#x20;

93\. How to check Redhat version release?&#x20;

\


uname –a or /etc/redhat-release&#x20;

94\. What is the difference between TCP and UDP?&#x20;

\


Search online&#x20;

95\. What is a zombie process?&#x20;

\


Search online&#x20;

96\. How do you search for a pattern/word in a file and then replace it in an entire file?&#x20;

\


sed command&#x20;

97\. Explain the purpose of “touch” command?&#x20;

\


To create an empty file&#x20;

98\. If a command hangs, how to stop it and get the prompt back?&#x20;

\


Ctrl C&#x20;

99\. Which command is used to count words or lines?&#x20;

\


wc&#x20;

100\. How to check the number of users logged in?&#x20;

\


who&#x20;

101\. What is the command to view the calendar of 2011?&#x20;

\


cal 2011&#x20;

102\. Which command is used to view disk space?&#x20;

\


df –h&#x20;

103\. How to create a new group in Linux?&#x20;

\


groupadd&#x20;

104\. What is the command to send a message to everyone who is logged into the system?&#x20;

\


wall&#x20;

105\. Which command is used to check total number of disks?&#x20;

\


fdisk –l&#x20;

106\. What is an mail server record in DNS?&#x20;

\


MX&#x20;

107\. What does the following command line do?&#x20;

\


ps -ef | awk '{print $1}' | sort | uniq&#x20;

List the first column of all running processes, sort them and remove duplicates&#x20;

108\. You get a call that when a user goes to www.yourwebsite.com it fails and gets an error, how do you troubleshoot?&#x20;

\


Check for user internet&#x20;

Check to see if user computer has DNS for hostname lookup&#x20;

Check to see if the server is up that is running that website&#x20;

Check to see if the server’s web service is running&#x20;

Check for DNS availability which is resolving that website&#x20;

109\. List 4 different directories in /?&#x20;

\


/etc, /bin, /tmp, /home&#x20;

110\. What is the output of the following command: $tail -10 filename | head -1&#x20;

\


It will show the first line from the last 10 lines of a file&#x20;

111\. What are the different fields in /etc/passwd file?&#x20;

\


Search online&#x20;

112\. Which command is used to list the processes?&#x20;

\


ps –ef&#x20;

113\. What is the difference between “hostname” and “uname” commands?&#x20;

\


Hostname will give you system name and uname will give you OS information&#x20;

114\. How to check system load?&#x20;

\


top and uptime command&#x20;

115\. How to schedule jobs?&#x20;

\


crontab and at&#x20;

116\. What is the 3rd field when setting up crontab?&#x20;

\


Day of the month&#x20;

\


117\. What is the command to create a new user?&#x20;

\


useradd&#x20;

118\. What is the “init #” for system reboot?&#x20;

\


6&#x20;

119\. How to restart a service?&#x20;

\


systemctl restart servicename&#x20;

120\. How to shutdown a system?&#x20;

\


shutdown or init 0&#x20;

121\. What is “ftp” command used for?&#x20;

\


To transfer files from one computer to another&#x20;

122\. Explain cron job syntax? First is minute, second is..?&#x20;

\


Min, house, day of the month, month, day of the week and command&#x20;

123\. How to delete a package in Linux?&#x20;

\


rpm –e packagename&#x20;

124\. What is the file name where user password information is saved?&#x20;

\


/etc/shadow&#x20;

125\. Which command you would use to find the location of chmod command?&#x20;

\


which chmod&#x20;

126\. Which command is used to check if the other computer is online?&#x20;

\


ping othercomputer&#x20;

127\. Please explain about LAN, MAN and WAN?&#x20;

\


Search online&#x20;

128\. How to list hidden files in a directory?&#x20;

\


ls –la&#x20;

129\. What is the difference between telnet and ssh?&#x20;

\


ssh is secure where telnet is not&#x20;

130\. How to run a calculator on Linux and exit out of it?&#x20;

\


bc and quit&#x20;

131\. List any 4 commands to monitor system?&#x20;

\


top, df –h, iostat, dmesg&#x20;

132\. You are notified that your server is down, list the steps you will take to troubleshoot?&#x20;

\


Check the system physically&#x20;

Login through system console&#x20;

Ping the system&#x20;

Reboot or boot if possible&#x20;

133\. What is difference between static and DHCP IP?&#x20;

\


Search online&#x20;

134\. How to write in vi editor mode?&#x20;

\


i = insert, a = insert in next space, o = insert in new line&#x20;

\


135\. What is the difference between “crontab” and “at” jobs?&#x20;

\


crontab is for repetitive jobs where at is for one time job&#x20;

136\. What is vCenter server in VMWare?&#x20;

\


Search online&#x20;

137\. What is “dmidecode” command used for?&#x20;

\


To get system information&#x20;

138\. What is the difference between SAN and NAS?&#x20;

\


Search online&#x20;

139\. What is the location of system logs? E.g. messages&#x20;

\


/var/log directory&#x20;

140\. How to setup an alias and what is it used for?&#x20;

\


alias aliasname=”command”&#x20;

It is used to created short-cuts for long commands&#x20;

141\. What is the purpose of “netstat” command?&#x20;

\


Search online&#x20;

142\. What are terminal control keys, list any 3?&#x20;

\


Crtl C, D and Z&#x20;

143\. Which command(s) you would run if you need to find out how many processes are running on your system?&#x20;

\


ps –ef | wc –l&#x20;

144\. What are the different types of shells?&#x20;

\


sh, bash, ksh, csh etc.&#x20;

145\. How to delete a line when in vi editor mode?&#x20;

\


dd&#x20;

146\. Which is the core of the operating system?&#x20;

\


a) Shell&#x20;

b) Kernel&#x20;

c) Commands&#x20;

d) Script&#x20;

147\. Which among the following interacts directly with system hardware?&#x20;

\


a) Shell&#x20;

b) Commands&#x20;

c) Kernel&#x20;

d) Applications&#x20;

148\. How to save and quit from vi editor?&#x20;

\


Shift ZZ or :wq!&#x20;

149\. What is the difference between a process and daemon?&#x20;

\


Search online&#x20;

150\. What is the process or daemon name for NTP?&#x20;

\


ntpd&#x20;

\


151\. What are a few commands you would run if your system is running slow?&#x20;

\


top, iostat, df –h, netstat etc.&#x20;

152\. How to install a package in Redhat Linux?&#x20;

\


yum install packagename&#x20;

153\. What is the difference between “ifconfig” and “ipconfig” commands?&#x20;

\


ifconfig for Linux and ipconfig for Windows&#x20;

154\. What is the first line written in a shell script?&#x20;

\


Define shell&#x20;

e.g. #!/bin/bash&#x20;

155\. Where is the network (Ethernet) file located, please provide exact directory location and file name?&#x20;

\


/etc/sysconfig/network-scripts/ifcfg-nic&#x20;

156\. Why do we use “last” command?&#x20;

\


To see who has logged in the system whether active or logged off&#x20;

157\. What is RHEL Linux stands for?&#x20;

\


Search online&#x20;

158\. To view your command history, which command is used and how to run a specific command?&#x20;

\


history and history #&#x20;

159\. What is NTP and briefly explain how does it work and where is the config files and related commands of NTP?&#x20;

\


Search online&#x20;

160\. How to disable firewall in Linux?&#x20;

\


Search online&#x20;

161\. How to configure mail server relay for sendmail service?&#x20;

\


Edit /etc/mail/sendmail.mc file and add SMART\_HOST entry&#x20;

162\. Where is samba log file located?&#x20;

\


/var/log/samba&#x20;

163\. What is mkfs command used for?&#x20;

\


To create a new filesystem&#x20;

164\. If you create a new group, which file does it get created in?&#x20;

\


/etc/group&#x20;

165\. Which file has DNS server information (e.g. DNS resolution)?&#x20;

\


/etc/resolv.conf&#x20;

166\. What are the commands you would run if you need to find out the version and build date of a package (e.g. http)?&#x20;

\


rpm –qi http&#x20;

\


167\. On the file permissions? What are the first 3 bits for and who is it for?&#x20;

\


Read, write and execute. They are used for the owner of the file&#x20;

168\. How to create a soft link?&#x20;

\


ln –s&#x20;

169\. How to write a script to delete messages in a log file older than 30 days automatically?&#x20;

\


Search online&#x20;

170\. How to quit out of “man” command?&#x20;

\


q&#x20;

171\. Which command is used to partition disk in Linux?&#x20;

\


fdisk&#x20;

172\. What is the difference between “shutdown” and “halt” command?&#x20;

\


Search online&#x20;

173\. What is the exact syntax of mounting NFS share on a client and also how to un-mount?&#x20;

\


Search online&#x20;

174\. What experience do you have with scripting, explain?&#x20;

\


if-the, do-while, case, for loop scripts&#x20;

175\. How to get information on all the packages installed on the system?&#x20;

\


rpm –qa&#x20;

176\. Explain VMWare?&#x20;

\


Search online&#x20;

177\. You are tasked to examine a log file in order to find out why a particular application keep crashing. Log file is very lengthy, which command can you use to simplify the log search using a search string?&#x20;

\


grep for error, warning, failure etc. in /var/log/messages file&#x20;

178\. What is /etc/fstab file and explain each column of this file?&#x20;

\


Search online&#x20;

179\. What the latest version of Windows server?&#x20;

\


Search online&#x20;

180\. What is the exact command to list only the first 2 lines of history output?&#x20;

\


history | head -2&#x20;

181\. How to upgrade Linux from 7.3 to 7.4?&#x20;

\


yum install update&#x20;

182\. How to tell which shell you are in or running?&#x20;

\


$0&#x20;

\


183\. You have tried to “cd” into a directory but you have been denied. You are not the owner of that directory, what permissions do you need and where?&#x20;

\


\- - - - - - - r – x&#x20;

184\. What is CNAME record in DNS?&#x20;

\


Entry for hostname to hostname&#x20;

185\. What is the name of VMWare operating system?&#x20;

\


ESXi&#x20;

186\. What is the client name used to connect to ESXi or vCenter server?&#x20;

\


vSphere client&#x20;

187\. You get a call from a user saying that I cannot write to a file because it says, permission denied. The file is owned by that user, how do you troubleshoot?&#x20;

\


Give write permission on the first 3 bits&#x20;

188\. What is the latest version of VMWare?&#x20;

\


Search online&#x20;

189\. What is the name of firewall daemon in Linux?&#x20;

\


firewalld&#x20;

190\. Which command syntax you can use to list only the 20th line of a file?&#x20;

\


Search online&#x20;

191\. What is the difference between run level 3 and 5?&#x20;

\


3 = Boot system with networking, 5 = boot system with networking and GUI&#x20;

192\. List a few commands that are used in troubleshooting network related issue?&#x20;

\


netstat, tcpdump etc.&#x20;

193\. What is the difference between domain and nameserver?&#x20;

\


Search online&#x20;

194\. You open up a file and it has 3000 lines and it scrolled up really fast, which command you will use to view it one page at a time?&#x20;

\


more or less&#x20;

195\. How to start a new shell. E.g. start a new ksh shell?&#x20;

\


Simply type ksh, or bash&#x20;

196\. How to kill a process?&#x20;

\


kill processID&#x20;

197\. How to check scheduled jobs?&#x20;

\


crontab –l&#x20;

\


198\. How to check system memory and CPU usage?&#x20;

\


free and top&#x20;

199\. Which utility could you use to repair the corrupted file system?&#x20;

\


fsck&#x20;

200\. What is the command to make a service start at boot?&#x20;

\


systemctl enable servicename&#x20;

201\. How to combine 2 files into 1? E.g. you 3 lines in file “A” and 5 lines in file “B”, which command syntax to use that will combine into one file of 3+5 = 8 lines&#x20;

\


cat fileA >> fileB&#x20;

202\. What is echo command used for?&#x20;

\


To output to a screen&#x20;

203\. What does the following command do?&#x20;

\


echo This year the summer will be great > file1&#x20;

It will create a new file “file1” with the content as “This year the summer will be great”&#x20;

204\. Which file to modify to allow users to run root commands&#x20;

\


/etc/sudoers&#x20;

205\. You need to modify httpd.conf file but you cannot find it, Which command line tool you can use to find file?&#x20;

\


find / -name “httpd.conf”&#x20;

206\. Your system crashed and being restarted, but a message appears, indicating that the operating system cannot be found. What is the most likely cause of the problem?&#x20;

\


The /boot file is most likely corrupted&#x20;
