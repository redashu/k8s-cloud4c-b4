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

### creating deployment 

```
kubectl  create  deployment ashu-dep --image=mysql:8.0 --port 3306 --dry-run=client -o yaml >dbdep.yaml
```

### manifest file for deployment 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-dep
  name: ashu-dep
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-dep
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-dep
    spec:
      volumes:
      - name: ashu-db-volx1
        persistentVolumeClaim:
          claimName: ashu-cliam-new 
      containers:
      - image: mysql:8.0
        name: mysql
        ports:
        - containerPort: 3306
        resources: {}
        env: 
        - name: MYSQL_ROOT_PASSWORD
          value: RootPass@123
        volumeMounts:
        - name: ashu-db-volx1
          mountPath: /var/lib/mysql/
status: {}

```

### creating it 

```
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  create -f dbdep.yaml 
deployment.apps/ashu-dep created
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  get deploy 
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
ashu-dep   1/1     1            1           4s
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  get po
NAME                       READY   STATUS    RESTARTS   AGE
ashu-dep-f6db987bc-xwtm6   1/1     Running   0          7s
[ashu@ip-172-31-9-111 day17-testing]$ 
```

### Multi contaienr pod 

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashudb
  name: ashudb
spec:
  volumes: 
  - name: ashu-volx9
    hostPath:
      path: /data/new/ashudb
      type: DirectoryOrCreate
  containers:
  - image: alpine 
    name: ashuc1
    command: ['sh','-c','sleep 100000']
    volumeMounts:
    - name: ashu-volx9
      mountPath: /mnt/data/
      readOnly: true 
  - image: mysql
    name: ashudb
    ports:
    - containerPort: 3306
    resources: {}
    env: 
    - name: MYSQL_ROOT_PASSWORD
      value: RootDb@1234
    volumeMounts:
    - name:  ashu-volx9
      mountPath: /var/lib/mysql/
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

### creating it

```
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  create -f task.yaml 
pod/ashudb created
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  get po
NAME                       READY   STATUS              RESTARTS   AGE
ashu-dep-f6db987bc-xwtm6   1/1     Running             0          28m
ashudb                     0/2     ContainerCreating   0          3s
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  get po
NAME                       READY   STATUS              RESTARTS   AGE
ashu-dep-f6db987bc-xwtm6   1/1     Running             0          28m
ashudb                     0/2     ContainerCreating   0          9s
[ashu@ip-172-31-9-111 day17-testing]$ kubectl  get po
NAME                       READY   STATUS    RESTARTS   AGE
ashu-dep-f6db987bc-xwtm6   1/1     Running   0          28m
ashudb                     2/2     Running   0          22s
[ashu@ip-172-31-9-111 day17-testing]$ 
```



