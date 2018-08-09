---
title:  "Git Learning"
date:   2017-07-17 15:04:23
categories: [javascript]
tags: [git]
---
This post is to keep my git knowledge at one place. It consists of the commands used frequently and will evolve as I discover more about git.


### Working Areas
It's useful to visualize the workspace and the best diagram for this is Oliver Steel Git Data Transport Commands.

<img src="{{ site.baseurl }}/images/blog/Git-data-transport-cmds.PNG" class="fullsize-image" alt="Git data transport commands">

For more information checkout <a href="https://osteele.com/">Oliver blog</a>

As per diagram git keeps your files at these distinct places.
Workspace: Your local directory where physical files are kept.
Index: Staging area holds the files for next commit.
Local Repository: Committed files ready to be pushed to remote.
Remote Repository: Contains commits from all users.

### What is Head 
HEAD is a reference to the last commit in the currently checked-out branch. There is a small exception to this, which is the detached HEAD. A detached HEAD is the situation you end up in whenever you check out a commit (or tag) instead of a branch. In this case, you have to imagine this as a temporary branch without a name; so instead of having a named branch reference, we only have HEAD. It will still allow you to make commits (which will update HEAD), so the above short definition is still true if you think of a detached HEAD as a temporary branch without a name.

HEAD is used in together with tilde and carat. They are relative commit markers in Git. They both mean "parent" but in a different way. Always HEAD^1 (or HEAD^ for short) is the same as HEAD~ (or HEAD~1). 
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

commit and Head are usually interchangeable.

Remember:
ref~ is shorthand for ref~1 and means the commit's first parent. ref~2 means the commit's first parent's first parent. ref~3 means the commit's first parent's first parent's first parent. And so on.

ref^ is shorthand for ref^1 and means the commit's first parent. But where the two differ is that ref^2 means the commit's second parent (remember, commits can have two parents when they are a merge).
If commit was a merge, then
first parent is the branch into which we merged,
second parent is the branch we merged.

### Setup and Init
``` js
git config --global user.name “[firstname lastname]”
# set a name that is identifiable for credit when review version history
```

``` js
git config --global user.email “[valid-email]”
# set an email address that will be associated with history
```

``` js
git config --global color.ui auto
# set automatic command line coloring for Git for easy reviewing
```

``` js
git init
# initialize an existing directory as a Git repository
```

``` js
git clone [url]
# retrieve an entire repository from a hosted location via URL
```

### Staging
``` js
git status
# show modified files in working directory, staged for your next commit
```

``` js
git status -s
# shorter version of status
```

``` js
git add [file]
# add a file as it looks now to your next commit (stage)
```

``` js
git reset [file]
# unstage a file while retaining the changes in working directory
```

``` js
git diff
# diff of what is changed but not staged
```

``` js
git diff --staged
# diff of what is staged but not yet committed
```

``` js
git commit -m “[descriptive message]”
# commit your staged content as a new commit snapshot
```

### Branching ops
``` js
git branch
# list your branches. a * will appear next to the currently active branch
```

``` js
git branch -r
# list remote branches
```

``` js
git branch [branch-name]
# create a new branch at the current commit
```

``` js
git checkout [branch-name]

```

``` js
git checkout -b temp-branch-name commit-hash
# create a temp branch to avoid having detached head
```

``` js
git merge [branch]
# merge the specified branch’s history into the current one
```

``` js
git log
# show all commits in the current branch’s history
```

### Sharing Changes
``` js
git remote add [alias] [url]
# add a git URL as an alias
```

``` js
git remote -v
# list remote alias
```

``` js
git fetch [alias]
# fetch down all the branches from that Git remote
```

``` js
git merge [alias]/[branch]
# merge a remote branch into your current branch to bring it up to date
```

``` js
git push [alias] [branch]
# transmit local branch commits to the remote repository branch
```

``` js
git pull
# fetch and merge any commits from the tracking remote branch
```

``` js
git log
# show all commits in the current branch’s history
```

### Discarding Changes For Local Changes
``` js
git reset --soft [commit]
# resets HEAD back to another commit, does not touch the staged or the working directory at all (this leaves all your changed staged and marked for commit)
```

``` js
git reset [commit] # (default parameter --mixed applied. if commit omitted resets to last commit)
# resets HEAD back to another commit, resets the staged to match it, does not touch the working directory (changed files are preserved but not marked for commit)
```

``` js
git reset --hard [commit]
# resets HEAD back to another commit, resets the staged to match it, and resets the working directory to match it as well (any changes to tracked files in the working tree are discarded)
```

``` js
git reset [commit] [filepath]
# reset A specific file, commonly used with HEAD rather than an arbitrary commit
```

``` js
git commit -a -m "saving my work"
git branch my-saved-work
# saving work to new branch before reseting
```

``` js 
git fetch origin
git reset --hard origin/[remote]
# reset branch to exactly match the remote branch
```

``` js
git clean -f -d
# remove untracked, forced and remove directories
```

``` js
git clean -fxd :/
# cleans untracked and ignored files through the entire repo (without :/, the operation affects only the current directory)
```

### Discarding Changes For Pushed Changes


### Exploring
``` js
git log
show the commit history for the currently active branch
```

``` js
git log branchB..branchA
show the commits on branchA that are not on branchB
```

``` js
git log --follow [file]
show the commits that changed file, even across renames
```

``` js
git diff branchB...branchA
show the diff of what is in branchA that is not in branchB
```

``` js
git show
show various objects such as commit log
```

``` js
git show [SHA]
show any object in Git in human-readable format
```

### Saving temp work
``` js
git stash
# save modified and staged changes
```

``` js
git stash list
# list stack-order of stashed file changes
```

``` js
git stash pop
# write working from top of stash stack
```

``` js
git stash drop
# iscard the changes from top of stash stack
```

### Ignoring Patterns
logs/
*.notes
pattern*/
# Save a file with desired patterns as .gitignore with either direct string matches or wildcard globs.
```

### Other

``` js
git config --global core.excludesfile [file]
# system wide ignore patern for all local repositories
```























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