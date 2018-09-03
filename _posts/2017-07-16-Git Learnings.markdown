---
title:  "Git Learning"
date:   2017-07-17 15:04:23
categories: [javascript]
tags: [git]
---
This post is to keep my git knowledge at one place. It consists of the commands used frequently and will evolve as I discover more about git.
### Concepts
**Working Areas**<br/>
It's useful to visualize the workspace and the best diagram for this is Oliver Steel Git Data Transport Commands.

<img src="{{ site.baseurl }}/images/blog/Git-data-transport-cmds.PNG" class="fullsize-image" alt="Git data transport commands">

For more information checkout <a href="https://osteele.com/">Oliver blog</a>

As per diagram git keeps your files at these distinct places. <br>
_Workspace_: Your local directory where physical files are kept: <br>
_Index_: Staging area holds the files for next commit. <br>
_Local Repository_: Committed files ready to be pushed to remote. <br>
_Remote Repository_: Contains commits from all users. <br>


**Markers**<br/>
_HEAD_:  Reference to the last commit in the currently checked-out branch. There is a small exception to this, which is the detached HEAD. A detached HEAD is the situation you end up in whenever you check out a commit (or tag) instead of a branch. In this case, you have to imagine this as a temporary branch without a name; so instead of having a named branch reference, we only have HEAD. It will still allow you to make commits (which will update HEAD), so the above short definition is still true if you think of a detached HEAD as a temporary branch without a name.<br>

_^_: Parent of the commit.<br>
_HEAD^_: First parent of the tip of the current branch.
_HEAD^1-: Same as HEAD^, first parent of the tip of the current branch. Only useful in merge commits. The first parent is the branch you were on when you merged.
_HEAD^2-: Second parent of the tip of the current branch if HEAD was a merge, otherwise illegal. Second parent is the commit on the branch that you merged in.
_HEAD^^_: 2 commits older than HEAD. Not the same as HEAD^2. HEAD^^ means the first parent of the first parent (shorthand for HEAD^1^1). Confusing! Better use tilda.

_~_: Always means first parent. so HEAD~2 == HEAD^^ 
_HEAD~_: 1 commit older than HEAD. Equivalent to HEAD^ (HEAD's first parent)
_HEAD~2_: 2 commits older than HEAD. Equivalent to HEAD^^ (HEAD's first parent's first parent).
_HEAD~3_: Equivalent to HEAD^ (HEAD's first parent's first parent's first parent)

_@{}_: Reflog short names
_HEAD@{2}_: view the second prior value of the HEAD of your repository

^ and ~ operators can be used for other references such as commits. 
Commit and Head are usually interchangeable.
<br/>
### Git Commands
**Setup and Init**<br/>
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

**Staging**<br/>
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

**Branching**<br/>
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

**Sharing Changes**<br/>
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

**Discarding Local Changes**<br/>
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

**Discarding Pushed Changes**<br/>
<pre><code>
git revert [commit] 
<span># create a new commit that undoes all changes.</span>
</code></pre>

<pre><code>
git revert [commit] 
<span># create a new commit that undoes all changes.</span>
</code></pre>

<pre><code>
git revert [oldest_commit_hash]..[latest_commit_hash]
<span># would result in separate commits </span>
</code></pre>

<pre><code>
git revert --no-commit [oldest_commit_hash]
git revert --no-commit [latest_commit_hash]
git commit -m "the commit message"
<span># would result in one combined commit </span>
</code></pre>

<pre><code>
git revert -m 1 <merge-commit>
<span># with ‘-m 1’ git will revert to the first parent. The first parent is the branch you were on when you merged, and the second is the commit on the branch that you merged in </span>
</code></pre>

<pre><code>
git reset HEAD^ --hard
git push origin -f
<span># will result in rewritting the history </span>
</code></pre>

**Exploring**<br/>
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

**Saving temp work**<br/>
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