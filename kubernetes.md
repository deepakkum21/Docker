# KUBERNETES

## Nodes:-

- A node is a `machine – physical or virtual – on which kubernetes is installed`.
- A node is a worker machine and this is `were containers will be launched` by kubernetes

## Master:-

- The master is `another node with Kubernetes installed in it`, and is configured as a Master.
- The master `watches over the nodes in the cluster` and is `responsible for the actual orchestration of containers` on the worker nodes.

## Components installed while installing kubernetes:-

1. **API Server**

   - The API server `acts as the front-end for kubernetes`.
   - The `users, management devices, Command line interfaces all talk to the API` server to interact with the kubernetes cluster.

2. **ETCD service**

   - ETCD is a `distributed reliable key-value store` used by kubernetes `to store all data used to manage the cluster`.
   - Think of it this way, `when you have multiple nodes and multiple masters in your cluster, etcd stores all that information on all the nodes` in the cluster in a distributed manner.
   - ETCD is `responsible for implementing locks within the cluster to ensure there are no conflicts between the Masters`

3. **Schedulers service**

   - The scheduler is `responsible for distributing work or containers across multiple nodes`.
   - It `looks for newly created containers` and `assigns them to Nodes`.

4. **container Runtime**

   - The container runtime is the underlying `software that is used to run containers`.
   - In this case it `happens to be Docker`.
   - `other runtime are Rocket or CRIO`.

5. **Controllers**

   - The controllers `are the brain behind orchestration`.
   - They are `responsible for noticing and responding when nodes, containers or endpoints goes down`.
   - The controllers `makes decisions to bring up new containers in such cases`

6. **kubelet**

   - kubelet is the `agent that runs on each node in the cluster`.
   - The agent is `responsible for making sure that the containers are running on the nodes as expected`

## Master vs Worker Nodes

1. Master node have

   - API Server
   - Controllers
   - Schedulers
   - ETCD

2. Worker node have
   - kubelet
   - container runtine

## kubectl cmds:-

1.  `kubectl run ` kubectl run command is used to deploy an application on the cluster.
2.  `kubectl culster-info` used to view information about the cluster
3.  `kubectl get pods` list all the nodes part of the cluster

## minikube VS kubeadmin

- **minikube** `utility you could only setup a single node kubernetes cluster`.
- **kubeadmin** tool h`elps us setup a multi node cluster with master and workers on separate machines`.

## Steps to install kubeadmin (https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

1.  First, you must have multiple systems or virtual machines created for configuring a cluster. Once the systems are created, designate one as master and others as worker nodes.

2.  The next step is to `install a container runtime` on the hosts. We will be using Docker, so we must install Docker on all the nodes.

3.  The next step is to i`nstall kubeadmin tool on all the nodes`. The kubeadmin tool helps us bootstrap the kubernetes solution by installing and configuring all the required components in the right nodes.

    1.  **Have unique HOSTNAME for each node and master**

        - edit hostname file in `/etc/hostname`
        - also edit in `/etc/hosts`

    2.  add a static ip adapter like `Host only`

    3.  Now **add a ip**

        - edit file `/etc/network/interfaces`
        - add following cmd

                 auto nameofAdapterWhcihWasCreated(enp0s8)
                 iface nameofAdapterWhcihWasCreated(enp0s8) inet static
                       address 192.168.56.2(witin the range of the adapter)
                       netmask 255.255.255.0

    4.  **disable swap**
        - execute `swapoff -a`
        - go to `/etc/fstab` and `comment of swap line`.

4.  The next step is to `initialize the Master server`. During this process all the` required components are installed` and configured on the master server. That way we can start the cluster level configurations from the master server.

5.  Once the master is initialized and before joining the worker nodes to the master, we must ensure that the network pre-requisites are met. A normal network connectivity between the systems is not SUFFICIENT for this. Kubernetes requires a `special network between the master and worker nodes which is called as a` **`POD network`**.

6.  last step is to join the worker nodes to the master node.
