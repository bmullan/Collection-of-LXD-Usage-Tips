### Network I/O Limits 

There are two types of network limits that can be applied to containers.

Network interface limits: The &quot;bridged&quot; and &quot;p2p&quot; type of interfaces can be allocated max bit/s limits.

$ lxc config device set container1 eth0 limits.ingress 100Mbit

$ lxc config device set container1 eth0 limits.egress 100Mbit

Global network limits: It prioritizes the usage if the container accessing the network interface is saturated with network traffic.

$ lxc config set container1 limits.network.priority 50
 
