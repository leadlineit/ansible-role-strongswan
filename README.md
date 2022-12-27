# Ansible Role Strongswan

![Build Status](https://github.com/leadlineit/ansible-role-strongswan/actions/workflows/ansible-galaxy-ci.yml/badge.svg)
[![Galaxy Role](https://img.shields.io/badge/Ansible--Galaxy-leadlineit.strongswan-blue.svg?logo=ansible&logoColor=white)](https://galaxy.ansible.com/leadlineit/strongswan/)

This role helps to for install and configure IPsec-based VPN solution with strongSwan on a Debian (stretch/buster/bullseye).

Requirements
------------

This role requires Ansible 2.11 or higher.

Role Variables
--------------

```yaml
---
strongswan_logs:
  - name: charon
    append: "no"
    def_loglevel: '1'
    flush_line: "yes"
    ike_name: "yes"
    path: "/var/log/strongswan/charon.log"
    time_format: "%b %e %T"
    time_add_ms: "no"

strongswan:
  s2s:
    - name: some.host1.com
      left: "234.56.78.90"
      leftsubnet: "172.16.0.0/12"
      right: "123.45.67.89"
      rightsubnet: "10.0.0.0/8"
      secret: "SomeH@rdPSKKey1"
      ike: "aes256-sha256-modp2048,aes192-sha256-modp2048,aes128-sha256-modp2048!"
      ikelifetime: 1d
      keyexchange: "ikev2"
      esp: "aes256-sha256-modp2048,aes192-sha256-modp2048,aes128-sha256-modp2048,aes256-sha1-modp2048,aes192-sha1-modp2048,aes128-sha1-modp2048!"
      forceencaps: "no"
      lifetime: "8h"
      dpdaction: "restart"
      dpdtimeout: "120s"
      dpddelay: "30s"
    - name: some.host2.com
      left: "76.54.32.10"
      leftsubnet: "10.254.0.0/24"
      right: "98.76.54.32"
      rightsubnet: "192.168.0.0/16"
      ike: "aes256-sha256-modp2048,aes192-sha256-modp2048,aes128-sha256-modp2048!"
      secret: "SomeH@rdPSKKey2"
  vpn:
    - name: vpn1.domain.com
      ssl_key: "/etc/letsencrypt/live/vpn1.domain.com/privkey.pem"
      left: "%any"
      leftsubnet: "10.254.0.0/24"
      leftcert: "fullchain.pem"
      leftfirewall: "yes"
      leftsendcert: "always"
      right: "%any"
      rightsourceip: "10.254.0.10-10.254.0.60"
      rightdns: "8.8.8.8,8.8.4.4"
      rightsendcert: "never"
      ike: "aes256-sha256-modp2048,aes192-sha256-modp2048,aes128-sha256-modp2048!"
      keyexchange: "ikev2"
      esp: "aes256-sha256-modp2048,aes192-sha256-modp2048,aes128-sha256-modp2048,aes256-sha1-modp2048,aes192-sha1-modp2048,aes128-sha1-modp2048!"
      rekey: "no"
      fragmentation: "yes"
      dpdaction: "clear"
      dpdtimeout: "300s"
      dpddelay: "60s"
      users:
        - name: user1
          password: "StrongP@ssForUser1"
        - name: user2
          password: "StrongP@ssForUser2"
```
Section for "vpn" an "s2s" are optional (if you doesn't define this vars - that steps will be skip).
The "strongswan_logs" is optional too.
You can skip the optional vars part entirely if you don't need to (that'll be for default).
Default value for this variables:

```yaml
---
strongswan_logs:
  - name: charon
    append: "no"
    def_loglevel: '1'
    flush_line: "yes"
    ike_name: "yes"
    path: "/var/log/strongswan/charon.log"
    time_format: "%b %e %T"
    time_add_ms: "no"
```

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: servers
  roles:
    - { role: leadlineit.strongswan, tags: strongswan }
```

License
-------

MIT

Author Information
------------------

This role was created by Artem Kasianchuk.
