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
