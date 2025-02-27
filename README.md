# Docker-custom-image

===============================================================================
How to Build customer image in DOCKER 15/02/2025
================================================================================

cd myimages/
[root@test-rhel9 myimages]# ls -lrt
total 4
-rw-r--r--. 1 root root 131 Feb 19 22:02 mycentosdockerfile
[root@test-rhel9 myimages]#
=====================================================================
 cat mycentosdockerfile
FROM centos:latest
LABEL version="v1.0"
LABEL description="my containerzed centos image"
RUN yum update -y
RUN yum install crul -y
[root@test-rhel9 myimages]#
=====================
docker image build -t mycentos:v1 -f mycentosdockerfile .




docker image build --nocache -t mycentos:v1 -f mycentosdockerfile .                    =================>using no cache
====================================================================================
vi mycentosdockerfile

RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

RUN yum -y install java
==================================================================================================
 docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
mycentos      v1        2ebd76b30587   16 seconds ago   467MB
nginx         latest    9bea9f2796e2   2 months ago     192MB
hello-world   latest    d2c94e258dcb   21 months ago    13.3kB

vi mycentosdockerfile1
[root@test1-centos docker]# docker image build -t mycentos:v2 -f mycentosdockerfile1 .
[+] Building 20.8s (9/9) FINISHED                                                                                               docker:default
 => [internal] load build definition from mycentosdockerfile1                                                                             0.0s
 => => transferring dockerfile: 421B                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/centos:latest                                                                          1.5s
 => [internal] load .dockerignore                                                                                                         0.0s
 => => transferring context: 2B                                                                                                           0.0s
 => [1/5] FROM docker.io/library/centos:latest@sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177                    0.0s
 => CACHED [2/5] RUN cd /etc/yum.repos.d/                                                                                                 0.0s
 => CACHED [3/5] RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*                                                        0.0s
 => CACHED [4/5] RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*             0.0s
 => [5/5] RUN yum -y install vim                                                                                                         18.6s
 => exporting to image                                                                                                                    0.6s
 => => exporting layers                                                                                                                   0.6s
 => => writing image sha256:7272a8493fd481603b3bf21b847c92086fdd34963268ba04c34e4c4c85ed7f60                                              0.0s
 => => naming to docker.io/library/mycentos:v2                                                                                            0.0s
[root@test1-centos docker]#



root@test1-centos docker]# docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
mycentos      v1        2ebd76b30587   16 seconds ago   467MB
nginx         latest    9bea9f2796e2   2 months ago     192MB
hello-world   latest    d2c94e258dcb   21 months ago    13.3kB
[root@test1-centos docker]#


docker image build -t myfirstcustomimage:v3 -f mycentosdockerfile2 .


[+] Building 0.8s (9/9) FINISHED                                                                                                docker:default
 => [internal] load build definition from mycentosdockerfile2                                                                             0.0s
 => => transferring dockerfile: 422B                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/centos:latest                                                                          0.7s
 => [internal] load .dockerignore                                                                                                         0.0s
 => => transferring context: 2B                                                                                                           0.0s
 => [1/5] FROM docker.io/library/centos:latest@sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177                    0.0s
 => CACHED [2/5] RUN cd /etc/yum.repos.d/                                                                                                 0.0s
 => CACHED [3/5] RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*                                                        0.0s
 => CACHED [4/5] RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*             0.0s
 => CACHED [5/5] RUN yum -y install java                                                                                                  0.0s
 => exporting to image                                                                                                                    0.0s
 => => exporting layers                                                                                                                   0.0s
 => => writing image sha256:93a8367460779e822f8c4d6da2e4c34111dfb03319973634e3487955ac051751                                              0.0s
 => => naming to docker.io/library/myfirstcustomimage:v3                                                                                  0.0s
[root@test1-centos docker]#


docker image ls
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
mycentos             v2        7272a8493fd4   2 minutes ago   298MB
mycentos             v1        2ebd76b30587   8 minutes ago   467MB
myfirstcustomimage   v3        93a836746077   8 minutes ago   467MB
nginx                latest    9bea9f2796e2   2 months ago    192MB
hello-world          latest    d2c94e258dcb   21 months ago   13.3kB
[root@test1-centos docker]#

docker container run -d -it --name mycustomcentos mycentos:v1

docker container run -d -it --name myfirstimagerun mycentos:v2
docker container run -d -it --name myfirstcustomimage myfirstcustomimage:v3
=================================================================================
23/02/2025 
==================================================================================
cat mycentosdockerfile2

