FROM centos:7

# Install requirements
RUN yum update -y \
 && yum upgrade -y \
 && yum install -y \
	less wget tar openssh-clients openssh-server \
	gcc gdb glibc.i686 libgcc.i686 \
	make gcc-c++ gcc-gfortran \
 && ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t rsa \
 && mkdir -p -m 700 /root/.ssh \
 && echo "LogLevel DEBUG" >> /etc/ssh/sshd_config

# Install IBM Platform MPI Community Edition:
ADD files/platform_mpi-09.01.04.03r-ce.bin /
RUN /platform_mpi-09.01.04.03r-ce.bin -i silent || true

ENV MPI_ROOT=/opt/ibm/platform_mpi
WORKDIR "$MPI_ROOT"

# Compile samples
RUN make -C $MPI_ROOT/help

# Install SSH key
ADD files/ssh_config /root/.ssh/config
ADD files/id_rsa /root/.ssh/id_rsa
ADD files/id_rsa.pub /root/.ssh/authorized_keys
ADD files/mpirun.entrypoint /usr/bin/
RUN chmod -R go-rwx /root/.ssh

EXPOSE 22
