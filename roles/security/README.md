# **Ansible Role for Security**

Ansible Role Security is used to Retrieve the Security Profile Attributes configuration information, configure the Security Profile Attributes, perform operations on RADIUS Server Settings, and ACFs on the network elements, and Operations on Certificates.

## **Pre-requisites and conditions for the Ansible role Security Profile Attributes

* Host file lists all the network elements and its details. See example [file](../../playbooks/inventory/hosts.yml) "hosts.yml".
* Host group specifies the group of hosts to be used. See example "my_group" in "hosts.yml" [file](../../playbooks/inventory/hosts.yml).
* Input parameter file lists the inputs required to run tasks within this role. See example [file](../../playbooks/config/security_config.yml) "config/security_config.yml".

## **User tasks and capabilities**

* Retrieve existing Security Profile Attributes configuration.
* Edit/Configure the existing Security Profile Attributes configuration.
* Validate the configurations periodically.
* Retrieve ACFs.
* Create new ACFs.
* Delete ACFs.
* Clear ACF Counters.
* Retrieve RADIUS Servers' Settings.
* Edit RADIUS Servers' settings.
* Retrieve Authentication Policy.
* Edit Authentication Policy.
* Retrieve Local Certificates.
* Configure Local Certificates.
* Delete Local Certificates.
* Set TLS Certificates and Attributes.
* Dissociate stale(old deleted one) TLS certificate.

## **Supported Tasks**
  
