# Git Basics

https://stackoverflow.com/questions/14753603/shortcuts-for-git-commands

- Learn fundamental concepts to do git shit yk.

## Getting a git repository.

- Get via **two main approaches**
  - First takes **an existing project** or **directory** and *imports* it **into Git**
- Second **clones** an **existing git repository** from *another server*

### Initializing a repository in an existing directory

- If you **start** to **track** an **existing** project in **Git**
  - Go to *its directory* then **type**:

```bash
$ git init
```

- create a **new subdirectory** named `.git` contains all the **necessary repository** files
  - A *git repo* *skeleton*
    - There is **nothing** in your *project* is **tracked yet** (refer to **chapter 11** for specific .git descriptions)

- To start **version controlling** existing files
  - As opposed to **an empty directory**
    - should probably **begin tracking** those **files** and **do an initial commit**
- Accomplish with a **few git** `add` commands
  - These can **specify** the files **you want to track**
- Followed by `git commit` 

```bash
$git add *.c
$git add LICENSE
$git commit -m `this is a commment to update changes, i cant take it anymore`
```

- At this **point** $\to$ we have got a **Git repository**
  - with **tracked files** and **an initial commit**

### Cloning an Existing Repository

- To obtain a **copy** of some *existing Git repository*
  - Example $\to$ some **project** you wish **to contribute** t o
    - Require the use **of** `git clone`
- If you are **familiar** with **other VCS systems** like *subversion*
  - Notice the **command** is **clone** and **not checkout**
    - Vital *distinction* , instead of **just a working copy**
      - Git receives a **full copy** of *nearly all data* the *server has*
- Every *version* of **every file** for the **history** of *the project* is **pulled down** as default for `git clone`
  - If the **server disk corrupted** $\to$ use *any of the clones* on **any client** to *set the server back* to that **state** it was in **when it was cloned**
    - May lose **server side hooks**, but all the **versioned data** should be there.

- Clone the repo with `git clone [url]` 
  - Example $\to$ clone **git linkable library** *called* `libgit2` do so as such.

```bash
$ git clone https://github.com/libgit2/libgit2
```

- Creates some **directory** called `libgit2` and **initializes** a `.git` directory inside it
  - Pulls **down all the data** for that **repository**
    - Then **checks** out a **working copy** of the **latest version**.

- Go into **new** `libgit2` directory $\to$ see the **project files** 
  - Ready to be **worked** on or **used**
    - if you want to **clone** the **repository** into some **directory** named something *other* than **other than libgit2**
      - Can **specify** that as **the next command line option**

``` bash
$git clone https://github.com/libgit2/libgit2 mylibgit
```

- does same as before, changing the **target directory**

---

- Git many **transfer protocols** you can use
  - The previous example uses **https:// protocol**
- Alternatives:
  - **git:// **
  - **user@server:path/to/repo.git** $\to$ uses **SSH**

---

## Recording changes to the repository

- There is a **bona fide** *Git repository*
  - Alongside a **checkout** or **working copy** of the **files** for that project
    - Need to **make some changes** and **commit snapshot** of **those changes** into the **repository** each time the **project reaches** a **state** to **record**.

---

- Every file in the **working directory** is **one of two states**
  1. Tracked
  2. Untracked
- **Tracked** $\to$ those in the **last snapshot**
  - Can be **modified** / **unmodified**
  - Can be **staged**
- **Untracked** files is **everything else**
  - Any file in **the working directory** that was **not in the last snapshot**. 
  - And **not** within the **staging area**.

---

- The **first repository clone** , *all your files* will be **tracked** and **unmodified** 
  - As you have just **checked them out** and **not edited anything**.

---

- As you **edit files**
  - Git views as **modified** $\to$ as they are **changed** them since the **last commit**
    - You can **stage** the **modified files** then *COMMIT* all the **staged changes** > rinse and repeat.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709160538565.png)

### Checking the status of your files

- To *determine* which files are in **which state**
  - Use the `git status`
    - Running this **command directly** after a **clone** $\to$ see **something like this**

```bash
$ git status
On branch master
nothing to commit, working directory clean
```

- This means there is a **clean working directory**
  - i.e $\to$ there are **no tracked / modified** files
    - Git also *does not see any untracked files*, or they would be **listed here**
- This command tells us **you which branch** you are on
  - informs you **have not diverged** from the **same branch** on the **server**
    - For now $\to$ that branch is **always master** $\to$ default.
- **Branches / references** $\to$ discussed in detail of chapter 3.

----

- Say you are **adding** some **new file** to the project
  - Simple **README file** > if the **file didn't exist before**
    - Then run `git status` , see the **untracked file** like so.

```bash
$ echo 'My Project' > README
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    README

nothing added to commit but untracked files present (use "git add" to track)
```

- The README IS **untracked**
  - As its under the **untracked files** heading in **status output**
    - untracked means that git sees a file you **didn't have** in the **previous snapshot / commit**
- Git will **not start** including it in **commit snapshots**
  - Unless you **explicitly say so**
    - Does this so you **dont accidentally** begin to include **binary files** or other **incorrect files to track**

### Tracking new files

- use `git add` to **start tracking** some file

```bash
$ git add README
```

- Run the **status command**, see that README is **tracked** and **staged** to be **committed**

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
```

- Know its staged as its under `changes to be committed` heading
  - Commit $\to$ the **version** of the **file** at the *time* you ran **git add** is the **historical snapshot**
- Running `git init` earlier > then ran `git add fileName` 
  - This is our way to **start tracking** all the finals intially in the directory
    - The `git add` takes a **path name** for either a **file / directory**
      - if directory $\to$ command adds **all files recursively** within that directory.

### Staging Modified files

- Change a file that was **already tracked**
  - If you **change** some file being **tracked**
    - Then run `git status` 

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified: benchmarks.rb
```

