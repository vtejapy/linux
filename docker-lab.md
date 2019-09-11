## Installation Prerequisites

Docker must be installed on 64 bit OS with kernel more than 3.10. The kernel must support an appropriate storage driver.

For example:

 - Device Mapper
 - AUFS
 -  vfs
 - btrfs

The default storage driver is usually Device Mapper for centos and redhat and AUFS for ubuntu.


### Install On CentOS

The used Version in the installation is CentOS 7 Core 64 Bit, this should be applicable to both native and virtualized environment. For More information visit docker docs for CentOS
Installation with Script

Install latest updates to help you get the latest version of Docker
```
$ sudo yum update
```
Install Docker via Script
```
$ curl -sSL https://get.docker.com/ | sh
```
Run the Docker Engine
```
$ sudo service docker start
```
Test Docker Version
```
$ docker version
```
Incase of error use this command:
```
yum install docker-selinux
```
To start docker
```
sudo systemctl start docker
```
Then Test Docker Version
Installation from Source Code Repository (Without the Script)

Install latest updates to help you get the latest version of Docker
```
$ sudo yum update
```
Add the Repo to your package manager
```
$ cat >/etc/yum.repos.d/docker.repo <<-EOF [dockerrepo] name=Docker Repository baseurl=https://yum.dockerproject.org/repo/main/centos/7 enabled=1 gpgcheck=1 gpgkey=https://yum.dockerproject.org/gpg EOF
```
Install docker Package
```
$ sudo yum install docker-engine
```
Run the docker engine
```
$ sudo service docker start
```
Incase of error use this command:
```
yum install docker-selinux
```
And To start docker
```
sudo systemctl start docker
```
Test Docker Version
```
$ docker version
```


### Installation On Ubuntu

1 - Add the new gpg key.
```
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```
2 - add the repo location
```
$ sudo vi /etc/apt/sources.list.d/docker.list
```
3 - based on you version add 

deb https://apt.dockerproject.org/repo ubuntu-trusty main

4 - Update Package manager 
```
$ apt-get update
```
5 - install docker
```
sudo apt-get install docker-engine
```
6 - To test the installation
```
sudo service docker start
sudo docker version
```



### Run Command

Creating containers is the sole purpose of all these infrastructure.

Using this command will initiate the following work-flow:

 - It will check for the container locally
 -   Fetch the container from the Docker Hub Repository
 -   Run the container

Usage
```
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```
Although the Image Builder has a default value for each of the following one can override them.

**[Options]**

The run options control the image’s runtime behavior in a container. These settings affect:

- detached or foreground running
-  container identification and name
-  network settings and host name
-  runtime constraints on CPU and memory
-  privileges and LXC configuration

**IMAGE[:TAG|@DIGEST]**

Using an image based on a specific version based on tag or custom made identifier (Digest)

**[COMMAND]**

Specifies commands to run at the start of the container

**[ARG…]**

Other arguments which include adding users and setting environment variable and mounting devices.



### Container Control Commands

The docker have some commands that allows the some control over a container.

- Create
- Start
- Stop
- Restart
- Execute
- Copy
- Attache
- Inspect
- Delete


### Docker Create

The docker create command creates container that allows setting configuration you can set in run command the only difference that the created container is never started and in order to start a container run the start command.

For more the docker create check out the official documentation.
Usage
```
$ sudo docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
Example

$ docker create -t -i fedora bash

6d8af538ec541dd581ebc2a24153a28329acb5268abe5ef868c1f1a261221752

$ docker start -a -i 6d8af538ec5

bash-4.2#
```
### Docker Start, Stop and Restart

These three commands are used to change the state of a container from running to stopped or restart it.

Stop: When a user issues this command, the Docker engine sends SIGTERM (-15) to the main process, which is running inside the container.

The SIGTERM signal requests the process to terminate itself gracefully. Most of the processes would handle this signal and facilitate a graceful exit. However, if this process fails to do so, then the Docker engine will wait for a grace period. Even after the grace period, if the process has not been terminated, then the Docker engine will forcefully terminate the process. The forceful termination is achieved by sending SIGKILL (-9).

