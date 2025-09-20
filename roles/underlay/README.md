jiholland.underlay
===================

Configure underlay for multisite VXLAN-EVPN fabric on Cisco Nexus platform:
- OSPF routing.
- P2P L3 links.
- PIM Sparse with Anycast RP.
```YAML
                       OSPF AREA 0.0.0.0
       + - - - - - - - - - - - - - - - - - - - - - - -
       |         PIM Anycast RP (Loopback254)         |
       + - - - - - - - - - - - - - - - - - - - - - - -
       +--------------------+    +--------------------+
       |      SPINE-1       |    |      SPINE-2       |
       |   RID 192.0.2.30   |    |    RID 192.0.2.31  |
       +--------------------+    +--------------------+
                | |                       | |
           +-+--+-+-----------------------+ |
           | |  +---------Loopback0---------+-----+-+
           | |                                    | |
+--------------------+                            | |
|       LEAF-1       |                 +------------+-------+
|   RID 192.0.2.32   |                 |       LEAF-2       |
+--------------------+                 |    RID 192.0.2.33  |
                                       +--------------------+
```
Requirements
------------

[Cisco NXOS Collection](https://galaxy.ansible.com/ui/repo/published/cisco/nxos)

Role Variables
--------------

- underlay_network_role
- underlay_fabric_interfaces
- underlay_rid_if
- underlay_rid_ip
- underlay_rid_mask
- underlay_vtep_if
- underlay_vtep_ip
- underlay_vtep_secondary_ip
- underlay_vtep_mask
- underlay_pim_if
- underlay_pim_mask
- underlay_pim_ip_remote
- underlay_pim_group_range
- underlay_vpc_domain
- underlay_vpc_nve_vlan
- underlay_vpc_nve_ip

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
