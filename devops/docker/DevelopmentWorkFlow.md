Development Work Flow

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210817184301659.png)

- This is the flow that we will follow when creating our app.

---

- Revolves around creating some **github repository.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210817184447675.png)

- **Feature branch** $\to$ The generic branch we develop on from which to **merge** to the **final master branch**
  - Master branch **changes** are **automatically deployed** to the **hosting provider**.

---

1. Pull Code from **feature branch** to our **local machine**.
2. We would **change** the files and **push** to the **feature**. 
3. Creates a **pull request** to the **master** branch if it passes **travis CI tests**
4. Make it automatically pass into **travis CI** *continuous integration* *provider*
   - Pulls the code and **runs ** through **various tests** that are written for us.
5. Travis takes the codebase and pushes to the **cloud** such as being **hosted** on **AWS**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210817185504972.png)

- The diagram above the above one, travis runs twice for merge in master and for pull request for merge.

---

- Docker is **not actually mentioned** as its **not required**.
- Docker just **makes the tasks easier**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210817191802675.png)

- test > test our react project
- build > converts the react to javascript
- start > starts test server.

---

- Create a `Dockerfile.dev` $\to$ used in **development only**
- Use the build version in normal `Dockerfile` $\to$​​ in **production**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210817195654120.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210817194033687.png)

- use `-f` to add a **custom file name** to be used as the **docker file**

---

```docker
docker run -it -p 3000:3000 IMAGE_ID
```

- Use this to run a react app we have created. 
- connects the running console output

---

- Copying source files of react from local > docker
  - Takes a **snapshot**

### Volumes https://www.youtube.com/watch?v=p2PH_YPCsis

- configures a **placeholder** in the docker container. 
- No copying required, just place some references essentially which **point to the local machine.**
- Similar to the idea of **port mapping.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210817200018370.png)

- Use `-v` tag to create the volumes
  - second one maps the **current working directory i.e ${pwd}** to our **/app dir** in the **docker container**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210817201254808.png)

- Without the first command 
  - we obtain the **react scripts not found error.**
    - This is because we created a mapping from  app > pwd but **node_modules** is only created in the **docker container itself** so it **references** nothing and is **overwritten**, this is the reason for **-v /app/node_modules**

- The **second command** sets the **app_modules** position to be set in stone and to not attempt to map it with anything else.
- Volumes just **allow** the **container** to **read from our current project directory** for live code changes in order to know what files to change.
- Using the mode modules volume to keep the the same as it is always.

---

- Execute tests using `npm run tests` 
- Start the container using **docker run** , change its **default start command** to npm run test however.

```bash
docker run 3fcf5b16eb3a npm run test
```

##### Test file executed when running this, checks for the text of learn react being present.

```javascript
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});

```

##### Live testing updates - only on linux

- Changes are not actually updating live when we add new tests.

- This is because of old volumes being used therefore file changes do not update throughout

- One method is **using** `exec` to run a **command in the container**

  - ```
    docker exec -it 934ffb0bd90c npm run test
    ```
  - any edits to the test js file will update the test running in the container.

---

```dockerfile

# create another test service in the docker compose
version: '3'

services: 
  web:
    build: 
      context: .
      dockerfile: Dockerfile.dev
    ports: 
      - "3000:3000"
    volumes:
      - /home/node/app/node_modules
      - .:/home/node/app 

  test:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - /app/node_modules
      - .:/app
    command : ["npm", "run", "test"] # command to run when it starts.

```
























----

```dockerfile
FROM node:alpine

# defualt > docker node image includes non root node user , here we are selecting this user for the proceeding commands
USER node

# make the directories alongside the parente directories. 
RUN mkdir -p /home/node/app

# set our current WorkDir to be this file in the container, prevents perms issue as 
# WORKDIR by default creates a dir if it does not exist and set ownership to root
WORKDIR /home/node/app

# inline chown set ownership of ifles copying from local env to node user in container 
COPY --chown=node:node ./package.json ./
RUN npm install

# not required when using volumes but useful when not using docker compose.
COPY --chown=node:node ./ ./

CMD ["npm", "run" ,"start"]
```

```dockerfile
version: '3'

services: 
  # name of the service to run, this doesnt matter 
  web:
    # specify how to build this file. 
    build: 
      # where to pull the files from, the file context 
      context: .
      # the name of the docker file which we have renamed.
      dockerfile: Dockerfile.dev
    ports: 
      - "3000:3000"

    # to update the code as it changes
    # map the node modules file to 
    volumes:
      - /home/node/app/node_module

      # when the directory /home/node/app is accessed it references the . = our projects current working directory.
      # enabling live changes in react apps for example.
      - .:/home/node/app 
```

