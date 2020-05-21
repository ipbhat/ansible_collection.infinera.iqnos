# Ansible Collection - infinera.iqnos

Infinera Network IQNOS Collection comprises of Ansible network roles and modules that extend benefits of simple, agentless automation to network administrators. Infinera Network Automation Suite can configure a network, test and validate existing network state, discover and rectify network configuration drift. It supports a wide range of device types,and actions to manage the entire deployed Infinera network. 


## Infinera Ansible Roles

The Infinera Ansible roles provide a framework for independent or interdependent collection of tasks or files. 
These roles simplify a complex playbook into multiple files.

Infinera provides the following [**Ansible roles**](./README.md) to perform operations on Infinera network elements running IQ NOS Version R18.2.10.
These ansible roles and modules use TL1 over SSH protocol for secure network element          connectivity.

1. [RADIUS](./radius/README.md)
2. [NTP](./ntp/README.md)
3. [ACF](./acf/README.md)
4. [Security](./security/README.md)
5. [Database Management](./db_mgmt/README.md)
6. [Software Management](./sw_mgmt/README.md)
7. [Network Element Configurations](./NEConfig/README.md)
8. [Network Element Entity Configurations](./NEEntityConfig/README.md)
9. [Network Element Call Home](./ne_call_home/README.md)
10. [Alarm Severity Settings Assignment Profiles - ASAP](./asap/README.md)
11. [Active User Session Management](./session_mgmt/README.md)

With Infinera Network Automation Suite a user can:
* Use these roles and create custom playbooks.
* Use sample playbooks provided by Infinera.

## The following documentation is available for Ansible Network Automation

* [Installation Guide](./install/InstallationGuide.md)
* [Full Upgrade](./docs/FullUpgrade.md)
* [Using Ansible Vault for  Data Encryption](./docs/DataEncryption.md)
* [Task Output](./docs/TaskOutput.md)
* [License information](./docs/License.md)