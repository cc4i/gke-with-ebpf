
# GKE with eBPF

This is a project to demostrate to improve observability and security of GKE with eBPF powered technologies. 

## Prerequisite

## Pixie
## bpftrace
## eBPF program with golang

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