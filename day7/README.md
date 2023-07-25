# k8s-cloud4c-b4

### containerizing webui app to docker image

### source code 

```
[ashu@ip-172-31-9-111 ashu-apps]$ mkdir  day7-app
[ashu@ip-172-31-9-111 ashu-apps]$ cd day7-app/


[ashu@ip-172-31-9-111 day7-app]$ git clone https://github.com/microsoft/project-html-website.git
Cloning into 'project-html-website'...
remote: Enumerating objects: 24, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 24 (delta 0), reused 4 (delta 0), pack-reused 19
Receiving objects: 100% (24/24), 465.86 KiB | 1.78 MiB/s, done.


[ashu@ip-172-31-9-111 day7-app]$ ls
project-html-website
[ashu@ip-172-31-9-111 day7-app]$ 

```

### Adding Dockerfile

```
FROM nginx
LABEL name=ashutoshh
COPY  project-html-website /usr/share/nginx/html/   
```

### .dockerignore 

```
project-html-website/.git
project-html-website/LICENSE
project-html-website/*.md 
```

## build docker image 

```

```
