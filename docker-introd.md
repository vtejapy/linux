# What's Docker?

- A *platform* made of the *Docker Engine* and the *Docker Hub*
- The *Docker Engine* is a runtime for containers
- It's Open Source, and written in Go 
- It's a daemon, controlled by a REST API
- Although Docker has a huge echo system around its ideas. It container management tool that provides virtualization,management and orchestration. But there are five differences that makes docker a game changer:
- Docker adds a powerful stack of ideas that completed the vision of containers.
- Docker is an opensource project Docker Repository
-  Docker is supported by almot all the big players in the cloud computing market e.g.(Amazon, Azure, RackSpace , etc.)

## Docker is not

   - Docker is not an Automation or configuration management tool (Puppet, Chef, and JUJU)
   - Docker is not a Hardware Virtualization Solution (VMware, KVM, and XEN)
   - Docker is not a Cloud Platform (Open Stack, and Cloud Stack)
   - Docker is not a Deployment Framework (Capistrano, Fabric, etc.)
   - Docker is not a Workload Management Tool (Mesos, Fleet, etc.)
   - Docker is not Development Environment (Vagrant, etc.)


## Techniques Behind Docker

### The Techniques behind docker includes:

   - Control Groups
   - Kernel Name Spaces
   - Union File system
   - Capabilities
   - Copy on Write

### Control Groups

It’s a method to put processes into groups by allowing the Following:

  - **Resource limitation** : 
    groups can be set to not exceed a configured memory limit, which also includes the file system cache.

   - **Prioritization** : 
    some groups may get a larger share of CPU utilization or disk I/O throughput.

 - **Accounting**: 
  measures how much resources certain systems use, which may be used, for example, for billing purposes.

- **Control**: 
    freezing the groups of processes, their checkpointing and restarting

Each cgroup is represented by a directory in the cgroup file system containing the following files describing that cgroup:

- **Tasks**: 
   list of tasks (by PID) attached to that cgroup.

 -  **Cgroup.procs**: 
 list of thread group IDs in the cgroup.

- **Notify_on_release flag**: 
run the release agent on exit?
Release_agent: the path to use for release notifications (this file exists in the top cgroup only).




### Kernel Namespaces

A kernel namespace wraps a global system resource in an abstraction that makes it appear to the processes within the namespace that they have their own isolated instance of the global resource.

Changes to the global resource are visible to other processes that are members of the namespace, but are invisible to other processes.
### There are 6 Name Spaces:

- **PID namespace**  provides isolation for the allocation of process identifiers (PIDs), lists of processes and their details. While the new namespace is isolated from other siblings, processes in its "parent" namespace still see all processes in child namespaces—albeit with different PID numbers.
-  **Network namespace** isolates the network interface controllers (physical or virtual), iptables firewall rules, routing tables etc. Network namespaces can be connected with each other using the "veth" virtual Ethernet device.
- **"UTS" namespace**  allows changing the hostname.
- **Mount namespace** allows creating a different file system layout, or making certain mount points read-only.
- **IPC namespace** isolates the System V inter-process communication between namespaces.
- **User namespace** isolates the user IDs between namespaces.

Namespaces are created with the "unshare" command or syscall, or as new flags in a "clone" syscall.

### union File System

- Union file system represents file system by grouping directories and files in branches. A Docker image is made up of filesystems layered over each other (i.e. Branches that is stacked on top of each other) and grouped togeather. At the base is a boot filesystem, bootfs, which resembles the typical Linux/Unix boot filesystem. A Docker user will probably never interact with the boot filesystem.

- Indeed, when a container has booted, it is moved into memory, and the boot filesystem is unmounted to free up the RAM used by the initrd disk image. So far this looks pretty much like a typical Linux virtualization stack. Indeed, Docker next layers a root filesystem, rootfs, on top of the boot filesystem. This rootfs can be one or more operating systems (e.g., a Debian or Ubuntu filesystem).

- Docker calls each of these filesystems images. Images can be layered on top of one another. The image below is called the parent image and you can traverse each layer until you reach the bottom of the image stack where the final image is called the base image. Finally, when a container is launched from an image, Docker mounts a read-write filesystem on top of any layers below. This is where whatever processes we want our Docker container to run will execute. When Docker first starts a container, the initial read-write layer is empty. As changes occur, they are applied to this layer; for example, if you want to change a file, then that file will be copied from the read-only layer below into the read-write layer. The read-only version of the file will still exist but is now hidden underneath the copy.



## capabilities
### Process Capabilities

Each Linux process has four sets of bitmaps. By Default each bitmap is 32 bit for 32 different capability.

 - Effective (E):

    The effective capability set indicates what capabilities are effective The Process can use now

  - Permitted (P):
    The process can have capabilities set in the permitted set that are not in the effective set. This indicates that the process has temporarily disabled this capability. A process is allowed to set a bit in its effective set only if it is available in the permitted set. The distinction between effective and permitted makes it possible for a process to disable, enable and drop privileges

 - Inheritable (I):
    Indicates what capabilities of the current process should be inherited by the program executed by the current process

- Bset:
    Determine forbidden capabilities

### File Capabilities
Allows set, the forced set, and the effective set

## Copy On Write

The Middleware is responsible for creating and managing the container. When a container is started it appears to have its own Linux files system that you're using. The fact is you don't; at the start of the container it links to files in the base kernel that all containers shared, and the way it handle the changes is via the copy on write model, where each change to the file system is copied to an in memory version of the base file with the changes.
This results two main effects:

 - The container will result a small footprint.
 - changes in memory are faster than writing to storage.

This works in conjunction with UNION File system. Where, each image layer is read-only; this image never changes. When a container is created, Docker builds from the stack of images and then adds the read-write layer on top. That layer, combined with the knowledge of the image layers below it and some configuration data, form the container.




## docker vs Virtual Machines
|         | Container           | Hypervisor           | 
| ------------- |:-------------:|:-------------:| 
| Redundancy      | Only Application Code and Data Edited Files | Computation and Memory redundancy
| Flexibility      | Less Flexible,Only Supported in Linux, Cannot host Windows OS     | More Flexible where Linux OS can host windows and vice verse
| Security | Less Secure, DOS attacks can affect others Containers Container with root privileges could affect other Containers  |   Full Isolation, Hypervisor is responsible of limiting used resource by VM Cross Gust Access is prohibited
| Creating Time | Almost instantaneous     |   Even Preexisting VMs need OS Load Time
| Performance | Almost Native     |   Less than native due to middle ware
| Consolidation | Limited by actual Application Usage     |   Limited By OS reserved
| Memory | Allocation Memory     |   Allocated Files Disk Allocated Files



