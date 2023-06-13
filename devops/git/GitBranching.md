# Git branching

- Branching means to **diverge** from the **main line of development** 
  - Without **messing** with the **main line**
    - Many **VCS** tools $\to$ this is **expensive** which requires a **copy** of the **source code directory** , takes a while for **large projects**
- GIT branching differs though
  - Makes the process **near instantaneous** 
- GIT encourage many branches and **merges** in the **same day**

## Branches in a Nutshell

- Git does **not store data** as just a **series** of **change sets** or **differences**
  - Rather just **snapshot series** 
- Commit in **Git**
  - Stores a **commit object** that contains a **pointer** to a **staged snapshot** of your *content*
  - Contains **author name / email / message / points to commits / commits ** that directly came **before** this commit
  - There are **zero parents** for the **initial commit**
  - There is **one parent** for a **normal commit**
  - There are **many parents** for a **commit** that results from **a merge** of **two / more branches**

---

- Directory **with three files**
  - You **stage** and **commit** them all
    - **staging** $\to$ computes some **checksum** for **each one** 
      - **Store** this version of the **file** in the **git repo** (*blobs*)
        - Adds that **checksum** to the **staging area**

```bash
$ git add README test.rb LICENSE
$ git commit -m 'Initial commit'
```

- Running `git commit` $\to$​ git **checksums** each **subdirectory**
  - For this case just the **root project directory**
    - Stores as **tree object** in *git repository*
- Git creates **commit object** $\to$ has **metadata** / **pointer** to **root project tree**
  - To **re-create** that **snapshot** when **needed**

---

- Repo has **five objects**
  - **3 blobs** (*each one* represents the **contents** of **one of three files**)
  - **1 tree** to list the **contents** of the **directory** and **specifies** which **file names** are stored as **which blobs**
  - **1 commit** with **pointer** to that **root tree** and **all commit metadata**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904160425729.png)

---

- Make **some changes** and **commit again**
  - The **next commit** then stores a **pointer** to the **commit** that *came immediately before it*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904160937539.png)

- A **branch** in **Git** is a **lightweight movable pointer** to *one of these commits*
  - The **default branch** name in Git is **master**
    - As you **make commits** $\to$ you are given the **master branch** 
      - Points to the **last commit** you *made*.
    - Every **commit** $\to$​ the **master branch** pointer moves **forward automatically** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904162541463.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904162637472.png)

### Creating New branch

- This creates a **new  pointer** for you to **move around**

---

- Create **new branch** called `testing` using the `git branch` command

```bash
$ git branch testing
```

- Create a **new pointer** to the **same commit** you are **on**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904163031086.png)

- How does **git** *know which branch you are on?*
  - Keeps **special** `Head` pointer.
    - This is a **pointer** to the **local branch** you are **currently on**
- In *this case* $\to$​ you are **still on master** 
  - The `git branch` command only **created a new branch**
    - It **did not switch to that** branch.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904163810384.png)

- Run a `git log` command that **shows** you where the **branch** *pointers* are **pointing**
  - Called `--decorate`

```bash
$ git log --oneline --decorate
f30ab (HEAD -> master, testing) Add feature #32 - ability to add new formats to the
central interface
34ac2 Fix bug #1328 - stack overflow under certain conditions
98ca9 Initial commit
```

- Can see the `master` and `testing` branches that are **right there** next to `f30ab` commit

### Switching Branches

- To **switch** to an **existing branch** 
  - Can run the `git checkout` 
    - Switch to the **new `testing`** checkout

```bash
$ git checkout testing
```

- Moves the `HEAD` to point to the `testing` branch.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904165950568.png)

- *Significance*
  - Another commit:

```bash
$ vim test.rb
$ git commit -a -m 'made a change'
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904170147226.png)

- The `testing` branch has **moved forward**
  - But the `master` branch **points** to the **commit** you ran when `git checkout` was used to **switch branches**.
    - Switch **back** to the `master` branch.

```bash
$ git checkout master
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904170444260.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904170559394.png)

- Did **2 things**
  1. Moved the `HEAD` pointer **back** to *point* to **master branch**
  2. *Reverted* the files in **working directory** back to the **snapshot** that *master* points to.
     - The **changes** made from this point **diverge** from an **older version** of the **project**
- This **rewinds** the work that was done in `testing branch` 
  - So you can **go in different direction**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904172924386.png)

- Make a **few changes** and **commit again**