The SIGKILL signal cannot be caught or ignored, and so it will result in an abrupt termination of the process without a proper clean-up.
Usage
```
$ docker stop | start| restart [OPTIONS] CONTAINER [CONTAINER...]
```
**[Options]**
-t, --time=10      Seconds to wait for stop before killing it

**[Container]**    
Control the container either by Name or ID

Example
```
$ sudo docker start ubuntu

```

### Docker Execute

Run a command in a running container
Usage:
```
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

-d, --detach=false         Detached mode: run command in the background
-i, --interactive=false    Keep STDIN open even if not attached
-t, --tty=false            Allocate a pseudo-TTY
-u, --user=                Username or UID (format: <name|uid>[:<group|gid>])
```
Example:
```
$ docker exec -d ubuntu_bash touch /tmp/execWorks
```
This will create a new file /tmp/execWorks inside the running container ubuntu_bash, in the background.
```
$ docker exec -it ubuntu_bash bash
```
This will create a new Bash session in the container ubuntu_bash.



### Docker Copy

Copy data [From|To] container, It copies data from and to running containers.


Usage
```
$ docker cp [OPTIONS] CONTAINER:PATH LOCALPATH|-

$ docker cp [OPTIONS] LOCALPATH|- CONTAINER:PATH
```
Example

Copy Data from Container to host
```
$ sudo docker cp testcopy:/root/file.txt .
```
Copy Data from host to container
```
$ sudo docker cp host.txt testcopy:/root/host.txt
```


### Docker Attach

This commands brings a container to the foreground. The container must be running to be attached.
Usage
```
$ docker attach [OPTIONS] CONTAINER
```
Example
```
$ sudo docker attach Your_Ubuntu_Container
```



### Docker Inspect

This command is used to check for the current configuration of a container from network to drivers and name. It prints the information in the form of JSON File
Usage
```
$ docker inspect [OPTIONS] CONTAINER|IMAGE [CONTAINER|IMAGE...]
```
Example
```
$ sudo docker inspect Your_Ubuntu_Container
```

### Docker delete

This command is used to delete any container either by name or ID. Put Make sure that the container is stopped before you remove it.
Usage
```
$ docker rm [OPTIONS] CONTAINER [CONTAINER...]
```
Example
```
$ sudo docker rm Your_Ubuntu_Container
```

### Docker Login

Register or log in to a docker registry.
Usage
```
docker login [OPTIONS] [SERVER]
```
**[options]**

-e, --email="" Email

-p, --password="" Password

-u, --username="" Username

**[server]**

