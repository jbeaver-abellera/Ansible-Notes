<p align="center">
<img src="https://github.com/jbeaver-abellera/Ansible-Notes/assets/108796284/09e00dd3-2d0f-4d20-a547-300933e2284a" height=250>
</p>

# Ansible-Notes
Ansible is a software tool that enables Infrastructure as Code processing. It is an open source tool that aims to automate configuration tasks, deploy complex software architecture, all while using simple YAML syntax. You can start using ansible by defining a target host machine, and creating a playbook that will execute configurations on the target host.

## Core Concepts:
`IaC`: Execution and management of configuration in ansible uses code and automation. On top of the ability to automate these management tasks, IaC also enables employing various code development best practices and designs such as reusability, shareability, and packaging of files such as using Roles in Ansible to link yml files in different subdirectories.\
`Push - Based`: Unlike other infrastructure management tools like Puppet or Chef, Ansible operates on a push - based model. This means that the master node pushes the configuration down to the slave nodes. This is much more convenient and less prone to errors compared to pull - based models wherein slave nodes will pull configurations specified in the master node.\

one more pharagraph?

![Untitled Diagram drawio](https://github.com/jbeaver-abellera/Ansible-Notes/assets/108796284/9dfdfc89-1b21-4178-a379-3bc9af42ab3a)

## Key Components:

`Playbooks`:  The main component of Ansible that declares the play. The play then defines the tasks and steps that will be executed on the managed machines. Some of the most important parts of a play are the play name, hosts, tasks, handlers, roles, files, and many more.

`Modules`: Modules are specific individual tasks that can be executed on managed machines. They are not to be confused with playbooks as playbooks define the whole execution, while modules define the logic of the steps in the execution. Ansible also allows customization and creation of modules using scripts, python, or any language that can return JSON.

`Inventories`: List of managed machines that will be set for playbook execution. Target machines can be defined using a username, ip address, and a key for SSH-ing into the target machine. You can also group the machine identifiers, and then specify in the playbook which group/s or host/s the playbook will be executed on.

`Roles`: Helps in organizing your playbooks or configuration by referencing a folder where all configurations exist. Using the keyword **Roles: <role-directory/>**, all files in the role-directory will be read and interpreted by Ansible as configurations. Inside the role-directory, you can have various subdirectories as well to separate each configuraations, such as having a vars/ subdirectory for storing all your vars yaml files. 

`Handlers`:  A task that can be performed or executed if another task triggered the handler. Some example use case of this is when a service or configuration needs to be reloaded or restarted.
