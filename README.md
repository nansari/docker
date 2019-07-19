# docker
How to build a simple web containe, test it and push it on hub.docker.com

## install
Install docker package on Linux and Docker Desktop on Windos

## Build a web a container
Let's get a centos latest official container, install Apache httpd inside, crease a web server index.html to show some text in blue color.

Create a directory, say ```builddir``` and create a file Dockerfile with below text
```
# Either of below line works
#FROM centos:latest
FROM docker.io/centos:latest

MAINTAINER Nasimuddin Ansari

# Install httpd
RUN yum -y install httpd net-tools && \
    yum -y update && \
    yum clean all

# Create index.html for default web server
RUN echo '<html><h1 style="color:blue;">xxxxxBlue Docker Containerized Web Server</h1></html>' > /var/www/html/index.html

# Tiis container will expose itself of port 80
EXPOSE 80

ENTRYPOINT [ "/usr/sbin/httpd" ]

CMD [ "-D", "FOREGROUND" ]

RUN rm -rf /var/cache/yum
```

Build container
```
$ cd builddire
$ docker build -t centos-httpd:v1.0 .  # There is a ending dot
Sending build context to Docker daemon  43.52kB
Step 1/8 : FROM docker.io/centos:latest
 ---> 9f38484d220f
Step 2/8 : MAINTAINER Nasimuddin Ansari
 ---> Using cache
 ---> dbf198a1ee43
Step 3/8 : RUN yum -y install httpd net-tools &&     yum -y update &&     yum clean all
 ---> Using cache
 ---> c5f095791041
Step 4/8 : RUN echo '<html><h1 style="color:blue;">New container Blue Docker Containerized Web Server</h1></html>' > /var/www/html/index.html
 ---> Running in dd56bd516138
Removing intermediate container dd56bd516138
 ---> daf52640fa8c
Step 5/8 : EXPOSE 80
 ---> Running in 366715a7886a
Removing intermediate container 366715a7886a
 ---> c43e8f4632d8
Step 6/8 : ENTRYPOINT [ "/usr/sbin/httpd" ]
 ---> Running in 228533928b8a
Removing intermediate container 228533928b8a
 ---> 1da60362476c
Step 7/8 : CMD [ "-D", "FOREGROUND" ]
 ---> Running in e2731eb5e8c6
Removing intermediate container e2731eb5e8c6
 ---> 23486962641e
Step 8/8 : RUN rm -rf /var/cache/yum
 ---> Running in 00b718f75bae
Removing intermediate container 00b718f75bae
 ---> 758aa718dfcc
Successfully built d3be43b6c140
Successfully tagged centos-httpd:v1.0
$ 
```

Let's run newly created docker image. 
- image name should be last option of below run command
- ```-d``` detached running image from terminal; run in background
- ```-p```  docker_host_ip:container port
- docker_host_ip is either your docker images or 'docker-machine ip' ( if you are using docker desktop)
```
$ docker run -d -p 8090:80 centos-httpd:v1.0  
9467a59476952beaf33c04a6850432e771b12c2a1b79cccab8182aa50372eb8d
```

Newly build web image is running as docker container. Check running container details
```
$ docker ps
CONTAINER ID        IMAGE                       COMMAND                  CREATED              STATUS              PORTS                  NAMES
9467a5947695        centos-httpd:v1.0   "/usr/sbin/httpd -D …"   About a minute ago   Up About a minute   0.0.0.0:8090->80/tcp   determined_stallman
```

Access you web container on http://<docker_host_ip>:8090  (something line http://192.168.0.100:8090/)

See what is running inside contaer and finally stop it.

```
```




docker tag d3be43b6c140 docker.io/nansari/centos-httpd:v1.0





$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: nansari
Password: 

WARNING! Your password will be stored unencrypted in /home/nansari/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

$ docker push docker.io/nansari/centos-httpd:v1.0
The push refers to repository [docker.io/nansari/centos-httpd]
19f8fe399d3d: Pushed 
57c9d535f019: Pushed 
b2a0881e9f7c: Pushed 
d69483a6face: Mounted from library/centos 
v1.0: digest: sha256:38cacfbbfc1889e1a53cc6cb40b76067140c9198f4e725af2593a81dac6ac127 size: 1155


https://hub.docker.com/r/nansari/centos-httpd

