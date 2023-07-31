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

