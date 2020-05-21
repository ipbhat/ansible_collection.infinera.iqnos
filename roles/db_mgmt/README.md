# **Ansible Role for Database Image Management**

The Ansible Role for Database Image Management is used to clean up the database (DB) backup files on the network element, back up the current database image, schedule the backups, and restore a database image.

## **Pre-requisites and conditions for the Ansible role Database Image Management**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/db_mgmt_config.yml) "config/db_mgmt_config.yml".

## **User tasks and capabilities**

* Retrieve all the existing configuration of Database images.
* Delete single database backup file.
* Delete all database backup files.
* Local restore from the backup database image.
* Restore the database backup file using FTP server information.
* Local backup from the current database image.
* Backup the database to the FTP server.
* Schedule the backups.
* Unschedule the backup database images.
* Retrieve schedule task
* Download the database using FTP server information.


> Note:
- Input configurations are applied only if the existing configuration is not same as the new configuration.
- IPv4/IPv6 server configurations are supported.

## **Supported Tasks**

| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve_all](#Configuration-variables-for-Retrieve-All-Database-task)|Retrive all DB Management information |retrieve_all|NA
|[delete](#Configuration-variables-for-delete-backup-image-task)|Delete the existing backup DB image from network element|delete|<image_name>|
|[delete_all](#Configuration-variables-for-delete-all-backup-images-task)|Delete all the existing backup DB images from network element|delete_all|NA|
|[local_restore](#Configuration-variables-for-local-restore-backup-image-task)|Initiate a restore from local DB backup|local_restore|<image_name>|
|[restore](#Configuration-variables-for-restore-backup-image-task)|Initiate a restore the backup DB images|restore|<ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrFilePath</li><li>run_id</li><li>file_mapping</li></ul>|
|[local_backup](#Configuration-variables-for-local-backup-image-task)|Initiate a local DB backup|local_backup|NA|
|[backup](#Configuration-variables-for-backup-DB-images-task)|Initiate a DB backup|backup|<ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrSecondaryFilePath</li><li>XfrFilePath</li><li>SimultaneousXfr</li><li>OVWRT</li><li>run_id</li><li>file_mapping</li></ul>|
|[schedule_backup](#Configuration-variables-for-schedule-backup-Database-task)|Schedule to perform Backup DB Management |schedule_backup|<ul><li>ftpfiletype</li><li>time</li><li>day</li><li>interval</li>
|[unschedule_backup](#Configuration-variables-for-unschedule-backup-Database-task)|Unschedule to perform Backup DB Management|unschedule_backup|NA
|[retrieve_schedule](#Configuration-variables-for-retrieve-schedule-backup-DB)|Retrieve schedule time of the Backup DB|retrieve_schedule|NA
|[download](#Configuration-variables-for-download-DB)|Download the DB images |download|<ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrFilePath</li><li>run_id</li><li>file_mapping</li></ul>
|||

## **Configuration variables to Retrieve Selected or all Database task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|dbName| | By default Database Names taken `ALL`|To retrieve all DB Images information|No|No|

## **Configuration variables to delete backup image task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|run_id| string| Unique ID for each run , used to determine file name|Uniqe ID for each run|Yes|No|
|file_mapping|object|Key is TID ,value is DB file name|Valid TID and DB file name|Yes|No|

## **Sample configuration file to delete backup task**

```yaml
db_configs:
  delete:
    run_id: "UNIQUE_RUN_ID" # File DB_UNIQUE_RUN_ID_NODENAME will be deleted from NE
    #file_mapping is used when you want to use diffrent DB names for diffrent NEs
    file_mapping:
      "XTC284": "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" # For NE1 , "DB_1823012220190829061801" file will be considered for cleanup
      "NE57": "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" # For NE1 , "DB_1823012220190829061802" file will be considered for cleanup
```

* See example file ["config/db_mgmt_config.yml"](../../playbooks/config/db_mgmt_config.yml)

## **Configuration variables to delete all backup images task**

> Note:
 - The Delete all task does not require any input configuration file.
 - If the DB contains backup images by default, it will delete all the images.

## **Configuration variables for local restore backup image task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|run_id| string| Unique ID for each run , used to determine file name|Uniqe ID for each run|Yes|No|
|file_mapping|object|Key is TID ,value is DB file name|Valid TID and DB file name|Yes|No|

## **Sample configuration file for local restore backup task**

```yaml
db_configs:
  # Number of times to retry the restore check after the restore operation has been initiated
  check_restore_retries: 15
  # Delay in seconds between each retry
  check_restore_delay: 2
  # Delay in minutes after the restore operation has been initiated
  # Common for restore, and restore_local tasks
  restore_pause: 10
  restore:
    run_id: "UNIQUE_RUN_ID" # File DB_UNIQUE_RUN_ID_NODENAME will be deleted from NE
    #file_mapping is used when you want to use diffrent DB names for diffrent NEs
    file_mapping:
      "XTC284": "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" # For NE1 , "DB_1823012220190829061801" file will be considered for cleanup
      "NE57": "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" # For NE1 , "DB_1823012220190829061802" file will be considered for cleanup
```

* See example file ["config/db_mgmt_config.yml"](../../playbooks/config/db_mgmt_config.yml)

## **Configuration variables for restore backup image task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ftpfiletype| string| For restore, the file type should be `RestoreXfr`|RestoreXfr|Yes|No|
|XfrPrimaryIp46|string|Valid primary Ipv4 address| Ipv4 address Example: `10.220.13.23`|Yes|No|
|PrimarySftpPort|string|Valid port number has to give|Range from 1 to 65535|Yes|No|
|XfrPrimaryUser|string|Primary user name should be valid which is assigned for Ip address||Yes|No|
|XfrPrimaryPasswd|string|Primary password should set||Yes|No|
|XfrFilePath|string|Should set valid file path while tansfer the restore DB image|Ex: Path/of/the/file|Yes|No|
|run_id|string| Unique ID for each run , used to determine file name|Uniqe ID for each run|Yes|No|
|file_mapping|object|Key is TID ,value is DB file name|Valid TID and DB file name|Yes|No|

> Note for restore DB image
 - Either of the parameters highlighted in the closed bracket is required [`run_id` or `file_mapping`]. 
 - It is manadatory to configure the remaining parameters and initateTransfer (or download) the DB images.

## **Sample configuration file for restore backup task**

```yaml
db_configs:
  # Number of times to retry the restore check after the restore operation has been initiated
  check_restore_retries: 15
  # Delay in seconds between each retry
  check_restore_delay: 2
  # Delay in minutes after the restore operation has been initiated
  # Common for restore, and restore_local tasks
  restore:
    ftpfiletype: RestoreXfr
    XfrPrimaryIp46: "10.220.5.39"
    PrimarySftpPort: "22"
    XfrPrimaryUser: "primary_user"
    XfrPrimaryPasswd: "primary_passwd" # Primary password should use ansible-vault
    XfrFilePath: "Backup_dbbkups/"
    run_id: "UNIQUE_RUN_ID" # File will be saved as DB_UNIQUE_RUN_ID_NODENAME
    #file_mapping is used when you want to use diffrent DB names for diffrent NEs
    file_mapping:
      "XTC284": "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" # For NE1 , "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" file will be considered for backup
      "NE57": "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" # For NE2 , "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" file will be considered for backup
  initateTransfer:
    ftpfiletype: RestoreXfr
```

* See example file ["config/db_mgmt_restore_config.yml"](../../playbooks/config/db_mgmt_restore_config.yml)

## **Configuration variables for local backup image task**

>
> * No input config file is required for local_backup task
>
> * By default, if the DB contains current image then it catches the DB image and perform the local_backup task.

## **Configuration variables for backup DB images task**

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

## **Sample configuration file for backup DB images task**

```yaml
db_configs:
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

* See example file ["config/db_mgmt_backup_config.yml"](../../playbooks/config/db_mgmt_backup_config.yml)

## **Configuration variables for schedule backup database task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ftpfiletype| string| To schedule the backup, the file type should be `BackupXfr`|BackupXfr|Yes|No|
|interval|string|To schedule the BackupXfr, we need to setup the interval to take the backup of DB images.Case Sensitive|<ul><li>Daily</li><li>Weekly</li></ul>|Yes|Yes|
|day|string|To schedule the BackupXfr, we need to setup on which day to take the backup of DB images. Not case sensitive|<ul><li>sunday</li><li>monday</li><li>tuesday</li><li>wednesday</li><li>thrusday</li><li>friday</li><li>saturday</li></ul>|Yes|No|
|time|string|To setup the time in this format `01:00`||Yes|No|
|

## **Sample configuration file for schedule backup DB**

```yaml
db_configs:
  schedule_backup:
    ftpfiletype: BackupXfr
    interval: "Weekly" # Supported values are 'Daily' and 'Weekly'. Not case sensitive.
    day: "monday" # Supported values are 'monday', 'tuesday', 'wednesday', 'thursday', 'friday, 'saturday', and 'sunday'. Not case sensitive.
    time: "01:00" # supported value for time is 23:00, 01:00, 00:00
```

* See example file ["config/db_mgmt_backup_config.yml"](../../playbooks/config/db_mgmt_backup_config.yml)

>
> The `HH:MM` time string should be enclosed in quotes and the `MM` values are `0,15,30,45`
>
> If the schedule interval is `Daily` then day is not required(Command will not be executed)
>
> Schedule task will use FTP server details from `backup` section from config file , to configure FTP server.Currently only primary_server is supported.
>
> Few of the variables aren't required for some cases; it is left to the user to ensure the presence of the required variables for the requried tasks
>
> NOTE: It is left to the user to give the appropriate input:
> No `day` to be specified when `interval` is defined to be `Daily`
>
>The `day` to be specified when the `interval` is `Weekly`
>
> The validation merely validates the values against the schema and not any interdependencies(day-interval)
>
> If the user provides `day` when the `interval` is defined to be `Daily`, or does not provide a day when the `interval` is defined to be `Weekly` then the schedule task is executed and the corresponding XML error is shown to the user

## **Configuration variables for unschedule backup Database task**

>
> This unschedule_backup task is used for the BackupXfr. By default, the interval has taken as `Never` and then intitate transfer.
>

## **Configuration variables for retrieve schedule backup DB**

>
> No configuration variables are required. Run the playbook with the specified name to retrieve the schedule backup interval.

## **Configuration variables for download DB**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ftpfiletype| string| For dowwnload DB, the file type should be  one of the XFR ftp file type|SwDownloadXfr, fdbXfr, DebugInfoXFR|Yes|No|
|XfrPrimaryIp46|string|Valid primary Ipv4 address| Ipv4 address Example: `10.220.13.23`|Yes|No|
|PrimarySftpPort|string|Valid port number has to give|Range from 1 to 65535|Yes|No|
|XfrPrimaryUser|string|Primary user name should be valid which is assigned for Ip address||Yes|No|
|XfrPrimaryPasswd|string|Primary password should set||Yes|No|
|XfrFilePath|string|Should set valid file path while tansfer the restore DB image|Ex: Path/of/the/file|Yes|No|
|run_id|string| Unique ID for each run , used to determine file name. Valid values for run_id is SwDownloadXfr and DebugInfoXfr|Unique ID for each run|Yes|No|
|file_mapping|object|Key is TID ,value is DB file name. Valid values for file_mapping is SwDownloadXfr and DebugInfoXfr|Valid TID and DB file name|Yes|No|

> Note for download DB
 - Download task can use different XFR to download the DB images after configuration.
   For example, if you need to run  another XFR like DebugInfoXfr or fdbXfr, change the ftpfiletype in the sample configuration file.

## **Sample configuration file for download restore backup task**

```yaml
db_configs:
  # Number of times to retry the restore check after the restore operation has been initiated
  check_restore_retries: 15
  # Delay in seconds between each retry
  check_restore_delay: 2
  configure:
    ftpfiletype: RestoreXfr
    XfrPrimaryIp46: "10.220.5.39"
    PrimarySftpPort: "22"
    XfrPrimaryUser: "primary_user"
    XfrPrimaryPasswd: "primary_passwd" # Primary password should use ansible-vault
    XfrFilePath: "Backup_dbbkups/"
    run_id: "UNIQUE_RUN_ID" # File will be saved as DB_UNIQUE_RUN_ID_NODENAME
    #file_mapping is used when you want to use diffrent DB names for diffrent NEs
    file_mapping:
      "XTC284": "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" # For NE1 , "DB_UNIQUE_RUN_ID_CUSTOM_FILE1" file will be considered for backup
      "NE57": "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" # For NE2 , "DB_UNIQUE_RUN_ID_CUSTOM_FILE2" file will be considered for backup
  initateTransfer:
    ftpfiletype: RestoreXfr
```

* See example file ["config/db_mgmt_restore_config.yml"](../../playbooks/config/db_mgmt_restore_config.yml)

## **Sample DB Management [playbook](../../playbooks/db_mgmt_playbook.yml)**

```yaml
---

# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to DB Mgmt.. Please refer roles/db_mgmt/README.md
# For input configurations please refer Or edit group_vars/db_mgmt_config.yml

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

    # To Retrieve DB Management information from NE
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: retrieve_all
      tags: retrieve_all

    # To delete one of the database
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: delete
      tags: delete

    # To delete one of the database
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: delete_all
      tags: delete_all

    # To take local backup of the database
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: local_backup
      tags: local_backup

    # To restore the database
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: local_restore
      tags: local_restore

    # To schedule the backup database
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: schedule_backup
      tags: schedule_backup

    # To unschedule the backup database
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: unschedule_backup
      tags: unschedule_backup

    # To retrieve the backup schedule
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: retrieve_schedule
      tags: retrieve_schedule

    # To take the backup of the database  
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: backup
      tags: backup

    # To restore the database  
    - import_role:
       name: infinera.iqnos.db_mgmt
      vars:
        task_name: restore
      tags: restore
  
    # To download the DB  
    - import_role:
        name: infinera.iqnos.db_mgmt
      vars:
        task_name: download
      tags: download
```

## **Commands to run the DB Management Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * `ansible-playbook db_mgmt_playbook.yml -e "cfg_file=config/db_mgmt_config.yml host_group=DB_MGMT" -i myhosts.yml
`
## **Commands to run the Playbook with tags for each of the DB Management tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * `ansible-playbook db_mgmt_playbook.yml -e "cfg_file=config/db_mgmt_config.yml  host_group=DB_MGMT" -i myhosts.yml --tags=retrieve_all
`
> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
