---
changed: '2018-11-13T06:37:01.966512864'
created: '2016-09-27T21:34:28.978730642'
generator: 'LibreOffice 6.0.6.2 (Linux)'
---

**Everything LXD**

#### A Collection of Miscellaneous Information about

#### LXD Gathered from the Web

If you haven’t installed LXD yet then install and configure it on the container host computer or VM using either the APT or SNAP version of LXD.

To install LXD on Ubuntu follow the instructions on the linuxcontainers.org website:

<https://linuxcontainers.org/lxd/getting-started-cli/>

Set the remote authentication password:

**lxc config set core.trust\_password &lt;your-password-here&gt;**

Change the default profile network interface:

**lxc profile edit default** \# Change lxcbr0 to your value.

Create an image:

l**xd-images import lxc ubuntu xenial amd64 --alias xenial --alias ubuntu/xenial --alias ubuntu/xenial/amd64**

Create the container “cn1” from the image “ubuntu/cn1/amd64” you just made:

**lxc launch ubuntu/xenial/amd64 cn1**

You can Create an LXD container using a shortcut syntax. The following launches/creates an Ubuntu BIONIC (ie the “b”) container and calling it cn1.

**lxc launch ubuntu:b cn1**

Show the log if the above failed for some reason:

**lxc info cn1 --show-log**

Attach a shell on it:

**lxc exec cn1 bash**

Delete the container you made:

**lxc delete cn1**

On your own system if you are using the APT version of LXD install the LXD client:

**sudo apt-add-repository -y ppa:ubuntu-lxc/stable**

**sudo apt-get update**

**sudo apt-get install lxd-client**

Add a remote for the server you just configured (the following is a 1 liner):

**lxc remote add &lt;your-server-here&gt; https://&lt;your-server-fqdn-here&gt;:8443 --accept-certificate** \# enter the password you've set above here.

See if the remote works:

**lxc list &lt;put-the-server-name-here&gt;**

Create a image from a container (publish it)

You need to delete the image first if you already have one with one of that aliases:

**lxc delete &lt;server&gt;:ubuntu/cn1/amd64**

Now publish your container (make it available as image):

**lxc publish &lt;server&gt;:&lt;container&gt; &lt;server&gt;: --alias ubuntu/cn1 --alias ubuntu/cn1/amd64**

Delete the image container if needed:

**lxc delete &lt;server&gt;:cn1**

Launch a new container from the image you created:

**lxc launch &lt;server&gt;:ubuntu/cn1 &lt;server&gt;:&lt;your-new-container-name&gt;**

You can also do:

**lxc init &lt;server&gt;:ubuntu/cn1 &lt;server&gt;:&lt;your-new-container-name&gt;**

**lxc start &lt;server&gt;:&lt;your-new-container-name&gt;**

Start a shell in the new container:

**lxc exec srv01:&lt;new-container-name&gt; bash**

#### Ubuntu cloud-config with LXD

By default, LXD already uses ubuntu-cloudimg images. These are the same images used on Amazon AWS or Digital Ocean Public Clouds.

If you’ve not used Ubuntu on a Public Cloud before you may not know about the feature/capability called “*cloud-init*” or “*cloud-config*”. This capability allows you to preconfigure specific OS features/packages/etc when the Cloud instance is first started.

The information you pre-configure is termed “**user-data**”.

What you may not know is that with LXD that same capability exists for the LXC containers you create.

It turns out it is very easy to pass “***user-data***” to an LXD instance when you start it, just like you would on any cloud provider.

LXD even has the ***-e option to make your LXD instance ephemeral***. By “ephemeral” is meant that the LXC container will be deleted automatically when you “stop” it.

To create/install “user-data” create a file named &lt;something&gt;.yaml. The name can be anything.

Then start the LXC container:

**lxc launch ubuntu:16.04 cn2 -c user.user-data="$(cat &lt;something&gt;.yaml)"**

That is all there is to it.

Here is an example of a configuration:

\#cloud-config

output:

all: "|tee -a /tmp/cloud.out"

\#hostname: {{ hostname }}

bootcmd:

- rm -f /etc/dpkg/dpkg.cfg.d/multiarch

apt\_sources:

- source: ppa:yellow/ppa

