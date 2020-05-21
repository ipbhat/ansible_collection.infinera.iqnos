# **Ansible Role for Alarm Severity Assignment Profiles (ASAP)**
Ansible Role for ASAP is used to perform the following tasks on the Alarm Severity Assignment Profiles 
- to retrieve all, to retrieve selected, to configure and reset to factory settings.

## **Pre-requisites and conditions for the Ansible role ASAP**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/asap_config.yml) "config/asap_config.yml".

## **User tasks and capabilities**
* Retrieve a specific ASAP configuration.
* Retrieve all the ASAP configurations.
* Reset all the ASAPs to factory settings.
* Configure the Service Affecting Severity and the Chassis LED Control parameters for the selected ASAP.

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve](#Configuration-variables-for-Retrieve-Selected-ASAP-task)|Retrieve the ASAP for an object with specified object type and probable cause|retrieve|Mandatory:<ul><li>ObjectType</li><li>ProbableCause</li></ul>|
|[retrieve_all](#Configuration-variables-for-Retrieve-All-ASAP-task)|Retrieve all the ASAPs for all the objects (or) for a specific object type with a specified object type|retrieve_all|Mandatory: <ul><li>ObjectType</li></ul> Optional [one of the parameters only]: <ul><li>ProbableCause</li><li>EventSubType</li><li> IsAffectsChassisLED</li><li>ServiceAffectingSeverity</li></ul>|
|[reset](#Configuration-variables-for-Reset-ASAP-task)|Reset the ASAP to factory default settings for all the object types|reset|Optional: <ul><li>force</li></ul>|
|[configure](#Configuration-variables-for-Configure-ASAP-task)|Configure the Service Affecting Severity and the Chassis LED Control parameters for the object with specified object type and probable cause|configure|Mandatory:<ul><li>ObjectType</li><li>ProbableCause</li></ul> Optional [one of the parameters only]:<ul><li> IsAffectsChassisLED</li><li>ServiceAffectingSeverity</li></ul>|
|||||


## **Configuration variables to retrieve a selected ASAP task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ObjectType|string|A string denoting the object type|Refer IQNOS User document|Yes|No|
|ProbableCause|string|A string denoting the probable cause|Refer IQNOS User document|Yes|No|
|||||||

### **Sample Configuration file to retrieve a specific ASAP Task**

```yaml
asap_configs:
  retrieve:
    ObjectType: ASEM
    ProbableCause: INIT
```

## **Configuration variables to retrieve all the ASAP tasks**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ObjectType|string|A string denoting the object type to use as a filter|<ul><li>ALL</li></ul>|Yes|Yes|
|ProbableCause|string|A string denoting the probable cause to use as a filter|<ul><li>ALL</li></ul>|No|No|
|EventSubType|string|A string denoting the event sub type to use as a filter|<ul><li>ALARM</li><li>TCAA24</li><li>TCAA15</li><li>TCC</li></ul>|No|Yes|
|ServiceAffectingSeverity|string|A string denoting the service affecting severity to use as a filter|<ul><li>Critical</li><li>Warning</li><li>Major</li><li>Minor</li><li>NotReported</li><li>Event</li></ul>|No|Yes|
|IsAffectsChassisLED|string|To be set to true/false|<ul><li>true</li><li>false</li></ul>|No|Yes|
|||||||

### **Sample Configuration file to retrieve all the ASAP tasks**

```yaml
asap_configs:
  retrieve_all:
    ObjectType: ALL
    # Optional parameters
    EventSubType : ALARM
    IsAffectsChassisLED : false
    ServiceAffectingSeverity : Major
    ProbableCause : INIT
```

## **Configuration variables to reset ASAP task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|force|bool|The boolean value to reset all the Alarm Severity settings profiles to factory defaults, then pass the 'force' option and set it to 'True' while executing the playbook command. |True/False|No|No|
|||||||



## **Configuration variables to configure ASAP task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ObjectType|string|A string denoting the object type|Refer IQNOS User document|Yes|No|
|ProbableCause|string|A string denoting the probable cause|Refer IQNOS User document|Yes|No|
|ServiceAffectingSeverity|string|The severity to which the Identifier formed from the Object Type and Probable Cause is to be set|<ul><li>Critical</li><li>Major</li><li>Minor</li><li>Warning</li><li>Event</li><li>Not Reported</li><ul>|No|Yes|
|IsAffectsChassisLED|string|To be set to true/false.|<ul><li>true</li><li>false</li></ul>|No|Yes|
|||||||

### **Sample configuration file to configure ASAP Task**

```yaml
asap_configs:
  configure:
    ObjectType: ASEM
    ProbableCause: INIT
    ServiceAffectingSeverity: Warning
    IsAffectsChassisLED: true
```


## **Sample ASAP server [playbook](../../playbooks/asap_playbook.yml)**

```yaml

---

# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to Alarm Severity Assessment Profile. Please refer roles/asap/README.md
# While creating a new playbook, please copy the sections between ### Start Copy ### and ### End Copy ### as they are mandatory

### Start Copy ### 
- hosts: "{{ host_group }}"
  serial: "{{ batch_size | default(3) }}"
  connection: local
  gather_facts: False

  vars_files:
    - "{{ cfg_file | default('config/asap_config.yml') }}"

  vars:
    NE_IP: "{{ ne_ip | default(hostvars[inventory_hostname]['ansible_host']) }}"
    NE_User: "{{ ne_user | default(hostvars[inventory_hostname]['ansible_user']) }}"
    NE_Pwd: "{{ ne_pwd | default(hostvars[inventory_hostname]['ansible_password']) }}"
    TID: "{{ hostvars[inventory_hostname]['tid']| default('') }}"

### End Copy ###

  tasks:

    # To Retrieve All the ASAP Settings
    # To Retrieve the ASAP Setting for the specified Object Type.
    # Please refer to retrieve_all of config/asap_config.yml
    - import_role:
        name: infinera.iqnos.asap
      vars:
        task_name: retrieve_all
      tags: retrieve_all

    # To Retrieve the ASAP Setting for the specified entity.
    # Please refer to retrieve of config/asap_config.yml
    - import_role:
        name: infinera.iqnos.asap
      vars:
        task_name: retrieve
      tags: retrieve

    # To Configure/Edit the ASAP Setting for the specified entity.
    # Please refer to retrieve of config/asap_config.yml
    - import_role:
        name: infinera.iqnos.asap
      vars:
        task_name: configure
      tags: configure

    # To Reset the all of the ASAP Settings
    - import_role:
        name: infinera.iqnos.asap
      vars:
        task_name: reset
      tags: reset

```

## **Commands to run the ASAP Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook asap_playbook.yml -e "cfg_file=config/asap_config.yml host_group=ASAP" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the ASAP tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook asap_playbook.yml -e "cfg_file=config/asap_config.yml host_group=ASAP" -i myhosts.yml --tags=configure
  * ansible-playbook asap_playbook.yml -e "host_group=ASAP" -i myhosts.yml --tags=retrieve_all
  * ansible-playbook asap_playbook.yml -e "host_group=ASAP force=True" -i myhosts.yml --tags=reset

> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## License

[BSD License 2.0](../../../../License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**