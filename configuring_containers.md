### Configuring Containers 

Container settings like controlling container startup, including resource limitations and device pass-through options can be altered on live containers. 

LXD supports varioius devices like disk devices (physical disk, partition, block/character device), network devices (physical interface, bridged, macvlan, p2p) and none. None is used to stop inheritance of devices from profiles.

**Profiles**

Profiles store the container configuration. 

Any number of profiles can be applied to a container, but these profiles are applied in the order they are specified. 

Remember this as the last profile overrides the previous one. 

By default, LXD is preconfigured with a *default* profile which comes with one network device connected to LXDBR0 (LXD's default bridge). 

Any new container that is created has the "default" profile set.

Listing profiles

> **$ lxc profile list**

Viewing default profile content

> **$ lxc profile show default**

Editing default profile

> **$ lxc profile edit default**

Applying a list of profiles to a container named cn1

> **$ lxc profile apply cn1 \<profile1\> \<profile2\> \<profile3\> ...**

Editing the configuration of a single container

> **$ lxc config edit cn1**

Adding a network device to cn1

> **$ lxc config device add cn1 eth1 nic nictype=bridged parent=lxcbr0**

Listing the device configuration of container1

> **$ lxc config device list cn1**

Viewing container cn1's configuration

> **$ lxc config show cn1**

Above listed are a few examples of basic commands in use. 

There are many more options that can be used with these commands. 

A complete list of configuration parameters is mentioned in the Document section of www.linuxcontainers.org LXD subsection.
