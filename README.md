# Ansible AWX Spine-Leaf Architecture Deployer

This project provides a comprehensive Ansible AWX solution for deploying spine-leaf network architecture with tenant networks on Huawei CE6800 switches, including DC gateway announcements.

## Architecture Overview

- **Spine Switches**: Core layer providing high-speed connectivity between leaf switches
- **Leaf Switches**: Access layer connecting to servers and end devices
- **Tenant Networks**: Isolated network segments for different tenants
- **DC Gateway**: Border gateway for external connectivity

## Features

- Automated spine-leaf fabric deployment
- Tenant network creation and management
- BGP/EVPN configuration for fabric connectivity
- DC gateway route announcements
- VLAN and VRF management
- Interface configuration and validation

## Project Structure

```
ansible-awx-spine-leaf/
├── inventories/
│   ├── hosts.yml
│   └── group_vars/
├── playbooks/
│   ├── fabric/
│   ├── tenants/
│   └── gateway/
├── roles/
│   ├── spine_switch/
│   ├── leaf_switch/
│   ├── tenant_network/
│   └── dc_gateway/
├── templates/
├── vars/
├── awx/
│   ├── job_templates/
│   └── workflows/
└── docs/
```

## Prerequisites

- Ansible 2.9+
- AWX/Ansible Tower
- Huawei CE6800 switches with VRP
- Network connectivity to management interfaces
- Appropriate credentials and permissions

## Quick Start

1. Configure inventory in `inventories/hosts.yml`
2. Set variables in `group_vars/`
3. Import project into AWX
4. Run job templates for fabric deployment
5. Deploy tenant networks as needed

## Documentation

See `docs/` directory for detailed configuration guides and examples.
