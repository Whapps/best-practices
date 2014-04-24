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


**[⬆ back to top](#table-of-contents)**
