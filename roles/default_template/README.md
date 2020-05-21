# **Ansible Role for Default Templates**
Ansible Role for Default Templates is used to perform retrieve and configure tasks on the Default Template Settings and reset to factory settings.

## **Pre-requisites and conditions for the Ansible role Default Templates**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/default_template_config.yml) "config/default_template_config.yml".

## **User tasks and capabilities**

* Retrieve specific Default Template Settings.
* Reset Default Template Settings to factory Settings.
* Configure the Provisioned Service Type attribute to 10GbE LAN mode.

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve](#Configuration-variables-to-Retrieve-Default-Template-Settings-task)|Retrieve the factory and Custom Default Settings for the TRIB Port|reset|N/A|
|[reset](#Configuration-variables-to-Reset-Default-Template-Settings-task)|Reset the custom Default Settings to factory Default Settings|reset|N/A|
|[configure](#Configuration-variables-to-Configure-Provisioned-Service-Type-for-Default-Template-Settings-task)|Configure the ProvisionedServiceType attribute|configure|ProvisionedServiceType|
|||||

> When the retrieve task is executed, there are two Default Template Settings displayed.
>
> The settings whose `MoId` attribute has `CustomDefault` in it represents the custom defaults.
>
> The settings whose `MoId` attribute has `FactoryDefault` in it represents the factory defaults.

## **Configuration variables to Configure Provisioned Service Type for Default Template Settings task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ProvisionedServiceType|string|The value of the Service Type to be configured for the Custom Default|BW_10GBE_LAN|Yes|No|
|||||||

> The `BW_10GBE_LAN` value corresponds to `10GbE LAN` 

### **Sample configuration file to Configure Provisioned Service Type for Default Template Settings task**

```yaml
---

default_template_configs:
  trib:
    ProvisionedServiceType: BW_10GBE_LAN
```

## **Configuration variables to Retrieve Default Template Settings task**

> * No input config file is required for retrieve task

## **Configuration variables to Reset Default Template Settings task**

> * No input config file is required for reset task

## **Sample Default Template Settings [playbook](../../playbooks/default_template_playbook.yml)**

```yaml
---

# Requires ansible 2.9+
# This playbook is intended to help perform the tasks related to Default Templates. Please refer roles/default_template/README.md
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
    TID: "{{ hostvars[inventory_hostname]['tid'] | default('') }}"

### End Copy ###

  tasks:

    # Retrieve the factory and custom Default Settings.
    - import_role:
        name: infinera.iqnos.default_template
      vars:
        task_name: retrieve
      tags: retrieve

    # Reset the custom Default Settings to the factory Default Settings.
    - import_role:
        name: infinera.iqnos.default_template
      vars:
        task_name: reset
      tags: reset

    # Change the Provisioned Service Type attribute of the 
    # Custom Defaults to 10GbE LAN.
    # Please refer to config/default_template_config.yml
    - import_role:
        name: infinera.iqnos.default_template
      vars:
        task_name: configure
      tags: configure

```

## **Commands to run the Default Template Settings Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook default_template_playbook.yml -e "cfg_file=config/default_template_config.yml host_group=dflttmplt" -i myhosts.yml

## **Commands to run the Playbook with tags for each Default Template Settings task**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook default_template_playbook.yml -e "cfg_file=config/default_template_config.yml host_group=dflttmplt" -i myhosts.yml --tags=configure
  * ansible-playbook default_template_playbook.yml -e "cfg_file=config/default_template_config.yml host_group=dflttmplt" -i myhosts.yml --tags=retrieve
  * ansible-playbook default_template_playbook.yml -e "cfg_file=config/default_template_config.yml host_group=dflttmplt" -i myhosts.yml --tags=reset

> Note:
  - It is mandatory to provide a `host_group` along with the `input_file` wherever applicable
  - The default inventory is `hosts` if no inventory file is passed

## License

[BSD License 2.0](../../../../License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
