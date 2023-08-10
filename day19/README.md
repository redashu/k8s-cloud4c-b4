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



