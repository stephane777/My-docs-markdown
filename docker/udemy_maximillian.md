# DOCKER

### First image and container project

1. create the docker file
2. run the command below :
   `docker build .` // it will create the image
3. once done check the line where it's written : 'writing image sha256:922a38ee6ee401b68de9b885c0c06a8c1261dc07c6bfd136d9ff8ebb9c29a4c2'
4. run the container with the cmd below:
   `docker run -p 3000:3000 922a38ee6ee401b68de9b885c0c06a8c1261dc07c6bfd136d9ff8ebb9c29a4c2`
   -p to publish/translate the docker container port to its host.
5. check if http://localhost:3000 is working !
6. to stop the container open a new command window and run `docker ps`
7. check for the name of the container here => blissful_brahmagupta
8. run `docker stop blissful_brahmagupta` to stop the container

### Docker images & container

### Pre-Build image

`docker run node` // will download from dockerHub if not available locally
`docker ps -a` // exit 0
`docker run -it node` // execute node container and expose the shell to its host

> welcome to Node.js v14.17

### Build our own image with Dockerfile

nota: an image contains the environment and the content file

`FROM node` // hey I want to pull node image in my own image

`WORKDIR /app` // tell Docker that all the subsequent command will be executed inside the working directory /app

`COPY . /app` // which file should go in that image ?
// the first path right beside COPY is all the file at the same location where the Dockerfile is located ( Dockerfile excluded )
// the second path is the location where all the files should be copied into the image. Since we have WORKDIR = /app the destination
// can be changed form `COPY . /app` to `COPY . .` but we can leave `COPY . /app` as it's more clear.

`RUN npm install` // will create the node_modules inside /app

`RUN node server.js` // would try to run the server in the image but that's not what we want

`EXPOSE 80` // only for information purpose. the -p (publish) will do the job to expose the image port to its host.

`CMD ["node","server.js"]` // we only want to run the image from the container

Finally we will have :

```
  FROM node
  WORKDIR /app
  COPY . /APP
  RUN npm install
  EXPOSE 80
  CMD ["node","server.js"]
```

### Running a container based on our own image

Command to build the container based on that image, the path to locate the file is from the path where the cmd is executed.
```docker build .``` 

```
 => [2/4] WORKDIR /app                                                                                  0.2s
 => [3/4] COPY . /app                                                                                   0.0s
 => [4/4] RUN npm install                                                                               2.8s
 => exporting to image                                                                                  0.1s
 => => exporting layers                                                                                 0.1s
 => => writing image sha256:57c7f4ca43cbb6ce63bb0cf5c30e643eb74f185d670ac56a691ce3b3eb92bce4
```

Copy the id generated here it's : 57c7f4ca43cbb6ce63bb0cf5c30e643eb74f185d670ac56a691ce3b3eb92bce4

`docker run 57c7f4ca43cbb6ce63bb0cf5c30e643eb74f185d670ac56a691ce3b3eb92bce4` // will start the container base on that image.

At this stage it's not working yet.

```
 scandelas in ~/Documents/github_repo/UDEMY/Docker_Max/first-demo-starting-setup  > docker ps
 CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS     NAMES
 962efe02773c   57c7f4ca43cb   "docker-entrypoint.s…"   44 seconds ago   Up 43 seconds   80/tcp    agitated_carson
 scandelas in ~/Documents/github_repo/UDEMY/Docker_Max/first-demo-starting-setup  > docker stop agitated_carson
```

scandelas in ~/Documents/github_repo/UDEMY/Docker_Max/first-demo-starting-setup > docker run -p 80:80 57c7f4ca43cb

### Image are read-only

Once build the docker images are ready only. If we need to make a change to our code we need to run
```
docker build .
```
again and it will create an new image with the new code.

### Understanding image layer

When we make a change to a file after the image was created, Docker create layer and then cached.
Once you save your new change and rebuild again Docker won't run every command but only the one needed to update the change.

To optimise the process of building images with layer we can change the following :

```
  FROM node
  WORKDIR /app
  COPY package.json /app
  RUN npm install
  COPY . /APP
  EXPOSE 80
  CMD ["node","server.js"]
```

copy package.json and npm install before we copy all the files from the app we assure that next time we re build the image
the cache with all the packages will already exist.

### Stopping and Restarting Containers

```
  docker --help
  docker ps --help
  docker ps -a      // list all container running and stopped
  docker start beautiful_banach // running an existing container
  docker stop beautiful_branch // stop container
```

### Understanding attached and detached container

When running
`docker run image_id` // block the terminal
`docker start beautiful_banach` // run in the background
`docker start -a beatiful_banach` // run in attached mode, show the terminal

`docker run -p 80:80 beautiful_banach` // Attached container by running will listen the output and print msg to the console.
`docker run -p 80:80 -d beautiful_banach` // Detached container will run in the background without printing the msg to the console.

`docker attached image_id` // will switch the detached mode to attached mode.
`docker logs -f beautiful_banach` // will keep the logs live

