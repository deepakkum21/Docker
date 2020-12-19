# Docker

## Diff b/w RUN & CMD

** RUN:- the command is executed when the image is created
** CMD:- the command is executed when container is being created

## Info about layer

`if something is changed related to a layer then all subsequent layers will also re-run`

- eg we can copy package.json in UI node related project then run npm install then again copy rest of code since things in package.json will not cahnge frequently compared to code to avoid re-running of npm install.

## Docker imp cmds:-

1. docker build dockerFilePath
2. docker run -p outsideport:internalexposedport imageId/imgaeName **(it starts in attached mode**
3. docker stop containerName/id
4. docker ps
5. docker ps -a
6. docker rmi imageId/imgaeName (-removeimage)
7. docker rm containerName/id
8. docker start stoppedcontainerName/id (to start stopped container) **(it starts in dettached mode**
9. docker attach containerName/id **(run in attach mode) this mode helps in printing logs**
10. docker logs containerName/id **(to see past logs)**
11. docker logs -f containerName/id **(to see past logs and listen to upcoming logs)**
