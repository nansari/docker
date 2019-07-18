# docker


# Use centos latest image to build web container
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



docker tag d3be43b6c140 docker.io/nansari/centos-httpd:v1.0

Image name shoudl be last option on run command
$ docker run -p 8090:80 centos-httpd:v1.0  &
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message

http://<docker_host_ip>:8090

docker_host_ip is either your docker images or 'docker-machine ip' ( if you are using docker desktop)



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

