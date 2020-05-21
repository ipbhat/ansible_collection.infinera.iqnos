# **Ansible Role for NETCONF Call Home**

Ansible Role for NETCONF Call Home is used to retrieve all the Call Home Settings, invoke On-Demand Call Home Session to a particular destination, stop ongoing Call Home Session, and also to Create, Edit and Delete the existing Call Home Settings.

## **Pre-requisites and conditions for the Ansible role NETCONF Call Home**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See [example file](../../playbooks/config/call_home_config.yml) "config/call_home_config.yml".

## **User tasks and capabilities**

* Retrieve all the Call Home Settings
* Invoke On-Demand Call Home Session to a particular destination
* Create new Call Home Settings
* Edit existing Call Home Settings
* Delete existing Call Home Settings
* Stop Ongoing Call Home Session to a particular destination

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|retrieve|Retrieve all the Call Home Settings|retrieve|N/A|
|[invoke](###invoke)|Invoke On-Demand Call Home Session to a destination address that corresponds to the identifier in the input|invoke|Mo Identifier|
|[create](###create)|Create new Call Home Settings|create|Mo Identifier, Destination Address, Destination Port, TransportProtocol, maxCallhomeRetryTime|
|[edit](###edit)|Edit existing Call Home Instances|edit|Mo Identifier, Destination Address, Destination Port, TransportProtocol, maxCallhomeRetryTime|
|[delete](###delete)|Delete existing Call Home Instances|delete|Mo Identifier|
|[stop](###stop)|Stop Ongoing Call Home Session to a destination address that corresponds to the identifier in the input|stop|Mo Identifier|
|||||

## **Configuration variables**

### **invoke**

##### Invoke On-Demand Call Home Session

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|MoId| string| The unique identifier for the Call Home Setting|Yes|No|
|||||||

##### **Sample Configuration file**

```yaml
---

call_home_configs:
  invoke:
    - CallHomeIdentifier1
    - CallHomeIdentifier2
    - CallHomeIdentifier3

```

### **create**
#####  Create New Call Home Settings

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|MoId| string| The unique identifier for the Call Home Setting|Yes|Yes|
|DestinationAddress| IP Address| The location where the network element is required to dial-out in order to initiate the NETCONF Call Home Session	| A valid IPAddress|Yes|Yes|
|DestinationPort| integer| The TCP/IP port to which the network element will initiate and establish the NETCONF Call Home Session | 1 to 65535|Yes|Yes|
|TransportProtocol| string| The protocol that is to be used for the NETCONF Call Home communication| <li>SSH</li><li>TLS</li>|Yes|Yes|
|maxCallhomeRetryTime| integer| Default value 120 (in minutes)| 1 to 1440|Yes|Yes|
|||||||

> Note:
  The configuration supports a maximum of 6 Call Home Settings.

##### **Sample Configuration file**

```yaml
---

call_home_configs:
  create:
    - MoId: CallHomeIdentifier1
      DestinationAddress: 10.220.15.39
      DestinationPort: 8080
      TransportProtocol: SSH
      maxCallhomeRetryTime: 4

    - MoId: CallHomeIdentifier2
      DestinationAddress: 10.220.5.39
      DestinationPort: 9090
      TransportProtocol: SSH
      maxCallhomeRetryTime: 12

    - MoId: CallHomeIdentifier3
      DestinationAddress: 10.220.165.87
      DestinationPort: 5432
      TransportProtocol: SSH
      maxCallhomeRetryTime: 2

```

### **edit**
##### Edit existing Call Home Settings

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|MoId| string| The unique identifier for the Call Home Setting|Yes|Yes|
|DestinationAddress| IP Address| The location where the network element is required to dial-out to initiate the NETCONF Call Home Session	| A valid IPAddress|No|Yes|
|DestinationPort| integer| The TCP/IP port to which the network element will initiate and establish the NETCONF Call Home Session | 1 to 65535|No|Yes|
|TransportProtocol| string| The protocol to be used for the NETCONF Call Home communication| <ul><li>SSH</li><li>TLS<li></ul>|No|Yes|
|maxCallhomeRetryTime| integer| Default value 120 (in minutes)| 1 to 1440|No|Yes|
|||||||

> Note:
  - The Call Home Setting that corresponds to the `MoId` is updated with the input values.
  - At least one attribute has to be specified apart from the `MoId` in the input.
  - For TransportProtocol variable , value `TLS` is not supported for release R18.2.10.

##### **Sample Configuration file**

```yaml
---

call_home_configs:
  edit:
    - MoId: CallHomeIdentifier1
      DestinationAddress: 10.220.15.39
      DestinationPort: 8080
      TransportProtocol: SSH
      maxCallhomeRetryTime: 4

    - MoId: CallHomeIdentifier2
      DestinationAddress: 10.220.5.39
      DestinationPort: 9090
      TransportProtocol: SSH
      maxCallhomeRetryTime: 12

    - MoId: CallHomeIdentifier3
      DestinationAddress: 10.220.165.87
      DestinationPort: 5432
      TransportProtocol: SSH
      maxCallhomeRetryTime: 2
```

### **delete**
##### Delete existing Call Home Settings

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|MoId| string| The unique identifier for the Call Home Setting|Yes|No|
|||||||

##### **Sample Configuration file**

```yaml
---

call_home_configs:
  delete:
    - CallHomeIdentifier1
    - CallHomeIdentifier2
    - CallHomeIdentifier3

```

### **stop**
##### Stop Ongoing Call Home Session

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|MoId| string| The unique identifier for the Call Home Setting|Yes|No|
|||||||

##### **Sample Configuration file**

```yaml
---

call_home_configs:
  stop:
    - CallHomeIdentifier1
    - CallHomeIdentifier2
    - CallHomeIdentifier3

```

## **Sample Call Home [playbook](../../playbooks/call_home_playbook.yml)**

```yaml
---

# Requires ansible 2.9+
# This playbook is intended to help perform the tasks related to NE Call Home Settings Configurations. Please refer roles/call_home/README.md
# For input configurations please refer/edit config/call_home_config.yml
# To create a new playbook, please copy the sections between ### Start Copy ### and ### End Copy ### as they are mandatory

### Start Copy ### 

- hosts: "{{ host_group }}"
  connection: local
  gather_facts: False
  vars_files:
    - "{{ cfg_file }}"
  vars:
     NE_IP: "{{ ne_ip | default (hostvars[inventory_hostname]['ansible_host']) }}"
     NE_User: "{{ ne_user | default(hostvars[inventory_hostname]['ansible_user']) }}"
     NE_Pwd: "{{ ne_pwd | default (hostvars[inventory_hostname]['ansible_password']) }}"
     TID: "{{ tid | default(hostvars[inventory_hostname]['tid'] | default('')) }}"

### End Copy ###

  tasks:

    # To Retrieve the Call Home Settings from the network element.
    # In case of no Call Home settings, a message is displayed indicating the same.
    - import_role:
        name: infinera.iqnos.call_home
      vars:
        task_name: retrieve
      tags: retrieve

    # To Create the Call Home Settings if the given input is not available on the network element.
    # To Edit the existing Call Home Settings with the given input on the network element.
    # Please refer/edit config/call_home_config.yml - create
    - import_role:
        name: infinera.iqnos.call_home
      vars:
        task_name: create
      tags: create

    # To Delete the existing Call Home Settings on the network element.
    # Task fails if the input contains identifiers to the settings that do not exist.
    # Please refer/edit config/call_home_config.yml - delete
    - import_role:
        name: infinera.iqnos.call_home
      vars:
        task_name: delete
      tags: delete

    # To Edit the existing Call Home Settings on the network element.
    # Please refer/edit config/call_home_config.yml - edit
    - import_role:
        name: infinera.iqnos.call_home
      vars:
        task_name: edit
      tags: edit

    # To invoke On-Demand Call Home.
    # Please refer/edit config/call_home_config.yml - invoke
    - import_role:
        name: infinera.iqnos.call_home
      vars:
        task_name: invoke
      tags: invoke

```

## **Commands to run the NETCONF Call Home Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "input_file=path_to_the/input_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * `ansible-playbook call_home_playbook.yml -e "cfg_file=config/call_home_config.yml host_group=chgrp" -i myhosts.yml`

## **Commands to run the Playbook with tags for each task**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "input_file=path_to_the/input_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * `ansible-playbook call_home_playbook.yml -e "host_group=chgrp" -i myhosts.yml --tags=retrieve_all`
  * `ansible-playbook call_home_playbook.yml -e "cfg_file=config/call_home_config.yml host_group=chgrp" -i myhosts.yml --tags=terminate`

> Note:
  - It is mandatory to provide a `host_group` along with the `input_file` wherever applicable.
  - The default inventory is `hosts` if no inventory file is passed.

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
