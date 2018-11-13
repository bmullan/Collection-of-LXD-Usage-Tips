### Cloning 

Cloning or copying a container is a lot faster process than creating container from scratch.  This is especially important if you have already added applications, configurations, or users to an existing container but then decide you want everything in the first container for a *new use-case* which may require additional users or applications.

Example use-case:  you created an initial container for a business with specific generic applications & specific users.  Now you'd like a container specific to the Accounting Department which contains added applications pertinent to Accounting.  Or perhaps new for Human Resources or Sales etc.   Clone/copy the original and name it for the target organization.   This will save you a great deal of time.

Cloning a container resets the MAC address for the cloned container and does not copy the snapshots of parent container.

example: clone/copy cn1 to cn2

> **$ lxc copy cn1 cn2**
