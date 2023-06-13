#### docker run - https://docs.docker.com/engine/reference/run/

- Docker runs processes in isolated containers. A container is a process which runs on a host. The host may be local or remote. When an operator executes `docker run`, the container process that runs is isolated in that it has its own file system, its own networking, and its own isolated process tree separate from the host.

  

#### docker ps - https://docs.docker.com/engine/reference/commandline/ps/

- List containers

  

#### docker create - https://docs.docker.com/engine/reference/commandline/create/

- Create a new container

  

#### docker start - https://docs.docker.com/engine/reference/commandline/start/

- Start one or more stopped containers

  

#### docker system prune - https://docs.docker.com/engine/reference/commandline/system_prune/

- Remove all unused containers, networks, images (both dangling and unreferenced), and optionally, volumes.



#### docker logs - https://docs.docker.com/engine/reference/commandline/logs/

- The `docker logs` command batch-retrieves logs present at the time of execution of some container



#### docker stop - https://docs.docker.com/engine/reference/commandline/stop/

- Stop one or more running containers, SIGINT

  

#### docker kill - https://docs.docker.com/engine/reference/commandline/kill/

- Kill one or more running containers, SIGKILL

  

#### docker exec - https://docs.docker.com/engine/reference/commandline/exec/

- Run a command in a running container



#### docker build - https://docs.docker.com/engine/reference/commandline/build/

- The `docker build` command builds Docker images from a Dockerfile and a “context”. A build’s context is the set of files located in the specified `PATH` or `URL`. The build process can refer to any of the files in the context. For example, your build can use a [*COPY*](https://docs.docker.com/engine/reference/builder/#copy) instruction to reference a file in the context.
- -f > add a custom file name



## <u>Docker File</u> 

#### From - https://docs.docker.com/engine/reference/builder/#from

- The `FROM` instruction initializes a new build stage and sets the [*Base Image*](https://docs.docker.com/glossary/#base_image) for subsequent instructions. As such, a valid `Dockerfile` must start with a `FROM` instruction. The image can be any valid image – it is especially easy to start by **pulling an image** from the [*Public Repositories*](https://docs.docker.com/docker-hub/repos/).

#### Workdir - https://docs.docker.com/engine/reference/builder/#workdir

- The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the `Dockerfile`. If the `WORKDIR` doesn’t exist, it will be created even if it’s not used in any subsequent `Dockerfile` instruction.

#### Copy - https://docs.docker.com/engine/reference/builder/#copy

- The `COPY` instruction copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`.

#### Run - https://docs.docker.com/engine/reference/builder/#run

- The `RUN` instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the `Dockerfile`

#### CMD - https://docs.docker.com/engine/reference/builder/#cmd

- **The main purpose of a `CMD` is to provide defaults for an executing container.** These defaults can include an executable, or they can omit the executable, in which case you must specify an `ENTRYPOINT` instruction as well.

#### user - https://docs.docker.com/engine/reference/builder/#user

- The `USER` instruction sets the user name (or UID) and optionally the user group (or GID) to use when running the image and for any `RUN`, `CMD` and `ENTRYPOINT` instructions that follow it in the `Dockerfile`

#### ENV - https://docs.docker.com/engine/reference/builder/#env

- The `ENV` instruction sets the environment variable `<key>` to the value `<value>`

## <u>Docker  Compose</u>

#### docker-compose up - https://docs.docker.com/compose/reference/up/

- The `docker-compose up` command aggregates the output of each container (essentially running `docker-compose logs --follow`). When the command exits, all containers are stopped. Running `docker-compose up --detach` starts the containers in the background and leaves them running
- **docker-compose up --build** > builds alongside composing.

#### docker-compose down - https://docs.docker.com/compose/reference/down/

- Stops containers and removes containers, networks, volumes, and images created by `up`.

#### <u>Docker-Compose File</u> - https://docs.docker.com/compose/gettingstarted/

##### Version 

- The version of docker compose we are currently using

##### Services 

- The containers to be started with compose

##### Image

- Takes in the image to be used for this container

##### Build 

- Takes the context of where to build this container

##### ports

- port on our machine : port on container to map 