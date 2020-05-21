# **Ansible Role for Chassis**

Ansible Role for chassis is used to retrieve all or a specified chassis configuration details and configure the chassis on the network elements.

## **Pre-requisites and conditions for the Ansible role CHASSIS**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/chassis_config.yml) "config/chassis_config.yml".

## **User tasks and capabilities**

* Retrieve all the existing chassis configuration.
* Edit or Configure the existing chassis configuration.
* Validate the configurations periodically.

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve](#Configuration-variables-for-Retrieve-specified-or-All-chassis-task)|Retrieve the chassis for the object specified by Id=`ALL` or speified to retrieve|retrieve|Mandatory:<ul><li>Id</li></ul>|
|[configure/ Edit](#Configuration-variables-for-configure/edit-chassis-task)|Configure the chassis per input parameters.|configure/edit||
|||||

## **Configuration variables for Retrieve specified or All chassis task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|Id| string| Valid Id that can be retrieved|<ul><li>ALL(To retrieve all chassis configuration details)</li><li>string values from `1` to `256`</li></ul>|Yes|Yes|
|

### **Sample Configuration file for the Retrieve specific Chassis task**

```yaml
chassis_configs:
  retrieve:
    Id: ALL
```

## **Configuration variables for the Configure/Edit chassis task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|Id|string|Valid Id that can be send |<ul><li>ALL(To retrieve all chassis configuration information)</li><li>string values from `1` to `256`</li></ul>|Yes|Yes|
|Label|string|Return valid string for the label|<ul>N.A</ul>|No|No|
|ProvisionedChassisType|string|Enter the valid provisioned chassis type|<ul><li>DTC-A, DTC-B, MTC-A,</li></ul>|No|No|
|SwitchCapabilityMode|string|Returns the valid value for switch capability mode|<ul><li>RING</li><li>MESH</li></ul>|No|Yes|
|

### **Sample Configuration file for the Configure/Edit Chassis task**

```yaml
chassis_configs:
  configure:
    Id: "1"
    Label: "TestChassis"
    ProvisionedChassisType: "MTC_A"
    SwitchCapabilityMode: "RING"
```

## **Sample Chassis server [playbook](../../playbooks/chassis_playbook.yml)**

```yaml
# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to Chassis Please refer roles/chassis/README.md
# Its is mandatory copy the sections between ### Start Copy ### and ### End Copy ### while creating a new playbook

### Start Copy ###
- hosts: "{{ host_group }}"
  serial: "{{ batch_size | default(3) }}"
  connection: local
  gather_facts: False

  vars_files:
    - "{{ cfg_file }}"

  vars:
    NE_IP: "{{ ne_ip | default(hostvars[inventory_hostname]['ansible_host']) }}"
    NE_User: "{{ne_user | default(hostvars[inventory_hostname]['ansible_user']) }}"
    NE_Pwd: "{{ ne_pwd | default(hostvars[inventory_hostname]['ansible_password']) }}"
    TID: "{{ hostvars[inventory_hostname]['tid']| default('') }}"
### End Copy ###

  tasks:
  
      # To Retrieve specified or all the chassis in the network element.
      # Please refer to retrieve of config/chassis_config.yml
    - import_role:
        name: infinera.iqnos.chassis
      vars:
        task_name: "retrieve"
      tags: retrieve

      # To configure specific chassis is with the following input fields.
      # Please refer to retrieve of config/chassis_config.yml
    - import_role:
        name: infinera.iqnos.chassis
      vars:
        task_name: "configure"
      tags: configure
```

## **Commands to run the Chassis Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook chassis_playbook.yml -e "cfg_file=config/chassis_config.yml host_group=chassis" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the Chassis tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/xfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook chassis_playbook.yml -e "cfg_file=config/chassis_cfg.yml  host_group=chassis" -i myhosts.yml --tags=retrieve_all

> Note:
- It is mandatory to provide a `host_group` along with the `input_file` wherever applicable
- The default inventory is `hosts` if no inventory file is passed

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
