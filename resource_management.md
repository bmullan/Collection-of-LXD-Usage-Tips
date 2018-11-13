### RESOURCE MANAGEMENT {#resource-management}

LXD allows an efficient way to dynamically manage the resources like setting memory quotas, limiting CPU, I/O priorities and limiting disk usage. Resource allocation can be done on per container basis as well as globally through profiles. All limits can be configured in live environments where they can take effect immediately. In the below example, first command defines the limit on per container basis whereas the second sets the limits globally using profiles.

$ lxc config set **Illegal HTML tag removed :**<key><value></value></key>

$ lxc profile set **Illegal HTML tag removed :**<key><value></value></key>

Disk Limits

Unlike virtual machines containers don&#039;t reserve resources but allow us to limit the resources. Currently disk limits can be implemented only if ZFS or btrfs filesystems are in use.

CPU Limits

CPU limits can be configured using the following ways.

Limiting number of CPUs: Assigning only a particular number of CPUs, restricts LXD to use specified number of CPUs and not more than that. LXD load balances the workload among those number of CPUs as the containers start and stop. For example, we can allow LXD to use only 4 cores and it will load balance between them as per the requirement.

**$ lxc config set container1 limits.cpu 2**

Limiting to particular set of CPUs: Assigning only particular cores to be used by the containers. Load balance does not work here. For example, we can allow only cores 5, 7 and 8 to be used by the containers on a server.

$ lxc config set container1 limits.cpu 1,2,3,4

Pinning cpu core ranges

$ lxc config set container1 limits.cpu 0-2,7,8

Limiting to CPU usage percent: Containers can be limited to use only a particular percent of CPU time when under load even though containers can see all the cores. For example, a container can run freely when the system is not busy, but LXD can be configured to limit the CPU usage to 40% when there are a number of containers running.

$ lxc config set container1 limits.cpu.allowance 40%

Limiting CPU time: As in previous case, the containers can be limited to use particular CPU time even though the system is idle and they can see all the cores. For example, we can limit the containers to use only 50ms out of every 200ms interval of CPU time.

$ lxc config set container1 limits.cpu.allowance 50ms/200ms

The first two properties can be configured with last two to achieve a more complicated CPU resource allocation. For example, LXD makes it possible to limit 4 processors to use only 50ms of CPU time. We can also prioritize the usage in case there is a tiff between containers for a particular resource. In below example we set a priority of 50, if specified 0 it will provide least priority to the container among all.

$ lxc config set container1 limits.cpu.priority 50

Below command will help to verify the above set parameters.

$ lxc exec container1 -- cat /proc/cpuinfo | grep ^process