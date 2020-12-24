# Docker

## Diff b/w RUN & CMD

**RUN**:- the command is `executed when the image is created`
**CMD**:-

- the command is `executed when container is being created`
- if some cmd in mention in cmdline then this will override the cmd mentioned in CMD
- if `no CMD cmd is present in the dockerfile then the base image CMD cmd will execute`

**ENTRYPOINT**:-

- the command is `appended to the cmd mentioned in ENTRYPOINT`

## Diff b/w ENTRYPOINT & CMD

**ENTRYPOINT**:- the command is `appended to the cmd mentioned in ENTRYPOINT`
**CMD**:-

- the command is `executed when container is being created`
- if some cmd in mention in cmdline then this will override the cmd mentioned in CMD

## Info about layer

`if something is changed related to a layer then all subsequent layers will also re-run`

- eg we can copy package.json in UI node related project then run npm install then again copy rest of code since things in package.json will not cahnge frequently compared to code to avoid re-running of npm install.

## if entrypoint is not set then cmd instruction will be the default entrypoint

## name of image called as TAGS contains 2 things

- 1. RepoName
- 2. tagname or version
     `docker build . -t name:version`

## Docker imp cmds:-

1. `docker build dockerFilePath`
2. `docker run -p outsideport:internalexposedport imageId/imgaeName` **(it starts in attached mode**
3. `docker stop containerName/id`
4. `docker ps`
5. `docker ps -a`
6. `docker rmi imageId/imgaeName` (-removeimage) **(before stopping a image container's realted to the image has to removed )**
7. `docker rm containerName/id` **(before stopping a running container it has to stopped)**
8. `docker start stoppedcontainerName/id (to start stopped container)` **(it starts in dettached mode**
9. `docker attach containerName/id` **(run in attach mode) this mode helps in printing logs**
10. `docker logs containerName/id` **(to see past logs)**
11. `docker logs -f containerName/id` **(to see past logs and listen to upcoming logs)**
12. `docker run -it imageId/imgaeName` **( to run a conatiner in -i ----> interative -t ---->terminal)**
13. `docker image prune` **(to remove all the unused images)**
14. `docker run -d --rm -p outsideport:internalexposedport imageId/imgaeName` **(it starts in -d dttached mode, -p publish port, --rm for removing the container as soon it is stopped**
15. `docker image inspect imageId/imgaeName` **( to know more about the image)**
16. `docker cp sourcefoldername_local/. containerid/name:/destinationfolder_docker` **(copying from local to conatiner)**
17. `docker cp containerid/name:/sourcefoldername destinationfolder_local` **(copying from local conatiner to local)**
18. `docker run --rm -p outsideport:internalexposedport --name nameofcontainer imageId/imgaeName`
19. `docker tag previousname:tag reponame/newimagename:tag` **to rename a image for pushing in dockerhub**
20. `docker push reponame/imagename:version_or_tag`
21. `docker pull reponame/imagename:version_or_tag`

## Volumes and persistence

1. The data is stored in the thin layer which gets added on top of container i.e it is related to each conatiner.
2. stopping of a conatiner doesn't destroys data of that particular conatiner.
3. removal of conatiner removes data of that conatiner if volume concept is not used.
   - `docker volume ls`
   - `docker volume create vol_name`
   - `docker volume inspect vol_name` to see the details about a vol

### Types of volumes:-

1. `Anonymous volume` (managed by docker):-

   - **gets destroyed when container gets shutdown. It is attached to a particular conatiner**
   - path is unknown
   - `docker run -d -p 3000:80 --rm --name containerName -v /app/node_modules imageId_or_name`
   - **it is also helpful in avoiding the files which we donot want to be overriden**.
   - cannot be shared across containers

2. `Named volume`(managed by docker):-

   - **it can be created at the time of running the container. It is not meant to be accessed by normal user. The data survives the container shutdown. It is not attached to a particular conatiner**
   - `docker run -d -p 3000:80 --rm --name containerName -v volumeName:pathOfFolderToBeStored imageId_or_name`
   - path is unknown
   - can be shared across containers

3. `Bind Mount`(managed by user):-
   - We define the path of the folder on our host machine.
   - used for persistent and editable data.
   - **it can be created at the time of running the container.**
   - `docker run -d -p 3000:80 --rm --name containerName -v fullPathOnLocalHost:pathOfFolderToBeStored imageId_or_name`
   - Avoid overriden of some files use anonymous volumes with bind mount volumes.
   - `the more specific or long description folder will survive` in below -v /app/node_modules has more specific folder name than /app
   - `docker run -d -p 3000:80 --rm --name containerName -v fullPathOnLocalHost:pathOfFolderToBeStored -v /app/node_modules imageId_or_name`
   - **`docker run -d --rm --name feed -p 3000:80 -v "F:/gitrepos/Docker/03 Managing Data & Working with Volumes/data-volumes-06-adjusted-dockerfile:/app" -v /app/node_modules feedback:nodemon`**
   - can be shared across containers
   - **`:ro`** for read only when we dont want the conatiner should not write accidently in our local file system
   - **`:delegated`** used for **not immediately writing back to host machine instead to do in batches to improve performance**.

## ARGument (build-time)

- **Available in Dockerfile** , NOT accessible via CMD or any code.
- **can be set at the time of image** build using `docker build . --build-arg KEY=VALUE`.

## ENVironment (runtime)

- **Available in Dockerfile** , and in any code.
- `can be set in dockerfile` or via `Docker run --env`
- can be used in **setting dynamic port**
- `docker run --env KEY=Value imageId_or_ImageName` or `docker run -e KEY=Value imageId_or_ImageName`
- **if using a .env file to store nev key value pairs** `docker run --env-file ./.env imageId_or_ImageName`

## NETWORK:-

1. **Container to host machine**

   - `host.docker.internal` should be **used inplace of localhost where ever communicating to host machine**.

   - communicating to db running in localhost.

2. **Container to Container**

   - `docker inspect containername_or_id` to see the IP add

   - **using NETWORK**
     - to resolve IP automatically network is created, as inside same Network ip are resolved
     - `docker network create network-name` unlike volume network is not created automatically.
     - `docker run --network network-name -d -rm --name conatinername imageid_or_name`

## **Things to remember while dockerizing javascript code**:-

1. **Since javascript code is executed in browser we `cannot use containerName` while calling a api or any communication related to docker thing**.
2. **Also the `ports of those conatiner has to be published` with whom javascript code is going to communicate**
3. No need to run in the network created for docker conatiners.
4. **`While making the conatiner -it option has to be given` for interactive terminal otherwise it wouldn't start**.

## Mongo :-

1. `/data/db` is the path where mongodb stores data. So this can be used with named volume or bind mount.
2. `27017 is the port exposed` by mongo
3. `MONGO_INITDB_ROOT_USERNAME, MONGO_INITDB_ROOT_PASSWORD are the 2 env variables` which need to be passed for setting auth based mongodb.
4. `docker run -v mongodb_data:/data/db -d --network goals-net --name mongodb -e MONGO_INITDB_ROOT_USERNAME=deepak -e MONGO_INITDB_ROOT_PASSWORD=deepak mongo`

## Communication across multiple container in full fleged app

1. backend:-
   - `docker run -d --rm --name goals-backend -v "F:/gitrepos/Docker/05 Building Multi-Container Applications with Docker/multi-02-finished/backend:/app" -v logs:/app/logs -v /app/node_modules -p 80:80 --network goals-net goals:node`
   - **can also use --env for passing MONGODB_USERNAME=<value> --env MONGODB_PASSWORD=<value>**
2. DB:-
   - `docker run -v mongodb_data:/data/db -d --network goals-net --name mongodb -e MONGO_INITDB_ROOT_USERNAME=deepak -e MONGO_INITDB_ROOT_PASSWORD=deepak mongo`
3. react-frontend:-
   - `docker run -it -v "F:/gitrepos/Docker/05 Building Multi-Container Applications with Docker/multi-02-finished/frontend/src:/app/src" --rm --name goals-frontend -p 3000:3000 goals:react`

## **Docker Compose**

1.  it is `config file` having `orchestration cmds` (start, stop, build).
2.  it `does not replace Dockerfile for custom images`.
3.  `Not suitable for managing multiple conatiners on different host`.
4.  basically it replaces writing complex cmds for a image or conatiner.
5.  the `containers run using compose file are automatically removed`.
6.  `Creates a separate Network and put all the services present in the file` into that network.
7.  **`Docker-compose up`**
8.  **`Docker-compose up -d`** to run in detach mode by default it runs it attach mode
9.  **`Docker-compose down`** makes all conatiners stops but donot delete volumes
10. **`Docker-compose down -v`** to delete volumes also
11. the name given assigned to the conatiner **`foldernameOFDockerComposeFile_servicename_count`** also `serive a=name is memorized and that can be used in code for connection`
12. `docker-compose up --build` **forcing to rebuild the images if something has changed otherwise if the images were present** before it will use the previous image and latest changes might not get picked.
13. `docker-compose build` will only build custom images and will not make up.
14. `docker-compose run service-name cmdWhichWantToGiveAfterENTERPOINT/REPLACE?CMD`
15. example:-

            version: "3.8"
               services:
               mongodb:
                  image: "mongo"
                  volumes:
                     - data:/data/db
                  # container_name: mongodb                         // explicitly giving the name for the container since default name is profoldername_servicename_incrementedvalue
                  # environment:
                  #   MONGO_INITDB_ROOT_USERNAME: max
                  #   MONGO_INITDB_ROOT_PASSWORD: secret
                  # - MONGO_INITDB_ROOT_USERNAME=max
                  env_file:
                     - ./env/mongo.env
                  # networks:
                  #   - goals-net         // can have but compose will create a fresh new network automatically for all the container/services so no need to mention networks
               backend:
                  build: ./backend
                  # build:
                  #   context: ./backend   // this sets the context for the image i.e just like setting base dir if COPY or RUN cmd is  // present in dockerfile
                  #   dockerfile: Dockerfile
                  #   args:
                  #     some-arg: 1
                  ports:
                     - "80:80"
                  volumes:
                     - logs:/app/logs
                     - ./backend:/app
                     - /app/node_modules
                  env_file:
                     - ./env/backend.env
                  depends_on:
                     - mongodb
               frontend:
                  build: ./frontend
                  ports:
                     - "3000:3000"
                  volumes:
                     - ./frontend/src:/app/src
                  # for -i interative
                  stdin_open: true
                  # for -t tty
                  tty: true
                  depends_on:
                     - backend

               volumes:
                  data:
                  logs:

## Things Docker compose can do what dockerfiles can do

- it `has entrypoint` just like dockerfiles
- it `has work_dir` just like dockerfiles

## Things Docker compose `cannot do` what dockerfiles can do

- it `doesnot have RUN`
- it `doesnot have COPY`

## making Utility containers

- `docker exec -it container-name commnad-which-want-to-run` `docker exec -it node1 npm init`
  - before that create a container with `-it` option like `docker run -it -d --name node1 node`
- **exec is used to running commands inside the conatiner from outside**.

1.  **_Creating a node-util_**

    - `docker build . -t node-util` with docker file

             FROM node:14-alpine

             WORKDIR /app

    - `docker run -it -v "F:/gitrepos/Docker/07 Working with _Utility Containers_ & Executing Commands In Containers/01-creating-node-utility-withoutnode-installed-on-host:/app" --name node-create1 node-util npm init`

2.  **_Creating a node-util_ which can take nay cmd in cmdline**

    - `docker build . -t node-util` with docker file

           FROM node:14-alpine

           WORKDIR /app

           ENTRYPOINT [ "npm" ]

    - now **no need to give npm after image_name** only cmd **like init install will work as it will get appended with ENTRYPOINT cmd**
    - `docker run -it -v "F:/gitrepos/Docker/07 Working with _Utility Containers_ & Executing Commands In Containers/01-creating-node-utility-withoutnode-installed-on-host:/app" --name mynode node-util init` for npm init - creating a node project
    - `docker run -it -v "F:/gitrepos/Docker/07 Working with _Utility Containers_ & Executing Commands In Containers/01-creating-node-utility-withoutnode-installed-on-host:/app" node-util install express` to install dependency like express

3.  **USING DOCKER_COMPOSE**
    - `docker-compose up`
    - `docker-compose run --rm npm install init` since it doesnot delete once utility conatiner finishes its job
    - `docker-compose run --rm npm install express nodemon`

## DEPLOYMENT

1. `Shouldn't have bind mounts`

## Steps to connect to ec2 instance using PUTTY:-

1. have you .pem secret key file.
2. download puttygen.
3. get your .pem file to be converted into .ppk file from .pem file.
4. add hostname eg 'ec2-user@ec2-18-219-128-222.us-east-2.compute.amazonaws.com' after @ is host name.
5. add username in data section under connection which is before @ above.
6. load private key file .ppk in SSH section under connection and now connect.

## Steps to install docker in ec2 Instance

1. `sudo yum update -y`
2. `sudo amazon-linux-extras install docker`
3. `sudo service docker start` if no complaint about docker it means docker is installed
4. `docker run -d --rm -p externalport:inetrnalport dockerhubusername/reponame:tag`
5. now set inbound rules in security group as no traffic is allowed in
6. Add a new rule

## Disadvantages of DIY Approach:-

- keep essentials software updated.
- manage network and security groups/firewalls.
- SSH into the machine.

## Using ECS (elastic conatiner service);-

1. **it has four steps**:-
   - `container definition` :- config like port, conatiner name, network, repo name, mounts, vol etc
   - `task definition` :- defining in which was the ECS should launch eg `FARGATE` serverless style launches when it is required
   - `service`:- laod balancer
   - `cluster` :- creating a virtual network, helpfull in muti conatiner to make them available in one network.
2. **Multicontainer communication in ECS**:-
   - in normal or in local setup we use conatinername for communicating with one conatiner to other like
     - connecting to mongo:- `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@conatinerName:27017/course-goals?authSource=admin`,
   - but in ECS :-
     - connecting to mongo:- `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@localhost:27017/course-goals?authSource=admin`,
   - best way:-
     - `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@${process.env.MONGODB_URL}:27017/course-goals?authSource=admin`,
     - in this way we can can define ENV variable in Dockerfile with deafult value of conatinerName
     - when deploying to ECS we can give env value that time of conatiner config as localhost

## Building a multi stage :-

- `docker build --target stageName -f pathOFDockerfile ./context`

## Install `kubectl` (communicating with cluster)

- **install Chocolatey**
  - `Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))` run in admin mode
- `choco install kubernetes-cli`
- `cd %USERPROFILE%`
- `mkdir .kube`
- cd into the .kube and create a file `config` without a extension.

## install minikube

- `choco install minikube`
- https://minikube.sigs.k8s.io/docs/start/
- `minikube start --driver=hyperv` if using hyperV or can use docker, or virtualbox
- `minikube status` to check the status
- `minikube dashboard` for a web based bashboard

## The kubectl tool supports three kinds of object management:

- **Imperative commands**
- **Imperative object configuration**
- **Declarative object configuration**

## Kubectl cmd

- `kubectl create deployment nameOfDeployment --imag=repoName/imageWhichWantToDeploy`
- `kubectl cluster-info`
- `kubectl get deployments` list all deployments
- `kubectl get pods` list all pods
- `kubectl get nodes`
- `kubectl delete deployments nameOfDeployment` to delete a particular deployment
- `kubectl expose deployment nameOfDeployment --type=LoadBalancer --port=portNoToBeExposed`

  - **--type**
    - `ClusterIP (default)`
      - Exposes the Service on an internal IP in the cluster.
      - This type makes the Service only reachable from within the cluster.
    - `NodePort`
      - Exposes the Service on the same port of each selected Node in the cluster using NAT.
      - Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.
    - `LoadBalancer`
      - Creates an external load balancer in the current cloud (if supported) and `assigns a fixed, external IP to the Service`. Superset of NodePort.
    - `ExternalName`
      - Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name.
      - No proxy is used. This type requires v1.7 or higher of kube-dns.

- `kubectl get services`
- `minikube service nameOfDeployment` to see in browser

- `kubectl scale deployments/deploymentName --replicas=noOfReplica` replica is a instance of pod/container

### Updating the deployed pod image-

- first `rebuild ur image with proper tag` tag is imp as without tag the pod is not going to update.
- `kubectl set image deployment/nameOfDeployment containerName(usually imageName)=repoName/imageName:tag`
- `kubectl rollout status deployment/nameOfDeployment` inspect progress of rollout
- `kubectl rollout undo deployment/nameOfDeployment` roll back to the previous deployment
- `kubectl rollout history deployment/nameOfDeployment` to know the history of all rollout
- `kubectl rollout history deployment/nameOfDeployment --revision=revisionNo` to get more details of a particular revision
- `kubectl rollout undo deployment/nameOfDeployment --to-revision=1` to go back to a particular revision
- `kubectl delete service nameOfDeployment` to delete the deployed pod service
- `kubectl delete deployment nameOfDeployment` to delete the deployment
