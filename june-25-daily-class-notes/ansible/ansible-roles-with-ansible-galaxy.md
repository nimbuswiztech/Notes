# Ansible roles with ansible galaxy

**Ansible Roles** are a way to organize and structure playbooks by breaking them down into reusable, modular components. They help in managing complex automation tasks and make your code more maintainable by dividing it into smaller, focused pieces. Each role performs a specific function, such as installing and configuring a service (e.g., setting up a web server, database, or firewall).

Benefits:-

* _**Reusability**_: Roles can be reused across multiple playbooks and projects.
* _**Modularity**_: Each role is designed to do one thing (like configuring a web server) and can be combined with other roles.
* _**Separation of Concerns**_: Roles allow you to separate different responsibilities (e.g., database management, web server setup) in your playbooks.
* _**Scalability**_: Roles make managing large environments easier by organizing and reusing configuration across different servers.

In a real-world use case, we generally do not have 2–3 tasks only. One playbook may have to perform 50+ tasks. With this increased set of tasks, the size of the playbook increases and the readability and modularity of the playbook is hampered. To address these issues, _roles_ have been introduced. Additionally, these roles can also be shared. Similar to dockerhub for docker images, ansible has ‘ansible-galaxy’ as a registry to store roles that can be shared across an organization.

> _ansible-galaxy role init \<role\_name>_

Consider a playbook as below:-

```
# Playbook to install apache httpd and copy a static website file (index.html) from local to the managed node(s).

---
- hosts: all
  become: true
  tasks:
    - name: Install apache httpd
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: yes
    - name: Copy file with owner and permissions
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html
        owner: root
        group: root
        mode: '0644'
```

Now, if we execute the command to initialize ansible-roles, after the execution, ansible creates a folder with the role\_name, which has different files namely: vars, tasks, meta, handlers, tests, defaults, templates etc.

**Use Case**: _Deploying and Managing a LAMP Stack (_&#x4C;inux, Apache, MySQL, PH&#x50;_) using Ansible Galaxy Roles on a group of web-servers_

Necessary roles:

> apache\
> mysql\
> php

**Download Existing Roles from Ansible Galaxy**: Ansible Galaxy is a public repository of roles. It allows you to reuse community-contributed roles, speeding up development and reducing maintenance. The following commands will install the roles into the default roles directory (`~/.ansible/roles/`), which you can later use in your playbooks.

```
ansible-galaxy install geerlingguy.apache
ansible-galaxy install geerlingguy.mysql
ansible-galaxy install geerlingguy.php
```

Define inventory file

```
[webservers]
user@web1.example.com  # can be replaced by IP address
user@web2.example.com  # cab be replaced by IP address
```

**The main playbook —** references the roles

```
# setupWebServers.yml
---
- hosts: all
  become: true
  roles:
    - geerlingguy.apache
    - geerlingguy.mysql
    - geerlingguy.php
```

**Define the roles**

```
---
- hosts: webservers
  become: yes
  roles:
    - role: geerlingguy.apache
      vars:
        apache_listen_port: 8080    # Example: customize Apache to run on port 8080
        apache_vhosts:
          - servername: "www.example.com"
            documentroot: "/var/www/html"
    - role: geerlingguy.php
      vars:
        php_version: "7.4"          # Specify the PHP version to be installed
    - role: geerlingguy.mysql
      vars:
        mysql_root_password: "securepassword"
        mysql_databases:
          - name: mydb
            encoding: utf8mb4
        mysql_users:
          - name: myuser
            password: "mypassword"
            priv: "mydb.*:ALL"
```

Run the playbook

> ansible-playbook setupWebServers.yml

**Push custom roles to&#x20;**_**ansible-galaxy**_

To push a role that you have created on to ansible-galaxy, you have to first push it to a github repository which will then be imported to ansible-galaxy.

Push to a github repository:-

```
cd <role-name>
git init
git remote add origin <https://github.com/your_github_username/my_role.git>
git add .
git commit -m "Initial commit"
git push -u origin main
```

Import the role on ansible-galaxy

To import the role, ansible-galaxy will ask you to provide a token. For this, goto _ansible-galaxy collections -> API token -> Create_

```
ansible-galaxy role import <your_github_username> <role-name> --token 554fefkernekg544efne3k1n21k3
```

Under the created role folder, we have a _**meta**_ folder as well that ansible had created after executing the command : _ansible-galaxy role init \<role\_name>_

It is very important to update the file in this folder, so that your organization members can know who has created the role and its descriptions.
