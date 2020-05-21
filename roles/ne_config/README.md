# **Ansible Role for ne_config**

Ansible Role `ne_config` is used to retrieve & configure the Network Element configurations

## **Pre-requisites and conditions for the Ansible role ne_config**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/ne_config.yml) "config/ne_config.yml".

## **User tasks and capabilities**

* Retrieve existing Network Element configuration.
* Edit/Configure the existing Network Element configuration.
* Configure gRPC and DCN Parameters

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|retrieve|Retrieve the  system information for the Network Element |retrieve|NA|
|retrieve_grpc|Retrieve GRPC configuration of Network Element |retrieve_grpc|NA|
|[configure](##Configuration-variables-to-`configure`-ne_config-task)|Configure the ne_config as per the input parameters.|configure||
|||||

## **Configuration variables to `configure` ne_config task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|DcnIp|string|Valid dcn ip address that can be send |<ul><li>Valid Ipv4 address</li></ul>|No|No|
|DcnIpNetMask|string| IPv4 subnet mask for the DCN interface |<ul><li>Valid 32 bit IPv4 netmask Example: 255.255.255.224</li></ul>|No|No|
|DcnDestination|string|IPv4 destination/target for the DCN interface |<ul><li>Valid Ipv4 address</li></ul>|No|No|
|DcnGateway|string|IPv4 gateway for the DCN interface|<ul><li>Valid Ipv4 address</li></ul>|No|No|
|DcnPrefixLen|string|IPv4 prefix length of target management subnet range |<ul><li>string values from `1` to `32`</li></ul>|No|No
|DcnIp6|string|Valid dcn ip address that can be send |<ul><li>Valid 128-bit IPv6 address, in quotation marks. Example: "2001:0db8:85a3:0 000:0000:8a2e: 0370:7334"</li></ul>|No|No
|DcnIp6NetMask|string| IPv6 subnet mask for the DCN interface |<ul><li>Valid 128-bit IPv6 address, in quotation mark</li></ul>|No|No
|DcnDestination6|string|IPv6 destination/target for the DCN interface |<ul><li>Valid 128-bit IPv6 address, in quotation mark</li></ul>|No|No
|DcnGateway6|string|IPv6 gateway for the DCN interface|<ul><li>Valid 128-bit IPv6 address, in quotation mark</li></ul>|No|No
|DcnPrefixLen6|string|IPv6 prefix length of target management subnet range |<ul><li>string values from `1` to `128`</li></ul>|No|No|
|GrpcFeature|string|Enable or Disable |<ul><li>Enable or Disable</li></ul>|No|No|
|

### **Sample Configuration [file](../../playbooks/config/ne_config.yml) to `configure` ne_config task**

```yaml
ne_configs:
  configure:
    # To configure the ne_config information.
    DcnIp: "10.220.17.57"
    DcnIpNetMask: "255.255.254.0"
    DcnDestination: "10.0.0.0"
    DcnGateway: "10.220.16.1"
    DcnPrefixLen: "8"
    DcnIp6NetMask: "0"
    DcnDestination6: "::"
    DcnGateway6: "::"
    DcnPrefixLen6: "0"
    DcnInterfaceAdminState: "Enabled"
    # To configure Grpc Feature on Network Element.
    GrpcFeature: "Disable" # "Enable"

```

### **Configuration of GRPC**
* To enable GRPC, set GrpcFeature value to Enable
* To disable GRPC, set GrpcFeature value to Disable
* See example file "config/ne_grpc_config.yml"

### **Configuration of DCN**
* To Configure DNC parameters please refer example [file](../../playbooks/config/ne_dcn_config.yml) "config/ne_dcn_config.yml"

## **Sample ne_config [playbook](../../playbooks/ne_config_playbook.yml)** 

```yaml
# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to ne_config Please refer roles/ne_config/README.md
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
    NE_User: "{{ne_user | default(hostvars[inventory_hostname]['ansible_user']) }}"
    NE_Pwd: "{{ ne_pwd | default(hostvars[inventory_hostname]['ansible_password']) }}"
    TID: "{{ hostvars[inventory_hostname]['tid']| default('') }}"
### End Copy ###

  tasks:
  
      # To Retrieve the ne_config in the network element.
      # Please refer to retrieve of config/ne_config.yml
    - import_role:
        name: infinera.iqnos.ne_config
      vars:
        task_name: "retrieve"
      tags: retrieve
    
      # To Retrieve the GRPC configuration.
    - import_role:
        name: infinera.iqnos.ne_config
      vars:
        task_name: retrieve_grpc
      tags: retrieve_grpc
      
      # To configure specific ne_config is with the following input fields.
      # Please refer to retrieve of config/ne_config.yml
    - import_role:
        name: infinera.iqnos.ne_config
      vars:
        task_name: "configure"
      tags: configure
```

## **Commands to run the ne_config Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * `ansible-playbook ne_config_playbook.yml -e "cfg_file=config/ne_config.yml host_group=ne_config" -i myhosts.yml`

## **Commands to run the Playbook with tags for each of the ne_cofig tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * `ansible-playbook ne_config_playbook.yml -e "cfg_file=config/ne_config.yml  host_group=ne_config" -i myhosts.yml --tags=retrieve,configure`

> Note:
 - Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
 - If no inventory file is passed, then default inventory is `hosts`

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
