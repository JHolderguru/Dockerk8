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
 docker ps -a #view all containers

#delete container
docker rm 1dd1d07511a9 #or purse in name instead of ID

#remove multiple containers
docker ps -aq #list all containers by ID
docker rm $(docker ps -aq)#pass the argument of all that would have been listed, make sure none of the containers are running unless-docker rm -f $(docker ps -aq)


#verfiy there are no containers left
docker ps -a

# starting a containers and the ports you want

docker run -d -p 3000:80 -p 8083:80 nginx:latest

#starting the container but naming it website

docker run --name website -d -p 3000:80 -p 8083:80 nginx:latest

#Check
docker ps #should see the name as website

#stopping
docker stop website

#Using a format
docker ps --format="ID\t{{.ID}}\nName\t{{.Names}}\nImage\t{{.Image}}\nPorts\t{{.Ports}}\nCommand\t{{.Command}}\nCreated\t{{.CreatedAt}}\nStatus\t{{.Status}}\n"
#expected output
ID      2b639b72a1e2
Name    website
Image   nginx:latest
Ports   0.0.0.0:3000->80/tcp
Command "/docker-entrypoint.…"
Created 2021-03-02 12:58:19 +0000 GMT
Status  Up 14 minutes

#to remove information
export FORMAT="ID\t{{.ID}}\nName\t{{.Names}}\nImage\t{{.Image}}\nPorts\t{{.Ports}}\nCommand\t{{.Command}}\nCreated\t{{.CreatedAt}}\nStatus\t{{.Status}}\n"

#now that the format is stored in the variable

docker ps --format=$FORMAT
#expected output

ID      2b639b72a1e2
Name    website
Image   nginx:latest
Ports   0.0.0.0:3000->80/tcp
Command "/docker-entrypoint.…"
Created 2021-03-02 12:58:19 +0000 GMT
Status  Up 14 minutes

```

#### 3.Volumes - allow us to share Data or folders between conainers. Through the container and the file system we create a volume.


```python



#mounting a index.html folder to the nginx container
docker run --name website -v /$PWD:/usr/share/nginx/html:ro -d -p 8083:80 nginx


#to execute this in a interactive way(get into th container)

docker exec -it website bash
#ls to see the files inside container

ctrl d to exit
#to get into the container with more than just readme files

docker run --name website -v /$PWD:/usr/share/nginx/html: -d -p 8083:80 nginx

#create a file in the container
docker exec -it website bash
#nevigate to the dir
usr/share/nginx/html
#see the container contents in dir
ls
#create new files
touch about.html

# the file should appear in out local directory

#using a bootstrap website
https://startbootstrap.com/theme/grayscale


```

#### 4.Sharing volumes between our website container and a copy container.

```python
docker run --name website-copy --volumes website -d -p 8081:80 nginx

#check containers running
docker ps --format=$FORMAT


```

#### 5.Recreating this using our own image.


open up the IDE and create a docker file in the website folder.

```python
FROM nginx:latest
ADD . /usr/share/nginx/html

# open the CLI
docker build --tag websitelatest .
#the . will look for the contents in that folder and build using the contents of the folder you are in i,e index.htmls
#check the image
docker image ls

#run the container
#make make sure no images are running
docker run --name website -p 8080:80 -d website:latest

#reduce the size by getting an alphine version https://www.alpinelinux.org/about/

docker pull node:alpine

```
