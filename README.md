sysctl
======

Role for handling `/etc/sysctl.d/*.conf` files. Applies new sysctl setting on run.

Version
-------

* `1.2.0` --- remove ubuntu precise from testing
* `1.1.0` --- added ubuntu focal, 20.04
* `1.0.2` --- tested with Ansible 2.9.11
* `1.0.1` --- prepare for github
* `1.0.0` --- initial role
* `master` --- latest development version

Requirements
------------

This role is limited to

* Ubuntu 20.04 - Focal
* Ubuntu 18.04 - Bionic
* Ubuntu 16.04 - Xenial
* Ubuntu 14.04 - Trusty
* CentOS 8
* CentOS 7
* CentOS 6

Role Variables
--------------


* `sysctl_d` --- nested dictionary, key is filename in `/etc/sysctl.d`, value is dictionary with sysctl key and values, default `{}`
* `sysctl_exclusive` --- remove foreign files in `/etc/sysctl.d` - require reboot, default `false`

Dependencies
------------

None.

Example Playbook
----------------

Example variables is optimized 10G <100ms network. Enable qdisk `fq` on servers, `fq_codel` on routers, ie. VM hosts. Not enabled in example since this role should work on legacy OSes.

    - hosts: servers
      roles:
         - role: sysctl
           sysctl_d:
             50-network-100ms.conf:
               net.core.rmem_max: 67108864
               net.core.wmem_max: 67108864
               net.ipv4.tcp_rmem: 4096 87380 33554432
               net.ipv4.tcp_wmem: 4096 87380 33554432
               net.ipv4.tcp_mtu_probing: 1
               #net.core.default_qdisc: fq
            60-fast-start.conf:
               net.ipv4.tcp_slow_start_after_idle: 0

Testing
-------

### Test environment for all OSes

```bash
cd tests
vagrant up
```

### Rerun role

Run role on all OSes again.

```bash
vagrant provision
```

### Debug interactively

This uses cluster ssh to work with all vagrant boxes at the same time.

```bash
vagrant ssh-config > ~/.ssh/config
cat ~/.ssh/config | grep ^Host | cut -d\  -f2 | xargs cssh
```

License
-------

GPLv2

Author Information
------------------

Created 2020 by [Arnulf Heimsbakk](mailto:arnulf.heimsbakk@met.no) for MET Norway.

###### set vim: spell spelllang=en:
