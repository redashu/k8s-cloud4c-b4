# k8s-cloud4c-b4

### PRoject calico -- CNI in k8s is running as DS 

```
[ashu@ip-172-31-9-111 day18-things]$ kubectl  get po -n kube-system   -o wide  | grep -i calico
calico-kube-controllers-6c99c8747f-r4ghn   1/1     Running   30 (9m2s ago)   21d   192.168.166.142   node1        <none>           <none>
calico-node-hm9dh                          1/1     Running   31 (9m2s ago)   21d   172.31.0.13       node3        <none>           <none>
calico-node-j76x4                          1/1     Running   31 (9m2s ago)   21d   172.31.13.5       node2        <none>           <none>
calico-node-jlvpf                          1/1     Running   31 (9m2s ago)   21d   172.31.14.81      masternode   <none>           <none>
calico-node-tbj8x                          1/1     Running   31 (9m2s ago)   21d   172.31.5.138      node1        <none>           <none>
[ashu@ip-172-31-9-111 day18-things]$ 
[ashu@ip-172-31-9-111 day18-things]$ 
[ashu@ip-172-31-9-111 day18-things]$ kubectl  get  ds -A
NAMESPACE     NAME          DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   calico-node   4         4         4       4            4           kubernetes.io/os=linux   21d
kube-system   kube-proxy    4         4         4       4            4           kubernetes.io/os=linux   21d
[ashu@ip-172-31-9-111 day18-things]$ 
```
