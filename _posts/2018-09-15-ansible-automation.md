---
layout: post
title: "Ansible: Infrastructure Automation Made Easy"
date: 2018-09-05 10:00:00 +0000
categories: [ansible, automation, devops, infrastructure]
author: Amal
image: /assets/images/posts/2018-09-15-ansible-automation.svg
---



# Ansible: Infrastructure Automation Made Easy

## Introduction

Ansible is an open-source automation platform for configuration management, application deployment, and orchestration. It's agentless and uses SSH for communication.

## Part 1: Ansible Basics

### Installation

```bash
# Install Ansible
sudo apt-get install ansible

# Verify installation
ansible --version
```

### Inventory

```
# /etc/ansible/hosts or custom inventory file

[webservers]
web1.example.com
web2.example.com
web3.example.com

[databases]
db1.example.com
db2.example.com

[all:vars]
ansible_user=ubuntu
ansible_private_key_file=~/.ssh/id_rsa
```

### Basic Ansible Commands

```bash
# Ping all hosts
ansible all -m ping

# Execute command
ansible webservers -m command -a "uptime"

# Execute shell command
ansible webservers -m shell -a "df -h | grep sda"

# Copy file
ansible webservers -m copy -a "src=/local/file dest=/remote/file"

# Install package
ansible webservers -m apt -a "name=nginx state=present"
```

## Part 2: Playbooks

### Basic Playbook

```yaml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  tasks:
    - name: Update package list
      apt:
        update_cache: yes
    
    - name: Install Nginx
      apt:
        name: nginx
        state: present
    
    - name: Start Nginx
      service:
        name: nginx
        state: started
        enabled: yes
    
    - name: Copy configuration
      copy:
        src: /local/nginx.conf
        dest: /etc/nginx/nginx.conf
      notify: restart nginx
  
  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

### Playbook with Variables

```yaml
---
- name: Deploy application
  hosts: webservers
  vars:
    app_name: myapp
    app_version: 1.0.0
    app_port: 8080
  
  tasks:
    - name: Create app directory
      file:
        path: "/opt/{{ app_name }}"
        state: directory
    
    - name: Download application
      get_url:
        url: "https://releases.example.com/{{ app_name }}-{{ app_version }}.tar.gz"
        dest: "/opt/{{ app_name }}/app.tar.gz"
    
    - name: Extract application
      unarchive:
        src: "/opt/{{ app_name }}/app.tar.gz"
        dest: "/opt/{{ app_name }}"
        remote_src: yes
    
    - name: Start application
      systemd:
        name: "{{ app_name }}"
        state: started
        enabled: yes
```

### Conditional Playbooks

```yaml
---
- name: Conditional tasks
  hosts: all
  tasks:
    - name: Install MySQL on databases
      apt:
        name: mysql-server
        state: present
      when: inventory_hostname in groups['databases']
    
    - name: Install Nginx on webservers
      apt:
        name: nginx
        state: present
      when: '"webservers" in group_names'
    
    - name: Check OS version
      debug:
        msg: "Ubuntu {{ ansible_distribution_version }}"
      when: ansible_distribution == "Ubuntu"
```

## Part 3: Roles

### Role Structure

```
roles/
├── web/
│   ├── tasks/
│   │   └── main.yml
│   ├── handlers/
│   │   └── main.yml
│   ├── templates/
│   │   └── nginx.conf.j2
│   ├── files/
│   ├── vars/
│   │   └── main.yml
│   └── defaults/
│       └── main.yml
└── db/
    ├── tasks/
    │   └── main.yml
    └── vars/
        └── main.yml
```

### Using Roles

```yaml
---
- name: Configure servers
  hosts: all
  roles:
    - web
    - db
    - monitoring
```

## Part 4: Advanced Ansible

### Loops

```yaml
- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - mysql-server
    - git
    - curl

- name: Create users
  user:
    name: "{{ item.name }}"
    shell: "{{ item.shell }}"
  loop:
    - { name: 'alice', shell: '/bin/bash' }
    - { name: 'bob', shell: '/bin/bash' }
```

### Templates (Jinja2)

```
# templates/nginx.conf.j2
server {
    listen {{ nginx_port }};
    server_name {{ server_name }};
    
    location / {
        proxy_pass http://{{ app_server }};
    }
}
```

```yaml
- name: Deploy Nginx config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/sites-available/default
  vars:
    nginx_port: 80
    server_name: example.com
    app_server: localhost:8080
```

### Error Handling

```yaml
- name: Deploy with error handling
  block:
    - name: Stop application
      systemd:
        name: myapp
        state: stopped
    
    - name: Deploy new version
      synchronize:
        src: /local/app/
        dest: /opt/myapp/
    
    - name: Start application
      systemd:
        name: myapp
        state: started
  
  rescue:
    - name: Rollback deployment
      synchronize:
        src: /backup/app/
        dest: /opt/myapp/
    
    - name: Start old version
      systemd:
        name: myapp
        state: started
  
  always:
    - name: Send notification
      mail:
        host: smtp.example.com
        subject: "Deployment completed"
```

## Summary

Ansible provides:
- Simple syntax
- Agentless architecture
- Powerful orchestration
- Infrastructure as Code

Master Ansible for modern infrastructure automation!
