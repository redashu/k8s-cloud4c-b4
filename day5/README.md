# k8s-cloud4c-b4

### Revision 

<img src="rev.png">

## problems 

### -- auto generate manifest 
### -- pod level isolation 

### generating / printing yaml / json based pod manifest 

```
 183  kubectl   run  ashupod1  --image=docker.io/dockerashu/nodeapp:v1  --port 3000 --dry-run=client -o  yaml 
  184  kubectl   run  ashupod1  --image=docker.io/dockerashu/nodeapp:v1  --port 3000 --dry-run=client -o  json 
```

### storing pod manifest into files

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   run  ashupod1  --image=docker.io/dockerashu/nodeapp:v1  --port 3000 --dry-run=client -o  yaml >auto.yaml
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ ls
ashu-nodeapp-pod1.yaml  auto.yaml
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   run  ashupod1  --image=docker.io/dockerashu/nodeapp:v1  --port 3000 --dry-run=client -o json >hello.json
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ ls
ashu-nodeapp-pod1.yaml  auto.yaml  hello.json
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ 

```

### creating manifest request

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  create -f  auto.yaml 
pod/ashupod1 created
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  get  pods
NAME                READY   STATUS    RESTARTS   AGE
ashupod1            1/1     Running   0          3s
```

### replace and delete with manifest 

```
ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  delete -f auto.yaml 
pod "ashupod1" deleted

kubectl  replace -f auto.yaml --force

```

### same command with json based manifest as well

```
ashu@ip-172-31-9-111 ashu-k8s-manifest]$ ls
ashu-nodeapp-pod1.yaml  auto.yaml  hello.json
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  create -f hello.json 
pod/ashupod1 created
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  delete -f hello.json 
pod "ashupod1" deleted

```


### task 1

<img src="task1.png">

### solution 

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashupod-task1
  name: ashupod-task1
spec:
  nodeName: node3  # static scheduling of pod 
  containers:
  - image: adminer:4.8.1
    name: ashupod-task1
    ports:
    - containerPort: 8080
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

### checking 

```
ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  replace -f task1.yaml  --force 
pod "ashupod-task1" deleted
pod/ashupod-task1 replaced
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  get po -o wide
NAME                READY   STATUS              RESTARTS   AGE     IP                NODE     NOMINATED NODE   READINESS GATES
ashupod-task1       0/1     ContainerCreating   0          5s      <none>            node3    <none>           <none>
hari-nodeapp-pod1   1/1     Running             0          8m32s   192.168.166.178   node1    <none>           <none>
karteekpod-adm1     0/1     
```

### accessing container inside pod from kubectl 

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   exec -it  ashupod-task1  -- bash 
adminer@ashupod-task1:/var/www/html$ 
adminer@ashupod-task1:/var/www/html$ mkdir  /tmp/okhello
adminer@ashupod-task1:/var/www/html$ ls /tmp/
okhello
adminer@ashupod-task1:/var/www/html$ exit
exit
```
## Introduction to namespace

<img src="ns.png">

### creating custom namespace

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   create  namespace  ashu-space --dry-run=client -o yaml >ns.yaml 
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  create -f ns.yaml 
namespace/ashu-space created
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  namespaces 
NAME              STATUS   AGE
abbas-apps        Active   7h31m
adithya-apps      Active   7h28m
ankita-apps       Active   7h33m
ashu-apps         Active   7h33m
ashu-space        Active   6s
```

### setting namespace as default 

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  pods
No resources found in default namespace.
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   config  set-context  --current --namespace=ashu-space
Context "kubernetes-admin@kubernetes" modified.
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ 
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  pods
No resources found in ashu-space namespace.
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ 

```

### Networking in k8s 

<img src="net1.png">

## Using CONtainer network interface (CNI) distributed bridge concpet 

### each pod can connect to other pod as well

<img src="podn2.png">

## testing pod communication 

### creating pod 

```
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   run ashu-ui-pod --image=nginx:1.24  --port 80 --dry-run=client -o yaml >ui.yaml
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ ls
ashu-nodeapp-pod1.yaml  auto.yaml  hello.json  ns.yaml  task1.yaml  ui.yaml
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ 

==========>>
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl  create -f ui.yaml 
pod/ashu-ui-pod created
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  po -o wide
NAME          READY   STATUS              RESTARTS   AGE   IP       NODE    NOMINATED NODE   READINESS GATES
ashu-ui-pod   0/1     ContainerCreating   0          5s    <none>   node2   <none>           <none>
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ kubectl   get  po -o wide
NAME          READY   STATUS    RESTARTS   AGE   IP               NODE    NOMINATED NODE   READINESS GATES
ashu-ui-pod   1/1     Running   0          23s   192.168.104.59   node2   <none>           <none>
[ashu@ip-172-31-9-111 ashu-k8s-manifest]$ 

```


