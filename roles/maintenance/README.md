# **Ansible Role for Equipment Maintenance**

Ansible Role for Maintenance is used to retrieve the administrative state of the equipment,  lock the equipment, unlock the equipment.

## **Pre-requisites and conditions for the Ansible Maintenance**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/maintenance_config.yml) "config/maintenance_config.yml".

## **User tasks and capabilities**

* Retrieve Administrative State for the list of Equipment
* Lock the list of Equipment
* Unlock the list of Equipment

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve](#Configuration-variables-for-Retrieve-task)|Retrieve Administrative State for the list of Equipment|retrieve|List of AIDs and Equipment Type|
|[lock](#Configuration-variables-for-Lock-task)|Lock the list of Equipment|lock|List of AIDs and Equipment Type|
|[unlock](#Configuration-variables-for-Unlock-task)|Unlock the list of Equipment|unlock|List of AIDs and Equipment Type|
|[maintenance](#Configuration-variables-for-Maintenance-task)|Move the list of Equipment to Maintenance|maintenance|List of AIDs and Equipment Type|
|||||

> Note:
  - For controller cards like XCMH , maintenance task is not valid.

## **Configuration variables for Retrieve task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|eqpt_type| string| The equipment type on the Network Element|Yes|No|
|aids| list| The list of identifiers(AIDs) of the equipments on the Network Element|Yes|No|
|||||||

## **Sample Configuration File**

```yaml
---

maintenance_configs:
  retrieve:
    eqpt_type: XCMH
    aids:
      - 1-A-1-5
      - 1-A-9A

```

## **Configuration variables for Lock task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|eqpt_type| string| The equipment type on the Network Element|Yes|No|
|aids| list| The list of identifiers(AIDs) of the equipments on the Network Element|Yes|No|
|||||||

## **Sample Configuration File**

```yaml
---

maintenance_configs:
  lock:
    eqpt_type: XCMH
    aids:
      - 1-A-1-5
      - 1-A-9A

```

## **Configuration variables for Unlock task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|eqpt_type| string| The equipment type on the Network Element|Yes|No|
|aids| list| The list of identifiers(AIDs) of the equipments on the Network Element|Yes|No|
|||||||

## **Sample Configuration File**

```yaml
---

maintenance_configs:
  unlock:
    eqpt_type: XCMH
    aids:
      - 1-A-1-5
      - 1-A-9A

```

## **Configuration variables for Maintenance task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|eqpt_type| string| The equipment type on the Network Element|Yes|No|
|aids| list| The list of identifiers(AIDs) of the equipments on the Network Element|Yes|No|
|||||||

## **Sample Configuration File**

```yaml
---

maintenance_configs:
  maintenance:
    eqpt_type: XCMH
    aids:
      - 1-A-1-5
      - 1-A-9A

```

## **Sample Maintenance [playbook](../../playbooks/maintenance_playbook.yml)**

```yaml

# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to maintenance Please refer roles/maintenance/README.md
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

    # To Retrieve the Eqpt.
    # Please refer to retrieve of config/maintenance_config.yml
    - import_role:
        name: infinera.iqnos.maintenance
      vars:
        task_name: retrieve
      tags: retrieve

    # To lock the equipment
    # Please refer to lock of config/maintenance_config.yml
    - import_role:
        name: infinera.iqnos.maintenance
      vars:
        task_name: lock
      tags: lock

    # To Unlock the equipment
    # Please refer to Unlock of config/maintenance_config.yml
    - import_role:
        name: infinera.iqnos.maintenance
      vars:
        task_name: unlock
      tags: unlock

```

## **Commands to run the Active User Session Management Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "input_file=path_to_the/input_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook maintenance_playbook.yml -e "cfg_file=config/maintenance_config.yml host_group=mtncegrp" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "input_file=path_to_the/input_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook maintenance_playbook.yml -e "host_group=mtncegrp" -i myhosts.yml --tags=retrieve
  * ansible-playbook maintenance_playbook.yml -e "cfg_file=group_vars/maintenance_config.yml host_group=mtncegrp" -i myhosts.yml --tags=lock

> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
