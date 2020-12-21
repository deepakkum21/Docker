# Docker

## Diff b/w RUN & CMD

**RUN**:- the command is `executed when the image is created`
**CMD**:- the command is `executed when container is being created`

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
