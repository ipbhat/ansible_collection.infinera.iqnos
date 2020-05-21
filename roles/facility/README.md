# **Ansible Role for Facilities**

Ansible Role for Facilities is used to perform tasks such as Editing the AUTOTTI Settings of facilities.

## **Pre-requisites and conditions for the Ansible role Facility**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/facility_config.yml) "config/facility_config.yml".

## **User tasks and capabilities**

* Edit AUTOTTI Settings for Facilities

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[edit_autotti_settings](###Configurations-Variables-for-`edit_autotti_settings`-task)|Edit the AUTOTTI Settings attribute for the facilities|edit_facility_autotti_settings|facility, autotti, node_to_aid_mapping|
|||||

### **Configurations Variables for `edit_autotti_settings` task**

| Variable | Type | Description|Valid Instances/Ranges|Mandatory|Validated|
| ----|------|------------|------------|----------|---------|
|facility| string| The facility for which AUTOTTI is to be enabled or disabled | <ul><li>ODU4</li><li>OTU4</li><li>ODU2e</li></ul>|Yes|Yes|
|autotti| string| The mode to which the operation mode must be set to| <ul><li>Enabled</li><li>Disabled</li></ul> |Yes|Yes|
|node_to_aid_mapping| object(See Below)| The mapping between Node TID to the the list of trib AIDs(See below)| Valid Mapping(See Below)|Yes|No|
||||||

> The value for `facility` is case sensitive
>
> The value for the `AID` in the `node_to_aid_mapping` in the configuration block is assumed to be an `AID` that corresponds to a `trib` in `TIM-1-100GX` (or) `TIM-5-10GX`
>
> The value for the `AID` needs to be in uppercase - `1-A-1-T1-1` and not `1-a-1-t1-1`

#### **Sample Configuration File for `edit_autotti_settings` task**

```yaml
facility_configs:
  edit_autotti_settings:
    facility: ODU4 # And OTU4, ODU2e are supported
    autotti: Enabled # Or Disabled
    node_to_aid_mapping:
      "XTC286":
        - 1-A-1-T1-1
        - 1-A-5-T1-1
```

>Note: If a network element is included in the host group, it is mandatory to provide a mapping corresponding to it in the `node_to_aid_mapping` section.

## **Sample Facilities [playbook](../../playbooks/facility_playbook.yml)**

```yaml

# Requires ansible 2.9+
# This playbook is intended to help in performing tasks on facilities. Please refer roles/facility/README.md
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

    # To Edit AUTOTTI Settings
    # Please refer to retrieve of config/facility_config.yml
    - import_role:
        name: infinera.iqnos.facility
      vars:
        task_name: edit_autotti_settings
      tags: edit_autotti_settings

```

## **Commands to run the Facility Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * `ansible-playbook facility_playbook.yml -e "cfg_file=config/facility_config.yml host_group=Facility" -i myhosts.yml`

## **Commands to run the Playbook with tags for each of the Facility tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * `ansible-playbook facility_playbook.yml -e "cfg_file=config/facility_config.yml host_group=Facility" -i myhosts.yml --tags=edit_autotti_settings`

>
> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
