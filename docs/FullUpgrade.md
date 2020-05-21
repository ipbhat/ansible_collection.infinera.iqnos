# Full Upgrade for Ansible

This document provides an overview of the operations involved in a full upgrade process for Ansible along with the necessary code.

## Listed below are the operations involved during a full upgrade process

* Database Cleanup - Removes the image(s) apart from the current database image in use.
* Software Cleanup - Removes the image(s) apart from the current software image in use.
* Software Download - Downloads the software image from the FTP server to the network element.
* Database Backup - Creates a back-up of the existing database image and upload to an FTP server.
* Prepare for upgrade - Assists with pre-upgrade checks to verify the success of the upgrade; this operation is initiated before the upgrade.
* Upgrade - Performs actual upgrade of the software image on the network element.

## Supported Tasks

| Tasks | Description| Tags | Input|
|----|------------|----|----|
| [cleanup](#Configuration-variables-for-cleanup-software-images-task)|  Cleanup the "backup" software image from NE | cleanup| N/A |
|[delete_all](#Configuration-variables-for-delete-all-backup-images-task)|Delete all the existing backup DB images from network element|delete_all|NA|
| [download](#Configuration-variables-for-download-software-images-task) |Download the software to NE | download| <ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrFilePath</li><li>sw_image_version</li><li>arch</li></ul> |
|[backup](#Configuration-variables-for-backup-DB-images-task)|Initiate a DB backup|backup|<ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrSecondaryFilePath</li><li>XfrFilePath</li><li>SimultaneousXfr</li><li>OVWRT</li><li>run_id</li><li>file_mapping</li></ul>|
| [prepare_for_upgrade](#Configuration-variables-for-prepare-for-upgrade-software-images-task)| Prepare for upgrade|prepare_for_upgrade | N/A |
| [upgrade](#Configuration-variables-for-upgrade-software-images-task)| Upgrade NE with downloaded SW| upgrade| N/A |
|

## Configuration variables for cleanup software images task

>
> * No input config file is required. Thus, by default `cleanup` task takes the `All` software images information and listed out. Cleanup the softwares which has `usage` status expected `current`.
>

## Configuration variables to delete all backup images task

> Note:
 - The Delete all task does not require any input configuration file.
 - If the DB contains backup images by default, it will delete all the images.

## Configuration variables for download software images task

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

## Sample configuration file

```yaml
sw_configs:
  # retry an ddelay to download the operation
  check_download_retries: 30
  check_download_delay: 1
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
  initateTransfer:
    ftpfiletype: SwDownloadXfr
```

* See example file ["config/sw_upgrade_config.yml"](../playbooks/config/sw_upgrade_config.yml)

## Configuration variables for backup DB images task

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ftpfiletype| string| For restore, the file type should be `BackupXfr`|BackupXfr|Yes|No|
|XfrPrimaryIp46|string|Valid primary Ipv4 address| Ipv4 address Example: `10.220.13.23`|Yes|No|
|PrimarySftpPort|string|Valid port number has to give|Range from 1 to 65535|Yes|No|
|XfrPrimaryUser|string|Primary user name should be valid which is assigned for Ip address||Yes|No|
|XfrPrimaryPasswd|string|Primary password should set||Yes|No|
|XfrSecondaryIp46|string|Valid secondary Ipv4 address| Ipv4 address Example: `10.220.13.23`|Yes|No|
|SecondarySftpPort|string|Valid port number has to give|Range from 1 to 65535|Yes|No|
|XfrSecondaryUser|string|Secondary user name should be valid which is assigned for Ip address||Yes|No|
|XfrSecondaryPasswd|string|Secondary password should set||Yes|No|
|XfrFilePath|string|Should set valid file path while tansfer the backup DB image|Ex: Path/of/the/file|Yes|No|
|XfrSecondaryFilePath|string|Should set valid file path while tansfer the backup DB image|Ex: Path/of/the/file|Yes|No|
|run_id|string| Unique ID for each run , used to determine file name|Uniqe ID for each run|Yes|No|
|file_mapping|object|Key is TID ,value is DB file name|Valid TID and DB file name|Yes|No|

> Note for Backup DB image
- Either of the parameters highlighted in the closed bracket is required [`run_id` or `file_mapping`]. 
- It is manadatory to configure the remaining parameters and initateTransfer (or download) the DB images.

## Sample configuration file for backup DB images task

```yaml
db_configs:
  # Number of times to retry the backup check after the backup operation has been initiated
  check_backup_retries: 5

  # Delay in seconds between each retry
  check_backup_delay: 60
  backup:
    ftpfiletype: BackupXfr
    XfrPrimaryIp46: "10.220.5.39"
    PrimarySftpPort: "22"
    XfrPrimaryUser: "primary_user"
    XfrPrimaryPasswd: "primary_passwd" # Primary password should encrypted using Ansible vault
    XfrSecondaryIp46: "10.220.5.35"
    SecondarySftpPort: "22"
    XfrSecondaryUser: "secondary_user"
    XfrSecondaryPasswd: "secondary_passwd" # Secondary password should encrypted using Ansible vault
    XfrFilePath: "Backup_dbbkups/"
    XfrSecondaryFilePath: "Backup_secondary_dbbbk2/"
    OVWRT: "true"
    SimultaneousXfr: "true"
    # run_id: "UNIQUE_RUN_ID" # File will be saved as DB_UNIQUE_RUN_ID_NODENAME
    #file_mapping is used when you want to use diffrent DB names for diffrent NEs
    file_mapping:
      # "XTC284": "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" # For NE1 , "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" file will be considered for backup
      "NE57": "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" # For NE2 , "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" file will be considered for backup
  initateTransfer :
    ftpfiletype: BackupXfr
```

* See example file ["config/sw_upgrade_config.yml"](../playbooks/config/sw_upgrade_config.yml)

## Configuration variables for prepare for upgrade software images task

>
> * Input config file is required. Thus, by default `prepare_for_upgrade` task takes the software images to upgrade.
>

## Sample configuration file for prepare for upgrade

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

* See example file ["config/sw_upgrade_config.yml"](../playbooks/config/sw_upgrade_config.yml)

## Configuration variables for upgrade software images task

>
> * Input config file is required. Thus, by default `upgrade` task takes the software images to upgrade.
>

## Sample configuration file for upgrade

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

* See example file ["config/sw_upgrade_config.yml"](../playbooks/config/sw_upgrade_config.yml)


## The complete sample configuration file for full upgrade

```yaml
---

# Refer `sw_mgmt_config.yml` file
sw_configs:
  # Time in minutes to wait to check NE availability after restart_with_emptydb operation 
  restart_with_emptydb_wait_time: 10

  # Time in minutes to wait to check NE availability after revert operation 
  revert_wait_time: 10

  # Time in minutes to wait to check NE availability after fresh_install operation 
  fresh_install_wait_time: 10

  # Time in minutes to wait for after the prepare operation has been initiated
  prepare_wait_time: 10

  # Time in minutes to wait after the upgrade operation has been initiated
  upgrade_wait_time: 30

  # Number of times to retry, to check after the fresh_install, restart_with_emptydb, revert operation has been initiated
  check_retries: 15
  # Delay in seconds between each retry
  check_delay: 2
  # retry an ddelay to download the operation
  check_download_retries: 30
  check_download_delay: 1
  # To download SW image from FTP server to NE
  download :
    # ftp server details , please refer ftp_server.yml 
    ftpfiletype: SwDownloadXfr
    XfrPrimaryIp46: "10.220.0.234"
    PrimarySftpPort: "22"
    XfrPrimaryUser: "primary_user"
    XfrPrimaryPasswd: "primary_passwd" # Use ansible-vault to encrypt the password
    XfrFilePath: "/path/of/the/file/"
    sw_image_version: R18.2.10.0328
    # Arch type is sim/ppc/x86 and file with name R18.2.10.0118.<sim/ppc/x86>.tar.gz should be present in FTP Server
    arch: sim
    # Time to wait to check NE availability after download operation 
    wait_time: 15
  initateTransfer :
    ftpfiletype: SwDownloadXfr

# Refer `db_mgmt_config.yml` file

db_configs:
  # Number of times to retry the backup check after the backup operation has been initiated
  check_backup_retries: 5

  # Delay in seconds between each retry
  check_backup_delay: 60

  # Delay in minutes after the restore operation has been initiated
  # Common for restore, and restore_local tasks
  restore_pause: 10
##############################################################################################################################################  
  backup:
    ftpfiletype: BackupXfr
    XfrPrimaryIp46: "10.220.5.39"
    PrimarySftpPort: "22"
    XfrPrimaryUser: "primary_user"
    XfrPrimaryPasswd: "primary_passwd"
    XfrSecondaryIp46: "10.220.5.35"
    SecondarySftpPort: "22"
    XfrSecondaryUser: "secondary_user"
    XfrSecondaryPasswd: "secondary_passwd"
    XfrFilePath: "Backup_dbbkups/"
    XfrSecondaryFilePath: "Backup_secondary_dbbbk2/"
    OVWRT: "true"
    SimultaneousXfr: "false"
    run_id: "UNIQUE_RUN_ID" # File will be saved as DB_UNIQUE_RUN_ID_NODENAME
    #file_mapping is used when you want to use diffrent DB names for diffrent NEs 
    file_mapping:
      "XTC284": "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" # For NE1 , "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" file will be considered for backup
      "NE2": "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" # For NE2 , "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" file will be considered for backup
  initateTransfer :
    ftpfiletype: BackupXfr
```

## Sample Playbook for full upgrade

```yaml

# Requires ansible 2.9+
# This playbook is intended to help in performing full SW upgrade. Please refer roles/db_mgmt/README.md and  roles/sw_mgmt/README.md
# While creating a new playbook, please copy the sections between ### Start Copy ### and ### End Copy ### as they are mandatory

### Start Copy ### 
- hosts: "{{ host_group }}"
  serial: "{{ batch_size | default(3) }}"
  connection: local
  gather_facts: False
  vars_files: "{{ cfg_file | default('config/sw_upgrade_config.yml') }}"

  vars:
    NE_IP: "{{ ne_ip | default(hostvars[inventory_hostname]['ansible_host']) }}"
    NE_User: "{{ ne_user | default(hostvars[inventory_hostname]['ansible_user']) }}"
    NE_Pwd: "{{ ne_pwd | default(hostvars[inventory_hostname]['ansible_password']) }}"
    TID: "{{ hostvars[inventory_hostname]['tid']| default('') }}"

### End Copy ###

  tasks:

    - name: Aborting SW Upgrade , If user input is not 'yes'
      fail:
        msg: "Aborting SW Upgrade as per user input - {{ skip_confirm }}"
      when: skip_confirm != 'yes

    # Cleanup existing software images
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: cleanup

    # Cleanup existing database images
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: cleanup_all

    # Download the new software image
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: download

    # Backup the image up on an FTP server
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: backup

    # Initiate the upgrade preparation operation
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: prepare

    # Initiate the upgrade preparation operation
    - import_role:
        name: infinera.iqnos.sw_mgmt
      vars:
        task_name: upgrade
```

## Commands to run the Playbook for full upgrade

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * If the SW upgrade playbook to abort, no `skip_confrim` parameter should be provided while running this playbook. See the below example playbook,
   >
   > `ansible-playbook sw_upgrade_playbook.yml -e "cfg_file=config/sw_upgrade_config.yml host_group=SW_UPGRADE" -i myhosts.yml`
   >

  * If the SW upgrade playbook not to abort then pass an argument `skip_confrim=yes` while running the playbook. See the below example playbook,
  >
  > `ansible-playbook sw_upgrade_playbook.yml -e "skip_confirm=yes cfg_file=config/sw_upgrade_config.yml host_group=SW_UPGRADE" -i myhosts.yml`
  >

> Providing a `host_group` along with the `input_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## Issues
  * This playbook tasks uses 'pause' component of ansible. The 'pause' component has known issue that if pause is skipped for first host, then pause will not be executed for other hosts - This may lead to inconsistency in tasks result. We advice to run this playbook on host group with single node. See [here](https://github.com/ansible/ansible/issues/19966) more info about issue.
  * If user `abort` during `pause` action, may lead to inconsistency in task results. Infinera suggest to ignore these results and run the playbook again for correct results.

## License

BSD License 2.0

## Author Information

Infinera Corporation.

Technical Support - techsupport@infinera.com