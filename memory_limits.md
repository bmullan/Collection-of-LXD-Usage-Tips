### Memory Limits {#memory-limits}

LXD can also limit memory usage in various ways that are pretty simple to use.

Limiting memory to use particular size of RAM. For example, limiting containers to use only 512MB of RAM.

$ lxc config set container1 limits.memory 512MB

Limiting memory to use particular percent of RAM. For example, limiting containers to use only 40% of total RAM.

$ lxc config set container1 limits.memory 40%