| Tasks | Description| Tags | Input|
|----|------------|----|----|
|retrieve|Retrieve the Security Profile Attributes for the object |retrieve|NA|
|retrieve_https|Retrieve HTTPS configuration of  Security Profile object |retrieve_https|NA|
|retrieve_ssh_keys|Retrieve the SSHFingerprint and SSHPublicKey. |retrieve_ssh_keys|NA|
|regen_ssh_key|Regenerate the SSH Key on Network Element |retrieve|NA|
|[configure](#Configuration-variables-for-configure/edit-Security-Profile-Attributes-task)|Configure the Security Profile Attributes as input parameters are given.|configure||
|retrieve_radius|Retrieve RADIUS Servers' Settings|retrieve_radius|NA|
|[edit_radius](#Configuration-variables-for-edit_radius-task)|Edit RADIUS Servers' Settings|edit_radius|See Configuration Below|
|retrieve_auth_policy|Retrieve the Authentication Policy|retrieve_auth_policy|NA|
|[edit_auth_policy](#Configuration-variables-for-edit_auth_policy-task)|Edit the Authentication Policy|edit_auth_policy|See Configuration Below|
|retrieve_acf|Retrieve ACFs|retrieve_acf|NA|
|clear_acf|Clear the ACF Counters|clear_acf|NA|
|[create_acf](#Configuration-variables-for-Create-ACF-task)|Create new ACFs|create_acf|See Configuration Below|
|[delete_acf](#Configuration-variables-for-Delete-ACF-task)|Delete ACFs present on the Network Element|delete_acf|See Configuration Below|
|[set_tls_certificate](#Configuration-variables-for-Set-TLS-Certificate-task)|Set the TLS certificate to a certificate configured on the network element of the Node Controller Chassis|set_tls_certificate|security_config.set_tls_certificate|
|dissociate_tls_certificate|Set the TLS certificate to NONE. This is useful in the cases where there is a stale certificate associated with the TLS Attribute. A stale certificate in this context refers to a certificate that is deleted.|dissociate_tls_certificate|NA|
|[configure_certificate](#Configuration-variables-for-Configure-Certificate-task)|Configure X.509 certificates on the network element of the Node Controller Chassis |configure_certificate|security_config.configure_certificate|
|[delete_certificate](#Configuration-variables-for-Delete-Certificate-task)|Delete the X.509 certificates configured on the network element of the Node Controller Chassis |delete_certificate|security_config.delete_certificate|
|retrieve_certificate|Retrieve the X.509 certificates configured on the network element |retrieve_certificate|NA|
|||||

## **Configuration variables for configure/edit Security Profile Attributes task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|HttpSecurityEnforced|string|true or false |<ul><li>true or false</li></ul>|No|No|
|

### **Configuration of HTTPS**

* To enable HTTPS, set HttpSecurityEnforced value to true
* To disable HTTPS, set HttpSecurityEnforced value to false
* See example file "config/security_https_config.yml"

## **Configuration variables for `edit_radius` task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|RadiusServerIpAddress46|Indicates which RADIUS server to edit|RadiusServerIpAddress46|IPv4/IPv6 address|Server IP Address |Valid IP Address|No|No|
|RadiusServerAuthPort|integer|Authentication port number |1024 to 65535|No|No|
|MaxAttemptsPerServer|integer|true or false |1 to 5|No|No|
|RadiusServerRequestTimeOut|integer|Authentication request time out in `seconds` |1 to 90|No|No|
|RadiusServerSharedSecret|string|Shared Secret Key |16 to 128 characters long|No|No|
|||||||

> The keys `RADSERVER1`, `RADSERVER2`, `RADSERVER3` correspond to the RADIUS server whose settings are going to be modified
>
> They are required to be in the same format as shown below

### **Sample Configuration File**

```yaml
security_configs:
  edit_radius:
    RADSERVER1:
      RadiusServerIpAddress46: 10.220.154.32
      RadiusServerAuthPort: 1812
      MaxAttemptsPerServer: 3
      RadiusServerRequestTimeOut: 12
      RadiusServerSharedSecret: sharedsecfretkey
    RADSERVER2:
      RadiusServerIpAddress46: 10.220.15.32
      RadiusServerAuthPort: 1812
      MaxAttemptsPerServer: 2
      RadiusServerRequestTimeOut: 11
      RadiusServerSharedSecret: sharedsaecretkey
    RADSERVER3:
      RadiusServerIpAddress46: 10.20.154.32
      RadiusServerAuthPort: 1818
      MaxAttemptsPerServer: 1
      RadiusServerRequestTimeOut: 18
      RadiusServerSharedSecret: sharedsecrewtkey
```

## **Configuration variables for `edit_auth_policy` task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|AuthenticationPolicy|string|`user` authentication policy|<ul><li>LOCAL</li><li>RADIUS</li><li>RFL</li></ul>|Yes|No|
|||||||

* RFL - RADIUS Followed by Local

### **Sample Configuration File**

```yaml
security_configs:
  edit_auth_policy:
    AuthenticationPolicy: LOCAL
```

## **Configuration variables for Create ACF task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|MoId|string|The name (identifier) of Access Control Filter|Unique name with a maximum of 128 characters|Yes|No|
|Sequence| integer| A number that assigns priority to the rule | 200 to 4294967100 (in multiples of 100). The filter with sequence number 200 will be checked first|Yes|Yes|
|Interface| string| The interfaces to which the rule is applied|<ul><li>dcn</li><li>aux</li><li>igcc</li><li>gcc</li><li>all</li></ul> |Yes|Yes|
|Protocol| string| The protocol to which the rule is applied|<ul><li>tcp</li><li>udp</li><li>icmp</li><li>all</li></ul> |Yes|Yes|
|Direction| string| The direction of the packets to which the rule is applied|<ul><li>input</li><li>output</li></ul> |Yes|Yes|
|Operation| string| The operation (action) applied on the packets|<ul><li>accept</li><li>drop</li><li>reject</li><li>log</li></ul> |Yes|Yes|
|SourceIp| IPv4/IPv6 address| The source IP address filtered by the ACF| Valid IPv4/IPv6 address|No|Yes|
|SipPrefixLength| integer| The source IP address mask length |<ul><li>IPv4 - 1 to 32</li><li>IPv6 - 1 to 128</li></ul>  |No|No|
|SourcePort| integer| The source port number| 1 to 65535 |No|No|
|SourcePortRange| integer| The range of source ports filtered by the ACF (The range starts from the configured source port. A value of 0 means no range.) | 1 to 65535 |No|No|
|DestinationIp| IPv4/IPv6 address| The destination IP address filtered by the ACF| Valid IPv4/IPv6 address|No|Yes|
|DipPrefixLength| integer| The destination IP address mask length| <ul><li>IPv4 - 1 to 32</li><li>IPv6 - 1 to 128</li></ul>  |No|No|
|DestinationPort| integer| The destination port number| 1 to 65535 |No|No|
|DestinationPortRange| integer| The range of destination ports filtered by the ACF (The range starts from the configured dest port. A value of 0 means no range.) | 1 to 65535 |No|No|
|||||||

### **Sample Configuration File**

```yaml
security_configs:
  create_acf:
    XTC6655:
      - MoId: filter1
        AdministrativeState: UnLocked
        AlarmReportControl: Allowed
        Intf: igcc
        SourceIp: 10.22.5.39
        Protocol: tcp
        SipPrefixLength: 234
        DestinationIp: 10.220.4.99
        DipPrefixLength: 134
        SourcePort: 43
        SourcePortRange: 123
        DestinationPort: 12
        DestinationPortRange: 456
        Direction: output
        Operation: accept
        Sequence: 1200

      - MoId: filter2
        AdministrativeState: UnLocked
        AlarmReportControl: Inhibited
        Intf: aux
        SourceIp: 10.22.5.39
        Protocol: tcp
        SipPrefixLength: 234
        DestinationIp: 10.220.4.99
        DipPrefixLength: 134
        SourcePort: 43
        SourcePortRange: 123
        DestinationPort: 12
        DestinationPortRange: 456
        Direction: output
        Operation: accept
        Sequence: 300
```
*XTC6655 is node name
## **Configuration variables for Delete ACF task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|MoId List|list|The list of names (identifier) of Access Control Filter|Valid Names|Yes|No|
|||||||

### **Sample Configuration File**

```yaml
security_configs:
  delete_acf:
    NE57:
      - filter1
      - filter2
```

## **Configuration variables for Configure Certificate task**

| Variable | Type | Description|Valid Instances/Ranges|Mandatory|Validated|
| ----|------|------------|------------|----------|--------|
|MoId | string|The unique identifier assigned by the certificate authority| A string that is at-most 128 characters long|Yes|No|
|node_to_MoId_mapping| object| The Node TID to the certificate identifier mapping|Valid Mapping(See below)|No|No|
|CertificateFilePath| string| The complete path, including the file name and extension, to the certificate present on the Server | Valid path to the file as described(See below) | Yes|No|
|passphrase| string| The sensitive passphrase|A string|Yes|No|
|||||||

> The `MoId` variable is mandatory
>
> If the `node_to_MoId_mapping` is given then it will be used
>
> The `CertificateFilePath` is the absolute path of the certificate path that is present on the machine running ansible

### **Sample Configuration File**

```yaml
security_configs:
  configure_certificate:
    - MoId: Certificate1
      passphrase: sensitivepassphrase
      CertificateFilePath: /home/user1/certificates/server_certificate.p12
      node_to_MoId_mapping:
        "NE57": CertificateNE57
        "XTC286": Cert286

    - MoId: Certificate2
      passphrase: secretphrase
      CertificateFilePath: /home/user1/certificates/server_certificate2.p12
      node_to_MoId_mapping:
        "NE57": SecondCertificate
        "XTC286": SecondCert286
```

## **Configuration variables for Delete Certificate task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|MoId | string|The unique identifier assigned by the certificate authority| A string that is at-most 128 characters long|Yes|No|
|node_to_MoId_mapping| object| The Node TID to the certificate identifier mapping|Valid Mapping(See below)|No|No|
|||||||

> The `MoId` variable is mandatory
>
> If the `node_to_MoId_mapping` is given then it will be used

### **Sample Configuration File**

```yaml
security_configs:
  delete_certificate:
    - MoId: TLSCert1
      node_to_MoId_mapping:
        "NE57": TLSCertNE57
        "XTC286": TLS286

    - MoId: CertTLS2
      node_to_MoId_mapping:
        "NE57": TLSSecondCertificate
        "XTC286": TLSSecondCert286
```

## **Configuration variables for Set TLS Certificate task**

| Variable | Type | Description| Valid Instances| Mandatory| Validated|
|----|------|------------|----------------|----------|---------|
|TLSCertName | string|The unique identifier assigned by the certificate authority| A string that is at-most 128 characters long|Yes|No|
|node_to_TLSCertName_mapping| object| The Node TID to the certificate identifier mapping|Valid Mapping(See below)|No|No|
|TLSIdleTime|integer|The time in seconds up to which TLS session can be inactive|1 to 600. If no value is provided then a value of 180 is configured|No|No|
|TLSConnTime|integer|The time in seconds until which the TLS session can be active|0 to 172800. If no value is provided then a value of 86400 is configured. If 0 is given, the session never times out |No|No|
|||||||

> The `TLSCertName` variable is mandatory
>
> If the `node_to_TLSCertName_mapping` is given the it will be used

### **Sample Configuration File**

```yaml
security_configs:
  set_tls_certificate:
    TLSIdleTime: 550
    TLSConnTime: 8644
    TLSCertName: TLSSecondCertificate
    node_to_TLSCertName_mapping:
      "NE57": TLS57Cert
      "XTC286": TLS286Cert
```

## **Configuration variables for Disassociate TLS Certificate task**

> There are no input configuration variables for this task

## **Sample ne_config server [playbook](../../playbooks/security_playbook.yml)**

```yaml

# Requires ansible 2.9+
# This playbook is intended to help in performing the tasks related to Security Profile Configuration Please refer roles/security/README.md
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

      # To Retrieve the security.
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: retrieve
      tags: retrieve

      # To Retrieve the HTTPS configuration.
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: retrieve_https
      tags: retrieve_https

      # To Retrieve the SSHFingerprint and SSHPublicKey.
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: retrieve_ssh_keys
      tags: retrieve_ssh_keys

      # To Regenerate the SSH Key.
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: regen_ssh_key
      tags: regen_ssh_key

      # To configure the security
      # Please refer to cofigure of config/security_config.yml
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: configure
      tags: configure

    # To Retrieve Radius Servers
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: retrieve_radius
      tags: retrieve_radius

    # To Edit RADIUS Servers' Settigns the security
    # Please refer to edit_radius of config/security_radius_config.yml
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: edit_radius
      tags: edit_radius

    # To Retrieve Authentication Policy
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: retrieve_auth_policy
      tags: retrieve_auth_policy

    # To Edit Authentication Policy
    # Please refer to edit_auth_policy of config/security_radius_config.yml
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: edit_auth_policy
      tags: edit_auth_policy

    # To Retrieve ACFs
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: retrieve_acf
      tags: retrieve_acf

    # To Create ACFs
    # Please refer to create_acf of config/security_acf_config.yml
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: create_acf
      tags: create_acf

    # To Delete ACFs
    # Please refer to delete_acf of config/security_acf_config.yml
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: delete_acf
      tags: delete_acf

    # To Clear ACF Counters
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: clear_acf
      tags: clear_acf

    # To Retrieve Local Certificates
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: retrieve_certificate
      tags: retrieve_certificate

    # To Configure Local Certificates
    # Please refer to configure_certificate of config/security_certificate_config.yml
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: configure_certificate
      tags: configure_certificate

    # To Delete Local Certificates
    # Please refer to delete_certificate of config/security_certificate_config.yml
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: delete_certificate
      tags: delete_certificate

    # To Set TLS Certificate and Attributes
    # Please refer to set_tls_certificate of config/security_certificate_config.yml
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: set_tls_certificate
      tags: set_tls_certificate

    # To Disassociate old TLS Certificate Name
    - import_role:
        name: infinera.iqnos.security
      vars:
        task_name: dissociate_tls_certificate
      tags: dissociate_tls_certificate
```

## **Commands to run the ne_config Playbook**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory`
* *Example*
  * ansible-playbook security_playbook.yml -e "cfg_file=config/security_config.yml host_group=ne_config" -i myhosts.yml

## **Commands to run the Playbook with tags for each of the ne_cofig tasks**

* *Command*
  * `ansible-playbook name_of_playbook.yml -e "cfg_file=path_to_the/cfg_file.yml host_group=the_host_group" -i file_with_the_inventory --tags=tag1,tag2,tag3...`
* *Example*
  * ansible-playbook security_playbook.yml -e "cfg_file=config/security_config.yml  host_group=ne_config" -i myhosts.yml --tags=retrieve
  * ansible-playbook security_playbook.yml -e "cfg_file=config/security_radius_config.yml  host_group=radsgrp" -i myhosts.yml --tags=edit_radius
  * ansible-playbook security_playbook.yml -e "cfg_file=config/security_acf_config.yml  host_group=acfgrp" -i myhosts.yml --tags=delete_acf

> Note:
 - Providing a `host_group` along with the `cfg_file`(where applicable) is mandatory
 - If no inventory file is passed, then default inventory is `hosts`

## **License**

[BSD License 2.0](../../docs/License.md)

## **Author Information**

**Infinera Corporation**

**Technical Support - techsupport@infinera.com**