Docker registry server, if no server is specified ("https://index.docker.io/v1/) is the default.
Example
```
$ sudo docker login
Username: XXXXX
Password:
Email: XXX@XXX.com


**Warning**: login credentials saved in /root/.docker/config.json

Login Succeeded
```



### Docker Info

This command returns a list of any containers, any images (the building blocks Docker uses to build containers), the execution and storage drivers Docker is using, and its basic configuration.
Usage:
```
$ Docker info
```
Example
```
$ sudo docker info

Containers: 0 
Images: 0
Storage Driver: devicemapper
Pool Name: docker-253:0-3022913-pool
Pool Blocksize: 65.54 kB
Backing Filesystem: xfs
Data file: /dev/loop0
Metadata file: /dev/loop1
Data Space Used: 1.821 GB
Data Space Total: 107.4 GB
Data Space Available: 12.85 GB
Metadata Space Used: 1.479 MB
Metadata Space Total: 2.147 GB
Metadata Space Available: 2.146 GB
Udev Sync Supported: true
Deferred Removal Enabled: false
Data loop file: /var/lib/docker/devicemapper/devicemapper/data
Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
Library Version: 1.02.93-RHEL7 (2015-01-28)
Execution Driver: native-0.2
Logging Driver: json-file
Kernel Version: 3.10.0-229.el7.x86_64
Operating System: CentOS Linux 7 (Core)
CPUs: 4
Total Memory: 1.948 GiB
Name: localhost.localdomain
ID: Q3D4:BORV:ZTAW:QZ2P:I7DI:4SI3:62AT:5UUA:73NN:CVFC:FDKT:PJR4
```


### Docker Diff

List the changed files and directories in a container’s filesystem from the base image.
Usage
```
$ sudo docker diff container
```
Example

Assume There are 3 events that are listed in the diff:

- A - Add
- D - Delete
- C - Change
```
$ docker diff determined_bose

    C /root
    A /root/xyz
    A /root/.bash_history
    A /root/abc
    A /root/lmn
```
### Docker PS

Shows the current running containers on the system
Usage
```
$ docker ps [OPTIONS]
```
**[OPTIONS]**

-a all containers

-filter [] filtering container based on any parameter
Example
```
$ sudo docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                          PORTS               NAMES

2e4bea848301        hello-world         "/hello"            About a minute ago   Exited (0) Ab

```


## Docker Images Commands
### Docker Save and Load Images

Docker has two commands the save and load commands that allows you to have a portable version of a docker image.

For more check the save and load pages in the documentation.
Usage
```
$ docker save [OPTIONS] IMAGE [IMAGE...]
```
**[options]**
-o, --output=""    Write to a file, instead of STDOUT
$ docker load [OPTIONS]
**[options]**
-i, --input=""     Read from a tar archive file, instead of STDIN. The tarball may be compressed with gzip, bzip, or xz

Example
```
$ docker save -o ubuntu.tar ubuntu:lucid ubuntu:saucy
$ docker load --input fedora.tar
```
### Docker Images

Shows the currently available images. Images are saved in /var/lib/docker/containers directory.

For More Information visit the Images command page.
Usage
```
$ Docker images [OPTIONS] [REPOSITORY]
```
Example
```
$ sudo docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
hello-world         latest              af340544ed62        8 weeks ago         960 B
```
### Docker Pull

Docker pull commands gets an images from the Docker Registry either local or the global one.

This command is called if the run commands can’t find the stated image.

For More information visit the pull command page
usage
```
$docker pull [OPTIONS] NAME[:TAG] | [REGISTRY_HOST[:REGISTRY_PORT]/]NAME[:TAG]
```
**[options]**

-a, --all-tags=false          Download all tagged images in the repository
--disable-content-trust=true Skip image verification

Example
```
$ docker pull debian
```
will pull the debian:latest image and its intermediate layers

### Docker Search

Docker search commands search on the docker images repositories to find images For More information visit the search command page
Usage
```
$docker search [OPTIONS] TERM
```
**[options]**

--automated=false    Only show automated builds
--no-trunc=false     Don't truncate output
-s, --stars=0        Only displays with at least x stars



## Building Docker Images

Creating a custom docker image is not an easy task. There is two ways to create such image.
1. Docker Commit
This the old way of creating an image.
2. Docker File

The new and recommend way

### Docker Commit

This command will enable you to update the image you’re using by installing the required tools and packages and save it to be used by other users. The commit command enable you to save a container to an image to be reusable.
Usage:
```
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
[options]
  -a, --author=""     Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -c, --change=[]     Apply specified Dockerfile instructions while committing the image
  -m, --message=""    Commit message
  -p, --pause=true    Pause container during commit
```
Demo:

The Demo will start by using an Ubuntu image and install Nginx webserver on it and save it locally and on my local docker hub registry.

Step 1: Run the container and open it from base image
```
$ sudo docker run -i -t ubuntu /bin/bash
```
Step 2: Install required packages and tools
```
/# apt-get install nginx
```
Step 3: Exit the container
```
/# exit
```
Step 4: Find the edited container ID
```
$ sudo docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                        PORTS               NAMES
238c9baddb08        Ubuntu              "/bin/bash"         3 days ago          Up 59 minutes                                     walid
```
Step 5: Commit the image
```
$ sudo docker commit -m="A new nginx image" --author="washraf" 238c9baddb08 washraf/nginx

91064f90fa2c120fb7b10ec7354ab0ed5cdb36b942b2b6df65975f5cdc2403cc
```
Step 6: Check the image 
```
$sudo docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
walid/nginx         latest              91064f90fa2c        32 seconds ago      206.5 MB
ubuntu              latest              91e54dfb1179        6 weeks ago         188.3 MB
hello-world         latest              af340544ed62        8 weeks ago         960 B 
```
Step 7: inspect the image
```
$sudo docker inspect walid/nginx
```
 The data will appear as configured

Step 8: Run the container from the new Image
```
$ sudo docker run walid/nginx $ sudo docker run -i -t walid/nginx /bin/bash
```


### Docker Build

This is the recommended way to create a docker image.
Demo

Step 1: Create directory
```
$ mkdir NginxDir
```
Step 2: Create file
```
$ vi Dockerfile
```
Step 3: Write this commands in the file
Step4: Build Image
```
$ sudo docker build -t="walid/ngtest" .
```
Successfully built d69edbcc0346

Step 5: Check the Image
```
$ sudo docker images "walid/ngtest" .

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
washraf/ngtest      latest              d69edbcc0346        2 minutes ago       206.5 MB
```
Step 6: Run the container will the webserver
```
sudo docker run -d -p 80 washraf/ngtest nginx -g "daemon off;"
```
Step 7: check the assigned port from the docker deamon
```
$ sudo docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
6d575aeb39f1        washraf/ngtest      "nginx -g 'daemon off"   2 minutes ago       Up 2 minutes        0.0.0.0:32769->80/tcp   drunk_babbage
```


### Docker File Instructions

Creating a docker file is the most used way to create a docker image. In this section we are going to explore the ways to create a docker image file.

For More information visit the docker builder.
**From**:

This instructions sets the base image for the new Image. This is considered the most important instruction in the Dockerfile. This instruction must be the first one to be written and it treats the image as the docker run do. If the images are found locally it’s used and if not it will be pulled from the assigned registry and finally if the image is not found it will raise a build error.
Usage
```
FROM <image>

FROM <image>:<tag>

FROM <image>@<digest>
```
**Maintainer**:

Who is the Image creator
Usage
```
MAINTAINER <Name>
```
**WORKDIR**:

Sets the start directory for the container where the run and the CMD instructions are executed.
Usage
```
WORKDIR /path/to/workdir
```
**RUN**:

The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.
Usage
```
RUN <command> (the command is run in a shell - /bin/sh -c - shell form)

RUN ["executable", "param1", "param2"] (exec form)
```
**CMD**:

The CMD Command is used as a default entry point to container. And if more than one is added the last one only will take effect.
Usage
```
CMD ["executable","param1","param2"] (exec form, this is the preferred form)

CMD ["param1","param2"] (as default parameters to ENTRYPOINT)

CMD command param1 param2 (shell form)
```
**COPY**:

Copies files from the host file system to the image file system.
Usage
```
COPY <src>... <dest>

COPY ["<src>",... "<dest>"] (this form is required for paths containing whitespace)
```
**ADD**:

The add file is similar to the copy but can handle both tar files and remote URLs
Usage
```
ADD <src>... <dest>

ADD ["<src>",... "<dest>"] (this form is required for paths containing whitespace)
```
**ENV**:

Sets some environment variables for the container
Usage
```
ENV <key> <value> only one perline

ENV <key>=<value> ... allows multiple per line
```
**USER**:

Sets the user that will be running in the container and the default is the root.
Usage
```
USER <UID>|<UName>
```
**EXPOSE**:

This allows the container ports to be accessed from the global ports. Note that Port mapping has to be specified directly at creation time using the –p or –P commands
Usage
```
EXPOSE <port> [<port>...]
```
**ENTRYPOINT**:

The ENTRYPOINT instruction will help in crafting an image for running an application (entry point) during the complete life cycle of the container, which would have been spun out of the image. When the entry point application is terminated, the container would also be terminated along with the application and vice versa. The ENTRYPOINT instruction cannot be overridden in the run command.
Usage
```
ENTRYPOINT ["executable", "param1", "param2"] (the preferred exec form)

ENTRYPOINT command param1 param2 (shell form)
```
**Vloume**:

The VOLUME instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.
Usage
```
VOLUME ["/data"]
```
It is generally considered a bad idea to run commands like apt-get -y update or yum -y update in your application Dockerfiles because it can significantly increase the time it takes for all of your builds to finish. Instead, consider basing your application image on another image that already has these updates applied to it.
