# Git syntax

Here are some common commands:
* `git init` for creating a new repository
* `git status` for seeing which files have changed
* `git add` for "staging" files to commit
* `git commit` for committing changes to a repository
* `git log` for seeing all commits
* `git show` for showing the changes in a commit
* `git diff` for seeing what's changed
* `git reset` for "unstaging" changes
* `git revert` for reverting a commit
* `git branch` for encapsulating changes
* `git merge` for merging one branch into another

## Create a new repository

1. Create a new directory:

        $ mkdir foo

1. Change into that directory:

        $ cd foo

1. Create a new git repository:

        $ git init


## See which files have changed

    $ git status
    
    On branch master
    
    Initial commit
    
    nothing to commit

## Commit a change to a repository

1. Create a file:

        $ echo "hi" > file.txt

1. Observe the file is not yet "tracked" by the git repository:

        $ git status
        ...
        Untracked files:
          (use "git add <file>..." to include in what will be committed)
        
        	file.txt

1. Add the file to the list of changes "staged" for the next commit:

        $ git add file.txt

1. Observe the file is now in the list of changes for the next commit:

        $ git status
        On branch master
        Changes to be committed:
          (use "git reset HEAD <file>..." to unstage)
        
        	new file:   file.txt
        ...

1. Finalize the commit:

        $ git commit
Git will launch your default text editor. Write a brief message, eg "Add file", and exit the text editor.

## See all commits in a repository

    $ git log
    commit 230d2a431184c93da0d7a5f7fbeaa00e168e005e
    Author: Erik Eldridge <erik@twitter.com>
    Date:   Sun Feb 15 11:27:45 2015 -0800
    
        Add file
    ...

## Show the changes in a commit

    $ git show 230d2a431184c93da0d7a5f7fbeaa00e168e005e
    commit 230d2a431184c93da0d7a5f7fbeaa00e168e005e
    Author: Erik Eldridge <erik@twitter.com>
    Date:   Sun Feb 15 11:27:45 2015 -0800
    
        Add file
    
    diff --git a/file.txt b/file.txt
    new file mode 100644
    index 0000000..d72af31
    --- /dev/null
    +++ b/file.txt
    @@ -0,0 +1 @@
    +hi

## See what's changed

1. Create another change, eg:

        $ echo "world" >> file.txt

1. Run `git diff` to see what has changed:

        $ git diff
        diff --git a/file.txt b/file.txt
        index d72af31..87e05f0 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         hi
        +world

## Unstage changes

1. Run `git add` to stage the change made above
1. Run `git status` to see what's staged for the next commit
1. Run `git reset` to unstage:

        $ git reset
        Unstaged changes after reset:
        M file.txt

## Revert a commit

1. Run `git revert` to undo a given change:

    $ git revert 59d700c1db523b5a7d92161664cd5cdd5d9d477f
    [master 8190a0b] Revert "Update file.txt"
     1 file changed, 1 deletion(-)

1. Run `git log` to see the reversion:

    $ git log
    commit 8190a0b66231ddb06587b55c7cb6ade4c66bc6b3
    Author: Erik Eldridge <erik@twitter.com>
    Date:   Sun Feb 15 15:23:39 2015 -0800

        Revert "Update file.txt"
        
        This reverts commit 59d700c1db523b5a7d92161664cd5cdd5d9d477f.

    commit 59d700c1db523b5a7d92161664cd5cdd5d9d477f
    Author: Erik Eldridge <erikeldridge@gmail.com>
    Date:   Sun Feb 15 15:08:32 2015 -0800

        Update file.txt

    commit b974749b85a95f48fe6615f407a237320a68ed5d
    Author: Erik Eldridge <erik@twitter.com>
    Date:   Sun Feb 15 15:06:12 2015 -0800

        adding a file
## Creating a branch

To ensure a repository always contains functional code, we can create a "branch" to encapsulate our work in progress.

By convention, the main branch of a repository is usually called "master".

To create a new branch, run `git branch` with a branch name:

    $ git branch foo

## Select a branch to work in

1. Run `git branch` without a branch name to see all branches:

        $ git branch
          foo
        * master

1. Run `git checkout` with a branch name to work in that branch:

        $ git checkout foo
        Switched to branch 'foo'