FROM centos:latest
LABEL version="v3.0"
LABEL description="my containerzed centos image"
RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

RUN yum -y install java

EXPOSE 80
CMD ["nginx",  "-g" "daemon off;"]

=============================================================================================================

docker image build -t myfirstcustomimage:v4 -f mycentosdockerfile2 .
docker image build -t myfirstwebimage:v6 -f mycentosdockerfile2 .
docker image build -t myfirstwebimage:v6 -f mycentosdockerfile2 .


[+] Building 5.8s (9/9) FINISHED                                                                                               docker:default
 => [internal] load build definition from mycentosdockerfile2                                                                            0.0s
 => => transferring dockerfile: 467B                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/centos:latest                                                                         5.7s
 => [internal] load .dockerignore                                                                                                        0.0s
 => => transferring context: 2B                                                                                                          0.0s
 => [1/5] FROM docker.io/library/centos:latest@sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177                   0.0s
 => CACHED [2/5] RUN cd /etc/yum.repos.d/                                                                                                0.0s
 => CACHED [3/5] RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*                                                       0.0s
 => CACHED [4/5] RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*            0.0s
 => CACHED [5/5] RUN yum -y install java                                                                                                 0.0s
 => exporting to image                                                                                                                   0.0s
 => => exporting layers                                                                                                                  0.0s
 => => writing image sha256:91988362a6a4abac021c03c54860a3e440de0c412204a737cf24908153ea9418                                             0.0s
 => => naming to docker.io/library/myfirstwebimage:v6                                                                                    0.0s

 1 warning found (use docker --debug to expand):
 - JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 10)
[root@test1-centos docker]#



docker container run -d -it --name myfirstwebimage myfirstwebimage:v6
60878857420f24f54fd4a6e129df550b84fb71488b39a3a3f0217aa9d7252e93


1. docker image build -t mynginxwebimage:v7 -f mycentosdockerfile2 .

 docker image build -t mynginxwebimage:v7 -f mycentosdockerfile2 .
[+] Building 2.0s (9/9) FINISHED                                                                                               docker:default
 => [internal] load build definition from mycentosdockerfile2                                                                            0.0s
 => => transferring dockerfile: 465B                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/centos:latest                                                                         1.8s
 => [internal] load .dockerignore                                                                                                        0.0s
 => => transferring context: 2B                                                                                                          0.0s
 => [1/5] FROM docker.io/library/centos:latest@sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177                   0.0s
 => CACHED [2/5] RUN cd /etc/yum.repos.d/                                                                                                0.0s
 => CACHED [3/5] RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*                                                       0.0s
 => CACHED [4/5] RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*            0.0s
 => CACHED [5/5] RUN yum -y install java                                                                                                 0.0s
 => exporting to image                                                                                                                   0.0s
 => => exporting layers                                                                                                                  0.0s
 => => writing image sha256:413404b0c26d4801db8f0a2cf231a470b72fdd6a42d5c5f4cea8b5a606bb95b0                                             0.0s
 => => naming to docker.io/library/mynginxwebimage:v7                                                                                    0.0s
[root@test1-centos docker]#


docker container run -d -it --name customnginximage mynginxwebimage:v7 .


docker container run -d --name customnginx -P 8080:80 mynginxwebimage:v7 .

docker container run -d -it --name newprodimage1 nginxprod:v10

