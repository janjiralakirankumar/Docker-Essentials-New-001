# Docker Essentials Lab Cheat Sheet

### Docker Labs Pre-requisites
1. Basic understanding of Linux Commands.
2. Basic knowledge of a Cloud platform such as AWS.
3. Good to have an AWS-Free Tier Account for Practice.

## Lab-1: Creating an EC2 Instance in AWS and Installing Docker

To begin with Lab-1, log in to AWS Console.

### Task-1: Installing Docker on Ubuntu 20.04 operating system

* Manually Launch a `t2.micro` instance with OS version as `Ubuntu 22.04 LTS` in North Virginia (us-east-1) Region.
* Use tag "`Name:Docker-Server`"
* Create a new Keypair with the Name `Docker-Keypair-YourName`
* In security groups, include ports `22 (SSH)`  `80 (HTTP)` `443 (HTTPS)` and `8080 (custom TCP port)`
* Configure Storage: 10 GiB
* Launch the Instance.
* Once Launched, Connect to the Instance using `MobaXterm` or `Putty` with username "`ubuntu`".

Once the EC2 is ready, follow the below Commands to perform lab:

Switching to super user and setting up a host name
```
sudo hostnamectl set-hostname DockerServer
bash
```
```
sudo su
```
Updating the packages
```
apt update -y
```
Installing the packages
```
apt install curl -y
```
Connecting to url
```
curl -SSL https://get.docker.com/ | sh
```
Checking the status of the docker
```
service docker status
```
If the service is not active, then we need to start the service
```
service docker start
```
To add ubuntu user to docker group, if you are not working as the root user
```
usermod -aG docker ubuntu
```
Checking the version of the docker
```
docker --version
```
#### =========================END of LAB-01=========================

## Lab 2: Basic Docker Commands

### Task 1: Creating your first Docker container

```
docker run hello-world
```
### Task 2: Basic Commands to run the Container in Interactive mode

Will check the image in local repository if not found it will directly pull th image from dockerhub
```
docker pull ubuntu
```
To check the image present in local repository
```
docker image ls
```
Running ubuntu container, renaming it has ct1 and getting into interactive terminal mode
```
docker run -it --name ct1 ubuntu
```
creating new files f1 f2 f3
```
touch f1 f2 f3
```
checking the files
```
ls
```
Getting exited from container
```
exit
```
checking containers which are in running state
```
docker ps
```
shows all the containers even if its not running
```
docker ps -a
```
Running ubuntu container and renaming it has ct2
```
docker run -it --name ct2 ubuntu
```
Press Crtl+P+Q to switch the terminal to Docker Host.

checking containers which are in running state
```
docker ps
```
Getting into secondary shell(exec) of ct2 container
```
docker exec -it ct2 /bin/sh
```
```
exit
```
checking containers which are in running state
```
docker ps
```
Getting attached to the shell of ct2 container
```
docker attach ct2
```
```
exit
```
checking containers which are in running state
```
docker ps
```
shows all the containers even if its not running
```
docker ps -a
```
### Task 3: Port Mapping from Docker Host to container

Running httpd container in background and not getting attached to the shell (-d) and port mapping(-p)
```
docker run -d -p 80:80 httpd
```
Checking containers which are in running state
```
docker ps
```
Getting into secondary shell(exec)
```
docker exec -it replace container id/name /bin/bash
```
```
exit
```
Checking containers which are in running state
```
docker ps
```
Getting into secondary shell(exec)
```
docker exec -it replace container id/name /bin/bash
```
Terminating your container
```
kill 1
```
Shows all the containers even if its not running
```
docker ps -a
```

To stop the container
```
docker container stop replace container id/name 
```
To Remove the container
```
docker container rm  replace container id/name 
```
To check the image in local repository
```
docker image ls
```
To Remove the image
```
docker image rm replace image id 
```
#### =========================END of LAB-02=========================

## Lab 3: Docker container life cycle 

### Task 1: Docker Lifecycle 

