---
title:  "Git Learning"
date:   2017-07-17 15:04:23
categories: [javascript]
tags: [git]
---
This post is to keep my git knowledge at one place. It consists of the commands used frequently and will evolve as I discover more about git.
<br/>
#### Concepts
<b>Working Areas</b>
It's useful to visualize the workspace and the best diagram for this is Oliver Steel Git Data Transport Commands.

<img src="{{ site.baseurl }}/images/blog/Git-data-transport-cmds.PNG" class="fullsize-image" alt="Git data transport commands">

For more information checkout <a href="https://osteele.com/">Oliver blog</a>

As per diagram git keeps your files at these distinct places. <br>
Workspace: Your local directory where physical files are kept: <br>
Index: Staging area holds the files for next commit. <br>
Local Repository: Committed files ready to be pushed to remote. <br>
Remote Repository: Contains commits from all users. <br>

<b>What is Head</b>
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
<pre><code>
git config --global user.name “[firstname lastname]”
<span># set a name that is identifiable for credit when review version history</span>
</code></pre>

<pre><code>
git config --global user.email “[valid-email]”
<span># set an email address that will be associated with history</span>
</code></pre>

<pre><code>
git config --global color.ui auto
<span># set automatic command line coloring for Git for easy reviewing</span>
</code></pre>

<pre><code>
git init
<span># initialize an existing directory as a Git repository</span>
</code></pre>

<pre><code>
git clone [url]
<span># retrieve an entire repository from a hosted location via URL</span>
</code></pre>

### Staging
<pre><code>
git status
<span># show modified files in working directory, staged for your next commit</span>
</code></pre>

<pre><code>
git status -s
<span># shorter version of status</span>
</code></pre>

<pre><code>
git add [file]
<span># add a file as it looks now to your next commit (stage)</span>
</code></pre>

<pre><code>
git reset [file]
<span># unstage a file while retaining the changes in working directory</span>
</code></pre>

<pre><code>
git diff
<span># diff of what is changed but not staged</span>
</code></pre>

<pre><code>
git diff --staged
<span># diff of what is staged but not yet committed</span>
</code></pre>

<pre><code>
git commit -m “[descriptive message]”
<span># commit your staged content as a new commit snapshot</span>
</code></pre>

### Branching ops
<pre><code>
git branch
<span># list your branches. a * will appear next to the currently active branch</span>
</code></pre>

<pre><code>
git branch -r
<span># list remote branches</span>
</code></pre>

<pre><code>
git branch [branch-name]
<span># create a new branch at the current commit</span>
</code></pre>

<pre><code>
git checkout [branch-name]
<span># updates the files in the working directory to match the version stored in that branch</span>
</code></pre>

<pre><code>
git checkout -b temp-branch-name commit-hash
<span># create a temp branch to avoid having detached head</span>
</code></pre>

<pre><code>
git merge [branch]
<span># merge the specified branch’s history into the current one</span>
</code></pre>

<pre><code>
git log
<span># show all commits in the current branch’s history</span>
</code></pre>

### Sharing Changes
<pre><code>
git remote add [alias] [url]
<span># add a git URL as an alias</span>
</code></pre>

<pre><code>
git remote -v
<span># list remote alias</span>
</code></pre>

<pre><code>
git fetch [alias]
<span># fetch down all the branches from that Git remote</span>
</code></pre>

<pre><code>
git merge [alias]/[branch]
<span># merge a remote branch into your current branch to bring it up to date</span>
</code></pre>

<pre><code>
git push [alias] [branch]
<span># transmit local branch commits to the remote repository branch</span>
</code></pre>

<pre><code>
git pull
<span># fetch and merge any commits from the tracking remote branch</span>
</code></pre>

<pre><code>
git log
<span># show all commits in the current branch’s history</span>
</code></pre>

### Discarding Changes For Local Changes
<pre><code>
git reset --soft [commit]
<span># resets HEAD back to another commit, does not touch the staged or the working directory at all (this leaves all your changed staged and marked for commit)</span>
</code></pre>

<pre><code>
git reset [commit] (default parameter --mixed applied. if commit omitted resets to last commit)
<span># resets HEAD back to another commit, resets the staged to match it, does not touch the working directory (changed files are preserved but not marked for commit)</span>
</code></pre>

<pre><code>
git reset --hard [commit]
<span># resets HEAD back to another commit, resets the staged to match it, and resets the working directory to match it as well (any changes to tracked files in the working tree are discarded)</span>
</code></pre>

<pre><code>
git reset [commit] [filepath]
<span># reset A specific file, commonly used with HEAD rather than an arbitrary commit</span>
</code></pre>

<pre><code>
git commit -a -m "saving my work"
git branch my-saved-work
<span># saving work to new branch before reseting</span>
</code></pre>

<pre><code> 
git fetch origin
git reset --hard origin/[remote]
<span># reset branch to exactly match the remote branch</span>
</code></pre>

<pre><code>
git clean -f -d
<span># remove untracked, forced and remove directories</span>
</code></pre>

<pre><code>
git clean -fxd :/
<span># cleans untracked and ignored files through the entire repo (without :/, the operation affects only the current directory)</span>
</code></pre>

### Discarding Changes For Pushed Changes


### Exploring
<pre><code>
git log
<span># show the commit history for the currently active branch</span>
</code></pre>

<pre><code>
git log branchB..branchA
<span># show the commits on branchA that are not on branchB</span>
</code></pre>

<pre><code>
git log --follow [file]
<span># show the commits that changed file, even across renames</span>
</code></pre>

<pre><code>
git diff branchB...branchA
<span># show the diff of what is in branchA that is not in branchB</span>
</code></pre>

<pre><code>
git show
<span># show various objects such as commit log</span>
</code></pre>

<pre><code>
git show [SHA]
<span># show any object in Git in human-readable format</span>
</code></pre>

### Saving temp work
<pre><code>
git stash
<span># save modified and staged changes</span>
</code></pre>

<pre><code>
git stash list
<span># list stack-order of stashed file changes</span>
</code></pre>

<pre><code>
git stash pop
<span># write working from top of stash stack</span>
</code></pre>

<pre><code>
git stash drop
<span># iscard the changes from top of stash stack</span>
</code></pre>

### Ignoring Patterns
logs/
*.notes
pattern*/
<span># save a file with desired patterns as .gitignore with either direct string matches or wildcard globs.</span>
</code></pre>

### Other

<pre><code>
git config --global core.excludesfile [file]
<span># system wide ignore patern for all local repositories</span>
</code></pre>























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