[root@test1-centos docker]# docker ps -a
CONTAINER ID   IMAGE                     COMMAND                   CREATED          STATUS                        PORTS     NAMES
4b07d7db09ce   nginxprod:v10             "nginx -g 'daemon of…"    3 seconds ago    Created                       800/tcp   newprodimage1
d4b508180094   mynginxwebimagefinal:v8   "nginx -g 'daemon of…"    3 minutes ago    Created                       80/tcp    newprodimage
216190e77e71   mynginxwebimagefinal:v8   "nginx -g 'daemon of…"    5 minutes ago    Created                       80/tcp    prodimage
af37a644cb31   hello-world               "/hello"                  11 minutes ago   Exited (0) 11 minutes ago               infallible_engelbart
645ac048f716   mynginxwebimage:v7        "."                       15 minutes ago   Created                       80/tcp    customnginximage2
2df63f06dafa   mynginxwebimage:v7        "."                       16 minutes ago   Created                       80/tcp    customnginximage1
0a1b0cca88a4   mynginxwebimage:v7        "nginx -g 'daemon of…"    19 minutes ago   Created                       80/tcp    mynginxweb
e5b647ddec0a   mynginxwebimage:v7        "nginx -g 'daemon of…"    19 minutes ago   Created                       80/tcp    mynginxwebimage
f5dcb8b9e7cf   mynginxwebimage:v7        "nginx -g 'daemon of…"    20 minutes ago   Created                       80/tcp    customnginximage
60878857420f   myfirstwebimage:v6        "/bin/sh -c '[\"nginx…"   38 minutes ago   Exited (127) 38 minutes ago             myfirstwebimage
9b42d24817ab   myfirstcustomimage:v3     "/bin/bash"               3 days ago       Exited (0) 3 days ago                   myfirstcustomimage
5e79711518c4   mycentos:v2               "/bin/bash"               3 days ago       Exited (0) 3 days ago                   myfirstimagerun
3ed6dd565840   mycentos:v1               "/bin/bash"               3 days ago       Exited (0) 3 days ago                   mycustomcentos
f2996df88e46   nginx                     "/docker-entrypoint.…"    4 weeks ago      Exited (0) 3 weeks ago                  competent_blackwell
96bd517545ce   hello-world               "/hello"                  4 weeks ago      Exited (0) 4 weeks ago                  reverent_hellman
[root@test1-centos docker]#

docker container ls -l
CONTAINER ID   IMAGE           COMMAND                  CREATED              STATUS    PORTS     NAMES
4b07d7db09ce   nginxprod:v10   "nginx -g 'daemon of…"   About a minute ago   Created   800/tcp   newprodimage1
[root@test1-centos docker]#

docker container inspect newprodimage1


docker container run -d --name nginxportimage -P 8080:800 nginxprod:v10 .





docker image build -t nginxprod -f mycentosdockerfile2 .

[+] Building 1.9s (9/9) FINISHED                                                                                                               docker:default
 => [internal] load build definition from mycentosdockerfile2                                                                                            0.0s
 => => transferring dockerfile: 465B                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/centos:latest                                                                                         1.7s
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 2B                                                                                                                          0.0s
 => [1/5] FROM docker.io/library/centos:latest@sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177                                   0.0s
 => CACHED [2/5] RUN cd /etc/yum.repos.d/                                                                                                                0.0s
 => CACHED [3/5] RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*                                                                       0.0s
 => CACHED [4/5] RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*                            0.0s
 => CACHED [5/5] RUN yum -y install java                                                                                                                 0.0s
 => exporting to image                                                                                                                                   0.0s
 => => exporting layers                                                                                                                                  0.0s
 => => writing image sha256:413404b0c26d4801db8f0a2cf231a470b72fdd6a42d5c5f4cea8b5a606bb95b0                                                             0.0s
 => => naming to docker.io/library/nginxprod                                                                                                             0.0s
[root@test1-centos docker]#

docker container run -d --name nginxportimage -P 8080:800 nginxprod






docker image build -t nginxprodfinal -f mycentosdockerfile2:v1 .
[+] Building 0.1s (1/1) FINISHED                                                                                                               docker:default
 => [internal] load build definition from mycentosdockerfile2:v1                                                                                         0.0s
 => => transferring dockerfile: 2B                                                                                                                       0.0s
ERROR: failed to solve: failed to read dockerfile: open mycentosdockerfile2:v1: no such file or directory
[root@test1-centos docker]# docker image build -t nginxprodfinal:v1 -f mycentosdockerfile2 .
[+] Building 13.3s (9/9) FINISHED                                                                                                              docker:default
 => [internal] load build definition from mycentosdockerfile2                                                                                            0.0s
 => => transferring dockerfile: 467B                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/centos:latest                                                                                         1.5s
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 2B                                                                                                                          0.0s
 => [1/5] FROM docker.io/library/centos:latest@sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177                                   0.0s
 => CACHED [2/5] RUN cd /etc/yum.repos.d/                                                                                                                0.0s
 => CACHED [3/5] RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*                                                                       0.0s
 => CACHED [4/5] RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*                            0.0s
 => [5/5] RUN yum -y install telnet                                                                                                                     11.3s
 => exporting to image                                                                                                                                   0.4s
 => => exporting layers                                                                                                                                  0.4s
 => => writing image sha256:a8514da9099ccc73a1b2b81b0dd36c56cab95419fbbce25ec6804f2ffe954930                                                             0.0s
 => => naming to docker.io/library/nginxprodfinal:v1                                                                                                     0.0s
[root@test1-centos docker]#





