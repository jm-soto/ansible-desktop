# Ansible Desktop

Ansible repository to setup different desktop machines and play with this awesome tool. Feel free to collaborate if you want to improve Ansible :-)

## Requirements

- Git
- Ansible


## Usage
Install Git and Ansible and clone this repo. Then edit the inventory file (hosts.yml) and update the `ansible_user` variable.
```
$ ansible-playbook -K local.yaml
```