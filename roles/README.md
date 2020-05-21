# Ansible Collection - infinera.iqnos

Infinera Network IQNOS Collection comprises of Ansible network roles and modules that extend benefits of simple, agentless automation to network administrators. Infinera Network Automation Suite can configure a network, test and validate existing network state, discover and rectify network configuration drift. It supports a wide range of device types,and actions to manage the entire deployed Infinera network. 



## **Infinera Ansible Roles**

The Infinera Ansible roles provide a framework for independent or interdependent collection of tasks or files. 
These roles simplify a complex playbook into multiple files.

Infinera provides the following [**Ansible roles**](./README.md) to perform operations on Infinera network elements running **IQ NOS Version R18.2.10.**
These ansible roles and modules use XML over SSH protocol for secure network element connectivity. 

1. [Network Element Configurations](./ne_config/README.md)
2. [Network Element Equipment Configurations](./eqpt/README.md)
3. [Network Element Facility Configurations](./facility/README.md)
4. [Network Element Security Configurations](./security/README.md)
5. [Network Element User Configurations](./user/README.md)
6. [Network Element Maintenance](./maintenance/README.md)
7. [Network Element Database Management](./db_mgmt/README.md)
8. [Network Element Software Management](./sw_mgmt/README.md)
9. [Network Element File Transfer Management](./xfr/README.md)
10. [Network Element User Session Management](./session/README.md)
11. [Network Element Call Home Configurations](./call_home/README.md)
12. [Network Element NTP Configurations](./ntp/README.md)
13. [Network Element Controller Card Configurations](./controller/README.md)
14. [Alarm Severity Settings Assignment Profiles - ASAP](./asap/README.md)
15. [Network Element Chassis Configurations](./chassis/README.md)
16. [Network Element Default Template Configurations](./default_template/README.md)
17. [Miscellaneous Configurations](./misc/README.md)


With Infinera Network Automation Suite a user can:
* Use these roles and create custom playbooks.
* Use sample playbooks provided by Infinera.

## The following documentation is available for Ansible Network Automation

* [Installation Guide](../install/InstallationGuide.md)
* [Full Upgrade](../docs/FullUpgrade.md)
* [Using Ansible Vault for  Data Encryption](../docs/DataEncryption.md)
* [Task Output](../docs/TaskOutput.md)
* [License information](../docs/License.md)
