jiholland.dci
==============

Configure DCI for multisite VXLAN-EVPN fabric on Cisco Nexus platform:
- Border-leafs in vPC.
- eBGP in DCI underlay and overlay.
- Ingress-replication (BUM).
- MACsec encryption (dark-fiber).

Requirements
------------

[Cisco NXOS Collection](https://galaxy.ansible.com/ui/repo/published/cisco/nxos)

Role Variables
--------------

- dci_network_role
- dci_dci_if
- dci_dci_ip
- dci_dci_mask
- dci_dci_interfaces
- dci_bgp_asn
- dci_rid_if
- dci_rid_ip
- dci_underlay_bgp_neighbors
- dci_fabric_interfaces
- dci_overlay_bgp_neighbors
- dci_macsec_key_algorithm
- dci_macsec_key_string

Example Playbook
----------------
```YAML
---
- name: Build VXLAN-EVPN fabric.
  gather_facts: false
  hosts: "{{ target }}"

  roles:
    - role: jiholland.vxlan_evpn.vpc
      tags: vpc
      when: vpc_domain is defined
    - role: jiholland.vxlan_evpn.underlay
      tags: underlay
    - role: jiholland.vxlan_evpn.overlay
      tags: overlay
    - role: jiholland.vxlan_evpn.dci
      tags: dci
      when: dci_network_role is eq('border-leaf')
    - role: jiholland.vxlan_evpn.host_segments
      tags: host_segments
      when: host_segments_network_role is ansible.builtin.search('leaf')
    - role: jiholland.vxlan_evpn.verify
      tags: verify
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
