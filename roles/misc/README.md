# **Ansible Role for Miscellaneous Tasks**

Ansible Role for Miscellaneous Tasks is used to perform tasks such as Enabling LC for Neighbor Discovery.

## **Pre-requisites and conditions for the Ansible role Misc**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/misc_config.yml) "config/misc_config.yml".

## **User tasks and capabilities**

* Enable LC for Neighbor Discovery

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[enable_lc](#Configuration-Variables-for-Enable-LC-task)|Enable LC for neighbor discovery. This task sets the Facility Monitoring Mode to Intrusive for the ODU4, OTU4 facilities|enable_lc|enable_lc_configs|
|||||

### **Configuration Variables for Enable LC task**

* Order of execution of tasks for the `enable_lc` task
  1. Retrieve TIM, OTU4, ODU4
  2. Perform Idempotency Check
     1. Check if OTU4 Service Mode is not `Adaptation` and Facility Monitoring Mode is not `Im`
     2. Check if ODU4 Service Mode is not `Adaptation` and Facility Monitoring Mode is not `Im`
        * If both the conditions for are satisfied for ODU4 and OTU4 and the TIM Operation Mode is not ODUk - Mark as `xcon_required` for the particular AID
        * If both the conditions for are satisfied for ODU4 only and the TIM Operation Mode is not ODUk - Mark as `xcon_required` for the particular AID
        * Mark as `xcon_not_required` in other cases
  3. Retrieve XCON
  4. Create XCON
  5. Delete XCON
  6. Retrieve OTU4, ODU4
  7. Show Service Mode and Facility Monitoring Mode for each AID

| Variable | Type | Description|Valid Instances/Ranges|Mandatory|Validated|
| ----|------|------------|------------|----------|---------|
|node_to_aid_mapping| object(See Below)| The mapping between Node TID to the the list of AIDs| Valid Mapping(See Below)|Yes|No|
|||||||

> The value for the `AID` needs to be in uppercase - `1-A-1-1` and not `1-a-1-1`

### **Sample Configuration File for Enable LC task**

```yaml
---

misc_configs:
  enable_lc:
    node_to_aid_mapping:
      "XTC284":
        - 1-A-1-1
```

>Note: If a network element is included in the host group, it is mandatory to provide a mapping corresponding to it in the `node_to_aid_mapping` section.

## **Sample Miscellaneous [playbook](../../playbooks/misc_playbook.yml)**

```yaml

# Requires ansible 2.9+
# This playbook is intended to help in performing miscellaneous tasks. Please refer roles/misc/README.md
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

    # To Enable LC
    # Please refer to retrieve of config/misc_config.yml
    - import_role:
        name: infinera.iqnos.misc
      vars:
        task_name: enable_lc
      tags: enable_lc

```

## **Commands to run the Misc Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook misc_playbook.yml -e "cfg_file=config/misc_config.yml host_group=Misc" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the Misc tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook misc_playbook.yml -e "cfg_file=config/misc_config.yml host_group=Misc" -i myhosts.yml --tags=enable_lc

>
> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## License

[BSD License 2.0](../../../../License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
