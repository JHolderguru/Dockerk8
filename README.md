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
#### 6.Versioning
incase you break a button on your website but still want to revert to the previous version #powerOfVersioning
```python
docker tag auntyb-website:latest auntyb-website:latest

docker tag auntyb-website-1: auntyb-website:1


#run them on different Ports
docker run --name auntyb-website-latest -p 8080:80 -d auntyb-website:latest

docker run --name auntyb-website-1 -p 8081:80 -d auntyb-website:1

#pushing the repos to dockerhub
#1. Log in Dockerhub create a repo and name it.
 docker image ls

#tag the website to the repo name
 docker tag auntyb-website:latest jholderguru/website:latest
 docker tag auntyb-website:1 jholderguru/website:1

 #push images to the hub
 docker push jholderguru/website:latest
 docker push jholderguru/website:1

 #pulling the images from the hub
 docker pull jholderguru/website

#touble shoot
docket inspect (id of container)

docker --help
docker logs (id of container)

other useful commands

docker exec --help

docker exec -it (container Id) /bin/bash(or bin/sh)
```

## K8
#### 1.Pod - runs an application container inside of it, its the smallest unit of k8 and abastraction over container. Each Pod has IP address(app and DB use IP to communicate, these are ephemeral(die easy).When the container crushes or the server runs out of resources the pod dies and a new one is set in placeand a new IP is created but for that  a service  is used.

#### 2.Service and Ingress
#### service is a permanent IP address, life cycle of service and pod are not connected. when pod dies service is still active.
#### Internal service - for DB (does not traverse the internet)
#### External service -- secure protocol(https) and domain name ---> request goes to Ingress does forwarding to the service(also acts as a load balancer).

#### 3.i.e mongo-db-service (adjust URL in the application and push it to the ) - the endpoint is in the configMap: external configuration
```python

DB_URL =mongo-db-service.
mongo-db
mongo-user

mongo-pwd
#password should never be in plain text
#secret base 64 encoded format
#use ENV variables
DB_USER = mongo-user
DB_PWD = mongo-pwd

```

#### 4. Volumes
#### Data storage if the Pod gets restated the Data disappears. The way to make the data stay is Volumes attaches a physical storage on a hard drive to your Pod and that storage can be on a local machine or remote storage (outside the K8 cluster i.e cloud  or on prem (external ref) then everytime the cluster is restarted the data is loaded)(k8 doesnt manage the data persistance)

#### 5. Deployment -  have a replica that has the same so that if one goes down, there is one that has the same functionality, you just specify in the #blueprints for how many number of replicas.

#### so Pod is a layer of abstraction, on top of containers. Deployment is another abstraction on top of pods making it convenient to interact with the pods and replicate them and do some other configuration.

#### the DB cannot be replicated using a deployment.StatefulSet is meant for some applications like  databases. Takes care of DB consistency and scaling the db.

#### best practice is to always have DB hosted outside the K8 cluster.

#### 6. Install minikube and kubectl.

#### minikube -open source tool- one node cluster where the master processes and worker processes run on one Node. It runs through a virtual box (one node k8 cluster that runs for testing purposes )

#### cmdline
```python

#get the  start going
minikube start

kubectl help

kubectl get nodes
#expected output
minikube ready master

#check pods
kubectl get pods

#list components
kubectl get services

#kubectl create nginx Deployment
kubectl create deployment nginx-depl --image=nginx

kubectl get pod
#should be up and running
```
#### 7.Debugging pods

#### creating the database
```python

kubectl create deployment mongo-depl --image=mongo

kubectl describe pod [podname]

kubectl get pod
#will lit the pod ids

kubectl exec -it #podnameID  -- bin/bash 
#then you will be inside the pod to get a better debugging
```
