# Ansible Desktop

Ansible repository to setup different desktop machines and play with this awesome tool. Feel free to collaborate if you want to improve Ansible :-)

## Requirements

- Git
- Ansible


## Usage
Install Git and Ansible and clone this repo. Then edit the inventory file (hosts.yml) and update the `ansible_user` variable. 

By default the playbook doesn't install any roles; edit the  `local.yaml` playbook file to enable/disable the roles you want to install and check if the variables to be passed to the role are correct.

Use next command the check the playbook before applying the changes:
```
$ ansible-playbook -K -C local.yaml
```
If everything looks good run the next command to apply the playbook: 
```
$ ansible-playbook -K local.yaml
```