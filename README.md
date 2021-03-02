#### This is a docker and K8 ref file

#### 1. To pull our image we run (either form our git bash or powershell or CLI)
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


```
