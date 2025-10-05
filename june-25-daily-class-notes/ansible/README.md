# Ansible

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*sygxsC7Iojd2vgzPoOwJdA.png" alt="" height="700" width="700"><figcaption><p>AI-Generated Image</p></figcaption></figure>

Ansible is an open-source automation and configuration management tool. It simplifies deploying, configuring, and managing systems using a language called `YAML`. It is agentless, meaning it doesn’t require software installation on target systems.

Ansible performs version control on the steps executed. This means it will only perform updates on steps that require updates, instead of a full reprovision.

This article provides a comprehensive guide on Ansible. It explains the installation process and covers basic usage examples of Ansible playbooks.

### Installation Steps: <a href="#id-1122" id="id-1122"></a>

* Install the [Extra Packages for Enterprise Linux (EPEL) repository](https://docs.fedoraproject.org/en-US/epel/). The EPEL repository contains various system packages including the Ansible package which you will install in the next step:

```
sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

* Install Ansible:

```
sudo yum install ansible
```

> _During the installation, you will be prompted to install dependencies. Enter “**Y**” to install all dependencies._

The following packages will be downloaded, If they are not already installed:

> ansible\
> ansible-core\
> git-core\
> mpdecimal\
> python3-jmespath\
> python3.11\
> python3.11-cffi\
> python3.11-cryptography\
> python3.11-libs\
> python3.11-pip-wheel\
> python3.11-ply\
> python3.11-pycparser\
> python3.11-pyyaml\
> python3.11-setuptools-wheel\
> sshpass

### Verify the installation: <a href="#ad8e" id="ad8e"></a>

After the installation is complete, you can verify the installation of Ansible by checking its version:

```
ansible --version
```

### Ansible Configuration Files: <a href="#id-3062" id="id-3062"></a>

Ansible’s default configuration file is located at `/etc/ansible/ansible.cfg`. It is often empty or minimal because Ansible uses built-in [default settings](https://github.com/ansible/ansible/blob/stable-2.9/examples/ansible.cfg) that are applied automatically. These internal defaults will only be overridden if you modify the configuration file.

To display the current settings, use the following command:

```
ansible-config dump
```

### Inventory: <a href="#id-17ea" id="id-17ea"></a>

The inventory file contains a list of target machines that Ansible will manage. You can group servers by defining a group name in square brackets. For example:

```
[linux]
example1

[mail]
example2
```

#### Inventory Parameters <a href="#id-6da3" id="id-6da3"></a>

Here are some common parameters you can define for each host in the inventory file:

* `ansible_connection` – Defines the connection type (e.g., `ssh`, `winrm`, `localhost`).
* `ansible_port` – Specifies the port number (e.g., `22` for SSH, `5986` for WinRM).
* `ansible_user` – Sets the username to connect as (e.g., `root`, `administrator`).
* `ansible_ssh_pass` – Defines the password for SSH login.

If you don’t create a custom inventory file, Ansible will use the default inventory located at `/etc/ansible/hosts`.

### Playbooks: <a href="#id-19b2" id="id-19b2"></a>

In Ansible, playbooks are used to describe automation tasks. They are written in YAML format and consist of plays, containing a series of tasks to be executed on remote hosts.

```
+--------------------------+        +------------------------+        +-------------------+
|                          |        |                        |        |                   |
|   Ansible Control Server |        |       Inventory File   |        |     Playbooks     |
|                          |        |                        |        |                   |
+--------------------------+        +------------------------+        +-------------------+
|                          |        |                        |        |                   |
|  Acts as the central     |        |   Contains a list of   |        |   Contains the    |
|  management point for    |        |   target hosts where   |        |   automation      |
|  Ansible operations.     |        |   Ansible operations   |        |   instructions    |
|                          |        |   will be executed.    |        |   written in      |
|                          |        |                        |        |   YAML format.    |
|                          |        |                        |        |                   |
|  Manages and controls    |        |   Each host is defined |        |   Defines tasks,  |
|  the inventory of        |        |   by an IP address or  |        |   configurations, |
|  target hosts.           |        |   hostname along with  |        |   and other       |
|                          |        |   other optional       |        |   actions to be   |
|                          |        |   parameters.          |        |   executed on     |
|                          |        |                        |        |   target hosts.   |
|                          |        |                        |        |                   |
|  Executes playbooks      |        |                        |        |                   |
|  on target hosts.        |        |                        |        |                   |
|                          |        |                        |        |                   |
+--------------------------+        +------------------------+        +-------------------+
```

### Ansible user <a href="#e49f" id="e49f"></a>

For target authentication create a user named **ansible** (optional) on the target machines, and edit the sudoers file to add the (`NOPASSWD: ALL`) attribute, to allow passwordless sudo for the ansible user.

* Create the user `ansible` (on the target host):

```
useradd ansible
passwd ansible
```

* Edit the sudoers file (on the target host):

```
nano /etc/sudoers
```

Add the entry:

`ansible ALL=(ALL) NOPASSWD: ALL`

### Target Authentication: <a href="#fca2" id="fca2"></a>

Ansible enables host key checking by default. If a new host is not in ‘`known_hosts`’ your **Ansible control machine** may prompt for confirmation of the key, which results in an interactive experience.

If you understand the implications and wish to disable this behavior, you can do so by editing **/etc/ansible/ansible.cfg**:

```
[defaults]
host_key_checking = False
```

**Basic SSH Authentication**

You can configure your Ansible playbook to prompt for both the SSH password and the sudo password each time a playbook is run. This can be done by using the `--ask-pass` and `--ask-become-pass` options. These options ensure that no passwords are stored in your configuration files.

**SSH key-based Authentication**

Instead of using password authentication, it is recommended to set up SSH key-based authentication between the **Ansible control machine** and the **target hosts**. This involves generating SSH key pairs, adding the public key to the authorized keys of the target hosts, and configuring Ansible to use the private key for authentication. SSH key-based authentication improves security and eliminates the need for password authentication.

* Generate SSH key on the Ansible Control Server (this is only done once):

```
ssh-keygen
```

You will be prompted to enter a file name to save the key pair.

**Press Enter to use the default value**.

* From the Ansible Control server copy the key to the target host with the user created in the previous step:

```
ssh-copy-id ansible@10.147.90.21
```

Repeat this step for each target host.

> **Password changes on the targets will not have an affect on the authentication using the SSH keys.**

### Basic Example: <a href="#id-8724" id="id-8724"></a>

* Edit the `hosts` file (or create a new one) to add your target machines:

```
nano /etc/ansible/hosts
```

example:

```
[linux]
10.1XX.XX.XX ansible_user=ansible
10.1XX.XX.XX ansible_user=ritaj
```

> \[linux] is the group name (choose whatever name you need).

* Run a ping test on the target machines:

```
ansible -i /etc/ansible/hosts linux -m ping
```

The `-i` option in Ansible is used to specify the inventory file that contains the list of target hosts on which the playbook will be executed.

The `-m` option specifies the Ansible module to use, which is “ping” in this case.

**Debug Module:**

In Ansible, the debug module provides two parameters that can be used to display information: `msg` and `var`. Here’s the difference between them:

`msg` is used to display a simple message as the output of the debug module. It is useful for displaying static or pre-defined messages.

**Example:**

```
name: Display a message
debug:
  msg: "This is a static message"
```

In this case, the debug module will display the message “This is a static message” as the output.

`var` is used to display the value of a **variable** or attribute as the output of the debug module. It allows you to display dynamic values during playbook execution.

**Example:**

```
name: Display a variable value
vars:
  my_variable: "Hello, World!"
debug:
  var: my_variable
```

In this case, the debug module will display “**Hello, World!**”.

### Playbook Example1: <a href="#id-769f" id="id-769f"></a>

A playbook that checks the hostname of the target machines:

* Create a playbook file:

```
nano playbook.yml
```

* Paste the following:

```
---
- name: hostname check Playbook
  hosts: linux
  become: yes
  tasks:
    - name: Execute a command
      command: hostname
      register: command_output
    - name: Display command output
      debug:
       var: command_output.stdout
```

> **Note:** The spaces in the playbook are necessary for YAML structure, otherwise there will be a syntax error.

You can run a syntax check using the following command:

```
ansible-playbook --syntax-check -i hosts playbook.yml
```

The `— — —` indicates the start of a `YAML` document.

`name:` is a descriptive name given to the playbook.

`hosts:` specifies that the playbook should be applied to the `linux` group.

`Become: yes` is used for running the command with **Sudo** privilege.

`tasks:` indicates the start of the tasks section, where individual tasks are defined.

The `register` keyword allows you to store the result of the module’s execution into a variable that you can later reference in the playbook.

The `command_output` variable is created using the `register` keyword. (Note: You can choose any other name for the command\_output variable.)

`debug:` a module used to display information during playbook execution.

By accessing the `stdout` attribute of the `command_output` variable, you can retrieve the output generated by the “hostname” command.

* Run the playbook:

```
ansible-playbook -i hosts playbook.yml
```

> The `hosts` file and `playbook.yml` are both in my cwd.

**Notes about this playbook:**

> **(var: command\_output.stdout)** will only display the standard output (stdout) of the command stored in the command\_output variable. It will exclude any error output or any other information that might be captured in the command\_output variable.
>
> **(var: command\_output)** will display the entire content of the command\_output variable, including the stdout as well as other information like standard error (stderr), return code, command used, etc. It provides more detailed information about the command execution.

### Playbook Example2: <a href="#id-4afe" id="id-4afe"></a>

Another basic playbook for creating a file:

```
---
- name: Basic RHEL Playbook
  hosts: linux
  become: False
  tasks:
    - name: Execute a command
      command: touch ansible-test.txt
```

### Playbook Example3: <a href="#d69d" id="d69d"></a>

A playbook to only check if the specified packages are installed and reports back without installing them.

In this playbook, the `{{ }}` syntax allows for dynamic substitution of values or variables in the playbook. In this case, it is used to dynamically insert package names into the `rpm -q` command and to access the output of the command for each package.

```
---
- name: Check if listed package is installed or not on Red Hat Linux
  hosts: linux
  become: true

  vars:
    package_names:
      - sssd
      - realmd
      - oddjob
      - oddjob-mkhomedir
      - adcli
      - samba-common
      - samba-common-tools
      - krb5-workstation
      - openldap-clients

  tasks:
    - name: Check if packages are installed
      command: rpm -q "{{ item }}"
      loop: "{{ package_names }}"
      register: command_output
      changed_when: false

    - name: Display command output
      debug:
        msg: "{{ item.stdout }}"
      loop: "{{ command_output.results }}" #has to be a list
      loop_control:
        label: "{{ item.item }}"
```

The result of each iteration is stored in the `command_output` variable using the `register` directive.

The (`changed_when: false`) directive ensures that the task is always marked as “not changed” in the output.

> In this playbook, the purpose of the task is to check if packages are installed, not to install or update them. Therefore, even if the package is already installed, the task should be considered as “not changed” because it did not modify the system.

The `debug` module is used to display the output of the previous command for each package.

`item` is a default variable in Ansible. It is commonly used in looping constructs.

The `item.stdout` variable represents the standard output of the command.

The loop directive iterates over the `command_output.results` list, which contains the results of the previous task’s iterations.

The `loop_control` directive is used to label each iteration with the corresponding package name using `item.item`.

`label:` The label attribute within `loop_control` is used to set a custom label for each iteration of the loop. It can be useful for identifying or referencing a specific iteration.

### Playbook Example 4: <a href="#id-9ffb" id="id-9ffb"></a>

The following example shows a simpler output, It shows what servers have all packages installed and which do not:

```
---
- name: Check package installation
  hosts: linux
  become: true
  vars:
    package_names:
      - sssd
      - realmd
      - oddjob
      - oddjob-mkhomedir
      - adcli
      - samba-common
      - samba-common-tools
      - krb5-workstation
      - openldap-clients
  tasks:

    - name: "Check if listed package is installed or not"
      command: rpm -q "{{ item }}"
      loop: "{{ package_names }}"
      register: package_check
      changed_when: false
 
    - name: "Print execution results"
      debug:
        msg: "Packages are installed"
      when: package_check is succeeded
```

If the package is installed, The `rpm -q` the command will return the package version information. If the package is not installed, the command will return _**an error message**_.

The `package_check` variable will contain the output of the command execution, including the command’s return code and the standard output.

In the `when` condition, If the task was successful, it means that the package is installed for each item in the `package_names` list. Therefore, the debug task with the message “Packages are installed” will be executed (and If the task failed, the debug task will be skipped).

### Playbook Example 5: <a href="#b935" id="b935"></a>

The following playbook **Installs the missing packages**:

```
---
- name: Installing missing packages
  hosts: linux
  become: true
  tasks:
    - name: Install required packages
      yum:
        name: "{{ item }}"
        state: present
      loop:
        - libnetapi 
        - samba-dcerpc #already exists for testing
        - openldap-clients #already exists for testing
      register: package_installation

    - name: Display status of package installation
      debug:
        msg: "{{ item.item }} is successfully installed"
      loop: "{{ package_installation.results }}"
      when: package.changed
      loop_control:
        label: "{{ item.item }}"
```

The `yum` task installs the required packages using the `loop` directive.

The `state` parameter is set to `present`, indicating that the package should be present on the system.

The `item.item` expression is used to access the value of the item variable within each iteration of the loop.

In this case, the `item.item` expression is used to access the value of the item variable, which represents the **name of the package** being installed.

`package_installation.results`: This refers to the results attribute of the `package_installation` variable.

`loop: “{{ package_installation.results }}”`: This line specifies that the loop should iterate over the elements of the `package_installation.results` list.

The line `when: package.changed` sets a condition for the loop to execute the debug task. It specifies that the debug task will only be executed if the package being iterated over in the loop has changed.

### Playbook Example 6: <a href="#id-3a40" id="id-3a40"></a>

Restart the **SSH** Service:

```
- name: Restart the sshd Service
  hosts: linux
  become: true
  tasks:

- name: Restart sshd service
      service:
        name: sshd
        state: restarted
```

The `state` parameter is set to `restarted`, indicating that the task should ensure that the SSH service is in the “restarted” state. If the service is already running, it will be stopped and then started again, effectively restarting it.

The state parameter of the service module in Ansible can take several different values to manage the state of a service. Here are some of the commonly used states:

**`started`**: Ensures that the service is running. If the service is already running, no action is taken. If the service is stopped, it will be started.

**`stopped`**: Ensures that the service is stopped. If the service is already stopped, no action is taken.

**`restarted`**: Ensures that the service is restarted. If the service is running, it will be stopped and then started again. If the service is stopped, it will be started.

**`reloaded`**: Reloads the configuration of the service. This is useful when you want to apply changes to the service’s configuration without stopping or starting the service itself.

**`enabled`**: Ensures that the service is set to start automatically at system boot. This state will enable the service if it is currently disabled.

**`disabled`**: Ensures that the service is set to not start automatically at system boot. This state will disable the service if it is currently enabled.

**`masked`**: Masks the service, preventing it from starting or being managed by the service manager. This state is typically used to prevent a service from starting accidentally or to mark it as permanently disabled.

### Playbook Example7: <a href="#a2ce" id="a2ce"></a>

A playbook to create a user (if not already created):

```
---
- name: Create users and add to wheel group
  hosts: all
  become: yes

  vars_prompt:
    - name: "ritaj_password"
      prompt: "Enter password for the ritaj user"
      private: yes

  tasks:
    - name: Ensure user ritaj exists
      user:
        name: ritaj
        password: "{{ ritaj_password | password_hash('sha512', 'passlib') }}"
        state: present
      when: not ritaj_user_exists.stat.exists

    - name: Add user ritaj to the wheel group
      user:
        name: ritaj
        groups: wheel
        append: yes
      when: not ritaj_user_exists.stat.exists

  pre_tasks:
    - name: Check if ritaj user exists
      stat:
        path: /home/ritaj
      register: ritaj_user_exists
```

### Run the playbook without making Changes <a href="#id-396f" id="id-396f"></a>

You can run the playbook to see what changes would be made to the server without having to perform the actual changes.

To do that, you just have to add — check — diff to your ansible-playbook startup command

```
ansible-playbook -i  hosts playbook.yml --check --diff
```
