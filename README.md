# Ansible Collection - jiholland.vxlan_evpn

This collection includes helpful Ansible roles and content to help with Cisco VXLAN-EVPN automation.

Roles included in this collection (click on the link to see the role's README and documentation):

  - `jiholland.vxlan_evpn.vpc`([documentation](https://github.com/jiholland/ansible-collection_vxlan_evpn/blob/main/roles/vpc/README.md))
  - `jiholland.vxlan_evpn.vxlan_evpn`([documentation](https://github.com/jiholland/ansible-collection_vxlan_evpn/blob/main/roles/vxlan_evpn/README.md))

## Installation

Install via Ansible Galaxy:

```yaml
ansible-galaxy collection install git@github.com:jiholland/ansible-collection_vxlan_evpn.git
```

## Usage
```YAML
---
- name: Build VXLAN-EVPN fabric.
  hosts: "{{ target }}"
  gather_facts: false

  roles:

    - role: jiholland.vxlan_evpn.vpc
      when: vpc is defined

    - role: jiholland.vxlan_evpn.vxlan_evpn
```
## License

BSD-3-Clause