ssh\_import\_id: \[evarlast\] \# use -S option

packages:

- make

final\_message: "The system is finally up, after $UPTIME seconds"

runcmd:

- cd /home/ubuntu

- git clone https://www.github.com/jrwren/myproject

- cd myproject

- make deps run

#### CONTAINER MANAGEMENT

LXD provides a very user-friendly command line interface to manage containers. One can perform activities like create, delete, copy, restart, snapshot, restore like many other activities to manage the containers.

Creating a container with the below shown command is very easy, it will create a container with best supported Ubuntu image from ubuntu: image server, set a random name and start it.

**$ lxc launch ubuntu:**

Creating a container using latest, stable image of Ubuntu 12.04, set a random name and start it.

**$ lxc launch ubuntu:12.04**

Creating a container using latest, stable image of Ubuntu 16.04, set name "container0" and start it.

**$ lxc launch ubuntu:16.04 container0**

To create a container using CentOS 7 64-bit image, set name "container2" and start it, we first have to search the "images:" remote image server and copy the required alias name.

**$ lxc image list images: | grep centos | grep amd**

**$ lxc launch images:centos/7/amd64 container1**

Creating a container using OpenSuSE 13.2 64-bit image, set name "container3" without starting it.

**$ lxc init images:opensuse/13.2/amd64 container2**

Remote image server "ubuntu-daily" can be used to create a container using latest development release of Ubuntu.

Listing containers

**$ lxc list**

Query detailed information of a particular container

**$ lxc info container1**

Start, stop, stop forcibly and restart containers

**$ lxc start container1**

**$ lxc stop container1**

**$ lxc stop container1 --force**

**$ lxc restart container1**

Stateful stop

Containers start from scratch after a reboot.

To make the changes persistent across reboots, a container needs to be stopped in a stateful state.

With the help of ***CRIU***, the container state is written to the disk before shutting down. Next time the container starts, it restores the state previously written to disk.

**$ lxc stop container1 --stateful**

Pause containers

Paused containers do not use CPU but still are visible *and continue using memory.*

**$ lxc pause container1**

Deletion and forceful deletion of containers

**$ lxc delete container1**

**$ lxc delete container1 --force**

Renaming Containers

Just like the Linux move command renames a particular file or directory, similarly the containers can also be renamed. A running container cannot be renamed. Renaming a container doesnot change it's MAC address.

**$ lxc move container1 new-container**

#### Configuring Containers

Container settings like controlling container startup, including resource limitations and device pass-through options can be altered on live containers. LXD supports varioius devices like disk devices (physical disk, partition, block/character device), network devices (physical interface, bridged, macvlan, p2p) and none. None is used to stop inheritance of devices from profiles.

Profiles

Profiles store the container configuration. Any number of profiles can be applied to a container, but these profiles are applied in the order they are specified. Hence, always the last profile overrides the previous one. By default, LXD is preconfigured with "default" profile which comes with one network device connected to LXD's default bridge "lxdbr0". Any new container that is created has "default" profile set.

Listing profiles

**$ lxc profile list**

Viewing default profile content

**$ lxc profile show default**

Editing default profile

**$ lxc profile edit default**

Applying a list of profiles to a container

**$ lxc profile apply container1 &lt;profile1&gt; &lt;profile2&gt; &lt;profile3&gt; ...**

Editing the configuration of a single container

**$ lxc config edit container1**

Adding a network device to container1

**$ lxc config device add container eth1 nic nictype=bridged parent=lxcbr0**

Listing the device configuration of container1

**$ lxc config device list container1**

Viewing container1 configuration

**$ lxc config show container1**

Above listed are a few examples of basic commands in use. There are many more options that can be used with these commands. A complete list of configuration parameters is mentioned &lt;here&gt;.

#### Executing Commands

Commands executed through LXD will always run as the container's root user.

Getting a shell inside the container

**$ lxc exec container1 bash**

#### File Transfers

LXD can directly read / write in the container's filesystem.

Pulling a file from container1

**$ lxc file pull container1 /etc/redhat-release ~**

Reading a file from container1

**$ lxc file pull container1 /etc/redhat-release -**

Pushing a file to container1

