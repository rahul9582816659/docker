# Docker

Docker Client -> Docker Server or Docker Deamon

docker run hello-world
1. The Docker client contacted the Docker daemon.

2. The Docker daemon pulled the "hello-world" image from the Docker Hub, 
   first it looks into image cache when it dont find there than it go to 
   docker hub, download & store to image cache.

3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
    So containe ris an instance of image.

4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.


Container:
----------
chrome dependency on python v2 and nodejs dependency on pyhton v3 -> make system call -> request kernel -> CPU MEMORY HARDDRIVE Network

Now let suppose we have senerio where on our local machine we want above to run but we can have 1 python version at a time.

Namespace will help in this case:
chrome dependency on python v2 and nodejs dependency on pyhton v3 -> make system call -> request kernel -> CPU MEMORY Network HARDDRIVE(python v2 and python v3 with different namesapce)
so that when chrome needs to run kernel will look for namespace and give python v2.

Namespace: Isolating resource per process
Control Group: Limit amount of resources used per process

Note : Namespacing & Control Group are limited to linux os.
	   Mac and Windows when we install docker it install linux vm.

chrome dependency on python v2  -> make system call -> request kernel -> CPU MEMORY Network HARDDRIVE (python v2) : Container 1
nodejs dependency on pyhton v3 -> make system call -> request kernel -> CPU MEMORY Network HARDDRIVE (python v3) : Container 2

So container is process or set of processes having grouping of resource specifaclly assign to it.

Image : Having 2 section FileSystem Snapshot & Startup Command
for ex: Image : chrome & python as FS Snapshot and > run chrome as Startup Command

Now when instance of image ie conatiner is created :
Conatienr : running process (chrome) -> Kernel -> RAM NETWORK CPU HARDDRIVE(chrome & python)

Commands:
---------
1. Creating & Running a conatiner from a image:

docker run image-name : docker (refrence to docker client) run (try to create & run container) image-name(name of image to use for this conatiner)
ex: docker run hello-world

docker run image-name command: docker (refrence to docker client) run (try to create & run container) image-name(name of image to use for this conatiner) command (override default command)
ex: docker run busybox echo Rahul Choudhary

2. Listing ALl Running Container:

docker ps : docker (refrence to docker client) ps (listing all running conatiners)
ex: docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
e3c77cfce79c        busybox             "ping google.com"   10 seconds ago      Up 8 seconds                            hungry_feistel

docker ps --all : list all running and previously run

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                         PORTS               NAMES
e3c77cfce79c        busybox             "ping google.com"        2 minutes ago       Exited (0) 37 seconds ago                          hungry_feistel
5bbb51fbe2a0        busybox             "ls"                     8 minutes ago       Exited (0) 8 minutes ago                           modest_vaughan
f30e8b6c5f8f        busybox             "echo Rahul Choudhary"   9 minutes ago       Exited (0) 9 minutes ago                           angry_varahamihira
eac1f3751bc5        busybox             "echo 'Rahul Choudha…"   9 minutes ago       Exited (0) 9 minutes ago                           hungry_hertz
0df5942d55e3        busybox             "echo rahul"             26 minutes ago      Exited (0) 26 minutes ago                          peaceful_tharp
b538b3f7fe7c        busybox             "sh"                     26 minutes ago      Exited (0) 26 minutes ago                          hungry_taussig
e43d8295f53d        busybox             "sh"                     26 minutes ago      Exited (0) 26 minutes ago                          elated_swartz
236e54089db2        hello-world         "/hello"                 About an hour ago   Exited (0) About an hour ago                       flamboyant_banzai


docker run = docker create + docker start
docker create image-name : return container id
docker start conatiner-id

docker create hello-world
f410280f98a6c24a4c4f80a21533945b6b511102ee4cd316c7cba5c94eab8130

 docker start f410280f98a6c24a4c4f80a21533945b6b511102ee4cd316c7cba5c94eab8130
