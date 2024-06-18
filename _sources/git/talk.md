<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
<style>
pre.code {
    color: #000000;
    background: gruvbox-light;
    border-radius: 5px;
    font-size: 0.8em;
    padding: 15px;
}
span.red {
   color: red;
}
span.green {
   color: lightgreen;
}
span.cyan {
   color: cyan;
}
span.commit {
   color: orange;
}
</style>
# Introduction to git

BB1000 Programming in Python
KTH

---

## Learning objectives

* What version control is

* Why we should use version control

* Why we choose use git

* Know about the basic components in git
    - work directory
    - staging area
    - repository (local, remote)
    - branches

* Know commands for basic git workflow
    - How to save history
    - How to undo mistakes
    - How to collaborate with others (github)

---

## What is version control:

* A Version Control System (VCS) is a frameorks that tracks the history of a project

* The history of a project is a sequence of versions

* At any point in time we can go back to a previous version

* A VCS allows you to compare different versions

* A VCS allows several users to work on the same project files simultaneously

---

## Why use version control

You may already have used some manual version...

```
$ cp -r Project Project.save
```

... some work ...

```
$ cp -r Project Project.save.v2
```

... some work ...

```
$ cp -r Project Project.save.v2.new
```

---

<center>
<img src="http://www.phdcomics.com/comics/archive/phd101212s.gif" height="500"/>
</center>

---

## Introducing git

<center>
<img src="http://www.linux.com/images/stories/714/Linus-Torvalds-LinuxCon-Europe-2014.jpg" height="250" />
</center>


* Written by Linus Thorvalds, originally for the Linux kernel
* A distributed Version Control System
* Several servers have all information
* Any one can be chosen as the reference version
* One of the most popular frameworks today (others: bazaar, mercurial)

* Historical frameworks (subversion, CVS, RCS)

---

## When to use

### Scenario

* For source code development
* For manuscripts
* In single-user projects
* In collaborative projects

### Benefits

* No history is lost
* All versions of your documents are preserved
* Easy to backup to other sites

---

## Concepts

* Work directory: local directory where you work
* Cache: temporary area for files you intend to keep
* Commit: a snapshot of the project files at a point in time
* Repository: sequence/tree of commits (history of the project)
* Branch: an alias for a commit (often the latest along a line of development)

---

## 11 basic commands

* Initialize
   - clone
   - init
* Queries
   - status
   - log
   - diff
* Changing locally
   - add
   - commit
   - merge
* Remote interaction
   - push
   - fetch
   - pull

---

## Setup

### The first time around

```
    $ git config --global user.name "First Last"
    $ git config --global user.email "first.last@isp.com"
    $ git config --global init.defaultBranch main
```

* creates a configuration file ``~/.gitconfig``
* git wants to know who you are
* let "main" be the default branch name in new projects

```
    [user]
	name = First Last
	email = first.last@isp.com
```

*Note*:    You can create and edit the file directly

---

## Initializing 

* a new empty repository

~~~
$ git init proj
~~~
<pre>
    proj/
    └── .git
</pre>

* an existing project folder

~~~
$ cd oldproj
$ git init
~~~

<pre>
    oldproj/
    └── .git
</pre>

---

## Check status

<pre>
proj/
└── .git
</pre>


* Check repository status

```
$ git status
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

---
## Create a new file

<pre>
proj
├── .git
└── <text style="color: red;">hello.py</text>
</pre>


```
#hello.py
print("Hello world!")
```

* Recheck status

<pre>
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

   <text style="color: red;">hello.py</text>

nothing added to commit but untracked files present (use "git add" to track)
</pre>

* Git warns about *untracked* files (files that Git does not know anything
  about)

---
## Add file to Git

<pre>
proj
├── .git
└── <span style="color: green;">hello.py</span>
</pre>


<pre>
$ git add hello.py

$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    <text style="color: green;"> new file:   hello.py</text>
</pre>

### The staging area/cache

* After `git add` a file is in the staging area (cache), is staged to be
  committed to the repository
* This is an intermediate level between the work directory and repository

---
## Save to repository

<pre>
proj
├── .git
└── hello.py
</pre>


* Save the latest changes in the local repository (in `.git` directory)

```
    $ git commit -m "First hello"
    [main (root-commit) cf06b48] First hello
     1 file changed, 1 insertion(+)
     create mode 100644 hello.py
```

* Check the status again

```
    $ git status
    On branch main
    nothing to commit, working directory clean
```

---

## The work cycle

There are three levels

* The work directory
* The staging area
* The repository

```
repository (.git)
    ^
    |   commit

