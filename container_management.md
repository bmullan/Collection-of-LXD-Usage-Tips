### CONTAINER MANAGEMENT 

LXD provides a very user-friendly command line interface to manage containers. 

One can perform activities like create, delete, copy, restart, snapshot, restore like many other activities to manage the containers.

Creating a container with the below shown command is very easy, it will create a container with best supported Ubuntu image from ubuntu: image server, set a random name and start it.

> **$ lxc launch ubuntu:**

Creating a container using latest, stable image of Ubuntu 16.04, set a random name and start it.

> **$ lxc launch ubuntu:16.04**

Creating a container using latest, stable image of Ubuntu 18.04, set the Container name to cn1 and start cn1.

> **$ lxc launch ubuntu:18.04 cn1**

To create a container using CentOS 7 64-bit image, set name to "centos1" and start it, we first have to search the &quot;images:&quot; remote image server and copy the required alias name.

> **$ lxc image list images: | grep centos | grep amd**

> **$ lxc launch images:centos/7/amd64 centos1**

Creating a container using OpenSuSE 13.2 64-bit image, set name to "opensuse1" without starting it.

> **$ lxc init images:opensuse/13.2/amd64 opensuse1

Remote image server &quot;ubuntu-daily&quot; can be used to create a container using latest development release of Ubuntu.

Listing local containers

> **$ lxc list**

Query detailed information of a particular container (example cn1)

> **$ lxc info cn1**

Start, stop, stop forcibly and restart containers

> **$ lxc start cn1**

> **$ lxc stop cn1**

> **$ lxc stop cn1 --force**

> **$ lxc restart cn1**

Stateful stop

Containers start from scratch after a reboot.

To make the changes persistent across reboots, a container needs to be stopped in a stateful state.

With the help of **_CRIU_**, the container state is written to the disk before shutting down. Next time the container starts, it restores the state previously written to disk.

> **$ lxc stop cn1 --stateful**

Pause containers

Paused containers do not use CPU but still are visible _and continue using memory._

> **$ lxc pause cn1**

Deletion and forceful deletion of containers

> **$ lxc delete cn1**

> **$ lxc delete cn1 --force**

Renaming Containers

Just like the Linux move command renames a particular file or directory, similarly the containers can also be renamed. 

A running container cannot be renamed. 

Renaming a container *does not change its MAC address*.

example:  Rename container CN1 to CN2

> **$ lxc move cn1 cn2**
