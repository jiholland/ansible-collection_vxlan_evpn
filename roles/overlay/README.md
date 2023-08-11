🏭 jiholland.overlay
=====================

Configure overlay for multisite VXLAN-EVPN fabric on Cisco Nexus platform:
- iBGP EVPN control plane.
- Multicast-replication (BUM).
```YAML
                         BGP ASN 65001
        +--------------------+   +--------------------+
        |      SPINE-1       |   |      SPINE-2       |
        | RID 10.250.250.30  |   | RID 10.250.250.31  |
        +--------------------+   +--------------------+
                   | |                      | |
           +-+-----+-+----------------------+ |
           | |     +--------Loopback0---------+----+-+
           | |                                     | |
+--------------------+                             | |
|       LEAF-1       |                  +--------------------+
| RID 10.250.250.32  |                  |       LEAF-2       |
+--------------------+                  | RID 10.250.250.33  |
+ - - - - - - - - - -                   +--------------------+
         VTEP        |                  + - - - - - - - - - -
|     Loopback1                                  VTEP        |
   10.254.250.32/32  |                  |     Loopback1
|                                          10.254.250.33/32  |
 - - - - - - - - - - +                  + - - - - - - - - - -
+ - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
                 Distributed Anycast Gateway                 |
|                    GW IP 172.16.100.1
                    GW MAC 2020.DEAD.BEEF                    |
+ - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

defaults/main.yml:
- overlay_rid_if
- overlay_rid_ip
- overlay_vtep_if
- overlay_bgp_asn
- overlay_bgp_neighbors
- overlay_rmap_host_svi_name
- overlay_rmap_host_svi_tag
- overlay_anycast_gw_mac
- overlay_global_mcast_group_l2

Example Playbook
----------------
```YAML
---
- name: Build VXLAN-EVPN fabric.
  hosts: "{{ target }}"
  gather_facts: false

  roles:
    - role: jiholland.vxlan_evpn.vpc
      tags: vpc
      when: hostvars[inventory_hostname]['vpc_domain'] is defined
    - role: jiholland.vxlan_evpn.underlay
      tags: underlay
    - role: jiholland.vxlan_evpn.overlay
      tags: overlay
    - role: jiholland.vxlan_evpn.dci
      tags: dci
    - role: jiholland.vxlan_evpn.host_segments
      tags: host_segments
    - role: jiholland.vxlan_evpn.verify
      tags: verify
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