Will check the image in local repository if not found it will directly pull th image from dockerhub
```
docker pull httpd
```
To check the image in local repository
```
docker image ls
```
To check the image history
```
docker image history httpd
```
Creating an httpd container
```
docker container create httpd
```
shows all the containers even if its not running (Note:similar to docker ps -a)
```
docker container ls -a
```
Starting your container
```
docker container start <replace container id/name>
```
checking containers which are in running state(Note:similar to docker ps)
```
docker container ls
```

Stopping your container
```
docker container stop <replace container id/Name>
```
shows all the containers even if its not running (Note:similar to docker ps -a)
```
docker container ls -a
```
Starting your container
```
docker container start <replace container id/Name>
```
pausing your container
```
docker container pause <replace container id/Name>
```
shows all the containers even if its not running (Note:similar to docker ps -a)
```
docker container ls -a
```
unpausing your container
```
docker container unpause <replace container id/Name>
```
shows all the containers even if its not running (Note:similar to docker ps -a)
```
docker container ls -a
```
Getting into secondary shell(exec)
```
docker exec -it <replace container id/name> bash
```
To move to htdocs directory
```
cd htdocs
```
Updating the packages and installing the packages
```
apt update && apt install wget -y
```
Removing index.html
```
rm index.html
```
Fetching file 
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/docker-essentials/index.html
```
exiting from the shell
```
exit
```
To save the changes we made in the container and create a new image(myhttpd:version1) from modified container
```
docker commit <replace container id/name> myhttpd:version1
```
check all the images in local repository
```
docker image ls
```
Run myhttpd:version1 container in detach mode(-d) and do port mapping -p (host port:8080 to container port:80)
```
docker run -d -p 8080:80 myhttpd:version1
```
specify your public ip and port and check in browser (Note:If you are using killerkoda just go to traffic and ports and click on 8080)
```
curl <public IP>:8080
```
checking containers which are in running state(Note:similar to docker ps)
```
docker container ls
```
To check the activity of the container
```
docker logs <replace container id/name>
```
To check resource consumed by your containers
```
docker stats <replace container id/name>
```
checking containers which are in running state(Note:similar to docker ps)
```
docker container ls
```
To stop your container
```
docker stop <replace container id/name>
```
To remove your container
```
docker container rm <replace container id/name>
```
check all the images in local repository
```
docker image ls
```
To remove your image
```
docker image rm <replace image id/name >
```
check all the images in local repository
```
docker image ls
```
```
docker image ls -a
```
#### =========================END of LAB-03=========================

## Lab 4: Working with volume mounts in Docker

### Task 1: Starting Docker Containers Bind Mounts

Creating a share directory
```
mkdir /home/ubuntu/share
```
Adding the text(Hello From Docker Host) to the share directory which we created
```
echo 'Hello From Docker Host' > /home/ubuntu/share/index.html
```
Running ubuntu container and renaming it has container1 in interactive mode((-it) means getting into primary shell) and mounting source location to destination location
```
docker run -it --name container1 -p 80:80 -v /home/ubuntu/share:/var/www/html ubuntu:18.04 /bin/bash
```
Updating all the packages and installing apache2
```
apt-get update && apt-get install apache2 -y
```
starting apache2
```
service apache2 start
```
checking apache2 status
```
service apache2 status
```
Adding the text(Hello From Container1) to /var/www/html/index.html
```
echo 'Hello From Container1' > /var/www/html/index.html 
```
Press Ctrl+P+Q, to switch back to Host

Running ubuntu container and renaming it has container2 in interactive mode((-it) means getting into primary shell) and mounting source location to destination location
```
docker run -it --name container2 -v /home/ubuntu/share:/var/www/html ubuntu:18.04
```
Adding the text(Hello From Container2) to /var/www/html/index.html
```
echo 'Hello From Container2' > /var/www/html/index.html 
```
```
exit
```
shows all the containers even if its not running
```
docker ps -a
```
Removing the container forcefully(-vf)
```
docker rm -vf container1 container2 
```
### Task 2: Create a bind mount with --mount option and verify it

Running nginx container in detached mode(-d) and renaming it has newbind01 and mounting source location to destinatin location (here we using --mount option)
```
docker run -d --name newbind01 --mount type=bind,source=/home/ubuntu/share/,target=/app nginx:latest
```
Detailed description about newbind01
```
docker inspect newbind01 | grep -i /app
```
#### =========================END of LAB-04=========================

## Lab 5: Volume Mounting with Docker Containers
 
### Task 1: Creating a new docker volume and inspecting containers

Creating a new volume with the name ct-volume1
```
docker volume create ct-volume1
```
checking your volume
```
docker volume ls
```
Detailed description about ct-volume1
```
docker volume inspect ct-volume1
```

### Task 2: Launching a Nginx container mapped to a specific docker volume and verification

Running nginx container in detached mode(-d) and renaming it has nginx-container and mounting source location to destinatin location (here we using --mount option)
```
docker run -d -p 80:80 --name=nginx-container --mount src=ct-volume1,dst=/usr/share/nginx/html nginx
```
shows all the running containers
```
docker ps
```
Detailed description about nginx-container
```
docker container inspect nginx-container
```
list
```
ls /var/lib/docker/volumes/ct-volume1/_data/ 
```
getting into directory
```
cd /var/lib/docker/volumes/ct-volume1/_data/ 
```
creating files f1 f2 f3
```
touch f1 f2 f3
```
Getting into editor mode(vi)
```
vi /var/lib/docker/volumes/ct-volume1/_data/index.html
```

### Task 3: Deleting container and attaching the volume to another container

Stopping your nginx-container
```
docker container stop nginx-container
```
Removing your nginx-container
```
docker rm container nginx-container
```
shows all the containers even if its not running
```
docker ps -a
```
Running busybox container renaming it has busybox-container1 and getting to shell in interactive mode(-it) and mounting source location to destination location
```
docker run -it --name busybox-container1 --mount source=ct-volume1,target=/data busybox sh
```
list
```
ls
```
Getting into /data directory
```
cd /data
```
list
```
ls
```
exiting from the shell
```
exit
```
stopping busybox-container1
```
docker stop busybox-container1
```
Removing  busybox-container1
```
docker rm busybox-container1
```
shows all the containers even if its not running
```
docker ps -a
```
shows all the volumes
```
docker volume ls
```
Remove ct-volume1
```
docker volume rm ct-volume1
```
shows all the volumes
```
docker volume ls
```

### Task 4: Create a container with tmpfs mount and verify it

Running nginx container and renaming it has tmpmount and mounting source location to your destination location
```
docker run -d --name tmpmount --mount type=tmpfs,destination=/app nginx:latest
```
Detailed description of  tmpmount
```
docker container inspect tmpmount
```
Getting into secondary shell(exec)
```
docker exec -it tmpmount bash
```
Getting into /app directory
```
cd /app
```
creating a file called abc.txt
```
touch abc.txt
```
list
```
ls
```
exiting from the shell
```
exit
```
stopping your tmpmount
```
docker stop tmpmount
```
starting your tmpmount
```
docker start tmpmount
```
Getting into secondary shell(exec)
```
docker exec -it tmpmount bash
```
Getting into /app directory
```
cd /app
```
list
```
ls
```
see your files are not here.....
#### =========================END of LAB-05=========================

## Lab 6: Docker Networking 

### Task 1: Create a new docker bridge and check connectivity between containers of same bridge

list all the network in you host
```
docker network ls
```
Creating a new network bridge network with the name ct-bridge1
```
docker network create --driver bridge ct-bridge1
```
Details about your  ct-bridge1
```
docker network inspect ct-bridge1
```
list all the network in you host
```
docker network ls
```
Running busybox container and renaming it has ct-c1 and connecting it to ct-bridge1 network
```
docker run -it --network ct-bridge1 --name=ct-c1 busybox
```
Press Ctrl+P+Q, to switch back to Host

Running busybox container and renaming it has ct-c2 and connecting it to ct-bridge1 network
```
docker run -it --network ct-bridge1 --name=ct-c2 busybox
```
Press Ctrl+P+Q, to switch back to Host

Details about your  ct-bridge1
```
docker network inspect ct-bridge1
```
```
docker ps
```
To attach to your container ct-c2
```
docker attach ct-c2
```
```
ip addr
```
To ping to ct-c1
```
ping -c 5 ct-c1
```
Press Ctrl+P+Q, to switch back to Host

### Task 2: Create a new docker bridge and check connectivity between containers of different bridges

Creating a new network bridge network with the name ct-bridge2
```
docker network create --driver bridge ct-bridge2
```
Running busybox container and renaming it has ct-c3 and connecting it to ct-bridge2 network
```
docker run -it --network ct-bridge2 --name=ct-c3 busybox
```
Press Ctrl+P+Q, to switch back to Host

Running busybox container and renaming it has ct-c4 and connecting it to ct-bridge2 network
```
docker run -it --network ct-bridge2 --name=ct-c4 busybox
```
Press Ctrl+P+Q, to switch back to Host

To attach to your container ct-c4
```
docker attach ct-c4
```
To ping to ct-c3
```
ping -c 5 ct-c3
```
```
ip addr
```
To ping to ct-c1
```
ping -c 5 ct-c1
```
To ping to ct-c2
```
ping -c 5 ct-c2
```
Press Ctrl+P+Q, to switch back to Host


### Task 3: Using 'Docker network connect' command create a successful connection between containers of different bridges

list all the network in you host
```
docker network ls
```
To connect ct-c1 container to ct-bridge2
```
docker network connect ct-bridge2 ct-c1
```
Details about your  ct-bridge2
```
docker network inspect ct-bridge2
```
To attach to your container ct-c1
```
docker attach ct-c1
```
To ping to ct-c4
```
ping -c 5 ct-c4
```
```
ip addr
```
```
ip route
```

### Task 4: Launch a container to host network

Run container busybox and rename it has ct-c5 and choose host network
```
docker run -it --network host --name=ct-c5 busybox
```
```
ip addr
```
```
ifconfig
```
Press Ctrl+P+Q, to switch back to Host

check the name of your network
```
docker network inspect host
```

### Task 5: Launch a container to none network 

Run container busybox and rename it has ct-c6 and select type has None
```
docker run -it --network none --name=ct-c6 busybox
```
```
ip addr
```
Press Ctrl+P+Q, to switch back to Host

To check your network name
```
docker network inspect None
```

#### =========================END of LAB-06=========================

## Lab 7: Building a Dockerfile to setup an Ubuntu container with WordPress application

### Task 1: Deploying MySQL and WordPress containers

Create wordpress directory
```
mkdir wordpress
```
Getting into wordpress
```
cd wordpress
```
Editor mode(vi) and getting into Dockerfile
```
vi Dockerfile
```
Content of Dockerfile to paste
```
FROM ubuntu:20.04
MAINTAINER ADMIN "admin@cloudthat.com"
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
apt-get -q -y install apache2 \
php7.4 \
php7.4-fpm \
php7.4-mysql \
libapache2-mod-php7.4
ADD http://wordpress.org/latest.tar.gz /tmp
RUN tar xzvf /tmp/latest.tar.gz -C /tmp  \
&& cp -R /tmp/wordpress/* /var/www/html
RUN rm /var/www/html/index.html && \
chown -R www-data:www-data /var/www/html
EXPOSE 80
CMD ["/usr/sbin/apache2ctl","-D","FOREGROUND"]

#End of Dockerfile
```
#### You can download the above Dockerfile from S3 using - wget https://hpe-content.s3.ap-south-1.amazonaws.com/Dockerfile

Creating a new image wordpress:v1
```
docker build -t ct-wordpress:v1 .
```
list of all images
```
docker image ls
```
Creating a new network called ct-bridge and network type bridge
```
docker network create --driver bridge ct-bridge 
```
It contains all the details of your wordpress
```
docker run -d --network ct-bridge --name mysql -e MYSQL_DATABASE=wordpress -e MYSQL_USER=admin -e MYSQL_PASSWORD=password -e MYSQL_ROOT_PASSWORD=password mysql:5.7
```
Shows all the containers which are Running
```
docker ps 
```
Running ct-wordpress:v1 container attached to ct-bridge network
```
docker run -d --network ct-bridge -p 80:80 ct-wordpress:v1
```
Shows all the containers which are Running
```
docker ps
```
#### =========================END of LAB-07=========================
