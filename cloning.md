### Cloning {#cloning}

Cloning or copying a container is a lot faster process to create containers if the requirement permits so. Cloning a container resets the MAC address for the cloned container and does not copy the snapshots of parent container.

$ lxc copy container1 container1-copy