jiholland.overlay
==================

Configure overlay for multisite VXLAN-EVPN fabric on Cisco Nexus platform:
- iBGP EVPN control plane.
- Multicast-replication (BUM).
```YAML
                         BGP ASN 64501
        +--------------------+   +--------------------+
        |      SPINE-1       |   |      SPINE-2       |
        |   RID 192.0.2.30   |   |   RID 192.0.2.31   |
        +--------------------+   +--------------------+
                   | |                      | |
           +-+-----+-+----------------------+ |
           | |     +--------Loopback0---------+----+-+
           | |                                     | |
+--------------------+                             | |
|       LEAF-1       |                  +--------------------+
|   RID 192.0.2.32   |                  |       LEAF-2       |
+--------------------+                  |   RID 192.0.2.33   |
+ - - - - - - - - - -                   +--------------------+
         VTEP        |                  + - - - - - - - - - -
|     Loopback1                                  VTEP        |
   203.0.113.32/32   |                  |     Loopback1
|                                           203.0.113.33/32  |
 - - - - - - - - - - +                  + - - - - - - - - - -
+ - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
                 Distributed Anycast Gateway                 |
|                    GW IP 198.51.100.1
                    GW MAC 2020.DEAD.BEEF                    |
+ - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
Requirements
------------

[Cisco NXOS Collection](https://galaxy.ansible.com/ui/repo/published/cisco/nxos)

Role Variables
--------------

- overlay_network_role
- overlay_rid_if
- overlay_rid_ip
- overlay_vtep_if
- overlay_bgp_asn
- overlay_bgp_neighbors
- overlay_anycast_gw_mac
- overlay_global_mcast_group_l2

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
