# Dummy Interface

```
###################################
# configure a dummy interface for use for host-local sharing with containers

- name: create 2 dummies, because ipvs uses dummy0 and we want dummy1
  copy:
    contents: |
      options dummy numdummies=2
    dest: /etc/modprobe.d/dummy.conf
    owner: root
    group: root
    mode: 0644

- name: activate this kernel module on boot
  copy:
    content: "dummy"
    dest: '/etc/modules-load.d/dummy.conf'
    owner: root
    group: root
    mode: 0644

- name: activate this module now
  modprobe:
    state=present
    name=dummy

- name: configure dummy1 for our use
  copy:
    contents: |
      NAME=dummy1
      DEVICE=dummy1
      MACADDR=00:22:22:ff:ff:ff
      IPADDR=169.254.1.1
      NETMASK=255.255.255.0
      ONBOOT=yes
      TYPE=Ethernet
      NM_CONTROLLED=no
    dest: /etc/sysconfig/network-scripts/ifcfg-dummy1
    owner: root
    group: root
    mode: 0644
```
