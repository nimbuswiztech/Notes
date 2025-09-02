# Linux interview questions

Basic

### 1.    What is Linux? <a href="#id-1._what_is_linux" id="id-1._what_is_linux"></a>

Linus Torvalds developed Linux, a Unix-like, free, open-source, and kernel operating system. Mainly it is designed for systems, servers, embedded devices, mobile devices, and mainframes and is also supported on major computer platforms such as ARM, x86, and SPARC.

&#x20;

### 2.    Explain the basic features of the Linux OS. <a href="#id-2._explain_the_basic_features_of_the_lin" id="id-2._explain_the_basic_features_of_the_lin"></a>

Some basic features of Linux are:

&#x20;

• Linux is free and easily available.

&#x20;

• It is more secure than other operating systems because it uses security auditing and password authentication features.

&#x20;

• Linux has its personal software repository.

&#x20;

• It includes multiple languages throughout the world. Hence Linux supports different language keyboards.

&#x20;

• It offers CLI and GUI to use different commands and applications such as Firefox, VLC.

&#x20;

### 3.    Name some Linux Distros <a href="#id-3._name_some_linux_distros" id="id-3._name_some_linux_distros"></a>

There are various Linux distros but the following are the most commonly used:

&#x20;

• Ubuntu

&#x20;

• Debian

&#x20;

• CentOS

&#x20;

• Fedora

&#x20;

• RedHat

&#x20;

### 4.    What are the major differences between Linux and Windows? <a href="#id-4._what_are_the_major_differences_betwee" id="id-4._what_are_the_major_differences_betwee"></a>

\


<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Comparison Factor</td><td valign="top">Linux</td><td valign="top">Windows</td></tr><tr><td valign="top">Free/Paid</td><td valign="top">It is a free and open- source OS.</td><td valign="top">It is not open-source and is free to use.</td></tr><tr><td valign="top">Security</td><td valign="top">Linux is highly secure.</td><td valign="top">Windows is less secure compared to Linux.</td></tr><tr><td valign="top">Path separator</td><td valign="top">As a path separator, it uses a forward slash.</td><td valign="top">Windows uses a backward slash between the directories.</td></tr><tr><td valign="top">Efficiency</td><td valign="top">Linux is more efficient than Windows.</td><td valign="top">Windows is less efficient.</td></tr><tr><td valign="top">Kernel type</td><td valign="top">It uses a monolithic kernel.</td><td valign="top">It uses a microkernel.</td></tr><tr><td valign="top">File system</td><td valign="top">Linux file systems are case-sensitive.</td><td valign="top">Its file system is case-insensitive.</td></tr></tbody></table>

5\.    Define the basic components of Linux.

Majorly there are five basic components of Linux:

&#x20;

• Kernel: Linux kernel is a core part of the operating system that works as a bridge between hardware and software.

&#x20;

• Shell: Shell is an interface between a kernel and a user.

&#x20;

• GUI: Offers different way to interact with the system, known as the graphical user interface (GUI).

&#x20;

• Application programs: It is designed to perform a bundle of tasks through a bundle of functions.

&#x20;

• System Utilities: It is the software functions through which users manage the system.

&#x20;

### 6.    Elaborate all the file permission in Linux. <a href="#id-6._elaborate_all_the_file_permission_in" id="id-6._elaborate_all_the_file_permission_in"></a>

There are three types of file permissions in Linux:

&#x20;

• Read: Users open and read files with this permission.

\


• Write: Users can open and modify the files.

&#x20;

• Execute: Users can run the file.

&#x20;

### 7.    What is the Linux Kernel? Is it legal to edit it? <a href="#id-7._what_is_the_linux_kernel-_is_it_legal" id="id-7._what_is_the_linux_kernel-_is_it_legal"></a>

It is known as a low-level software system. TheLinux kernel tracks the resources and provides a user interface. This OS is released under GPL (General Public License). Hence every project is released under it. So, you can edit the Linux kernel legally.

&#x20;

### 8.    Explain LILO <a href="#id-8._explain_lilo" id="id-8._explain_lilo"></a>

LILO, i.e., Linux Loader and is a Linux Boot loader. It loads the Linux operating system into memory and starts the execution. Most operating systems like Windows and macOS come with a bootloader. While in Linux, you need to install a separate boot loader, and LILO is one of the Linux boot loaders.

&#x20;

### 9.    What is Shell in Linux? <a href="#id-9._what_is_shell_in_linux" id="id-9._what_is_shell_in_linux"></a>

In Linux, five Shells are used:

&#x20;

• csh (C Shell): This shell offers job control and spell checking and is similar to C syntax.]

&#x20;

• ksh (Korn Shell): A high-level shell for programming languages.

&#x20;

• ssh (Z Shell): This shell has a unique nature, such as closing comments, startup files, file name generating, and observing logout/login watching.

&#x20;

• bash (Bourne Again Shell): This is the default shell for Linux.

&#x20;

• Fish (Friendly Interactive Shell): This shell provides auto-suggestion, web- based configuration, etc.

&#x20;

### 10.     What is a root account? <a href="#id-10._what_is_a_root_account" id="id-10._what_is_a_root_account"></a>

The root is like the user’s name or system administrator account in Linux. The root account provides complete system control, which an ordinary user cannot do.

&#x20;

### 11.     Describe CLI and GUI in Linux. <a href="#id-11._describe_cli_and_gui_in_linux" id="id-11._describe_cli_and_gui_in_linux"></a>

CLI, i.e., command line interface. It takes input as a command and runs the tasks of the system. The term GUI refers to the Graphical User Interface or the human- computer interface. It uses icons, images, menus, and windows, which can be manipulated through the mouse.

&#x20;

### 12.     What is Swap Space? <a href="#id-12._what_is_swap_space" id="id-12._what_is_swap_space"></a>

\


Linux uses swap space to expand RAM. Linux uses this extra space to hold concurrently running programs temporarily.

&#x20;

### 13.     What is the difference between hard links and soft links? <a href="#id-13._what_is_the_difference_between_hard" id="id-13._what_is_the_difference_between_hard"></a>

Here is the table that shows the difference between soft links and hard links:

&#x20;

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Hard Links</td><td valign="top">Soft Links</td></tr><tr><td valign="top">It includes original content.</td><td valign="top">It includes the original file location.</td></tr><tr><td valign="top">Hard links are faster as compared to soft links.</td><td valign="top">Soft links are slower.</td></tr><tr><td valign="top">It shares similar inode numbers.</td><td valign="top">It shares different inode numbers.</td></tr><tr><td valign="top">There is no relative path for hard links.</td><td valign="top">Relative paths are used for soft links.</td></tr><tr><td valign="top">It didn’t link the directories.</td><td valign="top">It links the directories.</td></tr><tr><td valign="top">Any change in this link reflects other files directly.</td><td valign="top">Every change in this link reflects its hard link and the actual file directly.</td></tr><tr><td valign="top">It uses less memory.</td><td valign="top">It uses more memory.</td></tr></tbody></table>

&#x20;

### 14.     How do users create a symbolic link in Linux? <a href="#id-14._how_do_users_create_a_symbolic_link" id="id-14._how_do_users_create_a_symbolic_link"></a>

Symbolic links, symlink, or soft links are shortcuts to files and directories. Users can create the symbolic link in Linux through the’ ln’ command. The general command to create a symbolic link is as follows:

\


### 15.     What do you understand about the standard streams? <a href="#id-15._what_do_you_understand_about_the_sta" id="id-15._what_do_you_understand_about_the_sta"></a>

Output and input in Linux OS are divided into three standard streams:

\


• Stdin (standard input)

&#x20;

• stdout(standard output)

&#x20;

• stderr (standard error)

&#x20;

Under Linux, these standard streams channel communication of output and input between programs and their environment.

&#x20;

## Intermediate-Level Linux Interview Questions <a href="#intermediate-level_linux_interview_quest" id="intermediate-level_linux_interview_quest"></a>

The next 15 questions are the best suitable for those who have an intermediate level of experience in Linux:

&#x20;

### 16.     How do you mount and unmount filesystems in Linux? <a href="#id-16._how_do_you_mount_and_unmount_filesys" id="id-16._how_do_you_mount_and_unmount_filesys"></a>

In this case, you can use the ‘mount’ and ‘umount’ commands.

#### For mounting:

&#x20;

• First, identify the partition through the fdisk -l command. You can also use the lsblk command for it.

&#x20;

• After identifying the partition, create the directory which will work as the mount point. For example, running the mkdir /mnt/mountpnt will create the mountpnt directory as the mount point.

&#x20;

• Finally, you can run sudo mount \<partition> \<mount\_point\_directory> to complete the mounting.

&#x20;

#### For Unmounting:

Once you check if the specific filesystem is in use, you can run the \`sudo umount

\<mount\_point\_directory>\` for unmounting. If you want to learn more about the mount command in Linux.

### 17.     How do you troubleshoot network connectivity issues in Linux? <a href="#id-17._how_do_you_troubleshoot_network_conn" id="id-17._how_do_you_troubleshoot_network_conn"></a>

There are multiple ways to troubleshoot the network connectivity and find the issue correctly:

#### Check the Internet Connectivity:

First of all, please check if the internet connection option is on and also check the cables to find if there is any issue with it.

#### Verify the Network Configuration:

\


• Please check that your network is configured correctly and the network interface has your IP address. You can check it by running the ip addr or ifconfig commands.

&#x20;

• You can also run the ip route command to check if the default gateway is set properly.

&#x20;

• Finally, verify the DNS server configuration in the /etc/resolv.conf file.

&#x20;

#### Check the Firewall:

Sometimes, firewall rules block the internet connection for the system’s security. Hence, you can run the ufw or iptables command to modify the firewall rules.

#### Network Interface:

You can restart your network interface through the ifup and ifdown commands. Once you restart the network interface, please reboot the system to make changes successful.

&#x20;

### 18.     How do you list all the processes running in Linux? <a href="#id-18._how_do_you_list_all_the_processes_ru" id="id-18._how_do_you_list_all_the_processes_ru"></a>

You can list the currently running process in Linux through various commands such as:

#### ps Command:

The ps command displays brief information about the running processes. You can use the ps -f or ps -f command because the -f option shows the full-format result, and the

-e option displays all processes. Moreover, you can use the ps auxf command to get a detailed list of processes.

#### top and htop Command:

&#x20;

• The top command displays the real-time details about the system process and the complete resource usage.

&#x20;

• The htop command is the improved version of the top command because it displays the color-coded list with additional features such as sorting, filtering, sorting, etc.

&#x20;

### 19.     What is the chmod command in Linux, and how do you use it? <a href="#id-19._what_is_the_chmod_command_in_linux" id="id-19._what_is_the_chmod_command_in_linux"></a>

You can use the chmod command to change the file permissions of the directories. It offers a simple way to control the read and write permissions. For instance, if you want to change the permission of the ABC.sh script and give it the write and executable permission, you can run the below command:

chmod u+wx ABC.sh                                                           The chmod command is not limited to the write (w), read (r), and executable (x) permissions because there are symbolic modes and numeric modes.

\


### 20.     How do you check disk space usage? <a href="#id-20._how_do_you_check_disk_space_usage" id="id-20._how_do_you_check_disk_space_usage"></a>

There are some simple commands you can use to check disk space usage, such as:

#### df Command:

The df or disk-free command shows the used and the available disk space. You can use the additional options to check disk space differently. For instance, you can use the df -h command to check the disk usage in the human-readable format.

#### du Command:

The du or disk usage command estimates and shows the disk space usage, so running the du command with no option shows the disk usage of your current directory. However, you can run the following command to check the disk usage of a specific directory:

&#x20;

du -sh \~/\<directory>                                                      &#x20;

#### ncdu Command:

The NCurses Disk Usage, or ncdu command, displays more interactive disk usage. Similar to the du command, the ncdu command also requires the path of the specific directory to check its space.

&#x20;

### 21.     How do you find the process ID (PID) of a running process? <a href="#id-21._how_do_you_find_the_process_id_-pid" id="id-21._how_do_you_find_the_process_id_-pid"></a>

You can use the following command to find the Process ID or PID of the currently running process:

#### pgrep Command:

The pgrep command shows the PID of a process through its name or other different attributes. For example, you can find the PID of process\_1 using the below command:

&#x20;

pgrep \<process\_1>                                                         &#x20;

#### ps Command:

ps command not only displays the currently running process but also shows the process’s PID. However, if you want to check the PID of a specific process, you can combine the ps with the grep command:

&#x20;

ps -e | grep -i \<process\_1>                                               &#x20;

&#x20;

### 22.     What is the rsync command, and how do you use this command for synchronization? <a href="#id-22._what_is_the_rsync_command-_and_how_d" id="id-22._what_is_the_rsync_command-_and_how_d"></a>

The rsync command is used to synchronize and transfer the files in Linux. It synchronizes files between two local systems, directories, or a network. The basic rsync command contains the following:

&#x20;

rsync \<options> \<source> \<destination>                                    &#x20;

\


For example, let’s synchronize between Documents and the Downloads directory. For this, you need to run the following command:

&#x20;

rsync -av \~/Documents \~/Downloads                                         &#x20;

If you want to go one step further, then you can use the below command:

&#x20;

rsync -avz --delete \~/Documents \~/Downloads                               &#x20;

In the above command:

&#x20;

• The -a option preserves all the permissions and other attributes

&#x20;

• The -v option displays the detailed output of the synchronization

&#x20;

• The -z allows compression that decreases the bandwidth use.

&#x20;

• The –delete option removes the file in the Downloads that do not exist in the Documents directory.

&#x20;

### 23.     How do you create a user account? <a href="#id-23._how_do_you_create_a_user_account" id="id-23._how_do_you_create_a_user_account"></a>

You can use adduser and useradd commands to create a user for the system.

#### useradd Command:

Let’s create a username, “Ron,” and provide a password for accessing the system:

\


You can also explore the useradd command’s additional options to modify the new user’s permissions and privileges.

#### adduser Command:

The adduser command is similar to the useradd command, so let’s create a username “Shawn”:

\


### 24.     How do you format a disk in Linux? <a href="#id-24._how_do_you_format_a_disk_in_linux" id="id-24._how_do_you_format_a_disk_in_linux"></a>

The mkfs or make file system command helps format the disk in the Linux system. All you need to do is use the following method to format the disk:

First, run the lsblk command to list the available partitions and identify which disk you want to format.

If the selected disk is mounted, then unmount it through the following command:

umount \<partition>                                                          Now, find the file system type of the disk, like EXT4, NTFS, or XFS. Once you are done then, run one of the following commands according to the file system type:

\


![Text Box: mkfs.ext4 \<partition> mkfs.xfs \<partition> mkfs.ntfs \<partition>](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

Finally, mount the disk again through the mount command after the successful format. Moreover, please ensure that you have created a complete disk backup to eliminate the chances of data loss.

### 25.     How do you change the password for a user account? <a href="#id-25._how_do_you_change_the_password_for_a" id="id-25._how_do_you_change_the_password_for_a"></a>

Changing the password of a user account is simple because all you need to do is use the passwd command:

&#x20;

passwd username                                                           &#x20;

For example, let’s change the password of a user “Ron” through the below command:

passwd Ron                                                                  Once you run the command, the system will ask you to enter and confirm the new password.

&#x20;

### 26.     What is the difference between a process and a thread? <a href="#id-26._what_is_the_difference_between_a_pro" id="id-26._what_is_the_difference_between_a_pro"></a>

In Linux, processes are the independent program, while a thread is the unit of execution. So here are the complete differences between process and thread:

&#x20;

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Comparison Factors</td><td valign="top">Process</td><td valign="top">Thread</td></tr><tr><td valign="top">Creation time</td><td valign="top">Creation time is higher</td><td valign="top">Creation time is less.</td></tr><tr><td valign="top"><p> </p><p>Dependency</p></td><td valign="top">It is independent because it does not share memory.</td><td valign="top">It depends on other threads because they share some memory with other threads.</td></tr><tr><td valign="top">Resource</td><td valign="top">Resource use is higher</td><td valign="top">Requires lesser resources</td></tr><tr><td valign="top">Termination time</td><td valign="top">The termination time is higher</td><td valign="top">The termination time is less.</td></tr></tbody></table>

&#x20;

### 27.     What is the ulimit command, and how do you use it? <a href="#id-27._what_is_the_ulimit_command-_and_how" id="id-27._what_is_the_ulimit_command-_and_how"></a>

The ulimit command controls the resource limit for the user process. You can use the ulimit command to set the limit on the system resource to prevent consuming the higher resources. This command contains multiple options to set the limit. For example, you can use the u option to set a maximum number of processes to 50:

&#x20;

ulimit -u 50                                                              &#x20;

\


You can explore more options of the ulimit command.

### 28.     What is the find command, and how do you use it?

The find command searches for files based on different factors such as name, size, permissions, etc. Here is the basic command:

find \<directory> \<file>                                                     For example, let’s find a Linux.txt file located in the Downloads directory through the below command:

find \~/Downloads -name Linux.txt                                            Once you run the above command, the find command will start finding the Linux.txt in the Downloads directory and subdirectories.

&#x20;

### 29.     What is RAID in Linux? <a href="#id-29._what_is_raid_in_linux" id="id-29._what_is_raid_in_linux"></a>

The full form of RAID is the Redundant Array of Independent Disk that allows the system to combine the different physical disk drives into a logical unit. RAID is used to improve the system’s disk performance and data integrity. There are different RAID levels you can configure according to the requirements. Here is the detailed information about the RAID levels:

&#x20;

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top"><p>RAID</p><p>Level</p></td><td valign="top">Description</td></tr><tr><td valign="top">RAID 0</td><td valign="top">It is called striping, which allows you to split the data into multiple disks without redundancy.</td></tr><tr><td valign="top">RAID 1</td><td valign="top">It is called mirroring, which allows you to create a complete copy of data on multiple disks.</td></tr><tr><td valign="top">RAID 5</td><td valign="top">It distributes the parity information and data on multiple disks.</td></tr><tr><td valign="top">RAID 6</td><td valign="top">It is the improved version of RAID 5 as it uses two sets of parity information to provide higher data redundancy.</td></tr><tr><td valign="top">RAID 10</td><td valign="top">It combines RAID 0 and RAID 1 to generate the set of mirror disks to improve performance and redundancy.</td></tr></tbody></table>

&#x20;

### 30.     What are the challenges of using Linux? <a href="#id-30._what_are_the_challenges_of_using_lin" id="id-30._what_are_the_challenges_of_using_lin"></a>

There are numerous challenges that a user faces while using Linux:

\


• Linux shows hardware compatibility issues in certain devices because manufacturers prioritize Windows compatibility.

&#x20;

• Learning Linux is not easy because the configuration and commands require proper knowledge.

• Although Linux supports Steam, it still needs to be impressed regarding game compatibility and availability.

&#x20;

• Sometimes users face driver and firmware-related issues.

&#x20;

## Advanced-Level Linux Interview Questions <a href="#advanced-level_linux_interview_questions" id="advanced-level_linux_interview_questions"></a>

These 15 questions will revolve around your experience and help you in preparing for the advanced-level Linux interview:

&#x20;

### 31.     What is the /proc file system? <a href="#id-31._what_is_the_-proc_file_system" id="id-31._what_is_the_-proc_file_system"></a>

/proc (Proc File System) is the virtual file system that shows information about the system and the Kernel data structures. It is the essential interface to access the system, perform debugging tasks, check the Kernel functioning, find process-related information, and many more.

Therefore, you can use /proc file system in Linuxto get information about the system and modify the particular Kernel parameters at the runtime.

&#x20;

### 32.     How do you secure a Linux server? <a href="#id-32._how_do_you_secure_a_linux_server" id="id-32._how_do_you_secure_a_linux_server"></a>

There are multiple methods to secure the Linux server and protect it from data breaches, security threats, and unauthorized access. Here are some of these methods:

&#x20;

• Create a strong password

&#x20;

• Update the server and apply security patches.

&#x20;

• Use secured protocols like SSH and configure it to use key-based authentication for higher security.

&#x20;

• Use the intrusion detection system (IDS) to monitor network traffic and prevent malicious activities.

&#x20;

• Configure the firewall to limit the inbound and outbound traffic on the server.

&#x20;

• Disable all unused network services.

&#x20;

• Create regular backups.

&#x20;

• Review logs and perform regular security audits.

\


• Encrypt network traffic and enable monitoring.

&#x20;

### 33.     What is strace command? <a href="#id-33._what_is_strace_command" id="id-33._what_is_strace_command"></a>

The strace command is the diagnostic utility by which you can trace and monitor the system calls generated by the process. It allows you to find how programs interact with Kernel and can be used for debugging and troubleshooting. For example, let’s find the system calls generated by the ls command:

strace ls                                                                   Once you run the above command, the system will start tracing the list command and show the system calls generated by it. Output from the above command includes information like call name, argument, and return values.

&#x20;

### 34.     How do you optimize Linux system performance? <a href="#id-34._how_do_you_optimize_linux_system_per" id="id-34._how_do_you_optimize_linux_system_per"></a>

You can optimize the Linux performance through various strategies to improve resource usage and efficiency. So some of the strategies are:

&#x20;

• Updates the system as per the latest one available.

&#x20;

• Optimize the disk, enable the caching, and optimize the access pattern.

&#x20;

• Manage memory and CPU usage.

&#x20;

• Disable the necessary services and use lightweight alternatives of the tools.

&#x20;

• Monitor the system resources regularly.

&#x20;

• Perform the Kernel parameter tune-up.

&#x20;

• Use tools like Performance Co-Pilot (PCP) to monitor system-level performance.

&#x20;

### 35.     How to administer Linux servers? <a href="#id-35._how_to_administer_linux_servers" id="id-35._how_to_administer_linux_servers"></a>

Administering a Linux server requires different strategies and management to maintain the overall functionalities. Here are some major strategies you can follow:

&#x20;

• Handle user account management and assign appropriate access permissions.

&#x20;

• Configure the system to optimize the performance, improve the security and maintain the network connectivity.

&#x20;

• Implement the backup strategy to perform regular backups of the server.

&#x20;

• Implement the monitoring tools to track resource usage, system performance, and network.

\


• Set up monitoring tools to track system performance, resource usage, and network activity.

&#x20;

• Configure firewall, set up intrusion detection, manage user permissions and configure the SSH.

• Create a proper recovery planning that must include regular backup, critical configuration documentation, recovery process testing, and offsite storage.

&#x20;

### 36.     What is a Linux virtual memory system? <a href="#id-36._what_is_a_linux_virtual_memory_syste" id="id-36._what_is_a_linux_virtual_memory_syste"></a>

Virtual memory is a great memory management utility in any OS. You can use the virtual memory system as secondary memory. This memory is used by both software and hardware in Linux so that your system can cope with the lack of physical memory. Moreover, virtual memory is also used to compensate for the RAM usage by transferring the data temporarily from RAM to disk storage.

&#x20;

### 37.     What do you understand about process scheduling in Linux? <a href="#id-37._what_do_you_understand_about_process" id="id-37._what_do_you_understand_about_process"></a>

Process scheduling is the mechanism that identifies the order of processes running on the system. In other words, process schedulingdetermines the order and execution time of multiple processes running on the system concurrently. This process scheduler of Linux is priority-based and uses a preemptive algorithm. It allocates CPU time for different processes to ensure efficient CPU resource usage. These processes are dynamic, and their order can change depending on many factors, such as resource usage, process behavior, and scheduling policies.

&#x20;

### 38.     What are the most important Linux commands? <a href="#id-38._what_are_the_most_important_linux_co" id="id-38._what_are_the_most_important_linux_co"></a>

There are a ton of useful commands in Linux, and here are some of the commonly used commands:

&#x20;

• ls: Display directory contents such as folders and files.

&#x20;

• mkdir: Used to create a new directory.

&#x20;

• pwd: Shows the current directory.

&#x20;

• top: Display system running processes and resource usage.

&#x20;

• grep: Search a specific pattern in a file.

&#x20;

• cat: Through this command, users can add multiple files and also display the content of the files.

&#x20;

• tar: Archives directories and files into a tarball.

&#x20;

• wget: Download files from the browser or web.

\


• free: Shows memory usage.

&#x20;

• df: Shows disk space usage.

&#x20;

• man: Gives a manual page for a specific command that displays instructions and details.

&#x20;

### 39.     What is the iptables command, and how to use it for network filtering? <a href="#id-39._what_is_the_iptables_command-_and_ho" id="id-39._what_is_the_iptables_command-_and_ho"></a>

The iptables command configures Netfilter firewall rules providing the network address translation, packet filtering, etc. iptables inspects the network packet and then manages them according to the defined rules. Here is how you can use the iptables command for network filtering:

Run the below command to display the current iptables rules, including policies, chains, and other actions for the network:

iptables -L                                                                 The iptables configuration uses the predefined set of chains to process the network packages at different stages. So you can define rules to these chains for manipulating the network packets:

&#x20;

iptables -A \<chain> \<options> -j \<target>                                 &#x20;

In the above command:

&#x20;

• \<chain>: Specifies the chain where you want to define a new rule.

&#x20;

• \<options>: Defines the conditions for the rule, like ports, protocols, etc.

&#x20;

• -j \<target>: Defines the target action when the packet matches the rule.

&#x20;

By default, iptables rules get automatically removed after the system reboot, but you can use the following command to make the rules persistent:

&#x20;

iptables-save > /etc/iptables/rules.v4                                    &#x20;

&#x20;

### 40.     How do you troubleshoot a Linux OS that fails to boot? <a href="#id-40._how_do_you_troubleshoot_a_linux_os_t" id="id-40._how_do_you_troubleshoot_a_linux_os_t"></a>

In case of the system boot failure, you can follow various approaches such as:

&#x20;

• Check the warning and error messages you get during the boot process because it can help you diagnose the issues.

&#x20;

• Check the boot logs to find the exact reason behind the boot error.

&#x20;

• Open the GRUB bootloader and check the boot options to solve the booting problems.

&#x20;

• Check the hardware connections like cables, RAM, cooling fan, etc.

\


• If the system shows an error message related to the Kernel, try to boot it with the older Kernel version from GRUB.

&#x20;

• Identify the last changes you made in the system before the boot.

&#x20;

### 41.     What is the init process in Linux? <a href="#id-41._what_is_the_init_process_in_linux" id="id-41._what_is_the_init_process_in_linux"></a>

The init or also called the initialization process is the first process that begins during the system boot. It is responsible for initializing and processing the system in its functional state. Hence, init works as the parent process because its process ID is

1\. Originally Linux systems used to have SysV init, but now it is developed as the systemd init (an improved version of SysV).

&#x20;

### 42.     What is SMTP? <a href="#id-42._what_is_smtp" id="id-42._what_is_smtp"></a>

SMTP stands for Simple Mail Transfer Protocol. This set of communication guidelines allows the software to transmit electronic mail online. The main aim of SMTP is to set communication rules between servers. There are two models of SMTP:

&#x20;

• End-to-end model: This model is used to connect different organizations.

&#x20;

• Store-and-forward model: This model is used within an organization.

&#x20;

### 43.     What is LVM in Linux? <a href="#id-43._what_is_lvm_in_linux" id="id-43._what_is_lvm_in_linux"></a>

The full form of LVM is Logical Volume Manager, which provides an advanced disk management approach in Linux. It is a subsystem that allows a user to efficiently allocate the disk space on the physical storage device.

You can use the LVM to create the logical volume for easy storage management through various features like resizing, volume mirroring, and snapshots. LVM is a powerful utility for disk management where you need dynamic storage allocations.

&#x20;

### 44.     What is the difference between UDP and TCP? <a href="#id-44._what_is_the_difference_between_udp_a" id="id-44._what_is_the_difference_between_udp_a"></a>

The following table shows the difference between UDP and TCP:

&#x20;

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Factors</td><td valign="top">UDP</td><td valign="top">TCP</td></tr><tr><td valign="top"><p> </p><p>Connection- oriented</p></td><td valign="top"><p> </p><p>UDP does not establish a proper connection.</p></td><td valign="top">TCP is connection-oriented because it establishes a connection between the sender and receiver.</td></tr></tbody></table>

\


<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Factors</td><td valign="top">UDP</td><td valign="top">TCP</td></tr><tr><td valign="top"><p> </p><p>Reliability</p></td><td valign="top">UDP does not provide a reliability mechanism.</td><td valign="top">It guarantees reliable data delivery by retransmitting corrupt packets or lost packets.</td></tr><tr><td valign="top"><p> </p><p>Usage</p></td><td valign="top">It is used in low overhead, speed, and real-time communication applications.</td><td valign="top">It is used where ordered data is delivered, and reliable data must be delivered.</td></tr><tr><td valign="top"><p> </p><p>Applications</p></td><td valign="top">Video/voice conferencing, DNS, online gaming, streaming media, etc.</td><td valign="top">File transfers, email, web browsing, database transactions, etc.</td></tr></tbody></table>

### 45.     What is /etc/resolv.conf file <a href="#id-45._what_is_-etc-resolv.conf_file" id="id-45._what_is_-etc-resolv.conf_file"></a>

The /etc/resolv.conf is the config file used for the DNS server resolution process. This config file is used to specify the DNS server, set up the search directive for domains, and configure the resolver options.

&#x20;

### 46.     What is the difference between absolute and relative paths in Linux? <a href="#id-46._what_is_the_difference_between_absol" id="id-46._what_is_the_difference_between_absol"></a>

Absolute path = It specifies the exact location of a file or directory from the root directory (“/”). We will notice that they always start with a forward slash (“/”).

For Example: \`/home/user/jayesh/geeksforgeeks.txt\`

Relative paths = It specifies the location relative to the current working directory. In this we do not start with a forward slash (“/”).

For Example: \`documents/file.txt\`

&#x20;

### 47.     What is the grep command used for in Linux? <a href="#id-47._what_is_the_grep_command_used_for_in" id="id-47._what_is_the_grep_command_used_for_in"></a>

The grep command is used to search for specific patterns within files or input streams. It allows us to find and print lines that we give to match the pattern.

For example: If we want to search \`test\` in a text file name “file.txt”. We use the following command

grep "test" file.txt                                                        This command will search for the word \`test\` in the file named “file.txt” and print the matching lines.

&#x20;

### 48.     How do you check the status of a service or daemon in Linux? <a href="#id-48._how_do_you_check_the_status_of_a_ser" id="id-48._how_do_you_check_the_status_of_a_ser"></a>

\


To check the status of a service or daemon, we can use the \`systemctl\` command followed by the service name.

For example: If we want to display the status of the Apache Web server. We use the following command.

&#x20;

systemctl status apache2                                                  &#x20;

It will show whether the service is running, stopped, or in an error state.

&#x20;

### 49.     What is the difference between /etc/passwd and /etc/shadow files? <a href="#id-49._what_is_the_difference_between_-etc" id="id-49._what_is_the_difference_between_-etc"></a>

The /etc/passwd file stores essential user information like usernames, user IDs, home directories, and default shells. Each line in the file represents a user account.

The /etc/shadow file contains encrypted passwords and other security-related information. It is only accessible by the root user or privileged processes

&#x20;

### 50.     How do you compress and decompress files in Linux? <a href="#id-50._how_do_you_compress_and_decompress_f" id="id-50._how_do_you_compress_and_decompress_f"></a>

To compress files in Linux, you can use the tar command along with gzip compression.

For example: If we want to create a file name “jayesh” with gzip compression. We use the following command.

&#x20;

tar -czvf jayesh.tar.gz files                                             &#x20;

This command will create a compressed archive file containg the specified “files” To decompress the same, we use the following command.

tar -xzvf jayesh.tar.gz                                                   &#x20;

&#x20;

### 51.     What is the difference between a process and a daemon in Linux? <a href="#id-51._what_is_the_difference_between_a_pro" id="id-51._what_is_the_difference_between_a_pro"></a>

A process is an executing instance of a program. It can be a foreground process that interacts with the user or a background process started by a user or another process.

A daemon is a background process that runs independently of user sessions. It is typically started at system boot time and performs system tasks or provides services. Daemons often have no user interaction and continue running even when users log out.

&#x20;

### 52.     How do you schedule recurring tasks in Linux? <a href="#id-52._how_do_you_schedule_recurring_tasks" id="id-52._how_do_you_schedule_recurring_tasks"></a>

We can use \`crontab\` command for performing recurring tasks in Linux. By adding entries to the crontab file, we can specify when and how frequently a command or script should be executed

For Example: If we want to execute a script name “geeks.sh” every day at 3:30 AM. We use the following command.

\


crontab -e                                                                &#x20;

This command opens the crontab file in an editor.

&#x20;

30 3 \* \* \* /path/to/geeks.sh                                              &#x20;

&#x20;

### 53.     What is the sed command used for in Linux? <a href="#id-53._what_is_the_sed_command_used_for_in" id="id-53._what_is_the_sed_command_used_for_in"></a>

The sed command is used to perform text transformations on files. It can search for specific patterns and replace them with desired text.

#### For Example:

&#x20;

sed \`s/foo/bar/g\` file.txt                                                &#x20;

This command replaces all occurrences of “foo” with “bar” in the file name “file.txt”

&#x20;

### 54.     What are runlevels in Linux? <a href="#id-54._what_are_runlevels_in_linux" id="id-54._what_are_runlevels_in_linux"></a>

Runlevels in Linux define different system states, such as single-user mode or multi- user mode with or without a GUI. They determine which services start or stop during system startup and shutdown. The default runlevel is often set to a multi-user mode with a GUI (runlevel 5). Runlevel 3 is commonly used for a multi-user mode without a GUI.

&#x20;

## Bonus Linux Interview Questions <a href="#bonus_linux_interview_questions" id="bonus_linux_interview_questions"></a>

The next 5 Linux interview questions are the most common ones recruiters ask.

&#x20;

### 55.     What is sudo in Linux? <a href="#id-55._what_is_sudo_in_linux" id="id-55._what_is_sudo_in_linux"></a>

The word “sudo” is the short form of “Superuser Do” that allows you to run the command with system privileges. With this command, you can get the system’s administrative access to perform various tasks. The sudo command requires a password before the execution to verify the user’s authorization.

&#x20;

### 56.     What is umask? <a href="#id-56._what_is_umask" id="id-56._what_is_umask"></a>

It is used for user file creation mode. When a user creates any file, then it has default file permission. Umask specifies restrictions for these permissions on the file, i.e., controls the permissions.

&#x20;

### 57.     How to find and kill a process in Linux? <a href="#id-57._how_to_find_and_kill_a_process_in_li" id="id-57._how_to_find_and_kill_a_process_in_li"></a>

You can use different commands to kill a process, but first, you must find the PID of that specific process. So, please run the below command:

&#x20;

ps aux | grep \<process>                                                   &#x20;

Once you get the PID of the process then run the kill command to end it:

&#x20;

kill \<PID>                                                                &#x20;

\


If you don’t want to find the PID, then you can use the pkill command to kill a process by its name:

pkill \<process>                                                             The pkill command sends a signal (by default, SIGTERM) to the matched processes, causing them to terminate.

&#x20;

### 58.     What is network bonding in Linux? <a href="#id-58._what_is_network_bonding_in_linux" id="id-58._what_is_network_bonding_in_linux"></a>

Network bonding is the process of creating a single network by combining two or more network interfaces. This combination of networks improves redundancy and performance by increasing bandwidth and throughput. The major benefit of network bonding is that the overall network works fine even if a single network in the bonding does not work properly.

&#x20;

### 59.     What is SELinux? <a href="#id-59._what_is_selinux" id="id-59._what_is_selinux"></a>

SELinux or also known as Security-Enhanced Linux, is the security framework. It offers an additional layer of security to improve access control and strengthen security. SELinux was developed to improve the security policies to prevent unauthorized access and exploitation. However, learning about SELinux is essential before working on it can create serious security issues.

&#x20;

### 60.     What is the purpose of the SSH protocol in Linux, and how do you securely connect to a remote server using SSH? <a href="#id-60._what_is_the_purpose_of_the_ssh_proto" id="id-60._what_is_the_purpose_of_the_ssh_proto"></a>

The Secure Shell (SSH) is a protocol in Linux which is used to establish a secure encrypted connection between a local and remote machine. It allows to securely access and manage remote servers. If we want to connect to a remote server using SSH. We can use the following command.

ssh username@remote\_ip                                                      Here replace the \`username\` with the desired username of the remote server and replace the \`remote\_ip\` with the IP address of the remote server.

&#x20;

### 61.     How do you check the contents of a file without opening it in Linux? <a href="#id-61._how_do_you_check_the_contents_of_a_f" id="id-61._how_do_you_check_the_contents_of_a_f"></a>

In Linux we can use the \`cat\` command to view the content of a file without opening it in an editor form.

For example: If we want to check content of a file with file\_name = \`geeks.txt\`

&#x20;

cat geeks.txt                                                             &#x20;

&#x20;

### 62.     What is the purpose of the crontab file in Linux, and how do you schedule recurring tasks using cron jobs? <a href="#id-62._what_is_the_purpose_of_the_crontab_f" id="id-62._what_is_the_purpose_of_the_crontab_f"></a>

\


The crontab file in Linux is used to schedule recurring tasks or cron jobs. It contains a list of commands or scripts that are executed at specified time intervals. To edit the crontab file, you can use the crontab -e command.

For example: If we want to run a script name \`jayesh.sh\` every day at 5 AM, we can use the following procedure.

First, we need to open the crontab in editorial format.

&#x20;

crontab -e                                                                &#x20;

Secondly, add the entries in the crontab file.

&#x20;

0 5 \* \* \* /path/to/jayesh.sh                                              &#x20;

&#x20;

### 63.     How do you find and replace text in a file using the sed command in Linux? <a href="#id-63._how_do_you_find_and_replace_text_in" id="id-63._how_do_you_find_and_replace_text_in"></a>

The sed command (stream editor) can be used to find and replace text in a file. The basic syntax is sed ‘s/pattern/replacement/g’ filename.

For example: to replace all occurrences of “true” with “False” in a file

&#x20;

sed 's/true/False/g' file\_name                                            &#x20;

&#x20;

### 64.     What is the purpose of the sudoers file in Linux, and how do you configure sudo access for users? <a href="#id-64._what_is_the_purpose_of_the_sudoers_f" id="id-64._what_is_the_purpose_of_the_sudoers_f"></a>

The sudoers file in Linux controls the sudo access permissions for users. It determines which users are allowed to run commands with superuser (root) privileges. To configure sudo access, you can edit the sudoers file using the visudo command.

#### For example:

sudo visudo                                                                 Now add this line anywhere in the file. For instance, if we want to grant a user full sudo access.

&#x20;

user\_name ALL=(ALL) ALL                                                   &#x20;

&#x20;

### 65.     How do you change the ownership of a file or directory in Linux using the chown command? <a href="#id-65._how_do_you_change_the_ownership_of_a" id="id-65._how_do_you_change_the_ownership_of_a"></a>

In Linux, you can change the ownership of a file or directory using the chown command. The basic syntax is chown new\_owner: new\_group filename.

For example: If we want to change the ownership of a file to user “Jayesh” and group “users”.

&#x20;

chown jayesh:users file\_name                                              &#x20;

\


### 66.     What is the purpose of the ping command in Linux, and how do you test network connectivity to a remote host? <a href="#id-66._what_is_the_purpose_of_the_ping_comm" id="id-66._what_is_the_purpose_of_the_ping_comm"></a>

Ping command is used to test the network connectively between the local and remote hosts. It basically sends an ICMP echo request packet to the remote host and waits for the corresponding echo reply packet.

For example: If we want to check the connectivity to a remote host, we use the following command.

&#x20;

ping remote\_host\_ip                                                       &#x20;

Here replace \`remote\_host\_ip\` with the Ip address of the host

&#x20;

### 67.     How do you recursively copy files and directories in Linux using the cp command? <a href="#id-67._how_do_you_recursively_copy_files_an" id="id-67._how_do_you_recursively_copy_files_an"></a>

In linxux we can simply use \`-R\` option with the \`cp\` command to recursively copy the file and directories.

#### For example:

&#x20;

cp -R sourece\_durectory destination\_directory                             &#x20;

&#x20;

### 68.     What is the purpose of the netstat command in Linux, and how do you view network connections and listening ports? <a href="#id-68._what_is_the_purpose_of_the_netstat_c" id="id-68._what_is_the_purpose_of_the_netstat_c"></a>

The netstat command in Linux is used to display active network connections, routing tables, and listening ports. To view network connections and listening ports, use the netstat command with appropriate options.

For example: If we want to display all listening TCP ports, we can use the following command.

&#x20;

netstat -tuln                                                             &#x20;

&#x20;

### 69.     How do you set up a static IP address in Linux using the command-line interface? <a href="#id-69._how_do_you_set_up_a_static_ip_addres" id="id-69._how_do_you_set_up_a_static_ip_addres"></a>

To set up a static IP address in Linux using the command-line interface, you need to modify the network configuration file. The location and name of the file may vary depending on the Linux distribution, but commonly it is /etc/network/interfaces. Open the file with a text editor and modify the configuration to set a static IP address, subnet mask, gateway, and DNS servers.

#### For example:

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image005.jpg)iface eth0 inet static address 192.168.1.100

netmask 255.255.255.0

gateway 192.168.1.1

dns-nameservers 8.8.8.8 8.8.4.4

\


Save the file and restart the network service or reboot the system for the changes to take effect.

&#x20;

### 70.     How to copy a file to multiple directories in Linux? <a href="#id-70._how_to_copy_a_file_to_multiple_direc" id="id-70._how_to_copy_a_file_to_multiple_direc"></a>

We can copy a file to multiple directories in Linuxby these methods and command xargs, find, tee and shell loop.

&#x20;

• xargs command on Unix/Linux operating system converts input from standard input into an argument list for a specified command.

&#x20;

• The command find initiates a search and allows actions to be performed based on the search results.

&#x20;

• The tee command reads standard input and copies it to both standard outputs and to one or more files.

&#x20;

## Linux Admin Interview Questions <a href="#linux_admin_interview_questions" id="linux_admin_interview_questions"></a>

### 71. How are files organized in Linux? <a href="#id-71.how_are_files_organized_in_linux" id="id-71.how_are_files_organized_in_linux"></a>

Linux follows a hierarchical file system structure. The root directory is denoted by “/”, and files are organized in directories or folders within the root directory.

&#x20;

### 72. How can you find the IP address of a Linux system? <a href="#id-72.how_can_you_find_the_ip_address_of_a" id="id-72.how_can_you_find_the_ip_address_of_a"></a>

The ‘ifconfig’ or ‘ip addr show’ command can be used to display the IP address of a Linux system.

### 73. What is the distinction between a hard link and a symbolic link in Linux? <a href="#id-73.what_is_the_distinction_between_a_har" id="id-73.what_is_the_distinction_between_a_har"></a>

A hard link is a direct reference to a file, whereas a symbolic link is a reference to the file’s path. Deleting a hard link does not affect the file, but deleting a symbolic link breaks the link between the file and its path.

&#x20;

### 74. How do you check the amount of disk space being used in Linux? <a href="#id-74.how_do_you_check_the_amount_of_disk_s" id="id-74.how_do_you_check_the_amount_of_disk_s"></a>

The ‘df’ command displays information about the disk space usage on Linux, including the total, used, and available space on filesystems.

&#x20;

### 75. How do you start and stop a service in Linux? <a href="#id-75.how_do_you_start_and_stop_a_service_i" id="id-75.how_do_you_start_and_stop_a_service_i"></a>

The ‘systemctl start \<service>’ command is used to start a service, and ‘systemctl stop \<service>’ is used to stop a service in Linux.

&#x20;

### 76. What are common causes of file permission issues in Linux? <a href="#id-76.what_are_common_causes_of_file_permis" id="id-76.what_are_common_causes_of_file_permis"></a>

\


Common causes of file permission issues in Linux include incorrect ownership, improper permissions set for users or groups, and conflicts between different users’ permissions.

&#x20;

### 77. How do you troubleshoot a Linux system that cannot connect to a remote server? <a href="#id-77.how_do_you_troubleshoot_a_linux_syste" id="id-77.how_do_you_troubleshoot_a_linux_syste"></a>

Possible troubleshooting steps include checking network connectivity using tools like ‘ping’, verifying firewall rules, checking DNS settings, and examining relevant log files for error messages.

&#x20;

## Linux Troubleshooting Interview Questions: <a href="#linux_troubleshooting_interview_question" id="linux_troubleshooting_interview_question"></a>

### 78. What steps would you take to fix a network connectivity issue in Linux? <a href="#id-78.what_steps_would_you_take_to_fix_a_ne" id="id-78.what_steps_would_you_take_to_fix_a_ne"></a>

Steps would include checking physical connections, verifying IP configuration, checking firewall settings, ensuring DNS resolution is working, and using network troubleshooting tools like ‘ping’, ‘traceroute’, or ‘tcpdump’.

### 79. How do you check the system logs in Linux? <a href="#id-79.how_do_you_check_the_system_logs_in_l" id="id-79.how_do_you_check_the_system_logs_in_l"></a>

System logs can be checked using the ‘tail’ or ‘less’ command to view the contents of log files located in the ‘/var/log’ directory, such as ‘syslog’, ‘messages’, or ‘auth.log’.

&#x20;

### 80. What are the possible reasons for a Linux system running out of memory? <a href="#id-80.what_are_the_possible_reasons_for_a_l" id="id-80.what_are_the_possible_reasons_for_a_l"></a>

Possible reasons include memory leaks in applications, excessive memory usage by running processes, inadequate memory allocation, or high memory demands from large datasets.

&#x20;

### 81. How would you troubleshoot a slow-performing Linux server? <a href="#id-81.how_would_you_troubleshoot_a_slow-per" id="id-81.how_would_you_troubleshoot_a_slow-per"></a>

Troubleshooting steps might involve checking system resource usage with tools like ‘top’ or ‘htop’, monitoring disk I/O, analyzing network traffic, identifying memory or CPU bottlenecks, and reviewing application logs.

&#x20;

### 82. What are common causes of a Linux system running out of disk space? <a href="#id-82.what_are_common_causes_of_a_linux_sys" id="id-82.what_are_common_causes_of_a_linux_sys"></a>

Common causes include large log files, excessive data storage, uncontrolled growth of temporary files, improper cleanup of old files, or runaway processes generating excessive output.

&#x20;

### 83. How can you identify and terminate a process that is using a lot of CPU in Linux? <a href="#id-83.how_can_you_identify_and_terminate_a" id="id-83.how_can_you_identify_and_terminate_a"></a>

\


The ‘top’ or ‘htop’ command can display the processes using the most CPU. To terminate a process, the ‘kill’ command followed by the process ID (PID) can be used.

&#x20;

### 84. How would you troubleshoot a Linux system that cannot boot up? <a href="#id-84.how_would_you_troubleshoot_a_linux_sy" id="id-84.how_would_you_troubleshoot_a_linux_sy"></a>

Troubleshooting steps might include checking hardware connections, verifying BIOS/UEFI settings, booting into a recovery mode or live system, analyzing boot logs, and diagnosing disk or file system errors.

&#x20;

## Linux Networking Interview Questions: <a href="#linux_networking_interview_questions" id="linux_networking_interview_questions"></a>

### 85. What does the ‘ifconfig’ command do in Linux? <a href="#id-85.what_does_the_ifconfig_command_do_i" id="id-85.what_does_the_ifconfig_command_do_i"></a>

The ‘ifconfig’ command is used to configure or display network interfaces in Linux. It can be used to view or modify IP addresses, netmasks, and other network interface parameters.

&#x20;

### 86. How do you set up a fixed IP address in Linux? <a href="#id-86.how_do_you_set_up_a_fixed_ip_address" id="id-86.how_do_you_set_up_a_fixed_ip_address"></a>

A fixed IP address can be set up in Linux by editing the network configuration file (e.g., ‘/etc/network/interfaces’ or ‘/etc/sysconfig/network-scripts/ifcfg-\<interface>’) and assigning the desired IP address to the interface.

&#x20;

### 87. How do you configure a DNS server in Linux? <a href="#id-87.how_do_you_configure_a_dns_server_in" id="id-87.how_do_you_configure_a_dns_server_in"></a>

DNS server configuration involves editing the ‘/etc/named.conf’ (BIND) or ‘/etc/named/named.conf.options’ (ISC BIND) file to specify the server’s zone information, name resolution options, and defining forwarders or root hints.

&#x20;

### 88. What is a firewall in Linux, and how do you set it up? <a href="#id-88.what_is_a_firewall_in_linux-_and_how" id="id-88.what_is_a_firewall_in_linux-_and_how"></a>

A firewall is a network security system that filters and controls network traffic. In Linux, ‘iptables’ or newer ‘nftables’ can be used to set up firewall rules by defining filtering criteria, network zones, and desired actions.

&#x20;

### 89. How do you check the network connectivity between two Linux systems? <a href="#id-89.how_do_you_check_the_network_connecti" id="id-89.how_do_you_check_the_network_connecti"></a>

Network connectivity between two Linux systems can be checked using tools like ‘ping’ or ‘traceroute’, which send packets to the target system and report on the round-trip time and the path taken.

&#x20;

### 90. What is the purpose of the ‘route’ command in Linux? <a href="#id-90.what_is_the_purpose_of_the_route_co" id="id-90.what_is_the_purpose_of_the_route_co"></a>

The ‘route’ command is used to view or modify the IP routing table on a Linux system. It displays information about the network routes and allows adding or deleting routes.

\


### 91. How do you configure a Linux system to act as a router? <a href="#id-91.how_do_you_configure_a_linux_system_t" id="id-91.how_do_you_configure_a_linux_system_t"></a>

To configure a Linux system as a router, IP forwarding must be enabled by setting the appropriate value in the ‘/proc/sys/net/ipv4/ip\_forward’ file. Additionally, network interfaces and routing tables need to be configured accordingly.
