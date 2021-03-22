# RKE Kubernetes Cluster
The files in this repository provisions an RKE Kubernetes cluster with 2 nodes and Calico CNI configured.


### Pre-requisites
Make sure the following tools are installed:
- Vagrant - [installation docs](https://www.vagrantup.com/downloads)
- VirtualBox - [installation docs](https://www.virtualbox.org/wiki/Downloads). Ensure you have VirtualBox `6.1.16` or higher installed.
- RKE binary - [installation docs](https://rancher.com/docs/rke/latest/en/installation/)
- kubectl binary - [installation docs](https://kubernetes.io/docs/tasks/tools/#kubectl)

### Provision RKE cluster
To create an RKE cluster, follow the steps below:
#### 1. Create Vagrant boxes
```bash
vagrant up
```
This command create 2 Vagrant boxes using the Vagrantfile, with the following specifications:
- CPU - 2
- Memory - 2G
- OS - OpenSUSE Leap
- IPs - **192.168.50.101** and **192.168.50.102**
- Docker installed
- Root access enabled with password `vagrant` (required for RKE setup)

#### 2. Root SSH Access
**_Optional: Create an SSH key-pair_**

Verify if `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub` files exist. If these are not available create a new SSH key-pair using the following command and press `ENTER` for each prompt:
```bash
ssh-keygen -t rsa -b 2048
```

**Copy SSH Keys**

Copy the SSH key for each Vagrant box. This allows root access to the boxes without typing the password every time.

_Note:_ The password for root access is `vagrant`
```bash
ssh-copy-id root@192.168.50.101
ssh-copy-id root@192.168.50.102
```

#### 3. Create an RKE Cluster
Once the Vagrant boxes are up and running and the root SSH access is configured, we are ready to use RKE to bootstrap the Kubernetes cluster.

The `cluster.yml` file contains the RKE configuration that will create a 2 node Kubernetes cluster. Each Vagrant box is a master, worker and etcd node. To create a cluster use the following command:
```bash
rke up
```

Once the installation is complete, you can access the Kubernetes cluster using the `kube_config_cluster.yml` kubeconfig file:
```bash
kubectl --kubeconfig kube_config_cluster.yml get no

# example output
NAME    STATUS     ROLES                      AGE   VERSION
node1   Ready      controlplane,etcd,worker   68s   v1.20.4
node2   Ready      controlplane,etcd,worker   64s   v1.20.4
```
