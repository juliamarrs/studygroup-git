# Lesson 2

As we pointed out last time, git is a "distributed" version control system, which means that there are multiple copies of this git repository floating around. There's one on my local machine, one on Github's servers and all of your "forks" that you made on Github and then cloned to your own local machines. We saw how to make changes to one of these copies (your local repository). If you were working on a project by yourself, and you just wanted to track your changes so you could be sure that you don't lose anything, you wouldn't have to do much else. There are some tricks like [branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) that can make your life easier, but you'll still be working with the basic change-stage-commit cycle that we worked out last time.

If you want to collaborate with other people on your project -- say the other members of your lab or contributors to your open source software project -- you'll need to reconcile the different changes everyone has made to their own copies. That's what we're going to cover this week.

First, we'll see how to send our local changes to our forks on Github. Then we'll see how to use Github's "pull requests" to merge all of those changes into one copy on Github. Then we'll "fetch" the newly combined `bio.txt` file from the merged copy and make sure our forks are kept up-to-date with the changes. Finally, we'll talk a little bit about good workflows for using git and Github in collaborative open science projects.

## Recap from last time

Just to be sure we're all on the same page, this is what we did last time, along with the git commands that we used.

1. Configured git on our local machines (`git config`)
2. Forked the [studygroup-git](https://github.com/wkearn/studygroup-git) repository on Github
3. Cloned our forks to our local machines (`git clone`)
4. Checked the status and history of our repository (`git log`, `git status`)
5. Added our names and departments to a file called `bio.txt`
6. Staged the new file (`git add`)
7. Committed the new file to the repository (`git commit`)
8. Added our favorite programming language to `bio.txt`
9. Looked at the differences between the old `bio.txt` and the new one (`git diff`)
10. Staged and committed the new changes

At this stage you should have your fork of the `studygroup-git` repository on your local machine a file called `bio.txt` with your name, department and favorite programming language on three lines, and at least one commit of that `bio.txt` file to your local repository.

## Pushing changes

To send your local changes to Github (or any other git server) you use the command `git push`. You'll add two arguments to `git push`. The first one is the name of the "remote" that you are going to push your changes to. A "remote" is simply a git repository that is different from the one you are currently working in. It could be on a completely different computer (like one of Github's servers), but it doesn't have to be. It could just be another directory somewhere else on your computer.

