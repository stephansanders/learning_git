# Psychcore Git Tutorial

This is a basic tutorial on using Git.

## What Git does and why you should use it.

Git is a tool that allows you to save, track, and manage a file or project as it evolves over time. This is a good thing. You can use git at the inception of a project, or after it is already under way.

In either case, as the files in your project evolve, you can save snapshots of the files (and directory structure) with git, and have these snapshots available as you move forward. This makes it easy for you to revert to a specific snapshot of the project in case of emergency--accidents happen--or for other reasons--perhaps you intentionally removed a file (or section of a file) from the project earlier on and now you need it again.

The other great thing about Git is its facility for collaboration. If you are collaborating with others on a project, Git makes it easier to work on a shared set of files and orchestrate changes in a tractable way.

Finally, there are many free services that will host your Git repositories (projects tracked with Git) and add value to your git experience. Most notably, [GitHub](https://www.github.com) and [Bitbucket](https://bitbucket.org). These services are not integral to Git, but they do have some really nice free features.

Let's start with the basic Git workflow, which will be 90% of what you do with Git.

## Basic Git Workflow

The first thing you'll need is a copy of Git on your machine. The following links point to installs for Mac, Windows, and Linux.

* [Git for Mac](http://git-scm.com/download/mac)
* [Git for Windows](https://git-for-windows.github.io)
* [Git for Linux](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

You can check if Git is already installed on your machine by invoking the `git` command at the command line, e.g.

```
$ git
```

(*Note: the `$` indicates that the following text is entered at a command prompt. You should not enter the `$` itself*).

The results of running `git` on my machine are 

```
usage: git [--version] [--help] [-C <path>] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

...
```

With Git installed, let's create an empty repository.

### Creating a git project from scratch

First pick a suitable location where you can create files and directories. I've chosen `/Users/mike/Scratch/GitLearning` because it's under my home folder, so I have write access there.

From the command line, make sure you're in the right folder by entering the `pwd` command, which prints the current (working) directory. E.g.

```
$ pwd
```

If you're there, great. If not, `cd` into your chosen folder. Now initialize the empty Git repository with

```
$ git init
```

You should see something to the effect of

```
Initialized empty Git repository in /Users/mike/Scratch/GitLearning/.git/
```

Congratulations, you have created a Git repo! Now lets add some files.

### Adding the first file

Let's create a file with the contents "Hello, world" in our spiffy new Git repo. One way to do this is by redirecting the output of the `echo` command to a file like so

```
$ echo "Hello, world" > hello.txt
```

You could also use your editor of choice (`Vi`, right?) and create the file that way. Nevertheless, just ensure the file has been created, e.g.

```
$ cat hello.txt
```

Which should print `Hello, world` on its own line. Great. Now let's see what Git makes of this. To check the status of your Git repository, issue the command `git status`, e.g.

```
$ git status
```

Which should display

```
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        hello.txt

nothing added to commit but untracked files present (use "git add" to track)
```

For now, just ignore the `branch` stuff, and notice that your file **hello.txt** is `Untracked`. What this means is that you have not instructed Git that you wish to track the revisions of this file. It's useful to note that Git will only track files that you instruct it to track. This is a nice feature. As you become an expert, you'll find that some files you wish to track and others you do not.

Let's track **hello.txt**. To do so, we issue the `git add` command with **hello.txt** as an argument

```
$ git add hello.txt
```

Let's see the status of our repo now that we've tracked **hello.txt**. Enter `git status` once again.

```
$ git status
```

Which should display

```
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   hello.txt
```

We see that Git is aware of **hello.txt** as a `new file`. At this point, we've made Git acknowledge the file, but we haven't saved a snapshot of it yet. What we've done with `git add` is place the file in Git's **staging area**. 

The **staging area** is where we put files before we commit. It may seem weird, at first, to have this **staging area**. Why not just commit files directly? The short answer is that when you are working with many files, and you want a series of changes to be committed in concert, and other changes not to be committed, it is the **staging area** that allows us to achieve this isolation. 

With our staging area prep'd, let's now make our first commit 

```
$ git commit -m "Initial commit"
```

With that, you should see something along the lines of

```
[master (root-commit) 5bbb4d7] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

**hello.txt** has now been committed! Let's reflect on our work by looking at the history (albeit a short one) of our repository

```
$ git log
```

which outputs something like

```
commit 5bbb4d76b4a843ee3156a627c921a52a764effa8
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 20:54:14 2015 -0800

    Initial commit
```

Telling us the commit identifier, author, date, and message of our sole commit.

So what actually happened here? Where did Git put our data? If we list the directory contents

```
$ ls
```

We only see `hello.txt`. However, if we list *everything*

```
$ ls -al
```

We see the hidden `.git` folder as well

```
total 8
drwxr-xr-x   4 mike  staff  136 Dec  2 20:32 .
drwxr-xr-x   4 mike  staff  136 Dec  2 20:25 ..
drwxr-xr-x  13 mike  staff  442 Dec  2 20:54 .git
-rw-r--r--   1 mike  staff   13 Dec  2 20:32 hello.txt
```

When we staged **hello.txt** with `git add` and committed it with `git commit`, git was creating and modifying files in the `.git` folder to reflect these commands. Your git repository is effectively the `.git` folder. In your lifetime, you may never need to peak inside this folder--just rest assured that git is dutifully carrying out its job and maintaining your repository there. 

Services like [GitHub](https://github.com/) are effecitvely just storing a copy of this `.git` folder on their servers for you. When collaborators `git push` and `git pull` from a repository, they are just writing to and reading from a `.git` folder. The simplicity of this model is one of the finer points of git.

