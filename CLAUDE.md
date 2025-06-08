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

# Test with mock data (for Cisco log collection)
ansible-playbook test_cisco_mock.yml
```

## Architecture

### Inventory Structure
- `inventories/network/` - Network device inventory with separate group variables for Cisco and Junos devices
- Network devices are organized by vendor type (cisco, junos) with specific connection parameters
- Group variables define connection methods: SSH for Cisco, NETCONF for Junos

### Key Playbooks
- `aws_sg.yml` - AWS Security Group management using amazon.aws collection
- `create_ec2.yml` - EC2 instance creation with configurable parameters
- `cisco_log_collection.yml` - Comprehensive Cisco device log collection with Jinja2 templates
- `update_aws.yml` - AWX credential management with HashiCorp Vault integration
- `debug.yml` - Basic variable display and testing
- `test_cisco_mock.yml` - Mock testing for Cisco log collection without real devices

### Template Architecture
- `templates/` directory contains Jinja2 templates for output formatting
- Cisco log collection uses separate templates for different data categories:
  - `cisco_basic_info.j2` - System information formatting
  - `cisco_routing_info.j2` - Network routing data formatting
  - `cisco_log_info.j2` - System logging output formatting
  - `cisco_summary.j2` - Executive summary report formatting

### External Dependencies
- **Collections**: awx.awx (AWX automation), seiko.smartcs (network device management), cisco.ios/cisco.iosxr (network device management)
- **AWS Integration**: Uses amazon.aws collection for EC2 security group and instance management
- **Vault Integration**: HashiCorp Vault for dynamic AWS credential generation
- **AWX Integration**: Credential management and automation platform connectivity

### Variable Management
- Playbooks expect external variables (vpc_id, ANSIBLE_HASHI_VAULT_ADDR, etc.)
- EC2 playbooks support configurable instance types, AMI IDs, and security groups
- AWX playbooks require controller credentials (user, password, url)
- Network playbooks use ansible_network_os for device-specific handling
- Cisco log collection generates timestamped output files in `logs/` directory
- Template variables include inventory_hostname, ansible_date_time, and command output arrays