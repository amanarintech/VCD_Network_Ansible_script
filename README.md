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

🧾 Playbook Overview: routed.yml
This playbook creates a NAT-Routed Org VDC Network in VMware Cloud Director, optionally with dual-stack (IPv4 + IPv6) support, using the vCD REST APIs. It includes:

- Authentication
- Organization & VDC lookup
- Edge Gateway discovery
- Subnet and IP range processing
- Payload construction
- Network creation and task monitoring



Header and Variable Definitions

- name: Create Routed VDC Network on VMware Cloud Director
  hosts: localhost
  gather_facts: false


- Playbook Name: Describes its purpose.
- hosts: localhost: Runs locally, not on remote machines.
  - gather_facts: false: Skips collecting system info; not needed here.


🧾 vars Breakdown — Explained by Category
These are the input variables the playbook relies on. Make sure they are defined correctly before execution.

🔐 API & Authentication

| Variable            | Description                                                                 |
| ------------------- | --------------------------------------------------------------------------- |
| `vcd_api_host_name` | The base URL of your vCloud Director instance (e.g., `vcloud.example.com`). |
| `vcd_api_version`   | vCD API version to use. For example: `"36.3"`.                              |
| `vcd_api_token`     | Your refresh token used to obtain an access token (OAuth).                  |


🏢 Organization & VDC Context

| Variable                    | Description                                                           |
| --------------------------- | --------------------------------------------------------------------- |
| `vcd_organization_name`     | The name of the organization where the network will be created.       |
| `vcd_organization_vdc_name` | The name of the Org VDC (Virtual Data Center) under the organization. |


🌐 Network Configuration

| Variable                     | Description                                                                    |
| ---------------------------- | ------------------------------------------------------------------------------ |
| `network_name`               | The desired name of the new routed network.                                    |
| `network_description`        | A human-readable description of the network.                                   |
| `enable_dual_subnet_network` | Boolean (`true` or `false`): Enable IPv4+IPv6 if `true`; only IPv4 if `false`. |
| `guest_vlan_allowed`         | Boolean: Allows guest VLAN tagging if `true`.                                  |


📶 IPv4 Configuration

| Variable                | Description                                                                                               |
| ----------------------- | --------------------------------------------------------------------------------------------------------- |
| `Gateway_CIDR_IPv4`     | IPv4 Gateway address (e.g., `192.168.100.1`).                                                             |
| `ipv4_prefix_length`    | Subnet prefix length, typically 24 for `/24`.                                                             |
| `ipv4_ip_ranges_string` | Comma-separated IPv4 address ranges. Ex: `"192.168.100.10-192.168.100.20,192.168.100.30-192.168.100.40"`. |


🌐 IPv6 Configuration (Optional)

| Variable                | Description                                                                         |
| ----------------------- | ----------------------------------------------------------------------------------- |
| `Gateway_CIDR_IPv6`     | IPv6 Gateway address (e.g., `fd00:192:168:1::1`).                                   |
| `ipv6_prefix_length`    | IPv6 subnet size, usually `64`.                                                     |
| `ipv6_ip_ranges_string` | Comma-separated IPv6 address ranges. Ex: `"fd00:192:168:1::10-fd00:192:168:1::20"`. |


🌍 DNS Configuration

| Variable           | Description                                   |
| ------------------ | --------------------------------------------- |
| `ipv4_dns_suffix`  | Domain suffix for DNS, e.g., `"example.com"`. |
| `ipv4_dns_server1` | Primary DNS server (IPv4).                    |
| `ipv4_dns_server2` | Secondary DNS server (IPv4).                  |


🔌 Edge Gateway

| Variable               | Description                                                      |
| ---------------------- | ---------------------------------------------------------------- |
| `edge_connection_name` | The name of the Edge Gateway to which this network will connect. |


