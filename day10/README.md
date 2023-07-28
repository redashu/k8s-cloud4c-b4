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
