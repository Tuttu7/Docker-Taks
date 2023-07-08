#### 1) Run two Apache images with different versions in separate containers and map the container ports to host ports.

#### Pulling httpd images from docker repository. Need to login to the docker reposiriry via the command 'docker login' before this step.

```
docker pull httpd:2.4
docker pull httpd:2.2
```


#### Running 2 containers with different httpd versions 

```
docker run -d -p 8080:80 --name webserver1 httpd:2.4
docker run -d -p 8081:80 --name webserver2 httpd:2.2
```


````

tuttu@techtraining1:~$ docker container ls -a
CONTAINER ID   IMAGE       COMMAND              CREATED              STATUS              PORTS                                   NAMES
e712fb7e9ec3   httpd:2.2   "httpd-foreground"   5 seconds ago        Up 2 seconds        0.0.0.0:8081->80/tcp, :::8081->80/tcp   boring_hermann
a03022b1de40   httpd:2.2   "httpd-foreground"   About a minute ago   Created                                                     webserver2
1ff4bfbde327   httpd:2.4   "httpd-foreground"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp, :::8080->80/tcp   webserver1
````




#### Port mapping 

```
Format :

docker run -d -p <host-port>:<container-port> --name my-apache httpd:<version>

To map container ports to host ports when running an Apache container with Docker, you can use the -p or --publish flag followed by the port mappings.

Replace <host-port> with the port number on your host machine that you want to map, and <container-port> with the corresponding port number exposed by the Apache container. <version> should be replaced with the desired Apache version (e.g., 2.4).

For instance, if you want to map host port 8080 to container port 80 for Apache 2.4, the command would look like this:


docker container run --name webserver1 -d -p 8080:80 webserver1 httpd:2.4
docker container run --name webserver2 -d -p 8081:80 webserver2 httpd:2.2

```

#### 2) Create a docker file to install Nginx and build docker image using that file.

```
tuttu@techtraining1:~$ touch dockerfile

$ cat dockerfile 
# Use the official Nginx base image
FROM nginx

# Copy custom configuration file to the container
COPY nginx.conf /etc/nginx/nginx.conf


tuttu@techtraining1:~$ docker build -t my_ngnix_image .
[+] Building 19.9s (8/8) FINISHED                                                docker:default
 => [internal] load build definition from dockerfile                                       0.3s
 => => transferring dockerfile: 176B                                                       0.1s
 => [internal] load .dockerignore                                                          0.3s
 => => transferring context: 2B                                                            0.1s
 => [internal] load metadata for docker.io/library/nginx:latest                            4.1s
 => [auth] library/nginx:pull token for registry-1.docker.io                               0.0s
 => [internal] load build context                                                          0.1s
 => => transferring context: 31B                                                           0.0s
 => [1/2] FROM docker.io/library/nginx@sha256:08bc36ad52474e528cc1ea3426b5e3f4bad8a13031  10.5s
 => => resolve docker.io/library/nginx@sha256:08bc36ad52474e528cc1ea3426b5e3f4bad8a130318  0.2s
 => => sha256:08bc36ad52474e528cc1ea3426b5e3f4bad8a130318e3140d6cfe29c889 1.86kB / 1.86kB  0.0s
 => => sha256:1bb5c4b86cb7c1e9f0209611dc2135d8a2c1c3a6436163970c99193787d 1.78kB / 1.78kB  0.0s
 => => sha256:021283c8eb95be02b23db0de7f609d603553c6714785e7a673c6594a624 8.15kB / 8.15kB  0.0s
 => => sha256:76579e9ed380849b4d22696f292770963b5ea917a45ef77336a1a0a78 41.46MB / 41.46MB  4.3s
 => => sha256:cf707e2339551222cafe3bf835fddfb859f26bf59058b3487de2b7659309b6b 625B / 625B  0.5s
 => => sha256:91bb7937700d7d3496cf43cb0012e5f818064fecb766bd01041db23c127ab21 959B / 959B  0.6s
 => => sha256:4b962717ba558b7dfabe88c40e20ac86844b132015b66002deac49010cc96be 367B / 367B  0.9s
 => => sha256:f46d7b05649a846d7e24418b6ecea3b1efbdac88d361631e849e9c41917 1.21kB / 1.21kB  1.0s
 => => sha256:103501419a0aecf94398ffcc7404f22931d9b89bbb6021391c2cd4a286f 1.41kB / 1.41kB  1.3s
 => => extracting sha256:76579e9ed380849b4d22696f292770963b5ea917a45ef77336a1a0a782b410b5  4.0s
 => => extracting sha256:cf707e2339551222cafe3bf835fddfb859f26bf59058b3487de2b7659309b6b7  0.0s
 => => extracting sha256:91bb7937700d7d3496cf43cb0012e5f818064fecb766bd01041db23c127ab219  0.0s
 => => extracting sha256:4b962717ba558b7dfabe88c40e20ac86844b132015b66002deac49010cc96be1  0.0s
 => => extracting sha256:f46d7b05649a846d7e24418b6ecea3b1efbdac88d361631e849e9c41917ba776  0.0s
 => => extracting sha256:103501419a0aecf94398ffcc7404f22931d9b89bbb6021391c2cd4a286f37ca9  0.0s
 => [2/2] COPY nginx.conf /etc/nginx/nginx.conf                                            3.8s
 => exporting to image                                                                     0.5s
 => => exporting layers                                                                    0.4s
 => => writing image sha256:d39bc68ff6677f97e0a289ec440c52eb893c47ee8e9fc67e9fec076f291dc  0.0s
 => => naming to docker.io/library/my_ngnix_image


tuttu@techtraining1:~$ docker images
REPOSITORY       TAG       IMAGE ID       CREATED          SIZE
my_ngnix_image   latest    d39bc68ff667   41 seconds ago   187MB

```


