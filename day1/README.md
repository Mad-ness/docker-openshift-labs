# Lab 1. Configuring VM

There is a list of virtual machines provisioned for every participant.

- openshift-10.*
- openshift-11.*
- openshift-12.*
- openshift-13.*
- openshift-14.*
- openshift-15.*
- openshift-16.*
- openshift-17.*
- openshift-18.*
- openshift-19.*
- openshift-20.*

These VMs has:
- installed OS Red Hat 7.7
- 1 vCPU, 1.75 Gib RAM, 30 GB of disk space
- account __openshiftX__ with password _CSopenshift123_
- account __csadmin__ with password _CSadmin_p123_

Both accounts __openshiftX__ and __csadmin__ enabled to use sudo without restrictions. We will use user __openshiftX__ unless said other.


# Lab 1: Installing and configuring the Docker environment

## Installing Packages

Pay attention on which commands require raising permissions with __sudo__.

1. Check if the docker package is installed or not

    yum info docker

Check the output

```
Name        : docker
Arch        : x86_64
Epoch       : 2
Version     : 1.13.1
Release     : 104.git4ef4b30.el7
Size        : 65 M
Repo        : installed
From repo   : rhui-rhel-7-server-rhui-extras-rpms
Summary     : Automates deployment of containerized applications
URL         : https://github.com/docker/docker
License     : ASL 2.0
Description : Docker is an open-source engine that automates the deployment of any
            : application as a lightweight, portable, self-sufficient container that will
            : run virtually anywhere.
            :
            : Docker containers can encapsulate any payload, and will run consistently on
            : and between virtually any server. The same container that a developer builds
            : and tests on a laptop will run at scale, in production*, on VMs, bare-metal
            : servers, OpenStack clusters, public instances, or combinations of the above.
```

if the line __Repo : installed__ is there that means that the package is installed.

If it is not, we need to do that.

2. Install Docker service. Execute following:

    sudo yum install docker     # and answer 'Y' when prompted

3. Start the Docker service (daemon):

    sudo systemctl start docker

4. Check that the service is running:

    systemctl is-active docker

or, more detailed output

    systemctl status docker

5. Check that the service will be started automatically after rebooting:

    systemctl is-enabled docker

f it is not, enable it:

    sudo systemctl enable docker

## Check the Docker Command


By this time the docker service is ready to run containers. Try it out:

    sudo docker ps

you should get this output:

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

But we don't want use sudo every we execute the docker command. Check it:

```
$ docker ps

Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.26/containers/json: dial unix /var/run/docker.sock: connect: permission denied
```

Check who is allowed to read and write the mentioned socket

```
$ ls -l /var/run/docker.sock
srw-rw----. 1 root dockerroot 0 Oct 22 11:07 /var/run/docker.sock
```
It shows the root user and members of dockerroot group can access to it. Let's fix it.

## Giving Normal Users Permissions to Access Docker Service





