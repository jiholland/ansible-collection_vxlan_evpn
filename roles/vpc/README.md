👯 jiholland.vpc
================

Configure a pair of Cisco Nexus devices in virtual Port Channel (vPC).

Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

defaults:
- vpc\_keepalive\_vrf
- vpc\_delay\_restore
- vpc\_svi\_delay\_restore
- vpc\_native\_vlan\_id
- vpc\_native\_vlan\_name

Example - hostvars:
```YAML
vpc:
  domain: 1
  role_priority: 1
  keepalive:
    ip_dest: 10.16.5.1
    ip_src: 10.16.5.2
    if: Ethernet1/3
  peerlink:
    lag_name: port-channel1
    lag_members:
      - Ethernet1/1
      - Ethernet1/2
  nve:
    ip: 10.16.4.1
    vlan: 777
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
    - role: jiholland.cisco.vpc
      when: vpc is defined
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
