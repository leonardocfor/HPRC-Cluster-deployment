# HPRC simple Cluster deployment

Deployment of simple simulated High Performance Robotic Computing (HPRC) cluster step by step guide. For more information about HPRC please visit [here](https://www.sciencedirect.com/science/article/pii/S092188901830232X). This guide is to be followed in computers using Ubuntu. 

### General configuration
-----

Software installation (as root or with sudo) 

```
apt-get install openssh-server nfs-common 
apt-get install python-mpi4py openmpi-common openmpi-bin openmpi-doc libopenmpi-dev 
```

Add hosts to /etc/hosts , one line per host (as root or with sudo)

```
<IP>  <FQDN>  <Hostname>
```
Create home directory for HPRC cluster's users (as root or with sudo)

```
mkdir -p /export/home
```


Create user (as root or with sudo)

```
useradd -u <user_ID> -m -d /export/home/<username> -s /bin/bash <username>
passwd <username>
su <username>
```


Only in the master node: Generate SSH keys for <username> and configure passwordless authentication

```
ssh-keygen -t rsa
cd ~/.ssh
cat id_rsa.pub >> authorized_keys
chmod 600 authorized_keys
```

### File system 
-----
File system installation: In a HPRC cluster, the file system can be any technology traditionally used in Supercomputing. For a simple cluster, Network File System (NFS) will suffy. For this example, the folder /export/home will be shared from the master node. Such folder corresponds to the user space for this configuration


#### Master node

```
apt-get install nfs-kernel-server  
```

Edit the file _/etc/exports_ and add the following lines:

```
/export/home <Network/Mask>(rw,no_root_squash) 
```

Execute the following commands

```
service rpcbind restart
/etc/init.d/nfs-kernel-server restart
exportfs
```

#### Slave nodes

Edit the file _/etc/fstab_ and add the following lines:

```
<SERVER_IP>:/export/home /export/home    nfs    defaults,proto=tcp,port=2049    0 0 
```

Mount NFS shared folder

```
mount <SERVER_IP>:/export/home /export/home 
```


### ArduPilot SITL and APM Planner 2 
-----
For ArduPilot SITL please follow the instructions in [here](http://ardupilot.org/dev/docs/sitl-simulator-software-in-the-loop.html)

For APM Planner 2, please follow the instructions in [here](http://ardupilot.org/planner2/docs/installation-for-linux.html)


### Traditional supercomputing (HPC) examples
---

[Hello world with mpi4py](https://github.com/erdc/mpi4py/blob/master/demo/helloworld.py)

[PI computation with mpi4py](https://gist.github.com/jcchurch/930276)

To execute these examples use the following command:

```
mpirun -np <X> --hostfile <hostfile> python <pythonSoftware>

where,

X = Quantity of MPI parallel processes
<pythonSoftware> The corresponding software
<hostfile> is a text file with the hostnames of the computers in your cluster, e.g. 
  <Hostname1>
  <Hostname2>
  <Hostname3>
```

### HPRC example
---

For an example see swarming motion based on MPI and DroneKit in [here](https://github.com/leonardocfor/multi-robot-vicsek)

