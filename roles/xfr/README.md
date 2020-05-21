# **Ansible Role for XFR**
Ansible Role for XFR is used to retrieve a specified XFR configuration or all the XFR configuration information, configure the XFR FTP server details for the network elements.

## **Pre-requisites and conditions for the Ansible role XFR**

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/xfr_config.yml) "config/xfr_config.yml.yml".

## **User tasks and capabilities**

* Retrieve the existing specified XFR configuration or retrieve all the existing configuration of XFR.
* Edit or Configure the existing XFR configuration.
* Validate the configurations periodically.

> Note:
  - Input configurations are applied only if the existing configuration is not same as new configuration.
  - IPv4/IPv6 server configurations are supported.

### **FileType Supported parameters**

| FTP Input | Parameters Supports|
|----|------------|----|
|BackupXfr| <ul><li>AdministrativeState</li><li>AlarmReportControl</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrPrimaryPasswordEncrypted</li><li>XfrSecondaryPasswordEncrypted</li><li>XfrScheduleInterval</li><li>XfrScheduleOffset</li><li>XfrFileName</li><li>XfrSecondaryFileName</li><li>XfrFilePath</li><li>XfrSecondaryFilePath</li><li>XfrTimeOfDay</li><li>XfrDayOfWeek</li><li>OVWRT</li><li>SimultaneousXfr</li><li>BackUpInTechSupport</li></ul>|
|DebugInfoXfr|<ul><li>AdministrativeState</li><li>AlarmReportControl</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrPrimaryPasswordEncrypted</li><li>XfrScheduleInterval</li><li>XfrScheduleOffset</li><li>XfrFileName</li><li>XfrFilePath</li><li>XfrTimeOfDay</li><li>XfrDayOfWeek</li><li>AidList</li><li>DebugXfrCollectFilter</li><li>OVWRT</li><li>BackUpInTechSupport</li></ul>|
|OtdrResultXfr|<ul><li>AdministrativeState</li><li>AlarmReportControl</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrPrimaryPasswordEncrypted</li><li>XfrSecondaryPasswordEncrypted</li><li>XfrScheduleInterval</li><li>XfrScheduleOffset</li><li>XfrFilePath</li><li>XfrSecondaryFilePath</li><li>OVWRT</li><li>SimultaneousXfr</li><li>BackUpInTechSupport</li></ul>|
|RestoreXfr, SwDownloadXfr|<ul><li>AdministrativeState</li><li>AlarmReportControl</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrPrimaryPasswordEncrypted</li><li>XfrScheduleInterval</li><li>XfrScheduleOffset</li><li>XfrFileName</li><li>XfrFilePath</li><li>OVWRT</li><li>BackUpInTechSupport</li></ul>|
|fdbXfr|<ul><li>AdministrativeState</li><li>AlarmReportControl</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrPrimaryPasswordEncrypted</li><li>XfrSecondaryPasswordEncrypted</li><li>XfrScheduleInterval</li><li>XfrScheduleOffset</li><li>XfrFilePath</li><li>AidList</li><li>OVWRT</li><li>BackUpInTechSupport</li></ul>|
|pmXfr15min, pmXfr24hr|<ul><li>AdministrativeState</li><li>AlarmReportControl</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrPrimaryPasswordEncrypted</li><li>XfrSecondaryPasswordEncrypted</li><li>XfrScheduleInterval</li><li>XfrScheduleOffset</li><li>XfrFileName</li><li>XfrSecondaryFileName</li><li>XfrFilePath</li><li>XfrSecondaryFilePath</li><li>XfrTimeOfDay</li><li>XfrDayOfWeek</li><li>OVWRT</li><li>SimultaneousXfr</li></ul>||

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[retrieve](#Configuration-variables-for-Retrieve-Selected-or-All-XFR-task)|Retrieve XFR for the object specified by ftpfiletype [or] Retrive all XFR configuration information by providing ftpfiletype value as `ALL`|retrieve|Mandatory:<ul><li>ftpfiletype</li></ul>|
|[configure](#Configuration-variables-for-configure-XFR-task)|Configure the XFR as per the input parameters provided.|configure/edit|Mandatory:<ul><li>ftpfiletype</li></ul>Anyone parameter:<ul><li>AdministrativeState</li><li>AlarmReportControl</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrPrimaryPasswordEncrypted</li><li>XfrSecondaryPasswordEncrypted</li><li>XfrScheduleInterval</li><li>XfrScheduleOffset</li><li>XfrFileName</li><li>XfrSecondaryFileName</li><li>XfrFilePath</li><li>XfrSecondaryFilePath</li><li>XfrTimeOfDay</li><li>XfrDayOfWeek</li><li>LastXfrType</li><li>AidList</li><li>DebugXfrCollectFilter</li><li>OVWRT</li><li>SimultaneousXfr</li><li>BackUpInTechSupport</li></ul>|
|[restore](#Configuration-variables-for-restore-backup-image-task)|Initiate a restore the backup DB images|restore|<ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrFilePath</li><li>run_id</li><li>file_mapping</li></ul>|
|[backup](#Configuration-variables-for-backup-DB-images-task)|Initiate a DB backup|backup|<ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrSecondaryIp46</li><li>SecondarySftpPort</li><li>XfrSecondaryUser</li><li>XfrSecondaryPasswd</li><li>XfrSecondaryFilePath</li><li>XfrFilePath</li><li>SimultaneousXfr</li><li>OVWRT</li><li>run_id</li><li>file_mapping</li></ul>|
|[schedule_backup](#Configuration-variables-for-schedule-backup-Database-task)|Schedule to perform Backup DB Management |schedule_backup|<ul><li>ftpfiletype</li><li>time</li><li>day</li><li>interval</li>
|[unschedule_backup](#Configuration-variables-for-unschedule-backup-Database-task)|Unschedule to perform Backup DB Management|unschedule_backup|NA
|[retrieve_schedule](#Configuration-variables-for-retrieve-schedule-backup-DB)|Retrieve schedule time of the Backup DB|retrieve_schedule|NA
|[download](#Configuration-variables-for-download-DB)|Download the SW images |download|<ul><li>ftpfiletype</li><li>XfrPrimaryIp46</li><li>PrimarySftpPort</li><li>XfrPrimaryUser</li><li>XfrPrimaryPasswd</li><li>XfrFilePath</li><li>run_id</li><li>file_mapping</li></ul>
|||
|||||

## **Configuration variables for Retrieve Selected or All XFR task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ftpfiletype| string| Valid file types that can be transferred|<ul><li>ALL(To retrieve all XFR configuration information)</li><li>BackupXFR</li><li>DebugInfoXfr</li><li>OtdrResultXfr</li><li>RestoreXfr</li><li>SwDownloadXfr</li><li>fdbXfr</li><li>pmXfr15min</li><li>pmXfr24hr</li></ul>|Yes|Yes|
|

### **Sample configuration file for Retrieve Specific XFR Task**

```yaml
xfr_configs:
  retrieve:
    ftpfiletype: BackupXfr
```

## **Configuration variables for configure/edit XFR task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|ftpfiletype| string| Valid file types that can be transferred|<ul><li>BackupXFR</li><li>DebugInfoXfr</li><li>OtdrResultXfr</li><li>RestoreXfr</li><li>SwDownloadXfr</li><li>fdbXfr</li><li>pmXfr15min</li><li>pmXfr24hr</li></ul>|Yes|Yes|
|AdministrativeState| string| Valid Administrative State input. Default value is `UnLocked`|<ul><li>Locked</li><li>Maintenance</li><li>UnLocked</li></ul>|No|Yes|
|AlarmReportControl| string| Valid Alarm Report control input. Default value is `Allowed` |<ul><li>Allowed</li><li>Inhibited</li></ul>|No|Yes|
|XfrPrimaryIp46| string| Valid Xfr primary Ipv4 and Ipv6 address |<ul></ul>|No|Yes|
|PrimarySftpPort| integer| Valid primary sftp port. Default value is `22` |<ul>0-65535</ul>|No|Yes|
|XfrSecondaryIp46| string| Valid Xfr Secondary Ipv4 and Ipv6 address |<ul></ul>|No|Yes|
|SecondarySftpPort| integer| Valid secondary sftp port. Default value is `22` |<ul>0-65535</ul>|No|Yes|
|XfrPrimaryUser| string| Valid Xfr primary user |<ul><=64</ul>|No|Yes|
|XfrPrimaryPasswd| string| Valid Xfr primary password |<ul><=128</ul>|No|Yes|
|XfrSecondaryUser| string| Valid Xfr secondary user |<ul><=64</ul>|No|Yes|
|XfrSecondaryPasswd| string| Valid Xfr Secondary password |<ul><=128</ul>|No|Yes|
|XfrPrimaryPasswordEncrypted| string| Valid Xfr primary password encrypted. Default value is `False`|<ul>True/False</ul>|No|Yes|
|XfrSecondaryPasswordEncrypted| string| Valid Xfr Secondary password encrypted. Default value is `False` |<ul>True/False</ul>|No|Yes|
|XfrScheduleInterval| string| Valid Xfr schedule interval. Default value is `Never` |<ul><li>Never</li><li>Weekly</li><li>Daily</li><li>Hour_12</li><li>Hour_8</li><li>Hour_4</li><li>Hour_1</li><li>Min_15</li></ul>|No|Yes|
|XfrScheduleOffset| integer| Valid Xfr schedule offset. Default value is `0` |<ul></ul>|No|Yes|
|XfrFileName| string| Valid Xfr primary filename |<ul></ul>|No|Yes|
|XfrSecondaryFileName| string| Valid Xfr Secondary filename |<ul></ul>|No|Yes|
|XfrFilePath| string| Valid Xfr primary filepath |<ul></ul>|No|Yes|
|XfrSecondaryFilePath| string| Valid Xfr Secondary filepath |<ul></ul>|No|Yes|
|XfrTimeOfDay| integer | Valid Xfr time of the day. Default value is `0` |<ul></ul>|No|Yes|
|XfrDayOfWeek| integer | Valid Xfr day of that particular week. Default value is `0` |<ul></ul>|No|Yes|
|LastXfrType| string | Valid last Xfr type. Default is `NA` |<ul><li>Manual</li><li>Scheduled</li><li>NA</li></ul>|No|Yes|
|AidList| string | Valid aid list |<ul><=260</ul>|No|Yes|
|DebugXfrCollectFilter| string | Valid debug xfr collect filter. Default value is `Default` |<ul><li>Default</li><li>CompleteOlmDspFdr</li><li>CompleteGmplsData</li></ul>|No|Yes|
|OVWRT| boolean | Valid OVWRT. Default value is `true` |<ul></ul>|No|Yes|
|SimultaneousXfr| boolean | Valid simultaneous xfr. Default value is `false` |<ul></ul>|No|Yes|
|BackUpInTechSupport| boolean | Valid backup support. Default value is `false` |<ul></ul>|No|Yes|
|||||


### **Sample Configuration file for Configure XFR Task**

```yaml
xfr_configs:
  configure:
    ftpfiletype: BackupXfr
    # Need anyone of the parameters
    AdministrativeState: UnLocked
    AlarmReportControl: Allowed
    XfrPrimaryIp46: "10.220.5.39"
    PrimarySftpPort: "22"
    XfrSecondaryIp46: "10.220.5.35"
    SecondarySftpPort: "22"
    XfrPrimaryUser: "primary_user"
    XfrPrimaryPasswd: "primary_passwd"
    XfrSecondaryUser: "secondary_user"
    XfrSecondaryPasswd: "secondary_passwd"
    XfrPrimaryPasswordEncrypted: "False"
    XfrSecondaryPasswordEncrypted: "False"
    XfrScheduleInterval: Never
    XfrScheduleOffset: 0
    XfrFileName: "Backupinfo_DB_%D_%M"
    XfrSecondaryFileName: "MyDB_%D"
    XfrFilePath: "Backup_primary_dbbkups/"
    XfrSecondaryFilePath: "Backupsecondary_dbbbk2/"
    DebugXfrCollectFilter: Default
    SimultaneousXfr: "false"
    BackUpInTechSupport: "false"
```

### **List of examples for different XFR File types**

* See example file ["config/xfr_BackupXfr_config.yml"](../../playbooks/config/xfr_BackupXfr_config.yml)
* See example file ["config/xfr_DebugInfoXfr_config.yml"](../../playbooks/config/xfr_DebugInfoXfr_config.yml)
* See example file ["config/xfr_fdbXfr_config.yml"](../../playbooks/config/xfr_fdbXfr_config.yml)
* See example file ["config/xfr_OtdrResultXfr_config.yml"](../../playbooks/config/xfr_OtdrResultXfr_config.yml)
* See example file ["config/xfr_pmXfr24hr_config.yml"](../../playbooks/config/xfr_pmXfr24hr_config.yml)
* See example file ["config/xfr_pmXfr15min_config.yml"](../../playbooks/config/xfr_pmXfr15min_config.yml)
* See example file ["config/xfr_RestoreXfr_config.yml"](../../playbooks/config/xfr_RestoreXfr_config.yml)
* See example file ["config/xfr_SwDownloadXfr_config.yml"](../../playbooks/config/xfr_SwDownloadXfr_config.yml)

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
xfr_configs:
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

* See example file ["config/xfr_db_restore_config.yml"](../../playbooks/config/xfr_db_restore_config.yml)

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
xfr_configs:
  check_backup_retries: 15
  check_backup_delay: 2
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

* See example file ["config/xfr_db_backup_config.yml"](../../playbooks/config/xfr_db_backup_config.yml)

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
xfr_configs:
  schedule_backup:
    ftpfiletype: BackupXfr
    interval: "Weekly" # Supported values are 'Daily' and 'Weekly'. Not case sensitive.
    day: "monday" # Supported values are 'monday', 'tuesday', 'wednesday', 'thursday', 'friday, 'saturday', and 'sunday'. Not case sensitive.
    time: "01:00" # supported value for time is 23:00, 01:00, 00:00
```

* See example file ["config/xfr_db_backup_config.yml"](../../playbooks/config/xfr_db_backup_config.yml)

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
xfr_configs:
  # Number of times to retry the restore check after the restore operation has been initiated
  check_download_retries: 15
  # Delay in seconds between each retry
  check_download_delay: 2
  download:
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

* See example file ["config/xfr_db_restore_config.yml"](../../playbooks/config/xfr_db_restore_config.yml)

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
xfr_configs:
  # Number of times to retry the restore check after the restore operation has been initiated
  check_download_retries: 15
  # Delay in seconds between each retry
  check_download_delay: 2
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

* See example file ["config/xfr_sw_download_config.yml"](../../playbooks/config/xfr_sw_download_config.yml)

## **Sample XFR server [playbook](../../playbooks/xfr_playbook.yml)**

```yaml
# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to XFR Please refer roles/xfr/README.md
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
  
      # To retrieve a specified XFR config such as BackupXfr, or all the XFR configuration by providing the value `ALL` for the specified entity,
      # Please refer to retrieve of config/xfr_config.yml
    - import_role:
        name: infinera.iqnos.xfr
      vars:
        task_name: "retrieve"
      tags: retrieve

      # To configure specific ftpfiletype with the following input fields,
      # Please refer to retrieve of config/xfr_config.yml
    - import_role:
        name: infinera.iqnos.xfr
      vars:
        task_name: "configure"
      tags: configure

    # To schedule the backup database
    - import_role:
        name: infinera.iqnos.xfr
      vars:
        task_name: schedule_backup
      tags: schedule_backup

    # To unschedule the backup database
    - import_role:
        name: infinera.iqnos.xfr
      vars:
        task_name: unschedule_backup
      tags: unschedule_backup

    # To retrieve the backup schedule
    - import_role:
        name: infinera.iqnos.xfr
      vars:
        task_name: retrieve_schedule
      tags: retrieve_schedule

    # To backup the DB  
    - import_role:
        name: infinera.iqnos.xfr
      vars:
        task_name: backup
      tags: backup

    # To restore the DB  
    - import_role:
       name: infinera.iqnos.xfr
      vars:
        task_name: restore
      tags: restore

    # To download the SW and DB images 
    - import_role:
        name: infinera.iqnos.xfr
      vars:
        task_name: download
      tags: download
```

## **Commands to run the XFR Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook xfr_playbook.yml -e "cfg_file=config/xfr_config.yml host_group=XFR" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the XFR tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/xfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook xfr_playbook.yml -e "cfg_file=config/xfr_config.yml  host_group=XFR" -i myhosts.yml --tags=retrieve


> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
