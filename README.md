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

