# CLAUDE.md
必ず日本語で回答してください。
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is an Ansible playbook repository containing sample automation scripts for AWS infrastructure management and network device configuration. The repository includes playbooks for AWS security groups, network device management (Cisco and Junos), and AWX/Ansible Automation Platform integration.

## Common Commands

### Running Playbooks
```bash
# Run a basic playbook
ansible-playbook debug.yml

# Run with specific inventory
ansible-playbook -i inventories/network/inventory.yml <playbook.yml>

# Run with extra variables
ansible-playbook aws_sg.yml -e "vpc_id=vpc-12345678"

# Run with vault password (if using encrypted variables)
ansible-playbook --ask-vault-pass <playbook.yml>
```

### Collection Management
```bash
# Install required collections
ansible-galaxy collection install -r collections/requirements.yml

# Update collections
ansible-galaxy collection install -r collections/requirements.yml --force
```

### Testing and Validation
```bash
# Check playbook syntax
ansible-playbook --syntax-check <playbook.yml>

# Dry run (check mode)
ansible-playbook --check <playbook.yml>

# List tasks
ansible-playbook --list-tasks <playbook.yml>
```

## Architecture

### Inventory Structure
- `inventories/network/` - Network device inventory with separate group variables for Cisco and Junos devices
- Network devices are organized by vendor type (cisco, junos) with specific connection parameters
- Group variables define connection methods: SSH for Cisco, NETCONF for Junos

### Key Playbooks
- `aws_sg.yml` - AWS Security Group management using amazon.aws collection
- `update_aws.yml` - AWX credential management with HashiCorp Vault integration
- `debug.yml` - Basic variable display and testing
- Network playbooks target specific device groups using inventory structure

### External Dependencies
- **Collections**: awx.awx (AWX automation), seiko.smartcs (network device management)
- **AWS Integration**: Uses amazon.aws collection for EC2 security group management
- **Vault Integration**: HashiCorp Vault for dynamic AWS credential generation
- **AWX Integration**: Credential management and automation platform connectivity

### Variable Management
- Playbooks expect external variables (vpc_id, ANSIBLE_HASHI_VAULT_ADDR, etc.)
- AWX playbooks require controller credentials (user, password, url)
- Network playbooks use ansible_network_os for device-specific handling