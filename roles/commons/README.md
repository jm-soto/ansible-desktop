# Commons Role

This Ansible role provides common system setup and package management functionality for desktop environments. It handles system dependencies, package installation/removal, and tool verification across different operating systems.

## Features

- **System Package Management**: Install and remove common packages via APT
- **System Updates**: Optional system upgrade functionality
- **Tool Verification**: Check for required tools and set facts for conditional tasks
- **Homebrew Integration**: Install packages via Homebrew when available
- **OS-Specific Setup**: Support for different Linux distributions
- **Cleanup**: Automatic removal of unnecessary packages and dependencies

## Requirements

- Ansible 2.9 or higher
- Target system running a supported Linux distribution (Ubuntu, Debian, etc.)
- Sudo privileges for package management operations

## Role Variables

### Default Variables

The following variables are defined in `defaults/main.yml`:

#### Tool Checking
```yaml
tools_to_check:
  - brew  # List of tools to verify installation
```

#### System Management
```yaml
system_upgrade: true  # Enable/disable system upgrade
```

#### Package Installation
```yaml
install_packages: true
packages_to_install_list:
  - curl
  - gnupg
  - apt-transport-https
  - xz-utils
  - gnome-tweaks
  - pip
  - vim
  - git
  - zsh
  - vlc
  - flameshot
  - timeshift
  - build-essential
```

#### Package Removal
```yaml
uninstall_packages: true
packages_to_uninstall_list:
  - bcache-tools
  - byobu
  - cloud-guest-utils
  - cloud-initramfs-copymods
  - cloud-initramfs-dyn-netconf
  - landscape-common
  - lxd-agent-loader
  - open-vm-tools
  - popularity-contest
  - sosreport
```

#### Homebrew Packages
```yaml
brew_packages_install:
  - gcc
  - thefuck
  - commitizen
```

## Dependencies

None. This is a standalone role.

## Example Playbook

```yaml
---
- hosts: localhost
  become: yes
  roles:
    - commons
```

### Customizing Package Lists

```yaml
---
- hosts: localhost
  become: yes
  vars:
    # Add custom packages
    packages_to_install_list:
      - curl
      - git
      - vim
      - htop
      - tree
    
    # Disable system upgrade
    system_upgrade: false
    
    # Add custom brew packages
    brew_packages_install:
      - gcc
      - thefuck
      - commitizen
      - jq
      - yq
  roles:
    - commons
```

## Task Structure

### Main Tasks (`tasks/main.yml`)
- Debug PATH variable for troubleshooting
- Check required tools using `tools_check.yml`
- Include OS-specific setup tasks
- Install Homebrew packages if available

### Tool Checking (`tasks/tools_check.yml`)
- Verifies if specified tools are installed
- Creates facts like `tool_<tool_name>` for conditional usage
- Special handling for Homebrew (loads Linuxbrew environment)
- Provides debug output for tool availability

### Ubuntu Setup (`tasks/ubuntu-setup.yml`)
- Updates APT cache
- Performs system upgrade (if enabled)
- Removes unwanted packages
- Installs required packages
- Cleans up unnecessary packages and dependencies

### Homebrew Integration (`tasks/brew-packages.yml`)
- Installs Homebrew packages only if Homebrew is detected
- Gracefully skips installation if Homebrew is not available
- Provides informative debug messages

## Tags

The role supports the following tags for selective execution:

- `commons`: All role tasks
- `debug`: Debug and informational tasks
- `check`: Tool verification tasks
- `apt-upgrade`: System update tasks
- `brew`: Homebrew-related tasks

### Example Usage with Tags

```bash
# Run only package installation
ansible-playbook playbook.yml --tags "commons" --skip-tags "brew,apt-upgrade"

# Run only system updates
ansible-playbook playbook.yml --tags "apt-upgrade"

# Run only Homebrew tasks
ansible-playbook playbook.yml --tags "brew"

# Run with debug information
ansible-playbook playbook.yml --tags "commons,debug"
```

## Facts Created

The role creates the following facts for conditional usage:

- `tool_<tool_name>`: Contains the result of tool verification for each tool in `tools_to_check`

Example:
```yaml
# If brew is in tools_to_check
tool_brew:
  rc: 0  # 0 if found, non-zero if not found
  stdout: "/home/linuxbrew/.linuxbrew/bin/brew"
  stderr: ""
```

## License

This role is part of the ansible-desktop project. Please refer to the main project license.

## Author Information

This role was created as part of the ansible-desktop automation project for setting up development environments.
