### Editing LXD image profiles 

We can edit images with parameters like &quot;autoupdate&quot; or &quot;public&quot;.

> **$ lxc image edit cn1**

Deleting images

> **$ lxc image delete image_name**
  
By default, after a period of 10 days the image gets removed automatically and also every 6 hours by default LXD daemon looks out for new updates and automatically updates the images that are cached locally.

Below mentioned commands with relevant parameters help to configure and tune the above mentioned default properties.

> **$ lxc config set images.remote_cache_expiry**

> **$ lxc config set images.auto_update_interval**

> **$ lxc config set images.auto_update_cached**

The last parameter automatically updates only those images that have a flag &quot;--auto-update&quot; set and not all the images that are cached.

Get current resource usage

> **$ lxc info cn1**

Reference command syntax:

> **$ lxc config set CONTAINER KEY VALUE**

> **$ lxc config device set CONTAINER DEVICE KEY VALUE**

> **$ lxc profile device set PROFILE DEVICE KEY VALUE**

Examples for CPU

> **$ lxc config set cn1 limits.cpu 2 # limit to 1 cpu**

> **$ lxc config set cn1 limits.cpu 1,3                              # pin to specific CPUs in this case CPU 1 and 3**

> **$ lxc config set cn1 limits.cpu.allowance 10%           # CPU use Allowance of 10%**

> **$ lxc config set cn1 limits.cpu.allowance 25ms/200ms    # CPU timeslice**

> **$ lxc config set cn1 limits.cpu.priority 0              # Reduce priority of container**

Examples for Memory

> **$ lxc config set cn1 limits.memory 256MB                # limit CN1 container to 256MB of RAM**

> **$ lxc config set cn1 limits.memory.swap false           # Turn CN1 conatiner use of swap off**

> **$ lxc config set cn1 limits.memory.swap.priority 0      # set CN1 conatiner swap priority to &#039;s**

Examples for memory

> **$ lxc config set cn1 limits.memory.enfoce soft          # CN1 has no hard limits**

Examples for Disk/block IO

*NOTE: Requires backing store use of btrfs or zfs*

> **$ lxc config device set cn1 root size 20GB              # Restrict CN1 to 20GB of space*

> **$ lxc config device set cn1 limits.read 30MB            # Limit speeds**

> **$ lxc config device set cn1 limits.write 10MB   

Examples for Network

> **$ lxc config set cn1 limits.network.priority 5**

You can add as many tunnels to the configuration as you want.

The default VxLAN configuration also uses multicast, so any host that&#039;spart of the same multicast group will be connected.

Although Multicast is not supported by most ISP or Cloud Vendors you can choose among several options depending on your situation:

- GRE tunnels (unicast IPv4)  
- VXLAN group (multicast IPv4)  
- VXLAN point-to-point (unicast IPv4)  

Examples (note: *TEP = Tunnel End Point*):

To use GRE unicast tunneling:

> **tunnel.tun-a.protocol: gre**  
> **tunnel.tun-a.local: 10.0.3.4            # local TEP (tunnel end-point)**  
> **tunnel.tun-a.remote: 172.62.3.5         # remote TEP**

To use VxLAN in a multicast mode environment (assumes default Group and ID)

> **tunnel.tun-b.protocol: vxlan**

To use VXLAN in a unicast mode environment:

> **tunnel.tun-c.protocol: vxlan**  
> **tunnel.tun-c.local: 10.0.3.4           # local TEP (tunnel end-point is 10.0.3.4)**  
> **tunnel.tun-c.remote: 172.62.3.5        # remote TEP  

You can have any number of those tunnels defined for any bridge... well, up to the bridge capacity but that's 4096.

If you need more than 4096 VLANs then change your LXD bridge to an OVS (OpenVSwitch) by configuring:

> **bridge.driver: openvswitch** 

and then there is no real practical VLAN limit.