staging area (cache)
    ^
    |   add

work directory     <- init
```

---

## The work cycle

There are three levels

* The work directory
* The staging area
* The repository

```
repository (.git)
    ^
    |   commit

staging area (cache)
    ^
    |   add

work directory     <- init
```

The basic work cycle is edit-add-commit

```
    $ <edit> <file>   # edit your file
    $ git add <file>  # cache your changes
    $ git commit -m <message> <file>  # save your changes in repository
```

---

## A mental model of the .git folder

After committing two versions of a project file
we can imagine a folder tree like this (not real)

<pre>
proj
├── .git
│   ├── cache
│   │   └── hello.py
│   └── repository
│       └── commit1
│           └── hello.py
│       └── commit2
│           └── hello.py
└── hello.py
</pre>

* A `git add` copies from the work directory `proj/hello.py` to the cache
  `/proj.git/cache/hello.py`
* A `git commit` creates a new entry in the repository and copies from the
  cache to a new folder, e.g. `proj/.git/repository/commit2/hello.py`
* All old versions are preserved

---

## Review history

To see the commit history of the project files

<pre class="code">
$ git log --oneline
<span class="commit">f56e3da</span> (<span class="cyan">HEAD</span> -> <span class="green">main</span>) First hello
</pre>

<center>
<img src="gitink/c1.svg">
</center>

* A branch is viewed as a line of a development
* Initial default branch is always `main`
* A branch in practice a label for a particular commit
* HEAD is an alias for the current active branch

---

## Viewing changes

* Consider a modified file

<pre class="code">
#hello.py
print("Hello there world!")
</pre>

* git now recognizes this tracked file as modified

<pre class="code">
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        <span class="red">modified:   hello.py</span>

no changes added to commit (use "git add" and/or "git commit -a")
</pre>

---

## Viewing changes

<pre class="code">
$ git diff
diff --git a/hello.py b/hello.py
index ed708ec..01c97be 100644
--- a/hello.py
+++ b/hello.py
<span class="cyan" >@@ -1 +1 @@</span>
<span class="red"  >-print("Hello world!")</span>
<span class="green">+print("Hello there world!")</span>
</pre>

---

## Save changes

* First to cache

```bash
$ git add hello.py
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)

	modified:   hello.py
```

* then to repository

```bash
$ git commit -m "Change greeting"
[main f7efe62] Change greeting
 1 file changed, 1 insertion(+), 1 deletion(-)
```
---

## History after commits

```bash
$ git log --oneline
b895711 (HEAD -> main) Change greeting
f56e3da First hello
```

<center><img src="gitink/c2.svg"></center>

---

## Recovering old work

* To retreive old verions, use checkout with the commit string

```
$ git checkout f56e3da
Note: checking out 'f56e3da'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at f56e3da First hello
```

```
$ cat hello.py
print("Hello world")!
```

---

```
$ git log --oneline --all
b895711 (main) Change greeting
f56e3da (HEAD) First hello
```

<img src="gitink/c3.svg">

```
$git checkout main
Previous HEAD position was f56e3da... First hello
Switched to branch 'main'
```

<img src="gitink/c4.svg">

---

## The work cycle

There are three levels:

* The work directory
* The staging area
* The repository

```
repository (c1 -> c2 -> c3...)
    ^
    |   commit #save permanently in repository

staging area (cache)

    ^
    |   add  #adds new file or saves latest changes

work directory
```

---

## Branches

* A branch is a label for a particular commit

```
$ git branch in-more-languages # create a new branch
$ git switch in-more-languages  # change active branch
Switched to branch 'in-more-languages'
```
```
$ git log --oneline --all
b895711 (HEAD -> in-more-languages, main) Change greeting
f56e3da First hello
```
<img src="gitink/c5.svg">

---

## Work in the new branch

```python
#hello.py
print("Hello there world!")
print("Bonjour tout le monde!")
```

```
$ git add -u  # update the cache with the modified files
```

```
$ git commit -m "French"
[in-more-languages 84bfae8] French
 1 file changed, 1 insertion(+)
```

```
$ git log --oneline
84bfae8 (HEAD -> in-more-languages) French
b895711 (main) Change greeting
f56e3da First hello
```
---

## Work in the new branch

```
$ git log --oneline
84bfae8 (HEAD -> in-more-languages) French
b895711 (main) Change greeting
f56e3da First hello
```
<center><img src="gitink/c6.svg"></center>

---

## Keep the changes: merge to main

```
$ git switch main
Switched to branch 'main'
```
<center><img src="gitink/c7.svg"></center>

---

## Keep the changes: merge to main

* Incorporate the changes from the `in-more-languages` branch

```
$ git merge in-more-languages 
Updating b895711..84bfae8
Fast-forward
 hello.py | 1 +
 1 file changed, 1 insertion(+)
