# k8s-cloud4c-b4

### Project 2

<img src="pro.png">

### Introduction to need to external Storage to the container 

<img src="contst.png">

### testing the need of storage

```
[ashu@ip-172-31-9-111 ashu-apps]$ mkdir  day15-storage-check 
[ashu@ip-172-31-9-111 ashu-apps]$ cd day15-storage-check/
[ashu@ip-172-31-9-111 day15-storage-check]$ 
[ashu@ip-172-31-9-111 day15-storage-check]$ 
[ashu@ip-172-31-9-111 day15-storage-check]$ kubectl  create deployment  ashudb --image=mysql:8.0 --port 3306 --dry-run=client    -o yaml >deploy.yaml
```

### creating env file to store varibles info 
### dbname.env

```
MYSQL_DATABASE="ashudb"
```

### creating configmap using above env

```
 kubectl  create configmap  ashu-cm --from-env-file dbname.env  --dry-run=client -o yaml >cm.yaml

====>
[ashu@ip-172-31-9-111 day15-storage-check]$ kubectl  apply -f cm.yaml 
configmap/ashu-cm created
[ashu@ip-172-31-9-111 day15-storage-check]$ kubectl  get  cm 
NAME               DATA   AGE
ashu-cm            1      4s
kube-root-ca.crt   1      15m
```

