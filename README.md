![hand](https://user-images.githubusercontent.com/1682855/48437763-41ac2200-e750-11e8-985a-3958d99e40ee.png)

*Please feel free to add/correct content or improve readability of anything in these documents.*

If you haven’t installed LXD yet then install and configure it on the container host computer or VM using either the APT or SNAP version of LXD.

[To install LXD on Ubuntu follow the instructions on the linuxcontainers.org website.](https://linuxcontainers.org/lxd/getting-started-cli/)

## Miscellaneous LXD Usage Tips/Hints Gathered from the Web 

Set the remote authentication password:

> **$lxc config set core.trust_password**

Change the default profile network interface:

> **$ lxc profile edit default**                # Change lxcbr0 to your value.

Create an image:

> **$ lxd-images import lxc ubuntu xenial amd64 --alias xenial --alias ubuntu/xenial --alias ubuntu/xenial/amd64**

You can Create an LXD container using a shortcut syntax. The following launches/creates an Ubuntu BIONIC (ie the “b”) container and calling it cn1.

> **$ lxc launch ubuntu:b cn1**

Show the log if the above failed for some reason:

> **$ lxc info cn1 --show-log**

Attach a shell on it:

> **$ lxc exec cn1 bash**

Delete the container you made:

> **$ lxc delete cn1**
 
Only if you are using the APT version of LXD install the LXD client:

> **$ sudo apt-add-repository -y ppa:ubuntu-lxc/stable**

> **$ sudo apt-get update**

> **$ sudo apt-get install lxd-client**

Add a remote for the server you just configured (the following is a 1 liner):

> **$ lxc remote add \<your-server-here\> https:\/\/\<your-server-fqdn-here\>:8443 --accept-certificate   # enter the password you've set above here.**

See if the remote works:

> **$ lxc list \<put-the-server-name-here\>**

Create a image from a container (publish it)

You need to delete the image first if you already have one with that aliases:

> **$ lxc delete \<server\>:ubuntu\/cn1\/amd64**

Now publish your container (make it available as image):

> **$ lxc publish \<server\>:\<container\> \<server\>: --alias ubuntu/cn1 --alias ubuntu/cn1/amd64**

Delete the image container if needed:

> **$ lxc delete \<server\>:cn1**

Launch a new container from the image you created:

> **$ lxc launch \<server>\:ubuntu/cn1 \<server\>:\<your-new-container-name\>**
  
You can also do:

> **$ lxc init \<server\>:ubuntu/cn1 \<server\>:\<your-new-container-name\>**
  
> **$ lxc start \<server\>:\<your-new-container-name\>**
  
Start a shell in the new container:

> **$ lxc exec \<server\>:\<new-container-name\> bash**
