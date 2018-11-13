### Configuring Containers {#configuring-containers}

Container settings like controlling container startup, including resource limitations and device pass-through options can be altered on live containers. LXD supports varioius devices like disk devices (physical disk, partition, block/character device), network devices (physical interface, bridged, macvlan, p2p) and none. None is used to stop inheritance of devices from profiles.

Profiles

Profiles store the container configuration. Any number of profiles can be applied to a container, but these profiles are applied in the order they are specified. Hence, always the last profile overrides the previous one. By default, LXD is preconfigured with &quot;default&quot; profile which comes with one network device connected to LXD&#039;s default bridge &quot;lxdbr0&quot;. Any new container that is created has &quot;default&quot; profile set.

Listing profiles

$ lxc profile list

Viewing default profile content

$ lxc profile show default

Editing default profile

$ lxc profile edit default

Applying a list of profiles to a container

$ lxc profile apply container1 **Illegal HTML tag removed :** <profile2><profile3>...</profile3></profile2>

Editing the configuration of a single container

$ lxc config edit container1

Adding a network device to container1

$ lxc config device add container eth1 nic nictype=bridged parent=lxcbr0

Listing the device configuration of container1

$ lxc config device list container1

Viewing container1 configuration

$ lxc config show container1

Above listed are a few examples of basic commands in use. There are many more options that can be used with these commands. A complete list of configuration parameters is mentioned .