```
<center><img src="gitink/c8.svg"></center>

---

## Keep the changes: merge to main

* If the old branch is not needed it may be deleted

```
$ git branch -d in-more-languages 
Deleted branch in-more-languages (was 84bfae8).

```
<center><img src="gitink/c9.svg"></center>

* Note: the branch concept brings to mind a certain line of development, like the
branch of a tree. However a git branch name essentially a label for a
particular commit. 
* If one commit has two labels one of them can easily be
deleted without losing any information.

---
## Conflicts

* If two branches have modified the same file in the same place, a conflict
  arises
* Consider a German and a Spanish branch
* Maria checks out a German branch and modifies the same line with "Guten Tag Welt"
* Juan checks out a Spanish branch and adds "Hola mundo"

~~~

* df545d7 (HEAD -> Spanish) hola
| * 49b6bf5 (German) guten tag
|/
* 84bfae8 (main) French
* b895711 Change greeting
* f56e3da First hello

~~~
<center><img src="gitink/c9a.svg"></center>

~~~
$ git diff German Spanish
diff --git a/hello.py b/hello.py
index 570e7aa..5b5ade6 100644
--- a/hello.py
+++ b/hello.py
@@ -1,4 +1,4 @@
 #hello.py
 print("Hello there world!")
 print("Bonjour tout le monde!")
-print("Guten Tag Welt!")#red
+print("Hola mundo!")#green
~~~

---

* Maria finishes first and merges

~~~
* df545d7 (Spanish) hola
| * 49b6bf5 (HEAD -> main, German) guten tag
|/
* 84bfae8 French
* b895711 Change greeting
* f56e3da First hello
~~~
<center><img src="gitink/c9b.svg"></center>

---

* Juan tries to merge and gets a conflict

~~~
$ git merge Spanish
Auto-merging hello.py
CONFLICT (content): Merge conflict in hello.py
Automatic merge failed; fix conflicts and then commit the result.
~~~


* The file `hello.py` is now in a conflict state and contains

~~~
#hello.py
print("Hello there world!")
print("Bonjour tout le monde!")
<<<<<<< HEAD
print("Guten Tag Welt!")
=======
print("Hola mundo!")
>>>>>>> Spanish
~~~

~~~
$ git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   hello.py#red

no changes added to commit (use "git add" and/or "git commit -a")

~~~
---

* A choice to be made, on what to keep
* Remove the conflict markers and save the file

~~~
#hello.py
print("Hello there world!")
print("Bonjour tout le monde!")
print("Guten Tag Welt!")
print("Hola mundo!")
~~~

* Add to cache and commit

~~~
$ git add hello.py
$ git commit -m "Merge conflict resolved"
~~~

---

~~~
 $ git log --oneline --graph
 *   8d626a5 (HEAD -> main) Merge conflict resolved
 |\
 | * df545d7 (Spanish) hola
 * | 49b6bf5 (German) guten tag
 |/
 * 84bfae8 French
 * b895711 Change greeting
 * f56e3da First hello
~~~

<center><img src="gitink/c9c.svg"></center>

---

## Remote repositories

* Necessary for collaborative projects
* Useful for single-user projects
* Web-services, github, gitlab, bitbucket
* A shared directory (NFS, AFS, Dropbox....)
* `git pull` from remote
* `git push` to remote

```
repository (.git)  <->   remote
                pull,push

    ^
    |   commit

staging area (cache)

    ^
    |   add

work directory     <- init, clone
```

---

## Working with KTH github

* See https://www.kth.se/student/kth-it-support/work-online/kth-github
* Go to https://gits-15.sys.kth.se and log in with your KTH account
* You need to upload ssh keys for this service

---

## Uploading a local repository

* Having logged in create a new empty repository in the browser
* Locally define an alias for the new remote repository: any name will do but
  *origin* is a common convention

```
$ git remote add origin git@gits-15.sys.kth.se:<user>/proj
```

```
$ git remote -v
origin git@gits-15.sys.kth.se:<user>/proj (fetch)
origin git@gits-15.sys.kth.se:<user>/proj (push)
```

* push the local to the remote, and let the local branch track the remote repository

```
$ git push --set-upstream origin main
...
To git@gits-15.sys.kth.se
 * [new branch]      main -> master
Branch main set up to track remote branch master from origin.
$ git branch -vv
 * main 84bfae8 [origin/master] French
