# k8s-cloud4c-b4

### testing lab and delete all the resource of current namespace

```
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space

[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  get  all
NAME                             READY   STATUS    RESTARTS        AGE
pod/ashu-appp-7b99cb4549-9fm5j   1/1     Running   2 (8m17s ago)   22h
pod/ashu-appp-7b99cb4549-hd5ml   1/1     Running   2 (8m20s ago)   22h
pod/ashu-appp-7b99cb4549-qf2p4   1/1     Running   2 (8m18s ago)   22h
pod/ashu-appp-7b99cb4549-xfgxs   1/1     Running   2 (8m17s ago)   22h

NAME             TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/ashulb   NodePort   10.99.33.207   <none>        3000:31831/TCP   22h

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ashu-appp   4/4     4            4           22h

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/ashu-appp-7b99cb4549   4         4         4       22h


[ashu@ip-172-31-9-111 ashu-apps]$ kubectl delete all --all
pod "ashu-appp-7b99cb4549-9fm5j" deleted
pod "ashu-appp-7b99cb4549-hd5ml" deleted
```