- Goes under **changes not stages** for *commit*
  - Means its tracked but **modified** in the **working directory** and *not staged after modification*
    - Run `git add` to stage.
- `git add` is **multipurpose** $\to$ use it to **start tracking new files** and **staging files**
  - Also to do *other things* like **marking merge conflicted** files as **resolved**.
    - Think of it as ==add this content to the next commit== rather than ==add to the project== 
- Run `git add` and `git status` to some file.

```bash
$ git add benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   benchmarks.rb
```

- Both files are **staged** to go to the **next commit**
  - If you **modify a file** in the *changes to be committed* then run `git status` again:

```bash
$ vim benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   benchmarks.rb

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   benchmarks.rb
```

- listed as **unstaged** and **staged**
  - Git *stages* a file exactly as it is when you run `git add`
    - If you **commit now** $\to$ you will get the **old version** of benchmarks.rb
      - Must run `git add` again to **stage** the *latest version* of the file.

```bash
$ git add benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   benchmarks.rb
```

### Short status

https://www.git-scm.com/docs/git-status#_short_format

- The `git status` is **comprehensive**
  - Its quite **wordy**
- Git has a **short status flag**
  - To see the **changes** in a *more compact* 
    - running `git status -s` or `git status --short` to obtain a **far more simplified** output from the command.

```c++
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

- New **files** that are *not tracked* have a `??`
- New files that have **been added** to the **staging area** have `A`
- Modified files have a `M` 
- There are **two columns** in the *status codes*
  1. Left hand > indicates the **file is staged** 
  2. Right hand > indicates **modification**
- simplegit.rb therefore has a **modified file** that has been *staged*
- RakeFile has **two versions** of the file one **staged** and a *newer one* that is **unstaged** but *both modified.*

### Ignoring files

- Certain files should be **ignored** when you **automatically** *add* or even **show you** as **being untracked**
  - These are **generally automatically generated** files such as *log files / files* produced by the **build system**
    - Such cases $\to$ create a *file listing patterns* to match them named `.gitignore`
      - exampled.

```c++
$ cat .gitignore
*.[oa] // ignore any files ending .o or .a
*~ // tell git to ignore files ending in tilde, used by text editors to mark temp files
    
 /*
 	may include log , tmp, pid directories.
 	
 	auto generated documentation
 */
