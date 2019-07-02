# HPRC Cluster deployment

Deployment of simple simulated High Performance Robotic Computing (HPRC) cluster step by step guide. For more information about HPRC please visit [here](https://www.sciencedirect.com/science/article/pii/S092188901830232X). This guide is to be followed in computers using Ubuntu. 

### General software installation
-------------------------------
```
apt-get install openssh-server nfs-common python-mpi4py openmpi-common openmpi-bin openmpi-doc libopenmpi-dev 
pip install dronekit geographiclib
```
### ArduPilot SITL

Please follow the instructions in [here](http://ardupilot.org/dev/docs/sitl-simulator-software-in-the-loop.html)


### File system 

File system installation: In a HPRC cluster, the file system can be any technology traditionally used in Supercomputing. For a simple cluster, Network File System (NFS) will suffy. 


#### Master node

```
apt-get install nfs-kernel-server  
```

Edit the file _/etc/exports_ and add the following lines:

<folder_to_export> <Network/Mask>(rw,no_root_squash) 
