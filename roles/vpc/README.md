👯 jiholland.vpc
================

Configure a pair of Cisco Nexus devices in virtual Port Channel (vPC).

Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/ui/repo/published/cisco/nxos)

Role Variables
--------------

- vpc_domain
- vpc_priority
- vpc_keepalive_ip_dest
- vpc_keepalive_ip_src
- vpc_keepalive_if
- vpc_keepalive_vrf
- vpc_peerlink_lag_name
- vpc_peerlink_lag_members
- vpc_native_vlan_id
- vpc_native_vlan_name

Dependencies
------------

None.

Example Playbook
----------------
```YAML
---
- name: Configure a pair of Cisco Nexus devices in vPC.
  gather_facts: false
  hosts: "{{ target }}"

  roles:
    - role: jiholland.vxlan_evpn.vpc
      when: vpc_domain is defined
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
