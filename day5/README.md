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

