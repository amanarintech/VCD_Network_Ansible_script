# VMware Cloud Director (vCD) Network Automation using Ansible

This repository contains Ansible playbooks to automate the creation of different types of VDC networks in VMware Cloud Director, including:

- Routed Network
- Imported Network (DV Port Group or NSX-T Segment)
- Direct Network
- Isolated Network

---

## 📁 Playbooks

| Playbook File         | Description                                |
|-----------------------|--------------------------------------------|
| `routed.yml`          | Creates a routed VDC network with IPv4/IPv6 support |
| `imported.yml`        | Imports an existing NSX-T segment or DV port group into VCD |
| `direct.yml`          | Creates a direct network connected to a specific edge gateway |
| `isolated.yml`        | Creates a fully isolated network in the VDC |

---

## 🚀 Routed Network - `routed.yml`

This playbook automates the creation of a **NAT Routed VDC Network** with optional **dual-stack (IPv4 + IPv6)** support.

### 🔧 Variables to Configure

```yaml
vcd_api_host_name: ""               # vCD API hostname (e.g., vcloud.example.com)
vcd_api_version: "36.3"            # API version
vcd_api_token: ""                  # API token for authentication
vcd_organization_name: ""          # Org name in vCD
vcd_organization_vdc_name: ""      # VDC name in vCD
edge_connection_name: ""           # Name of edge gateway for NAT routing

network_name: ""                   # Desired network name
network_description: ""            # Description
enable_dual_subnet_network: true   # Enable both IPv4 and IPv6

# IPv4 subnet config
Gateway_CIDR_IPv4: "192.168.100.1"
ipv4_prefix_length: 24
ipv4_ip_ranges_string: "192.168.100.10-192.168.100.20"

# IPv6 subnet config
Gateway_CIDR_IPv6: "fd00:192:168:1::1"
ipv6_prefix_length: 64
ipv6_ip_ranges_string: "fd00:192:168:1::10-fd00:192:168:1::20"

# DNS settings
ipv4_dns_suffix: "example.com"
ipv4_dns_server1: "8.8.8.8"
ipv4_dns_server2: "1.1.1.1"



🏃‍♂️ How to Run the Playbook
Make sure you have Ansible installed (ansible-playbook --version).

Set the required variables in the routed.yml file or use --extra-vars.

Run the playbook:

