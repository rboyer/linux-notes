mostly from: https://wiki.debian.org/QEMU

    $ qemu-img create -f qcow2 alpine.qcow 4G
    $ qemu-system-x86_64 -hda alpine.qcow -cdrom alpine-virt-3.9.4-x86_64.iso -boot d -m 512

then https://wiki.alpinelinux.org/wiki/Installation

    $ qemu-system-x86_64 -hda alpine.qcow  -m 512
