### File Transfers 

LXD can directly read / write in the container&#039;s filesystem.

Pulling a file from container1

> **$ lxc file pull container1 /etc/redhat-release ~**

Reading a file from container1

> **$ lxc file pull container1 /etc/redhat-release -**

Pushing a file to container1

> **$ lxc file push /etc/myfile container1/**

Editing a file on container1

> **$ lxc file edit container1/etc/hosts**
