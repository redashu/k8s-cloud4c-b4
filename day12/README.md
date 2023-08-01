# k8s-cloud4c-b4

### revision 

<img src="rev.png">

### verify database things 

```
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  get  deploy
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashu-db   1/1     1            1           23h
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  get secret
NAME               TYPE     DATA   AGE
ashu-user-cred     Opaque   2      23h
ashudb-root-cred   Opaque   1      23h
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  get svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
ashu-db-lb   ClusterIP   10.96.101.231   <none>        3306/TCP   22h
[ashu@ip-172-31-9-111 ashu-apps]$ 
```