```bash
$ vim test.rb
$ git commit -a -m 'made other changes'
```

- The **project history** has **diverged**
  - *Created / switched* to a **branch**
    - Performed some **work**
      - Then **switched back** to the **main branch** and *did other work*.
- The changes are **isolated** in **seperate branches**
  - Can **switch** *back / forth* between the **branches**
    - Then **merge** together when **ready.**
      - Just done with **simple branch / checkout / commit** commands.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904185643551.png)

- View this easily with `git log` command
  - run `git log --oneline --decorate --graph --all`
    - Prints out **history** of **your commits**
    - Shows **where** your **branch pointers** are and *how history* has **diverged**.

```bash
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) Made other changes
| * 87ab2 (testing) Made a change
|/
* f30ab Add feature #32 - ability to add new formats to the central interface
* 34ac2 Fix bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
```

- A **branch** in *Git* is just a **simple file** that contains **40 character** *SHA-1* Checksum of the **commit** it **points to**
  - Branches are **cheap** to **create / destroy**
    - Creating *new branch* is as **quick / simple** as *writing 41 bytes* to **file** (*40 chars and newline*)

---

- Other VCS $\to$ copy **all projects files** to **second directory**
  - This can **take several seconds** or **even minutes** depending on **size of project**
    - Where in gits **its instantaneous**
- As we **recording** the **parents** when we *commit* 
  - Finding a **proper merge base** for **merging** is *automatically* done for us therefore **easy to do**
    - These features **encourage devs** to create **many branches**

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904193137576.png)

## Basic Branching and Merging

- Example of **branching / merging** with a *workflow* that may be used **in real world**
  1. Do **work** on **site**.
  2. Create **branch** for **new user** that you are **working on**.
  3. Do **work** in that branch
- At **this stage** $\to$ receive **call** that **another issue** is *critical* and you need a **hotfix**
  - Do the **following**
    1. Switch to **production branch**
    2. create a **branch** for the **hotfix**
    3. After **tested** $\to$ **merge** the **hotfix branch** and **push** to **production**
    4. Switch **back** to the **original user story** and **continue working**

### Basic branching

- Say you are **working on your project** $\to$ have a **couple of commits** on the `master` branch

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904200720881.png)

- Decided to work on **issue** `#53` for whatever **issue tracking system** the **company uses**
  - To create a **new branch** and **switch** to it at **same time**
    - Run  `git checkout -b` 

```bash
$ git checkout -b iss53
Switched to a new branch "iss53"
```

- Shorthand for

```bash
$ git branch iss53
$ git checkout iss53
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904201147427.png)

- Work on **your site** and perform **some commits**
  - Moves the `iss53` branch **forward**
    - As you have **checked out** (your `HEAD` is pointing to it)

```bash
$ vim index.html
$ git commit -a -m 'Create new footer [issue 53]'
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210904201646260.png)

- Obtain a **call** there is an **issue** with the **site**
  - Must **fix immediately**
    - With *Git* you do **not have to deploy** the *fix* along with the *iss53* changes **you have made.**
      - There is **no effort** to **revert changes** before you *work on applying fix* to **what is in production**
        - All you do is **switch back** to the **`master` branch**.

- Before doing this
  - Check for **uncommitted changes** that in **staging area** that *conflict* with the **branch** you are **checking out**
    - Otherwise Git wont **let you switch branches.**
- There are **ways around this** $\to$​ such as **cleaning and stashing**
  - Now $\to$ assume **you commit to all changes**, so you can **switch** back to the **your `master` branch**.

```bash
$ git checkout master
Switched to branch 'master'
```

- The **project working directory** is the *same way* before **you started working** on *issue* on `#53` 
  - Concentrate on **your hotfix**
    - Important to remember:
      - When **switching branches**, Git **resets** your **working directory** to *look like it* did the **last time** you **committed** on the **branch**
- It *adds / removes* and *modifies* files **automatically** to **make sure ** your **working copy** is what the **branch looked** like on **your last commit to it**.

- You have a **hotfix** to **make**
  - Create a `hotfix` *branch* 

```bash
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index.html
$ git commit -a -m 'Fix broken email address'
[hotfix 1fb7853] Fix broken email address
1 file changed, 2 insertions(+)

```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210906202408792.png)

- Run **tests**
  - Ensure **hotfix** is *correct* 
    - Then **merge** the `hotfix` *branch* $\to$​ `master`   for **production deployment**
    - use `git merge`                        