When you clone a repository from a server, as we did to get our copies of this repository, git will automatically add a remote called `origin` which points to wherever we cloned the repository from. Assuming you were following along last time, you should have a remote that points to your fork on Github (<https://github.com/USERNAME/studygroup-git>). You can check this with the command `git remote -v`, for example:

```
> git remote -v
origin	https://github.com/wkearn/studygroup-git (fetch)
origin	https://github.com/wkearn/studygroup-git (push)
```

If you don't have the `-v`, `git remote` will just list the name of the remote (`origin` in this case), which I don't usually find very helpful. Make sure that `origin` points to your fork on Github and not to my original copy. Github controls who can push to each repository, so if you try to push your fork to my copy, git will error out.

The second argument to `git push` is the name of the branch that you want to push to the remote. We haven't really talked about branches too much here, because they get complicated, but for now, you should know that the default branch in a git repository is called `master`. So if you haven't made any branches in your repository, all of your commits will be on the `master` branch.

The command to push `master` to the `origin` remote is

```
git push origin master
```

## Pull requests

Now that you've pushed your commits to your fork on Github, we want to combine the edits that everyone has made into one copy. Github makes this easy with its "Pull Requests." Pull requests are a feature of Github (and services like it such as Gitlab) and not git the software. When Github makes a pull request, it is basically running `git merge` to merge your changes into my copy, and you can use `git merge` on the command line to do the same thing. A pull request, however, is about more than just merging changes. Github lets you (and other people) comment on pull requests, so you can use pull requests as an opportunity to do a "code review." We'll talk more about code reviews and other ways to structure your collaboration when we talk about workflows below.


To create a pull request, you click on "New pull request" which should show up on the homepage of your fork (<https://github.com/USERNAME/studygroup-git>). You'll be taken to Github's "Open a pull request" interface. This should by default set you up for a pull request from your `master` branch into the `master` branch of my Github repository. You can add a title and comments to your pull request. When you're happy with everything, click the green "Open pull request" button at the bottom. This will open the pull request on my repository. From here on out, it's up to me, the "maintainer" of this project, to decide whether to merge your pull requests or not. Obviously I'll merge them today, but this is also where the contributors to a project might perform a code review.


## Merging pull requests

Now it's my job to merge the pull requests. This might get a little complicated because we're going to have conflicts when we start merging everyone's `bio.txt` file. Feel free to ask questions. The good thing is that we'll run through this process a few times to get everyone's pull request merged. If you need to do this on your own project, Github's documentation on [Merging a pull request](https://help.github.com/articles/merging-a-pull-request/) and [Checking out pull requests locally](https://help.github.com/articles/checking-out-pull-requests-locally/) might be helpful.

As the maintainer of the project, I will go to the "Pull Requests" tab on my repository. If I click on one of the pull requests you just made and scroll down, there will be a box. If there are no merge conflicts, I'll be able to merge the pull request on Github directly. If I click the "Merge pull request" button, it will automatically add your commit to my repository and close the pull request.

If there are merge conflicts, I'll need to resolve them before I am able to merge the pull request. A merge conflict arises when two people have made edits to the same parts of the same files. In this case, git isn't able to tell which edit should be kept and which should be thrown away. In our example, since each of us has added a couple of lines to the top of the `bio.txt` file, git can't decide whether it should replace my lines with yours or append your lines on to the bottom of mine. I'll have to tell it that I want to append the lines. I do that by editing the file in which there is a conflict. I can do this by pulling the pull request branch down to my local copy, editing it on my computer, and pushing it back to Github. Github also provides an interface for editing the file that I'll use here. Once I've fixed the conflicts, I can merge the pull request.

## Keeping your fork up-to-date

Now that I've merged the changes into my copy of the studygroup-git repository, you might want to add those changes to your fork and your local copy. This is important if, for instance, you are working on a collaborative software project and someone else makes changes to the official copy. You'll want to incorporate those changes into your code so that you can build off of them.

First, we'll add my repository as a new remote and then pull the changes I just merged into the `bio.txt` file. Traditionally, the remote that refers to the main copy of a project is called `upstream` (remember that your own copy is usually called `origin`). This is just a name, and you can call it whatever you like, but we'll stick with `upstream` for now. To add a remote called `upstream` which points to my studygroup-git repository, run

```
git remote add upstream https://github.com/wkearn/studygroup-git.git
```

Now, if you run `git remote -v`, you'll see the `origin` remote which should point to your fork on Github and a new `upstream` remote which points to my repository.

To sync your fork and the main repository, you first "pull" the changes from `upstream` into your local copy of the repository and then push those changes to Github. Git offers a command called `git pull` that will do this, but it just runs a few commands in sequence. It is good to know that sequence because `git pull` can sometimes cause problems since it assumes that you're trying to do one thing when you might want to do something else.

The first command is

```
git fetch upstream
```

This will copy the information that is stored in my Github repository to your local copy. It doesn't actually make any changes to any files locally.

Make sure you are on the `master` branch of your repository (`git checkout master`) and then run

```
git merge upstream/master
```

Here, `upstream/master` refers to the `master` branch on the `upstream` remote which you are trying to merge into your `master` branch. This will likely proceed without any intervention because there shouldn't be any merge conflicts. If there are, you'll have to resolve the merge conflicts, and I'll show you all how to do that.

Once you've merged `upstream/master` into `master`, you need to tell Github about these changes. We've done this before; it's just `git push origin master`.

## Working with others with git and Github

At this point, you've seen most of the workflow required to use git and Github. Roughly, the workflow looks like this:

1. Find a repository on Github that you want to contribute to
2. Fork that repository on Github
3. Clone your fork to your local machine
4. Make and commit changes to your local copy
5. Push changes to your fork on Github
6. Open a pull request to the upstream repository
7. Have your pull request reviewed by the maintainers of the project
8. The maintainers will either merge your pull request, ask you to make some changes, or reject your pull request
9. Keep your fork and local copy synced with the upstream repository by pulling from upstream and pushing to your fork
10. Repeat

The main thing that we haven't talked about is [branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell), which is a way of organizing commits so that multiple lines of development can proceed simultaneously. Basically, you create a branch, commit a few things to that branch, and then merge the branch back into the `master` branch, which is the kind of "official" code for the project.

This can be very helpful when you are working on a big collaborative software project. The maintainers of the project might want to keep the master branch of the upstream repository stable so that someone who downloads the code immediately has a copy that works, is well-documented, etc. In that case, when you want to create a new feature, you'll start a branch for that feature, make all of your changes, and then open a pull request not into the master branch of the upstream repository, but into a special "unstable" branch that holds all of the new features before they are tested and merged into the master branch as part of an official "release." A popular way for organizing branches for this kind of project is called [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/).
