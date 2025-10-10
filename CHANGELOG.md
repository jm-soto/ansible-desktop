## v0.14.1 (2025-10-10)

### Refactor

- **ohmyzsh**: move zsh user configuration files

## v0.14.0 (2025-10-10)

### Feat

- **Taskfile**: configure taskfile to run target commands
- **docker**: update playbook to use geerlingguy.docker role
- **docker**: configure role geerlingguy.docker to manage docker installation

## v0.13.0 (2025-10-10)

### Feat

- **docker**: upgrade docker to 28.5.1

## v0.12.0 (2025-10-10)

### Feat

- **homebrew**: install commitizen

## v0.11.0 (2025-10-10)

### Feat

- **homebrew**: configure homebrew in common role

## v0.10.0 (2025-10-10)

### Feat

- **terminator**: change terminal colours

## v0.9.1 (2024-06-04)

### Refactor

- change role name
- **local.yml**: add docker and slack tags
- **WIP**: add tags for roles and interestings role tasks

## v0.9.0 (2024-06-01)

### Feat

- **local.yaml**: ansible user from system variable

## v0.8.0 (2024-05-22)

### Feat

- **ohmyzsh-role**: add alias and modularize .zshrc
- **ohmyzsh-role**: tagged ohmyzsh tasks

## v0.7.0 (2024-05-22)

### Feat

- **gh-action**: remove release.yaml action

## v0.6.2 (2024-05-22)

### Fix

- **release.yaml**: fix release.yaml

## v0.6.1 (2024-05-22)

### Fix

- **gh-action**: fix release workload

## v0.6.0 (2024-05-22)

### Feat

- **gh-action**: force a new release

## v0.5.1 (2024-05-22)

### Fix

- **gh-actions**: fix release workflow tag name

## v0.5.0 (2024-05-22)

### Feat

- **gh-workflows**: generate a new release when new tag is added
- **bump**: update repository tags name
- **gh-actions**: update checkout action version

### Fix

- **CHANGELOG.md**: remove 0.5.0 bump
- **gh-actions**: configure workflows
- **gh-workflows**: correct tags name
- **test**: generate new tag to test actions
- **gh-workflows**: update release action version

## v0.4.0 (2024-05-22)

### Feat

- **bumpversion.yaml**: configure gh action to generate bump version
- **hosts.yml**: get user name from environment variable USER
- **neovim-role**: delete neovim role
- **docker-role**: add tags to run a group of tasks
- **docker-role**: remove docker-compose installation process
- **docker-role**: update docker version
- **nvim**: nvim configuration file
- **vscode-role**: vscode role to install this IDE in Ubuntu desktop 20.04 systems

### Fix

- **docker-setup-Debian.yaml**: typo

### Refactor

- **docker-role**: remove docker-compose installation task

## v0.3.0 (2022-10-16)

### Feat

- **Common-role**: Install The Fuck
- **ansible-role**: Create kubectl role

## 0.2.0 (2022-10-16)

### Feat

- **ansible-roles**: Create minikube role to install/manage minikube

### Fix

- **common-role**: remove variable type specification bool for when expresions

### Refactor

- **gitignore**: add idea directory to gitignore file

## 0.1.0 (2022-10-16)

### Feat

- **Commitizen**: Set up commitizen
