### CONTAINER MANAGEMENT {#container-management}

LXD provides a very user-friendly command line interface to manage containers. One can perform activities like create, delete, copy, restart, snapshot, restore like many other activities to manage the containers.

Creating a container with the below shown command is very easy, it will create a container with best supported Ubuntu image from ubuntu: image server, set a random name and start it.

$ lxc launch ubuntu:

Creating a container using latest, stable image of Ubuntu 12.04, set a random name and start it.

$ lxc launch ubuntu:12.04

Creating a container using latest, stable image of Ubuntu 16.04, set name &quot;container0&quot; and start it.

$ lxc launch ubuntu:16.04 container0

To create a container using CentOS 7 64-bit image, set name &quot;container2&quot; and start it, we first have to search the &quot;images:&quot; remote image server and copy the required alias name.

$ lxc image list images: | grep centos | grep amd

$ lxc launch images:centos/7/amd64 container1

Creating a container using OpenSuSE 13.2 64-bit image, set name &quot;container3&quot; without starting it.

$ lxc init images:opensuse/13.2/amd64 container2

Remote image server &quot;ubuntu-daily&quot; can be used to create a container using latest development release of Ubuntu.

Listing containers

$ lxc list

Query detailed information of a particular container

$ lxc info container1

Start, stop, stop forcibly and restart containers

$ lxc start container1

$ lxc stop container1

$ lxc stop container1 --force

$ lxc restart container1

Stateful stop

Containers start from scratch after a reboot.

To make the changes persistent across reboots, a container needs to be stopped in a stateful state.

With the help of **_CRIU_**, the container state is written to the disk before shutting down. Next time the container starts, it restores the state previously written to disk.

$ lxc stop container1 --stateful

Pause containers

Paused containers do not use CPU but still are visible _and continue using memory._

$ lxc pause container1

Deletion and forceful deletion of containers

$ lxc delete container1

$ lxc delete container1 --force

Renaming Containers

Just like the Linux move command renames a particular file or directory, similarly the containers can also be renamed. A running container cannot be renamed. Renaming a container doesnot change it&#039;s MAC address.

$ lxc move container1 new-container