🏭 jiholland.host_segments
==========================

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
|172.16.100.2/24| HOST-A$ ping 172.16.100.3  |172.16.100.3/24|
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
|172.16.100.2/24| HOST-A$ ping 172.16.101.2 |172.16.101.2/24|
|   VLAN 100    |- - - - - - - - - - - - - >|   VLAN 101    |
|   VNI 10100   |                           |   VNI 10101   |
+---------------+                           +---------------+
```
**Broadcast, Unknown Unicast, Multicast:**
- Note: BUM between sites (dci) are always ingress-replicated (unicast).
```YAML
                     + - - - - - - -
                      PIM Anycast RP|
                     + - - - - - - -
                     +--------------+
                     |   SPINE-1    |
                     +--------------+
                            ^ |  |
                            | |  +--------------239.0.0.2---------------+
                            | |                                         |
+-----+ +------239.0.0.1----+ +----239.0.0.1--------+ +-----+           |
|2.ARP| |                                           | |3.ARP|   +--------------+
+-----+ |                                           | +-----+   |    LEAF-3    |
        |                                           v           |     VTEP     |
+--------------+                            +--------------+    +--------------+
|    LEAF-1    |         VNI 10100          |    LEAF-2    |            |
|     VTEP     |--------------------------->|     VTEP     |            |
+--------------+                            +--------------+            |
        ^                                           |                   |
+-----+ |                                           | +-----+           |
|1.ARP| |                                           | |4.ARP|           |
+-----+ |                                           v +-----+           |
+---------------+                           +---------------+   +---------------+
|    HOST-A     | HOST-A$ ping 172.16.100.3 |    HOST-B     |   |    HOST-C     |
|172.16.100.2/24|   (MAC unknown --> ARP)   |172.16.100.3/24|   |172.16.101.2/24|
|   VLAN 100    |- - - - - - - - - - - - - >|   VLAN 100    |   |   VLAN 101    |
|   VNI 10100   |                           |   VNI 10100   |   |   VNI 10101   |
|Mcast 239.0.0.1|                           |Mcast 239.0.0.1|   |Mcast 239.0.0.2|
+---------------+                           +---------------+   +---------------+
```

Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

defaults/main.yml:

Example - hostvars/LEAF-1.yml:
```YAML
---
# Hostvars for leaf

network_role: leaf

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