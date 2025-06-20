# ☁️ VMware Cloud Director (vCD) Network Automation using Ansible

This repository contains a collection of **Ansible playbooks** for automating the creation and management of **VDC networks** in VMware Cloud Director (vCD).

Supported network types:

- 🛣️ Routed Networks (IPv4 / Dual-Stack)
- 🌐 Imported Networks (DV Port Group / NSX-T Segment)
- 🔗 Direct Networks
- 🔒 Isolated Networks

---

## 📁 Playbooks Overview

| Playbook File        | Description                                                               |
|----------------------|---------------------------------------------------------------------------|
| `routed.yml`         | Creates a NAT routed VDC network with optional dual-stack (IPv4/IPv6)     |
| `imported.yml`       | Imports an existing DV Port Group or NSX-T Segment into vCD               |
| `direct.yml`         | Creates a direct network connected to an external network (no NAT)        |
| `isolated.yml`       | Creates a fully isolated network with no external connectivity            |

---

## 🚀 Routed Network – `routed.yml`

Creates a **NAT-routed VDC network** with support for **dual-stack (IPv4 + IPv6)**.

### 🔧 Required Variables

# ========== vCD API & Authentication ==========
vcd_api_host_name: ""             # vCloud Director API hostname (e.g., vcloud.example.com)
vcd_api_version: "36.3"           # API version (e.g., 36.3, depending on your vCD)
vcd_api_token: ""                 # API token (from UI or generated via CLI)
vcd_organization_name: ""         # Organization name in vCD (e.g., "MyOrg")
vcd_organization_vdc_name: ""     # Virtual Data Center name (e.g., "MyVDC")
edge_connection_name: ""          # Edge Gateway name for routing and NAT

# ========== Network Info ==========
network_name: ""                  # Name of the new routed network (e.g., "routed-net-1")
network_description: ""           # Description (optional, for documentation)
enable_dual_subnet_network: true  # true for IPv4+IPv6, false for only IPv4

# ========== IPv4 Subnet ==========
Gateway_CIDR_IPv4: "192.168.100.1"                      # IPv4 gateway IP
ipv4_prefix_length: 24                                  # CIDR prefix (e.g., 24 for /24)
ipv4_ip_ranges_string: "192.168.100.10-192.168.100.20"  # Assignable IP range

# ========== IPv6 Subnet ==========
Gateway_CIDR_IPv6: "fd00:192:168:1::1"                       # IPv6 gateway
ipv6_prefix_length: 64                                       # CIDR prefix (typically 64)
ipv6_ip_ranges_string: "fd00:192:168:1::10-fd00:192:168:1::20"  # Assignable IPv6 range

# ========== DNS ==========
ipv4_dns_suffix: "example.com"       # DNS suffix for hosts
ipv4_dns_server1: "8.8.8.8"          # Primary DNS
ipv4_dns_server2: "1.1.1.1"          # Secondary DNS (optional)





🔒 Isolated Network – isolated.yml
Creates a fully isolated network without edge connectivity. Ideal for internal-only VM communication.

🔧 Required Variables
# ========== vCD API & Auth ==========
vcd_api_host_name: ""             # vCD API endpoint
vcd_api_version: "36.3"           # API version
vcd_api_token: ""                 # vCD API token

vcd_organization_name: ""         # Organization name
vcd_organization_id: ""           # Org ID (can be fetched via API)
vcd_organization_vdc_name: ""     # VDC name
vcd_organization_vdc_id: ""       # VDC ID (can be fetched via API)

# ========== Optional (for NSX-T backed network) ==========
backing_network_id: ""            # Optional NSX-T Segment ID (leave blank for new)

# ========== Network Info ==========
network_name: ""                  # Desired network name
network_description: ""           # Description
enable_dual_subnet_network: false  # Enable IPv6: true or false

# ========== IPv4 Subnet ==========
Gateway_CIDR_IPv4: "172.20.30.1"                        # Gateway IP for IPv4
ipv4_prefix_length: 24                                  # Subnet prefix length
ipv4_ip_ranges_string: "172.20.30.10-172.20.30.20"      # IP pool range

# ========== IPv6 Subnet ==========
Gateway_CIDR_IPv6: "fd00:172:20:30::1"                       # IPv6 Gateway
ipv6_prefix_length: 64                                       # Prefix length
ipv6_ip_ranges_string: "fd00:172:20:30::10-fd00:172:20:30::20"  # IPv6 range

# ========== DNS ==========
ipv4_dns_suffix: "example.com"       # DNS search domain
ipv4_dns_server1: "8.8.8.8"          # Primary DNS
ipv4_dns_server2: "1.1.1.1"          # Secondary DNS



🌐 Direct Network – direct.yml
Creates a Direct VDC Network attached directly to an external network (e.g., DV Port Group) without edge gateway.

🔧 Required Variables
# ========== vCD API & Auth ==========
vcd_api_host_name: ""             # API endpoint (vCloud Director)
vcd_api_version: "36.3"           # API version
vcd_api_token: ""                 # API token

vcd_organization_name: ""         # Organization name in vCD
vcd_organization_vdc_name: ""     # VDC name in vCD

# ========== Network Info ==========
external_network: ""              # Existing external network (e.g., DV Port Group name)
network_name: ""                  # Name of the new direct network
network_description: ""           # Description (optional)
shared_network: "true"            # Should this network be shared across orgs (string: "true"/"false")



🔁 Imported Network – imported.yml
Creates an Imported VDC Network from an existing DV Port Group or NSX-T Segment. Supports dual-stack.

🔧 Required Variables
# ========== vCD API & Auth ==========
vcd_api_host_name: ""             # API hostname
vcd_api_version: "36.3"           # API version
vcd_api_token: ""                 # API token

vcd_organization_name: ""         # vCD organization name
vcd_organization_vdc_name: ""     # VDC name
vim_server_id: ""                 # vCenter (VIM) server ID – get from vCD

# ========== Network Info ==========
network_name: ""                  # Name for imported network in vCD
network_description: ""           # Optional description
shared_network: "false"           # Should network be shared? (string: "true"/"false")
enable_dual_subnet_network: "true"  # Enable IPv6? "true"/"false"

# ========== IPv4 Subnet ==========
Gateway_CIDR_IPv4: "192.168.150.1"                      # IPv4 Gateway IP
ipv4_prefix_length: 24                                  # Subnet size (e.g., 24 for /24)
ipv4_ip_ranges_string: "192.168.150.10-192.168.150.20"  # Assignable IP range
ipv4_dns_suffix: ""                                     # Optional DNS suffix
ipv4_dns_server1: ""                                    # Optional DNS server 1
ipv4_dns_server2: ""                                    # Optional DNS server 2

# ========== IPv6 Subnet ==========
Gateway_CIDR_IPv6: "fd00:192:168:1::1"                        # IPv6 Gateway IP
ipv6_prefix_length: 64                                       # Prefix length
ipv6_ip_ranges_string: "fd00:192:168:1::10-fd00:192:168:1::20"  # Assignable IPv6 range
