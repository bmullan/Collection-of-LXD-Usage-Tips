### Using Remote LXD Hosts 

LXD supports the capability to manage (create/start/stop/freeze/unfreeze/delete etc) LXD containers on Remote LXD Host/Servers. 

#### Configuring LXD to be available over the Network/Internet

To accomplish this both the "**local**" LXD Host the "**remote**" LXD host(s) need to be configured for LXD to be accessed over 
the network.

_**If you have not done "lxd init" yet**_ 

When you do the "lxd init" step of installation one of the questions it asks you would be about whether you want that Host's
LXD available over the Network/Internet. 

When asked you would answer "yes", then accept "**default**" for the next 2 questions and then finally set 
a "**Trust password**" which you will use later on your "**local**" LXD host when you issue the "**lxd remote add**" 
command to add your Remote LXD Hosts so you can configure/manage them remotely:  

> Would you like LXD to be available over the network (yes/no) [**default=no**]? **yes**
> Address to bind LXD to (not including port) [**default=all**]: 
> Port to bind LXD to [**default=8443**]: 
> **Trust password** for new clients: 
> Again: 

_**If you HAD done "lxd init" already but did NOT specify "yes" for the above question about availabilty of LXD over the
Network**_ 

In order to configure LXD access over the Network/Internet after you'd already done a previous "lxd init" you would execute
the following commands on **that** LXD Host/Server:

NOTE: make sure to change the password from "_some-secret_string_" to whatever your "**Trust Password**" is!

> $ lxc config set core.https_address [::]
> $ lxc config set core.trust_password some-secret-string


#### Adding a Remote LXD Host for access by your Local LXD Host 

**NOTE: this assumes**
1. you have done the LXD Init on all hosts as described above  
2. you have opened Port 8443 on your firewall(s) (the default in "**lxd init**") or whichever Port you specified in "**lxd init**")

_Example_:  assume the remote LXD host IP address is 143.23.118.43

_On your Local Host_ you would execute:   

> **lxc remote add _<alias>_ 143.23.118.43**

where:  _<alias>_ is any "name" you want to use _to reference_ the Host at IP 143.23.118.43

The above command will result in you being prompted to accept a "key" for the Remote host and then you will also be prompted for 
the Trust Password you set for the Remote Host.

#### Copying an LXD container from LXD Host1 to LXD Host2

example assumptions:  
1. you have already added Host2 as a Remote LXD Host to Host1 using the **lxc remote add** command (see above)
2. there is already some container named **cn1** on the source host (Host1) 

If **cn1** is already in a "**stopped**" state then you can copy it to Host2:

> **lxc copy cn1 host2:cn1**

**NOTE**: 
In the above you can name the host2 container anything you want (re it doesn't have to be the same name as on the source).
The new container on Host2 will get a new IP address on Host2 but it will have all of the same information (including snapshots)
as the Host1 container CN1.

If **cn1** is **not stopped** but in a "**running**" state (or you don't want to stop it) then you have two choices
before you can copy it to Host2:  
1. stop cn1 first then copy it as above (**lxc stop cn1** then **lxc copy cn1 host2:cn1**)
2. don't stop it but take a snapshot of cn1 then copy the snapshot to Host2

**If #2 then the syntax is**:

> **$ lxc snapshot cn1 <snapshot-name>**   where: <snapshot-name> is whatever you want it to be

example: create the LXD snapshot of CN1 calling the snapshot "cn1-snap0":
> $ **lxc snapshot cn1 cn1-snap0**

To verify snapshots or see info about snapshots:
> $ **lxc info <container-name>**  where: <container-name> is any container you want to see names of snapshots that have been done

_example:_  
> $ **lxc info cn1**

would display information about CN1 and at the bottom of that information would be something like the following:

> :
> :
> Memory usage:
>   Memory (current): 46.48MB
>   Memory (peak): 86.09MB
> Network usage:
>   eth0:
>     Bytes received: 10.30kB
>     Bytes sent: 7.42kB
>     Packets received: 75
>     Packets sent: 71
>   lo:
>     Bytes received: 856B
>     Bytes sent: 856B
>     Packets received: 11
>     Packets sent: 11
> **Snapshots**:
>  **cn1-snap0** (taken at 2019/07/05 00:21 UTC) (stateless)


**If you have a Container CN1 that has a snapshot named "cn1-snap0" you can copy the **snapshot** to Host2 to create
the new container**

example:
> **lxc copy cn1/cn1-snap0 host2:cn1**

The above would copy snapshot cn1-snap0 to host2 and create a new container named cn1 _**without having to stop Host1's CN1 
container**_.

**Note:** 
The above example would create a copy of Host1's CN1 container but it would **not** copy  any of Host1's CN1 snapshots
to Host2.

