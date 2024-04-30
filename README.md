sysctl
======

Role for handling `/etc/sysctl.d/*.conf` files. Applies new sysctl setting on run.

Version
-------

* `3.0.0` --- Add Unsible-core 2.16. Removed support for Ubuntu xenial and bionic
* `2.2.0` --- Add Ubuntu 24.04 noble support
* `2.1.2` --- Allow Fedora CoreOS 39
* `2.1.1` --- bug fix, ansible-lint
* `2.1.0` --- add Fedora CoreOS support
* `2.0.0` --- updated for ansible 2.12.9
* `1.5.0` --- add RHEL9 and CentOS Stream 8 support
* `1.4.0` --- add jammy support; remove centos8 support
* `1.3.0` --- add rhel8 support; remove trusty and centos6 support
* `1.2.0` --- remove ubuntu precise from testing
* `1.1.0` --- added ubuntu focal, 20.04
* `1.0.2` --- tested with Ansible 2.9.11
* `1.0.1` --- prepare for github
* `1.0.0` --- initial role
* `master` --- latest development version

Requirements
------------

This role is limited to

* Ubuntu 24.04 - Noble
* Ubuntu 22.04 - Jammy
* Ubuntu 20.04 - Focal
* CentOS 7
* CentOS Stream 8
* RHEL 8
* RHEL 9
* Fedora CoreOS 38
* Fedora CoreOS 39

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

NOTICE: Fedora CoreOS is tested manually, but currently no automatic tests
are added for FCOS.

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

Created 2020 by IT Infrastructure at MET Norway
Contactpoint: [IT Infrastructure Basis Team](mailto:it-is-basis@met.no)

###### set vim: spell spelllang=en:
