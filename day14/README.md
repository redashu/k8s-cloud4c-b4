# k8s-cloud4c-b4

### cleaning namespace data

```
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  delete all,secret,cm,ingress --all
pod "ashu-db-84566c54f4-klf2z" deleted
pod "ashu-webapp-7d89dcb8b8-8hbdw" deleted
service "ashu-db-lb" deleted
service "ashulb1" deleted
deployment.apps "ashu-db" deleted
deployment.apps "ashu-webapp" deleted
secret "ashu-user-cred" deleted
secret "ashu-user-cred1" deleted
secret "ashudb-root-cred" deleted
configmap "ashu-db-name" deleted
configmap "ashu-web-config" deleted
configmap "kube-root-ca.crt" deleted
[ashu@ip-172-31-9-111 ashu-apps]$ 



```
