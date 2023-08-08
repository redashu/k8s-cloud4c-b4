# k8s-cloud4c-b4

### understandign CSI 

<img src="csi.png">

### creating PV -- which is going to take storage from External Server

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: ashu-pv
spec:
  capacity:
    storage: 5Gi # 3Gi  to 10Gi 
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany # change 1
  storageClassName: manual # change 2 
  nfs:
    path: /data/db-store/ashu/ # source location 
    server: 172.31.9.111 # storage server IP address 
```

### creating pv 

```
 kubectl  create -f ashupv.yaml
===
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  get  pv  | grep -i avai
ashu-pv-newst     5Gi        RWX            Retain           Available                                    manual                  68s
```

### PVC is all about sending request to pv and finding the best match 

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ashu-cliam-new
spec:
  accessModes:
    - ReadWriteMany
  volumeMode: Filesystem
  resources:
    requests:
      storage: 6Gi
  storageClassName: manual 
  
```

### 

```
[ashu@ip-172-31-9-111 day17-testing]$ ls
ashu-pvc.yaml  ashupv.yaml
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  create -f ashu-pvc.yaml 
persistentvolumeclaim/ashu-cliam-new created
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  get  pvc
NAME             STATUS   VOLUME      CAPACITY   ACCESS MODES   STORAGECLASS   AGE
ashu-cliam-new   Bound    mahesh-pv   6Gi        RWX            manual         3s
[ashu@ip-172-31-9-111 day17-testing]$ 
```
