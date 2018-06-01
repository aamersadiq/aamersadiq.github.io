---
title:  "Git Learning"
date:   2017-07-17 15:04:23
categories: [javascript]
tags: [git]
---
This post is to keep my git knowledge at one place. It consists of the commands used frequently and will evolve as I discover more about git.


### Working Areas
I always find it useful to visualize the workspace you are working in. The best diagram for this is Oliver Steel Git Data Transport Commands.

<img src="{{ site.baseurl }}/images/blog/Git-data-transport-cmds.PNG" class="fullsize-image" alt="Git data transport commands">

For more information checkout <a href="https://osteele.com/">Oliver blog</a>

### What is Head 

HEAD is a reference to the last commit in the currently checked-out branch.

There is a small exception to this, which is the detached HEAD. A detached HEAD is the situation you end up in whenever you check out a commit (or tag) instead of a branch. In this case, you have to imagine this as a temporary branch without a name; so instead of having a named branch reference, we only have HEAD. It will still allow you to make commits (which will update HEAD), so the above short definition is still true if you think of a detached HEAD as a temporary branch without a name.

Sometimes you don't want to have a detached head. The solution is to create a temp branch for that.

``` js
git checkout -b temp-branch-name commit-hash
```

Also HEAD is used together with tilde and carat. They are relative commit markers in Git. They both mean "parent" but in a different way. Always HEAD^1 (or HEAD^ for short) is the same as HEAD~ (or HEAD~1). 
The difference comes when they stack. 
So HEAD^2 means "The second parent of HEAD". This only means anything if there's been a merge. In a merge, the main branch is parent #1; the merged in branch is parent 2. So, HEAD^2 will be the merged parent, whereas HEAD^1 will be the parent merged into.
Now, HEAD^^ is not the same as HEAD^2. HEAD^^ means the first parent of the first parent. This is just shorthand for HEAD^1^1. Evaluate from left to right.
But HEAD^^ is ugly. Isn't there something nicer. Yes. Tilde. Tilde ALWAYS means first parent. so HEAD~2 == HEAD^^.
Finally, you can stack tildes and carats. Say you wanted the merged in branch of two heads ago. That would be HEAD~2^2. 

HEAD^ - first parent of the tip of the current branch.
HEAD^1 same as HEAD^ - first parent of the tip of the current branch.
HEAD^2 - second parent of the tip of the current branch.
HEAD~ - 

HEAD~2 : 2 commits older than HEAD
HEAD^2 : the second parent of HEAD, if HEAD was a merge, otherwise illegal
HEAD@{2} : refers to the 3rd listing in the overview of git reflog
HEAD~~ : 2 commits older than HEAD
HEAD^^ : 2 commits older than HEAD

The ^ and ~ operators can be used for other references such as commits. 

Remember:
ref~ is shorthand for ref~1 and means the commit's first parent. ref~2 means the commit's first parent's first parent. ref~3 means the commit's first parent's first parent's first parent. And so on.

ref^ is shorthand for ref^1 and means the commit's first parent. But where the two differ is that ref^2 means the commit's second parent (remember, commits can have two parents when they are a merge).
If commit was a merge, then
first parent is the branch into which we merged,
second parent is the branch we merged.

### Starting out

Create a new repository and push to remote:

``` js
git init

git add .

# Adds the files in the local repository and stages them for commit. To unstage a file, use 'git reset HEAD YOUR-FILE'.


```

### Looking back
git log -n 10 --oneline

### Saving temp work


### Undoing
Undoing a pushed merge





















# Discading local changes
https://stackoverflow.com/questions/1090309/git-undo-all-working-dir-changes-including-new-files

# Discarding changes one commit back
-----------
1. Changes committed locally but not pushed to remote. https://bytefreaks.net/programming-2/how-to-undo-a-git-commit-that-was-not-pushed
1A. undoes the commit and leaves the changes you committed upstaged

leaves changes as staged
``` js
$ git commit -m "Something terribly misguided"              (1)
$ git reset HEAD~                                           (2)
<< edit files as necessary >>                               (3)
$ git add ...                                               (4)
$ git commit -c ORIG_HEAD                                   (5)
... 
This is what you want to undo
This leaves your working tree (the state of your files on disk) unchanged but undoes the commit and leaves the changes you committed unstaged (so they'll appear as "Changes not staged for commit" in git status, and you'll need to add them again before committing). If you only want to add more changes to the previous commit, or change the commit message1, you could use git reset --soft HEAD~ instead, which is like git reset HEAD~ (where HEAD~ is the same as HEAD~1) but leaves your existing changes staged.
Make corrections to working tree files.
git add anything that you want to include in your new commit.
Commit the changes, reusing the old commit message. reset copied the old head to .git/ORIG_HEAD; commit with -c ORIG_HEAD will open an editor, which initially contains the log message from the old commit and allows you to edit it. If you do not need to edit the message, you could use the -C option.


leaves changes as unstaged
git reset HEAD~

https://blog.github.com/2015-06-08-how-to-undo-almost-anything-with-git/
-----

Reset local repository branch to be just like remote repository HEAD - https://stackoverflow.com/questions/1628088/reset-local-repository-branch-to-be-just-like-remote-repository-head
git reset --hard origin/[name-of-remote]

2. Discard already pushed to remote http://christoph.ruegg.name/blog/git-howto-revert-a-commit-already-pushed-to-a-remote-reposit.html
https://gist.github.com/gunjanpatel/18f9e4d1eb609597c50c2118e416e6a6
dangerous can work if no user has cloned the repo and made local changes. e.g release branch rollback: http://christoph.ruegg.name/blog/git-howto-revert-a-commit-already-pushed-to-a-remote-reposit.html

better way:
git revert <commit_hash>
This will create a new commit which reverts the changes of the commit you specified. Note that it only reverts that specific commit, and not commits after that. If you want to revert a range of commits, you can do it like this:

git revert <oldest_commit_hash>..<latest_commit_hash>   would result in separate commits e.g  git revert e403c82...a23f612
above didn't work. use multiple reverts https://stackoverflow.com/questions/1463340/revert-multiple-git-commits
$ git revert --no-commit D
$ git revert --no-commit C
$ git revert --no-commit B
$ git commit -m "the commit message"


For merge commit only
https://www.christianengvall.se/undo-pushed-merge-git/