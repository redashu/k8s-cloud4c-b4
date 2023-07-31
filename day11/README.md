# k8s-cloud4c-b4

### Revision 

<img src="rev.png">

### Project 1 

<img src="pro1.png">

### planning database mysql deployment 

### creating manifest of db deployment 

```
[ashu@ip-172-31-9-111 ashu-apps]$ mkdir  day11-project 
[ashu@ip-172-31-9-111 ashu-apps]$ ls
ashu-k8s-manifest  day11-project  day7-app  java-app  labs.txt  node-app  python-app  ui-app
[ashu@ip-172-31-9-111 ashu-apps]$ cd  day11-project/


[ashu@ip-172-31-9-111 day11-project]$ kubectl   create  deployment  ashu-db --image=mysql:8.0 --port 3306 --dry-run=client  -o yaml  >db_deploy.yaml

```

### creating secret to store root password of db 

```
[ashu@ip-172-31-9-111 day11-project]$ kubectl  create secret  generic  ashudb-root-cred  --from-literal  ashukey1="Db9@123"  --dry-run=client -o yaml     >root_cred.yaml

[ashu@ip-172-31-9-111 day11-project]$ kubectl  create -f root_cred.yaml 
secret/ashudb-root-cred created

[ashu@ip-172-31-9-111 day11-project]$ 
[ashu@ip-172-31-9-111 day11-project]$ kubectl   get  secret 
NAME               TYPE     DATA   AGE
ashudb-root-cred   Opaque   1      4s
[ashu@ip-172-31-9-111 day11-project]$ 
```

### updating deployment manifest to use cred

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-db
  name: ashu-db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-db
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-db
    spec:
      containers:
      - image: mysql:8.0
        name: mysql
        ports:
        - containerPort: 3306
        resources: {}
        env: 
        - name: MYSQL_ROOT_PASSWORD # variable to store mysql admin cred 
          valueFrom:
            secretKeyRef: # reading password from secret 
              name: ashudb-root-cred
              key: ashukey1 
status: {}

```

### creating db 

```
[ashu@ip-172-31-9-111 day11-project]$ ls
db_deploy.yaml  root_cred.yaml

[ashu@ip-172-31-9-111 day11-project]$ kubectl  create  -f db_deploy.yaml 
deployment.apps/ashu-db created

[ashu@ip-172-31-9-111 day11-project]$ kubectl   get  deploy
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashu-db   1/1     1            1           5s

[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  po
NAME                       READY   STATUS    RESTARTS   AGE
ashu-db-748f67f894-pdd69   1/1     Running   0          8s
[ashu@ip-172-31-9-111 day11-project]$ 
```

### creating an-other secret to store a generic db cred 

```
kubectl  create  secret  generic   ashu-user-cred  --from-literal  MYSQL_USER=ashu  --from-literal MYSQL_PASSWORD="
Hello@123"  --dry-run=client -o yaml >user_cred.yaml 
```

### create secret 2 

```
 kubectl  create -f user_cred.yaml

[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  secrets 
NAME               TYPE     DATA   AGE
ashu-user-cred     Opaque   2      62s
ashudb-root-cred   Opaque   1      17m
```

### updating secret in deployment manifest 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-db
  name: ashu-db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-db
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-db
    spec:
      containers:
      - image: mysql:8.0
        name: mysql
        ports:
        - containerPort: 3306
        resources: {}
        env: 
        - name: MYSQL_ROOT_PASSWORD # variable to store mysql admin cred 
          valueFrom:
            secretKeyRef: # reading password from secret 
              name: ashudb-root-cred
              key: ashukey1 
        envFrom:
        - secretRef:
            name: ashu-user-cred
status: {}

```

### apply changes 

```
[ashu@ip-172-31-9-111 day11-project]$ kubectl  apply -f  db_deploy.yaml 
Warning: resource deployments/ashu-db is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
deployment.apps/ashu-db configured
```

### checking db connection 

```
[ashu@ip-172-31-9-111 day11-project]$ kubectl   get  po
NAME                      READY   STATUS    RESTARTS   AGE
ashu-db-6bd5977cf-x6n8m   1/1     Running   0          11m

[ashu@ip-172-31-9-111 day11-project]$ kubectl  exec -it  ashu-db-6bd5977cf-x6n8m -- bash 
bash-4.4# 
bash-4.4# 
bash-4.4# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.34 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> exit;

bash-4.4# exit
exit
```