```bash
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
index.html | 2 ++
1 file changed, 2 insertions(+)
```

- Notice *`fast-forward`* in **that merge**
  - `C4` **commit** pointed to by `hotfix` **branch** that you **merged** ahead of the `C2` commit.
    - Git just **moves** the **pointer forward** 
      - When you **merge** a **commit** with a **commit** that can be **reached** from **following** by *first commit history* 
        - Git makes **it simple** by **moving pointe forward** $\to$ as there is no **divergent work** to **move together** $\to$ ==fast forward==
- The **change** is now **in the snapshot** of the **commit pointed** to by the `master` branch.
  - Then you **can deploy the fix**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909153722015.png)

- After **fix deployed**
  - switch **back** to what you were doing **before you were interrupted**
- Must *delete* `hotfix` branch
  - As its **no longer required**
    - The `master` points to the **same place**
      - `Delete` using `-d` with `git branch`

```bash
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```

- Switch back to **work in progress** which is the `iss53` branch.

```bash
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'Finish the new footer [issue 53]'
[iss53 ad82d7a] Finish the new footer [issue 53]
1 file changed, 1 insertion(+)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909154404015.png)

- The work in the **hotfix** would **not be contained here** `iss53`
  - If you must **pull** , then `merge` the `master` branch into `iss53` 
    - using `git merge master` 
    - Can **wait** to **interrogate changes** until you **decide** to pull the `iss53` back into `master` later.

### Basic Merging

- `iss53` is **complete** and ready to **merge** with the `master` branch.
  - Similar `merge` as to with the **`hotfix` earlier** 
    - Must **checkout** the branch you **wish to merge** into 
      - Then run `git merge` command.

```bash
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html | 1 +
1 file changed, 1 insertion(+)
```

- This is **different** than the `hotfix` merge **performed earlier**
  - The **dev history** has **diverged** from **some older point**
    - Because the **commit** on the **branch** you are on **isn't** a **direct ancestor** of the **branch** you are **merging in**.
      - Git must **do some work**
- Performs a **simple three way merge** 
  - Uses the **two snapshots** *pointed* to by the **branch** *tips* and the **common** ancestors **of the two**

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909161021938.png)

- Rather than **just moving the branch** *forward*
  - Git creates a **new snapshot** that **results** from the **three way merge**
    - Automatically **creates a new commit** that **points to it**
      - Referred to as **merge commit**
        - Special as there is **more than one parent**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909161834173.png)

- The work is **merged in**
  - There is **no need** for `iss53` 
    - Close issue **by deleting the branch**

```bash
$ git branch -d iss53
```

### Basic Merge Conflicts

- If the **same part** of **some file** happens to be **different** in the **same file** when **merging**
  - Git can **not merge cleanly**
    - If the **fix** for `iss53` modified the **same part** as the `hotfix branch` 
      - This becomes a ==merge conflicts==. 

```bash
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

- Git has **not automatically** created a **new merge commit**
  - It has **paused** the process while you **resolve the conflict**
    - To **view which files** are **unmerged** at any point after a **merge conflict**
      - Run `git status`.

```bash
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified: index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

- Anything with **merge conflicts** and has **not been resolved** is listed as **unmerged**
  - Git **adds** the **standard conflict resolution markers** to files **with conflicts**
    - Thus **manually open** and **resolve** the conflicts
      - The **file contains** some **section** that *looks as such.*

```bash
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

- The **version** placed under `HEAD` (`master`, as we checked this out under the merge)
  - is the **top part** of the **block**
    - The **version** in the `iss53` branch looks like **bottom**
- To **resolve** the **conflict** $\to$​ choose **one side** / **other** / **merge contents yourself**
  - Example $\to$ may **resolve** by **replacing** this block with:

```bash
<div id="footer">
please contact us at email.support@github.com
</div>
```

- The **resolution** has some **combination** of each
  - The `<<<<<` , `=====`, `>>>>>` have **been completely removed**
    - **After resolving** $\to$ run `git add` on **each file** to **mark as resolved.**
- *Staging* the **file** marks as **resolved.**

---

- Can use a **graphical tool**, `git mergetool` 
  - VS code automatically brings a visual comparison from which u can choose which one to accept.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909170915005.png)

- From when i was doing it the latest is the top, we merge commited from the twat branch
- and chose to edit it by hand and vs code helped show where the changes were present.

---

