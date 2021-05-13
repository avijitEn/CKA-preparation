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
### bind log etcd Server and verify advertise client URLs

```
 root@instance-1:~# etcd
[WARNING] Deprecated '--logger=capnslog' flag is set; use '--logger=zap' flag instead
2021-05-13 11:26:45.254665 I | etcdmain: etcd Version: 3.4.15
2021-05-13 11:26:45.254851 I | etcdmain: Git SHA: aa7126864
2021-05-13 11:26:45.254947 I | etcdmain: Go Version: go1.12.17
2021-05-13 11:26:45.255034 I | etcdmain: Go OS/Arch: linux/amd64
2021-05-13 11:26:45.255131 I | etcdmain: setting maximum number of CPUs to 1, total number of available CPUs is 1
2021-05-13 11:26:45.255240 W | etcdmain: no data-dir provided, using default data-dir ./default.etcd
2021-05-13 11:26:45.255404 N | etcdmain: the server is already initialized as member before, starting as etcd member...
[WARNING] Deprecated '--logger=capnslog' flag is set; use '--logger=zap' flag instead
2021-05-13 11:26:45.255972 I | embed: name = default
2021-05-13 11:26:45.256086 I | embed: data dir = default.etcd
2021-05-13 11:26:45.256183 I | embed: member dir = default.etcd/member
2021-05-13 11:26:45.256273 I | embed: heartbeat = 100ms
2021-05-13 11:26:45.256359 I | embed: election = 1000ms
2021-05-13 11:26:45.256452 I | embed: snapshot count = 100000
2021-05-13 11:26:45.256543 I | embed: advertise client URLs = http://localhost:2379
2021-05-13 11:26:45.256635 I | embed: initial advertise peer URLs = http://localhost:2380
2021-05-13 11:26:45.256734 I | embed: initial cluster =
2021-05-13 11:26:45.325766 I | etcdserver: restarting member 8e9e05c52164694d in cluster cdf818194e3a8c32 at commit index 11177
raft2021/05/13 11:26:45 INFO: 8e9e05c52164694d switched to configuration voters=()
raft2021/05/13 11:26:45 INFO: 8e9e05c52164694d became follower at term 4
raft2021/05/13 11:26:45 INFO: newRaft 8e9e05c52164694d [peers: [], term: 4, commit: 11177, applied: 0, lastindex: 11177, lastterm: 4]
2021-05-13 11:26:45.329270 W | auth: simple token is not cryptographically signed
2021-05-13 11:26:45.330831 I | mvcc: restore compact to 9146
2021-05-13 11:26:45.335469 I | etcdserver: starting server... [version: 3.4.15, cluster version: to_be_decided]
2021-05-13 11:26:45.337770 I | embed: listening for peers on 127.0.0.1:2380
raft2021/05/13 11:26:45 INFO: 8e9e05c52164694d switched to configuration voters=(10276657743932975437)
2021-05-13 11:26:45.338251 I | etcdserver/membership: added member 8e9e05c52164694d [http://localhost:2380] to cluster cdf818194e3a8c32
2021-05-13 11:26:45.338405 N | etcdserver/membership: set the initial cluster version to 3.4
2021-05-13 11:26:45.338521 I | etcdserver/api: enabled capabilities for version 3.4
raft2021/05/13 11:26:46 INFO: 8e9e05c52164694d is starting a new election at term 4
raft2021/05/13 11:26:46 INFO: 8e9e05c52164694d became candidate at term 5
raft2021/05/13 11:26:46 INFO: 8e9e05c52164694d received MsgVoteResp from 8e9e05c52164694d at term 5
raft2021/05/13 11:26:46 INFO: 8e9e05c52164694d became leader at term 5
raft2021/05/13 11:26:46 INFO: raft.node: 8e9e05c52164694d elected leader 8e9e05c52164694d at term 5
2021-05-13 11:26:46.331683 I | etcdserver: published {Name:default ClientURLs:[http://localhost:2379]} to cluster cdf818194e3a8c32
2021-05-13 11:26:46.332033 I | embed: ready to serve client requests
2021-05-13 11:26:46.332851 N | embed: serving insecure client requests on 127.0.0.1:2379, this is strongly discouraged!
2021-05-13 11:27:28.282328 I | mvcc: store.index: compact 9809
2021-05-13 11:27:28.283434 I | mvcc: finished scheduled compaction at 9809 (took 843.584µs)

```

### Download and Install the Kubernetes Controller Binaries

Download the official Kubernetes release binaries:

```
wget -q --show-progress --https-only --timestamping \
  "https://storage.googleapis.com/kubernetes-release/release/v1.19.0/bin/linux/amd64/kube-apiserver" \
  "https://storage.googleapis.com/kubernetes-release/release/v1.19.0/bin/linux/amd64/kube-controller-manager" \
  "https://storage.googleapis.com/kubernetes-release/release/v1.19.0/bin/linux/amd64/kube-scheduler" \
  "https://storage.googleapis.com/kubernetes-release/release/v1.19.0/bin/linux/amd64/kubectl"
```

Install the Kubernetes binaries:

```
{
  chmod +x kube-apiserver kube-controller-manager kube-scheduler kubectl
  sudo mv kube-apiserver kube-controller-manager kube-scheduler kubectl /usr/local/bin/
}
```

### Configure the Kubernetes API Server

```
run kube-apiserver command see the argument required 
kube-apiserver --etcd-servers=http://localhost:2379

now kube-api server bind with http port 8080 by using below command
netstat -natulp|grep LISTEN
** Note only used http port 8080 , https port 6443 not work due to ssl certification 
```
### kupe-api server description 
```
now if you run kubectl get all you can see kube-apiserver response correctly that mean kube-apiserver working fine 

let create the deployment object 
kubectl create deploy nginx --image=nginx:alpine --replicas=3

now check the resource , you will not find the resource because kube-controller-manager not deploy yet lets deploy   kube-controller-manager



```
