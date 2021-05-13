# Prerequisites

## Google Cloud Platform or any cloud infratructure with 2 vCPU and 2GB RAM assign to regired VM

 # Provisioning a CA and Generating TLS Certificates expicit in this lab

In this lab you not required provision PKI Infrastructure , just demonstrated kubernetes component with out authentication of certificate, key and service account token for the following components: etcd, kube-apiserver, kube-controller-manager, kube-scheduler, kubelet, and kube-proxy.


## Bootstrapping an etcd Cluster Member

### Download and Install the etcd Binaries

Download the official etcd release binaries from the [etcd](https://github.com/etcd-io/etcd) GitHub project:

```
wget -q --show-progress --https-only --timestamping \
  "https://github.com/etcd-io/etcd/releases/download/v3.4.15/etcd-v3.4.15-linux-amd64.tar.gz"
```

Extract and install the `etcd` server and the `etcdctl` command line utility:

```
{
  tar -xvf etcd-v3.4.15-linux-amd64.tar.gz
  sudo mv etcd-v3.4.15-linux-amd64/etcd* /usr/local/bin/
}
```
### Configure the etcd Server

```
run command to start etcd daemon
  sudo etcd
  
```