```bash
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge
p4merge araxis bc3 codecompare vimdiff emerge
Merging:
index.html

Normal merge conflict for 'index.html':
	{local}: modified file
	{remote}: modified file
Hit return to start merge resolution tool (opendiff):
```

- Cover this more in **advanced merging**
- After **leaving merge tool** $\to$​ git asks to **confirm** if the **merge was successful** 
  - Telling the **script** that it was
    - Stages **file to mark** as **resolved for you**
      - Can run `git status` again to **verify** that **all conflicts are resolved**.

```bash
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

    modified: index.html
```

- If **content**
  - Then **verify** that everything that **had conflicts** has **been staged**
    - Type `git commit` to **finalize** the **merge commit**
      - The **commit message** by default **looks as such**

```bash
Merge branch 'iss53'

Conflicts:
    index.html
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#        .git/MERGE_HEAD
# and try again.

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#        modified: index.html
#
```

- Can **modify** the **message** with *details* on **how the merge was resolved**
  - For people who are **viewing the history**
    - What you did **if not obvious**

## Branch Management

- `git branch` is **more** than just **creating / deleting** the **branches**
  - With **no arguments** $\to$ it **list the current branches**

```bash
$ git branch
iss53
* master
testing
```

- The `*` character that **prefixes** the `master` branch.
  - This is the **current checked out** `branch` $\to$ what `HEAD` points to.
    - If you **commit** now then `master` branch **moves forwards** with the **new work**.
- View the **last commit** with `-v` tag

```bash
$ git branch -v
	iss53 93b412c Fix javascript issue
* master 7a98805 Merge branch 'iss53'
	testing 782fd34 Add scott to the author list in the readme
```

- `--merged` / `--no-merged` options **can filter** the **list** to **branches** that you **have not yet merged** to the **current branch**
- run `git branch --merged` to view **already merged branches**

```bash
$ git branch --merged
iss53
* master
```

- You **merged** this `iss53` earlier
  - Therefore in the list.
    - Use the branches with **no `*`** to **delete** as its **often safe**.
    - `git branch -d` as the **work is already incorporated**.

---

- To see **work not incorporated** use `git branch --no-merged`

```bash
$ git branch --no-merged
testing
```

- Shows the **other branch**
  - Contains **work** that is **not merged yet**
    - Attempting to **delete** it with `git branch -d` *fails*.

```bash
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.
```

- If you wish to **force deleting the branch** , force with `-D`.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909180410137.png)

- `git branch --no-merged <branch-name>` analyse which branches are not merged to the `branch-name`

---

### Changing a branch name

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909181339067.png)

- Branch - `bad-branch-name` , wish to change to `corrected-branch-name`.
  - While **keeping history**
    - Also change **remote name** on *Github*.

----

- `git branch --move` 

```bash
$ git branch --move bad-branch-name corrected-branch-name
```

- Replaces `bad-branch-name` with `corrected-branch-name`
  - Change **is local as of now**
    - To let **others view** the **corrected** on the **remote**, `git push`

```bash
$ git push --set-upstream origin corrected-branch-name
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909182311715.png)

- Take **brief look** at where **we are now**

```bash
$ git branch --all
* corrected-branch-name
    main
    remotes/origin/bad-branch-name
    remotes/origin/corrected-branch-name
    remotes/origin/main
