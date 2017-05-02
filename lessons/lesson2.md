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


