### Block I/O Limits {#block-i-o-limits}

NOTE: Either ZFS or btrfs filesystem is required to set disk limits.

$ lxc config device set container1 root size 30GB

Limiting the root device speed

$ lxc config device set container1 root limits.read 40MB

$ lxc config device set container1 root limits.write 20MB

Limiting root device IOps

$ lxc config device set container1 root limits.read 30Iops

$ lxc config device set container1 root limits.write 20Iops

Assigning priority to container1 for disk activity

$ lxc config device set container1 limits.disk.priority 50

To monitor the current resource usage (memory, disk &amp; network) by container1

$ lxc info container1

Sharing a directory in the host machine with a container

$ lxc config device add shared-path path=**Illegal HTML tag removed :** source=<source-directory-on-container></source-directory-on-container>