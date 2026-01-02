# Interactive Deployment Guide

## Overview

This guide explains how to use the interactive deployment features of the Ansible AWX spine-leaf project, allowing you to configure all network parameters at runtime through AWX surveys.

## Interactive Parameters

### Fabric Deployment Parameters

When launching the "Deploy Spine-Leaf Fabric (Interactive)" job template, you'll be prompted for:

#### Network Configuration
- **Interface IP Prefix**: IP prefix for spine-leaf links (e.g., `10.255.1.0/31`)
- **VTEP Loopback Network**: Network for VTEP loopback interfaces (e.g., `10.0.0.0/24`)
- **Management Network**: Network for device management (e.g., `10.0.1.0/24`)

#### BGP Configuration
- **Spine ASN**: Autonomous System Number for spine switches (e.g., `65001`)
- **Leaf ASN Base**: Base ASN for leaf switches (will be incremented) (e.g., `65002`)
- **Gateway ASN**: ASN for DC gateway (e.g., `65400`)
- **BGP Hold Time**: BGP hold time in seconds (default: `180`)
- **BGP Keepalive**: BGP keepalive interval in seconds (default: `60`)

#### VXLAN Configuration
- **VNI Base**: Starting VNI number for VXLAN (default: `10000`)
- **VLAN Base**: Starting VLAN number for tenant networks (default: `1000`)

#### Interface Configuration
- **MTU Size**: Interface MTU size (default: `9216`)

#### Deployment Options
- **Enable Verification**: Run post-deployment verification tests
- **Deployment Mode**: Choose from `full`, `incremental`, or `validation_only`

### Tenant Network Parameters

When launching the "Deploy Tenant Networks (Interactive)" job template, you'll be prompted for:

#### Basic Tenant Information
- **Tenant ID**: Unique tenant identifier (e.g., `100`)
- **Tenant Name**: Descriptive tenant name (e.g., "Tenant A")
- **VRF Name**: VRF name for tenant isolation (e.g., `VRF_TENANT_A`)

#### Network Configuration
- **VLAN Range**: VLAN range for tenant (e.g., `1000-1099`)
- **VNI Number**: Unique VXLAN VNI (e.g., `10100`)
- **Network Prefix**: Subnet prefix (e.g., `10.100.`)
- **Subnet Mask**: Subnet mask (e.g., `24`)
- **Gateway IP**: Gateway IP suffix (e.g., `.1`)

#### BGP/EVPN Configuration
- **Route Distinguisher**: RD in format `ASN:ID` (e.g., `65001:100`)
- **Route Target**: RT in format `ASN:ID` (e.g., `65001:100`)

#### Advanced Options
- **Enable Gateway Announcement**: Announce to DC gateway
- **DHCP Servers**: DHCP server IPs (comma separated)
- **VRRP Priority**: VRRP priority for redundancy
- **Enable Multicast**: Enable multicast for tenant networks

## Usage Examples

### Example 1: Small Datacenter Fabric

```
Interface IP Prefix: 10.255.1.0/31
Spine ASN: 65001
Leaf ASN Base: 65002
VNI Base: 10000
VLAN Base: 1000
VTEP Loopback Network: 10.0.0.0/24
Management Network: 10.0.1.0/24
Gateway ASN: 65400
```

### Example 2: Enterprise Tenant Deployment

```
Tenant ID: 100
Tenant Name: Finance Department
VLAN Range: 1000-1099
VNI Number: 10100
VRF Name: VRF_FINANCE
Network Prefix: 10.100.
Subnet Mask: 24
Gateway IP: .1
Route Distinguisher: 65001:100
Route Target: 65001:100
Enable Gateway Announcement: true
DHCP Servers: 10.0.0.10,10.0.0.11
```

### Example 3: Multi-Tenant Environment

```
# Tenant 1 - Development
Tenant ID: 200
Tenant Name: Dev Team
VLAN Range: 2000-2099
VNI Number: 10200
VRF Name: VRF_DEV
Network Prefix: 10.200.

# Tenant 2 - Production
Tenant ID: 300
Tenant Name: Production
VLAN Range: 3000-3099
VNI Number: 10300
VRF Name: VRF_PROD
Network Prefix: 10.300.
```

## IP Address Planning

### Interface IP Allocation
The system automatically calculates IP addresses based on the provided prefix:

- For prefix `10.255.1.0/31`:
  - Spine01-Leaf01: 10.255.1.0/31 (spine) and 10.255.1.1/31 (leaf)
  - Spine01-Leaf02: 10.255.1.2/31 (spine) and 10.255.1.3/31 (leaf)
  - And so on...

### Loopback IP Allocation
VTEP and management IPs are allocated from the specified networks:

- VTEP Network: `10.0.0.0/24` → 10.0.0.1, 10.0.0.2, etc.
- Management Network: `10.0.1.0/24` → 10.0.1.1, 10.0.1.2, etc.

## Validation and Verification

### Pre-Deployment Checks
- Verify IP address ranges don't overlap
- Check ASN numbers are unique
- Validate VLAN ranges are available
- Ensure VNI numbers are unique

### Post-Deployment Verification
- BGP neighbor status
- VXLAN tunnel establishment
- VRF and VLAN configuration
- Route advertisement status

## Best Practices

### IP Address Management
- Use consistent IP addressing schemes
- Document IP allocations
- Reserve ranges for future expansion

### ASN Planning
- Use private ASNs (64512-65534) for internal networks
- Document ASN assignments
- Plan for hierarchical ASN structure

### VNI and VLAN Planning
- Use consistent numbering schemes
- Document VNI-to-VLAN mappings
- Reserve ranges for different purposes

### Security Considerations
- Use encrypted vault for sensitive parameters
- Validate user input in surveys
- Implement proper access controls in AWX

## Troubleshooting

### Common Issues
1. **IP Address Conflicts**: Check for overlapping IP ranges
2. **ASN Conflicts**: Ensure unique ASNs throughout the fabric
3. **VNI Conflicts**: Verify VNI numbers are unique
4. **VLAN Conflicts**: Check VLAN availability

### Debug Commands
```bash
# Check BGP neighbors
display bgp peer

# Check VXLAN tunnels
display vxlan tunnel

# Check VRF configuration
display ip vpn-instance

# Check interface status
display interface brief
```

## Scaling Considerations

### Adding New Devices
- Update inventory with new devices
- Re-run fabric deployment with updated parameters
- Verify connectivity

### Adding New Tenants
- Use interactive tenant deployment
- Ensure unique VLAN/VNI ranges
- Update documentation

### Network Expansion
- Plan IP address ranges for growth
- Document expansion plans
- Test in lab environment first
