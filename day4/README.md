# k8s-cloud4c-b4

## Revision 

<img src="rev.png">

### verify kubectl connection from client to k8s master

```
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  get nodes
NAME         STATUS   ROLES           AGE   VERSION
masternode   Ready    control-plane   27h   v1.27.3
node1        Ready    <none>          27h   v1.27.3
node2        Ready    <none>          27h   v1.27.3
node3        Ready    <none>          27h   v1.27.3
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  cluster-info 
Kubernetes control plane is running at https://13.200.76.193:6443
CoreDNS is running at https://13.200.76.193:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
[ashu@ip-172-31-9-111 ashu-apps]$ 


```

## Introduction to second most import master node component

<img src="etcd.png">



