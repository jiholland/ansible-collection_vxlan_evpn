👯 jiholland.vpc
================

Configure a pair of Cisco Nexus devices in virtual Port Channel (vPC).

Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

defaults/main.yml:
- vpc_delay_restore
- vpc_svi_delay_restore
- vpc_native_vlan_id
- vpc_native_vlan_name

Example - hostvars:
```YAML
vpc_domain: 1
vpc_priority: 1

vpc_keepalive_ip_dest: 10.16.5.1
vpc_keepalive_ip_src: 10.16.5.2
vpc_keepalive_if: Ethernet1/3
vpc_keepalive_vrf: vpc-keepalive-link

vpc_peerlink_lag_name: port-channel1
vpc_peerlink_lag_members:
  - Ethernet1/1
  - Ethernet1/2

vpc_nve_ip: 10.16.4.1
vpc_nve_vlan: 777
```
Dependencies
------------

None.

Example Playbook
----------------
```YAML
---
- name: Configure a pair of Cisco Nexus devices in vPC.
  hosts: "{{ target }}"
  gather_facts: false

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
