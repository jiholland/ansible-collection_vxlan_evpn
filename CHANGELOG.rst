===================================
Jiholland.Vxlan\_Evpn Release Notes
===================================

.. contents:: Topics

v24.8.0
=======

Bugfixes
--------

- vpc role - added missing peer-switch command

v24.6.0
=======

Minor Changes
-------------

- all roles - assert quotes
- ansible-lint - moved to .config
- playbooks - added playbook dir

v24.3.1
=======

Minor Changes
-------------

- all roles - use quotes for user-side strings such as descriptions, names, and messages
- overlay and dci role - rmap tags should be list

v24.3.0
=======

Minor Changes
-------------

- all roles - timeout as variable
- all roles - use check_mode for some tasks
- dci role - various improvements

v24.2.2
=======

Minor Changes
-------------

- all roles - added arg spec
- all roles - use IPv4 Address Blocks Reserved for Documentation

v24.2.1
=======

Minor Changes
-------------

- all roles - typo in comment

v24.2.0
=======

Minor Changes
-------------

- all roles - moved some variables from defaults to vars
- all roles - prefix internal variables with double underscore
- all roles - set network_role variable as default
- host_segments role - fix host_segments_vrf variable typo
- overlay and vpc role - removed quotation marks from templates

v23.0.2
=======

Minor Changes
-------------

- galaxy - added more tags.

v23.0.1
=======

Minor Changes
-------------

- Fixed typoes in readme

v23.0.0
=======

