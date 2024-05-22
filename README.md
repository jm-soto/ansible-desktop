# Ansible Desktop

Ansible repository to setup different desktop machines and play with this awesome tool. Feel free to collaborate if you want to improve Ansible :-)

## Requirements

- Git
- Ansible


## Usage
1. Install Git and Ansible and clone this repo:
```
    $ git clone https://github.com/Tekiroz/ansible-desktop.git
```

2. Then edit the inventory file (hosts.yml) and update the `ansible_user` variable.

3. If you enable the GNOME module, you will need to install the `community.general` collection installed. Run this commmand to install it:
```
    $ ansible-galaxy collection install community.general
``` 

4. By default the playbook doesn't install any roles; edit the  `local.yaml` playbook file to enable/disable the roles you want to install and check if the variables to be passed to the role are correct.

5. If everything looks good run the next command to apply the playbook: 
```
    $ ansible-playbook -K local.yml
```


