# k8s-cloud4c-b4

## targets

<img src="ext1.png">

### Extending k8s API's using CRD 

<img src="crd.png">

### Helm revision 

<img src="rev1.png">

### helm phases 

<img src="helm1.png">

## command helm commands 

### version and listing repo 

```
ashu@ip-172-31-9-111 ashu-apps]$ helm version 
version.BuildInfo{Version:"v3.12.2", GitCommit:"1e210a2c8cc5117d1055bfaa5d40f51bbc2e345e", GitTreeState:"clean", GoVersion:"go1.20.5"}
[ashu@ip-172-31-9-111 ashu-apps]$ 
[ashu@ip-172-31-9-111 ashu-apps]$ helm repo ls
NAME            URL                               
ashu-repo       https://charts.bitnami.com/bitnami
new-repo        https://charts.helm.sh/stable     
[ashu@ip-172-31-9-111 ashu-apps]$ 

```

### these days adding repo is an optional part 

```
[ashu@ip-172-31-9-111 ashu-apps]$ helm repo ls
NAME            URL                               
ashu-repo       https://charts.bitnami.com/bitnami
new-repo        https://charts.helm.sh/stable     
[ashu@ip-172-31-9-111 ashu-apps]$ 
[ashu@ip-172-31-9-111 ashu-apps]$ helm install ashu-app  oci://registry-1.docker.io/bitnamicharts/nginx
Pulled: registry-1.docker.io/bitnamicharts/nginx:15.1.2
Digest: sha256:1482570d46bca932ce08ac2afa438c15393035e77668e4447786ca182910aa58
NAME: ashu-app
LAST DEPLOYED: Thu Aug 10 12:13:24 2023
NAMESPACE: ashu-space
STATUS: deployed
REVISION: 1
TEST SUITE: None

=====>>
[ashu@ip-172-31-9-111 ashu-apps]$ helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
ashu-app        ashu-space      1

====>>
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  get deploy
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
ashu-app-nginx   1/1     1            1           66s
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  get svc
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
ashu-app-nginx   LoadBalancer   10.97.171.203   <pending>     80:31524/TCP   69s
[ashu@ip-172-31-9-111 ashu-apps]$

====>>
[ashu@ip-172-31-9-111 ashu-apps]$ helm uninstall  ashu-app
release "ashu-app" uninstalled

```

## Creating custom helm charts 

### directory for project

```
[ashu@ip-172-31-9-111 ashu-apps]$ mkdir helm-projects
[ashu@ip-172-31-9-111 ashu-apps]$ ls
ashu-k8s-manifest  day15-storage-check  day16-pre-project  day18-things  dbdep.yaml     hpa       labs.txt  python-app
day11-project      day16-mongo-project  day17-testing      day7-app      helm-projects  java-app  node-app  ui-app
[ashu@ip-172-31-9-111 ashu-apps]$ cd helm-projects/
[ashu@ip-172-31-9-111 helm-projects]$ ls
[ashu@ip-172-31-9-111 helm-projects]$ 
[ashu@ip-172-31-9-111 helm-projects]$ helm create  ashu-ui-app
Creating ashu-ui-app
[ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app
[ashu@ip-172-31-9-111 helm-projects]$ 

```

### see structure 

```
ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app

[ashu@ip-172-31-9-111 helm-projects]$ ls  ashu-ui-app/
charts  Chart.yaml  templates  values.yaml
[ashu@ip-172-31-9-111 helm-projects]$ 

```

### chart template

```
[ashu@ip-172-31-9-111 helm-projects]$ ls  ashu-ui-app/templates/
deployment.yaml  _helpers.tpl  hpa.yaml  ingress.yaml  NOTES.txt  serviceaccount.yaml  service.yaml  tests
[ashu@ip-172-31-9-111 helm-projects]$ 

```

### importance of values.yaml 

<img src="imp.png">

### deploy charts locally 

```
[ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app
[ashu@ip-172-31-9-111 helm-projects]$ helm install cloud4rc-ui   ./ashu-ui-app/ 
NAME: cloud4rc-ui
LAST DEPLOYED: Thu Aug 10 12:35:32 2023
NAMESPACE: ashu-space
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namesp

======>>
[ashu@ip-172-31-9-111 helm-projects]$ helm   ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                  APP VERSION
cloud4rc-ui     ashu-space      1

===>>
006  helm ls
 1007  ls
 1008  helm upgrade cloud4rc-ui  ./ashu-ui-app/
 1009  helm ls
 1010  kubectl  get  po
 1011  history 
[ashu@ip-172-31-9-111 helm-projects]$ kubectl  get  po
NAME                                       READY   STATUS    RESTARTS   AGE
cloud4rc-ui-ashu-ui-app-584585c764-bjdhw   1/1     Running   0          5m53s
cloud4rc-ui-ashu-ui-app-584585c764-gth6m   1/1     Running   0          12s
[ashu@ip-172-31-9-111 helm-projects]$  
```

### replace values.yaml 

