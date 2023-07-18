🏭 jiholland.dci
================

Configure DCI for multisite VXLAN-EVPN fabric on Cisco Nexus platform.
- Border-leafs in vPC.
- eBGP in DCI underlay and overlay.
- Ingress-replication (BUM).
- MACsec encryption (dark-fiber).

Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

defaults/main.yml:
- dci_ospf_area_id
- dci_ospf_process_id
- dci_mtu
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

dci_if: loopback100
dci_ip: 10.10.2.1
dci_tag: 54321

dci_interfaces:
  - name: Ethernet1/5
    ip: 10.16.3.0
    mask: 31
  - name: Ethernet1/6
    ip: 10.16.3.2
    mask: 31

dci_bgp_neighbors:  # Underlay.
  - name: BGW-3
    ip: 10.16.3.1
    asn: 65002
    source: Ethernet1/5
  - name: BGW-4
    ip: 10.16.3.3
    asn: 65002
    source: Ethernet1/6

dci_ebgp_neighbors:  # Overlay.
  - name: BGW-3
    ip: 10.250.250.41
    asn: 65002
  - name: BGW-4
    ip: 10.250.250.42
    asn: 65002

dci_macsec_key: abc3000000000000000000000000000000000000000000000000000000000000  # Change and encrypt with Ansible vault

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
    - role: jiholland.vxlan_evpn.host_segments
    - role: jiholland.vxlan_evpn.verify
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