```

---

## Remote repository (origin)

<img src="gitink/c9.svg">

After the push operations the remote repository  has the same information as
the local

---

## Local repository

<img src="gitink/c10.svg">

In local repository also have knowledge about the status of the remote
repository in terms of an extra branch name `origin/main`

---

## Continue work on another computer

* get a copy of the repository

```
    $ git clone git@gits-15.sys.kth.se:<user>/proj.git
    Cloning into 'proj'...
    done.
    $ cd proj
```
* work on other computer and push back changes
    - edit -> add -> commit -> push

---

## On another computer

```
$ git commit
```

<img src="gitink/c11.svg">

---

## On another computer

```
$ git push
```

<img src="gitink/c12.svg">

---

## Back on local computer

<img src="gitink/c10.svg">

---

## Back on local computer

* Retrieve changes that was made on another system

```
$ git fetch  # obtain new remote commits but do not change anything locally
```

<img src="gitink/c13.svg">

---

## Back on local computer

```
$ git merge origin/main # include changes in remote branch into current
```
<img src="gitink/c14.svg">

---

## Summary of work cycle

* Start a new project

```
    $ git init
```

* Get a copy of existing project

```
    $ git clone
```

* Locally: edit-add-commit

```
    $ [your-favorite-editor] filename
    $ git add...
    $ git commit...
```

* Sync with remote: pull-push

```
    $ git pull
    $ git push
```

---

## Github collaborative workflow

### Contributing to open source

A fork is your github copy the reference repository (here the class account) which you create in the web interface. We now have two remote repositories.


The name of a remote repository is an alias to a real address, and thinks may look different depending on order of commands, which repository you clone first and which you add later.

We have two situations

1. 
   * You clone your fork (`git clone <fork url>`), this will be given the alias origin by default
   * You may locally define the reference repository locally, `git remote add
     upstream <reference url>` (`upstream` is a conventional name)

2. 
   * You clone the reference repo (`git clone <reference url>`), which now will
   be the `origin`
   * You define locally your fork with e.g. `git remote add myfork <fork url>`
   which gives your github copy the alias `myfork`

Either method is equally valid, but the `origin` label means different things

---

## Github collaborative workflow (alt 1)

* Clone your fork
~~~
$ git clone <fork url>
~~~
* Define the reference repo as `upstream`
~~~
$ git remote add upstream <reference url>
~~~
* Retrieve the current information from the reference repo
```
$ git fetch upstream
```

<img src="gitink/c15.svg">

---

## Github collaborative workflow

* Create and checkout a 'fix' branch for a particular change

```
$ git checkout -b fix
```

* Edit the file, add and commit locally

```
$ git add .
$ git commit -m "This is my fix"
```

<img src="gitink/c16.svg">

---

## Github collaborative workflow

* Push the fix branch to your copy repo

```
$ git push origin fix
```

<img src="gitink/c17.svg">

* Now you are ready to do a pull request in the Github web interface

---

## Github workflow scheme

<!--
<div class="row">
<div class="col-md-9">
<img class="img-responsive" src="https://lh3.googleusercontent.com/e56nmW1H1vOmGoR0EVswqus0EcCFMPjefwrFDc6KS5Gm2Yc7P1VEy9WrxHu7iqkst6t-WFhhpXYqhwcY1cOAPUNLbimPm7Uvkl4ZanbNKed-wqFs1eae_hLvDB0jdko0hfNibGl7FA=w2400">
</div>
</div>
-->

<img src="https://lh3.googleusercontent.com/e56nmW1H1vOmGoR0EVswqus0EcCFMPjefwrFDc6KS5Gm2Yc7P1VEy9WrxHu7iqkst6t-WFhhpXYqhwcY1cOAPUNLbimPm7Uvkl4ZanbNKed-wqFs1eae_hLvDB0jdko0hfNibGl7FA=w2400" height=600>

Drawing by Erik Fasterius

---

### Links

* http://git-scm.com/book
* http://swcarpentry.github.io/git-novice/
* http://www.linux.com/news/featured-blogs/185-jennifer-cloer/821541-10-years-of-git-an-interview-with-git-creator-linus-torvalds
* https://gun.io/blog/how-to-github-fork-branch-and-pull-request/
* https://www.linux.com/learn/fixing-mistakes-git
* http://christoph.ruegg.name/blog/git-howto-revert-a-commit-already-pushed-to-a-remote-reposit.html
* http://git-man-page-generator.lokaltog.net/

### Acknowledgement

* Figures produced with [gitink](https://github.com/bast/gitink) software by Radovan Bast