```
[ashu@ip-172-31-9-111 helm-projects]$ helm install cc-app ./ashu-ui-app/  --values values.yaml  
NAME: cc-app
LAST DEPLOYED: Thu Aug 10 12:46:38 2023
NAMESPACE: ashu-space
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace ashu-space -l "app.kubernetes.io/name=ashu-ui-app,app.kubernetes.io/instance=cc-app" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace ashu-space $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace ashu-space port-forward $POD_NAME 8080:$CONTAINER_PORT
[ashu@ip-172-31-9-111 helm-projects]$ helm ls
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
cc-app  ashu-space      1               2023-08-10 12:46:38.904236612 +0000 UTC deployed        ashu-ui-app-0.1.0       1.16.0     
[ashu@ip-172-31-9-111 helm-projects]$ kubectl  get  po
NAME                                  READY   STATUS    RESTARTS   AGE
cc-app-ashu-ui-app-5fbd59b859-xq5ct   1/1     Running   0          14s
[ashu@ip-172-31-9-111 helm-projects]$ kubectl  get  svc
NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
cc-app-ashu-ui-app   ClusterIP   10.102.229.141   <none>        3000/TCP   16s
[ashu@ip-172-31-9-111 helm-projects]$ 
```

### packaging chart for publish

<img src="pkg.png">

### pulling sample package from repo 

```
[ashu@ip-172-31-9-111 helm-projects]$ helm search repo mysql 
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                                       
ashu-repo/mysql                         9.10.9          8.0.34          MySQL is a fast, reliable, scalable, and easy t...
new-repo/mysql                          1.6.9           5.7.30          DEPRECATED - Fast, reliable, scalable, and easy...
new-repo/mysqldump                      2.6.2           2.4.1           DEPRECATED! - A Helm chart to help backup MySQL...
new-repo/prometheus-mysql-exporter      0.7.1           v0.11.0         DEPRECATED A Helm chart for prometheus mysql ex...
ashu-repo/phpmyadmin                    12.0.0          5.2.1           phpMyAdmin is a free software tool written in P...
new-repo/percona                        1.2.3           5.7.26          DEPRECATED - free, fully compatible, enhanced, ...
new-repo/percona-xtradb-cluster         1.0.8           5.7.19          DEPRECATED - free, fully compatible, enhanced, ...
new-repo/phpmyadmin                     4.3.5           5.0.1           DEPRECATED phpMyAdmin is an mysql administratio...
ashu-repo/mariadb                       13.0.0          11.0.2          MariaDB is an open source, community-developed ...
ashu-repo/mariadb-galera                9.0.1           11.0.2          MariaDB Galera is a multi-primary database clus...
new-repo/gcloud-sqlproxy                0.6.1           1.11            DEPRECATED Google Cloud SQL Proxy                 
new-repo/mariadb                        7.3.14          10.3.22         DEPRECATED Fast, reliable, scalable, and easy t...

[ashu@ip-172-31-9-111 helm-projects]$ 
[ashu@ip-172-31-9-111 helm-projects]$ helm pull ashu-repo/mariadb

[ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app  mariadb-13.0.0.tgz  values.yaml
[ashu@ip-172-31-9-111 helm-projects]$ 

```

### untaring the package 

```
ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app  mariadb-13.0.0.tgz  values.yaml
[ashu@ip-172-31-9-111 helm-projects]$ tar  xvzf  mariadb-13.0.0.tgz 
mariadb/Chart.yaml
mariadb/Chart.lock
mariadb/values.yaml
mariadb/values.schema.json
mariadb/templates/NOTES.txt
```

### checking more detail

```
[ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app  mariadb  mariadb-13.0.0.tgz  values.yaml
[ashu@ip-172-31-9-111 helm-projects]$ cd mariadb/
[ashu@ip-172-31-9-111 mariadb]$ ls
Chart.lock  charts  Chart.yaml  README.md  templates  values.schema.json  values.yaml
[ashu@ip-172-31-9-111 mariadb]$ ls  templates/
extra-list.yaml  networkpolicy-egress.yaml  primary               rolebinding.yaml  secondary     serviceaccount.yaml
_helpers.tpl     NOTES.txt                  prometheusrules.yaml  role.yaml         secrets.yaml  servicemonitor.yaml
[ashu@ip-172-31-9-111 mariadb]$ 

```

### converting chart into a package 

```
[ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app  mariadb  mariadb-13.0.0.tgz  values.yaml
[ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app  mariadb  mariadb-13.0.0.tgz  values.yaml
[ashu@ip-172-31-9-111 helm-projects]$ 
[ashu@ip-172-31-9-111 helm-projects]$ helm package  ashu-ui-app/
Successfully packaged chart and saved it to: /home/ashu/ashu-apps/helm-projects/ashu-ui-app-0.1.0.tgz
[ashu@ip-172-31-9-111 helm-projects]$ ls
ashu-ui-app  ashu-ui-app-0.1.0.tgz  mariadb  mariadb-13.0.0.tgz  values.yaml
[ashu@ip-172-31-9-111 helm-projects]$ 
```



