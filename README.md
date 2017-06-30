# Docker MPI tutorial

This repository provides a tutorial demonstrating how to create a cluster
of Docker containers on your computer and execute an MPI job on it.

## Requirements

* docker
* wget
* openssh-client (ssh-keygen executable is used)

## Installation

### Prepare the working copy

Execute the bootstrap script: `./bootstrap`


### Spawn the containers

Execute the script: `./cluster-setup N`

where N is the cluster size.
This scripts will start the containers and generate a `cluster-mpirun` script that can be
used to execute job on this cluster.

For instance:

```
bash $ ./cluster-setup 4
# Starting cluster of 4 containers:
$ docker run -P -d -h node01 --name docker-mpi-01 centos7:mpi /usr/sbin/sshd -D
248774f7536a7e42059196fe526f6c33e536114abce27b6063da6e6250db0a0f
$ sleep 1
$ docker run -P -d -h node02 --name docker-mpi-02 centos7:mpi /usr/sbin/sshd -D
7a743872efcabd2dc29939191966ad3c0cb16d855af6da6e3c857b7305455c20
$ sleep 1
$ docker run -P -d -h node03 --name docker-mpi-03 centos7:mpi /usr/sbin/sshd -D
a0eae9492540f17d58273f2191356b2d8735f8e603963b5b1866cf71f7ddfb63
$ sleep 1
$ docker run -P -d -h node04 --name docker-mpi-04 centos7:mpi /usr/sbin/sshd -D
70747e640327a6349dffca85b954ac473d620296f3920113d4444e88f6f77c41
$ sleep 1
# Generating script: cluster-mpirun
# Generating script: cluster-teardown
bash $ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                    NAMES
e6c71cd05052        centos7:mpi         "/usr/sbin/sshd -D"      About a minute ago   Up About a minute   0.0.0.0:32777->22/tcp    cont04
4c587d97146a        centos7:mpi         "/usr/sbin/sshd -D"      About a minute ago   Up About a minute   0.0.0.0:32776->22/tcp    cont03
3f86426eb440        centos7:mpi         "/usr/sbin/sshd -D"      About a minute ago   Up About a minute   0.0.0.0:32775->22/tcp    cont02
6efed4edd8b7        centos7:mpi         "/usr/sbin/sshd -D"      About a minute ago   Up About a minute   0.0.0.0:32774->22/tcp    cont01
```

### Start an MPI job

Let's run one of the samples provided by IBM Platform MPI Community Edition:

```
./cluster-mpirun /opt/ibm/platform_mpi/help/hello_world
```

### Terminate the cluster

Execute the generate `cluster-teardown` script to get rid of Docker containers.

## Links

* https://www.ibm.com/developerworks/community/blogs/W8932c25e7e86_409c_ab4c_b1deebf711ff/entry/developing_message_passing_application_mpi_with_docker?lang=en
* https://www.ibm.com/developerworks/community/blogs/W8932c25e7e86_409c_ab4c_b1deebf711ff/entry/ibm_platform_mpi_and_lsf_ssh_environment_setup1?lang=en


## License

MIT License, see [LICENSE](LICENSE) file for more information.
