# **Ansible Role for Network Time Protocol (NTP)**

The Ansible role for Network Time Protocol is used to configure NTP server address and administrative state of the network elements.


## **Pre-requisites and conditions for the Ansible role infinera.iqnos.ntp**

1. The Host file lists all the network elements and their details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
2. The Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
3. The input configuration file lists the inputs required to run the tasks within the NTP. See example file "group_vars/config.yml".
4. The updated configuration file name or name of the config.file contains inputs required to run the tasks. See example [file](../../playbooks/config/ntp_config.yml) "config/iqnos_ntp_config.yml".

## **User tasks and capabilities**

1. Retrieve an existing NTP configuration.
2. User configurations are validated periodically.
3. User can perform a new NTP configuration.
4. User has permissions to set the administrative states of the NTP servers to Unlocked, Locked, Maintenance.
5. User can enable or disable the alarm reporting control.
6. Input configurations are provided only if the new configuration is different from the existing configuration.
7. IPv4/IPv6 server configurations are supported.
8. User can configure the requried parameters or all the parameters as applicable.
9. A maximum of three NTP server configuratuions are supported, of which user can configure a specific server or all servers, as applicable. 

## **Supported Tasks**

| Tasks | Description| Tags | Input|
|----|------------|----|----|
|retrieve|Retrieve the NTP configurations from the network element. |retrieve|NA|
|config|Configure NTP servers on the network element |lock|[configs](#Configuration-Variables)|
|||||

> During `retrieve` task if “AdministrativeState” Locked, NtpServerAddress46(configured) value is interchanged with PrevIpAddress46 value in JSON output.
## **Configuration variables to update NTP server IP address**

Every Network Time Protocol server configuration is ***created*** with a set of variables, details of which are listed below

| Variable | Type | Description| Valid Instances| Validated|
| ----:|------|------------|-----------|------|
|NtpServerAddress46 |IPv4/IPv6 address|NTP server IP Address|Valid IPv4/IPv6 Address|Yes|
|||||

### **Configuration variables to update Administrative state**

| Variable | Type | Description| Valid Instances| Validated|
| ----:|------|------------|-----------|--------|
|AdministrativeState |string|Administrative state|Locked or Maintenance or UnLocked|Yes|
|||||

*Please refer ntp_lock_unlock_config.yml

### **Configuration variables to update Alarm Report Control**

| Variable | Type | Description| Valid Instances| Validated|
| ----:|------|------------|-----------|--------|
|AlarmReportControl |string|Alarm Report Control| Allowed or Inhibited |Yes|
|||||

## **Sample configuration file to perform the NTP tasks**

```yaml
---
# NTP server configuration .
# Maximum of three NTP servers(NTP-1,NTP-2,NTP-3) are allowed to be configured for a given network element.
# Below is the sample template to configure the NTP servers for a given network element.
# IPv4 and IPv6 addresses are supported.
ntp_configs:
  configure:
    NTP-1:
      AdministrativeState: Locked # Locked | Maintenance | UnLocked
      AlarmReportControl: Allowed # Allowed | Inhibited
      NtpServerAddress46: 127.0.0.1 # IPv4 or IPv6 IP Address
    NTP-2:
      AdministrativeState: Locked # Locked | Maintenance | UnLocked
      AlarmReportControl: Allowed # Allowed | Inhibited
      NtpServerAddress46: 127.0.0.1 # IPv4 or IPv6 IP Address
    NTP-3:
      AdministrativeState: Locked # Locked | Maintenance | UnLocked
      AlarmReportControl: Allowed # Allowed | Inhibited
      NtpServerAddress46: 127.0.0.1 # IPv4 or IPv6 IP Address
```

## **Sample NTP [playbook](../../playbooks/ntp_playbook.yml)**

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
    NE_User: "{{ ne_user | default(hostvars[inventory_hostname]['ansible_user']) }}"
    NE_Pwd: "{{ ne_pwd | default(hostvars[inventory_hostname]['ansible_password']) }}"
    TID: "{{ hostvars[inventory_hostname]['tid']| default('') }}"

### End Copy ###

  tasks:

      # To Retrieve the NTP Server Details.
    - import_role:
        name: infinera.iqnos.ntp
      vars:
        task_name: retrieve
      tags: retrieve

      # To configure the NTP Servers on Network Element
      # Please refer to cofigure of config/ne_config.yml
    - import_role:
        name: infinera.iqnos.ntp
      vars:
        task_name: configure
      tags: configure

```

## **Commands to run the NTP Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook ntp_playbook.yml -e "cfg_file=config/ntp_config.yml host_group=NTP" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the NTP tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/xfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook ntp_playbook.yml -e "cfg_file=config/ntp_cfg.yml  host_group=NTP" -i myhosts.yml --tags=retrieve


> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**