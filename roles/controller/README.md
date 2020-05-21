# **Ansible Role for Controller Cards**

Ansible Role for Controller Cards is used to retrieve all the Controller Cards data, Switchover Active Controller Card to Standby, Lock Standby Controller Card and to Unlock Standby Controller Card.

## **Pre-requisites and conditions for the Ansible role Controller**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/controller_config.yml) "config/controller_config.yml".

## **User tasks and capabilities**

* Retrieve all the Controller Cards data containing the Redundancy Status for the given Chassis IDs
* Switchover the Active Controller Card to Standby Controller Card for the given Chassis IDs
* Lock the Standby Controller Card for the given Chassis IDs
* Unlock the Standby Controller Card for the given Chassis IDs

## **Supported Tasks**

| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve](#Configuration-variables-for-the-Retrieve-task)|Retrieve all the Controller Cards data containing the Redundancy Status for the given Chassis IDs|retrieve|List of Chassis IDs|
|[switchover](#Configuration-variables-for-the-Switchover-of-Active-Controller-Card-to-Standby-task)|Switchover the Active Controller Card to Standby Controller Card for the given Chassis IDs|switchover|List of Chassis IDs, Optional `force` flag|
|[lock](#Configuration-variables-for-the-Lock-Standby-Controller-Card-task)|Lock the Standby Controller Card for the given Chassis IDs|lock|List of Chassis IDs|
|[unlock](#Configuration-variables-for-the-Unlock-Standby-Controller-Card-task)|Unlock the Standby Controller Card for the given Chassis IDs|unlock|List of Chassis IDs|
|||||

## **Configuration variables for the Retrieve task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|chassis_ids| list| The list of chassis Ids for which the controller card data (containing the redundancy status) is to be retrieved|Valid values for chassis ids are - 1 to 254|Yes|No|
|||||||

### **Sample Configuration file for the Retrieve task**

```yaml
---

controller_configs:
  retrieve:
    chassis_ids:
      - 1
      - 2
      - 3
```

## **Configuration variables for the Switchover of Active Controller Card to Standby task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|chassis_ids| list| The list of chassis Ids for which the Active controller card will be switched to the standby controller card|Valid values for chassis ids are - 1 to 254|Yes|No|
|force|bool|The boolean flag to be passed in order to run the task. To run the task, pass the 'force' option and set it to 'True' while executing the playbook command. |<ul><li>True</li><li>False</li></ul>|No|No|
|||||||

> Note: The `switchover` operation causes the network element to reboot. A `force` flag is to be set to `True` and passed along as an extra arg. while you run the playbook.

### **Sample Configuration file for Switchover of Active Controller Card to Standby task**

```yaml
---

controller_configs:
  switchover:
    chassis_ids:
      - 1
      - 2
      - 3
```

## **Configuration variables for the Lock Standby Controller Card task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|chassis_ids| list| The list of chassis ids for which the standby controller card will be locked|Valid values for chassis ids are - 1 to 254|Yes|No|
|||||||

### **Sample Configuration file for the Lock Standby Controller Card task**

```yaml
---

controller_configs:
  lock:
    chassis_ids:
      - 1
      - 2
      - 3
```

## **Configuration variables for the Unlock Standby Controller Card task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|chassis_ids| list| The list of chassis ids for which the standby controller card will be unlocked|Valid values for chassis ids are - 1 to 254|Yes|No|
|||||||

### **Sample Configuration file for the Unlock Standby Controller Card task**

```yaml
---

controller_configs:
  unlock:
    chassis_ids:
      - 1
      - 2
      - 3
```

## **Sample Controller [playbook](../../playbooks/controller_playbook.yml)**

```yaml

# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to chassis Please refer roles/controller/README.md
# While creating a new playbook, please copy the sections between ### Start Copy ### and ### End Copy ### as they are mandatory

### Start Copy ###
- hosts: "{{ host_group }}"
  serial: "{{ batch_size | default(3) }}"
  connection: local
  gather_facts: False

  vars_files:
    - "{{ cfg_file }}"

  vars:
    NE_IP: "{{ ne_ip | default(hostvars[inventory_hostname]['ansible_host']) }}"
    NE_User: "{{ ne_user | default(hostvars[inventory_hostname]['ansible_user']) }}"
    NE_Pwd: "{{ ne_pwd | default(hostvars[inventory_hostname]['ansible_password']) }}"
    TID: "{{ hostvars[inventory_hostname]['tid']| default('') }}"

### End Copy ###

  tasks:

    # To Retrieve Controller Cards for specified Chassis.
    # Please refer to retrieve of config/controller_config.yml
    - import_role:
        name: infinera.iqnos.controller
      vars:
        task_name: retrieve
      tags: retrieve

    # To perform Active to Standby Switchover
    # Please refer to retrieve of config/controller_config.yml
    - import_role:
        name: infinera.iqnos.controller
      vars:
        task_name: switchover
      tags: switchover

    # To perform Lock of Standby
    # Please refer to retrieve of config/controller_config.yml
    - import_role:
        name: infinera.iqnos.controller
      vars:
        task_name: lock
      tags: lock

    # To perform Unlock of Standby
    # Please refer to retrieve of config/controller_config.yml
    - import_role:
        name: infinera.iqnos.controller
      vars:
        task_name: unlock
      tags: unlock

```

## **Commands to run the Active User Session Management Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "input_file=path_to_the/input_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook controller_playbook.yml -e "cfg_file=config/controller_config.yml host_group=ctrlgrp" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "input_file=path_to_the/input_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook controller_playbook.yml -e "host_group=ctrlgrp" -i myhosts.yml --tags=retrieve
  * ansible-playbook controller_playbook.yml -e "force=True cfg_file=config/controller_config.yml host_group=ctrlgrp" -i myhosts.yml --tags=switchover

> Note:
- It is mandatory to provide a `host_group` along with the `input_file` wherever applicable 
- The default inventory is `hosts` if no inventory file is passed

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
