# **Ansible Role for Software Image(s) Management**

Ansible Role for Software Image(s) Management is used to download software images, to delete software images on the network element, upgrade the software images, prepare the network element for upgrade, and revert the software and database image.

## **Pre-requisites and conditions for the Ansible role Database Image(s) Management**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "sw_group" in "hosts.yml" file.
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/sw_mgmt_config.yml) "config/sw_mgmt_config.yml.yml".

## **User tasks and capabilities**

* Retrieve all the existing configuration of Software images.
* Cleanup task delete the backup software images from the NE.
* Restart with emptyDB task with the current software image.
* Download the software image after configuring the software download XFR.
* Prepare for upgrade task on the upgraded software images.
* Cancel prepare for upgrade task applies, when the upgrade software is in progress after applying prepare for upgrade.
* Fresh install task applies, on the upgraded software images.
* Upgrade task applies on upgrade software images.
* Revert task applies on upgraded software image after upgraded task.
* Lock standby task applies before the upgrade task.
* Unlock standby task.

> Note

* Input configurations are applied only if the existing configuration is not same as new configuration.
* IPv4/IPv6 server configurations are supported.

## **Supported Tasks**

| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve_all](#Configuration-variables-for-Retrieve-All-software-images-task)|Retrive all SW Management information |retrieve_all|NA
| [cleanup](#Configuration-variables-for-cleanup-software-images-task)|  Cleanup the "backup" software image from NE | cleanup| N/A |
| [restart_with_emptydb](#Configuration-variables-for-restart-with-emptyDB-software-images-task)|  Restart NE with No or default configuration| restart_with_emptydb| N/A |
| [download](#Configuration-variables-for-download-software-images-task) |Download the software to NE | download| <ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrFilePath</li><li>sw_image_version</li><li>arch</li></ul> |
| [prepare_for_upgrade](#Configuration-variables-for-prepare-for-upgrade-software-images-task)| Prepare for upgrade|prepare_for_upgrade | N/A |
| [cancel_prepare_for_upgrade](#Configuration-variables-for-cancel-prepare-for-upgrade-software-images-task)| Cancel Prepare for upgrade |cancel_prepare_for_upgrade | N/A |
| [fresh_install](#Configuration-variables-for-fresh-install-software-images-task)| Fresh install downloaded SW on NE |fresh_install | N/A |
| [upgrade](#Configuration-variables-for-upgrade-software-images-task)| Upgrade NE with downloaded SW| upgrade| N/A |
| [revert](#Configuration-variables-for-revert-software-images-task)| Revert NE to older version of SW| revert| <ul><li>run_id</li><li>db_file_mapping</li></ul> |
| [lock_stby](#Configuration-variables-for-lock-stby-software-images-task)| Lock the standby controller card| lock_stby| <ul><li>Aid</li><li>AdministrativeState</li></ul>  |
| [unlock_stby](#Configuration-variables-for-unlock-stby-software-images-task)| Unlock the standby controller card| unlock_stby| <ul><li>Aid</li><li>AdministrativeState</li></ul> |

## **Configuration variables for Retrieve All software images task**

>
> * No input config file is required. Thus, by default `retrieve_all` task takes the `All` software images information and listed out.
>

## **Configuration variables for cleanup software images task**

>
> * No input config file is required. Thus, by default `cleanup` task takes the `All` software images information and listed out. Cleanup the softwares which has `usage` status expected `current`.
>

## **Configuration variables for restart with emptyDB software images task**

>
> * Input config file is required. Thus, by default `restart_with_emptyDB` task takes the software images of `current` Usage status and install with emptyDB.
> * To start fresh install, we need to set or pass the `force` parameter as `True` otherwise by default it will be set as `False`. And it won't tigger the restart_with_emptyDB.
> * #### Procedure to run the `restart_with_emptyDB` task, run as per below steps:
>> * Run the `lock_stby` task --> Check the input cfg file for `lock_stdy`  task 
>> * And the `restart_with_emptyDB` task --> No input cfg file is required.
>> * `unlock_stby` task -- check the input cfg file for `unlock_stby` task
>

## **Sample configuration file for restart with empty DB**

```yaml
sw_configs:
  # Connection availability timeout
  availability_timeout: 300
  # Number of times to retry, to check after the fresh_install, restart_with_emptydb, revert operation has been initiated
  check_retries: 15
  # Delay in seconds between each retry
  check_delay: 2
  # Time in minutes to wait to check NE availability after restart_with_emptydb operation
  restart_with_emptydb_wait_time: 10
```

* See example file ["config/sw_mgmt_config.yml"](../../playbooks/config/sw_mgmt_config.yml)

## **Configuration variables for download software images task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ftpfiletype| string| For dowwnload DB, the file type should be  one of the XFR ftp file type|SwDownloadXfr|Yes|Yes|
|XfrPrimaryIp46|string|Valid primary Ipv4 address| Ipv4 address Example: `10.220.13.23`|Yes|Yes|
|PrimarySftpPort|string|Valid port number has to give|Range from 1 to 65535|Yes|Yes|
|XfrPrimaryUser|string|Primary user name should be valid which is assigned for Ip address||Yes|Yes|
|XfrPrimaryPasswd|string|Primary password should set||Yes|Yes|
|XfrFilePath|string|Should set valid file path while tansfer the restore DB image|Ex: Path/of/the/file|Yes|No|
|sw_image_version|string| Software image version should be valid |Valid software image version|Yes|No|
|arch|string|Software architecture should be valid|Valid architecture |Yes|No|

## **Sample configuration file**

```yaml
sw_configs:
  download:
    ftpfiletype: SwDownloadXfr
    XfrPrimaryIp46: "10.220.0.234"
    PrimarySftpPort: "22"
    XfrPrimaryUser: "primary_user"
    XfrPrimaryPasswd: "primary_passwd" # Use ansible-vault to encrypt the password
    XfrFilePath: "/home/pub/R18.2.10.0328/tar_ne/SIM/XTC2/"
    sw_image_version: R18.2.10.0328
    # Arch type is sim/ppc/x86 and file with name R18.2.10.0118.<sim/ppc/x86>.tar.gz should be present in FTP Server
    arch: sim
    # Time to wait to check NE availability after download operation 
    wait_time: 15
  initateTransfer:
    ftpfiletype: SwDownloadXfr
```

* See example file ["config/sw_mgmt_sw_download_config.yml"](../../playbooks/config/sw_mgmt_sw_download_config.yml)

## **Configuration variables for prepare for upgrade software images task**

>
> * Input config file is required. Thus, by default `prepare_for_upgrade` task takes the software images to upgrade.
>

## **Sample configuration file for prepare for upgrade**

```yaml
sw_configs:
  # Connection availability timeout
  availability_timeout: 300
  # Number of times to retry, to check after the fresh_install, restart_with_emptydb, revert operation has been initiated
  check_retries: 15
  # Delay in seconds between each retry
  check_delay: 2

  # Time in minutes to wait for after the prepare operation has been initiated
  prepare_wait_time: 20
```

* See example file ["config/sw_mgmt_config.yml"](../../playbooks/config/sw_mgmt_config.yml)

## **Configuration variables for cancel prepare for upgrade software images task**

>
> * No input config file is required. Thus, by default `cancel_prepare_for_upgrade` task takes the software images to cancel the upgrade.
>

## **Configuration variables for fresh install software images task**

>
> * Input config file is required. Thus, by default fresh_install` task takes the software images to fresh install the software.
> * To start fresh install, we need to set or pass the `force` parameter as `True` otherwise by default it will be set as `False`. And it won't tigger the fresh_install.
> * #### Procedure to run the `fresh_install` task, run as per below steps:
>> * Run the `lock_stby` task -- Check the input cfg file for `lock_stdy`  task 
>> * And the `fresh_install` task -- No input cfg file is required.
>> * `unlock_stby` task -- check the input cfg file for `unlock_stby` task

## **Sample configuration file for fresh install**

```yaml
sw_configs:
  # Connection availability timeout
  availability_timeout: 300
  # Number of times to retry, to check after the fresh_install, restart_with_emptydb, revert operation has been initiated
  check_retries: 15
  # Delay in seconds between each retry
  check_delay: 2

  # Time in minutes to wait to check NE availability after fresh_install operation
  fresh_install_wait_time: 10
```

* See example file ["config/sw_mgmt_config.yml"](../../playbooks/config/sw_mgmt_config.yml)

## **Configuration variables for upgrade software images task**

>
> * Input config file is required. Thus, by default `upgrade` task takes the software images to upgrade.
>

## **Sample configuration file for upgrade**

```yaml
sw_configs:
  # Connection availability timeout
  availability_timeout: 300
  # Number of times to retry, to check after the fresh_install, restart_with_emptydb, revert operation has been initiated
  check_retries: 15
  # Delay in seconds between each retry
  check_delay: 2

  # Time in minutes to wait after the upgrade operation has been initiated
  upgrade_wait_time: 30
```

* See example file ["config/sw_mgmt_config.yml"](../../playbooks/config/sw_mgmt_config.yml)

## **Configuration variables for revert software images task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|run_id| string| Valid db file name |UNIQUE_ID|Yes|No|
|db_file_mapping|object|Should conatin valid db names with specified NE key names| DB_UNIQUE_FILE_NAME |Yes|No|

## **Sample configuration file name**

```yaml
sw_configs:
  # Connection availability timeout
  availability_timeout: 300
  # Number of times to retry, to check after the fresh_install, restart_with_emptydb, revert operation has been initiated
  check_retries: 15
  # Delay in seconds between each retry
  check_delay: 2
  # Time in minutes to wait to check NE availability after revert operation
  revert_wait_time: 10
  # Revert / downgrade to older version
  revert:
    # run_id used for taking backup
    run_id: "UNIQUE_RUN_ID"
    #db_file_mapping is used when you want to use diffrent DB names for diffrent NEs  
    db_file_mapping:
      "NE1": "DB_UNIQUE_RUN_ID_CUSTOM_FILE" #add here all NE name to DB name mapping if DB name is not in format DB_backup_08_05_15_NODENAME
      "NE2": "DB_UNIQUE_RUN_ID_CUSTOM_FILE" #add here all NE name to DB name mapping if DB name is not in format DB_backup_08_05_15_NODENAME
```

* See example file ["config/sw_mgmt_config.yml"](../../playbooks/config/sw_mgmt_config.yml)

### **Note for revert software image**

> * #### Procedure to run the `revert` task, run as per below steps:
>> * Run the `lock_stby` task -- Check the input cfg file for `lock_stdy`  task 
>> * And the `revert` task -- No input cfg file is required.
>> * `unlock_stby` task -- check the input cfg file for `unlock_stby` task

## **Configuration variables for lock stby software images task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|Aid| string| Valid Aid values should present |UNIQUE_ID|Yes|Yes|
|AdministrativeState|string|Valid administrative state should be present|<ul><li>Locked</li><li>UnLocked</li></ul> |Yes|Yes|
|

## **Sample configuration for lock standy**

```yaml
sw_configs:
  lock_stby:
    Aid: "1-A-9B"
    AdministrativeState: Locked
```

* See example file ["config/sw_mgmt_config.yml"](../../playbooks/config/sw_mgmt_config.yml)

### Note

>
> For more deatils about AID, refer this [AIDFormat](../../docs/AIDFormat.md)

> After running `fresh_install` or `restart_with_emptyDB`, Network Element password will be reset to default. Please use [user](../user) role to change the default password

## **Configuration variables for unlock stby software images task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|Aid| string| Valid Aid values should present |UNIQUE_ID|Yes|Yes|
|AdministrativeState|string|Valid administrative state should be present|<ul><li>Locked</li><li>UnLocked</li></ul> |Yes|Yes|
|

## **Sample configuration for unlock standy**

```yaml
sw_configs:
  lock_stby:
    Aid: "1-A-9B"
    AdministrativeState: UnLocked
```

* See example file ["config/sw_mgmt_config.yml"](../../playbooks/config/sw_mgmt_config.yml)

### Note

>
> For more deatils about AID, refer this [AIDFormat](../../docs/AIDFormat.md)
>

## **Sample SW Management [playbook](../../playbooks/sw_mgmt_playbook.yml)**

```yaml
---

# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to SW Mgmt.. Please refer roles/sw_mgmt/README.md
# For input configurations please refer Or edit group_vars/sw_mgmt_config.yml

# While creating a new playbook, please copy the sections between ### Start Copy ### and ### End Copy ### as they are mandatory

### Start Copy ###
- hosts: "{{ host_group }}"
  serial: "{{ batch_size | default(3) }}" # 3 Nodes are managed by default at a time
  connection: local
  gather_facts: False
  vars_files:
    - "{{ cfg_file }}"

  vars:
    NE_IP: "{{ ne_ip|default (hostvars[inventory_hostname]['ansible_host']) }}"
    NE_User: "{{ ne_user|default(hostvars[inventory_hostname]['ansible_user']) }}"
    NE_Pwd: "{{ ne_pwd|default (hostvars[inventory_hostname]['ansible_password']) }}"
    TID: "{{ hostvars[inventory_hostname]['tid']| default('') }}"
### End Copy ###

  tasks:

    # To Retrieve SW Management information from NE
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: retrieve_all
      tags: retrieve_all

    # To delete SW Management information from NE
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: cleanup
      tags: cleanup

    # To Lock standby SW Management information from NE
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: lock_stby
      tags: lock_stby

    # To Unlock standy SW Management information from NE
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: unlock_stby
      tags: unlock_stby

    # To restart with empty DB for SW Management information from NE
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: restart_with_emptydb
      tags: restart_with_emptydb

    # To download software image 
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: download
      tags: download

    # To prepare to upgrade the software image 
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: prepare_for_upgrade
      tags: prepare_for_upgrade

    # To cancel the prepare to upgrade the software image 
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: cancel_prepare_for_upgrade
      tags: cancel_prepare_for_upgrade

    # To fresh install of the software image 
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: fresh_install
      tags: fresh_install

    # To upgrade the software image 
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: upgrade
      tags: upgrade

    # To revert the software image 
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: revert
      tags: revert
```

## **Commands to run the SW Management Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * `ansible-playbook sw_mgmt_playbook.yml -e "cfg_file=config/sw_mgmt_config.yml host_group=SW_MGMT" -i myhosts.yml`

## **Commands to run the Playbook with tags for each of the SW Management tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * `ansible-playbook sw_mgmt_playbook.yml -e "cfg_file=config/sw_mgmt_config.yml  host_group=SW_MGMT" -i myhosts.yml --tags=retrieve_all`
  * `ansible-playbook sw_mgmt_playbook.yml -e "cfg_file=config/sw_mgmt_config.yml  host_group=SW_MGMT" -i myhosts.yml --tags=cleanup`
  * `ansible-playbook sw_mgmt_playbook.yml -e "cfg_file=config/sw_mgmt_sw_download_config.yml  host_group=SW_MGMT" -i myhosts.yml --tags=download`
  * `ansible-playbook sw_mgmt_playbook.yml -e "force=True cfg_file=config/sw_mgmt_sw_download_config.yml  host_group=SW_MGMT" -i myhosts.yml --tags=fresh_install`
  * `ansible-playbook sw_mgmt_playbook.yml -e "force=True cfg_file=config/sw_mgmt_sw_download_config.yml  host_group=SW_MGMT" -i myhosts.yml --tags=restart_with_emptydb`

> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
