# Ansible-Notes 
## Core Concepts
`IaC`: Execution and management of configuration in ansible uses code and automation. On top of the ability to automate these management tasks, IaC also enables employing various code development best practices and designs such as reusability, shareability, modularization of ansible logics, and organization pattern of various configuration files in the directory.\
`Push-Based`: Compared to other configuration tools like Puppet or Chef, Ansible uses the push-based model, wherein the master host will push the configurations to the target/slave hosts. This is a better model compared to pull-based model wherein an admin needs to login to each slave host and pull configurations to the master host. \
`Agentless `: Ansible does not require installation of software in its managed hosts, since it only connects to host using SSH. This also makes Ansible very lightweight, as it does not need to be installed on managed hosts.\
`Idempotent`: In the context of Ansible, idempotent refers to its property of maintaining the state of the host. This means that when a package manger like apt-get is updated, any succeeding update of apt-get will not get executed as Ansible has already recognized that the apt-get is updated. This can be beneficial in cases where multiple execution of the same task can cause issue to the host, and reducing execution time if some states are already fulfilled.


## Key Components:

`Playbooks`:  The main component of Ansible that declares the tasks and steps that will be executed on the managed machines.
`Plays`: groups of tasks that are executed on a set of hosts.

`Modules`: Modules are specific individual tasks that can be executed on managed machines. They are not to be confused with playbooks as playbooks define the whole execution, while modules define the logic of the steps in the execution. Ansible also allows customization and creation of modules using scripts, python, or any language that can return JSON.

`Inventories`: List of managed machines that will be set for playbook execution. Target machines can be defined using a username, ip address, and a key for SSH-ing into the target machine. You can also group the machine identifiers, so a playbook can be executed on the group of managed machines. 

`Roles`: Can recognize various configuration files such as html, variable declaration, yaml, ini, and many more.

`Handlers`: A task that can be performed or executed if another task triggered the handler. Some example use case of this is when a service needs to be restarted, or when you want to send the logs of the execution, you can invoke the handler to perform specified tasks.

## How it works:
There are many ways to use Ansible in automating infrastructure management and configuration. For this note, we will discuss about executing a playbook to a group of hosts called 'linux' group, and incorporate the concepts of roles and handlers. 

1. Ansible executes commands on target node by starting an SSH connection to target hosts. To do this, we must indicate in a file the target ip address, as well as the path in the master node where the private key is located.
> For better organization. The list of key paths and ip addresses are defined in its own file called inventory.ini

2. Once target host details are defined, we can now proceed on creating a playbook to describe the commands to execute in the target nodes. Lets look below at some sample playbook file.
``` yaml
---
- name: Example Playbook with Roles and Handlers
  hosts: linux

  roles:
    - webserver

  tasks:
    - name: Ensure services are running
      service:
        name: apache2
        state: started
        enabled: yes
        notify:
        - Notify Slack

  handlers:
    - name: Notify Slack
      slack:
        channel: "#devops"
        msg: "apache2 service started on {{ ansible_hostname}}".
```

  - The playbook will run on all hosts under the 'linux' group
  - The **webserver** role is included which references a folder called **webserver**. Roles are used to organize various tasks, and other aspects of a playbook, to make each aspect modular and reusable.
  - Under task **Ensure services are running**, commands will be executed to ensure that apache2 is enabled and that the service is started. 
  - The handlers **Notify Slack** is invoked under task in "notify: \n - Notify Slack". When invoked, all steps and actions under the handler name will be executed. For this example, the handler will send a message to Slack when the apache service has been started.


3. Once the playbook is successfully configured, we can now execute the playbook. To do so, go to a shell terminal and type in 'ansible-playbook -I inventory.ini <<playbook-name.yml>>'. Add options to the command as necessary.

``` bash
ansible-playbook -I /path/to/inventory.ini <playbook-name.yml>
```


## Limitations of Ansible
Like any other technology or tools, Ansible also has some challenges and limitations in using. Here are some of the cons of Ansible.
- Limited Built - in Error Handling: Compared to other tools in the frameworks, Ansible's error handling is somewhat lackluster or limited, making it hard to debug or learn the tool initially.
- Scalability challenges: Ansible is known to get increasingly more complex as the managed infrastructure grows and grows to thousand hosts. This again makes it more challenging to orchestrate and trace each individual aspect and files of the IaC.
- Dependency on SSH: Ansible relies very heavily on SSH to connect and push to managed hosts, making Ansible inherit the limitations and concerns on using SSH. Some of these are the security vulnerabilities of SSH, as well as performance issues specifically when master host needs to push the configuration to large number of hosts.


## Summary
While Ansible is a fairly new and is not a perfect configuration tool, it is still widely adopted for its convenience to use and learn, relatively fast performance, and ability to automate configuration tasks. As time passes on it will continue to improve and be a valuable tool in the DevOps field. As with any tool, great understanding of its strengths and weaknesses and assessing its relevance to the goal can enable effective utilization and automation of diverse IT infrastructures.
