# k8s-cloud4c-b4

### connecting to lab and cleaning everything in personal namespace

```
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  config  get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space

[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  delete all --all
pod "ashu-db" deleted

[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  delete secret  --all
secret "ashu-db-pass" deleted
secret "ashu-reg-cred" deleted
[ashu@ip-172-31-9-111 ashu-apps]$ 
```

### overall default is the namespace where we all connect by default as starting user

<img src="user.png">

### checking resource of any other namespace --  If you have permission 

```
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl   get  deployment   
No resources found in ashu-space namespace.
[ashu@ip-172-31-9-111 ashu-apps]$

[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space

[ashu@ip-172-31-9-111 ashu-apps]$ 
[ashu@ip-172-31-9-111 ashu-apps]$ 
[ashu@ip-172-31-9-111 ashu-apps]$ whoami
ashu


[ashu@ip-172-31-9-111 ashu-apps]$ kubectl   get  deployment    -n  kubernetes-dashboard  
NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
dashboard-metrics-scraper   1/1     1            1           3d8h
kubernetes-dashboard        1/1     1            1           3d8h


[ashu@ip-172-31-9-111 ashu-apps]$ 
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl   -n  kubernetes-dashboard    get  deployment 
NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
dashboard-metrics-scraper   1/1     1            1           3d8h
kubernetes-dashboard        1/1     1            1           3d8h
[ashu@ip-172-31-9-111 ashu-apps]$ 
```

### targeting any Resource to any other namespace 

<img src="ns11.png">

