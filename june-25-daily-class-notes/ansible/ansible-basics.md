# Ansible Basics

Ansible is an open-source automation tool for IT tasks such as configuration management, application deployment, and orchestration. With its simple, agentless architecture, Ansible has become a popular choice for DevOps teams and system administrators.

In this comprehensive blog, we’ll cover Ansible from its basic concepts to advanced usage, providing commands, examples, and tips to help you master Ansible.

### Table of Contents <a href="#id-90ca" id="id-90ca"></a>

1. [What is Ansible?](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#1)
2. [Key Components of Ansible](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#2)
3. [Setting Up Ansible](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#3)
4. [Basic Ansible Concepts](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#4)
5. [Writing Playbooks](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#5)
6. [Roles in Ansible](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#6)
7. [Ansible Vault: Securing Secrets](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#7)
8. [Advanced Topics in Ansible](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#8)
9. [Ansible Galaxy and Reusable Content](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#9)
10. [Ansible Tower (AWX): Enterprise Automation](https://medium.com/@shivam2003/ansible-a-complete-guide-from-basics-to-advanced-ffd1bf74322f#10)

### 1. What is Ansible? <a href="#id-79e7" id="id-79e7"></a>

Ansible is a powerful automation tool used for:

* **Configuration management**: Automate server setup, manage services, and configurations.
* **Application deployment**: Deploy and manage software applications.
* **Orchestration**: Coordinate complex, multi-step workflows, such as CI/CD pipelines.

Ansible operates by connecting over SSH, and it doesn’t require any agent to be installed on the target nodes. This makes it lightweight and simple to use.

### 2. Key Components of Ansible <a href="#id-44e9" id="id-44e9"></a>

Ansible consists of several key components:

1. **Inventory**: A list of servers (hosts) Ansible manages, which can be in the form of an `inventory` file or dynamic inventory.
2. **Playbook**: A YAML file that defines a series of tasks to be executed on the hosts.
3. **Module**: Small programs that Ansible runs to perform system changes (e.g., managing files, services).
4. **Task**: The individual unit of action in Ansible, such as installing a package or starting a service.
5. **Role**: A way to organize tasks, handlers, and variables in a structured way for reuse and sharing.
6. **Handlers**: Tasks triggered by changes, usually at the end of playbook execution.
7. **Variables**: Store data dynamically, allowing reusability and customization.
8. **Templates**: Generate dynamic files using the Jinja2 templating engine.

### 3. Setting Up Ansible <a href="#id-40a8" id="id-40a8"></a>

#### Installation <a href="#df57" id="df57"></a>

For most systems (e.g., Ubuntu, Debian, CentOS), installing Ansible is straightforward:

On **Ubuntu/Debian**:

```
sudo apt update
sudo apt install ansible -y
```

```
On CentOS/RedHat:
```

```
sudo yum install epel-release -y
sudo yum install ansible -y
```

Verify the installation:

```
ansible --version
```

#### Basic Directory Structure <a href="#id-661b" id="id-661b"></a>

After installing Ansible, the directory structure for your projects might look like this:

```
inventory/       # Contains the list of hosts
playbooks/       # Directory for storing your playbooks
roles/           # Directory for Ansible roles
ansible.cfg      # Configuration file
```

#### Inventory File <a href="#id-152d" id="id-152d"></a>

Create a simple `inventory` file:

```
[web]
web1.example.com
web2.example.com
[db]
db1.example.com
```

### 4. Basic Ansible Concepts <a href="#e813" id="e813"></a>

#### Ad-Hoc Commands <a href="#c2ee" id="c2ee"></a>

Ansible allows you to run commands on remote hosts without writing a playbook. For example, to check connectivity, run:

```
ansible all -i inventory -m ping
```

To install `nginx` on all `web` servers:

```
ansible web -i inventory -m apt -a "name=nginx state=present" --become
```

* **`-i`**: Specifies the inventory file.
* **`-m`**: Specifies the module to use (`ping`, `apt`, etc.).
* **`-a`**: Provides arguments for the module.
* **`--become`**: Executes the task with `sudo` or privilege escalation.

### 5. Writing Playbooks <a href="#df38" id="df38"></a>

A **playbook** is a YAML file containing one or more “plays.” Each play defines a set of tasks to be executed on a set of hosts.

#### Simple Playbook Example <a href="#cc99" id="cc99"></a>

```
---
- name: Install and configure nginx
  hosts: web
  become: true
```

```
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present    - name: Start nginx
      service:
        name: nginx
        state: started
```

This playbook installs and starts NGINX on all `web` hosts defined in the inventory.

#### Task Structure <a href="#b6a9" id="b6a9"></a>

Each task typically includes:

* **name**: A description of the task.
* **module**: The Ansible module to use (`apt`, `service`, etc.).
* **arguments**: Options for the module.

#### Conditionals and Loops <a href="#id-0633" id="id-0633"></a>

You can add conditionals to tasks:

```
- name: Install nginx on Ubuntu
  apt:
    name: nginx
    state: present
  when: ansible_distribution == "Ubuntu"
```

Loops allow you to repeat a task for a list of items:

```
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - curl
    - git
```

### 6. Roles in Ansible <a href="#cc3b" id="cc3b"></a>

**Roles** allow you to organize tasks, variables, and handlers in a reusable structure. Roles are typically stored in the `roles/` directory.

#### Basic Role Structure <a href="#id-9f95" id="id-9f95"></a>

```
roles/
  myrole/
    tasks/
      main.yml
    handlers/
      main.yml
    templates/
    files/
    vars/
      main.yml
```

To create a role:

```
ansible-galaxy init myrole
```

You can call a role in a playbook:

```
---
- hosts: web
  roles:
    - myrole
```

### 7. Ansible Vault: Securing Secrets <a href="#id-3397" id="id-3397"></a>

**Ansible Vault** is used to encrypt sensitive data such as passwords or API keys in your playbooks. This ensures that confidential information isn’t exposed in your version control system.

#### Encrypt a file: <a href="#e8c3" id="e8c3"></a>

```
ansible-vault encrypt secrets.yml
```

#### Decrypt a file: <a href="#de57" id="de57"></a>

```
ansible-vault decrypt secrets.yml
```

#### Edit an encrypted file: <a href="#id-9ddf" id="id-9ddf"></a>

```
ansible-vault edit secrets.yml
```

#### Using Vault in Playbooks: <a href="#id-0ac3" id="id-0ac3"></a>

After creating a `secrets.yml` file, use it in a playbook:

```
---
- hosts: all
  vars_files:
    - secrets.yml
```

```
  tasks:
    - name: Print the secret
      debug:
        msg: "{{ secret_variable }}"
```

To run a playbook with Vault:

```
ansible-playbook playbook.yml --ask-vault-pass
```

This is especially useful when you need to manage sensitive data securely across multiple environments.

### 8. Advanced Topics in Ansible <a href="#id-1fab" id="id-1fab"></a>

#### Dynamic Inventory <a href="#id-9882" id="id-9882"></a>

In some cases, you may not have static IP addresses or hostnames. **Dynamic inventory** allows you to pull inventory data from cloud providers like AWS, GCP, or Azure dynamically.

To use dynamic inventory:

Install the necessary inventory plugin (e.g., AWS EC2):

```
pip install boto boto3
```

Configure the dynamic inventory script (`ec2.py`) in your `ansible.cfg` file:

```
[inventory] enable_plugins = aws_ec2
```

Run playbooks against dynamic inventory:

```
ansible-playbook playbook.yml -i ec2.py
```

#### Delegation and Parallelism <a href="#f701" id="f701"></a>

* **Delegation**: Run a task on one host but apply it to another. For example, creating a user on a web server and also adding it to the load balancer:

```
tasks:   - name: Configure load balancer     command: /usr/local/bin/configure-lb     delegate_to: lb.example.com
```

* **Parallelism**: Control the number of hosts Ansible runs against in parallel using the `-f` option:

```
ansible-playbook playbook.yml -f 10
```

#### Callbacks and Notifications <a href="#efbc" id="efbc"></a>

Ansible allows you to integrate with external systems, sending notifications on task completion (e.g., to Slack or email). Callback plugins make this possible.

Enable a callback plugin in `ansible.cfg`:

```
[defaults]
callbacks_enabled = timer, mail
```

You can also develop custom callback plugins to integrate with other services.

### 9. Ansible Galaxy and Reusable Content <a href="#id-7547" id="id-7547"></a>

**Ansible Galaxy** is the official hub for sharing Ansible roles, making it easier to reuse configurations.

#### Using Galaxy Roles: <a href="#be8d" id="be8d"></a>

Install a role from Ansible Galaxy:

```
ansible-galaxy install geerlingguy.nginx
```

Use the installed role in your playbook:

```
--- - hosts: all   roles:     - geerlingguy.nginx
```

#### Publishing Roles to Galaxy: <a href="#c778" id="c778"></a>

To share a role with the community, follow these steps:

1. Create an account on Ansible Galaxy.
2. Initialize a role:

```
ansible-galaxy init myrole
```

3\. Add metadata and documentation, then publish your role using the GitHub integration.

### 10. Ansible Tower (AWX): Enterprise Automation <a href="#id-2503" id="id-2503"></a>

**Ansible Tower** (or its open-source counterpart **AWX**) is a web-based solution for scaling and managing Ansible automation in enterprise environments.

Key features include:

* **Centralized logging** and job tracking.
* **RBAC (Role-Based Access Control)** to manage who can run what.
* **Playbook scheduling** and **workflow automation**.
* **Graphical interface** for monitoring and managing playbook runs.

#### Setting Up AWX: <a href="#cf48" id="cf48"></a>

AWX can be installed via Docker:

```
git clone https://github.com/ansible/awx.git
cd awx/installer
ansible-playbook -i inventory install.yml
```

After installation, you can access AWX via a browser to manage your Ansible workflows.

### Conclusion <a href="#id-6ce9" id="id-6ce9"></a>

Ansible is a powerful tool for automation, configuration management, and orchestration, and can be easily scaled from small-scale environments to large enterprise systems with tools like Ansible Tower. In this guide, we covered:

* The basics: setting up, writing playbooks, and running tasks.
* Intermediate features: roles, variables, conditionals, and loops.
* Advanced topics: dynamic inventory, Ansible Vault, and callbacks.

Mastering these topics will empower you to automate and manage infrastructure efficiently with Ansible. The journey from basic usage to advanced features offers a clear path to improving your system administration and DevOps practices.
