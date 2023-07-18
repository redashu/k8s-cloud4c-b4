# k8s-cloud4c-b4

## Revision 

<img src="rev.png">

## Taking sample web-ui app source code from github 

```
[ashu@ip-172-31-9-111 ashu-apps]$ ls
java-app  node-app  python-app  ui-app
[ashu@ip-172-31-9-111 ashu-apps]$ cd  ui-app/
[ashu@ip-172-31-9-111 ui-app]$ ls
[ashu@ip-172-31-9-111 ui-app]$ git clone https://github.com/schoolofdevops/html-sample-app.git
Cloning into 'html-sample-app'...
remote: Enumerating objects: 74, done.
remote: Counting objects: 100% (74/74), done.
remote: Compressing objects: 100% (69/69), done.
remote: Total 74 (delta 5), reused 72 (delta 5), pack-reused 0
Receiving objects: 100% (74/74), 1.38 MiB | 28.28 MiB/s, done.
Resolving deltas: 100% (5/5), done.
[ashu@ip-172-31-9-111 ui-app]$ ls
html-sample-app
```
## web ui app dockerfile

```
FROM nginx
# this mean we are refering library image from docker hub
LABEL name=ashutoshh
LABEL email=ashutoshh@delvex.io
# optional keyword but you can write info about your self
# just for help purpose 
COPY html-sample-app /usr/share/nginx/html/ 
```

### lets build  webapp to docker image

```
[ashu@ip-172-31-9-111 ui-app]$ ls
Dockerfile  html-sample-app
[ashu@ip-172-31-9-111 ui-app]$ docker  build -t  ashu-uiapp:v1  . 
Sending build context to Docker daemon  3.629MB
Step 1/4 : FROM nginx
 ---> 021283c8eb95
Step 2/4 : LABEL name=ashutoshh
 ---> Running in bf897e1083a7
Removing intermediate container bf897e1083a7
 ---> 5815b9197f2e
Step 3/4 : LABEL email=ashutoshh@delvex.io
 ---> Running in 038adc3bbc52
Removing intermediate container 038adc3bbc52
 ---> 95308857e1f0
Step 4/4 : COPY html-sample-app /usr/share/nginx/html/
 ---> e66083f54983
Successfully built e66083f54983
Successfully tagged ashu-uiapp:v1
```

### verify image 

```
ashu@ip-172-31-9-111 ui-app]$ docker images
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
karteek-uiapp   v1        1e0ba0352a7a   18 seconds ago   190MB
ashu-uiapp      v1        e66083f54983   23 seconds ago   190MB
mahesh-uiapp    v1        72e0f6337047   25 seconds ago   190MB
nginx           latest    021283c8eb95   13 days ago      187MB
```
## image to container 

<img src="i2c.png">

### creating container and verify it 

```
[ashu@ip-172-31-9-111 ui-app]$ docker run --name ashuuic1 -d  ashu-uiapp:v1  
bee9715364d045c8440c1a48526463a152cfd431dfc3af815b4245e0724979ab

======> verify list 
[ashu@ip-172-31-9-111 ui-app]$ docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS                  PORTS     NAMES
eb93c408e3b8   karteek-uiapp:v1   "/docker-entrypoint.…"   2 seconds ago   Up Less than a second   80/tcp    karteekuic1
bee9715364d0   ashu-uiapp:v1      "/docker-entrypoint.…"   3 seconds ago   Up 2 seconds            80/tcp    ashuuic1
[ashu@ip-172-31-9-111 ui-app]$ 
```

### Resources consumption by container will same app need

```
 45  docker  stats ashuuic1 
   46  docker  stats 
```

## more container operations 

### stop/kill

```
[ashu@ip-172-31-9-111 ui-app]$ docker  kill ashuuic1
ashuuic1
```

### remove container as well

```
[ashu@ip-172-31-9-111 ui-app]$ docker rm  ashuuic1
ashuuic1
```



