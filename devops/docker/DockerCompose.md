# Docker Compose

- Cannot just store the **database** **server** and node app in the same container.

- May want to create **multiple** **servers** of the **same** **redis** **server**
  - Each one becomes **fragmented** and **disconnected** to produce incorrect results.
- Have **single** **instance** and just **connect** all **container** to it.
- **Containers** are **isolated**, **running** redis and docker in seperate containers require some form of link.

---

- Seperate CLI gets installed along with docker is docker-compose
  - Starts multiple docker containers at the same time
    - Automates some of the long winded arguments passed to docker run.

---

- Write our specific docker build and run commands into a docker compose **yml file**

---

- In our docker compose file
  - create redis server
    - Make it use the redis image.
  - create the node app
    - make it using the docker file in the current directory
    - map port 8081 to 8081.

---

## Running and building with docker compose.

- docker run <image> $\to$ 

```dockerfile
docker-compose up
```

- Docker build . && docker run image $\to$

```dockerfile
docker-compose up --build
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210729150034114.png)

- Automatically creates the networks default visits server.

---

### Other commands

- Launch in the background

```dockerfile
docker-compose up -d
```

- Stop containers

```dockerfile
docker-compose down
```

### Automatic container restarts

#### Restart policies

- No > **never** **attempt** to restart this **container** if stop or crashes

- always > if the container stops for any reason, always attempt restart
  - For web servers which should always stay online.
- on failure - only restart if container stops with error code
  - For temp worker processes that die after completing work
- unless stopped - always restart unless we forcibly stop.

---

```docker
version: '3' # version of docker compose

# services are types of containers such as our redis and node app 
services:
    redis-server:
        image: 'redis'
    node-app:
        restart: always # always attempt to restart after the container stops
        # directory to build this in, same file as the docker compose file.
        build: .
        ports:
            # dash is ana array , mapping our port 4001 to node-app container 8081
            - "4001:8081"

```

---

- Can use docker-compose ps in the yml file place to obtain container status

```docker
docker-compose ps // lists the container asscoc
```

