### SNAPSHOT MANAGEMENT 

Snapshots help in preserving the point in time running state of containers including container&#039;s filesystem, devices and configuration, if --stateful flag is used.

A stateful snapshot can only be taken on a running container, where stateless snapshot can be taken on stopped containers.

Creating container1 stateless snapshot

$ lxc snapshot container1

Creating container1 stateful snapshot with name c1s1

$ lxc snapshot container1 --stateful c1s1

Listing snapshots

Number of snapshots created per container can be listed using below mentioned command.

$ lxc list

A detailed snapshot information related to container1 like snapshot name, stateless / stateful can be obtained by executing below command.

$ lxc info container1

Restoring snapshot

$ lxc restore container1 c1s1

Renaming snapshot

$ lxc move container1/c1s1 container1/c1s1-new\

Creating a container using snapshot

$ lxc copy container1/c1s1 container-c1s1

Deleting snapshot

$ lxc delete container1/c1s1
