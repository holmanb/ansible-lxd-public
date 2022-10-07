Ansible LXD Example
===================

This repository exists to demonstrate using cloud-init, lxd, and ansible
together. These playbooks may be executed directly, or can be sourced from a
repository using cloud-init's ansible module (this repo exist to test that
functionality).

start.yml
---------
This shows how to start a lxd container using ansible. It shows
how to pass a user-data config to cloud-init before starting the container.
It also writes out an inventory file for the started container.


configure.yml
-------------

This playbook demonstrates connecting to a started lxd container and
configuring it.


Example Usage:
---------------

The first playbook adds the new container to /etc/hosts, and therefore
requires privilege escalation.

```ansible
> sudo ansible-playbook start-lxd.yml
[sudo] password for holmanb: 
[WARNING]: Unable to parse /home/ansible/my-repo/hosts as an inventory source
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [localhost] **************************************************************************************************************************************************************************************************

TASK [Create a started lxd container with a user for ansible] *****************************************************************************************************************************************************
ok: [localhost]

TASK [Print container address for debugging] **********************************************************************************************************************************************************************
ok: [localhost] => {
    "addresses.addresses.eth0": [
        "10.161.80.190"
    ]
}

TASK [Add lxd-container-00 to /etc/hosts file] ********************************************************************************************************************************************************************
ok: [localhost]

TASK [Create a new ansible inventory file (yaml format) at [/home/holmanb/workspace/ansible-lxd]] *****************************************************************************************************************
ok: [localhost]

PLAY RECAP ********************************************************************************************************************************************************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   



> ansible-playbook -i new_ansible_hosts configure-lxd.yml 

PLAY [all] ********************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************
[WARNING]: The "ansible_collections.community.general.plugins.connection.lxd" connection plugin has an improperly configured remote target value, forcing "inventory_hostname" templated value instead of the
string
ok: [lxd-container-00]

TASK [get the username running the deploy] ************************************************************************************************************************************************************************
changed: [lxd-container-00 -> localhost]

TASK [get the privileged user running the deploy] *****************************************************************************************************************************************************************
changed: [lxd-container-00 -> localhost]

TASK [Connect to started lxd container, write a single file as ansible user] **************************************************************************************************************************************
[WARNING]: The "ansible_collections.community.general.plugins.connection.lxd" connection plugin has an improperly configured remote target value, forcing "inventory_hostname" templated value instead of the
string
changed: [lxd-container-00]

TASK [Connect to started lxd container, write a single file as root] **********************************************************************************************************************************************
[WARNING]: The "ansible_collections.community.general.plugins.connection.lxd" connection plugin has an improperly configured remote target value, forcing "inventory_hostname" templated value instead of the
string
changed: [lxd-container-00]

PLAY RECAP ********************************************************************************************************************************************************************************************************
lxd-container-00           : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
