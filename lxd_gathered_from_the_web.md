### LXD Gathered from the Web {#lxd-gathered-from-the-web}

If you haven’t installed LXD yet then install and configure it on the container host computer or VM using either the APT or SNAP version of LXD.

To install LXD on Ubuntu follow the instructions on the linuxcontainers.org website:

[https://linuxcontainers.org/lxd/getting-started-cli/](https://linuxcontainers.org/lxd/getting-started-cli/)

Set the remote authentication password:

lxc config set core.trust_password

Change the default profile network interface:

**lxc profile edit default** # Change lxcbr0 to your value.

Create an image:

l**xd-images import lxc ubuntu xenial amd64 --alias xenial --alias ubuntu/xenial --alias ubuntu/xenial/amd64**

Create the container “cn1” from the image “ubuntu/cn1/amd64” you just made:

lxc launch ubuntu/xenial/amd64 cn1

You can Create an LXD container using a shortcut syntax. The following launches/creates an Ubuntu BIONIC (ie the “b”) container and calling it cn1.

lxc launch ubuntu:b cn1

Show the log if the above failed for some reason:

lxc info cn1 --show-log

Attach a shell on it:

lxc exec cn1 bash

Delete the container you made:

lxc delete cn1

On your own system if you are using the APT version of LXD install the LXD client:

sudo apt-add-repository -y ppa:ubuntu-lxc/stable

sudo apt-get update

sudo apt-get install lxd-client

Add a remote for the server you just configured (the following is a 1 liner):

**lxc remote add**

****Illegal HTML tag removed :** https://<your-server-fqdn-here>:8443 --accept-certificate</your-server-fqdn-here>**

# enter the password you&#039;ve set above here.

See if the remote works:

lxc list &lt;put-the-server-name-here&gt;

Create a image from a container (publish it)

You need to delete the image first if you already have one with one of that aliases:

lxc delete **Illegal HTML tag removed :** :ubuntu/cn1/amd64

Now publish your container (make it available as image):

lxc publish **Illegal HTML tag removed :** : <container><server>: --alias ubuntu/cn1 --alias ubuntu/cn1/amd64</server></container>

Delete the image container if needed:

lxc delete **Illegal HTML tag removed :** :cn1

Launch a new container from the image you created:

lxc launch **Illegal HTML tag removed :** :ubuntu/cn1 <server>:<your-new-container-name></your-new-container-name></server>

You can also do:

lxc init **Illegal HTML tag removed :** :ubuntu/cn1 <server>:<your-new-container-name></your-new-container-name></server>

lxc start **Illegal HTML tag removed :** :<your-new-container-name></your-new-container-name>

Start a shell in the new container:

lxc exec srv01:**Illegal HTML tag removed :** bash