# Manipulating containers with the Docker Client

#### Creating and Running a container from an image

```dockerfile
docker run <image_name>
// reference docker CLI / attempt to create and run container / name of image for the container.

// takes the snapshot of the image with the start command, place in container with limited resources/ access
// and execute in that isolated environment.
```

- Override the specific start command provided in the container creation

```dockerfile
docker run <image_name> command // default commmand override.
```

```bash
docker run busybox hey
> hey
```

---

##### View file system in a docker container

```bash
docker run busybox ls
```

- This shows etc/ bin... etc which were in the busybox image file snapshot.

- This allows bash commands to execute
- Hello World may not include this command which causes an error.

---

##### View all running containers

```bash
docker ps // lists all containers on my machine
```

- Previous containers were temporary and shutdown after the execution, so it wont show up here.

```bash
docker run busbybox ping google.com // this keeps the container running as it wont finish this execution
```

- Running `docker ps` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210718165037323.png)

- Shows the currently executing docker container.

##### View entire history of containers that were ran

```bash
docker run ps --all 
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210718165228913.png)

---

### Container lifecycle

- Docker run = docker create + docker start.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210718165437038.png)

#### Creation

- This is just configuring the **file snapshot** for use within a specific container.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210718165651306.png)

- Displays ID of container i just created

```bash
docker create hello-world
```



#### Start

- This is the command to execute to **start running** the **container.**
- This command often starts processes 

```bash
docker start -a <container_id> // -a watches for output on this container and print to the terminal
```

- With hello world program , this prints the output of that to my terminal rather than being only within container.

---

### Restarting Stopped Containers.

```bash
docker start <container_id> // can start the exited containers listed on ps --all
```

- Cannot change the default start command once the container has been created.

#### Remove all containers and associated images/cache

```bash
docker system prune // delets build cache also which may be taken from docker-hub, stored locally now though
```

### Retrieving Log Outputs

```bash
docker logs <container_id> // alternative to -a flag and obtains a log of output from the container.
```

- This is ran after the docker has started, or exited and returns a list of the log output.

---

### Stopping Containers

```bash
docker stop <container_id>

docker kill <container_id>
```

#### Docker stop

- *SIGTERM* sent to primary process in this container to issue a shutdown on its own time.
- Ensure it allows time for cleanup and stuff.
  - Programming > can listen for this interrupt to know when to clean resources.
- After 10 seconds of not doing anything, runs docker kill instead.
- For our simple ping program it wont respond to docker stop.

#### Docker Kill

- SIGKILL for instant shutdown.

---

### Multi Command containers

- Run redis

```bash
docker run redis.
```

- Downloads image to accept connections
- cannot connect with CLI yet , must append to the start command to have both running in the docker container.

#### Docker Exec

```bash
docker exec -it <container_id> <command>
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210719101223244.png)

- Execute more commands within a container.
  - see `-it` gives us direct access to type commands into the container.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210719101406817.png)

- redis cli within the docker container is now given access to us.

#### Meaning of -it tag

- Each process in the linux environment has three streams
  - Standard out / in and error.
- Communicates information into the process.
- -it , we are entering into the standard input of redis-cli running in a docker container.
- standard out is the output linked to our terminal also.
- -it > -i and -t
  - -i > attach our terminal to standard input of redis-cli process
  - -t > ensure the text is formatted for output into the terminal.

https://stackoverflow.com/questions/30137135/confused-about-docker-t-option-to-allocate-a-pseudo-tty

#### Obtain shell access to the container

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210719102525890.png)

- Execute the **sh** command.
  - a type of **shell** like **bash / zsh / powershell**

```bash
shriller44@Simon-PC:/mnt/c/Users/Simon$ docker run -it busybox sh
/ #

// run the busybox image, attach standard input and output and run the shell within that.
```

https://superuser.com/questions/169051/whats-the-difference-between-c-and-d-for-unix-mac-os-x-terminal/169055

ctrl+d to exit from this one.

### Container isolation.

- Between 2 running containers, they do not automatically share there filesystem.
- Running the same process in different containers creates a seperate filesystem therefore the same command does not ripple throughout both.

