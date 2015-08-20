# Kubernetes OpenStack

## Overview
This repo provides a basic Kubernetes implementation on OpenStack VMs using static routing.

## Diagram
![Network Diagram](https://raw.githubusercontent.com/lachie83/kubernetes-openstack/master/diagrams/k8s-openstack-logical-diagram.png)

## Design Details
 * Each VM has a single interface provided by the OpenStack overlay network (eth0 in this case).
 * Kubernetes nodes run docker-engine. Each docker-engine is configure to bind to an allocated address block and provide access to the running containers using the docker0 bridge interface.
  * k8s-node-01 - 10.2.2.1/24
  * k8s-node-02 - 10.2.3.1/24
 * The Kubernetes master is also assigned an allocated block for in order to provide external access to services running on the Kubernetes cluster. This block is provided to the kube-apiserver process as a parameter `--service-cluster-ip-range=10.2.1.1/24`
  * The allocated block are statically routed via eth0 on the respective VMs.