1. Make a change and commit
1. Use `git diff` to compare master and your current branch:

        $ git diff master
        diff --git a/file.txt b/file.txt
        index d72af31..c5cb59a 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         asd
        +qwe

1. Use `git log` to see commits on your current branch that aren't on master:

        $ git log master..foo
        commit cb86977de46709ab389a13741bdf8cd0495782cc
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 15:34:27 2015 -0800
    
            add text

## Merge branches

1. Checkout the branch you'd like to merge changes into, eg master:

        $ git checkout master

1. Use `git merge` to merge changes from another branch:

        $ git merge foo
        Updating 8190a0b..cb86977
        Fast-forward
         file.txt | 1 +
         1 file changed, 1 insertion(+)

1. Compare the branches and observe there are no differences:

        $ git diff master..foo

## Resolving conflicts

If Git can't merge branches cleanly, it will indicate conflicting lines and ask us to resolve.

1. Checkout master, create a change, and commit, eg

        $ git checkout master
        $ echo "starting text" > file.txt
        $ git add file.txt
        $ git commit -m "Add starting text"

1. Create two new branches:

        $ git branch foo
        $ git branch bar

1. Checkout each branch and make a different change to the same line of the same file:

        $ git checkout foo
        $ echo "foo text" > file.txt
        $ git add file.txt
        $ git commit -m "Add foo text"
        $ git checkout bar
        $ echo "bar text" > file.txt
        $ git add file.txt
        $ git commit -m "Add bar text"

1. Checkout master and merge both branches into it:

        $ git checkout master
        $ git merge foo
        Updating feac207..1f2e058
        Fast-forward
         file.txt | 2 +-
         1 file changed, 1 insertion(+), 1 deletion(-)
        $ git merge bar
        Auto-merging file.txt
        CONFLICT (content): Merge conflict in file.txt
        Automatic merge failed; fix conflicts and then commit the result.

1. View the conflicted state:

        $ git status
        On branch master
    
        You have unmerged paths.
          (fix conflicts and run "git commit")
    
        Unmerged paths:
          (use "git add <file>..." to mark resolution)
    
          both modified:   file.txt
    
        no changes added to commit (use "git add" and/or "git commit -a")
        $ cat file.txt 
        <<<<<<< HEAD
        foo text
        =======
        bar text
        >>>>>>> bar

1. Edit the file to remove all but the text you'd like to keep, eg:

        $ cat file.txt 
        bar text

1. Run `git add` and `git commit` to accept the resolved conflict
1. View the changes in your log:

        $ git log -p
        commit 5455e48644e5e6de1290b978d58f1a9930c9e18a
        Merge: 1f2e058 8f942cd
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 16:57:54 2015 -0800
    
            Merge branch 'bar'
            
            Conflicts:
                file.txt
    
        commit 8f942cd50b229547986d2afa171a1fcb8d7a9f8d
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 16:55:05 2015 -0800
    
            Add bar text
    
        diff --git a/file.txt b/file.txt
        index d2bc3f2..132d0f8 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1 @@
        -starting text
        +bar text
    
        commit 1f2e05856b78a1fd51c55f76d991b260f4cd0abe
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 16:54:49 2015 -0800
    
            Add foo text
    
        diff --git a/file.txt b/file.txt
        index d2bc3f2..5e02c89 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1 @@
        -starting text
        +foo text
    
        commit feac207c460002b7d90706f03d9e782180112553
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 15:06:12 2015 -0800
    
            Add starting text
    
        diff --git a/file.txt b/file.txt
        new file mode 100644
        index 0000000..d2bc3f2
        --- /dev/null
        +++ b/file.txt
        @@ -0,0 +1 @@
        +starting text

## Reflect

In the steps above, we've started working with git history in a "local" repository.

We've also learned about branching and merging, two fundamental concepts in distributed development.

In the github exercise, we'll start working with a "remote" repository.

## Related

* [git - the simple guide](http://rogerdudler.github.io/git-guide/)
* [Pro Git](http://git-scm.com/book/en/v2)'s [Git Basics](http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)
* [Pro Git](http://gitbookio.gitbooks.io/progit/)