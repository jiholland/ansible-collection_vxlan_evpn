jiholland.host_segments
=======================

Configure host segments for multisite VXLAN-EVPN fabric on Cisco Nexus platform.<br>

**L2 Host Segment:**
```YAML
+--------------+                             +--------------+
|    LEAF-1    |         VNI 10100           |    LEAF-2    |
|     VTEP     |---------------------------->|     VTEP     |
+--------------+                             +--------------+
        ^                                            |
        |                                            |
        | VLAN 100                         VLAN 100  v
+---------------+                            +---------------+
|    HOST-A     |                            |    HOST-B     |
|  192.0.2.2/24 |  HOST-A$ ping 192.0.2.3    |  192.0.2.3/24 |
|   VLAN 100    |- - - - - - - - - - - - - ->|   VLAN 100    |
|   VNI 10100   |                            |   VNI 10100   |
+---------------+                            +---------------+
```
**L3 Host Segment:**
- Distributed anycast gateway.
- Multi-tenancy (VRFs).
- ARP suppression at leafs.
```YAML
                        VRF Tenant-1
                        L3 VNI 10010
+--------------+                            +--------------+ 
|    LEAF-1    |         VNI 10010          |    LEAF-2    | 
|     VTEP     |--------------------------->|     VTEP     | 
+--------------+                            +--------------+ 
        ^                                           |        
        |                                           |        
        | VLAN 100                         VLAN 101 v        
+---------------+                           +---------------+
|    HOST-A     |                           |    HOST-C     |
| 192.0.2.2/24  | HOST-A$ ping 203.0.113.2  |203.0.113.2/24 |
|   VLAN 100    |- - - - - - - - - - - - - >|   VLAN 101    |
|   VNI 10100   |                           |   VNI 10101   |
+---------------+                           +---------------+
```
**Broadcast, Unknown Unicast, Multicast**<br>
Note: BUM between sites (dci) are always ingress-replicated (unicast).
```YAML
                     + - - - - - - -
                      PIM Anycast RP|
                     + - - - - - - -
                     +--------------+
                     |   SPINE-1    |
                     +--------------+
                            ^ |  |
                            | |  +--------------233.252.0.2--------------+
                            | |                                          |
+-----+ +----233.252.0.1----+ +----233.252.0.1------+ +-----+            |
|2.ARP| |                                           | |3.ARP|    +--------------+
+-----+ |                                           | +-----+    |    LEAF-3    |
        |                                           v            |     VTEP     |
+--------------+                            +--------------+     +--------------+
|    LEAF-1    |         VNI 10100          |    LEAF-2    |             |
|     VTEP     |--------------------------->|     VTEP     |             |
+--------------+                            +--------------+             |
        ^                                           |                    |
+-----+ |                                           | +-----+            |
|1.ARP| |                                           | |4.ARP|            |
+-----+ |                                           v +-----+            |
+-----------------+                         +-----------------+   +-----------------+
|     HOST-A      | HOST-A$ ping 192.0.2.3  |     HOST-B      |   |     HOST-C      |
|   192.0.2.2/24  | (MAC unknown --> ARP)   |  192.0.2.3/24   |   |  203.0.113.2/24 |
|    VLAN 100     |- - - - - - - - - - - - >|    VLAN 100     |   |    VLAN 101     |
|    VNI 10100    |                         |    VNI 10100    |   |    VNI 10101    |
|Mcast 233.252.0.1|                         |Mcast 233.252.0.1|   |Mcast 233.252.0.2|
+-----------------+                         +-----------------+   +-----------------+
```

Requirements
------------

[Cisco NXOS Collection](https://galaxy.ansible.com/ui/repo/published/cisco/nxos)

Role Variables
--------------

- host_segments_network_role
- host_segments_l2
- host_segments_l3
- host_segments_vrf

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
