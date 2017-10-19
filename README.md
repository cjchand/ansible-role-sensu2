Role Name
=========

This role deploys Sensu 2.x to the currently supported OS' (see Requirements section).

**Note**: Sensu 2.0 is currently in Alpha. This Ansible role requires a valid repo token, which is handed out by the Sensu team. To apply for the Alpha, visit [https://sensuapp.org/alpha]().

Requirements
------------

Sensu 2.x is only supported on CentOS/RHEL and Ubuntu/Debian.

If you wish to use the Vagrant setup, then you need to:

1. [Install Vagrant](https://www.vagrantup.com/docs/installation/)
2. Install the `hostmanager` plugin for Vagrant by running: `vagrant plugin install vagrant-hostmanager` 

Refer to the [Vagrant README](./docs/vagrant-README.md) for more details.

Role Variables
--------------

Role variables are split into three groups:

1. Global
2. Sensu Agent-specific
3. Sensu Backend-specific

One variable you **must** set is the `sensu_repo_token` variable. Many of these can be left as-is for a typical deployment, but the ones you will most likely want to change are...

### Global Variables

* `remote_user`: Name of the user that Ansible should use to perform the tasks in this role
* `sensu_repo_token`: The repo token provided to you by the Sensu team. If you are not in the Sensu 2.x alpha, visit [https://sensuapp.org/alpha]() to apply.  
* `sensu_organization`: The organization to use when configuring sensuctl and agent. (Default: "default")
* `sensu_environment`: The environment to use when configuring sensuctl and agent. (Default: "default")

### Agent Variables

* `user`: User to use when connecting to the agent API (Default: "agent")
* `password`: Password to use when connecting to the agent API (Default: "P@ssw0rd!")

### Backend Variables

* `user`: User to use when connecting to the backend's API (Default: "admin")
* `password`: Password to use when connecting to the backend's API (Default: "P@ssw0rd!")
* `agent_host`: IP address to listen for agent connections on (Defaults to all IPv4 and IPv6 addresses)
* `agent_port`: Port to listen for agent connections on (Default: 8081)
* `api_host`: IP address to listen for agent connections on (Defaults to all IPv4 and IPv6 addresses)
* `api_port`: Port to listen for agent connections on (Default: 8080)

Dependencies
------------

No dependencies on other Ansible Galaxy roles

Example Playbook
----------------

```
---
    - hosts: all
      roles:
         - { role: ansible-role-sensu2, remote_user: root }

```

License
-------

MIT

Author Information
------------------

Chris Chandler (christopher.chandler18@t-mobile.com, cjchand@gmail.com)
Source: [https://github.com/cjchand/ansible-role-sensu2]()