```

- Notice you are on `corrected-branch-name` and its **available on remote.**
- The **branch** with the **bad name** is **still present** but can **delete** via **this command**.

```bash
$ git push origin --delete bad-branch-name
```

- Now this **bad name** is **fully replaced** with the **corrected branch name**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210909182618927.png)

- Rename the **local `master`** branch to `main` 

```bash
$ git branch --move master main
```

- There is **no local `master`** branch anymore, as its `main` branch.
- Must **push** this to the **remote too**, makes the **renamed branch** available on the remote.

```bash
$ git push --set-upstream origin main
```

- **Reaches** this **state**.

```bash
git branch --all
* main
remotes/origin/HEAD -> origin/master
remotes/origin/main
remotes/origin/master
```

- The **local** `master` is **gone**
  - Replaced with the `main` branch
  - `main` is **on remote** but `master` still **remains** in the remote.
- Other **collaborators** continue to use `master` branch as **base** of **their work** 
  - Until you **make further changes**.

1. Any **project** depending on **this one** requires an **update** to **code / configuration**
2. **Update** the **test runner config files**
3. **Adjust** *build* and **release scripts**.
4. **Redirect** settings on **repo host** for things like **repo default branch**, **merge rules** and other things **matching branch name**
5. **Update** *references* to the **old branch** in **documentation**
6. **Close** / **Merge** any **pull requests** that are **targeting**.

- After **completion** 
  - And is **sure** that `main` performs **just as** `master`
    - Then **delete** the `master` branch. 

```bash
$ git push origin --delete master
```

## Branching Workflows

### Long Running Branches

- Git $\to$ **three way merge**
  - Merging **from one branch**  into **another multiple times** is **often easy**.
    - Can have **many branches** that are **always open** for **various stages** of the **development cycle.**
      - Can **merge** often from **some** of them **to others**.

---

- Often **git devs** have some **workflow** to **embrace** this **approach**
  - The **only code** that is **entirely stable** is in `master` branch.
    - Code that **has been/will be released**
- A **parallel branch** called `develop` / `next` to work from / use **for stability**.
  - Is **not always stable**, when it reaches that it **will be merged** to `master`.
    - This is used to **pull** in **topic branches** which are *short lived*.
      - When **they are ready** make sure **they pass** all the **test / dont introduce bugs**.

---

- In **reality** $\to$ talking about **pointers**, moving **up the line of commits** 
  - The **stable branches** are **further down** the *line* in the **commit history**
    - The **bleeding branches** are **farther up**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210910211942940.png)

- Easier to **think** about them as **work silos**
  - Where **sets of commits** *graduate* to a **more stable silo** when **fully tested**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210910212223835.png)

- Larger projects may have **proposed updates branch**
  - Has **integrated branches** that are **not ready** for `next / master` branch.
- Each **branch** has **varying** degrees of **stability**. 
  - When they **become more stable** $\to$ `merge` to the **branch above them**.
    - Having **multiple** *long running branches* is **not necessary** 
      - But **can be helpful** for **larger / complex projects**.

---

### Topic Branches

- These are **always useful**, for **short use , small updates/fixes**, often a **singular feature**.
  - Often **too expensive**, but in **git** its **not**.
    - Common too **create/ work / merge** on branches **several times a day**.

---

- Example for `hotfix` / `iss53` 
  - Perform **some commits** and **deleted them** after **merging** to `main` branch.
    - Enables **fast context switching** and **complete**.
      - The **work is separated** to *silos* where **all changes** in the **branch** are **concerned** with a **singular topic**.

- Easier to see what has **happened** with **code review**
  - Keep changes for **however long you like** and `merge` when **ready**
    - **Regardless** of the **order** in which **they were created** / **worked on**.

---

- Example $\to$ work on `master` 
  - **branch** for `iss91` 
    - **branch** another time for **same issue** but **different approach** `iss91v2`
      - Go **back** to `master`. Work here for a **while**
        - Further branch for **potentially bad idea** `dumbidea`

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210910215159396.png)

- Example , say you **prefer** the `iss91v2` 
- Alongside **also liking** the `dumbidea` 
- Can **throw away** the **other** `iss91` branch.
  - Losing commits `C5 / C6` and **merge** in **other two**
    - History **looks as such**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210910215930467.png)

- Read `distributed Git` to see **more workflows**.

> Remember its always local these changes, no communication with the server.

## Remote Branches

- The **remote references** are **references / pointers** in the **remote repositories**
  - Includes:
    1. Branches
    2. Tags
    3. etc....
- Obtain **full list** of every **reference** with `git ls-remote <remote-name>` 
  - or `git remote show <remote-name>` for **remote branches** as well as **more information**
    - Common way is **to take advantage** of **remote-tracking branches**

---

- ==Remote tracking branches== are **references** to **state** of **remote branches**
  - They are **local references** that *you cannot move*.
    - **Git** itself will **move for you** with any **network communication**
      - Ensure the **accurately represent** the **state** of the **remote repository.** 
- They are like **bookmarks**
  - Remind you **where the branches** in the **remote repository** were the **last time** you **connected**.

---

- The **remote tracked branches** take the **name off** `<remote>/<branch>` 
  - To see what **master** on the **origin** looked like **last comms** you would **view** the `origin/master` branch.
    - If you are **working** on some **issue** with a **partner** and they **pushed** up `iss53` branch $\to$ you could **also** have a **local** `iss53` branch
      - But the **server one** is **represented** by the **remote tracking branch** `origin/iss53`.
      - 
