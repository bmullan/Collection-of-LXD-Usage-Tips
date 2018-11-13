### NETWORK MANAGEMENT 

By default, LXD does not listen to the network. To make it listen to the network following parameters can be set:

$ lxc config set core.https_address [::]

$ lxc config set core.trust_password

First parameter tells the LXD to bind all addresses on port 8443\. Second parameter creates a trust password to contact that server remotely. These are set to make communication between multiple LXD hosts. Any LXD host can add this LXD server using below command.

$ lxc remote add lxdserver1

Doing so will prompt for a password that we had set earlier.

One can now communicate with the LXD server and access the containers. For example, below command will update the OS in container &quot;container1&quot; on LXD server &quot;server1&quot;.

$ lxc exec lxdserver1:container1 --apt update

Proxy Configuration

Setups requiring HTTP(s) to reach out to the outside world can set the below configuration.

$ lxc config set core.prox_http

$ lxc config set core.prox_https

$ lxc config set core.prox_ignore_hosts

Any communication initiated by LXD will use the proxy server except for the local image server.
 
