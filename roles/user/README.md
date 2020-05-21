Ansible Role for USER
=========

Ansible Role for USER is used to change the default password for the  specified USER on the network elements.

Pre-requisites and conditions for the Ansible role USER
------------

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/user_config.yml) "config/user_config.yml".
* Encrypt the password variable in the user config file using ansible-vault
* To know about the Ansible vault refer this, [data encryption](./../../DataEncryption.md)
* To encrypted the string value use [Ansible-Vault](#Ansible-Vault)

Ansible-Vault
---

    * ansible-vault encrypt_string "input_value" --name input_key

User tasks and capabilities
------------

* Change the default password of User security.

Note
------------

    * Input configurations of USER senstive information has to encrypt using Ansible vault.

Supported Tasks
------------

| Tasks | Description| Tags | Input|
|----|------------|----|----|
|[change_passwd](#Configuration-variables-to-change-the-passsword-for-USER-security-task)|Change the default password of the specified or current user on the network element|change_passwd|Mandatory:<ul> <li>NewPassword</li></ul>|
|||||

Configuration variables to change the passsword for USER security task
-----------------

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|NewPassword| string| Password shoud have atleast one special character, one lower and upper case letter and one numeric value|Valid:<ul><li>Test@123</li><li>TEST@123</li></ul>Invalid:<ul><li>test@123</li></ul>|Yes|No|
|

Sample Configuration File for change default password for USER security Task
----------

```yaml
user_configs:
  change_passwd:
    # To change the default password of the specified user or the current user. 
    # Hide the password input using ansible vault.
    NewPassword: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          35336461353038653737663939646365613663333762653236326463306338663833653630336637
          3063333532613134653634643937306134386435636162610a646633316161363034356534663339
          63633533643637326464366237316461383362646162663635343238616333396239653432613932
          3335333938626261370a393634306433663231643764313761323034396338653538323535303535
          3139
```

Sample USER [playbook](../../playbooks/user_playbook.yml)
----------------

``` yaml
# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to USER Profile. Please refer roles/user/README.md
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

    # To change the password to the USER.
    # Please refer to retrieve of config/user_config.yml
    - import_role:
        name: infinera.iqnos.user
      vars:
        task_name: change_passwd
      tags: change_passwd
```

Commands to run the USER Playbook
------------------------

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook user_playbook.yml -e "cfg_file=config/user_config.yml host_group=XFR" -i myhosts.yml

Commands to run the Playbook with tags for each of the USER tasks
---------------------

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/xfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook user_playbook.yml -e "cfg_file=config/user_cfg.yml  host_group=USER" -i myhosts.yml --tags=change_passwd

* *Command to decrypted ansible-vault using playbook*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/xfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3... --ask-vault-pass`
* *Example*
  * ansible-playbook user_playbook.yml -e "cfg_file=config/user_cfg.yml  host_group=USER" -i myhosts.yml --tags=change_passwd --ask-vault-pass

> Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
>
> If no inventory file is passed, then default inventory is `hosts`

License
--------

BSD License 2.0

Author Information
---------

Infinera Corporation

Technical Support - techsupport@infinera.com
