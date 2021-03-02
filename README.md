#### This is a docker and K8 ref file

#### 1. To pull our image and run we run (either form our git bash or powershell or CLI)
```python
#pull the image from the hub
docker pull nginx

#check for images and tags
docker images

#docker run
docker run nginx:latest

#open a separate command to view the run time
docker container ls

#running the container in detached mode
docker run - d nginx:latest

#Check which containers running
docker ps

#to stop
docker stop 73fe3df544a7 #ID of the machine

#check status
docker ps

```
#### 2.Using the container local host  8080 to map it to the container

```python

docker run -d -p 8080:80 nginx:latest
#or
docker run -d -p 8080:83 nginx:latest

#or
docker run -d -p 3000:80 nginx:latest

#or map two ports
docker run -d -p 3000:80 -p 8083:80 nginx:latest

```

#### 3. Managing your containers, Stop remove start and Name your containers

```python

#Stop
docker stop 1dd1d07511a9 #or name of container

#start
docker start 1dd1d07511a9 #or name of container

 #for more information running and non running containers
 docker --help

#delete container
docker rm 1dd1d07511a9 #or purse in name instead of ID

```