f410280f98a6c24a4c4f80a21533945b6b511102ee4cd316c7cba5c94eab8130

docker start -a f410280f98a6c24a4c4f80a21533945b6b511102ee4cd316c7cba5c94eab8130
Hello from Docker!

Note : -a will attach the output to the terminal

3. Remove docker images:
docker system prune
docker container prune
docker image prune
docker volume prune
docker network prune

WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all dangling images
        - all dangling build cache

4. Get Logs From Container:
docker logs container-id

5. Stop a Container
docker stop conatiner-id : we can perform clean up activity
docker kill conatiner-id : immedeatly kill the container without performing clean up activity

Note : when we run stop command it will wait for 10 seconds & if still container is running then kill will be issued.

6. Execute an additional command in a conatiner:
docker exec -it conatiner-id command : exec(run another command) -it(allow us to provide input to the conatiner) 

ex:
docker ps 
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
8a979cbd8e11        redis               "docker-entrypoint.s…"   4 minutes ago       Up 4 minutes        6379/tcp            vigilant_bell
[rahul@localhost 02 Manipulating Containers with the Docker Client]$ docker exec -it 8a979cbd8e11 redis-cli
127.0.0.1:6379> set name 'rahul choudhary'
OK
127.0.0.1:6379> get name
"rahul choudhary"
127.0.0.1:6379> 

Note: Every Process like redis-cli have STDIN STDOUT STDERROR, STDIN(stuff we type and provide to redis-server using redis-cli) STDOUT(stuff that shows up on the screen)

docker exec -it 8a979cbd8e11 sh : we are in shell and can execute redis-cli directly
#redis-cli
127.0.0.1:6379> set name 'rahul choudhary'
OK
127.0.0.1:6379> get name
"rahul choudhary"
127.0.0.1:6379> 


Creating Docker Images:
-----------------------
Dockerfile -> Docker Client -> Docker Server -> Usable Image

Dockerfile : Configuration to define how our container should behave

creating docker file:
1. specify base image
2. run some commands to install additional program
3. specify a command to run on container startup

creating image for redis server:
# use an existing docker image as a base
FROM alpine

# download and install dependency
RUN apk add --update redis

# tell image what to do when it start container
CMD ["redis-server"]

docker build . : in directory where Dockerfile is present
docker build . : build (take Dockerfile & give to Docker CLI) . (build context specify the directory of files/folder use for build)

writing Dockerfile == Being given a computer with no OS and being told to install chrome.

install operating system : specify a base image
startup your default browser + navigate to chrome.google.com + download installer + execute chrome installer : run command to install additional program
execute chrome : command to run on startup 

7. Tagging Docker Image

docker build -t your-docker-id/repo OR projectname : version  
docker build -t thecrazzyrahul/redis-server:latest .

Note: docker build . always look for Dockerfile, if Dockerfile is with some extension like Dockerfile.dev
      than we can run docker build -f Dockerfile.dev . : -f to give file name

8. Copy Files To Container:
copy from-local-machine to-container
ex: copy ./ ./ : relative to build context

9. Netwrok / PORT Access:
container can make request to outside world
outside world cant make request to container
we have to manually map the port from outside world to inside the container

Docker run with port mapping:
docker run -p 4200:8080 image-id : 4200 incoming request on localhost 8080 route to 8080 post inside container

ex: docker run -p 4200:80 --rm web

10. Work Directory
WORKDIR directory-path : work-dir will be used for all action like copy (data will be copied to work dir)

ex: in below data will be copied to /usr/share/nginx/html when we tell that build context is .
WORKDIR /usr/share/nginx/html
COPY dist/ . 

11. Angular Node Modules:
do we need to run npm install again & again in doecker file, every time we built the project?

using below only when any dependency is chnaged than only npm install will run otherwise cache will be used
COPY ./package.json ./
RUN npm install
COPY ./ ./

Create WebAPP :
---------------
Angular project is created & Dockerfile is:

FROM nginx:alpine
COPY nginx.conf /etc/nginx/nginx.conf
WORKDIR /usr/share/nginx/html
COPY dist/ .

build the angular project: ng build --prod & make sure dist will have all the files which need to copy to nginx server

Docker Compose:
---------------
docker compose is having buch of docker cli commands.
we dont need to input docker cli command again & again.
just add to docker compose and run. 

docker compose can be used to start multiple docker conatiners.

docker-compose.yml --> feed to docker-compose cli --> 

in docker-compose we have servivces which is likely a container:
ex: docker-compose.yml having 2 services redis & node

version: '3'             version of docker-compose
services:     
  redis-server:          name of service
    image: 'redis'       use the image redis to create this service/container
  node-app:              name of service
    build: .             this means look in current directory for Dockerfile
    ports:
      - "4200:8080"      - indicates we can use array, local maching port: conatiner port


Now to make cponnection between 2 services like Redis & Node
in client application : where we make connection : host will be service name 
ex: 'http://localhost:8161' will be 'active-mq' the service name.
i wonder it would be like : 'active-mq:port'

Docker Compose For Angular Project:
------------------------------------
docker-compose.yml:

version: '3.1'
services:
    web:
        build: '.'
        ports:
            - 4200:80

run: docker-compose up
     docker-compose up --build : this will rebuild if we have any changes in the image 
     docker-compose up -d : launch in background

     docker-compose down : stop container

ex:
[rahul@localhost web]$ docker-compose up -d
Starting web_web_1 ... done
[rahul@localhost web]$ docker ps -all
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
28374d747b79        web_web             "nginx -g 'daemon of…"   15 minutes ago      Up 2 seconds        0.0.0.0:4200->80/tcp   web_web_1
[rahul@localhost web]$ docker-compose down
Stopping web_web_1 ... done
Removing web_web_1 ... done
Removing network web_default
[rahul@localhost web]$ docker ps -all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[rahul@localhost web]$ 


Error/Exception Occur inside Docker Conatiner:
---------------------------------------------
if any error occur in server, server will crash and the container will stop

Automatic Conatiner Restart:

status code : 0 we exit and everything is OK, 1,2,3,,etc we exit because something went wrong.

we have below restart policy
'no' : never atempt to restart this conatiner if it stops or crashes
always : if this container stops for any reason always attempt ot restart it
on-faliure : only restart if container stops with error code
unless-stopped : always restart unless we force fully stop it

Note: 'no' will be in single/double quote as no is false in docker-compose.yml file


docker-compose.yml: added restart policy to node-app

version: '3'             version of docker-compose
services:     
  redis-server:          name of service
    image: 'redis'       use the image redis to create this service/container
  node-app:              name of service
    restart: always      restart policy is always
    build: .             this means look in current directory for Dockerfile
    ports:
      - "4200:8080"      - indicates we can use array, local maching port: conatiner port

Status Using Docker Compose:
----------------------------
docker ps
docker-compose ps : but this will be run only in directory where we have docker-compose.yml file otherwise it will throw error
                    it looks for docker-compose.yml file and find the conatiner and show the status.


Development Workflow:
---------------------
Development --> Testing --> Deployment
    |                           | 
    ----<----------<-------<-----

Creating React App:
-------------------
sudo npm install -g create-react-app
create-react-app react-web

npm run start : starts up the devlopment server
npm run test : runs tests associated with the project
npm run build : build production version of application


Dockerfile.dev:

FROM node:alpine
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "run", "start"]

docker build -f Dockerfile.dev .
sudo docker run -p 3000:3000 2c388dd9f2fa

Docker Volume:
--------------
used to map folder inside the container to filder outside the conatiner

sudo docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app 2c388dd9f2fa

-v $(pwd):/app - map current working dir to /app inside conatiner
but in current working dir we dont have node_modules as we deleted that in previous steps
but we have node_modules inside container

