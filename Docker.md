# Docker

* Still a draft

The reasons that Docker was innovated so fast are:
- Make the VM lighter
- Make the app inside it more reach


Namespace - an isolated space for processes.
When you create a namespace, all the processes are not familiar with the other processes inside other namespaces.
This is the basis of how containers are working, in comparison to Virtualization that takes an entire OS.

On Linux there are 7 namespaces:
- UTS
- network
- time
- …

OS has 2 basic parts:
	- Kernel
	- File System

chroot / pivot_root - a command for changing the root filesystem.

cgroup -

Docker is a wrap of Linux Kernel capabilities:
- namespace
- chroot
- cgroup

process that live on namespace, which limited on cgroup, and familliar only with the files inside the chroot tree.

Docker is a “small VM for spesific deployment”
Docker is an “isolated inside a big VM”
