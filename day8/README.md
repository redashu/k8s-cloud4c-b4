# k8s-cloud4c-b4

### connecting to the lab env

```
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  get all
NAME           READY   STATUS    RESTARTS      AGE
pod/ashupod1   1/1     Running   2 (12m ago)   22h
[ashu@ip-172-31-9-111 ashu-apps]$ kubectl  delete all --all
pod "ashupod1" deleted

```
