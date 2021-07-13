# Introduction

Bootstraping kubernetes cluster via `kubeadm`  using `Ansible` as automation and configuration management tool on AWS ec2 instances.

## Ansible 

Ansible is a radically simple IT automation engine that automates cloud provisioning, configuration management, application deployment, intra-service orchestration, and many other IT needs

## Kubernetes 

[Kubernetes](https://kubernetes.io/ (K8s)) is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications. The open source project is hosted by the Cloud Native Computing Foundation ([CNCF](https://www.cncf.io/ (CNCF))).

## Prerequisites 

1. Ansible - [How to install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-rhel-centos-or-fedora (Install Ansible))
2. Respective Inventory 
3. Passwordless authentication between master and slave.

## Reference Info

|Hosts     |   Meaning   |
|:--------:|:------------:|
|`k8`| This host group contains all nodes including master and workers|
|`master`| This host group contains only master nodes |
|`worker`| This host group contains only worker nodes |


## Setup 

- Run `new_setup.yaml` to set up the machines.

This playbook will set up nodes with prerequisites for Kubernetes in following steps:
  - Disable `SWAP` & `SELINUX`
  - Creation of package registry 
  - Installation of packages for master and workers nodes
  - Enable [docker](https://docs.docker.com/get-started/overview/ (Docker)) & [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/ (kubectl))
  - Enable `Kubectl` auto completion. 

Command : 

```bash

ansible-playbook  new_setup.yaml

```
- Run `kubeadm_init.yaml`

This playbook will initiate kubernetes cluster using [pod](https://kubernetes.io/docs/concepts/workloads/pods/ (Pod)) network CIDR as `172.18.0.0/16` . Also it will ignore CPU number preflight errors. 

It will print output/status  of `kubeadm` initialization command, please save this output for future used cases. 

_Note_: If you have more than 2 CPU already feel free to remove `--ignore-preflight-errors=numCPU` option from playbook , also you can use another pod CIDR network if needed. Please use LB IP with port 6443 as control plane endpoint if multi master cluster is needed. 

Command : 

```bash

ansible-playbook  kubeadm_init.yaml

```
- Run `join_worker.yaml`

This playbook will help you to join worker nodes to kubernetes cluster using the output of `kubeadm_init.yaml` playbook.


>**TIP:**
>> Run `ansible-playbook  <playbook_name.yaml> -C`  to dry run the playbook, it will render the playbook outbook.


