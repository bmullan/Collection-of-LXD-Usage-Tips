### Limiting swap usage 

A container can be configured to turn on / off swap device usage. We can also configure a container to swap out memory to the disk first on priority basis. By default swap is enabled for all containers.

> **$ lxc config set container1 limits.memory.swap false**

Setting soft limits: Memory limits are hard by default. We can configure soft limits so that a container can enjoy full worth of memory as long as the system is idle. As soon as there is something that is important that has to be run on the system, a container cannot allocate anything until it is in it&#039;s soft limit.

> **$ lxc config set container1 limits.memory.enforce soft**