**$ lxc file push /etc/myfile container1/**

Editing a file on container1

**$ lxc file edit container1/etc/hosts**

#### SNAPSHOT MANAGEMENT

Snapshots help in preserving the point in time running state of containers including container's filesystem, devices and configuration, if --stateful flag is used.

A stateful snapshot can only be taken on a running container, where stateless snapshot can be taken on stopped containers.

Creating container1 stateless snapshot

**$ lxc snapshot container1**

Creating container1 stateful snapshot with name c1s1

**$ lxc snapshot container1 --stateful c1s1**

Listing snapshots

Number of snapshots created per container can be listed using below mentioned command.

**$ lxc list**

A detailed snapshot information related to container1 like snapshot name, stateless / stateful can be obtained by executing below command.

**$ lxc info container1**

Restoring snapshot

**$ lxc restore container1 c1s1**

Renaming snapshot

**$ lxc move container1/c1s1 container1/c1s1-new\\**

Creating a container using snapshot

**$ lxc copy container1/c1s1 container-c1s1**

Deleting snapshot

**$ lxc delete container1/c1s1**

#### Cloning

Cloning or copying a container is a lot faster process to create containers if the requirement permits so. Cloning a container resets the MAC address for the cloned container and does not copy the snapshots of parent container.

**$ lxc copy container1 container1-copy**

#### RESOURCE MANAGEMENT

LXD allows an efficient way to dynamically manage the resources like setting memory quotas, limiting CPU, I/O priorities and limiting disk usage. Resource allocation can be done on per container basis as well as globally through profiles. All limits can be configured in live environments where they can take effect immediately. In the below example, first command defines the limit on per container basis whereas the second sets the limits globally using profiles.

**$ lxc config set &lt;container&gt; &lt;key&gt; &lt;value&gt;**

**$ lxc profile set &lt;profile&gt; &lt;key&gt; &lt;value&gt;**

Disk Limits

Unlike virtual machines containers don't reserve resources but allow us to limit the resources. Currently disk limits can be implemented only if ZFS or btrfs filesystems are in use.

CPU Limits

CPU limits can be configured using the following ways.

Limiting number of CPUs: Assigning only a particular number of CPUs, restricts LXD to use specified number of CPUs and not more than that. LXD load balances the workload among those number of CPUs as the containers start and stop. For example, we can allow LXD to use only 4 cores and it will load balance between them as per the requirement.

**$ lxc config set container1 limits.cpu 2**

Limiting to particular set of CPUs: Assigning only particular cores to be used by the containers. Load balance does not work here. For example, we can allow only cores 5, 7 and 8 to be used by the containers on a server.

**$ lxc config set container1 limits.cpu 1,2,3,4**

Pinning cpu core ranges

**$ lxc config set container1 limits.cpu 0-2,7,8**

Limiting to CPU usage percent: Containers can be limited to use only a particular percent of CPU time when under load even though containers can see all the cores. For example, a container can run freely when the system is not busy, but LXD can be configured to limit the CPU usage to 40% when there are a number of containers running.

**$ lxc config set container1 limits.cpu.allowance 40%**

Limiting CPU time: As in previous case, the containers can be limited to use particular CPU time even though the system is idle and they can see all the cores. For example, we can limit the containers to use only 50ms out of every 200ms interval of CPU time.

**$ lxc config set container1 limits.cpu.allowance 50ms/200ms**

The first two properties can be configured with last two to achieve a more complicated CPU resource allocation. For example, LXD makes it possible to limit 4 processors to use only 50ms of CPU time. We can also prioritize the usage in case there is a tiff between containers for a particular resource. In below example we set a priority of 50, if specified 0 it will provide least priority to the container among all.

**$ lxc config set container1 limits.cpu.priority 50**

Below command will help to verify the above set parameters.

**$ lxc exec container1 -- cat /proc/cpuinfo | grep ^process**

#### Memory Limits

LXD can also limit memory usage in various ways that are pretty simple to use.

Limiting memory to use particular size of RAM. For example, limiting containers to use only 512MB of RAM.

**$ lxc config set container1 limits.memory 512MB**

Limiting memory to use particular percent of RAM. For example, limiting containers to use only 40% of total RAM.

**$ lxc config set container1 limits.memory 40%**

#### Limiting swap usage

A container can be configured to turn on / off swap device usage. We can also configure a container to swap out memory to the disk first on priority basis. By default swap is enabled for all containers.

**$ lxc config set container1 limits.memory.swap false**

Setting soft limits: Memory limits are hard by default. We can configure soft limits so that a container can enjoy full worth of memory as long as the system is idle. As soon as there is something that is important that has to be run on the system, a container cannot allocate anything until it is in it's soft limit.

**$ lxc config set container1 limits.memory.enforce soft**

#### Network I/O Limits

There are two types of network limits that can be applied to containers.

Network interface limits: The "bridged" and "p2p" type of interfaces can be allocated max bit/s limits.

**$ lxc config device set container1 eth0 limits.ingress 100Mbit**

**$ lxc config device set container1 eth0 limits.egress 100Mbit**

Global network limits: It prioritizes the usage if the container accessing the network interface is saturated with network traffic.

**$ lxc config set container1 limits.network.priority 50**

#### Block I/O Limits

***NOTE: Either ZFS or btrfs filesystem is required to set disk limits.***

**$ lxc config device set container1 root size 30GB**

Limiting the root device speed

**$ lxc config device set container1 root limits.read 40MB**

**$ lxc config device set container1 root limits.write 20MB**

Limiting root device IOps

**$ lxc config device set container1 root limits.read 30Iops**

**$ lxc config device set container1 root limits.write 20Iops**

Assigning priority to container1 for disk activity

**$ lxc config device set container1 limits.disk.priority 50**

To monitor the current resource usage (memory, disk & network) by container1

**$ lxc info container1**

Sharing a directory in the host machine with a container

**$ lxc config device add shared-path path=&lt;destination-directory-on-container&gt; source=&lt;source-directory-on-container&gt;**

#### NETWORK MANAGEMENT

By default, LXD does not listen to the network. To make it listen to the network following parameters can be set:

**$ lxc config set core.https\_address \[::\]**

**$ lxc config set core.trust\_password &lt;some-password&gt;**

First parameter tells the LXD to bind all addresses on port 8443. Second parameter creates a trust password to contact that server remotely. These are set to make communication between multiple LXD hosts. Any LXD host can add this LXD server using below command.

**$ lxc remote add lxdserver1 &lt;IP-Address&gt;**

Doing so will prompt for a password that we had set earlier.

One can now communicate with the LXD server and access the containers. For example, below command will update the OS in container "container1" on LXD server "server1".

**$ lxc exec lxdserver1:container1 --apt update**

Proxy Configuration

Setups requiring HTTP(s) to reach out to the outside world can set the below configuration.

**$ lxc config set core.prox\_http &lt;proxy-address&gt;**

**$ lxc config set core.prox\_https &lt;proxy-address&gt;**

**$ lxc config set core.prox\_ignore\_hosts &lt;local-image-server&gt;**

Any communication initiated by LXD will use the proxy server except for the local image server.

#### IMAGE MANAGEMENT

When a container is created from a remote image, LXD downloads the image by pulling its full hash, short hash or alias into it's image store, marks it as cached and records it's origin.

Importing Images

From Remote Image Servers to Local Image Store

LXD can simply cache the image locally by copying the remote image into the local image store. This process will not create a container from it.

Below example will simply copy the Ubuntu 14.04 image into the local image store and create a filesystem for it.

**$ lxc image copy ubuntu:14.04 local**

We can also provide an alias name for the fingerprint that will be generated for the new image. Specifying alias name is an easy way to remember the image.

**$ lxc image copy ubuntu:14.04 local: --alias ubuntu1404**

It is also possible to use the alias that are already set on the remote image server. LXD can also keep the local image updated just like the images that are cached by specifying the "--auto-update" flag while importing the image.

**$ lxc image copy images:centos/6/amd64 local: --copy-aliases --auto-update**

Later we can create a container using these local images.

**$ lxc launch centos/6/amd64 c2-centos6**

**From Tarballs to Local Image Store**

Alternatively, containers can also be made from images that are created using tarballs. These tarballs can be downloaded fromlinuxcontainers.org. There we can find one LXD metadata tarball and filesystem image tarball.

The below example will import an image using both tarballs and assign an alias "imported-ubuntu".

**$ lxc image import meta.tar.xz rootfs.tar.xz --alias imported-ubuntu**

**From URL to Local Image Store**

LXD also facilitates importing of images from a local webserver in order to create containers. Images can be pulled using their LXD-image-URL and ultimately get stored in the image store.

**$ lxc image import http://imageserver.org/lxd/images --alias opensuse132-amd64**

**Exporting Images**

The images of running containers stored in local image store can also be exported to tarballs.

**$ lxc image export &lt;fingerprint / alias&gt;**

Exporting images creates two tarballs: metadata tarball containing the metadata bits that LXD uses and filesystem tarball containing the root filesystem to bootstrap new containers.

**Creating & Publishing Images**

Creating Images using Containers

To create an image, stop the container whose image you want to publish in the local store, then we can create a new container using the new image.

**$ lxc publish container1 --alias new-c1s1**

A snapshot of a container can also be used to create images.

**$ lxc publish container1/c1s1 --alias new-snap-image**

**Creating Images Manually**

1. Generate the container filesystem for ubuntu usingdebootstrap.

2. Make a compressed tarball of the generated filesystem.

3. Write a metadata yaml file for the container.

4. Make a tarball of metadata.yaml file.

Sample metadata.yaml file

architecture: "i686"

creation\_date: 1458040200

properties:

architecture: "i686"

description: "Ubuntu 12.04 LTS server (20160315)"

os: "ubuntu"

release: "precise"

templates:

/var/lib/cloud/seed/nocloud-net/meta-data:

when:

- start

template: cloud-init-meta.tpl

/var/lib/cloud/seed/nocloud-net/user-data:

when:

- start

template: cloud-init-user.tpl

properties:

default: |

\#cloud-config

{}

/var/lib/cloud/seed/nocloud-net/vendor-data:

when:

- start

template: cloud-init-vendor.tpl

properties:

default: |

\#cloud-config

{}

/etc/init/console.override:

when:

- create

template: upstart-override.tpl

/etc/init/tty1.override:

when:

- create

template: upstart-override.tpl

/etc/init/tty2.override:

when:

- create

template: upstart-override.tpl

/etc/init/tty3.override:

when:

- create

template: upstart-override.tpl

/etc/init/tty4.override:

when:

- create

template: upstart-override.tpl

5. Import both the tarballs as LXD images.

**$ lxc image import &lt;rootfs.tar.gz&gt; &lt;meta.tar.gz&gt; --alias imported-container**

LXD is very likely going to deprecate the lxd-images import functionality for LXD. The image servers are much more efficient for this task.

By default, every LXD daemon plays image server role and every created image is private image i.e. only trusted clients can pull those private images. To create a public image the LXD server must be listening to the network. Below are a few steps to make the LXD server listen to the networ and serve as public image server.

1. Bind all addresses on port 8443 to enable remote connections to the LXD daemon.

**$ lxc config set core.https\_address "\[::\]:8443"**

2. Add public image server in the client machines.

**$ lxc remote add &lt;public-image-server&gt; &lt;IP-Address&gt; --public**

Adding a remote server as public provides an authentication-less connection between client and server. Still, the images that are marked as private in public image server cannot be accessed by the client.

Images can be marked as public / private using "lxc image edit" command described above in previous sections.

**Listing available remote image servers**

**$ lxc remote list**

List images in images: remote server

**$ lxc image list images:**

List images using filters

**$ lxc image list amd64**

**$ lxc image list os=ubuntu**

Get detailed information of an image

**$ lxc image info ubuntu**

#### Editing images

We can edit images with parameters like "autoupdate" or "public".

**$ lxc image edit container1**

**Deleting images**

**$ lxc image delete &lt;fingerprint / alias&gt;**

By default, after a period of 10 days the image gets removed automatically and also every 6 hours by default LXD daemon looks out for new updates and automatically updates the images that are cached locally.

Below mentioned commands with relevant parameters help to configure and tune the above mentioned default properties.

**$ lxc config set images.remote\_cache\_expiry &lt;no-of-days&gt;**

**$ lxc config set images.auto\_update\_interval &lt;no-of-hours&gt;**

**$ lxc config set images.auto\_update\_cached &lt;false&gt;**

The last parameter automatically updates only those images that have a flag "--auto-update" set and not all the images that are cached.

**Get current resource usage**

**$ lxc info my-container**

Reference

**$ lxc config set CONTAINER KEY VALUE**

**$ lxc config device set CONTAINER DEVICE KEY VALUE**

**$ lxc profile device set PROFILE DEVICE KEY VALUE**

CPU

**$ lxc config set my-container limits.cpu 2 \# limit to 1 cpu**

**$ lxc config set my-container limits.cpu 1,3 \# pin to specific CPUs**

**$ lxc config set my-container limits.cpu.allowance 10% \# CPU Allowance**

**$ lxc config set my-container limits.cpu.allowance 25ms/200ms \# CPU timeslice**

**$ lxc config set my-container limits.cpu.priority 0 \# Reduce priority of container**

Memory

**$ lxc config set my-container limits.memory 256MB \# limit to 256MB**

**$ lxc config set my-container limits.memory.swap false \# Turn off swap**

**$ lxc config set my-container limits.memory.swap.priority 0 \# swap container's**

**memory first**

**lxc config set my-container limits.memory.enfoce soft \# No hard limits**

Disk/block IO

***NOTE: Requires btrfs or zfs***

**$ lxc config device set my-container root size 20GB \#\# Restrict to 20GB space**

**$ lxc config device set my-container limits.read 30MB \#\# Limit speeds**

**$ lxc config device set my-container limits.write 10MB**

Network

**$ lxc config set my-container limits.network.priority 5**

<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">You can add as many tunnels to the configuration as you want.</span></span></span></span>**
**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">The default VXLAN configuration also uses multicast, so any host that's</span></span></span></span>**
**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">part of the same multicast group will be connected.</span></span></span></span>**
**<span style="font-weight: normal">Although Multicast is not supported by most ISP or Cloud Vendors you can choose among several options depending on your situation</span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">:</span></span></span></span>

<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">- GRE tunnels (unicast IPv4)</span></span></span></span>**
**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">- VXLAN group (multicast IPv4)</span></span></span></span>**
**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">- VXLAN point-to-point (unicast IPv4)</span></span></span></span>**
Examples:**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">:</span></span></span></span>

<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">To use </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">GRE unicast </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">tunneling:</span></span></span></span>

<span style="font-variant: normal"><span style="letter-spacing: normal"> </span></span>**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">tunnel.tun-a.protocol: gre</span></span></span>
<span style="font-variant: normal"><span style="letter-spacing: normal"> </span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">tunnel.tun-a.local: 1</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">0</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">.</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">0</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">.3.4 </span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">\# local TEP (tunnel end-point)</span></span></span>
<span style="font-variant: normal"><span style="letter-spacing: normal"> </span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">tunnel.tun-a.remote: 1</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">72</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">.</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">6</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">2.3.5 </span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">\# remote TEP</span></span></span>
**<span style="font-weight: normal">To use</span> **** <span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">VXLAN </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">in a</span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal"> multicast </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">mode environment </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">(</span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">assumes </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">default </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">G</span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">roup and </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">ID</span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">)</span></span></span></span>

**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">tunnel.tun-b.protocol: vxlan</span></span></span>
**<span style="font-weight: normal">To use </span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">VXLAN </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">in a </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">unicast </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">mode environment:</span></span></span></span>

**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">tunnel.tun-c.protocol: vxlan</span></span></span>
<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">tunnel.tun-c.local: 1</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">0</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">.</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">0</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">.3.4 </span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">\# local TEP (tunnel end-point is 10.0.3.4)</span></span></span>
<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">tunnel.tun-c.remote: 1</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">72</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">.</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">62</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">.3.5 </span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">\# remote TEP</span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"> </span></span></span>
**<span style="font-weight: normal">Y</span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">ou can have any number of those tunnels defined for any bridge, well, up to the bridge capacity, but that's 4096.</span></span></span></span>

<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">If you need more than 4096 VLANs then change your LXD bridge to an OVS (OpenVSwitch) by configuring</span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal"> "</span></span></span></span>**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal">bridge.driver: openvswitch</span></span></span>**<span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">" and </span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal">then there is no real practical VLAN</span></span></span></span><span style="font-variant: normal"><span style="letter-spacing: normal"><span style="font-style: normal"><span style="font-weight: normal"> limit.</span></span></span></span>
