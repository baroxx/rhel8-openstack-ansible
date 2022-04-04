# rhel8-openstack-ansible

This project provides roles to setup a OpenStack cluster with the services listed below:

- Core services

    - **Keystone** - for identity management
    - **Glance** - for images
    - **Nova and Placement** - for instances
    - **Neutron** - for networking
- Optional services

    - **Horizon** - OpenStack UI
    - **Swift** - for object storage

Nova and Neutron are splitted into controll and compute nodes. Swift is splitted into controll and object. You can install swift as the object storage on the compute nodes.

# Requirements

The following packages are required on RHEL8:

- **ansible-collection-ansible-posix** for SELinux updated
- **ansible-collection-community-mysql** for database queries
- **ansible-collections-openstack** for project, service, endpoint, user and role management

Each target must be accessible via SSH and requires passwordless sudo rights.

# Preperation

1. Update the variables in [constants.yml](constants.yml)
1. Set the IPs in [inventory](inventory)
1. Select the roles in [openstack.yml](openstack.yml)

# Run

 ```
ansible-playbook openstack.yml -i inventory
 ```

## Swift

There are manual steps if the role for swift is used: You need to copy the account.ring.gz, container.ring.gz, and object.ring.gz files to the /etc/swift directory on each storage node.

# OpenStack module

This project uses the [OpenStack module](https://docs.ansible.com/ansible/latest/collections/openstack/cloud/index.html)

# Copyright

The config files in this repository are adjusted files based on the default configs on RHEL8 with RDO. [local_settings](roles/horizon/templates/local_settings.j2) is provided by OpenStack and adjusted in this project.