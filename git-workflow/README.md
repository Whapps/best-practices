# Online Rewards Git Workflow

## Table of Contents
  1. [Overview](#overview)
  1. [Cookbook](#cookbook)
  
## Overview

Follow the simple branching model recommended by [GitHub Flow](https://guides.github.com/introduction/flow/index.html). Each feature or fix should be in its own branch. Branches should be named as descriptively as possible. 

When finished with development on a branch, merge update to master into your branch (if any exist), and test. Code in master should always be deployable.

After testing, if discussion of a change is necessary create a pull request with your changes. If no discussion is necessary merge the code into master and delete your branch.

**[⬆ back to top](#table-of-contents)**

## Cookbook

### Create and switch to a new branch containing any unstaged changes

    $ git checkout -b document-git-workflow
    M	git-workflow/README.md
    Switched to a new branch 'document-git-workflow'
    $ git status
    On branch document-git-workflow
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
    	modified:   README.md

### Work normally in the branch

    $ git commit -a -m 'add initial documentation'
    [document-git-workflow c309084] add initial documentation
    1 file changed, 19 insertions(+)

### Merge the branch back in to master

    $ git checkout master
    Switched to branch 'master'
    Your branch is up-to-date with 'origin/master'.
    $ git merge document-git-workflow

### Or push the branch in order to create a pull request

    $ git push origin document-git-workflow
    Counting objects: 8, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 851 bytes | 0 bytes/s, done.
    Total 4 (delta 0), reused 0 (delta 0)
    To git@github.com:Whapps/best-practices.git
     * [new branch]      document-git-workflow -> document-git-workflow

### Revert work mistakenly committed to master

Suppose that you come across a master branch that has commits that are not ready for deployment.  What we want to do is sweep the commits into their own feature branch to preserve them, then point master back to an earlier good commit.

Although `git reset --hard` can adjust which commit master points to, GitHub will reject non-fast-forward updates.  Instead we have to use `git revert`, which will generate the changes that reverse the offending commits.  These can then safely be committed and pushed to origin.

    $ git branch
    * master
    $ touch foo.txt; git add foo.txt; git commit -m "First commit"
    $ touch bar.txt; git add bar.txt; git commit -m "Second commit"
    $ touch baz.txt; git add baz.txt; git commit -m "Third commit"
    $ touch qux.txt; git add qux.txt; git commit -m "Fourth commit"
    $ git log
    commit 3104c9c173c3f356f5b01972fd488f17706a6835
    Author: ... <...>
    Date:   Tue Apr 29 11:43:03 2014 -0400

        Fourth commit

    commit 108613cda5c0ee8dcd9b2e0c3c0c0840ca627a9b
    Author: ... <...>
    Date:   Tue Apr 29 11:40:22 2014 -0400

        Third commit

    commit 24400761200dd46b8070d4222be54fc7a32861cf
    Author: ... <...>
    Date:   Tue Apr 29 11:40:03 2014 -0400

        Second commit

    commit b40a3d3d75a305ed104c8fd3985a3e79d821e2a2
    Author: ... <...>
    Date:   Tue Apr 29 11:39:44 2014 -0400

        First commit

Suppose that the last three commits are speculative work that is not ready to be deployed.  We will preserve the current state of master in a feature branch `speculative-work`, update master to reverse those commits, then push both master and the new branch back to origin.

    $ git branch speculative-work
    $ git revert -n master~3..master
    $ git commit -m "Revert last three commits"
    [master 3a69aa1] Revert last three commits
     3 files changed, 0 insertions(+), 0 deletions(-)
     delete mode 100644 bar.txt
     delete mode 100644 baz.txt
     delete mode 100644 qux.txt
    $ ls
    foo.txt
    $ git push
    $ git checkout speculative-work
    Switched to branch 'speculative-work'
    $ ls
    bar.txt	baz.txt	foo.txt	qux.txt
    $ git push origin speculative-work

**[⬆ back to top](#table-of-contents)**
