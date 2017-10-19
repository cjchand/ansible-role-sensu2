Vagrant README
==============

Included with this repo is a simple Vagrant setup. This readme covers the absolute basics of how to use this Vagrant deployment. Experience with Vagrant is assumed (though some 101-level Vagrant commands are included, just in case).

Topology Overview
=================

The topology is simple:

* 1x server (AKA: backend)
* 1x agent

The sensu-agent process is installed and deployed on both servers, meaning when all is working well, you will see both of them via sensuctl on `sensu-server-01`:

```
[root@sensu-server-01 vagrant]# sensuctl entity list
        ID           OS     Subscriptions             Last Seen
 ───────────────── ─────── ─────────────── ───────────────────────────────
  sensu-agent-01    linux                   2017-10-19 17:51:37 +0000 UTC
  sensu-server-01   linux                   2017-10-19 17:51:51 +0000 UTC
```

Selecting an OS
===============

While defaulted to CentOS 7, there are commented entries in the `Vagrantfile` for Debian and Ubuntu starting on line 12:
```
box_name = "centos/7"
# box_name = "debian/jessie64"
# box_name = "ubuntu/trusty64"
```

Simply uncomment the OS you wish to use.

**Note**: If you have already deployed Vagrant VMs, make sure to use `vagrant destroy` to get gid of those prior to making this change. Vagrant can get confused if you change things in `Vagrantfile` or `vagrant-hosts.yml` while it has existing VMs.


Deploying VMs via Vagrant
=========================

To deploy the VMs, ensure you are connected to the Internet, then

1. `cd` to the root of this repo
2. `vagrant up`

This will stand up two VMs with hostnames `sensu-server-01` and `sensu-agent-01`, after which the Ansible playbook will run against them both.

Accessing the VMs
=================

To ssh into the VMs, ensure you are in the root of this repo, then type `vagrant ssh <hostname>` where `<hostname>` is either `sensu-server-01` or `sensu-agent-01`. 

At this point, you will be logged in as the `vagrant` user. If you need to start/stop services, you will likely want to become root by typing `sudo su`. 

Also, `sensuctl` maintains config on a per-user basis. Since the Ansible playbook configures `sensuctl` as the root user, you will want to use `sudo` before using `sensuctl`. 

Re-running Ansible
==================

If you need to re-run the Ansible playbook (e.g.: after changing a variable), type `vagrant provision`.

Starting Over (AKA: Nuke-n-Pave)
================================

If you'd like to start again from scratch, type: `vagrant destroy` and answer `y` to any of the VM's you want to destroy. From there, you can type `vagrant up` again.

Further Reading
===============

For details on how to use Sensu 2.0, refer to the [documentation on GitHub](https://github.com/sensu/sensu-alpha-documentation).