```

- setting up .gitignore **before** you *get going* is best idea
  - So you **dont accidentally** *commit files* that you *dont want* in the **Git repository**
    - The *rules* for the patterns you can **place in** this file.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709184343312.png)

- Glob patterns are **simplified regex**
  - Asterisk matches **zero or more characters**
- `[abc]` > matches any character **inside the brackets**
- `?` > matches a **single character**
- `([0-9])` > matches **any character** between them i.e. 0-9 for this.
- can use **two asterisk** `a/**/z` > matches `a/z` , `a/b/z` , `/a/b/c/z`  

---

- Another **.gitignore file**

```bash
# a comment - this is ignored
*.a       # no .a files
!lib.a    # but do track lib.a, even though you're ignoring .a files above
/TODO     # only ignore the root TODO file, not subdir/TODO
build/    # ignore all files in the build/ directory
doc/*.txt # ignore doc/notes.txt, but not doc/server/arch.txt
```

>  **Tip** GitHub maintains a fairly comprehensive list of good .gitignore file examples for dozens or projects and languages at https://github.com/github/gitignore if you want a starting point for your project.

### Viewing staged and unstaged changes

https://stackoverflow.com/questions/2529441/how-to-read-the-output-from-git-diff

- Use `git diff` to see **what has changed** between the *versions*
  - Detailed review later.
- Uses of `git diff`
  - what have you **changed** but **not staged**
  - What have you **staged** and are **about to commit**
- `git status` answers these **generally**
  - `git diff` shows you the **exact lines** that are **added and removed** i.e. the *patch*

---

- Say you edit **README**
  - Then edit the **benchmarks.rb** with **no staging**
    - If you run `git status` command $\to$ see this.

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   benchmarks.rb
```

- To see **what has changed**, type `git diff` with **no other arguments**

```bash
$ git diff
diff --git a/benchmarks.rb b/benchmarks.rb
index 3cb747f..e445e28 100644
--- a/benchmarks.rb
+++ b/benchmarks.rb
@@ -36,6 +36,10 @@ def main
           @commit.parents[0].parents[0].parents[0]
         end

+        run_code(x, 'commits 1') do
+          git.commits.size
+        end
+
         run_code(x, 'commits 2') do
           log = git.commits('master', 15)
           log.size
```

- Compares the **working directory** to the **staging area**
  - The *result* tells you **the changes you have made** but have **not staged**
- To view what you have **staged** to go into **next commit**
  - use `git diff --staged` , compares the **staged changes** to the **last commit**

```bash
$ git diff --staged
diff --git a/README b/README
new file mode 100644
index 0000000..03902a1
--- /dev/null
+++ b/README
@@ -0,0 +1,4 @@
+My Project
+
+ This is my project and it is amazing.
+
```

- Note that `git diff` by itself does **not show all changes** since the **last commit**
  - Only changes that are **still unstaged** (add --staged to view this)
    - This can be **weird**, as if you have **staged all changes**, `git diff` passes **no output**
- Another example $\to$ staging benchmarks.rb
  - Then **edit**
    - Use `git diff` to **see changes** in the file that are **staged** and **changes** that are *unstaged*. 

> - Git Diff in an **External Tool**
>   We will continue to use the git diff command in various ways throughout the rest
>   of the book.
> -  There is another way to look at these diffs if you prefer a graphical or
>   external diff viewing program instead. If you run git difftool instead of git diff,
>   you can view any of these diffs in software like emerge, vimdiff and many more
>   (including commercial products). 
> - Run git difftool --tool-help to see what is
>   available on your system.

```bash
$ git add benchmarks.rb
$ echo '# test line' >> benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified: benchmarks.rb

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified: benchmarks.rb
```

- use `git diff` 

```bash
$ git diff
diff --git a/benchmarks.rb b/benchmarks.rb
index e445e28..86b2f7c 100644
--- a/benchmarks.rb
+++ b/benchmarks.rb
@@ -127,3 +127,4 @@ end
 main()

 ##pp Grit::GitRuby.cache_client.stats
+# test line
```

- use `git diff --cached` to view what has **staged so far**

```bash
$ git diff --cached
diff --git a/benchmarks.rb b/benchmarks.rb
index 3cb747f..e445e28 100644
--- a/benchmarks.rb
+++ b/benchmarks.rb
@@ -36,6 +36,10 @@ def main
          @commit.parents[0].parents[0].parents[0]
        end

+        run_code(x, 'commits 1') do
+          git.commits.size
+        end
+
        run_code(x, 'commits 2') do
          log = git.commits('master', 15)
          log.size
```

### Committing your changes

- The **staging area** now set up *correctly*
  - Must **commit your changes**
    - Recall that its **still unstaged** $\to$ any *file* that you **have created** or **modified** and not `git add` ran on it since edited will **not commit**
- They stay **modified values** on the disk
  - For this case $\to$ say the last time you ran `git status` 
    - Saw its **all staged** $\to$ thus **ready to commit**
- simply enter `git commit` 

```bash
$ git commit
```

- This launches **editor of choice**
  - This is the **shells** $EDITOR environment variable, often vim. configure with: 

```bash
git config --global core.editor
```

- Editor displays **following text**

```bash
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#       new file:   README
#       modified:   benchmarks.rb
#
~
~
~
".git/COMMIT_EDITMSG" 9L, 283C
```

- See the **default commit message** contains *latest output* of `git status` commented out and **one empty line at top**
-  Can *remove* the comments and **type the commit message**
  - can leave them there to help you **remember** what's being committed (more **explicit reminder** of what you have **modified**, add `-v` option to `git commit` , this puts the **diff** of the **change** in the **editor** to see **exact changes** that are **being committed**) 

- Exit the **text editor**
  - Git **creates your commit** with that **commit message**
    - With the **comments** and **diff stripped**
- Alternatively $\to$ type the **commit message** *inline* with the **commit** command, specifying an `m` flag:

```bash
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```

- Created the first **commit**
  - This gives you some **output**
    1. Which **branch** you have *committed* to **master**
    2. SHA-11 checksum the commit has `463dc4f`
    3. **Number** of *files* that *changed*
    4. Stats on **lines added** and **removed**
- Commit **records** the *snapshot* you set up in the **staging area**
  - Anything you *did not stage* is **still there modified** 
    - Perform **more commits** to *add to the history*
- Every time you **commit** $\to$ you *record* a **snapshot** of the **project** that you can *revert* to *later*.

### Skipping the staging area

- Sometimes staging area is **too complex** for our needs
  - Wish to **skip** via adding `-a` to `git commit` 
    - This automatically **adds** all the **files** and *commits* as long as the files are tracked.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified: benchmarks.rb

no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
```

### Removing Files

- To **remove files** from *Git*
  - Must *remove* from **tracked files** (remove from **staging area**)
    - Then **commit**
- `git rm` does this, this also **removes** from the **working directory** so you *do not see it* as an **untracked file** for *next time*
  - If you just **remove a file** from the **working directory**
    - Shows under **changed but not updated** *unstaged* area of your `git status` output.

```bash
$ rm grit.gemspec
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    deleted: grit.gemspec

no changes added to commit (use "git add" and/or "git commit -a")
```

- use `git rm <files> ` to **stage** the **removal** of some file

```bash
$ git rm grit.gemspec
rm 'grit.gemspec'
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    deleted: grit.gemspec
```

- The **next commit** $\to$ file is **gone** and **no longer tracked**
  - If you **modified the file** and *added* to the **index already** 
    - force the removal with **-f option**.

https://stackoverflow.com/questions/8130954/when-is-git-rm-f-used

- This is a **safety feature** to *prevent accidental removal* of data
  - that has **not yet been recorded** in a **snapshot** and *cannot be recovered from git*

---

- Useful to **keep** the **file** in your **working tree** but **remove** from the **staging area**
  - in other words $\to$ keep on the **hard drive** but **not let it be tracked.**
    - Useful if you **forgot** to add something to **.gitignore** and *accidentally added*
      - Like a **large log file** or lots of **compiled files**
        - use the ``--cached`` option.

```bash
$ git rm --cached README
```

- Can pass **files, directories, file-glob** patterns to `git rm` 
  - Do things like:

```bash
$ git rm log/\*.log
```

- Note the **backslash** in front of ***** 
  - This is because **Git** does its **own filename expansion** next to the **shells**
    - This command **removes** all files that have **.log** extension in **log/** directory.

- Alternatively:

```bash
$ git rm \*~
```

- Removes all files that end with `~`

### Moving files

- Git does **not explicitly track** any *file movement*
  - If you **rename** a *file* in Git $\to$ there is **no metadata** stored to mention the **rename**
    - Git is smart on figuring out after the fact, deal later.

- Rename file using `git mv`

```bash
$ git mv file_from file_to
```

- Works perfectly, if you **run something** like this and look at **status**
  - Git considers it a **renamed file**

```bash
$ git mv README.md README
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed: README.md -> README
```

- This is the **same** as this:
- https://stackoverflow.com/questions/7434449/why-use-git-rm-to-remove-a-file-instead-of-rm

```bash
$ mv README.md README
$ git rm README.md
$ git add README
```

- Git works out its an **implicit rename**
  - Does *not matter* if you **rename a file** that way or **with mv** command
    - The difference is just the `git mv` command is **one** instead **of three**

- Can use **any tool** you wish to **rename a file**
  - Then address the **add / rm later** before the *commit*

### View commit history

- After creating **several commits**
  - Or if you have **cloned a repo** with some *existing commit history* $\to$ possibly want to **look back** to view **what has happened**

- most fundamental tool is `git log` 

---

- These examples use a **project called** *simplegit*, obtain the project :

```bash
git clone https://github.com/schacon/simplegit-progit
```

```bash
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
```

- With **no arguments**

  - `git log` lists the **commits** made in the **repository** in *reverse chronological order*
    - The **most recent commits** will *show first*
      - This **command lists** each of the **SHA-1 checksums**, author email/name , date written and commit message.

  #### Options

  - `-p` > shows the **difference** between each commit
  - use `-2` > limits output the **last two entries**

```bash
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number

diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  =   Gem::Platform::RUBY
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gee-mail.com"
     s.summary   =   "A simple gem for using Git in Ruby code."

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
     end

 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
\ No newline at end of file
```

- This **option** displays the **same information** however with a `diff` directly **following each entry**
  - Useful for **code review** $\to$ or *quickly browsing* what **happened** during a *series* of **commits** that some **collaborator has added**
- Use a **series** of *summarising* options 
  - Example $\to$ `--stat` option

```bash
$ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number

 Rakefile | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

 lib/simplegit.rb | 5 -----
 1 file changed, 5 deletions(-)

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit

 README           |  6 ++++++
 Rakefile         | 23 +++++++++++++++++++++++
 lib/simplegit.rb | 25 +++++++++++++++++++++++++
 3 files changed, 54 insertions(+)
```

- With this option $\to$ prints **below** each **commit entry** with a
  1. **list of modified files**
  2. How **many files were changed**
  3. how many **lines** in those files **were added / removed**
  4. Places **summary** of **info** at the **end**
- `--pretty` $\to$ changes the log output to **formats** other than default
  - Few **prebuilt** options available to use
    - The `oneline` option prints **each commit** on *one line* $\to$ useful if there are **lots of commits**
    - `short, full, fuller` options **show the output** in *roughly* the **same format** with *less / more information*

```bash
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the verison number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
```

- `format` can be used to **specify** a *custom log output format*
  - useful when **generating** output for *machine parsing*
    - Because you **specify** the **format explicitly**
      - Know it *wont change* with **updates to Git**

```bash
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number
085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
a11bef0 - Scott Chacon, 6 years ago : first commit
```

- List of some more useful options that **format takes**

| Option | Description of Output                          |
| :----- | :--------------------------------------------- |
| %H     | Commit hash                                    |
| %h     | Abbreviated commit hash                        |
| %T     | Tree hash                                      |
| %t     | Abbreviated tree hash                          |
| %P     | Parent hash                                    |
| %p     | Abbreviated parent hash                        |
| %an    | Author name                                    |
| %ae    | Author e-mail                                  |
| %ad    | Author date (format respects the –date= option |
| %ar    | Author date, relative                          |
| %cn    | Committer name                                 |
| %ce    | Committer e-mail                               |
| %cd    | Committer date                                 |
| %cr    | Committer date, relative                       |
| %s     | Subject                                        |

- What is difference in **author / committer**
  - The **author** $\to$ is **who originally wrote** the *work*
    - The *committer* is the person **who last applied the work**
- If you **send in a patch** to some *project*
  - One of the **core members** *applies the patch* $\to$ both **of you get credit** , you are author, he is committer.
- `oneline` and `format` are useful with **another log option** called `--graph` 
  - Adds a nice little **ASCII** graph that shows **branch** and **merge history**.

```bash
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
|\
| * 420eac9 Added a method for getting the current branch.
* | 30e367c timeout code and tests
* | 5a09431 add timeout protection to grit
* | e1193f8 support for heads with slashes in them
|/
* d6016bc require time for xmlschema
*  11d191e Merge branch 'defunkt' into local
```

- Table of **common formatting options** that may be **useful**

| Option          | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| -p              | Show the patch introduced with each commit.                  |
| --stat          | Show statistics for files modified in each commit.           |
| --shortstat     | Display only the changed/insertions/deletions line from the --stat command. |
| --name-only     | Show the list of files modified after the commit information. |
| --name-status   | Show the list of files affected with added/modified/deleted information as well. |
| --abbrev-commit | Show only the first few characters of the SHA-1 checksum instead of all 40. |
| --relative-date | Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format. |
| --graph         | Display an ASCII graph of the branch and merge history beside the log output. |
| --pretty        | Show commits in an alternate format. Options include oneline, short, full, fuller, and format (where you specify your own format). |

### Limiting Log Output

- `git log` takes a **number** of **useful limiting options** 
  - Display just a **subset** of **commits**
- `-2` option performs this $\to$ display **last two commits**.
  - Can do `-<n>` where `n` is **any integer** to show the **last** `n` commits
    - REALITY $\to$ unlikely to **use that often**, Git default $\to$ **pipes** all the **output** through a **paper** so you need **one page of log** *output* at a **time**

---

- Time limiting options $\to$ `--since` / `--until` are **very useful**
  - obtain commits in the **last two weeks**

```bash
$ git log --since=2.weeks
```

- Works with **various formats**
  - Can specify a **specific date** such as **"2008-01-15"** or **relative date** **2 years 1 day and 3 minutes ago**
    - Can *filter* the **list** to **commits** that **match some criteria**
- `--author` options $\to$ allows filter **on specific author**
- `--grep` options $\to$ lets you **search** for **keywords** in the **commit message.**

> - You can specify **more than one instance** of both the `--author` and `--grep` search
>   criteria, which will limit the commit output to commits that match any of the
>   `--author` patterns and any of the --grep patterns; 
> - however, adding the `--all-match`
>   option further limits the output to just those commits that match all `--grep`
>   patterns.

- `-S` $\to$ takes a **string** and shows **only those** commits that **changed** the **number** of **occurrences** for *that string*
  - instance $\to$ if you wish to **find the last commit** that *added / removed* a **reference** to a **specific function**
    - Can **call**.

```bash
$ git log -S function_name
```

- Look for specific code **blocks for example**

---

- Final option is to **filter a path**
  - If you **specify a directory** or **file name**
    - Limit the **log output** to commits that **introduced** a **change** to **those files**
- This is the **final option** in any git logs , often preceded by **double dashes** `--`  to **seperate** paths from **options**

```bash
$ git log -- path/to/file
```

- Other common options for **git log**

| Option            | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| -(n)              | Show only the last *n* commits                               |
| --since, --after  | Limit the commits to those made after the specified date     |
| --until, --before | Limit the commits to those made before the specified date    |
| --author          | Only show commits in which the author entry matches the specified string |
| --committer       | Only show commits in which the committer entry matches the specified string |
| --grep            | Only show commits with a commit message containing the string |
| -S                | Only show commits adding or removing code matching the string |

- Example $\to$ if you wish to **see** the **commits** that *modify* the **test files** in the **Git source code history**
  - That were **committed** by **junio Hamano** in *October 2008* and are **not merge commits**
    - Run something as such $\to$ 

```bash
$ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \
--before="2008-11-01" --no-merges -- t/
5610e3b - Fix testcase failure when extended attributes are in use
acd3b9e - Enhance hold_lock_file_for_{update,append}() API
f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
51a94af - Fix "checkout --track -b newbranch" on detached HEAD
b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn
branch
```

- out of **40,000** commits $\to$ the command shows the **6** that **match**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210714145252253.png)

## Undoing things

- Common undo $\to$ **committing** *too early*, possibly **forgetting** to add **some files** / *Incorrect commit message*
  - To **redo** that *commit* $\to$ make the **additional** changes you **forgot** $\to$ *stage* $\to$ commit with `--amend`

```bash
$ git commit --amend
```

-  Takes the **staging area** and **uses** it for the **commit**
  - If there are **no changes** since the **last commit** (run *immediately* after **first commit**)
    - Then **the snapshot** will *look the same* $\to$ all that changes is the **commit message**

---

- Same commit message editor opens $\to$ contains the **message** of the **previous commit**
  - Can **edit** the **message** as *always*, just **overwrites** the *previous commit*.

----

- Example $\to$ if you **commit** and *then realize* you **forgot** to the **changes** in a **file you wanted**
  - To *add* to **this commit**
    - Do something as **such**:

```bash
$ git commit -m 'Initial commit'
$ git add forgotten_file
$ git commit --amend
```

- End **with a single commit**
  - The *second commit* just **replaces** the *results* of the *first*.

> - It’s important to **understand** that when you’re **amending your last commit**, you’re
>   not so much **fixing it** as **replacing** it **entirely with a new**, improved commit that
>   pushes the old commit out of the way and puts the new commit in its place.
> - Effectively, it’s as if the **previous commit never happened**, and it won’t show up in
>   your repository history.
> - The obvious value to amending commits is to make minor improvements to your
>   last commit, without cluttering your repository history with commit messages of
>   the form, **“Oops, forgot to add a file” or “Darn, fixing a typo in last commit”.**

> - Only **amend commits** that are **still local** and **have not been pushed somewhere**.
>   Amending **previously pushed commits** and **force pushing** the **branch** will cause
>   **problems for your collaborators**.
> -  For more on **what happens** when you do this and
>   **how to recover** if you’re on the **receiving end read The Perils of Rebasing**

### Unstaging a Staged file

- Here is **demonstrated** how to work with **your staging area** / **working directory** *changes*
  - Command used to **determine** the *state* of **two areas** , reminds you also **how to undo changes**
- Example $\to$ say you **changed two files** and you **wish to commit** as *two seperate changes*
  - Accidentally typed `git add *`  to **stage both**
    - How to **unstage** *one of the two*

```bash
$ git add *
$ git status
On branch master
Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)
	
	renamed: README.md -> README
	modified: CONTRIBUTING.md
```

- Below the **changes to be committed**
  - Tells you to use `git reset HEAD <file>...` to **unstage**
    - Use this to **unstage** the **CONTRIBUTING.md**

```bash
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)
	
		renamed: README.md -> README
		
Changes not staged for commit:
	(use "git add <file>..." to update what will be committed)
	(use "git checkout -- <file>..." to discard changes in working directory)
	
		modified: CONTRIBUTING.md
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210714151851668.png)

- This is all you need to **know** about the `git reset` 
  - Go into **more detail** about `reset` does and **how to master**
    - In **Reset Demystified**

---

### Unmodifying a Modified File

- Say you **do not want to keep** the changes to `CONTRIBUTING.md` 
  - How do you **easily** *unmodify it* $\to$ **revert back** to what **it looked like** to *the last commit* (or **initially cloned** / however > working directory)
- `git status` $\to$ tells you **how to do this**
  - In the **last example** $\to$ the **unstaged area** looks as such:

```bash
Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
		modified: CONTRIBUTING.md
```

- Tells you clearly *how to discard changes*

```bash
$ git checkout -- benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed: README.md -> README
```

- See the **changes have been reverted**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210714152826311.png)

- If you wish **to keep changes** that are *made* to the **file** but need to **keep it out of the way** 
  - Go over **stashing / branching** in  **Git Branching**
    - There are often **better approaches**
- Anything that is **committed** in Git can **almost** *always* be **recovered**
  - Even the commits that were **on branches** that were **deleted** or **commits** that were **overwritten** with `--amend`
    - Can **be recovered**
- Anything you **lose** that was **never committed** is *likely* to **never be seen again**

---

### Undoing With git restore

- New command called `git restore` 
  - Basically an **alternative** to `git reset` that was **covered**.

- Git since 2.33.0 uses `git restore` instead of **git reset**

- RETRACE our steps and **undo things** with `git restore` rather than `git reset` 

#### Unstaging a staged file git restore

- Demonstrate how **to work** with the **staging area** and **working directory** with `git restore`
  - Neat part is $\to$ the **command** you use to **determine the state** of **those two areas** also *reminds* you how **to undo changes them**

- Say you **changed two files** and wish to **commit them** as *two seperate changes*
  - Accidentally type **git add *** and stage *both*, how to **unstage one of the two**
    - `git status` commands **reminds you**

```bash
$ git add *
$ git status
On branch master
Changes to be committed:
	(use "git restore --staged <file>..." to unstage)
	
        modified: CONTRIBUTING.md
        renamed: README.md -> README
```

- Below the **changes to be committed**
  - Use the `git restore --stage <file>...`  to **unstage**
- Use the **advice** to **unstage** the `CONTRIBUTING.md` 

```bash
$ git restore --staged CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
        renamed: README.md -> README
        Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
        modified: CONTRIBUTING.md
```

- Modified but **once** again **unstaged**

#### Unmodifying a Modified File with git restore

- If you **dont want to keep** the *changes* to `CONTRIBUTING.md` 
  - How to **easily unmodify it** $\to$ *revert* back to what it looked when **you last committed**

```bash
Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
	modified: CONTRIBUTING.md
```

- Tells you **pretty explicitly** how to **discard** the **changes** you *made*
  - Do as it says:

```bash
$ git restore CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
    	renamed: README.md -> README

```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210714160211195.png)

## Working With Remotes

- To **collaborate** on any Git project $\to$ manage **remote repositories**
- Can have **several remote repositories** 
  - Either **read-only** or **read/write** for you

- Collaboration **involves** the **management** of these **remote repos** 

  - Alongside **pushing** and **pulling** data *to/from* when **sharing work**

- Management of remote repos includes:

  1. **adding** *remote repositories*
  2. **remove** *remotes* that are **no longer valid**
  3. **Manage** *various branches* 
     - Define them as **tracked / not**

  - etc......

- Section $\to$ cover **some** of these skills

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210714161542766.png)

### Showing Your Remotes

- List which **remote servers** you *have configured*
  - run `git remote` $\to$ lists **short names** of *each remote handle*

- If you **cloned your repository**
  - Should at least see **origin** from the *server* you **cloned from**

```bash
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
$ cd ticgit
$ git remote
origin
```

- `-v` $\to$ shows the **URLs** that Git has stored for **short name** used when **reading / writing** to *that remote*

```bash
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
```

- If there is **multiple** *than one remote*
  - The **command** will *list them all*
    - Example $\to$ with **multiple remotes** for *working* with **several collaborators** may *look something like this*

```bash
$ cd grit
$ git remote -v
bakkdoor https://github.com/bakkdoor/grit (fetch)
bakkdoor https://github.com/bakkdoor/grit (push)
cho45 https://github.com/cho45/grit (fetch)
cho45 https://github.com/cho45/grit (push)
defunkt https://github.com/defunkt/grit (fetch)
defunkt https://github.com/defunkt/grit (push)
koke git://github.com/koke/grit.git (fetch)
koke git://github.com/koke/grit.git (push)
origin git@github.com:mojombo/grit.git (fetch)
origin git@github.com:mojombo/grit.git (push)
```

- Can **pull contributions** from *any user* quite **easily**

- May additionally have **permission** to *push* to **one or more** of these
  - Though we **can't tell there**
- These **remotes** use a **variety of protocols**
  - Cover more in Server section of this book.

### Adding Remote Repositories

- Mentioned / Given **some demonstrations** of `git clone` command **implicitly** adding the **origin** *remote for you.*
  - Here is **how** to add a **new remote explicitly.**
    - Add a **new remote** `Git` **repository** as a *shortname* $\to$ you **can reference** easily

```bash
git remote add <shortname> <url>
```

```bash
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
pb https://github.com/paulboone/ticgit (fetch)
pb https://github.com/paulboone/ticgit (push)
```

- Can use the **string** `pb` $\to$ in **lieu** of the **entire URL**
  - Example to fetch **all information** that **paul** has **but** you *dont yet have* in **your repository.**

- run `git fetch pb` 

```bash
$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
    * [new branch] master -> pb/master
    * [new branch] ticgit -> pb/ticgit
```

- Pauls **master** branch now **accessible** locally as `pb/master` 
  - Can **merge** it into **one** of **your branches**
    - Or you can **check** out some **local branch** at that point if you wish to **inspect it**

### Fetching and Pulling from Your Remotes

- To **obtain data** from the **remote projects**
  - Run `git fetch <remote>` as seen before. 
- Command goes to the **remote project** and **pulls** down **all the data** from that **remote project**
  - That you *don't have yet* $\to$ After doing this $\to$ should have **references** to **all** the **branches** from *that remote*
    - Can **merge / inspect** at any time.

- **Cloning** a *repository* $\to$ the *command* **automatically** adds that **remote repository** under the name `origin` 
  - This is why `git fetch origin` adds **any new work** that has been **pushed** to the **server** since **last clone**

![Git Fetch vs Git Pull. I noticed that many people are facing… | by Sabbir  Hossain | Medium](https://miro.medium.com/max/600/1*SKR0Zz4S0M_0Rp-aPsZw0Q.png)

- Git fetch vs Git pull.

---

- `git fetch` does **not automatically merge** , just downloads the data to the **local repository.** 
  - Must perform a **manual merge**.

---

- If the **current branch** is configured to **track a remote branch**
  - Use `git pull` to **automatically** perform **fetch / merge** process. 
    - Merge **remote** branch to **current** branch

- May be **easier** / **comfortable** workflow for you
  - By default $\to$ `git clone` automatically sets up the **local *master* branch** to *track* the *remote* **master** (*insert default name branch...*)
    - From the **server** that it was **cloned from** 
- `git pull` often **fetches** data from the **server** that was **originally cloned**
  - Will **automatically** attempt a ***remerge*** into the **code** that is **currently** being **worked on**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210714183012415.png)

### Pushing to Your Remotes

- Project a point to share
  - Must **push upstream** 
- Command is `git push <remote> <branch>` 
  - To push the `master` branch to `origin` server
    - Again **cloning** generally sets up **both of those names** for you **automatically**

```bash
git push origin master
```

- Only works if you **cloned from a server** from which you **have write access** 
  - And if **nobody** has **pushed** in the *meantime*
    - If someone else **clones at the same time** and they **push upstream** > *your* push is **rejected.**

- Must **fetch** the *work first* and **incorporate** into yours **before** you are **allowed to push**
  - See more in Git Branching.

### Inspecting a Remote

- View **more information** on *some particular remote*
  - use `git remote show <remote>` 

```bash
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

- Lists URL of **remote repo**
- Command notifies if you are on the **master branch** $\to$ running **git pull** 
  - Will automatically **merge** the **remote master** branch into the **local one** after fetching.
- List remote **references** that are **pulled down**

---

- Using more Git more heavily
  - More information from `git remote show`  shown here.

```bash
$ git remote show origin
* remote origin
  URL: https://github.com/my-org/complex-project
  Fetch URL: https://github.com/my-org/complex-project
  Push  URL: https://github.com/my-org/complex-project
  HEAD branch: master
  Remote branches:
    master                           tracked
    dev-branch                       tracked
    markdown-strip                   tracked
    issue-43                         new (next fetch will store in remotes/origin)
    issue-45                         new (next fetch will store in remotes/origin)
    refs/remotes/origin/issue-11     stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    dev-branch merges with remote dev-branch
    master     merges with remote master
  Local refs configured for 'git push':
    dev-branch                     pushes to dev-branch                     (up to date)
    markdown-strip                 pushes to markdown-strip                 (up to date)
    master                         pushes to master
```

- Shows **which branch** is *automatically* pushed when running **git push** on **certain branches*
- Shows the **remote branches** on the *server* which **you dont own.** 
- Shows the **remote branches** that you **have** but are **removed** from the **server**
- Multiple local branches are that are **able to merge** *automatically* with *their* **remote tracking branch** when you run `git pull` 

### Renaming and Removing Remotes

- using `git remote rename` to change the **remotes shortname**
  - Instance $\to$ to rename `pb` to `paul` $\to$ do so with `git remote rename`

```bash
$ git remote rename pb paul
$ git remote
origin
paul
```

- Changes all the **remote tracking branch names**
  - `pb/master` > `paul/master` 

- To **remove a remote for some reason**
  - You probably moved the server or are **no longer using mirror**
    - Someone is not contributing anymore

- use `git remote remove` or `git remote rm`. 

```bash
$ git remote remove paul
$ git remote
origin
```

- After deleting the **reference** to a **remote**
  - All **remote tracking branches** and **config settings** associated with that **remote** are **also deleted**.

## Tagging

- Can **tag** *specific points* in a **repository's history** as being *vital*
  - Often used to **mark release points** (v 1.0 / v 2.0)

### Listing Tags

- Listing the tags in Git

```bash
$ git tag
v1.0
v2.0
```

- Lists in **alphabetical order** 
  - The order in **which** they are **displayed** has **no importance**
- Search for tags that **match some pattern**
  - Git source repo has **over 500 tags**
    - If you are interested in **1.8.5** series:

```bash
$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5.5
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210718174913835.png)

### Creating Tags

- There are **two types of tags**
  1. LIGHTWEIGHT
  2. ANNOTATED
- **lightweight** is like a **branch** that *does not change*
  - Just some **pointer** to a **specific commit**
- **annotated** $\to$ stored as **full objects** within in GIT DATABASE
  - They have a **check sum** / **tagger name** / **email** / **date** / **tagging message**
  - Can be **signed / verified** with GNU Privacy Guard
- Recommended to **create *annotated tags*** to store more **useful information**
  - To  create basic **temp tags** just use lightweight.

### Annotated Tags

- Creating **annotated tag** in GIT is **simple**
  - Easiest method is the `-a` when running the `tag` command

```bash
$ git tag -a v1.4 -m "my version 1.4"
$ git tag
v0.1
v1.3
v1.4
```

- `-m` specifies the **tagging message** > stored with the tag
  - If you **dont specify** a **message** for an **annotated tag**
    - GIT launches **the editor** to **type in**
- Can view the **tag data along** with the **commit** that was tagged using **git show**

```bash
$ git show v1.4
tag v1.4
Tagger: Ben Straub <ben@straub.cc>
Date:   Sat May 3 20:19:12 2014 -0700

my version 1.4

commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number
```

- Shows **tagger information**
- Shows **data commit** was **tagged**
- Shows **annotation message** before showing the **commit information**

### Lightweight Tags

- The **commit checksum** stored in a **file** is all this is
- Create this by **not supplying** any `-a` `-s` `-m `flag
- Just provide **tag name**

```bash
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
```

- This time $\to$ running `git show` on the tag
  - Do not **see the extra information**
    - Command just **shows the commit** 

``` bash
$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number
```

### Tagging Later

- Tag commits after you **moved past them**
  - Commit history:

```bash
$ git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 Create write support
0d52aaab4479697da7686c15f77a3d64d9165190 One more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc Add commit function
4682c3261057305bdd616e23b64b0857d832627b Add todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a Create write support
9fceb02d0ae598e95dc970b74767f19372d61af8 Update rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc Commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a Update readme
```

- Suppose you **forget** to **tag** the **project** at `v1.2` 
  - Which was **update rakefile** commit 
    - Add it after the fact
- To tag $\to$ specify its **commit checksum**
  - At the **end** (can identify with **partial checksum **)

```bash
$ git tag -a v1.2 9fceb02
```

- Now see you tagged the commit

```bash
$ git tag
v0.1
v1.2
v1.3
v1.4
v1.4-lw
v1.5

$ git show v1.2
tag v1.2
Tagger: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Feb 9 15:32:16 2009 -0800

version 1.2
commit 9fceb02d0ae598e95dc970b74767f19372d61af8
Author: Magnus Chacon <mchacon@gee-mail.com>
Date:   Sun Apr 27 20:43:35 2008 -0700

    updated rakefile
...
```

### Sharing Tags

- Default $\to$ `git push` does **not transfer** the *tags* to **remote servers**
- Must explicitly **push tags** to some **shared server** after creation.
- run `git push origin <tagname>` 

```bash
$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -> v1.5
```

- If there are **lots of tags** to push **at once**
  - use `--tags` origin to `git push` command
    - This **transfers** all the **tags** 

```bash
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -> v1.4
 * [new tag]         v1.4-lw -> v1.4-lw
```

- When someone else **clones / pulls** from the repo
  - They collect the tags also

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210718184310735.png)

### Deleting Tags

- To **delete a tag** on the **local repo**
  - Use `git tag -d <tagname>` 
    - Example removal of **lightweight** tag above as follows

```bash
$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
```

- This **does not remove** the tag from any **remote server**
- The *first variation* is `git push <remote> :refs/tags/<tagname>:` 

```bash
$ git push origin :refs/tags/v1.4-lw
To /git@github.com:schacon/simplegit.git
- [deleted] v1.4-lw
```

- Interpret above is via **reading** it as the **null value** before the **colon**
  - This is being **pushed** to the **remote tag name**, thus deleting
- Second more intuitive.

```bash
$ git push origin --delete <tagname>
```

### Checking out Tags

- View versions of the **files** some tag is **pointing to**
  - Done via `git checkout` of that tag
- This places the **repo** in a **detached HEAD** state
  - Has some **ill side effects**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210718185138894.png)

- Within **detached HEAD state**
  - If you **make changes** $\to$ create **commit** $\to$ the tag **stays the same**.
    - The **new commit** will *not belong* to any **branch** and is **unreachable** except by the **same commit hash**

- If you need to **make changes** $\to$ such as fixing a **bug** or **older version**
  - For instance $\to$ generally want to **create  branch**

```bash
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```

- Do this and **make a commit** 
  - The **version2** branch is *slightly different* than the **v2.0.0** tag since it **moves forward** with the **new changes**
    - Therefore be careful.

https://stackoverflow.com/questions/57265785/whats-the-difference-between-git-switch-and-git-checkout-branch

https://stackoverflow.com/questions/14613540/do-git-tags-only-apply-to-the-current-branch/27154277#:~:text=A%20tag%20is%20a%20pointer,commits%20exist%20independently%20of%20branches.&text=That%20commit%20can%20be%20pointed,number%20of%20branches%20%2D%20including%20none. 

## Git Aliases

- Aliases make the git experience more simple

---

- Git does not automatically infer the command if you **partially type it in**
  - To not have to type the entire text $\to$ set up an alias for each command in the `git config` command.

```bash
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

- add custom unstage alias

```bash
git config --global alias.unstage 'reset HEAD --'
```

- useful to view most recent commit

```bash
git config --global alias.last 'log -1 HEAD'
```





