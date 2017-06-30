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
$ ./cluster-setup 4
Starting 4 containers:
6efed4edd8b7030385e76293a37ef757338506581fb1ad5590545923809f0c6e
3f86426eb440851746616339daaeb352f905300d68372978cc8b697054611d58
4c587d97146af54ac7f44280e1c4f114ef9f656c732a4974fb071fbd8725833a
e6c71cd05052800f3867a4946fe09d4f729a717b76148676c992130d6eb70577
$ docker ps
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