-v /app/node_modules - this will tell that use node_modules which is inside the conatiner

shorthand above using docker compose: docker-compose.yml file

version: '3'
services:
  web:
    restart: always
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - /app/node_modules
      - .:/app

Multi Step Docker Build:
------------------------
Build Phase:
  1. use node:alpine
  2. copy package.json file
  3. install dependencies
  4. run 'npm run build'
Run Phase:
  1. use nginx
  2. copy over the result of npm run build
  3. start nginx

Dockerfile: for Prod Build and Run Phase

FROM node:alpine as builder
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html

Services Overview:
------------------
Github
Travis CI
AWS

Github:
------
create repo in github
git init : initialize local work with
git add.
git commit -m "initial commit"
git remote add origin git-url : point your local git to remote repo that we just created
git push origin master : push your code to github

Travis CI:
----------
https://travis-ci.org/ : login using github account
under : Legacy Services Integration : check that repo you want to control by travis
travis does work based on .travis.yml file

.travis.yml file:
1. tell travis we need a copy of docker running 
2. build our image using Dockerfile.dev
3. tell travis our to run our test suite
4. tell travis how to deploy our code to AWS


Setting Environment Varibale:
-----------------------------
version: '3'
services:
  web:
    restart: always
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - /app/node_modules
      - .:/app
    environment:
      - key=value : Sets the variable in the container
      - key : Sets the variable in the container, value is taken from your computer

Kubernetes:
-----------
kubernetes cluster = master + nodes
node can be virtual machine or physical computer, used to run containers
master will control these nodes
we reach out to cluster using master

request --> load balancer --> cluster (nodes + master)

minekube : use to run kubernetes cluster inside vm & managing the vm
kubectl : used to manage containers inside the nodes

    docker-compose.yml file :                                       kubernetes :
1. each entry can optionally get docker-compose to build an image : kubernetes expects all images to be already built : make sure our images are hosted on docker hub
2. each entry represent a container we want to create : one config file per object we want to create : make one config file to create our container
3. each entry define the networking requirements (ports) : we have to manually set up all networking : make one config file to setup networking

step 1 : upload your images to docker hub

step 2 : creating configuration file for web web-pod.yml 
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: thecrazzyrahul/web
      ports:
        - containerPort: 8080


step 3 : web-node-port.yml
apiVersion: v1
kind: Service
metadata:
  name: web-node-pod
spec:
  type: NodePort
  ports:
    - port: 8081
      targetPort: 8080
  selector:
    component: web



config file ----> used to create object ----> ( type of objects )  statefulset, replica controller, pod, service
pod is used to run a container
service is used to setup a networking

apiVersion: v1 will provide us set of predefined objects

Pods : grouping of similar contianers, we can run 1 or more containers inside it
When we start minikube we get node --> inside node we have pod.

node ----> pod ---> postgress + backup manager 2 containers inside 1 pod
so conatiners which has very tight relationship will be deployed under same pod

Services : is used to setup a networking
there are 4 sub types : ClusterIP, NodePort, LoadBalancer, Ingress
NodePort : expose container to outside world

request---> kube proxy ---> ServiceNode ---> Pod

- port: 3050 : other pod inside node can communicate to this pod using port
  targetPort: 3000 : port inside the pod which we want to open for traffic, this will send request to client-node on port 3000 & client-node has containerPort:3000 which listens
  nodePort: 31515 : used to dev/testing, it range from (30000 - 32767) we will use this localhost:31515 in browser to access client-pod, so nodePort is exposed to outside world,
                    if we dont assign any, random port will be assigned. this is for dev/testing use not for prod use.

kubectl apply -f filename : apply(chnage the current configuration of our cluster) -f (we want to specify a file that has the config changes) filename(path to the file with the config)

kubectl apply -f web-pod.yaml
kubectl apply -f web-node-port.yaml

kubectl get pods
kubectl get services

localhost:4200 : we can access the application