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

```yaml
# API & Auth
vcd_api_host_name: ""
vcd_api_version: "36.3"
vcd_api_token: ""
vcd_organization_name: ""
vcd_organization_vdc_name: ""
edge_connection_name: ""

# Network Config
network_name: ""
network_description: ""
enable_dual_subnet_network: true

# IPv4 Config
Gateway_CIDR_IPv4: "192.168.100.1"
ipv4_prefix_length: 24
ipv4_ip_ranges_string: "192.168.100.10-192.168.100.20"

# IPv6 Config
Gateway_CIDR_IPv6: "fd00:192:168:1::1"
ipv6_prefix_length: 64
ipv6_ip_ranges_string: "fd00:192:168:1::10-fd00:192:168:1::20"

# DNS
ipv4_dns_suffix: "example.com"
ipv4_dns_server1: "8.8.8.8"
ipv4_dns_server2: "1.1.1.1"





🔒 Isolated Network – isolated.yml
Creates a fully isolated network without edge connectivity. Ideal for internal-only VM communication.

🔧 Required Variables
vcd_api_host_name: ""
vcd_api_version: "36.3"
vcd_api_token: ""
vcd_organization_name: ""
vcd_organization_id: ""
vcd_organization_vdc_name: ""
vcd_organization_vdc_id: ""

network_name: ""
network_description: ""
enable_dual_subnet_network: false

# IPv4 Config
Gateway_CIDR_IPv4: "172.20.30.1"
ipv4_prefix_length: 24
ipv4_ip_ranges_string: "172.20.30.10-172.20.30.20"

# IPv6 Config
Gateway_CIDR_IPv6: "fd00:172:20:30::1"
ipv6_prefix_length: 64
ipv6_ip_ranges_string: "fd00:172:20:30::10-fd00:172:20:30::20"

# DNS
ipv4_dns_suffix: "example.com"
ipv4_dns_server1: "8.8.8.8"
ipv4_dns_server2: "1.1.1.1"



🌐 Direct Network – direct.yml
Creates a Direct VDC Network attached directly to an external network (e.g., DV Port Group) without edge gateway.

🔧 Required Variables
vcd_api_host_name: ""
vcd_api_version: "36.3"
vcd_api_token: ""

vcd_organization_name: ""
vcd_organization_vdc_name: ""

external_network: ""     # External network name (e.g., DVPG)
network_name: ""
network_description: ""
shared_network: "true"   # "true" or "false" (string)



🔁 Imported Network – imported.yml
Creates an Imported VDC Network from an existing DV Port Group or NSX-T Segment. Supports dual-stack.

🔧 Required Variables
vcd_api_host_name: ""
vcd_api_version: "36.3"
vcd_api_token: ""

vcd_organization_name: ""
vcd_organization_vdc_name: ""
vim_server_id: ""

network_name: ""
network_description: ""
shared_network: "false"
enable_dual_subnet_network: "true"

# IPv4 Config
Gateway_CIDR_IPv4: "192.168.150.1"
ipv4_prefix_length: 24
ipv4_ip_ranges_string: "192.168.150.10-192.168.150.20"
ipv4_dns_suffix: ""
ipv4_dns_server1: ""
ipv4_dns_server2: ""

# IPv6 Config
Gateway_CIDR_IPv6: "fd00:192:168:1::1"
ipv6_prefix_length: 64
ipv6_ip_ranges_string: "fd00:192:168:1::10-fd00:192:168:1::20"
