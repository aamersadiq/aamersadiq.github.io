---
title:  "Git Learning"
date:   2017-07-17 15:04:23
categories: [javascript]
tags: [git]
---
This post is to keep my git knowledge at one place. It consists of the commands used frequently and will evolve as I discover more about git.


## Working Areas
I always find it useful to visualize the workspace you are working in. The best diagram for this is Oliver Steel Git Data Transport Commands.

<img src="{{ site.baseurl }}/images/blog/Git-data-transport-cmds.PNG" class="fullsize-image" alt="Git data transport commands">


### W

Create a new repository and push to remote:

``` bash
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

### History, Status Etc
git log -n 10 --oneline

# Stashing


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