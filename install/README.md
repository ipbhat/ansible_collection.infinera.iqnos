# Infinera Network Automation Module Installation Guide

This document provides detailed instructions to install the Infinera Network Automation modules and Ansible.

## Listed below are the  characteristic features of Ansible

- Ansible manages machines over the SSH protocol by default. 
- With Ansible installed, there is no database added and no daemons to start or keep running. 
- Installing Ansible on one machine, a laptop for instance, can manage an entire fleet of remote machines from a single laptop. 
- Ansible only manages remote machines, these machines do not need software installed, so there is no upgrade issue to a new version of Ansible.

### Control Node

 The device with Ansible installed is considered the control node.
 Ansible on the control node currently works on any machine with Python 3 (version 3.6 and higher) installed. Control nodes do not support Windows operating system.

### Managed Node

The devices managed with Ansible is considered the managed node. Here Managed node refers to IQNOS Nodes.


# Installation Procedure for IQNOS Collection

## Installing Python and Dependencies 
[Install Dependencies](./InstallDependencies.md)

## Installing Ansible
[Install Ansible](./InstallAnsible.md)

## Installing Collection
[Install Collections](./InstallCollection.md)
