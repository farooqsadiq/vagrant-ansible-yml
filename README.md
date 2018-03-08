# Simple Vagrant provisioning with Ansible Inventory

This is a simple example of provisioning vagrant machines
using ansible inventory. 

The inventory is read from a yaml inventory file, so ansible >= 2.4 is required

To rerun the playbook without vagrant, you will have to update your ~/.ssh/config file
```
vagrant ssh-config >> ~/.ssh/config
```

To get alpine linux to work, you need to
```
vagrant plugin install vagrant-alpine
```
use the 'maier/alpine-3.6-x86_64' box
Install python before provisioning with ansible

The default alpine/alpine64 box does not provision, producing the error
```
Vagrant attempted to execute the capability 'change_host_name'
on the detect guest OS 'linux', but the guest doesn't
support that capability. This capability is required for your
configuration of Vagrant. Please either reconfigure Vagrant to
avoid this capability or fix the issue by creating the capability.
```
