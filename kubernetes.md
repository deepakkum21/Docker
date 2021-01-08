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
