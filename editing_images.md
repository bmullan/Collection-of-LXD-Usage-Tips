### Editing images {#editing-images}

We can edit images with parameters like &quot;autoupdate&quot; or &quot;public&quot;.

$ lxc image edit container1

Deleting images

$ lxc image delete

By default, after a period of 10 days the image gets removed automatically and also every 6 hours by default LXD daemon looks out for new updates and automatically updates the images that are cached locally.

Below mentioned commands with relevant parameters help to configure and tune the above mentioned default properties.

$ lxc config set images.remote_cache_expiry

$ lxc config set images.auto_update_interval

$ lxc config set images.auto_update_cached

The last parameter automatically updates only those images that have a flag &quot;--auto-update&quot; set and not all the images that are cached.

Get current resource usage

$ lxc info my-container

Reference

$ lxc config set CONTAINER KEY VALUE

$ lxc config device set CONTAINER DEVICE KEY VALUE

$ lxc profile device set PROFILE DEVICE KEY VALUE

CPU

$ lxc config set my-container limits.cpu 2 # limit to 1 cpu

$ lxc config set my-container limits.cpu 1,3 # pin to specific CPUs

$ lxc config set my-container limits.cpu.allowance 10% # CPU Allowance

$ lxc config set my-container limits.cpu.allowance 25ms/200ms # CPU timeslice

$ lxc config set my-container limits.cpu.priority 0 # Reduce priority of container

Memory

$ lxc config set my-container limits.memory 256MB # limit to 256MB

$ lxc config set my-container limits.memory.swap false # Turn off swap

$ lxc config set my-container limits.memory.swap.priority 0 # swap container&#039;s

memory first

lxc config set my-container limits.memory.enfoce soft # No hard limits

Disk/block IO

NOTE: Requires btrfs or zfs

$ lxc config device set my-container root size 20GB ## Restrict to 20GB space

$ lxc config device set my-container limits.read 30MB ## Limit speeds

$ lxc config device set my-container limits.write 10MB

Network

$ lxc config set my-container limits.network.priority 5

You can add as many tunnels to the configuration as you want.The default VXLAN configuration also uses multicast, so any host that&#039;spart of the same multicast group will be connected.Although Multicast is not supported by most ISP or Cloud Vendors you can choose among several options depending on your situation:

- GRE tunnels (unicast IPv4)- VXLAN group (multicast IPv4)- VXLAN point-to-point (unicast IPv4)Examples::

To use GRE unicast tunneling:

tunnel.tun-a.protocol: gretunnel.tun-a.local: 10.0.3.4 # local TEP (tunnel end-point)tunnel.tun-a.remote: 172.62.3.5 # remote TEPTo useVXLAN in a multicast mode environment (assumes default Group and ID)

tunnel.tun-b.protocol: vxlanTo use VXLAN in a unicast mode environment:

tunnel.tun-c.protocol: vxlantunnel.tun-c.local: 10.0.3.4 # local TEP (tunnel end-point is 10.0.3.4)tunnel.tun-c.remote: 172.62.3.5 # remote TEPYou can have any number of those tunnels defined for any bridge, well, up to the bridge capacity, but that&#039;s 4096.

If you need more than 4096 VLANs then change your LXD bridge to an OVS (OpenVSwitch) by configuring &quot;bridge.driver: openvswitch&quot; and then there is no real practical VLAN limit.