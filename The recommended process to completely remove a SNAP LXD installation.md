###The recommended process to completely remove a SNAP LXD installation

Installing LXD using SNAP is becoming the recommended method over the traditional APT install process.

However, there may be times when you need to completely remove the SNAP LXD installation.

Stephane Graber (LXD project lead) had posted a response to someone who had posted that question and here is his
recommendation:

> To Delete SNAP LXD the right way - per Stephane Graber  
> 
> Execute the following commands:   
>  
> $ **lxc list**  
> $ **lxc delete \<whatever came from list\>**  
> $ **lxc image list**  
> $ **lxc image delete** \<whatever came from *lxc list* comamnd\>  
> $ **lxc network list**  
> $ **lxc network delete** \<whatever came from *lxc list* command\>  
> $ **echo ‘{“config”: {}}’ | lxc profile edit default**  
> $ **lxc storage volume list default**    
> $ **lxc storage volume delete default** \<whatever came from *lxc list* command\>  
> $ **lxc storage delete default**

Effectively listing and the deleting all the objects until everything’s clean.

*End of Stephane's recommendation for how to properly remove SNAP LXD installation*

Additional note:

Some LXD users have a great number of LXD containers and executing some of these command could become
quite time consuming unless you were to build a script to automate some of the steps.

I found a very useful script on Github which automates the first 4 using a python script:   

> **https://github.com/ilhaan/lxd-delete-containers**
