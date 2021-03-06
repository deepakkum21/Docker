# Docker

## Diff b/w RUN & CMD

- **RUN**:-

  - the command is `executed when the image is created`

- **CMD**:-
  - the command is `executed when container is being created`
  - if some cmd in mention in cmdline then this will override the cmd mentioned in CMD
  - if `no CMD cmd is present in the dockerfile then the base image CMD cmd will execute`

**ENTRYPOINT**:-

- the command is `appended to the cmd mentioned in ENTRYPOINT`

## Diff b/w ENTRYPOINT & CMD

- **ENTRYPOINT**:-

  - the command is `appended to the cmd mentioned in ENTRYPOINT`

- **CMD**:-

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
2. `docker run -p outsideport:internalexposedport imageId/imageName` **(it starts in attached mode**
3. `docker stop containerName/id`
4. `docker ps` or `docker container ls/ps`
5. `docker ps -a` or `docker container ls/ps -a`
6. `docker rmi imageId/imageName` (-removeimage) **(before stopping a image container's realted to the image has to removed )**
7. `docker rm containerName/id` **(before stopping a running container it has to stopped)**
8. `docker start stoppedcontainerName/id (to start stopped container)` **(it starts in dettached mode**
9. `docker attach containerName/id` **(run in attach mode) this mode helps in printing logs**
10. `docker logs containerName/id` **(to see past logs)**
11. `docker logs -f containerName/id` **(to see past logs and listen to upcoming logs)**
12. `docker run -it imageId/imgaeName` **( to run a container in -i ----> interative -t ---->terminal)**
13. `docker image prune` **(to remove all the unused images)**
14. `docker run -d --rm -p outsideport:internalexposedport imageId/imgaeName` **(it starts in -d dttached mode, -p publish port, --rm for removing the container as soon it is stopped**
15. `docker image inspect imageId/imgaeName` **( to know more about the image)**
16. `docker cp sourcefoldername_local/. containerid/name:/destinationfolder_docker` **(copying from local to container)**
17. `docker cp containerid/name:/sourcefoldername destinationfolder_local` **(copying from local container to local)**
18. `docker run --rm -p outsideport:internalexposedport --name nameofcontainer imageId/imgaeName`
19. `docker tag previousname:tag reponame/newimagename:tag` **to rename a image for pushing in dockerhub**
20. `docker push reponame/imagename:version_or_tag`
21. `docker pull reponame/imagename:version_or_tag`
22. `docker container top containerName_ID` to show the processes running inside the container.
23. `docker container stats` to show the stats like ram, cpu, mem, network used by container
24. `docker exec [OPTIONS] CONTAINER COMMAND [ARG...]` Run a command in a running container

## Volumes and persistence

1. The data is stored in the thin layer which gets added on top of container i.e it is related to each container.
2. stopping of a container doesn't destroys data of that particular container.
3. removal of container removes data of that container if volume concept is not used.
   - `docker volume ls`
   - `docker volume create vol_name`
   - `docker volume inspect vol_name` to see the details about a vol

### Types of volumes:-

1. `Anonymous volume` (managed by docker):-

   - **gets destroyed when container gets shutdown. It is attached to a particular container**
   - path is unknown
   - `docker run -d -p 3000:80 --rm --name containerName -v /app/node_modules imageId_or_name`
   - **it is also helpful in avoiding the files which we donot want to be overriden**.
   - cannot be shared across containers

2. `Named volume`(managed by docker):-

   - **it can be created at the time of running the container. It is not meant to be accessed by normal user. The data survives the container shutdown. It is not attached to a particular container**
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
   - **`:ro`** for read only when we dont want the container should not write accidently in our local file system
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
     - `docker run --network network-name -d -rm --name containername imageid_or_name`

3. **Different Docker network drivers**

   1. **`bridge`**:
      - The `default network driver`.
      - If you don’t specify a driver, this is the type of network you are creating.
      - Bridge networks are usually used when your applications run in standalone containers that need to communicate.
      - the `default bridge network doesnot have DNS`.
      - but the `user created bridge network has DNS`.
      - User-defined bridge networks are `best when you need multiple containers to communicate on the same Docker host`.
   2. **`host`**:-
      - For standalone containers, remove network isolation between the container and the Docker host, and use the host’s networking directly.
      - All the process will be isolated but the network will not be isolated as it will use host network.
      - Host networks are `best when the network stack should not be isolated from the Docker host`, but you want other aspects of the container to be isolated.
      - `At a time one network with host driver can be created`, if want to create a new network with host driver first have to delete the default one.
      - use case:- `when you don't want to do port mapping and want to use host network`.
   3. **`overlay`**:-
      - Overlay networks `connect multiple Docker daemons together` and `enable swarm services to communicate with each other`.
      - You can also `use overlay networks to facilitate communication between a swarm service and a standalone container`, or `between two standalone containers on different Docker daemons`. This strategy removes the need to do OS-level routing between these containers.
      - Overlay networks are `best when you need containers running on different Docker hosts to communicate`, or w`hen multiple applications work together using swarm services`.
   4. **`none`** :-
      - For this container, disable all networking.
      - if `one want to create a container and want doesnot want to attach with any network then will use none` as if you don't specify any network it will attach to bridge network.
      - Usually used in conjunction with a custom network driver. none is not available for swarm services.

4. **Connecting/Disconnecting container to network on fly**
   - `docker network connect netorkName containerID_Name`
   - `docker network disconnect netorkName containerID_Name`
   - **Note**
     - one **`cannot connect a container connected to none or host to another network`**.

## **Things to remember while dockerizing javascript code**:-

1. **Since javascript code is executed in browser we `cannot use containerName` while calling a api or any communication related to docker thing**.
2. **Also the `ports of those container has to be published` with whom javascript code is going to communicate**
3. No need to run in the network created for docker containers.
4. **`While making the container -it option has to be given` for interactive terminal otherwise it wouldn't start**.

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

## Docker Local Registry (https://docs.docker.com/registry/deploying/):-

- used for creating ur own private repo.
- `docker container run -p 5000:5000 -d --name simple_registry registry`
- can be accessed at `/v2/_catalog`
- annonymous vol created by registry image conatiner is at `/var/lib/registry`
- so get the access of the images saved in the registery bind this vol at this.
- by default it used local driver, one can use S3 driver for saving in aws, or other cloud service driver.
- **use case**:-
  - this is helpful when we don't have the inetrnet and would not like have alot of downloaded images in the images list.
- **for pushing in local registery**:-
  - `docker image tag imageNameWhichYouWantToTag 127.0.0.1:5000/nameWhichYouWantToGiveToImage:tag`
  - `docke image push 127.0.0.1:5000/nameWhichYouWantToGiveToImage:tag` can use localhost instead of 127.0.0.1
  - list of local images pushed in the registery can be seen at `localohst:5000/v2/_catalog`
- **pulling from local registry**
  - `docker image pull 127.0.0.1/imageName:tag`
- docker **only allows pulling image from secured https exception** for `(127.0.0.1/8)`
- **making a insecure ip to be allowed**
  - `create a demon.json file` with following entries
    - `{ "insecure-registries" : ["ipwhichYouWantToAdd"] }`
  - put this file in `/etc/docker/`
- **making a secure ip using cert (https://gabrieltanner.org/blog/docker-registry)**

  - `openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:4096 -keyout private.key -out certificate.crt` create a new certificate
  - place these two certs in `/etc/docker/` under a folder `certs.d/repo.docker.local:5000/ca.crt`
  - repo.docker.local has to be added in `/etc/hosts/` so that this should be resloved.
  - restart the docker.
  - now run the registry container with following cmd
  - `docker container run -d -p 5000:5000 --name secure_registry -v $(pwd)/certs/:/certs -e REGISTRY_HTTP_TLS_CERTIFICATE=certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=certs/domain.key registry`
  - compose.yml

            version: '3'
            services:
               registry:
                  image: registry:2
                  ports:
                  - "443:443"
                  environment:
                     # if want to change the default port from 5000 to 443
                     REGISTRY_HTTP_ADDR=0.0.0.0:443
                     REGISTRY_HTTP_TLS_CERTIFICATE=certs/domain.crt
                     REGISTRY_HTTP_TLS_KEY=certs/domain.key
                  volumes
                  - ./certs:/certs

- **Customize Storage**

  - The docker registry `also lets you customize the location where the data of the registry is saved`. For that, you just have to `add an extra environment variable that defines` the path the data should be saved to.
  - compose.yml

            version: '3'
            services:
               registry:
                  image: registry:2
                  ports:
                  - "443:443"
                  environment:
                     REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY: /data
                  volumes:
                  - ./data:/data

- **basic authentication for registry**

  - start by `installing the htpasswd` package
    - `sudo apt install apache2-utils`
  - we will create a folder that will hold our password files.
    - `mkdir auth`
  - we will continue by creating a user using the following command
    - `htpasswd -Bc registry.password testuser`
    - The last parameter is the name of the user in this case testUser. After executing the command, you will be prompted to enter your password
  - docker-compose file:

           version: '3'
           services:
              registry:
                 image: registry:2
              ports:
              - "5000:5000"
              environment:
                 REGISTRY_AUTH: htpasswd
                 REGISTRY_AUTH_HTPASSWD_REALM: Registry Realm
                 REGISTRY_AUTH_HTPASSWD_PATH: /auth/registry.password
              volumes:
                 - ./auth:/auth

  - For `REGISTRY_AUTH you have to provide the authentification scheme` you are using.
  - `REGISTRY_AUTH_HTPASSWD_PATH is the path of the authentification file` we just created above.
  - restart
    - `docker-compose up --force-recreate`
  - `docker login localhost:5000`

## **Docker Compose**

1.  it is `config file` having `orchestration cmds` (start, stop, build).
2.  it `does not replace Dockerfile for custom images`.
3.  `Not suitable for managing multiple containers on different host`.
4.  basically it replaces writing complex cmds for a image or container.
5.  the `containers run using compose file are automatically removed`.
6.  `Creates a separate Network and put all the services present in the file` into that network.
7.  **`Docker-compose up`**
8.  **`Docker-compose up -d`** to run in detach mode by default it runs it attach mode
9.  **`Docker-compose down`** makes all containers stops but donot delete volumes
10. **`Docker-compose down -v`** to delete volumes also
11. the name given assigned to the container **`foldernameOFDockerComposeFile_servicename_count`** also `serive a=name is memorized and that can be used in code for connection`
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

## Using Variable interpolation in Docker compose

- where ever using `${vraible}` in docker compose file just prepend the variablw=value before docker-compose cmd
- eg. `VARIABLE=VALUE docker-compose up`

## Avoiding redundancy in using **docker-compose.override.yml**

- suppose if u want to have a extra service running in some mode either dev or prod. Instead of creating the same service again and adding extra one one can add only the extra one in the **docker-compose.override.yml** .
- running `docker-compose up` by default searches for `docker-compose.yml` file and also merges the services present in `
  **docker-compose.override.yml** if present.
- `https://docs.docker.com/compose/extends/`

## Understanding multiple Compose files

1. By default, Compose reads two files, a
   - `docker-compose.yml` contains your base configuration
   - an `optional docker-compose.override.yml file`. can contain `configuration overrides for existing services or entirely new services`.
2. If a service is defined in both files, Compose merges the configurations using the rules described in Adding and overriding configuration( https://docs.docker.com/compose/extends/#adding-and-overriding-configuration) .
3. To use multiple override files, or an override file with a different name, you can use the -f option to specify the list of files. Compose merges files in the order they’re specified on the command line.
4. When you use multiple configuration files, you must `make sure all paths in the files are relative to the base Compose file` (the first Compose file specified with -f).
5. eg in docker-compose-with-override folder

## Understand the extends configuration

1.  When defining any service in docker-compose.yml, you can declare that you are extending another service like this:

         web:
            extends:
               file: common-services.yml
               service: webapp

2.  This instructs Compose to re-use the configuration for the webapp service defined in the common-services.yml file. Suppose that common-services.yml looks like this:

         webapp:
            build: .
            ports:
               - "8000:8000"
            volumes:
               - "/data"

3.  In this case, you get exactly the same result as if you wrote docker-compose.yml with the same build, ports and volumes configuration values defined directly under web.

4.  You can go further and define (or re-define) configuration locally in docker-compose.yml:

         web:
            extends:
               file: common-services.yml
               service: webapp
            environment:
               - DEBUG=1
            cpu_shares: 5

         important_web:
            extends: web
            cpu_shares: 10

5.  You can also write other services and link your web service to them:

         web:
            extends:
               file: common-services.yml
               service: webapp
            environment:
               - DEBUG=1
            cpu_shares: 5
            depends_on:
               - db
         db:
            image: postgres

6.  more details `https://docs.docker.com/compose/extends/?ref=hackernoon.com#example-use-case-1`

## Things Docker compose can do what dockerfiles can do

- it `has entrypoint` just like dockerfiles
- it `has work_dir` just like dockerfiles

## Things Docker compose `cannot do` what dockerfiles can do

- it `doesnot have RUN`
- it `doesnot have COPY`

## making Utility containers

- `docker exec -it container-name commnad-which-want-to-run` `docker exec -it node1 npm init`
  - before that create a container with `-it` option like `docker run -it -d --name node1 node`
- **exec is used to running commands inside the container from outside**.

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
    - `docker-compose run --rm npm install init` since it doesnot delete once utility container finishes its job
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

## Using ECS (elastic container service);-

1. **it has four steps**:-
   - `container definition` :- config like port, container name, network, repo name, mounts, vol etc
   - `task definition` :- defining in which was the ECS should launch eg `FARGATE` serverless style launches when it is required
   - `service`:- laod balancer
   - `cluster` :- creating a virtual network, helpfull in muti container to make them available in one network.
2. **Multicontainer communication in ECS**:-
   - in normal or in local setup we use containername for communicating with one container to other like
     - connecting to mongo:- `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@containerName:27017/course-goals?authSource=admin`,
   - but in ECS :-
     - connecting to mongo:- `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@localhost:27017/course-goals?authSource=admin`,
   - best way:-
     - `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@${process.env.MONGODB_URL}:27017/course-goals?authSource=admin`,
     - in this way we can can define ENV variable in Dockerfile with deafult value of containerName
     - when deploying to ECS we can give env value that time of container config as localhost

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

- **Imperative commands** `individual cmds are executed to trigger the kubernetes` actions just like docker run
- **Imperative object configuration**
- **Declarative object configuration** in this we run `kubectl apply -f config.yaml` cmd pointing the config.yaml file just like docker-compose

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
      - this also has a loadbalancer but for internal communication not from the outside world.
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

## Declarative approach

- `kubectl apply -f configFile.yaml`

         ## deployment
            apiVersion: apps/v1
            kind: Deployment
            metadata:
               name: second-app-deployment

            # spec for deployment
            spec:
               replicas: 1

            # selector helps kubernetes to identifies the pods which will be manged by this deployment
            # eg in this all container with labels app having name second-app and label with label teir with name backend will managed # by this deployment and not other then this
               selector:
                  matchLabels:
                     app: second-app
                     tier: backend
               template:
                  metadata:
                     labels:
                        app: second-app
                        tier: backend
                  # spec for container
                  spec:
                     containers:
                     - name: second-node
                        image: deepakkum21/kub-first-app:v2
                        imagePullPolicy: Always
                        livenessProbe:
                           httpGet:
                           path: /
                           port: 8080
                           periodSeconds: 10
                           initialDelaySeconds: 5
                     # - name: ...
                     #   image: ...


         ## Service
            apiVersion: v1
            kind: Service
            metadata:
               name: backend
            spec:
               selector:
                  app: second-app
               ports:
               - protocol: 'TCP'
                 port: 80
                 targetPort: 8080
                  # - protocol: 'TCP'
                  #   port: 443
                  #   targetPort: 443
               type: LoadBalancer

- `minikube service serviceName` to run the service in browser
- `kubectl delete -f=deployment.yaml,file2` or`kubectl delete -f=deployment.yaml -f=service.yaml` to delete or can also use Imperative cmd.
- `kubectl delete -f deployment.yaml -f service.yaml`
- `kubectl delete typesOfKind -l labelKey=labelvalue` deleting using label

  - eg `kubectl delete deployments,services -l group=example` deployments,services having labels group=example will be deleted

           apiVersion: apps/v1
           kind: Deployment
           metadata:
              name: second-app-deployment
              labels:
                 group: example

## Kubernetes Volumes

- `https://kubernetes.io/docs/concepts/storage/volumes/`

## Limitations of emptyDir volume type:-

- since emptyDir volume is created inside the pod and incase of `replicas` the volume emptyDir will be created for each pod replica and incase of one pod getting shutdown due to anyerror till the time it gets up the datawill be lost.
- solution of this is using `hostPath volume type`.
- just like `named vol in docker`
- it `always create a new empty directory` at the starting.

               spec:
                  containers:
                  - name: story
                     image: deepakkum21/kub-data-demo:v2
                     volumeMounts:
                        - mountPath: /app/story
                        name: story-volume
                  volumes:
                  - name: story-volume
                     emptyDir: {}

## **hostPath volume type**

- it` creates volume inside the host machine` that can be shared with multiple pods.
- this doesnot create pod scpecific vol like emptyDir.
- `doesnot solve problem when we have multiple host machine`. since in cluster level every pod run on different host or node.
- just like bind mount in docker.
- this can be used to push some data from the begining ie. `sharing some already existing data`.

               spec:
                  containers:
                  - name: story
                     image: deepakkum21/kub-data-demo:1
                     volumeMounts:
                        - mountPath: /app/story
                        name: story-volume
                  volumes:
                  - name: story-volume
                     hostPath:
                        path: /data
                        type: DirectoryOrCreate

## **CSI container storage interface** volume type

- its a flexible volume type created by kubernetes team on which other cloud provider can build vol type like amazon has aws efs csi driver.

## Persistent Volume:-

- Thses volumes are independent of node or host .
- All volumes are available expect emptyDir.
- `host` type Persistent Volume are `only available for one node like minikube` where we have all in one node

         apiVersion: v1
         kind: PersistentVolume
         metadata:
            name: host-pv
         spec:
            capacity:
               storage: 1Gi
            volumeMode: Filesystem
            storageClassName: standard
            accessModes:
               - ReadWriteOnce
               # ReadWriteMany  :- multiple nodes from multiple host can read and write not availble for host-pv
               # ReadOnlyMany   :- multiple nodes from multiple host can read not availble for host-pv
            hostPath:
               path: /data
               type: DirectoryOrCreate



         apiVersion: v1
         kind: PersistentVolumeClaim
         metadata:
            name: host-pvc
         spec:
            volumeName: host-pv
            accessModes:
               - ReadWriteOnce
            storageClassName: standard
            # storage class name can be found :- kubectl get sc     standard in default for minikube which is of hostPath
            resources:
               requests:
                  storage: 1Gi

## Types of kubernetes resources:-

- `deployment`
- `service`
- `pv` (persistent volume)
- `pvc` (persistent volume claim)
- `configMap`

## Creating Cluster in EKS

1. Select EKS. give a name to your cluster.
2. Select kubernetes Ver.
3. `Pick a cluster service role` create if not - this helps kubernetes to create some node of EC2 type with proper permission.
   - click IAM(identity access management) create a new role
   - `choose AWS service and choose EKS-Cluster` with predefined roles required for EKS.
   - give a name to the clusterRole.
4. Specifying the network. so that it should also be accessible from inside and outside
   - open `cloudformation` service in new tab.
   - it helps in creating some services using templates.
   - click create. select template ready, template source AmazonS3.
     - paste `https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml`
     - more info at https://docs.aws.amazon.com/eks/latest/userguide/create-public-private-vpc.html#create-vpc
   - give stack a name.
5. in the VPC section select the newly ccreated VPC.
6. Choosing cluster end point access as `public and private`.

## Steps to use kubectl cms from ur machine for AWS - EKS

1. install aws cli.
2. go to my security credentials in AWS.
3. open Access keys and create one and download the key file.
4. run cmd `aws configure` and enter access key
5. enter region name.
6. `aws eks --region nameOfRegion update-kubeconfig --name nameOfClusterOnEKS` this will configure the config file in .kube in users folder so that u will be able to use kubctl cmd.

## Creating NodeGroup

1. in compute section. click add node group.
2. Give a name.
3. Creating a node IAM role.
   - click IAM(identity access management) create a new role
   - `choose AWS service and ec2`
   - add `AmazonEKSWorkerNodePolicy` `AmazonEKS_CNI_Policy` `AmazonEC2ContainerRegistryReadOnly` add these 3 policies.
   - give a name.
4. now select image for node, instance type and disk size
5. this will install kubectl on node and all other needed things

## Docker Swarm

- By default **sarm is inactive**
- when created `it uses ingress overlay network` **with builtin loadbalancing**
- `docker swarm init` to activate swarm. it initailizes single node swarm
- `docker node ls` to list the nodes. swarm manager is also is a node.
- `docker node inspect self/nodeId`
- `docker node promote`
- `docker node demote`
- `docker node ps`

- `docker swarm join-token [manager|worker]` manager will give token to join as manager, worker for joining as worker
- `docker swarm leave` this cmd to be executed on worker to remove from swarm
- `docker swarm join`
- `docker node rm nodeName` this cmd to be executed on manager to remove from swarm -`-f` to be removed forcefully

- `docker service create` docker service is similar to docker run
- `docker service update servicename --replicas count`
- `docker service scale serviceName1=count serviceName2=count` to scale up

## Notes:-

- one can access the service from the any of the node IP present in the swarm cluster with the port of the service.
  this is called `routing mesh`
- `ingress network` already **has a builtin loadbalancer**.
  - this is `automatically created when one creates a docker swarm`

## Docker swarm Overlay network:-

- `overlay network` is the only network which is used in the swarm.
  - The overlay network driver creates a distributed network among multiple Docker daemon hosts. This network sits on top of (overlays) the host-specific networks, allowing containers connected to it (including swarm service containers) to communicate securely when encryption is enabled.
  - `docker network create -d overlay networknameWhichYouWantToGive` -d drivername:- overlay

## Docker Stack:-

1.  docker stack is a command that's embedded into the Docker CLI.
2.  It lets you `manage a cluster of Docker containers through Docker Swarm`.
3.  It just so `happens both Docker Compose and the docker stack command support the same docker-compose. yml file with slightly different features`.
4.  docker stack cmds:-
    - `docker stack deploy -c composeFile.yaml nameWhichYouWantToGiveForStack`
    - `docker stack ls`
    - `docker stack ps stackName`
    - `docker stack rm stackName`
    - `docker stack services stackName`
5.  more info https://docs.docker.com/compose/compose-file/compose-file-v3/
6.  can be created using docker-compose
7.  **Note when using docker stack deploy**:-

    - The `build option is ignored when deploying a stack in swarm mode` The docker stack command does not build images before deploying.
    - The `container_name option is ignored when deploying` a stack in swarm mode.
    - The `depends_on option is ignored` when deploying a stack in swarm mode

8.  **deploy**

    - `Specify configuration related to the deployment and running of services`.
    - This `only takes effect when deploying to a swarm with docker stack deploy`, and is `ignored by docker-compose up and docker-compose run`

              version: "3.9"
              services:
                 redis:
                    image: redis:alpine
                    deploy:
                       replicas: 6
                       placement:
                          max_replicas_per_node: 1
                       update_config:
                          parallelism: 2
                          delay: 10s
                       restart_policy:
                          condition: on-failure

# Docker SECRET (only available to swarm services):-

1. You can use secrets to manage any sensitive data which a container needs at runtime but you don’t want to store in the image or in source control, such as:
   - Usernames and passwords
   - TLS certificates and keys
   - SSH keys
   - Other important data such as the name of a database or internal server
   - Generic strings or binary content (up to 500 kb in size)
2. `Docker secrets are only available to swarm services, not to standalone containers`.
3. Only those services can use the secret which has been assigned.
   - `docker service create -d --secret nameOfSecret imgaeName`
4. these secrets are **stored under** the `/run/secrets/secretName` in the container whcih has been assigned the secret.
5. more info https://docs.docker.com/engine/swarm/secrets/

- `docker secret create nameOfSecret -`:- this will create a secret with nameOfSecret `-` this will allow you to write the secret.
- `docker secret create nameOfSecret fileNameWhichYouWantToMakeSecret`:- another way of creating secret by using file
- `docker secret ls`:- list out the secret
- `docker secret inspect nameOfSecret`:-

# Docker swarm visualizer

- more info https://github.com/dockersamples/docker-swarm-visualizer

## Docker Service mode --global

- `docker service create --mode=global imageName` the mode global creates a instance in every node.
- `if the node increases in future the service with mode global will automatically` will be created.
- this use case is `helpful when one want a particular service to be running in every node created like antivirus, health monitoring` service.

## CONSTAINT node.role

1. if `want to run services only on manager`

   - **--constraint="node.role==manager"**
   - `docker service create --constraint="node.role==manager" imageName` this will make this service eligible to run only on manager nodes.

2. if `want to run services only on worker nodes`
   - **--constraint="node.role==worker"**
   - `docker service create --constraint="node.role==worker" imageName` this will make this service eligible to run only on worker nodes.

## Assigning Custom Lables to nodes.

- use case:-
  - suppose u have a node with ssd and would like some of the services to be executed on ssd.
- `docker node update --label-add="ssd=true" nodeName` to add a custom label to node
- `docker service create --constraint="node.labels.customLabelName==value" imageName` to add to the particular node having the custom label.
- **NOTE**

  - `once service with label has been created` and more load is seen on the node and one `creates/updates another node with the same custom label`. the `load or the service instance will not be shared` so create the node with the custom label in advance

  ## Setting custom label at different level:

  1. `Node Level`:-

  - `docker node update --label-add="ssd=true" nodeName` to add a custom label to node
  - `docker service create --constraint="node.labels.customLabelName==value" imageName` to add to the particular node having the custom node label.

  2. `Engine Level`:-

  - **edit dameon.json file**
  - add:-
  - "labels": ["key=value"]
  - if the node is already started then restart it.
  - one can see the assigned labels by inspecting the node. engine level label will be under the engine key.
  - `docker service create --constraint="engine.labels.customLabelName==value" imageName` to add to the particular node having the custom engine label.

  ## Docker Swarm Node Availability [active | pause | drain]

  1. `Active`:- this state of the node means it can take new jobs from the manager.
  2. `pause`:- this means all the existing services running in the node will run as usual but it will not take new task from the manager.

  - `docker node update --availability=pause nodeName`

  3. `drain`:- this will shift all the runnin services from the drain node to the other node in the cluster. so that one can do maintainance work in that node.

  - `docker node update --availability=drain nodeName`

## Reserving and Limiting resources in Docker swarm

- `--reserve-cpu`reserving cpu for the service
- `--reserve-memory` reserving no of bytes for the service
- `--limit-cpu` limiting cpu for the service so that it should not exceed
- `--limit-memory` limiting no of bytes for the service so that it should not exceed
- `docker service create aboveOptions`

## Docker swarm update service with upadte options

| update cmd with --update option  | description                                                              |
| -------------------------------- | ------------------------------------------------------------------------ |
| --update-delay duration          | Delay between updates (ns,us,ms,s,m,h)                                   |
| --update-failure-action string   | Action on update failure ("pause","continue","rollback")                 |
| --update-max-failure-ratio float | Failure rate to tolerate during an update                                |
| --update-monitor duration        | Duration after each task update to monitor for failure (ns,us,ms,s,m,h)  |
| --update-order string            | Update order ("start-first","stop-first")                                |
| --update-parallelism uint        | Maximum number of tasks updated simultaneously (0 to update all at once) |

## Docker EVENTS [https://docs.docker.com/engine/reference/commandline/events/]

- to `get real-time events from the server`.
- These events differ per Docker object type.
- Different event types have different scopes.
- Local scoped events are only seen on the node they take place on, and `swarm scoped events are seen on all managers`.
- Only the `last 1000 log events are returned`. You can `use filters to further limit the number of events returned`.

1. `docker events` Listening for events
2. **LIMIT EVENTS BY TIME**
   - `docker events --since 1483283804`
   - `docker events --since '2017-01-05'`
   - `docker events --since '2013-09-03T15:49:29'`
   - `docker events --since '10m'`
   - `docker events --since '2017-01-05T00:35:30' --until '2017-01-05T00:36:05'`

| Name, shorthand | Description                                   |
| --------------- | --------------------------------------------- |
| --filter , -f   | Filter output based on conditions provided    |
| --format        | Format the output using the given Go template |
| --since         | Show all events created since timestamp       |
| --until         | Stream events until this timestamp            |

3. **Filter events by criteria**

   - The filtering flag (-f or --filter) format is of “key=value”.
   - If you would like to use multiple filters, pass multiple flags (e.g., --filter "foo=bar" --filter "bif=baz")
   - The currently supported filters are:
     - config (config=<name or id>)
     - container (container=<name or id>)
     - daemon (daemon=<name or id>)
     - event (event=<event action>)
     - image (image=<repository or tag>)
     - label (label=<key> or label=<key>=<value>)
     - network (network=<name or id>)
     - node (node=<id>)
     - plugin (plugin=<name or id>)
     - scope (scope=<local or swarm>)
     - secret (secret=<name or id>)
     - service (service=<name or id>)
     - type (type=<container or image or volume or network or daemon or plugin or service or node or secret or config>)
     - volume (volume=<name>)
   - `docker events --filter "event=stop"`
   - `docker events --filter "image=alpine"`
   - `docker events --filter "container=test"`
   - `docker events --filter "container=test" --filter "container=d9cdb1525ea8"`
   - `docker events --filter "container=test" --filter "event=stop"`
   - `docker events --filter "type=volume"`
   - `docker events --filter "type=network"`
   - `docker events --filter "container=container_1" --filter "container=container_2"`
   - `docker events --filter "type=volume"`
   - `docker events --filter "type=network"`
   - `docker events --filter "type=plugin"`
   - `docker events -f type=service`
   - `docker events -f type=node`
   - `docker events -f type=secret`
   - `docker events -f type=config`
   - `docker events --filter "scope=swarm"`

4. **FORMAT**

   - If a format `(--format)` is specified, the given template will be executed instead of the default format. Go’s text/template (https://golang.org/pkg/text/template/) package describes all the details of the format.

   - If a format is set to `{{json .}}`, the events are streamed as valid JSON Lines. For information about JSON Lines, please refer to http://jsonlines.org/.
   - `docker events --filter "type=container" --format "Type={{.Type}} Status={{.Status}} ID={{.ID}}"`
   - `docker events --format "{{json .}}"`
