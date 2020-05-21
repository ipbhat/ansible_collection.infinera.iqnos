# **Ansible Role for Active User Sessions Management**

Ansible Role for Active User Sessions Management is used to retrieve all the active user sessions specified by optional filters, terminate a list of active user sessions on a network element and to terminate all the user sessions on the network elements specified by a filter.

## **Pre-requisites and conditions for the Ansible role Active User Sessions Management**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/session_config.yml) "config/session_config.yml".

## **User tasks and capabilities**

* Retrieve all the active user sessions with optional filters.
* Terminate the list of active user sessions on a single network element.
* Terminate all the user sessions as specified by the filters.

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve_all](#Configuration-variables-to-retrieve-all-the-Active-User-Sessions)|Retrieve all active user sessions|retrieve_all|`AccountName`, `MgmtAppType` (Optional)|
|[terminate](#Configuration-variables-to-terminate-a-list-of-Active-User-Sessions)|Terminate a specific active user session|terminate|list of `SessionId`|
|[terminate_all](#Configuration-variables-to-terminate-all-the-Active-User-Sessions)|Terminate all active user sessions specified by the filters|retrieve_all|`AccountName`, `MgmtAppType` (One is mandatory)|
|||||

## **Configuration variables to terminate a list of Active User Sessions**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|SessionId| list| A list of identifiers for an Active User Session to be terminated. SessionId contains the User Account Name and an underscore followed by a long integer|AccountName_Integer; admin_1236547|Yes|No|
|||||||

## **Sample Configuration file**

```yaml
---

session_configs:
  terminate:
    SessionId:
      - emsadmin_12356
      - asdasd_132
```

## **Configuration variables to retrieve all the Active User Sessions**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|AccountName| string| The account or user name by which the retrieved results are filtered|Valid AccountName; admin|No|No|
|MgmtAppType| string| The management application type by which the retrieved results are filtered|<ul><li>GNM</li><li>EMS</li><li>TL1</li></ul>|No|No|
|||||||

> Note:
  - The AccountName and MgmtAppType are optional filters. If the filtering option is not necessary, comment out the `retrieve_all` block in the input file.

  - If one of the options is chosen, the sessions whose AccountName/MgmtAppType is equal to the one specified in the input are retrieved.

  - If both the options are chosen, the sessions whose AccountName and MgmtAppType are equal to the one specified in the input are      retrieved.

  - The account name is the username that you provide during a log in to other management applications such as the GNM, EMS.

## **Sample Configuration file**

```yaml
---

session_configs:
  retrieve_all:
    AccountName: accname
    MgmtAppType: GNM
```

## **Configuration variables to terminate all the Active User Sessions**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|AccountName| string| The account name by which the sessions to be terminated are filtered|Valid AccountName; admin|No|No|
|MgmtAppType| string| The management application type by which the sessions to be terminated are filtered|<ul><li>GNM</li><li>EMS</li><li>TL1</li></ul>|No|No|
|||||||

> Note:
  - The AccountName and MgmtAppType are filters,specifying one of them is mandatory.

  - If one of the options is chosen, the sessions whose AccountName/MgmtAppType is equal to the one specified in the input are retrieved.

  - If both the options are chosen, the sessions whose AccountName and MgmtAppType are equal to the ones in the input are retrieved.

  - The account name is the username that you provide during a log in to other management applications such as the GNM, EMS.

## **Sample configuration file**

```yaml
---

session_configs:
  terminate_all:
    AccountName: admin
    MgmtAppType: GNM
```

## **Sample Active User Session Management [playbook](../../playbooks/session_playbook.yml)**

```yaml
---

# Requires ansible 2.9+
# This playbook is intended to help performing the tasks related to Active User Sessions. Please refer roles/session/README.md
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

    # To Terminate the user sessions specified by the list of session ids.
    # Please refer to retrieve_all of config/session_config.yml
    - import_role:
        name: infinera.iqnos.session
      vars:
        task_name: terminate
      tags: terminate

    # To Retrieve all Active User Sessions, filter by Account Name, Mgmt App Type is supported.
    # Please refer to retrieve_all of config/session_config.yml
    - import_role:
        name: infinera.iqnos.session
      vars:
        task_name: retrieve_all
      tags: retrieve_all

    # To Terminate the user sessions with given Account Name, Mgmt App Type.
    # Please refer to retrieve_all of config/session_config.yml
    - import_role:
        name: infinera.iqnos.session
      vars:
        task_name: terminate_all
      tags: terminate_all
```

## **Commands to run the Active User Sessions Management Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook session_playbook.yml -e "cfg_file=config/session_config.yml host_group=sessgrp" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook session_playbook.yml -e "cfg_file=config/session_config.yml host_group=sessgrp" -i myhosts.yml --tags=retrieve_all
  * ansible-playbook session_playbook.yml -e "cfg_file=config/session_config.yml host_group=sessgrp" -i myhosts.yml --tags=terminate

> Note:
 - It is mandatory to provide a `host_group` along with the `cfg_file`(where applicable).

 - If no inventory file is passed, then default inventory is `hosts`.

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
