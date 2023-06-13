## Purpose of docker

- Error message when installing software $\to$ docker attempts to fix this.
- Make it easy to install and run software on any given device / cloud based computing platform

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704065517997.png)

- This is what **docker is trying to fix**.
- easy to install and run software without worrying about setup or dependencies.

## What is docker

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704065833089.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704065907100.png)

- image just contains the files to create a container which is an instance of a program of the required deps and config.
- Each container is isolated in the resources it has available to it.

---

## Docker for mac / windows

- we use the docker client to interact with the docker daemon which manages container creation from images.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704070212017.png)

## Working with docker client

- run command 

  ```bash
  docker run hello-world

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704072019067.png)

- Docker server does the main work
  - Our command says to **start a new container** using the **image** of **hello-world**

1. first check for **local copy** of the **hello world image** in the *image cache*.
2. Reaches to **docker hub** where it has **free downloadable images** to run on **my pc**
3. loads this file which is now in the **image cache** 
4. creates an **instance** and **runs** this **program** on our computer.

- trying it again does **not produce same text** as we have it present in the **image cache**

## What is a container.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704072850142.png)

- we do not have the correct version on the disk, cannot install both at the same time, each apps requires a different one.

- can take use of **name spacing instead**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704073117792.png)

- kernel must route the correct app to the dedicated segment of the correct version.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704073240329.png)

- name spacing restricts what resources and process are viewable and accessible.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704073358151.png)

- This namespace and control groups combine to form a specific container.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704073439949.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704073619035.png)

- File system snapshot that creates a copy past of specific files / directories
- start-up command such as **run chrome**

---

- We allocate a specific section of the containers hard drive access to the **chrome and python installation.**
  - Once chrome is ran its isolated to the **resources** available to it **within the container.**

- Namespacing and control groups on **LINUX ONLY**
  - The installs on mac and windows are **linux virtual machines.** for my case its wsl2 however so its native.

## Using docker client

```dockerfile
docker run hello-world
```

- This runs the hello-world image.
