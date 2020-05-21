# Install Ansible


## Installing Ansible 2.9+

### Install using yum or apt
- Red Hat or CentOS

     ```bash
     sudo yum install ansible
     ```

- Ubuntu

   ```bash
   sudo apt-get update
   sudo apt-get install software-properties-common
   sudo apt-add-repository --yes --update ppa:ansible/ansible
   sudo apt-get install ansible
   ```
### Install using pip
- pip

   ```bash
   sudo pip3 install ansible
   ```
### Official guide

Follow [link](./InstallAnsible.rst) to install ansible.


## Verify the installation
Execute the following commands and verify the installation

```shell
# Check version
ansible --version

# For printing help doc
ansible -help
```

## Variables to be added in the Ansible Config [file](../playbooks/ansible.cfg)

|Variable Name|Description|Value to Set to|
|----|-----|-----|
|interpreter_python|The python interpreter to use for running ansible.|/usr/bin/python3|
|stdout_callback, callback_whitelist|The call back to use for formatting the console output.|infinera.iqnos.infnstdout|

>**If the above variables are not added to the `config` file then the below variables can be exported as shell variables**

|Variable Name|Description|Value to Set to|
|----|-----|-----|
|ANSIBLE_PYTHON_INTERPRETER|The python interpreter to use for running ansible.|/usr/bin/python3|
|ANSIBLE_STDOUT_CALLBACK|The python interpreter to use for running ansible.|infinera.iqnos.infnstdout|

>**The above variables can be exported as shell variables**
```shell
# Example
export ANSIBLE_STDOUT_CALLBACK="infinera.iqnos.infnstdout"
export ANSIBLE_PYTHON_INTERPRETER="/usr/bin/python3"
```
>**They can also be added to the `~/.bash_profile` file as shown below. Just replace ANSIBLE_CONFIG=VALUE with the corresponding variable and value**

## Configuring Ansible 

Set the `ANSIBLE_CONFIG` environmental variable to the `ansible.cfg` file present in the `src/ansible/` folder

```shell
cd playbook
export ANSIBLE_CONFIG=./ansible.cfg
```
> The above approach has to be repeated everytime we open a new shell
>
> Instead, the variable can be set in the `~/.bash_profile` file.
>
> Follow the below procedure to accomplish it -
>
> 1. `vim ~/.bash_profile`
>
> 2. Press the `i` key or the `insert` key and go to the end of the file by scrolling down with the `down-arrow` key
>
> 3. Type in the following line after reaching the bottom of the file - `shell ANSIBLE_CONFIG=path_to_the/ansiblefolder_with_infinera_code/ansible.cfg`
>
> 4. Once the line ine entered press the `escape` followed by the `:` key (colon) and `q` key and press `enter`
>
> 5. This can be checked by running the following - `printenv ANSIBLE_CONFIG`

## Links for Reference

* [Ansible 2.9 Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* [Python3](https://www.python.org/downloads/)
