🏭 jiholland.verify
===================

Verify multisite VXLAN-EVPN fabric on Cisco Nexus platform.

Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

None.

Example Playbook
----------------
```YAML
---
- name: Build VXLAN-EVPN fabric.
  hosts: "{{ target }}"
  gather_facts: false

  roles:
    - role: jiholland.vxlan_evpn.vpc
      when: vpc_domain is defined
    - role: jiholland.vxlan_evpn.underlay
    - role: jiholland.vxlan_evpn.overlay
    - role: jiholland.vxlan_evpn.dci
    - role: jiholland.vxlan_evpn.host_segments
    - role: jiholland.vxlan_evpn.verify
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
