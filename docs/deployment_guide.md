# Spine-Leaf Fabric Deployment Guide

## Overview

This guide provides step-by-step instructions for deploying a spine-leaf network architecture using Ansible AWX with Huawei CE6800 switches.

## Prerequisites

### Hardware Requirements
- Huawei CE6800 switches (minimum 2 spine, 2 leaf)
- Management network connectivity
- Appropriate cabling between spine and leaf switches

### Software Requirements
- Ansible 2.9+
- AWX/Ansible Tower
- Huawei VRP operating system
- Required Ansible collections (see requirements.yml)

### Network Requirements
- Management IP addresses for all switches
- IP addressing plan for underlay networks
- Tenant network planning (VLANs, VRFs, subnets)

## Pre-Deployment Checklist

### 1. Inventory Configuration
- [ ] Update `inventories/hosts.yml` with correct IP addresses
- [ ] Configure device credentials in vault
- [ ] Verify network connectivity to all devices
- [ ] Test SSH access to switches

### 2. Variable Configuration
- [ ] Review `group_vars/all.yml` for fabric settings
- [ ] Update BGP AS numbers if needed
- [ ] Configure interface mappings
- [ ] Set tenant network parameters

### 3. AWX Setup
- [ ] Import project into AWX
- [ ] Create credentials for network devices
- [ ] Import job templates
- [ ] Configure workflows

## Deployment Steps

### Step 1: Fabric Deployment

1. Navigate to AWX web interface
2. Go to Templates section
3. Select "Deploy Spine-Leaf Fabric" template
4. Click "Launch" and configure survey parameters:
   - Enable Verification: Yes
   - Deployment Mode: Full
5. Monitor job execution

### Step 2: Verification

After fabric deployment, run verification:
1. Launch "Verify Fabric Connectivity" template
2. Check BGP neighbor status
3. Verify VXLAN tunnel establishment
4. Validate underlay routing

### Step 3: Tenant Network Deployment

1. Launch "Deploy Tenant Networks" template
2. Configure tenant parameters:
   - Tenant ID: Unique identifier
   - Tenant Name: Descriptive name
   - VLAN Range: Available VLAN range
   - Network Prefix: Subnet prefix
   - Gateway Announcement: Enable if needed
3. Monitor deployment progress

### Step 4: Gateway Configuration

1. Launch "Announce to DC Gateway" template
2. Verify route advertisements
3. Test external connectivity

## Post-Deployment Verification

### BGP Verification Commands
```bash
# Check BGP neighbors
display bgp peer

# Check BGP routes
display bgp routing-table

# Check EVPN routes
display bgp evpn routing-table
```

### VXLAN Verification Commands
```bash
# Check VXLAN tunnels
display vxlan tunnel

# Check VNI status
display vxlan vni

# Check NVE interface
display interface Nve1
```

### Tenant Verification Commands
```bash
# Check VRF status
display ip vpn-instance

# Check VLAN interfaces
display interface Vlanif

# Check routing in VRF
display ip routing-table vpn-instance <vrf_name>
```

## Troubleshooting

### Common Issues

#### BGP Neighbor Not Established
1. Verify IP connectivity between devices
2. Check BGP configuration (AS numbers, router IDs)
3. Verify firewall rules
4. Check interface status

#### VXLAN Tunnel Not Up
1. Verify underlay routing
2. Check VTEP loopback reachability
3. Validate NVE interface configuration
4. Verify VNI binding

#### Tenant Network Not Working
1. Check VRF configuration
2. Verify VLAN creation
3. Validate SVI configuration
4. Check route advertisements

### Log Locations
- AWX job logs: AWX web interface
- Ansible logs: `logs/ansible.log`
- Switch logs: Device syslog

## Maintenance

### Regular Tasks
- Monitor BGP session status
- Check interface utilization
- Verify tenant network performance
- Update documentation

### Backup Procedures
- Regular configuration backups
- AWX job template backups
- Inventory and variable backups

## Scaling the Fabric

### Adding New Leaf Switches
1. Update inventory with new leaf
2. Configure spine-leaf links
3. Run incremental deployment
4. Verify connectivity

### Adding New Tenants
1. Plan VLAN and VNI allocation
2. Update tenant variables
3. Deploy tenant networks
4. Configure gateway announcements

## Security Considerations

- Use encrypted vault for credentials
- Implement proper access controls in AWX
- Regularly update device passwords
- Monitor configuration changes
- Use SNMPv3 where possible
