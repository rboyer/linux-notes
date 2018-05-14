LXD 2 (on ubuntu xenial)
========================

Install from backports (https://insights.ubuntu.com/2018/01/26/lxd-5-easy-pieces)

    sudo apt install -t xenial-backports lxd lxd-client

Both `lxd --version` and `lxc --version` should return something >= `2.21`.

Ensure you are in the lxd group (reboot or run `newgrp lxd` to apply).

    sudo usermod -G lxd -a <username>

Initialize LXD with some subnet:

    $ sudo lxd init
    Do you want to configure a new storage pool (yes/no) [default=yes]? 
    Name of the new storage pool [default=default]: 
    Name of the storage backend to use (dir, btrfs, lvm) [default=btrfs]: 
    Create a new BTRFS pool (yes/no) [default=yes]? 
    Would you like to use an existing block device (yes/no) [default=no]? 
    Size in GB of the new loop device (1GB minimum) [default=87GB]: 64
    Would you like LXD to be available over the network (yes/no) [default=no]? 
    Would you like stale cached images to be updated automatically (yes/no) [default=yes]? 
    Would you like to create a new network bridge (yes/no) [default=yes]? 
    What should the new bridge be called [default=lxdbr0]? 
    What IPv4 address should be used (CIDR subnet notation, “auto” or “none”) [default=auto]? 10.41.41.1/24
    Would you like LXD to NAT IPv4 traffic on your bridge? [default=yes]? 
    What IPv6 address should be used (CIDR subnet notation, “auto” or “none”) [default=auto]? none
    LXD has been successfully configured.

If you have to redo `lxd init` you may have to manually deprovision the
bridge device on LXD<2:

    sudo ip link set lxdbr0 down
    sudo brctl delbr lxdbr0

Create machine containers with static IPs (https://stgraber.org/2016/10/27/network-management-with-lxd-2-3/)

    $ lxc init images:centos/7 host1
    $ lxc network attach lxdbr0 host1 eth0
    $ lxc config device set host1 eth0 ipv4.address 10.41.41.2
    $ lxc start host1

Tip for scripting connecting to the box and waiting for networking:

    lxc exec host1 -- systemctl isolate multi-user.target
