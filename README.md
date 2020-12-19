# Docker

## Diff b/w RUN & CMD

** RUN:- the command is executed when the image is created
** CMD:- the command is executed when container is being created

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
