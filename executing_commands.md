### Executing Commands 

Commands executed through LXD will always run as the container&#039;s root user.

Getting a shell inside a container named cn1

> **$ lxc exec cn1 bash**

Executing any general command line in cn1 from the Host

> **$ lxc exec \[\<remote\>:\]cn1 \[flags\] \[--\] \<command line\>

example: change directory to /tmp in cn1 the execute pwd to print the working directory to see that it worked

> **$ lxc exec cn1 -- sh -c "cd /tmp \&\& pwd"**
