ansible-role-wireguard-vyatta
=========

A simple role to install Wireguard DEB package on Vyatta based routers and to configure peers

Currently only supports EdgeOS

To do both do installation and configuration, the playbook must be defined with two entries:

- First is to install the executable using regular SSH (`wireguard_install`)
- Second is to configure Vyatta using networking module (`wireguard_configure`)

Requirements
------------

Vyatta based units (Vyatta, EdgeOS, VyOS)

Role Variables
--------------

`wireguard_url` (default: `https://api.github.com/repos/Lochnair/vyatta-wireguard/releases`)

`wireguard_release` (default: `0.0.20191012-1`)

`wireguard_config_dir` (default: `/config/wireguard`)

`wireguard_wg_interfaces` (default: `[]`)

Dependencies
------------

None

Example Playbook
----------------

Playbook to run against EdgeOS routers.

```yaml
---
- name: EdgeOS wireguard install
  hosts: routers
  gather_facts: false
  vars:
    wireguard_install: true
    wireguard_configure: false
  roles:
    - ansible-role-wireguard-vyatta

- name: EdgeOS wireguard configure
  hosts: routers
  gather_facts: false
  connection: network_cli
  vars:
    ansible_network_os: edgeos
    wireguard_install: false
    wireguard_configure: true
    wireguard_wg_interfaces:
      - interface: wg0
        description: "VPN Clients"
        address: 192.168.58.1/24
        port: 51820
        peer:
          - id: "AAAAAAAAAABBBBBBBBBBCCCCCCCCCCCCDDDDDDDDDDD="
            description: "peer 1"
            allowed_ips: 192.168.53.101/32
  roles:
    - ansible-role-wireguard-vyatta
```

License
-------

Apache 2.0

Author Information
------------------

Author: Richard Holmboe <richard@dynamist.se>

Website: https://dynamist.se/en/