### Entering interactive mode

'python-app-starting-setup'

`docker run -i -t 806af482d05b3cbdcc502e26d1a9b23ba74777be1c87dfc5a20f59940a522de9`
`docker run -it 806af482d05b3cbdcc502e26d1a9b23ba74777be1c87dfc5a20f59940a522de9`
`docker start -a -i awesome_ramanujan`

### Deleting images & containers

`docker rm` // for removing container
`docker rmi` // for removing images

`docker rm awesome_ramanujan determined_mclean`
`docker container prune` // will remove all stopped container at once

`docker rmi dd1a4dabdfae` // will remove that image if no container run it or reference it
`docker image prune` // will remove all non used images
`docker image prune -a` // will remove all non used images including ones with tag

### Removing stopped Containers Automatically

`docker run -p 3000:80 -d --rm 57c7f4ca43cb`
// --rm option will tell docker hey remove that container when the container is stopped !

### Inspecting images

`docker image inspect 57c7f4ca43cb`

### Copying files into & From a running container

// if you create a dummy folder at the root of your running image you can copy it on the running container
`docker cp dummy/. mystifying_merkle:/test`

// this will copy all the files in dummy to the container inside /test

To check if all files have been copied, delete the dummy folder locally and run :
`docker cp mystifying_merkle:/test dummy`

### Naming & Tagging Containers and images

// container
// for container we can simply add a name precedeed by --name
`docker run -p 8000:80 -d --rm --name container_name 4rr6djw7g8e`
`docker run -p 8000:80 -d --name docker_test 57c7f4ca43cb`

// images
// for images we have name:tag
// name => define a group of, possible more specialized, images e.g : node
// tag => Define a specialized image within a group of images e.g : 14

`FROM node:14`
`docker build . -t goals:latest`

### Sharing images

// Everyone who has an image, can create containers based on that image!
// 2 way to share a Docker project :

- Compress with zip the root folder of the app which should contain the Dockerfile as well, then run docker build . at root.
- Share a the image and then run Docker run image

### Share image on DockerHub

2 ways :

- DockerHub ( official Docker Image Registry )
- Private Registry ( any provider/ registry)

// Docker command available:

- docker push image
- docker pull image

1. create the repository on your account : https://hub.docker.com/repository/docker/stephane777/node-hello-world
2. check the name of the repo you want to publish on dockerhub
3. The repository name should match with the one created on dockerhub

ex: on Dockerhub I've created stephane777/node-hello-world

`docker images`
REPOSITORY TAG IMAGE ID CREATED SIZE
hello_from_inside 1 30344a76a330 4 days ago 949MB

`docker tag hello-from-inside:1 stephane777/node-hello-world`
`docker images`
REPOSITORY TAG IMAGE ID CREATED SIZE
hello_from_inside 1 30344a76a330 4 days ago 949MB
stephane777/node-hello-world latest 30344a76a330 4 days ago 949MB

// we have created a clone with a repo name matching with the one on dockerhub.

4. Login to your account with cli.
   `docker login`

5. finally run the push command to publish your image to the repo.
   `docker push stephane777/node-hello-world`

## Managing Data & Working with volumes ( section 3)

// Creating the images for data-volumes-01-starting-setup
`docker build . -t stephane777/feedback-node`

`docker run -p 8000:80 --rm -d --name feedback-app stephane777/feedback-node`

// to persist data in a container even though the container is removed we need to create volumes.
// we need to tell docker in our Dockerfile which volume docker needs to create e.g : /app/feedback

The feedback folder is where all the data received by the app will go, here our data form.

1. update the Dockerfile with the new volume
   `VOLUME ["/app/feedback"]`

2. create a new images based on this new update
   docker build . -t stephane777/feedback-node:volumes

3. create a new container based on this new image
   `docker run -d -p 8000:80 --rm --name feedback-app stephane777/feedback-node:volume`

// Now we have a problem !
check the logs
`docker logs feedback-node`
(node:1) UnhandledPromiseRejectionWarning: Error: EXDEV: cross-device link not permitted, rename '/app/temp/volumes.txt' -> '/app/feedback/volumes.txt'
// solution for this remove the fs.rename funciton in server.js by fs.copyFile

### Named volume

- Volumes // Docker sets up a folder / path on your host machine, exact location is unknown. Managed via `docker volume` commands.
- Bind Mounts

`docker volumes --help`
Commands:
create Create a volume
inspect Display detailed information on one or more volumes
ls List volumes
prune Remove all unused local volumes
rm Remove one or more volumes

`docker volume ls`
DRIVER VOLUME NAME
local 1e1f20b00959394f91994ec39e80a1024b621e7af5a251f5101cfca4b857f497

As we have created here an anonymous volume it won't persist on disk if we remove the container.
This volume exist only

The solution to persit data is named volume
`docker run -d -p 8000:80 --rm --name feedback-app -v feedback:/app/feedback stephane777/feedback-node:volumes`

`docker volume ls`
DRIVER VOLUME NAME
local feedback
