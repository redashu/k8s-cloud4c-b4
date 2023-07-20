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

## lets push last day image of node app to docker hub 

```
[ashu@ip-172-31-9-111 ashu-apps]$ docker images  | grep ashu
ashunodejs                               v1           2a5d82047f86   24 hours ago   186MB
ashunode                                 appv1        ce00f146e406   24 hours ago   1.1GB
ashualp                                  pycodev1     04935507879e   47 hours ago   50.7MB
ashupython                               v1           a5e4ed77cbd0   2 days ago     1GB
dockerashu/ashu-app                      v1           89f0a2c36f4b   2 days ago     190MB
ashu-uiapp                               v1           89f0a2c36f4b   2 days ago     190MB
[ashu@ip-172-31-9-111 ashu-apps]$ 
[ashu@ip-172-31-9-111 ashu-apps]$ docker tag  ashunode:appv1   docker.io/dockerashu/nodeapp:v1 
[ashu@ip-172-31-9-111 ashu-apps]$ docker login 
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/ashu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[ashu@ip-172-31-9-111 ashu-apps]$ docker push docker.io/dockerashu/nodeapp:v1 
The push refers to repository [docker.io/dockerashu/nodeapp]
bc305fc3c296: Pushed 
53b54bda291d: Pushed 
0e3cb208d303: Pushed 
89e26aac0240: Mounted from library/node 
```

### understanding role or kubelet and docker in minion node 

<img src="node1.png">

### Introduction to pod 

<img src="pod1.png">

### creating pod using manifest file 

<img src="manifest.png">

### creating a directory to put manifest

```
[ashu@ip-172-31-9-111 ashu-apps]$ ls
java-app  node-app  python-app  ui-app
[ashu@ip-172-31-9-111 ashu-apps]$ mkdir  ashu-k8s-manifest
[ashu@ip-172-31-9-111 ashu-apps]$ ls
ashu-k8s-manifest  java-app  node-app  python-app  ui-app
[ashu@ip-172-31-9-111 ashu-apps]$ ls  ashu-k8s-manifest/
ashu-nodeapp-pod1.yaml
[ashu@ip-172-31-9-111 ashu-apps]$ 

```

### first pod manifest 

```
apiVersion: v1 # we are targing api server version 1 
kind: Pod # we are asking k8s master to take req about pod
metadata: # info about Kind value
  name: ashu-nodeapp-pod1 # name of my pod 
spec: # everything we need in pod 
  containers: 
  - name: ashuc1
    image: docker.io/dockerashu/nodeapp:v1 
```

### lets deploy it 

```
[ashu@ip-172-31-9-111 ashu-apps]$ ls
ashu-k8s-manifest  java-app  node-app  python-app  ui-app
[ashu@ip-172-31-9-111 ashu-apps]$ cd  ashu-k8s-manifest/
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ ls
ashu-nodeapp-pod1.yaml
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   create  -f  ashu-nodeapp-pod1.yaml 
pod/ashu-nodeapp-pod1 created
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  pods
NAME                  READY   STATUS    RESTARTS   AGE
ashu-nodeapp-pod1     1/1     Running   0          18s
harithanodeapp-pod1   1/1     Running   0          14s
mogal-nodeapp-pod1    1/1     Running   0          14s
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ 

```

### checking node details about my pod 

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  nodes
NAME         STATUS   ROLES           AGE   VERSION
masternode   Ready    control-plane   28h   v1.27.3
node1        Ready    <none>          28h   v1.27.3
node2        Ready    <none>          28h   v1.27.3
node3        Ready    <none>          28h   v1.27.3
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  pods
NAME                     READY   STATUS             RESTARTS      AGE
ashu-nodeapp-pod1        1/1     Running            0             8m33s
hari-nodeapp-pod1        1/1     Running            0             3m54s
harithanodeapp-pod1      1/1     Running            0             8m29s
mahesh-nodeapp-pod1      1/1     Running            0             4m16s
mogal-nodeapp-pod1       1/1     Running            0             8m29s
rajeswari-nodeapp-pod1   1/1     Running            0             3m3s
venkat-nodeapp-pod1      1/1     Running            0             6m33s
venkat-nodeapp-pod2      0/1     CrashLoopBackOff   5 (41s ago)   4m
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ 
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$  kubectl   get  pods ashu-nodeapp-pod1  -o wide 
NAME                READY   STATUS    RESTARTS   AGE     IP               NODE    NOMINATED NODE   READINESS GATES
ashu-nodeapp-pod1   1/1     Running   0          8m35s   192.168.135.11   node3   <none>           <none>
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ 
```
### checking all the pods node info 

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  pods -o wide
NAME                     READY   STATUS             RESTARTS      AGE     IP                NODE    NOMINATED NODE   READINESS GATES
ashu-nodeapp-pod1        1/1     Running            0             11m     192.168.135.11    node3   <none>           <none>
hari-nodeapp-pod1        1/1     Running            0             6m46s   192.168.166.144   node1   <none>           <none>
harithanodeapp-pod1      1/1     Running            0             11m     192.168.135.12    node3   <none>           <none>
mahesh-nodeapp-pod1      1/1     Running            0             7m8s    192.168.135.13    node3   <none>           <none>
mogal-nodeapp-pod1       1/1     Running            0             11m     192.168.104.15    node2   <none>           <none>
```

### Introduction to schedular in k8s master node

<img src="sch.png">

### more kubectl pod related instructions

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  describe  pod  ashu-nodeapp-pod1 
Name:             ashu-nodeapp-pod1
Namespace:        default
Priority:         0
Service Account:  default
Node:             node3/172.31.0.13
Start Time:       Thu, 20 Jul 2023 13:04:18 +0000
Labels:           <none>
Annotations:      cni.projectcalico.org/containerID: bb40a866b951fa0aad95469f851d0cc18a3bf44995f0a29d95e8b9780dcaed5b
                  cni.projectcalico.org/podIP: 192.168.135.11/32
```

### logs of pod app

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   logs  ashu-nodeapp-pod1

> demo-app@1.0.0 start
> node index.js

Example app listening on port 3000
```
### deleting pod 

```

  [ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  delete  pod  ashu-nodeapp-pod1
pod "ashu-nodeapp-pod1" deleted
```

