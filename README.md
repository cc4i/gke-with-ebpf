
# GKE with eBPF

This is a project to demostrate to improve observability and security of GKE with eBPF powered technologies. 

## Prerequisite
1. Provision a GKE cluster for test environment.

```bash

export cluster_version="1.22.8-gke.200"
export zone="asia-southeast1-b"

gcloud container clusters create "gke-with-ebpf" \
    --zone ${zone} \
    --no-enable-basic-auth \
    --cluster-version ${cluster_version} \
    --release-channel "regular" \
    --machine-type "n2d-standard-2" \
    --image-type "COS_CONTAINERD" \
    --disk-type "pd-standard" \
    --disk-size "100" \
    --metadata disable-legacy-endpoints=true \
    --scopes "https://www.googleapis.com/auth/cloud-platform" \
    --max-pods-per-node "40" \
    --enable-ip-alias \
    --enable-intra-node-visibility \
    --enable-autoscaling --min-nodes "2" --max-nodes "6" \
    --enable-dataplane-v2 \
    --addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver \
    --enable-shielded-nodes

```

## Pixie
```bash
```
## bpftrace
```bash
```
## eBPF program with golang
```bash
```

https://github.com/lizrice/libbpfgo-beginners

1. Build an eBPF program with golang

https://github.com/iovisor/bpftrace

gcloud compute ssh ebpf-program-instance --zone asia-southeast1-b
sudo su

bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[comm] = count(); }' 

strace -e bpf bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[comm] = count(); }' 
strace -e bpf, perf_event_open,ioctl bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[comm] = count(); }'

cd libbpfgo-beginners
vi Makefile
vi hello.bpf.c
vi hello.bpf.go

make hello.bpf.o

readelf -a hello.bpf.o


2. Use Pixie to troubleshoot on GKE
Pixie uses less than 5% of cluster CPU, and in most cases less than 2%.

k port-forward svc/front-end-internal -n px-sock-shop

2.1  Infra perforrmance 
px/nodes

2.2 DNS
px/dns_flow_graph

2.3 SQL
px/sql_queries

2.4 Customized by PxL 
PxL







git clone https://github.com/lizrice/libbpfgo-beginners.git
cd libbpfgo-beginners
vi makefile
make c?
