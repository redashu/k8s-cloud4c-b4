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

