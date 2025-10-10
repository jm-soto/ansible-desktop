# Ansible Desktop

Ansible repository to setup different desktop machines and experiment with Ansible.  
Feel free to collaborate if you want to improve this project.  

---

## Requirements

Before using this repository, make sure you have installed:

- Git
- Ansible (>= 2.14 recommended)
- [Homebrew](https://brew.sh/) (optional, for certain tasks)
- [Taskfile](https://taskfile.dev/) (optional, recommended for automation)

---

## Initial Setup

1. **Clone the repository:**

```bash
git clone https://github.com/Tekiroz/ansible-desktop.git
cd ansible-desktop
```

2. Install Ansible roles and collections using Taskfile:

```bash
task setup
```

This command installs:

Required roles from requirements.yml (including geerlingguy.docker)

Required collections from requirements.yml (e.g., community.general)

3. Enable/Disable roles:

- By default, the playbook does not apply any roles.
- Edit local.yml (or your playbook) to enable the roles you want.
- Verify that variables passed to each role are correct.

---

## Usage

Run the full playbook:

```bash
ansible-playbook -K local.yml

```
-K prompts for the sudo password if needed.

### Run specific roles
```bash
ansible-playbook -K local.yml --tags commons
ansible-playbook -K local.yml --tags docker
ansible-playbook -K local.yml --tags brew
```

