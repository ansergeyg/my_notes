Sometimes when you change the version of your image in a docker-compose.yml file you may face the issue with container not being visible when running **docker ps** command.

The issue may appear in different forms actually.

Couple of examples:

Scenatio 1:

0) you have image: mysql:5.7
1) now you do some docker clean up or rebuilding. Basically you touch images/containers/volumes and other docker related stuff.
2) you run your containers and you **don't see the container**

Scenario 2:

0) you have image: mysql:5.7
1) you changed it to newer version: mysql:8.0
2) you run your containers and everything works.
3) you change back the version: mysql:5.7
4) you run containers and now you **don't see the container**

##Possible resolutions:

0) In any situation if you have issues with containers it's always good to check container logs.
   But how to find the container's id if you don't see it running **docker ps** command?
     ```
     # you should run:
     
     docker ps -a
     ```
   - ```
     # now if you can see the container you can run:
     
     docker logs -f --details container_id
     ```

2) One of the common reasons is you ran out of **disk space**. It's easily fixable. Just clean up your disk space or increase it if you can.

3) The other reason is related to docker volumes. It often happens with database images as the have volumes.
  - run **docker volume ls**
  - find volumes related to the image and run **docker volume rm volume_name**
  - run docker containers

**Sometimes even when you run docker prune to remove all docker images and container, volumes may still be left untouched**.

So you need to check if they were cleaned up.
  
