# k8s-cloud4c-b4

### using secret & configmap to call deployment manifest 

<img src="depm.png">

### using configmap to store db name info 

```
[ashu@ip-172-31-9-111 day11-project]$ kubectl  create configmap  ashu-db-name --from-literal  ashudbn="wordpress"  --dry-run=client -o yaml  >db-info.yaml
[ashu@ip-172-31-9-111 day11-project]$ kubectl  create -f db-info.yaml 
configmap/ashu-db-name created
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get configmaps 
NAME               DATA   AGE
ashu-db-name       1      4s
kube-root-ca.crt   1      12m
[ashu@ip-172-31-9-111 day11-project]$ 
```

### updating manifest with configmap 

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
        - name: MYSQL_DATABASE  # to create a database 
          valueFrom: # reading value 
            configMapKeyRef: # configmap 
              name: ashu-db-name # name of cm 
              key: ashudbn  # key of cm 
        - name: MYSQL_ROOT_PASSWORD # variable to store mysql admin cred 
          valueFrom:
            secretKeyRef: # reading password from secret 
              name: ashudb-root-cred
              key: ashukey1 
        envFrom:
        - secretRef:
            name: ashu-user-cred # name of the secret 
status: {}

```

### Redeploy db 

```
[ashu@ip-172-31-9-111 day11-project]$ ls
db_deploy.yaml  dbsvc.yaml      user_cred.yaml      web-cred.yaml
db-info.yaml    root_cred.yaml  webapp_deploy.yaml  weblb.yaml
[ashu@ip-172-31-9-111 day11-project]$ kubectl  apply  -f db-info.yaml  -f root_cred.yaml  -f  user_cred.yaml  -f db_deploy.yaml 
Warning: resource configmaps/ashu-db-name is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
configmap/ashu-db-name configured
secret/ashudb-root-cred created
secret/ashu-user-cred created
deployment.apps/ashu-db created

=======>
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  cm
NAME               DATA   AGE
ashu-db-name       1      10m
kube-root-ca.crt   1      22m

[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  secret
NAME               TYPE     DATA   AGE
ashu-user-cred     Opaque   2      43s
ashudb-root-cred   Opaque   1      43s

[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  deploy
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashu-db   1/1     1            1           45s

[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  po
NAME                       READY   STATUS    RESTARTS   AGE
ashu-db-84566c54f4-klf2z   1/1     Running   0          47s

===> Db service

[ashu@ip-172-31-9-111 day11-project]$ kubectl  apply -f dbsvc.yaml 
service/ashu-db-lb created
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
ashu-db-lb   ClusterIP   10.98.217.227   <none>        3306/TCP   3s
[ashu@ip-172-31-9-111 day11-project]$ 


```

### creating Configmap for web app 

```
kubectl  create  configmap ashu-web-config --from-literal WORDPRESS_DB_HOST="ashu-db-lb"  --from-literal WORDPRESS_DB_NAME="wordpress" --dry-run=client -o yaml >webcm.yaml 
[ashu@ip-172-31-9-111 day11-project]$ kubectl  create -f webcm.yaml 
configmap/ashu-web-config created
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  cm
NAME               DATA   AGE
ashu-db-name       1      19m
ashu-web-config    2      56s
kube-root-ca.crt   1      31m
```

### using it in deployment manifest 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-webapp
  name: ashu-webapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-webapp
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-webapp
    spec:
      containers:
      - image: wordpress:6.2.1-apache
        name: wordpress
        ports:
        - containerPort: 80
        resources: {}
        envFrom:
        - configMapKeyRef:
            name: ashu-web-config
        envFrom:
        - secretRef:
            name: ashu-user-cred1
status: {}

```

### deploy it 

```
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  cm
NAME               DATA   AGE
ashu-db-name       1      21m
ashu-web-config    2      2m29s
kube-root-ca.crt   1      33m
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  secret
NAME               TYPE     DATA   AGE
ashu-user-cred     Opaque   2      11m
ashu-user-cred1    Opaque   2      35s
ashudb-root-cred   Opaque   1      11m
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  deploy
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-db       1/1     1            1           11m
ashu-webapp   1/1     1            1           47s
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  po
NAME                          READY   STATUS    RESTARTS   AGE
ashu-db-84566c54f4-klf2z      1/1     Running   0          11m
ashu-webapp-b7f56d855-gj4gp   1/1     Running   0          50s
```

### deploy svc

```
[ashu@ip-172-31-9-111 day11-project]$ kubectl  apply -f weblb.yaml 
service/ashulb1 created
[ashu@ip-172-31-9-111 day11-project]$ kubectl  get  svc
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
ashu-db-lb   ClusterIP   10.98.217.227    <none>        3306/TCP       8m45s
ashulb1      NodePort    10.108.231.225   <none>        80:30860/TCP   3s
[ashu@ip-172-31-9-111 day11-project]$ 
```





