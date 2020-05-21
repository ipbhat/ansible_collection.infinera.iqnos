# **Ansible Role for Equipment Operations**

Ansible Role `eqpt` is used to perform the following tasks on the Equipment - to COLD/WARM reset on the controllers, line modules and MUX modules, Edit the operation mode of TIM-1-100GX.

## **Pre-requisites and conditions for the Ansible role eqpt**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/eqpt_config.yml) "config/eqpt_config.yml".

## **User tasks and capabilities**

* WARM Reset task on the controllers, line modules and MUX modules cards.
* COLD Reset task on controllers, line modules and MUX modules cards.
* Edit the operation mode of TIM-1-100GX.

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[reset](#Configuration-variables-for-Reset-eqpt-task)|Reset the controllers, line modules and MUX modules cards by using COLD or WARM reset task|reset|Mandatory:<ul><li>EquipmentType</li><li>Aid</li><li>Reset</li></ul>Optional: <ul><li>force</li></ul>|
|[edit_operation_mode](#Configuration-variables-for-Edit-Operation-Mode-task)|Edit the operation mode of TIM-1-100GX|edit_operation_mode|eqpt, mode, node_to_aid_mapping|
|||||

## **Configuration variables for `reset` eqpt task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|EquipmentType|string|Equipment type contains the valid values. |<ul><li>Chassis</i><li>Shelf</li><li>XCMH</li><li>OTXM</li><li>OTSM</li><li>TIM</li><li>TSM</li><li>LIM</li><li>OFX</li><li>OFM</li><li>IAM</li><li>IRM</li><li>FRM</li><li>FMMC</li><li>FSM</li><li>OTDM</li><li>FAN</li><li>PEM</li></ul>|Yes|Yes|
|Aid|string|[AID](../../docs/AIDFormat.md) contains the valid values. |chassis= {1-254}, shelf = {A}, slot = {9A, 9B}|Yes|Yes|
|Reset|string|Reset contains the valid values. |<ul><li>WARM</li><li>COLD</li>|Yes|Yes|
|force|bool|The boolean denoting to reset all the Equipments to factory defaults then pass the 'force' option and set it to 'True'. While executing the playbook command. |True/False|No|No|
|||||||

### **Sample Configuration File for WARM/COLD Reset task**

```yaml
eqpt_configs:
  reset:
    EquipmentType: XCMH
    Aid : 1-A-9A
    Reset : WARM
```

### **Note for Reset**

>
> * For reset task, EquipmentType, Aid and the type of Reset should required to run.
> * Before COLD reset we need to change `AdministrativeState` to `Lock` or `Maintanace`, except controller cards. To  chage Admin state refer `maintanatce role`
> * The value for the AID needs to be in uppercase - `1-A-9A` and neither `1-a-9a` nor `1-a-9A`.
> * For more information regarding `AID format`. Refer this link [AIDFormat](../../docs/AIDFormat.md)
>

## **Configuration variables for Edit Operation Mode task**

For Each AID:

1. After changing the operation mode of the TIM equipment the task waits for 32 minutes so that the change takes place.
2. Post this wait time the equipment is moved to the unlocked state

| Variable | Type | Description|Valid Instances/Ranges|Mandatory|Validated|
| ----|------|------------|------------|----------|---------|
|EquipmentType| string| The equipment for which the operation mode is to be edited| TIM|Yes|Yes|
|OperatingMode| string| The mode to which the operation mode must be set to| <ul><li>ODU4_ODL</li><li>ODUk_ODUj</li><li>ODUk</li><li>GBE100_ODU4_4i_2ix10V</li></ul>|Yes|Yes|
|node_to_aid_mapping| object(See Below)| The mapping between Node TID to the the list of AIDs| Valid Mapping(See Below)|Yes|No|
||||||

Note:

> * The `EquipmentType` variable is required to be `TIM`.
>
> * The `OperatingMode` value is expected to be the above values (case sensitive) and not others.
>
> The value for the `AID` in the `node_to_aid_mapping` in the configuration block is assumed to be an `AID` that corresponds to a `TIM` of type `TIM-1-100GX`
>
> The value for the `AID` needs to be in uppercase - `1-A-1-1` and not `1-a-1-1`

### **Sample Configuration File for Edit Operation Mode task**

```yaml
eqpt_configs:
  edit_operation_mode:
    EquipmentType: TIM # DON'T MODIFY
    OperatingMode: ODUk
    node_to_aid_mapping:
      'XTC6653':
        - 1-A-1-5
        - 1-A-1-3
      'NE57':
        - 1-A-1-1
        - 1-A-1-3
        - 1-A-1-5
        - 1-A-1-7
```

> Note : If a network element is included in the host group, it is mandatory to provide a mapping corresponding to it in the `node_to_aid_mapping` section.

## **Sample iqnos_eqpt server [playbook](../../playbooks/eqpt_playbook.yml)**

```yaml
---

    # Requires ansible 2.9+
    # This playbook is intended to help in performing the tasks related to eqpt. Please refer roles/eqpt/README.md
    # While creating a new playbook, please copy the sections between ### Start Copy ### and ### End Copy ### as they are mandatory

    ### Start Copy ###
    - hosts: "{{ host_group }}"
      serial: "{{ batch_size | default(3) }}"
      connection: local
      gather_facts: false

      vars_files:
        - "{{ cfg_file }}"

      vars:
        NE_IP: "{{ ne_ip | default(hostvars[inventory_hostname]['ansible_host']) }}"
        NE_User: "{{ ne_user | default(hostvars[inventory_hostname]['ansible_user']) }}"
        NE_Pwd: "{{ ne_pwd | default(hostvars[inventory_hostname]['ansible_password']) }}"
        TID: "{{ hostvars[inventory_hostname]['tid']| default('') }}"

    ### End Copy ###

      tasks:
        # To Retrieve the eqpt Setting for the specified entity.
        # Please refer the config/eqpt_config.yml
        - import_role:
            name: infinera.iqnos.eqpt
          vars:
            task_name: reset
          tags: reset

        # To Edit the Operation Mode of TIM-1-100GX.
        # Please refer the config/eqpt_config.yml
        - import_role:
            name: infinera.iqnos.eqpt
          vars:
            task_name: edit_operation_mode
          tags: edit_operation_mode
```

## **Commands to run the eqpt Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * `ansible-playbook eqpt_playbook.yml -e "cfg_file=config/eqpt_config.yml host_group=eqpt" -i myhosts.yml`

## **Commands to run the Playbook with tags for each of the eqpt tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * `ansible-playbook eqpt_playbook.yml -e "cfg_file=config/eqpt_config.yml host_group=eqpt" -i myhosts.yml --tags=reset`


> If the user want to reset forcefully then, use `force=True` argument in playbook. See below example
>
> * ansible-playbook eqpt_playbook.yml -e "force=True cfg_file=config/eqpt_config.yml host_group=eqpt" -i myhosts.yml --tags=reset

>
> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## License

[BSD License 2.0](../../../../License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
