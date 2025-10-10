# Docker Role

This Ansible role installs and configures Docker Engine on Linux systems. It supports multiple distributions including Ubuntu, Debian, and Raspbian, with automatic detection of system architecture and distribution-specific configurations.

## Features

- **Multi-Distribution Support**: Ubuntu, Debian, and Raspbian
- **Architecture Detection**: Automatic ARM/AMD64 architecture handling
- **Clean Installation**: Removes old Docker versions before installation
- **Repository Management**: Adds official Docker repositories with GPG keys
- **Service Management**: Starts and enables Docker service
- **User Management**: Adds users to docker group for non-sudo access
- **Version Pinning**: Supports specific Docker versions (Ubuntu)
- **Dependency Management**: Installs required system dependencies

## Requirements

- Ansible 2.9 or higher
- Target system running Ubuntu, Debian, or Raspbian
- Sudo privileges for package installation and service management

## Role Variables

### Default Variables (`defaults/main.yaml`)

#### Old Package Removal
```yaml
docker_old_packages:
  - docker-ce
  - docker-ce-cli
  - docker-buildx-plugin
  - docker-ce-rootless-extras
  - docker-compose-plugin
old_packages_status: absent
```

#### Docker Packages
```yaml
docker_package:
  - docker-ce
  - docker-ce-cli
  - containerd.io
docker_package_state: present
```

#### Service Configuration
```yaml
docker_restart_handler_state: restarted
docker_service_state: started
docker_service_enabled: true
```

### Distribution-Specific Variables

#### Debian Variables (`vars/Debian.yaml`)
```yaml
dependencies:
  - apt-transport-https
  - ca-certificates
  - curl
  - gnupg
  - lsb-release
dependencies_status: present

docker_apt_key: 9DC858229FC7DD38854AE2D88D81803C0EBFCD88
docker_apt_key_status: present
docker_apt_arch: amd64
docker_repo_url: https://download.docker.com/linux
docker_apt_release_channel: stable
docker_apt_repository: "deb [arch={{ docker_apt_arch }}] {{ docker_repo_url }}/{{ ansible_distribution | lower }} {{ ansible_distribution_release }} {{ docker_apt_release_channel }}"
docker_apt_gpg_key: "{{ docker_repo_url }}/{{ ansible_distribution | lower }}/gpg"
docker_apt_repository_state: present
```

#### Ubuntu Variables (`vars/Ubuntu.yaml`)
```yaml
docker_package:
  - "docker-ce=5:28.5.1-1~ubuntu.24.04~noble"
  - "docker-ce-cli"
  - containerd.io
docker_package_state: present
```

#### Raspbian Variables (`vars/Raspbian.yaml`)
```yaml
docker_apt_arch: armhf
docker_compose_dependencies:
  - python3-dev
  - python3-setuptools
  - python3-pip
docker_compose_dependencies_status: present
docker_compose_pip_name: docker-compose
docker_compose_status: present
```

### Customizable Variables

You can override these variables in your playbook:

```yaml
# Custom Docker version (Ubuntu)
docker_package:
  - "docker-ce=5:20.10.24~3-0~ubuntu-focal"
  - "docker-ce-cli"
  - containerd.io

# Add users to docker group
docker_users:
  - "{{ ansible_user }}"
  - "developer"
  - "admin"

# Service configuration
docker_service_state: started
docker_service_enabled: true
```

## Dependencies

None. This is a standalone role.

## Example Playbook

### Basic Installation
```yaml
---
- hosts: docker_hosts
  become: yes
  roles:
    - docker
```

### Custom Configuration
```yaml
---
- hosts: docker_hosts
  become: yes
  vars:
    # Specify Docker version
    docker_package:
      - "docker-ce=5:20.10.24~3-0~ubuntu-focal"
      - "docker-ce-cli"
      - containerd.io
    
    # Add multiple users to docker group
    docker_users:
      - "{{ ansible_user }}"
      - "developer"
      - "ci-user"
    
    # Service configuration
    docker_service_enabled: true
    docker_service_state: started
  roles:
    - docker
```

### Raspberry Pi Installation
```yaml
---
- hosts: raspberry_pi
  become: yes
  vars:
    # Raspbian automatically uses ARM architecture
    docker_users:
      - "{{ ansible_user }}"
      - "pi"
  roles:
    - docker
```

## Task Structure

### Main Tasks (`tasks/main.yaml`)
1. **Variable Loading**: Includes OS-specific variables
2. **Old Package Removal**: Removes existing Docker installations
3. **OS-Specific Setup**: Includes distribution-specific setup tasks
4. **Docker Installation**: Installs Docker packages
5. **Service Management**: Starts and enables Docker service
6. **User Management**: Adds users to docker group

### Debian Setup (`tasks/setup-Debian.yaml`)
1. **Dependencies**: Installs required system packages
2. **GPG Key**: Adds Docker's official GPG key
3. **Repository**: Adds Docker's official APT repository
4. **Cache Update**: Updates package cache

### User Management (`tasks/docker-user.yaml`)
1. **Group Assignment**: Adds specified users to docker group
2. **Non-sudo Access**: Enables Docker usage without sudo

### Handlers (`handlers/main.yaml`)
1. **Service Restart**: Restarts Docker service when needed

## Tags

The role supports the following tags for selective execution:

- `docker-install`: All installation tasks
- `docker-uninstall`: Package removal tasks

### Example Usage with Tags

```bash
# Install Docker only
ansible-playbook playbook.yml --tags "docker-install"

# Remove old Docker versions only
ansible-playbook playbook.yml --tags "docker-uninstall"

# Full installation
ansible-playbook playbook.yml --tags "docker-install,docker-uninstall"
```

## Post-Installation

After running this role:

1. **Logout and login** or run `newgrp docker` to apply group changes
2. **Verify installation**:
   ```bash
   docker --version
   docker run hello-world
   ```
3. **Check service status**:
   ```bash
   sudo systemctl status docker
   ```

## Architecture Support

- **AMD64**: Standard x86_64 systems (Ubuntu, Debian)
- **ARMHF**: Raspberry Pi and ARM-based systems (Raspbian)

## Version Management

### Ubuntu
- Supports version pinning for specific Docker versions
- Example: `docker-ce=5:28.5.1-1~ubuntu.24.04~noble`
- Check available versions: `apt-cache madison docker-ce`

### Debian/Raspbian
- Installs latest stable version from official repository
- No version pinning by default

## Troubleshooting

### Common Issues

1. **Permission Denied**: Ensure user is in docker group and has logged out/in
2. **Service Won't Start**: Check system logs: `sudo journalctl -u docker`
3. **Repository Issues**: Verify GPG key and repository configuration
4. **Architecture Mismatch**: Ensure correct architecture is detected

### Debug Commands

```bash
# Check Docker installation
docker --version
docker info

# Check service status
sudo systemctl status docker

# Check user groups
groups $USER

# Check Docker group
getent group docker
```

## Security Considerations

- Users added to docker group have root-equivalent access
- Only add trusted users to the docker group
- Consider using Docker rootless mode for enhanced security
- Regularly update Docker to latest stable version

## License

This role is part of the ansible-desktop project. Please refer to the main project license.

## Author Information

This role was created as part of the ansible-desktop automation project for setting up development environments with Docker support.
