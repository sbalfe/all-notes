# Creating Node App and placing into a container.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724195608000.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724200342397.png)

```dockerfile
# specify base image

FROM alpine

# install deps

RUN npm install

# default command

CMD ["npm", "start"]
```

- This causes an error as `npm` is not a command within the alpine base image.
- use an image that contains node installed already on it.

```dockerfile
# specify base image, base image is a lightweight alpine node version with
# less base packages.

FROM node:alpine

# install deps

RUN npm install

# default command

CMD ["npm", "start"]

```

#### Copying in docker file

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724203237918.png)

```dockerfile
# specify base image, base image is a lightweight alpine node version with
# less base packages.

FROM node:alpine

# define working directory of the docker container at a given time
WORKDIR /usr/app

# copy takes our contexts ./ directory and places into the containers /usr/app (which is its root )container.
COPY ./ ./

# install deps

RUN npm install

# default command

CMD ["npm", "start"]

```

- At this point we can run the node apps
- but we must map the correct port for it to run on 

#### Port mapping

- Take a request on our localhost to some port , etc 8080 and map to the docker containers port 8080

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724203919339.png)

- This is incoming requests.
- Containers can always go out to request calls of course.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724204153115.png)

- the port map host : container doesnt have to match
- Just has to map so that the container port is the application that is running on it
- our port doesnt matter as long as we run the url from that port.

---

## Removing need for unnecessary builds 

- If we change the contents of the docker build file it will re run the commands
- otherwise it caches them
- changing our node project updates the build every time unless we specify the copy of only the package.json file.

---

