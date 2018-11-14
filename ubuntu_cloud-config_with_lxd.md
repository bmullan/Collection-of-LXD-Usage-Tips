### Ubuntu cloud-config with LXD {#ubuntu-cloud-config-with-lxd}

By default, LXD already uses ubuntu-cloudimg images. These are the same images used on Amazon AWS or Digital Ocean Public Clouds.

If you’ve not used Ubuntu on a Public Cloud before you may not know about the feature/capability called “_cloud-init_” or “_cloud-config_”. This capability allows you to preconfigure specific OS features/packages/etc when the Cloud instance is first started.

The information you pre-configure is termed “**user-data**”.

What you may not know is that with LXD that same capability exists for the LXC containers you create.

It turns out it is very easy to pass “**_user-data_**” to an LXD instance when you start it, just like you would on any cloud provider.

LXD even has the **_-e option to make your LXD instance ephemeral_**. By “ephemeral” is meant that the LXC container will be deleted automatically when you “stop” it.

To create/install “user-data” create a file named .yaml. The name can be anything.

Then start the LXC container:

> **$ lxc launch ubuntu:16.04 cn2 -c user.user-data=&quot;$(cat .yaml)&quot;**

That is all there is to it.

Here is an example of a configuration:

#cloud-config

output:

all: &quot;|tee -a /tmp/cloud.out&quot;

#hostname: {{ hostname }}
 
bootcmd:

- rm -f /etc/dpkg/dpkg.cfg.d/multiarch

apt_sources:

- source: ppa:yellow/ppa

ssh_import_id: [evarlast] # use -S option

packages:

- make

final_message: &quot;The system is finally up, after $UPTIME seconds&quot;

runcmd:

- cd /home/ubuntu

- git clone https://www.github.com/jrwren/myproject

- cd myproject

- make deps run
