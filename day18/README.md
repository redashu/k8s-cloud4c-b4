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

### Deployment of portainer in k8s as Deployment controller 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-mon
  name: ashu-mon
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-mon
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-mon
    spec:
      volumes: 
      - name: ashu-socket-volume
        hostPath:
          path: /var/run/docker.sock
          type: Socket 
      containers:
      - image: portainer/portainer-ce
        name: portainer-ce
        ports:
        - containerPort: 9443
        - containerPort: 9000
        volumeMounts:
        - name: ashu-socket-volume
          mountPath: /var/run/docker.sock
        resources: {}
status: {}

```

### deploy it

```
[ashu@ip-172-31-9-111 day18-things]$ kubectl apply -f portainer.yaml 
deployment.apps/ashu-mon created
[ashu@ip-172-31-9-111 day18-things]$ kubectl  get deploy
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
ashu-mon   0/1     1            0           5s
[ashu@ip-172-31-9-111 day18-things]$ kubectl  get po
NAME                        READY   STATUS              RESTARTS   AGE
ashu-mon-6749cc5448-j2sc8   0/1     ContainerCreating   0          6s
[ashu@ip-172-31-9-111 day18-things]$ kubectl  get po
NAME                        READY   STATUS    RESTARTS   AGE
ashu-mon-6749cc5448-j2sc8   1/1     Running   0          7s
[ashu@ip-172-31-9-111 day18-things]$ kubectl  describe pod ashu-mon-6749cc5448-j2sc8 
Name:             ashu-mon-6749cc5448-j2sc8
Namespace:        ashu-space
Priority:         0
Service Account:  default
Node:             node3/172.31.0.13
```

### creating svc manifest 

```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: ashu-mon
  name: lb1
spec:
  ports:
  - port: 9443
    name: https-based
    protocol: TCP
    targetPort: 9443
  - port: 9000
    name: http-based
    protocol: TCP
    targetPort: 9000
  selector:
    app: ashu-mon
  type: NodePort
status:
  loadBalancer: {}

```

### creating svc

```
[ashu@ip-172-31-9-111 day18-things]$ kubectl  create -f svc.yaml 
service/lb1 created
[ashu@ip-172-31-9-111 day18-things]$ kubectl  get  svc
NAME   TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                         AGE
lb1    NodePort   10.97.210.201   <none>        9443:31560/TCP,8000:32296/TCP   3s
[ashu@ip-172-31-9-111 day18-things]$ 

```

