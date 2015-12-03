# Psychcore Git Tutorial

## What Git does and why you should use it.

Git is a tool that allows you to save, track changes, and manage a file or project as it evolves over time. This is a good thing. You can use git at the inception of a project, or after it is already under way.

In either case, as the files in your project evolve, you can save snapshots of the files (and directory structure) with git, and have these snapshots available as you move forward. This makes it easy for you to revert to a specific snapshot of the project in case of emergency--accidents happen--or for other reasons--perhaps you intentionally removed a file (or section of a file) from the project earlier on and now you need it again.

The other great thing about Git is its facility for collaboration. If you are collaborating with others on a project, Git makes it easier to work on a shared set of files and orchestrate changes in a tractable way.

Finally, there are many free services that will host your Git repositories and add value to your git experience. Most notably, [GitHub](https://www.github.com) and [Bitbucket](https://bitbucket.org). These services are not integral to Git, but they do have some really nice free features.

Let's start with the basic Git workflow, which will be 90% of what you do with Git.

## Basic Git Workflow

The following is a walk-through of your basic Git workflow. It's composed of these sections

1. Installation
2. Creating a git project from scratch
3. Adding the first file
4. Making a second commit
5. Making use of history
6. Making use of (distant) history
7. Returning to our 3-line version

which will teach the basics of repository creation, repository structure, making changes to files and writing snapshots, and reverting to versions from history.

### Installation

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

### Making a second commit

Let's add something to our file.

```
$ echo "It's a beautiful day" >> hello.txt
```

Again, you can just use your editor of choice. Let's make sure the changes are what we expect

```
$ cat hello.txt
```

which gives

```
Hello, world
It's a beautiful day
```

Amazing. What has git made of all this? `git status` is your friend in determining the state of your repository.

```
$ git status
```

gives

```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Git has noticed that **hello.txt** has been modified, and it notices that **hello.txt** has not been staged for a commit. This is an interesting situation. The version of **hello.txt** on our filesystem has one additional line compared to the version of **hello.txt** in our git repository.

To make ourselves aware of the differences between what's in our *working directory* and what's in our *repository*, we can issue a `git diff`

```
$ git diff
```

which gives us the useful, but arcane

```
diff --git a/hello.txt b/hello.txt
index a5c1966..9cf85e0 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,2 @@
 Hello, world
+It's a beautiful day
```

If you're not familiar with `diff`ing files, what we see is the *difference* between the two versions of **hello.txt**. If you look at the last two lines

```
 Hello, world
+It's a beautiful day
```

It's apparent that our working copy of **hello.txt** has one line added with respect to the previous version. This is good, it's what we intended. Let's stage our changes and commit.

Stage it.

```
$ git add hello.txt
```

Verify it's staged.

```
$ git status

On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   hello.txt

```

Yep, it's ready to be committed.
Now commit it.

```
$ git commit -m "Brilliant new line"

[master 6f00341] Brilliant new line
 1 file changed, 1 insertion(+)
```

And behold the new history of our repository

```
$ git log

commit 6f003416432edfc70c85244017c54939748ac9e9
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:23:37 2015 -0800

    Brilliant new line

commit 5bbb4d76b4a843ee3156a627c921a52a764effa8
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 20:54:14 2015 -0800

    Initial commit
```

Excellent! Our initial commit is now followed our recent commit.

### Making use of history

So we've built up these snapshots of a file. How would we ever use them to our benefit? One primary use case is rolling back to previous versions. Let's take a look at how to do that.

We'll begin by making a change that we regret.

```
$ echo "I think I'll take a walk" > hello.txt
```

Invoking this command with the `>` redirection operator *overwrites* the file, rather than appending to it. I.e. we mystyped `>` for `>>`. Now if we look at the contents of **hello.txt**, only the line added above is in the file.

```
$ cat hello.txt 

I think I'll take a walk
```

The rest of our brilliant prose has been demolished! Git can help us. If we `git status` there is a clue of how to recover the file.

```
$ git status

On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

As indicated above, we can replace the modified copy of **hello.txt** in our working directory with the latest copy from the repository by issuing `git checkout -- hello.txt`. I.e. see the line

```
  (use "git checkout -- <file>..." to discard changes in working directory)
```

So

```
$ git checkout -- hello.txt
```

And doing another `git status` shows that there are no longer modifications.

```
$ git status

On branch master
nothing to commit, working directory clean
```

We can print the file to show that it's been reverted to the last snapshot.

```
$ cat hello.txt

Hello, world
It's a beautiful day
```

Yay. Now let's properly add that last line, stage, and commit

```
$ echo "I think I'll take a walk" >> hello.txt
$ cat hello.txt

Hello, world
It's a beautiful day
I think I'll take a walk

$ git status

On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git add hello.txt
$ git status

On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   hello.txt

$ git commit -m "Exercise is good"

[master 9d7dab6] Exercise is good
 1 file changed, 1 insertion(+)
 
$ git log

commit 9d7dab6193f1ede33aba72bc3f2cf0f400b036f3
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:41:17 2015 -0800

    Exercise is good

commit 6f003416432edfc70c85244017c54939748ac9e9
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:23:37 2015 -0800

    Brilliant new line

commit 5bbb4d76b4a843ee3156a627c921a52a764effa8
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 20:54:14 2015 -0800

    Initial commit
```

In recap, we recovered from a major mistake by reverting to the last copy of a file int the repository. What if we want to go back... further?

### Making use of (distant) history

Suppose we want to revert our file to some ancestral state, beyond just the latest copy in the repository. `git diff` and `git log` will help us accomplish this.

Maybe we're not sure which version of the file we want to revert to, so we use `git log` and look at our commit messages for some clues.

```
$ git log

commit 9d7dab6193f1ede33aba72bc3f2cf0f400b036f3
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:41:17 2015 -0800

    Exercise is good

commit 6f003416432edfc70c85244017c54939748ac9e9
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:23:37 2015 -0800

    Brilliant new line

commit 5bbb4d76b4a843ee3156a627c921a52a764effa8
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 20:54:14 2015 -0800

    Initial commit
```

Perhaps we think we want to go back to the inital commit, but we're not sure what the difference is? We can use `git diff` in combination with the commit id to find out. So the commit id for the `Initial commit` is `5bbb4d76b4a843ee3156a627c921a52a764effa8`, so to compare our current **hello.txt** to the version from the commit we

```
$ git diff 5bbb4d76b4a843ee3156a627c921a52a764effa8

diff --git a/hello.txt b/hello.txt
index a5c1966..e503a0c 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,3 @@
 Hello, world
+It's a beautiful day
+I think I'll take a walk
```

The last three lines show us that the current version of **hello.txt** has two additional lines with respect to the version in commit `5bbb4d76b4a843ee3156a627c921a52a764effa8`. If we wish to go all the way back to that original version, we just check it out, with the syntax `git checkout <commit id> <file name>`

```
$ git checkout 5bbb4d76b4a843ee3156a627c921a52a764effa8 hello.txt
```

Now if we do a `git status`, we see that the file has been modified.

```
$ git status

On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   hello.txt
```

And the contents are...

```
$ cat hello.txt

Hello, world
```

Perfect! We're happy. Let's stage this thing and commit it.

```
$ git add hello.txt
$ git status

On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   hello.txt

$ git commit -m "A oldie but goodie"
[master ccd7f8e] A oldie but goodie
 1 file changed, 2 deletions(-)
```

But what will our history look like now? Did we just demolish all of our intermediate revisions? Let's ask `git log` for the answer

```
$ git log

commit ccd7f8e01d1af1065e30575d710dd297c3114c57
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:58:37 2015 -0800

    A oldie but goodie

commit 9d7dab6193f1ede33aba72bc3f2cf0f400b036f3
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:41:17 2015 -0800

    Exercise is good

commit 6f003416432edfc70c85244017c54939748ac9e9
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:23:37 2015 -0800

    Brilliant new line

commit 5bbb4d76b4a843ee3156a627c921a52a764effa8
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 20:54:14 2015 -0800

    Initial commit
```

No! They are all still there. We've just written another snapshot into history. Git didn't care that we reverted to an old copy. As far as git is concerned, we just made yet another modification. To prove to ourselves that we can recover the 3-line version of **hello.txt**, let's revert to *that* commit!

### Returning to our 3-line version

Let's get the commit id of the version we want.

```
$ git log

commit ccd7f8e01d1af1065e30575d710dd297c3114c57
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:58:37 2015 -0800

    A oldie but goodie

commit 9d7dab6193f1ede33aba72bc3f2cf0f400b036f3
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:41:17 2015 -0800

    Exercise is good

commit 6f003416432edfc70c85244017c54939748ac9e9
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:23:37 2015 -0800

    Brilliant new line

commit 5bbb4d76b4a843ee3156a627c921a52a764effa8
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 20:54:14 2015 -0800

    Initial commit
```

So, the commit with the message `Exercise is good` is the 3-line version of the file. Let's make sure it's the version we want. First copy the commit id `9d7dab6193f1ede33aba72bc3f2cf0f400b036f3` to the clipboard, and do a `git diff` with it.

```
$ git diff 9d7dab6193f1ede33aba72bc3f2cf0f400b036f3

diff --git a/hello.txt b/hello.txt
index e503a0c..a5c1966 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1,3 +1 @@
 Hello, world
-It's a beautiful day
-I think I'll take a walk
```

The `diff` tells us that our current copy is missing the last two lines from the version in the commit we specified. Looks like this is the right commit. Let's check out the copy of **hello.txt** from that commit.

```
$ git checkout 9d7dab6193f1ede33aba72bc3f2cf0f400b036f3 hello.txt 
```

And inspect the file to make sure it's correct.

```
$ cat hello.txt

Hello, world
It's a beautiful day
I think I'll take a walk
```

Success! Let's add another line for grins.

```
$ echo "I think I'll take a nap" >> hello.txt
```

And inspect it

```
$ cat hello.txt

Hello, world
It's a beautiful day
I think I'll take a walk
I think I'll take a nap
```

Now stage it, commit it, and inspect the log!

```
$ git add hello.txt
$ git status

On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   hello.txt

$ git commit -m "Final commit. (for now)"

[master f5ac00c] Final commit. (for now)
 1 file changed, 3 insertions(+)
 
$ git log

commit f5ac00c0a3347571c89dbe9ed75c3069d5594c35
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 22:08:23 2015 -0800

    Final commit. (for now)

commit ccd7f8e01d1af1065e30575d710dd297c3114c57
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:58:37 2015 -0800

    A oldie but goodie

commit 9d7dab6193f1ede33aba72bc3f2cf0f400b036f3
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:41:17 2015 -0800

    Exercise is good

commit 6f003416432edfc70c85244017c54939748ac9e9
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 21:23:37 2015 -0800

    Brilliant new line

commit 5bbb4d76b4a843ee3156a627c921a52a764effa8
Author: Michael Gilson <gilson@cs.wisc.edu>
Date:   Wed Dec 2 20:54:14 2015 -0800

    Initial commit
```

That's it for now!
