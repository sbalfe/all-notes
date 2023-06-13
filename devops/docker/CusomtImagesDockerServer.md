# Building custom images with docker server

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210719103743367.png)

- Docker file is where we define the image.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210719103830456.png)

### Create a Docker File

- It will run redis server.

```dockerfile
# Use an existing docker image as a base
FROM alpine

# download and install dependency
RUN apk add --update redis

# tell the image what to do when it starts as a container
CMD ["redis-server"]
```

- Build the file

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210719104831072.png)

- Run the container copying the sha256 at the bottom.

---

### Teardown of the docker file

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724182447687.png)

### Base image

- Docker file writing > given a computer with no OS, being told to install chrome
  - First step = install OS > commands to download and install then execute it.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724182740878.png)

#### Alpine reason for base

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724182856522.png)

- Requires the minimum needs we require for our OS to download our programs on it.

- `apk` command is linked to the alpine OS and **not docker**

### Detailed Build Process

```bash
docker build . // the dot is our build context 
```

- Uses our docker file to be passed to docker CLI
  - Creates the build context > set of files / folders to use in our image 4

https://docs.docker.com/develop/develop-images/build_enhancements/ - buildkit process 

https://brianchristner.io/what-is-docker-buildkit/#:~:text=To%20summarize%2C%20BuildKit%20has%20better,every%20layer%20of%20an%20image.

https://www.docker.com/blog/advanced-dockerfiles-faster-builds-and-smaller-images-using-buildkit-and-multistage-builds/

- Previous build process creates a temporary container 
  - executes the command in this container which is the apk add command,

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724191222138.png)

- This is the output without using build kit
  - Creates intermediate container > save step 2 > new image> that contains the redis packages.

- Execute CMD on step 3 , view this new image > create temp container
- CMD sets the command to run if the image was to run.
  - Shuts it down
    - takes snapshot of filesystem ----> = snapshot.

- This creates a snapshot and startup command.
- 

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724192045471.png)

- Remember docker uses the cache when it has already created some temp container or the alpine image for example
- Until it finds a new RUN command to create a new temp container.
- Uses cache only when something has never ran

- You can use the `--no-cache` flag when building the image to not use the same containers to update a package.

https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

---

### running

- We can now run this image using the ID of the sha256 / or even just a bit at the end.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724193959119.png)

#### Name our image with a tag so we can run it using that name instead.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724194050671.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724194128286.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724194230315.png)

https://docs.docker.com/engine/reference/commandline/tag/ 

#### Manual image creation with docker commit.

- Seems to be we can create images from containers as seen by the build process taking images from temporary containers with packages built on them.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210724195314645.png)

- I just created a container , ran the sh command within it 
- Built some redis packages in it
- then ran this commit command to create an image out of it.
- -c > shows default start command to use with this file snapshot of the command.



