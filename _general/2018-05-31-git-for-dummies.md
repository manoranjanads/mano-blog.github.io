---
title: "Git for Dummies"
permalink: /general/basic-git-commands-for-beginners.html
excerpt: "Basic Git Commands"
header:
  overlay_image: /assets/images/general/2018-05-31-git-for-dummies/git.png
  teaser: /assets/images/general/2018-05-31-git-for-dummies/git.png
  overlay_filter: 0.5
last_modified_at: 2018-10-19T15:59:07-04:00
toc: true
# categories:
#   - Git
#   - Trainee
# tags:
#   - Git
#   - Beginner
---

Git is the most favourite version controlling system loved by millions of developers. 
Simplicity and the distributed nature of it made it our favorite version controlling tool.
If you are really new to programming and version controlling, you might be having following questions in your mind.

* How can I separate my code from the working code, while I get all the updates from others.
* How to share my updates with others
* How to get the latest updates from others.
* How to bring the changes that I have done to the working code base.
* How to compare two files or two versions of the same file with each other.

To find the answers to these beginners problems, lets clarify following keywords

### git pull
Use this command to receive latest update of the code base.

### git push
This command is used to share your updates with other developers.

### git branch 
Git branch is similar to a branch of a tree. A branch starts to grow from another branch.
Most of the projects has few common branches such as master, alpha, beta, staging, testing.
Basically it's they are different versions of the same code base.
For an example, let's say that you need to fix an issue in the working code.
Usually, you have create a new branch from the working branch (usually master or dev branch) and fix the issue there.
Then you have to do something called `git commit` to save the changes

### git commit
It's like asking to save the changes in a different folder name.
Each commit represents a different version of the code base.
Before you `commit` the changes you have to specify what files that the git should save in this commit.
It can be done via 'git add'.
Once you commit your changes, to bring them to the working branch, you have to `merge` the changes.
  
### git merge
Once you have fixed the issue on your own branch, you have to merge, these changes with the master/dev branches(or the working branch).
This is done via a git merge. Some times, you might ask to send a `PR` (Pull Request) instead.

### Pull Request

Before allowing you to merge your code directly to the working branch,
the maintainer or the lead develoepr of the project might want to review your code.
This can be done via a Pull Requests or a Merge Request.
Almost all git platforms including GitHub, GitLab and BitBucket provide this feature.
This allows the maintainer to review the code and approve the merge.

### git status
If you need to know what file have been changed since the last version, you can use `git status` command

### git stash
Well, if you have done some changes to the code, and you have not committed (saved) the changes you have done, and if you do not need them,
and you wants to go back to where you started,
`git stash` is the command that you want to know.

### git checkout
You can use this command to create branches and to move between branches.

### git diff
When you want to compare two files or two versions of the same files, this is the command that you are going to use.



I know there is not much here and this is very basic, but hope this would help!
