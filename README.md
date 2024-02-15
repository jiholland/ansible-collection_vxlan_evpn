# Ansible Collection - jiholland.vxlan_evpn

This collection configure multisite VXLAN-EVPN fabric on Cisco Nexus platform.

Roles included in this collection (click on the link to see the role's README and documentation):

  - `jiholland.vxlan_evpn.vpc`([documentation](https://github.com/jiholland/ansible-collection_vxlan_evpn/blob/main/roles/vpc/README.md))
  - `jiholland.vxlan_evpn.underlay`([documentation](https://github.com/jiholland/ansible-collection_vxlan_evpn/blob/main/roles/underlay/README.md))
  - `jiholland.vxlan_evpn.overlay`([documentation](https://github.com/jiholland/ansible-collection_vxlan_evpn/blob/main/roles/overlay/README.md))
  - `jiholland.vxlan_evpn.dci`([documentation](https://github.com/jiholland/ansible-collection_vxlan_evpn/blob/main/roles/dci/README.md))
  - `jiholland.vxlan_evpn.host_segments`([documentation](https://github.com/jiholland/ansible-collection_vxlan_evpn/blob/main/roles/host_segments/README.md))
  - `jiholland.vxlan_evpn.verify`([documentation](https://github.com/jiholland/ansible-collection_vxlan_evpn/blob/main/roles/verify/README.md))

## Collection wide variables

These variables should be defined as host_vars in the inventory.
  - network_role:
    The role each network device has in the network; spine, leaf or border-leaf.

## Installation

Install via Ansible Galaxy:

```yaml
ansible-galaxy collection install jiholland.vxlan_evpn
```

📚**Resources:**<br>
- [Cisco Nexus VXLAN configuration guide](https://www.cisco.com/c/en/us/td/docs/dcn/nx-os/nexus9000/102x/configuration/vxlan/cisco-nexus-9000-series-nx-os-vxlan-configuration-guide-release-102x.html)<br>
- [Cisco VXLAN-EVPN Multi-Site Design and Deployment whitepaper](https://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/white-paper-c11-739942.html#Introduction)<br>
- [Ansible documentation for Cisco Nexus](https://docs.ansible.com/ansible/latest/collections/cisco/nxos/index.html)<br>

## License

BSD-3-Clause
