# Install Infinera IQNOS Collection

## Install Galaxy Collection
```
ansible-galaxy collection install my_namespace.my_collection
```
### Install Infinera IQNOS Collection 
```
ansible-galaxy collection install infinera.iqnos
```
You can also directly use the tarball :
```
ansible-galaxy collection install infinera-iqnos-X.Y.Z.tar.gz
```
> If you want install on specific version use 

```
ansible-galaxy collection install infinera.iqnos:==1.0.0
```
> If you want install on specific location use -p option 

```
ansible-galaxy collection install infinera-iqnos-X.Y.Z.tar.gz -p ./collections
```

## Links for Reference

* [Installing Content from Ansible Galaxy ](https://galaxy.ansible.com/docs/using/installing.html)
* [Installing Ansible Collections ](https://docs.ansible.com/ansible/devel/user_guide/collections_using.html#collections)

