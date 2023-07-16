🏭 jiholland.dci
================

Configure DCI for multisite VXLAN-EVPN fabric on Cisco Nexus platform.

Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

defaults/main.yml:
- dci_ospf_area_id
- dci_ospf_process_id
- dci_mtu
- dci_macsec_key
- dci_bgp_asn
- dci_rid_if
- dci_rid_ip
- dci_rid_tag
- dci_fabric_interfaces

Example - hostvars/bgw-1.yml:
```YAML
---
# Hostvars for border-leaf

network_role: border-leaf

bgp_asn: 65001

rid_if: loopback0
rid_ip: 10.250.250.38
rid_tag: 54321

dci_desc: MULTISITE INTERFACE (VIP VTEP)
dci_if: loopback100
dci_ip: 10.10.2.1
dci_tag: 54321
dci_ebgp_neighbors:
  - name: BGW-3
    ip: 10.250.250.41
    asn: 65002
  - name: BGW-4
    ip: 10.250.250.42
    asn: 65002
dci_interfaces:
  - name: Ethernet1/5
    ip: 10.16.3.0
    mask: 31
    bgp_neighbor_ip: 10.16.3.1
    bgp_neighbor_name: BGW-3
    bgp_remote_asn: 65002
    tag: 54321
  - name: Ethernet1/6
    ip: 10.16.3.2
    mask: 31
    bgp_neighbor_ip: 10.16.3.3
    bgp_neighbor_name: BGW-4
    bgp_remote_asn: 65002
    tag: 54321

fabric_interfaces:
  - name: Ethernet1/7
  - name: Ethernet1/8
```
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
    - role: jiholland.vxlan_evpn.verify
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
