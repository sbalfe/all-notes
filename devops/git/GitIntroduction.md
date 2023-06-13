# Chapter 1

## About version control

- version control > records **changes** to a **file** or **set of files** over time so you can **recall specific** *versions later*
  - Examples > use **software source code** as the files **being version controlled**
    - Though, in reality you can do this **with nearly any type** of *file on a computer*.
- Graphic / web designer, want to keep every version of an **image / layout** 
  - A **version control system** is wise.
- Can *revert* to **previous state / revert entire project** to a *previous state*
  - Can **compare changes over time**
    - See who **last modified** something that **may cause a problem*
- Using **VCS** > screw up or **lose files**
  - Can **easily recover**
    - Additionally > get for **very little overhead**.

### Local version control systems

- Just copying files to other directories, unsafe.
- Deal by created simple VCS **database** to keep all changes to files **under revision control**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709140135836.png)

- Popular **VCS tools** > called **RCS**
  - still distributed, even amongst mac OS X.
- keeps patch sets in special format on disk, then re-create what any file looked like at **any point in time** by adding **all the patches**

### Centralized version control systems

- Collaboration with other developers is an issue.
  - Deal with this via **CVCS** 
    - Have **single server** that contains all the **versioned files** and a **number of clients** that check out from the **central place**
      - many years > standard of **version of control**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709140425384.png)

- has **single point of failure** at cost of **more organised team management**.

### Distributed version control systems

- includes GIT.
- clients dont just check out the **latest snapshot** of the file
  - They **mirror** the **repository**
    - Thus if a **server dies** > any of the **client local repos** can **be used to replace.**
- Each *checkout* is a **full backup** in this sense.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709140705064.png)

- Easy collaboration as many **repositories** work **in tangent**
  - Set up **workflows** that were **not possible before** in **centralized systems** such as **hierarchical methods**.

## GIT history

- Linux community created in contention with bitkeeper software leaving to follow these principles:

1. Speed
2. simple design
3. strong support for non linear development, many branches
4. fully distributed
5. handles large projects like linux kernel effectively. 

## Git basics

- Snapshots > *not differences*
- Major difference of GIT to other VCS > the way it **thinks about the data**
  - Conceptually > most other systems store **information** as a **list** of **file based changes**
    - They think of the information they keep as a **set of files** and **the changes** that are made to each **file over time**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709141238135.png)

- Git considers data as a set **of snapshots** of a *mini filesystem*
  - Each **commit** / **save state** in **GIT** > takes a **picture** of what the **files look like** at that moment 
    - then stores a **reference** to that **snapshot.**
      - To **be efficient** > if **files have not changed** > Git does **not store the file again**
        - Just a *link* to previous identical file **already stored**, consider it as a **stream of snapshots**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709141542969.png)

- Makes git more like a **mini filesystem** with some very **powerful tools** built on top of it
  - Rather than being a **simple VCS**

### Nearly every operation is local

- Most operations requires only **local files / resources** to *operate*
  - Often **no information** is needed from *another computer* on the network
- The **entire project history** is based on the **disk** locally therefore everything is **very fast**.

---

- To **browse history of your project**
  - Git does *not* need to go out to the server to get history and display for you.
    - just read from **local database**
      - Can see **project history** almost instantly.
- To view **changes introduced** between *current version* and one a **month ago**
  - Git can look up **file a month ago** and do *local difference calculation* , rather than doing in **remote server** or pull **older version** from **remote server**

---

- This means $\to$ very little you **cannot do** when you are **offline**
  - Can keep committing offline 

## Git has integrity

- Every operation in Git is **check summed** before stored then **referred** by that checksum
  - Its **impossible** to *change contents* of **any file / directory** without git knowing.
    - This is built into its **lowest levels** and integral to its philosophy.
      - Can **not lose information** in *transit / get file corruption* without git being able to detect.
- Git uses **SHA-1 hash**
  - A **40 character** string of **hex numbers** 
    - Calculated based on **contents of file** or *ordinary structure in Git*

```git
24b9da6552252987aa493b52f8696cd6d3b00373
```

- see values like this all over the place.

### Git Generally only adds data

- Doing action in Git > nearly all add **data** to the **git database**
  - Hard to **get system** to *do anything* that is **not undoable** or to **make** it **erase data** in any form
    - hard to lose once you **commit a snapshot** , especially if you **regularly** push database to **another repo**.
- Can test without fuck ups.

### Three Git states

- Git has **three main states** that your *files* reside in:
  1. **==committed==**
  2. **==modified==**
  3. **==staged==**

#### Committed

- Data is **safely** *stored* in the **local database**

#### Modified

- you have **changed the file** but **not committed** to the database.

#### Staged

- marked a **modified file** in its **current version** to *go to the next commit snapshot*

---

- Leads to **three main sections** of some **Git project**
  1. Git directory
  2. Working directory
  3. staging area

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709142750163.png)

- Git directory is where git **stores the metadata** and **object database** for the project
  - The most vital part of git
    - This is what is **copied** when you **clone a repository** from **another computer**
- The **working directory** is a *single checkout* of **one version** of the project
  - These files are **pulled** out of the **compressed** database in **Git directory** and placed on **disk** for you to **use / modify**
- The **==staging area==** often contained in the **git directory**
  - Stores information on what **goes into the next commit**
    - Referred to as the **index** also

### Basic Git Workflow

1. **Modify files** in your **working directory**
2. **Stage files** , adding snapshots of them to the **staging area**
3. Perform a **commit** which **takes the files** as they are in the staging area and **stores the snapshot** *permanently* to the **git directory**

---

- If some **version of a file** is in the **git directory**
  - Its considered **committed**
    - if its modified but has **been added** to the **staging area** > its **staged**
      - If its **changed** since it was **checked out** but has **not been staged** , its **modified.**

### First time git setup.

- After installing git on the system , perform some changes to **customize** the **git environment**
  - Only perform once.
- Use `git config` tool
  - Set **configuration variables** that control all aspects of **how git looks and operates**
    - Stored in **three seperate places**
      1. `/etc/gitconfig` 
         - Contain  values of **all users** on system and the repos.
         - passing `--system` to this reads and writes from this file only.
      2. `~/.gitconfig ` or `~/.config/git/config` 
         - Specific to your user
         - make git read / write to this file specifically by passing the `--global option`
      3. `config file` in the **git directory** > `.git/config` of whatever repo you are using
         - Specific to this single **repository.**

- Every level **overrides values** in the **previous level**
  - Therefore values in `.git/config` higher over **those in** `/etc/gitconfig`

- For windows > git searches for the `.gitconfig` file in the `$HOME` dir.
  - Looks for `/etc/gitconfig` also, even if that is **relative to MSys root**
    - Which is wherever you choose to install Git on the windows system on the installer.

### Identity

- add username and email address to git
  - As every **git commit** uses **information**, and its **immutably baked** into the *commits from creation*

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

- only done once if using **--global**
  - Git will now use this information for anything performed on the system
    - Want to **override** this with a different email for **specific projects** $\to$ run the **command** with **no global** 

### Editor

- Configure default **text editor** used with Git
  - If not configured $\to$ git uses **system default editor**, generally vim.
    - use a different one $\to$ such as emacs do this.

```bash
$ git config --global core.editor
```

---

- use `git config --list` to view settings.
- find a specific key values with `git config <key-value-name> ` 

### Help

```bash
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

- Example for config

```bash
$git help config
```

- Can access **offli**