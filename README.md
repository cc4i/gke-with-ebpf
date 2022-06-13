
# GKE with eBPF

This is a project to demostrate to improve observability and security of GKE with eBPF powered technologies. 

## Prerequisite
1. Provision a GKE cluster for test environment.

```bash

export zone="asia-southeast1-b"

gcloud container clusters create "gke-with-ebpf" \
    --zone ${zone} \
    --no-enable-basic-auth \
    --machine-type "n2d-standard-2" \
    --scopes "https://www.googleapis.com/auth/cloud-platform" \
    --enable-ip-alias \
    --enable-intra-node-visibility \
    --enable-autoscaling --min-nodes "2" --max-nodes "6" \
    --enable-dataplane-v2

```

2. Launch an instance for eBPF program.

```bash
gcloud compute instances create dev-ebpf-1 \
    --zone=${zone} \
    --machine-type=e2-medium \
    --network-interface=network-tier=PREMIUM,subnet=default \
    --scopes=https://www.googleapis.com/auth/cloud-platform \
    --create-disk=image=projects/ubuntu-os-cloud/global/images/ubuntu-2110-impish-v20220609

```



## eBPF program with Go
```bash
gcloud compute ssh dev-ebpf-1 --zone ${zone}
sudo apt-get update
sudo apt-get install -y libbpf-dev make clang llvm libelf-dev bpftrace  strace golang

# 'sudo su' to use bpftrace with root previlege
sudo su
bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[comm] = count(); }' 
strace -e bpf bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[comm] = count(); }' 

# Code ref to https://github.com/lizrice/libbpfgo-beginners
cd ebpf-hello
make all
readelf -a hello.bpf.o
./hello


```



## Use Pixie to troubleshoot on GKE
Install [Pixie](https://docs.pixielabs.ai/installing-pixie/install-guides/), which uses less than 5% of cluster CPU, and in most cases less than 2%. You need to install Pixie into your Kubernetes cluster for further exprimental.

```bash
# Install demo application
px demo deploy px-sock-shop
kubectl port-forward svc/front-end-internal -n px-sock-shop

#  Infra perforrmance 
px run px/nodes

# DNS
px run px/dns_flow_graph

# SQL
px run px/sql_queries

# Customized by PxL 
PxL

```
