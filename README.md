# RHEL8 OpenStack Ansible

This project uses Ansible to setup an OpenStack cluster with the services listed below. 

- Core services

    - **Keystone** - for identity management
    - **Glance** - for images
    - **Nova and Placement** - for instances
    - **Neutron** - for networking
- Optional services

    - **Swift** - for object storage
    - **Horizon** - OpenStack UI

This [project for RHEL8 Kickstart](https://github.com/baroxx/rhel8-kickstart) provides kickstart files to automate the installation of the nodes. There are also scripts to create some virtual machines.

# Prerequisites

The following packages are required on RHEL8:

- **ansible-collection-ansible-posix** for SELinux updated
- **ansible-collection-community-mysql** for database queries
- **ansible-collections-openstack** for project, service, endpoint, user and role management

There should be at least one controll node and two compute nodes (three object nodes). Each target must be accessible via SSH and requires passwordless sudo rights.

Swift uses directories on the object nodes as storage. The [roles/role for the rings](roles/swift_rings) creates rings for all storage devices set in [vars/config.yml](vars/config.yml) (object_storage). Each object node can have multiple storage devices. **The mount point for these storage devices must be at /srv/node/**

# Prepare and Run

**Preperation:**

1. Install [required ansible packages](#prerequisites) on the local machine
1. Prepare passwordless SSH and sudo on all nodes
1. Swift: Prepare mount points for storage devices on object nodes **(only for Swift)** 
1. Select the [roles](#roles) in [vars/service-selection.yml](vars/service-selection.yml)
1. Rename [vars/secrets-tmp.yml](vars/secrets-tmp.yml) to vars/secrets.yml and enter your secrets
1. Update the variables in [vars/config.yml](vars/config.yml) (at the moment only a setup with a singe controller node is supported)
1. Set the IPs in [inventory](inventory)

**Run:**

 ```
ansible-playbook openstack.yml
 ```

# Roles

You can find more information in the README of the roles. You should install at least the core services mentioned at the top of this README. 

**It is recommended to use the roles in this order.**

- [Preparation](prepare)
- [Misc Services](misc)
- [keystone](keystone)
- [Glance](glance)
- Nova

    - [Controller](nova_controll)
    - [Compute node](nova_compute)
- Neutron

    - [Controller](neutron_controll)
    - [Compute node](neutron_compute)
- Swift

    - [Proxy server](swift_proxy)
    - [Object nodes](swift_object)
    - [Rings](swift_rings)
    - [All nodes](swift_all_nodes)
- [Horizon](Horizon)

# OpenStack module

This project uses the [OpenStack module](https://docs.ansible.com/ansible/latest/collections/openstack/cloud/index.html)

# Copyright

The config files in this repository are adjusted files based on the default configs on RHEL8 with RDO. [local_settings](roles/horizon/templates/local_settings.j2) is provided by OpenStack and adjusted in this project.